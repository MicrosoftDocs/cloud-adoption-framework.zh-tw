---
title: 混合式和多重雲端案例簡介
description: 深入瞭解混合式和多重雲端案例。
author: BrianBlanchard
ms.author: brblanch
ms.date: 02/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.custom: e2e-hybrid
ms.openlocfilehash: 7a14b0e3384667d9ea3e2545b223be0df6f229bc
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794832"
---
# <a name="introduction-to-the-hybrid-and-multicloud-scenario"></a>混合式和多重雲端案例簡介

當客戶因應更大、更複雜的雲端採用形式時，對雲端的旅程會稍微複雜一點。 本文系列合併了針對混合式和多重雲端採用案例做好準備所需的各種技術和非技術性考慮。

此案例著重于啟用兩個目標結果：

- [混合式和多重雲端採用](./index.md)：建立、部署和（或）遷移可攜的解決方案，讓雲端平臺之間能輕鬆地切換至最小廠商的鎖定。
- [整合作業](./unified-operations.md)：透過一組共用一般作業進程的集中治理和管理程式，簡化在每個雲端上支援這些解決方案的作業，不論解決方案目前的部署位置為何。

## <a name="components-of-the-scenario"></a>案例的元件

此案例的設計目的是要引導在雲端採用生命週期期間的端對端客戶旅程圖。

![混合式多重雲端方法的圖形](../../_images/hybrid/hybrid-multicloud-approach.png)

傳遞完整旅程需要一些重要的元件，或指引集：

| <span title="圖示">&nbsp;</span> | <span title="描述">&nbsp;</span> |
|--|--|
| <br> :::image type="icon" source="../../_images/hybrid/cloud-journey.png"::: | <br> [適用于 Azure 的 Microsoft 雲端採用架構](../../get-started/index.md)：這些文章會逐步解說每個 CAF 方法的最小考慮與實施。 使用這些文章來準備決策者、中央 IT 和雲端中心，以在您的組合中採用混合式和多重雲端工作負載。 |
| <br> :::image type="icon" source="../../_images/hybrid/hybrid-well-architected.png"::: | <br> [Microsoft Azure Well-Architected Framework](/azure/architecture/framework/)：這些文章概述每個工作負載擁有者必須在混合式和多重雲端環境中部署工作負載時所應進行的考慮。 |
| <br> :::image type="icon" source="../../_images/hybrid/hybrid-architectures.png"::: | <br> [參考架構](/azure/architecture/browse/)：這些參考解決方案可協助加速部署許多常見的混合式和多重雲端案例。 |
| <br> :::image type="icon" source="../../_images/hybrid/hybrid-best-practices.png"::: | <br> 最佳做法：這些等級300以上的文章可協助中央 IT 小組使用 Azure Arc、ARM 範本和其他相關的 Azure 產品，將資產上線至整合的作業解決方案。 |
| <br> :::image type="icon" source="../../_images/hybrid/hybrid-product-docs.png"::: | <br> Azure 產品功能：深入瞭解在 Azure 中支援混合式和多重雲端策略的產品。 |
| <br> :::image type="icon" source="../../_images/hybrid/hybrid-skills.png"::: | <br> [Microsoft 學習課程模組](/learn/azure/)：獲得執行、維護及支援混合式和多重雲端解決方案所需的實際操作技能。 |

## <a name="common-customer-challenges-and-supporting-guidance"></a>常見的客戶挑戰和支援指導方針

**為混合式和多重雲端的集中式作業做好準備：** 複習右邊的目錄中的七個 [雲端採用架構](../../get-started/index.md) 文章。 建立跨混合式和多重雲端環境支援整個工作負載組合所需的程式和方法。

**監視現有混合式和多重雲端組合的資產：** 專注于整合作業、治理及管理文章，以將整合的作業整合到您現有的作業流程。 使用準備好的文章，將這些增強功能部署到您所有的雲端環境。

**(中央 IT) 影響個別工作負載的變更：** 隨著混合式和多重雲端控制項的改進，中央 IT 團隊將會遇到需要依賴個別工作負載背後架構知識的需求。 使用 [Azure Well-Architected 架構](/azure/architecture/framework/) 指引，協助工作負載擁有者瞭解其工作負載的潛在改進，以改善混合式和多重雲端作業。

**(工作負載小組) 優化個別工作負載：** 工作負載擁有者應該從 [Microsoft Azure Well-Architected 審核](/assessments/?id=azure-architecture-review&mode=pre-assessment) 指導方針著手，以瞭解將混合式和多重雲端策略整合至其工作負載的最佳方式。 如果中央 IT 或 CCoE 團隊也支援該團隊，則本指南將提供最佳做法和架構的見解， (中央 IT 小組通常負責提供) 來加速您的工作負載開發。

**將個別資產上線的流程：** 使用最佳做法一節來執行一連串的程式，以將所有混合式資產上架。

**執行特定的 Azure 產品：** 使用精選產品一節所述的各種 Azure 產品，加速及改善混合和多重雲端功能。
