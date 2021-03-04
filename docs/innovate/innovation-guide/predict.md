---
title: Azure 創新：使用 AI 來創新
description: 了解 Azure 解決方案以預測客戶的需求、讓商務程序自動化、探索非結構化資料中的潛在資訊，並以新的方式與客戶互動，以提供更好的體驗。
author: BrianBlanchard
ms.author: brblanch
ms.date: 01/27/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.localizationpriority: high
ms.custom: internal, fasttrack-edit, AQC, seo-caf-innovate
keywords: 將商務程式自動化、ai 創新、機器學習服務、知識挖掘
ms.openlocfilehash: 1b61201bba7af957b661fa9f755350482b3db955
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101790199"
---
<!-- cSpell:ignore ONNX -->

# <a name="innovate-with-ai"></a>使用 AI 來創新

身為創新者，貴公司有豐富的商業和客戶相關資訊。 您的公司可以使用 AI 創新：

- 對客戶的需求進行預測。
- 自動化商務流程。
- 探索非結構化資料中的潛在資訊。
- 以新的方式與客戶互動，以提供更好的體驗。

 本文介紹一些使用 AI 進行創新的方法。 創新可將貴公司的商業見解擴充至現有的資料。 下表可協助您根據實作需求找到最佳的解決方案。

| 解決方案類別 | 描述                                                                                                                              | 所需技能              |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| 機器學習服務            | **Azure Machine Learning** <br> 建置、部署和管理您自己的機器學習模型。                                                       | 資料科學家與開發人員 |
| AI 應用程式和代理程式             | **Azure 認知服務** <br> 將領域專屬 AI 模型用於視覺、語音、語言，以及可使用您的資料自訂的決策。 <br><br> **Azure Bot 服務** <br> 將 Bot 新增至您的應用程式和網站，以改善客戶參與度。 | 開發人員                    |
| 知識採礦            | **Azue 認知搜尋** <br> 發掘內容中的潛在深入解析，包括文件、合約、影像和其他資料類型。      | 開發人員                    |

## <a name="machine-learning"></a>機器學習服務

Azure 提供先進的機器學習功能。 使用 Azure Machine Learning，在雲端和邊緣之間建置、定型及部署機器學習模型。 使用自動化機器學習更快地開發模型。 使用您選擇的工具和架構，不會被拘束。

如需更多詳細資訊，請參閱 [Azure Machine Learning 概觀](/azure/machine-learning/overview-what-is-azure-ml)和[開始使用您的第一個機器學習實驗](/azure/machine-learning/tutorial-1st-experiment-sdk-setup)。 如需機器學習的開放原始碼模型格式和執行階段的詳細資訊，請參閱 [ONNX Runtime](https://www.onnxruntime.ai/)。

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

## <a name="ai-applications-and-agents"></a>AI 應用程式和代理程式

Azure 提供一組稱為認知服務的預先建置 AI 服務，來建置 AI 應用程式。 此外，Azure 提供 Bot 服務，可讓開發人員建置交談式 AI 代理程式，以改善客戶和員工參與。

### <a name="ai-applications"></a>AI 應用程式

認知服務可讓您將視覺、語音、語言和決策的 AI 功能納入您的應用程式中。 大部分的預測性模型都不需要額外的訓練。 如果您的員工之中沒有可以訓練預測模型的資料科學家，這些服務會非常實用。 其他服務則只需要最少的訓練。

如需跨視覺、語音、語言和決策的可用服務清單，以及所需訓練的更多資訊，請參閱[認知服務](/azure/cognitive-services/cognitive-services-and-machine-learning#service-requirements-for-the-data-model)文件。

#### <a name="action"></a>動作

若要開始使用認知服務 API：

1. 在 Azure 入口網站中，搜尋並選取 [認知服務]。

1. 選取 [新增] 以在 Azure Marketplace 中尋找認知服務 API。

1. 搜尋並選取服務：

    - 如果您知道所要使用的服務名稱，則可在 [搜尋 Marketplace] 方塊中輸入此名稱。 接著選取服務。

    - 如需認知服務 API 的清單，請在 [認知服務] 標題旁選取 [查看更多]。 接著選取服務。

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

透過 Bot Framework 和 Azure Bot Service 提供的交談式體驗，即可更自然地與您的客戶互動，並改善客戶的參與度。 此外，請使用 Language Understanding (LUIS)、QnA Maker 和語音服務之類的認知服務 API。 這些可協助您的客戶進行一般工作，讓您的撥打電話中心代理程式有時間專注於更差別細微和更高的價值案例。

如需有關如何建置 Bot 的詳細資訊，請參閱 [Azure Bot Service](/learn/paths/create-bots-with-the-azure-bot-service/)。

#### <a name="action"></a>動作

若要開始使用 Azure Bot Service：

1. 在 Azure 入口網站中，搜尋並選取 [Bot Service]。

1. 選取 [新增]，然後選取 [Web 應用程式 Bot] 或 [Bot 通道註冊]。

1. 選取 [建立]。 接著遵循入口網站中的步驟來佈建服務。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.BotService%2FBotServices]" submitText="Go to Azure Bot Service" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

::: zone target="docs"

在 [Azure 入口網站](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.BotService%2FBotServices)中直接移至 Azure Bot Service。

::: zone-end

## <a name="knowledge-mining"></a>知識採礦

知識挖掘使用 AI 來推動大量非結構化、半結構化和結構化資訊的內容瞭解。 使用 Azure 認知搜尋從您的內容中發現潛在的深入解析，包括文件、影像和媒體。 您可以探索內容中的模式和關聯性、了解情感，以及擷取關鍵片語。

<!-- docutune:ignore "Azure Search" -->

Azure 認知搜尋會使用 Bing 和 Microsoft Office 使用的相同自然語言堆疊。 花更多時間在創新上面，而減少維護複雜雲端搜尋解決方案的時間。

如需詳細資訊，請參閱[什麼是 Azure 認知搜尋？](/azure/search/search-what-is-azure-search)

### <a name="action"></a>動作

開始進行之前：

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
