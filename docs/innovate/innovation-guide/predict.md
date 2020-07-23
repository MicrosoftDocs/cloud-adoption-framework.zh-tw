---
title: Azure 創新：使用 AI 來創新
description: 了解 Azure 解決方案以預測客戶的需求、讓商務程序自動化、探索非結構化資料中的潛在資訊，並以新的方式與客戶互動，以提供更好的體驗。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/26/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.custom: fasttrack-edit, AQC
ms.localizationpriority: high
ms.openlocfilehash: 74db213a6d03eaf3df75f8eed88260bb1e457cee
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86448855"
---
<!-- cSpell:ignore ONNX -->

::: zone target="docs"

# <a name="azure-innovation-guide-innovate-with-ai"></a>Azure 創新指南：使用 AI 來創新

::: zone-end

::: zone target="chromeless"

# <a name="innovate-with-ai"></a>使用 AI 來創新

::: zone-end

身為創新者，貴公司有豐富的商業和客戶相關資訊。 使用 AI，貴公司即可預測客戶的需求、讓商務程序自動化、探索非結構化資料中的潛在資訊，並以新的方式與客戶互動，以提供更好的體驗。 本文介紹一些使用 AI 進行創新的方法。 下表可協助您根據實作需求找到最佳的解決方案。

| 解決方案類別 | 描述                                                                                                                              | 所需技能              |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| 機器學習服務            | **Azure Machine Learning** <br> 建置、部署和管理您自己的機器學習模型。                                                       | 資料科學家與開發人員 |
| AI 應用程式和代理程式             | **Azure 認知服務** <br> 將領域專屬 AI 模型用於視覺、語音、語言，以及可使用您的資料自訂的決策。 <br><br> **Azure Bot 服務** <br> 將 Bot 新增至您的應用程式和網站，以改善客戶參與度。 | 開發人員                    |
| 知識採礦            | **Azue 認知搜尋** <br> 發掘內容中的潛在深入解析，包括文件、合約、影像和其他資料類型。      | 開發人員                    |

## <a name="machine-learning"></a>[機器學習服務](#tab/MachineLearning)

Azure 提供先進的機器學習功能。 使用 Azure Machine Learning，在雲端和邊緣之間快速且輕鬆地建置、定型及部署機器學習模型。 使用自動化機器學習更快地開發模型。 使用您選擇的工具和架構，不會被拘束。

如需開始使用 Azure Machine Learning 的詳細資訊，請參閱 [Azure Machine Learning 概觀](https://docs.microsoft.com/azure/machine-learning/overview-what-is-azure-ml)和[開始使用您的第一個機器學習實驗](https://docs.microsoft.com/azure/machine-learning/tutorial-1st-experiment-sdk-setup)。 如需機器學習的開放原始碼模型格式和執行階段的詳細資訊，請參閱我們的 [ONNX Runtime](http://onnxruntime.ai) 文件。

<!-- markdownlint-disable MD024 -->

### <a name="action"></a>動作

資料科學家可以使用 Azure Machine Learning，以使用進階語言 (例如 Python 和 R) 以及使用拖放視覺效果體驗，來訓練和建置模型。 若要開始使用 Azure Machine Learning：

1. 在 Azure 入口網站中，搜尋並選取 [Machine Learning]。

1. 選取 [新增]，然後遵循入口網站中的步驟來建立工作區。

1. 新的工作區同時提供低程式碼和以程式碼驅動的方法，讓資料科學家訓練、建置、部署及管理模型。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.MachineLearningServices%2FWorkspaces]" submitText="Go to Azure Machine Learning resources" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

::: zone target="docs"

在 [Azure 入口網站](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.MachineLearningServices%2FWorkspaces)中直接移至 Azure Machine Learning 資源。

::: zone-end

## <a name="ai-applications-and-agents"></a>[AI 應用程式和代理程式](#tab/AIAppsAndAgents)

Azure 提供一組稱為認知服務的預先建置 AI 服務，以輕鬆建置 AI 應用程式。 此外，Azure 提供 Bot 服務，可讓開發人員建置交談式 AI 代理程式，以改善客戶和員工參與。

### <a name="ai-applications"></a>AI 應用程式

認知服務可讓您將視覺、語音、語言和決策的 AI 功能納入您的應用程式中，而不需要針對預測模型進行額外的定型。 如果您的員工之中沒有可以訓練預測模型的資料科學家，認知服務就是最好的服務。 針對某些服務，不需要定型；其他服務只需要最少的訓練。

如需跨視覺、語音、語言和決策的可用服務清單，以及可能需要的訓練量，請參閱[認知服務](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-and-machine-learning#service-requirements-for-the-data-model)文件。

#### <a name="action"></a>動作

若要開始使用認知服務 API：

1. 在 Azure 入口網站中，搜尋並選取 [認知服務]。

1. 選取 [新增] 以在 Azure Marketplace 中尋找認知服務 API。

1. 搜尋並選取服務：

    - 如果您知道所要使用的服務名稱，則可在 [搜尋 Marketplace] 中輸入名稱，然後選取該服務。

    - 如需認知服務 API 的清單，請選取 [認知服務] 標題旁的 [查看更多]，然後選取服務。

1. 選取 [建立]，然後遵循入口網站中的步驟來佈建服務。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.CognitiveServices%2FAccounts]" submitText="Go to Cognitive Services" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

::: zone target="docs"

在 [Azure 入口網站](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.CognitiveServices%2FAccounts)中直接移至 [認知服務]。

::: zone-end

### <a name="ai-agents"></a>AI 代理程式

透過 Bot Framework 和 Azure Bot Service 提供的交談式體驗，即可更自然地與您的客戶互動，並改善客戶的參與度。 此外，使用 Language Understanding (LUIS)、QnA Maker 和語音服務之類的認知服務 API，您的客戶可以自行處理一般工作，讓您的話務中心服務專員時間專注於更細微、更高價值的案例。

如需有關如何建置 Bot 的詳細資訊，請參閱 [Azure Bot Service](https://docs.microsoft.com/learn/paths/create-bots-with-the-azure-bot-service/) 學習路徑。

#### <a name="action"></a>動作

若要開始使用 Azure Bot Service：

1. 在 Azure 入口網站中，搜尋並選取 [Bot Service]。

1. 選取 [新增]，然後選取 [Web 應用程式 Bot] 或 [Bot 通道註冊]。

1. 選取 [建立]，然後遵循入口網站中的步驟來佈建服務。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.BotService%2FBotServices]" submitText="Go to Azure Bot Service" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

::: zone target="docs"

在 [Azure 入口網站](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.BotService%2FBotServices)中直接移至 Azure Bot Service。

::: zone-end

## <a name="knowledge-mining"></a>[知識採礦](#tab/KnowledgeMining)

使用 Azure 認知搜尋從您的內容中發現潛在的深入解析，包括文件、影像和媒體。 使用唯一具備內建 AI 功能的雲端搜尋服務，探索內容中的模式和關聯性、了解情感、擷取關鍵字組等等。

<!-- docsTest:ignore "Azure Search" -->

Azure 認知搜尋 (之前稱為 Azure 搜尋服務) 使用相同的整合式 Microsoft 自然語言堆疊 (Bing 和 Microsoft Office 已使用超過十年)，以及跨視覺、語言和語音的 AI 服務。 花更多時間在創新上面，而減少維護複雜雲端搜尋解決方案的時間。

如需詳細資訊，請參閱[什麼是 Azure 認知搜尋？](https://docs.microsoft.com/azure/search/search-what-is-azure-search)

### <a name="action"></a>動作

若要開始使用 Azure 認知搜尋：

1. 在 Azure 入口網站中，搜尋並選取 [Azure 認知搜尋]。

1. 遵循入口網站中的步驟來佈建服務。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FSearchServices]" submitText="Go to Azure Cognitive Search" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

::: zone target="docs"

在 [Azure 入口網站](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FSearchServices)中直接移至 Azure 認知搜尋。

::: zone-end

---
