---
title: 開始使用：利用正確的控制項提升可靠性
description: 瞭解透過治理控制項和管理基準來提升可靠性的基本概念。
author: JanetCThomas
ms.author: janet
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.openlocfilehash: 0f7ae79bbcc9fb4c02a0c9e731d2e826f4682138
ms.sourcegitcommit: 070e6a60f05519705828fcc9c5770c3f9f986de5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83814812"
---
# <a name="get-started-improve-reliability-with-the-right-controls"></a>開始使用：利用正確的控制項提升可靠性

您要如何套用正確的控制項來提升可靠性？ 本文可協助您將與下列專案相關的中斷降至最低：

- 設定中的不一致。
- 資源組織。
- 安全性基準。
- 資源保護。

這篇文章中的步驟可協助營運小組平衡 IT 組合的可靠性和成本。 本文也會協助治理小組確保一致地套用餘額。 可靠性也取決於其他角色和功能。 本文會對應支援的函式，以協助您在相關小組之間建立對齊方式。

營運管理與治理是企業可靠性中的夥伴。 您對操作實務做出的決策會設定可靠性的基準。 用來管理整體環境的方法，可確保所有資源之間的一致性。

這篇文章中的前兩個步驟可協助這兩個小組開始進行。 它們會依序列出，但是您可以平行執行它們。 後續的步驟可協助您讓整個企業開始在整個企業中更可靠的解決方案之間進行共用。

![企業可靠性入門](../_images/get-started/reliability-map.png)

## <a name="step-1-establish-operations-management-requirements"></a>步驟1：建立作業管理需求

並非所有工作負載都是以相同的建立。 在任何環境中，都有工作負載對企業有直接且持續的影響。 也支援商務程式和工作負載，對整體業務的影響較小。 在此步驟中，雲端營運小組會識別並實行初始需求，以支援整體 IT 組合。

**項**

- 執行管理基準，以定義所有生產工作負載所需的標準作業。
- 與雲端策略小組協商商務承諾，以開發先進作業和復原需求的計畫。
- 如果大部分的工作負載都需要額外的作業，請展開您的管理基準。
- 將 advanced 作業需求套用至登陸區域和資源，以支援最重要的工作負載。
- 在[operations management 活頁簿](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx)中的 IT 組合上記載作業決策。

**支援交付後完成的指引：**

- [管理基準](../manage/considerations/discipline.md)：

  - [清查和可見度](../manage/considerations/inventory.md)：[雲端原生工具](../manage/azure-management-guide/inventory.md)可協助您[收集資料](../manage/monitor/data-collection.md)並[設定警示](../manage/monitor/index.md)。 這些工具也可協助您執行最適合您的作業模型的[監視平臺](../manage/monitor/index.md)。
  - 作業[合規性](../manage/considerations/operational-compliance.md)：最高的中斷百分比通常來自于資源設定的變更或不佳的維護實務。 遵循[Azure 伺服器管理指南](../manage/azure-server-management/index.md)來執行雲端原生工具，以管理資源設定的修補和變更。
  - [保護與](../manage/considerations/protect.md)復原：任何平臺上的中斷都是不可避免的。 發生中斷時，請使用[備份和復原解決方案](../manage/azure-management-guide/protect-recover.md)來準備，以將持續時間減至最少。
- [Advanced 作業](../manage/design-principles.md)：使用管理基準作為[商務對齊](../manage/considerations/business-alignment.md)交談的基礎。 它可協助您清楚地討論[重要性](../manage/considerations/criticality.md)、[業務衝擊](../manage/considerations/impact.md)和[營運承諾](../manage/considerations/commitment.md)。 商務對齊有助於量化並驗證要求的[增強基準](../manage/azure-management-guide/enhanced-baseline.md)、管理特定[技術平臺](../manage/azure-management-guide/workload-specialization.md)或[工作負載特定作業](../manage/azure-management-guide/platform-specialization.md)。
- **引導您進行架構審查：** 若要符合作業需求，可能需要工作負載層級的架構變更。 [Microsoft Azure 架構完善的架構](https://docs.microsoft.com/azure/architecture/framework/cost/tradeoffs)和[Microsoft Azure 架構良好的審查](https://docs.microsoft.com/assessments?id=azure-architecture-review)，可以協助引導這些與特定工作負載的技術擁有者交談。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端營運小組 | <li> 雲端策略小組 <li> 雲端採用小組 <li> 雲端治理小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-2-consistently-apply-the-management-baseline"></a>步驟2：一致地套用管理基準

企業可靠性需要一致的管理基準應用程式。 這種一致性來自于適當的公司原則、IT 流程和自動化工具。 這些資源會針對所有受影響的資源控制管理基準的執行。

**項**

- 針對所有受影響的系統，確保適當的管理基準應用程式。
- 記錄[資源一致性專業領域範本](../govern/resource-consistency/template.md)中的資源一致性原則、處理常式和設計指引。

**支援交付後完成的指引：**

- 確保所有的工作負載和資源都遵循[適當的命名和標記慣例](../ready/azure-best-practices/naming-and-tagging.md)。 [使用 Azure 原則來強制標記慣例](https://docs.microsoft.com/azure/governance/policy/tutorials/govern-tags)，並特別強調重要性的標記。
- 如果您不熟悉雲端治理，請使用管控方法來建立[治理原則、處理常式和專業領域](../govern/index.md)。
- 如果您不熟悉成本管理專業領域，請遵循[成本管理改進](../govern/guides/complex/cost-management-improvement.md)文章中的指引。 將焦點放在 [[執行](../govern/guides/complex/cost-management-improvement.md#incremental-improvement-of-the-best-practices)] 區段。

> [!NOTE]
> **啟動與其他小組的可靠性合作關係的步驟：** 整個雲端採用週期的各種決策可能會直接影響可靠性。 下列步驟概述在 IT 組合上提供一致的可靠性所需的合作關係和支援工作。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端治理小組 | <li> 雲端策略小組 <li> 雲端營運小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-3-define-your-strategy"></a>步驟3：定義您的策略

策略性決策會直接影響可靠性。 他們會透過採用生命週期和長期作業來進行連鎖。 策略性的清晰度可改善可靠性。

**項**

- 記錄[策略和計畫範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)中的動機、結果和業務理由。
- 確保管理基準能夠提供符合雲端採用策略方向的營運支援。

**支援交付後完成的指引：**

- [瞭解動機](../strategy/motivations.md)：重要的商務事件和某些遷移動機通常會有成本效益。 這些區域可以增加成本控制的重要性，以供所有之後的工作之用。 其他與創新或透過遷移成長相關的正向動機，可能會更專注于頂線收益。 瞭解動機可協助您設定成本管理的優先順序。
- [商務結果](../strategy/business-outcomes/index.md)：某些會計結果通常會非常耗費成本。 當想要的結果對應至會計計量時，您應該提早投入成本管理治理專業領域。
- [商業理由](../strategy/cloud-migration-business-case.md)：商業理由可做為雲端採用的整體財務計畫的高階觀點。 這可能是初步的預算工作的好來源。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端治理小組 <li> 雲端營運小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-4-develop-a-cloud-adoption-plan"></a>步驟4：開發雲端採用方案

數位資產（或現有 IT 組合的分析）可協助您驗證商業理由。 它可以為整體 IT 組合提供精簡的觀點。 採用方案可讓您清楚理解採用期間的啟用時間軸。

當您將採用方案與數位資產分析一致時，可以規劃未來的作業管理相依性。 瞭解採用方案也會邀請雲端營運小組進入開發週期。 他們可以評估並規劃提供工作負載作業所需之管理基準的任何變更。

**項**

- 更新[策略和計畫範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)，以反映達成所需策略所需的變更。 記錄的變更可能包括：

  - 評估現有的數位資產。
  - 雲端採用方案，會反映所需的變更和涉及的工作。
  - 在計畫上交付所需的組織變更。
  - 一種計畫，用以解決讓現有小組成功完成所需的工作所需的技能。
- 與治理小組合作，以配合成本模型和預測模型。 此套裝程式括開始透過量化分析來優化支出的工作。

**支援交付後完成的指引：**

- [收集清查](../digital-estate/inventory.md)：建立資料來源，以在採用之前分析數位資產。
- [最佳做法-Azure Migrate](../plan/contoso-migration-assessment.md)：使用 Azure Migrate 來收集清查。
- [增量合理化](../digital-estate/rationalize.md#incremental-rationalization)：在累加式合理化期間，量化分析可以識別用於預算的雲端候選項目。
- [調整成本模型和預測模型](../digital-estate/calculate.md)：使用 Azure 成本管理，藉由[建立預算](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-acm-create-budgets?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)來對齊成本和預測模型。
- [建立您的雲端採用方案](../plan/plan-intro.md#build-your-cloud-adoption-plan)：使用可操作的工作負載、資產和時間軸詳細資料來建立方案。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端採用小組 <li> 雲端治理小組 <li> 雲端營運小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-5-implement-landing-zone-best-practices"></a>步驟5：執行登陸區域的最佳作法

適用于 Azure 的雲端採用架構的準備方法，著重于在裝載區域的開發，以在雲端中裝載工作負載。 在登陸區域執行期間，多個決策可能會影響作業。 請洽詢雲端營運小組，以協助審查登陸區域的作業改進。 另請諮詢雲端治理小組，以瞭解可能會影響登陸區域設計的資源一致性原則和設計指導方針。

**項**

- 在短期採用計畫中部署一或多個可裝載工作負載的登陸區域。
- 確保所有登陸區域符合作業決策和資源一致性需求。

**支援交付後完成的指引：**

- [改善登陸區域作業](../ready/considerations/landing-zone-operations.md)：改善指定登陸區域內作業的最佳作法。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端營運小組 <li> 雲端策略小組 <li> 雲端治理小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-6-complete-waves-of-adoption-effort-and-change"></a>步驟6：採用成果和變更的完整波浪

長期作業會受到遷移和創新工作期間所做的決策所影響。 在採用流程早期維持一致的對齊有助於移除生產版本的阻礙。 它也可減少將新解決方案引進作業管理實務所需的工作。

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

這些步驟可協助您執行必要的控制項和程式，以確保整個企業和所有託管資源的可靠性。
