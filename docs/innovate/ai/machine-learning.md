---
title: 機器學習服務
description: 這些檢查清單和資源可協助您規劃應用程式的開發和部署。
author: v-hanki
ms.author: janet
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: 1002391130afdc11a9617022be2802564b543c44
ms.sourcegitcommit: d31a9043d1ae9283ed126bf118ca26d1d18d6948
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/10/2020
ms.locfileid: "88040774"
---
<!-- cSpell:ignore scikit RLlib ONNX Jupyter -->

# <a name="machine-learning"></a>機器學習服務

Azure 可讓您擁有最先進的機器學習服務功能。 使用 Azure Machine Learning，快速且輕鬆地建立、定型及部署您的機器學習模型。 Machine Learning 可以用於任何類型的機器學習，從傳統到深度、受監督和不受監督的學習。 無論您想要撰寫 Python 或 R 程式碼，或是使用零碼或低程式碼的選項（例如[設計](https://docs.microsoft.com/azure/machine-learning/tutorial-designer-automobile-price-train-score)工具），您都可以在 Machine Learning 工作區中建立、定型和追蹤高精確度的機器學習和深度學習模型。

您甚至可以開始訓練您的本機電腦，然後向外延展到雲端。 此服務也會與熱門的深度學習和增強式開放原始碼工具（例如 PyTorch、TensorFlow、scikit-learn 學習和光線和 RLlib）互通。

開始使用[Machine Learning](https://docs.microsoft.com/azure/machine-learning/)。 您將會找到如何[開始使用第一個機器學習實驗](https://docs.microsoft.com/azure/machine-learning/tutorial-1st-experiment-sdk-setup)的教學課程。 若要深入瞭解機器學習的開放原始碼模型格式和執行時間，請參閱[ONNX runtime](http://onnxruntime.ai)。

機器學習解決方案的常見案例包括：

- 預測性維護
- 庫存管理
- 詐騙偵測
- 需求預測
- 智慧型建議
- 銷售預測

## <a name="checklist"></a>檢查清單

- **首先，先熟悉 Machine Learning**，然後選擇要開始使用的經驗。 您可以遵循搭配 Python 使用 Jupyter 筆記本、視覺拖放體驗，或自動化機器學習 (AutoML) 的步驟。

  - [Machine Learning 總覽](https://docs.microsoft.com/azure/machine-learning/overview-what-is-azure-ml)
  - [使用 Python 搭配 Jupyter 筆記本來建立您的第一個機器學習實驗](https://docs.microsoft.com/azure/machine-learning/tutorial-1st-experiment-sdk-setup)
  - [視覺拖放實驗](https://docs.microsoft.com/azure/machine-learning/tutorial-designer-automobile-price-train-score)
  - [使用 AutoML UI](https://docs.microsoft.com/azure/machine-learning/tutorial-first-experiment-automated-ml)
  - [設定您的開發環境](https://docs.microsoft.com/azure/machine-learning/how-to-configure-environment)

- **試驗更先進的教學**課程，以預測出租車費用、分類影像，並建立用於批次計分的管線。

  - [使用 AutoML 來預測出租車費用](https://docs.microsoft.com/azure/machine-learning/tutorial-auto-train-models)
  - [使用 scikit-learn 分類影像-學習](https://docs.microsoft.com/azure/machine-learning/tutorial-train-models-with-aml)
  - [建立批次評分的 Machine Learning 管線](https://docs.microsoft.com/azure/machine-learning/tutorial-pipeline-batch-scoring-classification)

- **遵循影片教學**課程，以深入瞭解 Machine Learning 的優點，例如無程式碼模型建立、MLOPS、ONNX 執行時間、模型 interpretability 和透明度等等。

  - [Machine Learning 的新功能](https://channel9.msdn.com/Shows/AI-Show/Allup-Azure-ML)
  - [使用 AutoML 建立模型](https://aka.ms/automlvideo)
  - [使用 Machine Learning 設計工具建立零程式碼模型](https://aka.ms/studioanddesigner)
  - [管理端對端生命週期的 MLOps](https://aka.ms/mlopsvideo)
  - [將 ONNX 執行時間併入模型中](https://www.youtube.com/watch?v=qy7X2JGLUC4)
  - [模型 interpretability 和透明度](https://aka.ms/azuremlinterpret)
  - [使用 R 建立模型](https://aka.ms/Rmodels)

- [審查 AI 解決方案的參考架構](https://docs.microsoft.com/azure/architecture/browse/#ai--machine-learning)

## <a name="next-steps"></a>後續步驟

探索其他 AI 解決方案類別：

- [AI 應用程式和代理程式](./ai-applications.md)
- [知識採礦](./knowledge-mining.md)
