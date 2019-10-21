---
title: 雲端創新：量值
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 雲端創新簡介-測量內容
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/27/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: 00928171c3bfba1638a7e8e43ea08df4745754d2
ms.sourcegitcommit: 35c162d2d09ec1c4a57d3d57a5db1d56ee883806
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2019
ms.locfileid: "72557644"
---
# <a name="measure-for-customer-impact"></a>對客戶影響的測量

有數種方式可測量客戶的影響。 本文將協助定義量值，以協助驗證在嘗試[以客戶的方式建立](./build.md)時所建立的假設。

## <a name="strategic-metrics"></a>策略性計量

在雲端採用生命週期的[策略階段](../../strategy/index.md)中，讀者會引導您建立和說明[動機](../../strategy/motivations.md)和[商務結果](../../strategy/business-outcomes/index.md)。 這些練習會提供一組計量，做為客戶影響的測試。 當創新成功時，它會產生與策略目標一致的結果。

建立學習計量之前，請定義這項創新預期會影響的少數策略性計量。 這些策略性計量通常會符合下列一個或多個結果區域：[商業](../../strategy/business-outcomes/agility-outcomes.md)彈性、[客戶參與](../../strategy/business-outcomes/engagement-outcomes.md)、[客戶](../../strategy/business-outcomes/reach-outcomes.md)觸達、[財務衝擊](../../strategy/business-outcomes/fiscal-outcomes.md)，或在操作創新案例中：[解決方案效能](../../strategy/business-outcomes/fiscal-outcomes.md)。

記錄已同意的計量，並經常追蹤影響。 但是，不預期會導致這些計量中的任何一個專案出現，以進行數個反復專案。 請參閱[反覆運算承諾](./index.md#commitment-to-iteration)以配合預期。

除了動機和商業結果計量以外，本文的其餘部分將著重于學習設計來引導透明探索和客戶焦點反復專案的度量。 請參閱[對透明度的承諾](./index.md#commitment-to-transparency)，使其符合預期。

## <a name="learning-metrics"></a>學習計量

當第一版的最小可行產品（MVP）與客戶共用時，最好是在第一次開發反復專案結束時，不會對策略性計量造成任何影響。 幾個反復專案之後，小組可能仍在努力變更行為，以對策略計量進行切實的影響。 在學習程式期間（例如，組建-測量-學習週期），建議小組採用學習計量。 您可以更輕鬆地追蹤這些計量，並利用此機會學習。

### <a name="customer-flow-and-learning-metrics"></a>客戶流程和學習計量

如果 MVP 解決方案驗證以客戶為焦點的假設，解決方案將會驅動客戶行為中的一些變更。 各種客戶世代的行為會推動商務成果。 幸好，變更客戶行為通常是一個多步驟的流程。 每個步驟都提供測量影響的機會，讓採用小組可以學習並建立更好的解決方案。

從對應 MVP 解決方案所建立的所需流程開始，瞭解客戶行為的變更。

![用來判斷學習計量的客戶流程](../../_images/innovate/customer-flow-learning-metrics.png)

在大部分情況下，客戶流程會有一個容易定義的起點，而且不會有兩個以上的結束點。 在起點和終點之間，會使用各種學習計量來作為意見反應迴圈中的量值：

1. **起始點-初始觸發程式：** 起始點是觸發此解決方案需求的案例。 當解決方案是以客戶的理解方式建立時，該初始觸發程式應該會激發客戶嘗試 MVP 解決方案。
2. **符合客戶需求：** 當使用解決方案滿足客戶的需求時，就會驗證假設。
3. **解決方案步驟：** 將客戶從初始觸發程式移至成功結果所需的每個步驟都是解決方案步驟。 每個步驟都會根據客戶決策產生學習度量，以移至下一個步驟。
4. 已**達到個別採用：** 下一次遇到觸發程式時，如果客戶回到解決方案以再次符合需求，則會達成個別採用。
5. **商務結果指標：** 當客戶的行為與已定義的商務結果有正面的影響時，就會觀察到商務結果指標。
6. **真正的創新：** 當「商務結果指標」和「個別採用」兩者都發生在所需的規模時，真正的創新已經有經驗。

客戶流程的每個步驟都會產生學習計量。 在每個反復專案（或發行）之後，會測試新的假設版本。 同時，調整解決方案的工作會經過測試，以反映假設中的調整。 當客戶在任何指定的步驟遵循指定的路徑時，就會記錄正面的計量。 當客戶偏離指定的路徑來符合其需求時，就會記錄負的度量。

這些對齊和偏差計數器會建立學習計量。 當雲端採用小組進行商業成果和真正的創新時，應記錄並追蹤每個專案。 在下一篇文章中，透過[客戶合作夥伴學習](./learn.md)，我們將討論如何利用這些計量來學習及建立更好的解決方案。

### <a name="grouping-and-observing-customer-partners"></a>分組和觀察客戶合作夥伴

定義學習度量的第一個測量是客戶合作夥伴定義。 參與創新週期的任何客戶都有資格成為客戶合作夥伴。 若要準確地測量行為，請務必在世代模型中定義客戶合作夥伴。 在此模型中，客戶會進行分組，以充分瞭解其如何回應 MVP 中的任何變更。 客戶合作夥伴通常會根據如下的資料點進行分組：

- **實驗或焦點群組：** 根據參與特定實驗的客戶進行分組，以測試經過一段時間的變更。
- **區段：** 依公司的大小將客戶分組。
- **垂直：** 依產業的垂直方向來分組客戶。
- **個別人口統計：** 根據個人人口統計（例如年齡或實體位置）分組。

這種類型的群組可讓您在創新工作期間選擇與您合作的各種客戶跨區段進行學習計量驗證。 所有後續的計量都應該根據可定義的客戶群組來觀察。

## <a name="next-steps"></a>後續步驟

隨著學習計量的累積，小組可以開始[學習客戶](./learn.md)。

> [!div class="nextstepaction"]
> [向客戶學習](./learn.md)

這篇文章中的一些概念，是根據 Eric Ries 所撰寫[的精簡啟動](http://theleanstartup.com/book)中所述的主題來建立。
