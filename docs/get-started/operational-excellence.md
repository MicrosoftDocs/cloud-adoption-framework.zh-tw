---
title: 開始使用：提供卓越的營運
description: 瞭解數位轉型期間操作卓越的基本概念。
author: JanetCThomas
ms.author: janet
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.openlocfilehash: 8859d2fa1228bb97ed9c0f6f0d2f05853c163969
ms.sourcegitcommit: 9a84c2dfa4c3859fd7d5b1e06bbb8549ff6967fa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2020
ms.locfileid: "83752571"
---
# <a name="get-started-deliver-operational-excellence-during-digital-transformation"></a>開始使用：在數位轉型期間提供卓越的營運

您在數位轉型期間如何確保操作卓越？ 營運卓越是直接影響 IT 決策的商務功能。 若要實現卓越的營運，您必須注意收入、風險和成本的影響，以專注于客戶和專案關係人價值。

此組織的變更管理方法需要：

- 定義的策略。
- 清除商務成果。
- 變更管理規劃。

從雲端的觀點來看，您可以藉由進行後續的變更並持續改進作業程式，來管理風險和成本的影響。 監視的領域包括系統自動化、IT 作業管理實務，以及整個雲端採用週期的資源一致性專業領域。

這篇文章中的步驟可協助策略小組領導組織變更管理，這是一致確保操作卓越的必要條件。

整個企業和組合的營運卓越，從策略和計畫的對等程式開始，到協調和報告組織變更管理的期望。 下列步驟可協助技術團隊提供達成卓越營運所需的專業領域。

![開始使用卓越的營運](../_images/get-started/operational-excellence-map.png)

## <a name="step-1-define-a-strategy-to-guide-digital-transformation-and-operational-excellence-expectations"></a>步驟1：定義策略以引導數位轉型和營運卓越期望

明確的商務策略是任何數位轉型和營運卓越的基礎。 它可以降低成本並簡化 IT 程式。 如果沒有清楚的策略，很難以瞭解這些變更可能會如何影響更廣泛轉換工作中所識別的商業結果。

**項**

- 記錄[策略和計畫範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)中的動機、結果和業務理由。
- 確保學習計量清楚易懂，並包含在 [商務結果] 區段中。 這些計量會引導營運卓越活動和 IT 內的報告。

**支援交付後完成的指引：**

- [瞭解動機](../strategy/motivations.md)：重要的商務事件和某些遷移動機通常會有成本效益。 這些區域可以增加成本控制的重要性，以供所有之後的工作之用。 其他與創新或透過遷移成長相關的正向動機，可能會更專注于頂線收益。 瞭解動機可協助您設定成本管理的優先順序。
- [商務結果](../strategy/business-outcomes/index.md)：某些會計結果通常會非常耗費成本。 當想要的結果對應至會計計量時，您應該提早投入成本管理治理專業領域。
- [商業理由](../strategy/cloud-migration-business-case.md)：商業理由可做為雲端採用的整體財務計畫的高階觀點。 這可能是初步的預算工作的好來源。
- [學習度量](../strategy/learning-metrics.md)：若要維持整體商務策略與更多戰術變更管理計畫之間的一致，請建立學習計量。 這些計量應該設計成顯示計畫的反復和增量進度。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端採用小組 <li> 雲端治理小組 <li> 雲端營運小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-2-develop-an-organizational-change-management-plan-to-span-cloud-adoption"></a>步驟2：開發組織變更管理計畫，以跨越雲端採用

組織變更管理是一種反復調整人員、程式和技術以支援整體商務策略的反復方式。 對於數位轉型的卓越操作案例，此方法通常是以 IT 為主的雲端採用方案為中心。

**項**

- 更新[策略和計畫範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)，以反映達成所需策略所需的變更。 記錄的變更可能包括：

  - 評估現有的數位資產。
  - 雲端採用方案，會反映所需的變更和涉及的工作。
  - 在計畫上交付所需的組織變更。
  - 一種計畫，用以解決讓現有小組成功完成所需的工作所需的技能。

**支援交付後完成的指引：**

- [收集清查](../digital-estate/inventory.md)：建立資料來源，以在採用之前分析數位資產。
- [最佳做法-Azure Migrate](../plan/contoso-migration-assessment.md)：使用 Azure Migrate 來收集清查。
- [增量合理化](../digital-estate/rationalize.md#incremental-rationalization)：在累加式合理化期間，量化分析會針對預算目的識別雲端候選項目。
- [調整成本模型和預測模型](../digital-estate/calculate.md)：使用 Azure 成本管理，藉由[建立預算](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-acm-create-budgets?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)來對齊成本和預測模型。
- [建立您的雲端採用方案](../plan/plan-intro.md#build-your-cloud-adoption-plan)：使用可操作的工作負載、資產和時間軸詳細資料來建立方案。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端採用小組 <li> 雲端治理小組 <li> 雲端營運小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-3-manage-change-across-cloud-adoption-efforts"></a>步驟3：跨雲端採用工作管理變更

實現商業成果是持續傳遞採用波的結果。 這些波浪可能包括遷移和創新週期。 不論是哪一種情況，都能從週期性變更管理週期開始傳遞操作卓越。

每一波（或以 agile 術語發行）都會將一組工作負載傳遞至雲端。 隨著每一波採用完成，雲端策略小組會回報學習計量、商業成果和整體策略的進度。 同樣地，當每一波採用完成時，採用小組需要待處理專案更新，以反映計畫中的優先順序工作負載。 這些更新是以商務計畫和客戶需求的任何變更為基礎。

**項**

- 根據不斷變化的市場狀況，以及完成最新的技術變更，來持續測試策略和變更管理計畫。

**支援交付後完成的指引：**

- [發行規劃](../digital-estate/approach.md)：透過發行規劃變更管理的方法。
- [增量合理化](../digital-estate/rationalize.md#incremental-rationalization)：變更管理的反復方式。 重點在於管理發行待處理專案，以支援可管理的變更波。
- [10 種方法的強大功能](../digital-estate/rationalize.md#release-planning)：限制變更管理計畫。 重點在於詳細分析和規劃10個工作負載的連續基底，以平衡增量變更和反復採用的工作。
- [對齊反復專案路徑](../plan/iteration-paths.md)：更新並新增每個版本的詳細資料，以確保目前的反復專案路徑。
- [評估工作負載](../migrate/azure-migration-guide/assess.md?tabs=challenge-assumptions)：雲端採用小組致力於評估並採取最新的一組遷移優先順序。
- [共識的商業價值](../innovate/business-value.md)：雲端採用小組致力於確保每一版新創新的商業價值一致。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端採用小組 |

## <a name="value-statement"></a>Value 語句

先前的步驟概述了企業導向的方法，可在整個數位轉型中建立營運卓越需求。 這種方法提供了透過其他作業模式功能的一致基礎。

![共用架構原則](../_images/shared-principles.png)

## <a name="next-steps-to-delivering-operational-excellence-across-the-portfolio"></a>跨組合提供營運卓越的後續步驟

營運卓越需要具有可靠性、效能、安全性和成本優化的嚴格方法。 使用此系列中的其餘指導方針，以協助確保每個原則都透過一致的自動化方法來實現。

- **成本優化：** 使用快速入門手冊來[管理企業成本](./manage-costs.md)，以持續優化營運成本
- **安全性：** 藉由使用快速入門手冊來[跨組合執行安全性](./security.md)，以降低風險。
- **效能管理：** 使用快速入門手冊來瞭解[企業的效能管理](./performance.md)，以確保 IT 資產效能支援商務程式。
- **可靠性：** 使用快速入門手冊[來建立可靠性](./reliability.md)，藉此提升可靠性並降低業務中斷。
