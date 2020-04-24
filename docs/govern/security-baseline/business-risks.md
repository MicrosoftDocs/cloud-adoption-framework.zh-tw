---
title: 安全性基準商務風險
description: 瞭解並查看典型客戶在雲端治理策略中採用安全性基準專業領域的範例。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 40b92c15af9ea4677d049cc76902d33e6807f139
ms.sourcegitcommit: 7d3fc1e407cd18c4fc7c4964a77885907a9b85c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "80433636"
---
# <a name="security-baseline-motivations-and-business-risks"></a>安全性基準動機和業務風險

本文討論客戶通常會在雲端治理策略中採用安全性基準專業領域的原因。 它也提供幾個能驅使原則聲明之潛在商務風險的範例。

<!-- markdownlint-disable MD026 -->

## <a name="security-baseline-relevancy"></a>安全性基準相關性

安全性是任何 IT 組織的一大考量。 雲端部署面臨許多與裝載於傳統內部部署資料中心的工作負載相同的安全性風險。 不過，公用雲端平台的性質，缺乏對儲存和執行工作負載之實體硬體的直接擁有權，意味著雲端安全性需要其本身的原則和程序。

除了傳統的安全性原則之外，設定雲端安全性治理的其中一個主要事項，就是可以輕鬆地建立資源，如果在部署之前未考慮到安全性，可能會增加弱點。 [軟體定義網路 (SDN)](../../decision-guides/software-defined-network/index.md) 等技術為快速變更雲端架網路拓樸提供的靈活性，也可以輕鬆地以出人意料的方式修改整體網路攻擊面。 雲端平台也提供工具和功能，可以不一定可行的方式在內部部署環境中改善您的安全性功能。

您投入安全性原則和程序的金額，將大量取決於您雲端部署的本質。 初始測試部署可能只需要最基本的安全性原則，而任務關鍵性工作負載則需要解決複雜而廣泛的安全性需求。 在某種程度上所有部署都需要參與該專業領域。

安全性基準專業領域涵蓋了您可以採取的公司原則和手動程序，以保護您的雲端部署免受安全性風險的影響。

> [!NOTE]
>雖然在安全性基準內容下理解[身分識別基準](../identity-baseline/index.md)以及它與存取控制的關係非常重要，但[五個雲端治理專業領域](../index.md)會呼叫[身分識別基準](../identity-baseline/index.md)作為自己的專業領域，與安全性基準區隔。

## <a name="business-risk"></a>業務風險

安全性基準專業領域會嘗試解決與核心安全性相關的商業風險。 在您規劃和實作雲端部署時，與您的企業一起識別這些風險，並監視它們的關聯性。

組織之間的風險會有所不同，但以下是常見的安全性相關風險，您可以將其作為起點，以在雲端治理小組內進行討論：

- **資料缺口：** 無意暴露或遺失敏感性雲端裝載的資料，可能會導致客戶遺失、合約問題或法律後果。
- **服務中斷：** 因為不安全的基礎結構中斷正常作業，而中斷和其他效能問題，可能會導致生產力遺失或失去業務。

## <a name="next-steps"></a>後續步驟

使用[雲端管理範本](./template.md)，記錄了目前的雲端採用方案可能引進的業務風險。

一旦建立對於實際商務風險的了解，下一步是記錄風險的業務承受度與用來監視承受度的指標和關鍵計量。

> [!div class="nextstepaction"]
> [了解指標、計量及風險承受度](./metrics-tolerance.md)
