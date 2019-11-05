---
title: 雲端卓越中心
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 描述卓越雲端中心（CCoE）的構成。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/10/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: organize
ms.custom: organize
ms.openlocfilehash: c1819bafaed5e75e6667754d598b075eb3204925
ms.sourcegitcommit: bf9be7f2fe4851d83cdf3e083c7c25bd7e144c20
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/04/2019
ms.locfileid: "73564307"
---
# <a name="cloud-center-of-excellence"></a>雲端卓越中心

商務和技術靈活性是大多數 IT 組織的核心目標。 卓越的雲端中心（CCoE）是在速度和穩定性之間建立平衡的功能。

## <a name="function-structure"></a>函數結構

CCoE 需要下列各項功能之間的共同作業：

- 雲端採用（特別是解決方案架構設計人員）
- 雲端策略（特別是方案和專案經理）
- 雲端治理
- 雲端平台
- 雲端自動化

## <a name="impact-and-cultural-change"></a>影響和文化變更

當此函式正確地結構化並受到支援時，參與者可以加速創新和遷移工作，同時降低變更的整體成本並增加企業的靈活性。 成功實行時，此函式可能會產生顯著的上市時間縮減。 隨著小組實務成熟，品質指標將會改善，包括可靠性、效能效率、安全性、維護性和客戶滿意度。 如果公司打算執行大規模的雲端遷移工作，或想要使用雲端來驅動與市場差異相關聯的創新，這些提升效率、彈性和品質方面特別重要。

成功時，CCoE 模型會在其中建立重大的文化轉變。 CCoE 方法的基本部署，是做為企業的代理人、合作夥伴或代表。 此模型是從傳統觀點轉移到企業和 IT 資產之間的營運單位或抽象層的一種模式。

下圖提供此文化變更的比喻。 如果沒有 CCoE 方法，它通常會專注于提供控制和集中式責任，其作用就像是交集的 stoplights。 當 CCoE 成功時，焦點就是在自由和委派的責任上，這種方式比較像是交集的這繞遠路。

![類似于 CCoE 架構的轉變](../_images/ready/ccoe-paradigm-shift.png)

上述的類比影像中所述的兩種方法都是正確或不正確，它們只是責任和管理的替代觀點。 如果想要建立自助模型，讓業務單位做出自己的決策，同時遵守一組指導方針和已建立的可重複控制項，則 CCoE 模型可能會納入技術策略內。

## <a name="key-responsibilities"></a>主要責任

CCoE 小組的主要職責是透過雲端原生或混合式解決方案來加速雲端採用。

CCoE 的目標是：

- 透過靈活的方法，協助打造現代化的 IT 組織，以獲取並實行商務需求。
- 使用與安全性、合規性和服務管理原則一致的可重複使用部署套件。
- 維護功能完善的 Azure 平臺，配合操作程式。
- 審查及核准使用雲端原生工具。
- 經過一段時間，將經常需要的平臺元件和解決方案標準化並自動化。

## <a name="meeting-cadence"></a>會議步調

CCoE 是由四個高需求小組所組成的函式。 請務必允許進行有機共同作業，並透過通用存放庫/解決方案目錄來追蹤成長。 最大化自然互動，但最小化會議。 當此功能成熟時，小組應該嘗試限制專用會議。 週期性會議的出席者（例如雲端採用小組所裝載的發行會議）將會提供資料輸入。 就平行而言，每個發行計畫在共用後的會議可以為這個小組提供最小的觸控點。

## <a name="solutions-and-controls"></a>解決方案和控制項

CCoE 的每個成員都負責瞭解導致目前 IT 控制項集的必要條件約束、風險和保護。 CCoE 的集體成果應該將該瞭解轉變為雲端原生（或混合式）解決方案或控制項，以實現所需的自助商業成果。 建立解決方案時，會以控制項或自動化形式與各種小組共用，以作為各種工作的護欄。 這些護欄有助於路由各種小組的免費流動活動，同時將責任委派給參與者，以進行各種不同的遷移或創新工作。

此轉換的範例：

| 案例 | 預先 CCoE 的解決方案 | CCoE 後解決方案 |
|---------|---------|---------|
| 布建生產 SQL Server | 網路、IT 和資料平臺小組會在數天或甚至數周內布建各種元件。 | 需要伺服器的小組會部署 Azure SQL Database 的 PaaS 實例。 或者，預先核准範本可用來將所有 IaaS 資產部署到雲端（以小時為單位）。 |
| 布建開發環境 | 網路、IT、開發和 DevOps 小組同意規格和部署環境。 | 開發小組會定義自己的規格，並根據配置的預算來部署環境。 |
| 更新安全性需求以改善資料保護 | 網路、IT 和安全性小組會跨多個環境更新各種網路裝置和 Vm，以新增保護。 | 雲端治理工具可用來更新原則，以立即套用至所有雲端環境中的所有資產。 |

## <a name="negotiations"></a>初始化

At the root of any CCoE effort is an ongoing negotiation process. The CCoE team negotiates with existing IT functions to reduce central control. The trade-offs for the business in this negotiation are freedom, agility, and speed. The value of the trade-off for existing IT teams is delivered as new solutions. The new solutions provide the existing IT team with one or more of the following benefits:

- Ability to automate common issues.
- Improvements in consistency (reduction in day-to-day frustrations).
- Opportunity to learn and deploy new technical solutions.
- Reductions in high severity incidents (fewer quick fixes or late-night pager-duty responses).
- Ability to broaden their technical scope, addressing broader topics.
- Participation in higher-level business solutions, addressing the impact of technology.
- Reduction in menial maintenance tasks.
- Increase in technology strategy and automation.

In exchange for these benefits, the existing IT function may be trading the following values, whether real or perceived:

- Sense of control from manual approval processes.
- Sense of stability from change control.
- Sense of job security from completion of necessary yet repetitive tasks.
- Sense of consistency that comes from adherence to existing IT solution vendors.

In healthy cloud-forward companies, this negotiation process is a dynamic conversation between peers and partnering IT teams. The technical details may be complex, but are manageable when IT understands the objective and is supportive of the CCoE efforts. When IT is less than supportive, the following section on enabling CCoE success can help overcome cultural blockers.

## <a name="enable-ccoe-success"></a>Enable CCoE success

Before proceeding with this model, it is important to validate the company's tolerance for a growth mindset and IT's comfort with releasing central responsibilities. As mentioned above, the purpose of a CCoE is to exchange control for agility and speed.

This type of change takes time, experimentation, and negotiation. There will be bumps and set backs during this maturation process. However, if the team stays diligent and isn't discouraged from experimentation, there is a high probability of success in improving agility, speed, and reliability. One of the biggest factors in success or failure of a CCoE is support from leadership and key stakeholders.

### <a name="key-stakeholders"></a>Key stakeholders

IT Leadership is the first and most obvious stakeholder. IT managers will play an important part. However, the support of the CIO and other executive-level IT leaders is needed during this process.

Less obvious is the need for business stakeholders. Business agility and time-to-market are key motivations for CCoE formation. As such, the key stakeholders should have a vested interest in these areas. Examples of business stakeholders include line-of-business leaders, finance executives, operations executives, and business product owners.

### <a name="business-stakeholder-support"></a>Business stakeholder support

CCoE efforts can be accelerated with support from the business stakeholders. Much of the focus of CCoE efforts is centered around making long-term improvements to business agility and speed. Defining the impact of current operating models and the value of improvements is valuable as a guide and negotiation tool for the CCoE. Documenting the following items is suggested for CCoE support:

- Establish a set of business outcomes and goals that are expected as a result of business agility and speed.
- Clearly define pain points created by current IT processes (such as speed, agility, stability, and cost challenges).
- Clearly define the historical impact of those pain points (such as lost market share, competitor gains in features and functions, poor customer experiences, and budget increases).
- Define business improvement opportunities that are blocked by the current pain points and operating models.
- Establish timelines and metrics related to those opportunities.

These data points are not an attack on IT. Instead, they help CCoE learn from the past and establish a realistic backlog and plan for improvement.

**Ongoing support and engagement:** CCoE teams can demonstrate quick returns in some areas. However, the higher-level goals, like business agility and time-to-market, can take much longer. During maturation, there is a high risk of the CCoE becoming discouraged or being pulled off to focus on other IT efforts.

During the first six to nine months of CCoE efforts, we recommend that business stakeholders allocate time to meet monthly with the IT leadership and the CCoE. There is little need for formal ceremony to these meetings. Simply reminding the CCoE members and their leadership of the importance of this program can go along way to driving CCoE success.

Additionally, we recommend that the business stakeholders stay informed of the progress and blockers experienced by the CCoE team. Many of their efforts will seem like technical minutiae. 不過，對於商務專案關係人而言，請務必瞭解計畫的進度，讓他們可以參與團隊何時失去串流，或是因其他優先順序而成為因應。

### <a name="it-stakeholder-support"></a>IT 專案關係人支援

**支援願景：** 成功的 CCoE 工作需要與現有的 IT 小組成員進行大量的協調。 當一切順利完成時，就會對解決方案造成影響，並熟悉這項變更。 如果不是這種情況，現有 IT 小組的某些成員可能會因為各種原因而想要控制機制。 IT 專案關係人的支援對於發生這些狀況時的 CCoE 是否成功非常重要。 鼓勵和增強式 CCoE 的整體目標，對於將封鎖器解析成適當的協商非常重要。 在罕見的情況下，IT 專案關係人甚至可能需要逐步執行並中斷鎖死或系結的投票，以保持 CCoE 的進展。

**維護焦點：** 對於任何受資源限制的 IT 小組而言，CCoE 可能是相當重要的承諾。 從短期專案中移除強式架構設計人員以專注于長期增益，對於不屬於 CCoE 的小組成員來說，可能會造成困難。 It 領導地位和 IT 專案關係人必須專注于 CCoE 的目標。 IT 領導人和 IT 專案關係人的支援必須推遲每日作業的中斷，以利 CCoE 的責任。

**建立緩衝區：** CCoE 小組會試驗新的方法。 其中一些方法無法妥善配合現有的作業或技術限制。 當實驗失敗時，可能會有 CCoE 面臨壓力或要是其他小組的風險。 從「快速失敗」學習機會的結果中，鼓勵和緩衝處理小組是很重要的。 小組必須負責成長思維，以確保他們從這些實驗中學習，並尋找更好的解決方案。

## <a name="next-steps"></a>後續步驟

卓越的雲端中心需要[雲端平臺功能](./cloud-platform.md)和[雲端自動化功能](./cloud-automation.md)。 下一步是要讓[雲端平臺功能](./cloud-platform.md)保持一致。

> [!div class="nextstepaction"]
> [配合雲端平臺功能](./cloud-platform.md)
