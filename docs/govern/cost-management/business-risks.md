---
title: 成本管理業務風險
description: 瞭解並查看典型客戶在雲端治理策略中採用成本管理專業領域的範例。 
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 38fb0523b2c5915a351223fdb603dbb0cacbf602
ms.sourcegitcommit: ea63be7fa94a75335223bd84d065ad3ea1d54fdb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/27/2020
ms.locfileid: "80357145"
---
<!-- cSpell:ignore prepurchases -->

# <a name="cost-management-motivations-and-business-risks"></a>成本管理動機和業務風險

本文將討論客戶通常會在雲端治理策略中採用成本管理專業領域的原因。 它也提供數個衍生原則聲明的業務風險範例。

<!-- markdownlint-disable MD026 -->

## <a name="is-cost-management-relevant"></a>是否與成本管理相關？

在成本治理方面，採用雲端會建立範式轉移。 傳統內部部署世界中的成本管理會以下列各項為基礎：重新整理週期、資料中心收購、主機更新，以及週期性維護問題。 您可以預測、規劃和精簡這其中每一個成本，以便與每年的資本支出預算保持一致。

對於雲端解決方案，許多企業都傾向於對成本管理採用更具反應性的方法。 在許多情況下，企業將會預購 (或承諾使用) 一定數量的雲端服務。 此模型假設要根據企業規劃來針對特定雲端廠商花費的費用來取得最大折扣，即會建立對已規劃之主動式成本週期的認知。 不過，該認知只有在企業也會實作成熟的成本管理專業領域時成真。

雲端提供在傳統內部部署資料中心前所未聞的自助功能。 這些新功能讓企業能夠更靈活、更少限制且更開放地採用新技術。 不過，自助的缺點是終端使用者會在不知情的情況下超過已配置的預算。 相反地，相同的使用者可以體驗方案中的變化，並且意外地不使用所預測的雲端服務數量。 在治理小組內，任一個方向的轉移可能性，會證明成本管理專業領域中投資的正當性。

## <a name="business-risk"></a>業務風險

成本管理專業領域會嘗試解決與在裝載雲端式工作負載時所產生費用相關的核心業務風險。 在您規劃和實作雲端部署時，與您的企業一起識別這些風險，並監視它們的關聯性。

組織之間的風險會有所不同，但以下是常見的成本相關風險，您可以將其作為起點，以在雲端治理小組內進行討論：

- **預算控制：** 不控制預算可能會導致過多的雲端廠商支出。
- **使用量損失：** 未使用的 Prepurchases 或 precommitments 可能會導致投資遺失。
- **消費異常：** 任一方向的非預期尖峰可能是不當使用的指標。
- **過度布建資產：** 當資產部署在超過應用程式或虛擬機器（VM）需求的設定時，可能會造成浪費。

## <a name="next-steps"></a>後續步驟

使用[雲端管理範本](./template.md)，記錄了目前的雲端採用方案可能引進的業務風險。

在您瞭解實際的商務風險之後，下一步就是記載企業對於風險的承受度，以及用來監視該容錯的指標和關鍵計量。

> [!div class="nextstepaction"]
> [了解指標、計量及風險承受度](./metrics-tolerance.md)
