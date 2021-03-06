---
title: 使用 Azure Machine Learning 的 MLOps
description: 瞭解可提高工作流程效率的 MLOps 原則和做法，例如持續整合、傳遞和部署。
author: v-hanki
ms.author: janet
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: think-tank
ms.openlocfilehash: 6e9b6734dd982c3e5529f216262d25cf7dcc264b
ms.sourcegitcommit: c167c45b66cc7324b60c88b8b7aac439f956b65d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2021
ms.locfileid: "102208195"
---
# <a name="mlops-with-azure-machine-learning"></a>使用 Azure Machine Learning 的 MLOps

*MLOps (機器學習作業)* 以 DevOps 準則和做法為基礎，可提高工作流程的效率，例如持續整合、傳遞和部署。 MLOps 會將這些原則套用至機器學習程式，以便：

- 更快速地實驗和開發模型。
- 更快速地將模型部署到生產環境。
- 實務和精簡品質保證。

Azure Machine Learning 提供下列 MLOps 功能：

- **建立可重現的管線。** 機器學習管線可讓您針對資料準備、定型和評分程式，定義可重複且可重複使用的步驟。
- **建立可重複使用的軟體環境** ，以定型和部署模型。
- **從任何地方註冊、封裝及部署模型。** 您可以追蹤使用模型所需的相關中繼資料。
- **捕捉端對端生命週期的治理資料。** 記錄的資訊可能包括正在發佈模型的人員、進行變更的原因，以及在生產環境中部署或使用模型的時間。
- **通知並警示生命週期中的事件。** 例如，您可以取得實驗完成、模型註冊、模型部署和資料漂移偵測的警示。
- **監視應用程式，以瞭解操作與機器學習相關的問題。** 比較定型和推斷之間的模型輸入、探索模型特定的計量，並在您的機器學習基礎結構上提供監視和警示。
- **使用 Azure Machine Learning 和 Azure 管線將端對端機器學習生命週期自動化。** 透過管線，您可以經常更新模型、測試新的模型，並與其他應用程式和服務持續推出新的機器學習模型。

## <a name="best-practices-for-mlops-with-azure-machine-learning"></a>使用 Azure Machine Learning 進行 MLOps 的最佳做法

模型與程式碼不同，因為它們有有機的生命週期，除非維持，否則會下降。 部署完成後，他們可以加入真正的商業價值，而且當資料科學家提供採用標準工程實務的工具時，這會變得更容易。

使用 Azure MLOps 可協助您：

- 建立可重現的模型和可重複使用的訓練管線。
- 簡化品質控制和 a/B 測試的模型封裝、驗證和部署。
- 說明並觀察模型行為，並將重新定型程式自動化。

MLOps 改進了機器學習解決方案的品質與一致性。 若要深入瞭解如何使用 Azure Machine Learning 來管理模型的生命週期，請參閱 [MLOps：使用 Azure Machine learning 進行模型管理、部署和監視](/azure/machine-learning/concept-model-management-and-deployment)。

## <a name="next-steps"></a>下一步

若要深入瞭解，請閱讀及探索下列資源：

- [MLOps：使用 Azure Machine Learning 來管理、部署及監視模型](/azure/machine-learning/concept-model-management-and-deployment)
- [使用 Azure Machine Learning 部署模型](/azure/machine-learning/how-to-deploy-and-where)的方式和位置
- 教學課程： [在 Azure 容器實例中部署影像分類模型](/azure/machine-learning/tutorial-deploy-models-with-aml)
- [端對端 MLOps 範例存放庫](https://github.com/microsoft/MLOps)
- [使用 Azure 管線進行機器學習模型的 CI/CD](/azure/devops/pipelines/targets/azure-machine-learning)
- 建立使用已[部署模型](/azure/machine-learning/how-to-consume-web-service)的用戶端
- [大規模機器學習](/azure/architecture/data-guide/big-data/machine-learning-at-scale)
- [Azure AI 參考架構和最佳做法存放庫](https://github.com/microsoft/AI)
