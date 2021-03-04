---
title: 訂用帳戶決策指南
description: 了解訂用帳戶設計策略和管理群組階層，以組織您的 Azure 資產。
author: alexbuckgit
ms.author: abuck
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: decision-guide
ms.custom: internal
ms.openlocfilehash: 668a2b7583948a812bda7c0f4f0265117095da49
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101791542"
---
# <a name="subscription-decision-guide"></a>訂用帳戶決策指南

有效的訂用帳戶設計可協助組織建立一種結構，以在雲端採用期間在 Azure 中組織及管理資產。 本指南將協助您決定何時要建立其他訂用帳戶並擴充管理群組階層，以支援您的商務優先順序。

## <a name="prerequisites"></a>必要條件

採用 Azure 時，首先請建立 Azure 訂用帳戶、將其與帳戶建立關聯，然後將虛擬機器和資料庫等資源部署至訂用帳戶。 如需這些概念的概觀，請參閱 [Azure 基本概念](../../ready/considerations/fundamental-concepts.md)。

- [建立您的初始訂用帳戶。](../../ready/azure-best-practices/initial-subscriptions.md)
- [建立額外的訂用帳戶](../../ready/azure-best-practices/scale-subscriptions.md)以調整您的 Azure 環境。
- 使用 Azure 管理群組來[組織及管理您的訂用帳戶](../../ready/azure-best-practices/organize-subscriptions.md)。

## <a name="model-your-organization"></a>為您的組織建立模型

由於每個組織都不同，所以 Azure 管理群組的設計非常具有彈性。 將雲端資產模型化以反映您的組織階層，可協助您在更高層級的階層中定義並套用原則，並依賴繼承關係，以確保這些原則會自動套用至較低階層的管理群組。 雖然訂用帳戶可在不同的管理群組之間移動，但是設計初始的管理群組階層，以反映您預期的組織需求，這樣做會很有幫助。

在完成您的訂用帳戶設計之前，亦請考慮[資源一致性](../resource-consistency/index.md)考量可能會對您的設計選擇產生何種影響。

> [!NOTE]
> Azure Enterprise 合約 (EA) 可讓您基於計費目的定義另一個組織階層。 此階層與您的管理群組階層有所區別，其著重在提供繼承模型，以輕鬆地將適當的原則和存取控制套用至您的資源。

## <a name="subscription-design-strategies"></a>訂用帳戶設計策略

請考慮使用下列訂用帳戶設計策略，來處理您的商務優先順序。

### <a name="workload-separation-strategy"></a>工作負載分隔策略

當組織新增工作負載至雲端時，訂用帳戶的不同擁有權或基本責任區分可能會在生產和非生產管理群組中產生多個訂用帳戶。 雖然這個方法提供基本工作負載分隔，但是它不會善用繼承模型，跨您的訂用帳戶子集自動套用原則。

![工作負載分隔策略](../../_images/ready/management-group-hierarchy-v2.png)

### <a name="application-category-strategy"></a>應用程式分類策略

當組織的雲端使用量增長時，通常會建立額外的訂用帳戶，以支援在商務關鍵性、合規性需求、存取控制或資料保護需求中具有基本差異的應用程式。 從初始生產和非生產的訂用帳戶中建置時，支援這些應用程式類別目錄的訂用帳戶會組織在適用的生產或非生產管理群組下。 這些訂用帳戶通常是由中央 IT 小組的作業人員擁有及管理。

![應用程式分類策略](../../_images\decision-guides\decision-guide-subscriptions-hierarchy.png)

每個組織將其應用程式分類的方式各有不同，通常會根據特定的應用程式或服務，或是應用程式原型之類的訂用帳戶來分隔訂用帳戶。 此類別通常會設計為支援可能耗用大部分訂用帳戶有限資源的工作負載，或個別的任務關鍵性工作負載，以確保其不會在這些限制之下與其他工作負載競爭。 可能證明個別訂用帳戶的部分工作負載包括：

- 任務關鍵性工作負載。
- 屬於公司內部已售出貨物成本 (COGS) 一部分的應用程式。 例如，公司製造的每個小工具都包含會傳送遙測資料的 Azure IoT 模組。 作為 COGS 的一部分，這可能需要使用專用訂用帳戶來進行會計/治理工作。
- 受限於法規需求 (例如 HIPAA 或 FedRAMP) 的應用程式。

### <a name="functional-strategy"></a>功能策略

功能策略使用管理群組階層，沿著功能線 (如財務、銷售或 IT 支援) 組織訂用帳戶和帳戶。

### <a name="business-unit-strategy"></a>營業單位策略

營業單位策略會使用管理群組階層，根據損益分類、營業單位、部門、利潤中心或類似的商務結構，將應用程式和帳戶分組。

### <a name="geographic-strategy"></a>地理策略

對於全球營運的組織，地理策略會使用管理群組階層，根據地理區域將訂用帳戶和帳戶分組。

## <a name="mix-subscription-strategies"></a>混合訂用帳戶策略

管理群組階層的深度最多可有六層。 這可讓您彈性建立階層，結合其中數種策略，以符合您的組織需求。 例如，下圖顯示結合了業務單位策略與地理策略的組織階層。

![混合的訂用帳戶策略](../../_images\decision-guides\decision-guide-subscriptions-hierarchy-mixed.png)

## <a name="related-resources"></a>相關資源

- [Azure 中的資源存取管理](../../govern/resource-consistency/resource-access-management.md)
- [大型企業中的多層治理](../../govern/guides/complex/multiple-layers-of-governance.md)
- [多個地理區域](../../migrate/azure-best-practices/multiple-regions.md)

## <a name="next-steps"></a>後續步驟

訂用帳戶設計只是雲端採用程序期間，其中一個需針對架構做出決策的核心基礎結構元件。 請瀏覽架構決策指南概觀，以了解在為其他基礎結構類型制定設計決策時，使用的其他策略。

> [!div class="nextstepaction"]
> [架構相關決策指南](../index.md)
