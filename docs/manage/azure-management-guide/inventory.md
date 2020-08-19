---
title: Azure 中的清查和可見性
description: 了解可針對庫存執行狀態提供清查和可見性的工具，以便用於收集操作資料。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: fasttrack-edit, AQC
ms.localizationpriority: high
ms.openlocfilehash: e82727e6d8f90ecf3681ff8a85096088b4a6de70
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88572182"
---
# <a name="inventory-and-visibility-in-azure"></a>Azure 中的清查和可見性

「清查和可見性」是三個雲端管理基準專業領域中的第一個。

![雲端管理基準](../../_images/manage/management-baseline.png)

這個專業領域之所以位列第一，原因是您在制定作業相關決策時，能否正確收集操作資料至關重要。 雲端管理小組必須了解所管理的項目，以及這些資產的運作情形有多良好。 本文會說明各種可針對庫存執行狀態來提供清查和可見性的工具。

針對任何企業級環境，下表會概述管理基準的最低建議。

| Process | 工具 | 目的 |
|---|---|---|
| 監視 Azure 服務的健康情況 | Azure 服務健康狀態 | Azure 中所執行服務的健康情況、效能和診斷 |
| 記錄集中化 | Log Analytics | 適用於所有可見性用途的集中式記錄 |
| 監視集中化 | Azure 監視器 | 操作資料和趨勢的集中監視 |
| 虛擬機器清查和變更追蹤 | Azure 變更追蹤和清查 | 清查 VM 和監視來賓作業系統層級的變更 |
| 訂用帳戶監視 | Azure 活動記錄檔 | 監視訂用帳戶層級的變更 |
| 來賓作業系統監視 | 適用於 VM 的 Azure 監視器 | 監視 VM 的變更和效能 |
| 網路監視 | Azure 網路監看員 | 監視網路變更和效能 |
| DNS 監視 | DNS 分析 | DNS 的安全性、效能和作業 |

::: zone target="docs"

## <a name="azure-service-health"></a>Azure 服務健康狀態

::: zone-end
::: zone target="chromeless"

## <a name="azure-service-health"></a>[Azure 服務健康狀態](#tab/AzureServiceHealth)

::: zone-end

Azure 服務健康狀態會提供您 Azure 服務和區域的健康情況個人化檢視。 作用中問題的相關資訊會發佈到「Azure 服務健康狀態」，協助您了解資源的影響。 定期更新可讓您立即掌握問題已解決的資訊。

我們也會將計劃性維護事件發佈至「Azure 服務健康狀態」，讓您能夠知道可能影響資源可用性的變更。 設定「服務健康狀態」警示，以便獲知可能影響您 Azure 服務和區域的服務問題、計劃性維護或其他變更。

Azure 服務健康狀態包含：

- **Azure 狀態：** Azure 服務健康狀態的全域檢視。
- **服務健康狀態：** Azure 服務健康狀態的個人化檢視。
- **資源健康狀態：** 個別資源健康狀態的更深入檢視。

::: zone target="chromeless"

<!-- markdownlint-disable MD024 -->

### <a name="action"></a>動作

若要設定服務健康狀態警示：

1. 請移至 [服務健康狀態]。
2. 選取 [健康狀態警示]。
3. 建立服務健康狀態警示。

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/healthalerts]" submitText="Go to Service Health" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

::: zone target="docs"

若要設定服務健康狀態警示，請移至 [Azure 入口網站](https://portal.azure.com/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/healthalerts)。

### <a name="learn-more"></a>深入了解

如需詳細資訊，請參閱 [Azure 服務健康情況](/azure/service-health)。

## <a name="log-analytics"></a>Log Analytics

::: zone-end
::: zone target="chromeless"

## <a name="log-analytics"></a>[Log Analytics](#tab/Log-Analytics)

::: zone-end

[Log Analytics 工作區](/azure/azure-monitor/learn/quick-create-workspace)是用來儲存 Azure 監視器記錄資料的唯一環境。 每個工作區都有自己的資料存放庫和設定。 資料來源和解決方案會設定為將其資料儲存在特定工作區中。 Azure 監視解決方案會要求所有伺服器都必須連線到工作區，以便能夠儲存和存取其記錄資料。

::: zone target="chromeless"

### <a name="action"></a>動作

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.OperationalInsights%2FWorkspaces]" submitText="Explore Azure Monitor" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

::: zone target="docs"

### <a name="learn-more"></a>深入了解

若要深入了解，請參閱 [Log Analytics 工作區建立文件](/azure/azure-monitor/learn/quick-create-workspace)。

## <a name="azure-monitor"></a>Azure 監視器

::: zone-end
::: zone target="chromeless"

## <a name="azure-monitor"></a>[Azure 監視器](#tab/Azure-Monitor)

::: zone-end

Azure 監視器針對 Azure 中的所有監視和診斷資料提供了單一的整合中樞，可讓您檢視全部資源。 透過 Azure 監視器，您將可找出並修正問題，進而將效能最佳化。 您也可以了解客戶的行為。

- **監視計量並以視覺化方式呈現。** 計量是可從 Azure 資源取得的數值。 這些資訊可協助您了解系統的健康情況。 請自訂儀表板的圖表並使用活頁簿來提供報告。

- **查詢和分析記錄。** 記錄包含來自 Azure 的活動記錄和診斷記錄。 從適用於雲端或內部部署資源的其他監視和管理解決方案收集其他記錄。 Log Analytics 可作為彙總所有這些資料的中央存放庫。 您可以從該處執行查詢來協助針對問題進行疑難排解，也可以將資料視覺化。

- **設定警示和動作。** 警示會通知您重大情況。 您可以根據來自計量、記錄或服務健康情況問題的觸發程序來採取矯正措施。 您可以設定不同的通知和動作，也可以將資料傳送至 IT 服務管理工具。

::: zone target="chromeless"

### <a name="action"></a>動作

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/overview]" submitText="Explore Azure Monitor" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

::: zone target="docs"

 開始監視：

- [應用程式](/azure/application-insights/app-insights-overview)
- [容器](/azure/monitoring/monitoring-container-overview)
- [虛擬機器](/azure/monitoring/monitoring-service-map)
- [網路](/azure/networking/network-monitoring-overview)

若要監視其他資源，請尋找 Azure Marketplace 中的其他解決方案。

若要探索 Azure 監視器，請移至 [Azure 入口網站](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/overview)。

### <a name="learn-more"></a>深入了解

若要深入了解，請參閱 [Azure 監視器文件](/azure/monitoring-and-diagnostics)。

## <a name="onboard-solutions"></a>讓解決方案上線

::: zone-end
::: zone target="chromeless"

## <a name="onboard-solutions"></a>[讓解決方案上線](#tab/Configure-solutions)

::: zone-end

若要啟用解決方案，您必須設定 Log Analytics 工作區。 讓 Azure VM 和內部部署伺服器上線，便可從其所連線的 Log Analytics 工作區取得解決方案。

有兩種方式可以上線：

- [單一 VM](../../manage/azure-server-management/onboard-single-vm.md)
- [整個訂用帳戶](../../manage/azure-server-management/onboard-at-scale.md)

每篇文章都會引導您完成一系列的步驟，以將下列解決方案上線：

- 更新管理
- 變更追蹤與詳細目錄
- Azure 活動記錄檔
- Azure Log Analytics 代理程式健全狀況
- 反惡意程式碼評估
- 適用於 VM 的 Azure 監視器
- Azure 資訊安全中心

上述每一個步驟都有助於建立清查和可見性。
