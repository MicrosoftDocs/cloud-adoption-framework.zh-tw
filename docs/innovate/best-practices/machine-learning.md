---
title: 什麼是機器學習服務？
description: 什麼是機器學習服務？
author: v-hanki
ms.author: janet
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: df003da24bc539dceb4ee31ebb4137dc83174de4
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86451763"
---
<!-- cSpell:ignore scikit RLlib Jupyter MLflow Kubeflow -->

# <a name="what-is-machine-learning"></a>什麼是機器學習？

機器學習是一項資料科學技術，可讓電腦使用現有資料來預測未來的行為、結果和趨勢。 使用機器學習，電腦不需要明確進行程式設計就能學習。

機器學習服務的預測或預測可以讓應用程式和裝置更聰明。 例如，當您線上購物時，機器學習服務可根據您已經購買的產品，協助推薦其他您可能會想要的產品。 或是當您的信用卡被刷過時，機器學習服務可將該筆交易與交易資料庫進行比對，協助偵測詐騙。 而且，當您的吸塵器機器人清潔房間時，機器學習服務可協助它判斷作業是否已完成。

## <a name="machine-learning-tools-to-fit-each-task"></a>適用於每個工作的機器學習工具

Azure Machine Learning 為開發人員和資料科學家提供其機器學習工作流程需要的所有工具，包括：

- [Azure Machine Learning 設計工具（預覽）](https://docs.microsoft.com/azure/machine-learning/tutorial-designer-automobile-price-train-score)：拖放模組以建立您的實驗，然後部署管線
- Jupyter 筆記本：使用我們的[範例筆記本](https://github.com/Azure/MachineLearningNotebooks)，或建立您自己的筆記本來使用我們的 SDK for Python 範例。
- R 指令碼或筆記本，您可以在其中使用[適用於 R 的 SDK](https://azure.github.io/azureml-sdk-for-r/reference/index.html) 來撰寫您自己的程式碼，或在設計工具中使用 R 模組。
- [許多模型解決方案加速器（預覽）](https://github.com/microsoft/solution-accelerator-many-models)都是以 Azure Machine Learning 為基礎，可讓您定型、操作及管理上百個或甚至數千個機器學習模型。
- [Visual Studio Code 延伸](https://docs.microsoft.com/azure/machine-learning/tutorial-setup-vscode-extension)模組。
- [機器學習服務 CLI](https://docs.microsoft.com/azure/machine-learning/reference-azure-machine-learning-cli)。
- 開放原始碼架構，例如 PyTorch、TensorFlow 及 scikit-learn 等等。
- 使用光線 RLlib 的[增強式學習](https://docs.microsoft.com/azure/machine-learning/how-to-use-reinforcement-learning)。

您甚至可以使用[MLflow 來追蹤計量，以及部署模型](https://docs.microsoft.com/azure/machine-learning/how-to-use-mlflow)或[Kubeflow](https://www.kubeflow.org/docs/azure/)以建立端對端工作流程管線。

## <a name="build-machine-learning-models-in-python-or-r"></a>以 Python 或 R 建立機器學習模型

使用 Azure Machine Learning [Python SDK](https://docs.microsoft.com/python/api/overview/azure/ml/?view=azure-ml-py) 或 [R SDK](https://azure.github.io/azureml-sdk-for-r/reference/index.html)，開始訓練您的本機電腦。 然後可以擴增至雲端。 透過許多可用的[計算目標](https://docs.microsoft.com/azure/machine-learning/how-to-set-up-training-targets)（例如 Azure Machine Learning 計算和[Azure Databricks](https://docs.microsoft.com/azure/databricks/scenarios/what-is-azure-databricks)，以及使用[advanced 超參數微調服務](https://docs.microsoft.com/azure/machine-learning/how-to-tune-hyperparameters)），您可以更快速地使用雲端的功能來建立更好的模型。 您也可以使用 SDK，[自動進行模型定型和微調](https://docs.microsoft.com/azure/machine-learning/tutorial-auto-train-models)。

## <a name="build-machine-learning-models-with-no-code-tools"></a>使用無程式碼工具建立機器學習模型

若要進行無程式碼或低程式碼的訓練和部署，請嘗試：

- Azure Machine Learning 設計工具 (預覽)

  使用設計工具來準備資料、定型、測試、部署、管理和追蹤機器學習模型，而不需撰寫任何程式碼。 不需要設計程式，就能以視覺化方式連線資料集和模組來建構模型。 試用[設計工具教學課程](https://docs.microsoft.com/azure/machine-learning/tutorial-designer-automobile-price-train-score)。

  若要深入瞭解，請參閱[Azure Machine Learning 設計工具總覽文章](https://docs.microsoft.com/azure/machine-learning/concept-designer)。
- 自動化機器學習（AutoML） UI

  瞭解如何在便於使用的介面中建立[AutoML 實驗](https://docs.microsoft.com/azure/machine-learning/tutorial-first-experiment-automated-ml)。

## <a name="mlops-deploy-and-lifecycle-management"></a>MLOps：部署與生命週期管理

Machine Learning 作業（MLOps）是以[DevOps](https://azure.microsoft.com/overview/what-is-devops/)原則和實務為基礎，可提高工作流程的效率。 例如，持續整合、傳遞和部署。 MLOps 會將這些原則套用至機器學習程式，其目標為：

- 更快速地測試和開發模型
- 更快速地將模型部署至生產環境
- 品質保證

當您有正確的模型時，您可以在 Web 服務中、在 IoT 裝置上或從 Power BI 輕鬆使用它。 如需詳細資訊，請參閱有關[如何部署和部署位置](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-and-where)的文章。

接著，您可以使用[適用於 Python 的 Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/?view=azure-ml-py)、[Azure Machine Learning Studio](https://ml.azure.com/) 或[機器學習 CLI](https://docs.microsoft.com/azure/machine-learning/reference-azure-machine-learning-cli) 來管理所部署的模型。

這些模型可以使用，並[即時](https://docs.microsoft.com/azure/machine-learning/how-to-consume-web-service)或[非同步](https://docs.microsoft.com/azure/machine-learning/how-to-use-parallel-run-step)地傳回大量資料的預測。

另外，透過進階[機器學習管線](https://docs.microsoft.com/azure/machine-learning/concept-ml-pipelines)，您可以在資料準備、模型定型與評估及部署的每個步驟上共同作業。 管線可讓您執行下列作業：

- 自動化雲端中的端對端機器學習程序
- 重複使用元件，並且只在需要時重新執行步驟
- 在每個步驟中使用不同的計算資源
- 執行批次評分工作

如果您想要使用指令碼來自動化機器學習工作流程，[機器學習 CLI](https://docs.microsoft.com/azure/machine-learning/reference-azure-machine-learning-cli) 會提供命令列工具來執行一般工作，例如提交定型回合或部署模型。

若要開始使用 Azure Machine Learning，請參閱以下的[後續步驟](https://docs.microsoft.com/azure/machine-learning/overview-what-is-azure-ml#next-steps)。

## <a name="automated-machine-learning"></a>自動化 Machine Learning

資料科學家會花費相當長的時間，在實驗階段中逐一查看模型。 在建立可接受的模型之前，嘗試不同的演算法和超參數組合的整個程式，對於資料科學家而言極為困難，因為工作的單調和非挑戰性的本質。 雖然這是在模型效力方面產生大量增益的練習，但有時候成本可能會因為時間和資源而變得太多，因此投資報酬率（ROI）可能會有負面報酬。

這是自動化機器學習（AutoML）的進入位置。 它會使用概率 matrix 分解上的研究檔中的概念，並根據所呈現資料的啟發學習法，執行自動化的管線來嘗試以智慧方式選取的演算法和 hypermeter 設定，並將其納入考慮，並將重點放在特定的問題或案例中。 此管線的結果是一組最適合指定問題和資料集的模型。

如需 AutoML 的詳細資訊，請參閱[使用 Azure Machine Learning 的 AutoML 和 MLOps](https://azure.microsoft.com/blog/automated-machine-learning-and-mlops-with-azure-machine-learning/)。

## <a name="responsible-ml"></a>負責任的 ML

在整個開發和使用 AI 系統的同時，信任是最重要的核心。 包括對平台、流程和模型的信任。 當 AI 和自發系統將更多整合到社會結構時，請務必主動設法預測並減輕這些技術的非預期結果。

- 瞭解您的模型並建立公平：說明模型行為，並找出對預測有最大影響的功能。 在模型定型和推斷期間，使用內建的 explainers 做為半透明和黑色箱模型。 使用互動式視覺效果來比較模型並執行假設分析，以改善模型的精確度。 使用最先進的演算法來測試模型的公平。 在機器學習生命週期中降低 unfairness、比較緩和的模型，並視需要做出刻意公平與精確度取捨。
- 保護資料隱私權和機密性：使用差異隱私權的最新創新來保存隱私權的組建模型，這會在資料中插入精確的統計雜訊層級，以限制機密資訊的洩漏。 識別資料流程失，並以智慧方式限制重複查詢，以管理暴露風險。 使用加密和機密機器學習（即將推出）專為機器學習設計的技術，以使用機密資料安全地建立模型。
- 控制和控制機器學習程式的每個步驟：存取內建功能來自動追蹤歷程，並在機器學習服務生命週期中建立 audit 試用版。 藉由追蹤資料集、模型、實驗、程式碼等等，取得機器學習程式的完整可見度。 使用自訂標籤來執行模型資料工作表、檔索引鍵模型中繼資料、提高責任，以及確保負責處理。

深入瞭解如何執行負責的[ML](https://docs.microsoft.com/azure/machine-learning/concept-responsible-ml)。

## <a name="integration-with-other-services"></a>與其他服務整合

Azure Machine Learning 可與 Azure 平台上的其他服務搭配運作，也可以與 Git 和 MLFlow 等開放原始碼工具整合。

- 計算目標，例如 Azure Kubernetes Service、Azure 容器執行個體、Azure Databricks、Azure Data Lake Analytics 和 Azure HDInsight。 如需計算目標的詳細資訊，請參閱[什麼是計算目標？](https://docs.microsoft.com/azure/machine-learning/concept-compute-target)。
- Azure 事件格線。 如需詳細資訊，請參閱[取用 Azure Machine Learning 事件](https://docs.microsoft.com/azure/machine-learning/how-to-use-event-grid)。
- Azure 監視器。 如需詳細資訊，請參閱[監視 Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/monitor-azure-machine-learning)。
- 資料存放區，例如 Azure 儲存體帳戶、Azure Data Lake Storage、Azure SQL Database、適用於 PostgreSQL 的 Azure 資料庫和 Azure 開放資料集。 如需詳細資訊，請參閱[存取 Azure 儲存體 services 中的資料](https://docs.microsoft.com/azure/machine-learning/how-to-access-data)和[使用 Azure 開放資料集建立資料集](https://docs.microsoft.com/azure/machine-learning/how-to-create-register-datasets#create-datasets-with-azure-open-datasets)。
- Azure 虛擬網路。 如需詳細資訊，請參閱[虛擬網路中的安全實驗和推斷](https://docs.microsoft.com/azure/machine-learning/how-to-enable-virtual-network)。
- Azure Pipelines。 如需詳細資訊，請參閱[定型和部署機器學習服務模型](https://docs.microsoft.com/azure/devops/pipelines/targets/azure-machine-learning?view=azure-devops&tabs=yaml)。
- Git 存放庫記錄。 如需詳細資訊，請參閱 [Git 整合](https://docs.microsoft.com/azure/machine-learning/concept-train-model-git-integration)。
- MLflow。 如需詳細資訊，請參閱[MLflow 以追蹤計量和部署模型](https://docs.microsoft.com/azure/machine-learning/how-to-use-mlflow)。
- Kubeflow。 如需詳細資訊，請參閱[建立端對端工作流程管線](https://www.kubeflow.org/docs/azure/)。
- 安全通訊。 您的 Azure 儲存體帳戶、計算目標和其他資源可以在虛擬網路內安全地使用，以定型模型及執行推斷。 如需詳細資訊，請參閱[虛擬網路中的安全實驗和推斷](https://docs.microsoft.com/azure/machine-learning/how-to-enable-virtual-network)。

## <a name="next-steps"></a>後續步驟

- 複習[Machine Learning studio](https://azure.microsoft.com/resources/whitepapers/search/?service=machine-learning-studio)和[Machine Learning 服務](https://azure.microsoft.com/resources/whitepapers/search/?service=machine-learning-service)的機器學習白皮書和電子書。
- 回顧[AI + Machine Learning 架構](https://docs.microsoft.com/azure/architecture/browse/)。
