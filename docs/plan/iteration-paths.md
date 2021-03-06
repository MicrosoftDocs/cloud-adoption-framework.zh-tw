---
title: 建立反覆項目和發行方案
description: 使用適用于 Azure 的雲端採用架構，瞭解如何定義反復專案和發行計畫，以協助您管理您的執行。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.custom: internal
ms.openlocfilehash: c954fba8ebc2b29ddda91c831bfcf0154b4244ce
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102116058"
---
# <a name="establish-iterations-and-release-plans"></a>建立反覆項目和發行方案

敏捷式及其他反復的方法都是以反覆運算和版本的概念為基礎。 本文概述在規劃期間指派反覆運算和發行。 這些指派可驅動時間軸可見度，讓雲端策略小組成員之間的交談更加容易。 指派也會以雲端採用小組在執行期間所能管理的方式來調整技術工作。

## <a name="establish-iterations"></a>建立反覆運算

在技術實施的反復方法中，您規劃了週期性時間區塊的技術工作。 反覆運算通常會是一到六周的時間區塊。 共識建議兩周是大部分雲端採用小組的平均反復專案持續時間。 但是反覆運算持續時間的選擇取決於技術工作的類型、系統管理負荷，以及小組的喜好設定。

若要開始將工作與時間軸保持一致，我們建議您定義一組最近6到12個月的反覆運算。

## <a name="understand-velocity"></a>瞭解速度

您必須瞭解速度，才能將工作與反復專案和版本保持一致。 速度是可在任何指定的反復專案中完成的工作量。 在提早規劃期間，速度是估計值。 經過幾個反復專案之後，速度就會成為小組可以安心進行之承諾的極具價值指標。

您可以用抽象類別來測量速度，例如案例點。 您也可以用更具體的詞彙來測量，例如時數。 針對大部分的反復式架構，我們建議使用抽象的度量，以避免在精確度和認知方面的挑戰。 本文中的範例代表每個短期衝刺的速度（以小時為單位）。 此標記法可讓主題更廣泛地理解。

**範例：** 有五個人的雲端採用小組致力於為期兩周的短期衝刺。 由於目前的義務（例如會議和其他程式的支援），每個小組成員在採用工作時，每週會持續提供20小時的時間。 針對此小組，每個短期衝刺的初始速度預估為100小時。

## <a name="iteration-planning"></a>反覆項目規劃

一開始，您可以根據排定優先順序的待辦專案來評估技術工作，以規劃反復專案。 雲端採用小組會評估完成各種工作所需的工作。 然後，這些工作會指派給第一個可用的反復專案。

在反復專案計劃期間，雲端採用小組會驗證並精簡預估值。 他們會這麼做，直到將所有可用的速度都調整到特定工作為止。 此程式會針對每個優先順序的工作負載繼續進行，直到所有的工作都符合預測的反復專案為止。

在這個過程中，小組會驗證指派給下一個短期衝刺的工作。 小組會根據小組對每項工作的對話來更新其估價。 然後，小組會將每個估計的工作新增至下一個短期衝刺，直到達到可用的速度為止。 最後，小組會預估額外的工作，並將其新增至下一個反復專案。 小組會執行這些步驟，直到該反覆運算的速度也耗盡為止。

先前的進程會繼續執行，直到將所有工作指派給反復專案為止。

**範例：** 讓我們根據上述範例建立。 假設每個工作負載遷移都需要40個工作。 也假設您預估每個工作的平均時間為一小時。 合併的估計大約是每個工作負載遷移40小時。 如果所有10個優先順序的工作負載的這些估計值都保持一致，則這些工作負載將需要400小時。

上一個範例中定義的速度，建議遷移前10個工作負載將會採用四個反復專案，也就是兩個月的行事歷時間。 第一個反復專案將包含導致兩個工作負載遷移的100工作。 在下一個反復專案中，類似的100工作集合會導致三個工作負載的遷移。

> [!WARNING]
> 上述的工作和估計數目會嚴格作為範例使用。 技術工作很少是一致的。 您不應該看到此範例反映出遷移工作負載所需的時間量。

## <a name="release-planning"></a>版本規劃

在雲端採用中，版本會定義為交付專案的集合，以產生足夠的商業價值，以證明商務程式中斷的風險。

將任何工作負載相關的變更釋出至生產環境，可為商務程式建立一些變更。 在理想情況下，這些變更很順暢，而且企業會看到變更的價值，而不會嚴重中斷服務。 但是，任何變更都有業務中斷的風險，且不應稍微小心。

為了確保變更會依可能的報酬來調整，雲端策略小組應參與發行計畫。 當工作與短期衝刺一致之後，小組就可以決定每個工作負載準備進行生產版本的粗略時間軸。 雲端策略小組會檢查每個版本的時間。 然後，小組會找出風險與商業價值之間的變化點。

**範例：** 繼續先前的範例，雲端策略小組已檢查過反復專案計劃。 此評論指出兩個發行端點。 在第二個反復專案中，總共有五個工作負載可以進行遷移。 這五個工作負載將提供重要的商業價值，並會觸發第一次的發行。 下一版會在接下來的五個工作負載可供發行時，稍後再進行兩次反覆運算。

## <a name="assign-iteration-paths-and-tags"></a>指派反復專案路徑和標記

對於在 Azure DevOps 中管理雲端採用方案的客戶而言，先前的處理常式會藉由將反復專案路徑指派給每個工作和使用者案例來反映。 我們也建議您將每個工作負載標記為特定版本。 該標記和指派摘要會自動填入時間軸報表。

## <a name="next-steps"></a>下一步

[預估時程表](./timelines.md) 以適當地傳達期望。

> [!div class="nextstepaction"]
> [預估時間表](./timelines.md)
