---
title: 身分識別和存取管理
description: 檢查與企業環境中的身分識別和存取管理相關的設計考慮和建議。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 8c634942d867bdc55bba84b15ac0789a4abd27a0
ms.sourcegitcommit: bcc73d194c6d00c16ae2e3c7fb2453ac7dbf2526
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/09/2020
ms.locfileid: "86194675"
---
# <a name="identity-and-access-management"></a>身分識別和存取管理

身分識別提供大量安全性保證的基礎。 身分識別會根據雲端服務中的身分識別驗證和授權控制來啟用存取，以保護資料和資源，並決定應允許的要求。

身分識別與存取管理 (IAM) 是公用雲端中的界限安全性，而且必須視為任何安全且完全相容公用雲端架構的基礎。 Microsoft Azure 提供一組完整的服務、工具和參考架構，讓組織能夠建立高度安全且操作有效率的環境，如這裡所述。

本節將探討與企業環境中的身分識別和存取管理相關的設計考慮和建議。

## <a name="why-we-need-identity-and-access-management"></a>為何需要身分識別和存取管理

企業中的技術層面變得複雜且異構。 為了管理此環境的合規性和安全性，IAM 可讓正確的人員在適當的時間存取正確的資源，以因應原因。

### <a name="planning-for-identity-and-access-management"></a>規劃身分識別和存取管理

企業組織通常會遵循最低許可權的操作存取方法。 此模型應該透過 Azure AD 角色型存取控制 (RBAC) 和自訂角色定義來擴充，以考慮 Azure。 規劃如何管理控制和資料平面存取 Azure 中的資源是非常重要的。 IAM 和 RBAC 的任何設計都必須符合法規、安全性和營運需求，才能接受。

身分識別與存取管理是一個多步驟的程式，其中包含仔細規劃身分識別整合和其他安全性考慮，例如封鎖舊版驗證和規劃新式密碼。 預備規劃也牽涉到選取 B2B) 或企業對消費者 (B2C) 身分識別和存取管理的企業對企業 (。 雖然這些需求有所不同，但還是需要考慮企業登陸區域的常見設計考慮和建議。

![身分識別和存取管理](./media/iam.png)

_圖1：身分識別和存取管理。_

**設計考慮：**

- 根據 IAM 和治理來配置架構時，必須考慮的自訂角色和角色指派數目有所限制。 可以在這裡找到： [AZURE RBAC 服務限制](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#role-based-access-control-limits)
- 每個訂用帳戶的自訂 RBAC 角色指派限制為2000。

- 每個管理群組的自訂 RBAC 角色指派限制為500。

- 集中式與同盟資源擁有權。

  - 共用資源或環境中任何執行或強制安全性界限（例如網路）的層面，都必須集中管理。 對於授與或拒絕機密或重要商務資源存取權的任何組織而言，這是許多法規架構和標準實務的需求。

  - 管理不違反安全性界限的應用程式資源或維護安全性和合規性所需的其他層面，都可以委派給應用程式小組。 允許使用者在安全管理的環境中布建資源，可讓組織充分利用雲端的 agile 特性，同時防止違反任何重要的安全性或治理界限。

<!-- docsTest:ignore Azure-AD-only Azure-AD-managed NetOps SecOps AppOps -->

**設計建議：**

- 使用 Azure AD [RBAC](https://docs.microsoft.com/azure/role-based-access-control/overview)來管理對資源的資料平面存取，盡可能 (例如 Azure Key Vault、儲存體帳戶或 SQL Database) 。

- 針對具有 Azure 環境許可權的任何使用者，部署 Azure AD 條件式存取原則。 這麼做可提供另一種機制，協助保護受控制的 Azure 環境不受未經授權的存取。

- 對具有 Azure 環境許可權的任何使用者強制執行多重要素驗證。 這是許多合規性架構的需求，可大幅降低認證竊取和未經授權存取的風險。

- 使用[Azure AD Privileged Identity Management (PIM) ](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure)來建立零的存取權和最低許可權。 將貴組織的角色對應到所需的最低存取層級。 Azure AD PIM 可以是現有工具和進程的延伸模組，如上面所述使用 Azure 原生工具，或視需要使用。

- 授與資源的存取權時，請在 Azure AD PIM 中，針對 Azure 控制平面資源使用 Azure-僅限 AD 群組。

  - 如果群組管理系統已準備就緒，請將內部部署群組新增至 Azure AD 專用群組。

- 使用 Azure AD PIM 存取審查來定期驗證資源權利。 存取審查是許多合規性架構的一部分，因此許多組織都已經有處理這項需求的程式。

- 整合 Azure AD 記錄與平臺中心[Azure 監視器](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-activity-logs-azure-monitor)。 Azure 監視器允許在 Azure 中記錄和監視資料的單一事實來源，為組織提供雲端原生選項，以符合記錄收集和保留的需求。

- 如果有任何資料主權需求，則可以部署自訂使用者原則來強制執行。

- 考慮下列重要角色時，請使用 Azure AD 租使用者內的自訂 RBAC 角色定義：

| 角色 | 使用方式 | 動作 | 沒有動作 |
|---|---|---|---|
| Azure 平臺擁有者               | 管理群組和訂用帳戶生命週期管理                                                           | `*`                                                                                                                                                                                                                  |                                                                                                                                                                                         |
| 網路管理 (NetOps)         | 整個平臺的全球連線管理： Vnet、Udr、Nsg、Nva、VPN、ExpressRoute 等等            | `*/read`, `Microsoft.Authorization/*/write`, `Microsoft.Network/vpnGateways/*`, `Microsoft.Network/expressRouteCircuits/*`, `Microsoft.Network/routeTables/write`, `Microsoft.Network/vpnSites/*`                              |                                                                                                                                                                               |
|  (SecOps) 的安全性作業       | 在整個 Azure 資產和 Azure Key Vault 清除原則中，以水準視圖安全性系統管理員角色 | `*/read`, `*/register/action`, `Microsoft.KeyVault/locations/deletedVaults/purge/action`, `Microsoft.Insights/alertRules/*`, `Microsoft.Authorization/policyDefinitions/*`, `Microsoft.Authorization/policyAssignments/*`, `Microsoft.Authorization/policySetDefinitions/*`, `Microsoft.PolicyInsights/*`, `Microsoft.Security/*` |                                                                            |
| 訂用帳戶擁有者                 | 衍生自訂用帳戶擁有者角色之訂用帳戶擁有者的委派角色                                       | `*`                                                                                                                                                                                                                  | `Microsoft.Authorization/*/write`, `Microsoft.Network/vpnGateways/*`, `Microsoft.Network/expressRouteCircuits/*`, `Microsoft.Network/routeTables/write`, `Microsoft.Network/vpnSites/*` |
| 應用程式擁有者 (DevOps/AppOps)  | 已針對資源群組層級的應用程式/作業小組授與參與者角色                                 |                                                                                                                                                                                                                    | `Microsoft.Network/publicIPAddresses/write`, `Microsoft.Network/virtualNetworks/write`, `Microsoft.KeyVault/locations/deletedVaults/purge/action`                                         |

- 使用 Azure 資訊安全中心的即時 (JIT) 所有基礎結構即服務的存取 (IaaS) 資源，以啟用對 IaaS 虛擬機器的暫時使用者存取的網路層級保護。

- 使用適用于 Azure 資源的 Azure AD 受控識別，以避免以使用者名稱和密碼為基礎的驗證。 由於公用雲端資源的許多安全性缺口源自內嵌在程式碼或其他文字來源中的認證竊取，因此強制執行以程式設計方式存取的受控識別，可大幅降低認證竊取的風險。

- 針對需要更高存取權限的自動化 runbook，使用特殊許可權的身分識別。 違反重要安全性界限的自動化工作流程，應該受相同的工具和原則使用者對等許可權的規範。

- 請勿將使用者直接新增至 Azure 資源範圍。 這不是集中式管理，會大幅增加保護所需的管理，以防止未經授權存取受限制的資料。

### <a name="planning-for-authentication-inside-a-landing-zone"></a>規劃登陸區域內的驗證

採用 Azure 時，企業組織必須進行的一項重要設計決策是要將內部部署身分識別網域延伸到 Azure，還是要建立全新的身分。 應徹底評估登陸區域內的驗證需求，並將其納入計畫中，以在 Windows server、Azure AD Domain Services 或兩者中部署 Active Directory Domain Services (AD DS) 。 大部分的 Azure 環境至少會使用 Azure 網狀架構驗證的 Azure AD，以及 AD DS 本機主機驗證和群組原則管理。

**設計考慮：**

- 請考慮集中式和委派的責任，以管理在登陸區域內部署的資源。

- 依賴網域服務和使用舊版通訊協定的應用程式可以使用[Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services)。

**設計建議：**

- 使用集中式和委派的責任，根據角色和安全性需求來管理部署于登陸區域內的資源。

- 特殊許可權作業（例如建立服務主體物件、在 Azure AD 中註冊應用程式，以及採購和處理憑證或萬用字元憑證）都需要特殊許可權。 請考慮哪些使用者將處理這類要求，以及如何根據所需的努力來保護和監視其帳戶。

- 如果組織有一個案例，其中使用整合式 Windows 驗證的應用程式必須透過 Azure AD 從遠端存取，則請考慮使用[Azure AD 應用程式 Proxy](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy)。

- 在 Windows server 上執行 Azure AD、Azure AD Domain Services 和 AD DS 之間有差異。 評估您的應用程式需求，並瞭解並記錄每個使用者將使用的驗證提供者。 針對所有應用程式進行相應的規劃。

- 評估 Windows server 上 AD DS 和 Azure AD Domain Services 的工作負載相容性。

- 確定您的網路設計允許 Windows server 上需要 AD DS 的資源進行本機驗證和管理，以存取適當的網域控制站。

  - 對於 Windows server 上的 AD DS，請考慮在較大的企業級網路內容中提供本機驗證和主機管理的共用服務環境。

- 在主要區域中部署 Azure AD Domain Services，因為此服務只能投射到一個訂用帳戶。

- 使用受控識別，而不是服務主體來驗證 Azure 服務。 這可減少認證竊取的風險。
