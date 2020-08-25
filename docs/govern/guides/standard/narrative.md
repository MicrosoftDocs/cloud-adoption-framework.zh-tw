---
title: 標準企業治理：治理策略背後的敘述
description: 使用適用于 Azure 的雲端採用架構，瞭解如何在標準企業雲端採用旅程期間建立治理的使用案例。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/05/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 688c9c11e1366621769baaccdcdb335275e4712a
ms.sourcegitcommit: 8b5fdb68127c24133429b4288f6bf9004a1d1253
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/25/2020
ms.locfileid: "88847865"
---
# <a name="standard-enterprise-governance-guide-the-narrative-behind-the-governance-strategy"></a>標準企業治理指南：治理策略背後的敘述

下列敘述描述 [標準企業的雲端採用旅程](./index.md)期間的治理使用案例。 在實施旅程之前，請務必瞭解此敘述中反映的假設和原理。 然後您可以讓治理策略與貴組織的旅程更一致。

## <a name="back-story"></a>背景故事

董事會今年開始計劃以多種方式為企業注入活力。 他們正推動領導階層，以改善客戶體驗以贏得市場佔有率。 他們也會推送新的產品和服務，將公司定位為產業的考慮領導者。 他們同時還盡力減少浪費並縮減不必要的成本。 雖然令人生畏，但董事會和領導階層的行動展現出這項工作將盡可能地將資金集中投入未來發展。

在過去，公司的 CIO 已從這些策略性對話中排除。 由於未來的願景與技術成長有本質的連結，因此它在資料表上有基座可協助引導這些大型方案。 IT 現在應該以新的方式提供。 小組未準備好進行這些變更，而且可能會在學習曲線中困難。

## <a name="business-characteristics"></a>商務特性

公司具備下列商務設定檔：

- 所有的銷售與營運都位於單一國家/地區，且全球客戶的百分比低。
- 企業會以單一業務單位運作，預算與函式一致，包括銷售、行銷、營運和 IT。
- 企業將大多數的 IT 視為資本流失或成本中心。

## <a name="current-state"></a>目前狀態

以下是該公司的 IT 和雲端作業目前的狀態：

- IT 負責運作兩個託管的基礎結構環境。 其中一個環境含有生產資產。 另一個環境則含有災害復原和一些開發/測試資產。 這些環境是由兩個不同的提供者所託管。 它會將這兩個資料中心分別稱為生產和 DR。
- 它會將所有使用者的電子郵件帳戶遷移至 Microsoft 365，以進入雲端。 此移轉已在六個月前完成。 其他一些 IT 資產已部署至雲端。
- 應用程式開發小組正在開發/測試容量中工作，以瞭解雲端原生功能。
- 商業智慧 (BI) 小組將試驗雲端中的巨量資料，以及新平台上的資料鑑藏。
- 該公司有一個鬆散定義的原則，指出個人客戶資料和財務資料無法裝載在雲端中，這會限制目前部署中的任務關鍵性應用程式。
- IT 投資主要是以資本支出來控制。 這些投資每年規劃一次。 在過去幾年中，投資僅包含基本維護需求。

## <a name="future-state"></a>未來的狀態

預計未來幾年將發生下列變化：

- CIO 正在查看個人資料和財務資料的原則，以允許未來的狀態目標。
- 應用程式開發和 BI 團隊希望在未來的 24 個月內，根據客戶業務開發和新產品的願景，將雲端式解決方案發佈到生產環境中。
- 今年，IT 團隊會將 2,000 個 VM 移轉至雲端，來完成淘汰 DR 資料中心的災難復原工作負載。 這預期會在未來五年產生預估的 $ 25m 美元成本節約。
    ![內部部署成本與 Azure 成本的比較，在未來五年來示範 $ 25m 美元的報酬](../../../_images/govern/calculator-small-to-medium-enterprise.png)
- 公司計畫變更已認可的資本支出，使其成為其投資成本，藉此改變 it 投資的方式。 這項改變將提供更好的成本控制，並使 IT 能夠加速完成其他計劃的工作。

## <a name="next-steps"></a>後續步驟

公司已經制訂一項公司原則，使治理實施具體化。 公司原則推動了許多技術決策。

> [!div class="nextstepaction"]
> [檢閱最初的公司原則](./initial-corporate-policy.md)
