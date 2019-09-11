---
title: 建立初始雲端治理基礎
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 藉由建立初始雲端治理基礎來開始使用雲端治理。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/05/2019
ms.topic: landing-page
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
layout: LandingPage
ms.openlocfilehash: f12ef1603ae0749aa893f0631481ec30ed881379
ms.sourcegitcommit: 5846ed4d0bf1b6440f5e87bc34ef31ec8b40b338
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/11/2019
ms.locfileid: "70906061"
---
# <a name="establish-an-initial-cloud-governance-foundation"></a>建立初始雲端治理基礎

建立雲端治理是很廣泛的反復工作。 在速度與控制之間達成有效的平衡是很困難的，尤其是在雲端採用的早期階段。 雲端採用架構中的治理指導方針有助於透過敏捷式的採用方式來提供平衡。

本文提供兩個建立治理初始基礎的選項。 其中一個選項可確保治理條件約束可以隨著採用計畫的實行進行調整和擴充，而且需求也會更清楚地定義。 根據預設，初始基礎會假設隔離和控制位置。 它也著重于資源組織，而不是資源管理。 這個輕量的起點稱為治理的_最小可行產品（MVP）_ 。 MVP 的目標是降低建立初始治理位置的障礙，然後讓解決方案的快速成熟解決各種有形風險。

## <a name="already-using-the-cloud-adoption-framework"></a>已在使用雲端採用架構

如果您已遵循雲端採用架構，可能已經部署了治理 MVP。 指引是任何操作模型的核心層面。 它存在於雲端採用週期的每個階段。 因此，[雲端採用架構](../index.md)會提供指引，將治理插入與[雲端採用方案](../plan/index.md)的執行相關的活動。 這項治理整合的其中一個範例是使用藍圖來部署一個或多個登陸區域，這些是[現成](../ready/index.md)的指導方針。 另一個範例是向[外擴充訂閱](../ready/considerations/scaling-subscriptions.md)的指引。 如果您已遵循上述任一建議，則下列 MVP 章節只會回顧現有的部署決策。 在快速審查之後，立即跳至[成熟的初始治理解決方案，並套用最佳作法的控制項](./best-practices.md)。

## <a name="establish-an-initial-governance-foundation"></a>建立初始的治理基礎

以下是兩個不同的初始治理基礎（也稱為治理 Mvp）範例，以將用於治理的音效基礎套用至新的或現有的部署。 選擇最符合您商務需求的 MVP 以開始使用：

<!-- markdownlint-disable MD033 -->

<ul class="panelContent cardsZ">
<li style="display: flex; flex-direction: column;">
    <a href="./journeys/standard-enterprise/index.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>標準治理指南</h3>
                        <p>根據建議的兩個訂用帳戶模型，針對在多個區域中的部署而設計，但不橫跨公用和主權/政府雲端的組織指南。</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./journeys/complex-enterprise/index.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>複雜企業的治理指南</h3>
                        <p>一份指南，適用于由多個獨立 IT 業務單位或跨公用和主權/政府雲端管理的企業。</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>
<!-- markdownlint-enable MD033 -->

## <a name="next-steps"></a>後續步驟

一旦有了治理基礎之後，請套用適當的建議來改善解決方案，並防範有形的風險。

> [!div class="nextstepaction"]
> [改善最初的治理基礎](./best-practices.md)
