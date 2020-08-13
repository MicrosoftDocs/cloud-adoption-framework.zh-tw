---
title: 移轉期間的預備活動
description: 使用適用于 Azure 的雲端採用架構，瞭解遷移期間所需的預備活動和相關聯的交付專案。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 72f793383bd525b48f3f8ae30cd6e0d470ddba64
ms.sourcegitcommit: 949b87bad28d32df84df190160089f01826f3a31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2020
ms.locfileid: "88193294"
---
# <a name="understand-staging-activities-during-a-migration"></a>了解移轉期間的預備活動

如升級模型文章所述，「預備」__ 是資產遷移至雲端的時間點。 不過，他們尚未準備好升級至生產環境。 這通常是移轉的遷移程序最終步驟。 在預備過後，工作負載便會由 IT 營運或雲端營運小組管理，以便讓其準備好用於生產環境。

## <a name="deliverables"></a>交付項目

預備資產可能尚未準備好用於生產環境。 必須先完成數個生產環境整備檢查後，我們才會認為此階段已完成。 以下是通常與資產預備完成相關聯的交付項目清單。

- **自動化測試。** 所有可用於驗證工作負載效能的自動化測試都應該先執行，才能結束預備程序。 資產離開預備程序後，與原始來源系統的同步處理便會終止。 因此，在資產進入預備狀態以便進行最佳化之後，就更難以重新部署所複寫的資產。
- **遷移檔。** 大部分移轉工具都可以針對所要遷移的資產產生自動化報告。 為了清楚起見，請在結束預備活動之前，記下所有遷移後的資產。
- **設定文件。** 對資產所進行的任何變更 (在修復、複寫或預備期間) 都應該記載下來以供完成營運整備。
- **待辦項目文件。** 請更新移轉待辦項目以反映處於預備狀態的工作負載和資產。

## <a name="next-steps"></a>後續步驟

在測試並記載預備資產後，您便可以繼續進行最佳化活動。

> [!div class="nextstepaction"]
> [將已移轉的工作負載最佳化](../optimize/index.md)
