---
title: 全球市場決策的影響
description: 使用適用于 Azure 的雲端採用架構，瞭解全球市場決策會如何影響雲端的轉型旅程。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: strategy
ms.custom: internal
ms.openlocfilehash: d01e7333d6e5e85576287248073562fc2f2b181f
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97012850"
---
# <a name="how-will-global-market-decisions-affect-the-transformation-journey"></a>全球市場決策會如何影響轉型旅程？

雲端會開啟新的機會來執行全球規模。 全球營運的阻礙會大幅降低，藉由讓公司將資產部署到市場，而不需要大量投資新的資料中心。 可惜的是，這也會從技術和法律觀點來增加許多複雜性。

## <a name="data-sovereignty"></a>資料主權

許多地緣政治區域都已建立資料主權規定。 這些法規會限制儲存資料的位置、哪些資料可以離開國家/地區，以及可以針對該區域的公民收集哪些資料。 在外部地理位置操作任何雲端式解決方案之前，您應該先瞭解該雲端提供者如何處理資料主權。 如需 Azure 針對每個地理位置的方法詳細資訊，請參閱 [azure 地理](https://azure.microsoft.com/global-infrastructure/geographies)位置。 如需 Azure 中合規性的詳細資訊，請參閱 microsoft 信任中心的 [Microsoft 隱私權](https://www.microsoft.com/trust-center/privacy) 。

本文的其餘部分假設法律顧問已在國外的國家/地區審核並核准作業。

## <a name="business-units"></a>業務單位

請務必瞭解哪些營業單位營業于外部國家/地區，以及哪些國家/地區受到影響。 此資訊將用來設計解決方案，以裝載、計費和部署至雲端提供者。

## <a name="employee-usage-patterns"></a>員工使用模式

請務必瞭解全域使用者如何存取未與使用者位於相同國家/地區的應用程式。 全球廣域網路 (Wan) 根據現有的網路通訊協定來路由使用者。 在傳統的內部部署環境中，某些條件約束會限制 WAN 的設計。 如果在雲端採用之前未適當瞭解，這些條件約束可能會導致使用者體驗不佳。

在雲端模型中，商用網際網路也會開啟許多新選項。 跨多個地理位置傳達員工散佈，可協助雲端採用小組設計 WAN 解決方案來建立正面的使用者體驗 **，並** 可能降低網路成本。

## <a name="external-user-usage-patterns"></a>外部使用者使用模式

瞭解外部使用者的使用模式（例如客戶或合作夥伴）也同樣重要。 就像員工使用模式一樣，外部使用者使用模式可能會對雲端部署的效能產生負面影響。 當大型或任務關鍵性的使用者群位於外部國家/地區時，在整體解決方案設計中包含全域部署策略可能是明智的做法。

## <a name="next-steps"></a>後續步驟

瞭解雲端採用旅程 [策略階段期間所需的技能](./suggested-skills.md) 。

> [!div class="nextstepaction"]
> [策略階段期間所需的技能](./suggested-skills.md)
