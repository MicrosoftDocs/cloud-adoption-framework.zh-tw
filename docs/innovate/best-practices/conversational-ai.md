---
title: 什麼是 AI 代理程式？
description: 深入瞭解 AI 代理程式和對話式介面。 規劃、建立、測試、發佈、連接及評估 bot。
author: v-hanki
ms.author: janet
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: e6eb6532fdc79b93e0f1bba67a7b3e6b91f5fbf4
ms.sourcegitcommit: d31a9043d1ae9283ed126bf118ca26d1d18d6948
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/10/2020
ms.locfileid: "88040840"
---
<!-- cSpell:ignore Twilio -->

# <a name="what-are-ai-agents"></a>什麼是 AI 代理程式？

使用者透過交談介面來吸引更多的人，這可能會帶來更自然的體驗，讓他能以自然語言表達其需求，並快速完成工作。 對於許多公司而言，交談式 AI 應用程式會成為競爭優勢。 許多組織會策略性地在其客戶花費時間的相同訊息平臺內提供 bot。

世界各地的組織會使用交談式 AI 來轉換其企業，這可以提升其客戶及其員工的效率與自然互動。 以下是一些常見的使用案例：

- 客戶支援
- 企業助理
- 撥打電話中心優化
- 汽車中的語音助理

## <a name="build-a-bot"></a>建置 Bot

Azure Bot Service 和 Bot Framework 提供一組整合的工具和服務，以協助進行此程式。 選擇您慣用的開發環境或命令列工具來建立 bot。 Sdk 有適用于 c #、JavaScript、TypeScript 和 Python。 適用于 JAVA 的 SDK 正在開發中。 我們在 Bot 各種開發階段皆提供各項工具，協助您設計及建置 Bot。

![此圖顯示 bot 開發各種階段的工具。](../../_images/ai-bot-dev-tools.png)

<!-- docsTest:ignore "natural language understanding" -->

### <a name="plan"></a>計畫

徹底瞭解目標、程式和使用者需求對於建立成功 bot 的程式很重要。 在您撰寫程式碼之前，請先參閱 bot[設計指導方針](https://docs.microsoft.com/azure/bot-service/bot-service-design-principles?view=azure-bot-service-4.0)以取得最佳作法，並找出您的 bot 需求。 您可以建立簡單的 bot，或包含更複雜的功能，例如語音、自然語言理解和問題解答。

當您在計畫階段中設計 bot 時，請考慮下列各方面：

- 定義 bot 角色：
  - 您的 bot 應該看起來像什麼？
    - 它的命名方式為何？
    - 您的 bot 的個人化是什麼？ 是否有性別？
    - 您的 bot 應該如何處理艱難的情況和問題？
- 設計對話流程：
  - 您可以預期使用案例的交談類型為何？
- 定義評估計畫：
  - 您要如何測量成功？
  - 您想要使用哪些度量來改善您的服務？

若要深入瞭解如何設計您的 bot，請參閱[bot 設計的原則](https://docs.microsoft.com/azure/bot-service/bot-service-design-principles?view=azure-bot-service-4.0)。

### <a name="build"></a>Build

Bot 是一項 Web 服務，可實作對話式介面並透過 Bot Framework Service 進行通訊，以傳送和接收訊息和事件。 Bot Framework 服務是 Azure Bot Service 和 Bot Framework 的其中一個元件。 您可以使用任意多個環境和語言建立 Bot。 您可以在[Azure 入口網站](https://docs.microsoft.com/azure/bot-service/bot-service-quickstart?view=azure-bot-service-4.0)中啟動您的 bot 開發，或使用[c #、JavaScript 或 Python](https://docs.microsoft.com/azure/bot-service/dotnet/bot-builder-dotnet-sdk-quickstart?view=azure-bot-service-4.0)範本進行本機開發。 您也可以存取各種[範例](https://github.com/microsoft/botbuilder-samples)，這些範例展現許多可透過 SDK 取得的功能。 這些範例適用于想要功能更豐富之起始點的開發人員。

作為 Azure Bot 服務和 Bot Framework 的一部分，我們提供您可用來擴充 Bot 功能的其他元件。

| 功能 | 描述 | 連結 |
| --- | --- | --- |
| 新增自然語言處理 | 讓您的 bot 瞭解自然語言、瞭解拼寫錯誤、使用語音，以及辨識使用者的意圖。 | 如何使用 [LUIS](https://docs.microsoft.com/azure/bot-service/bot-builder-howto-v4-luis?view=azure-bot-service-4.0) |
| 回答問題 | 新增知識庫，以回答使用者以更自然的對話方式提出的問題。 | 如何使用 [QnA Maker](https://docs.microsoft.com/azure/bot-service/bot-builder-howto-qna?view=azure-bot-service-4.0) |
| 管理多個模型 | 如果您使用多個模型（例如 LUIS 和 QnA Maker），請以智慧方式判斷 bot 交談期間使用哪一個。 | [分派](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-dispatch?view=azure-bot-service-4.0)工具 |
| 新增卡片和按鈕 | 使用文字以外的媒體（例如圖形、功能表和卡片）來增強使用者體驗。 | 如何[新增卡片](https://docs.microsoft.com/azure/bot-service/bot-builder-howto-add-media-attachments?view=azure-bot-service-4.0) |

> [!NOTE]
> 此資料表不是完整的清單。 如需詳細資訊，請參閱[Azure Bot 服務檔](https://docs.microsoft.com/azure/bot-service/)。

### <a name="test"></a>測試

Bot 是一種複雜的應用程式，具有許多不同的元件可一起工作。 就像任何其他複雜的應用程式一樣，這種複雜性可能會導致一些有趣的錯誤，或讓 bot 的行為與預期不同。 在發佈您的 bot 之前，請先進行測試。 我們提供數種方式來測試 bot，再將其發行以供使用：

- 使用[模擬器](https://docs.microsoft.com/azure/bot-service/bot-service-debug-emulator?view=azure-bot-service-4.0)在本機測試 Bot。 Bot Framework Emulator 是獨立的應用程式，不僅提供聊天介面，同時也會偵測和訊問工具，協助您瞭解 Bot 的運作方式和原因。 模擬器可以隨著開發中的 Bot 應用程式在本機執行。
- 在 [Web](https://docs.microsoft.com/azure/bot-service/bot-service-manage-test-webchat?view=azure-bot-service-4.0) 上測試您的 Bot。 當您的 bot 透過 Azure 入口網站設定之後，也可以透過網路聊天介面來觸達。 網路聊天介面是將 bot 存取權授與給測試人員的絕佳方式，以及無法直接存取執行中程式碼的其他人。
- 使用 Bot Framework SDK 的7月更新，對[bot 進行單元測試](https://docs.microsoft.com/azure/bot-service/unit-test-bots)。

### <a name="publish"></a>發佈

當您準備好讓您的 bot 可在 web 上使用時，請將[它發佈至 Azure](https://docs.microsoft.com/azure/bot-service/bot-builder-howto-deploy-azure?view=azure-bot-service-4.0)或您自己的 web 服務或資料中心。 公用網際網路上的位址是讓 bot 在您的網站上或在聊天頻道內進入現實的第一個步驟。

### <a name="connect"></a>連線

將您的 bot 連接到 Facebook、Messenger、Kik、Skype、時差、Microsoft 小組、Telegram、text/SMS、twilio、Cortana 及 Skype 等通道。 Bot Framework 會執行從所有這些不同平臺傳送和接收訊息所需的大部分工作。 您的 bot 應用程式不論連線的通道數目和類型為何，都會收到統一、正規化的訊息資料流程。 如需如何新增通道的詳細資訊，請參閱[通道](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels?view=azure-bot-service-4.0)。

### <a name="evaluate"></a>評估

使用 Azure 入口網站中收集的資料，來識別改善 bot 功能和效能的機會。 您可以取得服務層級和流量、延遲與整合等檢測資料。 分析也提供使用者、訊息和通道資料的交談層級報告。 如需詳細資訊，請參閱[如何收集分析](https://docs.microsoft.com/azure/bot-service/bot-service-manage-analytics?view=azure-bot-service-4.0)。

<!-- docsTest:ignore "John Doe" "Jane Doe" -->

### <a name="patterns-for-common-use-cases"></a>常見使用案例的模式

有一些常見的模式可用來執行對話式 AI 應用程式：

- **知識庫：** 知識 bot 可以設計成提供幾乎任何主旨的相關資訊。 例如，某個知識 bot 可能會回答有關事件的問題，例如「此會議中有哪些 bot 事件？」 或「何時會顯示下一個 reggae？」 另一個 bot 可能會回答與 IT 相關的問題，例如「如何? 更新我的作業系統？」 但是另一個 bot 可能會回答有關連絡人的問題，例如「誰是 John Doe？」 「John Doe 的電子郵件地址是什麼？」等有關連絡資訊的問題。

   如需知識 bot 之設計項目的相關資訊，請參閱[設計知識 bot](https://docs.microsoft.com/azure/bot-service/bot-service-design-pattern-knowledge-base?view=azure-bot-service-4.0)。

- **交給人類：** 無論 bot 擁有多少 AI，有時可能還是需要將交談交給人類的人來進行。 在這種情況下，bot 應該辨識何時需要遞交，並為使用者提供順暢的轉換。

   如需有關要遞交的模式資訊，請參閱將[交談從 Bot 轉換成人力](https://docs.microsoft.com/azure/bot-service/bot-service-design-pattern-handoff-human?view=azure-bot-service-4.0)。

- **在應用程式中內嵌 bot：** 雖然 bot 最常存在於應用程式之外，它們也可以與應用程式整合。 例如，您可以在應用程式中內嵌[知識 bot](https://docs.microsoft.com/azure/bot-service/bot-service-design-pattern-knowledge-base?view=azure-bot-service-4.0) ，以協助使用者尋找資訊。 您也可以在服務台應用程式中內嵌 bot，以作為傳入使用者要求的第一個回應。 Bot 可以獨立解決簡單的問題，並將較為複雜的問題[遞交](https://docs.microsoft.com/azure/bot-service/bot-service-design-pattern-handoff-human?view=azure-bot-service-4.0)給人類專員。

   如需在應用程式中整合 bot 的方式資訊，請參閱[在應用程式中內嵌 bot](https://docs.microsoft.com/azure/bot-service/bot-service-design-pattern-embed-app?view=azure-bot-service-4.0)。

- **在網站中內嵌 bot：** 如同在應用程式中內嵌 bot，bot 也可以內嵌在網站內，以啟用跨通道的多個通訊模式。

   如需在網站內整合 bot 的方式資訊，請參閱[在網站中內嵌 bot](https://docs.microsoft.com/azure/bot-service/bot-service-design-pattern-embed-web-site?view=azure-bot-service-4.0)。

## <a name="next-steps"></a>後續步驟

- 回顧機器學習技術白皮書及關於[Azure Bot Service](https://azure.microsoft.com/resources/whitepapers/search/?service=bot-service)的電子書。
- 回顧[AI + Machine Learning 架構](https://docs.microsoft.com/azure/architecture/browse/)。
- [使用認知 Api 建立智慧型應用程式 (電子書) ](https://azure.microsoft.com/resources/building-intelligent-apps-with-cognitive-apis/)。
- [常見問題聊天機器人架構](https://azure.microsoft.com/resources/faq-chatbot-architecture/)。
