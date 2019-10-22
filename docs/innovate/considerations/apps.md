---
title: 雲端創新：參與應用程式
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 雲端創新簡介-透過應用程式進行互動
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: 3db2349e3c1da7c80f3089ea187a3de72d006d1f
ms.sourcegitcommit: f3371811a36e12533ecbc3aa936e2a68e0cee25f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/21/2019
ms.locfileid: "72683308"
---
# <a name="engage-through-applications"></a>參與應用程式

如[democratizing 資料](./data.md)文章中所述，資料是新的石油。 它燃料了各種數位經濟的創新。 以這種比喻為基礎，應用程式是將該燃料放入正確手中所需的激發站和基礎結構。

在某些情況下，資料就足以推動變更並符合客戶的需求。 更常見的情況是，客戶需要的解決方案會要求應用程式塑造資料並建立經驗。 應用程式是我們與使用者互動的方式。 這些是回應客戶觸發程式所需的進程的主資料夾。 它們是客戶提供資料和接收指引的方式。 本文將概述幾個原則，以根據要驗證的假設來調整適當的應用程式方案。

![透過應用程式參與](../../_images/innovate/engage-via-apps.png)

## <a name="shared-code"></a>共用程式碼

小組可以更快速且精確地回應客戶的意見反應、市場變更，以及創新的機會，將其各自的市場導向創新。 創新應用程式的第一個原則是在[成長思維方式總覽](./learn.md#growth-mindset)中概述「共用程式碼」。 經過一段時間的創新，只能來自文化的焦點。 為了維持創新，將需要各種不同的觀點和貢獻。

為了準備好進行創新，所有應用程式的開發都應該從共用程式碼存放庫開始。 管理程式碼存放庫最常見的工具是[GitHub](https://guides.github.com/)，只要按幾下滑鼠就可以建立共用程式碼儲存機制。 或者，Azure DevOps 的[Azure Repos](/azure/devops/repos/get-started/what-is-repos?view=azure-devops)功能可以用來建立[Git](/azure/devops/repos/get-started/what-is-repos?view=azure-devops#git)或[Team Foundation](/azure/devops/repos/get-started/what-is-repos?view=azure-devops#tfvc)存放庫。

## <a name="citizen-developers"></a>公民開發人員

專業開發人員是創新的重要元件。 當假設有正確的大規模證明時，專業開發人員就必須穩定並準備解決方案以進行調整。 本文中所參考的大部分原則，都需要專業開發人員的支援。 不幸的是，目前的趨勢建議專業開發人員有更多的空缺，而開發人員則有。 此外，當需要專業開發的暸解嚴格時，創新的成本和步調也比較不理想。 公民開發人員提供一種方式來調整開發工作，並加速早期假設測試。

當您使用適用于應用程式介面的[PowerApps](https://docs.microsoft.com/powerapps/powerapps-overview) 、流程和預測的[AI](/powerapps/use-ai-builder)產生器、工作流程[Microsoft Flow](https://docs.microsoft.com/flow)或[Power BI](https://docs.microsoft.com/power-bi)的資料來驗證早期假設時，公民開發人員可能是明智的方法率.

> [!NOTE]
> 當您利用公民開發人員測試假設時，建議專業開發人員提供支援、審查和指引。 假設在大規模驗證之後，將應用程式轉換成更健全的程式設計模型的流程，將會加速創新。 與早期程式定義中的專業開發人員合作，稍後可能會產生更清楚的轉換。

## <a name="intelligent-experiences"></a>智慧型體驗

智慧型體驗結合現代化 web 應用程式的速度和規模，以及認知服務和 bot 的智慧。 單獨，每個都可能足以滿足您客戶的需求。 結合可以透過數位體驗達成的需求，也會受到擴充，但仍可包含開發投資。

### <a name="modern-web-apps"></a>新式 web 應用程式

當需要應用程式或經驗以滿足客戶需求時，現代化 web 應用程式可能是達到該需求最快的方式。 新式 web 體驗可以快速地與內部或外部客戶互動，並允許在解決方案上快速反覆運算。

### <a name="infusing-intelligence"></a>加入智慧型功能情報

機器學習服務和人工智慧越來越適用于開發人員。 具有預測性功能的通用 Api 可廣泛使用，讓開發人員能夠透過擴充的資料和預測存取，更符合客戶的需求。

將智慧新增至解決方案，可以啟用語音轉換文字、文字翻譯、電腦視覺，甚至是視覺效果搜尋。 透過這些擴充的功能，開發人員可以更輕鬆地建立運用智慧的解決方案，以建立互動式和現代化的體驗。

### <a name="bots"></a>Bot

Bot 提供的體驗比較不像使用電腦，比較像是與人溝通，或至少是與智慧型機器人溝通。 在不再需要直接人為介入的自動化系統上，Bot 可用於輪替簡單、重複性工作，例如預訂晚餐或蒐集設定檔資訊。 使用者可使用文字、互動式卡片和語音來與 Bot 交談。 Bot 互動可以是快速的問與答，也可以是以智慧方式提供服務存取權的複雜對話。

Bot 很類似現代化 Web 應用程式，在網際網路上運作，並使用 API 來傳送和接收訊息。 視 Bot 的種類而定，Bot 的功能差異很大。 現代化 Bot 軟體依賴一些技術和工具，可在各種平台上提供日益複雜的體驗。 不過，簡單的 Bot 可能只會收到訊息，並以極少的相關程式碼來回應使用者。

Bot 可執行其他類型的軟體可以執行的作業：讀取和寫入檔案、使用資料庫和 API，以及進行一般計算工作。 Bot 的特點就是其使用通常保留給人與人通訊的機制。

## <a name="cloud-native-solutions"></a>雲端原生解決方案

從頭開始建立雲端原生應用程式。 雲端原生應用程式已針對雲端規模和效能進行優化。 雲端原生應用程式通常是使用微服務、無伺服器、以事件為基礎或以容器為基礎的方法來建立。 最常見的雲端原生解決方案會運用微服務架構、受控服務和持續傳遞的組合，以達成可靠性並加快上市時間。

雲端原生解決方案可讓集中式開發小組維持商務邏輯的控制，而不需要整合單一集中式解決方案。 這種類型的解決方案也會建立錨點，以促進公民開發人員和現代化體驗之間的一致性。 最後，雲端原生解決方案藉由釋放公民和專業開發人員，以安全且最少的封鎖程式來創新，以提供創新的加速器。

## <a name="innovate-through-existing-solutions"></a>透過現有的解決方案創新

許多客戶假設最適合透過現有解決方案的現代化版本來傳遞。 當目前的商務邏輯符合客戶的需求（或確實關閉）時，您可以在現代化解決方案之上建立，以加速創新。

大部分的現代化形式，包括稍微重構應用程式，都包含在雲端採用架構內的[遷移方法](../../migrate/index.md)中。 該方法會透過將[數位資產](../../digital-estate/index.md)遷移至雲端的流程，引導雲端採用小組。 [Azure 遷移指南](../../migrate/azure-migration-guide/index.md)為相同的方法提供了一種簡單的方法，適用于少量的工作負載或甚至是單一應用程式。

一旦遷移並現代化，就有多種方式可以利用解決方案來建立新的創新解決方案，以滿足客戶的需求。 比方說，[公民開發人員](#citizen-developers)可以測試假設或專業開發人員，以建立[智慧型體驗](#intelligent-experiences)或[雲端原生解決方案](#cloud-native-solutions)。

### <a name="extend-an-existing-solution"></a>擴充現有的解決方案

擴充解決方案是一種常見的現代化形式。 當下列條件成立時，此方法可能是最快速的創新途徑：

- 現有的商務邏輯應該符合現有客戶的需求（或接近會議）。
- 改善的體驗會更符合特定客戶世代的需求。
- MVP 解決方案所需的商務邏輯，通常是透過多[層](/azure/architecture/guide/architecture-styles/n-tier)式 web 服務、API 或[微服務](/azure/architecture/guide/architecture-styles/microservices)設計來進行。 此方法包含將現有的解決方案包裝在雲端中託管的新體驗。 在 Azure 中，此解決方案可能會在 Azure App 服務中運作。

### <a name="rebuild-an-existing-solution"></a>重建現有的解決方案

如果無法輕鬆擴充應用程式，可能需要重構解決方案。 在此方法中，工作負載會遷移至雲端。 遷移之後，會將應用程式的部分修改或複製為 web 服務或[微服務](/azure/architecture/guide/architecture-styles/microservices)，並以平行方式部署到現有的解決方案。 以平行服務為基礎的解決方案可視為擴充解決方案。 此解決方案只會將現有的解決方案包裝在雲端中託管的新體驗。 在 Azure 中，此解決方案可能會在 Azure App 服務中運作。

> [!CAUTION]
> 重構或重新架構解決方案或集中商務邏輯可能很快就會成為耗時的[技術尖峰](./build.md#reduce-complexity-and-delay-technical-spikes)，而不是客戶價值的來源。 這是創新的風險，特別是在假設驗證的初期。 有了設計解決方案的創意，應該會有一個不需要重構現有解決方案的 MVP 路徑。 最好是延遲重構，直到可以大規模驗證初始假設為止。

## <a name="operating-model-innovations"></a>操作模型創新

除了建立應用程式的現代化創新方法外，還有關于應用程式作業的創新。 這些方法衍生了許多組織的移動。 其中一個最重要的動作是移至[卓越作業模型的雲端中心](../../organize/cloud-center-of-excellence.md)。 當完整的個人擁有和成熟時，商務團隊可以選擇為解決方案提供自己的營運支援。

自助服務營運管理模型的類型（位於卓越雲端中心）可讓您在解決方案環境內進行更緊密的控制和更快速的反復專案。 這是藉由將操作控制和責任轉移給商務小組來完成。

當目標是要調整或符合現有解決方案的全球需求時，這個方法可能就足以驗證客戶假設。 一旦遷移並稍微現代化之後，商務團隊就能夠調整解決方案，以測試與客戶世代相關的各種假設，這些是關於效能、全域散發或其他客戶需要妨礙運作業務.

## <a name="reduce-overhead-and-management"></a>減少額外負荷和管理

在解決方案內維護的越多，解決方案的反覆運算速度就越慢。 藉由減少影響作業對於可用的速度來加速創新。

若要準備交付創新解決方案所需的許多反復專案，請務必預先考慮。 藉由優先列出無伺服器選項，儘早在程式中將營運負擔降至最低。

在 Azure 中，無伺服器應用程式選項可以包含[Azure App Service](https://docs.microsoft.com/azure/app-service/overview)或[容器](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rearchitect-container-sql)。

Azure 會同時提供無伺服器交易資料選項，以減少額外負荷。 [[資料庫產品] 清單](https://docs.microsoft.com/azure/#pivot=products&panel=databases)提供裝載資料的選項，而不需要完整的資料平臺。

## <a name="next-steps"></a>後續步驟

視假設和解決方案而定，本文中的原則可以協助設計符合 MVP 定義和參與使用者的應用程式。 下一步是讓[採用](./ci-cd.md)的原則，這將討論如何快速地將應用程式和資料帶入客戶手中。

> [!div class="nextstepaction"]
> [加強採用](./ci-cd.md)
