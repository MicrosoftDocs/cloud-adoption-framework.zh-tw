---
title: 開始使用：使用正確的控制項提升可靠性
description: 瞭解透過治理控制項和管理基準來提升可靠性的基本概念。
author: JanetCThomas
ms.author: janet
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.openlocfilehash: 385bfcaefd7fcb5dab78e9dd80478a0fe2c7d644
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88569258"
---
# <a name="get-started-improve-reliability-with-the-right-controls"></a>開始使用：使用正確的控制項提升可靠性

如何套用正確的控制項以改善可靠性？ 本文可協助您將與下列專案相關的中斷降到最低：

- 設定中的不一致。
- 資源組織。
- 安全性基準。
- 資源保護。

本文中的步驟可協助營運團隊平衡 IT 組合間的可靠性和成本。 本文也可協助治理小組確保一致地套用餘額。 可靠性也取決於其他角色和功能。 本文會對應支援的功能，以協助您建立相關小組之間的對齊。

營運管理和治理與企業可靠性的夥伴相同。 您對操作實務所做的決策會設定可靠性的基準。 用來管理整體環境的方法可確保所有資源之間的一致性。

這篇文章中的前兩個步驟可協助這兩個小組開始著手。 這些會依序列出，但您可以平行執行。 後續的步驟可協助您從共用旅程開始整個企業，讓整個企業都能獲得更可靠的解決方案。

![企業可靠性入門](../_images/get-started/reliability-map.png)

## <a name="step-1-establish-operations-management-requirements"></a>步驟1：建立作業管理需求

並非所有工作負載都有相等的建立。 在任何環境中，都有工作負載對企業有直接且不受影響的工作負載。 此外也支援商務程式和工作負載，對整體業務的影響較小。 在此步驟中，雲端作業小組會識別並實行初始需求，以支援整體的 IT 組合。

**交付：**

- 執行管理基準，以定義所有生產工作負載所需的標準作業。
- 與雲端策略小組協商商務承諾，以開發先進作業和復原需求的計畫。
- 如果大部分的工作負載都需要額外的作業，請展開您的管理基準。
- 將先進的作業需求套用至登陸區域和資源，以支援最重要的工作負載。
- 在 [operations management 活頁簿](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx)中的 IT 組合上記錄作業決策。

**支援交付完成的指導方針：**

- [管理基準](../manage/considerations/discipline.md)：

  - [清查和可見度](../manage/considerations/inventory.md)： [雲端原生工具](../manage/azure-management-guide/inventory.md) 可協助您 [收集資料](../manage/monitor/data-collection.md) 和 [設定警示](../manage/monitor/index.md)。 這些工具也可協助您執行最適合您作業模型的 [監視平臺](../manage/monitor/index.md) 。
  - 作業[合規性](../manage/considerations/operational-compliance.md)：中斷的最高百分比，通常是來自資源設定或不佳維護實務的變更。 遵循 [Azure 伺服器管理指南](../manage/azure-server-management/index.md) 來執行雲端原生工具，以管理修補和資源設定的變更。
  - [保護和](../manage/considerations/protect.md)復原：任何平臺上的中斷都是不可避免的。 當發生中斷時，請使用 [備份和復原解決方案](../manage/azure-management-guide/protect-recover.md) 來做好準備，以將持續時間降到最低。
- [Advanced 作業](../manage/design-principles.md)：使用管理基準做為您的 [業務協調](../manage/considerations/business-alignment.md) 對話的基礎。 它可協助您清楚地討論 [重要性](../manage/considerations/criticality.md)、 [業務衝擊](../manage/considerations/impact.md)和 [營運承諾](../manage/considerations/commitment.md)。 企業調整有助於量化和驗證要求的 [增強基準](../manage/azure-management-guide/enhanced-baseline.md)、特定 [技術平臺](../manage/azure-management-guide/workload-specialization.md)的管理，或 [工作負載特定的作業](../manage/azure-management-guide/platform-specialization.md)。
- **引導架構審核：** 工作負載層級的架構變更可能需要符合作業需求。 [Microsoft Azure 架構良好的架構](/azure/architecture/framework/cost/tradeoffs)，並[Microsoft Azure 架構良好的審查](/assessments?id=azure-architecture-review)，可協助引導這些交談與特定工作負載的技術擁有者。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端營運小組 | <li> 雲端策略小組 <li> 雲端採用小組 <li> 雲端治理小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-2-consistently-apply-the-management-baseline"></a>步驟2：一致地套用管理基準

企業可靠性需要一致的管理基準應用程式。 該一致性來自適當的公司原則、IT 流程和自動化工具。 這些資源會針對所有受影響的資源管理管理基準的實施。

**交付：**

- 確定所有受影響系統的管理基準都有適當的應用程式。
- 記錄資源 [一致性專業人員範本](../govern/resource-consistency/template.md)中的資源一致性原則、程式和設計指引。

**支援交付完成的指導方針：**

- 確定所有工作負載和資源都遵循 [適當的命名和標記慣例](../ready/azure-best-practices/naming-and-tagging.md)。 [使用 Azure 原則來強制執行標記慣例](/azure/governance/policy/tutorials/govern-tags)，並針對重要性的標記特別強調。
- 如果您不熟悉雲端治理，請使用管理方法來建立 [治理原則、程式和專業領域](../govern/index.md) 。
- 如果您不熟悉成本管理專業領域，請遵循 [成本管理改進](../govern/guides/complex/cost-management-improvement.md) 文章中的指導方針。 將焦點放在 [ [執行](../govern/guides/complex/cost-management-improvement.md#incremental-improvement-of-the-best-practices) ] 區段。

> [!NOTE]
> **與其他小組開始可靠性合作的步驟：** 整個雲端採用生命週期的各種決策可能會對可靠性有直接的影響。 下列步驟概述在 IT 組合之間提供一致的可靠性所需的合作關係和支援工作。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端治理小組 | <li> 雲端策略小組 <li> 雲端營運小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-3-define-your-strategy"></a>步驟3：定義您的策略

策略性決策會直接影響可靠性。 他們會在採用生命週期和長期作業中進行 ripple。 策略性的清楚改進了可靠性的努力。

**交付：**

- 記錄 [策略和方案範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx)中的動機、結果和業務理由。
- 請確定管理基準提供可符合雲端採用策略方向的營運支援。

**支援交付完成的指導方針：**

- [瞭解動機](../strategy/motivations.md)：重要的商務事件和某些遷移動機通常會受到成本影響。 這些區域可以增加成本控制在所有後續工作中的重要性。 其他與創新或成長有關的向前搜尋動機，可能會著重于最頂尖的收入。 瞭解動機可協助您設定成本管理的優先順序。
- [商務成果](../strategy/business-outcomes/index.md)：某些會計結果通常會非常符合成本效益。 當想要的結果對應至會計計量時，您應該提早投資成本管理治理專業領域。
- [商業理由](../strategy/cloud-migration-business-case.md)：商業理由可作為雲端採用整體財務計畫的高階觀點。 這可能是初始預算工作的良好來源。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端治理小組 <li> 雲端營運小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-4-develop-a-cloud-adoption-plan"></a>步驟4：開發雲端採用方案

 (或分析現有 IT 組合) 的數位資產，可協助您驗證商務理由。 它可以提供整體 IT 組合的精簡觀點。 採用方案提供採用期間啟用時間表的清楚明瞭。

當您將採用方案與數位資產分析對齊時，您可以規劃未來的作業管理相依性。 瞭解採用方案也會邀請雲端營運團隊進入開發週期。 他們可以評估和規劃提供工作負載作業所需之管理基準的任何變更。

**交付：**

- 更新 [策略和方案範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx) ，以反映達成所需策略所需的變更。 記錄的變更可能包括：

  - 現有數位資產的評量。
  - 雲端採用方案，可反映所需的變更和相關工作。
  - 在方案上提供所需的組織變更。
  - 此計畫可滿足讓現有團隊順利完成所需工作所需的技能。
- 與治理小組合作，以配合成本模型和預測模型。 此套裝程式括透過量化分析開始優化支出的工作。

**支援交付完成的指導方針：**

- [收集清查](../digital-estate/inventory.md)：在採用之前，建立資料來源以分析數位資產。
- [最佳做法： Azure Migrate](../plan/contoso-migration-assessment.md)：使用 Azure Migrate 來收集清查。
- [增量合理化](../digital-estate/rationalize.md#incremental-rationalization)：在增量合理化期間，量化分析可針對預算用途找出雲端候選項目。
- [調整成本模型和預測模型](../digital-estate/calculate.md)：使用 Azure 成本管理藉由 [建立預算](/azure/cost-management-billing/costs/tutorial-acm-create-budgets?bc=%2fazure%2fcloud-adoption-framework%2f_bread%2ftoc.json&toc=%2fazure%2fcloud-adoption-framework%2ftoc.json)來調整成本和預測模型。
- [打造您的雲端採用方案](../plan/plan-intro.md#build-your-cloud-adoption-plan)：建立具有可採取動作的工作負載、資產和時間軸詳細資料的方案。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端採用小組 <li> 雲端治理小組 <li> 雲端營運小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-5-implement-landing-zone-best-practices"></a>步驟5：實施登陸區域最佳作法

適用于 Azure 的雲端採用架構的準備方法，主要著重于登陸區域的開發，以裝載雲端中的工作負載。 登陸區域執行期間，多個決策可能會影響作業。 請參閱雲端營運小組，以協助複習登陸區域以進行操作改進。 另請參閱雲端治理小組，以瞭解可能會影響登陸區域設計的資源一致性原則和設計指引。

**交付：**

- 部署一或多個能夠在短期採用方案中裝載工作負載的登陸區域。
- 確定所有登陸區域都符合操作決策和資源一致性需求。

**支援交付完成的指導方針：**

- [改進登陸區域作業](../ready/considerations/landing-zone-operations.md)：改善指定登陸區域內作業的最佳做法。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端營運小組 <li> 雲端策略小組 <li> 雲端治理小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-6-complete-waves-of-adoption-effort-and-change"></a>步驟6：完成採用工作及變更

長期作業可能會受到在遷移和創新工作期間所做的決策影響。 在採用程式初期維持一致的一致性，有助於移除生產版本的阻礙。 也可減少在營運管理實務中引入新解決方案所需的工作。

**交付：**

- 使用資源一致性原則，測試生產部署的作業就緒程度。
- 遵循資源一致性設計指引和作業需求的驗證。
- 記錄 [operations management 活頁簿](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx)中的任何 advanced 作業需求。

**支援交付完成的指導方針：**

- [環境就緒檢查清單](../migrate/migration-considerations/prerequisites/planning-checklist.md)
- [升級前檢查清單](../migrate/migration-considerations/optimize/ready.md)
- [生產版本檢查清單](../migrate/migration-considerations/optimize/promote.md)

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 <li> 雲端營運小組 <li> 雲端治理小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="value-statement"></a>Value 語句

這些步驟可協助您執行必要的控制項和程式，以確保整個企業和所有託管資源的可靠性。
