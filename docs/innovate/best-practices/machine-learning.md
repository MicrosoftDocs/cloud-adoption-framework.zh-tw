---
title: 什麼是機器學習服務？
description: 什麼是機器學習服務？
author: v-hanki
ms.author: janet
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: 3a0edf01c1474f7f3fa7d75ca2edb94ebbf401a5
ms.sourcegitcommit: 412b945b3492ff3667c74627524dad354f3a9b85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/18/2020
ms.locfileid: "94880937"
---
<!-- cSpell:ignore scikit RLlib Jupyter MLflow Kubeflow -->

# <a name="what-is-machine-learning"></a>什麼是機器學習服務？

機器學習是一項資料科學技術，可讓電腦使用現有資料來預測未來的行為、結果和趨勢。 使用機器學習，電腦不需要明確進行程式設計就能學習。

機器學習的預測或預測可讓應用程式和裝置更聰明。 例如，當您線上購物時，機器學習服務可根據您已經購買的產品，協助推薦其他您可能會想要的產品。 或是當您的信用卡被刷過時，機器學習服務可將該筆交易與交易資料庫進行比對，協助偵測詐騙。 而且，當您的吸塵器機器人清潔房間時，機器學習服務可協助它判斷作業是否已完成。

## <a name="machine-learning-tools-to-fit-each-task"></a>適用於每個工作的機器學習工具

Azure Machine Learning 為開發人員和資料科學家提供其機器學習工作流程需要的所有工具，包括：

- [Azure Machine Learning 設計工具 (預覽) ](/azure/machine-learning/tutorial-designer-automobile-price-train-score)：拖放模組以建立您的實驗，然後部署管線
- Jupyter 筆記本：使用我們的 [範例筆記本](https://github.com/Azure/MachineLearningNotebooks) 或建立您自己的筆記本，以使用我們的 SDK for Python 範例。
- R 指令碼或筆記本，您可以在其中使用[適用於 R 的 SDK](https://azure.github.io/azureml-sdk-for-r/reference/index.html) 來撰寫您自己的程式碼，或在設計工具中使用 R 模組。
- [許多模型解決方案加速器 (預覽版) ](https://github.com/microsoft/solution-accelerator-many-models)建置於 Azure Machine Learning 上，可讓您定型、操作及管理數百或甚至數千個機器學習模型。
- [Visual Studio Code 延伸](/azure/machine-learning/tutorial-setup-vscode-extension)模組。
- [Machine LEARNING CLI](/azure/machine-learning/reference-azure-machine-learning-cli)。
- 開放原始碼架構，例如 PyTorch、TensorFlow 及 scikit-learn 等等。
- 使用光線 RLlib 的[增強式學習](/azure/machine-learning/how-to-use-reinforcement-learning)。

您甚至可以使用 [MLflow 來追蹤計量，以及部署模型](/azure/machine-learning/how-to-use-mlflow) 或 [kubeflow](https://www.kubeflow.org/docs/azure/) ，以建立端對端工作流程管線。

## <a name="build-machine-learning-models-in-python-or-r"></a>以 Python 或 R 建立機器學習模型

使用 Azure Machine Learning [Python SDK](/python/api/overview/azure/ml/?view=azure-ml-py) 或 [R SDK](https://azure.github.io/azureml-sdk-for-r/reference/index.html)，開始訓練您的本機電腦。 然後可以擴增至雲端。 有許多可用的 [計算目標](/azure/machine-learning/how-to-set-up-training-targets)，例如 Azure Machine Learning 計算和 [Azure Databricks](/azure/databricks/scenarios/what-is-azure-databricks)，以及具有 [advanced 超參數微調服務](/azure/machine-learning/how-to-tune-hyperparameters)，您可以使用雲端的強大功能，更快建立更好的模型。 您也可以使用 SDK，[自動進行模型定型和微調](/azure/machine-learning/tutorial-auto-train-models)。

## <a name="build-machine-learning-models-with-no-code-tools"></a>使用無程式碼工具建立機器學習模型

若要進行無程式碼或低程式碼的訓練和部署，請嘗試：

- Azure Machine Learning 設計工具 (預覽)

  使用設計工具來準備資料、定型、測試、部署、管理和追蹤機器學習模型，而不需撰寫任何程式碼。 不需要設計程式，就能以視覺化方式連線資料集和模組來建構模型。 試用[設計工具教學課程](/azure/machine-learning/tutorial-designer-automobile-price-train-score)。

  若要深入瞭解，請參閱 [Azure Machine Learning 設計工具總覽文章](/azure/machine-learning/concept-designer)。
- 自動化機器學習 (AutoML) UI

  瞭解如何在便於使用的介面中建立 [AutoML 實驗](/azure/machine-learning/tutorial-first-experiment-automated-ml) 。

## <a name="mlops-deploy-and-lifecycle-management"></a>MLOps：部署和生命週期管理

機器學習作業 (MLOps) 是以提高工作流程效率的 [DevOps](https://azure.microsoft.com/overview/what-is-devops/) 準則和作法為基礎。 例如，持續整合、傳遞和部署。 MLOps 會將這些原則套用至機器學習程式，其目標為：

- 更快速地實驗和開發模型
- 更快速地將模型部署到生產環境
- 品質保證

當您有正確的模型時，您可以在 Web 服務中、在 IoT 裝置上或從 Power BI 輕鬆使用它。 如需詳細資訊，請參閱 [使用 Azure Machine Learning 部署模型](/azure/machine-learning/how-to-deploy-and-where)。

然後，您可以使用 [適用于 Python](/python/api/overview/azure/ml/?view=azure-ml-py)、 [Azure Machine Learning studio](https://ml.azure.com/)或 [MACHINE Learning CLI](/azure/machine-learning/reference-azure-machine-learning-cli)的 Azure Machine Learning SDK 來管理已部署的模型。

您可以使用這些模型，並以 [即時](/azure/machine-learning/how-to-consume-web-service) 或 [非同步方式](/azure/machine-learning/how-to-use-parallel-run-step) 傳回大量資料的預測。

另外，透過進階[機器學習管線](/azure/machine-learning/concept-ml-pipelines)，您可以在資料準備、模型定型與評估及部署的每個步驟上共同作業。 管線可讓您執行下列作業：

- 自動化雲端中的端對端機器學習程序
- 重複使用元件，並且只在需要時重新執行步驟
- 在每個步驟中使用不同的計算資源
- 執行批次評分工作

如果您想要使用腳本來自動化機器學習工作流程， [machine LEARNING CLI](/azure/machine-learning/reference-azure-machine-learning-cli) 會提供命令列工具來執行一般工作，例如提交定型回合或部署模型。

若要開始使用 Azure Machine Learning，請參閱以下的[後續步驟](/azure/machine-learning/overview-what-is-azure-ml#next-steps)。

## <a name="automated-machine-learning"></a>自動化 Machine Learning

資料科學家花費大量時間在測試階段反復查看模型。 在建立可接受的模型之前，嘗試使用不同演算法和超參數組合的整個程式，對資料科學家而言極為困難，因為工作的單調和非挑戰性的本質。 雖然這是在模型效力方面帶來大量增益的練習，但有時候成本可能會在時間和資源方面太多，因此在投資方面可能會有負面的報酬率)  (ROI。

這是自動化機器學習 (AutoML) 的位置。 它使用概率矩陣分解的研究白皮書中的概念，並根據所呈現資料的啟發學習法來實行自動化管線，以根據所呈現的資料啟發學習法，並考慮指定的問題或案例。 此管線的結果是一組最適合指定問題和資料集的模型。

如需 AutoML 的詳細資訊，請參閱 [使用 Azure Machine Learning 的 AutoML 和 MLOps](https://azure.microsoft.com/blog/automated-machine-learning-and-mlops-with-azure-machine-learning/)。

## <a name="responsible-ml"></a>負責任的 ML

在整個開發和使用 AI 系統的同時，信任是最重要的核心。 包括對平台、流程和模型的信任。 當 AI 和自發系統整合到社會的網狀架構時，請務必主動地努力預測並減輕這些技術的非預期結果。

- 瞭解您的模型並建立公平：說明模型行為，並找出對預測有最大影響的功能。 在模型定型和推斷期間，將內建 explainers 用於半透明和黑色方塊模型。 使用互動式視覺效果來比較模型，並執行模擬分析以改善模型的精確度。 使用最先進的演算法，測試您的模型是否公平。 減緩整個機器學習生命週期的不公平性、比較緩和的模型，並視需要做出刻意公平與精確度的取捨。
- 保護資料的隱私權和機密性：使用差異隱私權的最新創新來建立可保存隱私權的模型，這會在資料中插入精確的統計雜訊層級，以限制機密資訊的洩漏。 識別資料流程失並以智慧方式限制重複查詢，以管理暴露風險。 使用加密與機密機器學習 (即將推出的) 技術，特別為機器學習設計，以使用機密資料安全地建立模型。
- 透過機器學習程式的每個步驟控制和治理：存取內建功能，可自動追蹤歷程記錄，並在機器學習生命週期內建立審核試用。 藉由追蹤資料集、模型、實驗、程式碼等，取得機器學習程式的完整可見度。 使用自訂標籤來執行模型資料工作表、記錄重要模型中繼資料、提高責任，以及確保負責任的流程。

深入瞭解如何執行 [負責任的 ML](/azure/machine-learning/concept-responsible-ml)。

## <a name="integration-with-other-services"></a>與其他服務整合

Azure Machine Learning 可與 Azure 平臺上的其他服務搭配使用，也可與開放原始碼工具（例如 Git 和 MLflow）整合。

- 計算目標，例如 Azure Kubernetes Service、Azure 容器執行個體、Azure Databricks、Azure Data Lake Analytics 和 Azure HDInsight。 如需計算目標的詳細資訊，請參閱[什麼是計算目標？](/azure/machine-learning/concept-compute-target)。
- Azure 事件格線。 如需詳細資訊，請參閱[取用 Azure Machine Learning 事件](/azure/machine-learning/how-to-use-event-grid)。
- Azure 監視器。 如需詳細資訊，請參閱[監視 Azure Machine Learning](/azure/machine-learning/monitor-azure-machine-learning)。
- 資料存放區，例如 Azure 儲存體帳戶、Azure Data Lake Storage、Azure SQL Database、適用於 PostgreSQL 的 Azure 資料庫和 Azure 開放資料集。 如需詳細資訊，請參閱 [存取 Azure 儲存體服務中的資料](/azure/machine-learning/how-to-access-data) ，以及 [使用 Azure 開放資料集來建立資料集](/azure/machine-learning/how-to-create-register-datasets#create-datasets-with-azure-open-datasets)。
- Azure 虛擬網路。 如需詳細資訊，請參閱[虛擬網路中的安全實驗和推斷](/azure/machine-learning/how-to-enable-virtual-network)。
- Azure Pipelines。 如需詳細資訊，請參閱[定型和部署機器學習服務模型](/azure/devops/pipelines/targets/azure-machine-learning?tabs=yaml&view=azure-devops)。
- Git 存放庫記錄。 如需詳細資訊，請參閱 [Git 整合](/azure/machine-learning/concept-train-model-git-integration)。
- MLflow. 如需詳細資訊，請參閱 [MLflow 來追蹤計量和部署模型](/azure/machine-learning/how-to-use-mlflow)。
- Kubeflow。 如需詳細資訊，請參閱 [組建端對端工作流程管線](https://www.kubeflow.org/docs/azure/)。
- 安全通訊。 您的 Azure 儲存體帳戶、計算目標和其他資源可以在虛擬網路內安全地使用，以定型模型及執行推斷。 如需詳細資訊，請參閱[虛擬網路中的安全實驗和推斷](/azure/machine-learning/how-to-enable-virtual-network)。

## <a name="next-steps"></a>後續步驟

- 請參閱 [Machine Learning studio](https://azure.microsoft.com/resources/whitepapers/search/?service=machine-learning-studio) 和 [Machine Learning 服務](https://azure.microsoft.com/resources/whitepapers/search/?service=machine-learning-service)的機器學習技術白皮書和電子書。
- 複習 [AI + Machine Learning 架構](/azure/architecture/browse/)。
