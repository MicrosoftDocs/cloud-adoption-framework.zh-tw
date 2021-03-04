---
title: 成熟的團隊結構
description: 使用這些常見的小組結構範例，找出最符合您的雲端採用作業需求的組織結構。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/18/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: organize
ms.custom: internal
ms.openlocfilehash: 332273c7e1400b850cfeea6a67cad431d41d9e52
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101789689"
---
# <a name="mature-team-structures"></a>成熟的團隊結構

每個雲端功能都是由某人在每個雲端採用工作期間提供。 這些指派和小組結構可以開發茁壯，也可以刻意設計來符合已定義的小組結構。

當採用需求成長時，就需要平衡和結構。 觀賞這段影片，瞭解組織成熟度各階段的一般團隊結構。

<!-- markdownlint-disable MD034 -->

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4wvTS]

<!-- markdownlint-enable MD034 -->

下圖和清單會根據一般成熟階段來概述這些結構。 您可以使用這些範例來尋找最符合您操作需求的組織結構。

![組織成熟度週期](../_images/ready/org-ready-maturity.png)

組織結構通常會透過以下所述的一般成熟度模型來移動：

1. [僅限雲端採用小組](#cloud-adoption-team-only)
2. [MVP 最佳作法](#best-practice-minimum-viable-product-mvp)
3. [中央 IT 小組](#central-it-team)
4. [策略性調整](#strategic-alignment)
5. [營運一致性](#operational-alignment)
6. [卓越的雲端中心 (CCoE) ](#cloud-center-of-excellence)

大部分的公司從 *雲端採用小組* 開始著手。 但是，我們建議您建立一個與 [MVP 最佳作法](#best-practice-minimum-viable-product-mvp) 結構更相近的組織結構。

## <a name="cloud-adoption-team-only"></a>僅限雲端採用小組

雲端採用小組是所有雲端採用工作的系統核心。 此小組會推動可採用的技術變更。 根據採用工作的目標，這個小組可能會包含各種不同範圍的團隊成員，這些成員會處理一組廣泛的技術和商務工作。

![僅限雲端採用小組](../_images/ready/org-ready-adoption-only.png)

針對小規模或初期採用的工作，這個團隊可能會與一個人一樣小。 在大規模或最晚的工作中，您通常會有數個雲端採用小組，每個小組都有大約六個工程師。 無論大小或工作為何，任何雲端採用小組的一致層面都能提供將解決方案上架到雲端的方法。 對於某些組織而言，這可能是足夠的組織結構。 [雲端採用小組](./cloud-adoption.md)文章提供雲端採用小組結構、組合和功能的更深入解析。

> [!WARNING]
>  (或多個雲端採用小組操作時， **只** 會將雲端採用小組操作) 視為反模式，應予以避免。 至少請考慮 [MVP 最佳做法](#best-practice-minimum-viable-product-mvp)。

## <a name="best-practice-minimum-viable-product-mvp"></a>最佳做法： MVP) 的最小可行產品 (

![最佳做法：採用小組和治理小組的最小可行產品組織來建立餘額](../_images/ready/org-ready-best-practice.png)

建議您讓兩個小組在雲端採用工作之間建立平衡。 這兩個小組會負責整個採用工作中的各種功能。

- **雲端採用小組：** 此小組負責技術解決方案、商務一致性、專案管理和採用解決方案的作業。
- **雲端治理小組：** 為了在雲端採用小組之間取得平衡，雲端治理小組致力於確保採用的解決方案具有卓越的能力。 雲端治理小組負責平臺成熟度、平臺操作、治理和自動化。

這項經證實的方法被視為 MVP，因為它可能不是持續性。 每個小組都有許多職，如「 [*負責任」、「負責任* 」、「諮詢」 (RACI) 圖表](./raci-alignment.md)中所述。

下列各節描述完整的已驗證組織結構，以及將適當結構與組織對齊的方法。

## <a name="central-it-team"></a>中央 IT 小組

![中央 IT 小組](../_images/ready/org-ready-central-it.png)

雲端治理小組會隨著採用規模的發展，與多個雲端採用小組的創新流程保持一致。 尤其是在具有繁重合規性、作業或安全性需求的環境中。 在這個階段，公司通常會將雲端責任轉移至現有的中央 IT 團隊。 如果該小組可以重新評估工具、程式和人員，以更妥善地支援雲端採用，則包括中央 IT 小組可以增加重要價值。 從營運、自動化、安全性和系統管理的主題專家，到將中央 IT 團隊現代化，都能推動有效的營運創新。

可惜的是，「中央 IT 小組」階段可能是組織成熟度的風險最高階段之一。 中央 IT 團隊必須在資料表中提供強大的成長思維。 如果小組將雲端視為成長和調整的機會，則它可以在整個過程中提供絕佳的價值。 但是，如果中央 IT 團隊將雲端採用主要視為對其現有模型的威脅，則中央 IT 小組會成為雲端採用小組的障礙，以及支援的商務目標。 某些中央 IT 團隊花了數個月甚至幾年的時間，嘗試強制雲端與內部部署方法保持一致，只會產生負面的結果。 雲端不需要在中央 IT 小組內進行任何變更，但需要進行重大變更。 如果變更的阻力在中央 IT 團隊內普遍存在，這個成熟度階段可能很快就成為文化反模式。

雲端採用方案大多著重于平臺即服務 (PaaS) 、DevOps 或其他需要較少作業支援的解決方案，比較不可能在此成熟度階段中看到價值。 相反地，這些類型的解決方案最有可能被嘗試集中進行妨礙運作或封鎖。 較高層級的成熟度（像是 [卓越 (CCoE) 的雲端中心 ](#cloud-center-of-excellence)）比較有可能為這些類型的轉換工作產生正面的結果。 若要瞭解在雲端集中式 IT 與 CCoE 之間的差異，請參閱 [卓越的雲端中心](./cloud-center-of-excellence.md)。

## <a name="strategic-alignment"></a>策略性調整

![策略性調整](../_images/ready/org-ready-strategy-aligned.png)

隨著雲端採用的投資成長，以及實現商業價值，商務專案關係人通常會更參與。 如下圖所示，已定義的雲端策略小組會將這些商務專案關係人保持一致，以充分發揮雲端採用投資的價值。

當成熟度發生茁壯時，由於 IT 主導的雲端採用工作，策略上的調整通常會在治理或中央 IT 小組之前。 當雲端採用工作由企業領導時，操作模型和組織的重點通常會在先前發生。 可能的話，業務成果和雲端策略小組都應該在程式初期定義。

## <a name="operational-alignment"></a>營運一致性

![營運一致性](../_images/ready/org-ready-operations-aligned.png)

從雲端採用工作實現商業價值需要穩定的作業。 雲端中的作業可能需要新的工具、進程或技能。 當需要穩定的 IT 作業來達成業務成果時，請務必新增已定義的雲端作業小組，如下所示。

雲端作業可由現有的 IT 作業角色來傳遞。 但是，將雲端作業委派給 IT 營運以外的其他物件並不罕見。 受管理的服務提供者、DevOps 團隊和業務單位，通常會假設與雲端作業相關聯的責任，以及 IT 作業所提供的支援和護欄。 這對於將焦點放在 DevOps 或 PaaS 部署上的雲端採用工作來說，日益普遍。

## <a name="cloud-center-of-excellence"></a>雲端卓越中心

![卓越的雲端中心 (CCoE) ](../_images/ready/org-ready-ccoe.png)

在成熟度的最高狀態中，雲端的卓越中心會讓小組在新式雲端優先作業模型方面保持一致。 這種方法提供集中化的 IT 功能，例如治理、安全性、平臺和自動化。

此結構與上面中央 IT 小組結構之間的主要差異，是自助和 democratization 的強大焦點。 此結構中的小組會盡可能地以委派控制權的意圖進行組織。 將治理和合規性實務與雲端原生解決方案保持一致，可建立護欄和保護機制。 與中央 IT 小組模型不同的是，雲端原生方法可充分發揮創新，並將作業的額外負荷降至最低。 若要採用此模型，您必須將 IT 流程現代化的相互協定，從商務和 IT 領導力都需要。 此模型不太可能發生茁壯，而且通常需要執行支援。

## <a name="next-steps"></a>下一步

在符合組織結構成熟度的特定階段之後，您就可以使用 [RACI 圖表](./raci-alignment.md) 來調整每個小組的責任和責任。

> [!div class="nextstepaction"]
> [對齊適當的 RACI 圖](./raci-alignment.md)
