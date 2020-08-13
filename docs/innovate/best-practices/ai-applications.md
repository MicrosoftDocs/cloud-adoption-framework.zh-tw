---
title: 什麼是 AI 應用程式？
description: 瞭解如何使用 Azure 認知服務將 AI 功能和突破整合到您的應用程式中。
author: v-hanki
ms.author: janet
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: baf95582ce322b7b5444d403197a1edf386cfb3a
ms.sourcegitcommit: 949b87bad28d32df84df190160089f01826f3a31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2020
ms.locfileid: "88195135"
---
<!-- cSpell:ignore Personalizer -->

# <a name="what-are-ai-applications"></a>什麼是 AI 應用程式？

在 Azure 中，您可以使用所選和內建 AI 的工具和技術，更快速地建立智慧型應用程式。

- **輕鬆地在任何地方建立及部署：** 使用您小組現有的技能集，以及您所知的工具來建立智慧型應用程式，並在不變更程式碼的情況下進行部署。 您可以建立一次，然後在雲端、內部部署和邊緣裝置上部署。 與任何其他提供者相比，您可以確信全域散發至更多資料中心。
- **使用開放平臺來建立影響：** 選擇您最愛的技術，這可以是開放原始碼。 Azure 支援各種不同的部署選項、熱門的堆疊和語言，以及一組完整的資料引擎。 利用這項彈性，加上 Microsoft 技術所提供的效能、規模和安全性，為任何案例打造應用程式。
- **使用內建的智慧功能開發應用程式：** 使用 Azure 建立智慧型應用程式很簡單，因為沒有其他平臺會將分析和原生 AI 帶入您的資料（不論其所在位置，以及您所使用的語言）。 您可以利用一組豐富的 [認知 api](https://azure.microsoft.com/services/cognitive-services/) ，輕鬆地在您的應用程式中建立新的體驗，以進行人類喜歡的智慧。

## <a name="what-is-azure-cognitive-services"></a>什麼是 Azure 認知服務？

Azure 認知服務可透過幾個簡單的程式碼，簡化將 AI 功能和突破整合到應用程式的方式。 它可讓您建立應用程式，以查看、聆聽、說話、瞭解，甚至是您商務程式之間的起點。 認知服務以便於使用並併入應用程式中的形式，提供 AI 智慧功能。

認知服務是由 Api、Sdk 和服務所組成，可協助開發人員建立智慧型應用程式，而不需要具備直接的 AI 或資料科學技能或知識。 認知服務可讓開發人員輕鬆地將認知功能加入其應用程式中。 認知服務內的服務目錄可以分類為五個主要部分：視覺、語音、語言、web 搜尋和決策。

### <a name="vision-apis"></a>視覺 API

| 服務名稱 | 服務說明 |
| --- | --- |
| [電腦視覺](https://docs.microsoft.com/azure/cognitive-services/computer-vision/) | 電腦視覺可供您存取進階演算法，以處理影像及傳回資訊。 |
| [自訂視覺](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/home) | 自訂視覺可讓您建立自訂影像分類器。 |
| [臉部](https://docs.microsoft.com/azure/cognitive-services/face/) | 臉部服務可讓您存取可偵測及辨識臉部屬性的先進字型演算法。 |
| [表單辨識器](https://docs.microsoft.com/azure/cognitive-services/form-recognizer/) (預覽)  | 表單辨識器會從表單檔識別並解壓縮索引鍵/值組和資料表資料。 然後，它會在原始檔案中輸出包含關聯性的結構化資料。 |
| [筆跡辨識器](https://docs.microsoft.com/azure/cognitive-services/ink-recognizer/) (預覽)  | 筆跡辨識器可讓您辨識和分析數位筆跡-筆劃資料、圖形和手寫內容，並輸出具有所有辨識實體的檔結構。 |
| [影片索引子](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview) | 影片索引子可讓您從影片中摘錄見解。 |

### <a name="speech-apis"></a>語音識別 API

| 服務名稱 | 服務說明 |
| --- | --- |
| [語音](https://docs.microsoft.com/azure/cognitive-services/speech-service/) | 語音服務會將語音啟用功能加入應用程式。 |
|  (preview 的[說話者](https://docs.microsoft.com/azure/cognitive-services/speaker-recognition/home "說話者辨識 API")辨識)  | 說話者辨識 API 提供說話者識別和驗證的演算法。 |
| [Bing 語音](https://docs.microsoft.com/azure/cognitive-services/speech/home) (淘汰)  | Bing 語音 API 可讓您輕鬆在應用程式中建立語音開啟功能。 |
| [翻譯工具語音](https://docs.microsoft.com/azure/cognitive-services/translator-speech/) (淘汰)  | 翻譯工具語音是機器翻譯服務。 |

### <a name="language-apis"></a>語言 API

| 服務名稱 | 服務說明 |
| --- | -- |
| [語言理解 (LUIS)](https://docs.microsoft.com/azure/cognitive-services/luis/) | Language Understanding 服務 (LUIS) 可讓您的應用程式瞭解某人在自己的單字中所要的內容。 |
| [QnA Maker](https://docs.microsoft.com/azure/cognitive-services/qnamaker/index "QnA Maker") | QnA Maker 可讓您從半結構化內容建立問答服務。 |
| [文字分析](https://docs.microsoft.com/azure/cognitive-services/text-analytics/) | 文字分析提供原始文字的自然語言處理，以進行情感分析、關鍵字組解壓縮和語言偵測。 |
| [翻譯工具](https://docs.microsoft.com/azure/cognitive-services/translator/) | Translator 以近乎即時的方式提供以電腦為基礎的文字轉譯。 |

### <a name="decision-apis"></a>決策 API

| 服務名稱 | 服務說明 |
| --- | --- |
| [異常](https://docs.microsoft.com/azure/cognitive-services/anomaly-detector/) 偵測器 (預覽)  | 異常偵測器可讓您監視和偵測時間序列資料中的異常狀況。 |
| [內容仲裁](https://docs.microsoft.com/azure/cognitive-services/content-moderator/overview "內容仲裁者") | 內容仲裁可監視潛在的冒犯、惡意或具風險之內容。 |
| [個人化工具](https://docs.microsoft.com/azure/cognitive-services/personalizer/) | 個人化工具可讓您從使用者的即時行為中學習，以選擇最適合他們的體驗。 |

### <a name="supported-cultural-languages"></a>支援的文化特性語言

認知服務以服務層支援各種不同的文化特性語言。 您可以在[支援的語言清單](https://docs.microsoft.com/azure/cognitive-services/language-support)中找到每個 API 的語言可用性。

### <a name="secure-resources"></a>安全資源

認知服務提供多層式安全性模型，其中包括透過 Azure Active Directory 認證、有效的資源金鑰和[Azure 虛擬網路](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-virtual-networks)的[驗證](https://docs.microsoft.com/azure/cognitive-services/authentication)。

### <a name="container-support"></a>容器支援

認知服務提供在雲端或內部部署環境中部署的容器。 深入瞭解 [認知服務容器](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support)。

<!-- docsTest:ignore "HIPAA BAA" "CSA STAR" -->

### <a name="certifications-and-compliance"></a>認證和合規性

認知服務已獲得認證，例如 CSA STAR 認證、FedRAMP 一般和 HIPAA BAA。

您可以[下載](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942)適用於自身稽核及安全性審查的認證。

若要瞭解隱私權和資料管理，請前往 [Microsoft 信任中心](https://servicetrust.microsoft.com/)。

## <a name="how-are-cognitive-services-and-azure-machine-learning-similar"></a>認知服務和 Azure Machine Learning 類似嗎？

認知服務和 Azure Machine Learning 具有套用 AI 來增強商務營運的常見目標。 每個專案在個別供應專案中提供這項功能的方式不同。 一般而言，這些物件是不同的：

- 認知服務適用于沒有機器學習服務經驗的開發人員。
- Machine Learning 是針對資料科學家量身打造的。

## <a name="how-is-a-cognitive-service-different-from-machine-learning"></a>認知服務與機器學習有何不同？

認知服務會為您提供已定型的模型。 此模型會將資料和演算法整合在一起，並可從 REST API 或 SDK 取得。 視您的案例而定，您可以在幾分鐘內執行此服務。 認知服務會提供一般問題的解答，例如文字中的關鍵字組或影像中的專案識別。

機器學習服務是一種程式，通常需要較長的時間才能順利執行。 這段時間是花在資料收集、清理、轉換、演算法選擇、模型定型和部署上，以取得認知服務所提供的相同功能層級。 有了機器學習服務，就可以為高度特殊化或特定的問題提供解答。 機器學習服務問題需要熟悉特定主題和問題的相關資料，以及考慮和專業知識。

## <a name="next-steps"></a>後續步驟

- 深入瞭解 [認知服務](https://docs.microsoft.com/azure/cognitive-services/)。
- 尋找 [AI 架構的最佳作法](https://docs.microsoft.com/azure/architecture/solution-ideas/articles/ai-at-the-edge)。
