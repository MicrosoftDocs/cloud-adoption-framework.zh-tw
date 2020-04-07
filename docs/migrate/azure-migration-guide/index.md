---
title: Azure 移轉指南簡介
description: 使用「適用於 Azure 的雲端採用架構」，以了解如何有效遷移您的組織服務到 Azure。
author: matticusau
ms.author: mlavery
ms.date: 02/25/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: fasttrack-new, AQC
ms.localizationpriority: high
ms.openlocfilehash: 4ef888e26089a2262fadeb93ec33ed063bcbf753
ms.sourcegitcommit: 88fbc36cd634c3069e1a841a763a5327c737aa84
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/03/2020
ms.locfileid: "80636498"
---
# <a name="azure-migration-guide-before-you-start"></a>Azure 移轉指南：開始之前

[雲端採用架構的遷移方法](../index.md)會引導讀者完成遷移一個工作負載或每一版小型工作負載集合的反覆程序。 執行每個反覆作業時，我們會遵循評估、移轉、最佳化和升階的程序，以確保工作負載能夠符合生產需求。 該程序沒有雲端限定，可引導您遷移至任何雲端提供者。

本指南會示範從內部部署環境遷移至 **Azure** 時的簡化版本。

::: zone target="docs"

> [!TIP]
> 如需互動式體驗，請在 Azure 入口網站中檢視本指南。 請移至 Azure 入口網站中的 [Azure 快速入門中心](https://portal.azure.com/?feature.quickstart=true#blade/Microsoft_Azure_Resources/QuickstartCenterBlade)並選取 [將環境遷移到 Azure]  ，然後遵循逐步指示進行。

::: zone-end

## <a name="migration-tools"></a>[移轉工具](#tab/MigrationTools)

本指南是第一次遷移至 Azure 的建議路徑，我們會告訴您遷移至 Azure 期間最常使用的方法和雲端原生工具。 這些工具會顯示在下列頁面：

> [!div class="checklist"]
>
> - **評估每個工作負載的技術適用性。** 驗證移轉技術整備程度與適用性。
> - **移轉服務。** 藉由將內部部署資源複寫至 Azure 來執行實際的移轉。
> - **管理成本和計費。** 了解在 Azure 中控制成本所需的工具。
> - **最佳化及升階。** 將工作負載升階至生產環境之前，先針對成本和效能平衡進行最佳化。
> - **取得協助。** 在移轉或移轉後活動期間取得說明和支援。

我們假設您已部署登陸區域，並且已遵循[雲端採用架構就緒方法](../../ready/index.md)中的最佳做法。

## <a name="when-to-use-this-guide"></a>[本指南使用時機](#tab/WhenToUseThisGuide)

雖然本指南中討論的工具支援各種不同的移轉案例，但本指南將重點放在「複雜性最小」  的有限範圍。 若要判斷本移轉指南是否適用於您的專案，請考慮您是否符合下列情況：

- 初次進行遷移的工作負載不是任務關鍵性，也不包含敏感性資料。
- 您要移轉的是同質環境。
- 只要幾個營業單位配合，便可完成移轉。
- 將整個移轉自動化，並不在您的規畫之中。
- 您要移轉少量的伺服器。
- 要移轉的元件相依性對應很容易定義。
- 您的產業和此移轉相關的法規需求最少。

如果上述有任一條件不適用於您的情況，就應該考慮改用其他[雲端移轉的最佳作法](../azure-best-practices/index.md)。 我們也建議您向其中一個 Microsoft 小組或合作夥伴要求協助，以執行更複雜的移轉。 有 Microsoft 或經認證的合作夥伴參與其中的客戶，較能成功移轉。 要求協助的詳細資訊可在本指南取得。

<!-- markdownlint-enable MD033 -->

::: zone target="docs"

如需詳細資訊，請參閱

- [雲端移轉的最佳做法](../azure-best-practices/index.md)

::: zone-end
