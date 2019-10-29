---
title: Azure 創新指南：預測和影響
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 了解如何使用 Azure 進行預測和影響。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.custom: fasttrack-edit, AQC
ms.localizationpriority: high
ms.openlocfilehash: b2ed1b072d5226649e5248e350edfa3578978c4c
ms.sourcegitcommit: 910efd3e686bd6b9bf93951d84253b43d4cc82b5
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2019
ms.locfileid: "72769249"
---
::: zone target="docs"

# <a name="azure-innovation-guide-predict-and-influence"></a>Azure 創新指南：預測和影響

::: zone-end

::: zone target="chromeless"

# <a name="predict-and-influence"></a>預測和影響

::: zone-end

身為創新者，貴公司將能深入解析客戶群的資料、行為和需求。 研究這些深入解析有助於在客戶還沒察覺到之前，就先預測出他們的需求。 本文會介紹一些傳遞預測性解決方案的方法。 在稍候的章節中，本文會介紹一些方法，讓您將這些預測反向整合到用以影響客戶行為的解決方案中。

下表的設計目的，是為了協助您根據實作需求找到最佳的解決方案。

|服務  |預先建置的模型  |建置和實驗  |使用 Python 來訓練和建置|所需技能|
|---------|---------|---------|---------|---------|
|認知服務|yes|否|否|API 和開發人員技能|
|Azure Machine Learning Studio|yes|是|否|大致了解預測演算法|
|Azure Machine Learning 服務|yes|是|yes|資料科學家|

## <a name="azure-cognitive-servicestabcognitiveservices"></a>[Azure 認知服務](#tab/CognitiveServices)

Azure 認知服務是能讓您最輕鬆快速地獲取預測能力的途徑。 認知服務可根據現有模型來做出預測，而不需要額外的訓練。 如果您的員工之中沒有可以訓練預測模型的資料科學家，認知服務就是最好的服務。 某些服務並不需要訓練。 某些服務則只需要最少的訓練。

如需可用服務和所需訓練量的清單，請參閱[認知服務和機器學習](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-and-machine-learning#service-requirements-for-the-data-model)。

### <a name="action"></a>動作

若要使用認知服務 API：

1. 移至 [認知服務]  。
2. 按一下 [新增 +]  以從市集尋找認知服務。
3. 如果您知道所要使用服務的名稱，則可在 [搜尋 Marketplace]  文字方塊中輸入服務名稱。
4. 或者，如需認知服務清單，請按一下 [認知服務] 標頭旁的 [查看更多]  連結。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.CognitiveServices%2Faccounts]" submitText="Go to Cognitive Services" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

::: zone target="docs"

在 [Azure 入口網站](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.CognitiveServices%2Faccounts)中直接移至 [認知服務]。

::: zone-end

## <a name="azure-machine-learning-studiotabmachinelearningstudio"></a>[Azure Machine Learning Studio](#tab/MachineLearningStudio)

如果認知服務內的現有模型無法配合所需的預測，Azure Machine Learning Studio 可能有辦法讓您建置所需的預測，而不需要高深的資料科學家技能。

<!-- markdownlint-disable MD024 -->

### <a name="action"></a>動作

您可以使用 Azure Machine Learning Studio 來建置模型並使用該模型進行實驗：

1. 移至 [Azure Machine Learning Studio]  。
2. 按一下 [建立 Machine Learning Studio 工作區]  ，然後遵循提示來建立工作區。
3. 新的工作區會提供拖放功能介面供您建置模型並使用該模型進行實驗，以替代深度訓練。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.MachineLearning%2Fworkspaces]" submitText="Go to Azure Machine Learning Studio" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

::: zone target="docs"

在 [Azure 入口網站](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.MachineLearning%2Fworkspaces)中直接移至 [Azure Machine Learning Studio]。

::: zone-end

## <a name="azure-machine-learning-servicetabmachinelearningservice"></a>[Azure Machine Learning Service](#tab/MachineLearningService)

Azure Machine Learning 服務會提供要更加深入地訓練客戶資料集所需的更高深程式碼架構方法。 資料科學家可以使用 Python 等語言進行訓練，然後再建置演算法來預測客戶需求。

### <a name="action"></a>動作

資料科學家可以使用 Azure Machine Learning 服務，以使用 Python 之類的進階語言來訓練和建置模型：

1. 移至 [Azure Machine Learning 服務]  。
2. 按一下 [建立 Machine Learning 服務工作區]  ，然後遵循提示來建立工作區。
3. 新的工作區會為資料科學家提供以程式碼驅動的方法，以便訓練和建置需要更先進分析技術才能精確預測客戶需求的模型。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.MachineLearningServices%2Fworkspaces]" submitText="Go to Azure Machine Learning Service" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

::: zone target="docs"

在 [Azure 入口網站](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.MachineLearningServices%2Fworkspaces)中直接移至 [Azure Machine Learning Studio]。

::: zone-end

## <a name="influence"></a>影響

上述所有方法都會產生對應用程式公開預測模型的 API。 在您的解決方案中，使用上述任何一種方法將所收集到的客戶資料饋送至預測 API。 然後，您可以將結果整合到客戶體驗中，作為建議的後續步驟。

後續步驟會嘗試形塑客戶的行為模式，並影響客戶的反應。 由於建議的後續步驟是以預測演算法為基礎，因此會使用先前的客戶資料和可用資料來預測客戶需求並符合該需求，而這些通常會在客戶甚至還沒察覺到他們有此需求前發生。
