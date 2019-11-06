---
title: 雲端移轉
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 雲端採用架構中的雲端移轉
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: landing-page
ms.service: cloud-adoption-framework
ms.subservice: migrate
layout: LandingPage
ms.openlocfilehash: a6e7cb52fdac7607765b32c5355c0f2df066538a
ms.sourcegitcommit: bf9be7f2fe4851d83cdf3e083c7c25bd7e144c20
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/04/2019
ms.locfileid: "73566868"
---
# <a name="cloud-migration-in-the-cloud-adoption-framework"></a>雲端採用架構中的雲端移轉

任何企業級規模的[雲端採用方案](../plan/index.md)，都會包含不保證在建立新商業邏輯方面進行大量投資的工作負載。 這些工作負載可以透過任意數目的方法移至雲端；原形移轉、原形最佳化或現代化。 這些方法各自都是一種移轉。 下列練習將協助建立反覆流程，來評估、移轉、最佳化、保護及管理這些工作負載。

## <a name="getting-started"></a>開始使用

為了準備此階段的雲端採用生命週期，架構建議下列五個練習：

<!-- markdownlint-disable MD033 -->
<ul class="panelContent cardsF">
    <li style="display: flex; flex-direction: column;">
        <a href="./azure-migration-guide/prerequisites.md?tabs=Checklist">
            <div class="cardSize">
                <div class="cardPadding" style="padding-bottom:10px;">
                    <div class="card" style="padding-bottom:10px;">
                        <div class="cardImageOuter">
                            <div class="cardImage">
                                <img alt="" src="../_images/icons/1.png" data-linktype="external">
                            </div>
                        </div>
                        <div class="cardText" style="padding-left:0px;">
                            <h3>移轉必要條件</h3>
驗證登陸區域是否已完成部署，並已準備好裝載將移轉至 Azure 的前幾個工作負載。 如果尚未建立雲端採用策略和雲端採用方案，請驗證這兩項工作是否正在進行中。
                        </div>
                    </div>
                </div>
            </div>
        </a>
    </li>
    <li style="display: flex; flex-direction: column;">
        <a href="./azure-migration-guide/index.md">
            <div class="cardSize">
                <div class="cardPadding" style="padding-bottom:10px;">
                    <div class="card" style="padding-bottom:10px;">
                        <div class="cardImageOuter">
                            <div class="cardImage">
                                <img alt="" src="../_images/icons/2.png" data-linktype="external">
                            </div>
                        </div>
                        <div class="cardText" style="padding-left:0px;">
                            <h3>移轉您的第一個工作負載</h3>
請運用 Azure 移轉指南，引導您移轉第一個工作負載。 這將協助您熟悉調整採用工作所需的工具和方法。
                        </div>
                    </div>
                </div>
            </div>
        </a>
    </li>
    <li style="display: flex; flex-direction: column;">
        <a href="./expanded-scope/index.md">
            <div class="cardSize">
                <div class="cardPadding" style="padding-bottom:10px;">
                    <div class="card" style="padding-bottom:10px;">
                        <div class="cardImageOuter">
                            <div class="cardImage">
                                <img alt="" src="../_images/icons/3.png" data-linktype="external">
                            </div>
                        </div>
                        <div class="cardText" style="padding-left:0px;">
                            <h3>展開的移轉情節</h3>
請運用展開的範圍檢查清單，以找出需要修改未來狀態架構、移轉流程、登陸區域設定或移轉工具決策的情節。
                        </div>
                    </div>
                </div>
            </div>
        </a>
    </li>
    <li style="display: flex; flex-direction: column;">
        <a href="./azure-best-practices/index.md">
            <div class="cardSize">
                <div class="cardPadding" style="padding-bottom:10px;">
                    <div class="card" style="padding-bottom:10px;">
                        <div class="cardImageOuter">
                            <div class="cardImage">
                                <img alt="" src="../_images/icons/4.png" data-linktype="external">
                            </div>
                        </div>
                        <div class="cardText" style="padding-left:0px;">
                            <h3>最佳做法</h3>
驗證對最佳做法一節所做的任何修改，以確保適當地實作展開的範圍或工作負載/架構特定移轉方法。
                        </div>
                    </div>
                </div>
            </div>
        </a>
    </li>
    <li style="display: flex; flex-direction: column;">
        <a href="./migration-considerations/index.md">
            <div class="cardSize">
                <div class="cardPadding" style="padding-bottom:10px;">
                    <div class="card" style="padding-bottom:10px;">
                        <div class="cardImageOuter">
                            <div class="cardImage">
                                <img alt="" src="../_images/icons/5.png" data-linktype="external">
                            </div>
                        </div>
                        <div class="cardText" style="padding-left:0px;">
                            <h3>流程改善</h3>
移轉是大量使用流程的活動。 當移轉工作擴展時，請使用移轉考量一節，來評估和完善流程的各個層面。
                        </div>
                    </div>
                </div>
            </div>
        </a>
    </li>
</ul>
<!-- markdownlint-enable MD033 -->

## <a name="iterative-migration-process"></a>反覆的移轉流程

基本上，移轉至雲端包含四個簡單階段：評估、移轉、最佳化，以及保護和管理。 雲端採用架構的這一節會教導讀者將流程每個階段的回報發揮到極致，並使這些階段與您的雲端採用方案保持一致。 下圖描述反覆方法中的那些階段：

![雲端採用架構移轉模型](../_images/operational-transformation-migrate.png)

## <a name="create-a-balanced-cloud-portfolio"></a>建立平衡的雲端組合

任何平衡技術組合都有由各種資產組成的混合資產。 某些應用程式已排程淘汰且提供最低限度支援。 其他應用程式或資產的支援處於維護狀態，但那些解決方案的功能是穩定的。 針對較新的商業流程，變更市場條件可能會刺激進行中的功能改進或現代化。 當推動營收來源的機會出現時，就會將新應用程式或資產引進生產環境中。 在資產的每個生命週期階段中，任何投資對營收和利潤的影響都會變更。 越接近生命週期階段後期，新功能或現代化工作可獲得高投資報酬的可能性就越小。

雲端提供各種採用機制，每個都有類似程度的投資和報酬。 建置雲端原生應用程式可大幅降低營運費用。 雲端原生應用程式發行之後，新功能和解決方案開發可以更快速反覆進行。 藉由移除內部部署開發模型相關的舊限制，應用程式現代化也會產生類似優點。 遺憾的是，這兩種方法相當耗費人力，而且取決於軟體開發團隊的大小、技能與經驗。 人力通常是不當配置&mdash;擁有將應用程式現代化之技能和才能的人員寧願建置新應用程式。 在人力有限的市場中，大型現代化專案可能因員工滿意度和才能問題而受阻礙。 在平衡組合中，應將此方法留給維持在內部部署能獲得大幅功能改進的應用程式。

## <a name="envision-an-end-state"></a>構想最終狀態

有效的過程需要目標目的地。 採取第一個步驟之前，請建立最終狀態的粗略願景。 此資訊圖概述由應用程式、資料和基礎結構所組成，定義數位資產的起點。 在移轉程序中，每個資產都透過右邊其中一個選項來轉換。

## <a name="migration-implementation"></a>移轉實作

這些文章概述兩個過程，每個都有類似的目標&mdash;將現有資產的一大部分遷移至 Azure。 不過，業務成果和目前狀態對達成目標所需的程序會有重大影響。 那些細微差異導致兩個完全不同的方法達到類似最終狀態。

![雲端採用架構移轉模型](../_images/operational-transformation-migrate.png)

為了在轉換至最終狀態期間引導累加執行，此模型將移轉分為兩個焦點區域。

**移轉準備：** 建立以目前狀態和所要成果為主的粗略移轉待辦項目。

- **業務成果：** 驅動此移轉的關鍵業務目標。
- **數位資產估計：** 要移轉之工作負載的數目和狀況的粗略估計。
- **角色和職責：** 小組結構、職責區分和存取需求的清楚定義。
- **變更管理需求：** 檢閱和核准變更所需的頻率、程序和文件。

這些初始輸入項目讓移轉待辦項目成形。 移轉待辦項目的輸出項目是要遷移到雲端之應用程式的優先順序清單。 該清單讓雲端移轉程序的執行成形。 經過一段時間之後，它也會增長為包含管理變更所需的大部分文件。

**移轉程序：** 每個雲端移轉活動都包含在下列程序的其中一個中，因為它與移轉待辦項目相關。

- **評定：** 評估現有資產並建立資產移轉計劃。
- **遷移：** 在雲端中複寫資產的功能。
- **最佳化：** 平衡雲端資產的效能、成本、存取和運作容量。
- **保護及管理：** 確定雲端資產已針對進行中的營運做好準備。

在建立移轉待辦項目期間收集的資訊，會決定雲端移轉程序期間，每次反覆和每次推出功能的複雜度和所需投入量層級。

## <a name="transition-to-the-end-state"></a>轉換至最終狀態

目標是順暢且部分自動化移轉至雲端。 移轉程序使用雲端廠商提供的工具，來將資產快速地複寫至雲端並暫存。 驗證之後，簡單的網路變更會將使用者路由傳送至雲端解決方案。 對於許多使用案例來說，達成此目標的技術很容易取得。 有示範在 Azure 中複寫 10,000 部 VM 之速度的範例案例。

不過，仍然需要累加移轉方法。 在大部分環境中，要遷移之 VM 的長清單必須分解成較小工作單元，移轉才能成功。 有需多因素都會限制給定期間內可遷移之 VM 數目。 輸出網路速度是少數技術限制之一，大部分的限制都會對企業驗證和適應變更的能力加諸限制。

雲端採用架構的累加移轉方法可協助您建置累加方案，它會反映和記錄技術與文化限制。 此模型的目標是要最大化移轉速度，同時將來自 IT 和企業的的負擔降至最低。 以下提供兩個以移轉待辦項目為基礎的累加移轉範例。

<!-- markdownlint-disable MD033 -->

<ul class="panelContent cardsZ">
<li style="display: flex; flex-direction: column;">
    <a href="./azure-migration-guide/index.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>Azure 移轉指南</h3>
                        <p><b>摘要記述：</b>此客戶正在遷移的 VM 少於 1,000 部。 少於十個支援的應用程式是由 IT 組織外的應用程式擁有者擁有。 剩下的應用程式、VM 和相關資料都是由雲端採用小組的成員擁有和支援。 雲端採用小組成員有現有資料中心內生產環境的系統管理存取權。</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./expanded-scope/index.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>複雜案例指南</h3>
                        <p><b>摘要記述：</b>此客戶的移轉之複雜度橫跨業務、文化和技術。 本指南包含多個特定複雜度挑戰和克服這些挑戰的方式。</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

<!-- markdownlint-enable MD033 -->

這兩個過程代表了投資雲端移轉之客戶體驗的兩個極端。 大部分公司表現出上述兩個案例的組合。 檢閱過程之後，請使用雲端採用架構移轉模型展開移轉對話，並修改基準過程以更加貼近您的需求。

## <a name="next-steps"></a>後續步驟

選擇其中一個旅程：

> [!div class="nextstepaction"]
> [Azure 移轉指南](./azure-migration-guide/index.md)
>
> [擴充範圍指南](./expanded-scope/index.md)
