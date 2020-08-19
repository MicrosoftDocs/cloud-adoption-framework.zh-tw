---
title: 開始使用：建立雲端治理小組
description: 建立治理小組的範圍、交付專案及功能，以準備成功的雲端治理。
author: JanetCThomas
ms.author: janet
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.topic: conceptual
ms.date: 05/15/2020
ms.openlocfilehash: 9c611e196b9bf5777181b99ef184759eb4d56bb6
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88573865"
---
# <a name="get-started-build-a-cloud-governance-team"></a>開始使用：建立雲端治理小組

雲端治理小組可確保正確評估和管理雲端採用風險和風險承受度。 小組會找出企業無法容忍的風險，並將風險轉換成管理公司原則。

![開始建立雲端治理小組](../../_images/get-started/governance-team-map.png)

## <a name="step-1-determine-whether-a-cloud-governance-team-is-needed"></a>步驟1：判斷是否需要雲端治理小組

適用于 Azure 的雲端採用架構中的官方指引是一律建立雲端治理小組。 一開始，小組可能會很小。 無論其大小為何，其角色都將證明很重要。 如果不需要小組，現有採用小組的群組或個人應同意履行與 [雲端治理功能](../../organize/cloud-governance.md)相關的責任。

**交付：**

- 判斷您是否需要雲端治理小組。
- 開發跨小組的矩陣，以識別 *負責、負責任、諮詢及告知 (RACI) * 合作物件的跨小組，以協調各小組的責任。 使用工作表中的 [RACI 範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/organize/raci-template.xlsx) ，記錄決策和負責任的個人 `Org Alignment` 。

**支援交付完成的指導方針：**

- [雲端治理功能](../../organize/cloud-governance.md) 可能已分散到多個個人或團隊。 擁有「雲端治理小組」標題的小組並不重要，但所需的功能應該與負責任的合作物件或小組並存。
- 如果公司的長期雲端採用策略可從一個雲端環境中的一個登陸區域來交付，則治理和作業工作的數量可能夠小，可供一個人或一個小組傳遞。 該小組不太可能被稱為「雲端治理」，因為它能提供雲端治理以外的許多功能。 即使是該團隊，此入門指南也有助於確保 it 能夠在治理的這項重要功能上提供。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端採用小組 |

## <a name="step-2-align-with-other-teams"></a>步驟2：與其他團隊保持一致

治理小組可確保一致性，並遵循一組常見的原則。 這些原則來自于與其他小組持續保持一致。

雲端治理小組必須先符合 RACI 範本中所識別的其他小組，才能建立原則或自動化雲端治理。 這將有助於確保重要主題的對齊，例如安全性、成本、效能、作業和部署。 步驟4和5可協助您加速對齊。

**交付：**

- 與每個小組討論目前狀態的執行和持續採用計畫。

**支援交付完成的指導方針：**

- 請與雲端策略小組的成員一起審視您公司的 [策略和方案範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx) ，以瞭解動機、計量和策略。
- 利用雲端採用小組的成員，複習您公司的 [雲端採用方案](../../plan/template.md) ，以瞭解時程表和優先順序。
- 請參閱作業小組的 [operations management 活頁簿](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx) ，以瞭解與企業建立的營運需求和承諾。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端治理小組 | <li> 雲端策略小組 <li> 雲端採用小組 <li> 雲端營運小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-3-establish-a-cadence-with-other-teams"></a>步驟3：與其他小組建立步調

雲端採用通常是以波浪或發行的方式進行。 與這些版本一致的定期步調，可讓雲端治理小組事先瞭解並瞭解下一波將引進的風險。 在規劃和審核期間保持與策略、採用和營運團隊保持聯繫，也可協助治理小組提前承擔風險。

**交付：**

- 與支援小組建立步調。 可能的話，請將該步調與發行和規劃週期保持一致。
- 您可以直接與雲端策略小組 (或不同的小組成員建立個別的步調，) 檢查與下一波採用相關的風險，並測量團隊的風險容忍度。

**支援交付完成的指導方針：**

- 如需步調 for meeting 的詳細指引，請參閱 [雲端治理功能](../../organize/cloud-governance.md#deliverable)。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端治理小組 | <li> 雲端策略小組 <li> 雲端採用小組 <li> 雲端營運小組 |

## <a name="step-4-review-the-methodology"></a>步驟4：檢查方法

若要協助建立未來對於治理的願景，以及該願景的運作方式，請參閱雲端採用架構中的控管方法。

**交付：**

- 深入瞭解支援治理方法的方法、方法和執行。

**支援交付完成的指導方針：**

- 請參閱 [管理方法](../../govern/methodology.md)。

**責任小組：**

- 雲端治理小組負責建立治理的願景和方法。

## <a name="step-5-complete-the-governance-benchmark"></a>步驟5：完成治理基準測試

治理是廣泛的主題。 簡短的評量可協助小組瞭解要從何處著手。

**交付：**

- 根據與各專案關係人的交談，完成治理基準測試。 或要求其他團隊自行完成評量。

**支援交付完成的指導方針：**

- 使用 [治理基準測試](../../govern/benchmark.md) 您的治理需求和優先順序。

**責任小組：**

- 雲端治理小組應該瞭解治理基準中所識別的差距，然後提供治理的方向，以協助解決差距。

## <a name="step-6-implement-the-initial-governance-best-practice-and-configuration"></a>步驟6：實行初始治理的最佳作法和設定

治理方法包含初始治理基礎的兩種方法。 請檢查每一種方法，並執行最符合您需求的方法。

**交付：**

- 在接下來的幾個採用工作期間，部署管理環境所需的基本治理工具和組織設定。

**支援交付完成的指導方針：**

- 如需設定和執行的指引，請參閱 [建立初始雲端治理基礎](../../govern/initial-foundation.md)。

**責任小組：**

- 雲端治理小組負責審查和實行治理最佳作法和初始治理基礎。

## <a name="step-7-continuously-improve-governance-maturity"></a>步驟7：持續改善治理成熟度

當額外的雲端採用工作完成時，治理需求會隨之增加。 持續保持一致的採用方案，以確保治理方法可以維持適當的治理與控制層級。

**交付：**

- 實行治理改進，以防止變更風險和治理需求。

**支援交付完成的指導方針：**

- 為了協助改善初始治理基礎，請執行 [擴充的治理案例](../../govern/foundation-improvements.md)。

**責任小組：**

- 雲端治理小組會負責配合持續採用計畫。

## <a name="step-8-evaluate-landing-zone-changes"></a>步驟8：評估登陸區域變更

當登陸區域已部署和展開時，可能會出現新的風險或治理違規。 定期審核登陸區域設定，以識別與雲端原生治理工具未攔截到的原則之間的任何偏差。 確定每個登陸區域部署都遵循登陸區域治理的指導方針。

**交付：**

- 協助雲端平臺小組針對登陸區域（必須符合治理原則）進行改進。

**支援交付完成的指導方針：**

- 改進 [登陸區域治理](../../ready/considerations/landing-zone-governance.md)。

**責任小組：**

- 雲端治理小組應該確定每個登陸區域部署都遵守治理指導方針。

## <a name="step-9-adoption-handoffs"></a>步驟9：採用遞交

當新的採用成果完成時，雲端採用小組會將營運責任交給雲端營運小組和雲端治理小組。 保持與採用版本步調一致，以確保適當的檔和原則一致，並協助小組承擔工作負載的責任。

**交付：**

- 定期審核並接受其他雲端採用小組的遞交。

**支援交付完成的指導方針：**

- 建立將新的 [工作負載和資源](/azure/azure-resource-manager/custom-providers/concepts-resource-onboarding)上線的流程。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端治理小組 <li> 雲端營運小組 |

## <a name="whats-next"></a>後續步驟

所有公司都是獨一無二的，因此其治理需求。 選擇適合您組織的成熟度等級，並使用「雲端採用架構」來引導可協助您達到此規範的作法、程式和工具。

隨著雲端治理的成熟，小組能夠以更快的步調採用雲端。 持續的雲端採用工作通常會觸發 IT 營運中的成熟度。 為了確保治理是作業開發的一部分，請開發 [雲端營運小組](./cloud-operations.md) ，或與您現有的雲端營運團隊進行同步處理。
