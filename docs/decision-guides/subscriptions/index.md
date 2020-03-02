---
title: 訂用帳戶決策指南
description: 將訂用帳戶設計模式和管理群組，視為在 Azure 移轉期間組織資產的核心服務。
author: alexbuckgit
ms.author: abuck
ms.date: 10/17/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: decision-guide
ms.custom: governance
ms.openlocfilehash: f00377f4da5a3c95114571af36e4a759a26c63f3
ms.sourcegitcommit: af45c1c027d7246d1a6e4ec248406fb9a8752fb5
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2020
ms.locfileid: "77707946"
---
# <a name="subscription-decision-guide"></a>訂用帳戶決策指南

有效的訂用帳戶設計可協助組織建立一種結構，以在雲端採用期間組織 Azure 中的資產。

Azure 中的每個資源 (例如虛擬機器或資料庫) 均與訂用帳戶相關聯。 採用 Azure 首先建立 Azure 訂用帳戶、將它與帳戶建立關聯，然後將資源部署至訂用帳戶。 如需這些概念的概觀，請參閱 [Azure 基本概念](../../ready/considerations/fundamental-concepts.md)。

當 Azure 中的數位資產增長時，您可能需要建立額外的訂用帳戶，以符合您的需求。 Azure 可讓您定義管理群組的階層來組織您的訂用帳戶，並輕鬆地將正確的原則套用至正確的資源。 如需詳細資訊，請參閱[使用多個 Azure 訂用帳戶進行調整](../../ready/azure-best-practices/scaling-subscriptions.md)。

使用管理群組來區隔不同工作負載的一些基本範例包括：

- **生產與非生產工作負載的比較：** 有些企業會建立管理群組以區隔其生產環境和非生產環境訂用帳戶。 管理群組可讓這些客戶更容易管理角色和原則。 例如，非生產環境訂用帳戶可允許開發人員擁有**參與者**存取權，但在生產環境中，他們只有**讀者**存取權。
- **內部服務與外部服務的比較：** 就像生產環境工作負載與非生產環境工作負載那樣，對於內部服務與面相客戶的外部服務，企業通常會有不同的需求、原則和角色。

本決策指南可協助您考慮不同的方法來組織您的管理群組階層。

## <a name="subscription-design-patterns"></a>訂用帳戶設計模式

由於每個組織都不同，所以 Azure 管理群組的設計非常具有彈性。 將雲端資產模型化以反映您的組織階層，可協助您在更高層級的階層中定義並套用原則，並依賴繼承關係，以確保這些原則會自動套用至較低階層的管理群組。 雖然訂用帳戶可在不同的管理群組之間移動，但是設計初始的管理群組階層，以反映您預期的組織需求，這樣做會很有幫助。

在完成您的訂用帳戶設計之前，亦請考慮[資源一致性](../resource-consistency/index.md)考量可能會對您的設計選擇產生何種影響。

> [!NOTE]
> Azure Enterprise 合約 (EA) 可讓您基於計費目的定義另一個組織階層。 此階層與您的管理群組階層有所區別，其著重在提供繼承模型，以輕鬆地將適當的原則和存取控制套用至您的資源。

下列訂用帳戶模式反映初步增加了訂用帳戶設計的複雜性，接著是數個更進階的階層，可能完美地與您的組織匹配：

### <a name="single-subscription"></a>單一訂用帳戶

如果組織需要部署少數裝載於雲端的資產，則針對每個帳戶使用單一訂用帳戶就已足夠。 這是您在開始雲端採用程序時將實作的第一個訂用帳戶模式，讓小規模的實驗性或概念證明部署能夠探索雲端的功能。

### <a name="production-and-nonproduction-pattern"></a>生產和非生產模式

一旦您準備好將工作負載部署至生產環境，您應該新增額外的訂用帳戶。 這可協助您將生產環境資料和其他資產保留在在開發/測試環境之外。 您也可以輕鬆地跨兩個訂用帳戶中的資源套用兩組不同的原則。

![生產和非生產訂用帳戶模式](../../_images/ready/basic-subscription-model.png)

### <a name="workload-separation-pattern"></a>工作負載分隔模式

當組織新增工作負載至雲端時，訂用帳戶的不同擁有權或基本責任區分可能會在生產和非生產管理群組中產生多個訂用帳戶。 雖然這個方法提供基本工作負載分隔，但是它不會善用繼承模型，跨您的訂用帳戶子集自動套用原則。

![工作負載分隔模式](../../_images/ready/management-group-hierarchy.png)

### <a name="application-category-pattern"></a>應用程式分類模式

當組織的雲端使用量增長時，通常會建立額外的訂用帳戶，以支援在商務關鍵性、合規性需求、存取控制或資料保護需求中具有基本差異的應用程式。 從生產和非生產的訂用帳戶模式中建置時，支援這些應用程式類別目錄的訂用帳戶會組織在適用的生產或非生產管理群組下。 這些訂用帳戶通常是由中央 IT 操作人員擁有及管理。

![應用程式分類模式](../../_images/infra-subscriptions/application.png)

每個組織將其應用程式分類的方式各有不同，通常會根據特定的應用程式或服務，或是應用程式原型之類的訂用帳戶來分隔訂用帳戶。 此類別通常會設計為支援可能耗用大部分訂用帳戶有限資源的工作負載，或個別的任務關鍵性工作負載，以確保它們不會在這些限制之下與其他工作負載競爭。 可能會證明此模式下之個別訂用帳戶的部分工作負載包括：

- 任務關鍵性工作負載。
- 屬於貴公司內「已售出貨物成本」(COGS) 一部分的應用程式。 範例：X 公司小工具的每個執行個體都包含會傳送遙測資料的 Azure IoT 模組。 在 COGS 中，這可能需要使用專用訂用帳戶來進行會計/治理工作。
- 受限於法規需求 (例如 HIPAA 或 FedRAMP) 的應用程式。

### <a name="functional-pattern"></a>功能模式

功能模式使用管理群組階層沿著功能線 (如財務、銷售或 IT 支援) 組織訂用帳戶和帳戶。

### <a name="business-unit-pattern"></a>營業單位模式

營業單位模式會使用管理群組階層，根據損益分類、營業單位、部門、利潤中心或類似的商務結構，將應用程式和帳戶分組。

### <a name="geographic-pattern"></a>地理模式

對於全球營運的組織，地理模式會使用管理群組階層，根據地理區域將訂用帳戶和帳戶分組。

## <a name="mixed-patterns"></a>混合模式

管理群組階層的深度最多可有六層。 這可讓您彈性建立階層，結合其中數種模式，以符合您的組織需求。 例如，下圖顯示的組織階層結合了營業單位模式與地理模式。

![混合的訂用帳戶模式](../../_images/infra-subscriptions/mixed.png)

## <a name="related-resources"></a>相關資源

- [Azure 中的資源存取管理](../../govern/resource-consistency/resource-access-management.md)
- [大型企業中的多層治理](../../govern/guides/complex/multiple-layers-of-governance.md)
- [多個地理區域](../regions/index.md)

## <a name="next-steps"></a>後續步驟

訂用帳戶設計只是雲端採用程序期間，其中一個需針對架構做出決策的核心基礎結構元件。 請瀏覽[決策指南概觀](../index.md)，以了解在為其他類型的基礎結構制定設計決策時使用的替代模式或模型。

> [!div class="nextstepaction"]
> [架構相關決策指南](../index.md)
