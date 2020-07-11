---
title: CAF 企業規模的執行指導方針
description: 瞭解如何在適用于 Azure 的 Microsoft Cloud 採用架構中，CAF 企業規模的執行方針。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 53763005953c65c8a009fd8b4bbc1a4ada96769a
ms.sourcegitcommit: 84d7bfd11329eb4c151c4c32be5bab6c91f376ed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2020
ms.locfileid: "86235070"
---
<!-- cSpell:ignore interdomain VMSS VWAN -->

# <a name="caf-enterprise-scale-implementation-guidelines"></a>CAF 企業規模的執行指導方針

本節涵蓋如何開始使用企業級的平臺原生參考，並概述設計目標。

有三個必須執行的活動類別，才能執行企業級架構：

<!-- docsTest:disable -->

1. **針對企業級架構，必須符合下列條件：** 包含必須由 Azure 和 Azure AD 系統管理員執行以建立初始設定的活動。 這些活動是依本質順序排列，主要是一次性活動。

2. **啟用新區域 (檔案 > 新 > 區域) ：** 當需要將企業級平臺擴充到新的 Azure 區域時，所需的一組活動。

3. **部署新的登陸區域 (檔案 > 新 > 登陸區域) ：** 這些是具現化新登陸區域所需的重複活動。

<!-- docsTest:enable -->

若要大規模進行讓，這些活動必須遵循基礎結構即程式碼 (IaC) 原則，而且必須使用部署管線自動化。

## <a name="what-must-be-true-for-a-caf-enterprise-scale-landing-zone"></a>CAF 企業級登陸區域必須符合的條件

### <a name="enterprise-agreement-ea-enrollment-and-azure-ad-tenants"></a>Enterprise 合約 (EA) 註冊和 Azure AD 租使用者

1. 設定 EA 系統管理員和通知帳戶。

2. 建立部門：商務網域/地理型/組織。

3. 在部門底下建立 EA 帳戶。

4. 如果要從內部部署同步處理身分識別，則會為每個 Azure AD 租使用者設定 Azure AD Connect。

5. 建立對 Azure 資源的零持續存取，並透過 Azure AD PIM 及時存取。

### <a name="management-group-and-subscription"></a>管理群組和訂用帳戶

1. 遵循本文中的建議，建立管理群組[階層。](./management-group-and-subscription-organization.md#define-a-management-group-hierarchy)

2. 定義訂用帳戶布建準則，以及訂用帳戶擁有者的責任。

3. 為平臺管理、全域網路和連線能力以及身分識別資源（如 Active Directory 網域控制站）建立管理、連線和身分識別訂用帳戶。

4. 設定 Git 儲存機制來裝載 IaC 和服務主體，以與平臺 CI/CD 管線搭配使用。

5. 使用訂用帳戶和管理群組範圍的 Azure AD PIM，建立自訂角色定義和管理權利。

6. 針對登陸區域，在下表中建立 Azure 原則指派。

| 名稱                  | 描述                                                                                   |
|-----------------------|-----------------------------------------------------------------------------------------------|
| [`Deny-PublicEndpoints`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/.AzState/Microsoft.Authorization_policySetDefinitions-Deny-PublicEndpoints.parameters.json) | 拒絕在所有登陸區域上建立具有公用端點的服務。 |
| [`Deploy-VM-Backup`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/Landing%20Zones/.AzState/Microsoft.Authorization_policyAssignments-Deploy-VM-Backup.parameters.json) | 確保已設定備份，並將其部署至登陸區域中的所有 Vm。 |
| [`Deploy-VNet`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-vNet.parameters.json) | 確保所有登陸區域都已部署 VNet，並將其對等互連至區域虛擬中樞。 |

### <a name="global-networking-and-connectivity"></a>全域網路和連線能力

1. 為將部署虛擬中樞和 Vnet 的每個 Azure 區域配置適當的 VNet CIDR 範圍

2. 如果您決定透過 Azure 原則建立網路資源，請將下表所列的原則指派給連線能力訂閱。 如此一來，Azure 原則可確保根據所提供的參數，建立下列清單中的資源。
   - 建立 Azure 虛擬 WAN 標準實例。
   - 為每個區域建立 Azure 虛擬 WAN 虛擬中樞。 請確定每個虛擬中樞至少已部署一個閘道 (ExpressRoute 或 VPN) 。
   - 藉由在每個虛擬中樞內部署 Azure 防火牆來保護虛擬中樞。
   - 建立必要的 Azure 防火牆原則，並將其指派給安全的虛擬中樞。
   - 請確定連線到安全虛擬中樞的所有 Vnet 都受到 Azure 防火牆的保護。

3. 部署和設定 Azure 私人 DNS 區域。

4. 使用 Azure 私用對等互連布建 ExpressRoute 線路。 請依照[建立和修改 ExpressRoute 線路的對等互連](https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager#private)中的指示進行。

5. 透過 ExpressRoute 線路將內部部署 HQs/Dc 連線到 Azure 虛擬 WAN 虛擬中樞。

6. 使用 Nsg 保護跨虛擬中樞的 VNet 流量。

7.  (選擇性： ) 透過 ExpressRoute 私用對等互連設定加密。 遵循[expressroute 加密：透過 expressroute 進行虛擬 WAN 的 IPsec](https://docs.microsoft.com/azure/virtual-wan/vpn-over-expressroute)中的指示。

8.  (選擇性： ) 透過 VPN 將分支連線至虛擬中樞。 請遵循[使用 Azure 虛擬 WAN 建立站對站](https://docs.microsoft.com/azure/virtual-wan/virtual-wan-site-to-site-portal)連線中的指示。

9.  (選擇性：當有多個內部部署位置透過 ExpressRoute 連線到 Azure 時，) 設定 ExpressRoute Global 觸及以連接內部部署 HQs/Dc。 請依照[設定 ExpressRoute 全球範圍](https://docs.microsoft.com/azure/expressroute/expressroute-howto-set-global-reach)中的指示進行。

下列清單顯示針對企業規模部署執行網路資源時所使用的 Azure 原則指派：

| 名稱                     | 描述                                                                            |
|--------------------------|----------------------------------------------------------------------------------------|
| [`Deploy-FirewallPolicy`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-FirewallPolicy.parameters.json) | 建立防火牆原則。 |
| [`Deploy-VHub`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-vHUB.parameters.json) | 此原則會部署虛擬中樞、Azure 防火牆和 VPN/ExpressRoute 閘道，並在已連線的 Vnet 上設定 Azure 防火牆的預設路由。 |
| [`Deploy-VWAN`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-vWAN.parameters.json)| 部署虛擬 WAN。 |

### <a name="security-governance-and-compliance"></a>安全性、治理和合規性

1. 定義和套用[服務啟用架構](./security-governance-and-compliance.md#service-enablement-framework)，以確保 Azure 服務符合企業安全性和治理需求。

2. 建立自訂的 Azure RBAC 角色定義。

3. 啟用 Azure AD PIM 並探索 Azure 資源，以加速 Privileged Identity Management。

4. 建立 Azure-僅限 AD 的群組，以使用 Azure AD PIM 來管理資源的 Azure 控制平面。

5. 套用下列第一個表格中所列的原則，以確保 Azure 服務符合企業需求。

6. 定義命名慣例，並透過 Azure 原則加以強制執行。

7. 在所有範圍建立原則矩陣 (例如，透過 Azure 原則) 來啟用所有 Azure 服務的監視。

下列原則應用來強制執行全公司的合規性狀態。

| 名稱                       | 描述                                                        |
|----------------------------|--------------------------------------------------------------------|
| [`Allowed-ResourceLocation`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/.AzState/Microsoft.Authorization_policyAssignments-Allowed-ResourceLocation.parameters.json)   | 指定可部署資源的允許區域。 |
| [`Allowed-RGLocation`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/.AzState/Microsoft.Authorization_policyAssignments-Allowed-RGLocation.parameters.json)         | 指定可在其中部署資源群組的允許區域。 |
| [`Denied-Resources`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/.AzState/Microsoft.Authorization_policyAssignments-Denied-Resources.parameters.json)           | 公司拒絕的資源。 |
| [`Deny-AppGW-Without-WAF`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/.AzState/Microsoft.Authorization_policyDefinitions-Deny-AppGW-Without-WAF.parameters.json)     | 允許部署已啟用 WAF 的應用程式閘道。 |
| [`Deny-IP-Forwarding`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/.AzState/Microsoft.Authorization_policyAssignments-Deny-IP-Forwarding.parameters.json)         | 拒絕 IP 轉送。 |
| [`Deny-RDP-From-Internet`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/.AzState/Microsoft.Authorization_policyAssignments-Deny-RDP-From-Internet.parameters.json)     | 拒絕來自網際網路的 RDP 連線。 |
| [`Deny-Subnet-Without-Nsg`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/.AzState/Microsoft.Authorization_policyDefinitions-Deny-Subnet-Without-Nsg.parameters.json)    | 拒絕建立不含 NSG 的子網。 |
| [`Deploy-ASC-CE`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-ASC-CE.parameters.json)              | 設定資訊安全中心連續匯出至您的 Log Analytics 工作區。 |
| [`Deploy-ASC-Monitoring`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/.AzState/Microsoft.Authorization_policyAssignments-Deploy-ASC-Monitoring.parameters.json)      | 在 Azure 資訊安全中心中啟用監視。 |
| [`Deploy-ASC-Standard`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-ASC-Standard.parameters.json)        | 確保訂閱已啟用資訊安全中心標準。 |
| [`Deploy-Diag-ActivityLog`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-Diagnostics-ActivityLog.parameters.json) | 啟用診斷活動記錄，並轉送至 Log Analytics。 |
| [`Deploy-Diag-LogAnalytics`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-Log-Analytics.parameters.json) | |
| [`Deploy-VM-Monitoring`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-Diagnostics-VM.parameters.json) | 確保已啟用 VM 監視。 |

### <a name="platform-identity"></a>平臺身分識別

1. 如果您決定透過 Azure 原則建立身分識別資源，請將下表所列的原則指派給身分識別訂用帳戶。 如此一來，Azure 原則可確保根據所提供的參數，建立下列清單中的資源。

2. 部署 Active Directory 網域控制站。

下列清單顯示在執行企業規模部署的身分識別資源時，可以使用的原則。

| 名稱                         | 描述                                                               |
|------------------------------|---------------------------------------------------------------------------|
| [`DataProtectionSecurityCenter`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/.AzState/Microsoft.Authorization_policyDefinitions-DataProtectionSecurityCenter.parameters.json) | Azure 資訊安全中心自動建立的資料保護。 |
| [`Deploy-VNet-Identity`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-vNet.parameters.json) | 將 VNet 部署到主機 (的身分識別訂用帳戶，例如 DC. )  |

### <a name="platform-management-and-monitoring"></a>平臺管理和監視

1. 為組織和以資源為中心的視圖建立原則合規性與安全性儀表板。

2. 建立平臺密碼的工作流程 (服務主體和自動化帳戶) 和金鑰變換。

3. 針對 Log Analytics 中的記錄設定長期封存和保留。

4. 設定 Azure Key Vault 以儲存平臺密碼。

5. 如果您決定透過 Azure 原則建立平臺管理資源，請將下表所列的原則指派給管理訂用帳戶。 如此一來，Azure 原則確保下列清單中的資源會根據提供的參數來建立。

| 名稱                   | 描述                                                                            |
|------------------------|----------------------------------------------------------------------------------------|
| [`Deploy-LA-Config`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-LA-Config.parameters.json) | Log Analytics 工作區的設定。 |
| [`Deploy-Log-Analytics`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-Log-Analytics.parameters.json) | 部署 Log Analytics 工作區。 |

## <a name="file--new--region"></a>**檔案 > 新 > 區域**

1. 如果您決定透過 Azure 原則建立網路資源，請將下表所列的原則指派給連線能力訂閱。 如此一來，Azure 原則將確保會根據提供的參數來建立下列清單中的資源。

    - 在 [連線能力] 訂用帳戶中，于現有的虛擬 WAN 內建立新的虛擬中樞。
    - 藉由在虛擬中樞內部署 Azure 防火牆，並將現有或新的防火牆原則連結至 Azure 防火牆，來保護虛擬中樞。
    - 請確定連線到安全虛擬中樞的所有 Vnet 都受到 Azure 防火牆的保護。

2. 透過 ExpressRoute 或 VPN 將虛擬中樞連線至內部部署網路。

3. 透過 Nsg 保護跨虛擬中樞的 VNet 流量。

4.  (選擇性： ) 透過 ExpressRoute 私用對等互連設定加密。

| 名稱                     | 描述                                                                            |
|--------------------------|----------------------------------------------------------------------------------------|
| [`Deploy-VHub`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560/contoso/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-vHUB.parameters.json) | 此原則會部署虛擬中樞、Azure 防火牆、閘道 (VPN/ExpressRoute) ，並在已連線的 Vnet 上設定 Azure 防火牆的預設路由。 |

<!-- docsTest:disable -->

## <a name="file---new---landing-zone-for-applications-and-workloads"></a>適用于應用程式和工作負載的檔案 > 新 > 登陸區域

<!-- docsTest:enable -->

1. 建立訂用帳戶，並將它移到 `Landing Zones` 管理群組範圍底下。

2. 建立訂用帳戶的 Azure AD 群組，例如 `Owner` 、 `Reader` 和 `Contributor` 。

3. 建立 Azure AD 群組的 Azure AD PIM 權利。
