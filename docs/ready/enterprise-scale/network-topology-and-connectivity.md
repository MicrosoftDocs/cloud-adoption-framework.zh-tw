---
title: 網路拓樸和連線能力
description: 檢查有關網路功能的主要設計考慮和建議，以及在 Microsoft Azure 之間的連線能力和連線能力。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 1f315f1c07ce381043b5f338fd7602678908cb63
ms.sourcegitcommit: 8b82889dca0091f3cc64116f998a3a878943c6a1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/09/2020
ms.locfileid: "89602231"
---
<!-- cSpell:ignore autoregistration BGPs MACsec MPLS MSEE onprem privatelink VPNs -->

# <a name="network-topology-and-connectivity"></a>網路拓樸和連線能力

本文將探討有關網路功能的主要設計考慮和建議，以及在 Microsoft Azure 之間的連線能力和連線能力。

## <a name="plan-for-ip-addressing"></a>規劃 IP 定址

您的組織務必規劃 Azure 中的 IP 位址，以確保在內部部署位置和 Azure 區域之間的 IP 位址空間不會重迭。

**設計考慮：**

- 跨內部部署和 Azure 區域的 IP 位址空間重迭會造成重大的爭用挑戰。

- 您可以在建立虛擬網路之後新增位址空間。 如果虛擬網路已透過虛擬網路對等互連連線到另一個虛擬網路，則此程式需要中斷，因為對等互連必須刪除再重新建立。

- Azure 會在每個子網中保留5個 IP 位址。 當您要調整虛擬網路和包含的子網大小時，這些位址中的因素。

- 某些 Azure 服務需要 [專用子網](/azure/virtual-network/virtual-network-for-azure-services#services-that-can-be-deployed-into-a-virtual-network)。 這些服務包括 Azure 防火牆和 Azure VPN 閘道。

- 您可以將子網委派給特定服務，以在子網內建立服務的實例。

**設計建議：**

- 事先規劃跨 Azure 區域與內部部署位置的非重迭 IP 位址空間。

- 使用私人網際網路位址配置中的 IP 位址 (RFC 1918) 。

- 針對可用性有限的私人 IP 位址 (RFC 1918) 的環境，請考慮使用 IPv6。

- 請勿建立非必要的大型虛擬網路 (例如， `/16`) 確保 IP 位址空間不會浪費。

- 請勿事先規劃所需的位址空間，而不建立虛擬網路。 新增位址空間將會在透過虛擬網路對等互連連線虛擬網路之後，導致中斷。

- 請勿將公用 IP 位址用於虛擬網路，特別是如果公用 IP 位址不屬於您的組織。

## <a name="configure-dns-and-name-resolution-for-on-premises-and-azure-resources"></a>設定內部部署和 Azure 資源的 DNS 和名稱解析

網域名稱系統 (DNS) 是整個企業規模架構中的重要設計主題。 某些組織可能會想要在 DNS 中使用現有的投資。 其他人可能會看到雲端採用將其內部 DNS 基礎結構現代化，並使用原生 Azure 功能的機會。

**設計考慮：**

- 您可以使用 DNS 解析程式搭配 Azure 私人 DNS 來進行跨單位名稱解析。

- 您可能需要在內部部署與 Azure 之間使用現有的 DNS 解決方案。

- 虛擬網路可與自動註冊連結的私人 DNS 區域數目上限為一個。

- 虛擬網路可以連結的私人 DNS 區域數目上限為1000，但未啟用自動註冊。

**設計建議：**

- 針對 Azure 中的名稱解析是必要的環境，請使用 Azure 私人 DNS 進行解析。 建立委派區域以進行名稱解析 (例如 `azure.contoso.com`) 。

- 針對在 Azure 和內部部署之間進行名稱解析的環境，請使用現有的 DNS 基礎結構 (例如，Active Directory 整合的 DNS) 部署到至少兩部 Azure 虛擬機器 (Vm) 。 在虛擬網路中設定 DNS 設定，以使用這些 DNS 伺服器。

- 您仍然可以將 Azure 私人 DNS 區域連結至虛擬網路，並使用 DNS 伺服器做為混合式解析程式，搭配使用內部部署 dns 名稱的條件式轉送（例如 `onprem.contoso.com` ），並使用內部部署 dns 伺服器。 您可以使用條件轉寄站來設定內部部署伺服器，以在 azure 中針對 Azure 私人 DNS 區域解析 Vm (例如 `azure.contoso.com`) 。

- 需要和部署自己的 DNS (（例如 Red Hat OpenShift) ）的特殊工作負載應使用其慣用的 DNS 解決方案。

- 啟用 Azure DNS 的自動註冊，自動管理虛擬網路內部署的虛擬機器 DNS 記錄的生命週期。

- 使用虛擬機器作為使用 Azure 私人 DNS 解析跨單位 DNS 的解析程式。

- 在全域連線訂用帳戶內建立 Azure 私人 DNS 區域。 例如，您可以建立其他 Azure 私人 DNS 區域 (`privatelink.database.windows.net` 或 `privatelink.blob.core.windows.net` Azure Private Link) 。

## <a name="define-an-azure-network-topology"></a>定義 Azure 網路拓撲

網路拓撲是企業規模架構的重要元素，因為它會定義應用程式彼此通訊的方式。 本節探討適用于企業 Azure 部署的技術與拓撲方法。 它著重于兩個核心方法：以 Azure 虛擬 WAN 和傳統拓撲為基礎的拓撲。

如果下列任何一項成立，請使用以 Azure 虛擬 WAN 為基礎的網路拓撲：

- 您的組織想要跨數個 Azure 區域部署資源，並需要將您的全球位置連線到 Azure 和內部部署。
- 您的組織想要使用軟體定義的 WAN (SD-WAN) 部署與 Azure 完全整合。
- 您想要在所有連線到單一 Azure 虛擬 WAN 中樞的 Vnet 上部署最多2000的虛擬機器工作負載。

虛擬 WAN 可用來滿足大規模的互連能力需求。 因為這是 Microsoft 管理的服務，所以它也會降低整體網路複雜度，並有助於將組織的網路現代化。

如果下列任何一項成立，請使用傳統的 Azure 網路拓撲：

- 您的組織打算只在幾個 Azure 區域中部署資源。
- 您不需要有全球互連的網路。
- 您每個區域的遠端或分支位置都有很多。 也就是說，您需要少於30個 IP 安全性 (IPsec) 通道。
- 您需要完整的控制和細微性，才能手動設定 Azure 網路。

此傳統拓撲可協助您在 Azure 中建立安全的網路基礎。

## <a name="virtual-wan-network-topology-microsoft-managed"></a>虛擬 WAN 網路拓撲 (Microsoft 管理的) 

![說明虛擬 WAN 網路拓撲的圖表。](./media/virtual-wan-topology.png)

_圖1：虛擬 WAN 網路拓撲。_

**設計考慮：**

- [Azure 虛擬 WAN](/azure/virtual-wan/virtual-wan-about) 是由 Microsoft 管理的解決方案，預設會提供端對端的全球傳輸連線能力。 虛擬 WAN 中樞無須手動設定網路連線能力。 例如，您不需要設定使用者定義的路由 (UDR) 或網路虛擬裝置 (Nva) 以啟用全域傳輸連線。

- 虛擬 WAN 藉由建立 [中樞和輪輻網路架構](/azure/virtual-wan/virtual-wan-global-transit-network-architecture)，大幅簡化了 Azure 和跨單位的端對端網路連線能力。 此架構會跨多個 Azure 區域和內部部署位置 (任何現成的連線) ，如下圖所示：

  ![此圖說明使用虛擬 WAN 的全球傳輸網路。](./media/global-transit.png)
  
  _圖2：使用虛擬 WAN 的全球傳輸網路。_

- 虛擬 WAN 任意對任意可轉移的連線能力支援在相同區域內和跨區域)  (下列路徑：

  - 虛擬網路對虛擬網路
  - 虛擬網路到分支
  - 分支至虛擬網路
  - 分支至分支

- 虛擬 WAN 中樞已鎖定。 您唯一可以在其中部署的資源為虛擬網路閘道 (點對站 VPN、站對站 VPN 和 Azure ExpressRoute) 、透過防火牆管理員的 Azure 防火牆，以及路由表。

- 虛擬 WAN 可透過 ExpressRoute 私用對等互連，將每個虛擬 WAN 中樞的10000首碼最多增加200個首碼。 10000首碼的限制也包含站對站 VPN 和點對站 VPN。

- 區域內和跨) 區域的網路對網路可轉移連線 (，現已正式推出 (GA) 。

- 虛擬 WAN 中樞對中樞連線現在已正式推出。

- 由於每個虛擬中樞都有路由器，因此會啟用標準虛擬 WAN 中虛擬網路之間的傳輸連線。 每個虛擬中樞路由器最多支援 50 Gbps 的彙總輸送量。

- 在所有 Vnet 上最多可以有2000的 VM 工作負載連線到單一虛擬 WAN 中樞。

- 虛擬 WAN 與各種 [SD wan 提供者](/azure/virtual-wan/virtual-wan-locations-partners)整合。

- 許多受控服務提供者為虛擬 WAN 提供 [受控服務](/azure/networking/networking-partners-msp) 。

- 虛擬 WAN 中的 VPN 閘道最多可以針對每個虛擬中樞擴大至 20 Gbps 和20000連線。

- 需要 premium 附加元件的 ExpressRoute 線路。 它們應該來自于 ExpressRoute 的全球接觸位置。

- Azure 防火牆管理員現已正式推出，可讓您在虛擬 WAN 中樞中部署 Azure 防火牆。

- 目前不支援透過 Azure 防火牆的虛擬 WAN 中樞對中樞流量。 或者，您也可以使用虛擬 WAN 中的原生中樞對中樞傳輸路由功能。 使用網路安全性群組 (Nsg) 來允許或封鎖跨中樞的虛擬網路流量。

**設計建議：**

- 針對 Azure 中的新大型或全球網路部署，我們建議虛擬 WAN，您需要跨 Azure 區域和內部部署位置的全球傳輸連線能力。 如此一來，您就不需要手動設定 Azure 網路的可轉移路由。

  下圖顯示在歐洲和美國分佈的資料中心的範例全球企業部署。 這兩個區域中的部署也有大量的分公司。 環境透過虛擬 WAN 和 ExpressRoute 全球接觸來全球連線。

  ![範例網路拓撲的圖表。](./media/global-reach-topology.png)
  
  _圖3：網路拓撲範例。_

- 使用虛擬 WAN 作為全球連線資源。 您可以使用每個 Azure 區域的虛擬 WAN 中樞，透過本機虛擬 WAN 中樞，將多個登陸區域連接在一起的 Azure 區域。

- 使用 ExpressRoute 將虛擬 WAN 中樞連線至內部部署資料中心。

- 透過站對站 VPN 將分支和遠端位置連接到最接近的虛擬 WAN 中樞，或透過 SD WAN 合作夥伴解決方案來啟用虛擬 WAN 的分支連線能力。

- 透過點對站 VPN 將使用者連線到虛擬 WAN 中樞。

- 遵循「Azure 中的流量會保留在 Azure 中」的原則，以便在 Azure 中跨資源的通訊會透過 Microsoft 骨幹網路進行，即使資源位於不同的區域。

- 在適用于東亞/西部的虛擬 WAN 中樞中部署 Azure 防火牆，並在 Azure 區域內部署南/北流量保護和篩選。

- 如果東部/西部或南/北流量保護和篩選需要合作夥伴 Nva，請將 Nva 部署到個別的虛擬網路，例如 NVA 虛擬網路。 將它連線到區域虛擬 WAN 中樞以及需要存取 Nva 的登陸區域。 如需詳細資訊，請參閱 [建立 nva 的虛擬 WAN 中樞路由表](/azure/virtual-wan/virtual-wan-route-table-portal)。

- 當您部署合作夥伴網路技術和 Nva 時，請遵循合作夥伴廠商的指導方針，以確保 Azure 網路功能沒有任何衝突的設定。

- 請勿在 Azure 虛擬 WAN 之上建立傳輸網路。 虛擬 WAN 可滿足所有的可轉移網路拓撲需求，包括使用 partner Nva 的能力。

- 請勿使用現有的內部部署網路，例如多重通訊協定標籤切換 (MPLS) ，以在 Azure 區域之間連接 Azure 資源。 Azure 網路技術支援透過 Microsoft 骨幹跨區域互連 Azure 資源。

- 針對您從不是以虛擬 WAN 為基礎的中樞和輪輻網路拓撲進行遷移的棕色地帶案例，請參閱 [遷移至 Azure 虛擬 wan](/azure/virtual-wan/migrate-from-hub-spoke-topology)。

- 在連接訂用帳戶內建立 Azure 虛擬 WAN 和 Azure 防火牆資源。

- 請勿為每個虛擬 WAN 虛擬中樞建立500個以上的虛擬網路連線。

- 請仔細規劃您的部署，並確定您的網路架構是在 [Azure 虛擬 WAN 限制](/azure/azure-resource-manager/management/azure-subscription-service-limits#virtual-wan-limits)內。

## <a name="traditional-azure-networking-topology"></a>傳統的 Azure 網路拓撲

雖然虛擬 WAN 提供各式各樣的強大功能，但在某些情況下，傳統的 Azure 網路方法可能會是最佳的：

- 如果不需要跨多個 Azure 區域或跨單位的全球可轉移網路。 例如，在美國的分支，需要連接到歐洲的虛擬網路。

- 如果不需要透過 VPN 連線到大量的遠端位置，或與 SD WAN 解決方案整合。

- 如果您組織的喜好設定是在 Azure 中設定網路拓撲時有細微的控制和設定。

![說明傳統 Azure 網路拓撲的圖表。](./media/customer-managed-topology.png)

_圖4：傳統的 Azure 網路拓撲。_

**設計考慮：**

- 各種網路拓撲都可以連接多個登陸區域虛擬網路。 範例是一個大型的一般虛擬網路、多個連接多個 ExpressRoute 線路的虛擬網路、中樞和輪輻、全網格和混合式。

- 虛擬網路不會跨越訂用帳戶界限。 但是，您可以使用虛擬網路對等互連、ExpressRoute 線路或 VPN 閘道，在不同訂用帳戶中的虛擬網路之間達到連線能力。

- 您可以使用虛擬網路對等互連，將相同區域、不同 Azure 區域，以及不同 Azure Active Directory (Azure AD) 租使用者之間的虛擬網路連線。

- 虛擬網路對等互連和全域虛擬網路對等互連無法轉移。 需要 Udr 和 Nva，才能啟用傳輸網路。 如需詳細資訊，請參閱 [Azure 中的中樞輪輻網路拓撲](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)。

- 您可以使用 ExpressRoute 線路，在相同地緣政治區域內的虛擬網路之間建立連線，或使用 premium 附加元件在地緣政治區域間進行連線。 請記住下列重點：

  - 網路對網路流量可能會遇到更多延遲，因為流量必須在 Microsoft enterprise edge (MSEE) 路由器上 hairpin。

  - 頻寬會受限於 ExpressRoute 閘道 SKU。

  - 如果您需要檢查或記錄跨虛擬網路的流量，您仍然必須部署及管理 Udr。

- 具有邊界閘道協定 (BGP) 的 VPN 閘道可在 Azure 和內部部署中轉移，但不會跨 ExpressRoute 閘道傳輸。

- 當有多個 ExpressRoute 線路連線到相同的虛擬網路時，請使用連線權數和 BGP 技術，以確保內部部署與 Azure 之間流量的最佳路徑。 如需詳細資訊，請參閱 [優化 ExpressRoute 路由](/azure/expressroute/expressroute-optimize-routing)。

- 使用 BGP 技術來影響 ExpressRoute 路由是 Azure 平臺以外的設定。 您的組織或連接提供者必須據以設定內部部署路由器。

- ExpressRoute 線路與 premium 附加元件提供全球連線能力。 不過，每個 ExpressRoute 閘道的 ExpressRoute 連線數目上限為四個。

- 雖然每個虛擬網路的虛擬網路對等互連連線數目上限是500，但可透過 ExpressRoute 私用對等互連從 Azure 向內部部署公告的最大路由數目是200。

- VPN 閘道的匯總輸送量上限為 10 Gbps。 它支援最多30個站對站或網路對網路通道。

**設計建議：**

- 在下列案例中，請考慮以中樞和輪輻網路拓撲為基礎的網路設計，搭配非虛擬 WAN 技術：

  - Azure 部署中的流量界限位於 Azure 區域內。

  - 網路架構最多可有兩個 Azure 區域，而且跨區域的登陸區域的虛擬網路之間有傳輸連線的需求。

  - 網路架構跨越多個 Azure 區域，而且跨區域的登陸區域的虛擬網路之間不需要可轉移的連線能力。

  - VPN 和 ExpressRoute 連線之間不需要可轉移的連線能力。

  - 主要的跨單位連線通道是 ExpressRoute，而且每個 VPN 閘道的 VPN 連線數目小於30。

  - 集中式 Nva 和複雜/細微路由的相依性很高。

- 針對區域部署，主要是使用中樞與輪輻拓撲。 使用登陸區域虛擬網路，將虛擬網路對等互連連線到中央中樞虛擬網路，以透過 ExpressRoute、適用于分支連線的 VPN、透過 Nva 和 Udr 的輪輻對輪輻連線，以及透過 NVA 的網際網路輸出保護來進行跨單位連線。 下圖顯示此拓撲。

  ![說明中樞和輪輻網路拓撲的圖表。](./media/hub-and-spoke-topology.png)

  _圖5：中樞和輪輻網路拓撲。_

- 當下列其中一個條件成立時，請使用與多個 ExpressRoute 線路連線之多個虛擬網路的拓撲：

  - 您需要高層級的隔離。
  - 您需要特定業務單位的專用 ExpressRoute 頻寬。
  - 您已達到每個 ExpressRoute 閘道的最大連線數目 (四個) 。
  
  下圖顯示此拓撲。

  ![說明多個 ExpressRoute 線路連線之多個虛擬網路的圖表。](./media/vnet-multiple-circuits.png)
  
  _圖6：多個連接到多個 ExpressRoute 線路的虛擬網路。_

- 部署一組最基本的共用服務，包括 ExpressRoute 閘道、VPN 閘道 (視需要) ，以及在中央中樞虛擬網路中) 所需的 Azure 防火牆或合作夥伴 Nva (。 如有必要，也請部署 Active Directory 網域控制站和 DNS 伺服器。

- 在中央中樞虛擬網路中，部署適用于東部/西部的 Azure 防火牆或合作夥伴 Nva，或南/北流量保護和篩選。

- 當您部署合作夥伴網路技術或 Nva 時，請遵循合作夥伴廠商的指導方針，以確保：

  - 廠商支援部署。
  - 本指南是針對高可用性和最大效能所設計。
  - Azure 網路功能沒有任何衝突的設定。

- 請勿將 L7 輸入 Nva （例如 Azure 應用程式閘道）部署為中央中樞虛擬網路中的共用服務。 相反地，請將其與應用程式一起部署在各自的登陸區域中。

- 使用您現有的網路（MPLS 和 SD WAN）來與公司總部連接分公司位置。 不支援 ExpressRoute 與 VPN 閘道之間的 Azure 傳輸。

- 針對跨 Azure 區域具有多個中樞和輪輻拓撲的網路架構，當少數登陸區域需要跨區域進行通訊時，請使用全域虛擬網路對等互連來連接登陸區域虛擬網路。 這種方法提供的優點像是具有全域虛擬網路對等互連的高網路頻寬，如同 VM SKU 所允許。 但它會略過中央 NVA，以防需要進行流量檢查或篩選。 這也會受到 [全域虛擬網路對等互連的限制](/azure/virtual-network/virtual-network-peering-overview#constraints-for-peered-virtual-networks)。

- 當您在兩個 Azure 區域中部署中樞和輪輻網路架構，且需要跨區域的所有登陸區域之間的傳輸連線時，請使用 ExpressRoute 搭配雙重線路，以提供跨 Azure 區域之登陸區域虛擬網路的傳輸連線能力。 在此案例中，登陸區域可以透過本機中樞虛擬網路中的 NVA，以及透過 ExpressRoute 線路跨區域在區域內傳輸。 流量必須在 MSEE 路由器上 hairpin。 下圖顯示此設計。

  ![說明登陸區域連線能力設計的圖表。](./media/vnet-dual-circuits.png)
  
  _圖7：登陸區域連接設計。_

- 當您的組織需要跨兩個 Azure 區域的中樞和輪輻網路架構，以及登陸區域之間的全球傳輸連線能力時，需要跨 Azure 區域的虛擬網路。 您可以藉由將中央中樞虛擬網路與全域虛擬網路對等互連進行虛擬化，並使用 Udr 和 Nva 來啟用全域傳輸路由，來執行此架構。 因為複雜性和管理的額外負荷很高，建議您部署具有虛擬 WAN 的全域傳輸網路架構。

- 使用 [Azure 監視器的網路深入](/azure/azure-monitor/insights/network-insights-overview) 解析 (目前為預覽狀態) 來監視 Azure 上網路的端對端狀態。

- 請勿為每個中央中樞虛擬網路建立超過200個對等互連連線。 雖然虛擬網路支援最多500對等互連連線，但具有私人對等互連的 ExpressRoute 僅支援從 Azure 到內部部署的最多200首碼。

## <a name="connectivity-to-azure"></a>連接至 Azure

本節將擴充網路拓撲，以考慮將內部部署位置連線到 Azure 的建議模型。

**設計考慮：**

- Azure ExpressRoute 提供 Azure 基礎結構即服務的專用私人連線， (IaaS) 和平臺即服務， (PaaS) 內部部署位置的功能。

- 您可以使用 Private Link，透過具有私人對等互連的 ExpressRoute 來建立 PaaS 服務的連線能力。

- 當多個虛擬網路連線至相同的 ExpressRoute 線路時，它們會成為相同路由網域的一部分，而且所有虛擬網路都會共用頻寬。

- 您可以使用 ExpressRoute 全球存取範圍（如有提供），透過 ExpressRoute 線路將內部部署位置連接在一起，以透過 Microsoft 骨幹網路傳輸流量。

- ExpressRoute Global 觸及可在許多 [expressroute 對等互連位置](/azure/expressroute/expressroute-global-reach#availability)中使用。

- ExpressRoute Direct 可讓您不需額外成本，即可建立多個 ExpressRoute 線路，最多可達 ExpressRoute Direct 埠容量 (10 Gbps 或 100 Gbps) 。 它也可讓您直接連接到 Microsoft 的 ExpressRoute 路由器。 針對 100-Gbps SKU，最小電路頻寬為 5 Gbps。 若為 10 Gbps SKU，最小電路頻寬為 1 Gbps。

<!-- cSpell:ignore prepending -->
<!-- docsTest:ignore "AS PATH prepending" -->

**設計建議：**

- 使用 ExpressRoute 作為將內部部署網路連線到 Azure 的主要連接通道。 您可以使用 Vpn 作為備份連線的來源，以增強連線恢復功能。

- 當您將內部部署位置連線到 Azure 中的虛擬網路時，請使用來自不同對等互連位置的雙重 ExpressRoute 線路。 這項設定可移除內部部署與 Azure 之間的單一失敗點，以確保 Azure 有重複的路徑。

- 當您使用多個 ExpressRoute 線路時，請透過 [BGP 本機喜好設定和路徑前面的路徑來優化 expressroute 路由](/azure/expressroute/expressroute-optimize-routing#solution-use-as-path-prepending)。

- 根據頻寬和效能需求，確定您使用的是 ExpressRoute/VPN 閘道的正確 SKU。

- 在支援的 Azure 區域中部署區域冗余 ExpressRoute 閘道。

- 針對需要高於 10 Gbps 或專用 10/100 Gbps 埠的頻寬的案例，請使用 ExpressRoute Direct。

- 當需要低延遲，或從內部部署至 Azure 的輸送量必須大於 10 Gbps 時，可讓 FastPath 略過資料路徑的 ExpressRoute 閘道。

- 使用 VPN 閘道將分支或遠端位置連接至 Azure。 如需更高的復原能力，請在可用)  (部署區域冗余閘道。

- 使用 ExpressRoute 全球接觸來連接大型辦公室、區域總部，或透過 ExpressRoute 連接到 Azure 的資料中心。

- 需要流量隔離或專用頻寬（例如用於分隔生產和非生產環境）時，請使用不同的 ExpressRoute 線路。 它將協助您確保隔離的路由網域，並減輕雜訊鄰近風險。

- 使用網路效能監控主動監視 ExpressRoute 線路。

- 請勿明確地從單一對等互連位置使用 ExpressRoute 線路。 這會產生單一失敗點，並讓您的組織容易發生對等互連位置中斷的影響。

- 請勿使用相同的 ExpressRoute 線路來連接多個需要隔離或專用頻寬的環境，以避免雜訊的風險。

## <a name="connectivity-to-azure-paas-services"></a>Azure PaaS 服務的連線能力

本節將探討先前的連線章節，探討使用 Azure PaaS 服務的建議連線方法。

**設計考慮：**

- 通常會透過公用端點來存取 Azure PaaS 服務。 不過，Azure 平臺會提供保護這類端點的功能，甚至讓它們完全私密：

  - 虛擬網路插入為支援的服務提供專用的私人部署。 但管理平面流量會流經公用 IP 位址。

  - [Private Link](/azure/private-link/private-endpoint-overview#private-link-resource) 使用私人 IP 位址來提供專用存取，方法是使用私人 IP 位址來存取 Azure PaaS 實例或 Azure Load Balancer Standard 後方的自訂服務

  - 虛擬網路服務端點可從選取的子網提供服務層級的存取權給選取的 PaaS 服務。

- 企業通常會對於必須適當地緩和的 PaaS 服務的公用端點有疑慮。

- 針對 [支援的服務](/azure/private-link/private-link-overview#availability)，Private Link 可解決與服務端點相關聯的資料遭到外泄問題。 或者，您可以透過 Nva 使用輸出篩選，以提供減少資料遭到外泄的步驟。

**設計建議：**

- 針對支援的 Azure 服務使用虛擬網路插入，使其可從您的虛擬網路中使用。

- 已插入虛擬網路的 Azure PaaS 服務仍會使用公用 IP 位址執行管理平面作業。 使用 Udr 和 Nsg，確定已在虛擬網路內鎖定此通訊。

- 使用適用于共用 Azure PaaS 服務的 Private Link （ [如果有的話](/azure/private-link/private-link-overview#availability)）。 Private Link 已正式推出數項服務，並為許多服務提供公開預覽。

- 透過 ExpressRoute 私用對等互連從內部部署存取 Azure PaaS 服務。 針對專用的 Azure 服務或 Azure Private Link，請使用虛擬網路插入來取得可用的共用 Azure 服務。 若要在虛擬網路插入或 Private Link 無法使用時從內部部署存取 Azure PaaS 服務，請使用 ExpressRoute 搭配 Microsoft 對等互連。 此方法可避免透過公用網際網路傳輸。

- 使用虛擬網路服務端點來保護從虛擬網路記憶體取 Azure PaaS 服務的安全，但只有當 Private Link 無法使用且沒有資料遭到外泄考慮時。 若要解決與服務端點相關的資料遭到外泄問題，請針對 Azure 儲存體使用 NVA 篩選或使用虛擬網路服務端點原則。

- 預設不會在所有子網上啟用虛擬網路服務端點。

- 如果有資料遭到外泄考慮，除非您使用 NVA 篩選，否則請勿使用虛擬網路服務端點。

- 我們不建議您執行強制通道，以啟用從 Azure 到 Azure 資源的通訊。

## <a name="plan-for-inbound-and-outbound-internet-connectivity"></a>規劃輸入和輸出網際網路連線能力

本節說明與公用網際網路之間的輸入和輸出連線所建議的連線性模型。

**設計考慮：**

- Azure-原生網路安全性服務（例如 Azure 防火牆、Azure Web 應用程式防火牆 (WAF) Azure 應用程式閘道）和 Azure Front Door 服務都是完全受控的服務。 因此，您不會產生與基礎結構部署相關聯的營運和管理成本，而這可能會變得複雜。

- 如果您的組織偏好使用 Nva 或原生服務無法滿足您組織的特定需求，則企業規模架構與合作夥伴 Nva 完全相容。

**設計建議：**

- 使用 Azure 防火牆來管理：

  - 流向網際網路的 Azure 輸出流量。

  - 非 HTTP/S 傳入連接。

  - 如果您的組織需要) 的東部/西部流量篩選 (。

- 使用具有虛擬 WAN 的防火牆管理員來部署和管理跨虛擬 WAN 中樞或中樞虛擬網路的 Azure 防火牆。 防火牆管理員現在已正式推出，適用于虛擬 WAN 和一般虛擬網路。

- 建立全域 Azure 防火牆原則，以控制全球網路環境的安全性狀態，並將其指派給所有的 Azure 防火牆實例。 透過以角色為基礎的存取控制，將累加式防火牆原則委派給本機安全性小組，以允許細微的原則符合特定區域的需求。

- 如果您的組織想要使用這類解決方案來協助保護輸出連線，請在防火牆管理員中設定支援的夥伴 SaaS 安全性提供者。

- 在登陸區域的虛擬網路內使用 WAF，以保護來自網際網路的輸入 HTTP/S 流量。

- 使用 Azure Front Door 服務和 WAF 原則，在 Azure 區域之間提供對登陸區域之輸入 HTTP/S 連線的全域保護。

- 當您使用 Azure Front Door 服務和 Azure 應用程式閘道來協助保護 HTTP/S 應用程式時，請使用 Azure Front Door 服務中的 WAF 原則。 鎖定 Azure 應用程式閘道，只接收來自 Azure Front Door 服務的流量。

- 如果東部/西部或南/北流量保護和篩選都需要合作夥伴 Nva：

  - 針對虛擬 WAN 網路拓撲，請將 Nva 部署至不同的虛擬網路 (例如 NVA 虛擬網路) 。 然後將它連線到區域虛擬 WAN 中樞以及需要存取 Nva 的登陸區域。 本文將說明[此](/azure/virtual-wan/virtual-wan-route-table-portal)流程。
  - 針對非虛擬 WAN 網路拓撲，請在中央中樞虛擬網路中部署合作夥伴 Nva。

- 如果輸入 HTTP/S 連線需要合作夥伴 Nva，請將它們部署在登陸區域的虛擬網路內，並與他們要保護並公開到網際網路的應用程式搭配使用。

- 使用 [Azure DDoS 保護標準保護方案](/azure/virtual-network/ddos-protection-overview) 來協助保護虛擬網路內裝載的所有公用端點。

- 請勿將內部部署周邊網路概念和架構複寫到 Azure。 您可以在 Azure 中使用類似的安全性功能，但必須將實體系和架構調整到雲端。

## <a name="plan-for-app-delivery"></a>規劃應用程式傳遞

本節將探討主要建議，以安全、可高度擴充且高可用性的方式，提供面向內部和外部應用程式。

**設計考慮：**

- Azure Load Balancer (內部和公用) 可針對區域層級的應用程式傳遞提供高可用性。

- Azure 應用程式閘道可讓您在區域層級進行 HTTP/S 應用程式的安全傳遞。

- Azure Front Door 服務允許在 Azure 區域之間提供高可用性 HTTP/S 應用程式的安全傳遞。

- Azure 流量管理員允許提供全球應用程式。

**設計建議：**

- 在登陸區域內執行應用程式傳遞，以進行面向內部和外部應用程式。

- 針對 HTTP/S 應用程式的安全傳遞，請使用應用程式閘道 v2，並確定已啟用 WAF 保護和原則。

- 如果您無法使用應用程式閘道 v2 來取得 HTTP/S 應用程式的安全性，請使用合作夥伴 NVA。

- 部署 Azure 應用程式閘道 v2 或合作夥伴 Nva，用於登陸區域虛擬網路內的輸入 HTTP/S 連線，以及其所保護的應用程式。

- 針對登陸區域中的所有公用 IP 位址，使用 DDoS 標準保護計劃。

- 使用 Azure Front Door Service 搭配 WAF 原則，來提供及協助保護橫跨 Azure 區域的全域 HTTP/S 應用程式。

- 當您使用 Azure Front Door 服務和應用程式閘道來協助保護 HTTP/S 應用程式時，請使用 Azure Front Door 服務中的 WAF 原則。 鎖定應用程式閘道，只接收來自 Azure Front Door 服務的流量。

- 使用流量管理員來提供跨越 HTTP/S 以外之通訊協定的全球應用程式。

## <a name="plan-for-landing-zone-network-segmentation"></a>規劃登陸區域網路分割

本節將探討在登陸區域內提供高度安全內部網路分割的主要建議，以推動網路的零信任實行。

**設計考慮：**

- 零信任模型會假設違反狀態，並驗證每個要求，就好像它源自于未受控制的網路一樣。

- 先進的零信任網路實施採用完全分散的輸入/輸出雲端微周邊和更深層的微型分割。

- 網路安全性群組可以使用 Azure 服務標籤來加速連線到 Azure PaaS 服務。

- 應用程式安全性群組不會跨越或提供跨虛擬網路的保護。

- Azure Resource Manager 範本現在支援 NSG 流量記錄。

**設計建議：**

- 將子網建立委派給登陸區域擁有者。 這可讓它們定義如何跨子網分割工作負載 (例如單一大型子網、多層式應用程式或網路插入的應用程式) 。 平臺小組可以使用 Azure 原則來確保具有特定規則的 NSG (例如拒絕來自網際網路的連入 SSH 或 RDP，或允許/封鎖跨登陸區域的流量) 一律與具有拒絕原則的子網相關聯。

- 使用 Nsg 協助保護跨子網的流量，以及跨平臺的東部/西部流量 (登陸區域間的流量) 。

- 應用程式小組應在子網層級 Nsg 使用應用程式安全性群組，以協助保護登陸區域內的多層式 Vm。

- 使用 Nsg 和應用程式安全性群組來微分割登陸區域內的流量，並避免使用中央 NVA 來篩選流量。

- 啟用 NSG 流量記錄，並將其饋送至流量分析，以深入瞭解內部和外部流量流程。

- 使用 Nsg 選擇性地允許登陸區域之間的連接。

- 針對虛擬 WAN 拓撲，如果您的組織需要在登陸區域間流動的流量進行篩選和記錄功能，請透過 Azure 防火牆將流量路由傳送到登陸區域。

## <a name="define-network-encryption-requirements"></a>定義網路加密需求

本節將探討可在內部部署與 Azure 之間，以及在 Azure 區域之間達成網路加密的重要建議。

**設計考慮：**

- 成本和可用頻寬與端點之間的加密通道長度成反比。

- 當您使用 VPN 連線至 Azure 時，流量會透過網際網路透過 IPsec 通道進行加密。

- 當您使用具有私人對等互連的 ExpressRoute 時，流量目前不會加密。

- 您可以將 [媒體存取控制安全性 (MACsec) ](/azure/expressroute/expressroute-howto-MACsec) 加密套用至 ExpressRoute Direct，以達成網路加密。

- Azure 目前不提供全域虛擬網路對等互連的原生加密。 如果您需要在 Azure 區域之間進行加密，您可以使用 VPN 閘道（而非全域虛擬網路對等互連）來連接虛擬網路。

**設計建議：**

![說明加密流程的圖表。](./media/enc-flows.png)

_圖8：加密流程。_

- 當您使用 VPN 閘道來建立從內部部署至 Azure 的 VPN 連線時，流量會透過 IPsec 通道以通訊協定層級進行加密。 上圖顯示此加密在 flow 中 `A` 。

- 當您使用 ExpressRoute Direct 時，請設定 [MACsec](/azure/expressroute/expressroute-howto-MACsec) ，以便在您組織的路由器與 MSEE 之間的兩層級加密流量。 此圖顯示此加密在 flow 中 `B` 。

- 針對不是 MACsec 選項 (的虛擬 WAN 案例，例如，請勿使用 ExpressRoute Direct) ，請使用虛擬 WAN VPN 閘道透過 ExpressRoute 私用對等互連來建立 IPsec 通道。 此圖顯示此加密在 flow 中 `C` 。

- 針對非虛擬 WAN 案例，而且 MACsec 不是選項 (例如，不使用 ExpressRoute Direct) ，唯一的選項如下：
  
  - 使用 partner Nva 透過 ExpressRoute 私用對等互連來建立 IPsec 通道。
  - 透過搭配 Microsoft 對等互連的 ExpressRoute 建立 VPN 通道。

- 如果必須加密 Azure 區域之間的流量，請使用 VPN 閘道來跨區域連接虛擬網路。

- 如果原生 Azure 解決方案 (如流程所示 `B` ，而 `C` 在圖表中) 不符合您的需求，請使用 Azure 中的合作夥伴 nva 來加密透過 ExpressRoute 私人對等互連的流量。

## <a name="plan-for-traffic-inspection"></a>規劃流量檢查

在許多產業中，組織要求將 Azure 中的流量鏡像至網路封包收集器，以進行深度檢查和分析。 這項需求通常著重于輸入和輸出網際網路流量。 本節探討在 Azure 虛擬網路內鏡像或點擊流量的重要考慮和建議方法。

**設計考慮：**

<!-- docsTest:ignore TAP -->

- [Azure 虛擬網路終端機存取點 (點) ](/azure/virtual-network/virtual-network-tap-overview) 目前為預覽狀態。 連絡人 `azurevnettap@microsoft.com` 以取得可用性詳細資料。

- Azure 網路監看員中的封包捕獲已正式運作，但會限制為五小時的最長期間。

**設計建議：**

您可以使用 Azure 虛擬網路的替代方法來評估下列選項：

- 您可以使用網路監看員封包來捕獲有限的捕獲視窗。

- 評估最新版本的 NSG 流量記錄是否提供您所需的詳細資料層級。

- 針對需要深入封包檢查的案例，請使用合作夥伴解決方案。

- 請勿開發自訂解決方案來鏡像流量。 雖然小規模案例可能可接受這種方法，但由於複雜性和可能發生的可支援性問題，我們不鼓勵大規模進行。
