---
title: 反復專案和發行待處理專案
description: 使用適用于 Azure 的雲端採用架構，瞭解如何建立反復專案和發行待處理專案來組織您的工作。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: internal
ms.openlocfilehash: aa0c464d187446d5c9d7baf249499cec278d042a
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101785439"
---
# <a name="manage-change-in-an-incremental-migration-effort"></a>管理累加移轉工作中的變更

本文假設移轉程序在本質上是累加的，且會與[控管程序](../../../govern/index.md)平行執行。 不過，您可以使用相同的指引，在傳統瀑布式變更管理方法的工作分工結構中導入初始作業。

## <a name="release-backlog"></a>發行待處理項目

*發行待處理項目* 由一系列的資產 (VM、資料庫、檔案和應用程式等) 所組成，必須先移轉這些資產後，才能發行工作負載以供雲端中的生產環境使用。 在每個反覆項目的執行期間，雲端採用小組都會記錄並預估將每個資產移至雲端所需的工作。 請參閱下面的「反復專案待辦專案」一節。

## <a name="iteration-backlog"></a>反覆項目待處理項目

*反覆項目待處理項目* 是將特定數量的資產從現有的數位資產移轉至雲端所需的詳細工作清單。 這份清單上的項目通常會儲存在敏捷式管理工具 (例如 Azure DevOps) 中，作為工作項目。

在開始執行第一個反覆項目之前，雲端採用小組會指定反覆項目的持續時間，通常為兩到四週。 要為每一組已認可的活動建立開始和完成時段，必須要有此時間範圍。 維護一致的執行時間範圍，可讓您輕鬆地測量速度 (移轉的步調) 並根據持續變化的業務需求進行調整。

在執行每個反覆項目之前，小組會檢閱發行待處理項目，對要移轉的資產估計工作量和優先順序。 然後，他們會認可以提供特定數量經同意的移轉。 在雲端採用小組同意這項作業之後，活動清單就會變成目前的 *反復專案待辦* 專案。

在每個反覆項目的執行期間，小組成員在工作時會形成自我組織的小組，以在目前的反覆項目待處理項目中履行承諾。

## <a name="next-steps"></a>下一步

在雲端採用小組定義並接受反復專案待辦專案之後，即可完成 [變更管理核准](./approve.md) 。

> [!div class="nextstepaction"]
> [在移轉前核准架構變更](./approve.md)
