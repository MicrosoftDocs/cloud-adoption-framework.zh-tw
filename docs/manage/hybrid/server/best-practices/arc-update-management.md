---
title: 使用 Azure 自動化中的更新管理來管理已啟用 Azure Arc 之伺服器的作業系統更新
description: 瞭解如何將已啟用 Azure Arc 的伺服器上線至 Azure 自動化中的更新管理。
author: likamrat
ms.author: brblanch
ms.date: 01/29/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: think-tank, e2e-hybrid
ms.openlocfilehash: af9318b4cdcdc62c3b692c81865e23602fcd39ab
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794724"
---
# <a name="use-update-management-in-azure-automation-to-manage-operating-system-updates-for-azure-arc-enabled-servers"></a>使用 Azure 自動化中的更新管理來管理已啟用 Azure Arc 之伺服器的作業系統更新

本文提供有關如何將已啟用 Azure Arc 的伺服器上線至 [Azure 自動化中的更新管理](/azure/automation/update-management/overview)的指引，讓您可以為執行 Windows 或 Linux 的已啟用 azure arc 的伺服器管理作業系統更新。

在下列程式中，您會執行下列步驟，以建立並設定 Azure 自動化帳戶和 Log Analytics 工作區，以支援啟用 Azure Arc 之伺服器的更新管理：

- 設定新的 Log Analytics 工作區和 Azure 自動化帳戶。
- 在啟用 Azure Arc 的伺服器上啟用更新管理。

> [!IMPORTANT]
> 本文中的程式假設您已經部署 Vm，或是在內部部署或其他雲端上執行的伺服器，而且您已將這些 Vm 連線到 Azure Arc。如果您尚未這麼做，下列資訊可協助您將這項工作自動化。

- [GCP Ubuntu 實例](./gcp-terraform-ubuntu.md)
- [GCP Windows 實例](./gcp-terraform-windows.md)
- [AWS Ubuntu EC2 實例](./aws-terraform-ubuntu.md)
- [AWS Amazon Linux 2 EC2 實例](./aws-terraform-al2.md)
- [VMware vSphere Ubuntu VM](./vmware-terraform-ubuntu.md)
- [VMware vSphere Windows Server VM](./vmware-terraform-windows.md)
- [Vagrant Ubuntu box](./local-vagrant-ubuntu.md)
- [Vagrant Windows box](./local-vagrant-windows.md)

## <a name="prerequisites"></a>必要條件

1. 複製 Azure Arc Jumpstart 存放庫。

    ```console
    git clone https://github.com/microsoft/azure_arc
    ```

2. 如前文所述，本指南會從您已部署 Vm 或裸機伺服器至 Azure Arc 的點開始。在此案例中，我們會使用 Amazon Web Services (AWS) EC2 實例，該實例已連線到 Azure Arc，且在 Azure 中會顯示為資源。

    ![Amazon Web Services 雲端主控台中 EC2 的螢幕擷取畫面。](./media/arc-update-management/aws-ec2-instance.png)

    ![Azure 入口網站中啟用 Azure Arc 之伺服器的螢幕擷取畫面。](./media/arc-update-management/arc-enabled-server.png)

3. [安裝或更新 AZURE CLI](/cli/azure/install-azure-cli)。 Azure CLI 應執行2.14 版或更新版本。 用 `az --version` 來檢查您目前安裝的版本。

## <a name="configure-update-management"></a>設定更新管理

「更新管理」會使用 Log Analytics 代理程式來收集 Windows 和 Linux 伺服器的記錄檔，而收集的資料會儲存在 Log Analytics 工作區中。

1. 使用此 [Azure Resource Manager 範本 (ARM 範本) ](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/updateManagement/law-template.json)來建立 Log Analytics 工作區。 這會建立新的 Log Analytics 工作區、定義更新管理解決方案，並為工作區啟用它。

2. 執行下列命令，為 Log Analytics 工作區建立新的資源群組，並將括弧中的值取代為您自己的值。

    ```console
    az group create --name <Name for your resource group> \
    --location <Location for your resources> \
    --tags "Project=jumpstart-azure-arc-servers"
    ```

    ![[Az group create] 命令的螢幕擷取畫面。](./media/arc-update-management/az-group-create.png)

3. 編輯 ARM 範本 [參數](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/updateManagement/law-template.parameters.json)檔案，提供 Log Analytics 工作區的名稱、位置，以及 Azure 自動化帳戶的名稱。 您也必須提供啟用 Azure Arc 之伺服器的名稱，以及包含已啟用 Azure Arc 之伺服器的資源組名，如下列範例所示：

    ![ARM 範本的螢幕擷取畫面。](./media/arc-update-management/arm-template.png)

4. 部署 ARM 範本。 移至 [ [部署] 資料夾](https://github.com/microsoft/azure_arc/tree/main/azure_arc_servers_jumpstart/updateManagement) ，然後執行下列命令：

    ```console
    az deployment group create --resource-group <Name of the Azure resource group you created> \
        --template-file law-template.json \
        --parameters law-template.parameters.json
    ```

   ![[Az deployment group] 命令的螢幕擷取畫面。](./media/arc-update-management/az-deployment-group.png)

5. 當部署完成時，您應該會在 Azure 入口網站中看到您的 Log Analytics 工作區、自動化帳戶和更新管理解決方案的資源群組。 如果您在 [Log Analytics 工作區 **解決方案** ] 索引標籤中切入，您應該會看到 **更新管理** 解決方案。

    ![Azure 入口網站中 Log Analytics 工作區的螢幕擷取畫面。](./media/arc-update-management/log-analytics-workspace.png)

## <a name="confirm-that-the-update-management-solution-is-deployed-on-your-azure-arc-enabled-server"></a>確認已在啟用 Azure Arc 的伺服器上部署更新管理解決方案

1. 在 Log Analytics 工作區中按一下 [ **解決方案** ]，然後從清單中選取 [ **更新** ] 解決方案，以檢查更新管理評定的進度。

    ![Log Analytics 工作區之 [方案] 索引標籤的螢幕擷取畫面。](./media/arc-update-management/solutions-tab.png)

   更新管理可能需要數小時的時間，才能收集足夠的資料來顯示 VM 的評量。 在下一個頁面上，您可以看到正在執行評量。

   ![Log Analytics 工作區中 [總覽] 索引標籤和 [更新] 視圖的螢幕擷取畫面。](./media/arc-update-management/overview-tab.png)

   評量完成後，您會在 [更新管理] 索引標籤上看到 [ **View 摘要** ] 選項。

   ![Log Analytics 工作區中更新視圖摘要的螢幕擷取畫面。](./media/arc-update-management/updates-summary.png)

2. 選取 [ **視圖摘要**]，然後再次選取以深入探索更新管理評定。 在下列範例中，我們可以看到已啟用 Azure Arc 的伺服器上缺少更新。

    ![啟用 Azure Arc 的伺服器所缺少之更新的螢幕擷取畫面。](./media/arc-update-management/updates-missing.png)

## <a name="schedule-an-update"></a>排程更新

既然我們已設定更新管理解決方案，您可以針對已啟用 Azure Arc 的伺服器，以設定的排程來部署更新。

1. 流覽至先前建立的自動化帳戶，然後選取 [更新管理] 索引標籤，如下列螢幕擷取畫面所示。 您應該會看到已列出啟用 Azure Arc 的伺服器。

    ![Azure 自動化帳戶的螢幕擷取畫面。](./media/arc-update-management/azure-automation-account.png)

1. 選取 [ **排程更新部署**]。 在下一個頁面上，選取已啟用 Azure Arc 的伺服器所使用的作業系統，然後選取 **要更新的電腦** ，如下列螢幕擷取畫面所示。

    ![排程更新部署的更新螢幕擷取畫面。](./media/arc-update-management/schedule-an-update.png)

1. 從 [ **類型** ] 下拉式清單中，選取 [ **機器**]，然後選取您的伺服器，然後選取 **[確定]**。

    ![針對具有更新部署的已排程更新所選取的類型和伺服器螢幕擷取畫面。](./media/arc-update-management/type-update.png)

1. 按一下 [ **排程設定** ]，然後提供所需的排程。

    ![設定更新部署內排程設定的欄位螢幕擷取畫面。](./media/arc-update-management/config-schedule-settings.png)

    ![更新部署內排程設定欄位的螢幕擷取畫面。](./media/arc-update-management/schedule-settings.png)

1. 最後，為您的部署提供名稱，然後選取 [ **建立**]。

    ![在更新部署中具名更新的螢幕擷取畫面。](./media/arc-update-management/naming-update.png)

1. 從 [自動化帳戶更新管理] 索引標籤中，您應該可以從 [ **部署** 排程] 索引標籤查看已排程的更新部署。

    ![更新管理內排程更新的螢幕擷取畫面。](./media/arc-update-management/scheduled-update.png)

此更新管理解決方案將根據您定義的排程，在部署視窗中更新已啟用 Azure Arc 的伺服器。 除了此案例的範圍之外，您還可以使用 [更新管理](/azure/automation/update-management/overview) 。 如需詳細資訊，請參閱 [檔](/azure/automation/update-management/overview) 。

## <a name="clean-up-your-environment"></a>清除您的環境

請完成下列步驟來清除您的環境。

1. 遵循每個指南的終止指示，從每個環境移除虛擬機器。

    - [GCP Ubuntu 實例](./gcp-terraform-ubuntu.md) 和 [GCP Windows 實例](./gcp-terraform-windows.md)
    - [AWS Ubuntu EC2 實例](./aws-terraform-ubuntu.md)
    - [Vmware VSphere UBUNTU VM](./vmware-terraform-ubuntu.md) 和 [Vmware VSPHERE Windows Server vm](./vmware-terraform-windows.md)
    - [Vagrant Ubuntu box](./local-vagrant-ubuntu.md) 和 [Vagrant Windows box](./local-vagrant-windows.md)

1. 刪除資源群組。

    ```console
    az group delete --name <Name of your resource group>
    ```

    ![[Az group delete] 命令的螢幕擷取畫面。](./media/arc-update-management/az-group-delete.png)
