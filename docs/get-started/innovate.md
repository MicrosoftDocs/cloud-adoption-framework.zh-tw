---
title: 開始使用：在雲端中建立新的產品和服務
description: 瞭解創新方法，做為引導新雲端產品和服務開發的方法。
author: JanetCThomas
ms.author: janet
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.openlocfilehash: 4e2dbcc350e3ec0bf7ff700d4ef44601e60a2ead
ms.sourcegitcommit: 949b87bad28d32df84df190160089f01826f3a31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2020
ms.locfileid: "88195980"
---
# <a name="get-started-accelerate-new-product-and-service-innovation-in-the-cloud"></a>開始使用：加速雲端中新的產品和服務創新

在雲端中建立新的產品和服務需要不同于遷移所需的方法。 Azure 雲端採用架構中的創新方法會建立一種方法，以引導新產品和服務的開發。

本指南使用下圖中反白顯示的雲端採用架構區段。 創新比標準遷移的預測性更低，但仍適用于更廣泛的雲端採用方案。 本指南可協助您的企業提供創新所需的支援，並提供結構來建立整個雲端採用的平衡組合。

![雲端採用架構的圖表，包括創新方法。](../_images/get-started/innovation-map.png)

## <a name="step-1-document-the-business-strategy"></a>步驟1：記錄商務策略

為避免常見的封鎖程式，請為創新建立清楚且簡潔的商務策略。 專案關係人在動機和預期的商務成果方面的調整，雲端採用小組所做的決定。

**項**

- 使用 [策略和計畫範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx) 來記錄動機和所需的商務結果。

<!-- docsTest:ignore "Get started: Accelerate migration" -->

**支援交付後完成的指引：**

- [動機](../strategy/motivations.md)：策略對齊的第一個步驟是取得推動創新工作之動機的合約。 一開始先瞭解並分類來自企業和 IT 的專案關係人的動機和一般主題。
- [商務結果](../strategy/business-outcomes/index.md)：當動機對齊之後，就可以捕捉所需的商務結果。 這種資訊提供清楚的計量，可讓您用來測量整體轉換。
- [平衡組合](../strategy/balance-the-portfolio.md)：創新不是每個工作負載的正確採用路徑。 這種採用方式與 *需要* 改造或完整重建的新自訂建立應用程式或工作負載有關。 當動機非常適合所有工作負載的創新時，請務必評估該組合，以確保這些投資可以產生所需的投資報酬率。 特定資源的現代化和小規模的重建工作都可以創新，但在開始使用之後，可能會更好服務 [：加速遷移](./migrate.md)。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端採用小組 <li> 卓越或中央 IT 小組的雲端中心 |

## <a name="step-2-evaluate-the-business-justification"></a>步驟2：評估商業理由

在第一次建立商業案例時，評估潛在雲端採用工作的初始高階報酬率。 此步驟的目標是要讓所有專案關係人都能滿足一個簡單的問題：根據可用的資料，雲端的整體採用是明智的商業決策嗎？ 根據該問題，小組可以更妥善地配合此創新專案如何協助滿足使用者在採用雲端時的預估需求。

**項**

- 使用 [策略和計畫範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx) 來記錄商業理由。

**支援交付後完成的指引：**

- [商務理由](../strategy/cloud-migration-business-case.md)：評估每個機會以在雲端中進行創新之前，請先完成高階的商業理由，為整體採用計畫建立專案關係人的一致程度。
- [商業價值共識](../innovate/business-value.md)：量化創新價值可能會在程式初期變得很棘手。 本文中的練習可協助您評估特定創新工作的商業價值。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端採用小組 |

## <a name="step-3-gather-data-and-analyze-assets-and-workloads"></a>步驟3：收集資料並分析資產和工作負載

在大部分的企業中，可以透過使用現有資產（例如應用程式、虛擬機器 (Vm) 和資料）來加速創新。 當您規劃創新時，請務必瞭解這些資產遷移至雲端的方式和時間。

**項**

- 取得現有清查的原始資料，例如應用程式、Vm 和資料。
- 如果提議的創新與現有的清查具有相依性，請完成下列交付專案：
  - 支援規劃的創新所需之任何支援清查的量化分析。
  - 以定性分析提供創新所需的任何支援工作負載。
- 計算支援創新工作所需的新清查成本。
- 使用精簡計算來更新 [策略和計畫範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx) 中的商業理由。

**支援交付後完成的指引：**

探索和評量提供更深入的技術調整層級。 接著，您可以建立動作計畫，以遷移規劃的創新所需的任何相依工作負載。 當公司擁有現有的資料來源、集中式應用程式或服務層級，而這些都是在企業其他部分的內容中提供創新時，這種情況很常見。

當有相依的系統時，下列文章可以引導您探索和評估：

- [清查現有系統](../digital-estate/inventory.md)：從程式設計、資料驅動的方法中瞭解目前的狀態是第一個步驟。 探索並收集資料以啟用所有評估活動。
- [增量合理化](../digital-estate/rationalize.md#incremental-rationalization)：簡化評估工作，將焦點放在所有資產的定性分析，甚至是支援商務案例。 然後為前10個工作負載新增深入的定性分析。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 |

## <a name="step-4-plan-for-migration-of-dependent-assets"></a>步驟4：規劃相依資產的遷移

當新的創新取決於現有的工作負載或資產時，雲端採用方案會提供開發專案待處理專案的加速方法。 然後可以修改待處理專案，以反映探索結果、合理化、所需的技能，以及合作夥伴的合約。

**項**

- 部署待處理專案（backlog）範本。
- 更新範本，以反映前10個要遷移的工作負載。
- 更新人員和速度 (人員的時間) 估計發行時間。
- 時程表風險：
  - 缺乏熟悉 Azure DevOps 可能會使部署程式變慢。
  - 每個工作負載可用的複雜度和資料也會影響時間軸。

**支援交付後完成的指引：**

- [雲端採用方案](../plan/template.md)：使用「基本」範本定義您的方案。
- [工作負載對齊](../plan/workloads.md)：定義待處理專案中的工作負載。
- [工作量調整](../plan/assets.md)：對齊待處理專案中的資產和工作負載，以清楚地定義優先順序工作負載的工作量。
- [人員和時間對齊](../plan/iteration-paths.md)：建立工作負載的反復專案、速度和發行。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 |

## <a name="step-5-align-governance-requirements-to-your-adoption-plan"></a>步驟5：使治理需求符合您的採用方案

與治理小組討論計畫的創新可協助您避免許多封鎖器發生。 有時候，創新的新解決方案可能需要不鼓勵在音效治理實務中採用的做法。 某些必要的功能甚至可能會因為治理強制的自動化工具而遭到封鎖。

**項**

- 建立創新需求與治理限制之間的透明度與瞭解。
- 必要時，更新原則和進程以反映現有治理條件約束的任何變更或例外狀況。

**支援交付後完成的指引：**

這些連結可協助採用小組瞭解雲端治理小組的方法：

- [治理方法](../govern/index.md)：此方法概述考慮公司原則和流程的程式。 接著，您可以建立在雲端企業工作方面提供治理所需的專業領域。
- [公司原則的定義](../govern/corporate-policy.md)：識別並降低業務風險。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端治理小組 <li> 雲端採用小組 | <li> 雲端策略小組 <li> 卓越或中央 IT 小組的雲端中心 |

## <a name="step-6-define-operational-needs-and-business-commitments"></a>步驟6：定義營運需求和商務承諾

針對規劃的創新，定義長期營運責任的計畫。 已建立的管理基準是否符合您的營運需求？ 如果不是，請評估支援這項創新技術之特定資金作業的選項。

**項**

- 完成 [Microsoft Azure 架構審查](https://docs.microsoft.com/assessments/?id=azure-architecture-review) ，以評估各種架構和作業決策。
- 調整 [operations management 活頁簿](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx) 以反映任何必要的 advanced 作業。

**支援交付後完成的指引：**

- [擴充 [管理基準](../manage/best-practices.md)]：雲端採用架構的這一節會引導您完成各種轉換，進入雲端的營運管理。
- [取得特定的先進作業](../manage/design-principles.md)：探索超越您的管理基準的方式。
- 如果需要進行 advanced 作業以支援您的作業需求，請評估 [商務承諾](../manage/considerations/business-alignment.md) ，以判斷兩個小組的營運責任。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端營運小組 <li> 雲端採用小組 | <li> 雲端策略小組 <li> 卓越或中央 IT 小組的雲端中心 |

## <a name="step-7-deploy-an-aligned-landing-zone"></a>步驟7：部署對齊的登陸區域

裝載于雲端的所有資產都在登陸區域內。 該登陸區域可能會有明確的治理、安全性和操作需求。 或者，這可能是不支援其他小組的新訂用帳戶。 在任一案例中，請務必從一開始就遵循治理和營運需求的登陸區域開始。

從核准的登陸區域開始，可協助您的小組在開發期間及早探索原則違規，而不是將解決方案發行到生產環境。 早期探索可協助您的小組移除封鎖程式，並讓採用和治理小組有足夠的時間進行變更。

**項**

- 在早期創新期間，部署第一個登陸區域進行初始、低風險的實驗。
- 開發計畫來與卓越或中央 IT 小組的雲端中心重構，以確保治理、安全性和營運的一致。
- 時程表風險：
  - 前10個工作負載的治理、作業和安全性需求可能會使此程式變慢。 重構第一個登陸區域和之後的登陸區域會花費較長的時間，但應該與遷移工作同時發生。

**支援交付後完成的指引：**

- [選擇登陸區域](../ready/landing-zone/index.md)：使用本節來尋找根據您採用模式部署登陸區域的正確方法。 然後部署該標準化的程式碼基底。
- [擴充您的登陸區域](../ready/considerations/index.md)：不論起點為何，在已部署的登陸區域中找出間距，以新增資源組織、安全性、治理、合規性和作業所需的元件。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端平臺小組 <li> 雲端採用小組 | <li> 雲端採用小組 <li> 卓越或中央 IT 小組的雲端中心 |

## <a name="step-8-innovate-in-the-cloud"></a>步驟8：在雲端中創新

創新方法提供了最常用於雲端創新之工具和產品管理方法的指引。 這些步驟可協助您開始使用這種方法。

**項**

- 以技術為基礎的解決方案，可讓您的客戶生活及推動企業價值。
- 使用雲端更快速地反復執行這些解決方案並增加更多價值的程式和工具：
  - 反復的開發方法。
  - 自訂建立的應用程式。
  - 以技術為基礎的經驗。
  - 使用 IoT 整合實體產品和技術。
  - 環境智慧：將不造成干擾技術整合至環境中。
  - Azure 認知服務： big data、AI、機器學習服務和預測性解決方案。

**支援交付後完成的指引：**

- [商業價值共識](../innovate/business-value.md)：如果自從上一個商業價值共識後已超過三個月，或從未完成過，請從這裡開始。
- [Azure 創新指南](../innovate/innovation-guide/index.md)：使用 azure 創新指南來加速部署創新的解決方案，方法是瞭解可協助您建立最基本的可行產品 (MVP) 的工具和流程。
- [創新最佳做法](../innovate/best-practices/index.md)：結合 Azure 服務來建立數位家發明的工具鏈。
- [意見反應迴圈](../innovate/considerations/adoption.md)：開發改良的意見反應迴圈，以快速提供影響力創新給您的客戶。

## <a name="step-9-assess-the-innovation-maturity-of-your-organization"></a>步驟9：評估您組織的創新成熟度

為了支援創新策略的開發，AI 成熟度模型是免費的工具，可協助組織評估其建立和擁有以 AI 為基礎之系統的能力。 成熟度有四個層級：基本、接近、渴望和成熟。 每個層級都包含一組特定的特性，以協助判斷您的組織能夠採用特定類型的 AI 解決方案、降低相關風險，以及實施策略。

評量需要5到10分鐘的時間，並測量貴組織在四個類別之間的功能：策略、文化、組織特性和功能。 測量這些類別可讓 AI 成熟度模型計算您組織的分數，並在曲線上提供 AI 創新成熟度的預估。

**項**

- 使用 [Ai 成熟度模型工具](https://aiready.microsoft.com) 來評估貴組織的 ai 成熟度，以建立以 ai 為基礎的系統。

**支援交付後完成的指引：**

- 當評估完成時，工具的輸出會提供評估 AI 創新成熟度狀態的分數。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端卓越中心 <li> 卓越或中央 IT 小組的雲端中心 |

## <a name="value-statement"></a>Value 語句

本指南中所述的步驟可協助您和您的小組在雲端建立創新的解決方案，以建立商業價值、妥善管控，並妥善架構。

## <a name="next-steps"></a>後續步驟

雲端採用架構是生命週期解決方案。 它可協助您開始創新的旅程。 它可協助您的組織開始創新的旅程，並提升支援創新工作的團隊成熟度。

下列小組可以使用這些後續步驟來繼續提升其工作的成熟度。 這些平行處理常式不是線性的，而且不應該被視為封鎖器。 相反地，每個都是平行值串流，以協助您的公司整體雲端就緒程度成熟。

| 小組 | 下一次反復專案 |
|---|---|
| 雲端 &nbsp; 採用 &nbsp; 小組 | 程式[改善](../innovate/considerations/index.md)可讓您深入瞭解會影響客戶並推動週期性採用的創新方法。 |
| 雲端 &nbsp; 策略 &nbsp; 小組 | [策略方法](../strategy/index.md)和[計畫方法](../plan/index.md)是以採用計畫發展的反復流程。 返回這些總覽頁面，並繼續逐一查看您的商務和技術策略。 |
| 雲端 &nbsp; 平臺 &nbsp; 小組 | 請造訪 [準備好的方法](../ready/index.md) ，繼續推進支援遷移或其他採用成果的整體雲端平臺。 |
| 雲端 &nbsp; 治理 &nbsp; 小組 | 使用 [管控方法](../govern/index.md) 來繼續改善治理流程、原則和專業領域。 |
| 雲端 &nbsp; 營運 &nbsp; 小組 | 建基於 [管理方法](../manage/index.md) ，以在 Azure 中提供更豐富的作業。 |
