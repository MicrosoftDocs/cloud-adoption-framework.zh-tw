---
title: 成本管理專業領域概觀
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 說明與雲端治理相關的成本管理
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: landing-page
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
layout: LandingPage
ms.openlocfilehash: ac6c3cb0a26cebf655a1161a3fd54197c795c283
ms.sourcegitcommit: bf9be7f2fe4851d83cdf3e083c7c25bd7e144c20
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/04/2019
ms.locfileid: "73566414"
---
# <a name="cost-management-discipline-overview"></a>成本管理專業領域概觀

成本管理是 [Cloud Adoption Framework 治理模型](../index.md)中的[五個雲端治理專業領域](../governance-disciplines.md)之一。 對於許多客戶而言，治理成本是採用雲端技術時的主要考量。 平衡效能需求、採用步調和雲端服務成本，頗具挑戰性。 這在實作雲端技術的主要業務轉換期間特別重要。 本節將概述開發成本管理專業領域作為雲端管理策略一部分的方法。

> [!NOTE]
> 成本管理治理不會取代現有的業務小組、帳戶處理實務，以及牽涉到您組織中對 IT 相關成本進行財務管理的程序。 此專業領域的主要目的是找出與 IT 支出有關的潛在雲端相關風險，並提供降低風險指導方針給負責部署和管理雲端資源的業務和 IT 小組。

本指引之主要適用對象為組織的雲端架構設計師和雲端治理小組的其他成員。 不過，從此專業領域衍生的決策、原則和流程，應會涉及與您的業務和 IT 小組相關成員進行互動與討論，特別是那些負責擁有和管理雲端式工作負載並支付費用的領導人。

## <a name="policy-statements"></a>Policy statements

可操作的原則聲明和所產生的架構需求，均可做為成本管理專業領域的基礎。 若要查看原則聲明範例，請參閱關於[成本管理原則聲明](./policy-statements.md)的文章。 這些範例可做為貴組織治理原則的起點。

> [!CAUTION]
> 範例原則來自於常見的客戶體驗。 若要進一步使這些原則與特定雲端治理需求保持一致，請執行下列步驟，以建立符合您獨特商務需求的原則聲明。

## <a name="develop-governance-policy-statements"></a>開發治理原則聲明

下列六個步驟將協助您定義治理原則來控制環境中的成本。

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
                        <h3>成本管理範本</h3>
                        <p class="x-hidden-focus">下載此範本以記載成本管理專業領域</p>
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
                        <p class="x-hidden-focus">了解通常會與成本管理專業領域相關聯的動機與風險。</p>
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
                        <p class="x-hidden-focus">了解其是否為投資成本管理專業領域之正確時機的指標。</p>
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
                        <p class="x-hidden-focus">用以在成本管理專業領域中支援原則合規性的建議流程。</p>
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
                        <p class="x-hidden-focus">可實作來支援成本管理專業領域的 Azure 服務。</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

## <a name="next-steps"></a>後續步驟

開始評估特定環境中的商務風險。

> [!div class="nextstepaction"]
> [了解商務風險](./business-risks.md)

<!-- markdownlint-enable MD033 -->
