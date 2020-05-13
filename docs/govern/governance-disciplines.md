---
title: 雲端治理的五個專業領域
description: 使用適用于 Azure 的雲端採用架構來瞭解成本管理、部署加速、身分識別基準、資源一致性和安全性基準。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: landing-page
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
layout: LandingPage
ms.openlocfilehash: c1c54d7587bf73f5c9348c878caea7ae7cde89ae
ms.sourcegitcommit: 60d8b863d431b5d7c005f2f14488620b6c4c49be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83220187"
---
# <a name="the-five-disciplines-of-cloud-governance"></a>雲端治理的五個專業領域

<!-- docsTest:disable TODO -->
<!-- markdownlint-disable MD033 -->

<ul class="panelContent cardsI">
    <li style="display: flex; flex-direction: column;">
        <div class="cardSize">
            <div class="cardPadding" style="padding-bottom:10px;">
                <div class="card" style="padding-bottom:10px;">
                    <div class="cardText" style="padding-left:0px;">
對業務程序或技術平台進行的任何變更都會引進風險。 雲端治理小組（其成員有時也稱為雲端保管人）負責降低這些風險，並確保採用或創新工作的中斷程度降至最低。
    <br>
    <br>
雲端採用架構治理模型會著重于<a href="./corporate-policy.md">公司原則的開發</a>和<a href="#disciplines-of-cloud-governance">雲端治理的五個專業領域</a>，以引導這些決策（不論選擇的雲端平臺為何）。 <a href="./guides/index.md">可操作的設計指南</a>使用 Azure 服務示範此模型。 深入瞭解雲端採用架構治理模型的專業領域。
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li style="display: flex; flex-direction: column;">
        <a href="../_images/operational-transformation-govern-highres.png" style="display: flex; flex-direction: column; flex: 1 0 auto;">
            <div class="cardSize">
                <div class="cardPadding" style="padding-bottom:10px;">
                    <div class="card" style="padding-bottom:10px;">
                        <div class="cardText" style="padding-left:0px;">
    <img src="../_images/operational-transformation-govern-highres.png" alt="Diagram of the Cloud Adoption Framework governance model: Corporate policy and governance disciplines">
    <br>
    <i>圖1：公司原則的圖表和雲端治理的五個專業領域。</i>
                        </div>
                    </div>
                </div>
            </div>
        </a>
    </li>
</ul>

<!-- markdownlint-enable MD033 -->

## <a name="disciplines-of-cloud-governance"></a>雲端治理的專業領域

在任何雲端平臺中，都有共同的治理專業領域，可協助通知原則並使工具鏈一致。 這些專業領域會引導您在雲端平臺上適當層級的自動化和強制執行公司原則的決策。

<!-- markdownlint-disable MD033 -->

<ul class="panelContent cardsA">
<li style="display: flex; flex-direction: column;">
    <a href="./cost-management/index.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../_images/govern/cost-management.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>成本管理</h3>
                        <p>成本是雲端使用者的主要考量。 開發適用於所有雲端平台的成本控制原則。</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./security-baseline/index.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../_images/govern/security-baseline.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>安全性基準</h3>
                        <p>安全性是一個複雜的主旨，對於每一家公司都是獨一無二的。 一旦建立安全性需求之後，雲端治理原則和強制會將這些需求套用到網路、資料和資產設定。</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./identity-baseline/index.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../_images/govern/identity-baseline.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>身分識別基準</h3>
                        <p>身分識別需求套用中的不一致可能會增加遭入侵的風險。 身分識別基準專業領域著重于確保身分識別一致地套用到雲端採用工作。</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./resource-consistency/index.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../_images/govern/resource-consistency.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>資源一致性</h3>
                        <p>雲端作業取決於一致的資源設定。 透過治理工具，可以一致地設定資源，以管理與上線、漂移、發現和復原相關的風險。</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./deployment-acceleration/index.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../_images/govern/deployment-acceleration.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>部署加速</h3>
                        <p>部署和設定方法中的集中化、標準化和一致性可改善治理實務。 透過雲端式治理工具提供時，他們會建立可加速部署活動的雲端因素。</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

<!-- markdownlint-enable MD033 -->
