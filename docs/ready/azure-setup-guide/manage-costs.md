---
title: 管理 Azure 資源的成本和計費
description: 使用「適用於 Azure 的雲端採用架構」來了解發票，並了解如何設定 Azure 資源的預算和付款。
author: dchimes
ms.author: kfollis
ms.date: 04/09/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: fasttrack-edit, AQC, setup
ms.localizationpriority: high
ms.openlocfilehash: e7d04e91c8ec5a97d302d47ed5c52e7d58e6aae5
ms.sourcegitcommit: 84d7bfd11329eb4c151c4c32be5bab6c91f376ed
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2020
ms.locfileid: "86233132"
---
<!-- cSpell:ignore dchimes -->

# <a name="manage-costs-and-billing-for-your-azure-resources"></a>管理 Azure 資源的成本和計費

成本管理是有效規劃和控制商務相關成本的流程。 成本管理工作通常是由財務、管理和應用程式小組執行。 Azure 成本管理和計費可協助您在規劃時考量成本。 它也可協助您有效地分析成本，並採取動作來將雲端費用最佳化。

如需在整個組織中整合雲端成本管理程序的詳細資訊，請參閱「雲端採用架構」一文中有關於如何[跨業務單位、環境或專案追蹤成本](../azure-best-practices/track-costs.md)的部分。

## <a name="manage-your-costs-with-azure-cost-management-and-billing"></a>使用 Azure 成本管理和計費來管理成本

Azure 成本管理和計費提供數種方式來協助您預測和管理成本：

- **分析雲端成本**可協助您探索及分析您的成本。 您可以檢視帳戶的彙總成本，或檢視一段時間的累積成本。
- **透過預算進行監視**可讓您建立預算，然後設定警示以在即將超出預算時警告您。
- **透過建議達到最佳化**有助於識別閒置和未充分使用的資源，因此您可採取行動來減少浪費。
- **管理發票和付款**可讓您看見雲端投資。

::: zone target="docs"

### <a name="predict-and-manage-costs"></a>預測和管理成本

1. 移至 [[成本管理 + 帳單](https://portal.azure.com/#blade/Microsoft_Azure_Billing/ModernBillingMenuBlade/Overview)]。
1. 選取 [成本管理]。
1. 探索有助於分析和最佳化雲端成本的功能。

### <a name="manage-invoices-and-payment-methods"></a>管理發票和付款方式

1. 移至 [[成本管理 + 帳單](https://portal.azure.com/#blade/Microsoft_Azure_Billing/ModernBillingMenuBlade/Overview)]。
1. 從左窗格中的 [計費] 區段選取 [發票] 或 [付款方式]。

## <a name="billing-and-subscription-support"></a>計費和訂用帳戶支援

我們為 Azure 客戶提供全年無休的計費和訂用帳戶支援。 如果您在了解 Azure 使用情況方面需要協助，請建立支援要求。

### <a name="create-a-support-request"></a>建立支援要求

若要提交新的支援要求：

1. 移至[説明 + 支援](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)。
1. 選取 [新增支援要求]。

### <a name="view-a-support-request"></a>檢視支援要求

若要檢視您的支援要求及其狀態：

1. 移至[説明 + 支援](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)。
1. 選取 [所有支援要求]。

## <a name="learn-more"></a>深入了解

若要深入了解，請參閱：

- [Azure 計費與成本管理文件](https://docs.microsoft.com/azure/billing)
- [雲端採用架構：跨業務單位、環境或專案追蹤成本](../azure-best-practices/track-costs.md)
- [雲端採用架構：成本管理專業領域](../../govern/cost-management/index.md)

::: zone-end

::: zone target="chromeless"

## <a name="actions"></a>動作

**預測和管理成本：**

1. 移至 [**成本管理 + 帳單**]。
1. 選取 [成本管理]。

**管理發票和付款方式：**

1. 移至 [**成本管理 + 帳單**]。
1. 從左窗格中的 [計費] 區段選取 [發票] 或 [付款方式]。

::: form action="OpenBlade[#blade/Microsoft_Azure_Billing/ModernBillingMenuBlade/Overview]" submitText="Go to Cost Management + Billing" ::: form-end

**計費和訂用帳戶支援：**

我們為 Azure 客戶提供全年無休的計費和訂用帳戶支援。 如果您在了解 Azure 使用情況方面需要協助，請建立支援要求。

**建立支援要求：**

若要提交新的支援要求：

1. 移至 **説明 + 支援**。
2. 選取 [新增支援要求]。

**檢視支援要求：** 若要檢視您的支援要求及其狀態：

1. 移至 **説明 + 支援**。
2. 選取 [所有支援要求]。

::: form action="OpenBlade[#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview]" submitText="Go to Help + Support" ::: form-end

::: zone-end
