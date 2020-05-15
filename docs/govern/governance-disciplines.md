---
title: 雲端治理的五個專業領域
description: 使用適用于 Azure 的雲端採用架構來瞭解成本管理、部署加速、身分識別基準、資源一致性和安全性基準。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 550dd724ae6a8eedd34b5a8ffc2146425500bc59
ms.sourcegitcommit: 5d6a7610e556f7b8ca69960ba76a3adfa9203ded
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2020
ms.locfileid: "83400585"
---
# <a name="the-five-disciplines-of-cloud-governance"></a>雲端治理的五個專業領域

<!-- docsTest:ignore "Disciplines of Cloud Governance" "Cost Management" "Deployment Acceleration" "Identity Baseline" "Resource Consistency" "Security Baseline" -->
<!-- markdownlint-disable MD033 -->

| | |
|---|---|
| 對業務程序或技術平台進行的任何變更都會引進風險。 雲端治理小組（其成員有時也稱為雲端保管人）負責降低這些風險，並確保採用或創新工作的中斷程度降至最低。 <br><br> 雲端採用架構治理模型會將重點放在[公司原則的開發](./corporate-policy.md)和[雲端治理的五個專業領域](#disciplines-of-cloud-governance)，以引導這些決策，而不論選擇的雲端平臺。 [可操作的設計指南](./guides/index.md)使用 Azure 服務示範此模型。 深入瞭解雲端採用架構治理模型的專業領域。 | <br><br> [![雲端採用架構治理模型的圖表：公司原則和治理專業領域](../_images/operational-transformation-govern-thumbnail.png)](../_images/operational-transformation-govern-large.png#lightbox) <br> _圖1：公司原則和雲端治理的五個專業領域的視覺效果。_ |

## <a name="disciplines-of-cloud-governance"></a>雲端治理的專業領域

在任何雲端平臺中，都有共同的治理專業領域，可協助通知原則並使工具鏈一致。 這些專業領域會引導您在雲端平臺上適當層級的自動化和強制執行公司原則的決策。

| | |
|---|---|
| <br> ![成本管理](../_images/govern/cost-management.png) | <br> [成本管理](./cost-management/index.md)：成本是雲端使用者的主要考慮。 開發適用於所有雲端平台的成本控制原則。 |
| <br> ![安全性基準](../_images/govern/security-baseline.png) | <br> [安全性基準](./security-baseline/index.md)：安全性是一種複雜的主旨，對於每一家公司都是獨一無二的。 一旦建立安全性需求之後，雲端治理原則和強制會將這些需求套用到網路、資料和資產設定。|
| <br> ![身分識別基準](../_images/govern/identity-baseline.png) | <br> 身分[識別基準](./identity-baseline/index.md)：識別需求的應用程式不一致可能會增加缺口的風險。 身分識別基準專業領域著重于確保身分識別一致地套用到雲端採用工作。 |
| <br> ![資源一致性](../_images/govern/resource-consistency.png) | <br> [資源一致性](./resource-consistency/index.md)：雲端作業取決於一致的資源設定。 透過治理工具，可以一致地設定資源，以管理與上線、漂移、發現和復原相關的風險。 |
| <br> ![部署加速](../_images/govern/deployment-acceleration.png) | <br> [部署加速](./deployment-acceleration/index.md)：部署和設定的方法中的集中化、標準化和一致性可改善治理實務。 透過雲端式治理工具提供時，他們會建立可加速部署活動的雲端因素。 |
