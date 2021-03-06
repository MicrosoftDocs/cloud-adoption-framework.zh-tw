---
title: 開發商務變更計畫
description: 使用適用于 Azure 的雲端採用架構，瞭解業務變更計畫如何協助您實現更廣泛的使用者採用計畫。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: internal
ms.openlocfilehash: d109ec2fc98b8cdfddbcb5b9dc738b9eb1274945
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101785507"
---
<!-- cSpell:ignore Eason -->
<!-- docutune:casing "Eason Matrix" -->

# <a name="business-change-plan"></a>業務變更方案

傳統上，IT 已監督新工作負載的發行。 在進行重大轉換期間 (例如，資料中心移轉或雲端移轉)，可能會套用類似的 IT 前導採用模式。 不過，傳統方法可能會錯失實現額外商業價值的機會。 為此，在將遷移後的工作負載升階至生產環境之前，建議您先實作更廣泛的使用者採用方法。 本文會概述用來將業務變更方案新增至標準使用者採用方案的方法。

## <a name="traditional-user-adoption-approach"></a>傳統的使用者採用方法

使用者採用方案著重在使用者會如何採用新技術或指定技術的變更。 此方法已經過時間測試，可用於對使用者介紹新工具。 在典型的使用者採用方案中，IT 著重在與業務環境所引進的技術變更相關聯的安裝、設定、維護和訓練上。

雖然方法可能不一，但大部分的使用者採用方案都有普遍的主題。 這些主題通常會以可配合增量改變的風險控制和促進方法為基礎。 下圖所示的 Eason 矩陣，代表這些主題在一系列採用型別背後的驅動程式。

![使用者採用考慮圖表的 Eason 矩陣 ](../../../_images/migrate/eason-matrix.jpg)
 *：使用者採用類型的 Eason 矩陣。*

這些主題通常會以「引進新解決方案給使用者應主要著重於變更的風險控制和促進上」的假設作為基礎。 此外，IT 主要著重於技術變更的風險和該變更的促進上。

## <a name="create-business-change-plans"></a>建立商務變更計畫

業務變更方案不只會探討技術變更本身，還會假設移轉工作中的每個發行都可推動某種程度的業務程序變更。 其會探討技術變更的上下游。 下列問題可協助參與者從業務變更的觀點思考使用者採用，以發揮最大的業務影響：

**上游問題。** 上游問題會探討使用者採用發生之前的影響或變更：

- 預期的[業務成果](../../../strategy/business-outcomes/index.md)是否已量化？
- 業務影響是否會對應到已定義的[學習計量](../../../strategy/learning-metrics.md)？
- 哪些業務程序和小組會利用此技術解決方案？
- 公司中有誰最能在測試和意見反應方面配合進階使用者？
- 受影響的業務主管是否有參與規劃優先順序和移轉？
- 公司是否有任何重大事件或日期可能會受到這項變更影響？
- 業務變更方案是否會發揮最大影響但將業務干擾降到最低？
- 是否預期會有停機？ 是否已向使用者告知停機時間範圍？

**下游問題。** 採用完成後，就可以開始進行業務變更。 可惜的是，許多使用者採用方案會至此結束。 下游問題可協助雲端策略小組在技術變更完成後持續關注該轉換：

- 業務使用者是否能良好地回應變更？
- 既然已採用技術變更，效能是否符合預期？
- 業務程序或客戶體驗是否以預期的方式變更？
- 是否需要其他變更才能實現學習計量？
- 變更是否符合目標業務成果？ 如果不是，原因為何？
- 是否需要其他變更才能參與業務成果？
- 是否有觀察到這項變更造成了任何負面效果？

業務變更方案會因公司而異。 這些問題的目標是要協助讓業務更好地整合到與每個發行相關聯的變更。 不要將每個發行視為要採用的技術變更，而是視為業務變更方案，業務成果就會變得更唾手可得。

## <a name="next-steps"></a>下一步

在記載並規劃業務變更後，就可以開始進行[業務測試](./business-test.md)。

> [!div class="nextstepaction"]
> [在移轉期間進行商務測試 (UAT) 的指引](./business-test.md)

## <a name="references"></a>參考資料

<!-- docutune:disable -->

- Eason，K. (1988) *資訊技術和組織變更*，紐約： Taylor 和 Francis。

<!-- docutune:enable -->
