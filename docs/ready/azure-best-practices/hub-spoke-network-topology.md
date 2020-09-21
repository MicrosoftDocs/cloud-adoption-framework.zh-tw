---
title: 中樞和輪輻網路拓撲
description: 深入瞭解中樞和輪輻網路拓撲，以更有效率地管理常見的通訊或安全性需求。
author: tracsman
ms.author: jonor
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
manager: rossort
ms.custom: virtual-network
ms.openlocfilehash: 9aa123f458258a8fbb666345006661332701f12d
ms.sourcegitcommit: 014fe718162573f02d41bdc151a7302f02ca777b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/21/2020
ms.locfileid: "90832887"
---
# <a name="hub-and-spoke-network-topology"></a>中樞和輪輻網路拓撲

_中樞和輪輻_ 是一種網路模型，可有效率地管理常見的通訊或安全性需求。 這也有助於避開 Azure 訂用帳戶的限制。 此模型可解決下列問題：

- **節省成本和管理效率。** 將可由多個工作負載共用的服務 (例如網路虛擬設備 (NVA) 和 DNS 伺服器) 集中在單一位置，讓 IT 能夠將多餘的資源和管理投入量降至最低。
- **克服訂用帳戶限制。** 大型雲端式工作負載可能需要使用超過單一 Azure 訂用帳戶所允許的資源。 將工作負載虛擬網路從不同的訂用帳戶對等互連到中央中樞，即可克服這些限制。 如需詳細資訊，請參閱 [Azure 訂](/azure/azure-resource-manager/management/azure-subscription-service-limits)用帳戶限制。
- **關注點分離。** 您可以在中央 IT 小組和工作負載小組之間部署個別工作負載。

較小的雲端資產可能無法受益於此模型所提供的新增結構和功能。 但較大型的雲端採用工作應該考慮採用中樞和輪輻網路架構，如果它們有任何先前所列的考慮。

> [!NOTE]
> Azure 參考架構網站包含範例範本，您可以使用這些範本作為基礎來執行您自己的中樞和輪輻網路：
>
> - [在 Azure 中執行中樞和輪輻網路拓撲](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)
> - [在 Azure 中使用共用服務來實行中樞和輪輻網路拓撲](/azure/architecture/reference-architectures/hybrid-networking/shared-services)

## <a name="overview"></a>概觀

![中樞和輪輻網路拓撲的範例](../../_images/azure-best-practices/network-hub-spoke-high-level.png)  
_圖1：中樞和輪輻網路拓撲的範例。_

如圖所示，Azure 支援兩種類型的中樞和輪輻設計。 它支援通訊、共用資源和集中式安全性原則 (標示為 `VNet hub` 圖表) ，或根據 Azure 虛擬 WAN (標示為 Azure 虛擬 WAN 的設計， `Virtual WAN` 適用于大規模分支對分支和分支對 Azure 通訊的圖表) 。

中樞是中央網路區域，可控制和檢查區域之間的輸入或輸出流量：網際網路、內部部署和輪輻。 中樞和輪輻拓撲可讓您的 IT 部門有效地在中央位置強制執行安全性原則。 此外也可降低設定錯誤和暴露的可能性。

中樞通常包含輪輻所使用的一般服務元件。 以下是常用中央服務的範例：

- Windows server Active Directory 基礎結構，為協力廠商從未受信任的網路取得存取權，然後才能夠存取輪輻中的工作負載所需的使用者驗證。 其中包括相關的 Active Directory 同盟服務 (AD FS)。
- DNS 服務，用來對輪輻中的工作負載進行命名解析，以存取內部部署和網際網路上的資源 (如果未使用 [Azure DNS](/azure/dns/dns-overview))。
- 公開金鑰基礎結構 (PKI)，用以實作工作負載的單一登入。
- 輪輻網路區域與網際網路之間的 TCP 和 UDP 流量的流量控制。
- 輪輻與內部部署之間的流量控制。
- 兩個輪輻之間的流量控制 (如有需要)。

您可以使用共用中樞基礎結構來支援多個輪輻，將備援降到最低、簡化管理並降低整體成本。

每個支點的角色都可以裝載不同類型的工作負載。 輪輻也提供適用的模組化方法，供相同的工作負載進行可重複的部署。 範例包括開發/測試、使用者接受度測試、預備和生產環境。

輪輻也可區隔和啟用組織內的不同群組。 Azure DevOps 群組為範例之一。 在輪輻內部，可以使用各層之間的流量控制來部署基本工作負載或複雜的多層式工作負載。

## <a name="subscription-limits-and-multiple-hubs"></a>訂用帳戶限制和多個中樞

在 Azure 中，每個任何類型的元件都會部署在 Azure 訂用帳戶中。 不同 Azure 訂用帳戶中的 Azure 元件隔離可以滿足不同企業營運的需求，例如設定不同層級的存取和授權。

單一中樞和輪輻執行可以擴大為大量輪輻。 但如同每個 IT 系統的共同問題，平台有其限制。 中樞部署會繫結至具有限制的特定 Azure 訂用帳戶。 其中一個範例是虛擬網路對等互連的最大數目。 如需詳細資訊，請參閱 [Azure 訂用帳戶和服務限制](/azure/azure-resource-manager/management/azure-subscription-service-limits)。

如果限制可能會產生問題，您可以將模型從單一中樞和輪輻延伸到中樞和輪輻叢集，即可進一步延展架構。 您可以使用虛擬網路對等互連、Azure ExpressRoute、Azure 虛擬 WAN 或站對站 VPN，將一或多個 Azure 區域中的多個中樞互連。

![中樞和輪輻叢集](../../_images/azure-best-practices/network-hub-spokes-cluster.png)  
_圖2：中樞和輪輻的群集。_

導入多個中樞會增加系統的成本和管理額外負荷。 此狀況只能從使用者效能或災害復原的延展性、系統限制或備援和區域複寫來調整。 在需要多個中樞的案例中，所有中樞都應該致力於提供一組相同的易操作服務。

## <a name="interconnection-between-spokes"></a>支點之間的互相連線

在單一輪輻內可以實作複雜的多層式工作負載。 若要實作多層式設定，您可以在相同虛擬網路中使用子網路 (每一層一個)，以及使用網路安全性群組來篩選流程。

架構設計人員可能會想要跨多個虛擬網路部署一個多層式工作負載。 透過虛擬網路對等互連，輪輻可以連線至相同中樞或不同中樞內的其他輪輻。

此案例的常見範例是，應用程式處理伺服器位於一個輪輻 (或虛擬網路) 中。 資料庫則部署在多個輪輻 (或虛擬網路) 中。 在此情況下，可以輕易地透過虛擬網路對等互連將輪輻互相連線，而避免透過中樞傳輸。 解決方法是執行仔細的架構和安全性審查，以確保略過中樞並不會略過可能僅存在於中樞的重要安全性或審核點。

![連線到中樞及彼此連線的輪輻](../../_images/azure-best-practices/network-spoke-to-spoke.png)  
_圖3：輪輻彼此連接，以及一個中樞。_

支點也可以與作為中樞的支點互相連接。 這種方法會建立雙層階層：較高層級的輪輻 (層級 0) 會變成階層中較低輪輻 (層級 1) 的中樞。 中樞和輪輻執行的輪輻必須將流量轉送到中央中樞，讓流量可以傳輸至內部部署網路或公用網際網路的目的地。 具有兩層中樞的架構引進複雜路由，以移除簡單中樞和輪輻關聯性的優點。
