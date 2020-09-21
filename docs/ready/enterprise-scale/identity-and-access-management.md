---
title: 身分識別和存取管理
description: 檢查與企業環境中的身分識別和存取管理相關的設計考慮和建議。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 2b8866d90e93ace8ada24da7162f2c665a02abaa
ms.sourcegitcommit: 4e12d2417f646c72abf9fa7959faebc3abee99d8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2020
ms.locfileid: "90776443"
---
# <a name="identity-and-access-management"></a>身分識別和存取管理

身分識別提供大量安全性保證的基礎。 它會根據身分識別驗證和雲端服務中的授權控制來啟用存取，以保護資料和資源，以及決定應允許的要求。

身分識別和存取管理 (IAM) 是公用雲端中的界限安全性。 它必須被視為任何安全且完全符合規範之公用雲端架構的基礎。 Azure 提供一組完整的服務、工具和參考架構，讓組織能夠以高度安全且有效率的方式執行環境，如下所述。

本節將探討與企業環境中 IAM 相關的設計考慮和建議。

## <a name="why-we-need-identity-and-access-management"></a>為何需要身分識別和存取管理

企業中的技術環境會變得複雜且不穩定。 為了管理此環境的合規性和安全性，IAM 讓適當的人員能夠在適當的時間存取適當的資源，以獲得正確的原因。

### <a name="plan-for-identity-and-access-management"></a>規劃身分識別和存取管理

企業組織通常會遵循最低許可權的操作存取方法。 您應擴充此模型，以透過 Azure Active Directory (Azure AD) 以角色為基礎的存取控制 (RBAC) 和自訂角色定義來考慮 Azure。 規劃如何在 Azure 中管理資源的控制和資料平面存取是很重要的。 IAM 和 RBAC 的任何設計都必須符合法規、安全性和營運需求，才能接受。

身分識別和存取管理是多步驟的程式，其中牽涉到謹慎規劃身分識別整合和其他安全性考慮，例如封鎖舊版驗證和規劃新式密碼。 預備規劃也牽涉到選取企業對企業或企業對消費者的身分識別與存取管理。 雖然這些需求不同，但仍有一般的設計考慮和建議，可考慮用於企業登陸區域。

![顯示身分識別和存取管理的圖表。](./media/iam.png)

_圖1：身分識別和存取管理。_

**設計考慮：**

- 當您針對 IAM 和治理來配置架構時，必須考慮的自訂角色和角色指派數目有一些限制。 如需詳細資訊，請參閱 [AZURE RBAC 服務限制](/azure/azure-resource-manager/management/azure-subscription-service-limits#role-based-access-control-limits)。
- 每個訂用帳戶的自訂 RBAC 角色指派限制為2000。
- 每個管理群組的自訂 RBAC 角色指派限制為500。
- 集中式與同盟資源擁有權：
  - 共用資源或環境中執行或強制執行安全性界限（例如網路）的任何層面都必須集中管理。 這項需求是許多法規架構的一部分。 對於授與或拒絕存取機密或重要商務資源的任何組織而言，都是標準做法。
  - 管理不違反安全性界限的應用程式資源，或是維護安全性和合規性所需的其他層面，都可以委派給應用程式小組。 允許使用者在安全管理的環境內布建資源，可讓組織充分利用雲端的敏捷本質，同時防止違反任何重要的安全性或治理界限。

<!-- docutune:ignore Azure-AD-only Azure-AD-managed -->

**設計建議：**

- 使用 Azure AD [RBAC](/azure/role-based-access-control/overview) 來管理資源的資料平面存取（可能的話）。 範例有 Azure Key Vault、儲存體帳戶或 SQL 資料庫。
- 針對任何具有 Azure 環境許可權的使用者部署 Azure AD 條件式存取原則。 這麼做會提供另一種機制，以協助保護受控制的 Azure 環境，防止未經授權的存取。
- 針對任何具有 Azure 環境許可權的使用者，強制執行多重要素驗證 (MFA) 。 MFA 強制是許多合規性架構的需求。 它可大幅降低認證遭竊和未經授權存取的風險。
- 使用 [Azure AD Privileged Identity Management (PIM) ](/azure/active-directory/privileged-identity-management/pim-configure) 來建立零的長期存取和最低許可權。 將您組織的角色對應至所需的最低存取層級。 Azure AD PIM 可以是現有工具和程式的延伸模組、如所述使用 Azure 原生工具，或視需要使用兩者。
- 當您授與資源的存取權時，請針對 Azure AD PIM 中的 Azure 控制平面資源使用僅限 Azure AD 的群組。
  - 如果已有群組管理系統，請將內部部署群組新增至僅限 Azure AD 的群組。
- 使用 Azure AD PIM 存取評論來定期驗證資源權利。 存取評論是許多合規性架構的一部分。 如此一來，許多組織都已有程式可解決這項需求。
- 整合 Azure AD 記錄與平臺中央 [Azure 監視器](/azure/active-directory/reports-monitoring/concept-activity-logs-azure-monitor)。 Azure 監視器可讓您在 Azure 中建立記錄和監視資料的單一事實來源，提供組織雲端原生選項，以符合記錄收集和保留的需求。
- 如果有任何資料主權需求，可以部署自訂使用者原則來強制執行。
- 當您考慮下列金鑰角色時，請使用 Azure AD 租使用者內的自訂 RBAC 角色定義：

| 角色 | 使用方式 | 動作 | 沒有任何動作 |
|---|---|---|---|
| Azure 平臺擁有者               | 管理群組和訂用帳戶生命週期管理                                                           | `*`                                                                                                                                                                                                                  |                                                                                                                                                                                         |
| 網路管理 (NetOps)         | 全平臺全球連線管理：虛擬網路、Udr、Nsg、Nva、VPN、Azure ExpressRoute 和其他            | `*/read`, `Microsoft.Authorization/*/write`, `Microsoft.Network/vpnGateways/*`, `Microsoft.Network/expressRouteCircuits/*`, `Microsoft.Network/routeTables/write`, `Microsoft.Network/vpnSites/*`                              |                                                                                                                                                                               |
| 安全性作業 (SecOps)        | 在整個 Azure 資產和 Azure Key Vault 清除原則之間進行水準視圖安全性系統管理員角色 | `*/read`, `*/register/action`, `Microsoft.KeyVault/locations/deletedVaults/purge/action`, `Microsoft.Insights/alertRules/*`, `Microsoft.Authorization/policyDefinitions/*`, `Microsoft.Authorization/policyAssignments/*`, `Microsoft.Authorization/policySetDefinitions/*`, `Microsoft.PolicyInsights/*`, `Microsoft.Security/*` |                                                                            |
| 訂用帳戶擁有者                 | 衍生自訂用帳戶擁有者角色之訂用帳戶擁有者的委派角色                                       | `*`                                                                                                                                                                                                                  | `Microsoft.Authorization/*/write`, `Microsoft.Network/vpnGateways/*`, `Microsoft.Network/expressRouteCircuits/*`, `Microsoft.Network/routeTables/write`, `Microsoft.Network/vpnSites/*` |
| 應用程式擁有者 (DevOps/AppOps)  | 在資源群組層級授與應用程式/作業小組的參與者角色                                 |                                                                                                                                                                                                                    | `Microsoft.Network/publicIPAddresses/write`, `Microsoft.Network/virtualNetworks/write`, `Microsoft.KeyVault/locations/deletedVaults/purge/action`                                         |

- 使用 Azure 資訊安全中心即時存取所有基礎結構即服務 (IaaS) 資源，以啟用暫時使用者對 IaaS 虛擬機器存取的網路層級保護。
- 使用 azure 資源的 Azure AD 受控識別，以避免根據使用者名稱和密碼進行驗證。 由於公用雲端資源的許多安全性缺口源自內嵌于程式碼或其他文字來源的認證遭竊，因此強制使用受控識別進行程式設計存取可大幅降低認證遭竊的風險。
- 針對需要提升存取權限的自動化 runbook，請使用特殊許可權身分識別。 違反重要安全性界限的自動化工作流程，應該受相同的工具和原則使用者的相同許可權。
- 請勿直接將使用者新增至 Azure 資源範圍。 這種缺乏集中化的管理會大幅增加所需的管理，以防止未經授權的存取受限制的資料。

### <a name="plan-for-authentication-inside-a-landing-zone"></a>規劃登陸區域內的驗證

企業組織在採用 Azure 時必須進行的重要設計決策，是要將現有的內部部署身分識別網域擴充至 Azure，還是要建立全新的身分識別網域。 登陸區域內的驗證需求必須經過徹底評估並納入規劃，以部署 Windows server 中的 Active Directory Domain Services (AD DS) ，Azure AD Domain Services (Azure AD DS) 或兩者。 大部分的 Azure 環境都會使用至少 Azure AD 的 Azure 網狀架構驗證和 AD DS 本機主機驗證和群組原則管理。

**設計考慮：**

- 請考慮集中式和委派的責任，以管理在登陸區域內部署的資源。
- 依賴網域服務和使用舊版通訊協定的應用程式，可以使用 [AZURE AD DS](/azure/active-directory-domain-services)。

**設計建議：**

- 使用集中和委派的責任，根據角色和安全性需求來管理登陸區域內部署的資源。
- 特殊許可權作業（例如建立服務主體物件、在 Azure AD 中註冊應用程式，以及採購和處理憑證或萬用字元憑證）需要特殊許可權。 請考慮哪些使用者將處理這類要求，以及如何以所需的審慎程度保護和監視其帳戶。
- 如果組織有使用整合式 Windows 驗證的應用程式必須透過 Azure AD 從遠端存取的案例，請考慮使用 [Azure AD 應用程式 Proxy](/azure/active-directory/manage-apps/application-proxy)。
- 在 Windows server 上執行的 Azure AD、Azure AD DS 和 AD DS 之間有差異。 評估您的應用程式需求，並瞭解並記錄每一個應用程式所使用的驗證提供者。 據以針對所有應用程式進行規劃。
- 針對 Windows server 上的 AD DS 以及 Azure AD DS 評估工作負載的相容性。
- 確定您的網路設計可讓需要 Windows server AD DS 的資源進行本機驗證和管理，以存取適當的網域控制站。
  - 針對 Windows server 上的 AD DS，請考慮在較大的企業級網路內容中提供本機驗證和主機管理的共用服務環境。
- 在主要區域內部署 Azure AD DS，因為這項服務只能投射到一個訂用帳戶中。
- 使用受控識別而非服務主體來向 Azure 服務進行驗證。 這種方法可減少認證遭竊的風險。
