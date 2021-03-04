---
title: 組織和管理多個 Azure 訂用帳戶
description: 使用適用于 Azure 的雲端採用架構，瞭解如何建立管理群組階層，以簡化您的訂用帳戶和資源管理。
author: alexbuckgit
ms.author: abuck
ms.date: 05/20/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: internal
ms.openlocfilehash: 47ae0478c8e9a793e0d669fb4c938f621042422b
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102115402"
---
# <a name="organize-and-manage-multiple-azure-subscriptions"></a>組織和管理多個 Azure 訂用帳戶

如果您只有幾個訂用帳戶，則個別管理它們相當簡單。 不過，如果您有許多訂用帳戶，請建立管理群組階層，以協助管理您的訂用帳戶和資源。

## <a name="azure-management-groups"></a>Azure 管理群組

Azure 管理群組可協助您有效率地管理訂用帳戶的存取、原則和合規性。 每個管理群組都是一個或多個訂用帳戶的容器。

管理群組會以單一階層的方式排列。 您可以在 Azure Active Directory 中定義此階層， (Azure AD) 租使用者，以符合組織的結構和需求。 最上層稱為「根管理群組」。 您最多可以在階層中定義六個層級的管理群組。 每個訂用帳戶只能包含在一個管理群組中。

Azure 提供四個層級的管理範圍：

- 管理群組
- 訂用帳戶
- 資源群組
- 資源

階層中某一層級所套用的任何存取或原則，都會由其底下的層級繼承。 資源擁有者或訂用帳戶擁有者無法改變繼承的原則。 這項限制有助於改善治理方式。

> [!NOTE]
> 尚不支援標記繼承，但很快就會提供。

此繼承模型可讓您在階層中排列訂用帳戶，讓每個訂用帳戶遵循適當的原則和安全性控制。

![組織 Azure 資源的四個範圍層級 ](../../ready/azure-setup-guide/media/organize-resources/scope-levels.png)
 *圖1：組織 Azure 資源的四個範圍層級。*

根管理群組上的任何存取權或原則指派，都會套用至目錄中的所有資源。 請仔細考慮要在此範圍上定義的項目。 這應該僅包含您必須擁有的指派。

## <a name="create-your-management-group-hierarchy"></a>建立您的管理群組階層

當您定義管理群組階層時，請先建立根管理群組。 然後將目錄中的所有現有訂用帳戶移至根管理群組。 新的訂用帳戶一律會建立在根管理群組中。 稍後，您可以將它們移至另一個管理群組。

當您將訂用帳戶移至現有的管理群組時，它會從其上方的管理群組階層繼承原則和角色指派。 為您的 Azure 工作負載建立多個訂用帳戶之後，您可以建立額外的訂用帳戶來包含其他訂用帳戶共用的 Azure 服務。

如果您希望 Azure 環境成長，您應該立即建立生產和非生產的管理群組，並在管理群組層級套用適當的原則和存取控制。 當新的訂用帳戶新增至每個管理群組時，會繼承適當的控制項。

![管理群組階層的範例 ](../../_images/ready/management-group-hierarchy-v2.png)
 *圖2：管理群組階層的範例。*

## <a name="example-use-cases"></a>使用案例範例

使用管理群組來區隔不同工作負載的一些基本範例包括：

**生產與非生產工作負載** 的比較：使用管理群組，更輕鬆地管理生產與非生產訂用帳戶之間的不同角色和原則。 例如，開發人員可能會在非生產訂用帳戶中擁有參與者存取權，但只有在生產訂用帳戶中的讀者存取

**內部服務與外部服務：** 企業通常會有不同的需求、原則，以及內部服務與外部客戶面向服務的角色。

## <a name="related-resources"></a>相關資源

請參閱下列資源，以深入瞭解如何組織及管理您的 Azure 資源。

- [使用 Azure 管理群組來組織資源](/azure/governance/management-groups/)
- [提升存取權以管理所有 Azure 訂用帳戶和管理群組](/azure/role-based-access-control/elevate-access-global-admin)
- [將 Azure 資源移至另一個資源群組或訂用帳戶](/azure/azure-resource-manager/management/move-resource-group-and-subscription)

## <a name="next-steps"></a>下一步

部署 Azure 資源時，檢閱並遵循[建議的命名和標記慣例](./naming-and-tagging.md)。

> [!div class="nextstepaction"]
> [建議的命名和標記慣例](./naming-and-tagging.md)
