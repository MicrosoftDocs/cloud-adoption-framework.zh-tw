---
title: 在 Azure 中部署 CAF Foundation 藍圖
description: 瞭解如何在 Azure 中部署 CAF 基礎藍圖。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/27/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: ca41e563d3a1be57d8d8e27b15179dfe7a37b6b8
ms.sourcegitcommit: d19b0fc9ef37bf1060fe7595cd2be1612a43ea4a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/04/2020
ms.locfileid: "96605824"
---
<!-- docutune:ignore "CAF Foundation blueprint" -->

# <a name="deploy-a-caf-foundation-blueprint-in-azure"></a>在 Azure 中部署 CAF Foundation 藍圖

CAF 基礎藍圖不會部署登陸區域。 相反地，它會部署建立治理 MVP 所需的工具， (最小可行產品) 開始開發您的治理專業領域。 此藍圖的設計目的是要附加至現有的登陸區域，並可透過單一動作套用至 CAF 遷移登陸區域藍圖。

## <a name="deploy-the-blueprint"></a>部署藍圖

在雲端採用架構中使用 CAF 基礎藍圖之前，請先參閱下列設計原則、假設、決策和實行指引。 如果本指南符合所需的雲端採用方案，則可以使用部署步驟來部署 [CAF 基礎藍圖](/azure/governance/blueprints/samples/caf-foundation) 。

> [!div class="nextstepaction"]
> [部署藍圖範例](/azure/governance/blueprints/samples/caf-foundation/deploy)

## <a name="design-principles"></a>設計原則

此實行選項提供固定方法，以供所有 Azure 登陸區域共用的通用設計區域使用。 如需進一步的技術詳細資料，請參閱下列假設和決策。

### <a name="deployment-options"></a>部署選項

此實行選項會部署 MVP，作為治理專業領域的基礎。 小組將遵循以模組化重構為基礎的方法，使用治理 [方法](../../govern/index.md)來成熟治理專業領域。

### <a name="enterprise-enrollment"></a>企業註冊

此執行選項不會在企業註冊上採用固有的位置。 無論 Microsoft 或 Microsoft 合作夥伴的合約合約為何，此方法的設計都適用于客戶。 在部署此實選項之前，假設客戶已建立目標訂用帳戶。

### <a name="identity"></a>身分識別

此實選項假設目標訂用帳戶已根據身分 [識別管理最佳作法](/azure/security/fundamentals/identity-management-best-practices?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)與 Azure Active Directory 實例相關聯。

### <a name="network-topology-and-connectivity"></a>網路拓樸和連線能力

此實行選項假設登陸區域已根據 [網路安全性最佳作法](/azure/security/fundamentals/network-best-practices?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)，已定義網路拓撲。

### <a name="resource-organization"></a>資源組織

此實選項示範 Azure 原則如何透過標記的應用程式來新增一些資源組織元素。 具體而言， `CostCenter` 會使用 Azure 原則將標記附加至資源。

治理小組應比較和對比資源組織的元素，使其能夠透過標記與應該透過訂用帳戶設計來處理的專案來解決。 這些基本決策會通知資源組織您的雲端採用方案進度。

為了在採用週期早期協助進行這項比較，應考慮下列文章：

- [初始 Azure](../azure-best-practices/initial-subscriptions.md)訂用帳戶：在採用規模的這個階段中，您的作業模型是否需要兩個、三個或四個訂用帳戶？
- [調整](../azure-best-practices/scale-subscriptions.md)訂用帳戶：採用調整規模時，將使用哪些準則來推動訂用帳戶調整？
- [組織](../azure-best-practices/organize-subscriptions.md)訂用帳戶：您要如何在調整規模時組織訂閱？
- [標記標準](../azure-best-practices/resource-tagging.md)：需要在標記中一致地捕捉哪些其他準則以增強您的訂用帳戶設計？

若要在小組進一步配合雲端採用時協助進行這項比較，請參閱《治理指南》中的治理模式一節 [-規範指引](../../govern/guides/complex/prescriptive-guidance.md#application-of-governance-defined-patterns) 文章。 這一節的規範指引會根據特定敘述和作業模型來示範一組模式。 該指導方針也包含其他應考慮之模式的連結。

### <a name="governance-disciplines"></a>治理專業領域

這項實行示範在治理方法的成本管理專業領域中，有一種成熟度的方法。 具體來說，它會示範如何使用 Azure 原則來建立特定 Sku 的允許清單。 限制可部署至登陸區域之資源的類型和大小，可減少超支的風險。

若要加速開發其他治理專業領域的平行開發，請參閱 [管理方法](../../govern/index.md)。 若要繼續成熟治理的成本管理專業領域，請參閱 [成本管理專業領域指南](../../govern/guides/complex/cost-management-improvement.md#incremental-improvement-of-best-practices)。

> [!WARNING]
> 當治理專業領域成熟時，可能需要重構。 可能需要重構。 具體而言，稍後可能需要將資源 [移至新的訂用帳戶或資源群組](/azure/azure-resource-manager/management/move-resource-group-and-subscription?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。

### <a name="operations-baseline"></a>作業基準

此執行選項不會執行任何作業基準的層面。 如果沒有已定義的作業基準，此登陸區域不應該用於任何任務關鍵性工作負載或敏感性資料。 系統會假設此登陸區域是用於有限的生產環境部署，以平行方式來開始學習、反復執行和開發整體作業模型，以平行處理這些早期階段遷移工作。

若要加速操作基準的平行開發，請參閱 [管理方法](../../manage/index.md) ，並考慮部署 [Azure 伺服器管理指南](../../manage/azure-server-management/index.md)。

> [!WARNING]
> 當作業基準進行開發時，可能需要重構。 具體而言，稍後可能需要將資源 [移至新的訂用帳戶或資源群組](/azure/azure-resource-manager/management/move-resource-group-and-subscription?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。

### <a name="business-continuity-and-disaster-recovery-bcdr"></a>業務持續性和災害復原 (BCDR)

此執行選項不會執行任何 BCDR 方案。 系統會假設保護和復原的解決方案將會由作業基準的開發來解決。

## <a name="assumptions"></a>假設

這項初步藍圖假設小組致力於成熟治理功能，以平行處理最初的雲端遷移工作。 如果這些假設符合您的條件約束，您可以使用藍圖來開始開發治理成熟度的流程。

- **合規性：** 此登陸區域不需要任何協力廠商合規性需求。
- **有限的生產環境範圍：** 此登陸區域可能會裝載生產工作負載。 它不適合用于敏感性資料或任務關鍵性工作負載的環境。

如果這些假設與您目前的採用需求一致，則此藍圖可能是建立登陸區域的起點。

## <a name="customize-or-deploy-this-blueprint"></a>自訂或部署此藍圖

深入瞭解並下載 CAF 基礎藍圖的參考範例，以進行部署或自訂 Azure 藍圖範例。

> [!div class="nextstepaction"]
> [部署藍圖範例](/azure/governance/blueprints/samples/caf-foundation/deploy)

## <a name="next-steps"></a>後續步驟

在部署您的第一個登陸區域之後，您就可以開始展開登陸區域。

> [!div class="nextstepaction"]
> [擴充登陸區域](../considerations/index.md)
