---
title: Azure 中的身分識別基準工具
description: 瞭解 Azure 原生工具如何協助支援身分識別基準專業領域的成熟原則和流程。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: internal
ms.openlocfilehash: 0ff9bb28035982564accac23c15cec1fe9133c47
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101792443"
---
# <a name="identity-baseline-tools-in-azure"></a>Azure 中的身分識別基準工具

身分 [識別基準專業領域](./index.md) 是 [雲端治理的五個專業領域](../governance-disciplines.md)之一。 此專業領域著重在建立原則的方式，無論裝載應用程式或工作負載的雲端提供者為何，都會確保使用者身分識別的一致性和持續性。

下列工具組含在混合式身分識別的探索指南中。

**(內部部署) 的 Active Directory：** Active Directory 是企業中最常使用的身分識別提供者，用以儲存及驗證使用者認證。

**Azure Active Directory：** 「軟體即服務」 (SaaS) 相當於 Active Directory，能夠與內部部署 Active Directory 進行同盟。

**Active Directory (IaaS) ：** 在 Azure 虛擬機器中執行的 Active Directory 應用程式實例。

身分識別是 IT 安全性的控制平面。 因此，驗證是組織對雲端的存取保護。 組織需要身分識別控制平面來增強其安全性，並讓其雲端應用程式免于受到入侵者的攻擊。

## <a name="cloud-authentication"></a>雲端驗證

選擇正確的驗證方法是組織想要將其應用程式移至雲端的第一項考慮。

當您選擇此方法時，Azure AD 會處理使用者的登入流程。 結合無縫單一登入 (SSO) ，使用者可以登入雲端應用程式，而不必重新輸入其認證。 若使用雲端驗證，有兩個選項可供您選擇：

**AZURE AD 密碼雜湊同步處理：** 在 Azure AD 中啟用內部部署目錄物件驗證的最簡單方式。 此方法也可與任何方法搭配使用，以便在您的內部部署伺服器當機時，作為備份容錯移轉驗證方法。

**AZURE AD 傳遞驗證：** 使用在一或多部內部部署伺服器上執行的軟體代理程式，為 Azure AD 驗證服務提供持續性密碼驗證。

<!-- docutune:casing "the pass-through authentication method" -->

> [!NOTE]
> 若公司具有立即強制執行內部部署使用者帳戶狀態、密碼原則及登入時數的安全性需求，則應該考慮使用傳遞驗證方法。

**同盟驗證：**

當您選擇此方法時，Azure AD 會將驗證程式傳遞給個別的受信任驗證系統（例如內部部署 Active Directory Federation Services (AD FS) 或受信任的協力廠商同盟提供者），以驗證使用者的密碼。

針對可協助您為組織選擇最佳解決方案的決策樹，請參閱 [為 Azure Active Directory 選擇正確的驗證方法](/azure/active-directory/hybrid/choose-ad-authn)。

下表列出的原生工具可協助您成熟支援此專業領域的原則和流程。

<!-- docutune:casing UserPrincipalName SamAccountName "conditional access options" -->

| 考量 | 密碼雜湊同步處理 + 無縫 SSO | 傳遞驗證 + 無縫 SSO | 與 AD FS 同盟 |
| --- | --- | --- | --- |
| 驗證的發生位置？ | 在雲端 | 在雲端中，安全密碼驗證與內部部署驗證代理程式交換之後 | 內部部署 |
| 高於佈建系統的內部部署伺服器需求是什麼：Azure AD Connect？ | None | 每個額外的驗證代理程式需要 1 部伺服器 | 2 部以上的 AD FS 伺服器 <br><br> 周邊網路中有兩部或多部 WAP 伺服器 |
| 除了提供系統之外，內部部署網際網路和網路的需求為何？ | 無 | 從執行驗證代理程式的伺服器[輸出網際網路存取](/azure/active-directory/hybrid/how-to-connect-pta-quick-start) | 周邊中 WAP 伺服器的[輸入網際網路存取](/windows-server/identity/ad-fs/overview/ad-fs-requirements) <br><br> 來自周邊 WAP 伺服器對 AD FS 伺服器的輸入網際網路存取 <br><br> 網路負載平衡 |
| 是否有 SSL 憑證需求？ | 否 | 否 | 是 |
| 是否有健康情況監視解決方案？ | 不需要 | [Azure Active Directory 系統管理中心](/azure/active-directory/hybrid/tshoot-connect-pass-through-authentication#general-issues)提供的代理程式狀態 | [Azure AD Connect Health](/azure/active-directory/hybrid/how-to-connect-health-adfs) |
| 使用者是否可以從公司網路中已加入網域的裝置中取得雲端資源的單一登入？ | 是，使用[無縫 SSO](/azure/active-directory/hybrid/how-to-connect-sso) | 是，使用[無縫 SSO](/azure/active-directory/hybrid/how-to-connect-sso) | 是 |
| 支援何種登入類型？ | UserPrincipalName + 密碼 <br><br> 使用[無縫 SSO](/azure/active-directory/hybrid/how-to-connect-sso)的整合式 Windows 驗證 <br><br> [替代登入識別碼](/azure/active-directory/hybrid/how-to-connect-install-custom) | UserPrincipalName + 密碼 <br><br> 使用[無縫 SSO](/azure/active-directory/hybrid/how-to-connect-sso)的整合式 Windows 驗證 <br><br> [替代登入識別碼](/azure/active-directory/hybrid/how-to-connect-pta-faq) | UserPrincipalName + 密碼 <br><br> SamAccountName + 密碼 <br><br> 整合式 Windows 驗證 <br><br> [憑證和智慧卡驗證](/windows-server/identity/ad-fs/operations/configure-user-certificate-authentication) <br><br> [替代登入識別碼](/windows-server/identity/ad-fs/operations/configuring-alternate-login-id) |
| 是否支援 Windows Hello 企業版？ | [金鑰信任模型](/windows/security/identity-protection/hello-for-business/hello-identity-verification) <br><br> [使用 Intune 的憑證信任模型](https://microscott.azurewebsites.net/2017/12/16/setting-up-windows-hello-for-business-with-intune/) | [金鑰信任模型](/windows/security/identity-protection/hello-for-business/hello-identity-verification) <br><br> [使用 Intune 的憑證信任模型](https://microscott.azurewebsites.net/2017/12/16/setting-up-windows-hello-for-business-with-intune/) | [金鑰信任模型](/windows/security/identity-protection/hello-for-business/hello-identity-verification) <br><br> [憑證信任模型](/windows/security/identity-protection/hello-for-business/hello-key-trust-adfs) |
| 多重要素驗證選項有哪些？ | [Azure Multi-Factor Authentication](/azure/active-directory/authentication/) <br><br> [使用 Azure AD 條件式存取的自訂控制項 *](/azure/active-directory/conditional-access/controls#custom-controls-preview) | [Azure Multi-Factor Authentication](/azure/active-directory/authentication/) <br><br> [使用 Azure AD 條件式存取的自訂控制項 *](/azure/active-directory/conditional-access/controls#custom-controls-preview) | [Azure Multi-Factor Authentication](/azure/active-directory/authentication/) <br><br> [Azure 多重要素驗證服務器](/azure/active-directory/authentication/howto-mfaserver-deploy) <br><br> [協力廠商多重要素驗證](/windows-server/identity/ad-fs/operations/configure-additional-authentication-methods-for-ad-fs) <br><br> [使用 Azure AD 存取的自訂控制項](/azure/active-directory/conditional-access/controls#custom-controls-preview) |
| 支援哪些使用者帳戶狀態？ | 停用的帳戶 <br>  (最多30分鐘的延遲)  | 停用的帳戶 <br><br> 帳戶已鎖定 <br><br> 帳戶已過期 <br><br> 密碼已過期 <br><br> 登入時數 | 停用的帳戶 <br><br> 帳戶已鎖定 <br><br> 帳戶已過期 <br><br> 密碼已過期 <br><br> 登入時數 |
| Azure AD 中的條件式存取選項有哪些？ | [Azure AD 條件式存取](/azure/active-directory/conditional-access/overview) | [Azure AD 條件式存取](/azure/active-directory/conditional-access/overview) | [Azure AD 條件式存取](/azure/active-directory/conditional-access/overview) <br><br> [AD FS 宣告規則](https://adfshelp.microsoft.com/AadTrustClaims/ClaimsGenerator) |
| 是否支援封鎖舊版通訊協定？ | [是](/azure/active-directory/fundamentals/concept-fundamentals-security-defaults) | [是](/azure/active-directory/fundamentals/concept-fundamentals-security-defaults) | [是](/windows-server/identity/ad-fs/operations/access-control-policies-w2k12) |
| 您是否可以自訂登入頁面上的標誌、影像和說明？ | [是，使用 Azure AD Premium](/azure/active-directory/fundamentals/customize-branding) | [是，使用 Azure AD Premium](/azure/active-directory/fundamentals/customize-branding) | [是](/azure/active-directory/hybrid/how-to-connect-fed-management#customlogo) |
| 支援哪些進階案例？ | [智慧型密碼鎖定](/azure/active-directory/authentication/concept-sspr-howitworks) <br><br> [認證外洩報告](/azure/active-directory/identity-protection/overview-identity-protection) | [智慧型密碼鎖定](/azure/active-directory/authentication/howto-password-smart-lockout) | 多網站低延遲驗證系統 <br><br> [AD FS 外部網路鎖定](/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-soft-lockout-protection) <br><br> [與第三方身分識別系統整合](/azure/active-directory/hybrid/how-to-connect-fed-compatibility) |

> [!NOTE]
> Azure AD 條件式存取中的自訂控制項目前不支援裝置註冊。

## <a name="next-steps"></a>下一步

<!-- TODO: The download button for this whitepaper returns 404. -->

<!-- docutune:casing "Hybrid Identity Digital Transformation Framework" -->

「 [混合式身分識別數位轉換架構」白皮書](https://resources.office.com/ww-landing-M365E-EMS-IDAM-Hybrid-Identity-WhitePaper.html) 概述選擇和整合這些元件的組合和解決方案。

[Azure AD Connect 工具](https://aka.ms/aadconnectwiz)可協助您整合內部部署目錄與 Azure AD。
