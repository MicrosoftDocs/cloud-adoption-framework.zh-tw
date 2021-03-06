---
title: Azure 中的資源存取管理
description: 瞭解 Azure 資源存取管理的概念，例如 Azure Resource Manager、訂用帳戶、資源群組和資源。
author: alexbuckgit
ms.author: abuck
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: internal
ms.openlocfilehash: 2924bbf984a8ed570f4d139f4a0a3b0ccf28a1e6
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102115453"
---
# <a name="resource-access-management-in-azure"></a>Azure 中的資源存取管理

治理 [方法](../index.md) 會概述雲端治理的五個專業領域，包括資源管理。 [什麼是資源存取治理](./index.md)進一步說明了資源存取管理如何融入資源管理專業領域。 在您繼續了解如何設計治理模型之前，務必了解 Azure 中的資源存取權管理控制項。 這些資源存取權管理控制項的組態會形成治理模型的基礎。

一開始請先仔細看看資源在 Azure 中的部署方式。

## <a name="what-is-an-azure-resource"></a>什麼是 Azure 資源？

在 Azure 中，*資源* 一詞是指由 Azure 管理的實體。 例如，虛擬機器、虛擬網路以及儲存體帳戶都是 Azure 資源。

![資源 ](../../_images/govern/design/governance-1-9.png)
 *圖1：資源* 的圖表。

## <a name="what-is-an-azure-resource-group"></a>什麼是 Azure 資源群組？

Azure 中的每個資源都必須屬於一個[資源群組](/azure/azure-resource-manager/management/overview#resource-groups)。 資源群組只是一個邏輯結構，可將多個資源群組在一起，以便 **根據生命週期和安全性** 將它們當作單一實體來管理。 例如，共用類似生命週期的資源如[多層式架構應用程式](/azure/architecture/guide/architecture-styles/n-tier)的資源，能以群組形式建立或刪除。 換句話說，在一起的所有專案都會一起進行管理，並一起淘汰，在資源群組中一起運作。

![包含資源的資源群組圖 ](../../_images/govern/design/governance-1-10.png)
 *2：資源群組包含資源。*

資源群組及其所包含的資源會與 Azure 訂用帳戶相關聯。

## <a name="what-is-an-azure-subscription"></a>什麼是 Azure 訂用帳戶？

Azure *訂* 用帳戶類似于資源群組，因為它是將資源群組和其資源群組在一起的邏輯結構。 Azure 訂用帳戶也會與 Azure Resource Manager 所使用的控制項相關聯。 進一步了解 Azure Resource Manager，以了解它與 Azure 訂用帳戶之間的關聯性。

![Azure 訂用帳戶的圖 ](../../_images/govern/design/governance-1-11.png)
 *3： azure 訂* 用帳戶。

## <a name="what-is-azure-resource-manager"></a>什麼是 Azure Resource Manager？

[Azure 如何運作？](../../get-started/what-is-azure.md)您已瞭解 azure 所包含的前端有許多可協調 Azure 功能的服務。 這些服務的其中之一就是 [Azure Resource Manager](/azure/azure-resource-manager/)，這個服務會裝載用戶端用來管理資源的 RESTful API。

![Azure Resource Manager ](../../_images/govern/design/governance-1-12.png)
 *圖4： Azure resource manager* 的圖表。

下圖顯示三個用戶端： [PowerShell](/powershell/azure/)、 [azure 入口網站](https://portal.azure.com)和 [azure CLI](/cli/azure/)：

![連線至 Resource Manager REST API 的 Azure 用戶端圖 ](../../_images/govern/design/governance-1-13.png)
 *5： azure 用戶端會連線至 RESOURCE MANAGER rest api。*

當這些用戶端使用 REST API 連線到 Resource Manager 時，Resource Manager 不包含直接管理資源的功能。 相反地，Azure 中的大多數資源類型都有自己的[資源提供者](/azure/azure-resource-manager/management/overview#terminology)。

![Azure 資源提供者 ](../../_images/govern/design/governance-1-14.png)
 *圖6： azure 資源提供者。*

當用戶端要求管理特定資源時，Azure Resource Manager 會連線到該資源類型的資源提供者，來完成要求。 例如，如果用戶端要求管理虛擬機器資源，Azure Resource Manager 會連接到 `Microsoft.Compute` 資源提供者。

![連接到 Microsoft 的 azure Resource Manager ](../../_images/govern/design/governance-1-15.png)
 *圖7： Azure resource manager 會連線到 `Microsoft.Compute` 資源提供者，以管理用戶端要求中指定的資源。*

Azure Resource Manager 需要用戶端同時指定訂用帳戶和資源群組的識別碼，才能管理虛擬機器資源。

既然您已了解 Azure Resource Manager 如何運作，請回到討論 Azure 訂用帳戶如何與 Azure Resource Manager 所使用的控制項相關聯。 在 Azure Resource Manager 可以執行任何資源管理要求之前，會先檢查一組控制項。

第一個控制項是要求必須由已驗證使用者提出，而 Azure Resource Manager 具有與 [Azure Active Directory (Azure AD)](/azure/active-directory/) 的信任關係，可以提供使用者身分識別功能。

![Azure Active Directory ](../../_images/govern/design/governance-1-16.png)
 *圖8： Azure active directory。*

在 Azure AD 中，使用者會劃分到不同租用戶中。 *租* 使用者是一種邏輯結構，代表通常與組織相關聯的安全、專用的 Azure AD 實例。 每個訂用帳戶都會與 Azure AD 租用戶相關聯。

![與訂用帳戶相關聯的 Azure AD 租使用者 ](../../_images/govern/design/governance-1-17.png)
 *： [圖 9]：與訂用帳戶相關聯的 azure ad 租使用者。*

對於要在特定訂用帳戶中管理資源的每個用戶端要求，都需要使用者在相關聯的 Azure AD 租用戶中具有帳戶。

下一個控制項會檢查使用者具有足夠權限可以提出要求。 許可權是使用 [azure 角色型存取控制 (AZURE RBAC) ](/azure/role-based-access-control/)指派給使用者。

![指派給 Azure 角色 ](../../_images/govern/design/governance-1-18.png)
 *的使用者圖10：租使用者中的每位使用者都會獲指派一或多個 Azure 角色。*

Azure 角色會指定使用者在特定資源上所採取的一組許可權。 當角色指派給使用者時，會套用這些權限。 例如， [內建 `owner` 角色](/azure/role-based-access-control/built-in-roles#owner) 可讓使用者在資源上執行任何動作。

下一個控制項會檢查在針對 [Azure 資源原則](/azure/governance/policy/)指定的設定下，是否允許要求。 Azure 資源原則會指定對特定資源允許的作業。 例如，Azure 資源原則可以指定只允許使用者部署特定類型的虛擬機器。

![Azure 資源原則 ](../../_images/govern/design/governance-1-19.png)
 *圖11： azure 資源原則。*

下一個控制項會檢查要求不超過 [Azure 訂用帳戶限制](/azure/azure-resource-manager/management/azure-subscription-service-limits)。 例如，每個訂用帳戶的資源群組上限為 980 個。 如果在達到限制時收到要求以部署額外的資源群組，就會遭到拒絕。

![Azure 資源限制 ](../../_images/govern/design/governance-1-20.png)
 *圖12： azure 資源限制。*

最後一個控制項會檢查要求仍在與訂用帳戶相關聯的財務承諾之內。 例如，如果要求是要部署虛擬機器，Azure Resource Manager 會確認訂用帳戶具有足夠的付款資訊。

![與訂用帳戶相關聯的財務承諾 ](../../_images/govern/design/governance-1-21.png)
 *圖13：財務承諾與訂用帳戶相關聯。*

## <a name="summary"></a>摘要

在本文中，您已了解如何使用 Azure Resource Manager 在 Azure 中管理資源存取權。

## <a name="next-steps"></a>下一步

既然您已了解如何在 Azure 中管理資源存取權，請繼續了解如何使用這些服務設計[適用於簡單工作負載](./governance-simple-workload.md)或適用於[多個小組](./governance-multiple-teams.md)的治理模型。

> [!div class="nextstepaction"]
> [治理概觀](../index.md)
