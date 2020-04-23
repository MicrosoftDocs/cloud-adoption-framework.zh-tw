---
title: 雲端治理指南
description: 檢閱雲端治理指南，這些指南說明了針對任何治理案例累加式方法的最佳做法。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/05/2019
ms.topic: landing-page
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
layout: LandingPage
ms.openlocfilehash: ff7b8669d71f72b87bbfcc3377dc5bf439311987
ms.sourcegitcommit: 7d3fc1e407cd18c4fc7c4964a77885907a9b85c0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "80995272"
---
# <a name="cloud-governance-guides"></a>雲端治理指南

本節中可採取動作的治理指南，根據先前描述的[治理方法](../methodology.md)，說明了雲端採用架構治理模型的漸進式方法。 您可以建立一個敏捷式雲端治理方法，以滿足任何雲端治理案例的需求。

## <a name="review-and-adopt-cloud-governance-best-practices"></a>檢閱並採用雲端治理最佳作法

若要開始進行雲端採用旅程，請選擇下列其中一個治理指南。 每個指南分別根據一組的虛構客戶體驗，概述了一系列最佳做法。 讀者若不熟悉雲端採用架構治理模型的漸進式方法，請先檢閱以下高階治理理論簡介，然後再採用其中一組最佳做法。

<!-- markdownlint-disable MD033 -->

<ul class="panelContent cardsZ">
<li style="display: flex; flex-direction: column;">
    <a href="./standard/index.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>標準治理指南</h3>
                        <p>此指南適用於使用建議的兩個訂用帳戶模型的大部分組織，設計訴求為在多個區域中部署，但不橫跨公用和主權/政府雲端。</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./complex/index.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>適用於複雜企業的治理指南</h3>
                        <p>適用於由多個獨立 IT 業務單位或跨公用和主權/政府雲端管理的企業指南。</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

<!-- markdownlint-enable MD033 -->

## <a name="an-incremental-approach-to-cloud-governance"></a>雲端治理的漸進式方法

## <a name="choose-a-governance-guide"></a>選擇治理指南

此指南示範如何實作治理 MVP。 從該處開始，每個指南都顯示了雲端治理小組如何與雲端採用小組合作，為其完成前置作業，以加速進行採用工作。 從基礎到後續改良和演進，雲端採用架構治理模型都會引導治理的應用。

若要開始治理旅程，請選擇下列兩個選項其中之一。 這些選項是依據綜合的客戶體驗。 標題會依據企業複雜度制定，以便於瀏覽。 不過，讀者的決定可能更加複雜。 下表概述了這兩個選項之間的差異。

> [!WARNING]
> 您可能需要更強固的治理起點。 在這種情況下，請考慮[下面](#azure-virtual-datacenter)所簡短描述的 [Azure 虛擬資料中心](#azure-virtual-datacenter)方法。 在進行企業規模的採用工作期間，特別是超過 10,000 個資產的工作，我們通常會建議您使用此方法。 此方法也是有下列任何需要的複雜治理案例所存在的既定選擇：廣泛的第三方合規性需求、深度網域專業知識或與成熟 IT 治理原則和合規性保持對應的需求。

<!-- markdownlint-disable MD028 -->

> [!NOTE]
> 每個指南都不太可能完全符合您的狀況。 請選擇最接近您的狀況的指南，並用它當作起點。 在整個指南中，會提供額外資訊來協助您自訂決策，以符合特定準則。

### <a name="business-characteristics"></a>商務特性

| 特性 | 標準組織 | 複雜企業 |
|---|---|---|
| 地理位置 (國家或地緣政治區域) | 客戶或員工主要位於一個地理位置 | 客戶或員工位於多個地理位置或需要主權雲端。 |
| 受影響的營業單位 | 共用一般 IT 基礎結構的業務單位 | 未共用一般 IT 基礎結構的業務單位 |
| IT 預算 | 單一 IT 預算 | 以不同的貨幣在多個營業單位間分配預算 |
| IT 投資 | 資本支出導向的投資是每年計劃一次，而且通常僅涵蓋基本維護。 | 資本支出導向的投資是每年計劃一次，而且通常包含三到五年的維護和更新週期。 |

### <a name="current-state-before-adopting-cloud-governance"></a>採用雲端治理之前的目前狀態

| State | 標準企業 | 複雜企業 |
|---|---|---|
| 資料中心或協力廠商主機服務提供者 | 少於五個資料中心 | 超過五個資料中心 |
| 網路功能 | 無 WAN，或是 1 &ndash; 2 個 WAN 提供者 | 複雜網路或全域 WAN |
| 身分識別 | 單一樹系、單一網域。 | 複雜、多個樹系、多個網域。 |

### <a name="desired-future-state-after-incremental-improvement-of-cloud-governance"></a>雲端治理累加式改進後所需的未來狀態

| State | 標準組織 | 複雜企業 |
|---|---|---|
| 成本管理 – 雲端帳戶處理 | 回報模型。 透過 IT 集中計費。 | 退款模型。 可透過 IT 採購來散發計費。 |
| 安全性基準 – 受保護的資料 | 公司財務資料和 IP。 有限的客戶資料。 沒有協力廠商合規性需求。 | 客戶的財務和個人資料有多個集合。 可能必須考慮協力廠商合規性。 |

## <a name="azure-virtual-datacenter"></a>Azure 虛擬資料中心

Azure 虛擬資料中心能充分發揮 Azure 雲端平台的功能，同時還能遵循企業的安全性與治理需求。

相較於傳統內部部署環境，Azure 可讓工作負載開發團隊和其業務贊助者善用雲端平台所提供的更佳部署靈活度。 不過，當雲端採用工作擴大而要納入關鍵任務資料和工作負載時，此靈活度可能會與 IT 團隊所建立的公司安全性和原則合規性需求衝突。 已有複雜治理和法規需求的大型企業尤其會如此。

Azure 虛擬資料中心方法的目標是要在採用生命週期中及早解決這些顧慮，其方法是提供模型、參考架構、自動化成品範例和指引，來協助讓開發人員和 IT 治理需求在進行企業雲端採用工作期間能夠取得平衡。 這種方法的核心是虛擬資料中心本身的概念：透過運用存取和安全性控制、網路原則和合規性監視，在雲端基礎結構周圍實作隔離邊界。

您可以將虛擬資料中心視為您自己在 Azure 平台內的隔離雲端，會整合管理程序、法規需求和治理原則所需的安全性程序。 在此虛擬邊界內，Azure 虛擬資料中心會在確保一致合規性的同時提供用於部署工作負載的模型範例，並提供基本指導來讓您了解如何在雲端中為組織實作角色和職責的隔離。

### <a name="azure-virtual-datacenter-assumptions"></a>Azure 虛擬資料中心假設

雖然小一點的團隊也能受益於 Azure 虛擬資料中心所提供的模型和建議，但這種方法的設計目的是要對管理大型雲端環境的企業 IT 群組提供指導。 對於符合下列準則的組織，建議您在設計 Azure 型雲端基礎結構時，可以考慮參閱 Azure 虛擬資料中心的指引：

- 貴企業受到法規合規性需求規範，而需要集中的監視和稽核功能。
- 您需要維護一般原則和治理合規性，以及對核心服務的集中 IT 控制。
- 您的行業依賴復雜的平台，需要復雜的控制項和深入網域的專業知識來管理平台。 這在金融、石油和天然氣或製造業的大型企業中最為常見。
- 現有的 IT 治理原則需要與現有功能更加緊密地保持對應，即使在早期階段採用期間也是如此。

如需詳細資訊，請造訪雲端採用架構的 [Azure 虛擬資料中心](../../reference/vdc.md)一節。

## <a name="next-steps"></a>後續步驟

請選擇其中一個指南：

> [!div class="nextstepaction"]
> [標準企業治理指南](./standard/index.md)
>
> [適用於複雜企業的治理指南](./complex/index.md)
