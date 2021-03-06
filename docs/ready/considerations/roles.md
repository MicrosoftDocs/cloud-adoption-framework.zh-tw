---
title: 建議的 Azure 角色型存取控制
description: 瞭解如何將小組內的職責分開，並授與 Azure 角色型存取控制，讓使用者和群組可以執行其工作。
author: alexbuckgit
manager: BrianBlanchard
ms.author: brblanch
ms.date: 11/28/2018
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: internal, virtual-network
ms.openlocfilehash: bfb25396e19b11258401dd68065282cc3e78566d
ms.sourcegitcommit: a0ddde4afcc7d8c21559e79d406dc439ee4f38d2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/21/2020
ms.locfileid: "97713364"
---
# <a name="azure-role-based-access-control"></a>Azure 角色型存取控制

群組型存取權和許可權是不錯的做法。 處理群組而不是個別使用者，會簡化存取原則的維護、提供跨小組的一致存取管理，以及減少設定錯誤。 將使用者指派至適當群組以及從中移除，有助於保持特定使用者的最新許可權。 [Azure 角色型存取控制 (AZURE RBAC) ](/azure/role-based-access-control/overview) 針對以使用者角色組織的資源提供更細緻的存取管理。

如需建議的 Azure RBAC 作法作為身分識別和安全性策略的一部分，請參閱 [azure 身分識別管理和存取控制安全性最佳作法](/azure/security/fundamentals/identity-management-best-practices#use-role-based-access-control)。

## <a name="overview-of-azure-role-based-access-control"></a>Azure 角色型存取控制的總覽

藉由使用 [Azure 角色型存取控制](/azure/role-based-access-control/overview)，您可以將小組內的職責區隔，並僅授與特定 Azure Active Directory 的足夠存取權 (Azure AD) 使用者、群組、服務主體或受控識別，以執行其工作。 您可以限制每一組資源的許可權，而不是讓每個人不受限制地存取您的 Azure 訂用帳戶或資源。

[Azure 角色定義](/azure/role-based-access-control/role-definitions) 會列出指派給該角色的使用者或群組允許或不允許的作業。 角色的[範圍](/azure/role-based-access-control/overview#scope)會指定要將這些定義的許可權套用到哪些資源。 您可以在多個層級指定範圍：管理群組、訂用帳戶、資源群組或資源。 範圍會以父子式關聯性進行結構化。

![Azure RBAC 範圍階層](../../_images/azure-best-practices/rbac-scope.png)

如需將使用者和群組指派給特定角色以及將角色指派給範圍的詳細指示，請參閱 [使用 Azure 入口網站新增或移除 Azure 角色指派](/azure/role-based-access-control/role-assignments-portal)。

在規劃存取控制策略時，請使用最低許可權存取模型，僅授與使用者執行其工作所需的許可權。 下圖顯示透過此方法使用 Azure RBAC 的建議模式。

![使用 Azure RBAC 的建議模式](../../_images/azure-best-practices/rbac-least-privilege.png)

> [!NOTE]
> 您定義的是更具體或更詳細的許可權，您的存取控制會變得很複雜且難以管理。 當您的雲端資產大小成長時，更是如此。 避免資源特定的許可權。 相反地，請將 [管理群組](/azure/governance/management-groups) 用於企業級的存取控制和 [資源群組](/azure/azure-resource-manager/management/overview#resource-groups) ，以進行訂用帳戶中的存取控制。 也請避免使用者特定的許可權。 相反地，請將存取權指派給 [Azure AD 中的群組](/azure/active-directory/fundamentals/active-directory-manage-groups)。

## <a name="use-azure-built-in-roles"></a>使用 Azure 內建角色

Azure 提供許多內建角色定義，其中包含三個核心角色來提供存取權：

- [擁有者角色](/azure/role-based-access-control/built-in-roles#owner)可以管理所有專案，包括對資源的存取。
- [參與者角色](/azure/role-based-access-control/built-in-roles#contributor)可以管理資源存取以外的所有專案。
- [讀者角色](/azure/role-based-access-control/built-in-roles#reader)可以查看所有內容，但不會進行任何變更。

從這些核心存取層級開始，其他內建角色會提供更詳細的控制項來存取特定資源類型或 Azure 功能。 例如，您可以使用下列內建角色來管理對虛擬機器的存取：

- [虛擬機器系統管理員登入角色](/azure/role-based-access-control/built-in-roles#virtual-machine-administrator-login)可以在入口網站中查看虛擬機器，並以身分登入 `administrator` 。
- 「 [虛擬機器參與者」角色](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) 可以管理虛擬機器，但無法存取它們或它們所連線的虛擬網路或儲存體帳戶。
- [虛擬機器使用者登入角色](/azure/role-based-access-control/built-in-roles#virtual-machine-user-login)可以在入口網站中查看虛擬機器，並以一般使用者身分登入。

如需使用內建角色來管理特定功能存取權的另一個範例，請參閱有關在 [業務單位、環境或專案之間追蹤](../azure-best-practices/track-costs.md#provide-the-right-level-of-cost-access)成本追蹤功能存取權的討論。

如需可用內建角色的完整清單，請參閱 [Azure 內建角色](/azure/role-based-access-control/built-in-roles)。

## <a name="use-custom-roles"></a>使用自訂角色

雖然內建至 Azure 的角色支援各種不同存取控制案例，但是可能不符合貴組織或小組的所有需求。 例如，如果您有一個負責管理虛擬機器和 Azure SQL Database 資源的單一使用者群組，您可能會想要建立自訂角色，以最佳化所需存取控制的管理。

Azure RBAC 文件包含[建立自訂角色](/azure/role-based-access-control/custom-roles)的相關指示，以及[角色定義的運作方式](/azure/role-based-access-control/role-definitions)的詳細資料。

## <a name="separation-of-responsibilities-and-roles-for-large-organizations"></a>為大型組織區分職責與角色

Azure RBAC 可讓組織將不同的小組指派給大型雲端資產中的各種管理工作。 它可以讓中央 IT 小組控制核心存取和安全性功能，同時讓軟體開發人員和其他小組對特定工作負載或資源群組進行大量控制。

大部分雲端環境也可以受益於使用多個角色的存取控制策略，並強調這些角色之間的職責分隔。 這種方法需要資源或基礎結構的任何重大變更有多個角色涉入才能完成，確保有多人必須檢閱並核准變更。 這種職責區隔可限制單一人員存取敏感性資料的能力，或在不知道其他小組成員的情況下引進弱點。

下表說明將 IT 職責劃分成不同自訂角色的常見模式：

| 群組 | 一般角色名稱 | 職責 |
| --- | --- | --- |
| 安全性作業 | SecOps | 提供一般的安全性監督。 <br> 建立並強制執行安全性原則，例如靜態加密。 <br><br> 管理加密金鑰。 <br><br> 管理防火牆規則。 |
| 網路作業 | NetOps | 管理虛擬網路中的網路設定和作業，例如路由和對等互連。 |
| 系統作業 | SysOps | 指定計算和儲存體基礎結構選項，並維護已部署的資源。 |
| 開發、測試和作業 | DevOps | 建立及部署工作負載功能和應用程式。 <br><br> 操作功能和應用程式，以符合服務等級協定和其他品質標準。 |

即使這些角色是由不同層級的不同人員執行，這些標準角色中的動作和許可權分解通常都是相同的。 因此，您可以建立一組通用的 Azure 角色定義，以套用至環境內的不同範圍。 接著，可以將一般角色指派給使用者和群組，但只適用於其負責管理的資源、資源群組、訂用帳戶或管理群組。

例如，在具有多個訂用帳戶的 [中樞和輪輻網路拓撲](../azure-best-practices/hub-spoke-network-topology.md) 中，您可能會有一組共同的中樞和所有工作負載輪輻的角色定義。 中樞訂用帳戶的 NetOps 角色可以指派給組織中央 IT 小組的成員，負責維護所有工作負載所使用之共用服務的網路。 然後，可以將工作負載支點訂用帳戶的 NetOps 角色指派給該特定工作負載小組的成員，讓他們能夠在該訂用帳戶內設定網路功能，以充分支援其工作負載需求。 這兩者都使用相同的角色定義，但是範圍型指派可確保使用者只擁有執行其工作所需的存取權。
