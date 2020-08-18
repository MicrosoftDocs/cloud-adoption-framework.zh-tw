---
title: 開始使用：確保所有組合的效能一致
description: 瞭解管理各組合效能的基本概念，包括維護效能、設定期望，以及建立組織一致。
author: JanetCThomas
ms.author: janet
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.openlocfilehash: e7c832ddfa310d176d16bdc21e28e61e99c96c63
ms.sourcegitcommit: 917188fa930cadddb03f9e9bbcdd7b630e4ee33e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/17/2020
ms.locfileid: "88281268"
---
# <a name="get-started-ensure-consistent-performance-across-a-portfolio"></a>開始使用：確保所有組合的效能一致

您要如何確保跨工作負載組合的效能有充分的？ 本指南中的步驟可協助您建立進程，以維護該效能層級。

效能也取決於其他角色和功能。 本文會對應這些支援的功能，以協助您在相關小組之間建立一致的。

集中式作業管理是最常見的方法，可在整個組合中維持一致的效能。 作業實務的決策會定義營運基準和任何全面的增強功能。

本指南的第一個步驟可協助作業小組開始使用。 後續步驟可協助整個企業開始使用跨工作負載組合之企業效能的共用旅程。

![企業效能管理入門](../_images/get-started/performance-map.png)

## <a name="step-1-establish-operations-management-requirements"></a>步驟1：建立作業管理需求

「適用于 Azure 的 Microsoft 雲端採用架構」中所述的營運管理基準提供了一組控制項和雲端原生作業工具，以確保一致的作業。 使用自動化工具擴充該基準可提供效能監視和自動化，以符合各組合的一致效能需求。

**交付：**

- 增強管理基準，以包含與效能預期偏差相關的自動化補救工作。
- 需要工作負載特定的資料模式或架構變更以符合效能需求時，請使用工作負載專屬的作業來提供更佳的效能控制項。
- 在 [operations management 活頁簿](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx)中記錄 IT 組合的操作決策。 在 [**基準**] 索引標籤的 [作業**合規性**] 區段中，將焦點放在包含效能自動化決策。

**支援交付完成的指導方針：**

- [增強型管理基準](../manage/azure-management-guide/enhanced-baseline.md)文章概述使用 Azure 自動化等工具來新增效能相關增強功能的範例。 這種方法有助於維持一致的效能，方法是對支援資產的大小和規模進行基本修改。
- [工作負載](../manage/azure-management-guide/platform-specialization.md) 專屬的作業使用 Microsoft Azure 架構良好的評論，針對特定工作負載提供自動化的指引。 當工作負載特定的資料應該推動操作動作時，此效能管理方法特別有用。
- 上述指導方針假設 [管理基準](../manage/considerations/discipline.md) 的現有執行支援完整的工作負載組合。

> [!NOTE]
> 整個雲端採用生命週期的各種決策可能會對效能產生直接的影響。 下列步驟可協助您概述合作關係和支援在 IT 組合間提供效能所需的工作。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端營運小組 | <li> 雲端策略小組 <li> 雲端採用小組 <li> 雲端治理小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-2-consistent-application-of-the-management-baseline"></a>步驟2：管理基準的一致應用程式

隨著管理基準的改進，請務必確保這些改良功能會帶到資源一致性治理專業領域。 這麼做可確保在所有受控環境中都能採用增強的基準。

**交付：**

- 確保所有受影響系統的增強式管理基準都能適當地應用。
- 記錄 [資源一致性專業人員範本](../govern/resource-consistency/template.md)中資源一致性的原則、程式和設計指引。

**支援交付完成的指導方針：**

- 確定所有工作負載和資源都遵循 [適當的命名和標記慣例](../ready/azure-best-practices/naming-and-tagging.md)。 [使用 Azure 原則來強制執行標記慣例](/azure/governance/policy/tutorials/govern-tags)，特別強調「重要性」的標記。
- 如果您不熟悉雲端治理，請使用管理方法來建立 [治理原則、程式和專業領域](../govern/index.md) 。
- 如果您不熟悉成本管理治理專業領域，請考慮遵循 [有關成本管理改進的文章](../govern/guides/complex/cost-management-improvement.md)，並將焦點放在 [ [執行](../govern/guides/complex/cost-management-improvement.md#incremental-improvement-of-the-best-practices) ] 區段。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端治理小組 | <li> 雲端策略小組 <li> 雲端營運小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-3-define-strategy"></a>步驟3：定義策略

策略性決策會直接影響效能，並透過採用生命週期和長期作業波及各。 策略性的清楚改進了各組合的效能。 這種清晰度也有助於營運團隊瞭解哪些工作負載需要某種程度的工作負載特製化和先進的作業。

**交付：**

- 記錄 [策略和方案範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx)中的動機、結果和業務理由。
- 確定管理基準提供可符合雲端採用策略方向的營運支援。

**支援交付完成的指導方針：**

- [瞭解動機](../strategy/motivations.md)：重要的商務事件和某些遷移動機通常會受到成本影響，進而提升成本控制對所有後續工作的重要性。 其他與創新或成長有關的向前搜尋動機，可能會著重于最頂尖的收入。 瞭解動機可協助您瞭解優先順序成本管理的高度。
- [商務成果](../strategy/business-outcomes/index.md)：某些會計結果通常會非常符合成本效益。 當想要的結果對應至會計計量時，您應該提早投資成本管理治理專業領域。
- [商業理由](../strategy/cloud-migration-business-case.md)：商業理由可作為雲端採用的財務計畫的高階觀點。 這可以是初步預算工作的絕佳來源。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端治理小組 <li> 雲端營運小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-4-assess-and-plan-for-workload-adoption"></a>步驟4：評定和規劃工作負載的採用

 (或分析現有 IT 組合) 的數位資產可以協助驗證商務理由，並提供更精簡的 IT 組合觀點。 採用方案提供採用期間啟用時間表的清楚明瞭。 調整該計畫和數位資產分析可提供一種規劃作業管理未來相依性的方法。

瞭解此方案也會邀請雲端營運小組進入開發週期。 然後，小組可以針對提供工作負載作業所需的管理基準進行評估和規劃變更。

**交付：**

- 更新 [策略和方案範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx) ，以反映數位資產分析所觸發的變更。
- 與雲端營運小組合作，清楚地定義每個工作負載在短期和長期採用方案中的重要性和業務衝擊。
- 與雲端營運小組合作，以建立作業就緒時間的時程表。

**支援交付完成的指導方針：**

- [收集清查](../digital-estate/inventory.md)：建立資料來源，以在採用之前分析數位資產。
- [最佳做法： Azure Migrate](../plan/contoso-migration-assessment.md)：使用 Azure Migrate 來收集清查。
- [增量合理化](../digital-estate/rationalize.md#incremental-rationalization)：在增量合理化期間，使用量化分析來識別適用于預算的雲端候選。
- [調整成本模型和預測模型](../digital-estate/calculate.md)：使用 Azure 成本管理藉由 [建立預算](/azure/cost-management-billing/costs/tutorial-acm-create-budgets?bc=%2fazure%2fcloud-adoption-framework%2f_bread%2ftoc.json&toc=%2fazure%2fcloud-adoption-framework%2ftoc.json)來調整成本和預測模型。
- [打造您的雲端採用方案](../plan/plan-intro.md#build-your-cloud-adoption-plan)：建立具有可採取動作的工作負載、資產和時間軸詳細資料的方案。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端治理小組 <li> 雲端營運小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-5-expand-the-landing-zones"></a>步驟5：展開登陸區域

雲端採用架構的準備方法，主要著重于登陸區域的開發，以裝載雲端中的工作負載。 登陸區域執行期間，各種決策可能會影響作業。

請參閱雲端營運小組，以協助複習登陸區域以進行操作改進。 此外，請參閱雲端治理小組，以瞭解「資源一致性」原則和設計指引，這可能會影響登陸區域的設計。

**交付：**

- 部署一或多個可在短期採用方案中裝載工作負載的登陸區域。
- 確定所有登陸區域都符合操作決策和資源一致性需求。

**支援交付完成的指導方針：**

- [改進登陸區域作業](../ready/considerations/landing-zone-operations.md)：改善登陸區域內作業的最佳做法。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端營運小組 <li> 雲端策略小組 <li> 雲端治理小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-6-adoption"></a>步驟6：採用

長期作業可能會受到您在遷移和創新工作期間所做的決策所影響。 在採用程式初期維持一致的一致性，有助於移除生產版本的阻礙。 它也可減少將新解決方案上線至作業管理實務所需的工作。

**交付：**

- 使用資源一致性原則，測試生產部署的作業就緒程度。
- 驗證遵循資源一致性和作業需求的設計指引。
- 記錄 [operations management 活頁簿](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx)中的任何 advanced 作業需求。

**支援交付完成的指導方針：**

- [環境就緒檢查清單](../migrate/migration-considerations/prerequisites/planning-checklist.md)
- [升級前檢查清單](../migrate/migration-considerations/optimize/ready.md)
- [生產版本檢查清單](../migrate/migration-considerations/optimize/promote.md)

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 <li> 雲端營運小組 <li> 雲端策略小組 <li> 雲端治理小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="value-statement"></a>Value 語句

上述步驟可協助您執行控制項和程式，以確保整個企業和所有託管資源的效能。