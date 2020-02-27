---
title: 改善您的初始雲端治理基礎
description: 使用適用于 Azure 的雲端採用架構，以瞭解如何以累加方式改善您的初始雲端治理基礎。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/13/2019
ms.topic: landing-page
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
layout: LandingPage
ms.openlocfilehash: 050d9cdfd2069a4d7da2e411233f6a60f270eb10
ms.sourcegitcommit: af45c1c027d7246d1a6e4ec248406fb9a8752fb5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2020
ms.locfileid: "77706603"
---
# <a name="improve-your-initial-cloud-governance-foundation"></a>改善您的初始雲端治理基礎

本文假設您已建立[初始雲端治理基礎](./initial-foundation.md)。 隨著您的雲端採用方案的實行，具體的風險將會從建議的方法中顯現，小組想要採用雲端。 當這些風險在發行規劃交談中呈現時，請使用下列方格快速找出採用計畫的最佳作法，以避免風險成為真正的威脅。

## <a name="maturity-vectors"></a>成熟度向量

在任何時候，您都可以將下列最佳作法套用至初始治理基礎，以解決下表所述的風險或需求。

> [!IMPORTANT]
> 資源組織可能會影響這些最佳作法的套用方式。 請務必從最符合您在上一個步驟中執行的初始雲端治理基礎的建議開始。

|風險/需求 | 標準企業 | 複雜企業 |
|---|---|---|
|雲端中的敏感性資料|[專業領域改進](./guides/standard/security-baseline-improvement.md)|[專業領域改進](./guides/complex/security-baseline-improvement.md)|
|雲端中的任務關鍵性應用程式|[專業領域改進](./guides/standard/resource-consistency-improvement.md)|[專業領域改進](./guides/complex/resource-consistency-improvement.md)|
|雲端成本管理|[專業領域改進](./guides/standard/cost-management-improvement.md)|[專業領域改進](./guides/complex/cost-management-improvement.md)|
|多重雲端|[專業領域改進](./guides/standard/multicloud-improvement.md)|[專業領域改進](./guides/complex/multicloud-improvement.md)|
|複雜/舊版身分識別管理|N/A|[專業領域改進](./guides/complex/identity-baseline-improvement.md)|
|多層控管|N/A|[專業領域改進](./guides/complex/multiple-layers-of-governance.md)|

## <a name="next-steps"></a>後續步驟

除了應用程式的最佳作法之外，也可以自訂雲端採用架構中的治理方法，以符合獨特的商務限制。 遵循適用的建議之後，請[評估公司原則，以瞭解其他自訂需求](./corporate-policy.md)。

> [!div class="nextstepaction"]
> [評估公司原則](./corporate-policy.md)
