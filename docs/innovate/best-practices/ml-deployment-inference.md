---
title: 部署期間的機器學習推斷
description: 瞭解您的 AI 模型在生產環境中部署時如何進行預測。
author: DonnaForlin
ms.author: brblanch
ms.date: 01/20/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.custom: think-tank
ms.openlocfilehash: c6fe231f81324815f8004a6ed3408ab326a6be02
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102115096"
---
# <a name="machine-learning-inference-during-deployment"></a>部署期間的機器學習推斷

在生產環境中部署您的 AI 模型時，您需要考慮它將如何進行預測。 AI 模型的兩個主要流程為：

- **批次推斷：** 以一批觀察為基礎的非同步處理常式。 預測會儲存為檔案，或儲存在使用者或商務應用程式的資料庫中。

- **即時 (或互動式) 推斷：** 釋出模型以隨時進行預測，並觸發立即的回應。 您可以使用此模式來分析串流和互動式應用程式資料。

請考慮下列問題來評估您的模型、比較兩個進程，然後選取適合您模型的程式：

- 預測產生的頻率為何？
- 需要的結果是多久？
- 應該以小型批次或以大量批次方式產生預測？
- 是否預期需要從模型延遲？
- 執行模型需要多少計算能力？
- 維護模型是否有操作含意和成本？

下列決策樹可協助您判斷哪一種部署模型最適合您的使用案例：

[![即時或批次推斷決策樹的圖表。](./media/inference-decision-tree.png)](./media/inference-decision-tree.png#lightbox)

## <a name="batch-inference"></a>批次推斷

批次推斷（有時稱為離線推斷）是比較簡單的推斷程式，可協助模型以計時間隔和商務應用程式來儲存預測。

針對批次推斷，請考慮下列最佳做法：

- **觸發批次評分：** 使用 azure machine Learning 管線和 `ParallelRunStep` Azure Machine learning 中的功能，來設定排程或事件型自動化。 流覽至 AI 節目，以[使用 Azure Machine Learning `ParallelRunStep` 執行批次推斷](https://channel9.msdn.com/Shows/AI-Show/How-to-do-Batch-Inference-using-AML-ParallelRunStep)，並深入瞭解該流程。

- **Batch 推斷的計算選項：** 由於批次推斷程式不會持續執行，因此建議您自動啟動、停止及調整可重複使用的叢集，以處理各種工作負載。 不同的模型需要不同的環境，而且您的解決方案必須能夠部署特定的環境，並在推斷結束時將其移除，以供下一個模型使用計算。 請參閱下列決策樹，以找出適合您模型的計算實例：

  [![計算決策樹的圖表。](./media/compute-decision-tree.png)](./media/compute-decision-tree.png#lightbox)

- **執行批次推斷：** Azure 支援批次推斷的多項功能。 `ParallelRunStep`Azure Machine Learning 中的一項功能，可讓客戶從儲存在 azure 中的數 tb 的結構化或非結構化資料獲得見解。 `ParallelRunStep` 提供現成的平行處理原則，並可在 Azure Machine Learning 管線內運作。

- **批次推斷挑戰：** 雖然批次推斷是在生產環境中使用和部署模型較簡單的方式，但它的確有選取的挑戰：

  - 根據推斷執行的頻率而定，所產生的資料可能會與存取的時間無關。

  - 冷啟動問題的變化;新資料可能無法使用結果。 例如，如果新使用者以零售建議系統建立及帳戶並開始購物，在下一次批次推斷執行之前，將無法使用產品建議。 如果這是您的使用案例的障礙，請考慮即時推斷。

  - 在批次推斷案例中，部署至許多區域和高可用性並不是重要的考慮。 模型不需要部署地區，而且資料存放區可能需要在許多位置使用高可用性策略來部署。 這通常會遵循應用程式 HA 設計和策略。

## <a name="real-time-inference"></a>即時推斷

即時或互動式的推斷是一種架構，可在任何時間觸發模型推斷，並預期立即回應。 您可以使用此模式來分析串流資料、互動式應用程式資料等等。 此模式可讓您即時利用機器學習模型，並解決上述批次推斷中所述的冷啟動問題。

如果即時推斷適合您的模型，則可以使用下列考慮和最佳作法：

- **即時推斷的挑戰：** 延遲和效能需求讓您的模型更複雜的即時推斷架構。 系統可能需要以100毫秒或更短的時間回應，在這段期間，它需要取出資料、執行推斷、驗證和儲存模型結果、執行任何必要的商務邏輯，並將結果傳回給系統或應用程式。

- **即時推斷的計算選項：** 實行即時推斷的最佳方式是將模型以容器形式部署至 Docker 或 AKS 叢集，並使用 REST API 將其公開為 web 服務。 如此一來，模型就會在自己的隔離環境中執行，而且可以像任何其他 web 服務一樣進行管理。 然後可以使用 Docker/AKS 功能來管理、監視、調整規模等等。 模型可以部署在內部部署、雲端或邊緣。 上述計算決策會概述即時推斷。

- **Dns 多區域性部署和高可用性：** 區域部署和高可用性架構必須在即時推斷案例中考慮，因為延遲和模型的效能將是解決問題的關鍵。 若要減少 dns 多區域性部署中的延遲，建議將模型盡可能靠近取用點。 模型和支援的基礎結構應遵循企業的高可用性和 DR 原則和策略。

## <a name="many-models-scenario"></a>多模型案例

單數模型可能無法抓住真實世界問題的複雜本質，例如預測超市的超市銷售，其中人口統計、品牌、Sku 和其他功能可能會導致客戶行為大幅改變。 區域可能導致開發智慧型計量的預測性維護也有很大的差異。 針對這些案例使用許多模型來捕捉區域資料或儲存層級關聯性，可能會產生比單一模型更高的精確度。 這種方法假設有足夠的資料可用於此層級的資料細微性。

概括而言，許多模型案例會在三個階段中發生：資料來源、資料科學及許多模型。

[![多模型案例的圖表。](./media/many-models-scenario.png)](./media/many-models-scenario.png#lightbox)

**資料來源：** 在資料來源階段中，將資料分割成不需要太多基數是很重要的。 產品識別碼或條碼不應納入主要分割區，因為這會產生太多區段，而且可能會禁止有意義的模型。 品牌、SKU 或位置可能更適合功能。 也請務必移除會扭曲資料分佈的異常，以 homogenize 資料。

**資料科學：** 數個實驗會平行執行至資料科學階段中的每個資料分割。 這是通常會反復進行的程式，其中會評估實驗中的模型，以判斷最適合的模型。

**許多模型：** 在模型登錄中註冊每個區段或類別的最佳模型。 將有意義的名稱指派給模型，使其更容易被推斷。 在需要時使用標記，將模型分組為特定類別。

## <a name="batch-inference-for-many-models"></a>許多模型的批次推斷

在許多模型的批次推斷期間，預測通常是排程、週期性，而且可以同時處理大量的資料執行。 與單一模型案例不同的是，許多模型都是同時推斷，所以請務必選取正確的模型。 下圖顯示許多模型批次推斷的參考模式：

[![許多模型批次推斷的參考模式圖表。](./media/many-models-batch-inference.png)](./media/many-models-batch-inference.png#lightbox)

此模式的核心目的是要觀察模型並同時執行多個模型，以達成可處理大型資料磁片區的高擴充性推斷解決方案。 為了達成階層式模型推斷，許多模型都可以分割成類別。 每個類別都可以有自己的推斷儲存體，例如 Azure data lake。 在實作為此模式時，需要以水準和垂直方式平衡模型的調整，因為這會影響成本和效能。 執行太多模型實例可能會提高效能，但會影響成本。 具有高規格節點的實例太少可能更符合成本效益，但可能會導致調整問題。

## <a name="real-time-inference-for-many-models"></a>許多模型的即時推斷

即時多模型推斷需要低延遲和隨選要求，通常是透過 REST 端點。 當外部應用程式或服務需要標準介面來與模型互動（通常是透過具有 JSON 承載的 REST 介面）時，這會很有用。

[![許多模型即時推斷的圖表。](./media/many-models-real-time-inference.png)](./media/many-models-real-time-inference.png#lightbox)

此模式的核心用途是使用探索服務來識別服務及其中繼資料的清單。 這可實作為 Azure 函式，並可讓用戶端取得服務的相關服務詳細資料，可使用安全的 REST URI 來叫用。 JSON 承載會傳送至服務，此服務會啟動相關的模型，並將 JSON 回應提供回用戶端。

每個服務都是無狀態微服務，可同時處理多個要求，且受限於實體虛擬機器資源。 如果選取多個群組，服務可以部署多個模型;這種情況下，建議使用同質群組，例如類別目錄、SKU 等等。 針對指定的服務所選的服務要求與模型之間的對應，必須內建到推斷邏輯中，通常是透過分數腳本。 如果模型的大小相對較小 (幾 mb 的) ，建議您基於效能考慮將它們載入記憶體中;否則，每個要求都可以動態載入每個模型。

## <a name="next-steps"></a>下一步

探索下列資源以深入瞭解如何使用 Azure Machine Learning 推斷：

- [建置 Azure Machine Learning 管線進行批次評分](/azure/machine-learning/tutorial-pipeline-batch-scoring-classification)
- [使用 Azure Machine Learning 設計工具執行批次預測](/azure/machine-learning/how-to-run-batch-predictions-designer)
- [Azure Machine Learning 中的批次推斷](https://techcommunity.microsoft.com/t5/azure-ai/batch-inference-in-azure-machine-learning/ba-p/1417010#:~:text=%20Batch%20Inference%20in%20Azure%20Machine%20Learning%20,Learning%20Pipelines.%20ParallelRunStep%20is%20available%20through...%20More%20)
