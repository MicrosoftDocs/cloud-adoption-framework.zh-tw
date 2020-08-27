---
title: 什麼是 AI 代理程式？
description: 瞭解 AI 代理程式和對話式介面。 規劃、建立、測試、發佈、連接和評估 bot。
author: v-hanki
ms.author: janet
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: 1cdf730339c30f681fc1dc39cad85fcd9c8a909f
ms.sourcegitcommit: 07d56209d56ee199dd148dbac59671cbb57880c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88884516"
---
<!-- docsTest:casing "natural language understanding" -->
<!-- cSpell:ignore Twilio -->

# <a name="what-are-ai-agents"></a>什麼是 AI 代理程式？

使用者會透過交談式介面更多的互動，進而提供更自然的體驗，讓人們透過自然語言表達他們的需求，並快速完成工作。 對於許多公司來說，對話式 AI 應用程式逐漸成為競爭優勢。 許多組織都有策略性地讓 bot 可在其客戶花時間的相同訊息平臺內使用。

全球各地的組織都會使用交談 AI 來轉換其企業，進而提升其客戶和員工的效率與自然互動。 以下是一些常見的使用案例：

- 客戶支援
- 企業助理
- 撥置中心優化
- 汽車內的語音助理

## <a name="build-a-bot"></a>建置 Bot

Azure Bot Service 和 Bot Framework 提供一組整合的工具和服務，以協助進行此程式。 選擇您最愛的開發環境或命令列工具來建立您的 bot。 C #、JavaScript、TypeScript 和 Python 都有 Sdk。 適用于 JAVA 的 SDK 正在開發中。 我們在 Bot 各種開發階段皆提供各項工具，協助您設計及建置 Bot。

![此圖顯示 bot 開發各階段的工具。](../../_images/ai-bot-dev-tools.png)

### <a name="plan"></a>計畫

徹底瞭解目標、處理常式和使用者需求，對於建立成功 bot 的程式非常重要。 撰寫程式碼之前，請先參閱 bot [設計指導方針](/azure/bot-service/bot-service-design-principles?view=azure-bot-service-4.0) ，以瞭解最佳作法，並找出 bot 的需求。 您可以建立簡單的 bot，或包含更複雜的功能，例如語音、自然語言理解和問題答案。

當您在計畫階段中設計 bot 時，請考慮下列層面：

- 定義 bot 角色：
  - 您的 bot 應該看起來像什麼？
    - 它的命名方式為何？
    - 聊天機器人的特質為何？ 是否有性別？
    - 您的 bot 應該如何處理困難的情況和問題？
- 設計對話流程：
  - 您可以預期使用案例的交談類型為何？
- 定義評估計畫：
  - 您要如何衡量成功？
  - 您要使用哪些度量來改善您的服務？

若要深入瞭解如何設計您的 bot，請參閱 [bot 設計的原則](/azure/bot-service/bot-service-design-principles?view=azure-bot-service-4.0)。

### <a name="build"></a>Build

Bot 是一項 Web 服務，可實作對話式介面並透過 Bot Framework Service 進行通訊，以傳送和接收訊息和事件。 Bot Framework Service 是 Azure Bot Service 和 Bot Framework 的其中一個元件。 您可以使用任意多個環境和語言建立 Bot。 您可以在 [Azure 入口網站](/azure/bot-service/bot-service-quickstart?view=azure-bot-service-4.0) 中開始進行 bot 開發，或使用 [c #、JavaScript 或 Python](/azure/bot-service/dotnet/bot-builder-dotnet-sdk-quickstart?view=azure-bot-service-4.0) 範本進行本機開發。 您也可以存取各種[範例](https://github.com/microsoft/botbuilder-samples)，這些範例展現許多可透過 SDK 取得的功能。 這些範例非常適合想要有更多功能的起點的開發人員使用。

在 Azure Bot Service 和 Bot Framework 中，我們提供您可用來擴充 Bot 功能的其他元件。

| 功能 | 描述 | 連結 |
| --- | --- | --- |
| 新增自然語言處理 | 讓您的 bot 瞭解自然語言、瞭解拼寫錯誤、使用語音，以及辨識使用者的意圖。 | 如何使用 [LUIS](/azure/bot-service/bot-builder-howto-v4-luis?view=azure-bot-service-4.0) |
| 回答問題 | 加入知識庫，以回答使用者以更自然的對話方式提出的問題。 | 如何使用 [QnA Maker](/azure/bot-service/bot-builder-howto-qna?view=azure-bot-service-4.0) |
| 管理多個模型 | 如果您使用多個模型（例如 LUIS 和 QnA Maker），可在 bot 對話期間，以智慧方式判斷要使用哪一種模型。 | [分派](/azure/bot-service/bot-builder-tutorial-dispatch?view=azure-bot-service-4.0)工具 |
| 新增卡片和按鈕 | 使用文字以外的媒體（例如圖形、功能表和卡片）來增強使用者體驗。 | 如何[新增卡片](/azure/bot-service/bot-builder-howto-add-media-attachments?view=azure-bot-service-4.0) |

> [!NOTE]
> 這份表格不是完整的清單。 如需詳細資訊，請參閱 [Azure Bot Service 檔](/azure/bot-service/)。

### <a name="test"></a>測試

Bot 是一種複雜的應用程式，其中有許多不同的元件可一起運作。 如同任何其他複雜的應用程式，這種複雜性可能會導致一些有趣的錯誤，或導致您的 bot 行為與預期不同。 在您發佈 bot 之前，請先進行測試。 我們提供數種方式來測試 bot，然後再加以釋放以供使用：

- 使用[模擬器](/azure/bot-service/bot-service-debug-emulator?view=azure-bot-service-4.0)在本機測試 Bot。 Bot Framework Emulator 是一個獨立的應用程式，不僅提供交談介面，還提供偵錯工具和訊問工具，協助您瞭解 Bot 的運作方式和原因。 模擬器可以隨著開發中的 Bot 應用程式在本機執行。
- 在 [Web](/azure/bot-service/bot-service-manage-test-webchat?view=azure-bot-service-4.0) 上測試您的 Bot。 透過 Azure 入口網站設定您的 bot 之後，也可以透過網路聊天介面來觸達。 網路聊天介面是將 bot 存取權授與給測試人員的絕佳方法，以及其他無法直接存取執行中程式碼的人員。
- 使用 Bot Framework SDK 的七月更新對[bot 進行單元測試](/azure/bot-service/unit-test-bots)。

### <a name="publish"></a>發行

當您準備好讓 bot 可在 web 上使用時，請將 [其發佈至 Azure](/azure/bot-service/bot-builder-howto-deploy-azure?view=azure-bot-service-4.0) 或您自己的 web 服務或資料中心。 在公用網際網路上使用位址，是在您的網站上或在聊天頻道中讓 bot 上線的第一個步驟。

### <a name="connect"></a>連線

將您的 bot 連接到 Facebook、Messenger、Kik、Skype、時差、Microsoft 小組、Telegram、text/SMS、Twilio、Cortana 及 Skype 等通道。 Bot Framework 會執行從所有這些不同平臺傳送和接收訊息所需的大部分工作。 您的 bot 應用程式會收到統一的正規化訊息串流，無論其所連接的通道數目和類型為何。 如需如何新增通道的詳細資訊，請參閱 [通道](/azure/bot-service/bot-service-manage-channels?view=azure-bot-service-4.0)。

### <a name="evaluate"></a>評估

使用 Azure 入口網站中收集的資料，找出提高 bot 功能和效能的機會。 您可以取得服務層級和流量、延遲與整合等檢測資料。 分析也提供使用者、訊息和通道資料的交談層級報告。 如需詳細資訊，請參閱 [如何收集分析](/azure/bot-service/bot-service-manage-analytics?view=azure-bot-service-4.0)。

### <a name="patterns-for-common-use-cases"></a>常見使用案例的模式

有一些常見的模式可用於執行對話式 AI 應用程式：

- **知識庫：** 知識 bot 可以設計為提供幾乎任何主旨的相關資訊。 例如，一個知識 bot 可以回答有關事件的問題，例如「此會議有哪些 bot 事件？」。 或 "next reggae show when？" 另一個 bot 可能會回答與 IT 相關的問題，例如「我要如何更新我的作業系統？」 但另一個 bot 可能會回答「誰是 john doe？」之類的連絡人問題。 或「什麼是 jane doe 的電子郵件地址？」

   如需知識 bot 設計項目的詳細資訊，請參閱 [設計知識 bot](/azure/bot-service/bot-service-design-pattern-knowledge-base?view=azure-bot-service-4.0)。

- **交給人類：** 無論 bot 擁有多少 AI，有時可能仍需要將對話交付給人類的人。 在這種情況下，bot 應該會辨識何時需要交付，並為使用者提供順暢的轉換。

   如需有關要交付之模式的詳細資訊，請參閱將 [對話從 Bot 轉換成人類](/azure/bot-service/bot-service-design-pattern-handoff-human?view=azure-bot-service-4.0)。

- **在應用程式中內嵌 bot：** 雖然 bot 最常存在於應用程式之外，但它們也可以與應用程式整合。 例如，您可以在應用程式中內嵌 [知識 bot](/azure/bot-service/bot-service-design-pattern-knowledge-base?view=azure-bot-service-4.0) ，以協助使用者尋找資訊。 您也可以將 bot 內嵌在技術支援中心的應用程式內，作為傳入使用者要求的第一個回應者。 Bot 可以獨立解決簡單的問題，並將較為複雜的問題[遞交](/azure/bot-service/bot-service-design-pattern-handoff-human?view=azure-bot-service-4.0)給人類專員。

   如需如何在應用程式中整合 bot 的詳細資訊，請參閱 [在應用程式中內嵌 bot](/azure/bot-service/bot-service-design-pattern-embed-app?view=azure-bot-service-4.0)。

- **將 Bot 內嵌至網站：** 如同在應用程式中內嵌 bot，bot 也可以內嵌在網站內，以啟用跨通道的多種通訊模式。

   如需如何在網站內整合 bot 的詳細資訊，請參閱將 [Bot 內嵌在網站中](/azure/bot-service/bot-service-design-pattern-embed-web-site?view=azure-bot-service-4.0)。

## <a name="next-steps"></a>後續步驟

- 複習機器學習技術白皮書和有關 [Azure Bot Service](https://azure.microsoft.com/resources/whitepapers/search/?service=bot-service)的電子書。
- 複習 [AI + Machine Learning 架構](/azure/architecture/browse/)。
- [使用認知 api 來建立智慧型應用程式 (電子書) ](https://azure.microsoft.com/resources/building-intelligent-apps-with-cognitive-apis/)。
- [常見問題聊天機器人架構](https://azure.microsoft.com/resources/faq-chatbot-architecture/)。
