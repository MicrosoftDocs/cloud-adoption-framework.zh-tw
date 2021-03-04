---
title: 將 Microsoft Monitoring Agent 部署至 Azure Arc Linux 和 Windows server
description: 瞭解如何管理擴充功能，並使用 Azure Resource Manager 範本將 Microsoft Monitoring Agent 部署至 Azure Arc Linux 和 Windows server。
author: likamrat
ms.author: brblanch
ms.date: 01/29/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: think-tank, e2e-hybrid
ms.openlocfilehash: 21bb83e68f7e31862e0485853016a6a059c7fc20
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794633"
---
# <a name="manage-extensions-and-use-an-azure-resource-manager-template-to-deploy-microsoft-monitoring-agent-to-azure-arc-linux-and-windows-servers"></a>管理延伸模組，並使用 Azure Resource Manager 範本將 Microsoft Monitoring Agent 部署至 Azure Arc Linux 和 Windows server

本文提供有關如何管理已啟用 Azure Arc 之伺服器的延伸模組的指引。 虛擬機器擴充功能是小型的應用程式，可提供部署後設定和自動化工作，例如軟體安裝、防毒軟體防護，或執行自訂腳本的機制。

啟用 azure Arc 的伺服器，可讓您將 Azure VM 擴充功能部署至非 Azure Windows 和 Linux Vm，讓您有層級的混合式或多重雲端管理體驗，可對 Azure Vm 進行層級。

您可以使用 Azure 入口網站、Azure CLI、Azure Resource Manager 範本 (ARM 範本) 、PowerShell 腳本或 Azure 原則來管理將擴充功能部署到已啟用 Azure Arc 的伺服器（Linux 和 Windows）。 在下列程式中，您將使用 ARM 範本將 Microsoft Monitoring Agent (MMA) 部署到您的伺服器。 這會在使用此代理程式的 Azure 服務中將上線它們： Azure 監視器、Azure 資訊安全中心、Azure Sentinel 等等。

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

請參閱 [Azure 監視器支援的作業系統檔](/azure/azure-monitor/insights/vminsights-enable-overview#supported-operating-systems) ，並確定您將在此練習中使用的 vm 是受支援的。 針對 Linux Vm，請檢查 Linux 發行版本和核心，以確定您使用的是支援的設定。

## <a name="prerequisites"></a>必要條件

1. 複製 Azure Arc Jumpstart 存放庫。

    ```console
    git clone https://github.com/microsoft/azure_arc.git
    ```

2. 如前文所述，本指南會從您已部署 Vm 或裸機伺服器至 Azure Arc 的點開始。針對此案例，我們會使用已連線到 Azure Arc 的 Google Cloud Platform (GCP) 實例，並在 Azure 中顯示為資源。 如下列螢幕擷取畫面所示：

    ![從已啟用 Azure Arc 之伺服器的資源群組螢幕擷取畫面。](./media/arc-vm-extension-mma/mma-resource-group.png)

    ![從已啟用 Azure Arc 之伺服器線上狀態的螢幕擷取畫面。](./media/arc-vm-extension-mma/mma-connected-status.png)

3. [安裝或更新 AZURE CLI](/cli/azure/install-azure-cli)。 Azure CLI 應執行2.7 版或更新版本。 用 `az --version` 來檢查您目前安裝的版本。

4. 建立 Azure 服務主體。

    若要將 VM 或裸機伺服器連線至 Azure Arc，必須使用「參與者」角色指派的 Azure 服務主體。 若要建立它，請登入您的 Azure 帳戶，然後執行下列命令。 您也可以在 [Azure Cloud Shell](https://shell.azure.com/)中執行此命令。

    ```console
    az login
    az ad sp create-for-rbac -n "<Unique SP Name>" --role contributor
    ```

    例如：

    ```console
    az ad sp create-for-rbac -n "http://AzureArcServers" --role contributor
    ```

    輸出應該看起來像這樣︰

    ```json
    {
      "appId": "XXXXXXXXXXXXXXXXXXXXXXXXXXXX",
      "displayName": "AzureArcServers",
      "name": "http://AzureArcServers",
      "password": "XXXXXXXXXXXXXXXXXXXXXXXXXXXX",
      "tenant": "XXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    }
    ```

    > [!NOTE]
    > 強烈建議您將服務主體的範圍設為特定的 [Azure 訂用帳戶和資源群組](/cli/azure/ad/sp)。

5. 您也需要部署 Log Analytics 工作區。 您可以藉由編輯 ARM 範本 [參數](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/extensions/arm/log_analytics-template.parameters.json)檔案，並提供工作區的名稱和位置，將部署自動化。

    ![名稱和位置的 ARM 範本參數螢幕擷取畫面。](./media/arc-vm-extension-mma/parameters-file-1.png)

6. 若要部署 ARM 範本，請流覽至 `../extensions/arm` 部署資料夾，然後執行下列命令：

    ```console
    az deployment group create --resource-group <Name of the Azure resource group> \
    --template-file <The `log_analytics-template.json` template file location> \
    --parameters <The `log_analytics-template.parameters.json` template file location>
    ```

## <a name="azure-arc-enabled-servers-microsoft-monitoring-agent-extension-deployment"></a>啟用 Azure Arc 的伺服器 Microsoft Monitoring Agent 擴充功能部署

1. 編輯[擴充功能參數](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/extensions/arm/mma-template.parameters.json)檔案

    ![ARM 擴充功能參數檔案的螢幕擷取畫面。](./media/arc-vm-extension-mma/parameters-file-2.png)

    若要符合您的設定，您需要提供：

    - 在 Azure Arc 中註冊的 VM 名稱。

      ![從已啟用 Azure Arc 之伺服器的電腦名稱稱螢幕擷取畫面。](./media/arc-vm-extension-mma/mma-machine-name.png)

    - 您註冊啟用 Azure Arc 之伺服器的資源群組位置。

      ![Azure 區域的螢幕擷取畫面。](./media/arc-vm-extension-mma/mma-azure-region.png)

    - 您先前建立的 Log Analytics 工作區 (工作區識別碼和金鑰) 的相關資訊。 這些參數可用來設定 MMA 代理程式。 您可以前往 Log Analytics 工作區，然後在 [ **設定**] 底下選取 [ **代理程式管理**]，以找到這項資訊。

      ![啟用 Azure Arc 之伺服器內的代理程式管理螢幕擷取畫面。](./media/arc-vm-extension-mma/agents-management.png)

      ![工作區設定的螢幕擷取畫面。](./media/arc-vm-extension-mma/mma-workspace-config.png)

2. 選擇符合您作業系統（ [Windows](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/extensions/arm/mma-template-windows.json) 或 [LINUX](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/extensions/arm/mma-template-linux.json)）的 ARM 範本，方法是執行下列命令來部署範本：

    ```console
    az deployment group create --resource-group <Name of the Azure resource group> \
    --template-file <The `mma-template.json` template file location> \
    --parameters <The `mma-template.parameters.json` template file location>
    ```

3. 範本完成其執行之後，您應該會看到類似下列的輸出：

    ![ARM 範本輸出的螢幕擷取畫面。](./media/arc-vm-extension-mma/mma-output.png)

4. 您將會在 Windows 或 Linux 系統上部署 Microsoft Monitoring Agent，並向您所選取的 Log Analytics 工作區報告。 您可以回到工作區中的 [ **代理程式管理** ]，然後選擇 [ **Windows** ] 或 [ **Linux**]，以進行確認。 您應該會看到其他已連線的 VM。

    ![適用于 Windows 伺服器之已連線代理程式的螢幕擷取畫面。](./media/arc-vm-extension-mma/windows-agents.png)

    ![Linux 伺服器的已連線代理程式的螢幕擷取畫面。](./media/arc-vm-extension-mma/linux-agents.png)

## <a name="clean-up-your-environment"></a>清除您的環境

請完成下列步驟來清除您的環境。

1. 遵循每個指南的終止指示，從每個環境移除虛擬機器。

    - [GCP Ubuntu 實例](./gcp-terraform-ubuntu.md) 和 [GCP Windows 實例](./gcp-terraform-windows.md)
    - [AWS Ubuntu EC2 實例](./aws-terraform-ubuntu.md)
    - [Vmware VSphere UBUNTU VM](./vmware-terraform-ubuntu.md) 和 [Vmware VSPHERE Windows Server vm](./vmware-terraform-windows.md)
    - [Vagrant Ubuntu box](./local-vagrant-ubuntu.md) 和 [Vagrant Windows box](./local-vagrant-windows.md)

2. 在 Azure CLI 中執行下列命令，以移除 Log Analytics 工作區。 提供您在建立 Log Analytics 工作區時所使用的工作區名稱。

    ```console
    az monitor log-analytics workspace delete --resource-group <Name of the Azure resource group> --workspace-name <Log Analytics Workspace Name> --yes
    ```
