---
title: 簡單工作負載的治理設計
description: 瞭解在 Azure 中設計資源治理模型的程式，以支援單一小組和簡單的工作負載。
author: alexbuckgit
ms.author: abuck
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 75f07c6f3c37d83321fd6758d3d79c7573792ef0
ms.sourcegitcommit: 60d8b863d431b5d7c005f2f14488620b6c4c49be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83218215"
---
# <a name="governance-design-for-a-simple-workload"></a>簡單工作負載的治理設計

本指南的目標是協助您了解在 Azure 中設計資源治理模型的程序，以支援單一小組和簡單工作負載。 您會查看一組假設性治理需求，然後瀏覽滿足這些需求的數個範例實作。

在基礎採用階段中，我們的目標是將簡單工作負載部署到 Azure。 這會導致下列需求：

- 單一**工作負載擁有者**的身分識別管理，該擁有者負責部署和維護簡單工作負載。 工作負載擁有者需要建立、讀取、更新和刪除資源的權限，以及將這些權限委派給身分識別管理系統中其他使用者的權限。
- 以單一管理單位的形式管理簡單工作負載的所有資源。

## <a name="azure-licensing"></a>Azure 授權

在您開始設計治理模型之前，務必先了解 Azure 的授權方式。 這是因為與您的 Azure 授權相關聯的系統管理帳戶，對您的 Azure 資源具有最高層級的存取權。 這些系統管理帳戶形成治理模型的基礎。

> [!NOTE]
> 如果貴組織擁有現有 [Microsoft Enterprise 合約](https://www.microsoft.com/licensing/licensing-programs/enterprise)，但是其中不包含 Azure，可以藉由預先付款承諾來新增 Azure。 如需詳細資訊，請參閱為[企業授權 Azure](https://azure.microsoft.com/pricing/enterprise-agreement)。

當 Azure 新增至貴組織的 Enterprise 合約時，系統會提示貴組織建立 **Azure 帳戶**。 在帳戶建立程序中，會建立 **Azure 帳戶擁有者**，以及具有**全域管理員**帳戶的 Azure Active Directory (Azure AD) 租用戶。 Azure AD 租用戶是一種邏輯建構，代表 Azure AD 專用的安全執行個體。

![具有 Azure 帳戶擁有者和 Azure AD 全域管理員的 azure 帳戶 ](../../_images/govern/design/governance-3-0.png)
 _圖1：具有 azure 帳戶擁有者和 Azure AD 全域管理員的 azure 帳戶。_

## <a name="identity-management"></a>身分識別管理

Azure 只信任由 [Azure AD](https://docs.microsoft.com/azure/active-directory) 來驗證使用者以及將資源存取權授權給使用者，因此 Azure AD 是我們的身分識別管理系統。 Azure AD 全域管理員具有最高等級權限，可以執行與身分識別相關的所有動作，包括建立使用者與指派權限。

我們的需求是單一**工作負載擁有者**的身分識別管理，該擁有者負責部署和維護簡單工作負載。 工作負載擁有者需要建立、讀取、更新和刪除資源的權限，以及將這些權限委派給身分識別管理系統中其他使用者的權限。

我們的 Azure AD 全域管理員將為工作負載擁有者建立**工作負載擁有**者帳戶：

![Azure AD 全域管理員建立工作負載擁有者帳戶 ](../../_images/govern/design/governance-1-2.png)
 _圖2： Azure AD 全域管理員建立工作負載擁有者使用者帳戶。_

在將此使用者新增至**訂**用帳戶之前，您無法指派資源存取權限，因此您將在接下來的兩個區段中執行此動作。

## <a name="resource-management-scope"></a>資源管理範圍

隨著貴組織部署的資源數成長，治理這些資源的複雜度也隨之增加。 Azure 會實作邏輯容器階層，讓貴組織以各種層級的細微性 (也稱為**範圍**) 管理群組中的資源。

最上層的資源管理範圍是**訂用帳戶**層級。 訂用帳戶是由 Azure **帳戶擁有者**建立，該擁有者會建立財務承諾，並且負責支付與訂用帳戶相關聯的所有 Azure 資源費用：

![Azure 帳戶擁有者會建立訂用帳戶 ](../../_images/govern/design/governance-1-3.png)
 _圖3： azure 帳戶擁有者會建立訂用帳戶。_

訂用帳戶建立時，Azure **帳戶擁有者**會讓 Azure AD 租用戶與訂用帳戶產生關聯，並且使用這個 Azure AD 租用戶來驗證和授權使用者：

![Azure 帳戶擁有者會將 Azure AD 租使用者與訂用帳戶建立關聯 ](../../_images/govern/design/governance-1-4.png)
 _圖4： azure 帳戶擁有者會將 Azure AD 租使用者與訂用帳戶建立關聯。_

您可能已經注意到，目前沒有任何使用者與訂用帳戶相關聯，這表示沒有任何使用者具有管理資源的權限。 事實上，**帳戶擁有者**是訂用帳戶的擁有者，而且具有對訂用帳戶中資源採取任何動作的權限。 不過，實際上**帳戶擁有者**更像是貴組織中的財務人員，不負責建立、讀取、更新和刪除資源，這些工作會由**工作負載擁有者**執行。 因此，您必須將**工作負載擁有者**新增至訂用帳戶並指派權限。

由於**帳戶擁有者**是目前唯一有權將**工作負載擁有者**新增至訂用帳戶的使用者，因此他們要將**工作負載擁有者**新增至訂用帳戶：

![Azure 帳戶擁有者會將 * * 工作負載擁有者 * * 新增至訂用帳戶 ](../../_images/govern/design/governance-1-5.png)
 _圖5： azure 帳戶擁有者會將工作負載擁有者新增至訂用帳戶。_

Azure **帳戶擁有者**會藉由指派[角色型存取控制 (RBAC)](https://docs.microsoft.com/azure/role-based-access-control) 角色，將權限授與**工作負載擁有者**。 RBAC 角色會指定一組權限，讓**工作負載擁有者**針對個別資源類型或一組資源類型使用。

請注意，在此範例中，**帳戶擁有者**已獲得指派[內建**擁有者**角色](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner)：

![* * 工作負載擁有者 * * 已獲指派內建擁有者角色 ](../../_images/govern/design/governance-1-6.png)
 _圖6：已為工作負載擁有者指派內建擁有者角色。_

內建**擁有者**角色會在訂用帳戶範圍內，將所有權限授與**工作負載擁有者**。

> [!IMPORTANT]
> Azure**帳戶擁有**者負責與訂用帳戶相關聯的財務承諾，但**工作負載擁有**者具有相同的許可權。 **帳戶擁有者**必須信任**工作負載擁有者**會在訂用帳戶預算內部署資源。

管理範圍的下一個層級是**資源群組**層級。 資源群組是資源的邏輯容器。 在資源群組層級套用的作業會套用至群組中的所有資源。 此外，請務必注意，每個使用者的權限繼承自下一個層級以上，除非在該範圍明確變更。

為了說明這點，讓我們看看當**工作負載擁有者**建立資源群組時會發生什麼事：

![* * 工作負載擁有者 * * 建立資源群組 ](../../_images/govern/design/governance-1-7.png)
 _圖7：工作負載擁有者會建立資源群組，並在資源群組範圍繼承內建擁有者角色。_

同樣地，內建**擁有者**角色會在資源群組範圍內，將所有權限授與**工作負載擁有者**。 如同稍早所述，此角色是繼承自訂用帳戶層級。 如果有不同的角色在此範圍中指派給這位使用者，則它僅適用於此範圍。

管理範圍的最低層級是**資源**層級。 在資源層級套用的作業僅會套用到資源本身。 同樣地，資源層級的許可權會繼承自資源群組範圍。 例如，讓我們看看如果**工作負載擁有者**將[虛擬網路](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)部署到資源群組，會發生什麼情況：

![* * 工作負載擁有者 * * 會建立資源 ](../../_images/govern/design/governance-1-8.png)
 _圖8：工作負載擁有者會建立資源，並在資源範圍繼承內建擁有者角色。_

**工作負載擁有者**會在資源範圍繼承擁有者角色，這表示工作負載擁有者具有虛擬網路的所有權限。

## <a name="implement-the-basic-resource-access-management-model"></a>執行基本資源存取管理模型

讓我們繼續了解如何實作先前所設計的治理模型。

若要開始，貴組織需要 Azure 帳戶。 如果貴組織擁有現有 [Microsoft Enterprise 合約](https://www.microsoft.com/licensing/licensing-programs/enterprise)，但是其中不包含 Azure，可以藉由預先付款承諾來新增 Azure。 如需詳細資訊，請參閱為[企業授權 Azure](https://azure.microsoft.com/pricing/enterprise-agreement)。

在建立您的 Azure 帳戶時，會指定貴組織中的某位人員成為 Azure **帳戶擁有者**。 預設會接著建立 Azure Active Directory (Azure AD) 租用戶。 您的 Azure **帳戶擁有者**，必須為組織中身為**工作負載擁有者**的人員[建立使用者帳戶](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory)。

接下來，您的 Azure **帳戶擁有者**必須[建立訂用帳戶](https://docs.microsoft.com/partner-center/create-a-new-subscription)，並且將其[關聯到 Azure AD 租用戶](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-how-subscriptions-associated-directory)。

最後，既然已建立訂用帳戶，且您的 Azure AD 租用戶已與其相關聯，就可以將**工作負載擁有者**[，新增至具備內建**擁有者**角色](https://docs.microsoft.com/azure/billing/billing-add-change-azure-subscription-administrator#to-assign-a-user-as-an-administrator)的訂用帳戶。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [將基本工作負載部署至 Azure](../../infrastructure/virtual-machines/basic-workload.md)
> [!div class="nextstepaction"]
> [深入了解多個小組的資源存取](./governance-multiple-teams.md)
