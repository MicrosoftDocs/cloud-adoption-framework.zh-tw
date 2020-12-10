---
title: Azure 基礎概念
description: 使用適用于 Azure 的雲端採用架構，瞭解 Azure 中使用的基本概念和詞彙，以及這些概念彼此之間的關聯性。
author: alexbuckgit
ms.author: abuck
ms.date: 05/20/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: internal
ms.openlocfilehash: db862e0329769f4093e1cd71f302e31f2aad01af
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97013513"
---
# <a name="azure-fundamental-concepts"></a>Azure 基礎概念

了解在 Azure 中使用的基本概念和詞彙，以及這些概念如何彼此相關。

## <a name="azure-terminology"></a>Azure 術語

當您開始進行 Azure 雲端採用工作時，了解下列定義會很有幫助：

- **資源：** 由 Azure 管理的實體。 範例包括 Azure 虛擬機器、虛擬網路和儲存體帳戶。
- **訂** 用帳戶：資源的邏輯容器。 每個 Azure 資源都只會與一個訂用帳戶相關聯。 建立訂用帳戶是採用 Azure 的第一步。
- **Azure 帳戶：** 當您建立 Azure 訂用帳戶時所提供的電子郵件地址就是訂用帳戶的 Azure 帳戶。 與電子郵件帳戶相關聯的對象需負責處理每月訂用帳戶資源所產生的成本。 當您建立 Azure 帳戶時，您會提供連絡人資訊和計費詳細資料，例如信用卡。 您可以針對多個訂用帳戶使用相同的 Azure 帳戶 (電子郵件地址)。 每個訂用帳戶只能與一個 Azure AD 帳戶相關聯。
- **帳戶管理員：** 與用來建立 Azure 訂用帳戶的電子郵件地址相關聯的合作物件。 帳戶管理員須負責支付訂用帳戶資源所產生的所有成本。
- **Azure Active Directory (Azure AD) ：** Microsoft 雲端式身分識別與存取管理服務。 Azure AD 可允許您的員工登入和存取資源。
- **Azure AD 租使用者：** Azure AD 的專用且受信任的實例。 當您的組織第一次註冊 Microsoft 雲端服務訂用帳戶（例如 Microsoft Azure、Intune 或 Microsoft 365）時，就會自動建立 Azure AD 租使用者。 一個 Azure 租用戶代表一個組織。
- **Azure AD 目錄：** 每個 Azure AD 租使用者都有單一、專用且受信任的目錄。 目錄包含租使用者的使用者、群組和應用程式。 此目錄可用來執行身分識別和存取租用戶資源的管理功能。 目錄可以與多個訂用帳戶相關聯，但每個訂用帳戶僅能與一個目錄相關聯。
- **資源群組：** 您用來將訂用帳戶中的相關資源分組的邏輯容器。 每個資源只能存在於一個資源群組中。 資源群組允許在訂用帳戶內進行更細微的分組，通常用來代表在訂用帳戶內支援工作負載、應用程式或特定功能所需的資產集合。
- **管理群組：** 您用於一或多個訂閱的邏輯容器。 您可以定義管理群組、訂用帳戶、資源群組和資源的階層，透過繼承來有效率地管理存取、原則和合規性。
- **區域：** 在延遲定義的周邊內部署的一組 Azure 資料中心。 資料中心會透過專用且低延遲的區域網路進行連線。 大部分 Azure 資源都是在特定的 Azure 區域中執行。

## <a name="azure-subscription-purposes"></a>Azure 訂用帳戶用途

Azure 訂用帳戶有數個用途。 Azure 訂用帳戶可以是：

- **法律合約。** 每個訂用帳戶都與 [Azure 供應](https://azure.microsoft.com/support/legal/offer-details)專案相關聯，例如免費試用或隨用隨付。 每個供應項目都有特定的費率方案、優點和相關聯的條款及條件。 您可以在建立訂用帳戶時，選擇 Azure 供應項目。
- **付款合約。** 當您建立訂用帳戶時，您會為該訂用帳戶提供付款資訊，例如信用卡號碼。 資源部署到該訂用帳戶上時產生的每月成本，會透過該付款方式計算並計費。
- **尺規的界限。** 您可以對訂用帳戶定義規模限制。 訂用帳戶的資源不能超過設定的調整限制。 例如，限制單一訂用帳戶中可建立的虛擬機器數目。
- **系統管理界限。** 訂用帳戶可以作為系統管理、安全性和原則的界限。 Azure 也會提供其他機制來滿足這些需求，例如管理群組、資源群組和角色型存取控制。

## <a name="azure-subscription-considerations"></a>Azure 訂用帳戶考量

當您建立 Azure 訂用帳戶時，您會對訂用帳戶進行數個重要的選擇：

- **誰負責支付訂用帳戶的費用？** 依預設，當您建立訂用帳戶時所提供的電子郵件地址相關聯的合作物件是訂用帳戶的帳戶管理員。 與此電子郵件地址相關聯的合作物件會負責支付訂用帳戶資源所產生的所有成本。
- **我該關注哪一個 Azure 供應項目？** 每個訂用帳戶都會與特定的 [Azure 供應項目](https://azure.microsoft.com/support/legal/offer-details)相關聯。 您可以選擇最符合您需求的 Azure 供應項目。 例如，如果您想要使用訂用帳戶來執行非生產工作負載，您可以選擇隨用隨付開發/測試供應專案或 Enterprise 開發/測試供應專案。

> [!NOTE]
> 當您註冊 Azure 時，您可能會看到「建立 Azure 帳戶」的語句。 當您建立 Azure 訂用帳戶並將訂用帳戶與電子郵件帳戶建立關聯時，您就會建立 Azure 帳戶。

## <a name="azure-administrative-roles"></a>Azure 管理角色

Azure 會定義三種類型的角色來管理訂用帳戶、身分識別和資源：

- 傳統訂用帳戶管理員角色
- Azure 角色型存取控制 (RBAC) 角色
- Azure Active Directory (Azure AD) 管理員角色

Azure 訂用帳戶的帳戶管理員角色會指派給建立 Azure 訂用帳戶時所用的電子郵件帳戶。 帳戶管理員是訂用帳戶的帳單擁有者。 帳戶管理員可以透過 Azure 入口網站 [管理訂](/azure/cost-management-billing/manage/add-change-subscription-administrator) 用帳戶管理員。

依預設，訂用帳戶的服務系統管理員角色也會指派給用來建立 Azure 訂用帳戶的電子郵件帳戶。 服務系統管理員具有等同于 RBAC 型擁有者角色的訂用帳戶許可權。 服務管理員也有 Azure 入口網站的完整存取權。 帳戶管理員可以將服務管理員變更為不同的電子郵件帳戶。

當您建立 Azure 訂用帳戶時，您可以將訂用帳戶與現有的 Azure AD 租用戶建立關聯。 否則，會建立具有相關聯目錄的新 Azure AD 租用戶。 Azure AD 目錄中的全域管理員角色會指派給用來建立 Azure AD 訂用帳戶的電子郵件帳戶。

電子郵件帳戶可以與多個 Azure 訂用帳戶相關聯。 帳戶管理員可以將訂用帳戶轉移到另一個帳戶。

如需 Azure 中定義的角色詳細說明，請參閱[傳統訂用帳戶管理員角色、Azure RBAC 角色和 Azure AD 管理員角色](/azure/role-based-access-control/rbac-and-directory-admin-roles)。

## <a name="subscriptions-and-regions"></a>訂用帳戶和區域

每個 Azure 資源都只會與一個訂用帳戶建立邏輯關聯。 當您建立資源時，您可以選擇要將該資源部署到哪個 Azure 訂用帳戶。 您之後可以將資源移至另一個訂用帳戶。

當訂用帳戶未系結至特定的 Azure 區域時，每個 Azure 資源都會部署到單一區域。 您可以在多個區域中擁有與相同訂用帳戶相關聯的資源。

> [!NOTE]
> 大部分的 Azure 資源都會部署到特定區域。 某些資源類型會被視為全域資源，例如您使用 Azure 原則服務所設定的原則。

## <a name="related-resources"></a>相關資源

下列資源會提供本文所述概念的詳細資訊：

- [Azure 如何運作？](../../get-started/what-is-azure.md)
- [Azure 中的資源存取管理](../../govern/resource-consistency/resource-access-management.md)
- [Azure Resource Manager 概觀](/azure/azure-resource-manager/management/overview)
- [Azure 資源的角色型存取控制 (RBAC)](/azure/role-based-access-control/overview)
- [什麼是 Azure Active Directory？](/azure/active-directory/fundamentals/active-directory-whatis)
- [將 Azure 訂用帳戶關聯或新增至您的 Azure Active Directory 租用戶](/azure/active-directory/fundamentals/active-directory-how-subscriptions-associated-directory)
- [Azure AD Connect 的拓撲](/azure/active-directory/hybrid/plan-connect-topologies)
- [Microsoft 雲端供應項目的訂閱、授權、帳戶和租用戶](/office365/enterprise/subscriptions-licenses-accounts-and-tenants-for-microsoft-cloud-offerings)

## <a name="next-steps"></a>後續步驟

現在您已了解基本的 Azure 概念，您可以接著了解如何使用多個 Azure 訂用帳戶進行調整。

> [!div class="nextstepaction"]
> [使用多個 Azure 訂用帳戶進行調整](../azure-best-practices/scale-subscriptions.md)
