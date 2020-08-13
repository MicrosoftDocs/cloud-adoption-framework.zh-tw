---
title: 全球範圍的結果範例
description: 使用適用于 Azure 的雲端採用架構，瞭解雲端轉換內容中的全球觸達結果。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: strategy
ms.openlocfilehash: 280281d28677b787e4d3dba1946f642d6df72d2b
ms.sourcegitcommit: 949b87bad28d32df84df190160089f01826f3a31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2020
ms.locfileid: "88196163"
---
<!-- cSpell:ignore Personalizer -->
<!-- docsTest:ignore "global reach" -->

# <a name="examples-of-global-reach-outcomes"></a>全球範圍的結果範例

如 [商務結果](./index.md)中所述，有數個潛在的商業成果可以做為與企業進行之任何轉換旅程的基礎。 本文著重于常見的商務措施：觸及。 觸達是一種精確的詞彙，在此案例*中是指*公司的全球化策略。 瞭解公司的全球化策略可協助您更清楚地表達企業轉型旅程目標的商業成果。

財富500和小型企業著重于服務和客戶全球化三年以上，大部分的企業都可能會參與全球商務，因為此全球化會繼續提取焦點。 裝載世界各地的資料中心可能會耗用超過80% 的年度 IT 預算，而使用私用線路來連接這些資料中心的領域網路，每年可能會花費數百萬美元的費用。 因此，支援全域作業既困難又昂貴。

雲端解決方案將全球化成本轉移到雲端提供者。 在 Azure 中，客戶可以快速地在與客戶或作業相同的區域中部署資源，而不需要購買和布建資料中心。 Microsoft 擁有全球最大範圍的網路，連接全球各地的資料中心。 連線能力和全域作業容量可供全球客戶隨選使用。

## <a name="global-access"></a>全域存取

擴大到新市場可能是轉換期間最有價值的商務成果之一。 能夠快速地在市場部署資源，而不需要長期承諾，可讓銷售和營運領導人探索過去未考慮的選項。

### <a name="manufacturing-example"></a>製造範例

「修飾廠商」已識別出趨勢。 有些產品會運送到亞太地區區域，即使沒有任何銷售小組在該區域中運作。 遠端銷售人員所需的最小系統很小，但是延遲會阻止遠端存取解決方案。 若要利用這種趨勢，銷售副總裁想要試驗日本和韓國的銷售團隊。 由於公司已經歷雲端遷移，因此能夠在數天內將必要的系統部署在日本和南非。 如此一來，銷售副總裁就能在三個月內，依 *x%* 來增加收益。 這兩個市場會繼續優於世界的其他部分，而導致整個區域的銷售作業。

### <a name="retail-example"></a>零售範例

全球發行產品的線上零售商可以跨時區和多種語言與客戶互動。 零售商會使用 azure Bot Service 和 Azure 認知服務中的各種功能，例如 Translator、Language Understanding (LUIS) 、QnA Maker 和文字分析。 這可確保客戶能夠在需要時取得所需的資訊，並以其語言提供給他們。 零售商會使用 [個人化工具服務](https://azure.microsoft.com/services/cognitive-services/personalizer/) ，為客戶進一步自訂體驗和類別目錄供應專案，確保會反映地理 tastes、喜好設定和可用性。

## <a name="data-sovereignty"></a>資料主權

在新市場中作業會引進額外的治理條件約束。 Azure 提供合規性供應專案，協助客戶符合受管制產業和全球市場的合規性義務。 如需詳細資訊，請參閱 [Microsoft Azure 合規性的總覽](https://azure.microsoft.com/overview/trusted-cloud/compliance)。

### <a name="example"></a>範例

以美國為基礎的公用程式提供者已獲得合約，以提供加拿大的公用程式。 加拿大的資料主權法要求加拿大的資料必須保持在加拿大。 這家公司一直致力於進行雲端式應用程式創新工作，多年來。 因此，他們的軟體是透過完全腳本的 DevOps 流程來部署。 除了程式碼基底的幾個次要變更，他們能夠將程式碼的工作複本部署到加拿大的 Azure 資料中心，符合資料主權合規性並留住客戶。

## <a name="next-steps"></a>後續步驟

深入了解客戶參與成果。

> [!div class="nextstepaction"]
> [客戶參與成果](./engagement-outcomes.md)
