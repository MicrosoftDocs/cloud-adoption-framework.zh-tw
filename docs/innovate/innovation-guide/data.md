---
title: 資料民主化
description: 瞭解如何使用 Azure 資料目錄、Azure Data Share 和其他工具來 democratization 資料，以增強資料的探索能力與理解。
author: BrianBlanchard
ms.author: brblanch
ms.date: 01/27/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.localizationpriority: high
ms.custom: internal, fasttrack-new, AQC, seo-caf-innovate
keywords: 將大眾化、將大眾化資料、將大眾化資料、資料 democratization、大眾化
ms.openlocfilehash: cffb167db17bf28c21f4abf0324b91f1ca708f55
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101792307"
---
# <a name="democratize-data"></a>將資料大眾化

資料 democratization 是讓資訊系統的一般非技術使用者可以存取數位資訊的能力，而不需具備閘道管理員或外部存取資料的協助。 將大眾化資料可協助使用者取得重要資料的自由存取權，而不會產生妨礙生產力的瓶頸。

增強資料的可搜尋性是將資料大眾化的首要步驟之一。 編目和管理資料共用可協助企業從現有的資訊資產獲得最大價值。 藉由將大眾化資料目錄，它可以讓管理資料的使用者輕鬆地探索和瞭解資料來源。 Azure 資料目錄可讓您在企業內部進行管理，Azure Data Share 則可讓您在企業外部進行管理和共用。

提供 Azure 時間序列深入解析和串流分析等資料處理工具的 Azure 服務，則是可供客戶和合作夥伴成功使用以滿足其創新需求的其他功能。

## <a name="catalog"></a>[目錄](#tab/Catalog)

### <a name="azure-data-catalog"></a>Azure 資料目錄

Azure 資料目錄可解決資料取用者的探索挑戰，還可讓資料產生者維護資訊資產。 它可弭平 IT 和業務之間的間隙，讓每個人都能貢獻獨到的見解。 您可以將資料存放在您想要的位置，然後使用您想使用的工具進行連線。 透過 Azure 資料目錄，您可以控制誰可以探索已註冊的資料資產。 您可以利用開放的 REST API 融入現有工具和程序。

> [!div class="checklist"]
>
> - 註冊
> - 搜尋和標註
> - 連線和管理

::: zone target="docs"

**移至 [Azure 資料目錄文件](/azure/data-catalog/)**

::: zone-end

::: zone target="chromeless"

#### <a name="action"></a>動作

您只能針對每個組織使用一個 Azure 資料目錄。 如果您已為組織建立目錄，就無法新增更多目錄。

若要為組織建立目錄：

1. 移至 [Azure 資料目錄]。
2. 選取 [建立]。

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.DataCatalog%2FCatalogs]" submitText="Go to Azure Data Catalog" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

## <a name="share"></a>[共用](#tab/Share)

### <a name="azure-data-share"></a>Azure Data Share

在公開共用資料與控制所能共用的資料以及誰可共用之間取得平衡，是促成創新的重要因子。 組織嘗試將資料大眾化時，很容易會因為資料的數量、步調和生命週期而應付不過來。 Azure Data Share 確保提供者可以掌控其資料的處理方法，只要為其資料共用指定使用規定即可。 資料取用者必須先接受這些規定才能接收資料。 資料提供者可以指定資料取用者收到更新的頻率。 資料提供者可以隨時撤銷新更新的存取權。

> [!div class="checklist"]
>
> - 建立資料共用。
> - 將資料集新增至資料共用。
> - 為資料共用啟用同步處理排程。
> - 將收件者新增至資料共用。

::: zone target="docs"

**移至 [Azure Data Share 文件](/azure/data-share/)**

::: zone-end

::: zone target="chromeless"

<!-- markdownlint-disable MD024 -->

#### <a name="action"></a>動作

若要建立資料共用：

1. 移至 [Azure Data Share]。
2. 選取 [建立資料共用]。

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.DataShare%2FAccounts]" submitText="Go to Azure Data Shares" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

## <a name="insights"></a>[深入解析](#tab/Insights)

### <a name="azure-time-series-insights"></a>Azure Time Series Insights

Azure 時間序列深入解析的資料創新功能無限。 它可針對 IoT 規模的時間序列資料，提供近乎即時的資料流和多層式儲存體資料探索。 此外，還提供用於情境化原始遙測資料和衍生資產型深入解析的模型。 您可以提供與其他資料解決方案進行順暢且持續整合的功能，以及提供根本原因分析和異常偵測，包括時間序列深入解析平台上的自訂應用程式選項。

> [!div class="checklist"]
>
> - 規劃時間序列深入解析環境。
> - 在檔案總管中將資料視覺化。
> - 了解資料保留。
> - 開發和共用自訂檢視。

::: zone target="docs"

**移至 [Azure 時間序列深入解析概觀](/azure/time-series-insights/overview-what-is-tsi)**

::: zone-end

::: zone target="chromeless"

#### <a name="action"></a>動作

若要建立 Azure 時間序列深入解析環境：

1. 移至 **Azure 時間序列深入解析環境**。
2. 選取 [建立時間序列深入解析環境]。
3. 將此環境指向事件來源，也就是 Azure IoT 中樞或事件中樞。

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.TimeSeriesInsights%2FEnvironments]" submitText="Go to Azure Time Series Insights" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end
