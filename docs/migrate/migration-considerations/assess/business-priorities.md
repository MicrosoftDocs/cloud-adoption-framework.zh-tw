---
title: 轉換過程中的業務優先順序
description: 使用適用于 Azure 的雲端採用架構，瞭解如何在長期轉換程式期間維護業務一致性。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: internal
ms.openlocfilehash: e4515018286a5aa36d26aea7af36625e82020f12
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101788958"
---
# <a name="business-priorities-maintaining-alignment"></a>業務優先順序：維持一致

*轉換* 通常定義為大幅或自發性的變更。 對董事會階層而言，變更可能形同大幅的轉換。 但對於在組織中執行變更的人員而言，轉換會有點誤導。 本質上，將轉換解釋為一系列從某個狀態到另一個狀態的適當轉型，會比較恰當。

工作負載的合理化或轉型所需的時間會有所不同，視涉及的技術複雜度而定。 不過，即便此程序可快速地套用至單一工作負載或應用程式群組，要在使用者群間產生實質性的改變，仍需要時間。 要將傳播到現有商務程序的各種層級，則需要更長的時間。 如果預期轉換會形塑消費者的行為模式，可能需要更長的時間才能產生顯著的結果。

可惜的是，市場並不會靜待企業轉型。 消費者行為模式本身就常會出乎意料地發生變化。 市場對公司及其產品的認知，可能會因為社交媒體或競爭同業的定位而搖擺不定。 面對快速而難測的市場，公司必須能靈活因應。

要能夠執行程序和技術轉型，須仰賴一致且穩定的工作。 必須要有迅速的決策和靈活的措施，才能因應市場狀況。 這兩者是互相矛盾的，因此優先順序很難保有一致性。 本文說明在移轉工作執行期間有哪些方法可維持過渡的一致性。

## <a name="how-can-business-and-technical-priorities-stay-aligned-during-a-migration"></a>在移轉期間，業務和技術的優先順序如何保持一致？

雲端採用小組和雲端治理小組將焦點放在目前反覆項目和目前發行的執行。 反覆項目可持續提供穩定的技術工作，繼而避免因作業中斷而付出移轉工作進度減緩的高昂代價。 發行可確保技術工作和人力會持續專注於工作負載移轉的業務目標。 移轉專案可能需要在較長的時間內進行多次發行。 完成後，市場狀況可能已大不相同。

在此同時，雲端策略小組會全力執行業務變更計劃，並為下一個版本做準備。 雲端策略小組通常會往回查看至少一個版本，且會監視不斷變化的市場狀況，並據以調整移轉待辦項目。 這種將重心放在管理轉換和調整計劃的做法，可自然形成技術工作的樞紐。 當業務優先順序變更時，採用的項目只會落後一個版本，技術和業務靈活性因而產生。

## <a name="business-alignment-questions"></a>業務一致性問題

下列問題可協助雲端策略小組形成移轉待辦項目並排定其優先順序，以利確保轉換工作能夠最符合目前的業務需求。

- 雲端採用小組是否已識別出可供移轉的工作負載清單？
- 雲端採用小組是否已從該工作負載清單中選取要進行初始移轉的單一候選項目？
- 雲端採用小組和雲端治理小組是否具有成功的工作負載和雲端環境所需的所有必要資料？
- 候選工作負載是否會在下一個版本中為業務帶來最具相關性的影響？
- 是否有其他工作負載更適合作為移轉的候選項目？

## <a name="tangible-actions"></a>實際動作

在執行業務變更計劃期間，雲端策略小組會監視正面和負面的結果。 當這些觀察結果需要技術變更時，可將調整當作工作項目新增至要在下一個反覆項目中優先處理的發行待處理項目。

當市場發生變化時，雲端策略小組會與業務部門合作，以了解如何最適當地因應變化。 如果該因應措施需仰賴移轉優先順序的變更，可以調整移轉待處理項目。 這會將先前優先順序較低的工作負載向上移。

## <a name="next-steps"></a>下一步

藉由適當調整業務優先順序，雲端採用小組可以放心地開始[評估工作負載](./evaluate.md)，以開發架構和移轉計劃。

> [!div class="nextstepaction"]
> [評估工作負載](./evaluate.md)
