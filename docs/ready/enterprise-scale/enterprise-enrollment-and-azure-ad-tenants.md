---
title: 企業註冊和 Azure Active Directory 租使用者
description: Enterprise 註冊和 Azure Active Directory 租使用者。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 3c894f04c071c9d6bec981b54671e72be1f053d9
ms.sourcegitcommit: 4bbd5f6444d033ef1f38dc6f3bad7b914a82f68f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/08/2020
ms.locfileid: "86128448"
---
# <a name="enterprise-enrollment-and-azure-active-directory-tenants"></a>企業註冊和 Azure Active Directory 租使用者

## <a name="planning-for-enterprise-enrollment"></a>規劃企業註冊

企業註冊（通常稱為 Enterprise 合約（EA））代表 Microsoft 與您的組織如何使用 Azure 之間的商業關係。 它提供您所有訂用帳戶的計費基礎，並影響您的資產管理。 Enterprise 註冊（也稱為 EA）是透過 Azure 企業版入口網站來管理。 Azure 企業版註冊通常代表組織的階層，包括部門、帳戶和訂用帳戶。 此階層代表組織內的成本註冊群組。

![Azure EA 階層 ](./media/ea.png)
 _圖1： azure 企業版註冊階層。_

- 部門有助於將成本分割成邏輯群組，並在部門層級設定預算或配額（注意：配額不會受到嚴格的強制，並用於報告用途）。

- 帳戶是 Azure 企業版入口網站中的組織單位;可以用來管理訂閱和存取報表。

- 訂用帳戶是 Azure 企業版入口網站中的最小單位。 這些訂用帳戶是由服務管理員所管理的 Azure 服務容器。 這些是您的組織部署 Azure 服務的地方。

- 企業註冊角色會連結具有其功能角色的使用者。 這些角色包括：
  - 企業系統管理員
  - 部門系統管理員
  - 帳戶擁有者
  - 服務管理員
  - 通知連絡人

**設計考慮：**

- 註冊提供階層式組織結構來管理訂用帳戶的管理。

- 可以在 EA 帳戶層級分隔多個環境，以支援整體隔離。

- 可以有多個系統管理員被視為單一註冊。

- 每個訂用帳戶都必須有相關聯的帳戶擁有者。

- 每個帳戶擁有者都會成為在該帳戶下布建之任何訂用帳戶的訂閱擁有者。

- 訂用帳戶在任何指定時間只能屬於一個帳戶。

- 訂用帳戶可以根據一組指定的準則來暫停。

**設計建議：**

- 請只將驗證類型用於 `Work or school account` 所有帳戶類型。 避免使用 `Microsoft account (MSA)` 帳戶類型。

- 設定通知連絡人的電子郵件地址，以確保通知會傳送至適當的群組信箱。

- 為每個帳戶指派預算，並建立與預算相關聯的警示。

- 組織可以有多種結構，例如功能、部門、地理、矩陣或小組結構。 使用組織結構將組織結構對應至 enterprise 註冊。

- 如果商務網域具有獨立的 IT 功能，請為 IT 建立新的部門。

- 限制並將註冊內的帳戶擁有者數目降至最低，以避免系統管理員對訂用帳戶和相關聯的 Azure 資源的存取權激增。

- 如果使用多個 Azure Active Directory （Azure AD）租使用者，請確認帳戶擁有者與帳戶的訂閱已布建所在的同一個租使用者相關聯。

- 在 EA 帳戶層級設定企業開發/測試/生產環境，以支援全面隔離。

- 請勿忽略傳送至通知帳戶電子郵件地址的通知電子郵件。 Microsoft 會將重要的 EA 範圍通訊傳送到此帳戶。

- 請勿移動或重新命名 Azure AD 中的 EA 帳戶。

- 定期審核 EA 入口網站，以審查誰有權存取，並盡可能避免使用 Microsoft 帳戶。

## <a name="define-azure-ad-tenants"></a>定義 Azure AD 租使用者

Azure AD 租使用者會提供身分識別和存取管理，這是安全性狀態的重要部分，以確保已驗證和授權的使用者只能存取他們具有存取權限的資源。 Azure AD 不僅會將這些服務提供給部署在 Azure 中的應用程式和服務，也會提供給 Azure 外部部署的服務和應用程式（例如內部部署或協力廠商雲端提供者）。 軟體即服務（SaaS）應用程式也會使用 Azure AD，例如 Microsoft 365 和 Azure Marketplace。 已經使用內部部署 Active Directory 的組織可以使用現有的基礎結構，並藉由與 Azure AD 整合，將驗證延伸到雲端。 每個 Azure AD 目錄都有一或多個網域。 一個目錄可以有多個與其相關聯的訂用帳戶，但只能有一個 Azure AD 的租使用者。

請務必在 Azure AD 設計階段詢問基本的安全性問題，例如您的組織如何管理認證，以及它如何控制人為、應用程式，以及以程式設計方式存取。

**設計考慮：**

- 多個 Azure AD 租使用者可以在同一個企業註冊中運作。

**設計建議：**

- 根據選取的[規劃拓撲](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-topologies)，使用 Azure AD 無縫單一登入。

- 如果您的組織沒有身分識別基礎結構，請從執行僅限 Azure AD 的身分識別部署開始。 [Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services)和[Microsoft Enterprise Mobility + Security](https://docs.microsoft.com/mem/intune/fundamentals/what-is-intune)的這類部署提供 SaaS 應用程式、企業應用程式和裝置的端對端保護。

- 多重要素驗證提供另一層的安全性和第二個驗證屏障。 針對所有特殊許可權帳戶強制執行[多重要素驗證](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks)和[條件式存取原則](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)，以獲得更高的安全性。

- 針對[緊急存取](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access)或全形帳戶進行規劃和實行，以防止整個租使用者帳戶鎖定。

- 使用[Azure AD Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure)進行身分識別和存取管理。

- 如果要將開發/測試/生產環境從身分識別的觀點來隔離，請透過多個租使用者將它們分隔在租使用者層級。

- 請避免建立新的 Azure AD 租使用者，除非有強身份識別和存取管理的理由，而且已有進程。
