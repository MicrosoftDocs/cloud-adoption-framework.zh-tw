---
title: 改善您的初始雲端治理基礎
description: 使用適用于 Azure 的雲端採用架構，瞭解如何以累加方式改進您的初始雲端治理基礎。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/13/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: internal
ms.openlocfilehash: 613851203fbc099df242686fd7665cfff7bd1fef
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97021299"
---
# <a name="improve-your-initial-cloud-governance-foundation"></a>改善您的初始雲端治理基礎

本文假設您已建立 [初始雲端治理基礎](./initial-foundation.md)。 當您的雲端採用方案實行時，會從小組想要採用雲端的建議方法中，浮現出實質的風險。 當這些風險在發行規劃對話中出現時，請使用下列方格來快速找出接受採用計畫的一些最佳作法，以避免風險成為真正的威脅。

## <a name="maturity-vectors"></a>成熟度向量

您可以隨時將下列最佳作法套用至初始治理基礎，以解決下表所述的風險或需要。

> [!IMPORTANT]
> 資源組織會影響這些最佳作法的套用方式。 請務必從最符合您在上一個步驟中所實行的初始雲端治理基礎的建議開始。

| 風險/需求 | 標準企業 | 複雜企業 |
|---|---|---|
| 雲端中的敏感性資料 | [專業領域改進](./guides/standard/security-baseline-improvement.md) | [專業領域改進](./guides/complex/security-baseline-improvement.md) |
| 雲端中的任務關鍵性應用程式 | [專業領域改進](./guides/standard/resource-consistency-improvement.md) | [專業領域改進](./guides/complex/resource-consistency-improvement.md) |
| 雲端成本管理 | [專業領域改進](./guides/standard/cost-management-improvement.md) | [專業領域改進](./guides/complex/cost-management-improvement.md) |
| 多重雲端 | [專業領域改進](./guides/standard/multicloud-improvement.md) | [專業領域改進](./guides/complex/multicloud-improvement.md) |
| 複雜/舊版身分識別管理 | N/A | [專業領域改進](./guides/complex/identity-baseline-improvement.md) |
| 多層控管 | N/A | [專業領域改進](./guides/complex/multiple-layers-of-governance.md) |

## <a name="next-steps"></a>後續步驟

除了應用程式的最佳作法之外，還可以自訂雲端採用架構的控管方法，以符合獨特的商務限制。 遵循適用的建議之後，請 [評估公司原則以瞭解其他自訂需求](./corporate-policy.md)。

> [!div class="nextstepaction"]
> [評估公司原則](./corporate-policy.md)
