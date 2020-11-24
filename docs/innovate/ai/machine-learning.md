---
title: 機器學習服務
description: 這些檢查清單和資源可協助您規劃應用程式開發和部署。
author: v-hanki
ms.author: janet
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.custom: think-tank
ms.openlocfilehash: 8b2cb606ded625dfcfecc5e8168f38d45ac01c2d
ms.sourcegitcommit: d957bfc1fa8dc81168ce9c7d801a8dca6254c6eb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/23/2020
ms.locfileid: "95446720"
---
<!-- cSpell:ignore scikit RLlib ONNX Jupyter -->

# <a name="machine-learning"></a>機器學習服務

Azure 可為您提供最先進的機器學習功能。 使用 Azure Machine Learning，快速且輕鬆地建立、定型及部署機器學習模型。 Machine Learning 可用於任何一種機器學習，從傳統到深度、監督和非監督式的學習。 無論您想要撰寫 Python 或 R 程式碼，或使用零程式碼或低程式碼選項（例如 [設計](/azure/machine-learning/tutorial-designer-automobile-price-train-score)工具），都可以在 Machine Learning 工作區中建立、定型和追蹤高精確度的機器學習和深度學習模型。

您甚至可以在本機電腦上開始訓練，然後向外延展到雲端。 此服務也會與熱門深度學習交互操作，並增強式開放原始碼工具，例如 PyTorch、TensorFlow、scikit-learn 學習和光線和 RLlib。

[Machine Learning](/azure/machine-learning/)入門。 您將會找到如何 [開始使用第一個機器學習實驗](/azure/machine-learning/tutorial-1st-experiment-sdk-setup)的教學課程。 若要深入瞭解機器學習的開放原始碼模型格式和執行時間，請參閱 [ONNX runtime](http://onnxruntime.ai)。

機器學習解決方案的常見案例包括：

- 預測性維護
- 庫存管理
- 詐騙偵測
- 需求預測
- 智慧型建議
- 銷售預測

## <a name="checklist"></a>檢查清單

- **首先熟悉您自己的 Machine Learning**，然後選擇要開始使用的體驗。 您可以遵循下列步驟，使用 Jupyter 筆記本搭配 Python、視覺效果拖放體驗，或自動化機器學習 (AutoML) 。

  - [Machine Learning 總覽](/azure/machine-learning/overview-what-is-azure-ml)
  - [使用 Python 搭配 Jupyter 筆記本建立您的第一個機器學習實驗](/azure/machine-learning/tutorial-1st-experiment-sdk-setup)
  - [視覺效果拖放實驗](/azure/machine-learning/tutorial-designer-automobile-price-train-score)
  - [使用 AutoML UI](/azure/machine-learning/tutorial-first-experiment-automated-ml)
  - [設定您的開發環境](/azure/machine-learning/how-to-configure-environment)

- **試驗更** 高階的教學課程，以預測出租車費用、分類影像，並建立批次評分的管線。

  - [使用 AutoML 來預測出租車費用](/azure/machine-learning/tutorial-auto-train-models)
  - [使用 scikit-learn 分類影像-學習](/azure/machine-learning/tutorial-train-models-with-aml)
  - [建立批次評分的 Machine Learning 管線](/azure/machine-learning/tutorial-pipeline-batch-scoring-classification)

- **遵循影片教學** 課程，以深入瞭解 Machine Learning 的優點，例如無程式碼模型建立、機器學習作業 (MLOps) 、ONNX 執行時間、模型可解譯性和透明度等等。

  - [Machine Learning 的新功能](https://channel9.msdn.com/Shows/AI-Show/Allup-Azure-ML)
  - [使用 AutoML 建立模型](https://aka.ms/automlvideo)
  - [使用 Machine Learning 設計工具建立零程式碼模型](https://aka.ms/studioanddesigner)
  - [管理端對端生命週期的 MLOps](https://aka.ms/mlopsvideo)
  - [將 ONNX 執行時間併入您的模型](https://www.youtube.com/watch?v=qy7X2JGLUC4)
  - [模型可解譯性和透明度](https://aka.ms/azuremlinterpret)
  - [使用 R 建立模型](https://aka.ms/Rmodels)

- [審核 AI 解決方案的參考架構](/azure/architecture/browse/#ai--machine-learning)

## <a name="next-steps"></a>後續步驟

探索其他 AI 解決方案類別：

- [AI 應用程式和代理程式](./ai-applications.md)
- [知識採礦](./knowledge-mining.md)
