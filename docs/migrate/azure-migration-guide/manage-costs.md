---
title: 聚焦在移轉的成本控制機制
description: 使用「適用於 Azure 的雲端採用架構」來了解如何設定預算、付款，並了解 Azure 資源的發票。
author: bandersmsft
ms.author: banders
ms.date: 08/08/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: fasttrack-edit, AQC
ms.localizationpriority: high
ms.openlocfilehash: 99f4b2ec0a9a92c9f919a005667558ebc2a036c6
ms.sourcegitcommit: 7d3fc1e407cd18c4fc7c4964a77885907a9b85c0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "80998021"
---
<!-- cSpell:ignore bandersmsft -->

# <a name="migration-focused-cost-control-mechanisms"></a>聚焦在移轉的成本控制機制

無論我們在技術小組中的角色為何，雲端都會對我們的工作方式帶來一些改變。 成本就是這種改變的絕佳範例。 在過去，只有財務和 IT 領導階層才會關心 IT 資產的成本 (基礎結構、應用程式和資料)。 雲端可讓每一位 IT 成員進行決策，並做出為使用者提供更佳支援的決策。 不過，伴隨著這種能力的責任是，在做出這些決策時必須要有成本意識。

本文介紹可協助您在移轉至 Azure 之前、期間和之後做出明智成本決策的工具。

本文介紹的工具包括：

> - Azure Migrate
> - Azure 定價計算機
> - Azure TCO 計算機
> - Azure 成本管理
> - Azure Advisor

要完成本文所述的程序，可能還需要與 IT 經理、財務部門或企業營運應用程式擁有者合作。

<!-- markdownlint-disable MD024 MD025 -->

# <a name="estimate-vm-costs-prior-to-migration"></a>[在移轉前預估 VM 成本](#tab/EstimateVMCosts)

在移轉任何資產 (基礎結構、應用程式或資料) 之前，您都有機會根據對這些資產觀察到的效能準則，來估計成本及調整大小。 估計成本有兩個用途：它可實現成本控制，並且可提供檢查點以確保目前的預算可支應必要的效能需求。

## <a name="cost-calculators"></a>成本計算機

針對手動成本計算，可使用兩個便利的計算機，根據要移轉的工作負載架構來提供快速成本預估。

- Azure [定價計算機](https://azure.microsoft.com/pricing/calculator)會根據手動輸入的 Azure 產品提供成本預估。
- 有時候，決策需要比較未來的雲端成本和目前的內部部署成本。 [擁有權總成本 (TCO) 計算機](https://azure.microsoft.com/pricing/tco/calculator)可提供此類比較。

這些手動成本計算機可單獨使用，以預測可能的支出和節省數額。 其也可以與 Azure Migrate 的成本預測工具搭配使用，以根據替代架構或效能限制適當調整成本期望。

## <a name="azure-migrate-calculations"></a>Azure Migrate 計算

**必要條件：** 此索引標籤的其餘部分均假設讀者已在 Azure Migrate 中填入要移轉的資產集合 (基礎結構、應用程式和資料)。 先前關於評估的文章已介紹如何收集初始資料。 填入資料之後，請依照接下來的幾個步驟，根據收集到的資料來預估每月成本。

Azure Migrate 會根據收集器和服務對應所擷取的資料來計算**每月成本預估**。 下列步驟將會載入成本預估：

1. 瀏覽至入口網站中的 [Azure Migrate 評量]。
1. 在專案的 [概觀]  頁面中，選取 [+建立評估]  。
1. 選取 [檢視全部]  以檢閱評量屬性。
1. 建立群組，並指定群組名稱。
1. 選取您想要新增至群組的機器。
1. 選取 [建立評估]  以建立群組和評估。
1. 建立評估之後，在 [概觀] > [儀表板] 中檢視該評估。
1. 在入口網站瀏覽的 [評估詳細資料] 區段中，選取[成本詳細資料]  。

產生的預估值 (如下圖所示) 可識別計算和儲存體的每月成本，這通常是雲端成本中最大的部分。

![成本詳細資料檢視](./media/manage-costs/compute-storage-monthly-cost-estimate.png)
*圖 1 - 此影像顯示 Azure Migrate 中評估的成本詳細資料檢視。*

## <a name="additional-resources"></a>其他資源

- [使用 Azure Migrate 設定和檢閱評估](https://docs.microsoft.com/azure/migrate/tutorial-assess-vmware#set-up-an-assessment)
- 如需以更完整的計劃來處理跨大量資產 (基礎結構、應用程式和資料) 的成本管理，請參閱[雲端採用架構治理模型](../../govern/guides/index.md)。 特別是，有關[成本管理專業領域](../../govern/cost-management/index.md)與[複雜企業治理指南中的成本管理改進](../../govern/guides/complex/cost-management-improvement.md)的指導方針。

# <a name="estimate-and-optimize-vm-costs-during-and-after-migration"></a>[在移轉期間和之後估計和最佳化 VM 成本](#tab/EstimateOptimize)

在移轉前估計成本，可提供可靠的成本預期目標。 此外，也可藉此機會考量要移轉的每個資產 (基礎結構、應用程式和資料) 的效能和成本需求。 不過，這仍是估計值。 一旦資產移轉並開始進行載入後，就可以根據實際或綜合負載更精確地計算成本。

## <a name="azure-advisor-cost-recommendations"></a>Azure Advisor 成本建議

在將資產 (基礎結構、應用程式和資料) 移轉至 Azure 的 24小時內，Azure Advisor 就會開始監視每個資產的效能，為您提供有關資產的意見反應。 收集到的每項意見反應都會與成本和使用率之間的平衡有關。

下列步驟提供您目前訂用帳戶內的資產 (基礎結構、應用程式和資料) 的成本建議：

1. 瀏覽至入口網站中的 [Azure Advisor]  。 若要這麼做，請在 Azure 入口網站的左側瀏覽窗格中選取 [Advisor]  。 如果在左窗格中沒有看到 [Advisor]，請選取 [所有服務]  。 在服務功能表窗格中，於 [監視與管理]  底下，選取 [Advisor]  。
2. Advisor 儀表板會顯示所有選取之訂用帳戶的建議摘要。 您可以使用訂用帳戶篩選下拉式清單，選擇您想要顯示建議的訂用帳戶。
3. 若要查看成本建議，請選取 [成本] 索引標籤。

## <a name="azure-cost-management"></a>Azure 成本管理

Azure 成本管理可以提供更全面的消費習慣檢視，包括一段時間內的成本和支出趨勢的詳細檢視。 對於大型或複雜的移轉，此檢視可以提供所需的深入解析，以進行廣泛的成本管理決策。

必要條件：此索引標籤的其餘部分會假設讀者已在完成 Azure 設定指南時完成了 Azure 成本管理的設定。 如需設定 Azure 成本管理的詳細資訊，請參閱 [Azure 設定指南中的這篇文章](../../ready/azure-setup-guide/manage-costs.md)。 填入資料之後，請依照接下來的幾個步驟，根據收集到的資料來預估每月成本。

下列步驟將為您的訂用帳戶載入 Azure 成本管理的成本分析資料：

1. 瀏覽至入口網站中的 [成本管理 + 帳單]  。 如果在左窗格中沒有看到 [成本管理 + 計費]，請選取 [所有服務]  。 在服務功能表窗格中，於 [監視與管理]  底下，選取 [成本管理 + 計費]  。
2. 在 [成本管理 + 帳單] 中，選取左側瀏覽窗格中的 [成本管理]  ，以開始分析雲端成本並將成本最佳化。
3. 在 [成本管理] 中，選取 [成本分析]  。
    a. 使用 [範圍]  框，以切換至成本分析中的不同範圍。

此分析可讓您檢閱總成本、預算 (如果有的話)，以及累計成本。 每項計算都可依服務、資源和時段來查看。 最重要的是，您可以透過標記來分析成本。 適當地命名和標記資產 (基礎結構、應用程式和資料)，是所有健全的控管和成本管理程序的基本起點。 適當的標記可提升成本管理效能，並且更清楚地顯示效能和成本最佳化的影響。

## <a name="additional-resources"></a>其他資源

- 如需以更完整的計劃來處理跨大量資產 (基礎結構、應用程式和資料) 的成本管理，請參閱[雲端採用架構治理模型](../../govern/guides/index.md)。 特別是，有關[成本管理專業領域](../../govern/cost-management/index.md)與[複雜企業治理指南中的增量成本管理改進](../../govern/guides/complex/cost-management-improvement.md)的指導方針。
- 如需 Azure Advisor 的詳細資訊，請參閱[使用 Azure Advisor 降低服務成本](https://docs.microsoft.com/azure/advisor/advisor-cost-recommendations)。
- 如需 Azure 成本管理的詳細資訊，請參閱[了解和使用範圍](https://docs.microsoft.com/azure/cost-management/understand-work-scopes)和[使用成本分析探索及分析成本](https://docs.microsoft.com/azure/cost-management/quick-acm-cost-analysis)。

# <a name="tips-and-tricks-to-optimize-costs"></a>[將成本最佳化的提示和秘訣](#tab/TipsTricks)

除了本文所述的工具之外，還有一些提示和秘訣可協助您快速降低整體雲端成本。 以下是一些應留意的簡要提示：

## <a name="avoid-unnecessary-spending"></a>避免不必要的費用

理論上，現有資料中心內的大部分資產 (基礎結構、應用程式和資料) 都可以移轉至雲端。 但這不代表您就應該這麼做。 在每個工作負載的評估期間，請驗證是否應移轉工作負載。 「雲端採用架構」一文中有關於[漸進式合理化](../../digital-estate/rationalize.md)的部分，可協助您判斷應移轉哪些資產。

## <a name="reduce-waste"></a>減少浪費

在 Azure 中部署基礎架構之後，務必確定正在使用它。 立即開始儲存最簡單的方式是檢閱您的資源，並移除未在使用的所有資源。

## <a name="reduce-overprovisioning"></a>減少過度佈建

即使採用最理想的估計方法，也可能會出現資產 (基礎結構、應用程式和資料) 過度佈建和使用量過低的情況。 使用先前兩個索引標籤中的工具來檢查這些資產，可找出能夠降低資產大小的可能方法，而更加符合效能需求並降低成本。

## <a name="take-advantage-of-available-discounts"></a>利用可用的折扣

洽詢您的 Microsoft 帳戶代表，以了解您可以如何利用目前的折扣選項。 以下是一些常用來降低成本的折扣範例。

## <a name="azure-reservations"></a>Azure 保留

[Azure 保留](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations)可讓您預付一年或三年期的虛擬機器或 SQL Database 計算容量。 預付費用可讓您在所使用的資源上取得折扣。 Azure 保留可以大幅降低虛擬機器或 SQL Database 的計算成本，透過預付一年或三年期的承諾用量費用，即可節省高達隨用隨付價格的 72%。 保留會提供計費折扣，且不會影響虛擬機器或 SQL Database 的執行階段狀態。

## <a name="use-azure-hybrid-benefit"></a>使用 Azure Hybrid Benefit

如果您在內部部署中已經有 Windows Server 或 SQL Server 授權，您可以使用 [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit) 程式儲存在 Azure 中。 利用 Windows Server 權益，每個授權都涵蓋 OS (最多兩部虛擬機器上) 的費用，而您只需支付基礎計算費用。 您可以使用現有的 SQL Server 授權，最多節省 55% 的 vCore 型 SQL Database 選項。 選項包括 Azure 虛擬機器和 SQL Server Integration Services 中的 SQL Server。

## <a name="low-priority-vms-with-batch"></a>以低優先順序的 VM 搭配 Batch

針對較低優先順序的背景程序，Batch 提供了用來管理背景服務 VM 和降低成本的方式。 不過，在選擇此折扣選項之前，請務必先了解[以低優先順序的 VM 搭配 Batch](https://docs.microsoft.com/azure/batch/batch-low-pri-vms) 的效能影響。

## <a name="additional-resources"></a>其他資源

如需以更完整的計劃來處理跨大量資產 (基礎結構、應用程式和資料) 的成本管理，請參閱[雲端採用架構治理模型](../../govern/guides/index.md)。 特別是，有關[成本管理專業領域](../../govern/cost-management/index.md)與[複雜企業治理指南中的增量成本管理改進](../../govern/guides/complex/cost-management-improvement.md)的指導方針。
