---
title: 安全性基準專業領域中的動機和業務風險
description: 瞭解並查看雲端治理策略中一般客戶採用安全性基準專業領域的範例。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: internal
ms.openlocfilehash: 2f289dd98e346e2b3aa0ce934b5fb1c0b2c4efce
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97019208"
---
# <a name="motivations-and-business-risks-in-the-security-baseline-discipline"></a>安全性基準專業領域中的動機和業務風險

本文討論客戶通常會在雲端治理策略中採用安全性基準專業領域的原因。 它也提供幾個能驅使原則聲明之潛在商務風險的範例。

## <a name="relevance"></a>相關性

安全性是任何 IT 組織的一大考量。 雲端部署面臨許多與裝載於傳統內部部署資料中心的工作負載相同的安全性風險。 公用雲端平臺的本質，缺乏儲存和執行工作負載之實體硬體的直接擁有權，表示雲端安全性需要自己的原則和流程。

除了傳統的安全性原則，設定雲端安全性治理的其中一個主要功能，是可讓您輕鬆地建立資源，可能會在部署之前未考慮到安全性的情況下新增弱點。 [軟體定義的網路功能 (SDN) ](../../decision-guides/software-defined-network/index.md)提供快速變更雲端式網路拓撲的彈性，也可讓您以未預期的方式輕鬆修改整體網路攻擊面。 雲端平台也提供工具和功能，可以不一定可行的方式在內部部署環境中改善您的安全性功能。

您投入安全性原則和程序的金額，將大量取決於您雲端部署的本質。 初始測試部署可能只需要最基本的安全性原則，而任務關鍵性工作負載則需要解決複雜而廣泛的安全性需求。 在某種程度上所有部署都需要參與該專業領域。

安全性基準專業領域涵蓋了您可以採取的公司原則和手動程序，以保護您的雲端部署免受安全性風險的影響。

> [!NOTE]
> 雖然在安全性基準專業領域的內容中瞭解身分 [識別基準專業](../identity-baseline/index.md) 領域，以及與存取控制有何關聯，但這 [五個雲端治理專業領域](../index.md) 將它視為個別的專業領域。

## <a name="business-risk"></a>業務風險

安全性基準專業領域會嘗試解決與核心安全性相關的商業風險。 在您規劃和實作雲端部署時，與您的企業一起識別這些風險，並監視它們的關聯性。

組織之間的風險不同。 使用這份常見的安全性相關風險清單作為起點，以便在雲端治理小組內進行討論：

- **資料缺口：** 無意地公開或遺失敏感性雲端裝載的資料可能會導致客戶遺失、契約問題或法律結果。
- **服務中斷：** 因為不安全的基礎結構會中斷正常作業，導致中斷和其他效能問題，可能會導致生產力遺失或企業遺失。

## <a name="next-steps"></a>後續步驟

使用「 [安全性基準專業領域」範本](./template.md) ，記錄可能由目前雲端採用方案引進的商務風險。

一旦建立對於實際商務風險的了解，下一步是記錄風險的業務承受度與用來監視承受度的指標和關鍵計量。

> [!div class="nextstepaction"]
> [了解指標、計量及風險承受度](./metrics-tolerance.md)
