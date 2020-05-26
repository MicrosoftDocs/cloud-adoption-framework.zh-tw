---
title: 擴充登陸區域
description: 使用適用於 Azure 的雲端採用架構來了解如何擴充登陸區域。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/15/2020
ms.topic: overview
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 0964da23da680755ea9d6c35fef0996e986780b4
ms.sourcegitcommit: 9a84c2dfa4c3859fd7d5b1e06bbb8549ff6967fa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2020
ms.locfileid: "83756561"
---
# <a name="expand-your-landing-zone"></a>擴充登陸區域

本節的就緒方法以[登陸區域重構](../landing-zone/refactor.md)的原則作為建置基礎。 如該文章所述，基礎結構即程式碼的重構方法可移除妨礙企業成功的因素，同時將風險降至最低。 這一系列的文章假設您已部署第一個登陸區域，且現在想要擴充該登陸區域以符合企業需求。

## <a name="shared-architecture-principles"></a>共用架構原則

擴充登陸區域可提供程式碼優先方法，將下列原則內嵌到登陸區域中，以及更廣泛地內嵌到整體雲端環境中。

![共用架構原則](../../_images/ready/shared-principles.png)

[Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-overview)、[Microsoft Azure Well-Architected Framework](https://docs.microsoft.com/azure/architecture/framework) 和 [Azure 架構中心](https://docs.microsoft.com/azure/architecture)中的解決方案均共用這些相同的架構原則。

## <a name="applying-these-principles-to-your-landing-zone-improvements"></a>將這些原則套用至您的登陸區域改進功能

為了更符合雲端採用架構的方法，上述原則可分成幾項可操作的登陸區域改進功能：

- 基本考量：重構登陸區域，以精簡裝載、基本概念和其他基礎元素。
- 作業擴充：新增作業管理設定，以改善**效能、可靠性和卓越營運**。
- 控管擴充：新增控管設定，以改善**成本、可靠性、安全性**和一致性。
- 安全性擴充：新增**安全性**設定，以改善敏感性資料和重要系統的保護。

> [!WARNING]
> 採用小組若具有**在雲端中裝載超過 1,000 項資產 (應用程式、基礎結構或資料資產)** 的中期目標 (在 24 個月內)，應考慮及早在其雲端採用旅程中進行每一項擴充。 若為其他各種採用模式，登陸區域擴充可以是平行的反覆項目，以利及早獲致商務成就。

## <a name="next-steps"></a>後續步驟

重構第一個登陸區域之前，請務必了解[登陸區域的測試驅動開發](./test-driven-development.md)。

> [!div class="nextstepaction"]
> [登陸區域的測試驅動開發](./test-driven-development.md)
