---
title: 身分識別和存取管理
description: 檢查與企業環境中的身分識別和存取管理相關的設計考慮和建議。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: think-tank
ms.openlocfilehash: f1d55875ca62dd78bc9840337f8ff5be49e1ae36
ms.sourcegitcommit: 86d51757bd34b49ce3b061123a6aaa8c88d3b2cc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97909441"
---
# <a name="identity-and-access-management"></a>身分識別和存取管理

身分識別提供大量安全性保證的基礎。 它可以根據雲端服務中的身分識別驗證和授權控制來啟用存取，以保護資料和資源，及判斷應該允許哪些要求。

身分識別與存取權管理 (IAM) 是公用雲端中的界限安全性。 必須將其視為任何安全且完全符合規範的公用雲端架構的基礎。 Azure 提供一組完整的服務、工具和參考架構，讓組織能夠以高度安全且有效率的方式執行環境，如下所述。

本節將探討與企業環境中 IAM 相關的設計考慮和建議。

## <a name="why-we-need-identity-and-access-management"></a>為何需要身分識別和存取管理

企業中的技術環境會變得複雜且不穩定。 為了管理此環境的合規性和安全性，IAM 讓適當的人員能夠在適當的時間存取適當的資源，以獲得正確的原因。

### <a name="plan-for-identity-and-access-management"></a>規劃身分識別與存取權管理

企業組織在操作存取上通常會遵循最低特殊權限的做法。 您應擴充此模型，以透過 Azure Active Directory (Azure AD) 、Azure 角色型存取控制 (Azure RBAC) 和自訂角色定義來考慮 Azure。 規劃如何在 Azure 中管理資源的控制和資料平面存取是很重要的。 IAM 和 Azure RBAC 的任何設計都必須符合法規、安全性和營運需求，才能接受。

身分識別與存取權管理是多步驟的程序，其中牽涉到仔細規劃身分識別整合和其他安全性考量，例如封鎖舊版驗證、規劃新式密碼。 臨時性規劃也牽涉到選擇企業對企業或企業對消費者的身分識別與存取權管理。 雖然這些需求各有不同，但還是需要考慮企業登陸區域的常見設計考量和建議。

![此圖顯示身分識別與存取管理。](./media/iam.png)

_圖1：身分識別和存取管理。_

**設計考慮：**

- 當您為 IAM 和治理制定架構時，必須考慮自訂角色和角色指派的數目限制。 如需詳細資訊，請參閱 [AZURE RBAC 服務限制](/azure/azure-resource-manager/management/azure-subscription-service-limits#azure-role-based-access-control-limits)。
- 每個訂用帳戶的角色指派限制為2000。
- 每個管理群組的角色指派限制為500。
- 集中式與同盟資源擁有權：
  - 共用資源或環境中任何實作或強制安全性界限 (例如網路) 的層面，都必須集中管理。 這項需求是許多法規架構的一部分。 對於授與或拒絕存取機密或重要商務資源的任何組織而言，這是標準做法。
  - 管理不違反安全性界限的應用程式資源，或管理其他需要維護安全性和合規性的層面，都可以委派給應用程式小組。 允許使用者在安全管理的環境中佈建資源，可讓組織充分利用雲端的靈活特性，同時防止違反任何重要的安全性或治理界限。

<!-- docutune:ignore Azure-AD-only Azure-AD-managed -->

**設計建議：**

- 您可以使用 [AZURE RBAC](/azure/role-based-access-control/overview) 來管理對資源的資料平面存取。 範例為 Azure Key Vault、儲存體帳戶或 SQL 資料庫。
- 針對具有 Azure 環境存取權限的任何使用者，部署 Azure AD 條件式存取原則。 這麼做可提供另一種機制，協助保護受控制的 Azure 環境不受未經授權的存取。
- 針對具有 Azure 環境許可權的任何使用者強制執行多重要素驗證。 強制執行多重要素驗證是許多合規性架構的要件。 它可大幅降低認證竊取和未經授權存取的風險。
- 使用 [Azure AD Privileged Identity Management (PIM) ](/azure/active-directory/privileged-identity-management/pim-configure) 來建立零的長期存取和最低許可權。 將貴組織的角色對應到所需的最低存取層級。 Azure AD PIM 可以是現有工具和程式的延伸模組、如所述使用 Azure 原生工具，或視需要使用兩者。
- 當您授與對資源的存取權時，請在 Azure AD PIM 中的 Azure 控制平面資源使用「僅限 Azure AD」的群組。
  - 如果已經有群組管理系統，請將內部部署群組新增至「僅限 Azure AD」群組。
- 使用 Azure AD PIM 存取權檢閱來定期驗證資源權利。 存取權檢閱是許多合規性架構的一部分。 如此一來，許多組織都已有程式可解決這項需求。
- 整合 Azure AD 記錄與平臺中央 [Azure 監視器](/azure/active-directory/reports-monitoring/concept-activity-logs-azure-monitor)。 Azure 監視器允許 Azure 中的記錄和監視資料使用單一事實來源，這可為組織提供雲端原生選項，以符合記錄收集和保留方面的需求。
- 如果有任何資料主權需求，可以部署自訂使用者原則來強制執行。
- 當您考慮下列金鑰角色時，請使用 Azure AD 租使用者內的自訂角色定義：

| 角色 | 使用方式 | 動作 | 沒有任何動作 |
|---|---|---|---|
| Azure 平臺擁有者 (例如內建擁有者角色)                | 管理群組和訂用帳戶生命週期管理                                                           | `*`                                                                                                                                                                                                                  |                                                                                                                                                                                         |
| 網路管理 (NetOps)         | 全平臺全球連線管理：虛擬網路、Udr、Nsg、Nva、VPN、Azure ExpressRoute 和其他            | `*/read`, `Microsoft.Network/vpnGateways/*`, `Microsoft.Network/expressRouteCircuits/*`, `Microsoft.Network/routeTables/write`, `Microsoft.Network/vpnSites/*`                              |                                                                                                                                                                               |
| 安全性作業 (SecOps)        | 在整個 Azure 資產和 Azure Key Vault 清除原則之間進行水準視圖安全性系統管理員角色 | `*/read`, `*/register/action`, `Microsoft.KeyVault/locations/deletedVaults/purge/action`, `Microsoft.Insights/alertRules/*`, `Microsoft.Authorization/policyDefinitions/*`, `Microsoft.Authorization/policyAssignments/*`, `Microsoft.Authorization/policySetDefinitions/*`, `Microsoft.PolicyInsights/*`, `Microsoft.Security/*` |                                                                            |
| 訂用帳戶擁有者                 | 衍生自訂用帳戶擁有者角色之訂用帳戶擁有者的委派角色                                       | `*`                                                                                                                                                                                                                  | `Microsoft.Authorization/*/write`, `Microsoft.Network/vpnGateways/*`, `Microsoft.Network/expressRouteCircuits/*`, `Microsoft.Network/routeTables/write`, `Microsoft.Network/vpnSites/*` |
| 應用程式擁有者 (DevOps/AppOps)  | 在資源群組層級授與應用程式/作業小組的參與者角色                                 | `*`                                                                                                                                                                                                                   | `Microsoft.Authorization/*/write`, `Microsoft.Network/publicIPAddresses/write`, `Microsoft.Network/virtualNetworks/write`, `Microsoft.KeyVault/locations/deletedVaults/purge/action`                                         |

- 在所有基礎結構即服務 (IaaS) 資源使用 Azure 資訊安全中心的 Just-In-Time 存取權，以便對 IaaS 虛擬機器的暫時性使用者存取實施網路層級保護。
- 對 Azure 資源使用 Azure AD 管理的身分識別，來避免以使用者名稱和密碼進行驗證。 因為公用雲端資源的許多安全性缺口源自於內嵌在程式碼或其他文字來源的認證竊取，對寫在程式碼中的存取強制執行受控識別，可大幅降低認證竊取的風險。
- 針對需要較高存取權限的自動化 Runbook，使用特殊權限的身分識別。 違反重要安全性界限的自動化工作流程，應該受相同的工具和原則使用者的相同許可權。
- 不要將使用者直接新增至 Azure 資源範圍。 相反地，會將使用者新增至已定義的角色，然後將這些角色指派給資源範圍。 直接使用者指派規避集中式管理，大幅增加所需的管理，以防止未經授權的資料存取受限制的資料。

### <a name="plan-for-authentication-inside-a-landing-zone"></a>規劃登陸區域內的驗證

企業組織採用 Azure 時，必須做出一個重要的設計決策：要將現有的內部部署身分識別網域擴充至 Azure 或要建立全新的身分識別網域。 企業應徹底評估登陸區域內的驗證需求，將其納入在 Windows Server、Azure AD Domain Services (Azure AD DS) 或兩者中部署 Active Directory Domain Services (AD DS) 的規劃。 大部分的 Azure 環境至少會在 Azure 網狀架構驗證、AD DS 區域主機驗證、群組原則管理上使用 Azure AD 。

**設計考慮：**

- 請考慮使用集中和委派的責任，來管理在登陸區域內部署的資源。
- 依賴網域服務和使用舊版通訊協定的應用程式，可以使用 [AZURE AD DS](/azure/active-directory-domain-services)。

**設計建議：**

- 根據角色和安全性需求，使用集中和委派的責任來管理部署在登陸區域內的資源。
- 特殊權限的作業 (例如建立服務主體物件、在 Azure AD 中註冊應用程式、採購和處理憑證或萬用字元憑證) 都需要特殊存取權限。 請考慮哪些使用者將處理這類要求，以及如何根據所需的努力來保護和監視其帳戶。
- 如果組織有使用整合式 Windows 驗證的應用程式必須透過 Azure AD 從遠端存取的案例，請考慮使用 [Azure AD 應用程式 Proxy](/azure/active-directory/manage-apps/application-proxy)。
- 在 Windows Server 上執行的 Azure AD、Azure AD DS 和 AD DS 之間有差異。 評估您的應用程式需求，瞭解並記錄每個使用者將使用的驗證提供者。 針對所有應用程式做出相應的規劃。
- 評估 Windows Server 上 AD DS 和 Azure AD DS 的工作負載的相容性。
- 確定您的網路設計可讓需要 Windows Server AD DS 的資源進行本機驗證和管理，以存取適當的網域控制站。
  - 針對 Windows Server 上的 AD DS，可考慮使用共用服務環境，此環境能在較大的整體企業網路中提供本機驗證和主機管理。
- 在主要區域中部署 Azure AD DS，因為此服務只能投射到一個訂用帳戶。
- 使用受控識別而不是服務主體來向 Azure 服務驗證身分。 這種做法可減少認證竊取的風險。
