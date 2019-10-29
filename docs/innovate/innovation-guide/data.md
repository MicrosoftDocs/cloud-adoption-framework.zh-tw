---
title: Azure 創新指南：將資料大眾化
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 了解如何使用 Azure 將資料大眾化。
author: absheik
ms.author: absheik
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.custom: fasttrack-new, AQC
ms.localizationpriority: high
ms.openlocfilehash: 65c1ecd1d722286fb495af3069862131629c35d1
ms.sourcegitcommit: 0d14d89b9004a65a322724342cb5086ad2c77467
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2019
ms.locfileid: "72777080"
---
::: zone target="docs"

# <a name="azure-innovation-guide-democratize-data"></a>Azure 創新指南：將資料大眾化

::: zone-end

::: zone target="chromeless"

# <a name="democratize-data"></a>將資料大眾化

::: zone-end

增強資料的可搜尋性是將資料大眾化的首要步驟之一。 編目和管理資料共用可協助企業從現有的資訊資產獲得最大價值。 資料目錄能讓管理資料的使用者輕鬆地探索和了解資料來源。 Azure 資料目錄可讓您在企業內部進行管理，Azure Data Share 則可讓您在企業外部進行管理和共用。

提供 Azure 時間序列深入解析和串流分析等資料處理工具的 Azure 服務，則是可供客戶和合作夥伴成功利用以滿足其創新需求的其他功能。

# <a name="catalogtabcatalog"></a>[目錄](#tab/Catalog)

## <a name="azure-data-catalog"></a>Azure 資料目錄

Azure 資料目錄除了可以讓資料產生者維護資訊資產外，還可解決資料取用者的探索挑戰。 弭平 IT 和業務之間的間隙，讓每個人都能貢獻獨到的見解。 將資料存放在您指定的位置，並連結到您選擇的工具。 控制可探索註冊之資料資產的使用者。 利用開放的 REST API 融入現有工具和程序。

> [!div class="checklist"]
>
> - 註冊
> - 搜尋和標註
> - 連線和管理

::: zone target="docs"

**移至 [Azure 資料目錄](https://docs.microsoft.com/azure/data-catalog)。**

::: zone-end

::: zone target="chromeless"

### <a name="action"></a>動作

每個組織只支援使用一個 Azure 資料目錄。 如果您已為組織建立目錄，就無法新增其他目錄。

若要為組織建立 Azure 資料目錄：

1. 移至 [Azure 資料目錄]  。
2. 按一下 [ **建立** ] 按鈕。

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.DataCatalog%2Fcatalogs]" submitText="Go to Azure Data Catalog" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

# <a name="sharetabshare"></a>[共用](#tab/Share)

## <a name="azure-data-share"></a>Azure Data Share

在公開共用資料與控制所能共用的資料以及誰可共用之間取得平衡，是促成創新的重要因子。 組織在將資料大眾化時，很容易會因為這類資料的數量、步調和生命週期而應付不過來。 使用 Azure Data Share 可確保提供者可以隨時掌控其資料的處理方法，只要為其資料共用指定使用規定即可。 資料取用者必須先接受這些規定才能接收資料。 資料提供者可以指定資料取用者收到更新的頻率。 資料提供者可以隨時撤銷新更新的存取權。

> [!div class="checklist"]
>
> - 建立 Data Share。
> - 將資料集新增至 Data Share。
> - 為 Data Share 啟用同步處理排程。
> - 將收件者新增至 Data Share。

::: zone target="docs"
**移至 [Azure Data Share](https://docs.microsoft.com/azure/data-share)。**

::: zone-end

::: zone target="chromeless"

<!-- markdownlint-disable MD024 -->

### <a name="action"></a>動作

若要建立 Azure Data Share：

1. 移至 [Azure Data Share]  。
2. 按一下 [建立 Data Share]  。

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.DataShare%2Faccounts]" submitText="Go to Azure Data Share" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

使用 [Azure 開放資料集](https://docs.microsoft.com/azure/open-datasets/overview-what-are-open-datasets)來增強您的分析，方法是將[假日](https://azure.microsoft.com/services/open-datasets/catalog/public-holidays)、[天氣](https://azure.microsoft.com/services/open-datasets/catalog/noaa-global-forecast-system)和[空間影像](https://azure.microsoft.com/services/open-datasets/catalog/hls)資料併入到模型中。

這裡所提供的[將商業流程大眾化](https://docs.microsoft.com/business-applications-release-notes/october18/microsoft-flow/democratize-business-processes)和[賦予公民開發人員](https://docs.microsoft.com/business-applications-release-notes/october18/microsoft-flow/empower-citizen-developers)相關資訊是後續步驟。

# <a name="insightstabinsights"></a>[深入解析](#tab/Insights)

## <a name="azure-time-series-insights"></a>Azure Time Series Insights

針對 IoT 級別時間序列資料和模型的資料流與多層式儲存體進行近乎即時的資料探索，以將未經處理的遙測情境化，並衍生以資產為基礎的深入解析，資料創新功能永無止盡。 您可以提供與其他資料解決方案進行順暢且持續整合的功能、提供根本原因分析和異常偵測，包括時間序列深入解析平台上的自訂應用程式選項。

> [!div class="checklist"]
>
> - 規劃時間序列深入解析環境。
> - 在檔案總管中將資料視覺化。
> - 了解資料保留。
> - 開發和共用自訂檢視。

::: zone target="docs"

**移至 [Azure 時間序列深入解析](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-overview)。**

::: zone-end

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

### <a name="action"></a>動作

若要建立 Azure 時間序列深入解析環境：

1. 移至 [Azure 時間序列深入解析]  。
2. 按一下 [建立時間序列深入解析環境]  。
3. 將此環境指向事件來源，也就是 IoT 中樞或事件中樞。

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.TimeSeriesInsights%2Fenvironments]" submitText="Go to Azure Time Series Insights" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end
