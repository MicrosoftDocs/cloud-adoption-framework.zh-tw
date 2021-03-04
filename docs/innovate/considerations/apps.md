---
title: 透過應用程式參與數位發明
description: 瞭解如何建立應用程式解決方案來塑造資料，並建立與客戶互動並支援創新的體驗。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.custom: internal
ms.openlocfilehash: f5ddb6c25b237dee2f6f0d09aef027aedfb1a8f7
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101790250"
---
# <a name="engage-via-applications"></a>透過應用程式參與

如同 [將大眾化資料](./data.md)中所討論，資料是新的石油。 它燃料了數位經濟的創新。 以這種比喻為基礎，應用程式是激發的站和基礎結構，可讓該燃料進入正確的手。

在某些情況下，資料本身就足以推動變更並符合客戶的需求。 更常見的是，客戶需要的解決方案需要應用程式塑造資料並建立體驗。 應用程式是我們與使用者互動的方式，以及回應客戶觸發程式所需的處理常式。 應用程式是客戶提供資料和接收指引的方式。 本文將摘要說明可協助您根據要驗證的假設，為您的應用程式解決方案提供適當應用程式解決方案的數個原則。

![透過應用程式參與](../../_images/innovate/engage-via-apps.png)

## <a name="shared-code"></a>共用程式碼

更快速且正確地回應客戶意見反應、市場變更和創新機會的團隊，通常會在創新中領導各自的市場。 創新應用程式的第一個原則是在 [成長思維](./learn.md#growth-mindset)的考慮下總結：「分享程式碼」。 經過一段時間後，就會從文化焦點中發現創新。 為了維持創新，需要多樣化的觀點和貢獻。

為了準備進行創新，所有應用程式開發都應該以共用程式碼存放庫開始。 管理程式碼存放庫最廣泛採用的工具是 [GitHub](https://guides.github.com)，可讓您快速建立共用的程式碼存放庫。 此外， [Azure 存放庫](/azure/devops/repos/get-started/what-is-repos?view=azure-devops) 是 Azure DevOps Services 中的一組版本控制工具，可用來管理您的程式碼。 Azure 存放庫提供兩種類型的版本控制：

- [Git](/azure/devops/repos/get-started/what-is-repos?view=azure-devops#git)：分散式版本控制。
- [Team Foundation 版本控制 (TFVC) ](/azure/devops/repos/get-started/what-is-repos?view=azure-devops#tfvc)：集中式版本控制。

## <a name="citizen-developers"></a>公民開發人員

專業開發人員是創新的重要元件。 當假設有大規模證明時，專業開發人員必須穩定並準備解決方案以進行調整。 本文中參考的大部分原則都需要專業開發人員的支援。 可惜的是，目前的趨勢建議專業開發人員需要比開發人員更大的需求。 此外，當專業開發被視為必要時，創新的成本和步調可能較不理想。 為了因應這些挑戰，公民開發人員提供一種方式來調整開發工作，並加速提早假設測試。

當早期假設可透過適用于應用程式介面的 [Power Apps](/powerapps/powerapps-overview) 、適用于流程的 AI Builder、適用于流程的 [AI Builder](/powerapps/use-ai-builder) 、適用于工作流程的 [Microsoft Power](/power-automate/) [BI 和 power BI](/power-bi/) 的資料耗用量，來驗證早期的時，使用公民開發人員是可行且有效的

> [!NOTE]
> 當您依賴公民開發人員來測試假設時，建議您提供一些專業開發人員，以提供支援、評論和指引。 當假設經過大規模驗證之後，將應用程式轉換成更健全的程式設計模型的程式，將會加快創新的速度。 藉由及早在流程定義中牽涉到專業開發人員，您稍後可以實現更簡潔的轉換。

## <a name="intelligent-experiences"></a>智慧型體驗

智慧型體驗結合了現代化 web 應用程式的速度和規模，以及認知服務和 bot 的智慧。 這兩種技術都只是為了符合您客戶的需求。 當聰明地合併時，可以透過數位體驗來擴展需要符合的需求，同時協助包含開發成本。

### <a name="modern-web-apps"></a>現代化 Web 應用程式

當需要應用程式或經驗來滿足客戶需求時，新式的 web 應用程式可以是最快速的方式。 新式的 web 體驗可以快速地與內部或外部客戶互動，並允許在解決方案上快速進行反復專案。

### <a name="infusing-intelligence"></a>加入智慧型功能情報

開發人員逐漸推出機器學習和 AI。 一般 Api 的廣泛可用性與預測功能，可讓開發人員透過擴充資料和預測的存取，更符合客戶的需求。

將智慧新增至解決方案可以啟用語音轉換文字、文字翻譯、電腦視覺，甚至是視覺搜尋。 有了這些擴充的功能，開發人員就可以更輕鬆地建立利用智慧來建立互動式和新式體驗的解決方案。

### <a name="bots"></a>Bot

Bot 提供的體驗不像使用電腦更像是處理人員，或至少使用智慧型機器人。 它們可用來轉移簡單、重複性的工作 (例如，將晚餐保留或收集設定檔資訊) 至可能不再需要直接介入的自動化系統上。 使用者透過文字、互動式卡片和語音與 bot 對話。 聊天機器人互動的範圍可從快速問答至複雜的交談，以智慧方式提供服務的存取權。

Bot 非常類似新式的 web 應用程式：它們存留在網際網路上，並使用 Api 來傳送和接收訊息。 視 Bot 的種類而定，Bot 的功能差異很大。 新式 bot 軟體依賴一系列的技術和工具，在各種平臺上提供日益複雜的體驗。 不過，簡單的 Bot 可能只會收到訊息，並以極少的相關程式碼來回應使用者。

Bot 可以執行與其他軟體類型相同的動作：讀取和寫入檔案、使用資料庫和 Api，以及處理一般計算工作。 Bot 的特點就是其使用通常保留給人與人通訊的機制。

## <a name="cloud-native-solutions"></a>雲端原生解決方案

雲端原生應用程式是從頭開始建立的，而且已針對雲端規模和效能進行優化。 雲端原生應用程式通常使用微服務、無伺服器、事件型或容器型的方法來建立。 最常見的雲端原生解決方案會使用微服務架構、受控服務和持續傳遞的組合，以達成可靠性及加快上市時間。

雲端原生解決方案可讓集中式開發團隊掌控商務邏輯，而不需要整合式的集中式解決方案。 這種類型的解決方案也會建立一個錨點，以促進公民開發人員和新式體驗的輸入之間的一致性。 最後，雲端原生解決方案提供創新的加速器，讓公民和專業開發人員能夠安全地創新，而且至少有封鎖程式。

## <a name="innovate-through-existing-solutions"></a>透過現有解決方案創新

許多客戶假設最能以現代化版本的現有解決方案來提供。 當目前的商務邏輯符合客戶需求時 (或真的很接近) ，您可以藉由在現代化解決方案之上建立創新來加速創新。

大部分的現代化形式（包括輕微的應用程式重構）都包含在雲端採用架構內的 [遷移方法](../../migrate/index.md) 中。 該方法會引導雲端採用小組將 [數位資產](../../digital-estate/index.md) 遷移至雲端的過程。 [Azure 遷移指南](../../migrate/azure-migration-guide/index.md)為相同的方法提供簡單的方法，適用于少數工作負載或甚至單一應用程式。

在遷移和現代化解決方案之後，有各種不同的方式可用來建立新的創新解決方案，以滿足客戶的需求。 例如， [公民開發人員](#citizen-developers) 可以測試假設，或專業開發人員可以建立 [智慧型體驗](#intelligent-experiences) 或 [雲端原生解決方案](#cloud-native-solutions)。

### <a name="extend-an-existing-solution"></a>擴充現有的方案

擴充解決方案是一種常見的現代化形式。 當客戶假設有下列情況時，這種方法可以是創新的最快途徑：

- 現有的商務邏輯應該符合 (，或接近符合現有客戶需求的) 。
- 改善的體驗可以更符合特定客戶世代的需求。
- 最小可行產品 (MVP) 解決方案所需的商務邏輯，通常是透過多 [層](/azure/architecture/guide/architecture-styles/n-tier)式、web 服務、API 或 [微服務](/azure/architecture/guide/architecture-styles/microservices) 設計來集中進行。 此方法包含將現有解決方案包裝在雲端託管的新體驗內。 在 Azure 中，此解決方案可能會存留在 Azure App Service 中。

### <a name="rebuild-an-existing-solution"></a>重建現有的方案

如果無法輕鬆地擴充應用程式，可能需要重構解決方案。 在此方法中，會將工作負載遷移至雲端。 遷移應用程式之後，會將一部分的部分修改或複製為 web 服務或 [微服務](/azure/architecture/guide/architecture-styles/microservices)，並與現有的解決方案平行部署。 以平行服務為基礎的解決方案可視為擴充的解決方案。 此解決方案只會將現有的解決方案包裝在雲端託管的新體驗。 在 Azure 中，此解決方案可能會存留在 Azure App Service 中。

> [!CAUTION]
> 重構或重新架構解決方案或集中商務邏輯，可快速觸發耗時的 [技術尖峰](./build.md#reduce-complexity-and-delay-technical-spikes) ，而不是客戶價值的來源。 這是創新的風險，特別是在假設驗證的早期。 有了解決方案設計的創意，就應該有 MVP 的途徑，而不需要重構現有的解決方案。 最好是延遲重構，直到可以大規模驗證初始假設為止。

## <a name="operating-model-innovations"></a>作業模型創新

除了新式創新方法來建立應用程式，應用程式作業也有顯著的創新。 這些方法衍生了許多組織的移動。 最顯著的是 [雲端中心卓越](../../organize/cloud-center-of-excellence.md) 的營運模式。 當完整的個人擁有和成熟時，商務團隊可以選擇為解決方案提供自己的操作支援。

雲端中心內的自助營運管理模型類型，可讓您在解決方案環境內進行更緊密的控制和更快速的反覆運算。 這些目標是藉由將營運控制和責任轉移給商務小組來達成。

如果您嘗試調整或符合現有解決方案的全球需求，此方法可能就足以驗證客戶假設。 在遷移方案且稍微現代化之後，商務小組可以調整規模以測試各種假設。 這些通常牽涉到與效能、全球散發，以及 IT 營運妨礙運作的其他客戶需求相關的客戶世代。

## <a name="reduce-overhead-and-management"></a>減少額外負荷和管理

解決方案中的維護愈多，解決方案將會以較慢的速度進行反覆運算。 這表示您可以藉由降低作業對可用頻寬的影響來加速創新。

若要準備提供創新解決方案所需的許多反覆運算，請務必事先考慮。 例如，藉由 favoring 無伺服器選項，在流程初期將營運負擔降至最低。 在 Azure 中，無伺服器應用程式選項可能包含 [Azure App Service](/azure/app-service/overview) 或 [容器](/azure/containers/)。

Azure 會以平行方式提供無伺服器的交易資料選項，也可降低額外負荷。 [Azure 產品目錄](/azure/)提供可裝載資料的資料庫選項，而不需要完整的資料平臺。

## <a name="next-steps"></a>下一步

根據假設和解決方案，本文中的原則可協助您設計符合 MVP 定義的應用程式，以及與使用者互動。 接下來是授權 [採用](./ci-cd.md)的原則，可讓您更快速且更有效率地將應用程式和資料提供給客戶。

> [!div class="nextstepaction"]
> [實現採用](./ci-cd.md)
