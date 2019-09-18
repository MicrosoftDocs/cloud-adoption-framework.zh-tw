---
title: 部署加速專業領域概觀
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 說明與雲端治理相關的部署加速。
author: BrianBlanchard
ms.author: brblanch
ms.date: 02/11/2019
ms.topic: landing-page
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
layout: LandingPage
ms.openlocfilehash: 55dc7554e72f5ca1e2a19a29cf93f8b075b93c9b
ms.sourcegitcommit: 443c28f3afeedfbfe8b9980875a54afdbebd83a8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2019
ms.locfileid: "71025944"
---
# <a name="deployment-acceleration-discipline-overview"></a>部署加速專業領域概觀

部署加速是[雲端採用架構治理模型](../index.md)中的[五個雲端治理專業領域](../governance-disciplines.md)之一。 這個專業領域著重於建立原則來治理資產設定或部署的方式。 在這五個雲端治理專業領域中，部署加速包括部署、使設定保持一致，以及指令碼的重複使用性。 這可能會透過手動活動或完全自動化的 DevOps 活動。 在任一種情況下，原則基本上會保持不變。 隨著這個專業領域逐漸成熟，雲端治理小組可利用可重複使用的資產來加速部署及移除雲端採用的障礙，而成為 DevOps 和部署策略中的合作夥伴。

本文將概述公司在實作雲端解決方案的規劃、建置、採用及操作階段期間所體驗的部署加速流程。 沒有任何一份文件能夠滿足任一企業的所有需求。 因此，本文的每一節都將概述建議的最低和潛在活動。 這些活動的目的是要協助您建置[原則 MVP](../policy-compliance/index.md#minimum-viable-product-mvp-for-policy)，但也會建立[累加式原則](../policy-compliance/index.md#incremental-policy-growth)的改良架構。 雲端治理小組應決定要對這些活動進行多少投資，以改進部署加速效果。

> [!NOTE]
> 部署加速專業領域不會取代可讓貴組織有效部署和設定雲端式資源的現有 IT 小組、流程和程序。 此專業領域的主要目的是識別潛在的業務風險，並且為負責管理雲端資源的 IT 人員提供風險降低指引。 當您開發治理原則和流程時，請務必在規劃與檢閱流程中包含相關的 IT 小組。

本指引之主要適用對象為組織的雲端架構設計師和雲端治理小組的其他成員。 不過，從此專業領域衍生的決策、原則和流程，應會涉及與您的業務和 IT 小組相關成員進行互動與討論，特別是那些負責部署和設定雲端式工作負載的領導人。

## <a name="policy-statements"></a>Policy statements

可操作的原則聲明和所產生的架構需求，均可作為部署加速專業領域的基礎。 若要查看原則聲明範例，請參閱關於[部署加速原則聲明](./policy-statements.md)的文章。 這些範例可作為貴組織治理原則的起點。

> [!CAUTION]
> 範例原則來自於常見的客戶體驗。 若要進一步使這些原則與特定雲端治理需求保持一致，請執行下列步驟，以建立符合您獨特商務需求的原則聲明。

## <a name="developing-deployment-acceleration-governance-policy-statements"></a>開發部署加速治理原則聲明

下列六個步驟將可協助您定義治理原則，以便在雲端環境中控制資源的部署與設定。

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
                        <h3>部署加速範本</h3>
                        <p class="x-hidden-focus">下載用以記載部署加速專業領域的範本</p>
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
                        <h3>業務風險</h3>
                        <p class="x-hidden-focus">了解通常與部署加速專業領域相關聯的動機與風險。</p>
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
                        <p class="x-hidden-focus">了解其是否為投資部署加速專業領域之正確時機的指標。</p>
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
                        <p class="x-hidden-focus">用以在部署加速專業領域中支援原則合規性的建議流程。</p>
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
                        <p class="x-hidden-focus">可實作來支援部署加速專業領域的 Azure 服務。</p>
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

<!-- markdownlint-enable MD033 -->
