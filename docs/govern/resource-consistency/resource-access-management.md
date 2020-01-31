---
title: Azure 中的資源存取管理
description: Azure 中的資源存取管理結構說明： Azure Resource Manager、訂用帳戶、資源群組和資源
author: alexbuckgit
ms.author: abuck
ms.date: 09/17/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 825c220bda1c560c5d7bf07bcae649017525ff53
ms.sourcegitcommit: 2362fb3154a91aa421224ffdb2cc632d982b129b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/28/2020
ms.locfileid: "76805620"
---
# <a name="resource-access-management-in-azure"></a>Azure 中的資源存取管理

[雲端治理](../index.md)概述雲端治理的五個專業領域，其中包括資源管理。 [什麼是資源存取治理](./index.md)進一步說明了資源存取管理如何融入資源管理專業領域。 在您繼續了解如何設計治理模型之前，務必了解 Azure 中的資源存取權管理控制項。 這些資源存取權管理控制項的組態會形成治理模型的基礎。

一開始請先仔細看看資源在 Azure 中的部署方式。

<!-- markdownlint-disable MD026 -->

## <a name="what-is-an-azure-resource"></a>什麼是 Azure 資源？

在 Azure 中，_資源_一詞是指由 Azure 管理的實體。 例如，虛擬機器、虛擬網路以及儲存體帳戶都是 Azure 資源。

![資源的圖表](../../_images/govern/design/governance-1-9.png)
*圖 1-資源。*

## <a name="what-is-an-azure-resource-group"></a>什麼是 Azure 資源群組？

Azure 中的每個資源都必須屬於一個[資源群組](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)。 資源群組只是將多個資源分組在一起的邏輯結構，因此可以根據_生命週期和安全性_，以單一實體的形式進行管理。 例如，共用類似生命週期的資源如[多層式架構應用程式](https://docs.microsoft.com/azure/architecture/guide/architecture-styles/n-tier)的資源，能以群組形式建立或刪除。 還有另一種方式：在一起合作的所有專案，一起一起管理，並淘汰在一起，一起放在資源群組中。

![圖包含資源的資源群組](../../_images/govern/design/governance-1-10.png)
*圖 2-資源群組包含資源。*

資源群組及其所包含的資源會與 Azure **訂用帳戶**相關聯。

## <a name="what-is-an-azure-subscription"></a>什麼是 Azure 訂用帳戶？

Azure 訂用帳戶類似於資源群組，它是一個邏輯建構，將資源群組及其資源群組在一起。 不過，Azure 訂用帳戶也與 Azure Resource Manager 所使用的控制項相關聯。 進一步了解 Azure Resource Manager，以了解它與 Azure 訂用帳戶之間的關聯性。

Azure 訂用帳戶的 ![圖](../../_images/govern/design/governance-1-11.png)
*圖 3-azure 訂*用帳戶。

## <a name="what-is-azure-resource-manager"></a>什麼是 Azure Resource Manager？

在 [Azure 如何運作？](../../getting-started/what-is-azure.md)中，您已了解 Azure 包含「前端」與會協調 Azure 所有函式的多個服務。 這些服務的其中之一就是 [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager)，這個服務會裝載用戶端用來管理資源的 RESTful API。

Azure Resource Manager 的 ![圖](../../_images/govern/design/governance-1-12.png)
[*圖 4] Azure Resource Manager。*

下圖顯示三個用戶端： [PowerShell](https://docs.microsoft.com/powershell/azure/overview)、 [Azure 入口網站](https://portal.azure.com)和[Azure CLI](https://docs.microsoft.com/cli/azure)：

連接到 Azure Resource Manager API 的 Azure 用戶端 ![圖](../../_images/govern/design/governance-1-13.png)
*圖 5-azure 用戶端會連線至 Azure Resource Manager RESTFUL api。*

雖然這些用戶端會使用 RESTful API 連線到 Azure Resource Manager，但 Azure Resource Manager 不包含直接管理資源的功能。 相反地，Azure 中的大多數資源類型都有自己的[**資源提供者**](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#terminology)。

![Azure 資源提供者](../../_images/govern/design/governance-1-14.png)
*圖 6-azure 資源提供者。*

當用戶端要求管理特定資源時，Azure Resource Manager 會連線到該資源類型的資源提供者，來完成要求。 例如，如果用戶端要求管理虛擬機器資源，Azure Resource Manager 會連線到 **Microsoft.compute** 資源提供者。

![Azure Resource Manager 連線到 Microsoft。計算資源提供者](../../_images/govern/design/governance-1-15.png)
*圖 7-Azure Resource Manager 會連線到**Microsoft. 計算**資源提供者，以管理用戶端要求中指定的資源。*

Azure Resource Manager 需要用戶端同時指定訂用帳戶和資源群組的識別碼，才能管理虛擬機器資源。

既然您已了解 Azure Resource Manager 如何運作，請回到討論 Azure 訂用帳戶如何與 Azure Resource Manager 所使用的控制項相關聯。 在 Azure Resource Manager 可以執行任何資源管理要求之前，會先檢查一組控制項。

第一個控制項是要求必須由已驗證使用者提出，而 Azure Resource Manager 具有與 [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory) 的信任關係，可以提供使用者身分識別功能。

![Azure Active Directory](../../_images/govern/design/governance-1-16.png)
*圖 8-Azure Active Directory。*

在 Azure AD 中，使用者會劃分到不同**租用戶**中。 租用戶是一種邏輯建構，代表通常與組織相關聯的 Azure AD 專用的安全執行個體。 每個訂用帳戶都會與 Azure AD 租用戶相關聯。

![與訂用帳戶相關聯的 Azure AD 租使用者](../../_images/govern/design/governance-1-17.png)
*圖 9-與訂用帳戶相關聯的 Azure AD 租使用者。*

對於要在特定訂用帳戶中管理資源的每個用戶端要求，都需要使用者在相關聯的 Azure AD 租用戶中具有帳戶。

下一個控制項會檢查使用者具有足夠權限可以提出要求。 您要使用[角色型存取控制 (RBAC)](https://docs.microsoft.com/azure/role-based-access-control) 將權限指派給使用者。

![指派給 RBAC 角色的使用者](../../_images/govern/design/governance-1-18.png)
*圖 10。租使用者中的每個使用者都會被指派一個或多個 RBAC 角色。*

RBAC 角色指定了一組權限，使用者可能會在特定資源上採用那些權限。 當角色指派給使用者時，會套用這些權限。 例如，[內建**擁有者**角色](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner)可讓使用者對資源執行任何動作。

下一個控制項會檢查在針對 [Azure 資源原則](https://docs.microsoft.com/azure/governance/policy)指定的設定下，是否允許要求。 Azure 資源原則會指定對特定資源允許的作業。 例如，Azure 資源原則可以指定只允許使用者部署特定類型的虛擬機器。

![Azure 資源原則](../../_images/govern/design/governance-1-19.png)
*圖 11。Azure 資源原則。*

下一個控制項會檢查要求不超過 [Azure 訂用帳戶限制](https://docs.microsoft.com/azure/azure-subscription-service-limits)。 例如，每個訂用帳戶的資源群組上限為 980 個。 當達到限制時，如果收到要求部署其他資源群組，則會遭到拒絕。

![Azure 資源限制](../../_images/govern/design/governance-1-20.png)
[*圖 12]。Azure 資源限制。*

最後一個控制項會檢查要求仍在與訂用帳戶相關聯的財務承諾之內。 例如，如果要求是要部署虛擬機器，Azure Resource Manager 會確認訂用帳戶具有足夠的付款資訊。

![與訂用帳戶相關聯的財務承諾](../../_images/govern/design/governance-1-21.png)
[*圖 13]。財務承諾與訂用帳戶相關聯。*

## <a name="summary"></a>摘要

在本文中，您已了解如何使用 Azure Resource Manager 在 Azure 中管理資源存取權。

## <a name="next-steps"></a>後續步驟

既然您已了解如何在 Azure 中管理資源存取權，請繼續了解如何使用這些服務設計[適用於簡單工作負載](./governance-simple-workload.md)或適用於[多個小組](./governance-multiple-teams.md)的治理模型。

> [!div class="nextstepaction"]
> [治理概觀](../index.md)
