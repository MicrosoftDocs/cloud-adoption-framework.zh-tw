---
title: Moodle 遷移工作、架構和範本
description: 深入瞭解 Moodle 遷移所涉及的工作和架構與範本。
author: BrianBlanchard
ms.author: brblanch
ms.date: 11/06/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.openlocfilehash: f3e5d88e894289e615ac0573d6c640832b8ed028
ms.sourcegitcommit: a7eb2f6c4465527cca2d479edbfc9d93d1e44bf1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94714518"
---
# <a name="moodle-migration-tasks-architecture-and-template"></a>Moodle 遷移工作、架構和範本

## <a name="moodle-migration-task-outline"></a>Moodle 遷移工作大綱

Moodle 遷移包含下列工作：

- 使用 Azure Resource Manager 範本部署 Azure 基礎結構。
- 下載並安裝 AzCopy。
- 從 Azure Resource Manager 部署將備份封存複製到控制器虛擬機器實例。
- Moodle 應用程式和設定的遷移。
- 設定 Moodle 控制器實例和背景工作節點。
- 設定 PHP 和 web 伺服器。

## <a name="deploy-azure-infrastructure-with-azure-resource-manager-templates"></a>使用 Azure Resource Manager 範本部署 Azure 基礎結構

- 使用 Azure Resource Manager 範本將基礎結構部署至 Azure 時，您有幾個選項可供您使用。 下圖提供基礎結構資源的總覽。

![Azure 基礎結構資源。](images/architecture.png)

可完全設定的部署可為部署帶來更多的彈性和選擇。 預先定義的部署大小會使用四個預先定義的 Moodle 大小之一。 四個預先定義的範本選項是最小、最簡單、最大且最大的選項，並可在 [Moodle GitHub 存放庫](https://github.com/Azure/Moodle)中取得。

- [最小](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FMoodle%2Fmaster%2Fazuredeploy-minimal.json)：此部署將使用 NFS、MySQL 和較小的自動調整 web 前端虛擬機器 sku (一個 vCore) ， (少於 30) 分鐘的時間提供更快速的部署時間，而且目前只需要兩部虛擬機器符合 Azure 免費試用訂用帳戶。

- [小型至中](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FMoodle%2Fmaster%2Fazuredeploy-small2mid-noha.json)：最多可支援1000個並行使用者。 此部署將使用 NFS (沒有高可用性) 和 MySQL (8 個虛擬核心) ，而不需要其他選項，例如彈性搜尋或 Redis 快取。

- [大型 (高可用性) ](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FMoodle%2Fmaster%2Fazuredeploy-large-ha.json)：支援超過2000個並行使用者。 此部署會使用 Azure 檔案儲存體、MySQL (16 虛擬核心) 和 Redis 快取，而不需要其他選項，例如彈性搜尋。

- [最大值：此最大](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FMoodle%2Fmaster%2Fazuredeploy-maximal.json)部署將使用 Azure 檔案儲存體、具有最高 SKU 的 MySQL、Redis 快取、彈性搜尋 (三個虛擬機器) ，以及 (資料磁片和資料庫) 的大型儲存體大小。

選取 [ **啟動** ] 以部署任何預先定義的範本。 這會將您導向至 Azure 入口網站，您必須在其中完成必要的欄位，例如 **訂** 用帳戶、 **資源群組**、 [**SSH 金鑰**](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)和 **區域**。

![自訂部署：從自訂範本進行部署。](images/custom-deployment.png)

先前預先定義的範本會部署預設版本。

```bash
Ubuntu: 18.04-LTS
PHP: 7.4
Moodle: 3.8
```

如果 PHP 和 Moodle 版本是以內部部署延遲，則請使用下列步驟來更新版本：

- 選取 [**自訂部署**] 頁面上的 [**編輯範本**]。

![編輯範本：編輯您的 Azure Resource Manager 範本。](images/edit-template.png)

- 在 [ **資源** ] 區段中，將 [Moodle] 和 [PHT] 版本新增至 [ **參數** ] 區塊。

    ```json
    "phpVersion":       { "value": "7.2" },
    "moodleVersion":    { "value": "MOODLE_38_STABLE"}
    ```

- 針對 Moodle 3.9，此值應為 `MOODLE_39_STABLE` 。

- 選取 [儲存]  來儲存變更。

## <a name="next-steps"></a>後續步驟

如需 Moodle 遷移程式的詳細資訊，請繼續 [Moodle 遷移資源](./migration-resources.md) 。
