---
title: 瞭解雲端採用功能
description: 瞭解雲端採用功能如何啟用技術解決方案，讓您能夠適當地為小組工作。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: organize
ms.custom: organize
ms.openlocfilehash: 7afed36939d3d143934ad962aa87e3921535ed06
ms.sourcegitcommit: 57b757759b676a22f13311640b8856557df36581
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2020
ms.locfileid: "94996531"
---
# <a name="cloud-adoption-functions"></a>雲端採用功能

雲端採用功能可讓您在雲端中執行技術解決方案。 就像任何 IT 專案一樣，提供實際工作的人員會判斷是否成功。 提供必要雲端採用功能的團隊可以從多個主題專家或實施合作夥伴來學習。

雲端採用小組是技術實行小組或專案團隊的現代化日對等專案。 但雲端的本質可能需要更流暢的團隊結構。 有些小組專門專注于雲端遷移，而其他小組則專注于利用雲端技術的創新。 有些小組包含完成大量採用工作所需的廣泛技術專長，例如完整的資料中心遷移。 其他團隊具有更緊密的技術焦點，並可在專案之間移動以達成特定目標。 其中一個範例是資料平臺專家的團隊，可協助將 SQL Vm 轉換成 SQL PaaS 實例。

無論雲端採用小組的類型或數目為何，雲端採用所需的功能是由 IT、商務分析或實施合作夥伴的主題專家所提供。

根據所需的業務成果，提供完整雲端採用功能所需的技能可能包括：

- 基礎結構實施者
- DevOps 工程師
- 應用程式開發人員
- 資料科學家
- 資料或應用程式平臺專家

為了獲得最佳的共同作業和效率，我們建議雲端採用小組的平均小組人數為六人。 這些團隊應該從技術執行的觀點來自我組織。 我們強烈建議這些小組也包括專案管理專長，以及敏捷式、Scrum 或其他反復模型的深度體驗。 此小組在使用一般結構管理時最有效。

## <a name="preparation"></a>準備

- [建立 azure 帳戶](/learn/modules/create-an-azure-account)：使用 azure 的第一個步驟是建立帳戶。
- [Azure 入口網站](/learn/modules/tour-azure-portal)：導覽 Azure 入口網站功能和服務，以及自訂入口網站。
- [Azure 簡介](/learn/modules/welcome-to-azure)：開始使用 azure。 在雲端中建立並設定第一部虛擬機器。
- [Azure 基本](/learn/paths/azure-for-the-data-engineer)概念：瞭解雲端概念、瞭解優點、比較和對比基本策略，以及探索 Azure 中可用的服務廣度。
- 請參閱 [遷移方法](../migrate/index.md)。

## <a name="minimum-scope"></a>最小範圍

所有雲端採用工作的系統核心都是雲端遷移小組。 此小組會推動可採用的技術變更。 根據採用工作的目標，這個小組可能會包含各種不同範圍的團隊成員，這些成員會處理一組廣泛的技術和商務工作。

小組範圍至少包含：

- [數位資產的合理化](../digital-estate/index.md)
- [排定優先順序的遷移待](../migrate/migration-considerations/assess/release-iteration-backlog.md)處理專案的檢查、驗證和進展
- 以學習機會的形式執行 [第一個工作負載](../digital-estate/rationalize.md#select-the-first-workload) 。

## <a name="deliverable"></a>交付項目

任何雲端採用函式的主要需求，是採用計畫中所述之技術解決方案的及時、高品質的實作為。 這些解決方案應該符合治理需求和業務成果，而且應該利用可供小組使用的技術、工具和自動化解決方案。

**早期規劃工作：**

- 執行 [數位資產的合理化](../digital-estate/index.md)。
- 檢查、驗證和預先設定 [優先的遷移待辦](../migrate/migration-considerations/assess/release-iteration-backlog.md)專案。
- 開始以學習機會執行 [第一個工作負載](../digital-estate/rationalize.md#select-the-first-workload) 。

**每月進行中的工作：**

- 監督 [變更管理流程](../migrate/migration-considerations/prerequisites/technical-complexity.md)。
- 管理發行和短期衝刺待處理專案 [（backlog）](../migrate/migration-considerations/assess/release-iteration-backlog.md)。
- 將採用登陸區域與治理需求一起建立和維護。
- 執行短期衝刺待處理專案 [（backlog）](../migrate/migration-considerations/assess/release-iteration-backlog.md)中所述的技術工作。

**會議步調：**

我們建議提供雲端採用功能的團隊必須致力於投入時間。

最好是這些小組以自我組織的方式每日符合。 每日會議的目標是要快速更新待處理專案，並傳達已完成的工作、今天要做什麼，以及封鎖的專案，需要額外的外部支援。

每一家公司都有唯一的發行排程和反復專案持續時間。 但每個反復專案的一到四周範圍似乎是平均持續時間。 無論反復專案或發行步調為何，我們建議團隊在每個版本結束時都符合所有支援小組，以傳達發行的結果，並重新調整即將進行的工作。 同樣地，您也可以在每個短期衝刺結束時以小組的形式來達成，並具備卓越或雲端治理小組的雲端中心，以保持一致的共同工作和支援需求。

某些與雲端採用相關聯的技術工作可能會重複。 小組成員應該每隔 3 &ndash; 個月輪替一次，以避免員工滿意度問題並維持相關的技能。 雲端中心卓越或雲端治理小組的旋轉基座可提供絕佳的機會，讓員工保持最新並掌控新的創新。

深入瞭解卓越或[雲端治理小組](./cloud-governance.md)的[雲端中心](./cloud-center-of-excellence.md)功能。

## <a name="next-steps"></a>下一步

- [建置雲端採用小組](../get-started/team/cloud-adoption.md)
- 利用 [雲端治理功能](./cloud-governance.md) 讓雲端採用工作保持一致，以加速採用和實行最佳作法，同時降低商務和技術風險。
