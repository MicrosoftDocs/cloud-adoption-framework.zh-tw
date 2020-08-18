---
title: 以數位發明實現採用
description: 您可以使用創新方法的成熟度模型，來減少會減緩採用的衝突，同時保持最佳做法。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: 8aa265343cfb2970c32649728d1703b4ed348b3b
ms.sourcegitcommit: 917188fa930cadddb03f9e9bbcdd7b630e4ee33e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/17/2020
ms.locfileid: "88284532"
---
<!-- cSpell:ignore deprioritize -->

# <a name="empower-adoption-with-digital-invention"></a>以數位發明實現採用

最終的創新測試是客戶對您發明的反應。 假設是否證明真？ 客戶使用解決方案嗎？ 它是否可調整以符合所需的使用者百分比需求？ 最重要的是，他們是否持續回來？ 在部署最小可行產品 (MVP) 解決方案之前，都不會詢問這些問題。 在本文中，我們將著重于授權採用的專業領域。

## <a name="reduce-friction-that-affects-adoption"></a>減少影響採用的摩擦

有幾個關鍵的因素可透過結合技術和程式來最小化。 若讀者瞭解持續整合 (CI) 和持續部署 (CD) 或 DevOps 程式，則會很熟悉下列各項。 本文為雲端採用小組建立了燃料創新和意見反應迴圈的起點。 未來，此起始點可能會促進更健全的 CI/CD 或 DevOps 方法，因為產品和團隊成熟。

如 [對客戶的影響進行測量](./measure.md)所述，任何假設的正面驗證都需要反復專案和判斷。 在任何創新週期期間，您將會遇到比 wins 更多的失敗。 這是預期行為。 不過，當客戶需要、假設和解決方案大規模調整時，世界會快速變更。 本文旨在將減緩創新的 [技術尖峰](./build.md#reduce-complexity-and-delay-technical-spikes) 降至最低，但仍可確保您保持一些穩固的最佳作法。 這樣做將有助於團隊設計未來的成功，同時提供目前客戶的需求。

## <a name="empower-adoption-the-maturity-model"></a>強化採用：成熟度模型

[創新方法](./index.md)的主要目標是建立客戶合作關係並加速意見反應迴圈，這會導致市場創新。 下圖和章節說明支援此方法的初始支援。

![強化採用：成熟度模型](../../_images/innovate/empower-adoption-maturity.png)

- [共用解決方案](#shared-solution)：為解決方案的所有層面建立集中式存放庫。
- [意見反應迴圈](#feedback-loops)：確定可以透過反覆運算一致地管理意見反應迴圈。
- [持續整合](#continuous-integration)：定期建立和合併解決方案。
- [可靠的測試](#reliable-testing)：驗證解決方案品質和預期的變更，以確保測試計量的可靠性。
- [解決方案部署](#solution-deployment)：部署解決方案，讓小組能夠快速地與客戶共用變更。
- [整合式測量](#integrated-measurements)：將學習計量新增至意見反應迴圈，以供完整的小組進行清楚的分析。

若要將技術尖峰降至最低，請假設在每個原則中的成熟度一開始都很低。 但是，藉由調整可隨著假設進行調整的工具和程式，以更精細的方式進行規劃。 在 Azure 中， [GitHub](https://guides.github.com) 和 [Azure DevOps](/azure/devops) 可讓小型團隊開始使用一些小小的部分。 這些小組可能會成長，包括數千位開發人員，這些開發人員會共同處理規模的解決方案，並測試上百個客戶假設。 本文的其餘部分將說明如何在每個原則中採用「全計畫」、「開始小型」的方法。

## <a name="shared-solution"></a>共用解決方案

如 [對客戶的影響進行測量](./measure.md)所述，任何假設的正面驗證都需要反復專案和判斷。 在任何創新週期期間，您將會遇到比 wins 更多的失敗。 這是預期行為。 不過，當客戶需要、假設和解決方案大規模調整時，世界會快速變更。

當您調整創新時，不會有比解決方案的共用程式碼基底更有價值的工具。 可惜的是，沒有可靠的方式可以預測哪個反復專案或哪些 MVP 會產生獲獎的組合。 這就是為什麼建立共用程式碼基底或存放庫絕對不太早的原因。 這是一項不應該延遲的 [技術尖峰](./build.md#reduce-complexity-and-delay-technical-spikes) 。 當小組逐一查看各種 MVP 解決方案時，共用存放庫可讓您輕鬆地共同作業和加速開發。 當解決方案的變更向下拖曳學習計量時，版本控制可讓您回復為較早、更有效的解決方案版本。

管理程式碼存放庫最廣泛採用的工具是 [GitHub](https://guides.github.com)，可讓您只需幾個步驟，就能建立共用的程式碼存放庫。 此外，Azure DevOps 的 [Azure Repos](/azure/devops/repos/get-started/what-is-repos?view=azure-devops) 功能可以用來建立 [Git](/azure/devops/repos/get-started/what-is-repos?view=azure-devops#git) 或 [TFVC](/azure/devops/repos/get-started/what-is-repos?view=azure-devops#tfvc) 存放庫。

## <a name="feedback-loops"></a>意見反應迴圈

讓客戶的解決方案成為解決方案的一部分，是在創新週期內建立客戶合作關係的關鍵。 這是透過 [測量客戶的影響](./measure.md)來完成。 它需要與客戶進行交談和直接測試。 兩者都會產生必須有效管理的意見反應。

每個意見反應都是客戶需求的潛在解決方案。 更重要的是，每位直接客戶的意見反應都代表提升合作關係的機會。 如果意見反應成為 MVP 解決方案，請與客戶一起慶祝。 即使有些意見反應無法採取行動，但推遲意見反應的決策也會示範 [成長思維](./learn.md#growth-mindset) ，並著重于 [持續學習](./learn.md#continuous-learning)。

[Azure DevOps](/azure/devops) 包括 [要求、提供和管理意見](/azure/devops/project/feedback)反應的方式。 這些工具每一個都會將意見反應集中，讓小組可以採取行動，並提供透明回饋迴圈的後續服務。

## <a name="continuous-integration"></a>持續整合

隨著採用規模和假設更接近真正的大規模創新，要測試的較小假設數目通常會快速成長。 為了取得準確的意見反應迴圈和順利採用流程，請務必將這些假設整合，並支援創新背後的主要假設。 這表示您也必須快速移動以創新和成長，這需要多位開發人員測試核心假設的變化。 針對稍後的階段開發工作，您可能甚至需要多個開發人員小組，而每個開發人員都在共用方案中打造。 持續整合是管理所有移動元件的第一步。

在持續整合中，程式碼變更經常會合並到主要分支中。 自動化的組建和測試程式可確保主要分支中的程式碼一律為生產環境品質。 這可確保開發人員共同作業，以開發提供準確且可靠的意見反應迴圈的共用解決方案。

Azure DevOps 和 [Azure Pipelines](/azure/devops/pipelines) 只需在 GitHub 或各種其他存放庫中執行幾個步驟，就能提供持續整合功能。 深入瞭解 [持續整合](/azure/devops/learn/what-is-continuous-integration)，或如需詳細資訊，請參閱 [實際操作實驗室](https://www.azuredevopslabs.com/labs/azuredevops/continuousintegration)。 解決方案架構可透過 [Azure DevOps 加快 CI/CD 管線](https://azure.microsoft.com/solutions/devops)的建立速度。

## <a name="reliable-testing"></a>可靠的測試

任何解決方案中的瑕疵都可能會產生誤報或誤否定。 非預期的錯誤很容易就會導致使用者採用計量的解釋。 他們也會產生負面的意見反應，讓客戶無法精確地表示您的假設測試。

在 MVP 解決方案的早期反復專案中，預期會發生瑕疵;早期採用者甚至可能會發現它們可愛。 在早期版本中，接受度測試通常不存在。 但是，其中一個建立的層面會理解需要和假設的驗證。 兩者都可以透過程式碼層級的單元測試完成，並在部署前手動接受測試。 這些都是在測試時提供一些可靠性的方法。 您應致力於自動化一系列定義完善的組建、單元和接受度測試。 這些可確保可靠的計量，與對假設和產生的解決方案進行更細微的調整相關。

[Azure Test Plans](/azure/devops/test/track-test-status?view=azure-devops)功能提供在手動或自動化測試執行期間開發和操作測試計劃的工具。

## <a name="solution-deployment"></a>解決方案部署

可能是讓採用考慮您控制解決方案發行給客戶的能力，這可能是最有意義的層面。 藉由提供自助或自動化管線來將解決方案發行給客戶，您將會加速意見反應迴圈。 藉由讓客戶快速與解決方案中的變更互動，您可以邀請他們進入流程。 這種方法也會觸發較快速的假設測試，藉此減少假設和可能的修改。

有數種方法可以部署方案。 以下是最常見的三種：

- **持續部署** 是最先進的方法，因為它會自動將程式碼變更部署至生產環境。 針對測試成熟假設的成熟團隊，持續部署可能非常有價值。
- 在開發初期階段， **持續傳遞** 可能更適合。 在持續傳遞中，任何程式碼變更都會自動部署至類似生產的環境。 開發人員、商務決策者和小組的其他人員都可以使用此環境來確認其工作是否已準備好用於生產環境。 您也可以使用此方法來測試客戶的假設，而不會影響進行中的商務活動。
- **手動部署** 是發行管理的最複雜方法。 顧名思義，小組的某個人會手動部署最新的程式碼變更。 這種方法很容易出錯、不可靠，而且是由許多經驗豐富的工程師所反模式。

在 MVP 解決方案的第一次反覆運算期間，即使先前的評量是手動部署也是常見的。 當解決方案非常流暢且客戶的意見反應不明時，重設整個解決方案 (甚至是核心假設) 會有很大的風險。 以下是手動部署的一般規則：無客戶證明，無部署自動化。

及早投資可能會導致遺失的時間。 更重要的是，它可以建立發行管線的相依性，讓小組更能抵抗早期的 pivot。 在前幾個反復專案之後，或當客戶意見反應可能成功時，應快速採用更先進的部署模型。

在假設驗證的任何階段，Azure DevOps 和 [Azure Pipelines](/azure/devops/pipelines) 提供持續傳遞和持續部署功能。 深入瞭解 [持續傳遞](/azure/devops/learn/what-is-continuous-delivery)，或查看 [實際操作實驗室](https://www.azuredevopslabs.com/labs/azuredevops/continuousdeployment)。 解決方案架構也可以透過 Azure DevOps，加速建立您的 [CI/CD 管線](https://azure.microsoft.com/solutions/devops)。

## <a name="integrated-measurements"></a>整合式度量

當您 [測量對客戶的影響](./measure.md)時，請務必瞭解客戶對解決方案中的變更有何反應。 這項資料（稱為 _遙測_資料）提供 (使用者在使用解決方案時) 所採取之動作的深入解析。 從這項資料，很容易就能取得假設的量化驗證。 然後可以使用這些計量來調整解決方案，並產生更精細的假設。 這些細微變更有助於在後續的反復專案中成熟初始解決方案，最終促使大規模重複採用。

在 Azure 中， [Azure 監視器](/azure/azure-monitor/overview) 提供工具和介面，可從客戶體驗收集和查看資料。 您可以使用 [Azure Boards](/azure/devops/boards)來套用這些觀察和見解，以精簡待處理專案。

## <a name="next-steps"></a>後續步驟

在您瞭解強化採用所需的工具和程式之後，現在可以檢查更先進的創新專業領域： [與裝置互動](./devices.md)。 此專業領域有助於降低實體和數位體驗之間的障礙，讓您的解決方案更容易採用。

> [!div class="nextstepaction"]
> [與裝置互動](./devices.md)