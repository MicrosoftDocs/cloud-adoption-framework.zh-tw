---
title: 對話式 AI
description: 針對交談式 AI，Azure 提供了 Azure Bot 服務和 Bot Framework SDK 和工具，可讓開發人員建立豐富的對話體驗。
author: v-hanki
ms.author: janet
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: b73d7243ec3db4b6f36066674b140adcb20b9219
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86451791"
---
# <a name="conversational-ai"></a>對話式 AI

Microsoft 的 Azure AI 平臺旨在讓開發人員能夠創新並加速其專案。 Azure 提供的 Azure Bot Service 和 Bot Framework SDK 和工具可讓開發人員建立豐富的交談體驗，特別是針對交談式 AI。 此外，開發人員可以使用 Azure 認知服務（可作為 Api 的特定領域 AI 服務），例如 Language Understanding （LUIS）、QnA Maker 和語音服務，以新增聊天機器人的功能，以瞭解您的使用者並與之交談。

對話式 AI 或聊天機器人解決方案的常見案例包括：

- 聊天機器人的資訊 Q&
- 客戶服務或支援聊天機器人
- IT 服務台或 HR 聊天機器人
- 電子商務或銷售聊天機器人
- 啟用語音的裝置

> [!NOTE]
> Microsoft 提供以 Bot Framework 為基礎的電源虛擬代理程式，適用于想要使用主要無程式碼或低程式碼體驗來建立聊天機器人的開發人員。 此外，開發人員也不會裝載 bot 本身，也不會使用認知服務控制自然語言或其他 AI 模型。

## <a name="checklist"></a>檢查清單

熟悉 Azure Bot Service 和 Microsoft Bot Framework。

- Bot Framework 是開放原始碼供應專案，提供 SDK （可在 c #、JavaScript、Python 和 JAVA 中使用），協助您設計、建立及測試您的 bot。 它也提供 Bot Framework 編輯器中的免費視覺化製作畫布和 Bot Framework Emulator 中的測試控管。
- Azure Bot Service 是 Azure 中的專用服務，可讓您在 Azure 中裝載或發佈您的 Bot，並聯機到熱門頻道。

- [瞭解 Azure Bot Service 和 Bot Framework 總覽](https://docs.microsoft.com/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0)
- [Bot 設計準則](https://docs.microsoft.com/azure/bot-service/bot-service-design-principles?view=azure-bot-service-4.0)
- [尋找最新版本的 Bot Framework SDK 和工具](https://docs.microsoft.com/azure/bot-service/what-is-new?view=azure-bot-service-4.0)

最簡單的入門方式之一，就是使用 Azure 認知服務的 QnA Maker，它可以在幾分鐘內將常見問題檔或網站以智慧方式轉換成 Q&體驗。

- [瞭解如何使用 QnA Maker 快速建立具有 Q&功能的 bot](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-add-qna?view=azure-bot-service-4.0&tabs=csharp)
- [直接測試 QnA Maker 服務](https://www.qnamaker.ai/)

下載並使用 Bot Framework SDK 和工具進行 bot 開發

- [適用于 Bot Framework 編輯器的5分鐘快速入門](https://docs.microsoft.com/composer/)
- [使用 Bot Framework SDK 建立和測試 bot （c #、JavaScript、Python）](https://docs.microsoft.com/azure/bot-service/dotnet/bot-builder-dotnet-sdk-quickstart?view=azure-bot-service-4.0)

瞭解如何新增認知服務，讓您的 bot 更聰明。

- [建立 AI 應用程式的開發人員指南](https://www.oreilly.com/library/view/a-developers-guide/9781492080619/)（電子書）
- [深入瞭解認知服務](https://docs.microsoft.com/azure/cognitive-services/)

瞭解如何使用 Bot Framework 解決方案加速器建立自己的虛擬小幫手，並選取一組常用的技能，例如行事曆、電子郵件、關注點和待辦事項。

- [Bot Framework 虛擬助理解決方案](https://microsoft.github.io/botframework-solutions/index)

## <a name="next-steps"></a>後續步驟

[知識採礦](./knowledge-mining.md)
