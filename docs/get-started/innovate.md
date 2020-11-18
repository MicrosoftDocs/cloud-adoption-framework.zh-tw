---
title: 開始使用：在雲端中建立新的產品和服務
description: 深入瞭解創新方法，以引導您開發新的雲端產品和服務。
author: JanetCThomas
ms.author: janet
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.openlocfilehash: c902bed3b665aac6ca41c5881a239a1efe6d2787
ms.sourcegitcommit: a7eb2f6c4465527cca2d479edbfc9d93d1e44bf1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94713729"
---
# <a name="get-started-accelerate-new-product-and-service-innovation-in-the-cloud"></a>開始使用：加速雲端中的新產品和服務創新

在雲端中建立新的產品和服務，需要的方法與遷移所需的方法不同。 雲端採用架構的創新方法會建立一種方法，以引導您開發新的產品和服務。

本指南使用下圖中反白顯示的雲端採用架構區段。 創新比標準遷移更不容易預測，但仍適用于更廣泛的雲端採用方案內容中。 本指南可協助您的企業提供創新所需的支援，並提供結構來建立整個雲端採用的平衡組合。

![雲端採用架構的圖表，包括創新方法。](../_images/get-started/innovation-map.png)

## <a name="step-1-document-the-business-strategy"></a>步驟1：記錄商務策略

若要避免常見的封鎖程式，請為創新建立清楚且簡潔的商業策略。 關於動機的專案關係人和預期的業務成果，是雲端採用小組所做出的決策。

**交付：**

- 使用 [策略和方案範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx) 來記錄動機和所需的業務成果。

**支援交付完成的指導方針：**

- [動機](../strategy/motivations.md)：策略性調整的第一個步驟是獲得推動創新工作的動機合約。 從商務和 IT 的專案關係人瞭解及分類動機和一般主題開始。
- [業務成果](../strategy/business-outcomes/index.md)：當動機對齊之後，您就可以捕獲所需的業務成果。 這種資訊提供明確的計量，讓您用來測量整體轉換。
- [平衡組合](../strategy/balance-the-portfolio.md)：創新不是每個工作負載的正確採用途徑。 這種採用方式與新的自訂建立應用程式或 *需要* 重新架構或完整重建的工作負載有關。 當動機大幅優先于所有工作負載的創新時，請務必評估組合，以確保這些投資能產生所需的投資報酬率。 您可以創新特定資源和小規模重建工作的現代化，但可能會因為下列快速入門而提供更好的服務 [：加速遷移](./migrate.md)。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端採用小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-2-evaluate-the-business-justification"></a>步驟2：評估商務理由

在第一次建立商務案例的過程中，請從潛在的雲端採用工作中評估初始的高階報酬率。 此步驟的目標是要讓所有專案關係人都能專注于一個簡單的問題：根據可用的資料，雲端的整體採用是一項明智的商務決策嗎？ 根據該問題，小組可以更清楚地瞭解這項創新專案在採用雲端的目標內，如何協助滿足使用者的投射需求。

**交付：**

- 使用 [策略和方案範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx) 來記錄商務理由。

**支援交付完成的指導方針：**

- [業務理由](../strategy/cloud-migration-business-case.md)：在您評估每個機會以在雲端中進行創新之前，請先完成高階商業理由，以建立整體採用計畫的專案關係人對齊。
- [商業價值共識](../innovate/business-value.md)：量化創新的價值可能會在程式初期變得很困難。 本文中的練習可以協助評估與特定創新工作的商業價值的一致。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端採用小組 |

## <a name="step-3-gather-data-and-analyze-assets-and-workloads"></a>步驟3：收集資料並分析資產和工作負載

在大部分的企業中，您可以使用現有的資產（例如應用程式、虛擬機器 (Vm) 和資料）加速創新。 當您規劃創新時，請務必瞭解這些資產遷移至雲端的方式和時機。

**交付：**

- 取得現有清查的原始資料，例如應用程式、Vm 和資料。
- 如果提議的創新相依于現有的清查，請完成下列交付專案：
  - 支援規劃的創新所需之任何支援清查的量化分析。
  - 對傳遞創新所需之任何支援工作負載的定性分析。
- 計算支援創新工作所需的新清查成本。
- 使用精簡的計算來更新 [策略和方案範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx) 中的業務理由。

**支援交付完成的指導方針：**

探索和評量可提供更深入的技術調整層級。 然後，您可以建立動作計畫，以遷移規劃的創新所需的任何相依工作負載。 當公司具有現有的資料來源、集中式應用程式或服務層級，而在企業其餘部分的內容中提供創新時，此案例很常見。

當有相依的系統時，下列文章可以引導探索和評量：

- [清查現有的系統](../digital-estate/inventory.md)：瞭解程式設計的目前狀態，資料驅動的方法是第一個步驟。 探索並收集資料，以啟用所有評量活動。
- [增量合理化](../digital-estate/rationalize.md#incremental-rationalization)：簡化評定工作，將焦點放在所有資產的定性分析，甚至可能支援商務案例。 然後為前10個工作負載新增深入的定性分析。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 |

## <a name="step-4-plan-for-migration-of-dependent-assets"></a>步驟4：規劃相依資產的遷移

當新的創新相依于現有的工作負載或資產時，雲端採用方案會提供開發專案待辦專案的加速方法。 然後可以修改待處理專案，以反映探索結果、合理化、所需的技能，以及夥伴合約。

**交付：**

- 部署待處理專案範本。
- 更新範本以反映前10個要遷移的工作負載。
- 更新人員和速度 (人員的時間) 預估發行時間。
- 時間軸風險：
  - 缺乏熟悉 Azure DevOps 可能會使部署程式變慢。
  - 每個工作負載的可用複雜性和資料也可能影響時間軸。

**支援交付完成的指導方針：**

- [雲端採用方案](../plan/template.md)：使用基本範本定義您的計畫。
- [工作負載調整](../plan/workloads.md)：定義待處理專案中的工作負載。
- [投入時間調整](../plan/assets.md)：調整待處理專案中的資產和工作負載，以清楚地定義優先順序工作負載的工作量。
- [人員和時間的對齊方式](../plan/iteration-paths.md)：建立工作負載的反復專案、速度和發行。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 |

## <a name="step-5-align-governance-requirements-to-your-adoption-plan"></a>步驟5：將治理需求對應至您的採用方案

與治理小組討論規劃的創新，可協助您在問題發生前避免許多阻礙。 有時候，創新的新解決方案可能需要在音效治理實務中不建議的作法。 某些必要功能甚至可能會透過自動化工具來封鎖，以進行治理強制。

**交付：**

- 建立創新需求和治理條件約束之間的透明度與瞭解。
- 必要時，更新原則和進程，以反映現有治理條件約束的任何變更或例外狀況。

**支援交付完成的指導方針：**

這些連結可協助採用小組瞭解雲端治理小組的方法：

- [治理方法](../govern/index.md)：此方法會概述考慮公司原則和流程的程式。 然後，您可以建立在雲端企業工作的治理上提供所需的專業領域。
- [公司原則的定義](../govern/corporate-policy.md)：識別及降低業務風險。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端治理小組 <li> 雲端採用小組 | <li> 雲端策略小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-6-define-operational-needs-and-business-commitments"></a>步驟6：定義營運需求和業務承諾

為規劃的創新定義長期營運責任的方案。 已建立的管理基準是否符合您的操作需求？ 如果不是，請針對支援這項創新的技術，評估特定資金營運的選項。

**交付：**

- 完成 [Microsoft Azure 架構審查](/assessments/?id=azure-architecture-review) ，以評估各種架構和作業決策。
- 調整 [operations management 活頁簿](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx) ，以反映任何必要的 advanced 作業。

**支援交付完成的指導方針：**

- [展開管理基準](../manage/best-practices.md)：雲端採用架構的這一節會引導您完成各種轉換到雲端中的作業管理。
- [取得特定的 advanced 作業](../manage/design-principles.md)：探索超越您管理基準的方式。
- 如果需要進行 advanced 作業以支援您的作業需求，請評估 [商務承諾](../manage/considerations/business-alignment.md) ，以判斷這兩個小組的營運責任。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端營運小組 <li> 雲端採用小組 | <li> 雲端策略小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-7-deploy-an-aligned-landing-zone"></a>步驟7：部署對齊的登陸區域

裝載在雲端中的所有資產都在登陸區域內運作。 該登陸區域可能具有明確的治理、安全性和操作需求。 或者，它可能是不支援其他小組的新訂用帳戶。 在任一案例中，請務必從一開始就符合治理和操作需求的登陸區域著手。

從核准的登陸區域開始，可協助您的小組在開發期間及早探索原則違規，或將解決方案發行至生產環境。 及早探索可協助您的小組移除封鎖程式，並讓採用和治理小組有足夠的時間進行變更。

**交付：**

- 在早期創新期間，為初始、低風險的實驗部署第一個登陸區域。
- 開發計畫來重構卓越的雲端中心或中央 IT 小組，以確保治理、安全性及營運一致。
- 時間軸風險：
  - 前10個工作負載的治理、操作和安全性需求可能會使此程式變慢。 重構第一個登陸區域和稍後的登陸區域需要較長的時間，但它應該與遷移工作平行發生。

**支援交付完成的指導方針：**

- [選擇登陸區域](../ready/landing-zone/index.md)：使用此區段來尋找根據採用模式部署登陸區域的正確方法。 然後部署該標準化的程式碼基底。
- [展開登陸區域](../ready/considerations/index.md)：無論起點為何，請找出部署的登陸區域中的間距，以新增資源組織、安全性、治理、合規性和作業所需的元件。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端平臺小組 <li> 雲端採用小組 | <li> 雲端採用小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-8-innovate-in-the-cloud"></a>步驟8：在雲端中創新

創新方法提供最常用於雲端創新的工具和產品管理方法的指引。 這些步驟可協助您開始使用這種方法。

**交付：**

- 以技術為基礎的解決方案，可讓您的客戶為企業提供更豐富的生活和推動價值。
- 以更快的速度反覆運算這些解決方案，並使用雲端增加更多價值的流程和工具：
  - 反復的開發方法。
  - 自訂建立的應用程式。
  - 以技術為基礎的體驗。
  - 使用 IoT 整合實體產品和技術。
  - 環境智慧：將不造成干擾技術整合至環境中。
  - Azure 認知服務：大型資料、AI、機器學習和預測解決方案。

**支援交付完成的指導方針：**

- [商業價值共識](../innovate/business-value.md)：如果自從上一個商業價值共識以來已超過三個月，或從未完成，請從這裡開始。
- [Azure 創新指南](../innovate/innovation-guide/index.md)：使用 azure 創新指南來加速部署創新的解決方案，方法是瞭解可協助您建立最小可行產品 (MVP) 的工具和流程。
- [創新的最佳作法](../innovate/best-practices/index.md)：結合 Azure 服務來建立數位發明的工具鏈。
- [意見反應迴圈](../innovate/considerations/adoption.md)：開發改良的意見反應迴圈，以快速將具影響力創新傳遞給您的客戶。

## <a name="step-9-assess-the-innovation-maturity-of-your-organization"></a>步驟9：評估組織的創新成熟度

為了支援您的創新策略開發，AI 成熟度模型是免費的工具，可協助組織評估其建立和擁有 AI 型系統的能力。 成熟度有四個層級：基本、接近、理想和成熟。 每個層級都包含一組特定的特性，以協助判斷您的組織是否能夠採用特定類型的 AI 解決方案、降低相關聯的風險，以及實行策略。

評量需要5到10分鐘的時間，並測量組織在四個類別中的功能：策略、文化、組織特性和功能。 測量這些類別可讓 AI 成熟度模型計算您的組織分數，並在曲線上提供 AI 創新成熟度的估計。

**交付：**

- 使用 [Ai 成熟度模型工具](https://aiready.microsoft.com) 來評估貴組織的 ai 成熟度，以建立 ai 型系統。

**支援交付完成的指導方針：**

- 當評量完成時，工具的輸出會提供一個分數，以評估 AI 創新成熟度的狀態。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端卓越中心 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="value-statement"></a>Value 語句

本指南所述的步驟可協助您和您的小組在雲端建立創新的解決方案，以建立商業價值、適當地進行控管，並妥善架構。

## <a name="next-steps"></a>後續步驟

雲端採用架構是生命週期的解決方案。 它可協助您開始創新的旅程。 它可協助您的組織開始創新旅程，並讓支援創新工作的團隊更成熟。

下列小組可以使用這些後續步驟來繼續提升其工作的成熟度。 這些平行處理並不是線性的，也不應該視為阻礙。 相反地，每個都是平行值串流，可協助您成熟公司的整體雲端就緒程度。

| 小組 | 下一次反復專案 |
|--|--|
| 雲端 &nbsp; 採用 &nbsp; 小組 | 程式[改進](../innovate/considerations/index.md)可讓您深入瞭解在創新方面提供的方法，以影響客戶及推動重複採用。 |
| 雲端 &nbsp; 策略 &nbsp; 小組 | [策略方法](../strategy/index.md)和[計畫方法](../plan/index.md)是利用採用計畫演進的反復程式。 返回這些總覽頁面，並繼續逐一查看您的商務和技術策略。 |
| 雲端 &nbsp; 平臺 &nbsp; 小組 | 請重新流覽 [就緒的方法](../ready/index.md) ，以繼續推進支援遷移或其他採用工作的整體雲端平臺。 |
| 雲端 &nbsp; 治理 &nbsp; 小組 | 使用 [管理方法](../govern/index.md) 來繼續改善治理流程、原則和專業領域。 |
| 雲端 &nbsp; 營運 &nbsp; 小組 | 建置於 [管理方法](../manage/index.md) 上，以在 Azure 中提供更豐富的作業。 |
