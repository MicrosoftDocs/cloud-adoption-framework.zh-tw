---
title: 開始使用：組建雲端作業小組
description: 本指南可協助雲端營運小組瞭解範圍、交付專案，以及它們負責的功能。
author: JanetCThomas
ms.author: janet
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.topic: conceptual
ms.date: 04/04/2020
ms.openlocfilehash: af65cff02d7c3768bf5fca554334923cf2172937
ms.sourcegitcommit: 5d6a7610e556f7b8ca69960ba76a3adfa9203ded
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2020
ms.locfileid: "83400155"
---
# <a name="get-started-build-a-cloud-operations-team"></a>開始使用：組建雲端作業小組

營運小組著重于監視、修復及補救與傳統 IT 營運和資產相關的問題。 在雲端中，許多資本成本和營運活動都會轉移給雲端提供者，讓 IT 營運有機會改善並提供更大的價值。

![開始建立雲端營運團隊](../../_images/get-started/operations-team-map.png)

## <a name="step-1-determine-if-a-cloud-operations-team-is-needed"></a>步驟1：判斷是否需要雲端作業小組

在將任何工作負載發行到生產環境之前，必須先達成合約，以提供[雲端作業功能](../../organize/cloud-governance.md)的責任。 針對某些組合，營運責任可能會保留在 DevOps 和雲端採用小組中。 在其他情況下，具有雲端作業經驗的受控服務提供者可能會採用持續的營運責任。

如果沒有任何 DevOps 或服務提供者作業合約，則可以安全地假設 IT 內的某位人員必須承諾進行與管理任何生產工作負載相關的營運責任。

**項**

- 判斷您是否需要雲端作業小組。
- 記錄 [組織對齊] 索引標籤下[RACI 範本](https://archcenter.blob.core.windows.net/cdn/fusion/management/raci-template.xlsx)中的決策和負責的個人。

**支援交付後完成的指引：**

- [雲端作業功能](../../organize/cloud-operations.md)可能會散佈到多個個人或小組。 您必須決定是否需要雲端作業小組。 但生產工作負載一定需要某些層級的作業。
- 如果公司長期的雲端採用策略可以從一個雲端環境中的一個登陸區域傳遞，那麼治理和營運工作的量可能就夠小，可以由一個人或一個小組傳遞。 該小組不太可能被稱為雲端作業，因為它們將提供許多功能。 即使是針對該小組或人員，下列指導方針將有助於確保小組已準備好提供這項重要的作業功能。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端採用小組 <li> 雲端治理小組 |

## <a name="step-2-align-with-other-teams"></a>步驟2：與其他小組對齊

營運小組會針對生產產品群組中的所有工作負載，繼承營運責任。 根據業務的期望和承諾，這些責任可能會因工作負載而異。 由遷移和創新為焦點的雲端採用小組所做的架構決策也會影響可進行的營運承諾。 在執行任何進行中的作業實務之前，請務必與其他小組保持一致。 雲端作業小組應符合 RACI 範本中所識別的其他小組，以確保重要主題的一致，例如安全性、成本、效能、治理、採用和部署。 步驟4和5可協助您進行對齊。

**項**

- 討論每個小組目前的狀態實行和持續採用計畫。

**支援交付後完成的指引：**

- 使用雲端策略小組的成員，檢查您公司的[策略和計畫範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)，以瞭解動機、計量和策略。
- 使用雲端採用小組的成員來檢查您公司的[雲端採用方案範本](../../plan/template.md)，以瞭解時程表和優先順序。
- 開始開發[operations management 活頁簿](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx)，以瞭解已與企業建立的營運需求和承諾。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端營運小組 | <li> 雲端策略小組 <li> 雲端採用小組 <li> 雲端治理小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-3-establish-cadence-with-other-teams"></a>步驟3：建立與其他小組的步調

雲端採用通常是以波浪或版本為准。 與這些版本一致的定期步調，可讓雲端營運小組準備在下一波的最後進行遞交。 在規劃與審查期間，保持與策略、採用及治理小組密切合作，可協助作業小組在未來的營運需求上保持領先。

**項**

- 建立每個支援小組的步調。 可能的話，請將該步調與發行和規劃週期保持一致。
- 與雲端策略小組（或各種小組成員）直接建立不同的步調，以審查與下一波採用相關的營運需求。

**支援交付後完成的指引：**

- 如需會議步調的其他指引，請參閱[雲端作業功能](../../organize/cloud-operations.md#deliverables)。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端營運小組 | <li> 雲端策略小組 <li> 雲端採用小組 <li> 雲端治理小組 |

## <a name="step-4-review-the-methodology"></a>步驟4：檢查方法

回顧雲端採用架構的管理方法，協助建立作業管理的未來願景，以及取得該願景的可行方法。

**項**

- 瞭解支援管理方法的方法、方法和實體系。

**支援交付後完成的指引：**

- 請參閱[管理方法](../../manage/index.md)。

**責任小組：**

- 雲端營運小組負責操作管理的願景和方法。

## <a name="step-5-implement-the-operations-baseline"></a>步驟5：執行作業基準

如果您的雲端環境沒有部署任何作業實務，請從作業基準開始。 該基準會實行雲端原生的無作業/低營運實務，以提供基本層級的作業保護。

**項**

- 在接下來的幾個採用過程中，部署操作環境所需的基本 Azure 伺服器管理設定。

**支援交付後完成的指引：**

- 執行[作業基準](../../manage/azure-server-management/index.md)設定。

**責任小組：**

- 雲端營運小組負責營運基準。

## <a name="step-6-align-business-commitments"></a>步驟6：協調商務承諾

回顧業務的營運基準承諾。 此基準可協助您評估大部分工作負載的一般需求。 此程式也有助於識別各種工作負載的商務專案關係人，並可讓您記錄其持續的操作期望。

**項**

- 記錄商務期望。
- 判斷特定工作負載或平臺是否需要進行 advanced 作業。

**支援交付後完成的指引：**

- 在雲端中建立[商務調整](../../manage/considerations/business-alignment.md)。
- 記錄[operations management 活頁簿](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx)中的組合和操作預期。

**責任小組：**

- 雲端營運小組應該瞭解商務期望，並負責與這些期望進行的營運一致。

## <a name="step-7-operations-maturity"></a>步驟7：作業成熟度

作業改進可能會產生幾個選項：

- 增強作業基準。
- 改善平臺作業。
- 執行工作負載特定的作業。

當其他工作負載轉換至雲端作業時，作業改善的需求就會變得更清楚。

**項**

- 改善營運成熟度以支援業務承諾。

**支援交付後完成的指引：**

- 評估[advanced operations management](../../manage/design-principles.md)的最佳選項。

**責任小組：**

- 雲端營運小組負責營運改進和一段時間的成熟度。

## <a name="step-8-scale-operations-consistency-through-governance"></a>步驟8：透過治理調整作業一致性

隨著作業成熟，請定期與雲端治理小組協調，以在組合中套用作業需求。

**項**

- 協助雲端治理小組實行新的資源一致性需求。

**支援交付後完成的指引：**

- [使用治理強制的資源一致性改進](../../govern/guides/complex/resource-consistency-improvement.md)範例。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端治理小組 | <li> 雲端營運小組 |

## <a name="step-9-adoption-handoffs"></a>步驟9：採用遞交

當新的採用成果完成時，雲端採用小組會將營運責任交給雲端營運小組和雲端治理小組。 保持一致的採用版本，以確保適當的檔和原則一致，以承擔工作負載的責任。

**項**

- 定期審查並接受雲端採用小組的遞交。

**支援交付後完成的指引：**

- 建立將新的[工作負載和資源](https://docs.microsoft.com/azure/azure-resource-manager/custom-providers/concepts-resource-onboarding)上線的程式。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端治理小組 <li> 雲端營運小組 |

## <a name="whats-next"></a>後續步驟

隨著採用和營運規模的增加，請務必定義和自動化治理最佳作法，以擴充現有的 IT 需求。 形成雲端的卓越中心（CCoE）小組是調整雲端採用、雲端作業和雲端治理工作的重要步驟。

深入了解：

- [卓越功能的雲端中心](../../organize/cloud-center-of-excellence.md)
- [組織反模式：接收器和 fiefdoms](../../organize/fiefdoms-silos.md)

瞭解如何藉由開發跨小組的矩陣，識別負責、參與、諮詢和通知（RACI）的合作物件，來協調各小組的責任。 下載並修改[RACI 試算表範本](https://archcenter.blob.core.windows.net/cdn/fusion/management/raci-template.xlsx)。
