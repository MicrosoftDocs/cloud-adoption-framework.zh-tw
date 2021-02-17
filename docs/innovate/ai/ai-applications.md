---
title: AI 應用程式
description: 使用 Microsoft Azure 認知服務，將 AI 融入到應用程式中。
author: v-hanki
ms.author: janet
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.custom: think-tank
ms.openlocfilehash: 8b4cb93b832205307e07aa2432989cfbcada67b3
ms.sourcegitcommit: 9d76f709e39ff5180404eacd2bd98eb502e006e0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/17/2021
ms.locfileid: "100632221"
---
# <a name="ai-applications-and-agents"></a>AI 應用程式和代理程式

將 AI 融入至應用程式可能會很困難且耗時。 在最近，您需要深入瞭解機器學習和開發月份來取得資料、訓練模型，以及大規模部署。 就算如此，也不保證成功。 此路徑已填妥阻礙、陷阱和陷阱，導致小組無法實現 AI 投資的價值。

## <a name="ai-applications"></a>AI 應用程式

Microsoft Azure 認知服務會移除這些挑戰，並設計為具有生產力、符合企業需求且受信任。 它們可讓您在 AI 中建立最新的突破，而不需要建立及部署您自己的模型;相反地，您可以只使用幾個簡單的程式碼來部署 AI 模型，即使沒有大型的資料科學小組，也可以快速建立應用程式，以查看、聆聽、說出、瞭解，甚至是開始原因。

AI 應用程式的常見案例包括：

- 情感分析
- 物件偵測
- 翻譯
- 個人化
- 機器人程式自動化

當您開始使用時，下列檢查清單和資源將協助您規劃應用程式開發和部署。

- 您是否熟悉 Azure 認知服務內提供的眾多功能和服務，以及您將使用哪些功能和服務？
- 判斷您是否有想要用來定型和自訂這些模型的自訂資料。 有可自訂的認知服務。
- 有數種方式可以使用 Azure 認知服務。 探索快速入門教學課程，以瞭解如何啟動並執行 SDK 和 REST Api。 注意：認知服務 Sdk 適用于許多熱門的開發語言，包括 c #、Python、JAVA、JavaScript 和 Go。
- 判斷您是否需要將這些認知服務部署在容器中。

## <a name="ai-applications-checklist"></a>AI 應用程式檢查清單

若要開始使用，請先熟悉 Azure 認知服務中的各種類別和服務。 流覽產品頁面以深入瞭解並與示範互動，以深入瞭解可用的功能，例如視覺、語音、語言和決策。 另外還有一本電子書，可逐步解說常見的案例，以及如何使用認知服務來建立您的第一個應用程式。

- [認知服務](/azure/cognitive-services/welcome)
- [跨產品/服務頁面的互動式示範](https://azure.microsoft.com/services/cognitive-services/)
- [使用認知 api 來建立智慧型應用程式](https://azure.microsoft.com/resources/building-intelligent-apps-with-cognitive-apis/) (電子書) 

選取您要在視覺、語言、語音、決策或 web 搜尋中使用的服務。 頁面上的每個類別都會提供一組快速入門、教學課程、操作指南，無論您是要使用 REST API 或 Sdk。

<!-- docutune:casing "Intelligent Kiosk" -->

您也可以下載智慧型 Kiosk 來體驗和示範這些服務。

- [認知服務文件](/azure/cognitive-services/)
- [使用認知 api 來建立智慧型應用程式](https://azure.microsoft.com/resources/building-intelligent-apps-with-cognitive-apis/) (電子書) 
- [安裝智慧型 Kiosk 以熟悉認知服務功能](https://github.com/Microsoft/Cognitive-Samples-IntelligentKiosk)

深入瞭解 Azure 認知服務的容器支援。

- [Azure 認知服務中的容器支援](/azure/cognitive-services/cognitive-services-container-support)

查看 AI 解決方案的參考架構。

- [AI + Machine Learning](/azure/architecture/browse/#ai--machine-learning)

## <a name="ai-agents"></a>AI 代理程式

Microsoft 的 Azure AI 平臺旨在讓開發人員能夠創新並加速其專案。 針對交談式 AI，Azure 提供了 Azure Bot 服務和 Bot Framework SDK 和工具，可讓開發人員建立豐富的對話式體驗。 此外，開發人員可以使用 Azure 認知服務 (可作為 Api) 的特定網域 AI 服務，例如 Language Understanding (LUIS) 、QnA Maker 和語音服務，以新增聊天機器人的功能，以瞭解和與終端使用者交談。

對話 AI 或聊天機器人解決方案的常見案例包括：

- 聊天機器人的資訊 Q&
- 客戶服務或支援聊天機器人
- IT 服務台或 HR 聊天機器人
- 電子商務或銷售聊天機器人
- 啟用語音的裝置

> [!NOTE]
> 我們提供了建置於 Bot Framework 之上的 Power Virtual Agents，適用于想要建立主要無程式碼/低程式碼體驗聊天機器人的開發人員。 此外，開發人員也不會裝載 bot 本身，而是使用認知服務來控制自然語言或其他 AI 模型。

## <a name="ai-agents-checklist"></a>AI 代理程式檢查清單

熟悉 Azure Bot Service 及 Microsoft Bot Framework。

- Bot Framework 是開放原始碼供應專案，提供 c #、JavaScript、Python 和 JAVA) 提供的 SDK (，可協助您設計、建立及測試您的 bot。 它也提供 Bot Framework 編輯器中的免費視覺化撰寫畫布，以及 Bot Framework Emulator 中的測試控管。
- Azure Bot Service 是 Azure 中的專用服務，可讓您在 Azure 中裝載或發佈您的 Bot，並聯機到熱門通道。

- 閱讀 [Azure Bot Service 並 Bot Framework 總覽](/azure/bot-service/bot-service-overview-introduction)
- 瞭解 [bot 設計的原則](/azure/bot-service/bot-service-design-principles)
- 取得 [BOT FRAMEWORK SDK 和工具的最新版本](/azure/bot-service/what-is-new)

開始使用的其中一個最簡單方式是使用 QnA Maker，這是 Azure 認知服務的一部分，可讓您以智慧方式將常見問題檔或網站轉換成 Q&體驗（以分鐘為單位）。

- [使用 QnA Maker 快速建立具有 Q&功能的 bot](/azure/bot-service/bot-builder-tutorial-add-qna)
- 測試 [QnA Maker 服務](https://www.qnamaker.ai/)

下載並使用 Bot Framework SDK 和工具來開發 Bot。

- [使用 Bot Framework 編輯器的5分鐘快速入門](/composer/)
- [使用 Bot Framework SDK (c #、JavaScript、Python) 建立及測試 bot ](/azure/bot-service/dotnet/bot-builder-dotnet-sdk-quickstart)

瞭解如何新增認知服務，讓您的 bot 更具智慧。

- [建立 AI 應用程式的開發人員指南](https://www.oreilly.com/library/view/a-developers-guide/9781492080619/) (電子書) 
- 深入瞭解 [認知服務](/azure/cognitive-services/)

瞭解如何使用 Bot Framework 的解決方案加速器來建立您自己的虛擬助理，並選取一組常用的技能，例如行事曆、電子郵件、感興趣的點和待辦事項。

- [Bot Framework 虛擬助理解決方案](https://microsoft.github.io/botframework-solutions/index)

## <a name="next-steps"></a>下一步

探索其他 AI 解決方案類別：

- [機器學習](./machine-learning.md)
- [知識採礦](./knowledge-mining.md)
