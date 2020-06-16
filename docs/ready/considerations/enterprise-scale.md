---
title: 開始使用企業級登陸區域
description: 開始使用企業級登陸區域
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/15/2020
ms.topic: overview
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 071a8edd7faebcc5df0e91154c02857287e3638d
ms.sourcegitcommit: d88c1cc3597a83ab075606d040ad659ac4b33324
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/15/2020
ms.locfileid: "84787466"
---
# <a name="start-with-enterprise-scale-landing-zones"></a>開始使用企業級登陸區域

有時候，雲端平臺小組會造成，以小規模開始進行調整。 小組在公司現有內部部署環境的限制範圍內工作，以達到安全性、營運及治理方面的成熟度目前狀態。 根據任何雲端環境的新條件約束，複寫所需的處理常式、工具和架構會需要一些時間。 若要加速該學習程式，需要稍微不同的起點。 將下圖與[此方法中的早期重構指引](../landing-zone/refactor.md)進行比較之後，基本的變更是起點，現在更複雜，本文稍後會更詳細地說明。

![登陸區域重構圖例-本文稍後的章節中所述](../../_images/ready/refactor-enterprise-scale.png)

## <a name="qualifiers-should-i-start-with-enterprise-scale"></a>限定詞：我應該從企業規模開始嗎？

對於大部分的採用模式而言，最好使用「開始小型和擴充」方法，因為它可讓小組從真實的經驗中學習。 對於符合本文參考的公司而言，需要更穩固的方法。

### <a name="scale-and-speed"></a>規模和速度

當採用小組在雲端中有 midterm 目標（在24個月內）來裝載超過1000個資產（應用程式、基礎結構或資料資產）時，就沒有足夠的時間學習實際操作方法。 需要預先設定多個準則，以提供大規模企業所需的速度和規模。

### <a name="security-compliance-and-culture"></a>安全性、合規性和文化特性

有多個業務動機可能需要企業級登陸區域和共用服務架構，才能開始使用。 企業級登陸區域解決方案的需求，對於其企業是以機密資料和複雜相互依存架構為基礎的公司來說，可能很明顯。 當公司需要符合嚴格協力廠商需求的雲端環境，再使用雲端時，可能也會很明顯。 具有深層根之中央 IT 控制模型的文化特性可能也需要一個以集中式控制開始的架構，以通過變更控制需求。

### <a name="all-in-on-the-cloud"></a>所有-在雲端上

企業規模登陸區域最常見的驅動程式，是來自決定在雲端「全部」的公司。 這可能是資料中心終止或大量移動的結果，以做為企業的敏捷度。 無論哪一種驅動程式，這項商業決策通常都會要求採用的規模和採用速度。 這種需求的組合，讓您難以花時間學習和準備雲端中的敏感性資料和任務關鍵性裝載。

### <a name="skill-requirements"></a>技能需求

從 enterprise scale 開始，假設雲端平臺小組有企業級的預算。 此辨識符號假設小組在雲端中具有比其他大部分公司更深入的技能。 這些技能可以來自公司內較小規模雲端採用的現有歷程記錄。 您也可以藉由吸引有經驗的人員或與經驗豐富的合作夥伴合作，來新增技能。 不論是哪一種方向，都必須具備下列技能，才能從企業規模開始。

建議的技能：

- 深入瞭解所選雲端提供者中的架構。
- 使用雲端式、基礎結構即程式碼方法的大量實際操作體驗。
- 適中熟悉 GitHub 或其他原始程式碼存放庫，包括分支和提取要求策略。
- 從原始程式碼自動部署到雲端提供者的可操作體驗。

如果這些技能在雲端平臺小組中無法使用（透過員工、合作夥伴或其他支援機制），則「開始小型和擴充」方法可能會以更高品質的輸出來達到企業就緒的速度。 這種方法可讓您在現有團隊內取得成本較低的技能。

## <a name="start-with-an-enterprise-scale-landing-zones"></a>從企業級登陸區域開始

Microsoft 有大量投資工具和方法的歷程記錄，可讓客戶更輕鬆地開發及管理企業規模的登陸區域。 過去幾年來，這項投資已導致 Azure 上的指引和工具。 這項投資方法會繼續進行，而且可能會導致此特定文章的這一節定期更新。

![登陸區域重構圖例](../../_images/ready/refactor-enterprise-scale.png)

下列每個範本都提供客戶一個企業規模的初始登陸區域，包括基礎結構模式：

- [ISO 27001 共用服務](https://docs.microsoft.com/azure/governance/blueprints/samples/iso27001-shared)
- [ISO 27001 App Service 環境/SQL Database 工作負載](https://docs.microsoft.com/azure/governance/blueprints/samples/iso27001-ase-sql-workload)
- [英國官方和英國 NHS 治理](https://docs.microsoft.com/azure/governance/blueprints/samples/ukofficial)

您可以使用[Azure 藍圖範例文章](https://docs.microsoft.com/azure/governance/blueprints/samples)中的其他範例，做為企業級登陸區域的「紅色/綠色」測試。 套用這些藍圖可確保在採用之前，環境符合合規性標準。 這個較新的方法特別適合用來驗證協力廠商或合作夥伴登陸區域，然後再採用雲端：

## <a name="next-steps"></a>後續步驟

選擇其中一個企業級登陸區域藍圖。
從這裡開始，您可以使用從[小規模和擴展](./index.md)中的相同指引來擴充您的企業規模登陸區域，以符合您的獨特需求。

> [!div class="nextstepaction"]
> [使用您的企業級別登陸區域做為初始來源，繼續「開始小型和擴充」指引](./index.md)
