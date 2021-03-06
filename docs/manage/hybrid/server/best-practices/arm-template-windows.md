---
title: 使用 Azure Resource Manager 範本來部署 Azure 虛擬機器並將其連線至 Azure Arc
description: 使用 Azure Resource Manager 範本來部署 Azure 虛擬機器，並將其連線到 Azure Arc。
author: likamrat
ms.author: brblanch
ms.date: 01/29/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: think-tank, e2e-hybrid
ms.openlocfilehash: b4dd445fd3ac8e9dc9677ebcae0933cf4bf386e1
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102112121"
---
# <a name="use-an-azure-resource-manager-template-to-deploy-and-connect-an-azure-virtual-machine-to-azure-arc"></a>使用 Azure Resource Manager 範本來部署 Azure 虛擬機器並將其連線至 Azure Arc

本文提供使用 [Azure Resource Manager 範本 (ARM 範本 ](/azure/azure-resource-manager/templates/overview) 的指引，) 自動將 azure 虛擬機器 (azure VM) 執行 Windows 加入 azure Arc。提供的 ARM 範本負責建立 Azure 資源，並在 VM 上執行 Azure Arc 上架腳本。

根據預設，azure Vm 會使用 [Azure Instance Metadata Service (IMDS) ](/azure/virtual-machines/windows/instance-metadata-service) 。 藉由將 Azure VM 投影為已啟用 Azure Arc 的伺服器，就會建立 *衝突* ，在使用 IMDS 時，不允許將 azure Arc 伺服器資源表示為一個。 相反地，Azure Arc 伺服器仍會以原生 Azure VM 的形式「act」。

本指南可讓您使用 Azure Vm 並將其上架至 Azure Arc，以 **供示範之** 用。 您將能夠模擬部署在 Azure 外部的伺服器，例如「內部部署」或其他雲端平臺。

> [!NOTE]
> Azure VM 不應為啟用 Azure Arc 的伺服器。 下列案例不受支援，且僅供示範和測試之用。

## <a name="prerequisites"></a>必要條件

1. 複製 Azure Arc Jumpstart 存放庫。

    ```console
    git clone https://github.com/microsoft/azure_arc.git
    ```

2. [安裝或更新 AZURE CLI 至2.7 版或更新版本](/cli/azure/install-azure-cli)。 使用下列命令來檢查您目前安裝的版本。

    ```console
    az --version
    ```

3. Azure 訂用帳戶：如果您沒有 Azure 訂用帳戶，您可以 [建立免費的 azure 帳戶](https://azure.microsoft.com/free/)。

4. 建立 Azure 服務主體。

    為了讓您使用 ARM 範本部署 Azure 資源，您必須使用「參與者」角色指派的 Azure 服務主體。 若要建立它，請登入您的 Azure 帳戶，然後執行下列命令。 您也可以在 [Azure Cloud Shell](https://shell.azure.com/)中執行此命令。

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

## <a name="automation-flow"></a>自動化流程

為了讓您熟悉自動化和部署流程，以下說明。

1. 使用者編輯 ARM 範本參數檔案 (一次編輯) 。 這些參數值會在整個部署中使用。

2. ARM 範本包含 Azure VM 自訂腳本擴充功能，可部署 [`install_arc_agent.ps1`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/azure/windows/arm_template/scripts/install_arc_agent.ps1) PowerShell 腳本。

3. 為了讓 Azure VM 成功投射為啟用 Azure Arc 的伺服器，腳本將會：

    1. 設定本機 OS 環境變數。

    2. 產生名為的本機 OS 登入腳本 `LogonScript.ps1` 。 此指令碼會：

        - 建立檔案 `LogonScript.log` 。

        - 停止及停用 Windows Azure 來賓代理程式服務。

        - 建立新的 Windows 防火牆規則，以封鎖 Azure IMDS 對遠端位址的輸出流量 `169.254.169.254` 。

        - 取消登錄登入腳本 Windows 排程工作，使其不會在第一次登入後執行。

    3. 停用並防止 Windows Server Manager 在啟動時執行。

4. 使用者透過 RDP 連線到 Windows VM，該 VM 會開始執行 `LogonScript.ps1` ，並將 VM 將上線至 Azure Arc。

## <a name="deployment"></a>部署

如前所述，此部署將使用 ARM 範本。 您將會部署單一範本，負責在單一資源群組中建立所有 Azure 資源，並將建立的 VM 上架至 Azure Arc。

1. 部署 ARM 範本之前，請使用 Azure CLI 搭配命令來登入 Azure `az login` 。

2. 部署使用 ARM 範本參數檔案。 開始部署之前，請編輯 [`azuredeploy.parameters.json`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/azure/windows/arm_template/azuredeploy.parameters.json) 位於您本機複製之存放庫資料夾中的檔案。 範例參數檔案位於 [此處](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/azure/windows/arm_template/azuredeploy.parameters.example.json)。

3. 若要部署 ARM 範本，請流覽至本機複製的 [部署資料夾](https://github.com/microsoft/azure_arc/tree/main/azure_arc_servers_jumpstart/azure/windows/arm_template) ，然後執行下列命令：

    ```console
    az group create --name <Name of the Azure resource group> --location <Azure Region> --tags "Project=jumpstart_azure_arc_servers"
    az deployment group create \
    --resource-group <Name of the Azure resource group> \
    --name <The name of this deployment> \
    --template-uri https://raw.githubusercontent.com/microsoft/azure-arc/main/azure_arc_servers_jumpstart/azure/windows/arm_template/azuredeploy.json \
    --parameters <The `azuredeploy.parameters.json` parameters file location>
    ```

    > [!NOTE]
    > 請確定您使用的 Azure 資源組名與您在檔案中使用的相同 `azuredeploy.parameters.json` 。

    例如：

    ```console
    az group create --name Arc-Servers-Win-Demo --location "East US" --tags "Project=jumpstart_azure_arc_servers"
    az deployment group create \
    --resource-group Arc-Servers-Win-Demo \
    --name arcwinsrvdemo \
    --template-uri https://raw.githubusercontent.com/microsoft/azure-arc/main/azure_arc_servers_jumpstart/azure/windows/arm_template/azuredeploy.json \
    --parameters azuredeploy.parameters.json
    ```

4. 布建 Azure 資源之後，您可以在 Azure 入口網站中看到它們。

    ![ARM 範本輸出的螢幕擷取畫面。](./media/arm-template/template-windows-output.png)

    ![資源群組中的螢幕擷取畫面資源。](./media/arm-template/template-windows-resources.png)

## <a name="windows-sign-in-and-post-deployment"></a>Windows 登入和部署後

1. 現在已建立 Windows Server VM，下一步是連接它。 使用其公用 IP 位址，以 RDP 連線至 VM。

    ![Azure VM 公用 IP 位址的螢幕擷取畫面。](./media/arm-template/template-windows-ip.png)

2. 第一次登入時，如「 [自動化流程](#automation-flow) 」一節所述，將會執行登入腳本。 此腳本會建立為自動化部署程式的一部分。

3. 讓腳本執行， **不要關閉** PowerShell 會話。 會話會在完成後自動關閉。

    > [!NOTE]
    > 腳本執行時間的長度為 ~ 1-2 分鐘。

    ![一種腳本輸出類型的螢幕擷取畫面。](./media/arm-template/template-windows-script-1.png)

    ![第二種類型之腳本輸出的螢幕擷取畫面。](./media/arm-template/template-windows-script-2.png)

    ![第三種類型腳本輸出的螢幕擷取畫面。](./media/arm-template/template-windows-script-3.png)

    ![第四種腳本輸出類型的螢幕擷取畫面。](./media/arm-template/template-windows-script-4.png)

4. 成功完成時，會將已啟用新的 Azure Arc 伺服器新增至資源群組。

![從已啟用 Azure Arc 之伺服器的資源群組螢幕擷取畫面。](./media/arm-template/template-windows-resource-gp.png)

![從已啟用 Azure Arc 之伺服器的詳細資料螢幕擷取畫面。](./media/arm-template/template-windows-server-details.png)

## <a name="cleanup"></a>清除

若要刪除整個部署，請從 Azure 入口網站中刪除資源群組。

![如何刪除資源群組的螢幕擷取畫面。](./media/arm-template/template-windows-delete.png)
