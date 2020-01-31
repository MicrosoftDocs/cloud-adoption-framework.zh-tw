---
title: 了解移轉期間的預備活動
description: 了解移轉期間的預備活動
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: c8dcc71cd47253bbc59e885a085802d78323c5fd
ms.sourcegitcommit: 2362fb3154a91aa421224ffdb2cc632d982b129b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/28/2020
ms.locfileid: "76801982"
---
# <a name="understand-staging-activities-during-a-migration"></a>了解移轉期間的預備活動

如升級模型文章所述，「預備」是資產遷移至雲端的時間點。 不過，這些資產尚未準備好升階至生產環境。 這通常是移轉的遷移程序最終步驟。 在預備過後，工作負載便會由 IT 營運或雲端營運小組管理，以便讓其準備好用於生產環境。

## <a name="deliverables"></a>交付項目

預備資產可能尚未準備好用於生產環境。 必須先完成數個生產環境整備檢查後，我們才會認為此階段已完成。 以下是通常與資產預備完成相關聯的交付項目清單。

- **自動化的測試。** 所有可用於驗證工作負載效能的自動化測試都應該先執行，才能結束預備程序。 資產離開預備程序後，與原始來源系統的同步處理便會終止。 因此，在資產進入預備狀態以便進行最佳化之後，就更難以重新部署所複寫的資產。
- **移轉文件。** 大部分移轉工具都可以針對所要遷移的資產產生自動化報告。 為了清楚起見，請在結束預備活動之前，記下所有遷移後的資產。
- **設定文件。** 對資產所進行的任何變更 (在修復、複寫或預備期間) 都應該記載下來以供完成營運整備。
- **待辦項目文件。** 請更新移轉待辦項目以反映處於預備狀態的工作負載和資產。

## <a name="next-steps"></a>後續步驟

在測試並記載預備資產後，您便可以繼續進行[最佳化活動](../optimize/index.md)。

> [!div class="nextstepaction"]
> [將遷移後的工作負載最佳化](../optimize/index.md)
