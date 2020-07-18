---
title: 什麼是 Azue 認知搜尋？
description: Azure 認知搜尋（先前稱為 Azure 搜尋服務）可讓您在編制索引期間套用 AI 程式。
author: v-hanki
ms.author: janet
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: 61a59170d26f8cb3521fd82141a26a1050e3d33d
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86451767"
---
<!-- cSpell:ignore Lucene -->

<!-- docsTest:ignore "Azure Search" "JFK Files" -->

# <a name="what-is-azure-cognitive-search"></a>什麼是 Azue 認知搜尋？

Azure 認知搜尋（之前稱為 Azure 搜尋服務）是一個受控雲端解決方案，可為開發人員提供 Api 和工具，以在 web、行動和企業應用程式中，為私人、異類內容新增豐富的搜尋體驗。 您的程式碼或工具會叫用資料擷取 (索引編制) 以建立和載入索引。 您可以選擇新增認知技能，以在編製索引期間套用 AI 程序。 這麼做可以新增對搜尋和其他案例有幫助的新資訊和結構。

在服務的另一方面，應用程式程式碼會發出查詢要求和處理回應。 搜尋體驗會透過 Azure 認知搜尋的功能，在您的用戶端中進行定義，而查詢會針對您在服務上建立、擁有及儲存的持續性索引執行。

![認知搜尋圖表](../../_images/ai-cognitive-search.png)

透過簡單的 [REST API](https://docs.microsoft.com/rest/api/searchservice/) 或 [.NET SDK](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk) 展現功能，並同時隱蔽資訊擷取固有之複雜性。 除了 API 之外，Azure 入口網站也提供系統管理和內容管理的支援，以及建立原型和查詢索引的工具。 因為服務在雲端執行，Microsoft 會管理基礎結構和可用性。

## <a name="when-to-use-azure-cognitive-search"></a>使用 Azure 認知搜尋的時機

Azure 認知搜尋適用於下列應用程式案例：

- 將異質內容類型合併為可搜尋的單一私人索引。 查詢一律會針對您搭配文件建立和載入的索引進行，而且索引一律會位於 Azure 認知搜尋服務的雲端中。 您可以從任何來源或平台，將 JSON 文件的資料流填入索引。 或者，針對 Azure 上的內容，您可以使用「索引子」將資料提取到索引中。 索引定義和管理/擁有權是使用 Azure 認知搜尋的主要原因。
- 原始內容是大型的無差異文字、影像檔案或應用程式檔，例如 azure 資料來源（例如 Azure Blob 儲存體或 Azure Cosmos DB）上 Microsoft Office 內容類型。 您可以在編製索引期間套用認知技能，以新增結構或從影像和應用程式檔中擷取意義。
- 輕鬆實作與搜尋相關的功能。 Azure 認知搜尋 Api 簡化了查詢結構、多面向導覽、篩選（包括地理空間搜尋）、同義字對應、預先輸入查詢，以及相關性調整。 透過內建功能，您可以滿足終端使用者希望搜尋體驗能類似 Web 搜尋引擎的期望。
- 編製非結構化文字的索引，或從影像檔擷取文字和資訊。 Azure 認知搜尋的 [AI 擴充](https://docs.microsoft.com/azure/search/cognitive-search-concept-intro)功能會將 AI 處理新增至索引管線。 一些常見的使用案例，包括已掃描文件的 OCR、大型文件的實體辨識和關鍵片語擷取、語言偵測和文字轉譯及情感分析。
- 使用 Azure 認知搜尋的自訂和語言分析器滿足語言需求。 如果您有非英文的內容，Azure 認知搜尋可支援 Lucene 分析器和 Microsoft 的自然語言處理器。 您也可以設定分析器來完成原始內容的特殊處理，例如篩選出變音符號。

## <a name="how-to-use-azure-cognitive-search"></a>Azure 認知搜尋的使用方式

### <a name="step-1-provision-service"></a>步驟 1:佈建服務

您可以在[Azure 入口網站](https://portal.azure.com/)中或透過[Azure Resource Manager REST API](https://docs.microsoft.com/rest/api/searchmanagement/)來布建 Azure 認知搜尋服務。 您可以選擇與其他訂閱者共用的免費服務，或選擇只為您的服務提供專用資源的付費層。 若選擇付費層，您可以從兩方面調整服務︰

- 新增複本以增加您的容量，以處理繁重的查詢負載。
- 新增資料分割以增加更多檔的儲存空間。

將文件儲存體和查詢輸送量分開處理，可讓您根據生產需求而校正資源分配。

### <a name="step-2-create-index"></a>步驟 2:建立索引

您必須先定義 Azure 認知搜尋索引，才能上傳可搜尋的內容。 索引就像是資料庫資料表，其中保存您的資料並可接受搜尋查詢。 您可定義索引結構描述，以反映您要搜尋的文件結構 (類似於資料庫中的欄位)。

您可以在 Azure 入口網站中建立結構描述，或使用 [.NET SDK](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk) 或 [REST API](https://docs.microsoft.com/rest/api/searchservice/) 以程式設計方式建立結構描述。

### <a name="step-3-load-data"></a>步驟 3：載入資料

定義索引之後，您便可以上傳內容。 您可以使用推送或提取模型。 提取模型會從外部資料來源擷取資料。 這可透過索引子而達成，索引子可簡化和自動化資料擷取的各層面，例如連線至、讀取和序列化資料。 [索引子](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)適用于 azure VM 中裝載的 Azure Cosmos DB、Azure SQL Database、azure Blob 儲存體和 SQL Server。 您可以針對隨選或已排程的資料重新整理設定索引子。 推送模式是透過 SDK 或 REST API 來提供，可用來將已更新的文件傳送到索引。 使用 JSON 格式，您幾乎可以從任何資料集發送資料。 如需詳細資訊，請參閱[新增、更新或刪除檔](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents)或[如何使用 .net SDK](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)取得載入資料的指引。

### <a name="step-4-search"></a>步驟 4：搜尋

填入索引後，您可以透過 [REST API](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) 或 [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations?view=azure-dotnet)，使用簡單的 HTTP 要求對服務端點[發出搜尋查詢](https://docs.microsoft.com/azure/search/search-query-overview)。 逐步執行建立[您的第一個搜尋應用程式](https://docs.microsoft.com/azure/search/tutorial-csharp-create-first-app)，以建立並擴充會收集使用者輸入和處理結果的網頁。 您也可以使用[Postman 來進行互動式 REST](https://docs.microsoft.com/azure/search/search-get-started-postman)呼叫，或在 Azure 入口網站內建的[搜尋瀏覽器](https://docs.microsoft.com/azure/search/search-explorer)來查詢現有的索引。

## <a name="next-steps"></a>後續步驟

- 深入瞭解[Azure 認知搜尋](https://docs.microsoft.com/azure/search/)
- 流覽更多[AI 架構](https://docs.microsoft.com/azure/architecture/browse/)
- 查看範例知識挖掘解決方案： [jfk 飛往檔範例架構和解決方案](https://docs.microsoft.com/azure/architecture/solution-ideas/articles/cognitive-search-with-skillsets)
