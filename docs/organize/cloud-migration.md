---
title: 瞭解雲端遷移功能
description: 瞭解雲端遷移功能。
author: JanetCThomas
ms.author: janet
ms.service: cloud-adoption-framework
ms.subservice: organize
ms.topic: conceptual
ms.date: 05/15/2020
ms.openlocfilehash: a5b3caa04e3f87c30236425aff770e82e0a37b4c
ms.sourcegitcommit: 9a84c2dfa4c3859fd7d5b1e06bbb8549ff6967fa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2020
ms.locfileid: "83755575"
---
# <a name="cloud-migration-functions"></a>雲端遷移功能

雲端遷移小組是技術執行小組或專案小組的現代化日。 但是雲端的本質可能需要更流暢的小組結構。 某些遷移小組僅著重于雲端遷移，而其他則著重于利用雲端技術的創新。 其中一些包括完成大型採用工作所需的廣泛技術專業知識，例如完整的資料中心遷移，其他人則有更緊密的技術焦點，而且可能會在專案之間移動以達成特定目標，例如，協助將 SQL Vm 轉換成 SQL PaaS 實例的資料平臺專家小組。

無論雲端遷移小組的類型或數目為何，這些小組通常會為 IT、商務分析或實施合作夥伴提供主題專長。

## <a name="prerequisites"></a>Prerequisites

- [建立 azure 帳戶](https://docs.microsoft.com/learn/modules/create-an-azure-account)：使用 azure 的第一個步驟是建立帳戶。
- [Azure 入口網站](https://docs.microsoft.com/learn/modules/tour-azure-portal)：流覽 Azure 入口網站功能和服務，並自訂入口網站。
- [Azure 簡介](https://docs.microsoft.com/learn/modules/welcome-to-azure)：開始使用 azure。 在雲端中建立並設定您的第一部虛擬機器。
- [Azure 基礎](https://docs.microsoft.com/learn/paths/azure-for-the-data-engineer)：瞭解雲端概念、瞭解優點、比較和對比基本策略，以及探索 Azure 中可用的服務廣度。
- 請參閱[遷移方法](../migrate/index.md)。

## <a name="minimum-scope"></a>最小範圍

所有雲端採用工作的系統核心是雲端遷移小組。 這個小組會驅動可實現採用的技術變更。 根據採用工作的目標，這個團隊可能會包含各種小組成員，負責處理一組廣泛的技術和商務任務。

小組範圍至少包含：

- [數位資產的合理化](../digital-estate/index.md)
- [排定優先順序的遷移待](../migrate/migration-considerations/assess/release-iteration-backlog.md)處理專案的審查、驗證和進展
- 以學習機會的形式執行[第一個工作負載](../digital-estate/rationalize.md#select-the-first-workload)。

## <a name="deliverable"></a>交付項目

來自任何雲端遷移小組的主要交付專案，是採用計畫中所述之技術解決方案的及時、高品質的執行，並使用可用的技術、工具和自動化解決方案來配合治理需求和商務成果。

### <a name="ongoing-monthly-tasks"></a>進行中的每月工作

- 監督[變更管理流程](../migrate/migration-considerations/prerequisites/technical-complexity.md)。
- 管理[發行和短期衝刺](../migrate/migration-considerations/assess/release-iteration-backlog.md)待處理專案
- 建立並維護採用登陸區域，並配合治理需求。
- 完成短期衝刺待處理專案[（backlog）](../migrate/migration-considerations/assess/release-iteration-backlog.md)中所述的技術工作。

### <a name="team-cadence"></a>小組步調

我們建議提供雲端採用功能的小組必須專注于全職。

如果這些小組以自我組織方式來每日都符合，這就是最佳選擇。 每日會議的目標是要快速更新待處理專案（backlog），並傳達已完成的專案、今天要做什麼，以及封鎖哪些事項，需要額外的外部支援。

發行排程和反復專案持續時間對每一家公司都是唯一的。 但是，每個反復專案1到4周的範圍似乎是平均持續時間。 無論反復專案或發行步調為何，我們建議小組在每次發行結束時都符合所有支援小組，以傳達發行的結果，並重新調整即將進行的工作。 在每個短期衝刺結束時，您也可以將其視為小組，讓卓越或[雲端治理小組](./cloud-governance.md)的[雲端中心](../organize/cloud-center-of-excellence.md)保持一致，以符合常見的工作和支援的任何需求。

某些與雲端採用相關聯的技術工作可能會重複。 小組成員應每 3 6 個月輪替一次， &ndash; 以避免員工滿意度問題並維持相關技能。 卓越或[雲端治理小組](./cloud-governance.md)[雲端中心](../organize/cloud-center-of-excellence.md)的輪替基座可提供絕佳的機會，讓員工保持最新的創新，並控管新的革新。

## <a name="baseline-capability"></a>基準功能

視所需的商業結果而定，提供完整雲端採用功能所需的技能可能包括：

- 基礎結構實施者
- DevOps 工程師
- 應用程式開發人員
- 資料科學家
- 資料或應用程式平臺專家

為了達到最佳共同作業和效率，我們建議雲端採用小組的平均小組大小為六個人。 這些小組應該從技術執行的觀點來自我組織。 我們強烈建議這些小組也包括專案管理專業知識，並具有 agile、scrum 或其他反復模型的深度體驗。 這個小組在使用一般結構進行管理時最有效。

## <a name="out-of-scope"></a>超出範圍

可能需要來自現有 IT 人員的其他支援。 這對於雲端採用有價值的貢獻，是雲端代理人和合作夥伴的創新和企業靈活性。

- [中央 IT 責任](../organize/central-it.md)

## <a name="whats-next"></a>後續步驟

採用很棒，但 ungoverned 採用可能會產生非預期的結果。 與[雲端治理小組](./cloud-governance.md)保持一致，以加速採用和最佳作法，同時降低業務和技術風險。

這兩個小組會在雲端採用的工作之間建立平衡，但會被視為 MVP，因為它可能不是可持續的。 每個小組都會戴上許多的帽子，如責任、參與、 [*諮詢、通知*（RACI）圖表](../organize/raci-alignment.md)中所述。

深入瞭解[組織反模式：定址接收器和 fiefdoms](../organize/fiefdoms-silos.md)。
