---
title: 與其他雲端提供者的連線能力
description: 檢查有關不同連線方法的重要設計考慮和建議，以將 Azure 企業規模的登陸區域架構整合到 Oracle 雲端基礎結構 (OCI) 。
author: alexandreweiss
ms.author: brblanch
ms.date: 02/18/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: think-tank
ms.openlocfilehash: 210a097d8e03a66e9951e7803de628d3fd5913a6
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102117292"
---
# <a name="connectivity-to-other-cloud-providers"></a>與其他雲端提供者的連線能力

檢查有關不同連線方法的重要設計考慮和建議，以將 Azure 企業規模的登陸區域架構整合至其他雲端提供者。

## <a name="oracle-cloud-infrastructure-oci"></a>Oracle 雲端基礎結構 (OCI) 

本節提供不同的連線方法，可將 Azure 企業規模登陸區域架構整合到 Oracle 雲端基礎結構 (OCI) 。

**設計考慮：**

- 如果私人 IP 位址空間沒有重迭，客戶可以使用 ExpressRoute 和 FastConnect，將 Azure 中的虛擬網路與 OCI 中的虛擬雲端網路連線。 一旦建立此連線，Azure 虛擬網路中的資源就能與 OCI 虛擬雲端網路中的資源進行通訊，就好像它們都在相同的網路中一樣。

- Azure ExpressRoute [FastPath](/azure/expressroute/about-fastpath) 的設計目的是要改善兩個網路之間的資料路徑效能 (內部部署和 azure) ，而在此案例中，則是在 OCI 和 azure 之間。 啟用時，FastPath 會將網路流量直接傳送到虛擬網路中的虛擬機器，略過 ExpressRoute 閘道。

  - FastPath 適用于所有 ExpressRoute 線路。

  - FastPath 仍然需要建立虛擬網路閘道，以供路由交換之用。 虛擬網路閘道必須使用 Ultra 效能 SKU 或適用于 ExpressRoute 閘道的 ErGw3AZ SKU 來啟用路由管理。

- ExpressRoute FastPath 目前 [不支援](/azure/expressroute/about-fastpath#supported-features) 某些功能，例如 AZURE 虛擬 WAN 中樞或 VNet 對等互連。

- 雖然您可以使用 [Expressroute 全球存取範圍](/azure/expressroute/expressroute-global-reach) 來啟用透過 ExpressRoute 線路從內部部署至 OCI 的通訊，但這可能會產生額外的頻寬成本，可使用 [Azure 定價計算機](https://azure.microsoft.com/pricing/calculator/)來計算。 當您使用 ExpressRoute 線路將大量資料從內部部署遷移至 Oracle 時，這是特別重要的考慮。

- 在支援 [可用性區域](/azure/availability-zones/az-overview#availability-zones)的 azure 區域中，將您的 azure 工作負載放在一個區域或另一個區域，對延遲可能會有些許影響。 設計應用程式以平衡可用性和效能需求。

- Azure 與 OCI 之間的互連能力僅適用 [于特定區域](/azure/virtual-machines/workloads/oracle/oracle-oci-overview#region-availability)。

- 如需 Azure 與 OCI 之間互連能力的更深入檔，請參閱 [整合 Microsoft Azure 與 Oracle 雲端基礎結構的 oracle 應用程式解決方案](/azure/virtual-machines/workloads/oracle/oracle-oci-overview) ，或參閱 [oracle 檔](https://docs.cloud.oracle.com/iaas/Content/Network/Concepts/azure.htm)集。

**設計建議：**

- 建立將在 **連線訂用** 帳戶中用來與 OCI 互連 Azure 的 ExpressRoute 線路 (s) 。

- 您可以透過將用來與 Azure 互連的 ExpressRoute 線路連線到中樞 VNet 或虛擬 WAN 中樞（如下圖所示），將 Azure 網路架構與傳統的中樞和輪輻架構或 Azure 虛擬 WAN 型網路拓撲互連。

  ![此圖顯示 Azure 至 OCI 中樞和輪輻。](./media/azure-oci-hub-and-spoke.png)

  *圖1：透過 ExpressRoute 在 Azure 與 OCI 之間互連能力。*

- 如果您的應用程式需要 Azure 和 OCI 之間可能的最低延遲，請考慮將您的應用程式部署在具有 ExpressRoute 閘道的單一 VNet 中，並啟用 FastPath。

  ![顯示 Azure 至 OCI-單一 vNet 的圖表。](./media/azure-oci-one-vnet.png)

  *圖2：在 Azure 與 OCI 之間使用單一 VNet 進行互連能力。*

- 跨可用性區域部署 Azure 資源時，請從位於不同可用性區域中的 Azure Vm 執行延遲測試，以 OCI 資源，以瞭解三個可用性區域中的哪一個可提供最低的 OCI 資源延遲。

- 若要使用 Azure 資源和技術來操作 OCI 中託管的 Oracle 資源，您可以：

  - **從 Azure：** 在輪輻 VNet 中部署 jumpbox。 Jumpbox 可讓您存取 OCI 中的虛擬雲端網路，如下圖所示：

    ![此圖顯示 Azure 至 OCI-Jumpbox 一個 VNet。](./media/azure-oci-jumpbox-one-vnet.png)

    *圖3：透過 jumpbox 從 Azure 管理 OCI 資源。*

  - **從內部部署：** 使用 ExpressRoute Global 觸及系結將內部部署連線至 Azure) 的現有 ExpressRoute 線路 (，以 OCI 將 Azure 互連至 OCI) 的 ExpressRoute 線路 (。 如此一來，Microsoft Enterprise Edge (MSEE) 路由器會成為兩個 ExpressRoute 線路之間的中央路由點。

    ![此圖顯示 Azure 到 OCI-via 全球。](./media/azure-oci-gr-hub-and-spoke.png)

  *圖4：透過 ExpressRoute 全球存取範圍從內部部署管理 OCI 資源。*
