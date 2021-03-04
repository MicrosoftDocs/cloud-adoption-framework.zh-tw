---
title: 虛擬 WAN 網路拓撲
description: 檢查有關 Microsoft Azure 中虛擬廣域網路的重要設計考慮和建議。
author: JefferyMitchell
ms.author: brblanch
ms.date: 02/18/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: think-tank
ms.openlocfilehash: 44d8ba71ced1ad925cec5df17ef17e0862233857
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101795004"
---
<!-- docutune:casing "Local, Standard, or Premium SKU" "ExpressRoute Standard or Premium circuits"-->

# <a name="virtual-wan-network-topology-microsoft-managed"></a>虛擬 WAN 網路拓撲 (Microsoft 管理的) 

探索 Microsoft Azure 中虛擬 WAN)  (虛擬廣域網路的主要設計考慮和建議。

![說明虛擬 WAN 網路拓撲的圖。](./media/virtual-wan-topology.png)

*圖1：虛擬 WAN 網路拓撲。*

**設計考慮：**

- [Azure 虛擬 WAN](/azure/virtual-wan/virtual-wan-about) 是一種受 Microsoft 管理的解決方案，預設會提供端對端、全域和動態傳輸的連線能力。 虛擬 WAN 中樞可免除手動設定網路連線的必要。 例如，您不需要管理 (UDR) 或網路虛擬裝置 (Nva) 的使用者定義路由，以啟用全域傳輸連線。

- 虛擬 WAN 藉由建立 [中樞和輪輻網路架構](/azure/virtual-wan/virtual-wan-global-transit-network-architecture)，在 azure 中簡化端對端網路連線，以及從內部部署環境到 Azure。 您可以輕鬆地調整架構，以支援多個 Azure 區域和內部部署位置 (任何連線) ，如下圖所示：

  ![此圖說明使用虛擬 WAN 的全球傳輸網路。](./media/global-transit.png)

 *圖2：使用虛擬 WAN 的全球傳輸網路。*

- 虛擬 WAN 任意對任意可轉移的連線能力支援在相同區域內和跨區域)  (下列路徑：

  - 虛擬網路對虛擬網路
  - 虛擬網路到分支
  - 分支至虛擬網路
  - 分支至分支

- 虛擬 WAN 中樞受限於 Microsoft 受控資源的部署。 您唯一可以在其中部署的資源包括虛擬網路閘道 (點對站 VPN、站對站 VPN、Azure ExpressRoute) 、透過防火牆管理員的 Azure 防火牆、路由表和 [某些網路虛擬裝置 (NVA) ](/azure/virtual-wan/about-nva-hub) 適用于廠商專屬的 SD WAN 功能。

- 虛擬 WAN 系結至某些 [虛擬 wan 的 Azure 訂](/azure/azure-resource-manager/management/azure-subscription-service-limits#virtual-wan-limits)用帳戶限制。

- 透過中樞對中樞) ，區域內和跨區域的網路對網路可轉移連線 (現已正式推出 (GA) 。

- 由於每個虛擬中樞都有 Microsoft 管理的路由功能，因此標準虛擬 WAN 中的虛擬網路之間的傳輸連線已啟用。 針對 VNet 對 VNet 流量，每個中樞最多可支援高達 50 Gbps 的匯總輸送量。

- 單一 Azure 虛擬 WAN 中樞可以跨所有直接連接的 Vnet 支援特定的 VM 工作負載數目上限，其記載于 [Azure 虛擬 WAN 限制](/azure/azure-resource-manager/management/azure-subscription-service-limits#virtual-wan-limits)中。

- 虛擬 WAN 與各種 [SD wan 提供者](/azure/virtual-wan/virtual-wan-locations-partners)整合。

- 許多受控服務提供者為虛擬 WAN 提供 [受控服務](/azure/networking/networking-partners-msp) 。

- 虛擬 WAN 中的使用者 VPN (點對站) 閘道可擴大為每個虛擬中樞最多 20 Gbps 的匯總輸送量和10000用戶端連線。

- 虛擬 WAN 中的站對站 VPN 閘道最多可擴大至 20 Gbps 的匯總輸送量。

- 使用本機、標準或 Premium SKU 的 ExpressRoute 線路可連接至虛擬 WAN 中樞。

- Expressroute Global 觸及所支援位置中的 ExpressRoute Standard 或 Premium 線路可以連線到虛擬 WAN ExpressRoute 閘道，並享有 (VPN 到 VPN、VPN 和 ExpressRoute 傳輸) 的所有虛擬 WAN 傳輸功能。 在全球存取範圍中不支援的 ExpressRoute Standard 或 Premium 線路可連接至 Azure 資源，但無法使用虛擬 WAN 傳輸功能。

- 如果連線到虛擬 WAN 中樞的輪輻 Vnet 與虛擬 WAN 中樞位於相同的區域中，則 Azure 虛擬 WAN 中樞支援 ExpressRoute Local。

- Azure 防火牆管理員現已正式推出，可讓您在虛擬 WAN 中樞中部署 Azure 防火牆。 請參閱 [Azure 防火牆管理員](/azure/firewall-manager/overview) 的安全虛擬中樞總覽和最新的 [限制](/azure/firewall-manager/overview#known-issues)。

- 當 Azure 防火牆部署在虛擬 WAN 中樞內 (安全的虛擬中樞) 時，目前不支援透過 Azure 防火牆的虛擬 WAN 中樞對中樞流量。 有幾項因應措施存在，視您的需求而定，包括將 [Azure 防火牆放在輪輻虛擬網路中](/azure/virtual-wan/scenario-route-through-nva)，或使用 nsg 進行流量篩選。

**設計建議：**

- 針對在 Azure 中需要跨 Azure 區域與內部部署位置進行全域傳輸連線的新大型或全域網路部署，建議使用虛擬 WAN。 如此一來，您就不需要手動設定 Azure 網路的可轉移路由。

  下圖顯示在歐洲和美國分佈的資料中心的範例全球企業部署。 這兩個區域中的部署也有許多分公司。 環境透過 Azure 虛擬 WAN 和 [ExpressRoute 全球接觸](/azure/expressroute/expressroute-global-reach)來全球連線。

  ![範例網路拓撲的圖表。](./media/global-reach-topology.png)

 *圖3：網路拓撲範例。*

- 使用每個 Azure 區域的虛擬 WAN 中樞，透過常見的全球 Azure 虛擬 WAN，將多個登陸區域一起連接到整個 Azure 區域。

- 使用 [虛擬中樞路由](/azure/virtual-wan/about-virtual-hub-routing) 功能進一步區隔 vnet 與分支之間的流量。

- 使用 ExpressRoute 將虛擬 WAN 中樞連線至內部部署資料中心。

- 在專用輪輻虛擬網路中部署必要的共用服務，例如 DNS 伺服器。 客戶部署的共用資源無法部署在虛擬 WAN 中樞本身內。

- 透過站對站 VPN 將分支和遠端位置連接到最接近的虛擬 WAN 中樞，或透過 SD WAN 合作夥伴解決方案來啟用虛擬 WAN 的分支連線能力。

- 透過點對站 VPN 將使用者連線到虛擬 WAN 中樞。

- 遵循「azure 中的流量會保留在 Azure 中」的原則，以便在 Azure 中跨資源的通訊會透過 Microsoft 骨幹網路進行，即使資源位於不同的區域。

- 針對網際網路輸出保護和篩選，請考慮在虛擬中樞部署 Azure 防火牆。

- 如果「東部/西部」或「南/北」流量保護和篩選需要合作夥伴 Nva，因為 Azure 虛擬 WAN 不允許在虛擬中樞中部署這類安全性 Nva，請評估是否將這些 Nva 部署到個別的輪輻虛擬網路，並依照 [此處](/azure/virtual-wan/scenario-route-through-nva) 所述設定靜態路由，以符合您的需求。 或者，請考慮以中樞和輪輻模型為基礎的傳統網路拓撲，因為合作夥伴 Nva 可以部署在一般的中樞虛擬網路中。

- 當您部署合作夥伴網路技術和 Nva 時，請遵循合作夥伴廠商的指導方針，以確保 Azure 網路功能沒有任何衝突的設定。

- 針對您從不是以虛擬 WAN 為基礎的中樞和輪輻網路拓撲進行遷移的棕色地帶案例，請參閱 [遷移至 Azure 虛擬 wan](/azure/virtual-wan/migrate-from-hub-spoke-topology)。

- 在連接訂用帳戶內建立 Azure 虛擬 WAN 和 Azure 防火牆資源。

- 請勿為每個虛擬 WAN 虛擬中樞建立500個以上的虛擬網路連線。

- 請仔細規劃您的部署，並確定您的網路架構是在 [Azure 虛擬 WAN 限制](/azure/azure-resource-manager/management/azure-subscription-service-limits#virtual-wan-limits)內。

- 使用 [Azure 監視器中的深入解析虛擬 wan (預覽版) ](/azure/virtual-wan/azure-monitor-insights) 來監視虛擬 wan 的端對端拓撲，以及狀態和 [關鍵計量](/azure/virtual-wan/azure-monitor-insights#detailed)。
