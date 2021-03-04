---
title: 將清查標記套用至已啟用 Azure Arc 的伺服器
description: 瞭解如何使用已啟用 Azure Arc 的伺服器，在混合式多重雲端和內部部署環境中提供伺服器清查管理功能
author: likamrat
ms.author: brblanch
ms.date: 01/29/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: think-tank, e2e-hybrid
ms.openlocfilehash: 25c0ecd913633a80db36cb3eb49eb85db635b290
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102114586"
---
# <a name="apply-inventory-tagging-to-azure-arc-enabled-servers"></a>將清查標記套用至已啟用 Azure Arc 的伺服器

本文提供如何使用已啟用 Azure Arc 之伺服器的指引，以提供跨混合式多重雲端和內部部署環境的伺服器清查管理功能。

啟用 azure Arc 的伺服器可讓您在公司網路或其他雲端提供者上，管理裝載于 Azure 外部的 Windows 和 Linux 機器。 這類似于您在 Azure 中管理原生虛擬機器的方式。 混合式機器連線到 Azure 時就會變成已連線的機器，並且視為 Azure 中的資源。 每個連線的機器都有資源識別碼，可作為訂用帳戶內資源群組的一部分來管理，並可從標準的 Azure 結構（例如 Azure 原則和套用標記）獲益。 使用 Azure 作為管理引擎輕鬆地組織及管理伺服器清查的能力，可大幅降低系統管理的複雜度，並為混合式和多重雲端環境提供一致的策略。

下列程式使用 [Resource Graph Explorer](/azure/governance/resource-graph/first-query-portal) 和 [Azure CLI](/cli/azure/install-azure-cli) 來示範如何從 Azure 中的單一窗格，在多個雲端上標記和查詢伺服器清查。

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

2. [安裝或更新 AZURE CLI 至2.7 版或更新版本](/cli/azure/install-azure-cli)。 使用下列命令來檢查目前安裝的版本。

   ```console
   az --version
   ```

## <a name="verify-that-your-azure-arc-connected-servers-are-ready-for-tagging"></a>確認您的 Azure Arc 連線伺服器已準備好進行標記

使用 Resource Graph Explorer 來查詢和查看 Azure 中的資源。

1. 在 Azure 入口網站的頂端搜尋列中輸入 **Resource Graph Explorer** ，然後選取它。

    ![Azure 入口網站中 Resource Graph Explorer 的螢幕擷取畫面。](./media/inventory-tagging/resource-graph-explorer.png)

1. 在查詢視窗中，輸入下列查詢，然後選取 [ **執行查詢**：

    ```kusto
    Resources
    | where type =~ 'Microsoft.HybridCompute/machines'
    ```

1. 如果您已正確建立啟用 Azure Arc 的伺服器，這些伺服器會列在 Resource Graph Explorer 的結果窗格中。 您也可以從 Azure 入口網站查看已啟用 Azure Arc 的服務。

    ![Resource Graph Explorer 查詢的螢幕擷取畫面。](./media/inventory-tagging/run-query.png)

    ![Azure 入口網站中已啟用 Azure Arc 之伺服器詳細資料的螢幕擷取畫面。](./media/inventory-tagging/arc-server.png)

## <a name="create-a-basic-azure-tag-taxonomy"></a>建立基本的 Azure 標記分類法

開啟 Azure CLI 並執行下列命令，以建立基本的分類結構，讓您可以輕鬆地查詢和報表服務器資源的裝載位置 (無論在 Azure、AWS、GCP 或內部部署) 。 如需建立標記分類法的詳細指引，請參閱 [資源命名和標記決策指南](../../../../decision-guides/resource-tagging/index.md)。

```console
az tag create --name "Hosting Platform"
az tag add-value --name "Hosting Platform" --value "Azure"
az tag add-value --name "Hosting Platform" --value "AWS"
az tag add-value --name "Hosting Platform" --value "GCP"
az tag add-value --name "Hosting Platform" --value "On-premises"
```

![' Az tag create ' 命令輸出的螢幕擷取畫面。](./media/inventory-tagging/az-tag-create.png)

## <a name="tag-your-azure-arc-resources"></a>標記您的 Azure Arc 資源

在您建立基本的分類結構之後，請將標記套用至已啟用 Azure Arc 的伺服器資源。 下列程式示範如何標記 AWS 和 GCP 中的資源。 如果您只有其中一個提供者的資源，則可以跳至 AWS 或 GCP 的適當區段。

### <a name="tag-the-azure-arc-connected-aws-ubuntu-ec2-instance"></a>標記已連線到 Azure Arc 的 AWS Ubuntu EC2 實例

在 CLI 中，執行下列命令以將 `Hosting Platform : AWS`  標記套用至已啟用 Azure Arc 的 AWS 伺服器。

> [!NOTE]
> 如果您使用 [Azure 教學](./aws-terraform-ubuntu.md)課程中所述的方法來連接您的 AWS EC2 實例，則需要調整和的值， `awsResourceGroup` `awsMachineName` 以符合您環境的特定值。

```console
export awsResourceGroup="arc-aws-demo"
export awsMachineName="arc-aws-demo"
export awsMachineResourceId="$(az resource show --resource-group $awsResourceGroup --name $awsMachineName --resource-type "Microsoft.HybridCompute/machines" --query id)"
export awsMachineResourceId="$(echo $awsMachineResourceId | tr -d "\"" | tr -d '\r')"
az resource tag --ids $awsMachineResourceId --tags "Hosting Platform"="AWS"
```

![「Az 資源標記」命令之一個輸出的螢幕擷取畫面。](./media/inventory-tagging/az-resource-tag-1.png)

### <a name="tag-azure-arc-connected-gcp-ubuntu-server"></a>標記已連線到 Azure Arc 的 GCP Ubuntu server

在 CLI 中，執行下列命令以將 `Hosting Platform : GCP`  標記套用至已啟用 Azure Arc 的 GCP 伺服器。

> [!NOTE]
> 如果您使用相關的 [Azure Arc Terraform 教學](./gcp-terraform-ubuntu.md)課程中所述的方法來連接您的 GCP 實例，則需要調整和的值， `gcpResourceGroup` `gcpMachineName` 以符合您環境的特定值。

```console
export gcpResourceGroup="arc-gcp-demo"
export gcpMachineName="arc-gcp-demo"
export gcpMachineResourceId="$(az resource show --resource-group $gcpResourceGroup --name $gcpMachineName --resource-type "Microsoft.HybridCompute/machines" --query id)"
export gcpMachineResourceId="$(echo $gcpMachineResourceId | tr -d "\"" | tr -d '\r')"
az resource tag --resource-group $gcpResourceGroup --ids $gcpMachineResourceId --tags "Hosting Platform"="GCP"
```

![' Az resource tag ' 命令的另一個輸出的螢幕擷取畫面。](./media/inventory-tagging/az-resource-tag-2.png)

## <a name="query-resources-by-tag-using-resource-graph-explorer"></a>使用 Resource Graph Explorer 依標記查詢資源

將標記套用至裝載于多個雲端的資源之後，請使用 Resource Graph Explorer 來查詢它們，並深入瞭解您的多重雲端環境。

1. 在查詢視窗中，輸入下列查詢︰

   ```kusto
   Resources
   | where type =~ 'Microsoft.HybridCompute/machines'
   | where isnotempty(tags['Hosting Platform'])
   | project name, location, resourceGroup, tags
   ```

   ![Resource Graph Explorer 查詢詳細資料的螢幕擷取畫面。](./media/inventory-tagging/run-query-details.png)

2. 按一下 [ **執行查詢** ]，然後選取 **格式化的結果** 切換。 如果正確完成，您應該會看到所有啟用 Azure Arc 的伺服器及其指派的 `Hosting Platform` 標記值。

   ![Resource Graph Explorer 查詢結果的螢幕擷取畫面。](./media/inventory-tagging/run-query-results.png)

   我們也可以從 Azure 入口網站查看投射的伺服器上的標記。

   ![啟用 Azure Arc 之伺服器上一組標記的螢幕擷取畫面。](./media/inventory-tagging/tags-1.png)

   ![啟用 Azure Arc 之伺服器上另一組標記的螢幕擷取畫面。](./media/inventory-tagging/tags-2.png)

## <a name="clean-up-your-environment"></a>清除您的環境

請完成下列步驟來清除您的環境。

1. 遵循每個指南的終止指示，從每個環境移除虛擬機器。

   - [GCP Ubuntu 實例](./gcp-terraform-ubuntu.md) 和 [GCP Windows 實例](./gcp-terraform-windows.md)
   - [AWS Ubuntu EC2 實例](./aws-terraform-ubuntu.md)
   - [Vmware VSphere UBUNTU VM](./vmware-terraform-ubuntu.md) 和 [Vmware VSPHERE Windows Server vm](./vmware-terraform-windows.md)
   - [Vagrant Ubuntu box](./local-vagrant-ubuntu.md) 和 [Vagrant Windows box](./local-vagrant-windows.md)

1. 藉由在 Azure CLI 中執行下列腳本，移除作為本指南一部分建立的標記。

   ```console
   az tag remove-value --name "Hosting Platform" --value "Azure"
   az tag remove-value --name "Hosting Platform" --value "AWS"
   az tag remove-value --name "Hosting Platform" --value "GCP"
   az tag remove-value --name "Hosting Platform" --value "On-premises"
   az tag create --name "Hosting Platform"
   ```
