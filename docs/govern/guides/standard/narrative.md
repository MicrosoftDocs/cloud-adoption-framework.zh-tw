---
title: 標準企業治理：治理策略背後的敘述
description: 使用適用于 Azure 的雲端採用架構，以瞭解如何在標準的企業雲端採用旅程期間，建立治理的使用案例。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/05/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 3781bb50f8d2ad76efd606f8d6914582cee0754c
ms.sourcegitcommit: 7d3fc1e407cd18c4fc7c4964a77885907a9b85c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "80434230"
---
# <a name="standard-enterprise-governance-guide-the-narrative-behind-the-governance-strategy"></a>標準企業治理指南：治理策略背後的敘述

下列敘述描述在[標準企業的雲端採用旅程](./index.md)期間的治理使用案例。 在執行旅程之前，請務必瞭解此敘述中反映的假設和原理。 然後您可以讓治理策略與貴組織的旅程更一致。

## <a name="back-story"></a>背景故事

董事會今年開始計劃以多種方式為企業注入活力。 他們將推動改善客戶體驗以獲得市場佔有率的領導地位。 他們也將推動新產品和服務，將公司定位為業界的思想領導者。 他們同時還盡力減少浪費並縮減不必要的成本。 雖然令人生畏，但董事會和領導階層的行動展現出這項工作將盡可能地將資金集中投入未來發展。

在過去，公司的 CIO 已從這些策略性交談中排除。 但是，未來的願景在本質上與技術發展息息相關，因此 IT 擁有一席之地，可以幫助指導這些重大計劃。 IT 現在應該以新的方式提供。 小組尚未針對這些變更進行準備，而且很可能會與學習曲線一起困難。

## <a name="business-characteristics"></a>商務特性

公司具備下列商務設定檔：

- 所有的銷售與營運都位於單一國家/地區，且全球客戶的百分比低。
- 企業當作單一業務單位營運，預算會隨著銷售、行銷、營運和 IT 等職責調整。
- 企業將大多數的 IT 視為資本流失或成本中心。

## <a name="current-state"></a>目前狀態

以下是該公司的 IT 和雲端作業目前的狀態：

- IT 負責運作兩個託管的基礎結構環境。 其中一個環境含有生產資產。 另一個環境則含有災害復原和一些開發/測試資產。 這些環境是由兩個不同的提供者所託管。 IT 將這兩個資料中心分別歸類為 Prod 和 DR。
- IT 將所有使用者電子郵件帳戶移轉至 Office 365，藉此進入雲端。 此移轉已在六個月前完成。 其他一些 IT 資產已部署至雲端。
- 應用程式開發小組在開發/測試容量中工作，以瞭解雲端原生功能。
- 商業智慧 (BI) 小組將試驗雲端中的巨量資料，以及新平台上的資料鑑藏。
- 該公司有一個鬆散定義的原則，說明無法在雲端中裝載個人客戶資料和財務資料，這會限制目前部署中的任務關鍵性應用程式。
- IT 投資主要是以資本支出來控制。 這些投資每年規劃一次。 在過去幾年中，投資僅包含基本維護需求。

## <a name="future-state"></a>未來的狀態

預計未來幾年將發生下列變化：

- CIO 正在審查個人資料和財務資料的原則，以允許未來的州目標。
- 應用程式開發和 BI 團隊希望在未來的 24 個月內，根據客戶業務開發和新產品的願景，將雲端式解決方案發佈到生產環境中。
- 今年，IT 團隊會將 2,000 個 VM 移轉至雲端，來完成淘汰 DR 資料中心的災難復原工作負載。 預計這將在未來五年內節省約 2500 萬美元的成本。
    ![內部部署成本與 Azure 成本，示範未來五年的美元傳回 $ 25M 美元](../../../_images/govern/calculator-small-to-medium-enterprise.png)
- 公司打算改變 it 投資的方式，因為它會將已認可的資本支出重新置放為營運費用。 這項改變將提供更好的成本控制，並使 IT 能夠加速完成其他計劃的工作。

## <a name="next-steps"></a>後續步驟

公司已經制訂一項公司原則，使治理實施具體化。 公司原則推動了許多技術決策。

> [!div class="nextstepaction"]
> [檢閱最初的公司原則](./initial-corporate-policy.md)
