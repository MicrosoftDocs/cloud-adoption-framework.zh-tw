---
title: 開始使用：加速遷移
description: 專案關係人對齊、遷移計畫、部署登陸區域，以及遷移前10個工作負載的建議步驟。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.openlocfilehash: d45898c16e7894a62d65e991737419964a570e34
ms.sourcegitcommit: 60d8b863d431b5d7c005f2f14488620b6c4c49be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83229570"
---
# <a name="get-started-accelerate-migration"></a>開始使用：加速遷移

適當的商務和 IT 專案關係人可以協助您的組織克服遷移障礙，並加速遷移工作。 本文提供專案關係人對齊、遷移計畫、部署登陸區域，以及遷移前10個工作負載的建議步驟。 它也可以協助您進行適當治理和管理所提供的長期成功。

使用本指南來減少整體遷移工作所需的資料量和處理常式。 此程式會利用下圖中反白顯示的雲端採用架構區段。

![開始在 Azure 中進行遷移](../_images/get-started/migration-map.png)

如果您的遷移案例是不尋常的，您可以使用[策略性的遷移和準備工具（智慧型）評](https://docs.microsoft.com/assessments/?id=strategic-migration-assessment)量，找出最符合您目前需求的指導方針，以個人化的方式評估組織的遷移準備。

## <a name="get-started"></a>開始使用

遷移工作負載所需的技術工作和流程相當簡單。 有效率地完成遷移程式很重要。 不過，策略遷移的準備工作將會對時間軸產生更大的影響，並成功完成整體遷移。

加速採用是指解決在遷移期間支援雲端採用小組所需的步驟。 本指南以線性檢查清單格式概述這些反復工作，以協助客戶在任何雲端遷移的正確路徑上開始作業。 為了說明支援步驟的重要性，在本文中，遷移會列為步驟10。 事實上，雲端採用小組很可能會開始進行第一次試驗遷移，同時執行步驟4或5。

## <a name="step-1-align-stakeholders"></a>步驟1：協調專案關係人

若要避免常見的遷移封鎖器，請建立一個清楚且簡潔的商務策略來進行遷移。 專案關係人在動機和預期的商務成果方面的一致，將形成雲端採用小組所做的決定。

- [動機](../strategy/motivations.md)：策略對齊的第一個步驟是在促進遷移工作的動機上獲得共識。 一開始先瞭解並分類來自企業和 IT 的各種專案關係人的動機和一般主題。
- [商務結果](../strategy/business-outcomes/index.md)：一旦動機一致之後，就可以捕獲所需的商務結果。 這會提供可測量整體轉換的清楚計量。

**項**

- 使用[策略和計畫範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)來記錄動機和所需的商務結果。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端採用小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-2-align-partner-support"></a>步驟2：協調合作夥伴支援

合作夥伴、Microsoft 服務或各種 Microsoft 程式都可在整個遷移過程中支援您。

- [瞭解合作關係選項](../migrate/migration-considerations/assess/partnership-options.md)，以找出正確的合作關係和支援層級。

**項**

- 請先建立條款及條件，或其他合約協定，再參與支援合作夥伴。
- 識別[策略和計畫範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)中的核准合作夥伴。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端採用小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-3-gather-data-and-analyze-assets-and-workloads"></a>步驟3：收集資料並分析資產和工作負載

探索和評量提供更深入的技術調整，以建立可採取行動的計畫來提供策略。 在此步驟中，商務案例是使用目前狀態環境的相關資料、該資料的量化分析，以及最高優先順序工作負載的深層評量來進行驗證。

- [清查現有系統](../digital-estate/inventory.md)：從程式設計、資料驅動的方法中瞭解目前的狀態是第一個步驟。 探索並收集資料以啟用所有評估活動。
- [增量合理化](../digital-estate/rationalize.md#incremental-rationalization)：簡化評估工作，將焦點放在所有資產的定性分析（可能甚至支援商業案例）。 然後為前10個要遷移的工作負載新增深入的定性分析。

**項**

- 現有清查上的原始資料。
- 對現有清查的量化分析，以精簡商業理由。
- 前10個工作負載的定性分析。
- 更新[策略和計畫範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)中的商業理由。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 |

## <a name="step-4-make-a-business-case"></a>步驟4：建立商業案例

進行遷移的商務案例可能會是專案關係人之間的反復交談。 在第一次建立商業案例時，評估潛在雲端遷移的初始高階退貨。 此步驟的目標是要確保所有專案關係人都符合一個簡單的問題：「根據可用的日期，雲端的整體採用是明智的商業決策嗎？」。

- [建立雲端遷移商務案例](../strategy/cloud-migration-business-case.md)是開發遷移商務案例的最佳起點，清楚瞭解有助於商業理由的公式和工具。

**項**

- 使用[策略和計畫範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)來記錄商業理由。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端採用小組 |

## <a name="step-5-create-a-migration-plan"></a>步驟5：建立遷移計畫

「雲端採用方案」範本提供加速開發專案待處理專案的方法。 然後您可以修改待處理專案，以反映探索結果、合理化、skilling，以及已簽訂的夥伴。

- [雲端採用方案範本](../plan/template.md)：部署基本範本。
- [工作負載對齊](../plan/workloads.md)：定義待處理專案中的工作負載。
- [工作量調整](../plan/assets.md)：對齊待處理專案中的資產和工作負載，以清楚地定義優先順序工作負載的工作量。
- [人員和時間對齊](../plan/iteration-paths.md)：針對遷移的工作負載建立反復專案、速度（人員的時間）和版本。

**項**

- 部署待處理專案（backlog）範本。
- 更新範本，以反映要遷移的前10個工作負載。
- 更新人員和速度來預估發行時間。
- 時程表風險：
  - 缺乏熟悉 Azure DevOps 可能會使部署程式變慢。
  - 每個工作負載可用的複雜度和資料也會影響時間軸。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 |

## <a name="step-6-build-a-skills-readiness-plan"></a>步驟6：建立技能準備就緒計畫

現有的員工可以在遷移工作中扮演實際操作角色。 不過，可能需要額外的技能。 在此步驟中，小組會自我評估以識別開發這些技能的機會，或使用合作夥伴來加強這些技能。

- [建立技能的準備就緒計畫](../plan/adapt-roles-skills-processes.md)。 快速評估所需和現有的技能，以進一步瞭解應解決的 skilling 需求。

**項**

- 將技能就緒方案新增至[策略和計畫範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 |

## <a name="step-7-deploy-and-align-a-landing-zone"></a>步驟7：部署和對齊登陸區域

所有遷移的資產都會部署在登陸區域中。 一開始，登陸區域將會簡化，以支援較小的工作負載。 經過一段時間，它會進行調整以解決更複雜的工作負載。

- [選擇登陸區域](../ready/landing-zone/first-landing-zone.md)：使用本文來尋找適當的方法，以根據您的採用模式部署登陸區域。 然後部署該標準化的程式碼基底。
- [擴充您的登陸區域](../ready/considerations/index.md)：不論起點為何，在已部署的登陸區域中找出間距，以新增資源組織、安全性、治理、合規性、作業等等的必要元件。

**項**

- 部署第一個登陸區域，以進行初始、低風險的遷移。
- 開發計畫來重構 CCoE 或中央 IT。
- 時程表風險：
  - 前10個工作負載的治理、作業和安全性需求可能會大幅降低此程式的速度。
  - 第一個登陸區域和後續登陸區域的實際重構需要更長的時間，但應平行處理以進行遷移。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端平臺小組 | <li> 雲端採用小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-8-migrate-your-first-10-workloads"></a>步驟8：遷移您的前10個工作負載

遷移前10個工作負載所需的技術工作相當簡單。 這也是一種反復的程式，您會在遷移更多資產時重複此過程。 此程式牽涉到評估您的工作負載（請參閱**步驟 4**）、部署您的工作負載，然後將其發行至您的生產環境。

![反復式遷移工作的階段：評估、部署、發行](../_images/migrate/methodology-effort-only.png)

雲端遷移工具可讓您在單一階段或反復專案中遷移資料中心內的所有 Vm。 不過，在每次反覆運算期間遷移較少的工作負載是比較常見的方式。 將遷移分解成較小的波浪或版本需要更多的規劃，但會降低技術風險和組織變更管理的影響。

在每次反覆運算中，雲端採用小組在遷移工作負載方面會更有説明。 下列步驟將會啟動此成熟度曲線上的技術小組：

1. 使用[Azure 遷移指南](../migrate/azure-migration-guide/index.md)中所述的工具，以純粹 IaaS 方法遷移您的**第一個工作負載**。
2. 展開 [工具] [選項]，以使用[遷移案例](../migrate/azure-best-practices/contoso-migration-overview.md)來進行**遷移和現代化**。
3. 使用[遷移最佳做法](../migrate/azure-best-practices/index.md)中所述的更廣泛方法來開發您的**技術策略**。
4. 透過有效率的**遷移 factory**方法來改善一致性、可靠性和效能，如遷移程式[改善](../migrate/migration-considerations/index.md)中所述。

**項**

持續改進採用小組遷移工作負載的能力。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-9-hand-off-production-workloads-to-cloud-governance"></a>步驟9：將生產工作負載交給雲端治理

治理是長期成功進行任何遷移工作的關鍵因素。 加速遷移和業務影響很重要。 但是，沒有治理的速度可能會造成危險。 您的組織需要做出關於治理的決策，其符合您的採用模式，以及治理和合規性需求。

- [治理方法](../govern/index.md)：此方法概述考慮公司原則和流程的程式。 然後建立必要的專業領域，以在雲端企業採用工作的治理上實現。
- [初始治理基礎](../govern/guides/complex/prescriptive-guidance.md)：瞭解建立治理 MVP 所需的身分識別基準專業領域、安全性基準專業領域和部署加速專業領域，以作為所有採用的基礎。

**項**

- 部署初始的治理基礎。
- 完成治理基準測試，以規劃未來的改進。
- 時程表風險：-改善原則和治理執行可能會在每個專業領域增加1-4 周。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端治理小組 | <li> 雲端策略小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-10-hand-off-production-workloads-to-cloud-operations"></a>步驟10：將生產工作負載交給雲端作業

Operations management 是另一項需求，可達到遷移成功。 將個別工作負載遷移至雲端，而不需要瞭解持續的企業營運，是一項有風險的決策。 與遷移同時，您應該開始規劃長期作業。

- [管理基準](../manage/index.md)
- [定義商務承諾](../manage/considerations/business-alignment.md)
- [擴展管理基準](../manage/best-practices.md)
- [使用 advanced 作業取得特定](../manage/design-principles.md)

**項**

- 部署管理基準。
- 完成 operations management 活頁簿。
- 識別任何需要 Azure 架構審查評量的工作負載。
- 時程表風險：
  - 檢查活頁簿：每個應用程式擁有者一小時。
  - 完成 Azure 架構審查評估：每個應用程式一小時。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端營運小組 | <li> 雲端策略小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="value-statement"></a>Value 語句

本指南中所述的步驟可協助您的小組透過更佳的變更管理和專案關係人對齊，加速其遷移工作。 遵循這些步驟可能會使程式變慢，但這些步驟將會移除常見的封鎖程式，並加速實現商業價值。

## <a name="next-steps"></a>後續步驟

雲端採用架構是生命週期解決方案。 它可協助您開始進行遷移旅程，但也可以協助您提升支援遷移工作的小組成熟度。 下列小組可以使用這些後續步驟來繼續提升其工作的成熟度。 這些平行處理常式不是線性的，而且不應該被視為封鎖器。 相反地，每個都是平行值串流，以協助您的公司整體雲端就緒程度成熟。

| 小組  | 下一次反復專案 |
|---|---|
| 雲端 &nbsp; 採用 &nbsp; 小組 | 程式[改善](../migrate/migration-considerations/index.md)可讓您深入瞭解如何透過有效率的持續遷移功能，移往遷移 factory。 |
| 雲端 &nbsp; 策略 &nbsp; 小組 | [策略方法](../strategy/index.md)和[計畫方法](../plan/index.md)是以採用計畫發展的反復流程。 返回這些總覽頁面，並繼續逐一查看您的商務和技術策略。 |
| 雲端 &nbsp; 平臺 &nbsp; 小組 | 請造訪[準備好的方法](../ready/index.md)，繼續推進支援遷移或其他採用成果的整體雲端平臺。 |
| 雲端 &nbsp; 治理 &nbsp; 小組 | 使用[管控方法](../govern/index.md)來繼續改善治理流程、原則和專業領域。 |
| 雲端 &nbsp; 營運 &nbsp; 小組 | 建基於[管理方法](../manage/index.md)，以在 Azure 中提供更豐富的作業。 |

如果您的遷移案例不尋常，您可以使用[策略性的遷移和準備工具（智慧型）評估](https://docs.microsoft.com/assessments/?id=strategic-migration-assessment)，針對組織的遷移準備工作進行個人化評估。 根據您在進行評估時提供的答案，我們可以協助您找出最符合您目前需求的指導方針。
