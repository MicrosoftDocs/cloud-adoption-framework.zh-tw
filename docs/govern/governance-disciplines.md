---
title: 雲端治理的五個專業領域
description: 使用適用于 Azure 的雲端採用架構，瞭解成本管理、部署加速、身分識別基準、資源一致性和安全性基準專業領域。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 15da317061924f0c631877a7a7a0430d8e336038
ms.sourcegitcommit: 07d56209d56ee199dd148dbac59671cbb57880c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88881121"
---
# <a name="the-five-disciplines-of-cloud-governance"></a>雲端治理的五個專業領域

<!-- docutune:casing "Disciplines of Cloud Governance" "Cost Management" "Deployment Acceleration" "Identity Baseline" "Resource Consistency" "Security Baseline" -->

|  |  |
|--|--|
| 對業務程序或技術平台進行的任何變更都會引進風險。 雲端治理小組（其成員有時也稱為雲端監管人）負責降低這些風險，並確保採用或創新的工作不會中斷。 <br><br> 雲端採用架構治理模型會根據 [公司原則的開發](./corporate-policy.md) 和 [雲端治理的五個專業領域](#disciplines-of-cloud-governance)，來引導這些決策（不論選擇的雲端平臺）。 [可操作的設計指南](./guides/index.md)使用 Azure 服務示範此模型。 瞭解以下雲端採用架構治理模型的專業領域。 | <br><br> [![雲端採用架構治理模型的圖表：公司原則和治理專業領域](../_images/operational-transformation-govern-thumbnail.png)](../_images/operational-transformation-govern-large.png#lightbox) <br> *圖1：公司原則的視覺效果，以及雲端治理的五個專業領域。* |

## <a name="disciplines-of-cloud-governance"></a>雲端治理的專業領域

在任何雲端平臺中，都有共同的治理專業領域，可協助通知原則並配合工具鏈。 這些專業領域會引導您決定適當層級的自動化，並強制執行跨雲端平臺的公司原則。

|  |  |
|--|--|
| <br> ![成本管理](../_images/govern/cost-management.png) | <br> [成本管理](./cost-management/index.md)：成本是雲端使用者的主要考慮。 開發適用於所有雲端平台的成本控制原則。 |
| <br> ![安全性基準](../_images/govern/security-baseline.png) | <br> [安全性基準](./security-baseline/index.md)：安全性是一種複雜的主題，每一家公司都是唯一的。 建立安全性需求之後，雲端治理原則和強制執行會將這些需求套用到網路、資料和資產設定。|
| <br> ![身分識別基準](../_images/govern/identity-baseline.png) | <br> 身分[識別基準](./identity-baseline/index.md)：身分識別需求的應用程式不一致可能會提高缺口的風險。 身分識別基準專業領域著重于確保身分識別一致地套用到雲端採用工作。 |
| <br> ![資源一致性](../_images/govern/resource-consistency.png) | <br> [資源一致性](./resource-consistency/index.md)：雲端作業相依于一致的資源設定。 透過治理工具，資源可以一致地設定，以管理與上線、漂移、發現性及復原相關的風險。 |
| <br> ![部署加速](../_images/govern/deployment-acceleration.png) | <br> [部署加速](./deployment-acceleration/index.md)：部署和設定方法中的集中化、標準化和一致性可改善治理實務。 當透過雲端式治理工具提供時，他們會建立可加速部署活動的雲端因素。 |
