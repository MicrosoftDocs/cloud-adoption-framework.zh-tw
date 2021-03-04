---
title: 將已啟用 Azure Arc 的伺服器連線到 Azure Sentinel
description: 瞭解如何將已啟用 Azure Arc 的伺服器上架到 Azure Sentinel。
author: likamrat
ms.author: brblanch
ms.date: 01/29/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: think-tank, e2e-hybrid
ms.openlocfilehash: 8b177db065887d3031797a26109389fd44c35f58
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101795085"
---
# <a name="connect-azure-arc-enabled-servers-to-azure-sentinel"></a>將已啟用 Azure Arc 的伺服器連線到 Azure Sentinel

本文提供有關如何將已啟用 Azure Arc 的伺服器上架到 [Azure Sentinel](/azure/sentinel/)的指引。 這可讓您開始收集安全性相關事件，並開始將它們與其他資料來源相互關聯。

下列程式將會在您的 Azure 訂用帳戶上啟用及設定 Azure Sentinel。 此流程包括：

- 設定 Log Analytics 工作區，以匯總記錄和事件以進行分析和相互關聯。
- 在工作區上啟用 Azure Sentinel。
- 使用延伸模組管理功能和 Azure 原則，在 Azure Sentinel 上架啟用 Azure Arc 的伺服器。

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

1. 如前文所述，本指南會從您已部署 Vm 或裸機伺服器至 Azure Arc 的點開始。針對此案例，我們會使用已連線到 Azure Arc 的 Google Cloud Platform (GCP) 實例，並在 Azure 中顯示為資源。 如下列螢幕擷取畫面所示：

    ![Azure 入口網站中已啟用 Azure Arc 伺服器總覽的螢幕擷取畫面。](./media/arc-azure-sentinel/sentinel-1.png)

    ![顯示 azure 入口網站中 Azure Arc 伺服器詳細資料的螢幕擷取畫面。](./media/arc-azure-sentinel/sentinel-2.png)

1. [安裝或更新 AZURE CLI](/cli/azure/install-azure-cli)。 Azure CLI 應執行2.7 版或更新版本。 用 `az --version` 來檢查您目前安裝的版本。

1. 建立 Azure 服務主體。

    若要將 VM 或裸機伺服器連線至 Azure Arc，必須使用「參與者」角色指派的 Azure 服務主體。 若要建立它，請登入您的 Azure 帳戶，然後執行下列命令。 或者，您也可以在 [Azure Cloud Shell](https://shell.azure.com/)中完成這項操作。

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

## <a name="onboard-azure-sentinel"></a>使 Azure Sentinel 上線

Azure Sentinel 會使用 Log Analytics 代理程式來收集 Windows 和 Linux 伺服器的記錄檔，並將其轉送到 Azure Sentinel。 收集的資料會儲存在 Log Analytics 工作區中。 因為您無法使用 Azure 安全性中心所建立的預設工作區，所以需要有一個自訂工作區。 您可以在與 Azure Sentinel 相同的自訂工作區中，取得 Azure 安全性中心的原始事件和警示。

1. 建立專用的 Log Analytics 工作區，並在其頂端啟用 Azure Sentinel 解決方案。 使用此 [Azure Resource Manager 範本 (ARM 範本) ](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/azuresentinel/arm/sentinel-template.json) 建立新的 Log Analytics 工作區、定義 Azure Sentinel 解決方案，並為工作區啟用它。 若要自動化部署，您可以編輯 ARM 範本 [參數](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/azuresentinel/arm/sentinel-template.parameters.json)檔案，提供工作區的名稱和位置。

    ![ARM 範本的螢幕擷取畫面。](./media/arc-azure-sentinel/sentinel-3.png)

1. 部署 ARM 範本。 流覽至 [部署資料夾](https://github.com/microsoft/azure_arc/tree/main/azure_arc_servers_jumpstart/azuresentinel/arm) ，然後執行下列命令。

  ```console
  az deployment group create --resource-group <Name of the Azure resource group> \
  --template-file <The `sentinel-template.json` template file location> \
  --parameters <The `sentinel-template.parameters.json` template file location>
  ```

例如：

   ![[Az deployment group create] 命令的螢幕擷取畫面。](./media/arc-azure-sentinel/sentinel-4.png)

## <a name="onboard-azure-arc-enabled-vms-on-azure-sentinel"></a>在 Azure Sentinel 上將已啟用 Azure Arc 的 Vm 上架

在您將 Azure Sentinel 部署至 Log Analytics 工作區之後，您需要將資料來源連接到該工作區。

有 Microsoft 服務的連接器，以及來自安全性產品生態系統的協力廠商解決方案。 您也可以使用常見事件格式 (CEF) 、syslog 或 REST API，將您的資料來源與 Azure Sentinel 連線。

針對伺服器和 Vm，您可以安裝 Microsoft Monitoring Agent (MMA) 代理程式或 Azure Sentinel 代理程式來收集記錄，並將它們轉送到 Azure Sentinel。 您可以使用 Azure Arc 以多種方式部署代理程式：

- [擴充功能管理](./arc-vm-extension-mma.md)：啟用 Azure Arc 的伺服器中的這項功能可讓您將 MMA 代理程式 VM 擴充功能部署至非 Azure Windows 或 Linux vm。 您可以使用 Azure 入口網站、Azure CLI、ARM 範本和 PowerShell 腳本來管理將擴充功能部署到已啟用 Azure Arc 的伺服器。

- [Azure 原則](./arc-policies-mma.md)：您可以指派原則，以在啟用 Azure Arc 的伺服器是否已安裝 MMA 代理程式的情況下進行審查。 如果未安裝代理程式，您可以使用 [擴充功能] 功能，利用補救工作（與 Azure Vm 比較的註冊體驗）將它自動部署到 VM。

## <a name="clean-up-your-environment"></a>清除您的環境

請完成下列步驟來清除您的環境。

1. 使用下列每個指南中的清除指示，從每個環境移除虛擬機器。

   - [GCP Ubuntu 實例](./gcp-terraform-ubuntu.md) 和 [GCP Windows 實例](./gcp-terraform-windows.md)
   - [AWS Ubuntu EC2 實例](./aws-terraform-ubuntu.md)
   - [Vmware VSphere UBUNTU VM](./vmware-terraform-ubuntu.md) 和 [Vmware VSPHERE Windows Server vm](./vmware-terraform-windows.md)
   - [Vagrant Ubuntu box](./local-vagrant-ubuntu.md) 和 [Vagrant Windows box](./local-vagrant-windows.md)

2. 在 Azure CLI 中執行下列腳本，以移除 Log Analytics 工作區。 提供您在建立 Log Analytics 工作區時所使用的工作區名稱。

  ```console
  az monitor log-analytics workspace delete --resource-group <Name of the Azure resource group> --workspace-name <Log Analytics Workspace Name> --yes
  ```
