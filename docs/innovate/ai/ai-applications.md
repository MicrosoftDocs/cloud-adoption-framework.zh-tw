---
title: AI 應用程式
description: 使用 Microsoft Azure 認知服務加入智慧型功能 AI 進入應用程式。
author: v-hanki
ms.author: janet
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: 9222f0cf5c61e0249e72675d3fe9eb46a0fe8f41
ms.sourcegitcommit: abbc6283f9f63a71333e0129ecdd8ad291517776
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/03/2020
ms.locfileid: "87523944"
---
# <a name="ai-applications-and-agents"></a>AI 應用程式和代理程式

將 AI 加入智慧型功能至應用程式可能會很棘手且耗時。 到目前為止，您都需要深入瞭解機器學習和月份的開發，以取得資料、訓練模型，以及大規模部署。 就算如此，也不保證成功。 此路徑已填入封鎖程式、問題和陷阱，導致小組無法實現其 AI 投資的價值。

## <a name="ai-applications"></a>AI 應用程式

Microsoft Azure 認知服務移除這些挑戰，並將其設計為具有生產力、企業就緒且受信任。 他們可以在 AI 中建立最新的突破，而不需要建立及部署您自己的模型。相反地，您可以只使用幾個簡單的程式碼來部署 AI 模型，如此一來，即使沒有大型資料科學小組，您也可以快速建立應用程式，以查看、聆聽、說出、瞭解，甚至是開始原因。

AI 應用程式的常見案例包括：

- 情感分析
- 物件偵測
- 翻譯
- 個人化
- 機器人流程自動化

當您開始使用時，以下的檢查清單和資源將可協助您規劃應用程式的開發和部署。

- 您是否熟悉在 Azure 認知服務中提供的許多功能和服務，以及您會使用哪一個？
- 判斷您是否有想要定型的自訂資料，並自訂這些模型。 有一些可自訂的認知服務。
- 有數種方式可以使用 Azure 認知服務。 探索快速入門教學課程，以瞭解如何針對 SDK 和 REST Api 來啟動並執行。 注意：認知服務 Sdk 適用于許多熱門的開發語言，包括 c #、Python、JAVA、JavaScript 和 Go。
- 判斷您是否需要將這些認知服務部署在容器中。

## <a name="ai-applications-checklist"></a>AI 應用程式檢查清單

若要開始使用，請先熟悉 Azure 認知服務中的各種類別和服務。 請流覽產品頁面以深入瞭解並與示範互動，以深入瞭解可用的功能，例如視覺、語音、語言和決策。 另外還有一本電子書，會逐步解說常見的案例，以及如何使用認知服務建立您的第一個應用程式。

- [認知服務](https://docs.microsoft.com/azure/cognitive-services/welcome)
- [跨產品/服務頁面的互動式示範](https://azure.microsoft.com/services/cognitive-services/)
- [使用認知 Api 建立智慧型應用程式](https://azure.microsoft.com/resources/building-intelligent-apps-with-cognitive-apis/)（電子書）

選取您想要在願景、語言、語音、決策或 web 搜尋上使用的服務。 無論您是否要使用 REST API 或 Sdk，頁面上的每個類別都會提供一組快速入門、教學課程和操作指南。

<!-- docsTest:ignore "Intelligent Kiosk" -->

您也可以下載智慧型 Kiosk 來體驗和示範這些服務。

- [認知服務文件](https://docs.microsoft.com/azure/cognitive-services/)
- [使用認知 Api 建立智慧型應用程式](https://azure.microsoft.com/resources/building-intelligent-apps-with-cognitive-apis/)（電子書）
- [安裝智慧型 Kiosk，讓您熟悉認知服務的功能](https://github.com/Microsoft/Cognitive-Samples-IntelligentKiosk)

深入瞭解 Azure 認知服務的容器支援。

- [Azure 認知服務中的容器支援](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-container-support?tabs=luis)

查看 AI 解決方案的參考架構。

- [AI + Machine Learning](https://docs.microsoft.com/azure/architecture/browse/#ai--machine-learning)

## <a name="ai-agents"></a>AI 代理程式

Microsoft 的 Azure AI 平臺旨在讓開發人員能夠創新並加速其專案。 Azure 提供的 Azure Bot Service 和 Bot Framework SDK 和工具可讓開發人員建立豐富的交談體驗，特別是針對交談式 AI。 此外，開發人員可以使用 Azure 認知服務（可作為 Api 的特定領域 AI 服務），例如 Language Understanding （LUIS）、QnA Maker 和語音服務，以新增聊天機器人的功能，以瞭解您的使用者並與之交談。

對話式 AI 或聊天機器人解決方案的常見案例包括：

- 聊天機器人的資訊 Q&
- 客戶服務或支援聊天機器人
- IT 服務台或 HR 聊天機器人
- 電子商務或銷售聊天機器人
- 啟用語音的裝置

> [!NOTE]
> 我們提供以 Bot Framework 為基礎的電源虛擬代理程式，適用于想要使用主要無程式碼/低程式碼體驗來建立聊天機器人的開發人員。 此外，開發人員也不會自行裝載 bot、使用認知服務控制自然語言或其他 AI 模型。

## <a name="ai-agents-checklist"></a>AI 代理程式檢查清單

熟悉 Azure Bot Service 和 Microsoft Bot Framework。

- Bot Framework 是一個開放原始碼的供應專案，可提供 SDK （以 c #、JavaScript、Python 和 JAVA 提供），協助您設計、建立及測試您的 bot。 它也提供 Bot Framework 編輯器中的免費視覺化製作畫布和 Bot Framework Emulator 中的測試控管。
- Azure Bot Service 是 Azure 中的專用服務，可讓您在 Azure 中裝載或發佈您的 Bot，並聯機到熱門頻道。

- [瞭解 Azure Bot Service 和 Bot Framework 總覽](https://docs.microsoft.com/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0)
- [Bot 設計準則](https://docs.microsoft.com/azure/bot-service/bot-service-design-principles?view=azure-bot-service-4.0)
- [尋找最新版本的 Bot Framework SDK 和工具](https://docs.microsoft.com/azure/bot-service/what-is-new?view=azure-bot-service-4.0)

最簡單的入門方式之一，就是使用 Azure 認知服務的 QnA Maker，它可以在幾分鐘內將常見問題檔或網站以智慧方式轉換成 Q&體驗。

- [瞭解如何使用 QnA Maker 快速建立具有 Q&功能的 bot](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-add-qna?view=azure-bot-service-4.0&tabs=csharp)
- [直接測試 QnA Maker 服務](https://www.qnamaker.ai/)

下載並使用 Bot Framework SDK 和工具進行 bot 開發

- [使用 Bot Framework 編輯器的5分鐘快速入門](https://docs.microsoft.com/composer/)
- [使用 Bot Framework SDK 建立和測試 bot （c #、JavaScript、Python）](https://docs.microsoft.com/azure/bot-service/dotnet/bot-builder-dotnet-sdk-quickstart?view=azure-bot-service-4.0)

瞭解如何新增認知服務，讓您的 bot 更聰明。

- [建立 AI 應用程式的開發人員指南](https://www.oreilly.com/library/view/a-developers-guide/9781492080619/)（電子書）
- 深入瞭解[認知服務](https://docs.microsoft.com/azure/cognitive-services/)

瞭解如何使用 Bot Framework 解決方案加速器建立自己的虛擬小幫手，並選取一組常用的技能，例如行事曆、電子郵件、關注點和待辦事項。

- [Bot Framework 虛擬助理解決方案](https://microsoft.github.io/botframework-solutions/index)

## <a name="next-steps"></a>後續步驟

探索其他 AI 解決方案類別：

- [機器學習服務](./machine-learning.md)
- [知識採礦](./knowledge-mining.md)
