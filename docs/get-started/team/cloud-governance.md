---
title: 開始使用：組建雲端治理小組
description: 建立治理小組的範圍、交付專案和功能，以準備成功的雲端治理。
author: JanetCThomas
ms.author: janet
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.topic: conceptual
ms.date: 05/15/2020
ms.openlocfilehash: ff5b2dba1cdb95cdcc007ba5cea52b86a902584d
ms.sourcegitcommit: 84d7bfd11329eb4c151c4c32be5bab6c91f376ed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2020
ms.locfileid: "86233319"
---
# <a name="get-started-build-a-cloud-governance-team"></a>開始使用：組建雲端治理小組

雲端治理小組可確保已正確評估和管理雲端採用風險和風險承受度。 小組會找出企業無法容忍的風險，並將風險轉換成治理公司原則。

![開始建立雲端治理小組](../../_images/get-started/governance-team-map.png)

## <a name="step-1-determine-whether-a-cloud-governance-team-is-needed"></a>步驟1：判斷是否需要雲端治理小組

Azure 雲端採用架構中的官方指引是一律建立雲端治理小組。 一開始，小組可能非常小。 無論其大小為何，其角色都非常重要。 如果不需要小組，現有採用小組的群組或個人應同意符合[雲端治理功能](../../organize/cloud-governance.md)的相關責任。

**項**

- 判斷您是否需要雲端治理小組。
- 藉由開發跨團隊的矩陣，識別負責、參與、_諮詢和通知的 (RACI) _方，來協調小組間的責任。 使用工作表中的[RACI 範本](https://archcenter.blob.core.windows.net/cdn/fusion/management/raci-template.xlsx)，記錄決策和負責的人員 `Org Alignment` 。

**支援交付後完成的指引：**

- [雲端治理功能](../../organize/cloud-governance.md)可能已散佈到多個個人或小組。 擁有「雲端治理小組」標題的小組並不重要，但所需的功能應該與負責的合作物件或小組一起運作。
- 如果公司的長期雲端採用策略可以從一個雲端環境中的一個登陸區域傳遞，則治理和營運作業的量可能會夠小，可供一個人或一個小組傳遞。 該小組不太可能被稱為雲端治理，因為它在雲端治理之外提供了許多功能。 即使是針對該小組，此入門指南可協助確保 it 能夠在治理方面提供這項重要的功能。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端策略小組 | <li> 雲端採用小組 |

## <a name="step-2-align-with-other-teams"></a>步驟2：與其他小組對齊

治理小組可確保一致性，並遵循一組常見的原則。 這些原則來自于與其他小組進行中的一致。

雲端治理小組必須符合 RACI 範本中所識別的其他小組，才能建立原則或自動化雲端治理。 這將有助於確保重要主題的對齊，例如安全性、成本、效能、作業和部署。 步驟4和5可協助您進行對齊。

**項**

- 討論每個團隊的目前狀態實行和持續採用計畫。

**支援交付後完成的指引：**

- 使用雲端策略小組的成員，檢查您公司的[策略和計畫範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)，以瞭解動機、計量和策略。
- 使用雲端採用小組的成員來檢查您公司的[雲端採用方案範本](../../plan/template.md)，以瞭解時程表和優先順序。
- 請參閱作業小組的[operations management 活頁簿](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx)，以瞭解已與企業建立的營運需求和承諾。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端治理小組 | <li> 雲端策略小組 <li> 雲端採用小組 <li> 雲端營運小組 <li> 卓越或中央 IT 小組的雲端中心 |

## <a name="step-3-establish-a-cadence-with-other-teams"></a>步驟3：建立與其他小組的步調

雲端採用通常是以波浪或版本為准。 與這些版本一致的一般步調可讓雲端治理小組仔細研究，並瞭解將在下一波推出的風險。 在規劃與審查期間，與策略、採用和營運小組保持聯繫，也有助於治理小組繼續承擔風險。

**項**

- 建立支援小組的步調。 可能的話，請將該步調與發行和規劃週期保持一致。
- 直接與雲端策略小組 (或各種小組成員) 檢查與下一波採用相關的風險，並測量小組對於這些風險的承受度。

**支援交付後完成的指引：**

- 如需步調會議的詳細指引，請參閱[雲端治理功能](../../organize/cloud-governance.md#deliverable)。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端治理小組 | <li> 雲端策略小組 <li> 雲端採用小組 <li> 雲端營運小組 |

## <a name="step-4-review-the-methodology"></a>步驟4：檢查方法

若要協助建立治理的未來願景和該願景的可行方法，請參閱雲端採用架構中的管理方法。

**項**

- 瞭解支援管理方法的方法、方法和實體系。

**支援交付後完成的指引：**

- 請參閱[管理方法](../../govern/methodology.md)。

**責任小組：**

- 雲端治理小組負責建立治理的願景和方法。

## <a name="step-5-complete-the-governance-benchmark"></a>步驟5：完成治理基準測試

治理是廣泛的主題。 簡短的評估可協助小組瞭解開始著手的位置。

**項**

- 根據與各種專案關係人的交談，完成治理基準評估。 或要求其他小組自行完成評估。

**支援交付後完成的指引：**

- 使用[治理基準](../../govern/benchmark.md)來評估您的治理需求和優先順序。

**責任小組：**

- 雲端治理小組應該瞭解治理基準測試所識別的差距，然後提供有助於解決缺口的治理方向。

## <a name="step-6-implement-the-initial-governance-best-practice-and-configuration"></a>步驟6：執行初始治理最佳做法和設定

管控方法包含兩種初始治理基礎的方法。 請檢查每個方法，並執行最符合您需求的方法。

**項**

- 部署在接下來的幾波採用工作期間，管理環境所需的基本治理工具和組織設定。

**支援交付後完成的指引：**

- 如需設定和執行的指引，請參閱[建立初始雲端治理基礎](../../govern/initial-foundation.md)。

**責任小組：**

- 雲端治理小組負責審查和實施治理最佳做法和初始治理基礎。

## <a name="step-7-continuously-improve-governance-maturity"></a>步驟7：持續改善治理成熟度

治理需求會隨著額外的雲端採用工作而成長。 保持一致，以確保治理方法可以維護適當的治理與控制層級。

**項**

- 執行治理改良功能，以防範不斷改變的風險和治理需求。

**支援交付後完成的指引：**

- 為了協助改善最初的治理基礎，請執行[擴充的治理案例](../../govern/foundation-improvements.md)。

**責任小組：**

- 雲端治理小組負責配合持續進行的採用方案。

## <a name="step-8-evaluate-landing-zone-changes"></a>步驟8：評估登陸區域變更

當登陸區域已部署並擴充時，可能會出現新的風險或治理違規。 定期檢查登陸區域設定，以識別與雲端原生治理工具未攔截到的原則有何偏差。 請確定每個登陸區域部署都遵守登陸區域治理的指導方針。

**項**

- 協助雲端平臺小組針對登陸區域開發改良功能，這必須符合治理原則。

**支援交付後完成的指引：**

- 改善[登陸區域治理](../../ready/considerations/landing-zone-governance.md)。

**責任小組：**

- 雲端治理小組應確保每個登陸區域部署都遵守治理方針。

## <a name="step-9-adoption-handoffs"></a>步驟9：採用遞交

當新的採用成果完成時，雲端採用小組會將營運責任交給雲端營運小組和雲端治理小組。 與採用 release 步調保持一致，以確保適當的檔和原則的一致，並協助小組承擔工作負載的責任。

**項**

- 定期審查並接受其他雲端採用小組的遞交。

**支援交付後完成的指引：**

- 建立將新的[工作負載和資源](https://docs.microsoft.com/azure/azure-resource-manager/custom-providers/concepts-resource-onboarding)上線的程式。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端治理小組 <li> 雲端營運小組 |

## <a name="whats-next"></a>後續步驟

所有公司都是獨一無二的，因此是他們的治理需求。 選擇適合您組織的成熟度層級，並使用雲端採用架構來引導可協助您取得的做法、程式和工具。

隨著雲端治理的成熟，小組能夠以更快的步調採用雲端。 持續的雲端採用工作往往會觸發 IT 營運的成熟度。 若要確保治理是作業開發的一部分，請開發[雲端作業小組](./cloud-operations.md)，或與您現有的雲端營運小組同步。
