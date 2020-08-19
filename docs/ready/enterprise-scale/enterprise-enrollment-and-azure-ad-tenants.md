---
title: Enterprise 合約註冊和 Azure Active Directory 租使用者
description: Enterprise 註冊和 Azure Active Directory 租使用者。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 8deee8d727fea6fd1f8f1ee43027cf72919871d2
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88574681"
---
# <a name="enterprise-agreement-enrollment-and-azure-active-directory-tenants"></a>Enterprise 合約註冊和 Azure Active Directory 租使用者

## <a name="plan-for-enterprise-enrollment"></a>規劃 enterprise 註冊

Enterprise 合約 (EA) 註冊代表 Microsoft 之間的商業關係，以及您的組織如何使用 Azure。 它提供在所有訂用帳戶中計費的基礎，並影響您的數位資產管理。 您的 EA 註冊可透過 Azure 企業版入口網站來管理。 註冊通常代表組織的階層，其中包括部門、帳戶和訂用帳戶。 此階層代表組織內的成本註冊群組。

![顯示 Azure EA 階層的圖表。](./media/ea.png)

_圖1： Azure EA 註冊階層。_

- 部門有助於將成本分割成邏輯群組，以及設定部門層級的預算或配額。 配額不會以固定的強制執行，而且會用於報告用途。
- 帳戶是 Azure 企業版入口網站中的組織單位。 可以用來管理訂閱和存取報表。
- 訂用帳戶是 Azure 企業版入口網站中的最小單位。 這些訂用帳戶是由服務管理員所管理的 Azure 服務容器。 它們是您的組織部署 Azure 服務的位置。
- EA 註冊角色會連結使用者及其功能角色。 這些角色包括：
  - 企業系統管理員
  - 部門系統管理員
  - 帳戶擁有者
  - 服務管理員
  - 通知連絡人

**設計考慮：**

- 註冊可提供階層式組織結構來管理訂用帳戶的管理。
- 您可以在 EA 帳戶層級分隔多個環境，以支援全面隔離。
- 可能會有多個系統管理員被視為單一註冊。
- 每個訂用帳戶都必須有相關聯的帳戶擁有者。
- 每個帳戶擁有者都會成為該帳戶所布建之任何訂用帳戶的訂用帳戶擁有者。
- 訂用帳戶在任何指定時間只能屬於一個帳戶。
- 訂用帳戶可以根據一組指定的準則來暫停。

**設計建議：**

- 只使用 `Work or school account` 所有帳戶類型的驗證類型。 請避免使用 `Microsoft account (MSA)` 帳戶類型。
- 設定通知連絡人電子郵件地址，以確保會將通知傳送到適當的群組信箱。
- 為每個帳戶指派預算，並建立與預算相關聯的警示。
- 組織可以有各種不同的結構，例如功能、部門、地理、矩陣或團隊結構。 使用組織結構將組織結構對應到您的註冊階層。
- 如果商務網域具有獨立的 IT 功能，請為 IT 建立新的部門。
- 限制並減少註冊內的帳戶擁有者數目，以避免系統管理員對訂用帳戶和相關 Azure 資源的存取權激增。
- 如果使用多個 Azure Active Directory (Azure AD) 的租使用者，請確認帳戶擁有者與帳戶的訂用帳戶布建所在的租使用者相關聯。
- 在 EA 帳戶層級設定企業開發/測試和生產環境，以支援全面隔離。
- 請勿忽略傳送給通知帳戶電子郵件地址的通知電子郵件。 Microsoft 會將重要的 EA 範圍通訊傳送至此帳戶。
- 請勿在 Azure AD 中移動或重新命名 EA 帳戶。
- 定期審核 EA 入口網站，以檢查誰有存取權，並盡可能避免使用 Microsoft 帳戶。

## <a name="define-azure-ad-tenants"></a>定義 Azure AD 租使用者

Azure AD 租使用者提供身分識別和存取管理，這是安全性狀態的重要部分。 Azure AD 租使用者可確保已驗證和已授權的使用者只能存取他們具有存取權限的資源。 Azure AD 將這些服務提供給部署在 Azure 中的應用程式和服務，以及部署在 Azure (外部的服務和應用程式，例如內部部署或協力廠商雲端提供者) 。

軟體即服務應用程式（例如 Microsoft 365 和 Azure Marketplace）也會使用 Azure AD。 已使用內部部署 Active Directory 的組織可以使用其現有的基礎結構，並藉由與 Azure AD 整合來將驗證延伸至雲端。 每個 Azure AD 目錄都有一個或多個網域。 一個目錄可以有多個相關聯的訂用帳戶，但只能有一個 Azure AD 的租使用者。

在 Azure AD 設計階段時詢問基本安全性問題，例如您的組織如何管理認證，以及它如何控制人為、應用程式和程式設計存取。

**設計考慮：**

- 多個 Azure AD 租使用者可在相同的註冊中運作。

**設計建議：**

- 根據選取的 [規劃拓撲](/azure/active-directory/hybrid/plan-connect-topologies)使用 Azure AD 的無縫單一登入。
- 如果您的組織沒有身分識別基礎結構，請先執行僅限 Azure AD 的身分識別部署。 [Azure AD Domain Services](/azure/active-directory-domain-services)和[Microsoft Enterprise Mobility + Security](/mem/intune/fundamentals/what-is-intune)這類部署可為 SaaS 應用程式、企業應用程式和裝置提供端對端的保護。
- 多重要素驗證可提供另一層安全性，以及第二個驗證屏障。 針對所有具特殊許可權的帳戶強制執行 [多重要素驗證](/azure/active-directory/authentication/concept-mfa-howitworks) 和 [條件式存取原則](/azure/active-directory/conditional-access/overview) ，以提高安全性。
- 規劃和實行 [緊急存取](/azure/active-directory/users-groups-roles/directory-emergency-access) 或半透明帳戶，以防止整個租使用者的帳戶鎖定。
- 使用 [Azure AD Privileged Identity Management](/azure/active-directory/privileged-identity-management/pim-configure) 進行身分識別和存取管理。
- 如果開發/測試和生產環境會從身分識別的觀點來看，請透過多個租使用者將它們與租使用者層級分開。
- 除非有強式身分識別和存取管理的理由，而且程式已就緒，否則請避免建立新的 Azure AD 租使用者。
