---
title: 最佳化及升階
description: 了解如何檢閱解決方案以找出最佳化的可能區域，包括解決方案的設計、調整服務的大小，以及分析成本。
author: matticusau
ms.author: mlavery
ms.date: 02/25/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: fasttrack-new, AQC
ms.localizationpriority: high
ms.openlocfilehash: 17ba2d635e702faedec3c3441c5171c88d8fc59e
ms.sourcegitcommit: 60d8b863d431b5d7c005f2f14488620b6c4c49be
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83217127"
---
<!-- markdownlint-disable MD025 DOCSMD001 -->

# <a name="release-workloads-test-optimize-and-handoff"></a>發行工作負載 (測試、最佳化和遞交)

現在您已將服務遷移至 Azure，下一個階段包括針對可能的最佳化區域審查解決方案。 此工作可能包括審查解決方案的設計、適當調整服務大小，以及分析成本。

這個階段也是將環境最佳化並執行環境可能轉換的機會。 例如，您可能已執行「重新裝載」移轉，且您的服務正在 Azure 上執行，您可以重新流覽解決方案設定或取用的服務，並可能執行「重構」來現代化和增加解決方案的功能。

本文的其餘部分著重於可最佳化已遷移工作負載的工具。 達到效能和成本的平衡後，工作負載便已準備好升階至生產環境。 如需有關升階選項的指引，請參閱[最佳化及升階](../migration-considerations/optimize/index.md)中的程序改善文章。

# <a name="right-size-assets"></a>[適當大小的資產](#tab/optimize)

所有提供以耗用量為基礎之成本模型的 Azure 服務，都可以透過 Azure 入口網站、CLI 或 PowerShell 來調整大小。 正確設定服務大小的第一個步驟是審查其使用計量。 Azure 監視器服務可供存取這些計量。 您可能需要設定所分析服務的計量集合，並容許適當的時間來根據您的工作負載模式收集有意義的資料。

1. 移至 [監視器]  。
1. 選取 [計量]  ，並設定圖表以顯示要分析的服務計量。

::: zone target="chromeless"

::: form action="OpenBlade[#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/metrics]" submitText="Go to Monitor" :::

::: zone-end

以下是一些您可調整大小的常用服務。

## <a name="resize-a-virtual-machine"></a>調整虛擬機器大小

Azure Migrate 會在其預先移轉評定階段中執行適當大小分析，而使用此工具遷移的虛擬機器可能已根據您的預先移轉需求進行大小調整。

不過，對於使用其他方法建立或遷移的虛擬機器，或者在移轉後虛擬機器需求需要調整的情況下，您可能會想要進一步調整虛擬機器的大小。

1. 移至 [虛擬機器]  。
1. 然後從清單中選取所需的虛擬機器。
1. 從清單中選取 [大小]  和所需的新大小。 您可能需要調整篩選條件，找出您所需的大小。
1. 選取 [調整大小]  。

調整生產虛擬機器的大小可能會導致服務中斷。 請先嘗試為您的 VM 套用正確的大小，再將其升階至生產環境。

::: zone target="chromeless"

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Compute%2FVirtualMachines]" submitText="Go to Virtual Machines" :::

::: zone-end

::: zone target="docs"

- [管理 Azure 資源的保留](https://docs.microsoft.com/azure/billing/billing-manage-reserved-vm-instance)
- [調整 Windows VM 大小](https://docs.microsoft.com/azure/virtual-machines/windows/resize-vm)
- [使用 Azure CLI 調整 Linux 虛擬機器大小](https://docs.microsoft.com/azure/virtual-machines/linux/change-vm-size)

合作夥伴可以使用合作夥伴中心來審查使用方式。

- [針對最大保留使用量進行 Microsoft Azure VM 大小調整](https://docs.microsoft.com/partner-center/azure-usage)

::: zone-end

## <a name="resize-a-storage-account"></a>調整儲存體帳戶大小

1. 移至 [儲存體帳戶]  。
1. 選取所需的儲存體帳戶。
1. 選取 [設定]  並調整儲存體帳戶的屬性，以符合您的需求。
1. 選取 [儲存]  。

::: zone target="chromeless"

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Storage%2FStorageAccounts]" submitText="Go to Storage Accounts" :::

::: zone-end

## <a name="resize-a-sql-database"></a>調整 SQL 資料庫大小

1. 移至 [SQL 資料庫]  或 [SQL 資料庫]  然後選取伺服器。
1. 選取所需的資料庫。
1. 選取 [設定]  和所需的新服務層大小。
1. 選取 [套用]  。

::: zone target="chromeless"

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Sql%2FServers%2FDatabases]" submitText="Go to SQL Databases" :::

::: zone-end

# <a name="cost-management"></a>[成本管理](#tab/ManageCost)

請務必執行持續成本分析和審查。 此工作讓您有機會視需要調整資源大小，以平衡成本和工作負載。

Azure 成本管理可搭配 Azure Advisor，提供成本最佳化建議。 Azure Advisor 可協助您透過識別閒置及使用量過低的資源來最佳化及改善效率。

1. 選取 [成本管理 + 計費]  。
1. 選取 [Advisor 建議]  和 [成本]  索引標籤。
1. 使用 [影響]  和 [每年可能的節省金額]  來檢閱潛在權益。

::: zone target="chromeless"

::: form action="OpenBlade[#blade/Microsoft_Azure_Billing/ModernBillingMenuBlade/Overview]" submitText="Go to Cost Management + Billing" :::

::: zone-end

您也可以使用 [Advisor]  並選取 [成本]  索引標籤，以找出潛在成本降低的建議。

> [!TIP]
> 對於不需要持續可用性的服務，視需要實作解決方案來啟動、停止或暫停服務有助於管理成本 (例如，Azure 虛擬機器或 Azure SQL 資料倉儲)。
>

::: zone target="chromeless"

::: form action="OpenBlade[#blade/Microsoft_Azure_Expert/AdvisorBlade]" submitText="Go to Azure Advisor" :::

::: zone-end

::: zone target="docs"

- [教學課程：透過建議最佳化成本](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-acm-opt-recommendations)
- [使用 Azure 計費與成本管理避免非預期的費用](https://docs.microsoft.com/azure/billing/billing-getting-started)
- [使用成本分析探索及分析成本](https://docs.microsoft.com/azure/cost-management/quick-acm-cost-analysis)

::: zone-end
