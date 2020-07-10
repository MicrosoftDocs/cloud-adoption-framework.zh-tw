---
title: 將內部部署 VMware 基礎結構移至 Azure
description: 瞭解 Contoso 如何將內部部署 VMware Vm 移至 Azure。
author: deltadan
ms.author: abuck
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: azure-migrate
ms.openlocfilehash: 12d83670b5d2c18a14229c2b14373da70c6b84d9
ms.sourcegitcommit: bcc73d194c6d00c16ae2e3c7fb2453ac7dbf2526
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/09/2020
ms.locfileid: "86198056"
---
<!-- docsTest:ignore "Bulk Migration" "Cold Migration" -->

# <a name="move-on-premises-vmware-infrastructure-to-azure"></a>將內部部署 VMware 基礎結構移至 Azure

將 VMware 虛擬 (機從內部部署資料中心) 遷移至 Azure 時，Contoso 有數個可用的選項。

| 移轉選項 | 成果 |
| --- | --- |
| [Azure Migrate](https://azure.microsoft.com/services/azure-migrate/) | [評估](https://docs.microsoft.com/azure/migrate/tutorial-assess-vmware)和[遷移](https://docs.microsoft.com/azure/migrate/tutorial-migrate-vmware)內部部署 vm。 <br><br> 使用 Azure IaaS 執行工作負載。 <br><br> 使用[Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)管理 vm。 |
| [Azure VMware 解決方案](https://azure.microsoft.com/overview/azure-vmware) | 使用 VMware HCX 或 vMotion 來移動內部部署 Vm。 <br><br> 在 Azure 裸機硬體上執行原生 VMware 工作負載。 <br><br> 使用 vSphere 管理 Vm。 |

Azure VMware 解決方案是用來在 Azure 中建立具有 VMware vCenter 原生存取權的私人雲端，以及 VMware 針對工作負載遷移所支援的其他工具。 Contoso 可以放心地使用 Azure VMware 解決方案，並瞭解它們是 VMware 所支援的第一方 Microsoft 供應專案。

> [!NOTE]
> 本文著重于使用 Azure VMware 解決方案 (AVS) 將內部部署 VMware 虛擬機器移至 Azure。

## <a name="business-drivers"></a>商業動機

與商務合作夥伴密切合作，Contoso IT 小組會定義將 VMware 遷移至 Azure 的商業驅動程式。 這些驅動程式可能包括：

- **資料中心撤離或關機：** 在以 VMware 為基礎的工作負載匯總或淘汰現有的資料中心時順暢地移動。
- 嚴重損壞**修復和業務持續性：** 使用在 Azure 中部署的 VMware 堆疊，作為內部部署資料中心基礎結構的主要或次要隨選災難復原網站。
- **應用程式現代化：** 利用 Azure 生態系統來現代化 Contoso 的應用程式，而不需要重建 VMware 環境。
- **執行 DevOps：** 將 Azure DevOps 工具鏈帶到 VMware 環境，並依照自己的步調現代化應用程式。
- **確保操作持續性：** 將以 vSphere 為基礎的應用程式重新部署至 Azure，同時避免進行程式管理和應用程式重構。 擴充對執行 Windows 和 SQL Server 的繼承應用程式的支援。

## <a name="vmware-on-premises-to-vmware-in-the-cloud-goals"></a>雲端目標中的 VMware 內部部署至 VMware

在考慮商務驅動程式之後，Contoso 已將此項遷移的目標固定在一起：

- 使用其小組熟悉的 VMware 工具繼續管理其現有的環境，同時使用原生 Azure 服務現代化應用程式。
- 將 Contoso 的 VMware 工作負載從其資料中心順暢地移至 Azure，並將 VMware 環境與 Azure 整合。
- 在遷移之後，Azure 中的應用程式應該具有與目前在 VMware 中相同的效能功能。 在雲端中，應用程式在內部部署時仍會保持嚴重的重要性。

這些目標支援使用 AVS 的決策，並將其驗證為 Contoso 的最佳遷移方法。

## <a name="benefits-of-running-vmware-workloads-in-azure"></a>在 Azure 中執行 VMware 工作負載的優點

使用 Azure VMware 解決方案 (AVS) ，Contoso 現在可以使用通用的作業架構，順暢地跨 VMware 環境和 Azure 來執行、管理及保護應用程式。

Contoso 會利用現有的 VMware 投資、技能和工具（包括 VMware vSphere、vSAN 和 vCenter），同時使用 Azure 的規模、效能和創新。 其他權益可能包括：

- 幾分鐘內就能在雲端布建 VMware 基礎結構，並依自己的步調將應用程式現代化。
- 使用專用、隔離、高效能的基礎結構和獨特的 Azure 產品和服務，增強 VMware 應用程式。
- 將內部部署 Vm 移動或延伸至 Azure，而不需要重構應用程式。
- 針對全球 Azure 基礎結構上的 VMware 工作負載，取得規模、自動化和快速布建。
- Azure VMware 解決方案由 Microsoft 提供，並由 VMware 驗證，並在 Azure 基礎結構上執行。

## <a name="solutions-design"></a>解決方案設計

在將目標和需求固定之後，Contoso 會設計和審查部署解決方案，並識別遷移程式。

### <a name="current-architecture"></a>目前架構

- 部署到[vSphere](https://www.vmware.com/products/vsphere.html)所管理之內部部署資料中心的 vm。
- 在 VMware ESXi 主機叢集上部署的工作負載，會使用[vCenter](https://www.vmware.com/products/vcenter-server.html)、 [vSAN](https://www.vmware.com/products/vsan.html)和[NSX](https://www.vmware.com/products/nsx.html)進行管理。

### <a name="proposed-architecture"></a>建議的架構

- 將[AVS 私人雲端](https://docs.microsoft.com/azure/azure-vmware/concepts-private-clouds-clusters)部署至 `West US` Azure 區域。
- 將內部部署資料中心連接到在中執行的 AVS， `West US` 並在啟用全球範圍的情況下使用虛擬網路和[ExpressRoute](https://docs.microsoft.com/azure/azure-vmware/concepts-networking) 。
- 使用[VMware 混合式雲端擴充功能 (HCX) ](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html)，將 vm 遷移至專用的 Azure vmware 解決方案。

![建議的架構](./media/contoso-migration-vmware-to-azure/on-premises-stretched-network-expressroute.png)

## <a name="solution-review"></a>解決方案檢閱

Contoso 會透過比較一份優缺點清單，來評估建議設計。

| 考量 | 詳細資料 |
| --- | --- |
| **優點** | 高效能的裸機 VMware 基礎結構。 完全專用於 Contoso 的基礎結構，實際上與其他客戶的基礎結構隔離。 <br><br> 因為 Contoso 使用的是 VMware 的重新裝載，所以沒有特殊的設定或遷移的複雜性。 <br><br> Contoso 可以使用舊版 Windows 和 SQL 平臺的[Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/)和[擴充安全性更新](https://www.microsoft.com/cloud-platform/windows-server-2008)，來充分利用軟體保證中的投資。 <br><br> Contoso 會保留 Azure 中應用程式 Vm 的完整控制權。 <br><br> |
| **缺點** | Contoso 必須繼續將應用程式支援為 VMware Vm，而不是將它們移至受控服務，例如 Azure App Service 和 Azure SQL Database。 <br><br> Azure VMware 解決方案是根據最少三個大型節點來布建和定價，而不是 Azure IaaS 中的個別 Vm。 他們必須規劃其容量需求，因為它們目前不在內部部署中，而不是從 Azure 中其他服務的隨選本質獲益。 |

> [!NOTE]
> 深入瞭解 Azure VMware 解決方案的[定價](https://azure.microsoft.com/pricing/details/azure-vmware/)。

## <a name="migration-process"></a>移轉程序

Contoso 會使用 VMware HCX 工具將 Vm 移至 AVS。 Vm 將會在 AVS 私人雲端中執行。  [VMWARE HCX 遷移類型](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-8A31731C-AA28-4714-9C23-D9E924DBB666.html)包括大量遷移、冷遷移，甚至是工作負載是透過使用 Vmotion 或 Replication 協助 VMOTION (RAV) 的即時移轉來執行。

- Contoso 會在 Azure 和 ExpressRoute 中規劃其網路功能。
- Contoso 會使用 Azure 入口網站建立 AVS 私用雲端。
- 接著會設定網路，包括 ExpressRoute 線路。
- 接著，Contoso 會設定 HCX 元件，將其內部部署 vSphere 環境連接到 AVS 私人雲端。
- 然後，Vm 會使用 VMware HCX 複寫並移至 Azure。

 ![移轉程序](./media/contoso-migration-vmware-to-azure/on-premises-vmware-hcx-azure.png)

## <a name="scenarios-steps"></a>案例步驟

1. 網路規劃
1. 建立 AVS 私人雲端
1. 設定網路功能
1. 使用 HCX 遷移 Vm

### <a name="step-1-network-planning"></a>步驟1：網路規劃

Contoso 必須規劃其網路功能，包括內部部署與 Azure 之間的 Azure 虛擬網路和連線能力。 Contoso 必須提供內部部署與 Azure 環境之間的高速連線，以及與 AVS 私人雲端的連線。

此連線會透過 Azure ExpressRoute 傳遞，而且需要一些特定的網路位址範圍和防火牆埠，才能啟用服務。 此高頻寬、低延遲連線可讓 Contoso 的使用者從 AVS 私用雲端環境存取其 Azure 訂用帳戶中執行的服務。

Contoso 將需要規劃 IP 位址配置，其中包含其[虛擬網路](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm)的非重迭位址空間。 他們將需要包含[ExpressRoute 閘道](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways)的閘道子網。

AVS 私人雲端會使用另一個 Azure ExpressRoute 連線，連接到 Contoso 的 Azure 虛擬網路。 將啟用 ExpressRoute Global 延伸，以允許從內部部署[直接](https://docs.microsoft.com/azure/azure-vmware/concepts-networking#on-premises-interconnectivity)連線到在 AVS 私人雲端上執行的 vm。 需要 ExpressRoute Premium SKU 才能實現全球範圍。

![具有 AVS 的 ExpressRoute 全球接觸](./media/contoso-migration-vmware-to-azure/adjacency-overview-drawing-double.png)

AVS 私人雲端至少需要 `/22` 子網的 CIDR 網路位址區塊。 若要連線到內部部署環境和虛擬網路，這必須是非重疊的網路位址區塊。

>[!NOTE]
> 深入瞭解使用[教學](https://docs.microsoft.com/azure/azure-vmware/tutorial-network-checklist/)課程進行 AVS 的網路規劃。

### <a name="step-2-create-an-avs-private-cloud"></a>步驟2：建立 AVS 私人雲端

完成其網路和 IP 位址規劃之後，Contoso 會接著專注于在美國西部 Azure 區域中布建 AVS 服務。 AVS 讓 Contoso 能夠在 Azure 中部署 vSphere 叢集。

AVS 私用雲端是隔離的 VMware 軟體定義資料中心，支援 ESXi 主機、vCenter、vSAN 和 NSX。 堆疊會在 Azure 區域中專用和隔離的裸機硬體節點上執行。 AVS 私用雲端的最低初始部署是三部主機。 您可以一次加入一部額外的主機，每個叢集最多 16 部主機。

>[!NOTE]
> 深入瞭解 AVS[私用雲端的概念](https://docs.microsoft.com/azure/azure-vmware/concepts-private-clouds-clusters)。

AVS 私人雲端是透過 AVS 入口網站來管理。 他們在自己的管理網域中有自己的 vCenter Server。

>[!NOTE]
> 瞭解如何使用[教學](https://docs.microsoft.com/azure/azure-vmware/tutorial-create-private-cloud)課程建立 AVS 私用雲端。

- Contoso 會使用下列命令，先向 Azure 註冊 AVS 提供者。

```bash
az provider register -n Microsoft.AVS --subscription <your subscription ID>
```

- Contoso 會使用 Azure 入口網站來建立 AVS 私用雲端，以提供其計畫的網路資訊。

![建立 AVS 私用雲端](./media/contoso-migration-vmware-to-azure/create-private-cloud.png)

> [!NOTE]
> 這個步驟需要大約 2 小時。

- Contoso 會藉由流覽至資源群組並選取私人雲端資源，來驗證 AVS 私用雲端部署是否已完成。 當部署完成時，的狀態會顯示為 [**成功**]。

![驗證私用雲端部署](./media/contoso-migration-vmware-to-azure/validate-deployment.png)

### <a name="step-3-configure-networking"></a>步驟3：設定網路功能

Azure VMware 解決方案 (AVS) 私人雲端需要虛擬網路。 因為 AVS 不會在預覽期間支援您的內部部署 vCenter，所以需要與內部部署環境整合的其他步驟。 設定 ExpressRoute 線路和虛擬網路閘道會將其虛擬網路連接到 AVS 私人雲端。

>[!NOTE]
> 瞭解如何使用[教學](https://docs.microsoft.com/azure/azure-vmware/tutorial-configure-networking)課程來設定 AVS 的網路功能。

- Contoso 會先建立具有閘道子網的虛擬網路。

> [!IMPORTANT]
> Contoso 必須使用與建立私人雲端時所使用的位址空間**不**重迭的位址空間。

- 接下來，他們會建立 ExpressRoute VPN 閘道，並務必選取正確的 SKU。

![ExpressRoute VPN 閘道](./media/contoso-migration-vmware-to-azure/create-virtual-network-gateway.png)

- 他們需要授權金鑰，才能將 ExpressRoute 連線到虛擬網路。 這可在 Azure 入口網站中的 AVS 私人雲端資源的 [連線能力] 畫面上找到。

![ER 驗證金鑰](./media/contoso-migration-vmware-to-azure/request-auth-key.png)

- 最後，他們會將 ExpressRoute 連線到將 AVS 私用雲端連線到其虛擬網路的 VPN 閘道。 這可透過在 Azure 中建立連線來完成。

![將 ER 連線到 Vnet](./media/contoso-migration-vmware-to-azure/add-connection.png)

>[!NOTE]
> 瞭解如何使用本[教學](https://docs.microsoft.com/azure/azure-vmware/tutorial-access-private-cloud)課程來存取私人雲端並聯機至 vSphere。

### <a name="step-4-migrate-using-vmware-hcx"></a>步驟4：使用 VMware HCX 遷移

若要使用 HCX 將 VMware Vm 移至 Azure，Contoso 必須遵循下列高階步驟：

- 安裝和設定 VMware HCX。
- 使用 HCX 執行至 Azure 的遷移。

>[!NOTE]
> 瞭解如何使用本[教學](https://docs.microsoft.com/azure/azure-vmware/hybrid-cloud-extension-installation)課程來安裝 HCX for AVS。

<!-- docsTest:ignore L2 -->

#### <a name="install-and-configure-vmware-hcx-for-public-cloud"></a>安裝和設定適用于公用雲端的 VMware HCX

[VMWARE HCX](https://cloud.vmware.com/vmware-hcx)是 Azure vmware 解決方案預設安裝的 vmware 產品元件。 預設會安裝 HCX advanced，但可以升級至 HCX enterprise，以取得更多功能的其他功能。 AVS 會自動化您的 HCX 在 AVS 中的 cloud manager 元件，並提供客戶啟用金鑰，以及必須在內部部署端和客戶的 vCenter 網域中設定之連接器 HCX 設備的下載連結。 然後，這些會與 AVS 雲端設備配對，而客戶可以在需要時，開始享用如遷移和 L2 延展等服務。

- Contoso 會使用 VMware 所提供的 OVA 來部署 HCX。

![HCX OVA](./media/contoso-migration-vmware-to-azure/configure-template.png)

如需為您的 AVS 私人雲端設定 HCX，請參閱本指南。

>[!NOTE]
> 瞭解如何使用本[教學](https://docs.microsoft.com/azure/azure-vmware/hybrid-cloud-extension-installation)課程來安裝 HCX for AVS。

- 在設定 HCX 期間，Contoso 已選擇在其他選項中啟用遷移，包括嚴重損壞修復。

![HCX 設定](./media/contoso-migration-vmware-to-azure/hcx-manager.png)

> 注意：深入瞭解[HCX 公用雲端的 HCX 安裝工作流程](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-FDE5473E-6B71-4A71-85B6-8C9BA2B73686.html)。

#### <a name="perform-migrations-to-azure-using-hcx"></a>使用 HCX 執行至 Azure 的遷移

當內部部署資料中心 (來源) 和 AVS 私人雲端 (目的地) 以 VMware cloud 和 HCX 設定時，Contoso 就可以開始遷移 Vm。 您可以使用多個遷移技術，將虛擬機器移入和移出已啟用 VMware HCX 的資料中心。

- Contoso 的 HCX 應用程式已上線，且狀態為綠色。 他們現在已準備好使用 HCX 來遷移和保護 AVS Vm

![HCX 狀態](./media/contoso-migration-vmware-to-azure/appliance-status.png)

#### <a name="vmware-hcx-bulk-migration"></a>VMware HCX 大量遷移

此遷移方法會使用 VMware vSphere 複寫通訊協定，將虛擬機器移至目的地網站。

- 大量遷移選項是針對平行移動虛擬機器而設計的。
- 此遷移類型可設定為 [依預先定義的排程完成]。
- 虛擬機器會在來源網站上執行，直到容錯移轉開始為止。 大量遷移的服務中斷相當於重新開機。

#### <a name="vmware-hcx-vmotion-live-migration"></a>VMware HCX vMotion 即時移轉

這個方法會使用 VMware vMotion 通訊協定，將虛擬機器移至遠端網站。

- VMotion 選項是設計用來一次移動單一虛擬機器。
- 已移動虛擬機器狀態。 VMware HCX vMotion 期間不會有任何服務中斷。

#### <a name="vmware-hcx-cold-migration"></a>VMware HCX 冷遷移

此遷移方法會使用 VMware NFC 通訊協定。 當來源虛擬機器電源關閉時，系統會自動選取此選項。

#### <a name="vmware-hcx-replication-assisted-vmotion"></a>VMware HCX Replication 輔助 vMotion

VMware HCX 複寫協助 vMotion (RAV) 結合 VMware HCX 大量遷移的優點 (平行作業、復原和排程) VMware HCX vMotion (零停機的虛擬機器狀態遷移) 。

> [!IMPORTANT]
> 請參閱 vmware 技術檔中的[VMWARE HCX 檔](https://docs.vmware.com/en/VMware-HCX/index.html)和[遷移具有 vmware HCX 的虛擬機器](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g)。
