---
title: 具有 Azure Machine Learning 的 MLOps
description: 使用適用于 Azure 的雲端採用架構來瞭解必須進行的各種轉換，以在雲端中啟用營運管理。
author: v-hanki
ms.author: janet
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: e5320b72d89e89c4034480614272c526e61e97b1
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86451739"
---
# <a name="mlops-with-azure-machine-learning"></a>具有 Azure Machine Learning 的 MLOps

Machine Learning 作業（MLOps）是以 DevOps 原則和實務為基礎，可提高工作流程的效率。 例如，持續整合、傳遞和部署。 MLOps 會將這些原則套用至機器學習程式，其目標為：

- 更快速地測試和開發模型
- 更快速地將模型部署至生產環境
- 品質保證

Azure Machine Learning 提供下列 MLOps 功能：

- **建立可重現的 ML 管線**。 Machine Learning 管線可讓您針對資料準備、定型和評分程式，定義可重複且可重複使用的步驟。
- **建立可重複使用的軟體環境**  用於定型和部署模型。
- **從任何地方註冊、封裝和部署模型**。 您也可以追蹤使用模型所需的相關中繼資料。
- **捕捉端對端 ML 生命週期的治理資料**。 記錄的資訊可能包括正在發行模型的物件、進行變更的原因，以及模型部署或用於生產環境中的時間。
- **通知並警示 ML 生命週期中的事件**。 例如，實驗完成、模型註冊、模型部署和資料漂移偵測。
- **監視 ml 應用程式的操作和 ml 相關問題**。 比較定型和推斷之間的模型輸入、探索模型特定計量，以及提供 ML 基礎結構的監視和警示。
- **使用 Azure Machine Learning 和 Azure Pipelines 自動化端對端 ML 生命週期**。 使用管線可讓您經常更新模型、測試新的模型，以及持續向其他應用程式和服務推出新的 ML 模型。

## <a name="best-practices-for-mlops-with-azure-machine-learning"></a>使用 Azure Machine Learning 進行 MLOps 的最佳做法

模型與程式碼不同，因為它們有有機的保質期，而且除非維護，否則將會降低。 一旦部署之後，他們就可以加入真正的商業價值，而當資料科學家提供採用標準工程實務的工具時，這會變得更容易。

「藉由統一我們的技術堆疊，並將我們的工程師與資料科學家結合在一起，讓我們的開發時間從數個月到短短幾周。」 *ASOS 的 Naeem Khedarun 首席軟體工程師*

透過 Azure 上的 MLOps，您可以快速實現模型的價值，並在一段時間內保留該值。

- 建立可重複使用的模型和可重複使用的定型管線。
- 簡化適用于品質控制、A/B 測試等的模型封裝、驗證和部署。
- 說明和觀察模型行為，並將重新定型程式自動化。

MLOps 可改善機器學習解決方案的品質與一致性。 若要深入瞭解如何使用 Azure Machine Learning 來管理模型的生命週期，請參閱[MLOps：使用 Azure Machine Learning 的模型管理、部署和監視](https://docs.microsoft.com/azure/machine-learning/concept-model-management-and-deployment)。

## <a name="next-steps"></a>後續步驟

若要深入瞭解，請閱讀並探索下列資源：

- [MLOps：使用 Azure Machine Learning 進行模型管理、部署和監視](https://docs.microsoft.com/azure/machine-learning/concept-model-management-and-deployment)
- 如何以及在何處[使用 Azure Machine Learning 部署模型](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-and-where)
- 教學課程：[在 ACI 中部署影像分類模型](https://docs.microsoft.com/azure/machine-learning/tutorial-deploy-models-with-aml)
- [端對端 MLOps 範例存放庫](https://github.com/microsoft/MLOps)
- [具有 Azure Pipelines 的 ML 模型 CI/CD](https://docs.microsoft.com/azure/devops/pipelines/targets/azure-machine-learning?view=azure-devops&tabs=yaml)
- 建立使用已 [部署模型](https://docs.microsoft.com/azure/machine-learning/how-to-consume-web-service)的用戶端
- [大規模機器學習](https://docs.microsoft.com/azure/architecture/data-guide/big-data/machine-learning-at-scale)
- [Azure AI 參考架構和最佳做法存放庫](https://github.com/microsoft/AI)
