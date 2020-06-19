---
title: 開始使用：確保所有組合的一致效能
description: 瞭解跨組合管理效能的基本概念，包括維護效能、設定期望，以及建立組織的對齊。
author: JanetCThomas
ms.author: janet
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.openlocfilehash: f6b6a715d8dc8ea9335ca482f22a54f23278aabe
ms.sourcegitcommit: 9b183014c7a6faffac0a1b48fdd321d9bbe640be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2020
ms.locfileid: "85076421"
---
# <a name="get-started-ensure-consistent-performance-across-a-portfolio"></a>開始使用：確保所有組合的一致效能

您要如何確保工作負載組合之間有足夠的效能？ 本指南中的步驟可協助您建立處理常式，以維護該效能層級。

效能也取決於其他角色和功能。 本文會對應這些支援函式，以協助您在相關小組之間建立對齊方式。

集中式作業管理是最常見的方法，可在整個組合中保持一致的效能。 操作實務的決策會定義作業基準和任何整體增強功能。

本指南的第一個步驟可協助作業小組開始使用。 後續步驟可協助整個企業開始使用跨工作負載組合的企業效能共同旅程。

![企業效能管理入門](../_images/get-started/performance-map.png)

## <a name="step-1-establish-operations-management-requirements"></a>步驟1：建立作業管理需求

在適用于 Azure 的 Microsoft Cloud 採用架構中所述的作業管理基準，會提供一組控制項和雲端原生作業工具，以確保一致的作業。 使用自動化工具擴充該基準可提供效能監視和自動化，以符合各組合的一致效能需求。

**項**

- 增強管理基準，以包含與效能預期偏差相關的自動化補救工作。
- 當需要工作負載特定的資料模式或架構變更以符合效能需求時，請使用工作負載特有的作業來提供更佳的效能控制。
- 記錄[operations management 活頁簿](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx)中 IT 組合的營運決策。 在 [**基準**] 索引標籤的 [作業**合規性**] 區段中，將焦點放在包含效能自動化決策。

**支援交付後完成的指引：**

- [增強的管理基準](../manage/azure-management-guide/enhanced-baseline.md)一文概述使用工具（例如 Azure 自動化）新增與效能相關的增強功能的範例。 這種方法可以透過對支援資產大小和規模的基本修改，協助維持一致的效能。
- [工作負載特有的作業](../manage/azure-management-guide/platform-specialization.md)會使用 Microsoft Azure 妥善架構的審查，針對特定的工作負載提供自動化的指引。 當工作負載特定的資料應該會驅動操作動作時，此效能管理的方法特別有用。
- 上述指導方針假設現有的[管理基準](../manage/considerations/discipline.md)執行支援完整的工作負載組合。

> [!NOTE]
> 整個雲端採用週期的各種決策，可能會對效能產生直接的影響。 下列步驟有助於概述在 IT 組合間提供效能所需的合作關係和支援工作。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端營運小組 | <li> 雲端策略小組 <li> 雲端採用小組 <li> 雲端治理小組 <li> 卓越或中央 IT 小組的雲端中心 |

## <a name="step-2-consistent-application-of-the-management-baseline"></a>步驟2：管理基準的一致應用

隨著管理基準的改善，請務必確保這些改良功能會帶到資源一致性治理專業領域。 這麼做可確保在所有受管理的環境中，都能應用增強的基準。

**項**

- 針對所有受影響的系統，確保適當地應用其增強的管理基準。
- 記錄[資源一致性專業領域範本](../govern/resource-consistency/template.md)中資源一致性的原則、程式和設計指導方針。

**支援交付後完成的指引：**

- 確保所有的工作負載和資源都遵循[適當的命名和標記慣例](../ready/azure-best-practices/naming-and-tagging.md)。 [使用 Azure 原則來強制執行標記慣例](https://docs.microsoft.com/azure/governance/policy/tutorials/govern-tags)，特別強調「重要性」標記。
- 如果您不熟悉雲端治理，請使用管控方法來建立[治理原則、處理常式和專業領域](../govern/index.md)。
- 如果您不熟悉成本管理治理專業領域，請考慮遵循[成本管理改良文章](../govern/guides/complex/cost-management-improvement.md)，並將焦點放在 [[執行](../govern/guides/complex/cost-management-improvement.md#incremental-improvement-of-the-best-practices)] 區段。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端治理小組 | <li> 雲端策略小組 <li> 雲端營運小組 <li> 卓越或中央 IT 小組的雲端中心 |

## <a name="step-3-define-strategy"></a>步驟3：定義策略

策略性決策會直接影響效能，並透過採用週期和長期作業進行波及各。 策略性的清晰度可改善整個組合的效能。 這種清晰度也有助於營運小組瞭解哪些工作負載需要某種程度的工作負載特製化和先進的作業。

**項**

- 記錄[策略和計畫範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)中的動機、結果和業務理由。
- 請確定管理基準提供符合雲端採用策略方向的營運支援。

**支援交付後完成的指引：**

- [瞭解動機](../strategy/motivations.md)：重要的商務事件和某些遷移動機通常會有成本效益，這會提高所有後續工作的成本控制重要性。 其他與創新或透過遷移成長相關的正向動機，可能會更專注于頂線收益。 瞭解動機可以協助您瞭解優先順序成本管理有多高。
- [商務結果](../strategy/business-outcomes/index.md)：某些會計結果通常會非常耗費成本。 當想要的結果對應至會計計量時，您應該及早投入成本管理治理專業領域。
- [商業理由](../strategy/cloud-migration-business-case.md)：商業理由可做為雲端採用之財務計畫的高階觀點。 這可能是初步的預算工作的好來源。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端治理小組 <li> 雲端營運小組 <li> 卓越或中央 IT 小組的雲端中心 |

## <a name="step-4-assess-and-plan-for-workload-adoption"></a>步驟4：評估和規劃工作負載採用

數位資產（或現有 IT 組合的分析）可以協助驗證商業理由，並提供 IT 組合的精簡觀點。 採用方案可讓您清楚理解採用期間的啟用時間軸。 配合該計畫和數位資產分析，提供了一種方式來規劃作業管理的未來相依性。

瞭解此方案也會邀請雲端營運小組進入開發週期。 然後，小組可以評估並規劃提供工作負載作業所需之管理基準的任何變更。

**項**

- 更新[策略和計畫範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)，以反映數位資產分析所觸發的變更。
- 與雲端營運小組合作，在近乎長期的採用計畫中，清楚地定義每個工作負載的重要性和業務影響。
- 與雲端營運小組合作，以建立作業就緒時間的時程表。

**支援交付後完成的指引：**

- [收集清查](../digital-estate/inventory.md)：建立資料來源，以在採用之前分析數位資產。
- [最佳做法-Azure Migrate](../plan/contoso-migration-assessment.md)：使用 Azure Migrate 來收集清查。
- [增量合理化](../digital-estate/rationalize.md#incremental-rationalization)：在累加式合理化期間，使用量化分析來識別用於預算的雲端候選項目。
- [調整成本模型和預測模型](../digital-estate/calculate.md)：使用 Azure 成本管理，藉由[建立預算](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-acm-create-budgets?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)來對齊成本和預測模型。
- [建立您的雲端採用方案](../plan/plan-intro.md#build-your-cloud-adoption-plan)：使用可操作的工作負載、資產和時間軸詳細資料來建立方案。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端治理小組 <li> 雲端營運小組 <li> 卓越或中央 IT 小組的雲端中心 |

## <a name="step-5-expand-the-landing-zones"></a>步驟5：展開登陸區域

雲端採用架構的準備方法著重于在登陸區域的開發，以裝載雲端中的工作負載。 在登陸區域執行期間，各種決策可能會影響作業。

請洽詢雲端營運小組，以協助審查登陸區域的作業改進。 此外，請洽詢雲端治理小組以瞭解「資源一致性」原則和設計指引，這可能會影響登陸區域的設計。

**項**

- 部署一或多個可在短期採用計畫中裝載工作負載的登陸區域。
- 確保所有登陸區域符合作業決策和資源一致性需求。

**支援交付後完成的指引：**

- [改善登陸區域作業](../ready/considerations/landing-zone-operations.md)：改善登陸區域內作業的最佳作法。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端營運小組 <li> 雲端策略小組 <li> 雲端治理小組 <li> 卓越或中央 IT 小組的雲端中心 |

## <a name="step-6-adoption"></a>步驟6：採用

長期作業可能會受到您在遷移和創新工作期間所做的決策所影響。 及早維護採用流程的一致對齊有助於移除生產版本的阻礙。 它也可減少將新解決方案上線至作業管理實務所需的工作。

**項**

- 使用資源一致性原則來測試生產部署的作業準備就緒。
- 驗證是否遵循資源一致性和作業需求的設計指引。
- 記錄[operations management 活頁簿](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx)中的任何先進作業需求。

**支援交付後完成的指引：**

- [環境就緒檢查清單](../migrate/migration-considerations/prerequisites/planning-checklist.md)
- [升級前檢查清單](../migrate/migration-considerations/optimize/ready.md)
- [生產版本檢查清單](../migrate/migration-considerations/optimize/promote.md)

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 <li> 雲端營運小組 <li> 雲端策略小組 <li> 雲端治理小組 <li> 卓越或中央 IT 小組的雲端中心 |

## <a name="value-statement"></a>Value 語句

上述步驟將協助您執行控制項和程式，以確保整個企業和所有託管資源的效能。
