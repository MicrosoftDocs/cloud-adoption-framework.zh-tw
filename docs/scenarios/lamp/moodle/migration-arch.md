---
title: Moodle 遷移架構和範本
description: 瞭解 Azure Resource Manager (ARM) 範本以 Moodle Azure 基礎結構部署，以及如何部署或編輯它們。
author: BrianBlanchard
ms.author: brblanch
ms.date: 11/30/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.openlocfilehash: e36261c7021f970d4ca94903fe5260a7f787803f
ms.sourcegitcommit: 18f3ee8fcd8838f649cb25de1387b516aa23a5a0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2020
ms.locfileid: "96327843"
---
# <a name="moodle-migration-architecture-and-templates"></a>Moodle 遷移架構和範本

Moodle 遷移包含下列工作：

1. 使用 Azure Resource Manager (ARM) 範本來部署 Azure 基礎結構。
1. [下載並安裝 AzCopy](migration-start.md#download-and-install-azcopy-on-the-controller-vm)。
1. [將 Moodle 備份封存複製到 Azure Resource Manager 部署中的控制器虛擬機器](migration-start.md#copy-the-archive-to-the-controller-vm) 實例。
1. [遷移 Moodle 應用程式和](migration-start.md#import-the-moodle-database-to-azure)設定。
1. [設定 Moodle 控制器實例和背景工作節點](azure-infra-config.md)。
1. [設定 PHP 和 web 伺服器](azure-infra-config.md)。

本文說明如何 Moodle Azure 基礎結構選項，以及如何使用您選擇的 ARM 範本部署您想要的 Azure 資源。

## <a name="azure-infrastructure"></a>Azure 基礎結構

下圖顯示 Azure Moodle 基礎結構資源的總覽：

![顯示 Azure 基礎結構資源的圖表。](images/architecture.png)

## <a name="arm-template-options"></a>ARM 範本選項

若要在 Azure 上部署 Moodle 資源，您可以使用完全可設定的 ARM 範本或數個預先定義的 ARM 範本之一。 可完全設定的部署可為您提供最大的彈性和部署選項。 您可以在 [Moodle GitHub 存放庫](https://github.com/Azure/Moodle)中找到可完全設定的範本和預先定義的範本。

預先定義的部署範本使用四種預先定義的 Moodle 大小之一：最小、最短、最大或最大。

- *最基本的部署* 只需要兩部虛擬機器 (vm) ，因此可搭配 Azure 免費試用訂用帳戶使用。 此部署會使用網路檔案系統 (NFS) 、MySQL 和較小的自動調整 web 前端 VM SKU 與一個 vCore。 此範本的快速部署時間為30分鐘。
  
  [![啟動最基本 Moodle 部署 ARM 範本的按鈕。](images/deploy-to-azure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FMoodle%2Fmaster%2Fazuredeploy-minimal.json)

- *小規模部署* 最多可支援1000個並行使用者。 此部署使用不具高可用性的 NFS，以及在八虛擬核心上的 MySQL。 此部署不包含 Elasticsearch 或 Redis cache 等選項。
  
  [![此按鈕會啟動小型至中型 Moodle 部署 ARM 範本。](images/deploy-to-azure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FMoodle%2Fmaster%2Fazuredeploy-small2mid-noha.json)

- *大型的高可用性部署* 支援超過2000個並行使用者。 此部署使用 Azure 檔案儲存體、具有16虛擬核心的 MySQL，以及 Redis 快取，而不需要其他選項，例如 Elasticsearch。
  
  [![此按鈕會啟動大型的高可用性 Moodle 部署 ARM 範本。](images/deploy-to-azure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FMoodle%2Fmaster%2Fazuredeploy-large-ha.json)

- 最 *大部署會* 使用 Azure 檔案儲存體、具有最高 SKU 的 MySQL、Redis 快取、三個 vm 上的 Elasticsearch，以及資料磁片和資料庫的大型儲存體大小。
  
  [![啟動最大 Moodle 部署 ARM 範本的按鈕。](images/deploy-to-azure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FMoodle%2Fmaster%2Fazuredeploy-maximal.json)

## <a name="deploy-the-template"></a>部署範本

若要部署其中一個預先定義的 ARM 範本：

1. 在上一節中，針對您想要的部署選取 [ **部署至 Azure** ] 按鈕。 此動作會帶您前往 Azure 入口網站。
   
1. 在 Azure 入口網站的 [ **自訂部署** ] 頁面上，完成 [強制 **訂** 用帳戶]、[ **資源群組**]、[ **區域**] 和 [ **Ssh 公用金鑰** ] 欄位。 如需有關如何新增 SSH 金鑰的詳細資訊，請參閱 [產生新的 ssh 金鑰，並將它新增至 ssh 代理程式](https://docs.github.com/free-pro-team@latest/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)。
   
   :::image type="content" source="images/custom-deployment.png" alt-text="顯示 Moodle deployment ARM 範本的 Azure 自訂部署畫面的螢幕擷取畫面。" border="false":::
   
1. 選取 [檢閱 + 建立]。

### <a name="edit-the-template"></a>編輯範本

預先定義的 ARM 範本會部署下列預設軟體版本：

- Ubuntu： 18.04 LTS
- PHP：7。4
- Moodle：3。8

如果您的內部部署 PHP 和 Moodle 版本與先前的值不同，請遵循下列步驟來更新範本版本以符合：

1. 在 Azure 入口網站的 [ARM 範本 **自訂部署** ] 頁面上，選取 [ **編輯範本**]。
   
1. 在範本的 **資源** 區段中，于 [ **參數**] 下，為您的 Moodle 和 PHP 版本新增參數。

   ```json
   "phpVersion":       { "value": "7.2" },
   "moodleVersion":    { "value": "MOODLE_38_STABLE"}
   ```
   
   例如，針對 Moodle 3.9，此 `moodleVersion` 值應為 `MOODLE_39_STABLE` 。
   
   :::image type="content" source="images/edit-template.png" alt-text="顯示 Moodle 部署 ARM 範本的 [編輯範本] 頁面的螢幕擷取畫面。" border="false":::
   
1. 選取 [儲存]。

## <a name="next-steps"></a>後續步驟

繼續 [Moodle 遷移資源](migration-resources.md) 以取得 ARM 範本部署至 Azure 之資源的相關資訊。
