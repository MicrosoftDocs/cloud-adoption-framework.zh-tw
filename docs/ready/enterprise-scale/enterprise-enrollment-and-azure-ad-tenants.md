---
title: Enterprise 合約註冊和 Azure Active Directory 租用戶
description: Enterprise 註冊和 Azure Active Directory 租使用者。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: fcdf1aff4fa57427cd2c6b839b6d7e843a6c57f2
ms.sourcegitcommit: 57b757759b676a22f13311640b8856557df36581
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2020
ms.locfileid: "94994865"
---
# <a name="enterprise-agreement-enrollment-and-azure-active-directory-tenants"></a>Enterprise 合約註冊和 Azure Active Directory 租用戶

## <a name="plan-for-enterprise-enrollment"></a>規劃 enterprise 註冊

註冊 Enterprise 合約 (EA) 代表 Microsoft 與您的組織如何使用 Azure 之間的商業關係。 它提供您所有訂用帳戶的計費基礎，並影響您的數位資產管理。 您的 EA 註冊可透過 Azure EA 入口網站進行管理。 註冊通常代表組織的階層，其中包括部門、帳戶和訂用帳戶。 此階層代表組織內的成本註冊群組。

![顯示 Azure EA 階層的圖表。](./media/ea.png)

_圖1： Azure EA 註冊階層。_

- 部門有助於將成本分割成邏輯群組，以及設定部門層級的預算或配額。 配額並不會強制執行，而是用於報告用途。
- 帳戶是 Azure EA 入口網站中的組織單位， 可以用來管理訂閱和存取報表。
- 訂用帳戶是 Azure EA 入口網站中的最小單位。 它們是由服務管理員管理之 Azure 服務的容器。 它們是您的組織部署 Azure 服務的位置。
- EA 註冊角色會連結使用者與其功能角色。 這些角色包括：
  - 企業系統管理員
  - 部門系統管理員
  - 帳戶擁有者
  - 服務管理員
  - 通知連絡人

**設計考慮：**

- 註冊提供階層式的組織結構來控管訂用帳戶的管理。
- 可以在 EA 帳戶層級分隔多個環境，以支援全面隔離。
- 可以將多個管理員指定至單一註冊。
- 每個訂用帳戶都必須有相關聯的帳戶擁有者。
- 每個帳戶擁有者都會成為該帳戶所布建之任何訂用帳戶的訂用帳戶擁有者。
- 訂用帳戶在任何指定的時間都只能屬於一個帳戶。
- 可以根據一組指定的準則將訂用帳戶暫止。

**設計建議：**

- 只使用 `Work or school account` 所有帳戶類型的驗證類型。 請避免使用 `Microsoft account (MSA)` 帳戶類型。
- 設定通知連絡人電子郵件地址，以確保會將通知傳送到適當的群組信箱。
- 指派每個帳戶的預算，並建立與預算相關聯的警示。
- 組織可以有各種不同的結構，例如功能、分部、地理、總體或小組結構。 使用組織結構將您的組織結構對應至您的註冊階層。
- 如果商務網域有獨立的 IT 功能，請為 IT 建立新的部門。
- 限制並減少註冊內的帳戶擁有者數目，以避免系統管理員對訂用帳戶和相關 Azure 資源的存取權激增。
- 如果使用多個 Azure Active Directory (Azure AD) 的租使用者，請確認帳戶擁有者與帳戶的訂用帳戶布建所在的租使用者相關聯。
- 在 EA 帳戶層級設定企業開發/測試和實際執行環境，以支援全面隔離。
- 不要忽略傳送至通知帳戶電子郵件地址的通知電子郵件。 Microsoft 會將全 EA 的重要通訊傳送到此帳戶。
- 請勿移動或重新命名 Azure AD 中的 EA 帳戶。
- 定期審核 EA 入口網站，以檢查誰有存取權，並盡可能避免使用 Microsoft 帳戶。

## <a name="define-azure-ad-tenants"></a>定義 Azure AD 租使用者

Azure AD 租用戶提供身分識別與存取權管理，這是安全性狀態的重要部分。 Azure AD 租用戶可確保已驗證和授權的使用者只能存取他們具有存取權限的資源。 Azure AD 會將這些服務提供給 Azure 中部署的應用程式和服務，也會提供給部署在 Azure 外部 (例如內部部署或協力廠商雲端提供者) 的服務和應用程式。

「軟體即服務」應用程式 (例如 Microsoft 365、Azure Marketplace) 也會使用 Azure AD。 已經使用內部部署 Active Directory 的組織可以使用現有的基礎結構，並藉由與 Azure AD 整合，將驗證延伸到雲端。 每個 Azure AD 目錄都有一或多個網域。 一個目錄可以有多個與其相關聯的訂用帳戶，但只能有一個 Azure AD 租用戶。

在 Azure AD 設計階段詢問基本的安全性問題，例如您的組織如何管理認證，以及如何控制人員、應用程式、寫在程式碼中的存取。

**設計考慮：**

- 多個 Azure AD 租用戶可以在相同的註冊中運作。

**設計建議：**

- 根據選取的 [規劃拓撲](/azure/active-directory/hybrid/plan-connect-topologies)使用 Azure AD 的無縫單一登入。
- 如果您的組織沒有身分識別基礎結構，請從實作僅限 Azure AD 的身分識別部署開始。 [Azure AD Domain Services](/azure/active-directory-domain-services)和[Microsoft Enterprise Mobility + Security](/mem/intune/fundamentals/what-is-intune)這類部署可為 SaaS 應用程式、企業應用程式和裝置提供端對端的保護。
- 多重要素驗證提供另一層的安全性和第二個驗證屏障。 針對所有具特殊許可權的帳戶強制執行 [多重要素驗證](/azure/active-directory/authentication/concept-mfa-howitworks) 和 [條件式存取原則](/azure/active-directory/conditional-access/overview) ，以獲得更高的安全性。
- 規劃和實行 [緊急存取](/azure/active-directory/users-groups-roles/directory-emergency-access) 或半透明帳戶，以防止整個租使用者的帳戶鎖定。
- 使用 [Azure AD Privileged Identity Management](/azure/active-directory/privileged-identity-management/pim-configure) 進行身分識別和存取管理。
- 如果要從身分識別的觀點將開發/測試和實際執行環境隔離，請透過多個租用戶在租用戶層級將它們分開。
- 避免建立新的 Azure AD 租用戶，除非有身分識別與存取權管理方面強而有力的理由，而且流程已經到位。
