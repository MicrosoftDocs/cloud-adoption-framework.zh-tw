---
title: 網路拓樸和連線能力
description: 網路拓撲和連線能力。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 4363ead6a4319629df964f3efa4ace802ac81ee4
ms.sourcegitcommit: 1c123a413725f7d2bfce91e9a6fb9e8c8c59f37b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/24/2020
ms.locfileid: "85335984"
---
<!-- cSpell:ignore autoregistration BGPs MACsec MPLS MSEE onprem privatelink VPNs -->

# <a name="network-topology-and-connectivity"></a>網路拓樸和連線能力

本章節將探討主要的設計考慮，以及在 Microsoft Azure 中的網路和連線能力的相關建議。

## <a name="planning-for-ip-addressing"></a>規劃 IP 位址

企業客戶要規劃 Azure 中的 IP 位址，以確保在內部部署位置和 Azure 區域之間沒有重迭的 IP 位址空間，這是很重要的。

**設計考慮：**

- 在內部部署和 Azure 區域之間重迭的 IP 位址空間將會造成重大的爭用挑戰。

- 建立虛擬網路（VNet）位址空間後，如果 VNet 已透過虛擬網路對等互連連線到另一個 VNet，則此程式需要中斷，因為對等互連將需要刪除並重新建立。

- Azure 會在每個子網中保留5個 IP 位址，在調整 Vnet 和包含的子網的大小時，應該將其納入考慮。

- 某些 Azure 服務需要[專用的子網](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services#services-that-can-be-deployed-into-a-virtual-network)，例如 azure 防火牆或 VNet 閘道。

- 子網可以委派給某些服務，以在子網內建立該服務的實例。

**設計建議：**

- 事先規劃跨 Azure 區域和內部部署位置的非重迭 IP 位址空間。

- 使用私人網際網路位址配置中的 IP 位址（RFC 1918）。

- 針對具有有限私人 IP 位址（RFC 1918）可用性的環境，請考慮使用 IPv6。

- 請勿建立不必要的大型 Vnet （例如： `/16` ），以確保不會浪費 IP 位址空間。

- 請不要事先規劃所需的位址空間來建立 Vnet，因為在 VNet 透過虛擬網路對等互連連線之後，新增位址空間將會導致中斷。

- 請勿將公用 IP 位址用於 Vnet，特別是當公用 IP 位址不屬於客戶時。

## <a name="configure-the-domain-name-system-dns-and-name-resolution-for-on-premises-and-azure-resources"></a>設定內部部署和 Azure 資源的網域名稱系統（DNS）和名稱解析

DNS 是整體企業級架構中的重要設計主題，而某些客戶可能會想要使用其在 DNS 中的現有投資，其他人可能會發現雲端採用是將其內部 DNS 基礎結構現代化並使用原生 Azure 功能的機會。

**設計考慮：**

- DNS 解析程式可與 Azure 私人 DNS 搭配使用，以進行跨單位的名稱解析。

- 客戶可能需要在內部部署和 Azure 之間使用現有的 DNS 解決方案。

- VNet 可以與自動註冊連結的私人 DNS 區域數目上限是一個。

- VNet 可以連結的私人 DNS 區域數目上限為1000，而未啟用自動註冊。

**設計建議：**

- 針對 Azure 中的名稱解析所需的環境，請使用 Azure 私人 DNS 來進行解析，方法是建立用於名稱解析的委派區域（例如 `azure.contoso.com` ）。

- 針對跨 Azure 和內部部署進行名稱解析的環境，請使用部署到至少兩個 Azure Vm 的現有 DNS 基礎結構（例如 Active Directory 整合式 DNS），並在 Vnet 中設定 DNS 設定以使用這些 DNS 伺服器。

  - 您仍然可以使用 azure 私人 dns，其中 Azure 私人 DNS 區域會連結至 Vnet，而 DNS 伺服器則用來作為混合式解析程式，搭配使用內部部署 dns 伺服器的條件式轉送至內部部署 DNS 名稱（例如 `onprem.contoso.com` ）。 內部部署伺服器可以使用條件轉寄站來設定，以便在 azure 中針對 Azure 私人 DNS 區域解析 Vm （例如 `azure.contoso.com` ）。

- 需要和部署自己的 DNS 的特殊工作負載（例如 Red Hat OpenShift）應使用其慣用的 DNS 解決方案。

- 自動註冊應啟用，Azure DNS 自動管理 VNet 中部署之虛擬機器的 DNS 記錄生命週期。

- 使用虛擬機器做為使用 Azure 私人 DNS 進行跨單位 DNS 解析的解析程式。

- 在全域連線訂用帳戶內建立 Azure 私人 DNS 區域。 可能會建立額外的 Azure 私人 DNS 區域（例如， `privatelink.database.windows.net` 或 `privatelink.blob.core.windows.net` 適用于 Azure 私人連結）。

## <a name="define-an-azure-network-topology"></a>定義 Azure 網路拓朴

網路拓撲是企業級架構的重要基礎元素，因為它最終會定義應用程式彼此通訊的方式。 本節探討企業 Azure 部署的相關技術和拓撲方法，著重于兩種核心方法：以 Azure 虛擬 WAN 為基礎的拓撲，以及傳統的拓撲。

以 Azure 虛擬 WAN 為基礎的網路拓撲是適用于大規模多區域部署的最佳企業級方法，其中客戶需要將其全球位置連線至 Azure 和內部部署。 每當客戶打算使用與 Azure 完全整合的軟體定義 WAN （SD WAN）部署時，也應該使用虛擬 WAN 網路拓撲。 虛擬 WAN 是用來滿足大規模的互連能力需求。 因為它是由 Microsoft 管理的服務，所以也會降低整體網路的複雜性，並協助將客戶的網路現代化。

傳統的 Azure 網路拓朴應用於僅想要在幾個 Azure 區域中部署資源的客戶、不需要全域互連的網路、每個區域的遠端或分支位置數（少於30個），或需要完全控制和資料細微性以手動設定其 Azure 網路。 此傳統拓撲可協助這些客戶在 Azure 中建立安全的網路基礎。

## <a name="virtual-wan-network-topology-microsoft-managed"></a>虛擬 WAN 網路拓撲（由 Microsoft 管理）

![網路拓撲和連線能力 ](./media/virtual-wan-topology.png)
 _圖1：虛擬 WAN 網路拓朴。_

**設計考慮：**

- [Azure 虛擬 WAN](https://docs.microsoft.com/azure/virtual-wan/virtual-wan-about)是一種由 Microsoft 管理的解決方案，預設會提供端對端全域傳輸連線能力。 虛擬 WAN 中樞可免除手動設定網路連線的需求。 例如，客戶不需要設定使用者定義的路由（UDR）或網路虛擬裝置（Nva），即可啟用全域傳輸連線能力。

- 虛擬 WAN 藉由建立橫跨多個 Azure 區域和內部部署位置（任意對任意連線能力）的[中樞和輪輻網路架構](https://docs.microsoft.com/azure/virtual-wan/virtual-wan-global-transit-network-architecture)，來大幅簡化 Azure 和跨單位的端對端網路連線，如下圖所示：

![網路拓撲和連線能力 ](./media/global-transit.png)
 _圖2：具有虛擬 WAN 的全球傳輸網路。_

- 虛擬 WAN any 對任何可轉移連線支援下列路徑（在相同區域內和跨區域）：

  - 虛擬網路到虛擬網路
  - 虛擬網路到分支
  - 分支到虛擬網路
  - 分支至分支

- 虛擬 WAN 中樞已鎖定，而且客戶可以在其中部署的唯一資源是虛擬網路閘道（點對站 VPN、站對站 VPN 和 ExpressRoute）、透過防火牆管理員的 Azure 防火牆，以及路由表。

- 虛擬 WAN 會透過 ExpressRoute 私用對等互連，將最多200個首碼從 Azure 公佈到內部部署，每個虛擬 WAN 中樞增加到10000個首碼。 10000首碼的限制也包括站對站 VPN 和點對站 VPN。

- VNet 對 VNet 轉移連線能力（區域內和跨區域）處於公開預覽狀態。

- 虛擬 WAN 中樞對中樞連線能力目前為公開預覽版。

- 虛擬 WAN 整合了各種[SD wan 提供者](https://docs.microsoft.com/azure/virtual-wan/virtual-wan-locations-partners)。

- 許多受控服務提供者都提供虛擬 WAN 的[受控服務](https://docs.microsoft.com/azure/networking/networking-partners-msp)。

- 虛擬 WAN 中的 VPN 閘道可針對每個虛擬中樞相應增加至 20 Gbps 和20000個連線。

- 具有 premium 附加元件的 ExpressRoute 線路是必要的，而且它們應該來自 ExpressRoute 全球範圍的位置。

- Azure 防火牆管理員（目前處於公開預覽狀態）允許在虛擬 WAN 中樞中部署 Azure 防火牆。

- 目前不支援透過 Azure 防火牆的虛擬 WAN 中樞對中樞流量。 或者，在虛擬 WAN 中使用原生中樞來中樞傳輸路由功能，並使用 Nsg 來允許/封鎖跨中樞的 VNet 流量。

**設計建議：**

- 針對 Azure 中的新大型/全球網路部署，強烈建議使用虛擬 WAN，其中客戶需要跨 Azure 區域和內部部署位置的全球傳輸連線，而不必手動設定 Azure 網路可轉移路由。

  下圖說明全球客戶部署的範例，其中的資料中心散佈在歐洲和美國，以及兩個區域內的大量分公司。 環境是透過虛擬 WAN 和 ExpressRoute 全球連線來全球化。

![範例網路拓撲 ](./media/global-reach-topology.png)
 _圖3：範例網路拓撲。_

- 使用虛擬 WAN 做為全域連線資源，並以每個 Azure 區域的虛擬 WAN 中樞，透過其本機虛擬 WAN 中樞，將多個登陸區域一起連線到跨 Azure 區域。

- 使用 ExpressRoute 將虛擬 WAN 中樞連線至內部部署資料中心。

- 透過站對站 VPN，將分支和遠端位置連線到最接近的虛擬 WAN 中樞，或透過 SD-WAN 合作夥伴解決方案，啟用對虛擬 WAN 的分支連線能力。

- 透過點對站 VPN，將終端使用者連線到虛擬 WAN 中樞。

- 遵循原則「Azure 中的流量會保留在 Azure 中」，讓 Azure 中資源間的通訊透過 Microsoft 骨幹網路進行，即使資源位於不同區域也一樣。

- 在 Azure 區域內的東部-西部和南北流量保護/篩選的虛擬 WAN 中樞中部署 Azure 防火牆。

- 如果在美國西部和（或）南部流量保護/篩選中需要協力廠商 Nva，請將 Nva 部署至個別的 VNet （例如 NVA VNet），並將其連線至區域虛擬 WAN 中樞和需要存取 Nva 的登陸區域。 如需詳細資訊，請參閱[建立 nva 的虛擬 WAN 中樞路由表](https://docs.microsoft.com/azure/virtual-wan/virtual-wan-route-table-portal)。

- 部署協力廠商網路技術和 Nva 時，請遵循協力廠商的指導方針，以確保沒有與 Azure 網路衝突的設定。

- 請勿在 Azure 虛擬 WAN 上建立傳輸網路，因為虛擬 WAN 符合所有可轉移的網路拓撲需求，包括使用協力廠商 Nva 的能力。

- 請不要使用現有的內部部署網路（例如多重通訊協定標籤切換（MPLS）），在 Azure 區域之間連線 Azure 資源，因為 Azure 網路技術支援透過 Microsoft 骨幹跨區域互連 Azure 資源。

- 針對您從不是以虛擬 WAN 為基礎的中樞和輪輻網路拓撲進行遷移的 brownfield 案例，請參閱[遷移至 Azure 虛擬 wan](https://docs.microsoft.com/azure/virtual-wan/migrate-from-hub-spoke-topology)。

- 應該在連線訂用帳戶內建立 azure 虛擬 WAN 和 Azure 防火牆資源。

- 請勿為每個虛擬 WAN 虛擬中樞建立超過500個 VNet 連線。

## <a name="traditional-azure-networking-customer-managed-topology"></a>傳統的 Azure 網路（客戶管理）拓撲

雖然虛擬 WAN 提供各種強大的功能，但在某些情況下，傳統的 Azure 網路方法可能會是最佳的：

- 如果跨多個 Azure 區域或跨單位的全球可轉移網路不是必要的，例如在美國的分支需要連線到歐洲的虛擬網路。

- 如果不需要透過 VPN 連接到大量的遠端位置，或與 SD WAN 解決方案整合。

- 如果客戶的喜好設定是在 Azure 中設定網路拓撲時，有細微的控制和設定。

![網路拓撲和連線能力 ](./media/customer-managed-topology.png)
 _圖4：客戶管理的 Azure 網路拓朴。_

**設計考慮：**

- 有多個網路拓朴可連接多個登陸區域 Vnet：一個大型的一般 VNet、多個 Vnet 連接多個 ExpressRoute 電路或連線、中樞輪輻、完整網狀架構，以及混合式。

- Vnet 不會跨越訂用帳戶界限，但可以使用虛擬網路對等互連、ExpressRoute 線路或使用 VPN 閘道，來達到不同訂用帳戶中的 Vnet 之間的連線能力。

- 您可以使用虛擬網路對等互連，將相同區域中的 Vnet 連線到不同的 Azure 區域，以及跨不同的 Azure AD 租使用者。

- 虛擬網路對等互連和全域 VNet 對等互連不可轉移。 因此，必須要有 Udr 和 Nva，才能啟用傳輸網路。 如需詳細資訊，請參閱[Azure 中的中樞輪輻網路拓撲](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)。

- ExpressRoute 線路可用來在相同的地理政治區域內建立跨 Vnet 的連線，或使用高階附加元件來跨地理政治區域進行連接。

  - VNet 對 VNet 流量可能會遇到額外的延遲，因為流量必須 hairpin 在 Microsoft enterprise edge （MSEE）路由器上。

  - 頻寬會受限於 ExpressRoute 閘道 SKU。

  - 客戶仍必須部署和管理 Udr，而他們需要跨 VNet 流量的檢查/記錄。

- 具有邊界閘道協定（BGP）的 VPN 閘道可在 Azure 和內部部署中轉移，但不會傳輸到 ExpressRoute 閘道。

- 當多個 ExpressRoute 線路連線到相同的 VNet 時，必須使用連接權數和 BGP 技術來確保內部部署與 Azure 之間流量的最佳路徑。 如需詳細資訊，請參閱[優化 ExpressRoute 路由](https://docs.microsoft.com/azure/expressroute/expressroute-optimize-routing)。

- 使用 BGP 技術來影響 ExpressRoute 路由是 Azure 雲端外部的設定。 因此，客戶或其連線提供者必須據以設定其內部部署路由器。

- 具有 premium 附加元件的 ExpressRoute 線路提供了全域連線能力;不過，每個 ExpressRoute 閘道的 ExpressRoute 連線數目上限為四個。

- 雖然每個 VNet 的虛擬網路對等互連連線數目上限為500，但可透過 ExpressRoute 私用對等互連從 Azure 通告至內部部署的最大路由數目為200。

- VPN 閘道的最大匯總輸送量為 10 Gbps，最多可支援30個站對站/VNet 對 VNet 通道。

**設計建議：**

- 在下列案例中，您應該考慮以具有非虛擬 WAN 技術的中樞和輪輻網路拓撲為基礎的網路設計：

  - 流量界限在 Azure 區域內的 azure 部署。

  - 一種網路架構，其中包含多達兩個 Azure 區域，其中每個區域的登陸區域 Vnet 都需要傳輸連線能力。

  - 跨越多個 Azure 區域的網路架構，並不需要在跨區域 Vnet 的登陸區域之間進行可轉移的連線。

  - VPN 和 ExpressRoute 連線之間不需要可轉移的連線能力。

  - 主要跨單位連線通道是 ExpressRoute，而 VPN 連線數目小於每個 VPN 閘道30。

  - 集中式 Nva 和複雜/細微的路由會有相當大的相依性。

- 針對區域部署，主要使用中樞與輪輻拓撲，並將 Vnet 與虛擬網路對等互連連線至中央中樞 VNet，以透過 ExpressRoute 進行跨單位連線、用於分支連線的 VPN、透過 Nva 和 Udr 的輪輻到輪輻連線，以及透過 NVA 的網際網路輸出保護，如下圖所示。

![網路拓樸和連線能力](./media/hub-and-spoke-topology.png)

_圖5：中樞和輪輻網路拓撲。_

- 當需要高層級的隔離時，特定的業務單位需要專用的 ExpressRoute 頻寬，或每個 ExpressRoute 閘道（最多四個）的連線數目上限，請使用與多個 ExpressRoute 線路拓撲連接的多個 Vnet，如下圖所示：

![網路拓朴和 ](./media/vnet-multiple-circuits.png)
 _連線能力圖6：多個使用多個 ExpressRoute 線路連接的虛擬網路。_

- 在中央中樞 VNet 中部署一組最基本的共用服務，包括 ExpressRoute 閘道、VPN 閘道（如有需要）和 Azure 防火牆或協力廠商 Nva （如有必要）。 如有需要，也 Active Directory 網域控制站和 DNS 伺服器。

- 在中央中樞 VNet 中，部署適用于中東和/或南部流量保護/篩選的 Azure 防火牆或協力廠商 Nva。

- 部署協力廠商網路技術/Nva 時，請遵循協力廠商的指導方針，以確保廠商支援部署、專為高可用性和最高效能而設計，而且沒有與 Azure 網路衝突的設定。

- L7 輸入 Nva （例如 Azure 應用程式閘道）不應部署為中央中樞 VNet 中的共用服務。 相反地，它們應該與應用程式一起部署在其各自的登陸區域中。

- 使用現有的客戶網路（MPLS 和 SD WAN）來連接分公司位置與公司總部。 不支援 ExpressRoute 與 VPN 閘道之間的 Azure 傳輸。

- 對於跨 Azure 區域具有多個中樞和輪輻拓撲的網路架構，當少數登陸區域需要跨區域通訊時，請使用全域 VNet 對等互連來連接登陸區域 Vnet。 這種方法提供的優點，像是具有全域 VNet 對等互連的高網路頻寬（如 VM SKU 所允許），但它會略過中央 NVA （萬一需要進行流量檢查或篩選）。 這也會受限於[全域 VNet 對等互連限制](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#constraints-for-peered-virtual-networks)。

- 當客戶在兩個 Azure 區域中部署中樞和輪輻網路架構，並在各區域之間的所有登陸區域之間傳輸連線時，請使用 ExpressRoute 搭配雙重線路來提供傳輸連線能力，以 Vnet 跨 Azure 區域的登陸區域。 在此案例中，登陸區域可以透過本機中樞 VNet 中的 NVA 和跨區域透過 ExpressRoute 線路在區域內傳輸，而流量必須 hairpin 在 Microsoft enterprise edge （MSEE）路由器。 這項設計如下圖所示：

![網路拓撲和連線能力 ](./media/vnet-dual-circuits.png)
 _圖7：登陸區域連線設計。_

- 當客戶需要跨兩個 Azure 區域的中樞和輪輻網路架構，而且需要跨 Azure 區域 Vnet 的登陸區域之間進行全域傳輸連線時，雖然此架構可透過將中央中樞 Vnet 與全域 VNet 對等互連進行分支處理，以及使用 Udr 和 Nva 來啟用全域傳輸路由，因此，複雜度和管理的額外負荷很高。 相反地，我們建議您使用虛擬 WAN 來部署全域傳輸網路架構。

- 使用[Azure 監視器 network insights](https://docs.microsoft.com/azure/azure-monitor/insights/network-insights-overview) （目前處於預覽狀態）來監視 Azure 上客戶網路的端對端狀態。

- 請勿為每個中央中樞 VNet 建立超過200個對等互連連線。 雖然 Vnet 支援最多500對等互連連線，但具有私用對等互連的 ExpressRoute 僅支援從 Azure 到內部部署的最多有200個首碼的公告。

## <a name="connectivity-to-azure"></a>Azure 的連線能力

本節將在網路拓撲中展開，以考慮將內部部署位置連線至 Azure 的建議模型。

**設計考慮：**

- Azure ExpressRoute 提供專用的私人連線，可從內部部署位置 Microsoft Azure 基礎結構即服務（IaaS）和平臺即服務（PaaS）功能。

- 私用連結可用來透過 ExpressRoute 與私人對等互連建立 PaaS 服務的連線能力。

- 當多個 Vnet 連線到相同的 ExpressRoute 線路時，它們會成為相同路由網域的一部分，而且所有 Vnet 都會共用頻寬。

- ExpressRoute 全球範圍（若有提供）可讓客戶使用 ExpressRoute 線路將內部部署位置連接在一起，以透過 Microsoft 骨幹網路傳輸流量。

- ExpressRoute 全球範圍可在許多[expressroute 對等互連位置](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach#availability)中取得。

- ExpressRoute Direct 可讓您無需額外成本即可建立多個 ExpressRoute 線路，最高可達 ExpressRoute Direct 埠容量（10 Gbps 或 100 Gbps），並可讓您直接連接到 Microsoft 的 ExpressRoute 路由器。 100 Gbps SKU 的最小電路頻寬為 5 Gbps。 對於 10 Gbps SKU，最小電路頻寬是 1 Gbps。

<!-- cSpell:ignore prepending -->
<!-- docsTest:ignore "AS PATH prepending -->

**設計建議：**

- 使用 ExpressRoute 作為主要連線通道，以將內部部署網路連線至 Microsoft Azure。 Vpn 可以做為備份連線的來源，以增強連線能力。

- 將內部部署位置連線至 Azure 中的 Vnet 時，請從不同的對等互連位置使用雙重 ExpressRoute 線路。 此設定可確保 Azure 的重複路徑，移除內部部署與 Azure 之間的單一失敗點。

- 使用多個 ExpressRoute 線路時，請透過[BGP 本機喜好設定和路徑前置的方式來優化 ExpressRoute 路由](https://docs.microsoft.com/azure/expressroute/expressroute-optimize-routing#solution-use-as-path-prepending)。

- 確定根據頻寬和效能需求，將正確的 SKU 用於 ExpressRoute/VPN 閘道。

- 在支援的 Azure 區域中部署區域的「多餘 ExpressRoute 閘道」。

- 針對需要高於 10 Gbps 或專用 10/100 Gbps 埠之頻寬的案例，請使用 ExpressRoute Direct。

- 當需要低延遲，或從內部部署環境到 Azure 的輸送量必須大於 10 Gbps 時，請讓 FastPath 略過來自資料路徑的 ExpressRoute 閘道。

- 使用 VPN 閘道將分支或遠端位置連線至 Azure。 如需更高的復原能力，請部署區域冗余閘道（如果有的話）。

- 使用 ExpressRoute 全球觸達，透過 ExpressRoute 連接到 Azure 連線的大型辦公室/區域總部/資料中心。

- 當需要流量隔離或專用頻寬時（例如用於分隔生產與非生產環境），應該使用不同的 ExpressRoute 線路來確保隔離的路由網域，並減少雜訊鄰近的風險。

- 使用網路效能監控主動監視 ExpressRoute 線路。

- 請勿從單一對等互連位置明確使用 ExpressRoute 線路，因為這會建立單一失敗點，並讓客戶容易遭受對等互連位置中斷的影響。

- 請勿使用相同的 ExpressRoute 線路來連接多個需要隔離或專用頻寬的環境，以避免發生雜訊干擾的風險。

## <a name="connectivity-to-azure-paas-services"></a>Azure PaaS 服務的連線能力

本節將根據先前的連線能力章節，探索使用 Azure PaaS 服務時的建議連線方法。

**設計考慮：**

- Azure PaaS 服務通常是透過公用端點來存取，不過，Azure 平臺會提供功能來保護這類端點，甚至讓它們完全私人。

  - VNet 插入會針對支援的服務提供專用的私用部署。 但是管理平面流量會流經公用 IP 位址。

  - [私人連結](https://docs.microsoft.com/azure/private-link/private-endpoint-overview#private-link-resource)會使用私人 IP 位址對 Azure PaaS 實例提供專用的存取權，或 Azure Load Balancer standard 背後的自訂服務。

  - VNet 服務端點可讓您從選取的子網到所選 PaaS 服務的服務層級存取。

- 企業客戶通常有關于 PaaS 服務的公用端點必須適當地緩和的問題。

- 針對[支援的服務](https://docs.microsoft.com/azure/private-link/private-link-overview#availability)，私用連結會解決與服務端點相關聯的資料外泄疑慮。 或者，透過 Nva 的輸出篩選可以用來提供減輕資料外泄的步驟。

**設計建議：**

- 針對支援的 Azure 服務使用 VNet 插入，使其可從客戶 VNet 內取得。

- 已插入 VNet 的 Azure PaaS 服務仍會使用公用 IP 位址執行管理平面作業。 請確定使用 Udr 和 Nsg 在 VNet 內鎖定此通訊。

- 針對共用的 Azure PaaS 服務，使用私用連結（如果有的話）。 私用連結已正式適用于數個服務，且適用于許多服務。 [這裡](https://docs.microsoft.com/azure/private-link/private-endpoint-overview#private-link-resource)詳述私人連結的可用性。

- 透過 ExpressRoute 私用對等互連從內部部署存取 Azure PaaS 服務，使用適用于專用 Azure 服務的 VNet 插入或 Azure 私人連結，用於可用的共用 Azure 服務。 若要在 VNet 插入或私用連結無法使用時從內部部署存取 Azure PaaS 服務，請搭配 Microsoft 對等互連使用 ExpressRoute。 這可避免透過公用網際網路進行傳輸。

- 使用 VNet 服務端點來保護從客戶 VNet 內部存取 Azure PaaS 服務的安全，但只有在私人連結無法使用，且沒有資料外泄考慮時。 若要解決服務端點的資料外泄問題，請使用 NVA 篩選或使用 VNet 服務端點原則進行 Azure 儲存體。

- 根據預設，請勿在所有子網上啟用 VNet 服務端點。

- 除非使用 NVA 篩選，否則請不要在發生資料外泄疑慮時使用 VNet 服務端點。

- 不建議執行強制通道，以啟用從 Azure 到 Azure 資源的通訊。

## <a name="planning-for-inbound-and-outbound-internet-connectivity"></a>規劃輸入和輸出網際網路連線能力

本節說明連至公用網際網路的輸入和輸出連線所建議的連線性模型。

**設計考慮：**

- Azure-原生網路安全性服務（例如 Azure 防火牆、Azure 應用程式閘道上的 Azure Web 應用程式防火牆（WAF）和 Azure Front）是完全受控的服務，因此客戶不會產生與基礎結構部署相關聯的營運和管理成本，而這可能會變得很複雜。

- 企業規模的架構與協力廠商 Nva 完全相容，客戶應使用 Nva，或在原生服務無法滿足特定客戶需求的情況下。

**設計建議：**

- 使用 Azure 防火牆來管理：

  - 連到網際網路的 Azure 輸出流量

  - 非 HTTP/S 傳入連接

  - 東部-西部流量篩選（如果客戶需要）

- 使用具有虛擬 WAN 的防火牆管理員，跨虛擬 WAN 集線器或中樞 Vnet 部署和管理 Azure 防火牆。 針對虛擬 WAN 和一般 Vnet，防火牆管理員目前為預覽狀態。

- 建立全域 Azure 防火牆原則來管理全域網路環境中的安全性狀態，並將其指派給所有 Azure 防火牆實例。 透過 RBAC 將增量防火牆原則委派給本機安全性小組，以允許細微的原則符合特定區域的需求。

- 如果客戶希望使用這類解決方案來保護輸出連線，請在防火牆管理員中設定支援的協力廠商 SaaS 安全性提供者。

- 使用登陸區域 VNet 內的 WAF 來保護來自網際網路的輸入 HTTP/S 流量。

- 使用 Azure Front 門板和 WAF 原則，針對傳入區域的輸入 HTTP/S 連線，提供跨 Azure 區域的全域保護。

- 使用 Azure Front 門板和 Azure 應用程式閘道來保護 HTTP/S 應用程式時，請使用 Azure Front WAF 中的原則，並鎖定 Azure 應用程式閘道，以便只接收來自 Azure Front 的流量。

- 如果 [美國西部] 和/或 [南北部流量保護/篩選] 需要協力廠商 Nva：

- 針對虛擬 WAN 網路拓撲，請將 Nva 部署至個別的 VNet （例如 NVA VNet），並將它連線至區域虛擬 WAN 中樞和需要存取 Nva 的登陸區域，[如本文中所述。](https://docs.microsoft.com/azure/virtual-wan/virtual-wan-route-table-portal) 針對非虛擬 WAN 網路拓撲，請在中央中樞 VNet 中部署協力廠商 Nva。

- 如果輸入 HTTP/S 連線需要協力廠商 Nva，則應該將它們部署在登陸區域 VNet 中，以及它們所保護和公開到網際網路的應用程式。

- 使用[Azure DDoS 保護標準保護方案](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview)來保護在客戶 vnet 中裝載的所有公用端點。

- 請勿將內部部署 DMZ 概念和架構複寫到 Azure 中，因為客戶可以在 Azure 中取得與內部部署類似的安全性功能，但必須將實施和架構調整為雲端。

## <a name="planning-for-app-delivery"></a>規劃應用程式傳遞

本節將探討主要的建議，以安全、可高度擴充且高可用性的方式提供內部和外部面向應用程式。

**設計考慮：**

- Azure Load Balancer （內部和公用）可為地區層級的應用程式傳遞提供高可用性。

- Azure 應用程式閘道允許在區域層級安全地傳遞 HTTP/S 應用程式。

- Azure Front-允許跨 Azure 區域安全地傳遞高可用性 HTTP/S 應用程式。

- Azure 流量管理員允許傳遞全域應用程式。

**設計建議：**

- 內部和外部面向應用程式的應用程式傳遞都應該在登陸區域內執行。

- 針對 HTTP/S 應用程式的安全傳遞，請使用應用程式閘道 v2，並確定已啟用 WAF 保護/原則。

- 如果應用程式閘道 v2 無法用於 HTTP/S 應用程式的安全性，請使用協力廠商 NVA。

- Azure 應用程式閘道 v2 或用於輸入 HTTP/S 連線的協力廠商 Nva，應部署在登陸區域 VNet 中，以及其所保護的應用程式中。

- 登陸區域中的所有公用 IP 位址都應該受到 DDoS 標準保護計劃的保護。

- 跨 Azure 區域的全域 HTTP/S 應用程式應該使用 Azure Front WAF 原則來傳遞及保護。

- 使用前門和應用程式閘道來保護 HTTP/S 應用程式時，請使用 Azure Front WAF 中的原則，並鎖定應用程式閘道，以便只接收來自前門的流量。

- 跨越 HTTP/S 以外通訊協定的全域應用程式應該使用流量管理員來傳遞。

## <a name="planning-for-landing-zone-network-segmentation"></a>規劃登陸區域網路分割

本節將探討在登陸區域內提供高度安全的內部網路分割以驅動網路零信任的執行的主要建議。

**設計考慮：**

- 零信任模型會假設違反的狀態，並驗證每個要求，就像是來自不受控制的網路一樣。

- 先進的零信任網路實施採用完全分散的輸入/輸出雲端微周邊和更深入的微分割。

- 網路安全性群組可以使用 Azure 服務標籤來加速連線到 Azure PaaS 服務。

- 應用程式安全性群組不會跨越或提供跨 Vnet 的保護。

- 使用 Azure Resource Manager 範本現在支援 NSG 流量記錄。

**設計建議：**

- 將子網建立委派給登陸區域擁有者。 這可讓他們定義如何跨子網分割工作負載（例如，單一大型子網、多層式應用程式或 VNet 插入的應用程式）。 平臺小組可以使用 Azure 原則，以確保具有特定規則的 NSG （例如拒絕來自網際網路的連入 SSH 或 RDP，或允許/封鎖登陸區域的流量）一律會與具有僅限拒絕原則的子網相關聯。

- Nsg 必須用來保護跨子網的流量，以及跨平臺的美國東部流量（登陸區域流量）。

- 應用程式小組應該使用子網層級 Nsg 的應用程式安全性群組，來保護其登陸區域內的多層式 Vm。

- 使用 Nsg 和應用程式安全性群組來微分割登陸區域內的流量，並避免使用中央 NVA 來篩選流量。

- 啟用 NSG 流量記錄，並將其饋送至流量分析，以深入瞭解內部和外部流量流程。

- 使用 Nsg 選擇性地將登陸區域連線的白名單。

- 針對虛擬 WAN 拓撲，如果客戶需要在登陸區域流動流量的篩選和記錄功能，請透過 Azure 防火牆將流量路由傳送到登陸區域。

## <a name="define-network-encryption-requirements"></a>定義網路加密需求

本節將探討在內部部署與 Azure 之間，以及跨 Azure 區域，達到網路加密的主要建議。

**設計考慮：**

- 成本和可用頻寬會與端點之間的加密通道長度成正比。

- 使用 VPN 連線到 Azure 時，流量會透過網際網路透過 IP 安全性（IPsec）通道進行加密。

- 搭配使用 ExpressRoute 與私人對等互連時，目前不會加密流量。

- MACsec 加密可以套用至 ExpressRoute Direct 以達成網路加密。

- Azure 目前不提供透過全域 VNet 對等互連的原生加密。 如果目前需要 Azure 區域之間的加密，則可以使用 VPN 閘道（而非全域 VNet 對等互連）來連接 Vnet。

**設計建議：** ![加密流程 ](./media/enc-flows.png)
 _圖8：加密流程。_

- 使用 VPN 閘道建立從內部部署至 Azure 的 VPN 連線時，流量會在通訊協定層級使用 IPsec 通道進行加密，如上 `Flow A` 圖所示。

- 使用 ExpressRoute Direct 時，請設定[媒體存取控制安全性（MACsec）](https://docs.microsoft.com/azure/expressroute/expressroute-howto-MACsec) ，以加密客戶路由器與 MSEE 之間第二層的流量，如上圖所示 `Flow B` 。

- 對於 MACsec 不是選項的虛擬 WAN 案例（例如，不使用 ExpressRoute Direct），請使用虛擬 WAN VPN 閘道，透過 ExpressRoute 私人對等互連來建立 IPsec 通道。 這是 `Flow C` 在上圖中所描述。

- 針對非虛擬 WAN 案例，以及 MACsec 不是選項的（例如，不使用 ExpressRoute Direct），今天唯一的選項是使用協力廠商 Nva，透過 ExpressRoute 私人對等互連建立 IPsec 通道，或透過 ExpressRoute 使用 Microsoft 對等互連建立 VPN 通道。

- 如果必須加密 Azure 區域之間的流量，請使用 VPN 閘道來連接跨區域的 Vnet。

- 在 Azure 中使用協力廠商 Nva，以透過 ExpressRoute 私用對等互連來加密流量，以防 Azure 中提供的原生解決方案（如上述的流程 B 和 C 所述）不符合客戶需求。

## <a name="planning-for-traffic-inspection"></a>規劃流量檢查

在許多產業中，客戶要求 Azure 中的流量（尤其是輸入和輸出網際網路流量）會鏡像到網路封包收集器，以進行深度檢查和分析。 本節將探討在 Azure 虛擬網路中鏡像或點擊流量的主要考慮和建議方法。

**設計考慮：**

<!-- docsTest:ignore TAP -->

- [Azure 虛擬網路](https://docs.microsoft.com/azure/virtual-network/virtual-network-tap-overview)點處於預覽狀態，但您必須聯絡 `azurevnettap@microsoft.com` 以取得可用性詳細資料。

- 網路監看員中的封包捕獲已正式運作，但會將捕捉限制為5小時的最長期間。

**設計建議：**

做為 Azure 虛擬網路點的替代方案，請評估下列選項：

- 您可以使用網路監看員封包來捕捉有限的捕捉視窗。

- 評估 NSG 流量記錄 v2 是否提供所需的詳細資料層級。

- 針對需要持續的深度封包檢查的案例，請使用協力廠商解決方案。

- 請勿開發可鏡像流量的自訂解決方案。 雖然這在小規模案例中可能是可接受的，但由於複雜性和可能發生的支援能力問題，因此不鼓勵採用這種方法。
