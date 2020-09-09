---
title: 開始使用：環境設計和設定
description: 開始解除封鎖雲端環境的設計和設定。
author: JanetCThomas
ms.author: janet
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.openlocfilehash: 82d216ea194e7d94091877dc1e80c2dde0d3f6d1
ms.sourcegitcommit: 8b82889dca0091f3cc64116f998a3a878943c6a1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/09/2020
ms.locfileid: "89603725"
---
# <a name="get-started-environment-design-and-configuration"></a>開始使用：環境設計和設定

環境設計和設定是採用專注于遷移或創新之採用工作的最常見阻礙。 快速執行支援您長期採用計畫的設計可能會很困難。 本文將建立一個方法和一連串的步驟，協助克服常見的封鎖程式並加速採用工作。

![設計與設定入門](../_images/get-started/environment-map.png)

建立有效環境設計和設定所需的技術工作可能很複雜。 您可以管理範圍，以改善雲端平臺小組成功的機率。 最大的挑戰是在多個專案關係人之間保持一致。 其中有些專案關係人有權停止或減緩採用工作。 這些步驟概述快速符合短期目標和建立長期成功的方法。

## <a name="step-1-document-the-business-strategy"></a>步驟1：記錄商務策略

若要避免常見的遷移封鎖器，請確定您有清楚且簡潔的商務策略。 關於動機的專案關係人調整、預期的業務成果，以及商務理由在採用和環境設定中都很重要。

明確且精確的商務策略可協助雲端平臺小組瞭解重要事項，以及在制定環境設定決策時應優先考慮的專案。 尤其是，它可協助小組在強制選擇創新速度或遵循控制項時，做出決策。

**交付：**

- 您可以使用 [策略和方案範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx) 來記錄動機、所需的業務成果，以及高層級的業務理由。

**支援交付完成的指導方針：**

- [瞭解業務動機](../strategy/motivations.md)：策略性調整的第一步，是同意推動遷移工作的動機。 一開始先從不同的專案關係人瞭解及分類動機和一般主題。
- 記載[業務成果](../strategy/business-outcomes/index.md)：當動機對齊之後，您可以抓住所需的業務成果。 此資訊提供您可用來測量整體轉換的清楚計量。
- [打造雲端遷移商務案例](../strategy/cloud-migration-business-case.md)：開始開發商務案例以進行遷移，包括可協助您的業務理由的公式和工具的清楚指引。

<br>

| 責任小組 | 負責和支援的團隊 | 通知的團隊 |
| --- | --- | --- |
| <li> 雲端策略小組 | <li> 雲端採用小組 <li> 卓越或中央 IT 團隊的雲端中心 | <li> 雲端平臺小組 |

## <a name="step-2-assess-the-digital-estate"></a>步驟2：評估數位資產

探索和評量提供更深入的技術調整，可協助您建立可用於提供策略的動作計畫。 在這個步驟中，您會使用環境目前狀態的相關資料來驗證商務案例。 然後，您可以執行該資料的量化分析，以及最高優先順序工作負載的深入定性評量。

數位資產評量的輸出提供雲端平臺小組清楚瞭解的終端狀態環境，以及支援採用計畫所需的需求。

**交付：**

- 現有清查上的原始資料。
- 現有清查的量化分析，以精簡業務理由。
- 前10個工作負載的定性分析。
- 已更新 [策略和方案範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx)中的業務理由。

**支援交付完成的指導方針：**

- [清查現有的系統](../digital-estate/inventory.md)：瞭解程式設計的目前狀態，資料驅動的方法是第一個步驟。 尋找並收集資料，以啟用所有評量活動。
- [增量合理化](../digital-estate/rationalize.md#incremental-rationalization)：簡化評定工作，將焦點放在所有資產的定性分析，甚至可能支援商務案例。 然後針對要遷移的前10個工作負載，新增深入的定性分析。

<br>

| 責任小組 | 負責和支援的團隊 | 通知的團隊 |
| --- | --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 | <li> 雲端平臺小組 |

## <a name="step-3-create-a-cloud-adoption-plan"></a>步驟3：建立雲端採用方案

您的雲端採用方案提供加速開發專案待辦專案的方法。 然後您可以修改待處理專案，以反映評定結果、合理化、所需的技能，以及夥伴合約。

複習短期雲端採用方案和待處理專案，可協助雲端平臺小組瞭解接下來幾個月的環境需求。 此背景有助於將前幾個登陸區域的「完成定義」加強。

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
- [投入](../plan/assets.md)時間調整：調整待處理專案中的資產和工作負載，以清楚地定義優先順序工作負載的工作。
- [人員和時間對齊](../plan/iteration-paths.md)：為已遷移的工作負載建立反復專案、速度和發行。

<br>

| 責任小組 | 負責和支援的團隊 | 通知的團隊 |
| --- | --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 <li> 雲端平臺小組 | <li> 雲端平臺小組 |

## <a name="step-4-deploy-the-first-landing-zone"></a>步驟4：部署第一個登陸區域

一開始，雲端採用小組需要可支援第一波工作負載需求的登陸區域。 經過一段時間後，登陸區域會進行調整以解決更複雜的工作負載。 現在，請從登陸區域開始著手，以針對雲端平臺小組和雲端採用小組進行早期學習。

**交付：**

- 部署第一個登陸區域，以進行初始低風險的遷移。
- 開發計畫以與卓越的雲端中心或中央 IT 團隊進行重構。
- 時間軸風險：
  - 前10個工作負載的治理、操作和安全性需求可能會使此程式變慢。 第一個登陸區域和後續登陸區域的實際重構需要較長的時間，但它應該與遷移工作平行發生。

**支援交付完成的指導方針：**

- [選擇登陸區域](../ready/landing-zone/index.md)：使用此區段可根據您的短期採用方案，找出部署登陸區域的正確方法。 然後部署該標準化的程式碼基底。
- 除非您需要支援短期採用方案，否則請[展開登陸區域](../ready/considerations/index.md)：不要嘗試符合長期治理、安全性或作業限制。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端平臺小組 | <li> 雲端採用小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-5-deploy-an-initial-governance-foundation"></a>步驟5：部署初始治理基礎

治理是長期成功的任何遷移工作的關鍵因素。 加速遷移和業務衝擊相當重要。 但是沒有治理的速度可能很危險。 您的組織必須制定符合您採用模式的治理決策，以及治理和合規性需求。

當這些決策完成時，他們會回到雲端平臺小組的平行工作。

**交付：**

- 部署初始治理基礎。
- 完成治理基準，以規劃未來的改進。
- 時間軸風險：
  - 原則和治理實施的改進可能會在每個專業領域增加一到四周。

**支援交付完成的指導方針：**

- [治理方法](../govern/index.md)：此方法會概述考慮公司原則和流程的程式。 然後建立在雲端企業採用工作間治理所需的專業領域。
- [治理](../govern/benchmark.md)效能評定工具：找出您目前狀態的間隙，讓您可以規劃未來。
- [初始治理基礎](../govern/guides/complex/prescriptive-guidance.md)：瞭解建立治理最小可行產品所需的治理專業領域 (MVP) 作為所有採用的基礎。

<br>

| 責任小組 | 負責和支援的團隊 | 諮詢小組 |
| --- | --- | --- |
| <li> 雲端治理小組 | <li> 雲端策略小組 <li> 卓越或中央 IT 團隊的雲端中心 | <li> 雲端平臺小組 |

## <a name="step-6-implement-an-operations-baseline"></a>步驟6：執行作業基準

遷移至雲端，而不需要瞭解進行中的作業是有風險的。 與遷移平行，開始規劃長期的作業管理。 將這些計畫送入雲端平臺小組的平行工作。

**交付：**

- 部署管理基準。
- 完成 operations management 活頁簿。
- 找出需要 Microsoft Azure 架構良好審核評量的任何工作負載。
- 時間軸風險：
  - 檢查活頁簿：每個應用程式擁有者預估一小時。
  - 完成 Microsoft Azure 架構良好的審核評定：每個應用程式預估一小時。

**支援交付完成的指導方針：**

- [建立管理基準](../manage/index.md)
- [定義商務承諾](../manage/considerations/business-alignment.md)
- [擴展管理基準](../manage/best-practices.md)
- [取得特定的 advanced 作業](../manage/design-principles.md)

<br>

| 責任小組 | 負責和支援的團隊 | 諮詢小組 |
| --- | --- | --- |
| <li> 雲端營運小組 | <li> 雲端策略小組 <li> 卓越或中央 IT 團隊的雲端中心 | <li> 雲端平臺小組 |

## <a name="step-7-expand-the-landing-zone"></a>步驟7：展開登陸區域

當雲端採用小組開始進行前幾個遷移時，雲端平臺小組就可以開始建立最終狀態環境設定，並支援雲端治理和雲端營運團隊。 根據雲端採用計畫的步調，此程式可能需要在反復發行中發生。 功能可能會在採用計畫的需求之前新增。

**交付：**

- 採用以測試為導向的開發方法來重構登陸區域。
- 改進登陸區域治理。
- 展開 [登陸區域作業]。
- 實施登陸區域安全性。

**支援交付完成的指導方針：**

- [重構登陸區域](../ready/landing-zone/refactor.md)
- [登陸區域的測試驅動開發](../ready/considerations/test-driven-development.md)
- [展開登陸區域治理](../ready/considerations/landing-zone-governance.md)
- [展開登陸區域作業](../ready/considerations/landing-zone-operations.md)
- [展開登陸區域安全性](../ready/considerations/landing-zone-security.md)

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端平臺小組 | <li> 雲端採用小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="value-statement"></a>Value 語句

本指南所述的步驟可協助您和您的小組將其路徑加速到已正確設定的企業就緒雲端環境。

## <a name="next-steps"></a>後續步驟

在未來的反復專案中，請考慮下列後續步驟，以初步努力：

- [環境技術就緒的學習途徑](../ready/suggested-skills.md)
- [遷移環境規劃檢查清單](../migrate/migration-considerations/prerequisites/planning-checklist.md)
