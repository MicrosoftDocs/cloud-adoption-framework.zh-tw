---
title: 將現有的 Windows Server 實例連線至 Azure Arc
description: 將現有的 Windows Server 實例連線到 Azure Arc。
author: likamrat
ms.author: brblanch
ms.date: 01/29/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: think-tank, e2e-hybrid
ms.openlocfilehash: 38b5d79919fa8e4f75af0e9f485e5fec32149aeb
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101793683"
---
# <a name="connect-an-existing-windows-server-instance-to-azure-arc"></a>將現有的 Windows Server 實例連線至 Azure Arc

本文提供使用簡單的 PowerShell 腳本將 Windows 機器連接至 Azure Arc 的指引。

## <a name="prerequisites"></a>必要條件

1. [安裝或更新 AZURE CLI 至2.7 版或更新版本](/cli/azure/install-azure-cli)。 使用下列命令來檢查您目前安裝的版本。

    ```console
    az --version
    ```

2. 建立 Azure 服務主體。

    若要將伺服器連線到 Azure Arc，需要有指派參與者角色的 Azure 服務主體。 若要建立它，請登入您的 Azure 帳戶，然後執行下列命令。 您也可以在 [Azure Cloud Shell](https://shell.azure.com/)中執行此命令。

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

3. 為您的機器建立新的 Azure 資源群組。

    ![Azure 入口網站中空白資源群組的螢幕擷取畫面。](./media/onboard-server/windows-resource-group.png)

4. 下載 [`az-connect-win`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/scripts/az_connect_win.ps1) PowerShell 腳本。

5. 根據您的環境變更環境變數，並將腳本複製到指定的電腦。

    ![要變更的環境變數螢幕擷取畫面。](./media/onboard-server/windows-variables.png)

## <a name="deployment"></a>部署

在指定的電腦上， **以系統管理員** 身分開啟 PowerShell ISE 並執行腳本。 請注意，腳本會使用 `$env:ProgramFiles` 做為代理程式安裝路徑，因此請 **確定您未使用 PowerShell ISE (x86)**。

![[Azcmagent connect] 命令的螢幕擷取畫面。](./media/onboard-server/azcmagent.png)

![' Az-connect ' Windows 腳本的螢幕擷取畫面。](./media/onboard-server/az-connect-windows-2.png)

完成時，您將會在資源群組內以新的 Azure Arc 資源連接到您的 Windows Server 實例。

![執行「az_connect」 Windows 腳本的螢幕擷取畫面。](./media/onboard-server/az-connect-windows.png)

![Azure 入口網站中已啟用 Azure Arc 資源的螢幕擷取畫面。](./media/onboard-server/windows-resource.png)

![Azure 入口網站中已啟用 Azure Arc 資源的詳細資料螢幕擷取畫面。](./media/onboard-server/windows-resource-detail.png)

## <a name="delete-the-deployment"></a>刪除部署

若要刪除伺服器，請選取伺服器，並從 Azure 入口網站刪除伺服器。

![在 Azure 入口網站中刪除資源之選項的螢幕擷取畫面。](./media/onboard-server/windows-delete-resource.png)

若要刪除整個部署，請從 Azure 入口網站刪除 Azure 資源群組。

![透過 Azure 入口網站刪除資源群組之選項的螢幕擷取畫面。](./media/onboard-server/windows-delete-resource-group.png)
