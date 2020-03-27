---
title: 組織和管理多個 Azure 訂用帳戶
description: 使用適用于 Azure 的雲端採用架構來瞭解如何建立管理群組階層，以簡化您的訂用帳戶和資源管理。
author: alexbuckgit
ms.author: abuck
ms.date: 05/20/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: e874c518537232104d9bbddbdf8e84841239e56b
ms.sourcegitcommit: ea63be7fa94a75335223bd84d065ad3ea1d54fdb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/27/2020
ms.locfileid: "80359773"
---
# <a name="organize-and-manage-multiple-azure-subscriptions"></a>組織和管理多個 Azure 訂用帳戶

如果您只有幾個訂用帳戶，獨立管理這些訂用帳戶相當簡單。 但如果您有許多訂用帳戶，請建立管理群組階層，以協助管理您的訂用帳戶和資源。

## <a name="azure-management-groups"></a>Azure 管理群組

Azure 管理群組可以有效率地管理組織訂用帳戶的存取、原則和合規性。 每個管理群組都是一個或多個訂用帳戶的容器。

管理群組會以單一階層的方式排列。 您會在 Azure Active Directory （Azure AD）租使用者中定義此階層，以符合您組織的結構和需求。 最上層稱為「根管理群組」。 您最多可以在階層中定義六個層級的管理群組。 每個訂用帳戶只能包含在一個管理群組中。

Azure 提供四個層級的管理範圍：

- 管理群組
- 訂用帳戶
- 資源群組
- 資源

階層中某一層級所套用的任何存取或原則，都會由其底下的層級繼承。 資源擁有者或訂用帳戶擁有者無法改變繼承的原則。 這項限制有助於改善治理方式。

> [!NOTE]
> 尚不支援標記繼承，但很快就會提供。

此繼承模型可讓您在階層中排列訂閱，讓每個訂用帳戶都遵循適當的原則和安全性控制項。

![用來組織 Azure 資源的四個範圍層級](../../ready/azure-setup-guide/media/organize-resources/scope-levels.png)

根管理群組上的任何存取權或原則指派，都會套用至目錄中的所有資源。 請仔細考慮要在此範圍上定義的項目。 這應該僅包含您必須擁有的指派。

## <a name="create-your-management-group-hierarchy"></a>建立您的管理群組階層

當您定義管理群組階層時，請先建立根管理群組。 然後將目錄中所有現有的訂用帳戶移到根管理群組。 新的訂用帳戶一律會建立在根管理群組中。 之後，您可以將它們移至另一個管理群組。

當您將訂用帳戶移至現有的管理群組時，它會從其上的管理群組階層繼承原則和角色指派。 建立 Azure 工作負載的多個訂用帳戶之後，您可以建立額外的訂用帳戶，以包含其他訂用帳戶共用的 Azure 服務。

如果您想要讓 Azure 環境成長，您應該立即建立生產和非生產的管理群組，並在管理群組層級套用適當的原則和存取控制。 新的訂閱將會在新增至每個管理群組時，繼承適當的控制項。

![管理群組階層的範例](../../_images/ready/management-group-hierarchy-v2.png)

## <a name="example-use-cases"></a>使用案例範例

使用管理群組來區隔不同工作負載的一些基本範例包括：

- **生產與非生產工作負載：** 使用管理群組，更輕鬆地管理生產與非生產訂用帳戶之間的不同角色和原則。 例如，非生產訂用帳戶可能會授與開發人員參與者存取權，而在生產開發人員中只有讀者存取權。
- **內部服務與外部服務的比較：** 企業通常會有不同的需求、原則和角色來提供內部服務與外部客戶面向的服務。

## <a name="related-resources"></a>相關資源

請參閱下列資源，以深入瞭解如何組織和管理您的 Azure 資源。

- [使用 Azure 管理群組來組織資源](https://docs.microsoft.com/azure/governance/management-groups)
- [提高存取權以管理所有 Azure 訂用帳戶和管理群組](https://docs.microsoft.com/azure/role-based-access-control/elevate-access-global-admin)
- [將 Azure 資源移至另一個資源群組或訂用帳戶](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)

## <a name="next-steps"></a>後續步驟

部署 Azure 資源時，檢閱並遵循[建議的命名和標記慣例](./naming-and-tagging.md)。

> [!div class="nextstepaction"]
> [建議的命名和標記慣例](./naming-and-tagging.md)
