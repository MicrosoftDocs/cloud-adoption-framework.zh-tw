---
title: 開始使用：解除封鎖環境設計和設定
description: 開始設計和設定您的雲端環境。
author: JanetCThomas
ms.author: janet
ms.date: 04/04/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.openlocfilehash: 39e5feba4e0d214a86767bf6a2152ae1887dc383
ms.sourcegitcommit: 60d8b863d431b5d7c005f2f14488620b6c4c49be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83229791"
---
<!-- docsTest:ignore Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template -->

# <a name="get-started-design-and-configuration"></a>開始使用：設計和設定

環境設計和設定是最常見的遷移或創新的攔截器。 快速實行支援長期採用計畫的設計，可能會很棘手。 本文會建立方法和系列的步驟，以協助克服常見的封鎖程式並加速您的採用工作。

![設計和設定入門](../_images/get-started/environment-map.png)

建立有效的環境設計和設定所需的技術工作可能很複雜，但您可以管理範圍，以改善雲端平臺小組的成功機率。 最大的挑戰就是在多個專案關係人之間進行調整，其中有些人員有權停止或減緩採用工作。 這些步驟概述快速符合短期目標並建立長期成功的方法。

## <a name="step-1-document-the-business-strategy"></a>步驟1：記錄商務策略

若要避免常見的遷移封鎖程式，請確定已記載清楚且簡潔的商務策略。 專案關係人的動機、預期的商務結果，以及商業理由在採用和環境設定中都很重要。

明確且精確的商務策略可協助雲端平臺小組瞭解重要的事項，以及在進行環境設定決策時應優先考慮的專案。 特別是，它可協助小組在強制選擇創新速度或遵循控制項時做出決策。

**項**

- 使用[策略和計畫範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)來記錄動機、所需的商業結果，以及高階的商業理由。

**支援交付後完成的指引：**

- [動機](../strategy/motivations.md)：策略對齊的第一個步驟是在促進遷移工作的動機上獲得共識。 一開始先瞭解並分類來自企業和 IT 的各種專案關係人的動機和一般主題。
- [商務結果](../strategy/business-outcomes/index.md)：一旦動機一致之後，就可以捕獲所需的商務結果。 這會提供清楚的計量，讓您用來測量整體轉換。
- [建立雲端遷移商務案例](../strategy/cloud-migration-business-case.md)：這是開發遷移商務案例的最佳起點，清楚瞭解有助於商業理由的公式和工具。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 | 通知的團隊 |
| --- | --- | --- |
| <li> 雲端策略小組 | <li> 雲端採用小組 <li> 卓越或中央 IT 的雲端中心 | <li> 雲端平臺小組 |

## <a name="step-2-assess-the-digital-estate"></a>步驟2：評估數位資產

探索和評量提供更深入的技術調整，並協助您建立可採取行動的計畫，以提供策略。 在此步驟中，您會使用有關環境目前狀態的資料、該資料的量化分析，以及對最高優先順序工作負載的深入質化評量來驗證商務案例。

數位資產評量的輸出可讓雲端平臺小組清楚瞭解端點狀態環境，以及支援採用方案所需的需求。

**項**

- 現有清查上的原始資料。
- 現有清查的量化分析，以精簡商業理由。
- 前10個工作負載的定性分析。
- 已更新[策略和計畫範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)中的商業理由。

**支援交付後完成的指引：**

- [清查現有系統](../digital-estate/inventory.md)：從程式設計、資料驅動的方法中瞭解目前的狀態是第一個步驟。 尋找並收集資料以啟用所有評估活動。
- [增量合理化](../digital-estate/rationalize.md#incremental-rationalization)：簡化評估工作，將焦點放在所有資產的定性分析（可能甚至支援商業案例）。 然後為前10個要遷移的工作負載新增深入的定性分析。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 | 通知的團隊 |
| --- | --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 | <li> 雲端平臺小組 |

## <a name="step-3-create-a-cloud-adoption-plan"></a>步驟3：建立雲端採用方案

「雲端採用方案」範本提供加速開發專案待處理專案的方法。 然後您可以修改待處理專案，以反映評量結果、合理化、skilling，以及已簽訂的夥伴。

回顧短期雲端採用方案和待處理專案，可協助雲端平臺小組在接下來幾個月內瞭解環境的需求。 這可讓他們將前幾個登陸區域的「完成定義」加強。

**項**

- 部署待處理專案（backlog）範本。
- 更新範本，以反映要遷移的前10個工作負載。
- 更新人員和速度來預估發行時間。
- 時程表風險：
  - 缺乏熟悉 Azure DevOps 可能會使部署程式變慢。
  - 每個工作負載可用的複雜度和資料也會影響時間軸。

**支援交付後完成的指引：**

- [雲端採用方案範本](../plan/template.md)：部署基本範本。
- [工作負載對齊](../plan/workloads.md)：定義待處理專案中的工作負載。
- [投入時間對齊](../plan/assets.md)：對齊待處理專案中的資產和工作負載，以清楚地定義優先順序工作負載的工作。
- [人員和時間對齊](../plan/iteration-paths.md)：針對遷移的工作負載建立反復專案、速度（人員的時間）和版本。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 | 通知的團隊 |
| --- | --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 <li> 雲端平臺小組 | <li> 雲端平臺小組 |

## <a name="step-4-deploy-the-first-landing-zone"></a>步驟4：部署第一個登陸區域

一開始，雲端採用小組需要一個登陸區域，可支援初始工作負載的需求。 經過一段時間後，登陸區域將會進行調整，以解決更複雜的工作負載。 現在，請先從初始登陸區域著手，以便及早學習雲端平臺小組和雲端採用小組。

**項**

- 部署第一個用於初始低風險遷移的登陸區域。
- 開發計畫來與雲端中心（CCoE）小組或中央 IT 團隊重構。
- 時程表風險：
  - 前10個工作負載的治理、作業和安全性需求可能會大幅降低此程式的速度。 第一個登陸區域和後續登陸區域的實際重構需要更長的時間，但應平行處理以進行遷移。

**支援交付後完成的指引：**

- [選擇登陸區域](../ready/landing-zone/first-landing-zone.md)：使用本文以根據您的短期採用方案，尋找部署登陸區域的正確方法。 然後部署該標準化的程式碼基底。
- [擴充您的登陸區域](../ready/considerations/index.md)：請不要嘗試符合長期治理、安全性或作業條件約束，除非他們必須支援短期採用計畫。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端平臺小組 | <li> 雲端採用小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-5-deploy-an-initial-governance-foundation"></a>步驟5：部署初始的治理基礎

治理是長期成功進行任何遷移工作的關鍵因素。 加速遷移和業務影響很重要。 但是，沒有治理的速度可能會造成危險。 您的組織需要做出關於治理的決策，以符合您的採用模式，以及治理和合規性需求。

當做出那些決策時，他們會回到雲端平臺小組的平行工作。

**項**

- 部署初始的治理基礎。
- 完成治理基準測試，以規劃未來的改進。
- 時程表風險：-改善原則和治理執行可能會在每個專業領域增加1-4 周。

**支援交付後完成的指引：**

- [治理方法](../govern/index.md)：此方法概述考慮公司原則和流程的程式。 然後建立必要的專業領域，以在雲端企業採用工作的治理上實現。
- [治理基準測試工具](../govern/benchmark.md)：尋找您目前狀態的間隙，讓您可以為未來進行規劃。
- [初始治理基礎](../govern/guides/complex/prescriptive-guidance.md)：瞭解身分識別基準專業領域、安全性基準專業領域，以及建立治理 MVP 所需的部署加速專業領域，這將做為所有採用的基礎。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 | 諮詢小組 |
| --- | --- | --- |
| <li> 雲端治理小組 | <li> 雲端策略小組 <li> 卓越或中央 IT 的雲端中心 | <li> 雲端平臺小組 |

## <a name="step-6-implement-an-operations-baseline"></a>步驟6：執行作業基準

Operations management 是另一項需求，可達到遷移成功。 遷移至雲端，而不需要瞭解進行中的作業是一項有風險的決策。 若要以平行方式進行遷移，您應該開始規劃長期作業。

當這些計畫進行時，它們會送回雲端平臺小組的平行工作。

**項**

- 部署管理基準。
- 完成 operations management 活頁簿。
- 識別任何需要 Azure 架構審查評量的工作負載。
- 時程表風險：
  - 檢查活頁簿：每個應用程式擁有者一小時。
  - 完成 Azure 架構審查評估：每個應用程式一小時。

**支援交付後完成的指引：**

- [管理基準](../manage/index.md)
- [定義商務承諾](../manage/considerations/business-alignment.md)
- [擴展管理基準](../manage/best-practices.md)
- [使用 advanced 作業取得特定](../manage/design-principles.md)

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 | 諮詢小組 |
| --- | --- | --- |
| <li> 雲端營運小組 | <li> 雲端策略小組 <li> 卓越或中央 IT 的雲端中心 | <li> 雲端平臺小組 |

## <a name="step-7-expand-the-landing-zone"></a>步驟7：展開登陸區域

當雲端採用小組開始進行前幾個遷移時，雲端平臺小組可以開始以雲端治理和雲端作業小組的支援，建立結束狀態環境設定。 視雲端採用方案的步調而定，這可能需要在反復發行中進行，並在採用計畫的需求前面加入功能。

**項**

- 採用以測試為導向的開發方法來重構登陸區域。
- 改善登陸區域治理。
- 展開 [登陸區域作業]。
- 執行登陸區域安全性。

**支援交付後完成的指引：**

- [重構登陸區域](../ready/landing-zone/refactor.md)
- [登陸區域的測試導向開發](../ready/considerations/test-driven-development.md)
- [展開登陸區域治理](../ready/considerations/landing-zone-governance.md)
- [展開登陸區域作業](../ready/considerations/landing-zone-operations.md)
- [展開登陸區域安全性](../ready/considerations/landing-zone-security.md)

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端平臺小組 | <li> 雲端採用小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="value-statement"></a>Value 語句

本指南中所述的步驟可協助您和您的小組加速其對已正確設定的企業級雲端環境的路徑。

## <a name="next-steps"></a>後續步驟

在未來的反復專案中考慮下列後續步驟，以建立初始工作：

- [環境技術就緒學習路徑](../ready/suggested-skills.md)
- [遷移環境規劃檢查清單](../migrate/migration-considerations/prerequisites/planning-checklist.md)
