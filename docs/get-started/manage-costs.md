---
title: 開始使用：管理雲端成本
description: 瞭解管理雲端採用相關成本的基本概念。
author: JanetCThomas
ms.author: janet
ms.date: 05/15/2020
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.openlocfilehash: 54d35e4b895961eb290de9ab151fa1332bb0bc8d
ms.sourcegitcommit: 8b82889dca0091f3cc64116f998a3a878943c6a1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/09/2020
ms.locfileid: "89603325"
---
# <a name="get-started-manage-cloud-costs"></a>開始使用：管理雲端成本

雲端治理的成本管理專業領域著重于建立預算、監視成本配置模式，以及實行控制項，以改善整個 IT 組合的雲端費用行為。 企業成本優化牽涉到許多其他角色和功能，以將成本降至最低，並平衡規模、效能、安全性和可靠性的需求。 本文將這些支援函式對應至快速入門手冊，以協助建立相關小組之間的一致。

不過，企業成本優化牽涉到許多其他角色和功能，以將成本降至最低，並平衡規模、效能、安全性和可靠性的需求。 本文會對應這些支援的功能，以協助建立相關小組之間的調整。

治理是在任何大型企業內進行成本優化的基石。 下一節將概述治理內容中的成本優化指導方針。 後續步驟可協助每個小組在成本優化中採取以其角色為目標的動作。 這些步驟可讓您的組織開始進行成本優化的旅程。

![開始使用成本優化](../_images/get-started/cost-map.png)

## <a name="step-1-optimize-enterprise-costs"></a>步驟1：將企業成本優化

雲端治理小組已準備好透過監視效能、減少資源大小調整，以及安全地終止未使用的資源，來評估及處理超支或非計畫的消費。 企業成本優化一開始會先瞭解所需的工具、程式和相依性，以明智的方式在環境層級上採取成本考慮。

**交付：**

- 跨企業執行成本管理原則的明智變更。
- 記錄 [成本管理專業領域範本](../govern/cost-management/template.md)中的成本管理原則、程式和設計指引。

這些交付專案是幾個週期性工作的結果：

- 確定與雲端策略小組的策略性一致 (，其中包括組合) 的工作負載專案關係人。
- 優化整個環境的成本：
  - 手動或自動關閉未使用的 Vm。
  - 刪除或解除配置已停止的 Vm。
  - 確定適當的資源大小。
  - 將支出與預算的期望保持一致。
- 使用 Microsoft Azure 架構良好的審查來驗證任何架構變更，以促進與工作負載技術擁有者的交談。

**支援交付完成的指導方針：**

- 確定所有工作負載和資源都遵循 [適當的命名和標記慣例](../ready/azure-best-practices/naming-and-tagging.md)。 使用 Azure 原則，並特別強調「成本中心」和「技術擁有者」的標記，[來強制執行標記慣例](/azure/governance/policy/tutorials/govern-tags)。
- 定期檢查並套用 [成本管理專業領域的最佳做法](../govern/cost-management/best-practices.md) ，以引導整個企業的分析和改進。 重要治理作法包括：

  - 以 [一般成本最佳作法](../govern/cost-management/best-practices.md) 為依據，以降低調整大小和成本，並停止未使用的電腦。
  - 運用 [混合式使用權益](../govern/cost-management/best-practices.md#best-practice-take-advantage-of-azure-hybrid-benefit) 來降低授權成本。
  - 將 [保留實例](../govern/cost-management/best-practices.md#best-practice-use-azure-reserved-vm-instances) 對齊，以降低資源成本。
  - [監視資源使用率](../govern/cost-management/best-practices.md#best-practice-monitor-resource-utilization) ，以將資源效能的影響降到最低。
  - 透過原則來[降低非生產成本](../govern/cost-management/best-practices.md#best-practice-reduce-nonproduction-costs)，以管理非生產環境。
  - 針對 [成本優化建議](/azure/cost-management-billing/costs/tutorial-acm-opt-recommendations?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)採取行動。

- 您可能需要在工作負載層級進行取捨，以實行有效的成本優化變更。 [Microsoft Azure 架構良好的架構](/azure/architecture/framework/cost/tradeoffs)，並[Microsoft Azure 架構良好的審查](/assessments/?id=azure-architecture-review)，可協助引導這些交談與特定工作負載的技術擁有者。
- 如果您不熟悉雲端治理，請使用管理方法來建立 [治理原則、程式和專業領域](../govern/index.md) 。
- 如果您不熟悉成本管理專業領域，請考慮遵循 [成本管理專業領域的改進文章](../govern/guides/complex/cost-management-improvement.md)，並將焦點放在 [ [執行](../govern/guides/complex/cost-management-improvement.md#incremental-improvement-of-the-best-practices) ] 區段。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端治理小組 | <li> 雲端策略小組 <li> 雲端採用小組 <li> 卓越或中央 IT 團隊的雲端中心 |

治理小組可以跨多數企業偵測和推動大量的成本優化。 基本、資料驅動的資源大小調整可能會對成本產生立即且可測量的影響。

如同建立符合 [成本的組織](../organize/cost-conscious-organization.md)所述，企業級的成本管理和成本優化的焦點可以提供更多價值。 下列步驟示範各種團隊如何協助打造符合成本的組織。

## <a name="step-2-define-a-strategy"></a>步驟2：定義策略

策略性決策會直接影響成本控制，並透過採用生命週期和長期作業波及各。 策略性的清楚可改善由治理小組推動的成本優化工作。

**交付：**

- 記錄 [策略和方案範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx)中的動機、結果和業務理由。
- 使用 Azure 成本管理和帳單來建立您的第一個預算。

**支援交付完成的指導方針：**

- [瞭解動機](../strategy/motivations.md)。 重要的商務事件和某些遷移動機通常會受到成本影響，進而提升成本控制在所有後續工作的重要性。 其他與創新或成長有關的向前搜尋動機，可能會著重于頂尖的收入。 瞭解動機可協助您決定成本管理的優先順序高。
- [商務成果](../strategy/business-outcomes/index.md)。 某些會計結果通常會非常符合成本效益。 當想要的結果對應至會計計量時，您應該提早投資成本管理治理專業領域。
- [業務理由](../strategy/cloud-migration-business-case.md)。 商業理由可作為雲端採用的財務計畫的高階觀點。 這是初始預算工作的絕佳來源。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端治理小組 <li> 雲端採用小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-3-develop-a-cloud-adoption-plan"></a>步驟3：開發雲端採用方案

採用方案提供採用期間啟用時間表的清楚明瞭。 調整方案和數位資產分析可讓您預測每月的消費成長。 它也可協助您的雲端治理小組調整流程，並識別消費模式。

**交付：**

- 完成建立 [雲端採用方案](../plan/plan-intro.md#build-your-cloud-adoption-plan)的步驟1到6。
- 與您的雲端治理小組合作，以精簡預算並建立實際的消費預測。

**支援交付完成的指導方針：**

- [收集清查](../digital-estate/inventory.md)。 建立資料來源，以在採用之前分析數位資產。
- [最佳做法： Azure Migrate](../plan/contoso-migration-assessment.md)。 使用 Azure Migrate 來收集清查。
- [增量合理化](../digital-estate/rationalize.md#incremental-rationalization)。 在增量合理化和量化分析期間，請找出用於預算的雲端候選項目。
- [調整成本模型和預測模型](../digital-estate/calculate.md)。 使用 Azure 成本管理和帳單，藉由 [建立預算](/azure/cost-management-billing/costs/tutorial-acm-create-budgets?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)來調整成本和預測模型。
- [打造您的雲端採用計畫](../plan/plan-intro.md#build-your-cloud-adoption-plan)。 使用可採取動作的工作負載、資產和時間軸詳細資料來建立方案。 此計畫可提供花費在時間 (或成本預測) 的基礎。 *花費一段時間* ，是成本管理治理專業領域內所有可操作優化分析的初始基準。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 <li> 雲端治理小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-4-implement-best-practices-for-landing-zones"></a>步驟4：執行登陸區域的最佳作法

適用于 Azure 的 Microsoft 雲端採用架構的準備方法，主要著重于登陸區域的開發，以裝載雲端中的工作負載。 在登陸區域的執行期間，組織應考慮各種成本優化的決策。

**交付：**

- 部署一或多個可在短期採用方案中裝載工作負載的登陸區域。
- 確定所有登陸區域符合成本優化決策和成本管理需求。

**支援交付完成的指導方針：**

- [追蹤成本](../ready/azure-best-practices/track-costs.md#provide-the-right-level-of-cost-access)。 建立妥善管理的環境階層、提供正確的成本存取層級，並在每個登陸區域中使用額外的成本管理資源。
- [優化您的雲端投資](/azure/cost-management-billing/costs/cost-mgt-best-practices?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。 瞭解將投資優化的最佳做法。
- [建立及管理預算](/azure/cost-management-billing/costs/tutorial-acm-create-budgets?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。 瞭解建立及管理預算的最佳作法。
- [從建議中將成本優化](/azure/cost-management-billing/costs/tutorial-acm-opt-recommendations?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。 瞭解使用可將成本優化的建議最佳作法。
- [監視使用量和支出](/azure/cost-management-billing/costs/cost-mgt-alerts-monitor-usage-spending?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。 瞭解在登陸區域內監視使用量和支出的最佳作法。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 <li> 雲端治理小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-5-complete-waves-of-migration-effort"></a>步驟5：遷移工作的完整波

遷移是雲端採用小組所執行的可重複流程。 在整個過程中，有許多機會可將成本優化于您的組合。 其中許多這些內含式決策會套用至每個遷移 wave 或反復專案期間的一小組工作負載。

**交付：**

- 將完整優化工作負載的集合進行基準測試、測試、調整大小及部署。

**支援交付完成的指導方針：**

- 以[遷移為導向的成本控制機制](../migrate/azure-migration-guide/manage-costs.md)可提供雲端原生成本優化控制項的深入解析，以在遷移期間提供協助。
- 將已[遷移工作負載的成本優化的最佳作法](../migrate/azure-best-practices/migrate-best-practices-costs.md)包含了14個最佳作法的檢查清單，可在遷移之前和之後追蹤，以將每個工作負載版本的成本優化最大化。

長期操作成本是每個遷移程式改進領域中的常見主題。 這份進程改進清單是由遷移程式的階段所組成：

- [必要條件](../migrate/migration-considerations/prerequisites/index.md) 提供管理變更和待處理專案（backlog）的相關資訊，這些變更會影響預算和實際的雲端成本。
- [評](../migrate/migration-considerations/assess/index.md) 量提供六個特定的流程，從驗證假設到了解合作夥伴選項。 每個流程都會影響雲端優化的機會。
- [遷移](../migrate/migration-considerations/migrate/index.md) 包含補救資產的一個處理建議。 這項建議讓您有機會將已設定的狀態優化，以利優化的解決方案。
- 將[焦點放](../migrate/migration-considerations/optimize/index.md)在測試、調整、驗證和釋出已遷移的資產，以及解除委任淘汰的資產。 這是預測和預算可針對實際效能和設定進行測試的第一個明確點。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 <li> 雲端治理小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-6-drive-customer-focused-innovation"></a>步驟6：推動客戶導向的創新

創新和開發新產品需要更深入的架構審查。 雲端採用架構提供有關創新程式和產品管理思考的詳細資料。 有關創新的成本優化決策大多都不在本指南的討論範圍內。

**交付：**

- 進行有關創新的主要架構決策，以平衡成本和其他重要的設計考慮。

**支援交付完成的指導方針：**

- 使用 [Microsoft Azure 架構良好的評論](/assessments/?id=azure-architecture-review) ，以瞭解架構決策的平衡。
- 複習 [Microsoft Azure 架構良好的架構](/azure/architecture/framework) ，以取得創新期間成本優化的更深入指引。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 <li> 雲端治理小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-7-implement-sound-operations"></a>步驟7：執行音效作業

建立穩固的管理基準可協助您收集資料和建立操作警示。 這項努力有助於偵測機會將成本優化。 它會在復原和成本優化之間建立平衡。

**交付：**

- 監視您的企業環境是否有持續的建議，以將成本優化，並符合每個工作負載的重要性和復原分類。

**支援交付完成的指導方針：**

- [建立商務調整](../manage/considerations/business-alignment.md) ，以取得有關復原投資的重要性和胃口的清楚資訊。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端營運小組 | <li> 雲端策略小組 <li> 雲端治理小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="value-statement"></a>Value 語句

遵循這些步驟可協助您 [建立符合成本的組織](../organize/cost-conscious-organization.md)。 使用共用擁有權，並在適當的時間與適當的小組合作，以簡化成本優化。
