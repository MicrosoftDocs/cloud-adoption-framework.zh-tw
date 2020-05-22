---
title: 成本管理專業領域中的動機和業務風險
description: 瞭解並查看典型客戶在雲端治理策略中採用成本管理專業領域的範例。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: e52e38803f37d8dec279eb9af5b422500b338b21
ms.sourcegitcommit: 9a84c2dfa4c3859fd7d5b1e06bbb8549ff6967fa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2020
ms.locfileid: "83755098"
---
<!-- cSpell:ignore prepurchases -->

# <a name="motivations-and-business-risks-in-the-cost-management-discipline"></a>成本管理專業領域中的動機和業務風險

本文將討論客戶通常會在雲端治理策略中採用成本管理專業領域的原因。 它也提供數個衍生原則聲明的業務風險範例。

## <a name="relevance"></a>相關性

在成本治理方面，採用雲端會建立範式轉移。 傳統內部部署世界中的成本管理會以下列各項為基礎：重新整理週期、資料中心收購、主機更新，以及週期性維護問題。 您可以預測、規劃和精簡這其中每一個成本，以便與每年的資本支出預算保持一致。

針對雲端解決方案，許多企業通常會採取更被動的方式來進行成本管理。 在許多情況下，企業將會預購 (或承諾使用) 一定數量的雲端服務。 此模型假設要根據企業規劃來針對特定雲端廠商花費的費用來取得最大折扣，即會建立對已規劃之主動式成本週期的認知。 只有當企業也實行成熟的成本管理專業領域時，該認知才會變成現實。

雲端提供在傳統內部部署資料中心前所未聞的自助功能。 這些新功能讓企業能夠更靈活、更少限制且更開放地採用新技術。 自助服務的缺點是終端使用者可能會在不知情的情況中超過配置的預算。 相反地，相同的使用者可以體驗方案中的變化，並且意外地不使用所預測的雲端服務數量。 在治理小組內，任一個方向的轉移可能性，會證明成本管理專業領域中投資的正當性。

## <a name="business-risk"></a>業務風險

成本管理專業領域會嘗試解決與在裝載雲端式工作負載時所產生費用相關的核心業務風險。 在您規劃和實作雲端部署時，與您的企業一起識別這些風險，並監視它們的關聯性。

組織之間的風險會有所不同，但以下是常見的成本相關風險，您可以將其作為起點，以在雲端治理小組內進行討論：

- **預算控制：** 不控制預算可能會導致過多的雲端廠商支出。
- **使用量損失：** 未使用的 Prepurchases 或 precommitments 可能會導致投資遺失。
- **消費異常：** 任一方向的非預期尖峰可能是不當使用的指標。
- **過度布建資產：** 當資產部署在超過應用程式或虛擬機器（VM）需求的設定時，可能會造成浪費。

## <a name="next-steps"></a>後續步驟

使用[成本管理原則範本](./template.md)來記載可能由目前雲端採用方案引進的業務風險。

在您瞭解實際的商務風險之後，下一步就是記載企業對於風險的承受度，以及用來監視該容錯的指標和關鍵計量。

> [!div class="nextstepaction"]
> [了解指標、計量及風險承受度](./metrics-tolerance.md)
