---
title: 管理 Azure 原則，並將 Azure monitoring agent 擴充功能部署至 Azure Arc Linux 和 Windows server
description: 瞭解如何使用已啟用 Azure Arc 的伺服器，將 Azure 原則指派給 Azure 外部的 Vm，不論它們是在內部部署或其他雲端上。
author: likamrat
ms.author: brblanch
ms.date: 01/29/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: think-tank, e2e-hybrid
ms.openlocfilehash: 56210c7fdef90f9ba50378adce35ab7f8a09b3e3
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101795065"
---
# <a name="manage-azure-policies-and-deploy-the-azure-monitoring-agent-extension-to-azure-arc-linux-and-windows-servers"></a>管理 Azure 原則，並將 Azure monitoring agent 擴充功能部署至 Azure Arc Linux 和 Windows server

本文提供有關如何使用已啟用 Azure Arc 的伺服器將 Azure 原則指派給 Azure 外部 Vm （不論是在內部部署或其他雲端上）的指引。 使用此功能，您現在可以使用 Azure 原則來在已啟用 Azure Arc 之伺服器的作業系統中進行審核設定，如果設定不符合規範，您也可以觸發補救工作。

在此情況下，您將指派原則，以在已安裝 Azure Arc 的電腦是否已安裝 (Microsoft Monitoring Agent) MMA 代理程式的情況下進行審查。 如果沒有，請使用延伸模組功能，將其自動部署至 VM，這是向 Azure Vm 層級的註冊體驗。 這種方法可以用來確保您的所有伺服器都上線至 Azure 監視器、Azure 資訊安全中心、Azure Sentinel 等服務。

您可以使用 Azure 入口網站、Azure Resource Manager 範本 (ARM 範本) 或 PowerShell 腳本，將原則指派給 Azure 訂用帳戶或資源群組。 下列程式使用 ARM 範本來指派內建原則。

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

請參閱 [Azure 監視器支援的作業系統檔](/azure/azure-monitor/insights/vminsights-enable-overview#supported-operating-systems) ，並確保支援您用於這些程式的 vm。 針對 Linux Vm，請檢查 Linux 發行版本和核心，以確定您使用的是支援的設定。

## <a name="prerequisites"></a>必要條件

1. 複製 Azure Arc Jumpstart 存放庫。

   ```console
   git clone https://github.com/microsoft/azure_arc
   ```

2. 如前文所述，本指南會從您已部署 Vm 或伺服器至 Azure Arc 的位置開始。在以下螢幕擷取畫面中，Google Cloud Platform (GCP) server 已與 Azure Arc 連線，並在 Azure 中顯示為資源。

   ![適用于已啟用 Azure Arc 之伺服器的資源群組螢幕擷取畫面。](./media/arc-policies-mma/resource-group.png)

   ![已啟用 Azure Arc 之伺服器的線上狀態螢幕擷取畫面。](./media/arc-policies-mma/connected-status.png)

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

   輸出看起來應該會像下面這樣：

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

您也需要部署 Log Analytics 工作區。 您可以藉由編輯 ARM 範本 [參數](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/policies/arm/log_analytics-template.parameters.json) 檔案，並提供工作區的名稱和位置，將部署自動化。

![ARM 範本參數檔案的螢幕擷取畫面。](./media/arc-policies-mma/parameter-file-1.png)

若要部署 ARM 範本，請流覽至 [部署資料夾](https://github.com/microsoft/azure_arc/tree/main/azure_arc_servers_jumpstart/policies/arm) ，然後執行下列命令：

```console
az deployment group create --resource-group <Name of the Azure resource group> \
--template-file <The `log_analytics-template.json` template file location> \
--parameters <The `log_analytics-template.parameters.json` template file location>
```

## <a name="assign-policies-to-azure-arc-connected-machines"></a>將原則指派給已連線到 Azure Arc 的電腦

設定好所有必要條件之後，您可以將原則指派給已連線到 Azure Arc 的電腦。 編輯 [參數](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/policies/arm/policy.json) 檔案，以提供您的訂用帳戶識別碼及 Log Analytics 工作區。

![另一個 ARM 範本參數檔案的螢幕擷取畫面。](./media/arc-policies-mma/parameter-file-2.png)

1. 若要開始部署，請使用下列命令：

   ```console
   az policy assignment create --name 'Enable Azure Monitor for VMs' \
   --scope '/subscriptions/<Your subscription ID>/resourceGroups/<Name of the Azure resource group>' \
   --policy-set-definition '55f3eceb-5573-4f18-9695-226972c6d74a' \
   -p <The *policy.json* template file location> \
   --assign-identity --location <Azure Region>
   ```

   旗標會 `policy-set-definition` 指向方案 `Enable Azure Monitor` 定義識別碼。

2. 指派計畫之後，將指派約30分鐘的時間才會將指派套用至定義的範圍。 然後，azure 原則會針對已連線到 Azure Arc 的電腦開始評估週期，並將其辨識為不符合規範，因為它仍未部署 Log Analytics 代理程式設定。 若要確認這一點，請移至 [原則] 區段下的 [Azure Arc 連線電腦]。

   ![不符合規範的 Azure 原則狀態的螢幕擷取畫面。](./media/arc-policies-mma/noncompliant-policy.png)

3. 現在，將補救工作指派給不符合規範的資源，使其進入符合規範的狀態。

   ![建立 Azure 原則補救工作的螢幕擷取畫面。](./media/arc-policies-mma/create-remediation-task.png)

4. 在 [**要補救的原則**] 底下，選擇 [ **\[ 預覽] 將 Log Analytics 代理程式部署到 Linux Azure Arc 機器**，然後選取 [**修復**]。 此補救工作會指示 Azure 原則執行 `DeployIfNotExists` 效果，並使用 Azure Arc 擴充功能管理功能在 VM 上部署 Log Analytics 代理程式。

   ![補救工作中 Azure 原則補救動作的螢幕擷取畫面。](./media/arc-policies-mma/remediation-action.png)

5. 指派補救工作之後，會再次評估原則。 它應該會顯示 GCP 上的伺服器符合規範，而且 Microsoft Monitoring Agent 擴充功能已安裝在 Azure Arc 電腦上。

   ![補救工作設定的螢幕擷取畫面。](./media/arc-policies-mma/task-config.png)

   ![符合規範的 Azure 原則狀態的螢幕擷取畫面。](./media/arc-policies-mma/compliant-status.png)

## <a name="clean-up-your-environment"></a>清除您的環境

請完成下列步驟來清除您的環境。

1. 遵循每個指南的終止指示，從每個環境移除虛擬機器。

   - [GCP Ubuntu 實例](./gcp-terraform-ubuntu.md) 和 [GCP Windows 實例](./gcp-terraform-windows.md)
   - [AWS Ubuntu EC2 實例](./aws-terraform-ubuntu.md)
   - [Vmware VSphere UBUNTU VM](./vmware-terraform-ubuntu.md) 和 [Vmware VSPHERE Windows Server vm](./vmware-terraform-windows.md)
   - [Vagrant Ubuntu box](./local-vagrant-ubuntu.md) 和 [Vagrant Windows box](./local-vagrant-windows.md)

2. 在 Azure CLI 中執行下列腳本，以移除 Azure 原則指派。

   ```console
   az policy assignment delete --name 'Enable Azure Monitor for VMs' --resource-group <resource-group>
   ```

3. 在 Azure CLI 中執行下列腳本，以移除 Log Analytics 工作區。 提供您在建立 Log Analytics 工作區時所使用的工作區名稱。

   ```console
   az monitor log-analytics workspace delete --resource-group <Name of the Azure resource group> --workspace-name <Log Analytics workspace Name> --yes
   ```
