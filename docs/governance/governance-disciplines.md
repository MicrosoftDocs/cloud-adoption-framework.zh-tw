---
title: 雲端治理的五個專業領域
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 瞭解雲端採用架構中的五個雲端治理專業領域。
author: BrianBlanchard
ms.author: brblanch
ms.date: 02/11/2019
ms.topic: landing-page
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
layout: LandingPage
ms.openlocfilehash: 605cc610ff73ab3556f15619f5a47532e38fa7f0
ms.sourcegitcommit: a26c27ed72ac89198231ec4b11917a20d03bd222
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70824791"
---
# <a name="the-five-disciplines-of-cloud-governance"></a>雲端治理的五個專業領域

<!-- markdownlint-disable MD033 -->

<ul class="panelContent cardsI">
    <li style="display: flex; flex-direction: column;">
        <div class="cardSize">
            <div class="cardPadding" style="padding-bottom:10px;">
                <div class="card" style="padding-bottom:10px;">
                    <div class="cardText" style="padding-left:0px;">
對業務程序或技術平台進行的任何變更都會引進風險。 雲端治理小組（其成員有時也稱為雲端保管人）負責降低這些風險，並確保採用或創新工作的中斷程度降至最低。<br/><br/>雲端採用架構治理模型會著重于<a href="./corporate-policy.md">公司原則的開發</a>和<a href="#disciplines-of-cloud-governance">雲端治理的五個專業領域</a>，以引導這些決策（不論選擇的雲端平臺為何）。 <a href="./journeys/index.md">可操作的設計指南</a>使用 Azure 服務示範此模型。 深入瞭解雲端採用架構治理模型的專業領域。
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
    <i>圖 1-公司原則和雲端治理的五個專業領域的圖表。</i>
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
                            <img src="../_images/governance/cost-management.png" class="x-hidden-focus"/>
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
                            <img src="../_images/governance/security-baseline.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>安全性基準</h3>
                        <p>安全性是一個複雜的主題，每一家公司都是獨一無二的。 一旦建立安全性需求之後，雲端治理原則和強制會將這些需求套用到網路、資料和資產設定。</p>
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
                            <img src="../_images/governance/identity-baseline.png" class="x-hidden-focus"/>
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
                            <img src="../_images/governance/resource-consistency.png" class="x-hidden-focus"/>
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
                            <img src="../_images/governance/deployment-acceleration.png" class="x-hidden-focus"/>
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
