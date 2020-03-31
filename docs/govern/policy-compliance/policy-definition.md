---
title: 定義雲端治理公司原則
description: 使用適用于 Azure 的雲端採用架構，學習如何建立可解決雲端轉換旅程期間已知風險和風險承受度的原則。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.openlocfilehash: 6c3ed91d77320f8c72b0a184dc2aa7054c0089d5
ms.sourcegitcommit: afe10f97fc0e0402a881fdfa55dadebd3aca75ab
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/31/2020
ms.locfileid: "80434971"
---
# <a name="define-corporate-policy-for-cloud-governance"></a>定義雲端治理的公司原則

一旦您為組織的雲端轉型旅程分析已知風險和相關風險承受度，下一步就是建立可明確解決這些風險的原則，並定義在可能的情況下補救它們所需的步驟。

<!-- markdownlint-disable MD026 -->

## <a name="how-can-corporate-it-policy-become-cloud-ready"></a>公司 IT 原則如何成為雲端就緒？

在傳統治理和雲端治理中，公司原則建立作用的治理定義。 大部分的「IT 治理」動作的目的是要實作技術，以監視、強制執行、操作那些公司原則並自動化。 雲端治理建立在類似的概念上。

![公司治理和治理專業領域](../../_images/operational-transformation-govern-highres.png)

*圖1 - 公司治理和治理專業領域。*

上圖說明商務風險、原則與合規性，以及監視和強制執行機制之間的關聯性，這兩者必須在治理策略中互動。 雲端治理的五個專業領域可讓您管理這些互動，並實現您的策略。

雲端治理是在一段時間內持續進行採用工作的成果，因為轉換不會在一夕之間發生。 使用快速積極的方法，嘗試在解決關鍵公司原則變更之前提供完整雲端治理，這樣很少會產生想要的結果。 相反地，我們建議漸進的方法。

雲端採用架構的不同之處在于購買週期，而且可以啟用真實的轉換。 因為不會有大量的資本支出取得需求，工程師可以更快開始進行實驗和採用。 在大部分公司文化中，消除採用的資本支出障礙可能導致回饋循環、有機成長及漸進執行變得緊迫。

雲端採用的移轉，需要是受治理的移轉。 在許多組織中，公司原則轉換透過漸進原則變更和自動化那些變更強制執行 (與雲端服務提供者一同定義的新能力)，而達到改善的治理和更高的遵守率。

<!-- markdownlint-enable MD026 -->

## <a name="review-existing-policies"></a>檢閱現有原則

由於治理是持續進行的程式，因此應定期向 IT 人員和專案關係人審查原則，以確保雲端中裝載的資源會繼續維持整體企業目標和需求的合規性。 您對於新風險和容忍程度的了解可記載成[現有原則評論](./cloud-policy-review.md)，以判斷適合您組織的必要治理層級。

> [!TIP]
> 如果您的組織使用廠商或其他受信任的商業夥伴，則需要考慮的其中一個最大業務風險可能是缺乏這些外部組織的[法規合規性](./regulatory-compliance.md)。 這項風險通常無法補救，而是可能需要嚴格遵循所有合作物件的需求。 開始進行原則審查之前，請確定您已識別並瞭解任何協力廠商合規性需求。

## <a name="create-cloud-policy-statements"></a>建立雲端原則陳述式

雲端型 IT 原則可確定 IT 人員和自動化系統必須支援的需求、標準和目標。 原則決策是您的[雲端架構設計](./governance-alignment.md)中的主要因素，以及您要如何實作[原則遵循流程](./processes.md)。

個別雲端原則聲明是解決在風險評估流程中識別出的特定風險方針。 雖然這些原則可以整合到您更廣泛的公司原則檔中，但整個雲端採用架構指引中所討論的雲端原則聲明，通常是更精確的風險摘要和計畫來處理它們。 每個定義定義都應該包含下列資訊：

- **業務風險：** 此原則將解決的風險摘要。
- **原則聲明：** 原則需求和目標的簡短說明。
- **設計或技術指導：** 可操作的建議、規格或其他指引，以支援和強制執行此原則，讓 IT 小組和開發人員可以在設計和建立其雲端部署時使用。

如果您需要協助開始定義原則，請參閱治理章節概觀中介紹的[治理專業領域](../governance-disciplines.md)。 這些專業領域的文章包含移至雲端時所遇到的常見商業風險範例，以及用來補救這些風險的範例原則（例如，請參閱成本管理專業領域的[範例原則定義](../cost-management/policy-statements.md)）。

## <a name="incremental-governance-and-integrating-with-existing-policy"></a>增量治理並與現有原則整合

對雲端環境的計劃新增項目應一律進行審核，以確保其符合現有原則，並更新原則以解決未涵蓋的任何問題。 您也應該定期執行[雲端原則檢閱](./cloud-policy-review.md)，以確保您的雲端原則已更新，並與任何新的公司原則同步處理。

將雲端原則與傳統 IT 原則整合的需求，主要取決於雲端治理程序的成熟度和雲端資產的規模。 如需在雲端轉換期間處理原則整合的更廣泛的討論，請參閱有關[增量治理和原則 MVP](./index.md) 一文。

## <a name="next-steps"></a>後續步驟

定義原則之後，草擬架構設計指南，為 IT 人員和開發人員提供可採取動作的指導方針。

> [!div class="nextstepaction"]
> [配合您的治理設計指南與公司原則](./governance-alignment.md)
