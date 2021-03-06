---
title: 瞭解雲端遷移功能
description: 瞭解雲端遷移功能。
author: JanetCThomas
ms.author: janet
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: organize
ms.custom: internal
ms.openlocfilehash: a34c7c9fd717ad8959f2e12df6c5b30b2d3710f7
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101785354"
---
# <a name="cloud-migration-functions"></a>雲端遷移功能

雲端遷移團隊是技術實行小組或專案團隊的現代化日對等專案。 但雲端的本質可能需要更流暢的團隊結構。 某些遷移小組專門專注于雲端遷移，而其他則著重于利用雲端技術的創新。 其中有些包括完成大量採用工作所需的廣泛技術專業知識，例如完整的資料中心遷移，而其他人則有更緊密的技術焦點，而且可能會在專案之間移動以達成特定目標，例如，可協助將 SQL Vm 轉換成 SQL PaaS 實例的資料平臺專家團隊。

無論雲端遷移團隊的類型或數目為何，這些團隊通常會為 IT、商務分析或實施合作夥伴提供主題專業知識。

## <a name="prerequisites"></a>必要條件

- [建立 azure 帳戶](/learn/modules/create-an-azure-account/)：使用 azure 的第一個步驟是建立帳戶。
- [Azure 入口網站](/learn/modules/tour-azure-portal/)：導覽 azure 入口網站的功能和服務，以及自訂入口網站。
- [Azure 簡介](/learn/modules/intro-to-azure-fundamentals/)：開始使用 azure。 在雲端中建立並設定第一部虛擬機器。
- [Azure 基本](/learn/paths/azure-for-the-data-engineer/)概念：瞭解雲端概念、瞭解優點、比較和對比基本策略，以及探索 Azure 中可用的服務廣度。
- 請參閱 [遷移方法](../migrate/index.md)。

## <a name="minimum-scope"></a>最小範圍

所有雲端採用工作的系統核心都是雲端遷移小組。 此小組會推動可採用的技術變更。 根據採用工作的目標，這個小組可能會包含各種不同範圍的團隊成員，這些成員會處理一組廣泛的技術和商務工作。

小組範圍至少包含：

- [數位資產的合理化](../digital-estate/index.md)
- [排定優先順序的遷移待](../migrate/migration-considerations/assess/release-iteration-backlog.md)處理專案的檢查、驗證和進展
- 以學習機會的形式執行 [第一個工作負載](../digital-estate/rationalize.md#select-the-first-workload) 。

## <a name="deliverable"></a>交付項目

任何雲端遷移團隊的主要交付專案，都是在採用計畫中，使用可用的技術、工具和自動化解決方案，及時、高品質的採用計畫中所述的技術解決方案，以配合治理需求和業務成果。

### <a name="ongoing-monthly-tasks"></a>每月進行中的工作

- 監督 [變更管理流程](../migrate/migration-considerations/prerequisites/technical-complexity.md)。
- 管理[發行和短期衝刺](../migrate/migration-considerations/assess/release-iteration-backlog.md)待處理專案
- 將採用登陸區域與治理需求一起建立和維護。
- 完成短期衝刺待處理專案 [（backlog）](../migrate/migration-considerations/assess/release-iteration-backlog.md)中所述的技術工作。

### <a name="team-cadence"></a>小組步調

我們建議提供雲端採用功能的團隊必須致力於投入時間。

最好是這些小組以自我組織的方式每日符合。 每日會議的目標是要快速更新待處理專案，並傳達已完成的工作、今天要做什麼，以及封鎖的專案，需要額外的外部支援。

每一家公司都有唯一的發行排程和反復專案持續時間。 但每個反復專案的一到四周範圍似乎是平均持續時間。 無論反復專案或發行步調為何，我們建議團隊在每個版本結束時都符合所有支援小組，以傳達發行的結果，並重新調整即將進行的工作。 您也可以在每個短期衝刺結束時以小組的形式達成，並以卓越或[雲端治理小組](./cloud-governance.md)的[雲端中心](../organize/cloud-center-of-excellence.md)來保持一致，以掌握共同工作和支援的任何需求。

某些與雲端採用相關聯的技術工作可能會重複。 小組成員應該每隔 3 &ndash; 個月輪替一次，以避免員工滿意度問題並維持相關的技能。 [雲端中心卓越](../organize/cloud-center-of-excellence.md)或[雲端治理小組](./cloud-governance.md)的旋轉基座可提供絕佳的機會，讓員工保持最新並掌控新的創新。

## <a name="baseline-capability"></a>基準功能

根據所需的業務成果，提供完整雲端採用功能所需的技能可能包括：

- 基礎結構實施者
- DevOps 工程師
- 應用程式開發人員
- 資料科學家
- 資料或應用程式平臺專家

為了獲得最佳的共同作業和效率，我們建議雲端採用小組的平均小組人數為六人。 這些團隊應該從技術執行的觀點來自我組織。 我們強烈建議這些小組也包括專案管理專長，以及敏捷式、Scrum 或其他反復模型的深度體驗。 此小組在使用一般結構管理時最有效。

## <a name="out-of-scope"></a>超出範圍

可能需要來自現有 IT 人員的其他支援。 這可能是雲端採用的重要參與者，其成為雲端代理人和創新和企業彈性的合作夥伴。

- [中央 IT 團隊責任](../organize/central-it.md)

## <a name="whats-next"></a>後續步驟

採用很棒，但 ungoverned 採用可能會產生非預期的結果。 與 [雲端治理小組](./cloud-governance.md) 保持一致，以加速採用和最佳作法，同時降低商務和技術風險。

這兩個小組會在雲端採用工作之間建立平衡，但視為 MVP，因為它可能不是持續性。 每個小組都有許多職，如「 [*負責任」、「負責任* 」、「諮詢」 (RACI) 圖表](../organize/raci-alignment.md)中所述。

深入瞭解 [組織反模式：定址接收器和領域](../organize/fiefdoms-silos.md)。
