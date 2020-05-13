---
title: 開始使用：確保所有組合的一致效能
description: 瞭解跨組合管理效能的基本概念，包括建立維護效能的程式、設定效能期望，以及在您的組織之間建立對齊。
author: JanetCThomas
ms.author: janet
ms.date: 04/04/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.openlocfilehash: 6052e119cc2bf2ce078bfdc3df6613ff665e503f
ms.sourcegitcommit: 60d8b863d431b5d7c005f2f14488620b6c4c49be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83229362"
---
<!-- docsTest:ignore Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template -->

# <a name="get-started-ensure-consistent-performance-across-a-portfolio"></a>開始使用：確保所有組合的一致效能

我們要如何確保工作負載組合的效能良好？ 本指南可協助您建立進程，以維護整個企業的效能。 這裡所述的步驟可協助作業小組確保所有工作負載的一致效能預期。 效能也取決於其他角色和功能。 本文會對應這些支援函數，以協助您在每個相關的小組之間建立對齊方式。

集中式作業管理是最常見的方法，可在整個組合中保持一致的效能。 關於操作實務的決策會定義作業基準和任何整體增強功能。 本指南的第一個步驟可協助作業小組開始使用。 後續的步驟可協助整個企業開始進行工作負載組合間企業效能的共用旅程。

![企業效能管理入門](../_images/get-started/performance-map.png)

## <a name="step-1-establish-operations-management-requirements"></a>步驟1：建立作業管理需求

在雲端採用架構中所述的作業管理基準提供了一組控制項和雲端原生作業工具，可確保一致的作業。 使用自動化工具擴充該基準可提供效能監視和自動化，以符合整個組合的一致效能需求。

**項**

- 增強管理基準，以包含與效能預期偏差相關的自動化補救工作。
- 當需要工作負載特定的資料模式或架構變更以符合效能需求時，工作負載特有的作業可以提供更高的效能控制。
- 記錄[operations management 活頁簿](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx)中 IT 組合的作業決策，著重于在 [基準] 索引標籤的 [操作相容性] 區段中包含效能自動化決策。

**支援交付後完成的指引：**

- [增強的管理基準](../manage/azure-management-guide/enhanced-baseline.md)一文概述使用工具（例如 Azure 自動化）新增效能相關增強功能的範例。 這種方法可透過對支援資產大小和規模的基本修改，協助維持一致的效能。
- [工作負載特有的作業](../manage/azure-management-guide/platform-specialization.md)會使用 Azure 架構審查，針對特定的工作負載提供自動化的指引。 當作業動作應由工作負載特定資料驅動時，此效能管理的方法特別有用。
- 上述指導方針假設有一個現有的[管理基準](../manage/considerations/discipline.md)執行，以支援完整的工作負載組合。

> [!NOTE]
> **開始讓組織中的效能預期一致的步驟：** 整個雲端採用週期的各種決策，可能會對效能產生直接的影響。 下列步驟有助於概述在 IT 組合間提供效能所需的合作關係和支援工作。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端營運小組 | <li> 雲端策略小組 <li> 雲端採用小組 <li> 雲端治理小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-2-consistent-application-of-the-management-baseline"></a>步驟2：管理基準的一致應用

隨著管理基準的改善，請務必確保這些改良功能會帶到雲端治理的資源一致性專業領域。 這麼做可確保在所有受管理的環境中，都能應用增強的基準。

**項**

- 針對所有受影響的系統，確保適當地應用其增強的管理基準。
- 記錄[資源一致性專業領域範本](../govern/resource-consistency/template.md)中的資源一致性原則、處理常式和設計指引。

**支援交付後完成的指引：**

- 請確定所有的工作負載和資源都遵循[適當的命名和標記慣例](../ready/azure-best-practices/naming-and-tagging.md)，並使用 Azure 原則特別強調「重要性」的標記來[強制執行標記慣例](https://docs.microsoft.com/azure/governance/policy/tutorials/govern-tags)。
- 如果您不熟悉雲端治理，請使用管理方法來建立[治理原則、處理常式和專業領域](../govern/index.md)。
- 如果您不熟悉成本管理專業領域，請考慮遵循[成本管理改良文章](../govern/guides/complex/cost-management-improvement.md)，並將焦點放在 [[執行](../govern/guides/complex/cost-management-improvement.md#incremental-improvement-of-the-best-practices)] 區段。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端治理小組 | <li> 雲端策略小組 <li> 雲端營運小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-3-define-strategy"></a>步驟3：定義策略

策略性決策會直接影響效能，並透過採用週期和長期作業進行波及各。 策略性的清晰度可改善整個組合的效能。 這種明確也有助於營運小組瞭解哪些工作負載需要某種程度的工作負載特製化和先進的作業。

**項**

- 記錄[策略和計畫範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)中的動機、結果和業務理由。
- 確保管理基準提供可與雲端採用策略方向一致的營運支援。

**支援交付後完成的指引：**

- [瞭解動機](../strategy/motivations.md)：重要的商務事件和某些遷移動機通常會有成本的影響，這會提高所有後續工作的成本控制重要性。 其他與創新或透過遷移的成長相關的正向動機，可能會更專注于頂尖的收益。 瞭解動機可以協助您瞭解優先順序成本管理有多高。
- [商務結果](../strategy/business-outcomes/index.md)：某些會計結果通常會非常耗費成本。 當想要的結果對應至會計計量時，您應該及早投入成本管理的治理專業領域。
- [商業理由](../strategy/cloud-migration-business-case.md)：商業理由可做為雲端採用的整體財務計畫的高階觀點。 這可能是初步的預算工作的好來源。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端治理小組 <li> 雲端營運小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-4-assess-and-plan-for-workload-adoption"></a>步驟4：評估和規劃工作負載採用

數位資產（或現有 IT 組合的分析）可以協助驗證商業理由，並提供整體 IT 組合的精簡觀點。 採用方案可讓您清楚理解採用期間的啟用時間軸。 調整該方案和數位資產分析，提供了規劃未來作業管理相依性的一種方法。 瞭解此方案也會邀請雲端作業小組進入開發週期，以評估和規劃對管理基準所做的任何變更，以提供工作負載作業。

**項**

- 更新[策略和計畫範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)，以反映數位資產分析所觸發的變更。
- 與雲端營運小組合作，在近乎長期的採用計畫中，清楚地定義每個工作負載的重要性和業務影響。
- 與雲端營運小組合作，以建立作業就緒時間的時程表。

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
| <li> 雲端策略小組 | <li> 雲端治理小組 <li> 雲端營運小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-5-expand-the-landing-zones"></a>步驟5：展開登陸區域

雲端採用架構的準備方法著重于在登陸區域的開發，以裝載雲端中的工作負載。 在登陸區域執行期間，可能會影響作業的各種決策。 請洽詢雲端營運小組，以協助審查登陸區域的作業改進。 此外，請洽詢雲端治理小組，以瞭解可能會影響登陸區域設計的「資源一致性」原則和設計指引。

**項**

- 部署一或多個登陸區域，其能夠在短期採用方案中裝載工作負載。
- 確保所有登陸區域都符合作業決策和資源一致性需求。

**支援交付後完成的指引：**

- [改善登陸區域作業](../ready/considerations/landing-zone-operations.md)：改善指定登陸區域內作業的最佳作法。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端營運小組 <li> 雲端策略小組 <li> 雲端治理小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-6-adoption"></a>步驟6：採用

長期作業可能會受到遷移和創新工作期間所做的決策所影響。 在採用程式初期保持一致的對齊有助於移除生產版本的阻礙，並減少將新解決方案上線到作業管理實務所需的工作。

**項**

- 使用資源一致性原則來測試生產部署的作業準備就緒。
- 驗證遵循資源一致性設計指引和作業需求。
- 記錄[operations management 活頁簿](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx)中的任何先進作業需求。

**支援交付後完成的指引：**

- [環境就緒檢查清單](../migrate/migration-considerations/prerequisites/planning-checklist.md)
- [升級前檢查清單](../migrate/migration-considerations/optimize/ready.md)
- [生產版本檢查清單](../migrate/migration-considerations/optimize/promote.md)

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 <li> 雲端營運小組 <li> 雲端策略小組 <li> 雲端治理小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="value-statement"></a>Value 語句

上述步驟將協助您執行所需的適當控制項和程式，以確保整個企業和所有託管資源的效能。
