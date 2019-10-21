---
title: 雲端創新：加強採用
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 雲端創新簡介-加強採用
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/24/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: 727686b70c7fb604274f74da7a043f589f79fa4c
ms.sourcegitcommit: 35c162d2d09ec1c4a57d3d57a5db1d56ee883806
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2019
ms.locfileid: "72557215"
---
# <a name="empower-adoption"></a>加強採用

創新的最終測試是客戶對您家發明的反應。 假設是否證明真了？ 客戶是否使用解決方案？ 它是否會調整以符合所需百分比使用者的需求？ 最重要的是，他們是否會繼續回頭？ 在部署 MVP 解決方案之前，都不會詢問這些問題。 在本文中，我們將著重于採用的專業領域。

## <a name="reducing-friction-to-adoption"></a>減少採用的摩擦

有幾個重要的難題，可以透過技術和程式的組合來最小化。 對於熟悉 CI/CD 或 DevOps 流程的讀者而言，下列內容看起來會非常類似。 本文的重點是為雲端採用小組建立起點，這將會帶來創新和意見反應迴圈。 長期來說，此起點可能會隨著產品和團隊的成熟而變得更健全的 CI/CD 或 DevOps 方法。

在[測量客戶影響](./measure.md)方面所述，任何假設的正面驗證都需要反復專案和判斷。 在任何創新週期期間，您將會有比 wins 更多的失敗情形。 這是預期行為。 不過，當客戶的需求、假設及解決方案的規模調整時，世界會快速變更。 本文的目標是要將[技術尖峰](./build.md#reduce-complexity-and-delay-technical-spikes)降到最低，這會降低創新的速度，但請確定已有幾個穩固的最佳作法。 這麼做可協助團隊設計未來的成功，但提供目前的客戶需求。

## <a name="empowering-adoption---maturity-model"></a>促進採用成熟度模型

[創新方法](./index.md)的主要目標是建立客戶合作關係並加速意見反應迴圈，這會導致市場創新。
下列影像和內容區段描述將支援方法的初始執行。

![加強採用成熟度模型](../../_images/innovate/empower-adoption-maturity.png)

- [共用解決方案](#shared-solution)：針對解決方案的所有層面建立集中式存放庫。
- [意見迴圈](#feedback-loops)：確保意見反應迴圈可以透過反復專案一致地進行管理。
- [持續整合](#continuous-integration)：定期建立併合並解決方案。
- [可靠的測試](#reliable-testing)：驗證解決方案品質和預期的變更，以確保量值。
- [解決方案部署](#solution-deployment)：解決方案的部署，可讓小組快速地與客戶共用變更。
- [整合式測量](#integrated-measurements)：將學習計量新增至意見反應迴圈，以供完整小組進行清除分析。

將技術尖峰降到最低，假設每個原則一開始都有較低的成熟度。 但是，請預先規劃，以配合可隨著假設調整的工具和程式變得更精細。 在 Azure 中， [GitHub](https://guides.github.com)和[Azure DevOps](https://docs.microsoft.com/azure/devops)的組合，可讓小型團隊開始進行少許困惑。 然後成長到數千名開發人員，共同作業以測試數百個客戶假設的規模解決方案。 本文的其餘部分將說明此方案的 big，開始針對每一項原則採用簡單的方法。

## <a name="shared-solution"></a>共用解決方案

在[測量客戶影響](./measure.md)方面所述，任何假設的正面驗證都需要反復專案和判斷。 在任何創新週期期間，您將會有比 wins 更多的失敗情形。 這是預期行為。 不過，當客戶的需求、假設及解決方案的規模調整時，世界會快速變更。

在調整創新時，沒有比解決方案的共用程式碼基底更有價值的工具。 可惜的是，沒有辦法預測哪一個反復專案或哪些 MVP 會產生獲勝的組合。 這就是為什麼要建立共用程式碼基底或儲存機制的原因不太早。 這是一項不應延遲的[技術尖峰](./build.md#reduce-complexity-and-delay-technical-spikes)。 當小組逐一查看各種 MVP 解決方案時，共用存放庫可讓您輕鬆進行共同作業和加速開發。 當解決方案的變更導致對學習計量產生負面影響時，版本控制可讓您復原到先前、更有效率的解決方案版本。

管理程式碼存放庫最常用的工具是[GitHub](https://guides.github.com) ，只要按幾下滑鼠就能建立共用程式碼儲存機制。 此外，Azure DevOps 的[Azure Repos](https://docs.microsoft.com/azure/devops/repos/get-started/what-is-repos?view=azure-devops)功能可以用來建立[Git](https://docs.microsoft.com/azure/devops/repos/get-started/what-is-repos?view=azure-devops#git)或[Team Foundation](https://docs.microsoft.com/azure/devops/repos/get-started/what-is-repos?view=azure-devops#tfvc)存放庫。

## <a name="feedback-loops"></a>意見反應迴圈

在創新週期中建立客戶合作關係的關鍵是讓客戶成為解決方案的一部分。 這是藉由[測量客戶的影響](./measure.md)來以扶手的長度來完成。 更密切的是，它需要與客戶進行交談和直接測試。 這兩個結果都會導致必須管理的意見反應。

每個意見反應都是客戶需求的可能解決方案。 更重要的是，直接來自客戶的每個意見反應都是改善合作關係的機會。 如果意見反應會使其成為 MVP 解決方案，請與客戶一起慶祝。 即使意見反應無法採取動作，只是為了推遲意見反應而透明化，示範[成長思維](./learn.md#growth-mindset)，並著重于[持續學習](./learn.md#continuous-learning)。

[Azure DevOps](https://docs.microsoft.com/azure/devops)包括[要求、提供及管理意見](https://docs.microsoft.com/azure/devops/project/feedback)反應的方法。 這其中每一項工具都集中了意見反應，讓小組可以採取動作並報告意見反應，並提供透明的意見反應迴圈。

## <a name="continuous-integration"></a>持續整合

隨著採用規模和假設的規模越來越接近真正的創新，要測試的較小假設數目將會快速成長。 如需精確的意見反應迴圈和順暢的採用流程，請務必將這些假設都整合在一起，並支援創新背後的主要假設。 可惜的是，您也必須快速地進行創新和成長，這將需要多個開發人員測試核心假設的變化。 針對稍後的開發工作，它可能需要多個開發人員小組，每個都有一個分享的解決方案。 持續整合是管理各種移動元件的第一步。

在持續整合中，程式碼變更經常會合並到主要分支中。 自動化的組建和測試流程可確保主要分支中的程式碼一律具有生產品質。 這可確保開發人員共同作業，以開發提供精確且可靠的意見反應迴圈的共用解決方案。

Azure DevOps 和[Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines)只要按幾下，就能為 GitHub 或各種其他存放庫提供持續整合功能。
如需詳細資訊，請深入瞭解[持續整合](https://docs.microsoft.com/azure/devops/learn/what-is-continuous-integration)或執行[實際操作實驗室](https://www.azuredevopslabs.com/labs/azuredevops/continuousintegration)。 另外還有解決方案架構，可以使用 Azure DevOps 加速建立[CI/CD 管線](https://azure.microsoft.com/solutions/devops)。

## <a name="reliable-testing"></a>可靠的測試

任何解決方案中的瑕疵都可能會建立誤報或誤否定。 非預期的錯誤可能會輕易地導致使用者採用計量的解釋。 它也可能會導致客戶無法正確表示假設測試的負面意見反應。

在 MVP 解決方案的早期反復專案中，預期會發生瑕疵，早期採用者甚至可以找到他們的可愛。 在早期版本中，接受測試可能不存在。 不過，使用「理解」的其中一個層面就是驗證需求和假設。 兩者都可以在程式碼層級透過單元測試完成，並在部署前手動接受測試。 這些方法會一起提供一些可靠性來進行測試。 長期而言，一系列定義完善的組建、單元和接受度測試應該會自動化，以確保可靠的計量與更細微的調整，以因應假設和產生的解決方案。

[Azure Test Plans](https://docs.microsoft.com/azure/devops/test/track-test-status?view=azure-devops)提供在手動或自動化測試執行期間開發和操作測試計劃的工具。

## <a name="solution-deployment"></a>解決方案部署

允許採用的最有意義的層面，就是能夠控制向客戶發行的解決方案。 提供自助或自動化管線，將解決方案發行給客戶可加速回饋迴圈。 讓客戶能夠快速地與解決方案中的變更互動，並邀請客戶進入此流程。 它也可讓您更快速地測試假設，以減少假設及可能的返工。

有數種方法可以部署解決方案，以下概述3個最常見的方法：

- 最先進的方法 [**持續部署**] 會自動將程式碼變更部署至生產環境。 針對成熟的團隊測試成熟的假設，持續部署可能非常有價值。
- 在開發初期階段，**持續傳遞**可能更適當。 在持續傳遞中，任何程式碼變更都會自動部署至類似生產的環境。 開發人員、商務決策者和小組中的其他人可以使用此環境來驗證其工作是否已準備好生產。 它也可以用來測試客戶的假設，而不會影響進行中的商務活動。
- **手動部署**是發行管理的最先進方法。 正如其名，小組中的某個人會手動部署最新的程式碼變更。 這種方法容易出錯、不可靠，並被大部分經驗豐富的工程師視為反模式。

在 MVP 解決方案的第一次反覆運算期間，手動部署是常見的方法，而不是上述的說明。 當解決方案非常流暢，而且客戶的意見反應不明時，重設整個解決方案（或甚至是核心假設）會有顯著的風險。 手動部署的一般規則：無客戶證明，沒有部署自動化。 提早投資可能會導致遺失的時間。 更重要的是，它可以建立發行管線的相依性，讓小組更能抵抗早期的資料透視。 在前幾個反復專案之後，或當客戶意見反應建議潛在的成功時，應快速採用更先進的部署模型。

在假設驗證的任何階段，Azure DevOps 和[Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines)只要按幾下滑鼠，就能提供持續傳遞和持續部署功能。 如需詳細資訊，請深入瞭解[持續傳遞](https://docs.microsoft.com/azure/devops/learn/what-is-continuous-delivery)或執行[實際操作實驗室](https://www.azuredevopslabs.com/labs/azuredevops/continuousdeployment)。 解決方案架構也可以使用 Azure DevOps 加速建立[CI/CD 管線](https://azure.microsoft.com/solutions/devops)。

## <a name="integrated-measurements"></a>整合式測量

當[測量對客戶的影響](./measure.md)時，請務必瞭解客戶對解決方案中的變更有何反應。 這項資料稱為遙測，可讓您深入瞭解使用解決方案時，使用（或使用者的世代）所採取的動作。 從這項資料中，您可以輕鬆地取得假設的量化驗證。 然後，這些計量可以用來調整解決方案，並產生更精細的假設。 在後續的反復專案中，這些細微變更有助於成熟初始解決方案，最後推動大規模重複採用。

在 Azure 中， [Azure 監視器](https://docs.microsoft.com/azure/azure-monitor/overview)提供工具和介面，以收集及查看來自客戶體驗的資料。 這些觀察和深入解析可用於使用[Azure Boards](https://docs.microsoft.com/azure/devops/boards)來精簡待處理專案。

## <a name="next-steps"></a>後續步驟

瞭解實現採用所需的工具和程式，現在可以檢查更先進的創新專業領域，[與裝置互動](./devices.md)。 這有助於降低實體和數位體驗之間的障礙，讓您的解決方案更容易採用。

> [!div class="nextstepaction"]
> [與裝置互動](./devices.md)
