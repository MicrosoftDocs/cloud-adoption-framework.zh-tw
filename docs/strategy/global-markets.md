---
title: 瞭解全球市場決策的影響
description: 全球市場概念的說明
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: strategy
ms.openlocfilehash: af3c9d8b155d2c6a5e861e64b6472effbafcbb6d
ms.sourcegitcommit: 2362fb3154a91aa421224ffdb2cc632d982b129b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/28/2020
ms.locfileid: "76798157"
---
<!-- markdownlint-disable MD026 -->

# <a name="how-will-global-market-decisions-affect-the-transformation-journey"></a>全球市場決策會如何影響轉型旅程？

雲端可開啟新的機會，以全球規模執行。 全球營運的阻礙會大幅降低，方法是讓公司能夠在市場部署資產，而不需要大量投資新的資料中心。 可惜的是，這也會增加技術和法律觀點的複雜複雜度。

## <a name="data-sovereignty"></a>資料主權

許多地緣政治區域都已建立資料主權法規。 這些規定會限制可以儲存資料的位置、哪些資料可以離開國家/地區，以及該區域公民的哪些資料可以收集。 在操作外部地理位置中的任何雲端式解決方案之前，您應該瞭解該雲端提供者如何處理資料主權。 如需每個地理位置的 Azure 方法詳細資訊，請參閱[這裡](https://azure.microsoft.com/global-infrastructure/geographies)。 如需 Azure 合規性的詳細資訊，請參閱 microsoft 信任中心的[Microsoft 隱私權](https://www.microsoft.com/trustcenter/privacy)。

本文的其餘部分假設法律顧問已審查並核准國外國家/地區的作業。

## <a name="business-units"></a>業務單位

請務必瞭解哪些業務單位在外部國家/地區運作，以及哪些國家/地區會受到影響。 此資訊將用來設計裝載、計費和部署至雲端提供者的解決方案。

## <a name="employee-usage-patterns"></a>員工使用模式

請務必瞭解全域使用者如何存取與使用者位於相同國家/地區的應用程式。 全球的廣域網路（Wan）會根據現有的網路通訊協定來路由傳送使用者。 在傳統的內部部署環境中，某些條件約束會限制 WAN 設計。 這些條件約束可能會導致不佳的使用者體驗，因為在雲端採用之前未正確瞭解。

在雲端模型中，商用網際網路也會開啟許多新的選項。 跨多個地理位置溝通員工的散佈，可協助雲端採用小組設計 WAN 解決方案，以建立正面的使用者體驗 **，並**可能降低網路成本。

## <a name="external-user-usage-patterns"></a>外部使用者使用模式

瞭解外部使用者（例如客戶或合作夥伴）的使用模式也同樣重要。 外部使用者使用模式與員工使用模式非常類似，可能會對雲端部署的效能造成負面影響。 當大型或關鍵性的使用者基底位於國外的國家/地區時，可以將全域部署策略納入整體解決方案設計中。

## <a name="next-steps"></a>後續步驟

一旦做出全球市場決策並傳達，小組就準備好開始針對這些計量[建立技術標準](../digital-estate/index.md)。
結果會是[轉換待處理專案或遷移待](..//migrate/migration-considerations/prerequisites/technical-complexity.md)處理專案。

> [!div class="nextstepaction"]
> [評估數位資產](../digital-estate/index.md)
