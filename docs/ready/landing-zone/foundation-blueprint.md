---
title: 在 Azure 中部署 CAF Foundation 藍圖
description: 瞭解如何在 Azure 中部署 CAF Foundation 藍圖。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/27/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: ea53df3a8d349299e08ecc0681b4dd24dc71336a
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86448209"
---
<!-- docsTest:ignore "CAF Foundation blueprint" -->

# <a name="deploy-a-caf-foundation-blueprint-in-azure"></a>在 Azure 中部署 CAF Foundation 藍圖

CAF Foundation 藍圖不會部署登陸區域。 相反地，它會部署建立治理 MVP （最基本的可行產品）所需的工具，以開始開發您的治理專業領域。 此藍圖的設計目的是要加到現有登陸區域，並可透過單一動作套用至 CAF 遷移登陸區域藍圖。

## <a name="deploy-the-blueprint"></a>部署藍圖

在您使用雲端採用架構中的 CAF Foundation 藍圖之前，請先參閱下列設計原則、假設、決策和實施指引。 如果本指南與所需的雲端採用方案一致，則可以使用[部署步驟][deploy-sample]來部署[CAF Foundation 藍圖](https://docs.microsoft.com/azure/governance/blueprints/samples/caf-foundation)。

> [!div class="nextstepaction"]
> [部署藍圖範例][deploy-sample]

## <a name="design-principles"></a>設計原則

此實作為選項提供固定方法，可供所有 Azure 登陸區域共用的通用設計區域使用。 如需額外的技術詳細資料，請參閱下列假設和決策。

### <a name="deployment-options"></a>部署選項

此實行選項會部署 MVP，做為您治理專業領域的基礎。 小組會遵循以模組化重構為基礎的方法，使用[管理方法](../../govern/index.md)來成熟治理專業領域。

### <a name="enterprise-enrollment"></a>企業註冊

此執行選項不會在企業註冊上採用固有的位置。 這種方法是設計成適用于客戶，而不論 Microsoft 或 Microsoft 合作夥伴的合約協定為何。 在部署此實作為選項之前，會假設客戶已建立目標訂用帳戶。

### <a name="identity"></a>身分識別

此實作為選項假設目標訂閱已經與身分[識別管理最佳作法](https://docs.microsoft.com/azure/security/fundamentals/identity-management-best-practices?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)相關聯 Azure Active Directory 實例。

### <a name="network-topology-and-connectivity"></a>網路拓樸和連線能力

此實作為選項假設登陸區域已根據[網路安全性最佳作法](https://docs.microsoft.com/azure/security/fundamentals/network-best-practices?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)，已有定義的網路拓撲。

### <a name="resource-organization"></a>資源組織

此 [執行] 選項會示範 Azure 原則如何透過標記的應用來新增資源組織的某些元素。 具體而言， `CostCenter` 會使用 Azure 原則將標記附加至資源。

治理小組應該比較和對比資源組織的元素，藉由標記與應該透過訂用帳戶設計來解決的專案進行比對。 這些基本決策會通知資源組織，做為您的雲端採用計畫進度。

為了在早期採用週期中協助進行這項比較，應考慮下列文章：

- [初始 Azure](../azure-best-practices/initial-subscriptions.md)訂用帳戶：在此採用規模的階段中，您的作業模型需要兩個、三個或四個訂用帳戶嗎？
- [調整](../azure-best-practices/scale-subscriptions.md)訂用帳戶：作為採用規模，將使用何種準則來驅動訂閱調整？
- [組織](../azure-best-practices/organize-subscriptions.md)訂用帳戶：您將如何在調整規模時組織訂閱？
- [標記標準](../azure-best-practices/naming-and-tagging.md#metadata-tags)：需要在標籤中一致地捕捉哪些其他準則，以加強您的訂用帳戶設計？

若要在小組進一步配合雲端採用時協助進行這項比較，請參閱[治理指南-規定指引](../../govern/guides/complex/prescriptive-guidance.md#application-of-governance-defined-patterns)一文中的治理模式一節。 規範性指引的這一節會示範一組以特定敘述和操作模式為基礎的模式。 該指引也包含其他應考慮的模式連結。

### <a name="governance-disciplines"></a>治理專業領域

此實施示範了一種方法，可在管控方法的成本管理專業領域中進行成熟度。 具體來說，它會示範如何使用 Azure 原則來建立特定 Sku 的允許清單。 限制可部署到登陸區域的資源類型和大小，會降低超支的風險。

若要加速其他治理專業領域的平行開發，請參閱[管理方法](../../govern/index.md)。 若要繼續成熟治理的成本管理專業領域，請參閱[成本管理專業領域指引](../../govern/guides/complex/cost-management-improvement.md#incremental-improvement-of-the-best-practices)。

> [!WARNING]
> 隨著治理專業領域的成熟，可能需要重構。 可能需要重構。 具體而言，資源稍後可能需要[移至新的訂用帳戶或資源群組](https://docs.microsoft.com/azure/azure-resource-manager/management/move-resource-group-and-subscription?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。

### <a name="operations-baseline"></a>作業基準

此執行選項不會執行任何作業基準的層面。 如果沒有定義的作業基準，此登陸區域不應用於任何任務關鍵性工作負載或敏感性資料。 這會假設此登陸區域用於有限的生產環境部署，以平行方式起始學習、反復執行及開發整體作業模型，以進行這些早期階段的遷移工作。

若要加速並行開發作業基準，請參閱[管理方法](../../manage/index.md)，並考慮部署[Azure 伺服器管理指南](../../manage/azure-server-management/index.md)。

> [!WARNING]
> 當作業基準開發時，可能需要重構。 具體而言，資源稍後可能需要[移至新的訂用帳戶或資源群組](https://docs.microsoft.com/azure/azure-resource-manager/management/move-resource-group-and-subscription?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。

### <a name="business-continuity-and-disaster-recovery-bcdr"></a>業務持續性和災害復原 (BCDR)

此執行選項不會執行任何 BCDR 解決方案。 這會假設用於保護和復原的解決方案將會由作業基準的開發來解決。

## <a name="assumptions"></a>假設

這個初始藍圖假設小組致力於成熟治理功能，以平行處理最初的雲端遷移工作。 如果這些假設符合您的條件約束，您可以使用藍圖來開始開發治理成熟度的程式。

- **合規性：** 此登陸區域不需要協力廠商合規性需求。
- **有限的生產範圍：** 此登陸區域可能會主控生產工作負載。 這不適合用于敏感性資料或任務關鍵性工作負載的環境。

如果這些假設符合您目前的採用需求，則此藍圖可能是建立登陸區域的起點。

## <a name="customize-or-deploy-this-blueprint"></a>自訂或部署此藍圖

深入瞭解並下載 CAF Foundation 藍圖的參考範例，以便從[Azure 藍圖範例][deploy-sample]進行部署或自訂。

> [!div class="nextstepaction"]
> [部署藍圖範例][deploy-sample]

## <a name="next-steps"></a>後續步驟

部署您的第一個登陸區域之後，您就可以開始[擴充您的登陸區域](../considerations/index.md)。

> [!div class="nextstepaction"]
> [擴充登陸區域](../considerations/index.md)

<!-- links -->

[Deploy-sample]: https://docs.microsoft.com/azure/governance/blueprints/samples/caf-foundation/deploy
