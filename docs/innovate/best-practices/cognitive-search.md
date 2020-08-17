---
title: 什麼是 Azue 認知搜尋？
description: Azure 認知搜尋先前稱為「Azure 搜尋服務」，可協助您在編制索引期間套用 AI 流程。
author: v-hanki
ms.author: janet
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: 6a29b99c03d69ef24ecede9548f700552eecf3ea
ms.sourcegitcommit: 917188fa930cadddb03f9e9bbcdd7b630e4ee33e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/17/2020
ms.locfileid: "88279534"
---
<!-- cSpell:ignore Lucene -->

<!-- docsTest:ignore "Azure Search" "JFK Files" -->

# <a name="what-is-azure-cognitive-search"></a>什麼是 Azue 認知搜尋？

Azure 認知搜尋是一種受控的雲端解決方案，可為開發人員提供 Api 和工具，透過 web、行動和企業應用程式中的私用、異類內容來新增豐富的搜尋體驗。 您的程式碼或工具會叫用資料擷取 (索引編制) 以建立和載入索引。 您可以選擇新增認知技能，以在編製索引期間套用 AI 程序。 這樣做可以加入適用于搜尋和其他案例的新資訊和結構。

在服務的另一方面，應用程式程式碼會發出查詢要求和處理回應。 搜尋體驗會透過 Azure 認知搜尋的功能，在您的用戶端中進行定義，而查詢會針對您在服務上建立、擁有及儲存的持續性索引執行。

![顯示 Azure 認知搜尋的圖表。](../../_images/ai-cognitive-search.png)

透過簡單的 [REST API](/rest/api/searchservice/) 或 [.NET SDK](/azure/search/search-howto-dotnet-sdk) 展現功能，並同時隱蔽資訊擷取固有之複雜性。 除了 API 之外，Azure 入口網站也提供系統管理和內容管理的支援，以及建立原型和查詢索引的工具。 因為服務在雲端執行，Microsoft 會管理基礎結構和可用性。

## <a name="when-to-use-azure-cognitive-search"></a>使用 Azure 認知搜尋的時機

Azure 認知搜尋適用於下列應用程式案例：

- 將異質內容類型合併為可搜尋的單一私人索引。 查詢一律會超過您建立和載入檔的索引。 索引一律會位於您 Azure 認知搜尋實例上的雲端中。 您可以從任何來源或平台，將 JSON 文件的資料流填入索引。 或者，針對 Azure 上的內容，您可以使用「索引子」將資料提取到索引中。 索引定義和管理/擁有權是使用 Azure 認知搜尋的主要原因。
- 原始內容是大型的無差異文字、影像檔案或應用程式檔，例如 azure 資料來源上的 Microsoft Office 內容類型，例如 Azure Blob 儲存體或 Azure Cosmos DB。 您可以在編製索引期間套用認知技能，以新增結構或從影像和應用程式檔中擷取意義。
- 輕鬆實作與搜尋相關的功能。 Azure 認知搜尋 Api 簡化了查詢結構、多面向導覽、篩選 (包括地理空間搜尋) 、同義字對應、預先輸入查詢和相關性調整。 透過內建功能，您可以滿足終端使用者希望搜尋體驗能類似 Web 搜尋引擎的期望。
- 編制非結構化文字的索引，或從影像檔中解壓縮文字和資訊。 Azure 認知搜尋的 [AI 擴充](/azure/search/cognitive-search-concept-intro)功能會將 AI 處理新增至索引管線。 一些常見的使用案例包括 OCR 超過掃描的檔、實體辨識和大型檔的主要片語解壓縮、語言偵測和文字轉譯，以及情感分析。
- 使用 Azure 認知搜尋的自訂和語言分析器滿足語言需求。 如果您有非英文的內容，Azure 認知搜尋可支援 Lucene 分析器和 Microsoft 的自然語言處理器。 您也可以設定分析器來完成原始內容的特殊處理，例如篩選出變音符號。

## <a name="use-azure-cognitive-search"></a>使用 Azure 認知搜尋

### <a name="step-1-provision-the-service"></a>步驟 1：佈建服務

您可以在 [Azure 入口網站](https://portal.azure.com/) 中或透過 [Azure Resource Manager REST API](/rest/api/searchmanagement/)布建 Azure 認知搜尋實例。 您可以選擇與其他訂閱者共用的免費服務，或選擇只專用您的服務所使用之資源的付費層。 若選擇付費層，您可以從兩方面調整服務︰

- 新增複本以增加您的容量，以處理繁重的查詢負載。
- 新增資料分割以成長更多檔的儲存體。

將文件儲存體和查詢輸送量分開處理，可讓您根據生產需求而校正資源分配。

### <a name="step-2-create-an-index"></a>步驟 2：建立索引

您必須先定義 Azure 認知搜尋索引，才能上傳可搜尋的內容。 索引就像是資料庫資料表，其中保存您的資料並可接受搜尋查詢。 您可以定義要對應的索引架構，以反映您要搜尋的檔結構，類似于資料庫中的欄位。

您可以在 Azure 入口網站中建立架構，或使用 [.NET SDK](/azure/search/search-howto-dotnet-sdk) 或 [REST API](/rest/api/searchservice/)以程式設計方式建立架構。

### <a name="step-3-load-data"></a>步驟 3：載入資料

定義索引之後，您便可以上傳內容。 您可以使用推送或提取模型。

提取模型會從外部資料來源擷取資料。 這可透過索引子而達成，索引子可簡化和自動化資料擷取的各層面，例如連線至、讀取和序列化資料。 [索引子](/rest/api/searchservice/Indexer-operations) 可用於 Azure Cosmos DB、Azure SQL Database、azure Blob 儲存體，以及裝載于 Azure 虛擬機器實例中的 SQL Server。 您可以針對隨選或排程的資料重新整理設定索引子。

發送模式是透過用於將更新的文件傳送到索引的 SDK 或 REST API 提供的。 您可以使用 JSON 格式，從幾乎任何資料集推送資料。 如需詳細資訊，請參閱 [新增、更新或刪除檔](/rest/api/searchservice/addupdate-or-delete-documents) 或 [如何使用 .net SDK](/azure/search/search-howto-dotnet-sdk) 取得載入資料的指引。

### <a name="step-4-search"></a>步驟 4：搜尋

填入索引之後，您可以使用簡單的 HTTP 要求搭配[REST api](/rest/api/searchservice/Search-Documents)或[.net SDK](/dotnet/api/microsoft.azure.search.idocumentsoperations?view=azure-dotnet)，將[搜尋查詢發出](/azure/search/search-query-overview)至服務端點。 逐步建立 [您的第一個搜尋應用程式](/azure/search/tutorial-csharp-create-first-app) ，以建立並擴充可收集使用者輸入和處理結果的網頁。 您也可以使用 [Postman 進行互動式 REST](/azure/search/search-get-started-postman) 呼叫，或使用內建的 [搜尋瀏覽器](/azure/search/search-explorer) ，在 Azure 入口網站中查詢現有的索引。

## <a name="next-steps"></a>後續步驟

- 深入瞭解 [Azure 認知搜尋](/azure/search/)。
- 流覽更多 [AI 架構](/azure/architecture/browse/)。
- 請參閱 [jfk 飛往檔範例架構和方案](/azure/architecture/solution-ideas/articles/cognitive-search-with-skillsets)一文中的範例知識挖掘解決方案。