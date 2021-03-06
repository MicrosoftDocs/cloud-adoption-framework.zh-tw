---
title: 透過 Azure 角色型存取控制來管理對 Azure 環境的存取
description: 了解如何透過 Azure 角色型存取控制為 Azure 環境設定存取控制。
author: LijuKodicheraJayadevan
ms.author: brblanch
ms.date: 04/09/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.localizationpriority: high
ms.custom: think-tank, fasttrack-edit, AQC, setup
ms.openlocfilehash: a89ae92dc9fd682b2a694610b394792361cf6fc0
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102115504"
---
# <a name="manage-access-to-your-azure-environment-with-azure-role-based-access-control"></a>透過 Azure 角色型存取控制來管理對 Azure 環境的存取

管理哪些人員可以存取 Azure 資源和訂用帳戶是 Azure 治理策略中很重要的一環，而且指派群組型存取權和許可權是不錯的做法。 處理群組而不是個別使用者，會簡化存取原則的維護、提供跨小組的一致存取管理，以及減少設定錯誤。 Azure 角色型存取控制 (Azure RBAC) 是對 Azure 中的存取進行管理的主要方法。

Azure RBAC 可讓您對 Azure 中的資源進行詳細的存取管理。 其可協助您管理可存取 Azure 資源的人員、這些人員如何使用資源，以及其可存取的範圍。

在規劃存取控制策略時，授與使用者完成其工作所需的最低權限。 下圖顯示指派 Azure RBAC 的建議模式。

![顯示 Azure 角色的圖示](./media/manage-access/role-examples.png)
圖 1：Azure 角色。

在規劃存取控制方法時，建議您與組織中擔任下列角色的人員合作：安全性和合規性、IT 管理和企業架構設計人員。

雲端採用架構會提供其他指引來使用[ Azure 角色型存取控制](../considerations/roles.md)來作為雲端採用工作。

::: zone target="chromeless"

## <a name="actions"></a>動作

**授與資源群組存取權：**

若要對使用者授與資源群組的存取權：

1. 移至 [**資源群組**]。
1. 選取資源群組。
1. 選取 [存取控制 (IAM)]。
1. 選取 [+新增] > [新增角色指派]。
1. 選取角色，然後將存取權指派給使用者、群組或服務主體。

::: form action="OpenBlade[#blade/HubsExtension/Resources/resourceType/Microsoft.Resources%2FSubscriptions%2FResourceGroups]" submitText="Go to resource groups" ::: form-end

**授與訂用帳戶存取權：**

若要對使用者授與訂用帳戶的存取權：

1. 請移至 [**訂用帳戶**]。
1. 選取一個訂用帳戶。
1. 選取 [存取控制 (IAM)]。
1. 選取 [+新增] > [新增角色指派]。
1. 選取角色，然後將存取權指派給使用者、群組或服務主體。

::: form action="OpenBlade[#blade/Microsoft_Azure_Billing/SubscriptionsBlade]" submitText="Go to subscriptions" ::: form-end

::: zone-end

::: zone target="docs"

## <a name="grant-resource-group-access"></a>授與資源群組存取權

若要對使用者授與資源群組的存取權：

1. 移至 [[資源群組](https://portal.azure.com/#blade/HubsExtension/BrowseResourceGroups)]。
1. 選取資源群組。
1. 選取 [存取控制 (IAM)]。
1. 選取 [+新增] > [新增角色指派]。
1. 選取角色，然後將存取權指派給使用者、群組或服務主體。

## <a name="grant-subscription-access"></a>授與訂用帳戶存取權

若要對使用者授與訂用帳戶的存取權：

1. 請移至 [[訂用帳戶](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)]。
1. 選取一個訂用帳戶。
1. 選取 [存取控制 (IAM)]。
1. 選取 [+新增] > [新增角色指派]。
1. 選取角色，然後將存取權指派給使用者、群組或服務主體。

## <a name="learn-more"></a>深入了解

若要深入了解，請參閱：

- [什麼是 Azure 角色型存取控制 (Azure RBAC)？](/azure/role-based-access-control/overview)
- [雲端採用架構：使用 Azure 角色型存取控制](../considerations/roles.md)

::: zone-end
