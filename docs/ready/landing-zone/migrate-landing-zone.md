---
title: 在 Azure 中部署移轉登陸區域
description: 了解如何在 Azure 中部署移轉登陸區域。
author: BrianBlanchard
ms.author: brblanch
ms.date: 02/25/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 828b61ae6064e4c3b00fb0248900fe8f8cbab59f
ms.sourcegitcommit: 949b87bad28d32df84df190160089f01826f3a31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2020
ms.locfileid: "88192899"
---
# <a name="deploy-a-migration-landing-zone-in-azure"></a>在 Azure 中部署移轉登陸區域

「遷移」登陸區域是已布建並準備要裝載從內部部署環境遷移至 Azure 之工作負載的環境。

## <a name="deploy-the-blueprint"></a>部署藍圖

在您使用雲端採用架構中的 CAF 遷移登陸區域藍圖之前，請先參閱下列設計原則、假設、決策和實施指引。 如果本指南與所需的雲端採用方案一致，則可以使用部署步驟來部署 [CAF 遷移登陸區域藍圖](https://docs.microsoft.com/azure/governance/blueprints/samples/caf-migrate-landing-zone) 。

> [!div class="nextstepaction"]
> [部署藍圖範例](https://docs.microsoft.com/azure/governance/blueprints/samples/caf-migrate-landing-zone/deploy)

## <a name="design-principles"></a>設計原則

此實作為選項提供固定方法，可供所有 Azure 登陸區域共用的通用設計區域使用。 如需額外的技術詳細資料，請參閱下列假設和決策。

### <a name="deployment-options"></a>部署選項

此實施選項會部署 (MVP) 的最小可行產品，以開始進行遷移。 當遷移進行時，客戶會遵循以模組化重構為基礎的方法，以平行指引來成熟作業模型，使用控管 [方法](../../govern/index.md) 和 [管理方法](../../manage/index.md) 來平行處理這些複雜主題，以進行初始遷移工作。

下列的 [決策](#decisions) 一節會概述此 MVP 方法所部署的特定資源。

### <a name="enterprise-enrollment"></a>企業註冊

此執行選項不會在企業註冊上採用固有的位置。 這種方法是設計成適用于客戶，而不論 Microsoft 或 Microsoft 合作夥伴的合約協定為何。 在部署此實作為選項之前，會假設客戶已建立目標訂用帳戶。

### <a name="identity"></a>身分識別

此實作為選項假設目標訂閱已根據身分[識別管理最佳做法](https://docs.microsoft.com/azure/security/fundamentals/identity-management-best-practices?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)，與 Azure Active Directory 實例相關聯

### <a name="network-topology-and-connectivity"></a>網路拓樸和連線能力

此 [執行] 選項會建立一個虛擬網路，其中包含閘道、防火牆、跳躍箱和登陸區域的子網。 在下一個步驟中，小組會遵循 [網路決策指南](../considerations/networking-options.md) ，在閘道子網與其他網路之間執行適當的連線形式，以配合 [網路安全性最佳作法](https://docs.microsoft.com/azure/security/fundamentals/network-best-practices?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。

### <a name="resource-organization"></a>資源組織

此 [執行] 選項會建立單一登陸區域，其中的資源會組織成特定資源群組所定義的工作負載。 為資源組織選擇此極簡方法，將會延遲資源組織的技術決策，直到小組的雲端操作模型更清楚地定義為止。

此方法是根據雲端採用工作不會超過訂用帳戶 [限制](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits)的假設。 此選項也會假設此登陸區域內的架構複雜性和安全性需求受到限制。

如果這項變更是透過雲端採用計畫的過程進行，則資源組織可能需要使用控管 [方法](../../govern/index.md)中的指引來重構。

### <a name="governance-disciplines"></a>治理專業領域

此執行選項不會執行任何治理工具。 如果沒有定義的原則自動化，此登陸區域不應用於任何任務關鍵性工作負載或敏感性資料。 這會假設此登陸區域用於有限的生產環境部署，以平行方式起始學習、反復執行及開發整體作業模型，以進行這些早期階段的遷移工作。

若要加速治理專業領域的平行開發，請參閱 [管理方法](../../govern/index.md) ，並考慮部署 [CAF Foundation 藍圖](./foundation-blueprint.md) ，以及 CAF 遷移登陸區域藍圖。

> [!WARNING]
> 隨著治理專業領域的成熟，可能需要重構。 可能需要重構。 具體而言，資源稍後可能需要 [移至新的訂用帳戶或資源群組](https://docs.microsoft.com/azure/azure-resource-manager/management/move-resource-group-and-subscription?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。

### <a name="operations-baseline"></a>作業基準

此執行選項不會執行任何作業。 如果沒有定義的作業基準，此登陸區域不應用於任何任務關鍵性工作負載或敏感性資料。 這會假設此登陸區域用於有限的生產環境部署，以平行方式起始學習、反復執行及開發整體作業模型，以進行這些早期階段的遷移工作。

若要加速並行開發作業基準，請參閱 [管理方法](../../manage/index.md) ，並考慮部署 [Azure 伺服器管理指南](../../manage/azure-server-management/index.md)。

> [!WARNING]
> 當作業基準開發時，可能需要重構。 具體而言，資源稍後可能需要 [移至新的訂用帳戶或資源群組](https://docs.microsoft.com/azure/azure-resource-manager/management/move-resource-group-and-subscription?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。

### <a name="business-continuity-and-disaster-recovery-bcdr"></a>業務持續性和災害復原 (BCDR)

此執行選項不會執行任何 BCDR 解決方案。 這會假設用於保護和復原的解決方案將會由作業基準的開發來解決。

## <a name="assumptions"></a>假設

這個初始登陸區域包含下列假設或條件約束。 如果這些假設符合您的條件約束，您可以使用藍圖來建立您的第一個登陸區域。 藍圖也可以加以擴充，以建立符合您特有條件限制的登陸區域藍圖。

- **訂用帳戶限制：** 這種採用成果不應超過 [訂](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits)用帳戶限制。
- **合規性：** 此登陸區域不需要協力廠商合規性需求。
- **架構複雜度：** 架構複雜度不需要額外的生產訂用帳戶。
- **共用服務：** Azure 中沒有任何現有的共用服務需要將此訂用帳戶視為中樞和輪輻架構中的輪輻。
- **有限的生產範圍：** 此登陸區域可能會主控生產工作負載。 這不適合用于敏感性資料或任務關鍵性工作負載的環境。

如果這些假設符合您目前的採用需求，則此藍圖可能是建立登陸區域的起點。

## <a name="decisions"></a>決策

下列決策會在登陸區域藍圖中表示。

| 元件                    | 決策                                                                                         | 替代方法                                                                                                                                                                                                                                                                |
|------------------------------|---------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 移轉工具              | 將會部署 Azure Site Recovery，並建立 Azure Migrate 專案。                | [移轉工具決策指南](../../decision-guides/migrate-decision-guide/index.md)                                                                                                                                                                                               |
| 記錄和監視       | 將會布建 Operational insights 工作區和診斷儲存體帳戶。                |                                                                                                                                                                                                                                                                                       |
| 網路                      | 系統會使用閘道、防火牆、跳躍箱和登陸區域的子網來建立虛擬網路。  | [網路決策](../considerations/networking-options.md)                                                                                                                                                                                                                       |
| 身分識別                     | 假設訂用帳戶已經與 Azure Active Directory 執行個體相關聯。 | [身分識別管理最佳做法](https://docs.microsoft.com/azure/security/fundamentals/identity-management-best-practices?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) |
| 原則                       | 此藍圖目前會假設未套用任何 Azure 原則。                        |                                                                                                                                                                                                                                                                                       |
| 訂用帳戶設計          | N/A-針對單一生產訂用帳戶所設計。                                              | [建立初始訂閱](../azure-best-practices/initial-subscriptions.md)                                                                                                                                                                                                      |
| 資源群組              | N/A-針對單一生產訂用帳戶所設計。                                              | [調整訂閱](../azure-best-practices/scale-subscriptions.md)                                                                                                                                                                                                                 |
| 管理群組            | N/A-針對單一生產訂用帳戶所設計。                                              | [組織和管理訂閱](../azure-best-practices/organize-subscriptions.md)                                                                                                                                                                                                |
| 資料                         | N/A                                                                                               | 在 Azure 和[azure 資料存放區](https://docs.microsoft.com/azure/architecture/guide/technology-choices/data-store-overview)[中選擇正確的 SQL Server 選項](https://docs.microsoft.com/azure/sql-database/sql-database-paas-vs-sql-server-iaas)                       |
| 儲存體                      | N/A                                                                                               | [Azure 儲存體指導方針](../considerations/storage-options.md)                                                                                                                                                                                                                        |
| 命名和標記標準 | N/A                                                                                               | [命名和標記最佳做法](../azure-best-practices/naming-and-tagging.md)                                                                                                                                                                                                    |
| 成本管理              | N/A                                                                                               | [追蹤成本](../azure-best-practices/track-costs.md)                                                                                                                                                                                                                              |
| 計算                      | N/A                                                                                               | [計算選項](../considerations/compute-options.md)                                                                                                                                                                                                                               |

## <a name="customize-or-deploy-a-landing-zone"></a>自訂或部署登陸區域

深入瞭解並下載 CAF 遷移登陸區域藍圖的參考範例，以便從 Azure 藍圖範例進行部署或自訂。

> [!div class="nextstepaction"]
> [部署藍圖範例](https://docs.microsoft.com/azure/governance/blueprints/samples/caf-migrate-landing-zone/deploy)

如需應對此藍圖或產生的登陸區域進行自訂的指引，請參閱 [登陸區域考慮](../considerations/index.md)。

## <a name="next-steps"></a>後續步驟

部署您的第一個登陸區域之後，您就可以開始擴充您的登陸區域。

> [!div class="nextstepaction"]
> [擴充登陸區域](../considerations/index.md)
