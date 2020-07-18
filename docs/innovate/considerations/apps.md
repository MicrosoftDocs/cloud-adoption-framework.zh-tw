---
title: 透過應用程式參與數位家發明
description: 瞭解如何建立應用程式解決方案來塑造資料，並建立與客戶互動並支援創新的體驗。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: 72f5a35bbf4fa248b82dce36fb345ee22a6b96f0
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86449569"
---
# <a name="engage-via-applications"></a>透過應用程式參與

如[將大眾化資料](./data.md)中所述，資料是新的石油。 它燃料了各種數位經濟的創新。 以這種比喻為基礎，應用程式是將該燃料放入正確手中所需的激發站和基礎結構。

在某些情況下，單獨的資料就足以推動變更並符合客戶的需求。 更常見的情況是，客戶需求的解決方案需要應用程式來塑造資料並建立經驗。 應用程式是我們與使用者互動的方式，以及回應客戶觸發程式所需之處理的首頁。 應用程式是客戶提供資料和接收指引的方式。 本文摘要說明一些可協助您根據要驗證的假設，為您提供正確應用程式解決方案的原則。

![透過應用程式參與](../../_images/innovate/engage-via-apps.png)

## <a name="shared-code"></a>共用程式碼

小組可以更快速且精確地回應客戶的意見反應、市場變化，以及創新的機會，通常會將其各自的市場導向創新。 創新應用程式的第一個原則會匯總在[成長思維](./learn.md#growth-mindset)方式中：「共用程式碼」。 經過一段時間後，創新就會從文化的焦點中浮現出來。 為了維持創新，需要不同的觀點和貢獻。

為了準備好進行創新，所有應用程式的開發都應該從共用程式碼存放庫開始。 管理程式碼存放庫最廣泛採用的工具是[GitHub](https://guides.github.com)，可讓您快速建立共用程式碼儲存機制。 或者， [Azure Repos](https://docs.microsoft.com/azure/devops/repos/get-started/what-is-repos?view=azure-devops)是 Azure DevOps Services 中的一組版本控制工具，可用來管理您的程式碼。 Azure Repos 提供兩種類型的版本控制：

- [Git](https://docs.microsoft.com/azure/devops/repos/get-started/what-is-repos?view=azure-devops#git)：分散式版本控制。
- [Team Foundation 版本控制（TFVC）](https://docs.microsoft.com/azure/devops/repos/get-started/what-is-repos?view=azure-devops#tfvc)：集中式版本控制。

## <a name="citizen-developers"></a>公民開發人員

專業開發人員是創新的重要元件。 當假設有正確的大規模證明時，專業開發人員就必須穩定並準備解決方案以進行調整。 本文中所參考的大部分原則都需要專業開發人員的支援。 可惜的是，目前的趨勢建議專業開發人員的需求比開發人員還多。 此外，當您認為需要專業開發時，創新的成本和步調也比較不理想。 為了因應這些挑戰，公民開發人員提供一種方法來調整開發工作，並加速早期假設測試。

當早期假設可以透過應用程式介面的[Power Apps](https://docs.microsoft.com/powerapps/powerapps-overview) 、流程和預測的[AI](https://docs.microsoft.com/powerapps/use-ai-builder)產生器、適用于工作流程的[Microsoft 電源自動化](https://docs.microsoft.com/power-automate)， [Power BI](https://docs.microsoft.com/power-bi)以及用於資料耗用量的工具進行驗證時，使用公民開發人員可以是可行且有效的。

> [!NOTE]
> 當您依賴公民開發人員測試假設時，建議您最好讓一些專業開發人員提供支援、審查和指導方針。 假設經過大規模驗證之後，將應用程式轉換成更健全的程式設計模型的流程，會加速創新。 藉由在早期的流程定義中牽涉到專業開發人員，您可以在稍後實現更整潔的轉換。

## <a name="intelligent-experiences"></a>智慧型體驗

智慧型體驗結合了現代化 web 應用程式的速度和規模，以及認知服務和 bot 的智慧。 這兩種技術都可以單獨滿足您的客戶需求。 當聰明地結合時，它們會擴大可透過數位體驗達成的需求範圍，同時協助包含開發成本。

### <a name="modern-web-apps"></a>現代化 Web 應用程式

當需要應用程式或經驗以滿足客戶需求時，新式 web 應用程式可能是最快的方式。 新式 web 體驗可以快速地與內部或外部客戶互動，並允許在解決方案上快速反覆運算。

### <a name="infusing-intelligence"></a>加入智慧型功能情報

機器學習服務和 AI 已逐漸提供給開發人員。 具有預測性功能的通用 Api 可廣泛使用，讓開發人員能夠透過擴充的資料和預測存取，更符合客戶的需求。

將智慧新增至解決方案，可以啟用語音轉換文字、文字翻譯、電腦視覺，甚至是視覺效果搜尋。 透過這些擴充的功能，開發人員可以更輕鬆地建立利用智慧的解決方案，以建立互動式和現代化的體驗。

### <a name="bots"></a>Bot

Bot 提供的體驗不如使用電腦，更像是處理人員，至少是使用智慧型機器人。 它們可以用來將簡單的重複性工作（例如建立晚餐或收集設定檔資訊）移到可能不再需要直接人為介入的自動化系統上。 使用者透過文字、互動式卡片和語音與 bot 對話。 Bot 互動的範圍可從快速的問題和答案，到聰明地提供服務存取權的複雜交談。

Bot 很像現代化 web 應用程式：它們會在網際網路上上線，並使用 Api 來傳送和接收訊息。 視 Bot 的種類而定，Bot 的功能差異很大。 新式 bot 軟體依賴一系列技術和工具，在各種平臺上提供日益複雜的體驗。 不過，簡單的 Bot 可能只會收到訊息，並以極少的相關程式碼來回應使用者。

Bot 可以執行與其他軟體類型相同的工作：讀取和寫入檔案、使用資料庫和 Api，以及處理一般的計算工作。 Bot 的特點就是其使用通常保留給人與人通訊的機制。

## <a name="cloud-native-solutions"></a>雲端原生解決方案

雲端原生應用程式是從頭開始建立的，並已針對雲端規模和效能進行優化。 雲端原生應用程式通常是使用微服務、無伺服器、以事件為基礎或以容器為基礎的方法來建立。 最常使用的雲端原生解決方案結合了微服務架構、受控服務和持續傳遞，以達成可靠性並加快上市時間。

雲端原生解決方案可讓集中式開發小組維持商務邏輯的控制，而不需要整合型、集中式解決方案。 這種類型的解決方案也會建立錨點，以促進公民開發人員和現代化經驗的輸入之間的一致性。 最後，雲端原生解決方案藉由釋放公民和專業開發人員，以安全且最少的封鎖程式來創新，以提供創新的加速器。

## <a name="innovate-through-existing-solutions"></a>透過現有的解決方案創新

許多客戶假設最適合透過現有解決方案的現代化版本來傳遞。 當目前的商務邏輯符合客戶的需求（或實際上已關閉）時，您可以在現代化解決方案之上建立來加速創新。

大部分的現代化形式，包括稍微重構應用程式，都包含在雲端採用架構內的[遷移方法](../../migrate/index.md)中。 該方法會引導雲端採用小組將[數位資產](../../digital-estate/index.md)遷移至雲端的過程。 [Azure 遷移指南](../../migrate/azure-migration-guide/index.md)為相同的方法提供了一種簡單的方法，適用于少量的工作負載或甚至是單一應用程式。

在遷移並現代化解決方案之後，有多種方式可用來建立新的創新解決方案，以滿足客戶的需求。 例如，[公民開發人員](#citizen-developers)可以測試假設，或專業開發人員可以建立[智慧型體驗](#intelligent-experiences)或[雲端原生解決方案](#cloud-native-solutions)。

### <a name="extend-an-existing-solution"></a>擴充現有的解決方案

擴充解決方案是一種常見的現代化形式。 當下列條件成立時，此方法可能是最快速的創新途徑：

- 現有的商務邏輯應該符合現有客戶的需求（或接近會議）。
- 改善的體驗會更符合特定客戶世代的需求。
- 最基本的可行產品（MVP）解決方案所需的商務邏輯，通常是透過多[層](https://docs.microsoft.com/azure/architecture/guide/architecture-styles/n-tier)式 web 服務、API 或[微服務](https://docs.microsoft.com/azure/architecture/guide/architecture-styles/microservices)設計來進行。 此方法包含將現有的解決方案包裝在雲端託管的新體驗中。 在 Azure 中，此解決方案可能會出現在 Azure App Service 中。

### <a name="rebuild-an-existing-solution"></a>重建現有的解決方案

如果無法輕鬆擴充應用程式，可能需要重構解決方案。 在此方法中，工作負載會遷移至雲端。 遷移應用程式之後，會將其部分修改或複製為與現有解決方案平行部署的 web 服務或[微服務](https://docs.microsoft.com/azure/architecture/guide/architecture-styles/microservices)。 以平行服務為基礎的解決方案可視為擴充解決方案。 此解決方案只會將現有的解決方案包裝在雲端中託管的新體驗。 在 Azure 中，此解決方案可能會出現在 Azure App Service 中。

> [!CAUTION]
> 重構或重新架構解決方案或集中商務邏輯，可以快速觸發耗時的[技術尖峰](./build.md#reduce-complexity-and-delay-technical-spikes)，而不是客戶價值的來源。 這是創新的風險，特別是在假設驗證的初期。 有了設計解決方案的創意，應該會有一個不需要重構現有解決方案的 MVP 路徑。 最好是延遲重構，直到可以大規模驗證初始假設為止。

## <a name="operating-model-innovations"></a>操作模型創新

除了應用程式建立的現代化創新方法外，應用程式作業中也有一些值得注意的創新。 這些方法衍生了許多組織的移動。 最顯著的一項是卓越作業模型的[雲端中心](../../organize/cloud-center-of-excellence.md)。 當完整的個人擁有和成熟時，商務團隊可以選擇為解決方案提供自己的營運支援。

卓越雲端中心的自助作業管理模型類型，可讓您在解決方案環境內進行更緊密的控制和更快速的反復專案。 這些目標是藉由將營運控制和責任轉移給商務小組來完成。

如果您想要調整或符合現有解決方案的全域需求，此方法可能就足以驗證客戶假設。 在遷移解決方案並稍微現代化之後，商務小組可以調整它來測試各種假設。 這些通常牽涉到客戶世代，他們關心效能、全球散發和 IT 營運所妨礙運作的其他客戶需求。

## <a name="reduce-overhead-and-management"></a>減少額外負荷和管理

在解決方案內維護的越多，解決方案的反覆運算速度就越慢。 這表示您可以藉由減少作業對可用頻寬的影響來加速創新。

若要準備交付創新解決方案所需的許多反復專案，請務必預先考慮。 例如，優先列出無伺服器選項，儘早在程式中將營運負擔降至最低。 在 Azure 中，無伺服器應用程式選項可以包含[Azure App Service](https://docs.microsoft.com/azure/app-service/overview)或[容器](https://docs.microsoft.com/azure/containers)。

Azure 會同時提供無伺服器交易資料選項，以減少額外負荷。 [Azure 產品目錄](https://docs.microsoft.com/azure)提供的資料庫選項可裝載資料，而不需要完整的資料平臺。

## <a name="next-steps"></a>後續步驟

視假設和解決方案而定，本文中的原則可以協助設計符合 MVP 定義和參與使用者的應用程式。 接下來是讓[採用](./ci-cd.md)的原則，可讓您更快速且更有效率地將應用程式和資料提供給客戶。

> [!div class="nextstepaction"]
> [實現採用](./ci-cd.md)
