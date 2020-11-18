---
title: 針對遷移至 Azure 的工作負載來設定網路的最佳做法
description: 使用「適用于 Azure 的雲端採用架構」來瞭解為您已遷移的工作負載設定網路的最佳做法。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: fd02f8755ebd44f7f3e3444772c76bc4357c7071
ms.sourcegitcommit: a7eb2f6c4465527cca2d479edbfc9d93d1e44bf1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94712879"
---
<!-- cSpell:ignore NSGs CIDR FQDNs BGP's ACLs WAFs -->

# <a name="best-practices-to-set-up-networking-for-workloads-migrated-to-azure"></a>針對遷移至 Azure 的工作負載來設定網路的最佳做法

在規劃和設計移轉時，除了移轉作業本身外，其中一個最重要的步驟便是設計和實作 Azure 網路。 本文說明當您在 Azure 中遷移至基礎結構即服務 (IaaS) 和平臺即服務 (PaaS) 執行時，適用于網路的最佳做法。

> [!IMPORTANT]
> 本文所述的最佳做法和意見是以本文撰寫當下可用的 Azure 平台和服務功能作為基礎。 特色與功能會隨著時間改變。 這些建議不一定全都適用於您的部署，因此請選取適合您部署的建議。

## <a name="design-virtual-networks"></a>設計虛擬網路

Azure 為虛擬網路提供下列功能：

- Azure 資源會透過虛擬網路，以私人、直接且安全的方式彼此通訊。
- 您可以針對需要網際網路通訊的 Vm 和服務，在虛擬網路上設定端點連線。
- 虛擬網路是專屬於您訂用帳戶的 Azure 雲端邏輯隔離。
- 您可以在每個 Azure 訂用帳戶和 Azure 區域內實作多個虛擬網路。
- 每個虛擬網路都與其他虛擬網路隔離。
- 虛擬網路可以包含 [RFC 1918](https://tools.ietf.org/html/rfc1918)中定義的私人和公用 IP 位址，以 CIDR 標記法表示。 虛擬網路位址空間中指定的公用 IP 位址無法直接從網際網路存取。
- 虛擬網路可以使用虛擬網路對等互連彼此連接。 已連線的虛擬網路可以位於相同或不同的區域。 因此，一個虛擬網路中的資源可以連接至其他虛擬網路中的資源。
- 根據預設，Azure 會在虛擬網路內的子網、已連線的虛擬網路、內部部署網路和網際網路之間路由傳送流量。

規劃您的虛擬網路拓撲時，您應該考慮如何安排 IP 位址空間、如何實行中樞和輪輻網路、如何將虛擬網路分割成子網、設定 DNS，以及執行 Azure 可用性區域。

## <a name="best-practice-plan-ip-addressing"></a>最佳做法：規劃 IP 定址

當您在遷移過程中建立虛擬網路時，請務必規劃您的虛擬網路 IP 位址空間。

針對每個虛擬網路，您應該指派的位址空間不會大於的 CIDR 範圍 `/16` 。 虛擬網路可讓您使用 65536 IP 位址。 指派小於的前置詞 `/16` ，例如 `/15` 具有131072位址的，將會導致額外的 IP 位址無法在別處變成無法使用。 務必不要浪費 IP 位址，即使其位於 RFC 1918 所定義的私人範圍內也是如此。

規劃的其他秘訣如下：

- 虛擬網路位址空間不應與內部部署網路範圍重迭。
- 重迭的位址可能會導致無法連線的網路，以及無法正常運作的路由。
- 如果網路重迭，您將需要重新設計網路。
- 如果您絕對無法重新設計網路， (NAT) 的網路位址轉譯可提供協助。 但應該盡可能避免或限制。

**瞭解更多資訊：**

- 閱讀 [Azure 虛擬網路總覽](/azure/virtual-network/virtual-networks-overview)。
- 請參閱 [Azure 虛擬網路常見問題](/azure/virtual-network/virtual-networks-faq)。
- 瞭解 [Azure 網路限制](/azure/azure-resource-manager/management/azure-subscription-service-limits)。

## <a name="best-practice-implement-a-hub-and-spoke-network-topology"></a>最佳做法：實行中樞和輪輻網路拓撲

中樞和輪輻網路拓撲會在共用服務（例如身分識別和安全性）時隔離工作負載。 中樞是 Azure 虛擬網路，可作為連線的中心點。 輪輻是使用對等互連連線至中樞虛擬網路的虛擬網路。 共用的服務會部署在中樞中，而個別的工作負載會部署為輪輻。

請考慮下列事項：

- 在 Azure 中執行中樞和輪輻拓撲會集中一般服務，例如連線至內部部署網路、防火牆，以及虛擬網路之間的隔離。 中樞虛擬網路提供與內部部署網路的連線中心點，以及裝載在輪輻虛擬網路中裝載之工作負載所使用之服務的位置。
- 大型企業通常會使用中樞與輪輻設定。 若網路較小，則可能要考慮簡化設計，以節省成本和降低複雜度。
- 您可以使用輪輻虛擬網路來隔離工作負載，並將每個輪輻與其他輪輻分開管理。 每個工作負載都會有多個層級，以及透過 Azure 負載平衡器連線的多個子網路。
- 您可以在不同的資源群組中，甚至在不同的訂用帳戶中，執行中樞和輪輻虛擬網路。 當您將不同訂用帳戶中的虛擬網路對等互連時，這些訂用帳戶可以與相同或不同的 Azure Active Directory (Azure AD) 租用戶相關聯。 如此可對每個工作負載進行非集中式管理，同時在中樞網路中維護共用服務。

![中樞和輪輻拓撲 ](./media/migrate-best-practices-networking/hub-spoke.png)
 _圖1：中樞和輪輻拓撲。_

**瞭解更多資訊：**

- 閱讀 [中樞和輪輻拓撲](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)的相關資訊。
- 取得在 Azure 中執行 [Windows vm](/azure/architecture/reference-architectures/n-tier/windows-vm) 和 [Linux vm](/azure/architecture/reference-architectures/n-tier/linux-vm) 的網路建議。
- 深入瞭解 [虛擬網路對等互連](/azure/virtual-network/virtual-network-peering-overview)。

## <a name="best-practice-design-subnets"></a>最佳做法：設計子網

若要在虛擬網路內提供隔離，請將它分割成一或多個子網，並將虛擬網路位址空間的一部分配置給每個子網。

- 您可以在每個虛擬網路內建立多個子網路。
- 根據預設，Azure 會在虛擬網路中的所有子網之間路由傳送網路流量。
- 請根據技術和組織的需求來決定如何劃分子網路。
- 您可以使用 CIDR 標記法來建立子網。

當您決定子網的網路範圍時，請注意，Azure 會保留每個子網中無法使用的五個 IP 位址。 例如，如果您建立 (的最小可用子網 `/29`) 有八個 IP 位址，Azure 會保留五個位址。 在此情況下，您只會有三個可指派給子網主機的可用位址。 在大部分的情況下，請使用 `/28` 做為最小的子網。

**範例︰**

下表顯示虛擬網路的範例，其中的位址空間會 `10.245.16.0/20` 分割成子網，以進行計畫的遷移。

| 子網路 | CIDR | 位址 | 使用方式 |
| --- | --- | --- | --- |
| `DEV-FE-EUS2` | `10.245.16.0/22` | 1019 | 前端或 web 層 Vm |
| `DEV-APP-EUS2` | `10.245.20.0/22` | 1019 | 應用程式層 VM |
| `DEV-DB-EUS2` | `10.245.24.0/23` | 507 | 資料庫 VM |

**瞭解更多資訊：**

- 瞭解如何 [設計子網](/azure/virtual-network/virtual-network-vnet-plan-design-arm#segmentation)。
- 瞭解 Contoso （虛構公司）如何 [準備其網路基礎結構來進行遷移](/azure/migrate/contoso-migration-infrastructure)。

## <a name="best-practice-set-up-a-dns-server"></a>最佳做法：設定 DNS 伺服器

當您部署虛擬網路時，Azure 預設會新增 DNS 伺服器。 這可讓您快速建立虛擬網路和部署資源。 但是，此 DNS 伺服器只會提供服務給該虛擬網路上的資源。 如果您想要將多個虛擬網路連接在一起，或從虛擬網路連線到內部部署伺服器，您需要額外的名稱解析功能。 例如，您可能需要 Active Directory 才能在虛擬網路之間解析 DNS 名稱。 若要這樣做，請在 Azure 中部署您自己的自訂 DNS 伺服器。

- 虛擬網路中的 DNS 伺服器可以將 DNS 查詢轉送至 Azure 中的遞迴解析程式。 這可讓您解析該虛擬網路內的主機名稱。 例如，在 Azure 中執行的網域控制站可以回應其專屬網域的 DNS 查詢，並將所有其他查詢轉送到 Azure。
- DNS 轉送可讓 VM 查看您的內部部署資源 (透過網域控制站) 以及 Azure 提供的主機名稱 (使用轉送工具)。 您可以使用虛擬 IP 位址來存取 Azure 中的遞迴解析程式 `168.63.129.16` 。
- DNS 轉送也會啟用虛擬網路之間的 DNS 解析，並可讓內部部署電腦解析 Azure 提供的主機名稱。
  - 若要解析 VM 主機名稱，DNS 伺服器 VM 必須位於相同的虛擬網路中，並設定為將主機名稱查詢轉送到 Azure。
  - 因為每個虛擬網路的 DNS 尾碼都不同，所以您可以使用條件性轉送規則來將 DNS 查詢傳送到正確的虛擬網路進行解析。
- 當您使用自己的 DNS 伺服器時，可以為每個虛擬網路指定多個 DNS 伺服器。 您也可以對每個網路介面 (適用於 Azure Resource Manager) 或對每個雲端服務 (適用於傳統部署模型) 指定多個 DNS 伺服器。
- 針對網路介面或雲端服務所指定的 DNS 伺服器，優先順序高於針對虛擬網路指定的 DNS 伺服器。
- 在 Azure Resource Manager 中，您可以為虛擬網路和網路介面指定 DNS 伺服器，但最佳做法是只在虛擬網路上使用此設定。

    ![虛擬網路的 DNS 伺服器螢幕擷取畫面。](./media/migrate-best-practices-networking/dns2.png)
    _圖2：虛擬網路的 DNS 伺服器。_

**瞭解更多資訊：**

- 瞭解 [當您使用自己的 DNS 伺服器時的名稱解析](/azure/migrate/contoso-migration-infrastructure)。
- 瞭解 [DNS 命名規則和限制](../../ready/azure-best-practices/naming-and-tagging.md)。

## <a name="best-practice-set-up-availability-zones"></a>最佳做法：設定可用性區域

可用性區域增加高可用性，以保護您的應用程式和資料不受資料中心故障影響。 「可用性區域」是 Azure 地區內獨特的實體位置。 每個區域皆由一或多個配備獨立電力、冷卻系統及網路的資料中心所組成。 若要確保復原，所有已啟用的地區中至少要有三個不同的區域。 地區內「可用性區域」的實體區隔可保護應用程式和資料不受資料中心故障影響。

以下是您在設定可用性區域時要注意的一些其他幾點：

- 區域冗余服務會跨可用性區域複寫您的應用程式和資料，以防止單一失敗點。

- 使用可用性區域，Azure 可提供 99.99% VM 執行時間的 SLA。

    ![Azure 區域內可用性區域的圖表。](./media/migrate-best-practices-networking/availability-zone.png)

    _圖3：可用性區域。_

- 藉由將運算、儲存體、網路及資料資源共置於某個區域內並複寫至其他區域，您即可在移轉架構內規劃和建置高可用性。 支援「可用性區域」的 Azure 服務分成兩個類別：
  - **區域性服務：** 您可以將資源與特定區域（例如 Vm、受控磁片或 IP 位址）相關聯。
  - **區域冗余服務：** 資源會自動跨區域複寫，例如區域冗余儲存體或 Azure SQL Database。
- 若要提供區域性容錯功能，您可以使用網際網路對應的工作負載或應用層來部署標準的 Azure 負載平衡器。

    ![標準 Azure 負載平衡器 ](./media/migrate-best-practices-networking/load-balancer.png) _圖4：負載平衡器_ 的圖表。

**瞭解更多資訊：**

- 閱讀 [可用性區域總覽](/azure/availability-zones/az-overview)。

## <a name="design-hybrid-cloud-networking"></a>設計混合式雲端網路

若要成功移轉，請務必將內部部署公司網路連線至 Azure。 這會建立稱為混合式雲端網路的 Always-On 連線，服務會從 Azure 雲端提供給公司使用者。 有兩個選項可供建立此類型的網路：

- **站對站 VPN：** 您可以在相容的內部部署 VPN 裝置與虛擬網路中部署的 Azure VPN 閘道之間建立站對站連線。 任何授權的內部部署資源都可以存取虛擬網路。 站對站通訊會在網際網路間透過加密通道傳送。
- **Azure ExpressRoute：** 您可以透過 ExpressRoute 合作夥伴，建立內部部署網路與 Azure 之間的 Azure ExpressRoute 連線。 這是私人連線，所以流量不會經過網際網路。

**瞭解更多資訊：**

- 深入瞭解 [混合式雲端網路功能](/azure/architecture/reference-architectures/hybrid-networking/vpn)。

## <a name="best-practice-implement-a-highly-available-site-to-site-vpn"></a>最佳做法：執行高可用性的站對站 VPN

若要實作站對站 VPN，請在 Azure 中設定 VPN 閘道。

- VPN 閘道是一種特定類型的虛擬網路閘道。 它會透過公用網際網路在 Azure 虛擬網路和內部部署位置之間傳送加密流量。
- VPN 閘道也可以透過 Microsoft 網路，在 Azure 虛擬網路之間傳送加密的流量。
- 每個虛擬網路只能有一個 VPN 閘道。
- 您可以對相同的 VPN 閘道建立多個連線。 當您建立多個連線時，所有 VPN 通道都會共用可用的閘道頻寬。

每個 Azure VPN 閘道都是由使用中-待命設定中的兩個實例所組成：

- 針對作用中實例預定進行的維修或非計畫中斷，會進行容錯移轉，且待命實例會自動接管。 此實例會繼續進行站對站或網路對網路連線。
- 切換時會造成短暫中斷。
- 對於計劃性維護，應在 10 到 15 秒內還原連線。
- 針對未規劃的問題，連接復原需要較長的時間，最糟的情況下，最多可達1.5 分鐘。
- 與閘道的點對站 VPN 用戶端連線已中斷連線，而使用者必須從用戶端電腦重新連線。

設定站對站 VPN 時：

- 您需要一個虛擬網路，其位址範圍未與 VPN 將連接的內部部署網路重迭。
- 在網路中建立閘道子網路。
- 您可以建立 VPN 閘道、指定 (VPN) 的閘道類型，以及閘道是以原則為基礎或以路由為基礎。 以路由為基礎的 VPN 可視為更有能力和未來的證明。
- 建立內部部署的區域網路閘道，並設定內部部署 VPN 裝置。
- 您會在虛擬網路閘道與內部部署裝置之間建立容錯移轉站對站 VPN 連線。 使用路由式 VPN 可讓您對 Azure 建立「主動-被動」或「主動-主動」連線。 以路由為基礎的選項也支援從單一電腦的任何電腦) 和點對站 (的站對站 (，同時) 連接。
- 指定您想要使用的閘道 SKU。 這取決於您的工作負載需求、輸送量、功能和 Sla。
- 邊界閘道協定 (BGP) 是選擇性功能。 您可以將它與 Azure ExpressRoute 和路由型 VPN 閘道搭配使用，以將內部部署 BGP 路由傳播至虛擬網路。

![站對站 VPN 的圖表。 ](./media/migrate-best-practices-networking/vpn.png)
_圖5：站對站 VPN。_

**瞭解更多資訊：**

- 檢查 [相容的內部部署 VPN 裝置](/azure/vpn-gateway/vpn-gateway-about-vpn-devices)。
- 閱讀 [AZURE VPN 閘道總覽](/azure/vpn-gateway/vpn-gateway-about-vpngateways)。
- 深入瞭解 [高可用性 VPN 連接](/azure/vpn-gateway/vpn-gateway-highlyavailable)。
- 瞭解如何 [規劃和設計 VPN 閘道](/azure/vpn-gateway/vpn-gateway-plan-design)。
- 檢查 [VPN 閘道設定](/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings#gwsku)。
- 檢查 [閘道 sku](/azure/vpn-gateway/vpn-gateway-about-vpngateways#gwsku)。
- 瞭解如何 [使用 AZURE VPN 閘道設定 BGP](/azure/vpn-gateway/vpn-gateway-bgp-overview)。

### <a name="best-practice-configure-a-gateway-for-vpn-gateways"></a>最佳做法：設定 VPN 閘道的閘道

當您在 Azure 中建立 VPN 閘道時，您必須使用名為的特殊子網 `GatewaySubnet` 。 當您建立此子網時，請注意下列最佳作法：

- `GatewaySubnet` 最大前置長度可以是 29 (例如 `10.119.255.248/29`) 。 目前的建議是使用前置長度 27 (例如 `10.119.255.224/27`) 。
- 當您定義閘道子網的位址空間時，請使用虛擬網路位址空間的最後一個部分。
- 當您使用 Azure 閘道子網時，絕對不會將任何 Vm 或其他裝置（例如 Azure 應用程式閘道）部署到閘道子網。
- 請勿將網路安全性群組 (NSG) 指派到這個子網路。 這會導致閘道停止運作。

**瞭解更多資訊：**

- [使用此工具](https://gallery.technet.microsoft.com/scriptcenter/Address-prefix-calculator-a94b6eed)來判斷您的 IP 位址空間。

## <a name="best-practice-implement-azure-virtual-wan-for-branch-offices"></a>最佳做法：為分公司執行 Azure 虛擬 WAN

針對多個 VPN 連線，Azure 虛擬 WAN 是一種網路服務，透過 Azure 提供最佳且自動化的分支對分支連線能力。

- 虛擬 WAN 可讓您連線，並設定與 Azure 通訊的分支裝置。 您可以手動執行此動作，或透過 Azure 虛擬 WAN 合作夥伴使用慣用的提供者裝置。
- 使用慣用的提供者裝置可讓您輕鬆地進行使用、連線和管理組態。
- Azure 虛擬 WAN 內建儀表板提供即時的疑難排解深入解析，可節省時間，並提供簡單的方式來追蹤大規模的站對站連線能力。

**深入瞭解：** 深入瞭解 [Azure 虛擬 WAN](/azure/virtual-wan/virtual-wan-about)。

### <a name="best-practice-implement-expressroute-for-mission-critical-connections"></a>最佳做法：針對任務關鍵性連接執行 ExpressRoute

Azure ExpressRoute 服務會在虛擬 Azure 資料中心與內部部署網路之間建立私人連線，以將您的內部部署基礎結構延伸至 Microsoft 雲端。 以下是一些執行詳細資料：

- ExpressRoute 連線可透過任意對任意 (IP VPN) 網路、點對點乙太網路，也可以透過連線提供者來進行。 但不會經過公用網際網路。
- ExpressRoute 連線可提供更高的安全性、可靠性、速度 (最高 10 Gbps)，以及一致的延遲。
- ExpressRoute 很適合虛擬資料中心，因為客戶可以獲得與私人連線建立關聯之相容性規則的優點。
- 有了 ExpressRoute Direct，您就可以在 100 Gbps 直接連線到 Microsoft 路由器，以達到較大的頻寬需求。
- ExpressRoute 會使用 BGP 在內部部署網路、Azure 執行個體與 Microsoft 公用位址之間交換路由。

部署 ExpressRoute 連線通常包含加入 ExpressRoute 服務提供者。 若要快速啟動，一開始通常會使用站對站 VPN 來建立虛擬資料中心與內部部署資源之間的連線能力。 然後您會在與服務提供者的實體互相連線建立時遷移至 ExpressRoute 連線。

**瞭解更多資訊：**

- 閱讀 ExpressRoute 的 [總覽](/azure/expressroute/expressroute-introduction) 。
- 深入瞭解 [ExpressRoute Direct](/azure/expressroute/expressroute-erdirect-about)。

### <a name="best-practice-optimize-expressroute-routing-with-bgp-communities"></a>最佳做法：使用 BGP 群體優化 ExpressRoute 路由

當您有多個 ExpressRoute 線路時，會有一個以上的路徑來連線到 Microsoft。 因此，可能會產生次佳的路由，而且您的流量可能會經由較長的路徑連到 Microsoft，而 Microsoft 也可能會經由較長的路徑連到您的網路。 網路路徑愈常，延遲愈久。 延遲會直接影響應用程式效能和使用者體驗。

**範例︰**

讓我們檢閱一個範例：

- 您在美國有兩個辦公室，一個在洛杉磯，一個在紐約城市。
- 您的辦公室是在 WAN 上連線，該網路可以是您自己的骨幹網路或服務提供者的 IP VPN。
- 您有兩個 ExpressRoute 線路，一個在中，另 `West US` 一個在 `East US` WAN 上連線。 很明顯地，您有兩個路徑可連線到 Microsoft 網路。

**問題：**

現在假設您有 Azure 部署 (例如，在和中 Azure App Service) `West US` `East US` 。

- 您希望每個辦公室的使用者存取與其最接近的 Azure 服務，以獲得最佳體驗。
- 因此，您想要將洛杉磯的使用者連線到 Azure `West US` ，並將紐約的使用者連線到 azure `East US` 。
- 這適用于「東亞」使用者，但不適用於 west coast 上的使用者。 問題在於：
  - 在每個 ExpressRoute 線路上，我們會在 Azure 中宣告這兩個首碼： `East US` (`23.100.0.0/16`) 和 azure `West US` (`13.100.0.0/16`) 。
  - 如果不知道前置詞來自哪個區域，就無法以不同方式處理前置詞。
  - 您的 WAN 網路可以假設這兩個前置詞都較接近 `East US` `West US` ，因此會將來自兩個辦公室的使用者路由傳送至中的 ExpressRoute 電路 `East US` 。 這對洛杉磯辦公室的使用者提供更糟的體驗。

![具有路徑路徑錯誤電路的 VPN 圖表。 ](./media/migrate-best-practices-networking/bgp1.png)
_圖6： BGP 群體未優化連接。_

**解決方案：**

若要將兩個辦公室的路由優化，您需要知道哪個前置詞來自 Azure `West US` ，哪些是來自 azure `East US` 。 您可以使用 BGP 社群值來編碼這項資訊。

- 您會將唯一的 BGP 社群值指派給每個 Azure 區域。 例如，12076:51004 代表的 `East US` 12076:51006 `West US` 。
- 您現在已清楚哪個前置詞屬於哪個 Azure 區域，因此可以設定偏好的 ExpressRoute 線路。
- 因為您要使用 BGP 來交換路由資訊，您可以使用 BGP 的本機喜好設定來影響路由。
- 在我們的範例中，您會將較高的本機喜好設定值指派到 `13.100.0.0/16` 中的 `West US` `East US` 。 同樣地，您可以將較高的本機喜好設定值指派給中的， `23.100.0.0/16` `East US` 而不是 `West US` 。
- 這項設定可確保當 Microsoft 的兩個路徑都可用時，洛杉磯的使用者會使用「西部」電路連接到該 `West US` 區域，而紐約的使用者會使用「東部線路」連接到該 `East US` 區域。

![透過正確電路路由路徑的 VPN 圖表。 ](./media/migrate-best-practices-networking/bgp2.png)
_圖7： BGP 團體優化連接。_

**瞭解更多資訊：**

- 瞭解如何將 [路由優化](/azure/expressroute/expressroute-optimize-routing)。

## <a name="secure-virtual-networks"></a>保護虛擬網路

Microsoft 與您之間可以共用保護虛擬網路的責任。 Microsoft 提供了許多網路功能以及服務，可協助保護資源的安全。 當您設計虛擬網路的安全性時，最好是採用周邊網路、使用篩選和安全性群組、安全地存取資源和 IP 位址，以及實行攻擊防護。

**瞭解更多資訊：**

- 閱讀 [網路安全性最佳作法的總覽](/azure/security/fundamentals/network-best-practices)。
- 瞭解如何為 [安全網路設計](/azure/virtual-network/virtual-network-vnet-plan-design-arm#security)。

<!-- docutune:casing "IDS/IPS" -->

## <a name="best-practice-implement-an-azure-perimeter-network"></a>最佳做法：執行 Azure 周邊網路

雖然 Microsoft 會大量投資在保護雲端基礎結構，但您也必須保護您的雲端服務和資源群組。 安全性的多層式方法提供最佳的防護。 備有周邊網路是該防禦策略中很重要的一部分。

- 周邊網路可防止不受信任的網路存取內部網路資源。
- 它是對網際網路公開的最外層。 它通常位於網際網路和企業基礎結構之間，且兩邊通常都會有某種形式的保護。
- 在典型的企業網路拓撲中，核心基礎結構的周邊有多層的安全性裝置，嚴加防禦。 每一層的界限都由裝置和原則強制執行點組成。
- 每一層都可包含網路安全性解決方案的組合，例如防火牆、阻絕服務 (DoS) 預防、入侵偵測/入侵保護系統 (IDS/IPS) 和 VPN 裝置。
- 若要在周邊網路強制執行原則，您可以使用防火牆原則、存取控制清單 (ACL) 或特定的路由。
- 當連入流量從網際網路抵達時，它會被防禦解決方案的組合攔截並處理。 解決方案會封鎖攻擊和有害的流量，同時允許合法的要求進入網路。
- 連入流量可直接路由至周邊網路的資源中。 接著，周邊網路資源可以與更深層網路中的其他資源通訊，並於驗證之後將流量轉送到網路中。

以下是在公司網路中具有兩個安全性界限的單一子網周邊網路範例。

![Azure 虛擬網路周邊網路部署的圖表。 ](./media/migrate-best-practices-networking/perimeter.png)
_圖8：周邊網路部署。_

**瞭解更多資訊：**

- 瞭解如何 [在 Azure 與您的內部部署資料中心之間部署周邊網路](/azure/architecture/reference-architectures/dmz/secure-vnet-dmz)。

## <a name="best-practice-filter-virtual-network-traffic-with-nsgs"></a>最佳做法：使用 Nsg 篩選虛擬網路流量

網路安全性群組 (Nsg) 包含多個輸入和輸出安全性規則，可篩選進出資源的流量。 您可以依據來源和目的地 IP 位址、連接埠及通訊協定來進行篩選。

- NSG 包含安全性規則，可允許或拒絕進出於多種 Azure 資源類型的輸入和輸出網路流量。 您可以為每個規則指定來源和目的地、連接埠及通訊協定。
- NSG 規則會使用五元組資訊（ (來源、來源埠、目的地、目的地埠和通訊協定) ）來評估，以允許或拒絕流量。
- 系統會為現有連線建立流程記錄。 允許或拒絕通訊都會以此流程記錄的連線狀態為依據。
- 流程記錄允許具狀態 NSG。 例如，如果您將輸出安全性規則指定至任何透過連接埠 80 的位址，則不需要有輸入安全性規則來回應輸出流量。 如果在外部起始通訊，您只需要指定輸入安全性規則。
- 反之亦然。 如果允許透過連接埠傳送輸入流量，則不必指定輸出安全性規則來透過連接埠回應流量。
- 當您移除啟用流量的安全性規則時，現有的連線不會中斷。 當連線停止，且兩個方向至少有數分鐘都沒有流量時，流量即會中斷。
- 建立 Nsg 時，請盡可能建立，但視需要建立許多。

### <a name="best-practice-secure-northsouth-and-eastwest-traffic"></a>最佳做法：保護北/南部和東部/西部流量

若要保護虛擬網路，請考慮攻擊媒介。 請注意下列幾點：

- 只使用子網路 NSG 可簡化環境，但卻只能保護進入子網路的流量。 這種流量稱為南/北流量。
- 位於相同子網路上 VM 之間的流量則稱為東/西流量。
- 請務必同時使用這兩種形式的保護，如此一來，當駭客從外部取得存取權時，就會在嘗試攻擊位於相同子網的電腦時停止。

### <a name="use-service-tags-on-nsgs"></a>在 NSG 上使用服務標籤

服務標籤表示一組 IP 位址前置詞。 使用服務標籤有助於降低建立 NSG 規則時的複雜性。

- 在建立規則時，您可以使用服務標籤來取代使用特定 IP 位址。
- Microsoft 會管理與服務標籤相關聯的位址前置詞，並隨著位址變更自動更新服務標籤。
- 您無法建立自己的服務標籤，也無法指定標籤中包含哪些 IP 位址。

服務標籤可讓您不必以手動方式將規則指派給 Azure 服務的群組。 例如，如果您想要允許包含 web 伺服器的子網存取 Azure SQL Database，您可以建立輸出規則到埠1433，並使用 **SQL** 服務標記。

- 此 **Sql** 標籤代表 Azure SQL Database 和 Azure SQL 資料倉儲服務的位址前置詞。
- 如果您將 **sql** 指定為值，就會允許或拒絕 sql 的流量。
- 如果您只需要允許存取特定 **區域** 中的 Sql，則可以指定該區域。 例如，如果您只想要允許存取美國東部地區的 Azure SQL Database，您可以指定服務標記的 **EastUS** 。
- 標籤代表服務，但不代表服務的特定執行個體。 例如，標記代表 Azure SQL Database 服務，但不代表特定的 SQL Database 或伺服器。
- 此標記所表示的所有位址前置詞，也都可用 **網際網路** 標記表示。

**瞭解更多資訊：**

- 瞭解 [ (nsg) 的網路安全性群組 ](/azure/virtual-network/security-overview)。
- 請參閱 [可供 nsg 的服務標記](/azure/virtual-network/security-overview#service-tags)。

## <a name="best-practice-use-application-security-groups"></a>最佳做法：使用應用程式安全性群組

應用程式安全性群組可讓您將網路安全性設定為應用程式結構的自然延伸。

- 您可以將 VM 分組，並根據應用程式安全性群組定義網路安全性原則。
- 應用程式安全性群組可讓您大規模重複使用您的安全性原則，而不需要手動維護明確的 IP 位址。
- 應用程式安全性群組可處理明確 IP 位址和多個規則集的複雜性，讓您專注於商務邏輯。

**範例︰**

![應用程式安全性群組 [圖 9] 的圖表 ](./media/migrate-best-practices-networking/asg.png)
 _：應用程式安全性群組範例。_

| 網路介面 | 應用程式安全性群組 |
| --- | --- |
| `NIC1` | `AsgWeb` |
| `NIC2` | `AsgWeb` |
| `NIC3` | `AsgLogic` |
| `NIC4` | `AsgDb` |

在本例中，每個網路介面都只屬於一個應用程式安全性群組，但事實上介面可以屬於多個群組 (取決於 Azure 限制)。 這些網路介面都沒有相關聯的 NSG。 `NSG1` 與這兩個子網相關聯，且包含下列規則：

| 規則名稱 | 目的 | 詳細資料 |
| --- | --- | --- |
| `Allow-HTTP-Inbound-Internet` | 讓流量從網際網路流向 Web 伺服器。 預設安全性規則會拒絕來自網際網路的輸入流量 `DenyAllInbound` ，因此 `AsgLogic` 或 `AsgDb` 應用程式安全性群組不需要額外的規則。 | 優先順序：`100`<br><br> 來源：`internet` <br><br> 來源埠： `*` <br><br> 目的地： `AsgWeb` <br><br> 目的地埠： `80` <br><br> 協定： `TCP` <br><br> 訪問： `Allow` |
| `Deny-Database-All` | `AllowVNetInBound` 預設安全性規則允許相同虛擬網路中的資源之間的所有通訊。 需要此規則才能拒絕來自所有資源的流量。 | 優先順序：`120` <br><br> 來源：`*` <br><br> 來源埠： `*` <br><br> 目的地： `AsgDb` <br><br> 目的地埠： `1433` <br><br> 協定： `All` <br><br> 訪問： `Deny` |
| `Allow-Database-BusinessLogic` | 允許從 `AsgLogic` 應用程式安全性群組到 `AsgDb` 應用程式安全性群組的流量。 此規則的優先順序高於 `Deny-Database-All` 規則，因此會先處理此規則。 因此， `AsgLogic` 允許來自應用程式安全性群組的流量，並封鎖所有其他流量。 | 優先順序：`110` <br><br> 來源：`AsgLogic` <br><br> 來源埠： `*` <br><br> 目的地： `AsgDb` <br><br> 目的地埠： `1433` <br><br> 協定： `TCP` <br><br> 訪問： `Allow` |

用於將應用程式安全性群組指定為來源或目的地的規則，只會套用至屬於此應用程式安全性群組成員的網路介面。 如果網路介面不是應用程式安全性群組的成員，即使網路安全性群組與子網相關聯，也不會將規則套用至網路介面。

**瞭解更多資訊：**

- 深入瞭解 [應用程式安全性群組](/azure/virtual-network/security-overview#application-security-groups)。

### <a name="best-practice-secure-access-to-paas-by-using-virtual-network-service-endpoints"></a>最佳做法：使用虛擬網路服務端點來保護對 PaaS 的存取

虛擬網路服務端點可透過直接連線，將您的虛擬網路私人位址空間和身分識別延伸至 Azure 服務。

- 端點可讓您只將重要的 Azure 服務資源保護到您的虛擬網路。 從您的虛擬網路到 Azure 服務的流量一律會保留在 Azure 骨幹網路上。
- 虛擬網路私人位址空間可以重迭，因此無法用來唯一識別源自虛擬網路的流量。
- 在您的虛擬網路中啟用服務端點之後，您可以將虛擬網路規則新增至服務資源，以保護 Azure 服務資源。 這可透過完全移除資源的公用網際網路存取，並只允許來自您虛擬網路的流量，來改善安全性。

![服務端點的圖表。 ](./media/migrate-best-practices-networking/endpoint.png)
_圖10：服務端點。_

**瞭解更多資訊：**

- 深入瞭解 [虛擬網路服務端點](/azure/virtual-network/virtual-network-service-endpoints-overview)。

## <a name="best-practice-control-public-ip-addresses"></a>最佳做法：控制公用 IP 位址

Azure 中的公用 IP 位址可與 VM、負載平衡器、應用程式閘道和 VPN 閘道相關聯。

- 公用 IP 位址可讓網際網路資源對 Azure 資源進行輸入通訊，以及讓 Azure 資源對網際網路進行輸出通訊。
- 公用 IP 位址會使用基本或標準 SKU 來建立，這有幾項差異。 標準 SKU 可以指派給任何服務，但最常設定於 VM、負載平衡器和應用程式閘道上。
- 請務必注意，基本的公用 IP 位址不會自動設定 NSG。 您必須設定自己的，並指派規則來控制存取權。 標準 SKU IP 位址具有 NSG 和預設指派的規則。
- 最佳做法是 VM 不應該使用公用 IP 位址來進行設定。
  - 如果您需要開啟埠，它應該只適用于 web 服務，例如埠80或443。
  - 標準遠端系統管理埠，例如 SSH (22) 和 RDP (3389) 以及所有其他埠，都應該使用 Nsg 設定為 [拒絕]。
- 更好的做法是將 Vm 放在 Azure Load Balancer 或 Azure 應用程式閘道後方。 然後，如果您需要遠端系統管理埠的存取權，您可以在 Azure 資訊安全中心中使用即時 VM 存取。

**瞭解更多資訊：**

- [Azure 中的公用 IP 位址](/azure/virtual-network/virtual-network-ip-addresses-overview-arm#public-ip-addresses)
- [使用即時管理虛擬機器存取](/azure/security-center/security-center-just-in-time)

## <a name="take-advantage-of-azure-security-features-for-networking"></a>針對網路利用 Azure 安全性功能

Azure 具有平台層級的安全性功能，包括 Azure 防火牆、Web 應用程式防火牆和網路監看員。

## <a name="best-practice-deploy-azure-firewall"></a>最佳做法：部署 Azure 防火牆

Azure 防火牆是受控的雲端式網路安全性服務，可協助保護您的虛擬網路資源。 它是完全具狀態的受控防火牆，具有內建的高可用性和不受限制的雲端擴充性。

![Azure 防火牆的圖表。 ](./media/migrate-best-practices-networking/firewall.png)
_圖11： Azure 防火牆。_

當您部署服務時，請注意以下幾點：

- Azure 防火牆可以跨訂用帳戶和虛擬網路集中建立、強制執行及記錄應用程式和網路連線原則。
- Azure 防火牆會針對您的虛擬網路資源使用靜態公用 IP 位址。 這可讓外部防火牆識別來自您虛擬網路的流量。
- Azure 防火牆與 Azure 監視器會完全整合，以進行記錄和分析。
- 當您建立 Azure 防火牆規則時，最好使用 FQDN 標記。
  - FQDN 標籤代表一群與知名 Microsoft 服務相關聯的 FQDN。
  - 您可以使用 FQDN 標籤，以允許必要的輸出網路流量通過防火牆。
- 例如，若要手動允許 Windows Update 網路流量通過防火牆，您需要建立多個應用程式規則。 藉由使用 FQDN 標籤，您可以建立應用程式規則，並包含 Windows Update 標記。 備有此規則後，流向 Microsoft Windows Update 端點的網路流量就能流過防火牆。

**瞭解更多資訊：**

- 閱讀 [Azure 防火牆總覽](/azure/firewall/overview)。
- 瞭解 [Azure 防火牆中的 FQDN 標記](/azure/firewall/fqdn-tags)。

## <a name="best-practice-deploy-web-application-firewall"></a>最佳做法：部署 Web 應用程式防火牆

Web 應用程式已逐漸成為利用常見已知弱點的惡意攻擊目標。 這些攻擊包括 SQL 插入式攻擊和跨網站指令碼攻擊。 避免應用程式程式碼中的這類攻擊可能會很困難，而且可能需要嚴格的維護、修補和監視應用程式拓撲的多個層級。

Web 應用程式防火牆 (WAF) 是 Azure 應用程式閘道的一項功能，可協助您更輕鬆地進行安全性管理，並協助應用程式系統管理員防範威脅或入侵。 您可以透過在中央位置修補已知弱點，更快地回應安全性威脅，而不是保護個別 web 應用程式的安全。 您可以輕鬆地將現有的應用程式閘道轉換成已啟用 Web 應用程式防火牆的應用程式閘道。

以下是有關 WAF 的一些其他注意事項：

- WAF 可集中保護 web 應用程式免于常見的攻擊和弱點。
- 您不需要修改您的程式碼，就能利用 WAF。
- 它可以在應用程式閘道背後同時保護多個 web 應用程式。
- WAF 已經與 Azure 資訊安全中心整合。
- 您可以自訂 WAF 規則和規則群組，以符合您的應用程式需求。
- 最佳做法是，您應該在任何 web 導向應用程式前面使用 WAF，包括 Azure Vm 或 Azure App Service 中的應用程式。

**瞭解更多資訊：**

- 深入瞭解 [WAF](/azure/application-gateway/waf-overview)。
- 複習 [WAF 限制和排除](/azure/application-gateway/application-gateway-waf-configuration)專案。

## <a name="best-practice-implement-azure-network-watcher"></a>最佳做法：執行 Azure 網路監看員

Azure 網路監看員提供的工具可監視 Azure 虛擬網路中的資源和通訊。 例如，您可以監視 VM 與端點之間的通訊，例如另一個 VM 或 FQDN。 您也可以在虛擬網路中查看資源與資源的關聯性，或診斷網路流量問題。

![網路監看員的螢幕擷取畫面。 ](./media/migrate-best-practices-networking/network-watcher.png)
_圖12：網路_ 監看員。

以下是更多詳細資料：

- 您可以使用網路監看員監視和診斷網路問題，而不需要登入 Vm。
- 您可以設定警示來觸發封包擷取，並能存取封包層級的即時效能資訊。 當您發現問題時，可以詳加調查。
- 最佳做法是使用網路監看員來檢閱 NSG 流程記錄。
  - 網路監看員中的 NSG 流程記錄可讓您檢視透過 NSG 的輸入和輸出 IP 流量相關資訊。
  - 流程記錄會以 JSON 格式撰寫。
  - 流程記錄會以每個規則為基礎顯示輸出和輸入流量，以及套用流量 (NIC) 的網路介面。 它們也會顯示關於流程 (來源/目的地 IP、來源/目的地埠和通訊協定) 的5元組資訊，以及流量是被允許或拒絕。

**瞭解更多資訊：**

- 閱讀網路監看員 [總覽](/azure/network-watcher)。
- 深入瞭解 [NSG 流量記錄](/azure/network-watcher/network-watcher-nsg-flow-logging-overview)。

## <a name="use-partner-tools-in-azure-marketplace"></a>在 Azure Marketplace 中使用合作夥伴工具

針對更複雜的網路拓撲，您可以使用 Microsoft 合作夥伴的安全性產品，尤其是網路虛擬設備 (NVA)。

- NVA 是執行網路功能的 VM，例如防火牆、WAN 最佳化或其他網路功能。
- Nva 會加強虛擬網路安全性和網路功能。 您可以針對高可用性防火牆、入侵預防、入侵偵測、Waf、WAN 優化、路由、負載平衡、VPN、憑證管理、Active Directory 和多重要素驗證進行部署。
- Nva 適用于 [Azure Marketplace](https://azuremarketplace.microsoft.com)中的許多廠商。

## <a name="best-practice-implement-firewalls-and-nvas-in-hub-networks"></a>最佳做法：在中樞網路中執行防火牆和 Nva

在中樞內，您通常會透過 Azure 防火牆、防火牆伺服器陣列或 WAF，來管理可存取網際網路) 的周邊網路 (。 下表提供這些的比較。

| 防火牆類型 | 詳細資料 |
| --- | --- |
| WAF | Web 應用程式很常見，其容易受到弱點和潛在攻擊的影響。 Waf 的設計是為了偵測對 web 應用程式的攻擊 (HTTP/HTTPS) 。 相較於傳統防火牆技術，WAF 有一組特定功能可保護內部網頁伺服器不受威脅。 |
| Azure 防火牆 | 如同 NVA 防火牆伺服器陣列，Azure 防火牆會使用常見的管理機制和一組安全性規則來保護輪輻網路中所裝載的工作負載。 Azure 防火牆也可協助控制對內部部署網路的存取。 Azure 防火牆具有內建的擴充性。 |
| NVA 防火牆 | 和 Azure 防火牆一樣，NVA 防火牆伺服器陣列具有一般管理機制和一組安全性規則，可保護輪輻網路中所裝載的工作負載。 NVA 防火牆也可協助控制對內部部署網路的存取。 NVA 防火牆可以在負載平衡器後方手動擴充。 <br><br> 雖然 NVA 防火牆所擁有的特製化軟體比 WAF 還要少，但其具有較廣泛的應用程式範圍，可篩選和檢查任何類型的輸出和輸入流量。 |

我們建議使用一組 Azure 防火牆 (或 Nva) 用於源自網際網路的流量，另一個用於源自內部部署的流量。 對兩者僅使用一組防火牆會造成安全性風險，因為它未提供兩組網路流量之間的安全性範疇。 使用個別的防火牆層級可降低檢查安全性規則的複雜度，並明確指出哪些規則對應到哪個傳入網路要求。

**瞭解更多資訊：**

- 瞭解如何 [在 Azure 虛擬網路中使用 nva](/azure/architecture/reference-architectures/dmz/secure-vnet-dmz)。

## <a name="next-steps"></a>後續步驟

檢閱其他最佳做法：

- 移轉後的安全性及管理[最佳做法](./migrate-best-practices-security-management.md)。
- 移轉後的成本管理[最佳做法](./migrate-best-practices-costs.md)。
