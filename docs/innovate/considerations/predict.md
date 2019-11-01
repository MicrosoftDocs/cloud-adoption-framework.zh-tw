---
title: 雲端創新：預測和影響
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 雲端創新簡介-預測和影響
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: 83165e21882b4979d0fb3b104fa4f2c12aed326c
ms.sourcegitcommit: 57390e3a6f7cd7a507ddd1906e866455fa998d84
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2019
ms.locfileid: "73239461"
---
# <a name="predict-and-influence"></a>預測和影響

數位經濟中有兩個類別的應用程式：歷程記錄和預測性。 您可以使用歷程記錄資料（包括近乎即時的資料）來單獨滿足許多客戶需求。 大部分的解決方案主要著重于匯總資料。 然後他們會以數位或環境體驗的形式，處理該資料並將其分享給客戶。

當預測性模型化變得更符合成本效益且可供使用時，客戶需求的正向體驗會導致更好的決策和動作。 不過，該需求不一定會導致預測性解決方案。 在大部分的情況下，歷程記錄視圖可以提供足夠的資料，讓客戶自行決定自己的決策。

可惜的是，客戶有一個 myopic 的觀點，可以根據他們的直屬和影響球來做出決策。 隨著數位和影響的選項和決策成長，myopic view 可能不會導致客戶的需求符合。 同時，假設是大規模證明，則提供解決方案的公司可以在數千或數百萬的客戶決策中看到。 您可以看到模式和這些模式的影響。 預測性功能是一種明智的投資，需要瞭解這些模式才能做出符合客戶需求的決策。

## <a name="examples-of-predictions-and-influence"></a>預測和影響的範例

使用資料進行預測可以在各種應用程式和環境體驗中看到。

- **電子商務：** 建議的專案是預測的範例。 根據其他人一起購買的專案，網站可以建議可能值得加入購物車的產品。
- 已**調整的事實：** IoT 提供更先進的範例。 元件線上的一個裝置會偵測電腦溫度的增加。 以雲端為基礎的預測模型決定如何回應。 根據該預測，會指示另一個裝置使元件行速度變慢，直到電腦可以很酷。
- **消費產品：** 行動電話、智慧型家庭，甚至是您的汽車，都是根據諸如位置或日時間等因素來建議活動，而包含某種程度的預測功能。 當預測和初始假設一致時，這些預測就會影響您的行為。 當這兩者都變得很成熟時，預測會導致採取行動，例如自我駕駛汽車。

## <a name="developing-predictive-capabilities"></a>開發預測性功能

一致提供精確預測功能的解決方案通常包含五個核心特性：資料、深入解析、模式、預測和互動。 每個都是開發預測性功能的必要項。 如同所有絕佳的創新，預測性功能的開發也需要[反復](./index.md#commitment-to-iteration)專案的承諾。 在每個反復專案中，一或多個下列特性會成熟，以驗證日益複雜的客戶假設。

![預測性功能的步驟](../../_images/innovate/predict-and-influence.png)

> [!CAUTION]
> 如果客戶假設是從組建中開發，[而「客戶理解](./build.md)」文章包含預測性功能，則本文可能適用。 但是，預測性功能需要大量投資時間和能源。 當預測性功能為[技術尖峰](./build.md#reduce-complexity-and-delay-technical-spikes)，而不是可達成的客戶價值的來源時，建議您延遲預測，直到客戶假設已大規模驗證為止。

## <a name="data"></a>資料

上述最簡單的特性為數據。 開發數位開始創造發明的每個先前專業領域都會產生資料。 該資料全都有助於預測的開發。 如需如何將資料帶入預測性解決方案的更多指引，請參閱有關[democratizing 資料](./data.md)或[與裝置互動](./devices.md)的文章。

有數個數據源可用來提供預測性功能：

## <a name="insights"></a>深入資訊

主題專家會利用客戶需求和行為的相關資料，從原始資料的研究中開發基本的商業見解。 這些深入解析可以找出所需客戶行為的出現次數（或其他不想要的結果）。 在預測的反復專案中，這些深入解析有助於識別潛在的相互關聯，這可能會對正面結果造成影響。 如需有關啟用主題專家來開發見解的指引，請參閱有關[democratizing 資料](./data.md)的文章。

## <a name="patterns"></a>模式

人類的人在處理大量資料的模式時，本身就很困難了。 電腦是針對該目的而設計的。 機器學習服務會透過偵測大量資料的模式來加速該目的，稱為機器學習服務模型。 接著會透過機器學習演算法套用這些模式，以預測在演算法中輸入一組新的資料點時，會發生什麼事。

利用深入解析做為起點，機器學習服務會將預測模型定型並套用至資料，以將資料中的模式變成大寫。 透過多個定型、測試和採用這些模型和演算法的反復專案，可以精確地預測未來結果。

[Azure Machine Learning 服務](https://docs.microsoft.com/azure/machine-learning/service/overview-what-is-azure-ml)是 Azure 中的雲端原生工具，可根據您的資料來建立和定型模型。 此工具也包含[用於加速開發機器學習演算法的工作流程](https://docs.microsoft.com/azure/machine-learning/service/concept-azure-machine-learning-architecture)。 此工作流程可以用來開發使用視覺化介面或 Python 的演算法。

針對更健全的機器學習模型， [Azure HDInsight 中的 ML 服務](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-overview)會提供建置於 Apache Hadoop 叢集上的機器學習平臺。 這種方法可讓您更細微地控制基礎叢集、儲存體和計算節點。 利用 Azure HDInsight 也透過 ScaleR 和 SparkR 等工具提供更好的整合，以根據整合式和內嵌的資料建立預測，甚至是使用資料流程中的資料。 [航班延誤預測解決方案](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-scaler-sparkr)會示範每個先進的功能，以根據天氣條件來預測航班延誤。 HDInsight 解決方案也允許企業控制項（例如資料安全性、網路存取和效能監視）來讓模式。

## <a name="predictions"></a>預測

一旦建立並訓練了模式之後，就可以使用 Api 來套用它，這可以在數位體驗的傳遞期間進行預測。 大部分的 Api 都是根據您資料中的模式，從訓練完善的模型建立而來。 但當有更多客戶將一般工作負載部署到雲端時，雲端提供者就能夠提供常用的預測 Api，以允許更快速地採用預測。

[Azure 認知服務](https://docs.microsoft.com/azure/cognitive-services)是雲端廠商的預測 API 組建範例。 此服務包含用於內容仲裁、異常偵測或個人化內容建議的預測性 Api。 這些 Api 已準備好可供使用，根據 Microsoft 用來定型模型的一般內容模式。 每個 Api 都會根據您饋送至 API 的資料進行預測。

[Azure Machine Learning 服務](https://docs.microsoft.com/azure/machine-learning)允許部署自訂建立的演算法，而您可以根據自己的資料單獨建立和定型。 [在這裡](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where)深入瞭解如何使用 Azure Machine Learning 來部署預測。

[設定 HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-provision-linux-clusters)叢集的相關文章討論在 Azure HDInsight 上公開針對 ML 服務所開發之預測的處理常式。

## <a name="interactions"></a>互動次數

一旦透過 API 提供預測，就可以使用預測來影響客戶的行為。 這項影響的形式在於互動。 與機器學習演算法的互動會發生在您的其他數位或環境體驗內。 因為資料是透過應用程式或經驗來收集，所以該資料會透過機器學習演算法來執行。 當演算法預測結果時，該預測可以透過現有的體驗與客戶共用。

深入瞭解已[調整的現實解決方案](./devices.md#adjusted-reality)內的互動，以取得在環境體驗中建立互動的詳細資料。

## <a name="next-steps"></a>後續步驟

根據在[創新方法](./index.md)中[家發明專業領域](./invention.md)所獲得的知識，您所知的技術工具必須以[理解的方式建立](./build.md)。

> [!div class="nextstepaction"]
> [以理解的組建](./build.md)
