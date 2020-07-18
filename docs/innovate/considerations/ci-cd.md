---
title: 利用數位家發明實現採用
description: 使用創新方法的成熟度模型來減少因採用而減緩的摩擦，同時保有最佳作法。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: 832c37753393d9c8230ae42f8dbb3cabe7d8e46d
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86449331"
---
<!-- cSpell:ignore deprioritize -->

# <a name="empower-adoption"></a>實現採用

創新的最終測試是客戶對您家發明的反應。 假設是否證明真了？ 客戶是否使用解決方案？ 它是否會調整以符合所需百分比使用者的需求？ 最重要的是，他們是否會繼續回頭？ 在最基本的可行產品（MVP）解決方案部署完成之前，都不會詢問這些問題。 在本文中，我們將著重于採用的專業領域。

## <a name="reduce-friction-that-affects-adoption"></a>減少影響採用的摩擦

有幾個重要的難題，可以透過技術和程式的組合來最小化。 對於具有持續整合（CI）和持續部署（CD）或 DevOps 流程知識的讀者，將會很熟悉下列各項。 本文為雲端採用小組建立燃料創新和意見反應迴圈的起點。 在未來，此起點可能會促進更強大的 CI/CD 或 DevOps 方法，因為產品和團隊成熟。

如[對客戶影響的測量](./measure.md)中所述，任何假設的正面驗證都需要反復專案和判斷。 在任何創新週期中，您會遇到比 wins 更多的失敗。 這是預期行為。 不過，當客戶需要、假設及解決方案的規模調整時，世界會快速變更。 這篇文章的目標是要將速度變慢的[技術尖峰](./build.md#reduce-complexity-and-delay-technical-spikes)降到最低，但仍可確保您保留一些穩固的最佳作法。 這麼做可協助團隊設計未來的成功，同時提供目前的客戶需求。

## <a name="empower-adoption-the-maturity-model"></a>加強採用：成熟度模型

[創新方法](./index.md)的主要目標是建立客戶合作關係並加速意見反應迴圈，這會導致市場創新。 下列影像和章節說明支援此方法的初始執行。

![加強採用：成熟度模型](../../_images/innovate/empower-adoption-maturity.png)

- [共用解決方案](#shared-solution)：針對解決方案的所有層面建立集中式存放庫。
- [意見迴圈](#feedback-loops)：確定可以透過反復專案一致地管理意見反應迴圈。
- [持續整合](#continuous-integration)：定期建立併合並解決方案。
- [可靠的測試](#reliable-testing)：驗證解決方案品質和預期的變更，以確保測試計量的可靠性。
- [解決方案部署](#solution-deployment)：部署解決方案，讓小組可以快速地與客戶共用變更。
- [整合式測量](#integrated-measurements)：將學習計量新增至意見反應迴圈，以供完整小組進行清除分析。

為了將技術尖峰降到最低，假設每個原則一開始都有較低的成熟度。 但最好的方式是配合可隨著假設調整的工具和程式，更精細地進行規劃。 在 Azure 中， [GitHub](https://guides.github.com)和[Azure DevOps](https://docs.microsoft.com/azure/devops)可讓小型團隊開始進行少許困惑。 這些小組可能會成長，包括數千名開發人員，共同處理規模解決方案並測試上百個客戶假設。 本文的其餘部分將說明「計畫大、開始小型」的方法，讓您能夠在每個準則中採用。

## <a name="shared-solution"></a>共用解決方案

如[對客戶影響的測量](./measure.md)中所述，任何假設的正面驗證都需要反復專案和判斷。 在任何創新週期中，您會遇到比 wins 更多的失敗。 這是預期行為。 不過，當客戶需要、假設及解決方案的規模調整時，世界會快速變更。

當您調整創新時，沒有比解決方案的共用程式碼基底更有價值的工具。 可惜的是，沒有可靠的方法可預測哪一個反復專案，或哪些 MVP 會產生獲勝的組合。 這就是為什麼建立共用程式碼基底或儲存機制的原因不太早。 這是一項不應延遲的[技術尖峰](./build.md#reduce-complexity-and-delay-technical-spikes)。 當小組逐一查看各種 MVP 解決方案時，共用存放庫可讓您輕鬆共同作業和加速開發。 當解決方案的變更向下拖曳學習計量時，版本控制可讓您回復為較早且更有效率的方案版本。

管理程式碼存放庫最廣泛採用的工具是[GitHub](https://guides.github.com)，可讓您只需幾個步驟就能建立共用程式碼儲存機制。 此外，Azure DevOps 的[Azure Repos](https://docs.microsoft.com/azure/devops/repos/get-started/what-is-repos?view=azure-devops)功能可以用來建立[Git](https://docs.microsoft.com/azure/devops/repos/get-started/what-is-repos?view=azure-devops#git)或[TFVC](https://docs.microsoft.com/azure/devops/repos/get-started/what-is-repos?view=azure-devops#tfvc)存放庫。

## <a name="feedback-loops"></a>意見反應迴圈

讓客戶參與解決方案，是在創新週期中建立客戶合作關係的關鍵。 這是藉由[測量客戶的影響](./measure.md)來完成。 它需要與客戶進行交談和直接測試。 兩者都會產生必須有效管理的意見反應。

每個意見反應都是客戶需求的可能解決方案。 更重要的是，每位直接客戶的意見反應都代表有機會改善合作關係。 如果意見反應會使其成為 MVP 解決方案，請與客戶一起慶祝。 即使某些意見反應無法採取動作，只是為了推遲意見反應的決策會示範[成長思維](./learn.md#growth-mindset)，並著重于[持續學習](./learn.md#continuous-learning)。

[Azure DevOps](https://docs.microsoft.com/azure/devops)包括[要求、提供及管理意見](https://docs.microsoft.com/azure/devops/project/feedback)反應的方法。 這些工具中的每一個都會將意見反應集中，讓小組可以採取動作，並提供透明意見反應迴圈服務的後續追蹤。

## <a name="continuous-integration"></a>持續整合

隨著採用規模和假設的規模更接近真正的創新，要測試的較小假設數目通常會快速成長。 如需精確的意見反應迴圈和順暢的採用流程，請務必將這些假設整合在一起，並支援創新背後的主要假設。 這表示您也必須快速地移至創新和成長，這需要多個開發人員測試核心假設的變化。 在稍後的階段開發工作中，您甚至可能需要多個開發人員小組，每個都是以共用的解決方案為基礎。 持續整合是管理所有移動元件的第一個步驟。

在持續整合中，程式碼變更經常會合並到主要分支中。 自動化的組建和測試程式可確保主要分支中的程式碼一律具有生產品質。 這可確保開發人員共同作業，以開發提供精確且可靠的意見反應迴圈的共用解決方案。

Azure DevOps 和[Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines)只要 GitHub 或各種其他存放庫中的幾個步驟，就能提供持續整合功能。 深入瞭解[持續整合](https://docs.microsoft.com/azure/devops/learn/what-is-continuous-integration)或查看[實際操作實驗室](https://www.azuredevopslabs.com/labs/azuredevops/continuousintegration)。 另外還有解決方案架構，可透過 Azure DevOps 加速建立[CI/CD 管線](https://azure.microsoft.com/solutions/devops)。

## <a name="reliable-testing"></a>可靠的測試

任何解決方案中的瑕疵都可能會建立誤報或誤否定。 非預期的錯誤可能會輕易地導致使用者採用計量的解釋。 他們也可以從不正確表示假設測試的客戶產生負面意見反應。

在 MVP 解決方案的早期反復專案中，預期會發生瑕疵;早期採用者甚至可能會發現它們可愛。 在早期版本中，接受測試通常不存在。 不過，建立的其中一個層面會考慮到需要的驗證和假設。 兩者都可以在程式碼層級透過單元測試完成，並在部署前手動接受測試。 這兩種方法一起提供一些可靠性來進行測試。 您應該努力自動化一系列定義完善的組建、單元和接受度測試。 這些會確保與假設和所產生解決方案更細微的微調相關的可靠計量。

[Azure Test Plans](https://docs.microsoft.com/azure/devops/test/track-test-status?view=azure-devops)功能提供在手動或自動化測試執行期間開發和操作測試計劃的工具。

## <a name="solution-deployment"></a>解決方案部署

最有意義的層面，就是讓採用考慮能夠控制向客戶發行的解決方案。 藉由提供自助或自動化管線將解決方案發行給客戶，您將會加速意見反應迴圈。 藉由讓客戶能夠快速地與解決方案中的變更互動，您就可以邀請他們進入此流程。 這種方法也會觸發假設的更快速測試，藉此減少假設和可能的返工。

是解決方案部署的數種方法。 以下是最常見的三種：

- **持續部署**是最先進的方法，因為它會自動將程式碼變更部署至生產環境。 對於測試成熟假設的成熟團隊而言，持續部署可能非常有價值。
- 在開發初期階段，**持續傳遞**可能更適當。 在持續傳遞中，任何程式碼變更都會自動部署至類似生產的環境。 開發人員、商務決策者和小組中的其他人可以使用此環境來驗證其工作是否已準備好生產。 您也可以使用這個方法來測試客戶的假設，而不會影響進行中的商務活動。
- **手動部署**是發行管理的最不復雜方法。 正如其名，小組中的某個人會手動部署最新的程式碼變更。 這種方法容易出錯、不可靠，並被大部分經驗豐富的工程師視為反模式。

在 MVP 解決方案的第一次反覆運算期間，即使先前的評估，手動部署也很常見。 當解決方案非常流暢，而且客戶的意見反應不明時，重設整個解決方案（或甚至是核心假設）會有相當大的風險。 以下是手動部署的一般規則：無客戶證明，沒有部署自動化。

提早投資可能會導致遺失的時間。 更重要的是，它可以建立發行管線的相依性，讓小組更能抵抗早期的資料透視。 在前幾個反復專案之後，或當客戶意見反應建議潛在的成功時，應快速採用更先進的部署模型。

在假設驗證的任何階段，Azure DevOps 和[Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines)提供持續傳遞和持續部署功能。 深入瞭解[持續傳遞](https://docs.microsoft.com/azure/devops/learn/what-is-continuous-delivery)，或查看[實際操作實驗室](https://www.azuredevopslabs.com/labs/azuredevops/continuousdeployment)。 解決方案架構也可以透過 Azure DevOps 加速建立您的[CI/CD 管線](https://azure.microsoft.com/solutions/devops)。

## <a name="integrated-measurements"></a>整合式測量

當您[測量對客戶的影響](./measure.md)時，請務必瞭解客戶對解決方案中的變更有何反應。 這項資料（稱為_遙測_）可讓您深入瞭解使用解決方案時，使用者（或使用者）所採取的動作。 從這項資料中，您可以輕鬆地取得假設的量化驗證。 然後，這些計量可以用來調整解決方案，並產生更精細的假設。 在後續的反復專案中，這些細微變更有助於成熟初始解決方案，最後推動大規模重複採用。

在 Azure 中， [Azure 監視器](https://docs.microsoft.com/azure/azure-monitor/overview)提供工具和介面，可收集和查看客戶經驗的資料。 您可以使用[Azure Boards](https://docs.microsoft.com/azure/devops/boards)來套用這些觀察和深入解析，以改善待處理專案。

## <a name="next-steps"></a>後續步驟

在您瞭解實現採用所需的工具和程式之後，現在可以檢查更先進的創新專業領域：[與裝置互動](./devices.md)。 這個專業領域可以協助降低實體和數位體驗之間的障礙，讓您的解決方案更容易採用。

> [!div class="nextstepaction"]
> [與裝置互動](./devices.md)
