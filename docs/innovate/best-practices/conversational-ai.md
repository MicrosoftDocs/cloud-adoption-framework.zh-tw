---
title: 什麼是 AI 代理程式？
description: 深入瞭解 AI 代理程式和對話式介面。 規劃、建立、測試、發佈連接和評估 bot。
author: v-hanki
ms.author: janet
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: c0c79ab45483b38268564f111f30147e68825582
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86451772"
---
<!-- cSpell:ignore Twilio -->

# <a name="what-are-ai-agents"></a>什麼是 AI 代理程式？

使用者透過交談介面來吸引更多的人，這可能會帶來更自然的體驗，讓他能以自然語言表達其需求，並快速完成工作。 對於許多公司而言，交談式 AI 應用程式會成為競爭優勢。 許多組織會策略性地在其客戶花費時間的相同訊息平臺內提供 bot。

世界各地的組織會使用交談式 AI 來轉換其企業，這可以提升其客戶及其員工的效率與自然互動。 以下是一些常見的使用案例：

- 客戶支援
- 企業助理
- 撥打電話中心優化
- 汽車中的語音助理

## <a name="building-a-bot"></a>建立 Bot

Azure Bot 服務和 Bot Framework 可提供一組整合式工具與服務，可加快此程序。 選擇您慣用的開發環境或命令列工具來建立 Bot。 Sdk 有適用于 c #、JavaScript、TypeScript 和 Python （適用于 JAVA 的 SDK 正在開發）。 我們在 Bot 各種開發階段皆提供各項工具，協助您設計及建置 Bot。

![適用于 bot 開發各種階段的工具](../../_images/ai-bot-dev-tools.png)

<!-- docsTest:ignore "natural language understanding" -->

### <a name="plan"></a>計畫

如同任何類型的軟體，務必徹底了解目標、程序及使用者需求，才能建立成功的 Bot。 在撰寫程式碼之前，請先參閱 bot[設計指導方針](https://docs.microsoft.com/azure/bot-service/bot-service-design-principles?view=azure-bot-service-4.0)以取得最佳作法，並找出您的 bot 需求。 您可以建立簡單的 bot，或包含更複雜的功能，例如語音、自然語言理解和問題解答。

在計畫階段中設計您的 bot 時，請考慮：

- 定義 bot 角色：
  - 您的 bot 應該看起來像什麼？
    - 它的命名方式為何？
    - 您的 bot 的個人化是什麼？ 他們是否有性別？
    - 您的 bot 應該如何處理艱難的情況和問題？
- 設計對話流程：
  - 您可以預期使用案例的交談類型為何？
- 定義評估計畫：
  - 您要如何測量成功？
  - 您想要使用哪些度量來改善您的服務？

若要深入瞭解如何設計 bot，請參閱[bot 設計的原則](https://docs.microsoft.com/azure/bot-service/bot-service-design-principles?view=azure-bot-service-4.0)。

### <a name="build"></a>Build

Bot 是一項 Web 服務，可實作對話式介面並透過 Bot Framework Service 進行通訊，以傳送和接收訊息和事件。 Bot Framework Service 是 Azure Bot Service 和 Bot Framework 的其中一個元件。 您可以使用任意多個環境和語言建立 Bot。 您可以在[Azure 入口網站](https://docs.microsoft.com/azure/bot-service/bot-service-quickstart?view=azure-bot-service-4.0)中啟動您的 bot 開發，或使用[c #、JavaScript 或 Python](https://docs.microsoft.com/azure/bot-service/dotnet/bot-builder-dotnet-sdk-quickstart?view=azure-bot-service-4.0)範本進行本機開發。 您也可以存取各種[範例](https://github.com/microsoft/botbuilder-samples)，這些範例展現許多可透過 SDK 取得的功能。 這些很適合尋求更多元用途起點的開發人員。

作為 Azure Bot 服務和 Bot Framework 的一部分，我們提供您可用來擴充 Bot 功能的其他元件：

| 功能 | 描述 | 連結 |
| --- | --- | --- |
| 新增自然語言處理 | 讓您的 Bot 能夠理解自然語言、理解拼字錯誤、使用語音，以及辨識使用者的意圖 | 如何使用 [LUIS](https://docs.microsoft.com/azure/bot-service/bot-builder-howto-v4-luis?view=azure-bot-service-4.0) |
| 回答問題 | 新增知識庫，以更自然的對話方式回答使用者的問題 | 如何使用 [QnA Maker](https://docs.microsoft.com/azure/bot-service/bot-builder-howto-qna?view=azure-bot-service-4.0) |
| 管理多個模型 | 如果使用多個模型（例如 LUIS 和 QnA Maker），請以智慧方式判斷在 bot 交談期間使用哪一個 | [分派](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-dispatch?view=azure-bot-service-4.0)工具 |
| 新增卡片和按鈕 | 利用文字以外的媒體 (例如圖形、功能表和卡片) 來強化使用者體驗 | 如何[新增卡片](https://docs.microsoft.com/azure/bot-service/bot-builder-howto-add-media-attachments?view=azure-bot-service-4.0) |

> [!NOTE]
> 上表並非完整清單。 請參閱[Azure.com](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)上的完整檔

### <a name="test"></a>測試

Bot 是複雜的應用程式，其中有許多不同的元件一起運作。 就像任何其他複雜的應用程式一樣，這可能會導致一些有趣的錯誤，或讓您的 bot 行為與預期不同。 在發佈之前，請先測試 Bot。 我們提供數種方式來測試 bot，再將其發行以供使用：

- 使用[模擬器](https://docs.microsoft.com/azure/bot-service/bot-service-debug-emulator?view=azure-bot-service-4.0)在本機測試 Bot。 Bot Framework Emulator 是獨立的應用程式，不僅提供聊天介面，也會偵測和訊問工具，以協助您瞭解 Bot 的運作方式和原因。 模擬器可以隨著開發中的 Bot 應用程式在本機執行。
- 在 [Web](https://docs.microsoft.com/azure/bot-service/bot-service-manage-test-webchat?view=azure-bot-service-4.0) 上測試您的 Bot。 透過 Azure 入口網站進行設定後，Bot 也可透過網路聊天介面觸達。 網路聊天介面是將 bot 存取權授與給測試人員的絕佳方式，以及無法直接存取執行中程式碼的其他人。
- 使用 Bot Framework SDK 的7月更新，對 bot 進行[單元測試](https://docs.microsoft.com/azure/bot-service/unit-test-bots)。

### <a name="publish"></a>發佈

當您準備好讓您的 bot 可在 web 上使用時，請將[它發佈至 Azure](https://docs.microsoft.com/azure/bot-service/bot-builder-howto-deploy-azure?view=azure-bot-service-4.0)或您自己的 web 服務或資料中心。 公用網際網路上的位址是讓 bot 在您的網站上或在聊天頻道內進入現實的第一個步驟。

### <a name="connect"></a>連線

將您的 bot 連接到 Facebook、messenger、Kik、Skype、時差、Microsoft 小組、Telegram、text/SMS、twilio、Cortana 及 Skype 等通道。 Bot Framework 會執行從所有這些不同平臺傳送和接收訊息所需的大部分工作。 您的 bot 應用程式會接收統一、正規化的訊息資料流程，而不論其連線的通道數目和類型為何。 如需新增通道的詳細資訊，請參閱[通道](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels?view=azure-bot-service-4.0)主題。

### <a name="evaluate"></a>評估

使用在 Azure 入口網站收集的資料，就有機會改善 Bot 功能和效能。 您可以取得服務層級和流量、延遲與整合等檢測資料。 Analytics 提供使用者、訊息和通道資料的相關交談層級報告。 如需詳細資訊，請參閱[如何收集分析](https://docs.microsoft.com/azure/bot-service/bot-service-manage-analytics?view=azure-bot-service-4.0)。

<!-- docsTest:ignore "John Doe" "Jane Doe" -->

### <a name="patterns-for-common-use-cases"></a>常見使用案例的模式

有一些常見的模式可用來執行對話式 AI 應用程式：

- 知識庫：您可以設計知識 bot 來提供幾乎任何主題的相關資訊。 例如，某個知識 bot 可能會回答有關事件的問題，例如「此會議中有哪些 bot 事件？」、「何時是下一個 reggae 節目？」 另一個則可能回答與 IT 相關的問題，例如「如何更新我的作業系統？」。 另一種可能會回答有關連絡人的問題，例如「誰是 John Doe？」 或是「Jane Doe 的電子郵件地址為何？」

知識 bot 的設計項目可在[設計知識 bot](https://docs.microsoft.com/azure/bot-service/bot-service-design-pattern-knowledge-base?view=azure-bot-service-4.0)中取得

- 遞交給人類：不論 bot 擁有多少 AI，有時可能需要將交談交給人類的人來進行。 在這類情況下，Bot 應辨識何時需要進行遞交，並且為使用者提供順暢的轉換。

將[交談從 Bot 轉換成人力](https://docs.microsoft.com/azure/bot-service/bot-service-design-pattern-handoff-human?view=azure-bot-service-4.0)時，可以使用要退出的模式

- 在應用程式中內嵌 bot：雖然 bot 最常存在於應用程式之外，它們也可以與應用程式整合。 例如，您可以將[知識 bot](https://docs.microsoft.com/azure/bot-service/bot-service-design-pattern-knowledge-base?view=azure-bot-service-4.0)內嵌在應用程式中，以協助使用者尋找資訊，或者您可以將 bot 內嵌在技術支援應用程式中，以作為傳入使用者要求的第一個回應。 Bot 可以獨立解決簡單的問題，並將較為複雜的問題[遞交](https://docs.microsoft.com/azure/bot-service/bot-service-design-pattern-handoff-human?view=azure-bot-service-4.0)給人類專員。

在應用程式中[內嵌](https://docs.microsoft.com/azure/bot-service/bot-service-design-pattern-embed-app?view=azure-bot-service-4.0)bot 時，可以使用在應用程式中整合 bot 的方式。

- 將 bot 內嵌在網站中：如同在應用程式中嵌入 bot，bot 也可以內嵌在網站內，以啟用跨通道的多種通訊模式。

將[Bot 內嵌](https://docs.microsoft.com/azure/bot-service/bot-service-design-pattern-embed-web-site?view=azure-bot-service-4.0)在網站中可取得整合您的 bot 的方式。

## <a name="next-steps"></a>後續步驟

- 複習機器學習服務白皮書和關於[Azure Bot Service](https://azure.microsoft.com/resources/whitepapers/search/?service=bot-service)的電子書
- 審查[AI + Machine Learning 架構](https://docs.microsoft.com/azure/architecture/browse/)
- [使用認知 Api 建立智慧型應用程式（電子書）](https://azure.microsoft.com/resources/building-intelligent-apps-with-cognitive-apis/)
- [常見問題聊天機器人架構](https://azure.microsoft.com/resources/faq-chatbot-architecture/)
