---
title: 周邊網路
description: 周邊網路
author: tracsman
ms.author: jonor
ms.date: 05/10/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: ready
manager: rossort
tags: azure-resource-manager
ms.custom: virtual-network
ms.openlocfilehash: 6125a428d67130d891623a30ca75a11527fbbe23
ms.sourcegitcommit: 2362fb3154a91aa421224ffdb2cc632d982b129b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/28/2020
ms.locfileid: "76799772"
---
# <a name="perimeter-networks"></a>周邊網路

[周邊網路][perimeter-network]可在雲端網路與內部部署或實體資料中心網路之間啟用安全連線，以及啟用任何進出網際網路的連線。 它們也稱為非管制區域 (DMZ)。

若要讓周邊網路有效，連入封包必須流經安全子網中裝載的安全性應用裝置，然後才能到達後端伺服器。 範例包括防火牆、入侵偵測系統 (IDS) 和入侵預防系統 (IP)。 來自工作負載的網際網路繫結封包應該也會先流經周邊網路中的安全性設備，然後才會離開網路。 此流程的目的是為了強制執行原則，以及進行檢查和稽核。

周邊網路會利用下列 Azure 功能和服務：

- [虛擬網路][virtual-networks]、[使用者定義的路由][user-defined-routes]和[網路安全性群組][network-security-groups]
- [網路虛擬裝置（Nva）][NVA]
- [Azure 負載平衡器][ALB]
- [Azure 應用程式閘道][AppGW]和[web 應用程式防火牆（WAF）][AppGWWAF]
- [公用 IP][PIP]
- 使用 [Web 應用程式防火牆][AFDWAF]的 [Azure Front Door][AFD]
- [Azure 防火牆][AzFW]

> [!NOTE]
> Azure 參考架構提供範例範本，可讓您用來實作您自己的周邊網路：
>
> - [實作 Azure 與內部部署資料中心之間的 DMZ](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid)
> - [實作 Azure 與網際網路之間的 DMZ](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-dmz?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)

通常，您的中央 IT 和安全性小組會負責定義操作周邊網路的需求。

![中樞和輪輻網路拓撲的範例][7]

上圖顯示的範例[中樞和輪輻網路拓撲](./hub-spoke-network-topology.md)，會執行可存取網際網路和內部部署網路的兩個周邊。 這兩個周邊都位於 DMZ 中樞內。 在 DMZ 中樞中，網際網路的周邊網路可以使用多個 WAF 的伺服器陣列和 Azure 防火牆執行個體來相應增加，以支援許多企業營運 (Lob)，協助保護支點虛擬網路。 中樞也可以視需要透過 VPN 或 Azure ExpressRoute 進行連線。

## <a name="virtual-networks"></a>虛擬網路

周邊網路通常是使用具有多個子網路的[虛擬網路][virtual-networks]來建置，以裝載不同類型的服務，透過 NVA、WAF 和 Azure 應用程式閘道來篩選及檢查進出網際網路的流量。

## <a name="user-defined-routes"></a>使用者定義路由

藉由使用[使用者定義的路由][user-defined-routes]，客戶可以部署防火牆、IDS/IPS 和其他虛擬設備。 然後，客戶可以透過這些安全性設備來路由傳送網路流量，以進行安全性界限原則強制執行、稽核及檢查。 您可以建立使用者定義的路由，以確保流量會通過指定的自訂 VM、NVA 和負載平衡器。

在中樞和輪輻網路範例中，確保位於輪輻中的虛擬機器所產生的流量通過中樞內正確的虛擬裝置，需要在輪輻的子網中定義使用者定義的路由。 此路由會將內部負載平衡器的前端 IP 位址設定為下一個躍點。 內部負載平衡器會將內部流量分散到虛擬設備 (負載平衡器後端集區)。

## <a name="azure-firewall"></a>Azure 防火牆

[Azure 防火牆][AzFW]是受控雲端式服務，可協助保護您的 Azure 虛擬網路資源。 它是完全具狀態的受控防火牆，具備內建的高可用性和不受限制的雲端擴充性。 您可以橫跨訂用帳戶和虛擬網路集中建立、強制執行以及記錄應用程式和網路連線原則。

Azure 防火牆會為您的虛擬網路資源提供靜態公用 IP 位址。 這可讓外部防火牆識別來自您虛擬網路的流量。 服務會與 Azure 監視器互通以進行記錄和分析。

## <a name="network-virtual-appliances"></a>網路虛擬設備

具有網際網路存取權的周邊網路通常是透過 Azure 防火牆執行個體或防火牆的伺服器陣列或 [Web 應用程式防火牆][AFDWAF]來管理。

不同的 LOB 通常會使用許多 Web 應用程式。 這些應用程式通常很容易受到各種弱點和潛在攻擊的影響。 Web 應用程式防火牆會偵測對 Web 應用程式 (HTTP/HTTPS) 的攻擊，比一般防火牆更深入。 與傳統防火牆技術相較之下，Web 應用程式防火牆具有一組特定功能，可協助保護內部網頁伺服器免於威脅。

Azure 防火牆執行個體和[網路虛擬設備][NVA]防火牆使用一般的系統管理平面，其中包含一組安全性規則，可協助保護支點中所裝載的工作負載，並控制對內部部署網路的存取。 Azure 防火牆具有內建的擴充性，而 NVA 防火牆可以在負載平衡器後方手動調整。

相較於 WAF，防火牆伺服器陣列的特殊軟體通常比較少，但它有更廣泛的應用程式範圍，可篩選和檢查輸出和輸入中的任何類型流量。 如果您使用 NVA 方法，可以從 Azure Marketplace 尋找並部署軟體。

針對源自網際網路的流量使用一組 Azure 防火牆執行個體 (或 NVA)，針對源自內部部署的流量使用另一組。 對兩者僅使用一組防火牆會造成安全性風險，因為這並未提供兩組網路流量之間的安全性周邊。 使用個別的防火牆層級可降低檢查安全性規則的複雜度，並明確指出哪些規則對應到哪個傳入網路要求。

## <a name="azure-load-balancer"></a>Azure Load Balancer

[Azure Load Balancer][ALB] 提供高可用性層級 4 (TCP、UDP) 服務，可將連入流量分散到負載平衡組中所定義的服務執行個體。 從前端端點 (公用 IP 端點或私人 IP 端點) 傳送到負載平衡器的流量，不論是否有位址轉譯，都可以轉散發至後端 IP 位址集區 (例如 NVA 或 VM)。

Azure Load Balancer 也可以探查各種伺服器執行個體的健全狀況。 當執行個體無法回應探查時，負載平衡器會停止將流量傳送至狀況不良的執行個體。

如需使用中樞和輪輻網路拓撲的範例，您可以將外部負載平衡器部署至中樞和輪輻。 在中樞中，負載平衡器會有效率地將流量路由傳送到支點中的服務。 在支點中，負載平衡器會管理應用程式流量。

## <a name="azure-front-door-service"></a>Azure Front Door Service

[Azure Front Door Service][AFD] 是 Microsoft 的高可用性和可擴充 Web 應用程式加速平台和全域 HTTPS 負載平衡器。 您可以使用 Azure Front Door Service 來建置、操作及相應放大您的動態 Web 應用程式和靜態內容。 它會在 Microsoft 全球網路邊緣的 100 多個位置中執行。

Azure Front Door Service 為您的應用程式提供統一的區域/戳記維護自動化、BCDR 自動化、統一的用戶端/使用者資訊、快取和服務見解。 平台提供效能、可靠性和支援 SLA。 它也提供合規性認證，以及由 Azure 原生開發、操作及支援的可稽核安全性做法。

## <a name="application-gateway"></a>應用程式閘道

[Azure 應用程式閘道][AppGW]是專用的虛擬設備，可提供受控應用程式傳遞控制器 (ADC)。 它會為您的應用程式提供各種第 7 層負載平衡功能。

應用程式閘道會將 CPU 密集 SSL 終止卸載至應用程式閘道，讓客戶最佳化 Web 伺服陣列的產能。 它也提供其他第 7 層路由功能，包括循環配置傳入流量、以 Cookie 為基礎的工作階段同質性、URL 路徑型路由，以及在單一應用程式閘道背後代管多個網站的能力。

應用程式閘道 WAF SKU 包含 Web 應用程式防火牆。 此 SKU 會保護 Web 應用程式免於遭遇常見的 Web 弱點和攻擊。 您可以將應用程式閘道設定為面向網際網路的閘道、內部專用閘道或兩者的組合。

## <a name="public-ips"></a>公用 IP

您可以利用某些 Azure 功能建立服務端點與[公用 IP][PIP] 位址的關聯，以便從網際網路存取資源。 此端點會使用網路位址轉譯 (NAT) 將流量路由至 Azure 虛擬網路的內部位址和連接埠。 這個路徑是外部流量進入虛擬網路的主要方式。 您可以設定公用 IP 位址以決定要傳入的流量，以及該流量在虛擬網路上的轉譯方式和目的地。

## <a name="azure-ddos-protection-standard"></a>Azure DDoS Protection Standard

[Azure DDoS 保護標準][DDoS]提供[基本服務][DDoS]層以外特別針對 Azure 虛擬網路資源調整的額外安全防護功能。 「DDoS 保護標準」很容易啟用，而且不需要變更應用程式。

您可以透過專用的流量監視和機器學習演算法來調整保護原則。 原則會套用至與虛擬網路中部署的資源相關聯的公用 IP 位址。 例如 Azure Load Balancer、Azure 應用程式閘道和 Azure Service Fabric 執行個體。

在攻擊期間和針對歷史記錄目的，可透過 Azure 監視器檢視取得即時遙測。 您可以使用 Azure 應用程式閘道中的 Web 應用程式防火牆來新增應用層保護。 針對 IPv4 Azure 公用 IP 位址提供保護。

<!-- images -->

[0]: ../../_images/azure-best-practices/network-redundant-equipment.png "元件重疊範例"
[1]: ../../_images/azure-best-practices/network-hub-spoke-high-level.png "高階中樞和輪輻範例"
[2]: ../../_images/azure-best-practices/network-hub-spokes-cluster.png "中樞和支點叢集"
[3]: ../../_images/azure-best-practices/network-spoke-to-spoke.png "支點對支點"
[4]: ../../_images/azure-best-practices/network-hub-spoke-block-level-diagram.png "中樞支點的區塊層級圖表"
[5]: ../../_images/azure-best-practices/network-users-groups-subscriptions.png "使用者、群組、訂用帳戶和專案"
[6]: ../../_images/azure-best-practices/network-infrastructure-high-level.png "高階基礎結構圖"
[7]: ../../_images/azure-best-practices/network-high-level-perimeter-networks.png "高階基礎結構圖"
[8]: ../../_images/azure-best-practices/network-vnet-peering-perimeter-networks.png "VNet 對等和周邊網路"
[9]: ../../_images/azure-best-practices/network-high-level-diagram-monitoring.png "高階監視圖"
[10]: ../../_images/azure-best-practices/network-high-level-workloads.png "高階工作負載圖"

<!-- links -->

[Limits]: https://docs.microsoft.com/azure/azure-subscription-service-limits
[Roles]: https://docs.microsoft.com/azure/role-based-access-control/built-in-roles
[virtual-networks]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview
[network-security-groups]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg
[DNS]: https://docs.microsoft.com/azure/dns/dns-overview
[PrivateDNS]: https://docs.microsoft.com/azure/dns/private-dns-overview
[VNetPeering]: https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview
[user-defined-routes]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview
[RBAC]: https://docs.microsoft.com/azure/role-based-access-control/overview
[azure-ad]: https://docs.microsoft.com/azure/active-directory/active-directory-whatis
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways
[ExR]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction
[ExRD]: https://docs.microsoft.com/azure/expressroute/expressroute-erdirect-about
[vWAN]: https://docs.microsoft.com/azure/virtual-wan/virtual-wan-about
[NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha
[AzFW]: https://docs.microsoft.com/azure/firewall/overview
[SubMgmt]: https://docs.microsoft.com/azure/architecture/cloud-adoption/reference/azure-scaffold
[RGMgmt]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview
[perimeter-network]: https://docs.microsoft.com/azure/best-practices-network-security
[ALB]: https://docs.microsoft.com/azure/load-balancer/load-balancer-overview
[DDoS]: https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview
[PIP]: https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address
[AFD]: https://docs.microsoft.com/azure/frontdoor/front-door-overview
[AFDWAF]: https://docs.microsoft.com/azure/frontdoor/waf-overview
[AppGW]: https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction
[AppGWWAF]: https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview
[Monitor]: https://docs.microsoft.com/azure/monitoring-and-diagnostics/
[ActLog]: https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs
[DiagLog]: https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs
[nsg-log]: https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log
[OMS]: https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview
[NPM]: https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor
[NetWatch]: https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview
[WebApps]: https://docs.microsoft.com/azure/app-service/
[HDI]: https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-introduction
[EventHubs]: https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs
[ServiceBus]: https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview
[traffic-manager]: https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview
