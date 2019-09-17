---
title: 中樞和輪輻網路拓撲
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 中樞和輪輻網路拓撲
author: tracsman
ms.author: jonor
ms.date: 05/10/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: ready
manager: rossort
tags: azure-resource-manager
ms.custom: virtual-network
ms.openlocfilehash: 35750064b0a88c65796f662d20dc51e9a38e77ac
ms.sourcegitcommit: 443c28f3afeedfbfe8b9980875a54afdbebd83a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2019
ms.locfileid: "71022400"
---
# <a name="hub-and-spoke-network-topology"></a>中樞和輪輻網路拓撲

「中樞和輪輻」是一種網路模型，可更有效地管理常見通訊或安全性需求。 這也有助於避開 Azure 訂用帳戶的限制。 此模型可解決下列問題：

- **節省成本和管理效率**。 將可由多個工作負載共用的服務 (例如網路虛擬設備 (NVA) 和 DNS 伺服器) 集中在單一位置，讓 IT 能夠將多餘的資源和管理投入量降至最低。
- **克服訂用帳戶限制**。 大型雲端式工作負載需要使用的資源，可能比單一 Azure 訂用帳戶內所允許的資源還要多。 將工作負載虛擬網路從不同的訂用帳戶對等互連到中央中樞，即可克服這些限制。 如需詳細資訊，請參閱訂用帳戶[限制](https://docs.microsoft.com/azure/azure-subscription-service-limits)。
- **考量分隔**。 您可以在中央 IT 小組和工作負載小組之間部署個別工作負載。

較小的雲端資產可能無法受益於此模型所提供的新增結構和功能。 但較大的雲端採用工作應該考慮實作中樞和輪輻網路架構 (如果他們有前述問題的話)。

> [!NOTE]
> Azure 參考架構網站有範例範本，當您實作自己的中樞和輪輻網路時，您可以使用該範本作為基礎：
>
> - [在 Azure 中實作中樞和輪輻網路拓撲](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)
> - [在 Azure 中實作中樞和輪輻網路拓撲與共用服務](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/shared-services)

## <a name="overview"></a>總覽

![中樞和輪輻網路拓撲的範例][1]

如圖所示，Azure 支援兩種類型的中樞和輪輻設計。 其支援通訊、共用資源和集中式安全性原則 (圖表中的「VNet 中樞」)，或適用於大規模分支對分支及分支對 Azure 通訊的虛擬 WAN 類型 (圖表中的「虛擬 WAN」)。

中樞是中央網路區域，可控制和檢查區域之間的輸入或輸出流量：網際網路、內部部署和輪輻。 中樞和輪輻拓撲可讓 IT 部門有效地在中央位置強制執行安全性原則。 此外也可降低設定錯誤和暴露的可能性。

中樞通常包含輪輻所使用的一般服務元件。 以下是常用中央服務的範例：

- 當第三方從不受信任的網路取得存取權時，需要 Windows Server Active Directory 基礎結構來進行使用者驗證，如此他們才能夠存取輪輻中的工作負載。 其中包括相關的 Active Directory 同盟服務 (AD FS)。
- DNS 服務，用來對輪輻中的工作負載進行命名解析，以存取內部部署和網際網路上的資源 (如果未使用 [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview))。
- 公開金鑰基礎結構 (PKI)，用以實作工作負載的單一登入。
- 輪輻網路區域與網際網路之間的 TCP 和 UDP 流量的流量控制。
- 輪輻與內部部署之間的流量控制。
- 兩個輪輻之間的流量控制 (如有需要)。

您可以使用共用中樞基礎結構來支援多個輪輻，將備援降到最低、簡化管理並降低整體成本。

每個支點的角色都可以裝載不同類型的工作負載。 輪輻也提供適用的模組化方法，供相同的工作負載進行可重複的部署。 範例包括開發和測試、使用者接受度測試、預備和生產環境。

輪輻也可區隔和啟用組織內的不同群組。 Azure DevOps 群組為範例之一。 在輪輻內部，可以使用各層之間的流量控制來部署基本工作負載或複雜的多層式工作負載。

## <a name="subscription-limits-and-multiple-hubs"></a>訂用帳戶限制和多個中樞

在 Azure 中，每個任何類型的元件都會部署在 Azure 訂用帳戶中。 不同 Azure 訂用帳戶中的 Azure 元件隔離可以滿足不同企業營運的需求，例如設定不同層級的存取和授權。

單一中樞和輪輻實作可以相應增加為大量輪輻。 但如同每個 IT 系統的共同問題，平台有其限制。 中樞部署會繫結至具有限制的特定 Azure 訂用帳戶。 (虛擬網路對等互連的數目上限即為一例。 如需詳細資訊，請參閱 [Azure 訂用帳戶和服務限制、配額及條件約束] [限制]。

如果限制可能會產生問題，您可以將模型從單一中樞和輪輻延伸到中樞和輪輻叢集，即可進一步延展架構。 您可以使用虛擬網路對等互連、Azure ExpressRoute、虛擬 WAN 或站對站 VPN，將一或多個 Azure 區域中的多個中樞互相連線。

![中樞和輪輻叢集][2]

導入多個中樞會增加系統的成本和管理額外負荷。 此狀況只能從使用者效能或災害復原的延展性、系統限制或備援和區域複寫來調整。 在需要多個中樞的案例中，所有中樞都應該致力於提供一組相同的易操作服務。

## <a name="interconnection-between-spokes"></a>支點之間的互相連線

在單一輪輻內可以實作複雜的多層式工作負載。 若要實作多層式設定，您可以在相同虛擬網路中使用子網路 (每一層一個)，以及使用網路安全性群組來篩選流程。

架構設計人員可能會想要跨多個虛擬網路部署一個多層式工作負載。 透過虛擬網路對等互連，輪輻可以連線至相同中樞或不同中樞內的其他輪輻。

此案例的常見範例是，應用程式處理伺服器位於一個輪輻 (或虛擬網路) 中。 資料庫則部署在多個輪輻 (或虛擬網路) 中。 在此情況下，可以輕易地透過虛擬網路對等互連將輪輻互相連線，而避免透過中樞傳輸。 此解決方案的目的是仔細檢閱架構和安全性，以確定略過中樞時並不會略過可能僅存在於中樞的重要安全性或稽核點。

![連線到中樞及彼此連線的輪輻][3]

支點也可以與作為中樞的支點互相連接。 這種方法會建立雙層階層：較高層級的輪輻 (層級 0) 會變成階層中較低輪輻 (層級 1) 的中樞。 必須實作中樞和輪輻實作的輪輻來將流量轉送到中央中樞，如此一來，流量才能傳送到內部部署網路或公用網際網路中的目的地。 具有雙層中樞的架構引進複雜路由，以移除簡單中樞和輪輻關聯性的優點。

<!-- images -->

[0]: ../../_images/azure-best-practices/network-redundant-equipment.png "元件重疊範例"
[1]: ../../_images/azure-best-practices/network-hub-spoke-high-level.png "高階中樞和輪輻範例"
[2]: ../../_images/azure-best-practices/network-hub-spokes-cluster.png "中樞和支點叢集"
[3]: ../../_images/azure-best-practices/network-spoke-to-spoke.png "支點對支點"
[4]: ../../_images/azure-best-practices/network-hub-spoke-block-level-diagram.png "中樞和輪輻的區塊層級圖表"
[5]: ../../_images/azure-best-practices/network-users-groups-subscriptions.png "使用者、群組、訂用帳戶和專案"
[6]: ../../_images/azure-best-practices/network-infrastructure-high-level.png "高階基礎結構圖"
[7]: ../../_images/azure-best-practices/network-high-level-perimeter-networks.png "高階基礎結構圖"
[8]: ../../_images/azure-best-practices/network-vnet-peering-perimeter-networks.png "虛擬網路對等互連和周邊網路"
[9]: ../../_images/azure-best-practices/network-high-level-diagram-monitoring.png "高階監視圖"
[10]: ../../_images/azure-best-practices/network-high-level-workloads.png "高階工作負載圖"

<!-- links -->

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
[SubMgmt]: ../../reference/azure-scaffold.md
[RGMgmt]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview
[DMZ]: https://docs.microsoft.com/azure/best-practices-network-security
[ALB]: https://docs.microsoft.com/azure/load-balancer/load-balancer-overview
[PIP]: https://docs.microsoft.com/azure/virtual-network/resource-groups-networking#public-ip-address
[AFD]: https://docs.microsoft.com/azure/frontdoor/front-door-overview
[AppGW]: https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction
[WAF]: https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview
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
