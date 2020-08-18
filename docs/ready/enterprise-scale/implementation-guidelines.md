---
title: CAF 企業規模的執行指導方針
description: 瞭解如何在適用于 Azure 的 Microsoft 雲端採用架構中 CAF 企業規模的實作為指導方針。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 92713f59e2b5c862bcad18310bbce254d4d70fcb
ms.sourcegitcommit: 917188fa930cadddb03f9e9bbcdd7b630e4ee33e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/17/2020
ms.locfileid: "88285025"
---
<!-- cSpell:ignore interdomain VMSS VWAN -->

# <a name="caf-enterprise-scale-implementation-guidelines"></a>CAF 企業規模的執行指導方針

本節說明如何開始使用企業規模的平臺原生參考，以及大綱設計目標。

有三種活動必須執行，才能實現企業規模的架構：

<!-- docsTest:disable -->

1. **符合企業規模架構的條件：** 包含必須由 Azure 和 Azure AD 系統管理員執行的活動，才能建立初始設定。 這些活動是依本質順序排列，而且主要是一次性活動。

2. **啟用新區域 (檔案-> 新 > 區域) ：** 需要將企業規模平臺擴展至新的 Azure 區域時，所需的一組活動。

3. **部署新的登陸區域 (檔案-> 新 > 的登陸區域) ：** 這些是具現化新登陸區域所需的週期性活動。

<!-- docsTest:enable -->

為了大規模讓，這些活動必須遵循基礎結構即程式碼 (IaC) 準則，而且必須使用部署管線自動化。

## <a name="what-must-be-true-for-a-caf-enterprise-scale-landing-zone"></a>CAF 企業規模登陸區域必須符合的條件

### <a name="enterprise-agreement-ea-enrollment-and-azure-ad-tenants"></a>Enterprise 合約 (EA) 註冊和 Azure AD 租使用者

1. 設定 EA 系統管理員和通知帳戶。

2. 建立部門：商務網域/異地/組織。

3. 在部門下建立 EA 帳戶。

4. 如果要從內部部署同步處理身分識別，請針對每個 Azure AD 租使用者設定 Azure AD Connect。

5. 建立零的 Azure 資源持續存取，以及透過 Azure AD PIM 的即時存取。

### <a name="management-group-and-subscription"></a>管理群組和訂用帳戶

1. 遵循本文中的建議，建立管理群組[階層。](./management-group-and-subscription-organization.md#define-a-management-group-hierarchy)

2. 定義訂用帳戶布建準則以及訂用帳戶擁有者的責任。

3. 為平臺管理、全球網路以及連線能力和身分識別資源（例如 Active Directory 網域控制站）建立管理、連線和身分識別的訂用帳戶。

4. 設定 Git 存放庫，以裝載與平臺 CI/CD 管線搭配使用的 IaC 和服務主體。

5. 使用訂用帳戶和管理群組範圍 Azure AD PIM 來建立自訂角色定義和管理權利。

6. 在下表中為登陸區域建立 Azure 原則指派。

| 名稱                  | 描述                                                                                   |
|-----------------------|-----------------------------------------------------------------------------------------------|
| [`Deny-PublicEndpoints`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/.AzState/Microsoft.Authorization_policySetDefinitions-Deny-PublicEndpoints.parameters.json) | 拒絕在所有登陸區域上建立具有公用端點的服務。 |
| [`Deploy-VM-Backup`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/Landing%20Zones%20(landingzones)/.AzState/Microsoft.Authorization_policyAssignments-Deploy-VM-Backup.parameters.json) | 確保備份已設定並部署至登陸區域中的所有 Vm。 |
| [`Deploy-VNet`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-vNet.parameters.json) | 確保所有登陸區域都已部署 VNet，且對等互連至區域虛擬中樞。 |

### <a name="global-networking-and-connectivity"></a>全球網路和連線能力

1. 針對將部署虛擬中樞和 Vnet 的每個 Azure 區域配置適當的 VNet CIDR 範圍

2. 如果您決定透過 Azure 原則建立網路資源，請將下表所列的原則指派給連線訂用帳戶。 如此一來，Azure 原則可確保系統會根據提供的參數來建立下列清單中的資源。
   - 建立 Azure 虛擬 WAN 標準實例。
   - 為每個區域建立 Azure 虛擬 WAN 虛擬中樞。 請確定每個虛擬中樞都至少部署一個閘道 (ExpressRoute 或 VPN) 。
   - 藉由在每個虛擬中樞內部署 Azure 防火牆來保護虛擬中樞。
   - 建立所需的 Azure 防火牆原則，並將其指派給安全的虛擬中樞。
   - 確定所有連線到安全虛擬中樞的 Vnet 都受到 Azure 防火牆的保護。

3. 部署及設定 Azure 私人 DNS 區域。

4. 使用 Azure 私人對等互連布建 ExpressRoute 線路。 遵循[建立和修改 ExpressRoute 線路對等互連](/azure/expressroute/expressroute-howto-routing-portal-resource-manager#private)中的指示

5. 透過 ExpressRoute 線路，將內部部署 HQs/Dc 連線到 Azure 虛擬 WAN 虛擬中樞。

6. 使用 Nsg 保護跨虛擬中樞的 VNet 流量。

7.  (選擇性： ) 透過 ExpressRoute 私用對等互連設定加密。 遵循 [expressroute 加密：適用于虛擬 WAN 的 expressroute 的 IPsec](/azure/virtual-wan/vpn-over-expressroute)中的指示。

8.  (選擇性： ) 透過 VPN 將分支連線到虛擬中樞。 遵循 [使用 Azure 虛擬 WAN 建立站對站](/azure/virtual-wan/virtual-wan-site-to-site-portal)連線的指示。

9.  (選擇性：當多個內部部署位置透過 ExpressRoute 連接到 Azure 時，) 設定 ExpressRoute Global 觸及以連接內部部署 HQs/Dc。 遵循 [設定 ExpressRoute Global 觸及範圍](/azure/expressroute/expressroute-howto-set-global-reach)中的指示。

下列清單顯示在執行企業規模部署的網路資源時，所使用的 Azure 原則指派：

| 名稱                     | 描述                                                                            |
|--------------------------|----------------------------------------------------------------------------------------|
| [`Deploy-FirewallPolicy`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-FirewallPolicy.parameters.json) | 建立防火牆原則。 |
| [`Deploy-VHub`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-vHUB.parameters.json) | 此原則會部署虛擬中樞、Azure 防火牆和 VPN/ExpressRoute 閘道，並在連線的 Vnet 上設定 Azure 防火牆的預設路由。 |
| [`Deploy-VWAN`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-vWAN.parameters.json)| 部署虛擬 WAN。 |

### <a name="security-governance-and-compliance"></a>安全性、治理和合規性

1. 定義並套用 [服務啟用架構](./security-governance-and-compliance.md#service-enablement-framework) ，以確保 Azure 服務符合企業安全性和治理需求。

2. 建立自訂的 Azure RBAC 角色定義。

3. 啟用 Azure AD PIM 並探索 Azure 資源，以加速 Privileged Identity Management。

4. 使用 Azure AD PIM，建立 azure 控制平面資源管理的 Azure-僅限 AD 群組。

5. 套用下列第一個表格所列的原則，以確保 Azure 服務符合企業需求。

6. 定義命名慣例，並透過 Azure 原則強制執行。

7. 在所有範圍建立原則矩陣 (例如，透過 Azure 原則) 啟用所有 Azure 服務的監視。

您應使用下列原則來強制執行全公司的合規性狀態。

| 名稱                       | 描述                                                        |
|----------------------------|--------------------------------------------------------------------|
| [`Allowed-ResourceLocation`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/.AzState/Microsoft.Authorization_policyAssignments-Allowed-ResourceLocation.parameters.json)   | 指定可在其中部署資源的允許區域。 |
| [`Allowed-RGLocation`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/.AzState/Microsoft.Authorization_policyAssignments-Allowed-RGLocation.parameters.json)         | 指定可在其中部署資源群組的允許區域。 |
| [`Denied-Resources`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/.AzState/Microsoft.Authorization_policyAssignments-Denied-Resources.parameters.json)           | 公司拒絕的資源。 |
| [`Deny-AppGW-Without-WAF`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/.AzState/Microsoft.Authorization_policyDefinitions-Deny-AppGW-Without-WAF.parameters.json)     | 允許啟用 WAF 的應用程式閘道。 |
| [`Deny-IP-Forwarding`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/.AzState/Microsoft.Authorization_policyAssignments-Deny-IP-Forwarding.parameters.json)         | 拒絕 IP 轉送。 |
| [`Deny-RDP-From-Internet`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/.AzState/Microsoft.Authorization_policyAssignments-Deny-RDP-From-Internet.parameters.json)     | 拒絕來自網際網路的 RDP 連線。 |
| [`Deny-Subnet-Without-Nsg`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/.AzState/Microsoft.Authorization_policyDefinitions-Deny-Subnet-Without-Nsg.parameters.json)    | 拒絕建立子網而不需要 NSG。 |
| [`Deploy-ASC-CE`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-ASC-CE.parameters.json)              | 將安全性中心的連續匯出設定為 Log Analytics 工作區。 |
| [`Deploy-ASC-Monitoring`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/.AzState/Microsoft.Authorization_policyAssignments-Deploy-ASC-Monitoring.parameters.json)      | 在 Azure 資訊安全中心中啟用監視。 |
| [`Deploy-ASC-Standard`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-ASC-Standard.parameters.json)        | 確定訂用帳戶已啟用安全中心標準。 |
| [`Deploy-Diag-ActivityLog`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-Diagnostics-ActivityLog.parameters.json) | 啟用診斷活動記錄，並轉送至 Log Analytics。 |
| [`Deploy-Diag-LogAnalytics`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-Log-Analytics.parameters.json) | |
| [`Deploy-VM-Monitoring`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-Diagnostics-VM.parameters.json) | 確保已啟用 VM 監視。 |

### <a name="platform-identity"></a>平臺身分識別

1. 如果您決定透過 Azure 原則建立身分識別資源，請將下表所列的原則指派給身分識別訂用帳戶。 如此一來，Azure 原則可確保系統會根據提供的參數來建立下列清單中的資源。

2. 部署 Active Directory 網域控制站。

下列清單顯示在企業規模部署中執行身分識別資源時，可使用的原則。

| 名稱                         | 描述                                                               |
|------------------------------|---------------------------------------------------------------------------|
| [`DataProtectionSecurityCenter`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/.AzState/) | Azure 資訊安全中心自動建立的資料保護。 |
| [`Deploy-VNet-Identity`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-vNet.parameters.json) | 將 VNet 部署到身分識別訂用帳戶，以裝載 (例如 DC. )  |

### <a name="platform-management-and-monitoring"></a>平臺管理與監視

1. 針對組織和以資源為中心的視圖建立原則合規性和安全性儀表板。

2. 建立平臺秘密 (服務主體和自動化帳戶) 和金鑰變換的工作流程。

3. 針對 Log Analytics 中的記錄設定長期封存和保留。

4. 設定 Azure Key Vault 儲存平臺秘密。

5. 如果您決定透過 Azure 原則建立平臺管理資源，請將下表所列的原則指派給管理訂用帳戶。 如此一來，Azure 原則可確保系統會根據提供的參數來建立下列清單中的資源。

| 名稱                   | 描述                                                                            |
|------------------------|----------------------------------------------------------------------------------------|
| [`Deploy-LA-Config`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-LA-Config.parameters.json) | Configuration Log Analytics 工作區。 |
| [`Deploy-Log-Analytics`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-Log-Analytics.parameters.json) | 部署 Log Analytics 工作區。 |

## <a name="file--new--region"></a>**檔案-> 新 > 區域**

1. 如果您決定透過 Azure 原則建立網路資源，請將下表所列的原則指派給連線訂用帳戶。 如此一來，Azure 原則可確保系統會根據提供的參數來建立下列清單中的資源。

    - 在連接訂用帳戶中，于現有的虛擬 WAN 內建立新的虛擬中樞。
    - 安全虛擬中樞，方法是在虛擬中樞內部署 Azure 防火牆，並將現有或新的防火牆原則連結到 Azure 防火牆。
    - 確定所有連線到安全虛擬中樞的 Vnet 都受到 Azure 防火牆的保護。

2. 透過 ExpressRoute 或 VPN 將虛擬中樞連線至內部部署網路。

3. 透過 Nsg 保護跨虛擬中樞的 VNet 流量。

4.  (選擇性： ) 透過 ExpressRoute 私用對等互連設定加密。

| 名稱                     | 描述                                                                            |
|--------------------------|----------------------------------------------------------------------------------------|
| [`Deploy-VHub`](https://github.com/Azure/Enterprise-Scale/blob/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/contoso%20(contoso)/.AzState/Microsoft.Authorization_policyDefinitions-Deploy-vHUB.parameters.json) | 此原則會將虛擬中樞、Azure 防火牆、閘道 (VPN/ExpressRoute) ，並在連線的 Vnet 上設定 Azure 防火牆的預設路由。 |

<!-- docsTest:disable -->

## <a name="file---new---landing-zone-for-applications-and-workloads"></a>適用于應用程式和工作負載的檔案 > 新 > 登陸區域

<!-- docsTest:enable -->

1. 建立訂用帳戶，並將它移至 `Landing Zones` 管理群組範圍下。

2. 建立訂閱的 Azure AD 群組，例如 `Owner` 、 `Reader` 和 `Contributor` 。

3. 為已建立的 Azure AD 群組建立 Azure AD PIM 權利。