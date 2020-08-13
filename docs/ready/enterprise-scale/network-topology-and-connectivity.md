---
title: 網路拓樸和連線能力
description: 檢查有關網路和連線的主要設計考慮和建議，以及與 Microsoft Azure 之間的連線和連線能力。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: b89e0e0faa81d3857ce0feb0b8923fc2bf8f4cde
ms.sourcegitcommit: 949b87bad28d32df84df190160089f01826f3a31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2020
ms.locfileid: "88196305"
---
<!-- cSpell:ignore autoregistration BGPs MACsec MPLS MSEE onprem privatelink VPNs -->

# <a name="network-topology-and-connectivity"></a>網路拓樸和連線能力

這篇文章探討主要的設計考慮，以及在 Microsoft Azure 中的網路和連線能力的相關建議。

## <a name="plan-for-ip-addressing"></a>規劃 IP 位址

您的組織務必規劃 Azure 中的 IP 位址，以確保 IP 位址空間不會在內部部署位置與 Azure 區域之間重迭。

**設計考慮：**

- 在內部部署和 Azure 區域之間重迭的 IP 位址空間將會造成重大的爭用挑戰。

- 建立虛擬網路之後，您可以新增位址空間。 如果虛擬網路已透過虛擬網路對等互連連線到另一個虛擬網路，則此程式需要中斷，因為必須刪除和重新建立對等互連。

- Azure 會在每個子網中保留5個 IP 位址。 當您要調整虛擬網路和包含的子網時，這些位址中的因素會納入考慮。

- 某些 Azure 服務需要 [專用的子網](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services#services-that-can-be-deployed-into-a-virtual-network)。 這些服務包括 Azure 防火牆和 Azure VPN 閘道。

- 您可以將子網委派給某些服務，以在子網內建立服務的實例。

**設計建議：**

- 事先規劃跨 Azure 區域和內部部署位置的非重迭 IP 位址空間。

- 使用私人網際網路的位址配置中的 IP 位址 (RFC 1918) 。

- 對於 (RFC 1918) 的私人 IP 位址可用性有限的環境，請考慮使用 IPv6。

- 請勿建立不必要的大型虛擬網路 (例如， `/16`) 以確保不會浪費 IP 位址空間。

- 不要事先規劃所需的位址空間，就不要建立虛擬網路。 新增位址空間將會在虛擬網路透過虛擬網路對等互連連線後導致中斷。

- 請勿將公用 IP 位址用於虛擬網路，特別是當公用 IP 位址不屬於您的組織時。

## <a name="configure-dns-and-name-resolution-for-on-premises-and-azure-resources"></a>設定內部部署和 Azure 資源的 DNS 和名稱解析

網域名稱系統 (DNS) 是整體企業規模架構中的重要設計主題。 有些組織可能會想要使用其在 DNS 中的現有投資。 其他人可能會發現雲端採用是將其內部 DNS 基礎結構現代化並使用原生 Azure 功能的機會。

**設計考慮：**

- 您可以搭配使用 DNS 解析程式與 Azure 私人 DNS 來進行跨單位名稱解析。

- 您可能需要在內部部署和 Azure 之間使用現有的 DNS 解決方案。

- 虛擬網路可以與自動註冊連結的私人 DNS 區域數目上限是一個。

- 若未啟用自動註冊，虛擬網路可連結的私人 DNS 區域數目上限為1000。

**設計建議：**

- 針對 Azure 中的名稱解析為必要的環境，請使用 Azure 私人 DNS 進行解析。 建立委派區域以進行名稱解析 (例如 `azure.contoso.com`) 。

- 對於跨 Azure 和內部部署進行名稱解析的環境，請使用現有的 DNS 基礎結構 (例如，Active Directory 整合的 DNS) 部署到至少兩部 Azure 虛擬機器 (Vm) 。 設定虛擬網路中的 DNS 設定，以使用這些 DNS 伺服器。

- 您仍然可以將 Azure 私人 DNS 區域連結至虛擬網路，並使用 DNS 伺服器作為混合式解析程式，搭配條件式轉送至內部部署 DNS 名稱，例如 `onprem.contoso.com` ，使用內部部署 dns 伺服器。 您可以使用條件轉寄站來設定內部部署伺服器，以便在 azure 中針對 Azure 私人 DNS 區域解析 Vm， (例如 `azure.contoso.com`) 。

- 需要和部署自己的 DNS (（例如 Red Hat OpenShift) ）的特殊工作負載應該使用其慣用的 DNS 解決方案。

- 啟用 Azure DNS 的自動註冊，自動管理部署在虛擬網路內之虛擬機器的 DNS 記錄生命週期。

- 使用虛擬機器做為使用 Azure 私人 DNS 進行跨單位 DNS 解析的解析程式。

- 在全域連線訂用帳戶內建立 Azure 私人 DNS 區域。 例如，您可能會建立其他 Azure 私人 DNS 區域 (例如， `privatelink.database.windows.net` 或 `privatelink.blob.core.windows.net` Azure 私人連結) 。

## <a name="define-an-azure-network-topology"></a>定義 Azure 網路拓朴

網路拓撲是企業級架構的重要元素，因為它會定義應用程式可以彼此通訊的方式。 本節探討企業 Azure 部署的技術和拓撲方法。 它著重于兩種核心方法：以 Azure 虛擬 WAN 為基礎的拓撲，以及傳統拓撲。

以 Azure 虛擬 WAN 為基礎的網路拓撲是適用于大規模多區域部署的最佳企業級方法，您的組織需要將全球位置連線至 Azure 和內部部署。 只要您的組織打算使用軟體定義的 WAN， (SD-WAN) 部署完全與 Azure 整合，您也應該使用虛擬 WAN 網路拓撲。 虛擬 WAN 是用來滿足大規模的互連能力需求。 因為它是由 Microsoft 管理的服務，所以也會降低整體網路的複雜性，並協助將組織的網路現代化。

如果符合下列任一條件，請使用傳統的 Azure 網路拓朴：

- 您的組織打算只在幾個 Azure 區域中部署資源。
- 您不需要全域互連網路。
- 您的每個區域都有很多的遠端或分支位置。 也就是說，您需要的 IP 安全性少於 30 (IPsec) 通道。
- 您需要完整的控制和資料細微性，才能手動設定 Azure 網路。

此傳統拓撲可協助您在 Azure 中建立安全的網路基礎。

## <a name="virtual-wan-network-topology-microsoft-managed"></a>受 Microsoft 管理)  (的虛擬 WAN 網路拓撲

![說明虛擬 WAN 網路拓撲的圖表。](./media/virtual-wan-topology.png)

_圖1：虛擬 WAN 網路拓朴。_

**設計考慮：**

- [Azure 虛擬 WAN](https://docs.microsoft.com/azure/virtual-wan/virtual-wan-about) 是 Microsoft 管理的解決方案，預設會提供端對端全域傳輸連線能力。 虛擬 WAN 中樞可免除手動設定網路連線的需求。 例如，您不需要設定使用者定義的路由 (UDR) 或網路虛擬裝置 (Nva) 來啟用全域傳輸連線能力。

- 虛擬 WAN 藉由建立 [中樞和輪輻網路架構](https://docs.microsoft.com/azure/virtual-wan/virtual-wan-global-transit-network-architecture)，大幅簡化 Azure 和跨單位的端對端網路連線能力。 此架構橫跨多個 Azure 區域和內部部署位置 (任何現成的連線) ，如下圖所示：

  ![說明具有虛擬 WAN 的全球傳輸網路的圖表。](./media/global-transit.png)
  
  _圖2：具有虛擬 WAN 的全球傳輸網路。_

- 虛擬 WAN 任何可轉移的連線，都支援在相同區域內和跨區域) 的下列路徑 (：

  - 虛擬網路到虛擬網路
  - 虛擬網路到分支
  - 分支到虛擬網路
  - 分支至分支

- 虛擬 WAN 中樞已鎖定。 唯一可在其中部署的資源是虛擬網路閘道 (點對站 VPN、站對站 VPN 和 Azure ExpressRoute) 、透過防火牆管理員的 Azure 防火牆，以及路由表。

- 虛擬 WAN 會透過 ExpressRoute 私用對等互連，將最多200個首碼從 Azure 公佈到內部部署，每個虛擬 WAN 中樞增加到10000個首碼。 10000首碼的限制也包括站對站 VPN 和點對站 VPN。

- 區域內和跨區域的網路對網路可轉移連線 () 現已正式推出 (GA) 。

- 虛擬 WAN 中樞對中樞連線能力現已正式推出。

- 標準虛擬 WAN 中虛擬網路之間的傳輸連線已啟用，因為每個虛擬中樞都有路由器。 每個虛擬中樞路由器最多支援 50 Gbps 的彙總輸送量。

- 虛擬 WAN 整合了各種 [SD wan 提供者](https://docs.microsoft.com/azure/virtual-wan/virtual-wan-locations-partners)。

- 許多受控服務提供者都提供虛擬 WAN 的 [受控服務](https://docs.microsoft.com/azure/networking/networking-partners-msp) 。

- 虛擬 WAN 中的 VPN 閘道可針對每個虛擬中樞相應增加至 20 Gbps 和20000個連線。

- 需要具有 premium 附加元件的 ExpressRoute 線路。 它們應該來自 ExpressRoute 全球範圍的位置。

- Azure 防火牆管理員現在在 GA 中，允許在虛擬 WAN 中樞中部署 Azure 防火牆。

- 目前不支援透過 Azure 防火牆的虛擬 WAN 中樞對中樞流量。 或者，使用虛擬 WAN 中的原生中樞對中樞傳輸路由功能。 使用 (Nsg) 的網路安全性群組來允許或封鎖跨中樞的虛擬網路流量。

**設計建議：**

- 我們強烈建議您在 Azure 中進行新的大型或全球網路部署的虛擬 WAN，其中您需要跨 Azure 區域和內部部署位置的全球傳輸連線能力。 如此一來，您就不需要手動設定 Azure 網路的可轉移路由。

  下圖顯示在歐洲與美國散佈資料中心的範例全球企業部署。 這兩個區域中的部署也有大量的分公司。 環境是透過虛擬 WAN 和 ExpressRoute 全球連線來全球化。

  ![範例網路拓撲的圖表。](./media/global-reach-topology.png)
  
  _圖3：範例網路拓撲。_

- 使用虛擬 WAN 作為全域連線能力資源。 使用每個 Azure 區域的虛擬 WAN 中樞，透過本機虛擬 WAN 中樞，將多個登陸區域一起連接到不同的 Azure 區域。

- 使用 ExpressRoute 將虛擬 WAN 中樞連線至內部部署資料中心。

- 透過站對站 VPN，將分支和遠端位置連線到最接近的虛擬 WAN 中樞，或透過 SD-WAN 合作夥伴解決方案，啟用對虛擬 WAN 的分支連線能力。

- 透過點對站 VPN 將使用者連線至虛擬 WAN 中樞。

- 遵循「Azure 中的流量會保留在 Azure 中」原則，讓 Azure 中資源間的通訊透過 Microsoft 骨幹網路進行，即使資源位於不同區域也一樣。

- 在 Azure 區域內的東部/西部和南/北流量保護和篩選的虛擬 WAN 中樞中部署 Azure 防火牆。

- 如果「東部/西部」或「南/北」流量保護和篩選需要合作夥伴 Nva，請將 Nva 部署到個別的虛擬網路，例如 NVA 虛擬網路。 將其連線至區域虛擬 WAN 中樞，以及需要存取 Nva 的登陸區域。 如需詳細資訊，請參閱 [建立 nva 的虛擬 WAN 中樞路由表](https://docs.microsoft.com/azure/virtual-wan/virtual-wan-route-table-portal)。

- 當您部署合作夥伴網路技術和 Nva 時，請遵循合作夥伴廠商的指導方針，以確保沒有與 Azure 網路衝突的設定。

- 不要在 Azure 虛擬 WAN 上建立傳輸網路。 虛擬 WAN 滿足所有可轉移的網路拓朴需求，包括能夠使用合作夥伴 Nva。

- 請勿使用現有的內部部署網路（例如多重通訊協定標籤切換 (MPLS) ，將 Azure 資源連接到 Azure 區域。 Azure 網路技術支援透過 Microsoft 骨幹跨區域互連 Azure 資源。

- 針對您從不是以虛擬 WAN 為基礎的中樞和輪輻網路拓撲進行遷移的 brownfield 案例，請參閱 [遷移至 Azure 虛擬 wan](https://docs.microsoft.com/azure/virtual-wan/migrate-from-hub-spoke-topology)。

- 在連線訂用帳戶內建立 Azure 虛擬 WAN 和 Azure 防火牆資源。

- 請勿為每個虛擬 WAN 虛擬中樞建立超過500個虛擬網路連線。

## <a name="traditional-azure-networking-topology"></a>傳統的 Azure 網路拓朴

雖然虛擬 WAN 提供各種強大的功能，但在某些情況下，傳統的 Azure 網路方法可能是最佳的：

- 如果跨多個 Azure 區域或跨單位的全域可轉移網路不是必要的。 例如，在美國的分支需要連線到歐洲的虛擬網路。

- 如果不需要透過 VPN 連線到大量的遠端位置，或與 SD WAN 解決方案整合。

- 如果您的組織偏好在 Azure 中設定網路拓撲時，有細微的控制和設定。

![說明傳統 Azure 網路拓朴的圖表。](./media/customer-managed-topology.png)

_圖4：傳統的 Azure 網路拓朴。_

**設計考慮：**

- 各種網路拓撲可以連接多個登陸區域虛擬網路。 範例是一個大型的一般虛擬網路、多個連接了多個 ExpressRoute 線路或連線的虛擬網路、中樞和輪輻、完整網狀和混合式。

- 虛擬網路不會跨越訂用帳戶界限。 但是，您可以使用虛擬網路對等互連、ExpressRoute 線路或 VPN 閘道，達到不同訂用帳戶中虛擬網路之間的連線能力。

- 您可以使用虛擬網路對等互連，將相同區域中的虛擬網路連線到不同的 Azure 區域，以及跨不同的 Azure Active Directory (Azure AD) 租使用者。

- 虛擬網路對等互連與全域虛擬網路對等互連不可轉移。 必須要有 Udr 和 Nva，才能啟用傳輸網路。 如需詳細資訊，請參閱 [Azure 中的中樞輪輻網路拓撲](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)。

- 您可以使用 ExpressRoute 線路，在相同地緣政治區域內的虛擬網路之間建立連線，或使用 premium 附加元件來跨地緣政治區域連線。 請記住下列重點：

  - 網路對網路流量可能會遇到較多的延遲，因為流量必須 hairpin 在 Microsoft enterprise edge (MSEE) 路由器上。

  - 頻寬會受限於 ExpressRoute 閘道 SKU。

  - 如果需要檢查或記錄跨虛擬網路的流量，您仍然必須部署和管理 Udr。

- 具有邊界閘道協定 (BGP) 的 VPN 閘道可在 Azure 和內部部署中轉移，但不會跨 ExpressRoute 閘道傳輸。

- 當多個 ExpressRoute 線路連線到相同的虛擬網路時，請使用連接權數和 BGP 技術，確保內部部署與 Azure 之間流量的最佳路徑。 如需詳細資訊，請參閱 [優化 ExpressRoute 路由](https://docs.microsoft.com/azure/expressroute/expressroute-optimize-routing)。

- 使用 BGP 技術來影響 ExpressRoute 路由是 Azure 平臺外部的設定。 您的組織或連線提供者必須據以設定內部部署路由器。

- 具有 premium 附加元件的 ExpressRoute 線路提供了全域連線能力。 不過，每個 ExpressRoute 閘道的 ExpressRoute 連線數目上限為四個。

- 雖然每個虛擬網路的虛擬網路對等互連連線數目上限為500，但可透過 ExpressRoute 私用對等互連從 Azure 通告至內部部署的最大路由數目為200。

- VPN 閘道的最大匯總輸送量為 10 Gbps。 最多可支援30個站對站或網路對網路通道。

**設計建議：**

- 在下列案例中，請考慮以具有非虛擬 WAN 技術的中樞和輪輻網路拓撲為基礎的網路設計：

  - Azure 部署中的流量界限位於 Azure 區域內。

  - 網路架構最多可有兩個 Azure 區域，而在區域間登陸區域的虛擬網路之間有傳輸連線的需求。

  - 網路架構跨越多個 Azure 區域，而跨區域的登陸區域虛擬網路之間不需要可轉移的連線能力。

  - VPN 和 ExpressRoute 連線之間不需要可轉移的連線能力。

  - 主要跨單位連線通道是 ExpressRoute，而 VPN 連線數目小於每個 VPN 閘道30。

  - 集中式 Nva 和複雜/細微的路由會有相當大的相依性。

- 針對區域部署，主要會使用中樞與輪輻拓撲。 使用登陸區域虛擬網路，將虛擬網路對等互連連線至中央中樞虛擬網路，以透過 ExpressRoute 進行跨單位連線、分支連線的 VPN、透過 Nva 和 Udr 的輪輻對輪輻連線，以及透過 NVA 的網際網路輸出保護。 下圖顯示此拓撲。

  ![說明中樞和輪輻網路拓撲的圖表。](./media/hub-and-spoke-topology.png)

  _圖5：中樞和輪輻網路拓撲。_

- 當下列其中一個條件成立時，使用多個 ExpressRoute 線路所連線的多個虛擬網路的拓撲：
 
  - 您需要高層級的隔離。
  - 您需要特定業務單位的專用 ExpressRoute 頻寬。
  - 您已達到每個 ExpressRoute 閘道的連線數目上限 (最多四個) 。
  
  下圖顯示此拓撲。

  ![說明與多個 ExpressRoute 電路連線的多個虛擬網路的圖表。](./media/vnet-multiple-circuits.png)
  
  _圖6：多個使用多個 ExpressRoute 線路連線的虛擬網路。_

- 在中央中樞虛擬網路中，部署一組最基本的共用服務，包括 ExpressRoute 閘道、 (必要) 的 VPN 閘道，以及 Azure 防火牆或合作夥伴 Nva (如必要) 。 如有需要，也請部署 Active Directory 網域控制站和 DNS 伺服器。

- 在中央中樞虛擬網路中，部署適用于東部/西部或南/北流量保護和篩選的 Azure 防火牆或合作夥伴 Nva。

- 當您部署合作夥伴網路技術或 Nva 時，請遵循合作夥伴廠商的指導方針，以確保：

  - 廠商支援部署。
  - 本指南是專為高可用性和最大效能所設計。
  - Azure 網路沒有任何衝突的設定。

- 請勿將 L7 輸入 Nva （例如 Azure 應用程式閘道）部署為中央中樞虛擬網路中的共用服務。 相反地，請將其與應用程式一起部署在其各自的登陸區域中。

- 使用您現有的網路（MPLS 和 SD-WAN）來連接分公司位置與公司總部。 不支援 ExpressRoute 與 VPN 閘道之間的 Azure 傳輸。

- 對於跨 Azure 區域具有多個中樞和輪輻拓撲的網路架構，當少數登陸區域需要跨區域通訊時，請使用全域虛擬網路對等互連來連接登陸區域虛擬網路。 這種方法可提供 VM SKU 允許的全球虛擬網路對等互連高網路頻寬等優點。 但它會略過中央 NVA，以防需要流量檢查或篩選。 這也會受到 [全域虛擬網路對等互連的限制](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#constraints-for-peered-virtual-networks)。

- 當您在兩個 Azure 區域中部署中樞和輪輻網路架構，並在各區域間的所有登陸區域之間傳輸連線時，請使用 ExpressRoute 搭配雙重線路，以提供跨 Azure 區域之登陸區域虛擬網路的傳輸連線能力。 在此案例中，登陸區域可以透過本機中樞虛擬網路中的 NVA，以及透過 ExpressRoute 線路跨區域，在區域內傳輸。 流量必須在 MSEE 路由器上 hairpin。 下圖顯示此設計。

  ![說明登陸區域連接設計的圖表。](./media/vnet-dual-circuits.png)
  
  _圖7：登陸區域連線能力設計。_

- 當您的組織需要跨兩個 Azure 區域的中樞和輪輻網路架構，以及登陸區域之間的全域傳輸連線時，需要跨 Azure 區域的虛擬網路。 您可以透過將中央中樞虛擬網路與全域虛擬網路對等互連進行互連，並使用 Udr 和 Nva 來啟用全域傳輸路由，藉此實現此架構。 由於複雜性和管理額外負荷很高，因此建議您部署具有虛擬 WAN 的全球傳輸網路架構。

- 使用目前在) 預覽中的 [Azure 監視器 network insights](https://docs.microsoft.com/azure/azure-monitor/insights/network-insights-overview) (，來監視您在 Azure 上的網路端對端狀態。

- 請勿為每個中央中樞虛擬網路建立超過200個對等互連連線。 雖然虛擬網路支援最多500對等互連連線，但具有私用對等互連的 ExpressRoute 僅支援從 Azure 到內部部署的最高可向200通告的首碼。

## <a name="connectivity-to-azure"></a>Azure 的連線能力

本節擴展網路拓撲，以考慮將內部部署位置連線至 Azure 的建議模型。

**設計考慮：**

- Azure ExpressRoute 提供專用的私人連線，以 Microsoft Azure 基礎結構即服務 (IaaS) 和平臺即服務 (PaaS) 來自內部部署位置的功能。

- 您可以使用私用連結，透過具有私人對等互連的 ExpressRoute 來建立 PaaS 服務的連線能力。

- 當多個虛擬網路連線到相同的 ExpressRoute 線路時，它們會成為相同路由網域的一部分，而且所有虛擬網路都會共用頻寬。

- 您可以使用 ExpressRoute 全球觸達（若有提供），透過 ExpressRoute 線路將內部部署位置連接在一起，以透過 Microsoft 骨幹網路傳輸流量。

- ExpressRoute 全球範圍可在許多 [expressroute 對等互連位置](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach#availability)中取得。

- ExpressRoute Direct 允許建立多個 ExpressRoute 線路，而不需要額外成本，最多可達 ExpressRoute Direct 埠容量 (10 Gbps 或 100 Gbps) 。 它也可讓您直接連接到 Microsoft 的 ExpressRoute 路由器。 針對 100-Gbps SKU，最小電路頻寬為 5 Gbps。 若為 10 Gbps SKU，最小電路頻寬為 1 Gbps。

<!-- cSpell:ignore prepending -->
<!-- docsTest:ignore "AS PATH prepending -->

**設計建議：**

- 使用 ExpressRoute 作為主要連線通道，以將內部部署網路連線至 Microsoft Azure。 您可以使用 Vpn 做為備份連線的來源，以增強連線能力。

- 當您將內部部署位置連線至 Azure 中的虛擬網路時，請使用來自不同對等互連位置的雙重 ExpressRoute 線路。 此設定會藉由移除內部部署與 Azure 之間的單一失敗點，來確保 Azure 的重複路徑。

- 當您使用多個 ExpressRoute 線路時，請透過 [BGP 本機喜好設定優化 ExpressRoute 路由，並在前面](https://docs.microsoft.com/azure/expressroute/expressroute-optimize-routing#solution-use-as-path-prepending)加上路徑。

- 請確定您是根據頻寬和效能需求，使用正確的 ExpressRoute/VPN 閘道 SKU。

- 在支援的 Azure 區域中部署區域-多餘的 ExpressRoute 閘道。

- 針對需要高於 10 Gbps 或專用 10/100 Gbps 埠之頻寬的案例，請使用 ExpressRoute Direct。

- 當需要低延遲，或從內部部署環境到 Azure 的輸送量必須大於 10 Gbps 時，請讓 FastPath 略過來自資料路徑的 ExpressRoute 閘道。

- 使用 VPN 閘道將分支或遠端位置連線至 Azure。 如需更高的復原能力，請在可用的)  (部署區域冗余閘道。

- 使用 ExpressRoute 全球觸達，連接透過 ExpressRoute 連線到 Azure 的大型辦公室、區域總部或資料中心。

- 當需要流量隔離或專用頻寬時（例如，用來分隔生產和非生產環境），請使用不同的 ExpressRoute 線路。 它可協助您確保隔離的路由網域，並減少雜訊鄰近風險。

- 使用網路效能監控主動監視 ExpressRoute 線路。

- 請勿從單一對等互連位置明確使用 ExpressRoute 線路。 這會建立單一失敗點，讓您的組織容易遭受對等互連位置中斷的影響。

- 請勿使用相同的 ExpressRoute 線路來連接多個需要隔離或專用頻寬的環境，以避免發生雜訊干擾的風險。

## <a name="connectivity-to-azure-paas-services"></a>Azure PaaS 服務的連線能力

本節將根據先前的連線能力章節，探索使用 Azure PaaS 服務的建議連線方法。

**設計考慮：**

- Azure PaaS 服務通常是透過公用端點來存取。 不過，Azure 平臺提供保護這類端點的功能，或甚至使其完全私用：

  - 虛擬網路插入會為支援的服務提供專用的私用部署。 但是管理平面流量會流經公用 IP 位址。

  - [私人連結](https://docs.microsoft.com/azure/private-link/private-endpoint-overview#private-link-resource) 會使用私人 IP 位址對 Azure PaaS 實例或 Azure Load Balancer Standard 背後的自訂服務，提供專用的存取權。

  - 虛擬網路服務端點提供從選取的子網到所選 PaaS 服務的服務層級存取。

- 企業通常會對必須適當緩和的 PaaS 服務的公用端點有疑慮。

- 針對 [支援的服務](https://docs.microsoft.com/azure/private-link/private-link-overview#availability)，私用連結會解決與服務端點相關聯的資料外泄疑慮。 或者，您可以透過 Nva 使用輸出篩選，以提供減輕資料外泄的步驟。

**設計建議：**

- 針對支援的 Azure 服務使用虛擬網路插入，使其可從您的虛擬網路內取得。

- 已插入虛擬網路中的 Azure PaaS 服務仍會使用公用 IP 位址來執行管理平面作業。 使用 Udr 和 Nsg，確保虛擬網路內的通訊受到鎖定。

- 針對共用的 Azure PaaS 服務，使用私用連結（ [如果有的話](https://docs.microsoft.com/azure/private-link/private-link-overview#availability)）。 私用連結已正式適用于數個服務，且適用于許多服務。

- 透過 ExpressRoute 私用對等互連從內部部署存取 Azure PaaS 服務。 針對專用的 Azure 服務或 Azure 私人連結使用虛擬網路插入，以取得可用的共用 Azure 服務。 若要在虛擬網路插入或私人連結無法使用時，從內部部署存取 Azure PaaS 服務，請搭配 Microsoft 對等互連使用 ExpressRoute。 這個方法可避免透過公用網際網路進行傳輸。

- 使用虛擬網路服務端點來保護從虛擬網路存取 Azure PaaS 服務的安全，但只有在私人連結無法使用，且沒有任何資料外泄考慮時。 若要解決服務端點的資料外泄問題，請使用 NVA 篩選或使用虛擬網路服務端點原則進行 Azure 儲存體。

- 預設不會在所有子網上啟用虛擬網路服務端點。

- 除非您使用 NVA 篩選，否則請不要在發生資料外泄問題時使用虛擬網路服務端點。

- 我們不建議您執行強制通道，以啟用從 Azure 到 Azure 資源的通訊。

## <a name="plan-for-inbound-and-outbound-internet-connectivity"></a>規劃輸入和輸出網際網路連線能力

本節說明連至公用網際網路的輸入和輸出連線所建議的連線性模型。

**設計考慮：**

- Azure-原生網路安全性服務（例如 Azure 防火牆、Azure Web 應用程式防火牆） (WAF) 在 Azure 應用程式閘道上，而 Azure Front 開門服務是完全受控的服務。 因此，您不會產生與基礎結構部署相關聯的營運和管理成本，而這可能會變得很複雜。

- 如果您的組織偏好使用 Nva，或在原生服務無法滿足貴組織的特定需求的情況下，企業規模架構與合作夥伴 Nva 完全相容。

**設計建議：**

- 使用 Azure 防火牆來管理：

  - Azure 對網際網路的輸出流量。

  - 非 HTTP/S 輸入連接。

  - 如果您的組織需要) 的「東部/西部」流量篩選 (。

- 使用具有虛擬 WAN 的防火牆管理員，在虛擬 WAN 集線器或中樞虛擬網路中部署和管理 Azure 防火牆。 針對虛擬 WAN 和一般虛擬網路，防火牆管理員現在已正式推出。

- 建立全域 Azure 防火牆原則來管理全域網路環境中的安全性狀態，並將其指派給所有 Azure 防火牆實例。 透過角色型存取控制將累加式防火牆原則委派給本機安全性小組，以允許細微的原則符合特定區域的需求。

- 如果您的組織想要使用這類解決方案來協助保護輸出連線，請在防火牆管理員中設定支援的合作夥伴 SaaS 安全性提供者。

- 使用登陸區域虛擬網路中的 WAF，來保護來自網際網路的輸入 HTTP/S 流量。

- 使用 Azure Front 門板服務和 WAF 原則，在 Azure 區域之間提供全球保護，以進行與登陸區域的輸入 HTTP/S 連接。

- 當您使用 Azure Front 門板服務並 Azure 應用程式閘道來協助保護 HTTP/S 應用程式時，請使用 Azure Front 門板服務中的 WAF 原則。 鎖定 Azure 應用程式閘道，只接收來自 Azure Front 服務的流量。

- 如果「東部/西部」或「南/北」流量保護和篩選需要合作夥伴 Nva：

  - 針對虛擬 WAN 網路拓撲，請將 Nva 部署至個別的虛擬網路 (例如，NVA 虛擬網路) 。 然後將它連線到區域虛擬 WAN 中樞，以及需要存取 Nva 的登陸區域。 [本文說明此](https://docs.microsoft.com/azure/virtual-wan/virtual-wan-route-table-portal) 程式。
  - 若是非虛擬 WAN 網路拓撲，請在中央中樞虛擬網路中部署合作夥伴 Nva。

- 如果輸入 HTTP/S 連線需要合作夥伴 Nva，請將它們部署在登陸區域虛擬網路中，並連同其保護並公開到網際網路的應用程式。

- 使用 [Azure DDoS 保護標準保護計劃](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview) ，協助保護虛擬網路內裝載的所有公用端點。

- 請勿將內部部署周邊網路概念和架構複寫到 Azure。 Azure 中有類似的安全性功能可供使用，但執行和架構必須適應雲端。

## <a name="plan-for-app-delivery"></a>規劃應用程式傳遞

本節將探討主要的建議，以安全、可高度擴充且高可用性的方式提供內部面向和外部面向應用程式。

**設計考慮：**

- Azure Load Balancer (內部和公用) 為區域層級的應用程式傳遞提供高可用性。

- Azure 應用程式閘道允許在區域層級安全地傳遞 HTTP/S 應用程式。

- Azure 前端服務可讓您在 Azure 區域之間安全地傳遞高可用性 HTTP/S 應用程式。

- Azure 流量管理員允許傳遞全域應用程式。

**設計建議：**

- 針對內部面向和外部面向應用程式，在登陸區域內執行應用程式傳遞。

- 針對 HTTP/S 應用程式的安全傳遞，請使用應用程式閘道 v2，並確定已啟用 WAF 保護和原則。

- 如果您無法使用應用程式閘道 v2 來取得 HTTP/S 應用程式的安全性，請使用合作夥伴 NVA。

- 部署 Azure 應用程式閘道 v2 或合作夥伴 Nva，用於登陸區域虛擬網路內的輸入 HTTP/S 連線，以及其所保護的應用程式。

- 針對登陸區域中的所有公用 IP 位址使用 DDoS 標準保護計劃。

- 使用 Azure Front 門板服務搭配 WAF 原則，以提供並協助保護跨越 Azure 區域的全域 HTTP/S 應用程式。

- 當您使用 Azure Front 門板服務和應用程式閘道來協助保護 HTTP/S 應用程式時，請使用 Azure Front 門板服務中的 WAF 原則。 鎖定應用程式閘道，只接收來自 Azure Front 服務的流量。

- 使用流量管理員傳遞跨越 HTTP/S 以外通訊協定的全球應用程式。

## <a name="plan-for-landing-zone-network-segmentation"></a>規劃登陸區域網路分割

本節將探討在登陸區域內提供高度安全的內部網路分割以驅動網路零信任的執行的主要建議。

**設計考慮：**

- 零信任模型會假設違反的狀態，並驗證每個要求，就像是來自不受控制的網路一樣。

- 先進的零信任網路實施採用完全分散的輸入/輸出雲端微周邊和更深入的微分割。

- 網路安全性群組可以使用 Azure 服務標籤來加速連線到 Azure PaaS 服務。

- 應用程式安全性群組不會跨越或提供跨虛擬網路的保護。

- Azure Resource Manager 的範本現在支援 NSG 流量記錄。

**設計建議：**

- 將子網建立委派給登陸區域擁有者。 這可讓他們定義如何跨子網劃分工作負載 (例如，單一大型子網、多層式應用程式，或網路插入的應用程式) 。 平臺小組可以使用 Azure 原則，以確保具有特定規則的 NSG (例如拒絕來自網際網路的連入 SSH 或 RDP，或允許/封鎖在登陸區域之間的流量) 一律與具有僅限拒絕原則的子網相關聯。

- 使用 Nsg 協助保護跨子網的流量，以及跨平臺的東部/西部流量 (登陸區域) 之間的流量。

- 應用程式小組應該使用子網層級 Nsg 的應用程式安全性群組，協助保護登陸區域內的多層式 Vm。

- 使用 Nsg 和應用程式安全性群組來微分割登陸區域內的流量，並避免使用中央 NVA 來篩選流量。

- 啟用 NSG 流量記錄，並將其饋送至流量分析，以深入瞭解內部和外部流量流程。

- 使用 Nsg 選擇性地允許登陸區域之間的連線。

- 針對虛擬 WAN 拓撲，如果您的組織需要在登陸區域流動流量的篩選和記錄功能，請透過 Azure 防火牆將流量路由傳送到登陸區域。

## <a name="define-network-encryption-requirements"></a>定義網路加密需求

本節將探討在內部部署與 Azure 之間，以及跨 Azure 區域，達到網路加密的主要建議。

**設計考慮：**

- 成本和可用頻寬會與端點之間的加密通道長度成正比。

- 當您使用 VPN 連線到 Azure 時，流量會透過網際網路透過 IPsec 通道來加密。

- 當您使用具有私人對等互連的 ExpressRoute 時，流量目前不會加密。

- 您可以將 [媒體存取控制安全性 (MACsec) ](https://docs.microsoft.com/azure/expressroute/expressroute-howto-MACsec) 加密套用至 ExpressRoute Direct，以達成網路加密。

- Azure 目前不提供透過全域虛擬網路對等互連的原生加密。 如果您需要在 Azure 區域之間進行加密，可以使用 VPN 閘道（而非全域虛擬網路對等互連）來連接虛擬網路。

**設計建議：**

![說明加密流程的圖表。](./media/enc-flows.png)

_圖8：加密流程。_

- 當您使用 VPN 閘道來建立從內部部署至 Azure 的 VPN 連線時，流量會透過 IPsec 通道以通訊協定層級加密。 上圖顯示此加密在流程中 `A` 。

- 當您使用 ExpressRoute Direct 時，請設定 [MACsec](https://docs.microsoft.com/azure/expressroute/expressroute-howto-MACsec) ，以將層級的流量加密（您組織的路由器和 MSEE 之間的兩個層級）。 此圖顯示此加密在流程中 `B` 。

- 針對 MACsec 不是 (例如，不使用 ExpressRoute Direct) 的虛擬 WAN 案例，請使用虛擬 WAN VPN 閘道，透過 ExpressRoute 私用對等互連來建立 IPsec 通道。 此圖顯示此加密在流程中 `C` 。

- 若是非虛擬 WAN 案例，而且 MACsec 不是 (的選項（例如，不使用 ExpressRoute Direct) ），唯一的選項是：
  
  - 使用合作夥伴 Nva 透過 ExpressRoute 私用對等互連來建立 IPsec 通道。
  - 使用 Microsoft 對等互連，透過 ExpressRoute 建立 VPN 通道。

- 如果必須加密 Azure 區域之間的流量，請使用 VPN 閘道將虛擬網路連線到各個區域。

- 如流程和圖表中所示 (原生 Azure 解決方案 `B` `C`) 不符合您的需求，請使用 Azure 中的合作夥伴 nva，透過 ExpressRoute 私用對等互連來加密流量。

## <a name="plan-for-traffic-inspection"></a>規劃流量檢查

在許多產業中，組織會要求 Azure 中的流量會鏡像到網路封包收集器，以進行深度檢查和分析。 這項需求通常著重在輸入和輸出網際網路流量。 本節將探討在 Azure 虛擬網路中鏡像或點擊流量的主要考慮和建議方法。

**設計考慮：**

<!-- docsTest:ignore TAP -->

- [Azure 虛擬網路終端機存取點 (請) ](https://docs.microsoft.com/azure/virtual-network/virtual-network-tap-overview) 處於預覽狀態。 請聯絡 `azurevnettap@microsoft.com` 可用性詳細資料。

- Azure 網路監看員中的封包捕獲已正式運作，但會將捕捉限制為5小時的最長期間。

**設計建議：**

作為 Azure 虛擬網路點的替代方案，請評估下列選項：

- 您可以使用網路監看員封包來捕捉有限的捕捉視窗。

- 評估最新版本的 NSG 流量記錄是否提供您所需的詳細資料層級。

- 針對需要深入封包檢查的案例，請使用合作夥伴解決方案。

- 請勿開發可鏡像流量的自訂解決方案。 雖然這種方法在小規模案例中可能是可接受的，但我們不會因為複雜度和可能發生的支援性問題而大規模地鼓勵您這麼做。
