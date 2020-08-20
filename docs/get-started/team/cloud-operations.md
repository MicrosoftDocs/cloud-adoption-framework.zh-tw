---
title: 開始使用：建立雲端作業小組
description: 本指南可協助雲端營運團隊瞭解範圍、交付專案，以及小組負責的功能。
author: JanetCThomas
ms.author: janet
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.topic: conceptual
ms.date: 05/15/2020
ms.openlocfilehash: eb9c57c8ddc91821eb6dcde48279cd08fbd5c3db
ms.sourcegitcommit: 12fa4597633ca8e04efbae7d0bd7526d3581618e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/20/2020
ms.locfileid: "88661940"
---
# <a name="get-started-build-a-cloud-operations-team"></a>開始使用：建立雲端作業小組

營運小組著重于監視、修復和修復與傳統 IT 作業和資產相關的問題。 在雲端中，許多資本成本和營運活動都會轉移給雲端提供者，讓 IT 營運人員有機會改善並提供更高的價值。

![開始建立雲端營運小組](../../_images/get-started/operations-team-map.png)

## <a name="step-1-determine-whether-a-cloud-operations-team-is-needed"></a>步驟1：判斷是否需要雲端營運小組

在您可以將任何工作負載發行到生產環境之前，必須先達成合約以提供 [雲端作業功能](../../organize/cloud-operations.md)的責任。 針對某些組合，營運責任可能屬於 DevOps 和雲端採用小組。 在其他情況下，具有雲端作業經驗的受控服務提供者可能會採用持續營運的職責。

如果沒有任何 DevOps 或服務提供者作業合約，就可以安全地假設 IT 內的某個人必須致力於管理生產工作負載的持續營運責任。

**交付：**

- 判斷您是否需要雲端營運團隊。
- 開發跨小組的矩陣，以識別 *負責、負責任、諮詢及告知 (RACI) * 合作物件的跨小組，以協調各小組的責任。 記錄工作表的 [RACI 範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/organize/raci-template.xlsx) 中的決策與負責任人員 `Org Alignment` 。

**支援交付完成的指導方針：**

- [雲端作業功能](../../organize/cloud-operations.md) 可能會散佈到多個個人或團隊。 決定是否需要雲端營運團隊。 生產工作負載一律需要某些層級的作業。
- 如果公司的長期雲端採用策略可從一個雲端環境中的一個登陸區域來傳遞，則治理和作業工作可能夠小，而無法由一個人或一個小組提供。 該小組不太可能被稱為雲端作業，因為它會提供許多功能。 針對該個人或團隊，下列指導方針可協助確保它可以提供這項重要的作業功能。

<!-- markdownlint-disable MD033 -->

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端採用小組 <li> 雲端治理小組 |

## <a name="step-2-align-with-other-teams"></a>步驟2：與其他團隊保持一致

雲端營運團隊針對生產產品群組中的所有工作負載，繼承營運責任。 這些責任會根據預期以及小組對商務專案關係人所做的承諾，在工作負載之間有所不同。 以遷移為焦點且創新的雲端採用小組所進行的架構決策也會影響小組的營運承諾。

在雲端營運團隊實施任何進行中的作業實務之前，請務必讓 it 與其他團隊保持一致。 小組應符合 RACI 範本中所識別的其他小組，以確保符合重要主題的一致性，例如安全性、成本、效能、治理、採用和部署。 步驟4和5可協助進行這種調整。

**交付：**

- 與每個小組討論目前狀態的執行和持續採用計畫。

**支援交付完成的指導方針：**

- 若要瞭解團隊動機、計量和策略，請使用雲端策略小組的成員來檢查貴公司的 [策略和方案範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx) 。
- 若要瞭解時程表和優先順序，請使用雲端採用小組的成員來檢查貴公司的 [雲端採用方案](../../plan/template.md) 。
- 若要瞭解小組與企業建立的營運需求和承諾，請開始開發 [operations management 活頁簿](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx)。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端營運小組 | <li> 雲端策略小組 <li> 雲端採用小組 <li> 雲端治理小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-3-establish-a-cadence-with-other-teams"></a>步驟3：與其他小組建立步調

雲端採用通常是以波浪或發行的方式進行。 與這些版本一致的定期步調，可讓雲端營運團隊在下一個 wave 結束時做好遞交準備。 在規劃和審核期間持續與策略、採用和治理小組保持聯繫，可協助營運團隊保持未來的營運需求。

**交付：**

- 與支援小組建立步調。 可能的話，請將該步調與發行和規劃週期保持一致。
- 您可以直接與雲端策略小組或其各個小組成員建立個別的步調，以審視與下一波採用相關聯的任何操作需求。

**支援交付完成的指導方針：**

- 如需步調 for meeting 的其他指引，請參閱 [雲端作業功能](../../organize/cloud-operations.md#deliverables)的「交付專案」一節。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端營運小組 | <li> 雲端策略小組 <li> 雲端採用小組 <li> 雲端治理小組 |

## <a name="step-4-review-the-methodology"></a>步驟4：檢查方法

若要協助建立作業管理的未來願景，以及達成該願景的可行方法，請參閱雲端採用架構的管理方法。

**交付：**

- 深入瞭解支援管理方法的方法、方法和執行。

**支援交付完成的指導方針：**

- 複習 [雲端採用架構的管理方法](../../manage/index.md)。

**責任小組：**

- 雲端營運小組負責處理營運管理的願景和方法。

## <a name="step-5-implement-the-operations-baseline"></a>步驟5：執行作業基準

如果作業實務尚未部署到您的雲端環境，請從作業基準開始著手。 該基準將會執行雲端原生、無 ops/低營運實務實務，以提供基本層級的作業保護。

**交付：**

- 在接下來的幾個採用工作期間，部署操作環境所需的基本 Azure 伺服器管理設定。

**支援交付完成的指導方針：**

- 執行 [作業基準](../../manage/azure-server-management/index.md) 設定。

**責任小組：**

- 雲端營運小組負責執行營運基準。

## <a name="step-6-align-business-commitments"></a>步驟6：調整商務承諾

請向商務專案關係人回顧團隊的營運基準承諾。 此基準可協助您評估大部分工作負載的一般需求。 此程式也可協助您識別各種工作負載的專案關係人，並可讓您記錄其持續運作的期望。

**交付：**

- 記錄商務專案關係人的期望。
- 判斷特定工作負載或平臺是否需要進行 advanced 作業。

**支援交付完成的指導方針：**

- 在雲端中建立 [商務調整](../../manage/considerations/business-alignment.md) 。
- 記錄 [operations management 活頁簿](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx)中的組合和操作預期。

**責任小組：**

- 雲端營運團隊應瞭解商務期望，並負責持續配合這些期望。

## <a name="step-7-operations-maturity"></a>步驟7：作業成熟度

藉由持續進行作業改進，小組可以：

- 增強作業基準。
- 改善平臺作業。
- 執行工作負載特定的作業。

當其他工作負載轉換至雲端作業時，作業改進的需求會變得更清楚。

**交付：**

- 改進作業成熟度，以支援對商務專案關係人的承諾。

**支援交付完成的指導方針：**

- 評估 [advanced operations management](../../manage/design-principles.md)的最佳選項。

**責任小組：**

- 雲端營運小組負責操作改進，以及一段時間的成熟度。

## <a name="step-8-scale-operations-consistency-through-governance"></a>步驟8：透過治理調整作業一致性

當作業規劃持續成熟時，小組應定期與雲端治理小組協調，以跨組合套用作業需求。

**交付：**

- 協助雲端治理小組實行資源一致性的新需求。

**支援交付完成的指導方針：**

- 請參閱 [治理指南，以改善資源一致性](../../govern/guides/complex/resource-consistency-improvement.md)。

<!-- markdownlint-disable MD033 -->

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端治理小組 | <li> 雲端營運小組 |

## <a name="step-9-adoption-handoffs"></a>步驟9：採用遞交

當新的採用成果完成時，雲端採用小組會將營運責任交給雲端營運和雲端治理小組。 為了確保適當的檔和原則對齊，並承擔工作負載的責任，小組應該保持與採用版本一致。

**交付：**

- 定期審核和接受雲端採用小組的遞交。

**支援交付完成的指導方針：**

- 建立將新的 [工作負載和資源](/azure/azure-resource-manager/custom-providers/concepts-resource-onboarding)上線的流程。

<!-- markdownlint-disable MD033 -->

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端治理小組 <li> 雲端營運小組 |

## <a name="whats-next"></a>後續步驟

隨著採用和作業規模的考慮，請務必定義和自動化治理的最佳做法，以擴充現有的 IT 需求。  (CCoE) 團隊來形成卓越的雲端中心，是調整雲端採用、雲端營運和雲端治理工作的重要步驟。

深入了解：

- [卓越雲端中心功能](../../organize/cloud-center-of-excellence.md)
- [組織反模式：定址接收器和領域](../../organize/fiefdoms-silos.md)

藉由開發可識別 RACI 合作物件的跨小組矩陣，讓小組之間的職責保持一致。 下載並修改 [RACI 範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/organize/raci-template.xlsx)。
