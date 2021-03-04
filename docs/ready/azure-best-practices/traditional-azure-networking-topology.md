---
title: 傳統的 Azure 網路拓撲
description: 查看 Azure 中關於網路拓撲的重要設計考慮和建議。
author: JefferyMitchell
ms.author: brblanch
ms.date: 02/18/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: think-tank
ms.openlocfilehash: 22e375a21556621c76b9ce20b5716d58d4523bde
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794964"
---
<!--docutune:casing "Layer-7 inbound" -->

# <a name="traditional-azure-networking-topology"></a>傳統的 Azure 網路拓撲

探索 Microsoft Azure 中關於網路拓撲的主要設計考慮和建議。

![說明傳統 Azure 網路拓撲的圖表。](./media/customer-managed-topology.png)

*圖1：傳統的 Azure 網路拓撲。*

**設計考慮：**

- 各種網路拓撲都可以連接多個登陸區域虛擬網路。 範例是一個大型的一般虛擬網路、多個連接多個 ExpressRoute 線路的虛擬網路、中樞和輪輻、全網格和混合式。

- 虛擬網路無法跨越訂用帳戶界限。 不過，您可以使用虛擬網路對等互連、ExpressRoute 線路或 VPN 閘道，在不同訂用帳戶之間達到虛擬網路之間的連線能力。

- 虛擬網路對等互連是在 Azure 中連接虛擬網路的慣用方法。 您可以使用虛擬網路對等互連，將相同區域中的虛擬網路、跨不同的 Azure 區域，以及跨不同 Azure Active Directory 的虛擬網路， (Azure AD) 租使用者。

- 虛擬網路對等互連和全域虛擬網路對等互連不是可轉移的。 使用者定義的路由 (Udr) 和網路虛擬裝置 (必須有 Nva) ，才能啟用傳輸網路。 如需詳細資訊，請參閱 [Azure 中的中樞輪輻網路拓撲](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)。

- 您可以使用 ExpressRoute 線路，在相同地緣政治區域內的虛擬網路之間建立連線，或使用 premium 附加元件在地緣政治區域間進行連線。 請記住下列重點：

  - 網路對網路流量可能會遇到更多延遲，因為流量必須在 Microsoft Enterprise Edge (MSEE) 路由器上 hairpin。

  - 頻寬會受限於 ExpressRoute 閘道 SKU。

  - 如果您需要檢查或記錄跨虛擬網路的流量，您仍然必須部署及管理 Udr。

- 具有邊界閘道協定 (BGP) 的 VPN 閘道在 Azure 和內部部署內是可轉移的，但它們並不會提供透過 ExpressRoute 連線之網路的可轉移存取權。

- 當有多個 ExpressRoute 線路連線到相同的虛擬網路時，請使用連線權數和 BGP 技術，以確保內部部署與 Azure 之間流量的最佳路徑。 如需詳細資訊，請參閱 [優化 ExpressRoute 路由](/azure/expressroute/expressroute-optimize-routing)。

- 使用 BGP 計量來影響 ExpressRoute 路由是在 Azure 平臺外部所做的設定變更。 您的組織或連接提供者必須據以設定內部部署路由器。

- ExpressRoute 線路與 premium 附加元件提供全球連線能力。

- ExpressRoute 系結至特定的限制，例如每個 ExpressRoute 閘道的 ExpressRoute 連線數目上限，或可透過 ExpressRoute 私用對等互連從 Azure 公告至內部部署的最大路由數目。 這類限制記載于 [ExpressRoute 限制](/azure/azure-resource-manager/management/azure-subscription-service-limits#expressroute-limits) 文章中。

- VPN 閘道的匯總輸送量上限為 10 Gbps。 它支援最多30個站對站或網路對網路通道。

**設計建議：**

- 針對下列案例，請考慮以傳統中樞和輪輻網路拓撲為基礎的網路設計：

  - 在單一 Azure 區域內部署的網路架構。

  - 跨多個 Azure 區域的網路架構，不需要跨區域之登陸區域的虛擬網路之間進行可轉移的連線。

  - 跨多個 Azure 區域的網路架構，以及全域 VNet 對等互連可用於跨 Azure 區域連接虛擬網路。

  - VPN 和 ExpressRoute 連線之間不需要可轉移的連線能力。

  - 主要的混合式連線方法是 ExpressRoute，每個 VPN 閘道的 VPN 連線數目小於30。

  - 集中式 Nva 和細微路由的相依性。

- 針對區域部署，主要是使用中樞與輪輻拓撲。 使用與虛擬網路對等互連連線到中央中樞虛擬網路的登陸區域虛擬網路，以透過 ExpressRoute 進行跨單位連線、適用于分支連線的 VPN、透過 Nva 和 Udr 的輪輻對輪輻連線，以及透過 Azure 防火牆或其他協力廠商 NVA 的網際網路輸出保護。 下圖顯示此拓撲。 這可讓適當的流量控制符合大部分的分割和檢查需求。

  ![說明中樞和輪輻網路拓撲的圖。](./media/hub-and-spoke-topology.png)

 *圖2：中樞和輪輻網路拓撲。*

- 當下列其中一個條件成立時，請使用與多個 ExpressRoute 線路連線之多個虛擬網路的拓撲：

  - 您需要高層級的隔離。

  - 您需要特定業務單位的專用 ExpressRoute 頻寬。

  - 您已達到每個 ExpressRoute 閘道的連線數目上限 (請參閱) 最大數目的 [expressroute 限制](/azure/azure-resource-manager/management/azure-subscription-service-limits#expressroute-limits) 文章。

下圖顯示此拓撲。

  ![說明多個 ExpressRoute 線路連線之多個虛擬網路的圖表。](./media/vnet-multiple-circuits.png)

 *圖3：多個虛擬網路與多個 ExpressRoute 線路連線。*

- 部署一組最基本的共用服務，包括 ExpressRoute 閘道、VPN 閘道 (視需要) ，以及在中央中樞虛擬網路中) 所需的 Azure 防火牆或合作夥伴 Nva (。 如有必要，也請部署 Active Directory 網域控制站和 DNS 伺服器。

- 在中央中樞虛擬網路中，部署適用于東部/西部的 Azure 防火牆或合作夥伴 Nva，或南/北流量保護和篩選。

- 當您部署合作夥伴網路技術或 Nva 時，請遵循合作夥伴廠商的指導方針，以確保：

  - 廠商支援部署。

  - 本指南是針對高可用性和最大效能所設計。

  - Azure 網路功能沒有任何衝突的設定。

- 請勿將第7層輸入 Nva （例如 Azure 應用程式閘道）部署為中央中樞虛擬網路中的共用服務。 相反地，請將其與應用程式一起部署在其各自的登陸區域中。

- 使用您現有的網路（MPLS 和 SD WAN）來與公司總部連接分公司位置。 不支援 ExpressRoute 與 VPN 閘道之間的 Azure 傳輸。

- 針對跨 Azure 區域具有多個中樞和輪輻拓撲的網路架構，當少數登陸區域需要跨區域進行通訊時，請使用全域虛擬網路對等互連來連接登陸區域虛擬網路。 這種方法可提供 VM SKU 所允許的像是具有全域虛擬網路對等互連的高網路頻寬等優點。 不過，它會略過中央 NVA，以防需要進行流量檢查或篩選。 這也會受到 [全域虛擬網路對等互連的限制](/azure/virtual-network/virtual-network-peering-overview#constraints-for-peered-virtual-networks)。

- 當您在兩個 Azure 區域中部署中樞和輪輻網路架構，且需要跨區域的所有登陸區域之間的傳輸連線時，請使用 ExpressRoute 搭配雙重線路，以提供跨 Azure 區域之登陸區域虛擬網路的傳輸連線能力。 在此案例中，登陸區域可以透過本機中樞虛擬網路中的 NVA，以及透過 ExpressRoute 線路跨區域在區域內傳輸。 流量必須在 MSEE 路由器上 hairpin。 下圖顯示此設計。

  ![說明登陸區域連線能力設計的圖表。](./media/vnet-dual-circuits.png)

*圖4：登陸區域連接設計。*

- 當您的組織需要跨兩個 Azure 區域的中樞和輪輻網路架構，以及登陸區域之間的全球傳輸連線能力時，需要跨 Azure 區域的虛擬網路。 您可以藉由將中央中樞虛擬網路與全域虛擬網路對等互連進行虛擬化，並使用 Udr 和 Nva 來啟用全域傳輸路由，來執行此架構。 因為複雜性和管理的額外負荷很高，所以建議使用虛擬 WAN 來評估全域傳輸網路架構。

- 使用 [適用于網路的 Azure 監視器 (預覽) ](/azure/azure-monitor/insights/network-insights-overview) 來監視 Azure 上網路的端對端狀態。

- 將輪輻虛擬網路連接到中央中樞虛擬網路時，必須考慮兩個 [限制](/azure/azure-resource-manager/management/azure-subscription-service-limits) ：

  - 每個虛擬網路的虛擬網路對等互連連線數目上限。
  - 透過具有私人對等互連的 ExpressRoute，從 Azure 向內部部署公告的首碼數目上限。

  確定連線至中樞虛擬網路的輪輻虛擬網路數目未超過任何一項限制。
