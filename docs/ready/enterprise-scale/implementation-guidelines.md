---
title: 企業規模的實作為指導方針
description: 瞭解適用于 Azure 的 Microsoft 雲端採用架構中的企業級執行指導方針。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: think-tank
ms.openlocfilehash: a7a3fdc5312053504d7ec1ef594661bd8b994dac
ms.sourcegitcommit: c167c45b66cc7324b60c88b8b7aac439f956b65d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2021
ms.locfileid: "102207940"
---
<!-- cSpell:ignore interdomain VMSS VWAN -->

# <a name="enterprise-scale-implementation-guidelines"></a>企業規模的實作為指導方針

本文涵蓋如何開始使用企業規模的平臺原生參考，以及大綱設計目標。

若要執行企業規模的架構，您必須考慮下列活動類別：

<!-- docutune:disable -->

1. **企業規模架構必須符合下列條件：** 包含必須由 Azure 和 Azure Active Directory 執行的活動， (Azure AD) 系統管理員來建立初始設定。 這些活動是依本質順序排列，而且主要是一次性活動。

2. **啟用新區域 (檔案-> 新 > 區域) ：** 包含需要將企業規模平臺擴展至新的 Azure 區域時，所需的活動。

3. **部署新的登陸區域 (檔案-> 新 > 的登陸區域) ：** 這些是具現化新登陸區域所需的週期性活動。

<!-- docutune:enable -->

為了大規模讓，這些活動必須遵循基礎結構即程式碼 (IaC) 準則，而且必須使用部署管線自動化。

## <a name="what-must-be-true-for-an-enterprise-scale-landing-zone"></a>企業規模登陸區域必須符合的條件

下列各節列出在適用于 Azure 的 Microsoft 雲端採用架構中完成此類活動的步驟。

### <a name="enterprise-agreement-enrollment-and-azure-ad-tenants"></a>Enterprise 合約註冊和 Azure AD 租使用者

1. 設定 (EA) 系統管理員和通知帳戶的 Enterprise 合約。

2. 建立部門：商務網域/異地/組織。

3. 在部門下建立 EA 帳戶。

4. 如果要從內部部署同步處理身分識別，請設定每個 Azure AD 租使用者的 Azure AD Connect。

5. 透過 Azure AD 特殊許可權身分識別管理 (PIM) ，建立對 Azure 資源的持續存取，以及即時存取。

### <a name="management-group-and-subscription"></a>管理群組和訂用帳戶

1. 遵循 [管理群組和訂](./management-group-and-subscription-organization.md#define-a-management-group-hierarchy)用帳戶組織中的建議，建立管理群組階層。

2. 定義訂用帳戶布建的準則和訂用帳戶擁有者的責任。

3. 為平臺管理、全球網路以及連線能力和身分識別資源（例如 Active Directory 網域控制站）建立管理、連線和身分識別的訂用帳戶。

4. 設定 Git 存放庫，以裝載 IaC 和服務主體，以用於持續整合和持續部署的平臺管線。

5. 針對訂用帳戶和管理群組範圍使用 Azure AD PIM 來建立自訂角色定義和管理權利。

6. 在下表中為登陸區域建立 Azure 原則指派。

   | 名稱 | 描述 |
   |--|--|
   | [`Deny-PublicEndpoints`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.) 上的 AzState/Microsoft.Authorization_policySetDefinitions-Deny-PublicEndpoints.parameters.js | 拒絕在所有登陸區域上建立具有公用端點的服務。 |
   | [`Deploy-VM-Backup`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /Landing%20Zones%20 (landingzones) /。) 上的 AzState/Microsoft.Authorization_policyAssignments-Deploy-VM-Backup.parameters.js | 確保備份已設定並部署至登陸區域中的所有 Vm。 |
   | [`Deploy-VNet`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.) 上的 AzState/Microsoft.Authorization_policyDefinitions-Deploy-vNet.parameters.js | 確保所有登陸區域都已部署虛擬網路，並對等互連至區域虛擬中樞。 |

#### <a name="sandbox-governance-guidance"></a>沙箱治理指導方針

如 [管理群組和訂用帳戶組織重要設計區域](./management-group-and-subscription-organization.md)中所述，位於沙箱管理群組階層內的訂用帳戶應該具有較不嚴格的原則方法。 因為企業中的使用者應該使用這些訂用帳戶來進行實驗，並使用 Azure 產品和服務來進行創新，所以您的登陸區域階層中可能尚未允許這些訂用帳戶，以驗證其想法/概念是否可運作;進入正式開發環境之前。

不過，在沙箱管理群組階層中，這些訂用帳戶仍然需要一些護欄，以確保正確使用這些訂用帳戶，例如進行創新、體驗新的 Azure 服務和功能，以及概念發想驗證。

**因此，我們建議您：**

1. 在下表的沙箱管理群組範圍中建立 Azure 原則指派：

  | 名稱                  |     描述                                                                                     | 指派注意事項 |
  |-----------------------|-----------------------------------------------------------------------------------------------|--------------------------------------------------------------|
  | [`Deny-VNET-Peering-Cross-Subscription`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/) /.AzState)  | 防止對訂用帳戶以外的其他 Vnet 建立 VNET 對等互連連線。 | 請確定此原則僅指派至沙箱管理群組階層的範圍層級。 |
  | [`Denied-Resources`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.) 上的 AzState/Microsoft.Authorization_policyAssignments-Denied-Resources.parameters.js | 在沙箱訂用帳戶中被拒絕建立的資源。 這會導致無法建立任何混合式連線資源; *例如 VPN/ExpressRoute/VirtualWAN* | 指派此原則時，請選取下列資源以拒絕建立： VPN 閘道： `microsoft.network/vpngateways` 、P2S 閘道： `microsoft.network/p2svpngateways` 、虛擬 wan： `microsoft.network/virtualwans` 、虛擬 WAN 中樞： `microsoft.network/virtualhubs` 、expressroute 線路： `microsoft.network/expressroutecircuits` 、expressroute 閘道： `microsoft.network/expressroutegateways` 、expressroute 埠： `microsoft.network/expressrouteports` 、Expressroute 交叉連線： `microsoft.network/expressroutecrossconnections` 和局域網路閘道： `microsoft.network/localnetworkgateways` 。 |
  | [`Deploy-Budget-Sandbox`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)/) /.AzState)  | 確保每個沙箱訂用帳戶的預算都有，並啟用電子郵件警示。 在每個訂用帳戶中，預算會命名為： `default-sandbox-budget` 。 | 如果在指派原則時，不會修改參數的預設值，就會 `default-sandbox-budget` 使用1000貨幣閾值限制來建立預算 () ，並根據 Azure 角色指派) 以90% 和100% 的預算臨界值，將電子郵件警示傳送給訂用帳戶的擁有者和參與者 (。 |

### <a name="global-networking-and-connectivity"></a>全球網路和連線能力

1. 針對將部署虛擬中樞和虛擬網路的每個 Azure 區域，配置適當的虛擬網路 CIDR 範圍。

2. 如果您決定透過 Azure 原則建立網路資源，請將下表所列的原則指派給連線訂用帳戶。 如此一來，Azure 原則便可確保根據提供的參數來建立下列清單中的資源。
   - 建立 Azure 虛擬 WAN 標準實例。
   - 為每個區域建立 Azure 虛擬 WAN 虛擬中樞。 請確定每個虛擬中樞都至少部署一個閘道 (Azure ExpressRoute 或 VPN) 。
   - 藉由在每個虛擬中樞內部署 Azure 防火牆來保護虛擬中樞。
   - 建立所需的 Azure 防火牆原則，並將其指派給安全的虛擬中樞。
   - 確定所有連線到安全虛擬中樞的虛擬網路都受到 Azure 防火牆的保護。

3. 部署及設定 Azure 私人 DNS 區域。

4. 使用 Azure 私人對等互連布建 ExpressRoute 線路。 遵循 [建立和修改 ExpressRoute 線路對等互連](/azure/expressroute/expressroute-howto-routing-portal-resource-manager#private)的指示。

5. 透過 ExpressRoute 線路，將內部部署 HQs/Dc 連線到 Azure 虛擬 WAN 虛擬中樞。

6. 使用 (Nsg) 的網路安全性群組，保護跨虛擬中樞的虛擬網路流量。

7.  (選擇性) 透過 ExpressRoute 私用對等互連設定加密。 遵循 [expressroute 加密：適用于虛擬 WAN 的 expressroute 的 IPsec](/azure/virtual-wan/vpn-over-expressroute)中的指示。

8.  (選擇性) 透過 VPN 將分支連線至虛擬中樞。 遵循 [使用 Azure 虛擬 WAN 建立站對站](/azure/virtual-wan/virtual-wan-site-to-site-portal)連線的指示。

9. 當有多個內部部署位置透過 ExpressRoute 連接到 Azure 時， (選擇性) 設定 ExpressRoute Global 觸及以連接內部部署 HQs/Dc。 遵循 [設定 ExpressRoute Global 觸及範圍](/azure/expressroute/expressroute-howto-set-global-reach)中的指示。

下列清單顯示當您在執行企業規模部署的網路資源時，所使用的 Azure 原則指派：

| 名稱                     | 描述                                                                            |
|--------------------------|----------------------------------------------------------------------------------------|
| [`Deploy-FirewallPolicy`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.) 上的 AzState/Microsoft.Authorization_policyDefinitions-Deploy-FirewallPolicy.parameters.js | 建立防火牆原則。 |
| [`Deploy-VHub`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.) 上的 AzState/Microsoft.Authorization_policyDefinitions-Deploy-vHUB.parameters.js | 此原則會部署虛擬中樞、Azure 防火牆和 VPN/ExpressRoute 閘道。 它也會在連線的虛擬網路上設定 Azure 防火牆的預設路由。 |
| [`Deploy-VWAN`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.) 上的 AzState/Microsoft.Authorization_policyDefinitions-Deploy-vWAN.parameters.js | 部署虛擬 WAN。 |

### <a name="security-governance-and-compliance"></a>安全性、治理和合規性

1. 定義並套用 [服務啟用架構](./security-governance-and-compliance.md#service-enablement-framework) ，以確保 Azure 服務符合企業安全性和治理需求。

2. 建立自訂角色定義。

3. 啟用 Azure AD PIM，並探索 Azure 資源以加速 PIM。

4. 使用 Azure AD PIM 建立 azure 控制平面資源管理的僅限 Azure AD 群組。

5. 套用下表所列的原則，以確保 Azure 服務符合企業需求。

6. 定義命名慣例，並透過 Azure 原則強制執行。

7. 在所有範圍建立原則矩陣 (例如，透過 Azure 原則) 啟用所有 Azure 服務的監視。

您應使用下列原則來強制執行全公司的合規性狀態。

| 名稱                       | 描述                                                        |
|----------------------------|--------------------------------------------------------------------|
| [`Allowed-ResourceLocation`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.) 上的 AzState/Microsoft.Authorization_policyAssignments-Allowed-ResourceLocation.parameters.js   | 指定可在其中部署資源的允許區域。 |
| [`Allowed-RGLocation`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.) 上的 AzState/Microsoft.Authorization_policyAssignments-Allowed-RGLocation.parameters.js         | 指定可在其中部署資源群組的允許區域。 |
| [`Denied-Resources`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.) 上的 AzState/Microsoft.Authorization_policyAssignments-Denied-Resources.parameters.js           | 公司拒絕的資源。 |
| [`Deny-AppGW-Without-WAF`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.) 上的 AzState/Microsoft.Authorization_policyDefinitions-Deny-AppGW-Without-WAF.parameters.js     | 允許使用 Azure Web 應用程式防火牆部署的應用程式閘道 (WAF) 啟用。 |
| [`Deny-IP-Forwarding`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.) 上的 AzState/Microsoft.Authorization_policyAssignments-Deny-IP-Forwarding.parameters.js         | 拒絕 IP 轉送。 |
| [`Deny-RDP-From-Internet`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.) 上的 AzState/Microsoft.Authorization_policyAssignments-Deny-RDP-From-Internet.parameters.js     | 拒絕來自網際網路的 RDP 連接。 |
| [`Deny-Subnet-Without-Nsg`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.) 上的 AzState/Microsoft.Authorization_policyDefinitions-Deny-Subnet-Without-Nsg.parameters.js    | 拒絕建立子網，而不使用 NSG。 |
| [`Deploy-ASC-CE`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.) 上的 AzState/Microsoft.Authorization_policyDefinitions-Deploy-ASC-CE.parameters.js              | 設定 Azure 安全性中心對 Log Analytics 工作區的連續匯出。 |
| [`Deploy-ASC-Monitoring`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.) 上的 AzState/Microsoft.Authorization_policyAssignments-Deploy-ASC-Monitoring.parameters.js      | 啟用安全中心的監視功能。 |
| [`Deploy-ASC-Standard`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.) 上的 AzState/Microsoft.Authorization_policyDefinitions-Deploy-ASC-Standard.parameters.js        | 確定訂用帳戶已啟用安全中心標準。 |
| [`Deploy-Diag-ActivityLog`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.) 上的 AzState/Microsoft.Authorization_policyDefinitions-Deploy-Diagnostics-ActivityLog.parameters.js | 啟用診斷活動記錄，並轉送至 Log Analytics。 |
| [`Deploy-Diag-LogAnalytics`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.) 上的 AzState/Microsoft.Authorization_policyDefinitions-Deploy-Log-Analytics.parameters.js | |
| [`Deploy-VM-Monitoring`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.) 上的 AzState/Microsoft.Authorization_policyDefinitions-Deploy-Diagnostics-VM.parameters.js | 確保已啟用 VM 監視。 |

### <a name="platform-identity"></a>平臺身分識別

1. 如果您透過 Azure 原則建立身分識別資源，請將下表所列的原則指派給身分識別訂用帳戶。 如此一來，Azure 原則便可確保根據提供的參數來建立下列清單中的資源。

2. 部署 Active Directory 網域控制站。

下列清單顯示當您在執行企業規模部署的身分識別資源時，可以使用的原則。

| 名稱                         | 描述                                                               |
|------------------------------|---------------------------------------------------------------------------|
| [`DataProtectionSecurityCenter`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.AzState/)  | 由安全性中心自動建立的資料保護。 |
| [`Deploy-VNet-Identity`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.) 上的 AzState/Microsoft.Authorization_policyDefinitions-Deploy-vNet.parameters.js | 將虛擬網路部署到身分識別訂用帳戶中，以裝載 (例如 DC) 。 |

### <a name="platform-management-and-monitoring"></a>平臺管理與監視

1. 針對組織和以資源為中心的視圖建立原則合規性和安全性儀表板。

2. 建立平臺秘密 (服務主體和自動化帳戶) 和金鑰變換的工作流程。

3. 在 Log Analytics 中設定記錄檔的長期封存和保留期。

4. 設定 Azure Key Vault 來儲存平臺秘密。

5. 如果您透過 Azure 原則建立平臺管理資源，請將下表所列的原則指派給管理訂用帳戶。 如此一來，Azure 原則便可確保根據提供的參數來建立下列清單中的資源。

| 名稱                   | 描述                                                                            |
|------------------------|----------------------------------------------------------------------------------------|
| [`Deploy-LA-Config`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.) 上的 AzState/Microsoft.Authorization_policyDefinitions-Deploy-LA-Config.parameters.js | Configuration Log Analytics 工作區。 |
| [`Deploy-Log-Analytics`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.) 上的 AzState/Microsoft.Authorization_policyDefinitions-Deploy-Log-Analytics.parameters.js | 部署 Log Analytics 工作區。 |

## <a name="file---new---region"></a>檔案-> 新 > 區域

1. 如果您透過 Azure 原則建立網路資源，請將下表所列的原則指派給連線訂用帳戶。 如此一來，Azure 原則便可確保根據提供的參數來建立下列清單中的資源。

    - 在連接訂用帳戶中，于現有的虛擬 WAN 內建立新的虛擬中樞。
    - 安全虛擬中樞，方法是在虛擬中樞內部署 Azure 防火牆，並將現有或新的防火牆原則連結到 Azure 防火牆。
    - 確定所有連線到安全虛擬中樞的虛擬網路都受到 Azure 防火牆的保護。

2. 透過 ExpressRoute 或 VPN 將虛擬中樞連線至內部部署網路。

3. 透過 Nsg 保護跨虛擬中樞的虛擬網路流量。

4.  (選擇性) 透過 ExpressRoute 私用對等互連設定加密。

| 名稱                     | 描述                                                                            |
|--------------------------|----------------------------------------------------------------------------------------|
| [`Deploy-VHub`](https://github.com/Azure/Enterprise-Scale/tree/main/azopsreference/3fc1081d-6105-4e19-b60c-1ec1252cf560%20(3fc1081d-6105-4e19-b60c-1ec1252cf560)) /.) 上的 AzState/Microsoft.Authorization_policyDefinitions-Deploy-vHUB.parameters.js | 此原則會將虛擬中樞、Azure 防火牆和閘道部署 (VPN/ExpressRoute) 。 它也會在連線的虛擬網路上設定 Azure 防火牆的預設路由。 |

<!-- docutune:disable -->

## <a name="file---new---landing-zone-for-applications-and-workloads"></a>適用于應用程式和工作負載的檔案 > 新 > 登陸區域

<!-- docutune:enable -->

1. 建立訂用帳戶，並將它移至 `Landing Zones` 管理群組範圍下。

2. 建立訂用帳戶的 Azure AD 群組，例如 `Owner` 、 `Reader` 和 `Contributor` 。

3. 為已建立的 Azure AD 群組建立 Azure AD PIM 權利。
