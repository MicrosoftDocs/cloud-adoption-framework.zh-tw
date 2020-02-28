---
title: 預測並影響客戶行為
description: 使用預測模型，透過資料、深入解析、模式、預測和互動來開發預測功能。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: e299025afcbf1066411f8d4792fe739663d46c74
ms.sourcegitcommit: 10637acba8c857a6f5aa8c4a80c0649903f60402
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/28/2020
ms.locfileid: "78171101"
---
# <a name="predict-and-influence"></a>預測和影響

數位經濟中有兩個類別的應用程式：歷程*記錄*和*預測*性。 許多客戶的需求都只能使用歷程記錄資料來滿足，包括近乎即時的資料。 大部分的解決方案主要著重于目前匯總資料。 然後他們會以數位或環境體驗的形式，處理該資料並將其分享給客戶。

當預測性模型化變得更符合成本效益且可供使用時，客戶會要求向前思考體驗，進而提供更好的決策和動作。 不過，該需求不一定會建議預測性解決方案。 在大部分情況下，歷程記錄視圖可以提供足夠的資料，讓客戶自行決定自己的決策。

可惜的是，客戶通常會採用 myopic 的觀點，根據其立即的環境和影響球來決定決策。 當選項和決策成長時，myopic view 可能無法滿足客戶的需求。 同時，假設是大規模證明，則提供解決方案的公司可以在數千或數百萬的客戶決策中看到。 這個大圖形的方法可讓您查看廣泛的模式，以及這些模式的影響。 預測性功能是一項明智的投資，因為必須瞭解這些模式，才能做出最佳服務客戶的決策。

## <a name="examples-of-predictions-and-influence"></a>預測和影響的範例

各種應用程式和環境經驗都會使用資料來進行預測：

- **電子商務：** 根據其他類似的消費者所購買的，電子商務網站會建議可能值得加入購物車的產品。
- 已**調整的事實：** IoT 提供更先進的預測功能實例。 例如，元件線上的裝置會偵測電腦溫度的增加。 以雲端為基礎的預測模型決定如何回應。 根據該預測，另一個裝置會減緩元件行，直到電腦可以很酷。
- **消費產品：** 行動電話、智慧型家庭（甚至是您的汽車）都會使用預測性功能，根據位置或當天的時間等因素來建議使用者行為。 當預測和初始假設對齊時，預測會導致採取動作。 在非常成熟的階段，這種對齊方式可以讓像是自我駕駛汽車的產品成為現實。

## <a name="develop-predictive-capabilities"></a>開發預測性功能

一致提供精確預測功能的解決方案通常包含五個核心特性：*資料*、*深入*解析、*模式*、*預測*和*互動*。 開發預測功能需要每個層面。 就像所有絕佳的創新一樣，預測性功能的開發也需要反復專案的[承諾](./index.md#commitment-to-iteration)。 在每個反復專案中，一或多個下列特性已成熟，以驗證日益複雜的客戶假設。

![預測性功能的步驟](../../_images/innovate/predict-and-influence.png)

> [!CAUTION]
> 如果客戶假設在組建中[以客戶理解的方式](./build.md)開發，其中包含預測性功能，則可能適用于此處所述的原則。 不過，預測功能需要大量投資時間和能源。 當預測性功能為[技術尖峰](./build.md#reduce-complexity-and-delay-technical-spikes)，而不是實際客戶價值的來源時，建議您延遲預測，直到客戶假設經過大規模驗證為止。

## <a name="data"></a>資料

資料是先前所述特性的最 elemental。 開發數位開始創造發明的每個專業領域都會產生資料。 當然，這項資料會提供預測的開發。 如需如何將資料帶入預測性解決方案的更多指引，請參閱[Democratizing 資料](./data.md)和[與裝置互動](./devices.md)。

您可以使用各種不同的資料來源來提供預測功能：

## <a name="insights"></a>深入解析

主題專家會使用客戶需求和行為的相關資料，從原始資料的研究中開發基本的商業見解。 這些深入解析可以找出所需客戶行為的出現次數（或者，或者不想要的結果）。 在預測的反復專案中，這些深入解析有助於識別可能會產生正面結果的可能相互關聯。 如需有關啟用主題專家來開發見解的指引，請參閱[Democratizing data](./data.md)。

## <a name="patterns"></a>模式

人們總是嘗試偵測大量資料中的模式。 電腦是針對該目的而設計的。 機器學習服務會藉由偵測精確的模式來加速該程式，這是由機器學習模型組成的技能。 這些模式接著會透過機器學習演算法來套用，以便在演算法中輸入新的資料集時預測結果。

使用深入解析做為起點，機器學習會開發並套用預測性模型，以利用資料中的模式。 透過多個定型、測試和採用反覆運算，這些模型和演算法可以精確地預測未來的結果。

[Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/overview-what-is-azure-ml)是 Azure 中的雲端原生服務，可根據您的資料來建立和定型模型。 此工具也包含[用於加速開發機器學習演算法的工作流程](https://docs.microsoft.com/azure/machine-learning/service/concept-azure-machine-learning-architecture)。 此工作流程可以用來透過視覺化介面或 Python 開發演算法。

針對更健全的機器學習模型， [Azure HDInsight 中的 ML 服務](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-overview)會提供建置於 Apache Hadoop 叢集上的機器學習平臺。 這種方法可讓您更細微地控制基礎叢集、儲存體和計算節點。 Azure HDInsight 也透過 ScaleR 和 SparkR 等工具提供更先進的整合，以根據整合式和內嵌的資料建立預測，甚至是使用資料流程中的資料。 [航班延誤預測解決方案](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-scaler-sparkr)會示範這些先進的功能，這些都是用來根據天氣條件來預測航班延誤。 HDInsight 解決方案也允許企業控制項（例如資料安全性、網路存取和效能監視）來讓模式。

## <a name="predictions"></a>預測

建立並訓練模式之後，您可以透過 Api 加以套用，這可在傳遞數位體驗期間進行預測。 大部分的 Api 都是根據您資料中的模式，從訓練完善的模型建立而來。 當有更多客戶將日常工作負載部署到雲端時，雲端提供者所使用的預測 Api 會導致採用的速度更快。

[Azure 認知服務](https://docs.microsoft.com/azure/cognitive-services)是雲端廠商所建立之預測性 API 的範例。 這種服務包含內容仲裁的預測性 Api、異常偵測，以及個人化內容的建議。 這些 Api 已準備好可供使用，並以 Microsoft 用來定型模型的知名內容模式為基礎。 每個 Api 都會根據您饋送至 API 的資料進行預測。

[Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning)可讓您部署自訂建立的演算法，而您可以根據自己的資料來建立和定型。 深入瞭解如何使用[Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where)部署預測。

[設定 HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-provision-linux-clusters)叢集討論在 Azure HDInsight 上公開針對 ML 服務所開發之預測的進程。

## <a name="interactions"></a>互動

透過 API 提供預測之後，您就可以使用它來影響客戶的行為。 這項影響會採用互動的形式。 與機器學習演算法的互動會發生在您的其他數位或環境體驗內。 透過應用程式或體驗收集資料時，會透過機器學習演算法來執行。 當演算法預測結果時，該預測可以透過現有的體驗與客戶共用。

深入瞭解如何透過已[調整的現實解決方案](./devices.md#adjusted-reality)建立環境體驗。

## <a name="next-steps"></a>後續步驟

熟悉家發明和[創新方法](./index.md)的[專業領域](./invention.md)之後，您就可以開始學習如何以客戶的理解來[打造](./build.md)。

> [!div class="nextstepaction"]
> [以理解的組建](./build.md)
