---
title: 身分識別和存取管理
description: 檢查與企業環境中的身分識別和存取管理相關的設計考慮和建議。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 2f2ff7392b20a10313b7629b5d4e780076a88310
ms.sourcegitcommit: 76edf563a08ff7dc81c3fc2dc6c8972ab3b4c55b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2020
ms.locfileid: "88237231"
---
# <a name="identity-and-access-management"></a>身分識別和存取管理

身分識別提供大量安全性保證的基礎。 它可以根據雲端服務中的身分識別驗證和授權控制來進行存取，以保護資料和資源，並決定應允許的要求。

身分識別和存取管理 (IAM) 是公用雲端中的界限安全性。 它必須被視為任何安全且完全相容的公用雲端架構的基礎。 Azure 提供一組完整的服務、工具和參考架構，讓組織能夠如這裡所述，建立高度安全且操作有效率的環境。

本節探討與企業環境中的 IAM 相關的設計考慮和建議。

## <a name="why-we-need-identity-and-access-management"></a>為何需要身分識別和存取管理

企業中的技術層面變得複雜且異構。 為了管理此環境的合規性和安全性，IAM 可讓正確的人員在適當的時間存取正確的資源，原因如下。

### <a name="plan-for-identity-and-access-management"></a>規劃身分識別和存取管理

企業組織通常會遵循最低許可權的操作存取方法。 此模型應透過 Azure Active Directory (Azure AD) 以角色為基礎的存取控制 (RBAC) 和自訂角色定義來加以考慮。 請務必規劃如何管理 Azure 中資源的控制和資料平面存取。 IAM 和 RBAC 的任何設計都必須符合法規、安全性和營運需求，才能接受。

身分識別與存取管理是多步驟的程式，其中牽涉到仔細規劃身分識別整合和其他安全性考慮，例如封鎖舊版驗證和規劃新式密碼。 預備規劃也牽涉到選取企業對企業或企業對消費者的身分識別與存取管理。 雖然這些需求有所不同，但還是需要考慮企業登陸區域的常見設計考慮和建議。

![顯示身分識別和存取管理的圖表。](./media/iam.png)

_圖1：身分識別和存取管理。_

**設計考慮：**

- 當您針對 IAM 和治理進行架構的版面配置時，必須考慮的自訂角色和角色指派數目有所限制。 如需詳細資訊，請參閱 [AZURE RBAC 服務限制](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#role-based-access-control-limits)。
- 每個訂用帳戶的自訂 RBAC 角色指派限制為2000。
- 每個管理群組的自訂 RBAC 角色指派限制為500。
- 集中式與同盟資源擁有權：
  - 共用資源或環境中任何執行或強制安全性界限（例如網路）的層面，都必須集中管理。 這項需求是許多法規架構的一部分。 對於授與或拒絕存取機密或重要商務資源的任何組織而言，這是標準作法。
  - 管理不違反安全性界限的應用程式資源或維護安全性和合規性所需的其他層面，都可以委派給應用程式小組。 允許使用者在安全管理的環境中布建資源，可讓組織充分利用雲端的 agile 特性，同時防止違反任何重要的安全性或治理界限。

<!-- docsTest:ignore Azure-AD-only Azure-AD-managed NetOps SecOps AppOps -->

**設計建議：**

- 在可能的情況下，使用 Azure AD [RBAC](https://docs.microsoft.com/azure/role-based-access-control/overview) 來管理對資源的資料平面存取。 範例包括 Azure Key Vault、儲存體帳戶或 SQL database。
- 針對具有 Azure 環境許可權的任何使用者，部署 Azure AD 條件式存取原則。 這麼做可提供另一種機制，協助保護受控制的 Azure 環境不受未經授權的存取。
- 針對具有 Azure 環境許可權的任何使用者，強制執行多重要素驗證 (MFA) 。 MFA 強制是許多合規性架構的需求。 它可大幅降低認證竊取和未經授權存取的風險。
- 使用 [Azure AD Privileged Identity Management (PIM) ](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure) 來建立零的存取權和最低許可權。 將貴組織的角色對應到所需的最低存取層級。 Azure AD PIM 可以是現有工具和進程的延伸模組，如有必要，請使用 Azure 原生工具，或視需要使用。
- 當您授與資源的存取權時，請為 Azure AD PIM 中的 Azure 控制平面資源使用 Azure-僅限 AD 的群組。
  - 如果群組管理系統已準備就緒，請將內部部署群組新增至 Azure AD 專用群組。
- 使用 Azure AD PIM 存取審查來定期驗證資源權利。 存取審查是許多合規性架構的一部分。 因此，許多組織都已經有處理這項需求的程式。
- 整合 Azure AD 記錄與平臺中心 [Azure 監視器](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-activity-logs-azure-monitor)。 Azure 監視器允許在 Azure 中使用記錄和監視資料的單一事實來源，這可為組織提供雲端原生選項，以符合記錄收集和保留方面的需求。
- 如果有任何資料主權需求，可以部署自訂使用者原則來強制執行。
- 當您考慮下列重要角色時，請使用 Azure AD 租使用者內的自訂 RBAC 角色定義：

| 角色 | 使用方式 | 動作 | 沒有動作 |
|---|---|---|---|
| Azure 平臺擁有者               | 管理群組和訂用帳戶生命週期管理                                                           | `*`                                                                                                                                                                                                                  |                                                                                                                                                                                         |
| 網路管理 (NetOps)         | 整個平臺的全球連線管理：虛擬網路、Udr、Nsg、Nva、VPN、Azure ExpressRoute 等等            | `*/read`, `Microsoft.Authorization/*/write`, `Microsoft.Network/vpnGateways/*`, `Microsoft.Network/expressRouteCircuits/*`, `Microsoft.Network/routeTables/write`, `Microsoft.Network/vpnSites/*`                              |                                                                                                                                                                               |
|  (SecOps) 的安全性作業       | 在整個 Azure 資產和 Azure Key Vault 清除原則中，以水準視圖安全性系統管理員角色 | `*/read`, `*/register/action`, `Microsoft.KeyVault/locations/deletedVaults/purge/action`, `Microsoft.Insights/alertRules/*`, `Microsoft.Authorization/policyDefinitions/*`, `Microsoft.Authorization/policyAssignments/*`, `Microsoft.Authorization/policySetDefinitions/*`, `Microsoft.PolicyInsights/*`, `Microsoft.Security/*` |                                                                            |
| 訂用帳戶擁有者                 | 衍生自訂用帳戶擁有者角色之訂用帳戶擁有者的委派角色                                       | `*`                                                                                                                                                                                                                  | `Microsoft.Authorization/*/write`, `Microsoft.Network/vpnGateways/*`, `Microsoft.Network/expressRouteCircuits/*`, `Microsoft.Network/routeTables/write`, `Microsoft.Network/vpnSites/*` |
| 應用程式擁有者 (DevOps/AppOps)  | 已針對資源群組層級的應用程式/作業小組授與參與者角色                                 |                                                                                                                                                                                                                    | `Microsoft.Network/publicIPAddresses/write`, `Microsoft.Network/virtualNetworks/write`, `Microsoft.KeyVault/locations/deletedVaults/purge/action`                                         |

- 針對所有基礎結構即服務 (IaaS) 資源，使用 Azure 資訊安全中心的即時存取，以啟用對 IaaS 虛擬機器之暫時使用者存取的網路層級保護。
- 使用適用于 Azure 資源的 Azure AD 受控識別，以避免以使用者名稱和密碼為基礎的驗證。 由於公用雲端資源的許多安全性缺口源自內嵌在程式碼或其他文字來源的認證竊取，因此強制執行以程式設計方式存取的受控識別，可大幅降低認證遭竊的風險。
- 針對需要更高存取權限的自動化 runbook，使用特殊許可權的身分識別。 違反重要安全性界限的自動化工作流程，應該受相同的工具和原則使用者對等許可權的規範。
- 不要將使用者直接新增至 Azure 資源範圍。 這不是集中式管理，會大幅增加保護所需的管理，以防止未經授權存取受限制的資料。

### <a name="plan-for-authentication-inside-a-landing-zone"></a>規劃登陸區域內的驗證

採用 Azure 時，企業組織必須進行的重要設計決策是要將現有的內部部署身分識別網域擴充至 Azure 或建立全新的身分。 應徹底評估登陸區域內的驗證需求，並將其納入計畫中，以在 Windows server、Azure AD Domain Services (Azure AD DS) 或兩者中部署 Active Directory Domain Services (AD DS) 。 大部分的 Azure 環境至少會使用 Azure 網狀架構驗證的 Azure AD，以及 AD DS 本機主機驗證和群組原則管理。

**設計考慮：**

- 請考慮集中式和委派的責任，以管理在登陸區域內部署的資源。
- 依賴網域服務和使用較舊通訊協定的應用程式可以使用 [AZURE AD DS](https://docs.microsoft.com/azure/active-directory-domain-services)。

**設計建議：**

- 根據角色和安全性需求，使用集中式和委派的責任來管理部署于登陸區域內的資源。
- 特殊許可權作業（例如建立服務主體物件、在 Azure AD 中註冊應用程式，以及採購和處理憑證或萬用字元憑證）都需要特殊許可權。 請考慮哪些使用者將處理這類要求，以及如何根據所需的努力來保護和監視其帳戶。
- 如果組織有一個案例，其中使用整合式 Windows 驗證的應用程式必須透過 Azure AD 從遠端存取，請考慮使用 [Azure AD 應用程式 Proxy](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy)。
- 在 Windows server 上執行的 Azure AD、Azure AD DS 和 AD DS 之間有差異。 評估您的應用程式需求，並瞭解並記錄每個使用者將使用的驗證提供者。 針對所有應用程式進行相應的規劃。
- 評估 Windows server 和 Azure AD DS 上 AD DS 工作負載的相容性。
- 確定您的網路設計允許 Windows server 上需要 AD DS 的資源進行本機驗證和管理，以存取適當的網域控制站。
  - 對於 Windows server 上的 AD DS，請考慮在較大的企業級網路內容中提供本機驗證和主機管理的共用服務環境。
- 在主要區域中部署 Azure AD DS，因為此服務只能投射到一個訂用帳戶。
- 使用受控識別，而不是服務主體來驗證 Azure 服務。 這種方法可減少認證竊取的風險。
