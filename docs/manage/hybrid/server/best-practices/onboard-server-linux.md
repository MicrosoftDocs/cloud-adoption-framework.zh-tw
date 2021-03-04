---
title: 將現有的 Linux 伺服器連線至 Azure Arc
description: 將現有的 Linux 伺服器連線至 Azure Arc。
author: likamrat
ms.author: brblanch
ms.date: 01/29/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: think-tank, e2e-hybrid
ms.openlocfilehash: 6961d80e893d59361ae0c9475972ddfb0e4f87f5
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101793695"
---
# <a name="connect-an-existing-linux-server-to-azure-arc"></a>將現有的 Linux 伺服器連線至 Azure Arc

本文提供使用簡單 shell 腳本將 Linux 伺服器連線至 Azure Arc 的指引。

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

3. 為您的伺服器建立新的 Azure 資源群組。

    ![Azure 入口網站的螢幕擷取畫面，其中包含空的資源群組。](./media/onboard-server/linux-resource-group.png)

4. 下載 [`az-connect-linux`](https://github.com/microsoft/azure_arc/blob/main/azure_arc_servers_jumpstart/scripts/az_connect_linux.sh) shell 腳本。

5. 根據您的環境變更環境變數。

    ![要變更的環境變數螢幕擷取畫面。](./media/onboard-server/linux-variables.png)

6. 使用您慣用的 (選擇工具將腳本複製到指定的伺服器，或將腳本複製/貼入伺服器) 內的新檔案。 下列範例顯示如何使用將腳本從 macOS 複製到伺服器 `scp` 。

    ![' Scp ' 腳本的螢幕擷取畫面。](./media/onboard-server/linux-scp.png)

## <a name="deployment"></a>部署

使用命令執行腳本 `. ./az-connect-linux.sh` 。

> [!NOTE]
> 額外的點是因為腳本有 *匯出* 函式，而且必須在與其余命令相同的 shell 會話中匯出變數。

成功完成後，您的 Linux 伺服器就會以新的 Azure Arc 資源連線到資源群組內。

![執行「az_connect」 Linux 腳本的螢幕擷取畫面。](./media/onboard-server/az-connect-linux.png)

![Azure 入口網站中已啟用 Azure Arc 資源的螢幕擷取畫面。](./media/onboard-server/linux-resource.png)

![Azure 入口網站中已啟用 Azure Arc 資源的詳細資料螢幕擷取畫面。](./media/onboard-server/linux-resource-detail.png)

## <a name="delete-the-deployment"></a>刪除部署

若要刪除伺服器，請選取伺服器，並從 Azure 入口網站刪除伺服器。

![在 Azure 入口網站中刪除資源之選項的螢幕擷取畫面。](./media/onboard-server/linux-delete-resource.png)

若要刪除整個部署，請從 Azure 入口網站刪除 Azure 資源群組。

![透過 Azure 入口網站刪除資源群組之選項的螢幕擷取畫面。](./media/onboard-server/linux-delete-resource-group.png)
