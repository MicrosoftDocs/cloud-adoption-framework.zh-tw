---
title: 建立您的初始 Azure 訂用帳戶
description: 藉由建立您的初始訂用帳戶，開始使用 Azure。
author: alexbuckgit
ms.author: abuck
ms.date: 05/20/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: ba648a2e26085b8a13b698097c54d184c27f8fff
ms.sourcegitcommit: 7d3fc1e407cd18c4fc7c4964a77885907a9b85c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "80433334"
---
# <a name="create-your-initial-azure-subscriptions"></a>建立您的初始 Azure 訂用帳戶

藉由建立一組初始的訂用帳戶來開始您的 Azure 採用。 根據您的初始需求，瞭解應開始的訂閱。

## <a name="your-first-two-subscriptions"></a>您的前兩個訂用帳戶

首先建立兩個訂用帳戶：

- 建立一個包含生產工作負載的 Azure 訂用帳戶。
- 使用[Azure 開發/測試供應](https://azure.microsoft.com/pricing/dev-test)專案來建立第二個訂用帳戶做為非生產（開發/測試）環境，以取得較低的價格。

![初始訂用帳戶模型，會在標示為「生產」和「非生產」的方塊旁顯示金鑰](../../_images/ready/initial-subscription-model.png)

這個方法有許多優點：

- 針對您的生產和非生產環境使用個別的訂用帳戶，會建立一個界限，讓您的資源管理更加簡單且更安全。
- Azure 具有用於非生產工作負載的特定開發/測試訂用帳戶供應專案。 這些供應專案會提供 Azure 服務和軟體授權的折扣費率。
- 您的生產和非生產環境可能會有不同組的 Azure 原則。 使用個別的訂用帳戶，可讓您輕鬆地在訂用帳戶層級套用每個不同的原則。
- 您可以在非生產訂用帳戶中允許特定類型的 Azure 資源，以供測試之用。 您可以在非生產訂用帳戶中啟用這些資源提供者，而不需要在生產環境中使用它們。
- 您可以使用開發/測試訂用帳戶作為隔離的沙箱環境。 這些沙箱可讓系統管理員和開發人員快速建立及卸載整個 Azure 資源集。 這種隔離也有助於資料保護和安全性考量。
- 您定義的可接受成本閾值可能會在生產環境和開發/測試訂用帳戶之間有所不同。

## <a name="sandbox-subscriptions"></a>沙箱訂閱

如果您將創新目標作為雲端採用策略的一部分，請考慮建立一或多個沙箱訂閱。 您可以套用安全性原則，讓這些測試訂閱與您的生產和非生產環境隔離。 使用者可以在這些隔離的環境中，輕鬆地試驗 Azure 功能。 使用 Azure 開發/測試供應專案來建立這些訂用帳戶。

![初始訂用帳戶模型，顯示標示為「生產」、「非生產」和「沙箱」之方塊旁的索引鍵](../../_images/ready/initial-subscription-model-with-sandboxes.png)

## <a name="shared-services-subscription"></a>共用服務訂用帳戶

如果您打算在**24 個月內裝載超過1000個 vm 或計算實例在雲端中**，請建立另一個 Azure 訂用帳戶來裝載共用服務。 這會讓您事先準備好支援您的最終狀態企業架構。

![初始訂用帳戶模型，會在標示為「生產」和「共用服務」的方塊旁顯示金鑰](../../_images/ready/initial-subscription-model-with-shared-services.png)

## <a name="next-steps"></a>後續步驟

瞭解您可能會想要[建立其他 Azure](./scale-subscriptions.md)訂用帳戶以符合您的需求的原因。

> [!div class="nextstepaction"]
> [建立額外的訂用帳戶來調整您的 Azure 環境](./scale-subscriptions.md)
