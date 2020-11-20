---
title: 全球接觸成果的範例
description: 使用適用于 Azure 的雲端採用架構，瞭解雲端轉型內容中的全球接觸成果。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: strategy
ms.openlocfilehash: 9471b396885c3eb3e29d4e5c9892cf572c8f6f5c
ms.sourcegitcommit: 57b757759b676a22f13311640b8856557df36581
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2020
ms.locfileid: "94996854"
---
<!-- cSpell:ignore Personalizer -->
<!-- docutune:ignore "global reach" -->

# <a name="examples-of-global-reach-outcomes"></a>全球接觸成果的範例

如同在 [業務成果](./index.md)中所討論的，有數個潛在的業務成果可作為與企業之間任何轉型旅程交談的基礎。 本文著重于常見的商務措施：觸及。 觸達是很簡潔的詞彙，在此案例 _中是指_ 公司的全球化策略。 瞭解公司的全球化策略可協助您更清楚地表達企業轉型旅程目標的業務成果。

財富500和小型企業都著重于服務和客戶全球超過三年的全球化，而且大部分的企業都有可能與全球商務合作，因為這項全球化仍會持續吸引焦點。 裝載全球的資料中心可能耗用超過80% 的年度 IT 預算，而使用私用線來連接這些資料中心的廣域網路可能會每年花費上百萬美元的費用。 因此，支援全域作業既困難又昂貴。

雲端解決方案可將全球化成本移至雲端提供者。 在 Azure 中，客戶可以快速地將資源部署在與客戶或作業相同的區域中，而不需要購買及布建資料中心。 Microsoft 擁有世界上最大的廣域網路，也就是連接全球各地的資料中心。 全球客戶可依需求使用連線能力和全域作業容量。

## <a name="global-access"></a>全域存取

在轉型期間，擴展到新市場可能是最有價值的業務成果之一。 在不需要長期承諾的情況下，能夠快速地在市場部署資源，可讓銷售和營運領導人探索過去未考慮的選項。

### <a name="manufacturing-example"></a>製造範例

修飾製造商已找出趨勢。 即使沒有任何銷售小組在該區域中運作，仍會將某些產品寄送給亞太地區區域。 遠端銷售人員所需的最小系統很小，但延遲會防止遠端存取解決方案。 為了充分利用這個趨勢，銷售副總裁想要試驗日本和韓國的銷售團隊。 因為公司已經歷雲端遷移，所以能夠在數天內于日本和南韓國部署必要的系統。 這可讓銷售副總裁在三個月內依 _x%_ 成長到區域中的收益。 這兩個市場持續優於全球的其他部分，導致整個區域的銷售營運。

### <a name="retail-example"></a>零售範例

全球發行產品的線上零售商可以跨多個時區和多個語言與客戶互動。 零售商使用 azure Bot 服務和 Azure 認知服務中的各種功能，例如 Translator、Language Understanding (LUIS) 、QnA Maker 和文字分析。 這可確保客戶在需要時取得所需的資訊，並以其語言提供。 零售商會使用 [個人化工具服務](https://azure.microsoft.com/services/cognitive-services/personalizer/) 為其客戶進一步自訂體驗和目錄供應專案，以確保反映地理 tastes、喜好設定和可用性。

## <a name="data-sovereignty"></a>資料主權

在新市場中操作會引進其他治理條件約束。 Azure 提供合規性供應專案，協助客戶符合受管制產業和全球市場的合規性義務。 如需詳細資訊，請參閱 [Microsoft Azure 合規性的總覽](https://azure.microsoft.com/overview/trusted-cloud/compliance)。

### <a name="example"></a>範例

以美國為基礎的公用程式提供者已被授與在加拿大提供公用事業的合約。 加拿大資料主權法律要求加拿大的資料必須保留在加拿大。 這家公司一直以來都有多年的雲端應用程式創新工作。 因此，他們的軟體是透過完整的腳本 DevOps 程式來部署。 只要稍微變更程式碼基底，就可以將程式碼的工作複本部署到加拿大的 Azure 資料中心，符合資料主權的合規性，並保留客戶。

## <a name="next-steps"></a>下一步

深入了解客戶參與成果。

> [!div class="nextstepaction"]
> [客戶參與成果](./engagement-outcomes.md)
