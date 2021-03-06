---
title: 簡單工作負載的治理設計
description: 瞭解在 Azure 中設計資源治理模型的程式，以支援單一小組和簡單的工作負載。
author: alexbuckgit
ms.author: abuck
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: internal
ms.openlocfilehash: 1424de868c8dd93f416bdfa726104294b0727881
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101791203"
---
# <a name="governance-design-for-a-simple-workload"></a>簡單工作負載的治理設計

本指南的目標是協助您了解在 Azure 中設計資源治理模型的程序，以支援單一小組和簡單工作負載。 您會查看一組假設性治理需求，然後瀏覽滿足這些需求的數個範例實作。

在基礎採用階段中，我們的目標是將簡單工作負載部署到 Azure。 這會導致下列需求：

- 單一 **工作負載擁有者** 的身分識別管理，該擁有者負責部署和維護簡單工作負載。 工作負載擁有者需要建立、讀取、更新和刪除資源的權限，以及將這些權限委派給身分識別管理系統中其他使用者的權限。
- 以單一管理單位的形式管理簡單工作負載的所有資源。

## <a name="azure-licensing"></a>Azure 授權

在您開始設計治理模型之前，務必先了解 Azure 的授權方式。 這是因為與您的 Azure 授權相關聯的系統管理帳戶具有最高層級的 Azure 資源存取權。 這些系統管理帳戶形成治理模型的基礎。

> [!NOTE]
> 如果貴組織擁有現有 [Microsoft Enterprise 合約](https://www.microsoft.com/licensing/licensing-programs/enterprise)，但是其中不包含 Azure，可以藉由預先付款承諾來新增 Azure。 如需詳細資訊，請參閱 [為企業授權 Azure](https://azure.microsoft.com/overview/sales-number/)。

當 Azure 新增至貴組織的 Enterprise 合約時，系統會提示貴組織建立 **Azure 帳戶**。 在帳戶建立程序中，會建立 **Azure 帳戶擁有者**，以及具有 **全域管理員** 帳戶的 Azure Active Directory (Azure AD) 租用戶。 Azure AD 租用戶是一種邏輯建構，代表 Azure AD 專用的安全執行個體。

![具有 Azure 帳戶擁有者和 Azure AD 全域管理員的 azure 帳戶 ](../../_images/govern/design/governance-3-0.png)
 *圖1：具有 azure 帳戶擁有者和 azure ad 全域管理員的 azure 帳戶。*

## <a name="identity-management"></a>身分識別管理

Azure 只信任由 [Azure AD](/azure/active-directory/) 來驗證使用者以及將資源存取權授權給使用者，因此 Azure AD 是我們的身分識別管理系統。 Azure AD 全域管理員具有最高層級的許可權，而且可以執行與身分識別相關的所有動作，包括建立使用者和指派許可權。

我們的需求是單一 **工作負載擁有者** 的身分識別管理，該擁有者負責部署和維護簡單工作負載。 工作負載擁有者需要建立、讀取、更新和刪除資源的權限，以及將這些權限委派給身分識別管理系統中其他使用者的權限。

我們的 Azure AD 全域管理員會為工作負載擁有者建立 **工作負載擁有** 者帳戶：

![Azure AD 全域管理員建立工作負載擁有者帳戶 ](../../_images/govern/design/governance-1-2.png)
 *圖2： azure ad 全域管理員建立工作負載擁有者使用者帳戶。*

在將此使用者新增至 **訂** 用帳戶之前，您無法指派資源存取權限，因此您將在接下來的兩個區段中進行。

## <a name="resource-management-scope"></a>資源管理範圍

隨著貴組織部署的資源數成長，治理這些資源的複雜度也隨之增加。 Azure 會實作邏輯容器階層，讓貴組織以各種層級的細微性 (也稱為 **範圍**) 管理群組中的資源。

最上層的資源管理範圍是 **訂用帳戶** 層級。 訂用帳戶是由 Azure **帳戶擁有者** 建立，該擁有者會建立財務承諾，並且負責支付與訂用帳戶相關聯的所有 Azure 資源費用：

![Azure 帳戶擁有者建立訂用帳戶 ](../../_images/govern/design/governance-1-3.png)
 *圖3： azure 帳戶擁有者建立訂用帳戶。*

訂用帳戶建立時，Azure **帳戶擁有者** 會讓 Azure AD 租用戶與訂用帳戶產生關聯，並且使用這個 Azure AD 租用戶來驗證和授權使用者：

![Azure 帳戶擁有者會將 Azure AD 租使用者與訂用帳戶 [圖 4] 產生關聯 ](../../_images/govern/design/governance-1-4.png)
 *： azure 帳戶擁有者會將 azure ad 租使用者與訂用帳戶產生關聯。*

您可能已經注意到，目前沒有任何使用者與訂用帳戶相關聯，這表示沒有任何使用者具有管理資源的權限。 在實務上， **帳戶擁有** 者是訂用帳戶的擁有者，而且具有在訂用帳戶中對資源採取任何動作的許可權。 實際上， **帳戶擁有** 者不太可能是您組織中的財務人員，也不負責建立、讀取、更新和刪除資源。 **工作負載擁有** 者會執行這些工作，因此您必須將 **工作負載擁有** 者新增至訂用帳戶並指派許可權。

由於 **帳戶擁有者** 是目前唯一有權將 **工作負載擁有者** 新增至訂用帳戶的使用者，因此他們要將 **工作負載擁有者** 新增至訂用帳戶：

![Azure 帳戶擁有者會將 * * 工作負載擁有者 * * 新增至訂用帳戶 ](../../_images/govern/design/governance-1-5.png)
 *圖5： Azure 帳戶擁有者會將工作負載擁有者新增至訂用帳戶。*

Azure **帳戶擁有** 者藉由指派 [Azure 角色](/azure/role-based-access-control/)來授與許可權給 **工作負載擁有** 者。 Azure 角色會指定 **工作負載擁有** 者針對個別資源類型或一組資源類型所擁有的一組許可權。

請注意，在此範例中，**帳戶擁有者** 已獲得指派 [內建 **擁有者** 角色](/azure/role-based-access-control/built-in-roles#owner)：

![* * 工作負載擁有者 * * 已獲指派內建的擁有者角色 ](../../_images/govern/design/governance-1-6.png)
 *圖6：指派了內建擁有者角色給工作負載擁有者。*

內建 **擁有者** 角色會在訂用帳戶範圍內，將所有權限授與 **工作負載擁有者**。

> [!IMPORTANT]
> Azure **帳戶擁有** 者會負責與訂用帳戶相關聯的財務承諾，但 **工作負載擁有** 者具有相同的許可權。 **帳戶擁有者** 必須信任 **工作負載擁有者** 會在訂用帳戶預算內部署資源。

管理範圍的下一個層級是 **資源群組** 層級。 資源群組是資源的邏輯容器。 在資源群組層級套用的作業會套用至群組中的所有資源。 此外，請務必注意，每個使用者的許可權都會從下一個層級中繼承，除非在該範圍明確變更。

為了說明這點，讓我們看看當 **工作負載擁有者** 建立資源群組時會發生什麼事：

![* * 工作負載擁有者 * * 建立資源群組 ](../../_images/govern/design/governance-1-7.png)
 *圖7：工作負載擁有者會建立資源群組，並繼承資源群組範圍內的內建擁有者角色。*

同樣地，內建 **擁有者** 角色會在資源群組範圍內，將所有權限授與 **工作負載擁有者**。 如同稍早所述，此角色是繼承自訂用帳戶層級。 如果有不同的角色在此範圍中指派給這位使用者，則它僅適用於此範圍。

管理範圍的最低層級是 **資源** 層級。 在資源層級套用的作業僅會套用到資源本身。 同樣地，資源層級的許可權會繼承自資源群組範圍。 例如，讓我們看看如果 **工作負載擁有者** 將 [虛擬網路](/azure/virtual-network/virtual-networks-overview)部署到資源群組，會發生什麼情況：

![* * 工作負載擁有者 * * 建立資源 ](../../_images/govern/design/governance-1-8.png)
 *圖8：工作負載擁有者建立資源，並在資源範圍繼承內建擁有者角色。*

**工作負載擁有** 者會繼承資源範圍的擁有者角色，這表示工作負載擁有者具有虛擬網路的擁有權限。

## <a name="implement-the-basic-resource-access-management-model"></a>執行基本資源存取管理模型

讓我們繼續了解如何實作先前所設計的治理模型。

若要開始，貴組織需要 Azure 帳戶。 如果貴組織擁有現有 [Microsoft Enterprise 合約](https://www.microsoft.com/licensing/licensing-programs/enterprise)，但是其中不包含 Azure，可以藉由預先付款承諾來新增 Azure。 如需詳細資訊，請參閱 [為企業授權 Azure](https://azure.microsoft.com/overview/sales-number/)。

在建立您的 Azure 帳戶時，會指定貴組織中的某位人員成為 Azure **帳戶擁有者**。 預設會接著建立 Azure Active Directory (Azure AD) 租用戶。 您的 Azure **帳戶擁有者**，必須為組織中身為 **工作負載擁有者** 的人員 [建立使用者帳戶](/azure/active-directory/fundamentals/add-users-azure-active-directory)。

接下來，您的 Azure **帳戶擁有者** 必須 [建立訂用帳戶](/partner-center/create-a-new-subscription)，並且將其 [關聯到 Azure AD 租用戶](/azure/active-directory/fundamentals/active-directory-how-subscriptions-associated-directory)。

最後，既然已建立訂用帳戶，且您的 Azure AD 租用戶已與其相關聯，就可以將 **工作負載擁有者**[，新增至具備內建 **擁有者** 角色](/azure/cost-management-billing/manage/add-change-subscription-administrator#to-assign-a-user-as-an-administrator)的訂用帳戶。

## <a name="next-steps"></a>下一步

> [!div class="nextstepaction"]
> [深入了解多個小組的資源存取](./governance-multiple-teams.md)
