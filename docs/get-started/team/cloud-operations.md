---
title: 開始使用：組建雲端作業小組
description: 本指南可協助雲端營運小組瞭解範圍、交付專案，以及它們負責的功能。
author: JanetCThomas
ms.author: janet
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.topic: conceptual
ms.date: 05/15/2020
ms.openlocfilehash: 08090044ab0d97f8bdcf5258b0df0cbcb4c54bcd
ms.sourcegitcommit: 9b183014c7a6faffac0a1b48fdd321d9bbe640be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2020
ms.locfileid: "85075767"
---
# <a name="get-started-build-a-cloud-operations-team"></a>開始使用：組建雲端作業小組

營運小組著重于監視、修復和補救與傳統 IT 營運和資產相關的問題。 在雲端中，許多資本成本和營運活動都會轉移給雲端提供者，讓 IT 營運有機會改善並提供更大的價值。

![開始建立雲端營運團隊](../../_images/get-started/operations-team-map.png)

## <a name="step-1-determine-whether-a-cloud-operations-team-is-needed"></a>步驟1：判斷是否需要雲端作業小組

在您可以將任何工作負載發行到生產環境之前，必須先達成合約，以提供[雲端作業功能](../../organize/cloud-operations.md)的責任。 針對某些組合，營運責任可能會與 DevOps 和雲端採用小組一起使用。 在其他情況下，具有雲端作業經驗的受控服務提供者可能會採用持續的營運責任。

如果沒有任何 DevOps 或服務提供者作業合約，則可以安全地假設 IT 內的某位人員必須認可與生產工作負載管理有關的持續營運責任。

**項**

- 判斷您是否需要雲端作業小組。
- 記錄工作表中[RACI （負責、參與、諮詢和通知）範本](https://archcenter.blob.core.windows.net/cdn/fusion/management/raci-template.xlsx)中的決策和責任人員 `Org Alignment` 。

**支援交付後完成的指引：**

- [雲端作業功能](../../organize/cloud-operations.md)可能會散佈到多個個人或小組。 決定是否需要雲端作業小組。 生產工作負載一定需要某種層級的作業。
- 如果公司的長期雲端採用策略可以從一個雲端環境中的一個登陸區域傳遞，則治理和營運作業可能夠小，而無法由一個人或一個小組傳遞。 該小組不太可能被稱為雲端作業，因為它會提供許多功能。 針對該個人或小組，下列指導方針可協助確保它可以在這項重要的作業功能上提供。

<!-- markdownlint-disable MD033 -->

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端採用小組 <li> 雲端治理小組 |

## <a name="step-2-align-with-other-teams"></a>步驟2：與其他小組對齊

雲端營運小組會針對生產產品群組中的所有工作負載，繼承營運責任。 這些責任可能因工作負載而異，這取決於小組對商務專案關係人所做的期望和承諾。 以遷移為焦點和創新的雲端採用小組所做的架構決策也會影響小組的營運承諾。

在雲端營運小組實行任何進行中的作業實務之前，請務必與其他小組保持一致。 小組應符合 RACI 範本中所識別的其他小組，以確保重要主題的對齊，例如安全性、成本、效能、治理、採用和部署。 步驟4和5可以協助達成這種對齊。

**項**

- 討論每個團隊的目前狀態實行和持續採用計畫。

**支援交付後完成的指引：**

- 若要瞭解小組動機、計量和策略，請使用雲端策略小組的成員來檢查貴公司的[策略和計畫範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)。
- 若要瞭解時程表和優先順序，請使用雲端採用小組的成員來檢查貴公司的[雲端採用方案範本](../../plan/template.md)。
- 若要瞭解小組與企業建立的營運需求和承諾，請開始開發[operations management 活頁簿](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx)。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li>  雲端營運小組 | <li> 雲端策略小組 <li> 雲端採用小組 <li> 雲端治理小組 <li> 卓越或中央 IT 小組的雲端中心 |

## <a name="step-3-establish-a-cadence-with-other-teams"></a>步驟3：建立與其他小組的步調

雲端採用通常是以波浪或版本為准。 與這些版本一致的一般步調可讓雲端作業小組在下一波結束時準備遞交。 在規劃與審查期間，保持與策略、採用及治理小組密切合作，可協助作業小組在未來的營運需求上保持領先。

**項**

- 建立支援小組的步調。 可能的話，請將該步調與發行和規劃週期保持一致。
- 直接與雲端策略小組或其各種小組成員建立不同的步調，以檢查與下一波採用相關的任何操作需求。

**支援交付後完成的指引：**

- 如需步調會議的其他指引，請參閱[雲端操作功能](../../organize/cloud-operations.md#deliverables)的「交付專案」一節。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端營運小組 | <li> 雲端策略小組 <li> 雲端採用小組 <li> 雲端治理小組 |

## <a name="step-4-review-the-methodology"></a>步驟4：檢查方法

若要協助建立作業管理的未來願景，以及達成該願景的可行方法，請參閱雲端採用架構中的管理方法。

**項**

- 瞭解支援管理方法的方法、方法和實體系。

**支援交付後完成的指引：**

- 請參閱[雲端採用架構中的管理方法](../../manage/index.md)。

**責任小組：**

- 雲端營運小組負責操作管理的願景和方法。

## <a name="step-5-implement-the-operations-baseline"></a>步驟5：執行作業基準

如果作業實務尚未部署到您的雲端環境，請從作業基準開始。 該基準會實行雲端原生的無作業/低營運實務，以提供基本層級的作業保護。

**項**

- 在接下來的幾個採用過程中，部署操作環境所需的基本 Azure 伺服器管理設定。

**支援交付後完成的指引：**

- 執行[作業基準](../../manage/azure-server-management/index.md)設定。

**責任小組：**

- 雲端營運小組負責實施作業基準。

## <a name="step-6-align-business-commitments"></a>步驟6：協調商務承諾

向商務專案關係人回顧小組的營運基準承諾。 此基準可協助您評估大部分工作負載的一般需求。 此程式也可協助您識別各種工作負載的專案關係人，並可讓您記錄其持續的操作預期。

**項**

- 記錄商務專案關係人的期望。
- 判斷特定工作負載或平臺是否需要進行 advanced 作業。

**支援交付後完成的指引：**

- 在雲端中建立[商務調整](../../manage/considerations/business-alignment.md)。
- 記錄[operations management 活頁簿](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx)中的組合和操作預期。

**責任小組：**

- 雲端營運小組應該瞭解商務期望，而且必須負責與這些預期一致。

## <a name="step-7-operations-maturity"></a>步驟7：作業成熟度

藉由持續進行作業改善，小組可以：

- 增強作業基準。
- 改善平臺作業。
- 執行工作負載特定的作業。

當其他工作負載轉換至雲端作業時，作業改善的需求就會變得更清楚。

**項**

- 改善營運成熟度，以支援對商務專案關係人的承諾。

**支援交付後完成的指引：**

- 評估[先進作業管理](../../manage/design-principles.md)的最佳選項。

**責任小組：**

- 雲端營運小組負責營運改進和一段時間的成熟度。

## <a name="step-8-scale-operations-consistency-through-governance"></a>步驟8：透過治理調整作業一致性

當作業規劃持續成熟時，小組應定期與雲端治理小組協調，以在組合中套用作業需求。

**項**

- 協助雲端治理小組為資源一致性實行新的需求。

**支援交付後完成的指引：**

- 請參閱[治理指南以改善資源一致性](../../govern/guides/complex/resource-consistency-improvement.md)。

<!-- markdownlint-disable MD033 -->

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端治理小組 | <li> 雲端營運小組 |

## <a name="step-9-adoption-handoffs"></a>步驟9：採用遞交

當新的採用成果完成時，雲端採用小組會將營運責任交給雲端營運和雲端治理小組。 為了確保適當的檔和原則對齊，並承擔工作負載的責任，小組應該保持與採用版本一致。

**項**

- 定期審查並接受雲端採用小組的遞交。

**支援交付後完成的指引：**

- 建立將新的[工作負載和資源](https://docs.microsoft.com/azure/azure-resource-manager/custom-providers/concepts-resource-onboarding)上線的程式。

<!-- markdownlint-disable MD033 -->

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端治理小組 <li> 雲端營運小組 |

## <a name="whats-next"></a>後續步驟

隨著採用和營運規模的增加，請務必定義和自動化治理最佳作法，以擴充現有的 IT 需求。 形成雲端的卓越中心（CCoE）小組是調整雲端採用、雲端作業和雲端治理工作的重要步驟。

深入了解：

- [卓越雲端中心功能](../../organize/cloud-center-of-excellence.md)
- [組織反模式：接收器和 fiefdoms](../../organize/fiefdoms-silos.md)

藉由開發可識別 RACI 合作物件的跨小組矩陣，來協調各小組的責任。 下載並修改[RACI 試算表範本](https://archcenter.blob.core.windows.net/cdn/fusion/management/raci-template.xlsx)。
