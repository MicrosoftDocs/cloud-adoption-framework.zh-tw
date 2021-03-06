---
title: 建立您的初始 Azure 訂用帳戶
description: 建立您的初始訂用帳戶以開始採用 Azure。
author: alexbuckgit
ms.author: abuck
ms.date: 05/20/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: internal
ms.openlocfilehash: 097f490a6dda451c2320e3ab51498b57cf66baf4
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102115793"
---
# <a name="create-your-initial-azure-subscriptions"></a>建立您的初始 Azure 訂用帳戶

藉由建立一組初始的訂用帳戶來開始採用 Azure。 根據您的初始需求，瞭解您應該開始使用哪些訂用帳戶。

## <a name="your-first-two-subscriptions"></a>前兩個訂用帳戶

首先，建立兩個訂閱：

- 建立一個 Azure 訂用帳戶，以包含您的生產工作負載。
- 使用 [Azure 開發/測試供應](https://azure.microsoft.com/pricing/dev-test/) 專案以獲得較低的定價，建立第二個訂用帳戶作為您的非生產環境。

![一種初始訂用帳戶模型，顯示標示為 * * 生產 * * 和 * * 非生產 * * 圖1的方塊旁邊的索引鍵：標示為「生產」和「非生產」的 ](../../_images/ready/initial-subscription-model.png)
 *方塊旁邊有索引鍵的初始訂用帳戶模型。*

這種方法有許多優點：

- 針對您的生產環境和非生產環境使用不同的訂用帳戶會建立界限，讓您的資源管理更簡單且更安全。
- Azure 開發/測試訂用帳戶供應專案適用于非生產工作負載。 這些供應專案提供 Azure 服務和軟體授權的折扣費率。
- 您的生產環境和非生產環境可能會有不同組的 Azure 原則。 使用不同的訂用帳戶可讓您輕鬆地在訂用帳戶層級套用每個不同的原則。
- 您可以在非生產訂用帳戶中允許特定類型的 Azure 資源，以供測試之用。 您可以在非生產訂用帳戶中啟用這些資源提供者，而不需要在生產環境中使用它們。
- 您可以使用 Azure 開發/測試訂用帳戶作為隔離的沙箱環境。 這些沙箱可讓系統管理員和開發人員快速建立和卸載整組 Azure 資源。 這種隔離也有助於資料保護和安全性考量。
- 您所定義的可接受成本閾值可能會因生產環境和非生產環境而異。

## <a name="sandbox-subscriptions"></a>沙箱訂閱

如果創新目標是雲端採用策略的一部分，請考慮建立一或多個沙箱訂閱。 您可以套用安全性原則，讓這些測試訂閱與您的生產環境和非生產環境隔離。 使用者可以在這些隔離的環境中輕鬆地試驗 Azure 功能。 使用 Azure 開發/測試供應專案來建立這些訂用帳戶。

![初始訂用帳戶模型，顯示標示為 * * 生產 * *、* * 非生產 * * 和 * * 沙箱 * * [圖 2] 的方塊旁邊的索引鍵 ](../../_images/ready/initial-subscription-model-with-sandboxes.png)
 *：具有沙箱訂閱的訂閱模型。*

## <a name="shared-services-subscription"></a>共用服務訂用帳戶

如果您打算在 **24 個月內裝載超過1000個 vm 或在雲端中計算實例**，請建立另一個 Azure 訂用帳戶來裝載共用服務。 這可讓您準備支援您的終端狀態企業架構。

![初始訂用帳戶模型，顯示標示為 * * 生產 * * 和 * * 共用服務 * * [圖 3] 的方塊旁的索引鍵 ](../../_images/ready/initial-subscription-model-with-shared-services.png)
 *：具有共用服務的訂用帳戶模型。*

## <a name="next-steps"></a>下一步

請參閱您可能想要 [建立其他 Azure](./scale-subscriptions.md) 訂用帳戶以符合需求的原因。

> [!div class="nextstepaction"]
> [建立額外的訂用帳戶以調整 Azure 環境](./scale-subscriptions.md)
