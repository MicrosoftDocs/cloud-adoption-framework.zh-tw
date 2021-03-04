---
title: 將已啟用 Azure Arc 的伺服器連線至 Azure 安全性中心
description: 瞭解如何將已啟用 Azure Arc 的伺服器上架到 Azure 安全性中心。
author: likamrat
ms.author: brblanch
ms.date: 01/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: think-tank, e2e-hybrid
ms.openlocfilehash: 0829a67a8ddc00d3db73ff2ac67d781b7e52f01e
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101795053"
---
# <a name="connect-azure-arc-enabled-servers-to-azure-security-center"></a>將已啟用 Azure Arc 的伺服器連線至 Azure 安全性中心

本文提供有關如何將已啟用 Azure Arc 的伺服器上架到 azure 資訊安全中心 [)  (azure ](/azure/security-center/)資訊安全中心的指引。 這可協助您開始收集安全性相關設定和事件記錄檔，讓您可以建議動作，並改善整體的 Azure 安全性狀況。

在下列程式中，您會在 Azure 訂用帳戶上啟用及設定 Azure 資訊安全中心標準層。 這可提供先進的威脅防護 (ATP) 和偵測功能。 此程序包括：

- 設定 Log Analytics 工作區，以匯總記錄和事件以進行分析。
- 指派安全性中心的預設安全性原則。
- 查看 Azure 安全性中心的建議。
- 使用 **快速修正** 補救在啟用 Azure Arc 的伺服器上套用建議的設定。

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

2. 如前文所述，本指南會從您已部署 Vm 或裸機伺服器至 Azure Arc 的點開始。針對此案例，我們會使用已連線到 Azure Arc 的 Google Cloud Platform (GCP) 實例，並在 Azure 中顯示為資源。 如下列螢幕擷取畫面所示：

    ![Azure 入口網站中啟用 Azure Arc 之伺服器的螢幕擷取畫面。](./media/arc-security-center/arc-overview.png)

    ![Azure 入口網站中已啟用 Azure Arc 之伺服器的詳細資料螢幕擷取畫面。](./media/arc-security-center/arc-status.png)

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

## <a name="onboard-azure-security-center"></a>上架 Azure 安全性中心

1. Azure 安全性中心所收集的資料會儲存在 Log Analytics 工作區中。 您可以使用 Azure 安全性中心所建立的預設，或您所建立的自訂帳戶。 如果您想要建立專用工作區，您可以藉由編輯 Azure Resource Manager 範本 (ARM 範本) [參數](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/securitycenter/arm/log_analytics-template.parameters.json)檔案，提供工作區的名稱和位置，來將部署自動化：

   ![ARM 範本的螢幕擷取畫面。](./media/arc-security-center/arm-template.png)

2. 若要部署 ARM 範本，請流覽至 [部署資料夾](https://github.com/microsoft/azure_arc/tree/main/azure_arc_servers_jumpstart/securitycenter/arm) ，然後執行下列命令：

   ```console
   az deployment group create --resource-group <Name of the Azure resource group> \
   --template-file <The `log_analytics-template.json` template file location> \
   --parameters <The `log_analytics-template.parameters.json` template file location>
   ```

3. 如果您想要使用使用者定義的工作區，您應該指示安全性中心使用它，而不是預設的工作區，請使用下列命令：

   ```console
   az security workspace-setting create --name default \
   --target-workspace '/subscriptions/<Your subscription ID>/resourceGroups/<Name of the Azure resource group>/providers/Microsoft.OperationalInsights/workspaces/<Name of the Log Analytics Workspace>'
   ```

4. 選取 Azure 安全性中心層。 預設會在您所有的 Azure 訂用帳戶上啟用免費層，並提供持續的安全性評估和可採取動作的安全性建議。 在本指南中，您會使用適用于 Azure 虛擬機器的標準層，以擴充這些功能，為混合式雲端工作負載提供統一的安全性管理和威脅防護。 若要啟用適用于 Vm 的 Azure 安全性中心標準層，請執行下列命令：

    ```console
    az security pricing create -n VirtualMachines --tier 'standard'
    ```

5. 指派預設的安全中心原則方案。 Azure 資訊安全中心會根據原則提出安全性建議。 有一個特定的方案會將包含定義識別碼的安全中心原則分組 `1f3afdf9-d0c9-4c3d-847f-89da613e70a8` 。 下列命令會將 Azure 安全性中心方案指派給您的訂用帳戶。

    ```console
    az policy assignment create --name 'Azure Security Center Default <Your subscription ID>' \
    --scope '/subscriptions/<Your subscription ID>' \
    --policy-set-definition '1f3afdf9-d0c9-4c3d-847f-89da613e70a8'
    ```

## <a name="azure-arc-and-azure-security-center-integration"></a>Azure Arc 和 Azure 安全性中心整合

成功將 Azure 資訊安全中心上線之後，您將會取得協助保護您的資源的建議，包括已啟用 Azure Arc 的伺服器。 Azure 安全性中心會定期分析 Azure 資源的安全性狀態，以找出潛在的安全性弱點。

在 **vm & 伺服器** 下的 **計算 & Apps** 區段中，Azure 安全性中心會概述您 vm 和電腦的所有探索到的安全性建議，包括 azure Vm、azure 傳統 Vm、伺服器和 azure Arc 電腦。

![Azure 安全性中心的 [計算 & Apps] * * 螢幕擷取畫面。](./media/arc-security-center/compute-apps.png)

在啟用 Azure Arc 的伺服器上，Azure 資訊安全中心會建議安裝 Log Analytics 代理程式。 每項建議也包含：

- 建議的簡短描述。
- 安全分數的影響，在此案例中，狀態為「 **高**」。
- 執行建議所需執行的補救步驟。

針對特定的建議，如下列螢幕擷取畫面所示，您也會取得 **快速修正** ，可讓您在多個資源上快速補救建議。

  ![適用于已啟用 Azure Arc 之伺服器的 Azure 安全性中心建議螢幕擷取畫面。](./media/arc-security-center/recommendation-quick-fix.png)

  ![Azure 安全性中心建議安裝 Log Analytics 的螢幕擷取畫面。](./media/arc-security-center/recommendation-remediate.png)

下列補救 **快速修正** 是使用 ARM 範本，在 Azure Arc 電腦上部署 Microsoft Monitoring Agent 擴充功能。

  ![Azure 安全性中心 * * 快速修正 * * ARM 範本的螢幕擷取畫面。](./media/arc-security-center/quick-fix-template.png)

您可以從 Azure 安全性中心儀表板，選取用於 Azure 安全性中心的 Log Analytics 工作區，然後選擇 [ **補救1資源**]，以從 Azure 安全性中心儀表板觸發補救。

  ![如何在 Azure 安全性中心內觸發補救步驟的螢幕擷取畫面。](./media/arc-security-center/remediation-trigger.png)

在啟用 Azure Arc 的伺服器上套用建議之後，資源會標示為狀況良好。

  ![狀況良好的已啟用 Azure Arc 伺服器螢幕擷取畫面。](./media/arc-security-center/healthy-server.png)

## <a name="clean-up-your-environment"></a>清除您的環境

請完成下列步驟來清除您的環境。

1. 遵循每個指南的終止指示，從每個環境移除虛擬機器。

    - [GCP Ubuntu 實例](./gcp-terraform-ubuntu.md) 和 [GCP Windows 實例](./gcp-terraform-windows.md)
    - [AWS Ubuntu EC2 實例](./aws-terraform-ubuntu.md)
    - [VMware VSphere UBUNTU VM](./vmware-terraform-ubuntu.md)  / [VMware VSphere Windows SERVER VM](./vmware-terraform-windows.md)
    - [Vagrant Ubuntu box](./local-vagrant-ubuntu.md)  / [Vagrant Windows box](./local-vagrant-windows.md)

2. 在 Azure CLI 中執行下列腳本，以移除 Log Analytics 工作區。 提供您在建立 Log Analytics 工作區時所使用的工作區名稱。

  ```console
  az monitor log-analytics workspace delete --resource-group <Name of the Azure resource group> --workspace-name <Log Analytics Workspace Name> --yes
  ```
