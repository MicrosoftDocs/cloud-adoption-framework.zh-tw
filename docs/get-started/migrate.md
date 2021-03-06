---
title: 開始使用：加速移轉
description: 專案關係人對齊、遷移規劃、部署登陸區域，以及遷移前10個工作負載的建議步驟。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.custom: internal
ms.openlocfilehash: 6c295bdb93d35c4832233fa03f525a236af23c69
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101793004"
---
# <a name="get-started-accelerate-migration"></a>開始使用：加速移轉

適當地調整企業和 IT 專案關係人有助於克服遷移障礙並加速遷移工作。 本文提供下列建議步驟：

- 專案關係人對齊
- 移轉規劃
- 部署登陸區域
- 遷移您的前10個工作負載

它也可協助您執行適當的治理和管理程式。

您可以使用本指南來簡化協調整體遷移工作所需的程式和資料。 本指南使用在此圖中反白顯示的雲端採用架構方法。

![開始在 Azure 中進行遷移](../_images/get-started/migration-map.png)

如果您的遷移案例是典型的，您可以使用 [策略性的遷移和準備就緒工具 (智慧型) 評](/assessments/?id=strategic-migration-assessment)量，以取得組織遷移的個人化評估。 您可以使用它來識別最符合您目前需求的指導方針。

## <a name="get-started"></a>開始使用

遷移工作負載所需的技術工作和程式相當簡單。 請務必有效率地完成遷移程式。 策略性的遷移準備工作對時程表和成功完成整體遷移有更大的影響。

為了加速採用，您必須在遷移期間採取步驟來支援雲端採用小組。 本指南概述這些反復工作，協助客戶在任何雲端遷移的正確途徑上開始。 若要顯示支援步驟的重要性，請在本文中將「遷移」列為步驟10。 在實務上，雲端採用小組可能會使用步驟4或5來平行開始首次試驗遷移。

## <a name="step-1-align-stakeholders"></a>步驟1：讓專案關係人

若要避免常見的遷移封鎖器，請建立明確且簡潔的商務策略來進行遷移。 針對動機和預期的業務成果進行專案關係人的調整，是由雲端採用小組做出的決策。

- [動機](../strategy/motivations.md)：策略性調整的第一個步驟是獲得推動遷移工作的動機合約。 一開始先從不同的專案關係人瞭解及分類動機和一般主題。
- [業務成果](../strategy/business-outcomes/index.md)：當動機對齊之後，您就可以捕獲所需的業務成果。 此資訊提供您可用來測量整體轉換的清楚計量。

**交付：**

- 使用 [策略和方案範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx) 來記錄動機和所需的業務成果。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端採用小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-2-align-partner-support"></a>步驟2：讓合作夥伴的支援保持一致

合作夥伴、Microsoft 服務或各種 Microsoft 程式都可在整個遷移過程中為您提供支援。

- [瞭解合作關係選項](../migrate/migration-considerations/assess/partnership-options.md) ，以找出正確的合作關係和支援層級。

**交付：**

- 在您與支援合作夥伴交流之前，請先建立條款及條件或其他合約協定。
- 在 [策略和方案範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx)中識別核准的合作夥伴。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端採用小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-3-gather-data-and-analyze-assets-and-workloads"></a>步驟3：收集資料並分析資產和工作負載

使用探索和評量來改善技術的調整，並建立執行策略的動作計畫。 在這個步驟中，請使用目前狀態環境的相關資料來驗證商務案例。 然後對最高優先順序的工作負載執行量化分析和深入的定性評量。

- [清查現有的系統](../digital-estate/inventory.md)：使用程式設計資料驅動方法來瞭解目前的狀態。 探索並收集資料，以啟用所有評量活動。
- [增量合理化](../digital-estate/rationalize.md#incremental-rationalization)：簡化評定工作，將焦點放在所有資產的定性分析，甚至可能支援商務案例。 然後針對要遷移的前10個工作負載，新增深入的定性分析。

**交付：**

- 現有清查上的原始資料。
- 對現有清查的量化分析，以精簡業務理由。
- 前10個工作負載的定性分析。
- [策略和方案範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx)中記載的業務理由。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 |

## <a name="step-4-make-a-business-case"></a>步驟4：建立商務案例

進行遷移的商務案例可能是專案關係人之間的反復交談。 在第一次建立商務案例的過程中，請評估可能的雲端遷移初始高階報酬。 此步驟的目標是要確保所有專案關係人都能符合一個簡單的問題：根據可用的資料，雲端的整體採用是一項明智的商務決策嗎？

- [建立雲端遷移商務案例](../strategy/cloud-migration-business-case.md) 是開發遷移商務案例的絕佳起點。 清楚地說明公式和工具可協助您進行商務理由。

**交付：**

- 使用 [策略和方案範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx) 來記錄商務理由。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端採用小組 |

## <a name="step-5-create-a-migration-plan"></a>步驟5：建立遷移計畫

雲端採用方案提供加速開發專案待辦專案的方法。 然後可以修改待處理專案，以反映探索結果、合理化、所需的技能，以及夥伴合約。

- [雲端採用方案](../plan/template.md)：使用基本範本定義您的雲端採用計畫。
- [工作負載調整](../plan/workloads.md)：定義待處理專案中的工作負載。
- [投入](../plan/assets.md)時間調整：調整待處理專案中的資產和工作負載，以明確地定義優先工作負載的投入時間。
- [人員和時間的對齊方式](../plan/iteration-paths.md)：建立反復專案、速度 (人員的時間) ，以及已遷移工作負載的版本。

**交付：**

- 部署待處理專案範本。
- 更新範本以反映前10個要遷移的工作負載。
- 更新人員和速度以預估發行時間。
- 時間軸風險：
  - 缺乏熟悉 Azure DevOps 可能會使部署程式變慢。
  - 每個工作負載的可用複雜性和資料也可能影響時間軸。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 |

## <a name="step-6-build-a-skills-readiness-plan"></a>步驟6：建立技能就緒計畫

現有的員工可以在遷移工作中扮演實際操作角色，但可能需要額外的技巧。 在此步驟中，您可以找到開發這些技能的方法，或使用合作夥伴來加入這些技能。

- [打造技能就緒方案](../plan/adapt-roles-skills-processes.md)。 快速評估您現有的技能，以找出小組應開發的其他技能。

**交付：**

- 將技能就緒計畫新增至 [策略和方案範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx)。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 |

## <a name="step-7-deploy-and-align-a-landing-zone"></a>步驟7：部署和對齊登陸區域

所有遷移的資產都會部署至登陸區域。 登陸區域開始簡單以支援較小的工作負載，然後調整規模以因應一段時間更複雜的工作負載。

- [選擇登陸區域](../ready/landing-zone/index.md)：根據採用模式尋找部署登陸區域的正確方法。 然後部署該標準化的程式碼基底。
- [展開登陸區域](../ready/considerations/index.md)：無論您的起點、找出部署的登陸區域中的間距，以及為資源組織、安全性、治理、合規性和作業新增必要的元件。

**交付：**

- 部署第一個登陸區域，以部署初始的低風險遷移。
- 使用卓越的雲端中心或中央 IT 團隊來開發重構計畫。
- 時間軸風險：
  - 前10個工作負載的治理、操作和安全性需求可能會使此程式變慢。
  - 重構第一個登陸區域和後續登陸區域需要較長的時間，但它應該與遷移工作平行發生。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端平臺小組 | <li> 雲端採用小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-8-migrate-your-first-10-workloads"></a>步驟8：遷移您的前10個工作負載

遷移前10個工作負載所需的技術工作相當簡單。 這也是您在遷移更多資產時所重複的反復程式。 在此程式中，您會評估您的工作負載、部署工作負載，然後將它們發行至生產環境。

![反復遷移工作的階段：評估、部署、發行](../_images/migrate/methodology-effort-only.png)

雲端遷移工具可讓您在單一階段或反復專案中，遷移資料中心內的所有虛擬機器。 在每個反復專案中遷移較少量的工作負載是比較常見的情況。 將遷移細分為較小的增量需要更多規劃，但它可降低技術風險以及組織變更管理的影響。

在每個反復專案中，雲端採用小組會在遷移工作負載時獲得更好的效能。 這些步驟可協助技術小組成熟其功能：

1. 使用 [Azure 遷移指南](../migrate/azure-migration-guide/index.md)中所述的工具，在單純的基礎結構即服務中遷移您的第一個工作負載 (IaaS) 方法。
2. 使用 [遷移範例](../migrate/azure-best-practices/contoso-migration-overview.md)展開工具選項，以使用遷移和現代化。
3. 使用 [Azure 雲端遷移最佳做法](../migrate/azure-best-practices/index.md)中所述的廣泛方法來開發技術策略。
4. 透過有效的遷移處理站方法來改善一致性、可靠性和效能，如 [遷移程式改進](../migrate/migration-considerations/index.md)所述。

**交付：**

持續改進採用小組遷移工作負載的能力。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端策略小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-9-hand-off-production-workloads-to-cloud-governance"></a>步驟9：將生產工作負載交給雲端治理

治理是長期成功的任何遷移工作的關鍵因素。 加速遷移和業務衝擊相當重要。 但是沒有治理的速度可能很危險。 您的組織必須制定符合您採用模式的治理決策，以及治理和合規性需求。

- [治理方法](../govern/index.md)：此方法會概述思考公司原則和流程的程式。 在決定您的方法之後，您可以建立在企業雲端採用工作之間啟用治理所需的專業領域。
- [初始治理基礎](../govern/guides/complex/prescriptive-guidance.md)：瞭解建立治理最小可行產品所需的專業領域 (MVP) ，作為所有採用的基礎。
- [治理基準測試](https://aka.ms/adopt/assess/govern)：找出組織目前治理狀態的差距。 取得個人化的基準報告，以及如何開始使用的策劃指引。

**交付：**

- 部署初始治理基礎。
- 完成治理基準，以規劃未來的改進。
- 時間軸風險：
  - 原則和治理實施的改進可能會在每個專業領域增加一到四周。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端治理小組 | <li> 雲端策略小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-10-hand-off-production-workloads-to-cloud-operations"></a>步驟10：將生產工作負載交給雲端作業

作業管理是達到成功遷移的另一項需求。 將個別工作負載遷移至雲端，而不需要瞭解進行中的企業營運，是有風險的決策。 與遷移平行，您應該開始規劃長期作業。

- [建立管理基準](../manage/index.md)
- [定義商務承諾](../manage/considerations/business-alignment.md)
- [擴展管理基準](../manage/best-practices.md)
- [取得特定的 advanced 作業](../manage/design-principles.md)

**交付：**

- 部署管理基準。
- 完成 operations management 活頁簿。
- 識別任何需要 Microsoft Azure Well-Architected 審核評定的工作負載。
- 時間軸風險：
  - 檢查活頁簿：每個應用程式擁有者預估一小時。
  - 完成 Microsoft Azure Well-Architected 審核評定：每個應用程式預估一小時。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端營運小組 | <li> 雲端策略小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="value-statement"></a>Value 語句

這些步驟可協助小組透過更佳的變更管理和專案關係人的調整來加速其遷移工作。 這些步驟也會移除常見的封鎖，並更快速地實現商業價值。

## <a name="next-steps"></a>下一步

雲端採用架構是一種生命週期解決方案，可協助您開始進行遷移旅程。 它也有助於成熟支援遷移工作的小組。 下列小組可以使用這些後續步驟來繼續成熟其功能。 這些平行處理並不是線性的，也不應視為封鎖程式。 相反地，每個都是平行值串流，可協助改善您組織的整體雲端就緒程度。

| 小組 | 下一次反復專案 |
|--|--|
| 雲端採用小組 | 使用 [遷移模型](../migrate/migration-considerations/index.md) 來瞭解如何移至可提供有效率的持續遷移功能的遷移 factory。 |
| 雲端策略小組 | 反復改善 [策略方法](../strategy/index.md) 和 [計畫方法](../plan/index.md) 以及採用計畫。 複習這些總覽，然後繼續逐一查看您的商務和技術策略。 |
| 雲端平臺小組 | 請重新流覽 [就緒的方法](../ready/index.md) ，以繼續推進支援遷移或其他採用工作的整體雲端平臺。 |
| 雲端 &nbsp; 治理 &nbsp; 小組 | 使用 [管理方法](../govern/index.md) 來繼續改善治理流程、原則和專業領域。 |
| 雲端營運小組 | 建置於 [管理方法](../manage/index.md) 上，以在 Azure 中提供更豐富的作業。 |

如果您的遷移案例是典型的，您可以使用 [策略性的遷移和準備就緒工具 (智慧型) 評](/assessments/?id=strategic-migration-assessment)量，以取得組織遷移的個人化評估。 您提供的答案可協助您找出最符合您目前需求的指導方針。
