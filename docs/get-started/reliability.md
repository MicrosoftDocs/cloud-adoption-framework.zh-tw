---
title: 開始使用：利用正確的控制項提升可靠性
description: 瞭解透過治理控制項和管理基準來提升可靠性的基本概念。
author: JanetCThomas
ms.author: janet
ms.date: 04/04/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.openlocfilehash: c75b7a17c8c2676688f5221ec0e4d0f2ed0641a5
ms.sourcegitcommit: 5d6a7610e556f7b8ca69960ba76a3adfa9203ded
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2020
ms.locfileid: "83400206"
---
# <a name="get-started-improve-reliability-with-the-right-controls"></a>開始使用：利用正確的控制項提升可靠性

我們要如何套用正確的控制項來提升可靠性？ 本指南可協助您將與設定、資源組織、安全性基準或資源保護不一致相關的中斷情形降到最低。 本指南中的步驟將協助營運小組平衡 IT 組合的可靠性和成本，並協助治理小組確保一致地套用餘額。 可靠性也取決於其他角色和功能。 本文會對應各種支援函數，以協助您在每個相關的小組之間建立對齊方式。

營運管理與治理是企業可靠性中的夥伴。 關於操作實務的決策會設定可靠性的基準。 用來管理整體環境的方法，可確保所有資源之間的一致性。 本指南中的前兩個步驟可協助這兩個小組開始使用。 依序列出時，下列步驟可以平行進行。 後續的步驟可協助整個企業開始進行，以在整個企業中提供更可靠的解決方案。

![企業可靠性入門](../_images/get-started/reliability-map.png)

## <a name="step-1-establish-operations-management-requirements"></a>步驟1：建立作業管理需求

所有工作負載的建立都不相同。 在任何環境中，都有工作負載對企業有直接且持續的影響。 也支援商務程式和工作負載，對整體業務的影響較小。 在此步驟中，雲端營運小組會識別並實行初始需求，以支援整體 IT 組合。

**項**

- 執行管理基準，以定義所有生產工作負載所需的標準作業。
- 與雲端策略小組協商商務承諾，以開發先進作業和復原需求的計畫。
- 如果大部分的工作負載都需要額外的作業，請展開您的管理基準。
- 將先進的作業需求套用至登陸區域和資源，以支援更高的重要性工作負載。
- 在[operations management 活頁簿](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx)中的 IT 組合上記載作業決策。

**支援交付後完成的指引：**

- **[管理基準](../manage/considerations/discipline.md)：**

  - [清查和可見度](../manage/considerations/inventory.md)：[雲端原生工具](../manage/azure-management-guide/inventory.md)可協助[收集資料](../manage/monitor/data-collection.md)、[設定警示](../manage/monitor/index.md)，以及執行最適合您的作業模型的[監視平臺](../manage/monitor/index.md)。
  - 作業[合規性](../manage/considerations/operational-compliance.md)：最高的中斷百分比通常來自于資源設定的變更或不佳的維護實務。 遵循[Azure 伺服器管理指南](../manage/azure-server-management/index.md)來執行雲端原生工具，以管理資源設定的修補和變更。
  - [保護與](../manage/considerations/protect.md)復原：任何平臺上的中斷都是不可避免的。 當發生中斷時，請使用[備份和復原解決方案](../manage/azure-management-guide/protect-recover.md)來準備，以將任何中斷的持續時間減至最少。

- **[Advanced 作業](../manage/design-principles.md)：** 使用管理基準作為[商務對齊](../manage/considerations/business-alignment.md)交談的基礎，以建立與重要性、[業務衝擊](../manage/considerations/impact.md)和[作業承諾](../manage/considerations/commitment.md)有關的清楚[程度](../manage/considerations/criticality.md)。 商務對齊有助於量化並驗證要求的[增強基準](../manage/azure-management-guide/enhanced-baseline.md)、管理特定[技術平臺](../manage/azure-management-guide/workload-specialization.md)或[工作負載特定作業](../manage/azure-management-guide/platform-specialization.md)。

- **引導您進行架構審查：** 若要符合作業需求，可能需要工作負載層級的架構變更。 [Azure 架構](https://docs.microsoft.com/azure/architecture/framework/cost/tradeoffs)架構和[azure 架構審查](https://docs.microsoft.com/assessments?id=azure-architecture-review)可協助引導這些與特定工作負載的技術擁有者交談。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端營運小組 | <li> 雲端策略小組 <li> 雲端採用小組 <li> 雲端治理小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-2-consistently-apply-the-management-baseline"></a>步驟2：一致地套用管理基準

企業可靠性需要一致的管理基準應用程式。 該一致性來自適當的公司原則、IT 流程和自動化工具，可控制所有受影響資源的管理基準的執行。

**項**

- 針對所有受影響的系統，確保適當的管理基準應用程式。
- 記錄[資源一致性專業領域範本](../govern/resource-consistency/template.md)中的資源一致性原則、處理常式和設計指引。

**支援交付後完成的指引：**

- 確保所有的工作負載和資源遵循[適當的命名和標記慣例](../ready/azure-best-practices/naming-and-tagging.md)，並[使用 Azure 原則強制執行標記慣例](https://docs.microsoft.com/azure/governance/policy/tutorials/govern-tags)，並特別強調「重要性」標記。
- 如果您不熟悉雲端治理，請使用管理方法來建立[治理原則、處理常式和專業領域](../govern/index.md)。
- 如果您不熟悉成本管理專業領域，請考慮遵循[成本管理改良文章](../govern/guides/complex/cost-management-improvement.md)，並將焦點放在 [[執行](../govern/guides/complex/cost-management-improvement.md#incremental-improvement-of-the-best-practices)] 區段。

> [!NOTE]
> **啟動與其他小組的可靠性合作關係的步驟：** 整個雲端採用週期的各種決策可能會直接影響可靠性。 下列步驟有助於概述在 IT 組合上提供一致的可靠性所需的合作關係和支援工作。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端治理小組 | <li> 雲端策略小組 <li> 雲端營運小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-3-define-your-strategy"></a>步驟3：定義您的策略

**項**

- 記錄[策略和計畫範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)中的動機、結果和業務理由。
- 確保管理基準提供可與雲端採用策略方向一致的營運支援。

**支援交付後完成的指引：**

- [瞭解動機](../strategy/motivations.md)：重要的商務事件和某些遷移動機通常會有成本的影響，這會提高所有後續工作的成本控制重要性。 其他與創新或透過遷移的成長相關的正向動機，可能會更專注于頂尖的收益。 瞭解動機可協助您瞭解優先順序成本管理有多高。
- [商務結果](../strategy/business-outcomes/index.md)：某些會計結果通常會非常耗費成本。 當想要的結果對應至會計計量時，您應該及早投入成本管理的治理專業領域。
- [商業理由](../strategy/cloud-migration-business-case.md)：商業理由可做為雲端採用的整體財務計畫的高階觀點。 這可能是初步的預算工作的好來源。

策略性決策直接影響可靠性，透過採用週期和長期作業來波及各。 策略性的清晰度可改善可靠性。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端治理小組 <li> 雲端營運小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-4-develop-a-cloud-adoption-plan"></a>步驟4：開發雲端採用方案

數位資產（或現有 IT 組合的分析）可以協助驗證商業理由，並提供整體 IT 組合的精簡觀點。 採用方案可讓您清楚理解採用期間的啟用時間軸。 調整該方案和數位資產分析，提供了規劃未來作業管理相依性的一種方法。 瞭解此方案也會邀請雲端作業小組進入開發週期，以評估和規劃對管理基準所做的任何變更，以提供工作負載作業。

**項**

- 更新[策略和計畫範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)，以反映實現所需策略所需的變更。 記錄的變更可能包括下列各項：
  - 評估現有的數位資產。
  - 雲端採用計畫會反映所需的變更和相關工作。
  - 在計畫上交付所需的組織變更。
  - 一種計畫，用以解決讓現有小組成功完成所需的工作所需的技能。
- 與治理小組合作，以配合成本模型和預測模型，包括開始透過量化分析來優化支出的工作。

**支援交付後完成的指引：**

- [收集清查](../digital-estate/inventory.md)。 建立資料來源，以在採用之前分析數位資產。
- [最佳做法-Azure Migrate](../plan/contoso-migration-assessment.md)。 使用 Azure Migrate 來收集清查。
- [增量合理化](../digital-estate/rationalize.md#incremental-rationalization)。 在累加式合理化期間，量化分析可以識別用於預算的雲端候選項目。
- [對齊成本模型和預測模型](../digital-estate/calculate.md)。 藉由[建立預算](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-acm-create-budgets?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)，使用 Azure 成本管理來對齊成本和預測模型。
- [建立您的雲端採用方案](../plan/plan-intro.md#build-your-cloud-adoption-plan)。 建立具有可採取動作之工作負載、資產和時間軸詳細資料的方案。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端採用小組 <li> 雲端治理小組 <li> 雲端營運小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-5-implement-landing-zone-best-practices"></a>步驟5：執行登陸區域的最佳作法

雲端採用架構的準備方法著重于在登陸區域的開發，以裝載雲端中的工作負載。 在登陸區域執行期間，多個決策可能會影響作業。 請洽詢雲端營運小組，以協助審查登陸區域的作業改進。 另請洽詢雲端治理小組，以瞭解可能會影響登陸區域設計的「資源一致性」原則和設計指引。

**項**

- 在短期採用方案中部署一或多個能夠裝載工作負載的登陸區域。
- 確保所有登陸區域都符合作業決策和資源一致性需求。

**支援交付後完成的指引：**

- [改善登陸區域作業](../ready/considerations/landing-zone-operations.md)：改善指定登陸區域內作業的最佳作法。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端營運小組 <li> 雲端策略小組 <li> 雲端治理小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-6-complete-waves-of-adoption-effort-and-change"></a>步驟6：採用成果和變更的完整波浪

長期作業會受到遷移和創新工作期間所做的決策所影響。 在採用程式初期保持一致的對齊有助於移除生產版本的阻礙，並減少將新解決方案上線到作業管理實務所需的工作。

**項**

- 使用資源一致性原則來測試生產部署的作業準備就緒。
- 驗證遵循資源一致性設計指引和作業需求。
- 記錄[operations management 活頁簿](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx)中的任何先進作業需求。

**支援交付後完成的指引：**

- [環境就緒檢查清單](../migrate/migration-considerations/prerequisites/planning-checklist.md)
- [升級前檢查清單](../migrate/migration-considerations/optimize/ready.md)
- [生產版本檢查清單](../migrate/migration-considerations/optimize/promote.md)

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 <li> 雲端營運小組 <li> 雲端治理小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="value-statement"></a>Value 語句

上述步驟將協助執行所需的適當控制項和程式，以確保整個企業和所有託管資源的可靠性。
