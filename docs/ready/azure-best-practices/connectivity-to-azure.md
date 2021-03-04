---
title: Azure 的連線能力
description: 檢查有關網路拓撲的重要設計考慮和建議，以將內部部署連線至 Azure。
author: JefferyMitchell
ms.author: brblanch
ms.date: 02/18/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: think-tank
ms.openlocfilehash: 5989d9b92cf3387cd4cf5e82d74212b50132aa7c
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101793673"
---
# <a name="connectivity-to-azure"></a>Azure 的連線能力

本節將擴充網路拓撲，以考慮將內部部署位置連線到 Azure 的建議模型。

**設計考慮：**

- Azure ExpressRoute 提供 Azure 基礎結構即服務的專用私人連線， (IaaS) 和平臺即服務， (PaaS) 內部部署位置的功能。

- 您可以使用 Private Link 透過具有私人對等互連的 ExpressRoute 來建立 PaaS 服務的連線能力。

- 當多個虛擬網路連線至相同的 ExpressRoute 線路時，它們會成為相同路由網域的一部分，而且所有虛擬網路都會共用頻寬。

- 您可以使用 ExpressRoute 全球存取範圍（如有提供），透過 ExpressRoute 線路將內部部署位置連接在一起，以透過 Microsoft 骨幹網路傳輸流量。

- ExpressRoute Global 觸及可在許多 [expressroute 對等互連位置](/azure/expressroute/expressroute-global-reach#availability)中使用。

- ExpressRoute Direct 可讓您不需額外成本，即可建立多個 ExpressRoute 線路，最多可達 ExpressRoute Direct 埠容量 (10 Gbps 或 100 Gbps) 。 它也可讓您直接連接到 Microsoft 的 ExpressRoute 路由器。 針對 100-Gbps SKU，最小電路頻寬為 5 Gbps。 若為 10 Gbps SKU，最小電路頻寬為 1 Gbps。

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
