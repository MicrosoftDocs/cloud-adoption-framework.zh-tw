---
title: 準備公司的雲端 IT 原則
description: 透過關鍵活動 (例如漸進式公司原則變更和自動化強制執行) 協助啟用擴充的治理模型。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: internal
ms.openlocfilehash: c2a019cbfbf1eec798bf4ca9858d415ad93c4884
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102112325"
---
# <a name="prepare-corporate-it-policy-for-the-cloud"></a>準備公司的雲端 IT 原則

雲端治理是在一段時間內持續進行採用工作的成果，因為轉換不會在一夕之間發生。 使用快速積極的方法，嘗試在解決關鍵公司原則變更之前提供完整雲端治理，這樣很少會產生想要的結果。 相反地，我們建議漸進的方法。

雲端採用架構的不同之處在於購買週期，以及該週期如何產生有效的轉換。 由於沒有龐大的基本建設費用收購需求，工程師可以更快開始實驗及採用。 在大部分公司文化中，消除採用的資本支出障礙可能導致回饋循環、有機成長及漸進執行變得緊迫。

雲端採用的移轉，需要是受治理的移轉。 在許多組織中，公司原則轉換透過漸進原則變更和自動化那些變更強制執行 (與雲端服務提供者一同定義的新能力)，而達到改善的治理和更高的遵守率。

本文概述能協助您改變您公司原則的關鍵活動，以啟用展開的治理模型。

## <a name="define-corporate-policy-to-mature-cloud-governance"></a>定義公司原則使雲端治理成熟

在傳統治理和雲端治理中，公司原則建立作用的治理定義。 大部分的「IT 治理」動作的目的是要實作技術，以監視、強制執行、操作那些公司原則並自動化。 雲端治理建立在類似的概念上。

![公司治理和治理專業領域](../../_images/operational-transformation-govern-large.png)
*圖 1：公司治理和治理專業領域。*

上圖示範業務風險、原則與合規性，以及監視與強制執行之間對於建立治理策略的互動。 其後是實現您策略的五個雲端治理專業領域。

## <a name="review-existing-policies"></a>檢閱現有原則

在上圖中，治理策略 (風險、原則與合規性、監視與強制執行) 是從辨識業務風險開始。 建立長久雲端治理策略的第一步是了解[業務風險](./business-risk.md)在雲端中有何變更。 與您的業務單位合作，取得精確的[業務風險容忍量表](./risk-tolerance.md)，以協助您了解需要補救哪些層級的風險。 您對於新風險和容忍程度的了解可記載成[現有原則評論](./cloud-policy-review.md)，以判斷適合您組織的必要治理層級。

> [!TIP]
> 如果您的組織受第三方合規性治理，要考慮的一個最大業務風險為遵守[法規合規性](./regulatory-compliance.md)的風險。 此風險通常無法補救，反之可能需要嚴格遵守。 開始原則檢閱之前，請務必了解您的第三方合規性需求。

## <a name="an-incremental-approach-to-cloud-governance"></a>雲端治理的漸進式方法

雲端治理的漸進方法假設無法接受超過 [企業風險的承受度](./risk-tolerance.md)。 相反地，它假設治理的角色是要加速業務變更、協助工程師了解架構指導方針，並確保定期交流及補救[業務風險](./business-risk.md)。 另一方面，治理的傳統角色可能會變成工程師或業務整體在採用上的障礙。

若使用雲端治理的漸進方法，建置新解決方案的小組和為企業防範業務風險的小組之間可能會有一些自然的摩擦。 這兩個小組在此模型中可能變成以漸進或短期衝刺方式合作的同事。 作為同事，雲端治理小組和雲端採用小組開始合作，以公開、評估及補救業務風險。 此工作可以建立減少摩擦並在小組織之間建立合作關係的自然方法。

## <a name="minimum-viable-product-mvp-for-policy"></a>原則的最簡可行產品 (MVP)

您雲端治理小組和採用小組之間新合作關係的第一步是關於原則 MVP 的協議。 您的雲端治理 MVP 應認可一開始有較小的業務風險，但隨著組織採用更多雲端服務，業務風險可能會增長。

例如，某個企業部署的 5 部 VM 全不包含高業務影響性 (HBI) 資料，則其業務風險很小。 當雲端採用流程後續的 VM 數目達到 1,000 部，而該企業開始移動 HBI 資料時，業務風險會增長。

原則 MVP 會嘗試定義所需原則的基礎，以部署前 *x* 部 VM 或前 *x* 個應用程式，其中的 *x* 代表數字不大卻有意義的裝置採用數量。 此原則集需要少數限制，但會包含快速增長到下一個漸進雲端採用工作所需的基礎層面。 透過漸進原則發展，此治理策略會隨時間增長。 透過緩慢細微的移轉，原則 MVP 可能會增長為原則檢閱活動結果的功能同位。

## <a name="incremental-policy-growth"></a>漸進原則成長

漸進原則成長是隨時間增長原則和雲端治理的關鍵機制。 這也是採用累加式模型來進行治理的關鍵需求。 為了讓此模型正常運作，治理小組必須於每次短期衝刺致力於正在進行的時間配置，才能評估及實作變更治理專業領域。

**短期衝刺時間需求：** 在每個反覆項目開始時，每個雲端採用小組都建立要在目前漸進階段中遷移或採用之資產的清單。 雲端治理小組應該要有足夠的時間可檢閱清單、驗證資產的資料分類、評估與每個資產相關聯的任何新風險、更新架構指導方針，並針對變更為小組進行教育。 這些工作通常需要 10-30 小時 (每次短期衝刺)。 此參與層級也應該需要至少一個專職員工來管理大型雲端採用工作中的治理。

**發行時間需求：** 在每次發行開始時，雲端採用小組和雲端策略小組應優先處理要在目前反覆項目中移轉的一系列應用程式或工作負載，以及任何業務變更活動。 這些資料點可讓雲端治理小組提早了解業務風險。 這樣就有時間配合業務及量測業務的風險容忍。

## <a name="next-steps"></a>後續步驟

有效的雲端治理策略從了解業務風險開始。

> [!div class="nextstepaction"]
> [了解業務風險](./business-risk.md)
