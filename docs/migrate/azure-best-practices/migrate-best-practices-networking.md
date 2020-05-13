---
title: 針對遷移至 Azure 的工作負載設定網路功能
description: 使用適用于 Azure 的雲端採用架構來瞭解為您的已遷移工作負載設定網路的最佳做法。
author: BrianBlanchard
ms.author: brblanch
ms.date: 12/04/2018
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 75a89a0020b4c0ae7ab336ab0d3789bf5096dab7
ms.sourcegitcommit: 60d8b863d431b5d7c005f2f14488620b6c4c49be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83222397"
---
<!-- cSpell:ignore NSGs CIDR FQDNs BGP's ACLs WAFs -->

# <a name="best-practices-to-set-up-networking-for-workloads-migrated-to-azure"></a>針對遷移至 Azure 的工作負載來設定網路的最佳做法

在規劃和設計移轉時，除了移轉作業本身外，其中一個最重要的步驟便是設計和實作 Azure 網路。 本文說明在移轉至 Azure 中的 IaaS 和 PaaS 實作時，該如何設定網路的最佳做法。

> [!IMPORTANT]
> 本文所述的最佳做法和意見是以本文撰寫當下可用的 Azure 平台和服務功能作為基礎。 特色與功能會隨著時間改變。 這些建議不一定全都適用於您的部署，因此請選取適合您部署的建議。

## <a name="design-virtual-networks"></a>設計虛擬網路

Azure 會提供虛擬網路 (VNet)：

- Azure 資源會透過 VNet 以私人、直接且安全地方式相互通訊。
- 您可以在 VNet 上為需要網際網路通訊的 VM 和服務設定端點連線。
- VNet 是專屬於您訂用帳戶的 Azure 雲端邏輯隔離。
- 您可以實作每個 Azure 訂用帳戶和 Azure 區域內的多個 VNet。
- 每個 VNet 會與其他 VNet 隔離。
- VNet 可以包含 [RFC 1918](https://tools.ietf.org/html/rfc1918) 中所定義的私人和公用 IP 位址 (以 CIDR 標記法表示)。 您無法直接從網際網路存取 VNet 位址空間中指定的公用 IP 位址。
- VNet 可使用 VNet 對等互連彼此連線。 所連線的 VNet 可位於相同或不同區域。 因此，某一個 VNet 中的資源可以連線至其他 VNet 中的資源。
- Azure 依預設會在 VNet 中的子網路、所連線的 VNet、內部部署網路和網際網路之間路由流量。

在規劃 VNet 拓撲時，您應考慮幾如何安排 IP 位址空間、如何實作中樞和輪輻網路、如何將 VNet 劃分成子網路、設定 DNS，以及實作 Azure 可用性區域。

## <a name="best-practice-plan-ip-addressing"></a>最佳做法：規劃 IP 位址

在移轉過程中建立 VNet 時，請務必要規劃 VNet IP 位址空間。

- 對每個 VNet 所指派的位址空間不應大於 /16 的 CIDR 範圍。 Vnet 允許使用 65536 IP 位址，並指派較小的前置詞（例如具有131072位址的/15），會導致多餘的 IP 位址無法在別處變成無法使用。 務必不要浪費 IP 位址，即使其位於 RFC 1918 所定義的私人範圍內也是如此。
- VNet 位址空間不得與內部部署網路範圍重疊。
- 請勿使用網路位址轉譯 (NAT)。
- 位址重疊可能會導致網路無法連線且路由無法正常運作。 如果網路重疊，您必須重新設計網路，或使用網路位址轉譯 (NAT)。

**瞭解更多資訊：**

- 取得[Azure 虛擬網路的總覽](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)。
- 閱讀[Azure 虛擬網路的常見問題](https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq)。
- 瞭解[Azure 網路限制](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits)。

## <a name="best-practice-implement-a-hub-and-spoke-network-topology"></a>最佳做法：執行中樞和輪輻網路拓撲

中樞和輪輻網路拓撲會在共用服務 (例如身分識別和安全性) 時隔離工作負載。

- 中樞是作為連線中心點的 Azure VNet。
- 輪輻是會使用 VNet 對等互連來連線至中樞 VNet 的 VNet。
- 共用的服務會部署在中樞中，而個別的工作負載會部署為輪輻。

請考慮下列事項：

- 在 Azure 中實作中樞和輪輻拓撲可集中管理常見服務，例如，內部部署網路的連線、防火牆以及 VNet 之間的隔離。 中樞 VNet 可作為中心點來連線至內部部署網路，也可用來裝載工作負載 (裝載於輪輻 VNet 中) 所使用的服務。
- 大型企業通常會使用中樞加上輪輻的組態。 若網路較小，則可能要考慮簡化設計，以節省成本和降低複雜度。
- 輪輻 VNet 可用來隔離工作負載，其中的每個輪輻會彼此分開地進行管理。 每個工作負載都會有多個層級，以及透過 Azure 負載平衡器連線的多個子網路。
- 中樞和輪輻 VNet 不僅可以在不同資源群組中實作，即使在不同訂用帳戶也可以。 當您將不同訂用帳戶中的虛擬網路對等互連時，這些訂用帳戶可以與相同或不同的 Azure Active Directory (Azure AD) 租用戶相關聯。 如此可對每個工作負載進行非集中式管理，同時在中樞網路中維護共用服務。

![變更管理](./media/migrate-best-practices-networking/hub-spoke.png)
_中樞和輪輻拓撲_

**瞭解更多資訊：**

- [了解](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)中樞和輪輻拓撲。
- 取得用來執行 Azure [Windows](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/windows-vm) 和 [Linux](https://docs.microsoft.com/azure/architecture/reference-architectures/n-tier/linux-vm) VM 的網路建議。
- [了解](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) VNet 對等互連。

## <a name="best-practice-design-subnets"></a>最佳做法：設計子網

若要在 VNet 中提供隔離機制，您可以將 VNet 劃分成一或多個子網路，並對每個子網路配置一部分的 VNet 位址空間。

- 您可以在每個 VNet 中建立多個子網路。
- 根據預設，Azure 會在 VNet 中的所有子網路之間路由傳送網路流量。
- 請根據技術和組織的需求來決定如何劃分子網路。
- 請使用 CIDR 表示法建立子網路。
- 在決定子網路的網路範圍時，務必請注意 Azure 會在每個子網路保留五個不得使用的 IP 位址。 例如，如果您建立最小的可用子網路 /29 (具有八個 IP 位址)，Azure 將會保留五個位址，因此您可以指派給子網路上主機的可用位址只剩三個。
- 在大部分的情況下，請使用/28 做為最小的子網。

**範例：**

下表所舉的 VNet 範例會將位址空間 10.245.16.0/20 劃分成子網路，以便進行計劃性移轉。

| **子網路** | **CIDR** | **位址** | **使用** |
| --- | --- | --- | --- |
| DEV-FE-EUS2 | 10.245.16.0/22 | 1019 | 前端/網路層 VM |
| DEV-APP-EUS2 | 10.245.20.0/22 | 1019 | 應用程式層 VM |
| DEV-DB-EUS2 | 10.245.24.0/23 | 507 | 資料庫 VM |

**瞭解更多資訊：**

- [了解](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm#segmentation)如何設計子網路。
- [了解](https://docs.microsoft.com/azure/migrate/contoso-migration-infrastructure)虛構公司 (Contoso) 如何讓其網路基礎結構做好移轉準備。

## <a name="best-practice-set-up-a-dns-server"></a>最佳做法：設定 DNS 伺服器

根據預設，Azure 會在您部署 VNet 時新增 DNS 伺服器。 這可讓您快速建置 VNet 和部署資源。 不過，此 DNS 伺服器只會對該 VNet 上的資源提供服務。 如果您想要將多個 VNet 彼此連線，或從 VNet 連線至內部部署伺服器，則還需要名稱解析功能。 例如，您可能需要 Active Directory 才能在虛擬網路之間解析 DNS 名稱。 若要這樣做，請在 Azure 中部署您自己的自訂 DNS 伺服器。

- 虛擬網路中的 DNS 伺服器可以將 DNS 查詢轉送到 Azure 中的遞迴解析程式。 這可讓您解析該虛擬網路內的主機名稱。 例如，在 Azure 中執行的網域控制站可以回應其自有網域的 DNS 查詢，並將所有其他查詢轉送到 Azure。
- DNS 轉送可讓 VM 查看您的內部部署資源 (透過網域控制站) 以及 Azure 提供的主機名稱 (使用轉送工具)。 您可以使用虛擬 IP 位址來存取 Azure 中的遞迴解析程式 `168.63.129.16` 。
- DNS 轉送也會實現 VNet 之間的 DNS 解析，並使內部部署電腦能夠解析 Azure 提供的主機名稱。
  - 為了解析 VM 主機名稱，DNS 伺服器 VM 必須位於同一個 VNet 中，且設定為將主機名稱查詢轉送到 Azure。
  - 因為每個 VNet 的 DNS 尾碼都不同，所以您可以使用條件性轉送規則來將 DNS 要求傳送到正確的 VNet 進行解析。
- 使用自有 DNS 伺服器時，您可以為每個 VNet 指定多個 DNS 伺服器。 您也可以對每個網路介面 (適用於 Azure Resource Manager) 或對每個雲端服務 (適用於傳統部署模型) 指定多個 DNS 伺服器。
- 針對網路介面或雲端服務所指定的 DNS 伺服器，優先順序高於針對 VNet 所指定的 DNS 伺服器。
- 在 Azure Resource Manager 部署模型中，您可以針 VNet 和網路介面指定 DNS 伺服器，但最佳做法是只在 VNet 上使用該設定。

    ![DNS 伺服器 VNet 的 DNS 伺服器](./media/migrate-best-practices-networking/dns2.png) __

**瞭解更多資訊：**

- [了解](https://docs.microsoft.com/azure/migrate/contoso-migration-infrastructure)您在使用自有 DNS 伺服器時的名稱解析。
- [了解](../../ready/azure-best-practices/naming-and-tagging.md) DNS 命名規則和限制。

## <a name="best-practice-set-up-availability-zones"></a>最佳做法：設定可用性區域

可用性區域可提升高可用性，讓應用程式和資料不因資料中心失敗而受影響。

- 「可用性區域」是 Azure 地區內獨特的實體位置。
- 每個區域皆由一或多個配備獨立電力、冷卻系統及網路的資料中心所組成。
- 若要確保復原，所有已啟用的地區中至少要有三個不同的區域。
- 某個地區內可用性區域的實體區隔可保護應用程式和資料不受資料中心故障影響。
- 區域備援服務會將應用程式和資料複寫至所有可用性區域，以防出現單一失敗點。 --使用可用性區域時，Azure 會提供 99.99% VM 運作時間的 SLA。

    ![可用性區域 可用性區域](./media/migrate-best-practices-networking/availability-zone.png) __

- 藉由將運算、儲存體、網路及資料資源共置於某個區域內並複寫至其他區域，您即可在移轉架構內規劃和建置高可用性。 支援可用性區域的 Azure 服務分成兩個類別：
  - **區域性服務：** 您會將資源與特定區域（例如 Vm、受控磁片或 IP 位址）建立關聯。
  - **區域冗余服務：** 資源會自動跨區域複寫，例如區域冗余儲存體或 Azure SQL Database。
- 您可以部署具有網際網路對向工作負載或應用程式層的標準 Azure 負載平衡器，以提供區域容錯。

    ![負載平衡器 負載平衡器](./media/migrate-best-practices-networking/load-balancer.png) __

**瞭解更多資訊：**

- [取得可用性區域概觀](https://docs.microsoft.com/azure/availability-zones/az-overview)。

## <a name="design-hybrid-cloud-networking"></a>設計混合式雲端網路

若要成功移轉，請務必將內部部署公司網路連線至 Azure。 這會建立稱為混合式雲端網路的 Always-On 連線，服務會從 Azure 雲端提供給公司使用者。 有兩個選項可供建立此類型的網路：

- **站對站 VPN：** 您會在相容的內部部署 VPN 裝置與 VNet 中部署的 Azure VPN 閘道之間建立站對站連線。 只要是獲得授權的內部部署資源就可以存取 VNet。 站對站通訊會在網際網路間透過加密通道傳送。
- **Azure ExpressRoute：** 您可以透過 ExpressRoute 合作夥伴，在您的內部部署網路與 Azure 之間建立 Azure ExpressRoute 連線。 這是私人連線，所以流量不會經過網際網路。

**瞭解更多資訊：**

- [深入了解](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/vpn)混合式雲端網路。

## <a name="best-practice-implement-a-highly-available-site-to-site-vpn"></a>最佳做法：執行高可用性的站對站 VPN

若要實作站對站 VPN，請在 Azure 中設定 VPN 閘道。

- VPN 閘道是一種特定類型的 VNet 閘道，可透過公用網際網路在 Azure VNet 與內部部署位置之間傳送加密的流量。
- VPN 閘道也可以透過 Microsoft 網路，在 Azure Vnet 之間傳送加密的流量。
- 每個 VNet 只能有一個 VPN 閘道。
- 您可以對相同的 VPN 閘道建立多個連線。 當您建立多個連線時，所有 VPN 通道都會共用可用的閘道頻寬。
- 每個 Azure VPN 閘道都是由作用中-待命組態中的兩個執行個體組成。
  - 當計劃性維護或非計劃性中斷影響到作用中執行個體時，便會發生容錯移轉，待命執行個體會自動接管，並繼續站對站或 VNet 對 VNet 的連線。
  - 切換時會造成短暫中斷。
  - 對於計劃性維護，應在 10 到 15 秒內還原連線。
  - 對於非計劃問題，連線復原需要更長時間，大約 1 到 1.5 分鐘 (最糟的情況)。
  - 對於閘道的點對站 (P2S) VPN 用戶端連線將會中斷，而使用者必須從用戶端電腦重新連線。

在設定站對站 VPN 時，請執行下列作業：

- 您需要一個 VNet，這個 VNet 的位址範圍沒有和 VPN 所要連線的內部部署網路重疊。
- 在網路中建立閘道子網路。
- 建立 VPN 閘道，指定閘道類型 (VPN) 以及閘道是原則式還是路由式的。 以路由為基礎的 VPN 會被視為更有能力和未來的證明。
- 建立內部部署的區域網路閘道，並設定內部部署 VPN 裝置。
- 在 VNet 閘道與內部部署裝置之間建立容錯移轉站對站 VPN 連線。 使用路由式 VPN 可讓您對 Azure 建立「主動-被動」或「主動-主動」連線。 路由式也同時支援站對站 (從任何電腦) 和點對站 (從單一電腦) 連線。
- 指定您想要使用的閘道 SKU。 這將取決於您的工作負載需求、輸送量、功能和 Sla。
- 邊界閘道協定 (BGP) 是選擇性功能，可與 Azure ExpressRoute 和路由式 VPN 閘道搭配使用，以將內部部署 BGP 路由傳播至 VNet。

![VPN](./media/migrate-best-practices-networking/vpn.png)
_站對站 VPN_

**瞭解更多資訊：**

- [檢閱](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices)相容的內部部署 VPN 裝置。
- [取得 VPN 閘道概觀](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)。
- [了解](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-highlyavailable)高可用性的 VPN 連線。
- [了解](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)如何規劃和設計 VPN 閘道。
- [檢閱](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings#gwsku) VPN 閘道設定。
- [檢閱](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways#gwsku)閘道 SKU。
- [了解](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-overview)如何設定 BGP 與 Azure VPN 閘道。

### <a name="best-practice-configure-a-gateway-for-vpn-gateways"></a>最佳做法：設定 VPN 閘道的閘道

當您在 Azure 中建立 VPN 閘道時，您必須使用名為 GatewaySubnet 的特殊子網路。 在建立這個子網路時，請注意下列最佳做法：

- 閘道子網路的前置長度最大可為 29 (例如，10.119.255.248/29)。 目前建議您使用 27 的前置長度 (例如，10.119.255.224/27)。
- 在定義閘道子網路的位址空間時，請使用 VNet 位址空間的最後一個部分。
- 在使用 Azure GatewaySubnet 時，請絕對不要將任何 VM 或其他裝置 (例如，應用程式閘道) 部署至閘道子網路。
- 請勿將網路安全性群組 (NSG) 指派到這個子網路。 這會導致閘道停止運作。

**瞭解更多資訊：**

- [使用此工具](https://gallery.technet.microsoft.com/scriptcenter/Address-prefix-calculator-a94b6eed)來判斷您的 IP 位址空間。

## <a name="best-practice-implement-azure-virtual-wan-for-branch-offices"></a>最佳做法：為分公司實行 Azure 虛擬 WAN

對於多個 VPN 連線，Azure 虛擬 WAN 可作為網路服務，透過 Azure 提供最佳且自動化的分支對分支連線。

- 虛擬 WAN 可讓您連線，並設定與 Azure 通訊的分支裝置。 做法是以手動方式，或透過虛擬 WAN 合作夥伴使用慣用的提供者裝置。
- 使用慣用的提供者裝置可讓您輕鬆地進行使用、連線和管理組態。
- Azure WAN 的內建儀表板可提供即時的疑難排解深入解析讓您節省時間，並可讓您輕鬆地追蹤大規模的站對站連線。

**深入瞭解：** 
[深入瞭解](https://docs.microsoft.com/azure/virtual-wan/virtual-wan-about)Azure 虛擬 WAN。

### <a name="best-practice-implement-expressroute-for-mission-critical-connections"></a>最佳做法：針對任務關鍵性連線執行 ExpressRoute

Azure ExpressRoute 服務可將內部部署基礎結構延伸至 Microsoft 雲端，您只要在虛擬 Azure 資料中心與內部部署網路之間建立私人連線即可達成。

- ExpressRoute 連線可透過任意對任意 (IP VPN) 網路、點對點乙太網路，也可以透過連線提供者來進行。 但不會經過公用網際網路。
- ExpressRoute 連線可提供更高的安全性、可靠性、速度 (最高 10 Gbps)，以及一致的延遲。
- ExpressRoute 很適合虛擬資料中心，因為客戶可以獲得與私人連線建立關聯之相容性規則的優點。
- 透過 ExpressRoute Direct，您可以在100Gbps 上直接連線到 Microsoft 路由器，以滿足較大的頻寬需求。
- ExpressRoute 會使用 BGP 在內部部署網路、Azure 執行個體與 Microsoft 公用位址之間交換路由。

部署 ExpressRoute 連線通常包含加入 ExpressRoute 服務提供者。 為快速入門，一開始通常會使用站對站 VPN 來建立虛擬資料中心與內部部署資源之間的連線，然後在與您的服務提供者建立實體互連時，遷移至 ExpressRoute 連線。

**瞭解更多資訊：**

- [閱讀 ExpressRoute 概觀](https://docs.microsoft.com/azure/expressroute/expressroute-introduction)。
- [了解](https://docs.microsoft.com/azure/expressroute/expressroute-erdirect-about) ExpressRoute Direct。

### <a name="best-practice-optimize-expressroute-routing-with-bgp-communities"></a>最佳做法：使用 BGP 社區優化 ExpressRoute 路由

當您有多個 ExpressRoute 線路時，會有一個以上的路徑來連線到 Microsoft。 因此，可能會產生次佳的路由，而且您的流量可能會經由較長的路徑連到 Microsoft，而 Microsoft 也可能會經由較長的路徑連到您的網路。 網路路徑愈常，延遲愈久。 延遲對於應用程式效能和使用者體驗有直接的影響。

**範例：**

讓我們檢閱一個範例：

- 您在美國有兩個辦公室，一個在洛杉磯，一個在紐約。
- 您的辦公室是在 WAN 上連線，該網路可以是您自己的骨幹網路或服務提供者的 IP VPN。
- 您有兩個也是在 WAN 上連線的 ExpressRoute 線路，一個在美國西部，一個在美國東部。 很明顯地，您有兩個路徑可連線到 Microsoft 網路。

**問題**

現在假設您在美國西部和美國東部均有 Azure 部署 (例如 Azure App Service)。

- 您希望每個辦公室的使用者存取與其最接近的 Azure 服務，以獲得最佳體驗。
- 因此，您希望將洛杉磯的使用者連線到 Azure 美國西部，將紐約的使用者連線到 Azure 美國東部。
- 這個想法在東岸使用者身上實現，但西岸使用者卻不行。 問題在於：
  - 在每個 ExpressRoute 線路上，我們會公告 Azure 美國東部的前置詞 (23.100.0.0/16)和 Azure 美國西部的前置詞 (13.100.0.0/16)。
  - 如果不知道前置詞來自哪個區域，就無法以不同方式處理前置詞。
  - 您的 WAN 網路可以假設這兩個前置詞較接近美國東部，離美國西部較遠，因此，會將這兩個辦公室的使用者路由至美國東部的 ExpressRoute 線路，而讓洛杉磯辦公室的使用者未能獲得最佳體驗。

![VPN](./media/migrate-best-practices-networking/bgp1.png)
_BGP 社群未最佳化的連線_

**解決方法：**

若要為這兩個辦公室的使用者最佳化路由，您需要知道哪個前置詞來自 Azure 美國西部以及哪個前置詞來自 Azure 美國東部。 您可以使用 BGP 社群值來編碼這項資訊。

- 您會將唯一的 BGP 社群值指派給每個 Azure 區域。 例如，12076:51004 適用於美國東部，12076:51006 適用於美國西部。
- 您現在已清楚哪個前置詞屬於哪個 Azure 區域，因此可以設定偏好的 ExpressRoute 線路。
- 因為您要使用 BGP 來交換路由資訊，您可以使用 BGP 的本機喜好設定來影響路由。
- 在我們的範例中，您會指派比美國東部還要高的本機喜好設定值給美國西部的 13.100.0.0/16，同樣地，指派比美國西部還要高的本機喜好設定值給美國東部的 23.100.0.0/16。
- 此組態可確保當Microsoft 的兩個路徑都可用時，洛杉磯的使用者會使用西部線路連線到 Azure 美國西部，而紐約的使用者會使用東部線路連線到 Azure 美國東部。 這兩端的路由均已最佳化。

![VPN](./media/migrate-best-practices-networking/bgp2.png)
_BGP 社群已最佳化的連線_

**瞭解更多資訊：**

- [深入瞭解](https://docs.microsoft.com/azure/expressroute/expressroute-optimize-routing)優化路由。

## <a name="secure-vnets"></a>安全 Vnet

Microsoft 與您須共同負責保護 VNet。 Microsoft 提供了許多網路功能以及服務，可協助保護資源的安全。 在設計 VNet 的安全性時，您應遵循一些最佳做法，包括實作周邊網路、使用篩選和安全性群組、保護對於資源和 IP 位址的存取，以及實作攻擊防範機制。

**瞭解更多資訊：**

- [取得網路安全性最佳做法的概觀](https://docs.microsoft.com/azure/security/fundamentals/network-best-practices)。
- [了解如何](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm#security)設計安全的網路。

## <a name="best-practice-implement-an-azure-perimeter-network"></a>最佳做法：執行 Azure 周邊網路

雖然 Microsoft 會大量投資在保護雲端基礎結構，但您也必須保護您的雲端服務和資源群組。 安全性的多層式方法提供最佳的防護。 備有周邊網路是該防禦策略中很重要的一部分。

- 周邊網路可防止不受信任的網路存取內部網路資源。
- 它是對網際網路公開的最外層。 它通常位於網際網路和企業基礎結構之間，且兩邊通常都會有某種形式的保護。
- 在典型的企業網路拓撲中，核心基礎結構的周邊有多層的安全性裝置，嚴加防禦。 每一層的界限都由裝置和原則強制執行點組成。
- 每一層均可結合各種網路安全性解決方案，包括防火牆、拒絕服務 (DoS) 預防、入侵偵測/保護系統 (IDS/IPS) 和 VPN 裝置。
- 若要在周邊網路強制執行原則，您可以使用防火牆原則、存取控制清單 (ACL) 或特定的路由。
- 當連入流量從網際網路抵達時，防禦解決方案組合會攔截並處理該流量，以阻擋攻擊和有害的流量，同時允許合法的要求進入網路。
- 連入流量可直接路由至周邊網路的資源中。 接著，周邊網路資源可以與更深層網路中的其他資源通訊，並於驗證之後將流量轉送到網路中。

下圖顯示公司網路中的單一子網路周邊網路範例，有兩個安全性界限。

![VPN](./media/migrate-best-practices-networking/perimeter.png)
_周邊網路部署_

**瞭解更多資訊：**

- 瞭解如何[在 Azure 與您的內部部署資料中心之間部署周邊網路](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-dmz)。

## <a name="best-practice-filter-vnet-traffic-with-nsgs"></a>最佳做法：使用 Nsg 篩選 VNet 流量

網路安全性群組 (NSG) 包含多個輸入和輸出安全性規則，會篩選進出資源的流量。 您可以依據來源和目的地 IP 位址、連接埠及通訊協定來進行篩選。

- NSG 包含安全性規則，可允許或拒絕進出於多種 Azure 資源類型的輸入和輸出網路流量。 您可以為每個規則指定來源和目的地、連接埠及通訊協定。
- 系統會依優先順序使用五 Tuple 資訊 (來源、來源連接埠、目的地、目的地連接埠和通訊協定) 來評估 NSG 規則，以允許或拒絕流量。
- 系統會為現有連線建立流程記錄。 允許或拒絕通訊都會以此流程記錄的連線狀態為依據。
- 流程記錄允許具狀態 NSG。 例如，如果您將輸出安全性規則指定至任何透過連接埠 80 的位址，則不需要有輸入安全性規則來回應輸出流量。 如果在外部起始通訊，您只需要指定輸入安全性規則。
- 反之亦然。 如果允許透過連接埠傳送輸入流量，則不必指定輸出安全性規則來透過連接埠回應流量。
- 當您移除啟用流量的安全性規則時，現有的連線不會中斷。 當連線停止，且兩個方向至少有數分鐘都沒有流量時，流量即會中斷。
- 在建立 NSG 時，所建立的數量越少越好，但又要能滿足所需。

### <a name="best-practice-secure-northsouth-and-eastwest-traffic"></a>最佳做法：保護北/南部和東部/西部流量

在保護 VNet 時，請務必考慮到攻擊媒介。

- 只使用子網路 NSG 可簡化環境，但卻只能保護進入子網路的流量。 這種流量稱為南/北流量。
- 位於相同子網路上 VM 之間的流量則稱為東/西流量。
- 請務必同時使用這兩種形式的保護，以便在駭客獲得從外部存取子網路的能力時，讓他們無法嘗試連結位於相同子網路內的機器。

### <a name="use-service-tags-on-nsgs"></a>在 NSG 上使用服務標籤

服務標籤表示一組 IP 位址前置詞。 使用服務標籤有助於降低建立 NSG 規則時的複雜性。

- 在建立規則時，您可以使用服務標籤來取代使用特定 IP 位址。
- Microsoft 會管理與服務標籤相關聯的位址前置詞，並隨著位址變更自動更新服務標籤。
- 您無法建立自己的服務標籤，也無法指定標籤中包含哪些 IP 位址。

服務標籤可讓您不必以手動方式將規則指派給 Azure 服務的群組。 例如，如果您想要允許包含 Web 伺服器的 VNet 子網路，讓其可以存取 Azure SQL Database，則可以對連接埠 1433 建立輸出規則，並使用 **Sql** 服務標籤。

- 此 **Sql** 標籤代表 Azure SQL Database 和 Azure SQL 資料倉儲服務的位址前置詞。
- 如果您指定 **Sql** 作為值，就會允許或拒絕 Sql 的流量。
- 如果您只需要允許存取特定**區域**中的 Sql，則可以指定該區域。 例如，如果您只想要允許存取美國東部區域中的 Azure SQL Database，您可以指定服務標記的**EastUS** 。
- 標籤代表服務，但不代表服務的特定執行個體。 例如，標籤可代表 Azure SQL Database 服務，但不可代表特定的 SQL 資料庫或伺服器。
- 此標記所表示的所有位址前置詞，也都可用**網際網路**標記表示。

**瞭解更多資訊：**

- [了解](https://docs.microsoft.com/azure/virtual-network/security-overview) NSG。
- [檢閱](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) NSG 可用的服務標籤。

## <a name="best-practice-use-application-security-groups"></a>最佳做法：使用應用程式安全性群組

應用程式安全性群組 (ASG) 可讓您將網路安全性設定為應用程式結構的自然擴充功能。

- 您可以將 VM 分組，並根據應用程式安全性群組定義網路安全性原則。
- 應用程式安全性群組可讓您大規模重複使用您的安全性原則，而不需進行明確 IP 位址的手動維護。
- 應用程式安全性群組可處理明確 IP 位址和多個規則集的複雜性，讓您專注於商務邏輯。

**範例：**

![應用程式安全性群組](./media/migrate-best-practices-networking/asg.png)
_應用程式安全性群組範例_

| **網路介面** | **應用程式安全性群組** |
| --- | --- |
| NIC1 | AsgWeb |
| NIC2 | AsgWeb |
| NIC3 | AsgLogic |
| NIC4 | AsgDb |

- 在本例中，每個網路介面都只屬於一個應用程式安全性群組，但事實上介面可以屬於多個群組 (取決於 Azure 限制)。
- 這些網路介面都沒有相關聯的 NSG。 NSG1 與這兩個子網路相關聯且包含下列規則。

<!-- markdownlint-disable MD033 -->

| **規則名稱** | **目的** | **詳細資料** |
| --- | --- | --- |
| Allow-HTTP-Inbound-Internet | 讓流量從網際網路流向 Web 伺服器。 來自網際網路的輸入流量會遭到 DenyAllInbound 預設安全性規則拒絕，因此 AsgLogic 或 AsgDb 應用程式安全性群組則不需要其他規則。 | 優先順序：100 <br><br> 來源：網際網路 <br><br> 來源埠：\* <br><br> 目的地： AsgWeb <br><br> 目的地埠：80 <br><br> 通訊協定：TCP <br><br> 存取： [允許]。 |
| Deny-Database-All | AllowVNetInBound 預設安全性規則允許相同 VNet 中各資源之間的所有通訊，因此需要此規則才能拒絕來自所有資源的流量。 | 優先順序：120 <br><br> 來源： \* <br><br> 來源埠：\* <br><br> 目的地： AsgDb <br><br> 目的地埠：1433 <br><br> 通訊協定：全部 <br><br> 存取：拒絕。 |
| Allow-Database-BusinessLogic | 讓流量從 AsgLogic應用程式安全性群組流向 AsgDb 應用程式安全性群組。 此規則的優先順序高於 [拒絕-資料庫-全部] 規則，因此會先處理此規則。 因此，允許來自 AsgLogic 應用程式安全性群組的流量，並封鎖所有其他流量。 | 優先順序：110 <br><br> 來源： AsgLogic <br><br> 來源埠：\* <br><br> 目的地： AsgDb <br><br> 目的地埠：1433 <br><br> 通訊協定：TCP <br><br> 存取： [允許]。 |

<!--markdownlint-enable MD033 -->

- 用於將應用程式安全性群組指定為來源或目的地的規則，只會套用至屬於此應用程式安全性群組成員的網路介面。 如果網路介面不是應用程式安全性群組的成員，則此規則不適用於此網路介面，即使子網路與網路安全性群組相關聯也一樣。

**瞭解更多資訊：**

- [了解](https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups)應用程式安全性群組。

### <a name="best-practice-secure-access-to-paas-using-vnet-service-endpoints"></a>最佳做法：使用 VNet 服務端點來保護對 PaaS 的存取

VNet 服務端點可透過直接連線，將 VNet 私人位址空間和身分識別延伸至 Azure 服務。

- 這些端點可讓您只將重要的 Azure 服務資源放到 VNet 進行保護。 從您的 VNet 到 Azure 服務的流量一定會保留在 Microsoft Azure 骨幹網路上。
- VNet 私人位址空間可以重疊，因此無法只用此項目來識別從 VNet 輸出的流量。
- 在 VNet 中啟用服務端點後，即可將 VNet 規則新增至服務資源，以保護 Azure 服務資源。 這樣可以藉由完全移除這些資源的公用網際網路存取，而且只允許來自您 VNet 的流量，來改善安全性。

![服務端點](./media/migrate-best-practices-networking/endpoint.png)
_服務端點_

**瞭解更多資訊：**

- [了解](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) VNet 服務端點。

## <a name="best-practice-control-public-ip-addresses"></a>最佳做法：控制公用 IP 位址

Azure 中的公用 IP 位址可與 VM、負載平衡器、應用程式閘道和 VPN 閘道相關聯。

- 公用 IP 位址可讓網際網路資源對 Azure 資源進行輸入通訊，以及讓 Azure 資源對網際網路進行輸出通訊。
- 公用 IP 位址會使用基本或標準 SKU 來建立，這兩種 SKU 有幾項差異。 標準 SKU 可以指派給任何服務，但最常設定於 VM、負載平衡器和應用程式閘道上。
- 請務必注意，基本的公用 IP 位址不會自動設定 NSG。 您需要自行設定並指派規則以控制存取。 標準 SKU 的 IP 位址則預設會獲派 NSG 和規則。
- 最佳做法是 VM 不應該使用公用 IP 位址來進行設定。
  - 如果您需要開啟連接埠，請只對 Web 服務開啟，例如連接埠 80 或 443。
  - SSH (22) 和 RDP (3389) 等標準遠端管理連接埠應使用 NSG 設定為拒絕 (其他所有連接埠也是如此)。
- 次佳做法是將 VM 放在 Azure 負載平衡器或應用程式閘道後方。 然後，如果需要存取遠端管理連接埠，您就可以在 Azure 資訊安全中心使用 Just-In-Time VM 存取。

**瞭解更多資訊：**

- [Azure 中的公用 IP 位址](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#public-ip-addresses)
- [使用 Just-In-Time 管理虛擬機器存取](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)

## <a name="take-advantage-of-azure-security-features-for-networking"></a>針對網路利用 Azure 安全性功能

Azure 有容易使用的平台安全性功能，並且會提供豐富的對策來處理常見的網路攻擊。 這些對策包括 Azure 防火牆、Web 應用程式防火牆和網路監看員。

## <a name="best-practice-deploy-azure-firewall"></a>最佳做法：部署 Azure 防火牆

Azure 防火牆是受控、雲端式網路安全性服務，可以保護您的 VNet 資源。 它是完全具狀態防火牆，具有內建高可用性和不受限的雲端延展性。

![服務端點](./media/migrate-best-practices-networking/firewall.png)
_Azure 防火牆_

- Azure 防火牆可讓您橫跨訂用帳戶和 VNet 集中建立、強制執行以及記錄應用程式和網路連線原則。
- Azure 防火牆會針對您的 VNet 資源使用靜態公用 IP 位址，允許外部防火牆識別源自您 VNet 的流量。
- Azure 防火牆與 Azure 監視器會完全整合，以進行記錄和分析。
- 在建立 Azure 防火牆規則時，最佳做法是使用 FQDN 標籤來建立規則。
  - FQDN 標籤代表一群與知名 Microsoft 服務相關聯的 FQDN。
  - 您可以使用 FQDN 標籤，以允許必要的輸出網路流量通過防火牆。
- 例如，若要手動允許 Windows Update 網路流量通過防火牆，您需要建立多個應用程式規則。 使用 FQDN 標籤，您便可以建立應用程式規則並包含 Windows Update 標籤。 備有此規則後，流向 Microsoft Windows Update 端點的網路流量就能流過防火牆。

**瞭解更多資訊：**

- 閱讀[Azure 防火牆的總覽](https://docs.microsoft.com/azure/firewall/overview)。
- 瞭解[Azure 防火牆中的 FQDN 標記](https://docs.microsoft.com/azure/firewall/fqdn-tags)。

## <a name="best-practice-deploy-a-web-application-firewall-waf"></a>最佳做法：部署 web 應用程式防火牆（WAF）

Web 應用程式已逐漸成為利用常見已知弱點的惡意攻擊目標。 這些攻擊包括 SQL 插入式攻擊和跨網站指令碼攻擊。 想要防止應用程式的程式碼受到這類攻擊會非常困難，而且可能需要對多層次的應用程式拓撲執行嚴格的維護、修補和監視工作。 集中式 Web 應用程式防火牆有助於簡化安全性管理作業，並有助於應用程式管理員對抗威脅或入侵。 相較於保護個別 Web 應用程式，Web 應用程式防火牆可透過在中央位置修補已知弱點，更快地因應安全性威脅。 現有的應用程式閘道可以輕易地轉換成已啟用 Web 應用程式防火牆的應用程式閘道。

Web 應用程式防火牆 (WAF) 是 Azure 應用程式閘道的一項功能。

- WAF 可集中保護 Web 應用程式，使其免於遭遇常見的攻擊和弱點。
- WAF 不必修改後端程式碼。
- 其可在應用程式閘道背後同時保護多個 Web 應用程式。
- WAF 已經與 Azure 資訊安全中心整合。
- 您可以自訂 WAF 規則和規則群組，以符合您的應用程式需求。
- 最佳做法是在任何 Web 對向應用程式前方使用 WAF，包括 Azure VM 上的應用程式或作為 Azure App Service。

**瞭解更多資訊：**

- [了解](https://docs.microsoft.com/azure/application-gateway/waf-overview) WAF。
- [檢閱](https://docs.microsoft.com/azure/application-gateway/application-gateway-waf-configuration) WAF 限制與排除項目。

## <a name="best-practice-implement-azure-network-watcher"></a>最佳做法：執行 Azure 網路監看員

Azure 網路監看員會提供工具讓您監視 Azure VNet 中的資源與通訊。 例如，您可以監視 VM 與端點 (例如，另一個 VM 或 FQDN) 之間的通訊、檢視 VNet 中的資源與資源關聯性，或診斷網路流量問題。

![網路監看員](./media/migrate-best-practices-networking/network-watcher.png)
_網路監看員_

- 使用網路監看員，您不必登入 VM 就能監視及診斷網路問題。
- 您可以設定警示來觸發封包擷取，並能存取封包層級的即時效能資訊。 當您發現問題時，可以詳加調查。
- 最佳做法是使用網路監看員來檢閱 NSG 流程記錄。
  - 網路監看員中的 NSG 流程記錄可讓您檢視透過 NSG 的輸入和輸出 IP 流量相關資訊。
  - 流程記錄會以 JSON 格式撰寫。
  - 流程記錄會顯示每一規則的輸出和輸入流程、套用流程的網路介面 (NIC)、關於流程的 5 Tuple 資訊 (來源/目的地 IP、來源/目的地連接埠、通訊協定)，以及流程已允許還是拒絕。

**瞭解更多資訊：**

- [取得網路監看員概觀](https://docs.microsoft.com/azure/network-watcher)。
- [深入了解](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) NSG 流程記錄。

## <a name="use-partner-tools-in-the-azure-marketplace"></a>使用 Azure Marketplace 中的合作夥伴工具

針對更複雜的網路拓撲，您可以使用 Microsoft 合作夥伴的安全性產品，尤其是網路虛擬設備 (NVA)。

- NVA 是執行網路功能的 VM，例如防火牆、WAN 最佳化或其他網路功能。
- NVA 可加強 VNet 安全性和網路功能。 部署 NVA 可以實現高可用性的防火牆、入侵預防、入侵偵測、Web 應用程式防火牆 (WAF)、WAN 最佳化、路由、負載平衡、VPN、憑證管理、Active Directory 和多重要素驗證等功能。
- [Azure Marketplace](https://azuremarketplace.microsoft.com)中的許多廠商都提供 NVA。

## <a name="best-practice-implement-firewalls-and-nvas-in-hub-networks"></a>最佳做法：在中樞網路中執行防火牆和 Nva

在中樞內，周邊網路 (具有網際網路存取權) 通常可透過 Azure 防火牆、防火牆伺服器陣列或 Web 應用程式防火牆 (WAF) 來進行管理。 請考量下列比較。

<!-- markdownlint-disable MD033 -->

| **防火牆類型** | **詳細資料** |
| --- | --- |
| WAF | Web 應用程式很常見，其容易受到弱點和潛在攻擊的影響。 <br><br> WAF 的設計是為了偵測對 Web 應用程式的攻擊 (HTTP/HTTPS)，其用途比一般防火牆更為具體。 <br><br> 相較於傳統防火牆技術，WAF 有一組特定功能可保護內部網頁伺服器不受威脅。 |
| Azure 防火牆 | Azure 防火牆和 NVA 防火牆伺服器陣列一樣，會使用常見的管理機制和一組安全性規則來保護輪輻網路中所裝載的工作負載，以及控制對內部部署網路的存取。 <br><br> Azure 防火牆具有內建延展性。 |
| NVA 防火牆 | NVA 防火牆伺服器陣列和 Azure 防火牆一樣，擁有常見的管理機制和一組安全性規則，以便保護輪輻網路中所裝載的工作負載，以及控制對內部部署網路的存取。 <br><br> NVA 防火牆可以在負載平衡器後方手動擴充。 <br><br> 雖然 NVA 防火牆所擁有的特製化軟體比 WAF 還要少，但其具有較廣泛的應用程式範圍，可篩選和檢查任何類型的輸出和輸入流量。 <br><br> 如果您想要使用 NVA，請至 Azure Marketplace 中尋找。 |

<!--markdownlint-enable MD033 -->

建議您使用一組 Azure 防火牆 (或 NVA) 來管理源自網際網路的流量，並使用另一組來管理源自內部部署環境的流量。

- 對兩者僅使用一組防火牆會造成安全性風險，因為它未提供兩組網路流量之間的安全性範疇。
- 使用個別的防火牆層級可降低檢查安全性規則的複雜度，並明確指出哪些規則對應到哪個傳入網路要求。

**瞭解更多資訊：**

- [了解](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-dmz)如何在 Azure VNet 中使用 NVA。

## <a name="next-steps"></a>後續步驟

檢閱其他最佳做法：

- 移轉後的安全性及管理[最佳做法](./migrate-best-practices-security-management.md)。
- 移轉後的成本管理[最佳做法](./migrate-best-practices-costs.md)。
