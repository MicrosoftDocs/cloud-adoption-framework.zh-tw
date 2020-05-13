---
title: 瞭解雲端採用功能
description: 瞭解雲端採用功能如何啟用技術解決方案，讓您可以適當地為小組提供人員。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/20/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: organize
ms.custom: organize
ms.openlocfilehash: ac8307d3af159f3d6d35ccf2ded55f77786a33d7
ms.sourcegitcommit: 60d8b863d431b5d7c005f2f14488620b6c4c49be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83215869"
---
# <a name="cloud-adoption-functions"></a>雲端採用功能

雲端採用功能可讓您在雲端中實現技術解決方案。 就像任何 IT 專案一樣，傳遞實際工作的人員也會決定是否成功。 提供必要「雲端採用」功能的小組可以從多個主題專家或執行合作夥伴學習。

雲端採用小組是技術執行小組或專案小組的現代化日。 不過，雲端的本質可能需要更流暢的小組結構。 有些小組專門專注于雲端遷移，而其他小組則著重于利用雲端技術的創新。 有些小組包括完成大型採用工作所需的廣泛技術專業知識，例如完整的資料中心遷移。 其他小組有更緊密的技術焦點，而且可以在專案之間移動以達成特定目標。 其中一個範例是一組資料平臺專家，可協助您將 SQL Vm 轉換成 SQL PaaS 實例。

無論雲端採用小組的類型或數目為何，雲端採用所需的功能都是由 IT、商務分析或實夥伴中的主題專家所提供。

視所需的商業結果而定，提供完整雲端採用功能所需的技能可能包括：

- 基礎結構實施者
- DevOps 工程師
- 應用程式開發人員
- 資料科學家
- 資料或應用程式平臺專家

為了達到最佳共同作業和效率，我們建議雲端採用小組的平均小組大小為六個人。 這些小組應該從技術執行的觀點來自我組織。 我們強烈建議這些小組也包括專案管理專業知識，並具有 agile、scrum 或其他反復模型的深度體驗。 這個小組在使用一般結構進行管理時最有效。

## <a name="preparation"></a>準備

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

任何雲端採用功能的主要需求，都是採用計畫中所述之技術解決方案的及時、高品質的執行。 這些解決方案應該符合治理需求和商務成果，而且應該利用小組提供的技術、工具和自動化解決方案。

**初期規劃工作：**

- 執行[數位資產的合理化](../digital-estate/index.md)。
- 審查、驗證和推進[排定優先順序的遷移待辦](../migrate/migration-considerations/assess/release-iteration-backlog.md)專案。
- 開始執行[第一個工作負載](../digital-estate/rationalize.md#select-the-first-workload)作為學習機會。

**進行中的每月工作：**

- 監督[變更管理流程](../migrate/migration-considerations/prerequisites/technical-complexity.md)。
- 管理[發行和短期衝刺](../migrate/migration-considerations/assess/release-iteration-backlog.md)待處理專案。
- 建立並維護採用登陸區域，並配合治理需求。
- 執行短期衝刺待處理專案[（backlog）](../migrate/migration-considerations/assess/release-iteration-backlog.md)中所述的技術工作。

**會議步調：**

我們建議提供雲端採用功能的小組必須專注于全職。

如果這些小組以自我組織方式來每日都符合，這就是最佳選擇。 每日會議的目標是要快速更新待處理專案（backlog），並傳達已完成的專案、今天要做什麼，以及封鎖哪些事項，需要額外的外部支援。

發行排程和反復專案持續時間對每一家公司都是唯一的。 不過，每個反復專案1到4周的範圍似乎是平均持續時間。 無論反復專案或發行步調為何，我們建議小組在每次發行結束時都符合所有支援小組，以傳達發行的結果，並重新調整即將進行的工作。 同樣地，您也可以在每個短期衝刺結束時以小組的身分來達成，而卓越或雲端治理小組的雲端中心可以保持一致，以符合常見的工作和支援的任何需求。

某些與雲端採用相關聯的技術工作可能會重複。 小組成員應每 3 6 個月輪替一次， &ndash; 以避免員工滿意度問題並維持相關技能。 卓越或雲端治理小組雲端中心的輪替基座可提供絕佳的機會，讓員工保持最新的創新能力。

深入瞭解卓越或[雲端治理小組](./cloud-governance.md)[雲端中心](./cloud-center-of-excellence.md)的功能。

## <a name="next-steps"></a>後續步驟

- [建立雲端採用小組](../get-started/team/cloud-adoption.md)
- 配合雲端[治理功能](./cloud-governance.md)，以加速採用並實行最佳作法，同時降低企業和技術風險。
