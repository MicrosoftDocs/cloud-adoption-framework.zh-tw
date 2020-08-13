---
title: 開始使用：管理雲端成本
description: 瞭解管理與雲端採用相關之成本的基本概念。
author: JanetCThomas
ms.author: janet
ms.date: 05/15/2020
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.openlocfilehash: b2e1b404389d9795787363fb9e88188979089f00
ms.sourcegitcommit: 949b87bad28d32df84df190160089f01826f3a31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2020
ms.locfileid: "88195964"
---
# <a name="get-started-manage-cloud-costs"></a>開始使用：管理雲端成本

雲端治理的成本管理專業領域著重于建立預算、監視成本配置模式，以及實施控制措施，以改善 IT 組合中的雲端支出行為。 企業成本優化牽涉到許多其他角色和函式，以將成本降至最低，並平衡規模、效能、安全性和可靠性的需求。 本文將各種支援功能對應到入門指南，協助建立相關小組之間的對齊。

不過，企業成本優化牽涉到許多其他角色和函式，以將成本降至最低，並平衡規模、效能、安全性和可靠性的需求。 本文會對應這些支援函式，以協助建立相關小組之間的對齊。

治理是在任何大型企業內的成本優化基石。 下一節將概述治理內容中的成本優化指引。 後續步驟可協助每個小組在成本優化中採取以其角色為目標的動作。 這些步驟一起可協助您的組織開始進行成本優化的旅程。

![企業成本管理入門](../_images/get-started/cost-map.png)

## <a name="step-1-optimize-enterprise-costs"></a>步驟1：優化企業成本

雲端治理小組已準備好透過監視效能、減少資源大小，以及安全地終止未使用的資源，來評估和採取超支或未計畫的費用。 企業成本優化的一開始，是由共同團隊瞭解在環境層級上明智地處理成本考慮所需的工具、程式和相依性。

<!-- docsTest:ignore "your cost management policies" -->

**項**

- 在整個企業中執行明智的成本管理變更。
- 記錄 [成本管理專業領域範本](../govern/cost-management/template.md)中的成本管理原則、程式和設計指引。

這些交付專案是幾個週期性工作的結果：

- 確保與雲端策略小組 (的策略性一致，包括組合) 的工作負載專案關係人。
- 將整個環境的成本優化：
  - 手動或自動關閉未使用的 Vm。
  - 刪除或解除配置已停止的 Vm。
  - 確保適當的資源大小。
  - 將支出與預算的預期保持一致。
- 藉由使用 Microsoft Azure 妥善架構的審查來驗證任何架構變更，以加速與工作負載的技術擁有者交談。

**支援交付後完成的指引：**

- 確保所有的工作負載和資源都遵循 [適當的命名和標記慣例](../ready/azure-best-practices/naming-and-tagging.md)。 藉由使用 Azure 原則與「成本中心」和「技術擁有者」標記的特定強調，[來強制執行標記慣例](https://docs.microsoft.com/azure/governance/policy/tutorials/govern-tags)。
- 定期檢查並套用 [成本管理最佳作法](../govern/cost-management/best-practices.md) ，以引導整個企業的分析和改進。 以下是幾個最影響力的治理作法：

  - 根據 [一般成本最佳做法](../govern/cost-management/best-practices.md) 採取行動，以降低大小和成本，並停止未使用的機器。
  - 套用 [混合式使用優勢](../govern/cost-management/best-practices.md#best-practice-take-advantage-of-azure-hybrid-benefit) ，以降低授權成本。
  - 對齊 [保留實例](../govern/cost-management/best-practices.md#best-practice-use-azure-reserved-vm-instances) ，以降低資源成本。
  - [監視資源使用率](../govern/cost-management/best-practices.md#best-practice-monitor-resource-utilization) ，以將對資源效能的影響降至最低。
  - 透過原則來管理非生產環境，以[降低非生產成本](../govern/cost-management/best-practices.md#best-practice-reduce-nonproduction-costs)。
  - 針對 [成本優化建議](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-acm-opt-recommendations?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)採取行動。

- 可能需要在工作負載層級進行取捨，才能執行有效的成本優化變更。 [Microsoft Azure 架構完善的架構](https://docs.microsoft.com/azure/architecture/framework/cost/tradeoffs)和[Microsoft Azure 架構良好的審查](https://docs.microsoft.com/assessments/?id=azure-architecture-review)，可以協助引導這些與特定工作負載的技術擁有者交談。
- 如果您不熟悉雲端治理，請使用管控方法來建立 [治理原則、流程和專業領域](../govern/index.md) 。
- 如果您不熟悉成本管理專業領域，請考慮遵循 [成本管理專業領域改進文章](../govern/guides/complex/cost-management-improvement.md)，並將焦點放在 [ [執行](../govern/guides/complex/cost-management-improvement.md#incremental-improvement-of-the-best-practices) ] 區段。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端治理小組 | <li> 雲端策略小組 <li> 雲端採用小組 <li> 卓越或中央 IT 小組的雲端中心 |

治理小組可以在大部分的企業中偵測並推動重大的成本優化。 「基本」、「資料驅動」的資源調整大小，對於成本有立即且可測量的影響。

如組建符合 [成本效益的組織](../organize/cost-conscious-organization.md)中所述，企業範圍的焦點在於成本管理和成本優化，可以提供更多價值。 下列步驟示範各種小組可以協助打造符合成本效益的組織的方式。

## <a name="step-2-define-a-strategy"></a>步驟2：定義策略

策略性決策會直接影響成本控制，並透過採用週期和長期作業進行波及各。 策略性的清楚會改善由治理小組推動的成本優化工作。

**項**

- 記錄 [策略和計畫範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx)中的動機、結果和業務理由。
- 使用 Azure 成本管理建立您的第一個預算。

**支援交付後完成的指引：**

- [瞭解動機](../strategy/motivations.md)。 重要的商務事件和一些遷移動機通常會對成本產生影響，因而提高了成本控制的重要性，以供之後所有的工作之用。 其他與創新或透過遷移成長相關的正向動機，可能會更專注于頂尖利潤。 瞭解動機將可協助您瞭解優先順序成本管理有多高。
- [商務成果](../strategy/business-outcomes/index.md)。 某些會計結果通常會非常符合成本效益。 當想要的結果對應至會計計量時，您應該儘早投入成本管理治理專業領域。
- [商業理由](../strategy/cloud-migration-business-case.md)。 商業理由是做為雲端採用之財務計畫的高階觀點。 這是初步預算投入的絕佳來源。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端治理小組 <li> 雲端採用小組 <li> 卓越或中央 IT 小組的雲端中心 |

## <a name="step-3-develop-a-cloud-adoption-plan"></a>步驟3：開發雲端採用方案

採用方案可讓您清楚理解採用期間的啟用時間軸。 調整方案和數位資產分析，可讓您預測每月的支出成長。 此外，它也可協助您的雲端治理小組配合處理常式，並識別消費模式。

**項**

- 完成建立 [雲端採用方案](../plan/plan-intro.md#build-your-cloud-adoption-plan)的步驟1到6。
- 與您的雲端治理小組合作，以精簡預算並建立實際的支出預測。

**支援交付後完成的指引：**

- [收集清查](../digital-estate/inventory.md)。 建立資料來源，以在採用之前分析數位資產。
- [最佳做法： Azure Migrate](../plan/contoso-migration-assessment.md)。 使用 Azure Migrate 來收集清查。
- [增量合理化](../digital-estate/rationalize.md#incremental-rationalization)。 在增量合理化和量化分析期間，識別用於預算的雲端候選項目。
- [對齊成本模型和預測模型](../digital-estate/calculate.md)。 藉由 [建立預算](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-acm-create-budgets?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)，使用 Azure 成本管理來對齊成本和預測模型。
- [建立您的雲端採用方案](../plan/plan-intro.md#build-your-cloud-adoption-plan)。 建立具有可採取動作之工作負載、資產和時間軸詳細資料的方案。 此計畫可讓您在一段時間內花費 (或成本預測) 的基礎。 _經過一段時間的花費_ 是成本管理治理專業領域中所有可採取動作的優化分析的初始基準。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 <li> 雲端治理小組 <li> 卓越或中央 IT 小組的雲端中心 |

## <a name="step-4-implement-best-practices-for-landing-zones"></a>步驟4：執行登陸區域的最佳做法

適用于 Azure 的 Microsoft Cloud 採用架構的準備方法，著重于在登陸區域的開發，以裝載雲端中的工作負載。 在登陸區域的執行期間，組織應考慮成本優化的各種決策。

**項**

- 部署一或多個可在短期採用計畫中裝載工作負載的登陸區域。
- 確保所有登陸區域都符合成本優化決策和成本管理需求。

**支援交付後完成的指引：**

- [追蹤成本](../ready/azure-best-practices/track-costs.md#provide-the-right-level-of-cost-access)。 建立妥善管理的環境階層、提供正確的成本存取層級，並在每個登陸區域中使用額外的成本管理資源。
- [優化您的雲端投資](https://docs.microsoft.com/azure/cost-management-billing/costs/cost-mgt-best-practices?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。 瞭解優化投資的最佳做法。
- [建立及管理預算](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-acm-create-budgets?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。 瞭解建立和管理預算的最佳做法。
- [從建議中將成本優化](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-acm-opt-recommendations?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。 瞭解使用可將成本優化之建議的最佳作法。
- [監視使用量和費用](https://docs.microsoft.com/azure/cost-management-billing/costs/cost-mgt-alerts-monitor-usage-spending?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。 瞭解在登陸區域內監視使用量和費用的最佳做法。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 <li> 雲端治理小組 <li> 卓越或中央 IT 小組的雲端中心 |

## <a name="step-5-complete-waves-of-migration-effort"></a>步驟5：完成遷移作業的波浪

「遷移」是由雲端採用小組執行的可重複處理常式。 在整個過程中，有許多機會可將整個組合的成本優化。 這些同進程的其中許多決策會在每個遷移波或反復專案期間套用至一小組工作負載。

**項**

- 基準測試、測試、調整大小，以及部署一系列完全優化的工作負載。

**支援交付後完成的指引：**

- 以[遷移為重點的成本控制機制](../migrate/azure-migration-guide/manage-costs.md)可提供雲端原生成本優化控制項的深入解析，以協助進行遷移。
- [優化遷移工作負載成本的最佳作法](../migrate/azure-best-practices/migrate-best-practices-costs.md) 包含在遷移前後執行14個最佳做法的檢查清單，以將每個工作負載發行的成本優化最大化。

長期營運成本是遷移程式改良功能的常見主題。 此程式改進清單是由遷移程式的階段所組成：

- [必要條件](../migrate/migration-considerations/prerequisites/index.md) 提供管理變更和待處理專案的相關資訊，這會影響預算和實際的雲端成本。
- [評](../migrate/migration-considerations/assess/index.md) 等提供六種特定的流程，從驗證假設到了解合作夥伴選項。 每個處理常式都會影響雲端優化的機會。
- 「[遷移](../migrate/migration-considerations/migrate/index.md)」包含一個有關補救資產的處理建議。 這項建議可讓您有機會將已設定的狀態優化，而改用優化的解決方案。
- [升](../migrate/migration-considerations/optimize/index.md) 階著重于測試、調整大小、驗證和發行已遷移的資產，以及解除委任已淘汰的資產。 這是可針對實際效能和設定測試預測和預算的第一個明確點。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 <li> 雲端治理小組 <li> 卓越或中央 IT 小組的雲端中心 |

## <a name="step-6-drive-customer-focused-innovation"></a>步驟6：推動客戶導向的創新

創新和開發新產品需要更深入的架構審查程度。 雲端採用架構提供有關創新程式和產品管理思考的詳細資料。 有關創新的成本優化決策主要不在本指南的範圍內。

**項**

- 進行創新的關鍵架構決策，以平衡成本和其他重要的設計考慮。

**支援交付後完成的指引：**

- 使用 [Microsoft Azure 妥善架構的審查](https://docs.microsoft.com/assessments/?id=azure-architecture-review) ，以瞭解架構決策的平衡。
- 如需創新期間成本優化的深入指引，請參閱 [Microsoft Azure 架構完善的架構](https://docs.microsoft.com/azure/architecture/framework) 。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 <li> 雲端治理小組 <li> 卓越或中央 IT 小組的雲端中心 |

## <a name="step-7-implement-sound-operations"></a>步驟7：執行音效作業

建立穩固的管理基準，可協助您收集資料並建立操作警示。 這項作業有助於偵測可將成本優化的機會。 它會在復原和成本優化之間建立平衡。

**項**

- 監視您的企業環境以取得持續的建議，以將成本優化，並與每個工作負載的重要性和復原分類一致。

**支援交付後完成的指引：**

- [建立商務對齊](../manage/considerations/business-alignment.md) ，以清楚瞭解復原投資的重要性和胃口。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端營運小組 | <li> 雲端策略小組 <li> 雲端治理小組 <li> 卓越或中央 IT 小組的雲端中心 |

## <a name="value-statement"></a>Value 語句

遵循這些步驟可協助您 [打造符合成本效益的組織](../organize/cost-conscious-organization.md)。 當組織使用共用擁有權，以及在適當時機與適當小組共同合作時，成本優化比較容易實行。
