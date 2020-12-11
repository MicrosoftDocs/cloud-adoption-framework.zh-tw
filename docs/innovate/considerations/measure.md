---
title: 測量客戶的影響
description: 定義所需的客戶流程，並建立學習計量來測量客戶的行為和採用。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/27/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.custom: internal
ms.openlocfilehash: 876906c45f1f12ec82cc24eec0b499a00af8a2ea
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97017967"
---
# <a name="measure-for-customer-impact"></a>測量客戶影響

有數種方式可測量客戶的影響。 本文將協助您定義計量，以驗證因 [客戶理解](./build.md)而產生的假設。

## <a name="strategic-metrics"></a>策略性計量

[策略方法](../../strategy/index.md)會檢查[動機](../../strategy/motivations.md)和[業務成果](../../strategy/business-outcomes/index.md)。 這些做法提供一組計量來測試客戶的影響。 當創新成功時，您通常會看到與策略性目標一致的結果。

在建立學習計量之前，請定義您想要這項創新影響的少數策略性計量。 這些策略性計量通常會與下列一或多個結果區域一致：

- [企業靈活性](../../strategy/business-outcomes/agility-outcomes.md)
- [客戶參與](../../strategy/business-outcomes/engagement-outcomes.md)
- [客戶觸及](../../strategy/business-outcomes/reach-outcomes.md)
- [財務衝擊](../../strategy/business-outcomes/fiscal-outcomes.md)
- [解決方案效能](../../strategy/business-outcomes/fiscal-outcomes.md)，以營運創新的情況為例。

記錄已同意的計量，並經常追蹤其影響，但不預期會導致這些計量中的任何一種反復發生。 如需有關設定相關各方期望的詳細資訊，請參閱 [反覆運算的承諾](./index.md#commitment-to-iteration)。

除了動機和業務成果的度量之外，本文的其餘部分將著重于學習計量，其設計目的是要引導透明探索和客戶導向的反覆運算。 如需有關這些層面的詳細資訊，請參閱 [透明度承諾](./index.md#commitment-to-transparency)。

## <a name="learning-metrics"></a>學習計量

當第一版的最小可行產品 (MVP) 與客戶共用時，最好是在第一次開發反復專案結束時，不會對策略性計量產生任何影響。 幾個反復專案之後，小組可能仍會改變行為，足以影響策略性的度量。 在學習過程中，例如組建-測量-學習週期，我們建議小組採用學習計量。 這些計量追蹤和學習商機。

### <a name="customer-flow-and-learning-metrics"></a>客戶流程與學習計量

如果 MVP 解決方案驗證客戶導向的假設，解決方案將會在客戶行為方面帶來一些變化。 在客戶世代間變更這些行為，應能改善業務成果。 請記住，變更客戶行為通常是多步驟的流程。 由於每個步驟都有機會測量影響，因此採用小組可以持續學習，並建立更好的解決方案。

藉由對應您希望在 MVP 解決方案中看到的流程，來瞭解客戶行為的變更開始。

![用來判斷學習計量的客戶流程](../../_images/innovate/customer-flow-learning-metrics.png)

在大部分的情況下，客戶流程將會有一個簡單定義的起點，而且不超過兩個端點。 在開始與結束點之間，您可以使用各種學習計量作為意見反應迴圈中的量值：

1. **起始點-初始觸發程式：** 起始點是觸發此解決方案需求的案例。 當您以客戶理解的方式建立解決方案時，該初始觸發程式應該會激發客戶嘗試 MVP 解決方案的過程。
2. **客戶需求符合：** 使用解決方案符合客戶的需求時，就會驗證假設。
3. **解決方案步驟：** 此詞彙是指將客戶從初始觸發程式移至成功結果所需的步驟。 每個步驟都會根據客戶決策來產生學習度量，以繼續進行下一個步驟。
4. 達成的 **個別採用：** 下一次遇到觸發程式時，如果客戶返回解決方案以達到其需求，則已達成個別採用。
5. **商務結果指標：** 當客戶的行為與所定義商務結果產生貢獻時，會觀察到商務結果指標。
6. **真正的創新：** 當 _商務結果_ 指標和 _個別採用_ 都發生在所需的規模時，您就能實現真正的創新。

客戶流程的每個步驟都會產生學習計量。 在每次反覆運算 (或發行) 之後，會測試新的假設版本。 同時，對方案進行調整會經過測試，以反映假設中的調整。 當客戶在任何指定的步驟中遵循指定的路徑時，會記錄正面的度量。 當客戶偏離指定的路徑時，會記錄負面度量。

這些對齊和偏差計數器會建立學習計量。 當雲端採用小組進行業務成果和真正的創新時，每個都應記錄和追蹤。 在 [瞭解客戶](./learn.md)的情況下，我們將討論套用這些計量的方式，以瞭解並建立更好的解決方案。

### <a name="group-and-observe-customer-partners"></a>群組及觀察客戶合作夥伴

定義學習計量的第一個測量是客戶夥伴定義。 參與創新週期的任何客戶都有資格成為客戶合作夥伴。 若要精確地測量行為，您應該使用世代模型來定義客戶合作夥伴。 在此模型中，會將客戶分組，以讓您瞭解其對 MVP 變更的回應。 這些群組通常如下所示：

- **實驗或焦點群組：** 根據其參與特定實驗（設計來測試一段時間的變更）來將客戶分組。
- **區段：** 依公司規模將客戶分組。
- **垂直：** 依產業所代表的 _產業_ 來將客戶分組。
- **個別人口統計：** 根據個人人口統計（例如年齡和實體位置）進行分組。

這些類型的群組可協助您在您的創新工作期間，為這些客戶選擇與您合作的各種不同的跨區段驗證學習計量。 所有後續的計量都應衍生自可定義的客戶群組。

## <a name="next-steps"></a>後續步驟

隨著學習計量的累積，小組可以開始向 [客戶學習](./learn.md)。

> [!div class="nextstepaction"]
> [向客戶學習](./learn.md)

本文中的一些概念是以 Eric Ries 所撰寫的「 [精簡啟動](http://theleanstartup.com/book)」中所述的主題為基礎。
