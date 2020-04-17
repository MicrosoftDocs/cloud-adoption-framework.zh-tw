---
title: 資源一致性專業領域的概觀
description: 了解開發資源一致性專業領域作為雲端管理策略一部分的方法。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: landing-page
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
layout: LandingPage
ms.openlocfilehash: ab79e6e0d55c8b7928e53415920c7ed285b625d8
ms.sourcegitcommit: 7d3fc1e407cd18c4fc7c4964a77885907a9b85c0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "80997391"
---
# <a name="resource-consistency-discipline-overview"></a>資源一致性專業領域的概觀

資源一致性是[雲端採用架構治理模型](../governance-disciplines.md)中的[五個雲端治理專業領域](../index.md)之一。 這個專業領域著重於訂定與環境、應用程式或工作負載之作業管理相關的原則。 IT 作業小組通常會監控應用程式、工作負載和資產效能。 其也經常執行滿足擴展需求、補救效能服務等級協定 (SLA) 違規和藉由自動補救的方式主動避免效能 SLA 違規所需任務。 在雲端治理的五個專業領域中，資源一致性是可確保一致配置資源的專業領域，以便 IT 作業能夠找到這些資源；此外，這些資源包含在復原方案中，且可納入可重複的作業程序中。

> [!NOTE]
> 資源一致性治理不會取代可讓貴組織有效管理雲端式資源所用的現有 IT 小組、流程和程序。 此專業領域的主要目的是識別潛在商務風險，且為負責管理雲端資源的 IT 人員提供降低風險指引。 當您開發治理原則和流程時，請務必在規劃與檢閱流程中包含相關的 IT 小組。

雲端採用架構的這個部分概述如何在雲端治理策略中開展資源一致性專業領域。 本指引之主要適用對象為組織的雲端架構設計師和雲端治理小組的其他成員。 不過，從此專業領域衍生的決策、原則和流程，應包含與負責實作和管理貴組織資源一致性解決方案的 IT 小組相關成員進行互動與討論。

如果貴組織沒有資源一致性策略的內部專業知識，請考慮聘請外部顧問成為此專業領域的一部分。 此外，考慮納入 [Microsoft Consulting Services](https://www.microsoft.com/industry/services/consulting)、[Microsoft FastTrack](https://azure.microsoft.com/programs/azure-fasttrack) 雲端採用服務，或聘請其他外部雲端採用專家來討論如何最充分組織、追蹤和最佳化雲端式資產。

## <a name="policy-statements"></a>Policy statements

可採取動作的原則聲明和所產生架構需求，均可作為資源一致性專業領域的基礎。 若要查看原則聲明範例，請參閱關於[資源一致性原則聲明](./policy-statements.md)的文章。 這些範例可作為貴組織治理原則的起點。

> [!CAUTION]
> 範例原則來自於常見的客戶體驗。 若要進一步使這些原則與特定雲端治理需求保持一致，請執行下列步驟，以建立符合您獨特商務需求的原則聲明。

## <a name="develop-governance-policy-statements"></a>開發治理原則聲明

下列六個步驟提供在開發資源一致性治理時所要考量的範例和可能選項。 使用每個步驟作為起點，在雲端治理小組中討論，並與組織內受影響的業務和 IT 小組討論，藉以建立管理資源一致性風險所需的原則和程序。

<!-- markdownlint-disable MD033 -->

<ul class="panelContent cardsE">
<li style="display: flex; flex-direction: column;">
    <a href="./template.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/govern/process-template.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>資源一致性範本</h3>
                        <p class="x-hidden-focus">下載範本以便記載資源一致性專業領域</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li><li style="display: flex; flex-direction: column;">
    <a href="./business-risks.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/govern/process-risks.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>商務風險</h3>
                        <p class="x-hidden-focus">了解通常與資源一致性專業領域建立關聯的動機與風險。</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./metrics-tolerance.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/govern/process-metrics.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>指標和計量</h3>
                        <p class="x-hidden-focus">了解其是否為投資資源一致性專業領域之正確時機的指標。</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./compliance-processes.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/govern/process-enforce.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>原則遵循流程</h3>
                        <p class="x-hidden-focus">用以在資源一致性專業領域中支援原則合規性的建議流程。</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./discipline-improvement.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/govern/process-maturity.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>成熟度</h3>
                        <p class="x-hidden-focus">使雲端管理成熟度和雲端採用階段保持一致。</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./toolchain.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/govern/process-toolchain.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>工具鏈</h3>
                        <p class="x-hidden-focus">可實作來支援資源一致性專業領域的 Azure 服務。</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

## <a name="next-steps"></a>後續步驟

開始評估特定環境中的[商務風險](./business-risks.md)。

> [!div class="nextstepaction"]
> [了解業務風險](./business-risks.md)
