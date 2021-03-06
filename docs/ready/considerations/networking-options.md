---
title: 檢查您的網路選項
description: 使用適用于 Azure 的雲端採用架構，瞭解如何找出登陸區域支援 Azure 工作負載所需的網路功能。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/15/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: internal
ms.openlocfilehash: 150ca96f40f6a2d068f837402f50f6e617e3ef5a
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101788686"
---
<!-- cSpell:ignore NVAs VPNs -->

# <a name="review-your-network-options"></a>檢查您的網路選項

設計和實作 Azure 網路功能是雲端採用工作的重要部分。 您必須進行網路設計決策，以適當地支援裝載於雲端的工作負載和服務。 Azure 網路產品和服務支援各式各樣的網路功能。 您如何針對您選擇的這些服務和網路架構建立結構，取決於貴組織的工作負載、控管和連線能力需求。

## <a name="identify-workload-networking-requirements"></a>識別工作負載網路需求

在登陸區域的評估和準備過程中，您需要識別登陸區域必須支援的網路功能。 此程序牽涉到評估組成工作負載的每個應用程式和服務，以判斷其連線網路控制需求。 在您識別並記錄需求之後，您可以為登陸區域建立原則，以根據您的工作負載需求來控制允許的網路資源和設定。

針對您要部署到登陸區域環境的每個應用程式或服務，請使用下列決策樹作為起點，以協助您判斷要使用的網路工具或服務：

![Azure 網路服務決策樹 ](../../_images/ready/network-decision-tree.png)
 _圖1： azure 網路服務決策樹。_

### <a name="key-questions"></a>重要問題

回答下列有關工作負載的問題，以協助您根據 Azure 網路服務決策樹來做出決策：

- **您的工作負載是否需要虛擬網路？** 受控平台即服務 (PaaS) 資源類型使用不一定需要虛擬網路的基礎平台網路功能。 如果您的工作負載不需要進階的網路功能，而且您不需要部署基礎結構即服務 (IaaS) 資源，則 [PaaS 資源所提供的預設原生網路功能](../../decision-guides/software-defined-network/paas-only.md)可能會符合您的工作負載連線能力和流量管理需求。
- **您的工作負載是否需要虛擬網路與內部部署資料中心之間的連線能力？** Azure 提供兩種解決方案來建立混合式網路功能： Azure VPN 閘道和 Azure ExpressRoute。 [AZURE VPN 閘道](/azure/vpn-gateway/vpn-gateway-about-vpngateways) 會透過站對站 vpn，將您的內部部署網路連線到 azure，類似于您可能會如何設定和連接至遠端分公司。 VPN 閘道的最大頻寬為 10 Gbps。 [Azure ExpressRoute](/azure/expressroute/expressroute-introduction) 使用 Azure 與您內部部署基礎結構之間的私人連線，提供更高的可靠性和較低的延遲。 ExpressRoute 的頻寬選項範圍從 50 Mbps 到 100 Gbps。
- **您是否需要使用內部部署網路裝置來檢查和稽核傳出流量？** 針對雲端原生工作負載，您可以使用 [Azure 防火牆](/azure/firewall/overview) 或裝載于雲端的協力廠商 [網路虛擬裝置 (nva) ](https://azure.microsoft.com/solutions/network-appliances) 來檢查和審核進出公用網際網路的流量。 但許多企業 IT 安全性原則都需要網際網路系結的連出流量，才能通過組織內部部署環境中集中管理的裝置。 [強制通道](/azure/virtual-network/virtual-networks-udr-overview)支援這些案例。 並非所有受控服務都支援強制通道。 服務和功能（例如 [Azure App service 中的 App Service 環境](/azure/app-service/environment/intro)、 [azure API 管理](/azure/api-management/api-management-key-concepts)、 [azure Kubernetes Service (AKS) ](/azure/aks/intro-kubernetes)、 [azure SQL 受控實例](/azure/sql-database/sql-database-managed-instance-index)、 [azure Databricks](/azure/azure-databricks/what-is-azure-databricks)和 [azure HDInsight](/azure/hdinsight) ）可在虛擬網路內部署服務或功能時支援此設定。
- **您需要連接多個虛擬網路嗎？** 您可以使用[虛擬網路對等互連](/azure/virtual-network/virtual-network-peering-overview)來連接多個 [Azure 虛擬網路](/azure/virtual-network/virtual-networks-overview)的執行個體。 對等互連可支援跨訂用帳戶和區域的連接。 針對您提供跨多個訂用帳戶共用的服務，或需要管理大量網路對等互連的案例，請考慮採用[中樞和輪輻網路架構](../../decision-guides/software-defined-network/hub-spoke.md)，或使用 [Azure虛擬 WAN](/azure/virtual-wan/virtual-wan-about)。 虛擬網路對等互連只會提供兩個對等互連網路之間的連線能力。 根據預設，它不會跨多個對等互連提供可轉移的連線能力。
- **您的工作負載是否可透過網際網路存取？** Azure 提供的服務是設計用來協助您管理及保護應用程式和服務的外部存取：
  - [Azure 防火牆](/azure/firewall/overview)
  - [網路設備](https://azure.microsoft.com/solutions/network-appliances)
  - [Azure Front Door](/azure/frontdoor/front-door-overview)
  - [Azure 應用程式閘道](/azure/application-gateway)
  - [Azure 流量管理員](/azure/traffic-manager/traffic-manager-overview)
- **您是否需要支援自訂 DNS 管理？** [Azure DNS](/azure/dns/dns-overview) 是適用於 DNS 網域的主機服務。 Azure DNS 使用 Azure 基礎結構來提供名稱解析。 如果您的工作負載需要的名稱解析超出 Azure DNS 所提供的功能，您可能需要部署其他解決方案。 如果您的工作負載也需要 Active Directory 服務，請考慮使用 [Azure Active Directory Domain Services](/azure/active-directory-domain-services/overview) 來增強 Azure DNS 功能。 如需更多的功能，您也可以[部署自訂 IaaS 虛擬機器](/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances)以支援您的需求。

## <a name="common-networking-scenarios"></a>常見的網路功能案例

Azure 網路是由提供不同網路功能的多項產品和服務所組成。 作為網路設計程序的一部分，您可以將工作負載需求與下表中的網路案例進行比較，以識別您可以用來提供這些網路功能的 Azure 工具或服務：

| 狀況 | 網路產品或服務 |
| --- | --- |
| 我需要網路基礎結構來連接所有項目，從虛擬機器到連入 VPN 連線。 | [Azure 虛擬網路](/azure/virtual-network) |
| 我需要對我的應用程式或服務進行輸入和輸出連線和要求的平衡。 | [Azure Load Balancer](/azure/load-balancer) |
| 我想要優化應用程式伺服器陣列的傳遞，同時提高應用程式安全性與 Web 應用程式防火牆。 | [Azure 應用程式閘道](/azure/application-gateway) <br> [Azure Front Door](/azure/frontdoor) |
| 我需要透過高效能 VPN 閘道，安全地使用網際網路來存取 Azure 虛擬網路。 | [Azure VPN 閘道](/azure/vpn-gateway) |
| 我想要確保所有網域需求的超快 DNS 回應和超高可用性。 | [Azure DNS](/azure/dns) |
| 我需要加速傳遞高頻寬內容給全球客戶，從應用程式和儲存內容到串流影片。 | [Azure 內容傳遞網路 (CDN)](/azure/cdn) |
| 我需要保護我的 Azure 應用程式免於遭受 DDoS 攻擊。 | [Azure DDoS 保護](/azure/virtual-network/ddos-protection-overview) \(部分機器翻譯\) |
| 我需要以最佳方式將流量分散到全球 Azure 區域的服務，同時提供高可用性和回應性。 | [Azure 流量管理員](/azure/traffic-manager) <br><br> [Azure Front Door](/azure/frontdoor) |
| 我需要新增私人網路連線，以從我的公司網路存取 Microsoft 雲端服務，如同存取我自己資料中心內的內部部署。 | [Azure ExpressRoute](/azure/expressroute) |
| 我需要在網路案例層級進行監視與診斷。 | [Azure 網路監看員](/azure/network-watcher) |
| 我需要原生防火牆功能，內建高可用性、不受限制的雲端擴充性及零維護。 | [Azure 防火牆](/azure/firewall/overview) |
| 我需要安全地連接商業辦公室、零售地點和網站。 | [Azure 虛擬 WAN](/azure/virtual-wan) |
| 我需要可調整並已加強安全性的傳遞點，適用於微服務型全域 Web 應用程式。 | [Azure Front Door](/azure/frontdoor) |

## <a name="choose-a-networking-architecture"></a>選擇網路架構

在您識別支援工作負載所需的 Azure 網路服務之後，您也需要設計結合這些服務的架構，以提供登陸區域的雲端網路基礎結構。 《雲端採用架構[軟體定義網路決策指南](../../decision-guides/software-defined-network/index.md)》提供一些在 Azure 上使用的最常見網路架構模式詳細資料。

下表摘要說明這些模式支援的主要案例：

| 狀況 | 建議的網路架構 |
| --- | --- |
| 所有部署至登陸區域的 Azure 裝載工作負載都將完全以 PaaS 為基礎，不需要虛擬網路，且不屬於更廣泛的雲端採用工作，包括 IaaS 資源。                                                                                                                                                          | [僅限 PaaS](../../decision-guides/software-defined-network/paas-only.md)            |
| Azure 裝載的工作負載將會部署 IaaS 型資源 (例如虛擬機器)，否則需要虛擬網路，但不需要連線到您的內部部署環境。                                                                                                                                                                            | [雲端原生](../../decision-guides/software-defined-network/cloud-native.md)      |
| 您的 Azure 裝載工作負載需要有限的內部部署資源存取權，但您必須將雲端連線視為不受信任。                                                                                                                                                                                                                             | [雲端 DMZ](../../decision-guides/software-defined-network/cloud-dmz.md)            |
| 您的 Azure 裝載工作負載需要有限的內部部署資源存取權，而且您計劃在雲端與內部部署環境之間實作成熟的安全性原則和安全的連線。                                                                                                                                                           | [混合式](../../decision-guides/software-defined-network/hybrid.md)                  |
| 您需要部署及管理大量的 VM 和工作負載，可能會超過 [Azure 訂用帳戶限制](/azure/azure-resource-manager/management/azure-subscription-service-limits)，您需要跨訂用帳戶共用服務，或者您需要更多的角色、應用程式結構或許可權隔離。 | [中樞與輪幅](../../decision-guides/software-defined-network/hub-spoke.md)        |
| 您有許多分公司需要彼此連線與連線至 Azure。                                                                                                                                                                                                                                                                                         | [Azure 虛擬 WAN](/azure/virtual-wan/virtual-wan-about) |

<!-- TODO: Refactor VDC content below. -->
<!-- docutune:casing "Azure Virtual Datacenter" -->

### <a name="azure-virtual-datacenter"></a>Azure 虛擬資料中心

除了使用其中一種架構模式，如果您的企業 IT 小組管理大型雲端環境，請考慮諮詢 [CAF 企業規模的登陸區域](../../ready/enterprise-scale/index.md)。 當您設計以 Azure 為基礎的雲端基礎結構時，如果您在24個月內有長期目標 (，就能將 CAF 企業規模的登陸區域提供給網路、安全性、管理和基礎結構的組合，) **裝載超過1000的資產 (應用程式、基礎結構或資料資產) 在雲端中**。

對於符合下列準則的組織，您也可以開始使用 [CAF 企業級登陸區域](../../ready/enterprise-scale/index.md)：

- 貴企業受到法規合規性需求規範，而需要集中的監視和稽核功能。
- 您需要維護一般原則和治理合規性，以及對核心服務的集中式 IT 控制。
- 您的行業依賴復雜的平台，需要復雜的控制項和深入網域的專業知識來管理平台。 這在金融、石油和天然氣或製造業的大型企業中最為常見。
- 現有的 IT 治理原則需要與現有功能更加緊密地保持對應，即使在早期階段採用期間也是如此。

## <a name="follow-azure-networking-best-practices"></a>遵循 Azure 網路最佳做法

在您的網路設計過程中，請參閱下列文章：

- [虛擬網路規劃](/azure/virtual-network/virtual-network-vnet-plan-design-arm?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。 了解如何根據您的隔離、連線和位置需求規劃虛擬網路。
- [適用於網路安全性的 Azure 最佳做法](/azure/security/fundamentals/network-best-practices?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。 深入了解可協助您增強網路安全性的 Azure 最佳做法。
- [將工作負載遷移至 Azure 時的網路最佳做法](/azure/migrate/migrate-best-practices-networking?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。 取得如何實作 Azure 網路以支援 IaaS 型和 PaaS 型工作負載的其他指引。
