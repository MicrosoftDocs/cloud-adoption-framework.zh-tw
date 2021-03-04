---
title: 定義 Azure 網路拓撲
description: 查看 Azure 中關於網路拓撲的重要設計考慮和建議。
author: JefferyMitchell
ms.author: brblanch
ms.date: 02/18/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: think-tank
ms.openlocfilehash: d9c6e3d2561a0252ff586feb5771b48675cbc34a
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794972"
---
# <a name="define-an-azure-network-topology"></a>定義 Azure 網路拓撲

網路拓撲是企業規模架構的重要元素，因為它會定義應用程式彼此通訊的方式。 本節探討適用于 Azure 部署的技術與拓撲方法。 它著重于兩個核心方法：以 Azure 虛擬 WAN 和傳統拓撲為基礎的拓撲。

[虛擬 WAN 可用來滿足大規模的互連能力需求](../azure-best-practices/virtual-wan-network-topology.md)。 因為這是 Microsoft 管理的服務，所以它也會降低整體網路複雜度，並有助於將組織的網路現代化。 如果下列任一點符合您的需求，則虛擬 WAN 拓撲可能最適合：

- 您的組織想要跨數個 Azure 區域部署資源，而且需要在這些 Azure 區域的 Vnet 與多個內部部署位置之間有全球連線能力。
- 您的組織想要透過軟體定義的 WAN (SD-WAN) 部署，將大規模分支網路直接整合到 Azure，或需要超過30個分支網站以進行原生 IPsec 終止。
- 您需要 VPN 和 ExpressRoute 之間的可轉移路由。 例如 透過站對站 VPN 或透過點對站 VPN 連線的遠端使用者連線的遠端分支，需要透過 Azure 連線到 ExpressRoute 連線的 DC。

[傳統的中樞和輪輻網路拓撲](../azure-best-practices/traditional-azure-networking-topology.md) 可協助您在 Azure 中建立自訂的安全大規模網路，並提供客戶所管理的路由和安全性。 如果下列任一點符合您的需求，傳統拓撲可能最適合：

- 您的組織想要在一或多個 Azure 區域中部署資源，而在 Azure 區域之間有一些流量是預期的 (例如，在兩個不同 Azure 區域之間的兩個虛擬網路之間的流量) ，則不需要跨所有 Azure 區域的完整網狀網路。
- 您每個區域的遠端或分支位置都有很多。 也就是說，您需要少於30個 IPsec 站對站通道。
- 您需要完整的控制和細微性，才能手動設定 Azure 網路路由原則。

## <a name="virtual-wan-network-topology-microsoft-managed"></a>虛擬 WAN 網路拓撲 (Microsoft 管理的) 

![說明虛擬 WAN 網路拓撲的圖。](./media/virtual-wan-topology.png)

*圖1：虛擬 WAN 網路拓撲。*

## <a name="traditional-azure-networking-topology"></a>傳統的 Azure 網路拓撲

![說明傳統 Azure 網路拓撲的圖表。](./media/customer-managed-topology.png)

*圖2：傳統的 Azure 網路拓撲。*
