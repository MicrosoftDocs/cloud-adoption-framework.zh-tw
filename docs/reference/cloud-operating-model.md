---
title: 雲端操作模式現在是 Azure 的雲端採用架構
description: 使用適用于 Azure 的雲端採用架構，以瞭解如何加速雲端採用的功能、原因及方式。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/12/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: reference
ms.custom: governance
ms.openlocfilehash: 2ef4d4c61ee573aa1233db5dcdbed427a3b5b100
ms.sourcegitcommit: 5d6a7610e556f7b8ca69960ba76a3adfa9203ded
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2020
ms.locfileid: "83401196"
---
<!-- docsTest:ignore "Cloud Operating Model" -->

# <a name="cloud-operating-model-is-now-part-of-the-microsoft-cloud-adoption-framework-for-azure"></a>雲端作業模式現在是 Azure Microsoft Cloud 採用架構的一部分

在 2018 年初，Microsoft 發行了雲端作業模型 (COM)。 COM 是一份指南，協助客戶瞭解數位轉型的**用途**和**原因**。 這可協助客戶瞭解所有需要解決的領域：商務策略、文化策略和技術策略。 COM 中未包含的內容是特定的「作法」步驟，_也就是_客戶想知道「哪裡可以從這裡開始？」

在2018年10月，我們開始回顧所有在 Microsoft 社區中散佈的模型，我們發現大約有60個不同的雲端採用模型。 我們建立了一個跨 Microsoft 團隊，將所有專案結合在一起，成為專屬的工程「產品」，並在服務、銷售和行銷之間定義了不同的實施。 這項工作成果建立單一模型，這是適用于 Azure 的 Microsoft Cloud 採用架構，其設計目的是協助客戶瞭解**什麼**和**原因**，並提供**如何**協助其加速雲端採用的整合指引。 此專案的目標是要建立一個 Microsoft 的雲端採用方法。

## <a name="using-cloud-operating-model-practices-within-the-cloud-adoption-framework"></a>在雲端採用架構內使用雲端作業模式實務

對於 COM 的類似方法，讀取器的開頭應該是下列其中一項：

- [開始使用：加速移轉](../get-started/migrate.md)
- [開始使用：在雲端中建立和創新](../get-started/innovate.md)
- [透過音效作業模型實現成功](../get-started/enable.md)

先前在 COM 中提供的指導方針仍然與雲端採用架構有關。 體驗不同，但雲端採用架構的結構只是該指引的擴充。 若要從 COM 轉換到雲端採用架構，請務必瞭解範圍和結構。 下列兩節將描述該轉換。

## <a name="scope"></a>影響範圍

COM 已建立由下列元件組成的範圍：

<!-- cSpell:ignore caf -->

![雲端採用架構的範圍](../_images/caf-scope.png)

- **商務策略：** 建立雲端採用所要支援的清楚商業目標和結果。
- **技術策略：** 讓整體策略一致，以引導採用雲端以配合商務策略。
- **人員策略：** 開發策略來訓練人員和變更文化特性，以實現商務成功。

雲端作業模式和雲端採用架構的高階範圍很類似。 整個指引和雲端採用架構中的每個方法都會反映商務、文化和技術。

> [!NOTE]
> 雲端採用架構的範圍有兩個顯著的清楚點。 在此架構中，商務策略超越了雲端成本的檔， &mdash; 其概念是瞭解動機、所需的結果、退貨和雲端成本，以建立可採取行動的計畫並清除商業理由。 在雲端採用架構中，人員策略超越訓練，以包含建立顯而易見的文化成熟度的方法。 藍圖的幾個領域包括 agile 管理、DevOps 整合、客戶理解和對，以及精簡產品開發方法的影響。

## <a name="structure"></a>結構

COM 包含一個資訊圖，其中概述了雲端採用工作期間所需的各種決策和動作。 該圖形提供了一種清楚的方法來傳達後續步驟和相依的決策。

雲端採用架構遵循類似的模型。 不過，隨著將動作和決策擴充到多個決策樹中，複雜的速度很快就會出現一個圖形化視圖。 為了簡化指引並使其更能立即採取動作，單一圖形已分解成下列結構。

在執行層級中，雲端採用架構已簡化為下列三個採用階段，以及兩個主要治理指南。

![雲端採用架構的主管層級結構](../_images/caf-structure.png)

採用的三個階段是：

- **計畫：** 開發商務計畫，引導雲端採用與所需的商業結果一致。
- **就緒：** 準備人員、組織和技術環境，以執行採用計畫。
- **採用：** 執行特定採用方案所需的技術策略，以符合特定的採用旅程，以實現商業成果。

雲端採用的三個階段已對應到兩個特定的旅程：

- [遷移](../get-started/migrate.md)：將現有的工作負載移至雲端。
- [創新](../get-started/innovate.md)：現代化現有的工作負載，並建立新的產品和服務。

成功採用雲端所需的其他資源，可在[啟用「實現成功](../get-started/enable.md)」中找到。

## <a name="next-steps"></a>後續步驟

若要繼續進行 COM 停止的旅程，請選擇下列其中一個快速入門手冊：

- [開始使用雲端遷移](../get-started/migrate.md)
- [開始使用具備雲端功能的創新](../get-started/innovate.md)
- [啟用採用成功](../get-started/enable.md)
