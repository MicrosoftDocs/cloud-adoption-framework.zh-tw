---
title: 在 Azure 中部署移轉登陸區域
description: 了解如何在 Azure 中部署移轉登陸區域。
author: BrianBlanchard
ms.author: brblanch
ms.date: 02/25/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 601481112f2d8144596951e1a68bd7d0bda0b95a
ms.sourcegitcommit: 7660521b631ea092fb805df9c9d28ad3024287ff
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/19/2020
ms.locfileid: "83621756"
---
<!-- cSpell:ignore vCPUs jumpbox -->

# <a name="deploy-a-migration-landing-zone"></a>部署移轉登陸區域

「_遷移」登陸區域_是已布建並準備要裝載從內部部署環境遷移至 Azure 之工作負載的環境。

## <a name="deploy-the-first-landing-zone"></a>部署第一個登陸區域

在您使用雲端採用架構中的「遷移」登陸區域藍圖之前，請先參閱下列假設、決策和實施指引。 如果本指南與所需的雲端採用方案一致，則可以使用[部署步驟][deploy-sample]來部署[遷移登陸區域藍圖](https://docs.microsoft.com/azure/governance/blueprints/samples/caf-migrate-landing-zone)。

> [!div class="nextstepaction"]
> [部署藍圖範例][deploy-sample]

## <a name="assumptions"></a>假設

這個初始登陸區域包含下列假設或條件約束。 如果這些假設符合您的條件約束，您可以使用藍圖來建立您的第一個登陸區域。 藍圖也可以加以擴充，以建立符合您特有條件限制的登陸區域藍圖。

- **訂用帳戶限制：** 這種採用成果不應超過[訂](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits)用帳戶限制。
- **合規性：** 此登陸區域不需要協力廠商合規性需求。
- **架構複雜度：** 架構複雜度不需要額外的生產訂用帳戶。
- **共用服務：** 在 Azure 中，沒有任何現有的共用服務需要將此訂用帳戶視為中樞和輪輻架構中的輪輻。
- **有限的生產範圍：** 此登陸區域可能會主控生產工作負載。 這不適合用于敏感性資料或任務關鍵性工作負載的環境。

如果這些假設符合您目前的採用需求，則此藍圖可能是建立登陸區域的起點。

## <a name="decisions"></a>決策

下列決策會在登陸區域藍圖中表示。

| 元件                    | 決策                                                                                         | 替代方法                                                                                                                                                                                                                                                                |
|------------------------------|---------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 移轉工具              | 將會部署 Azure Site Recovery，並建立 Azure Migrate 專案。                | [移轉工具決策指南](../../decision-guides/migrate-decision-guide/index.md)                                                                                                                                                                                               |
| 記錄和監視       | 將會布建 Operational Insights 工作區和診斷儲存體帳戶。                |                                                                                                                                                                                                                                                                                       |
| 網路                      | 將會建立包含子網路的虛擬網路，以用於閘道、防火牆、jumpbox 和登陸區域。  | [網路決策](../considerations/networking-options.md)                                                                                                                                                                                                                       |
| 身分識別                     | 假設訂用帳戶已經與 Azure Active Directory 執行個體相關聯。 | [身分識別管理最佳做法](https://docs.microsoft.com/azure/security/fundamentals/identity-management-best-practices?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) |
| 原則                       | 此藍圖目前會假設未套用任何 Azure 原則。                        |                                                                                                                                                                                                                                                                                       |
| 訂用帳戶設計          | N/A - 專為單一生產訂用帳戶所設計。                                              | [建立初始訂閱](../azure-best-practices/initial-subscriptions.md)                                                                                                                                                                                                      |
| 資源群組              | N/A - 專為單一生產訂用帳戶所設計。                                              | [調整訂閱](../azure-best-practices/scale-subscriptions.md)                                                                                                                                                                                                                 |
| 管理群組            | N/A - 專為單一生產訂用帳戶所設計。                                              | [組織和管理訂閱](../azure-best-practices/organize-subscriptions.md)                                                                                                                                                                                                |
| 資料                         | N/A                                                                                               | 在 Azure 和[Azure 資料存放區](https://docs.microsoft.com/azure/architecture/guide/technology-choices/data-store-overview)[中選擇正確的 SQL Server 選項](https://docs.microsoft.com/azure/sql-database/sql-database-paas-vs-sql-server-iaas)                       |
| 儲存體                      | N/A                                                                                               | [Azure 儲存體指導方針](../considerations/storage-options.md)                                                                                                                                                                                                                        |
| 命名和標記標準 | N/A                                                                                               | [命名和標記最佳做法](../azure-best-practices/naming-and-tagging.md)                                                                                                                                                                                                    |
| 成本管理              | N/A                                                                                               | [追蹤成本](../azure-best-practices/track-costs.md)                                                                                                                                                                                                                              |
| 計算                      | N/A                                                                                               | [計算選項](../considerations/compute-options.md)                                                                                                                                                                                                                               |

## <a name="customize-or-deploy-a-landing-zone"></a>自訂或部署登陸區域

深入瞭解並下載 CAF 遷移登陸區域藍圖的參考範例，以從[Azure 藍圖範例][deploy-sample]進行部署或自訂。

> [!div class="nextstepaction"]
> [部署藍圖範例][deploy-sample]

如需應對此藍圖或產生的登陸區域進行自訂的指引，請參閱[登陸區域考慮](../considerations/index.md)。

## <a name="next-steps"></a>後續步驟

部署您的第一個登陸區域之後，您就可以開始[擴充您的登陸區域](../considerations/index.md)

> [!div class="nextstepaction"]
> [擴充登陸區域](../considerations/index.md)

<!-- links -->

[deploy-sample]: https://docs.microsoft.com/azure/governance/blueprints/samples/caf-migrate-landing-zone/deploy
