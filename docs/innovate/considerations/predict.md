---
title: 預測和影響客戶行為
description: 使用預測性模型，透過資料、見解、模式、預測和互動來開發預測功能。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: 10ff8691285bdb6db157fd1d55428167f81c1be2
ms.sourcegitcommit: 07d56209d56ee199dd148dbac59671cbb57880c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88883258"
---
# <a name="predict-and-influence"></a>預測和影響

數位經濟中有兩種類別的應用程式：歷程 *記錄* 和 *預測*性。 您可以使用歷程記錄資料（包括近乎即時的資料），來單獨滿足許多客戶需求。 大部分的解決方案主要著重于匯總資料。 然後，他們會以數位或環境體驗的形式處理資料，並將其與客戶共用。

當預測性模型變得更符合成本效益且可供使用時，客戶會要求向前思考體驗，以提供更好的決策和動作。 不過，該需求不一定會建議預測性解決方案。 在大部分的情況下，歷程記錄視圖可以提供足夠的資料，讓客戶自行決定。

可惜的是，客戶通常會採用 myopic 的觀點，根據其立即的環境和影響因素，來導向決策。 當選項和決策隨著數量和影響而成長時，myopic view 可能無法滿足客戶的需求。 同樣地，假設是以大規模的方式證明，提供解決方案的公司可以跨數千或數百萬個客戶決策來查看。 這個大圖片方法可以看到廣泛的模式，以及這些模式的影響。 如果需要瞭解這些模式，以做出最適合客戶的決策，預測能力是一種明智的投資。

## <a name="examples-of-predictions-and-influence"></a>預測和影響的範例

各種應用程式和環境經驗都使用資料來進行預測：

- **電子商務：** 電子商務網站會根據其他類似的取用者購買的產品，建議可能會新增至購物車的產品。
- **調整的現實：** IoT 提供更先進的預測功能實例。 例如，元件線上的裝置會偵測到機器的溫度上升。 以雲端為基礎的預測模型會決定如何回應。 根據該預測，其他裝置會使元件行變慢，直到電腦很酷。
- **消費者產品：** 行動電話、智慧型家裡、甚至是您的汽車，全都使用預測性功能，它們會根據類似地點或時間的因素，進行分析以建議使用者行為。 當預測和初始假設對齊時，預測會導致動作。 在非常成熟的階段中，這種調整可能會使像是自我駕駛汽車的產品成為現實。

## <a name="develop-predictive-capabilities"></a>開發預測功能

持續提供精確預測功能的解決方案通常包含五個核心特性：

- 資料
- 深入解析
- 模式
- 預測
- 互動

開發預測功能需要每個層面。 就像所有絕佳的創新一樣，預測功能的開發也需要反復專案的 [承諾](./index.md#commitment-to-iteration)。 在每個反復專案中，有一或多個下列特性經過成熟，以驗證日益複雜的客戶假設。

![預測功能的步驟](../../_images/innovate/predict-and-influence.png)

> [!CAUTION]
> 如果客戶假設是在 [組建中以客戶理解的方式](./build.md) 納入預測功能，則可能會有同樣適用的原則。 不過，預測功能需要大量的時間和能源投資。 當預測功能是 [技術尖峰](./build.md#reduce-complexity-and-delay-technical-spikes)時（相對於真實客戶價值的來源），建議您延遲預測，直到客戶假設經過大規模驗證為止。

## <a name="data"></a>資料

資料是先前所述特性的最 elemental。 開發數位發明的每個專業領域都會產生資料。 當然，這項資料會參與預測的開發。 如需有關如何將資料帶入預測性解決方案的詳細指引，請參閱 [將大眾化資料](./data.md) 和 [與裝置互動](./devices.md)。

您可以使用各種資料來源來提供預測功能：

## <a name="insights"></a>深入解析

主題專家會使用客戶需求和行為的相關資料，從原始資料研究中開發基本的商業見解。 這些深入解析可以找出所需的客戶行為 (或不想要的結果) 。 在預測的反復專案期間，這些見解有助於找出可能最終產生正面結果的潛在相互關聯。 如需啟用主題專家以開發見解的指引，請參閱 [將大眾化資料](./data.md)。

## <a name="patterns"></a>模式

人們一律嘗試偵測大量資料中的模式。 電腦是針對該目的所設計。 機器學習藉由偵測這類模式（包含機器學習模型的技能）來加速該進行。 這些模式接著會透過機器學習演算法套用，以在演算法中輸入新的資料集時預測結果。

使用深入解析作為起點，機器學習會開發並套用預測模型，以將資料中的模式變成大寫。 透過多個定型、測試和採用反復專案，這些模型和演算法可以精確地預測未來的結果。

[Azure Machine Learning](/azure/machine-learning/service/overview-what-is-azure-ml) 是 Azure 中的雲端原生服務，可根據您的資料來建立和定型模型。 這項工具也包含 [加速開發機器學習演算法的工作流程](/azure/machine-learning/service/concept-azure-machine-learning-architecture)。 此工作流程可透過視覺化介面或 Python 來開發演算法。

如需更健全的機器學習模型， [Azure HDInsight 中的 ML 服務](/azure/hdinsight/r-server/r-server-overview) 提供建置於 Apache Hadoop 叢集上的機器學習平臺。 這種方法可讓您更精細地控制基礎叢集、儲存體和計算節點。 Azure HDInsight 也透過 ScaleR 和 SparkR 等工具提供更先進的整合，以根據整合和內嵌資料來建立預測，甚至是使用資料流程中的資料。 [飛行延遲預測解決方案](/azure/hdinsight/hdinsight-hadoop-r-scaler-sparkr)會在用來根據天氣狀況預測航班延遲時，示範每一個先進的功能。 HDInsight 解決方案也允許企業控制項（例如資料安全性、網路存取和效能監視）讓模式。

## <a name="predictions"></a>預測

建立並定型模式之後，您可以透過 Api 加以套用，這可以在傳遞數位體驗期間進行預測。 這些 Api 大多是根據您資料中的模式，從經過妥善定型的模型來建立。 當更多客戶將日常工作負載部署到雲端時，雲端提供者所使用的預測 Api 會導致更快速的採用。

[Azure 認知服務](/azure/cognitive-services) 是雲端廠商所建立的預測 API 範例。 此服務包含內容仲裁的預測性 Api、異常偵測，以及個人化內容的建議。 這些 Api 已備妥可供使用，並以 Microsoft 用來定型模型的知名內容模式為基礎。 這些 Api 都會根據您送入 API 的資料進行預測。

[Azure Machine Learning](/azure/machine-learning) 可讓您部署自訂建立的演算法，而您只需根據自己的資料來建立和定型。 深入瞭解如何使用 [Azure Machine Learning](/azure/machine-learning/service/how-to-deploy-and-where)部署預測。

[設定 HDInsight](/azure/hdinsight/hdinsight-hadoop-provision-linux-clusters) 叢集會討論在 Azure HDInsight 上公開針對 ML 服務所開發之預測的流程。

## <a name="interactions"></a>互動

透過 API 提供預測之後，您就可以使用它來影響客戶行為。 這項影響會採用互動的形式。 與機器學習演算法的互動會發生在您的其他數位或環境體驗內。 透過應用程式或體驗收集資料時，它會透過機器學習演算法來執行。 當演算法預測結果時，該預測可透過現有的體驗與客戶共用。

深入瞭解如何透過 [調整的現實解決方案](./devices.md#adjusted-reality)來建立環境體驗。

## <a name="next-steps"></a>後續步驟

熟悉發明和[創新方法](./index.md)的[專業領域](./invention.md)，您現在已準備好瞭解如何以[客戶理解](./build.md)的方式打造。

> [!div class="nextstepaction"]
> [以理解的方式打造](./build.md)
