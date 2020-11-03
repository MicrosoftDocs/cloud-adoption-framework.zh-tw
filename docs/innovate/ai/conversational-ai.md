---
title: 對話式 AI
description: 針對交談式 AI，Azure 提供了 Azure Bot 服務和 Bot Framework SDK 和工具，可讓開發人員建立豐富的對話式體驗。
author: v-hanki
ms.author: janet
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: 70d8c90bdc9f013b4e08c7b7e33207b07d49b4dd
ms.sourcegitcommit: fbfd66dab002b549d3e9cbf1b7efa0099d0b7700
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/03/2020
ms.locfileid: "93283089"
---
# <a name="conversational-ai"></a>對話式 AI

Microsoft 的 Azure AI 平臺旨在讓開發人員能夠創新並加速其專案。 針對交談式 AI，Azure 提供了 Azure Bot 服務和 Bot Framework SDK 和工具，可讓開發人員建立豐富的對話式體驗。 此外，開發人員可以使用 Azure 認知服務 (可作為 Api) 的特定網域 AI 服務，例如 Language Understanding (LUIS) 、QnA Maker 和語音服務，以新增聊天機器人的功能，以瞭解和與終端使用者交談。

對話 AI 或聊天機器人解決方案的常見案例包括：

- 聊天機器人的資訊 Q&
- 客戶服務或支援聊天機器人
- IT 服務台或 HR 聊天機器人
- 電子商務或銷售聊天機器人
- 啟用語音的裝置

> [!NOTE]
> Microsoft 提供以 Bot Framework 為基礎的 Power Virtual Agents，適用于想要以主要無程式碼或低程式碼體驗來建立聊天機器人的開發人員。 此外，開發人員也不會裝載 bot 本身，也不會使用認知服務來控制自然語言或其他 AI 模型。

## <a name="checklist"></a>檢查清單

熟悉 Azure Bot Service 及 Microsoft Bot Framework。

- Bot Framework 是開放原始碼供應專案，提供 c #、JavaScript、Python 和 JAVA) 提供的 SDK (，可協助您設計、建立及測試您的 bot。 它也提供 Bot Framework 編輯器中的免費視覺化撰寫畫布，以及 Bot Framework Emulator 中的測試控管。
- Azure Bot Service 是 Azure 中的專用服務，可讓您在 Azure 中裝載或發佈您的 Bot，並聯機到熱門通道。

- 閱讀 [Azure Bot Service 並 Bot Framework 總覽](/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0)
- 瞭解 [bot 設計的原則](/azure/bot-service/bot-service-design-principles?view=azure-bot-service-4.0)
- 取得 [BOT FRAMEWORK SDK 和工具的最新版本](/azure/bot-service/what-is-new?view=azure-bot-service-4.0)

開始使用的其中一個最簡單方式是使用 QnA Maker，這是 Azure 認知服務的一部分，可讓您以智慧方式將常見問題檔或網站轉換成 Q&體驗（以分鐘為單位）。

- [使用 QnA Maker 快速建立具有 Q&功能的 bot](/azure/bot-service/bot-builder-tutorial-add-qna?tabs=csharp&view=azure-bot-service-4.0)
- 測試 [QnA Maker 服務](https://www.qnamaker.ai/)

下載並使用 Bot Framework SDK 和工具來開發 Bot。

- [Bot Framework 編輯器的5分鐘快速入門](/composer/)
- [使用 Bot Framework SDK (c #、JavaScript、Python) 建立及測試 bot ](/azure/bot-service/dotnet/bot-builder-dotnet-sdk-quickstart?view=azure-bot-service-4.0)

瞭解如何新增認知服務，讓您的 bot 更具智慧。

- [建立 AI 應用程式的開發人員指南](https://www.oreilly.com/library/view/a-developers-guide/9781492080619/) (電子書) 
- 深入瞭解 [認知服務](/azure/cognitive-services/)

瞭解如何使用 Bot Framework 的解決方案加速器來建立您自己的虛擬助理，並選取一組常用的技能，例如行事曆、電子郵件、感興趣的點和待辦事項。

- [Bot Framework 虛擬助理解決方案](https://microsoft.github.io/botframework-solutions/index)

## <a name="next-steps"></a>後續步驟

[知識採礦](./knowledge-mining.md)
