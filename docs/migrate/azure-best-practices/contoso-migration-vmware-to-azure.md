---
title: 將內部部署 VMware 基礎結構移至 Azure
description: 瞭解虛構公司 Contoso 如何將內部部署 VMware Vm 移至 Azure。
author: deltadan
ms.author: abuck
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: azure-migrate
ms.openlocfilehash: b19e0796a293563a13ece3c7690387a7171565fd
ms.sourcegitcommit: 949b87bad28d32df84df190160089f01826f3a31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2020
ms.locfileid: "88193107"
---
<!-- docsTest:ignore "Bulk Migration" "Cold Migration" -->

# <a name="move-on-premises-vmware-infrastructure-to-azure"></a>將內部部署 VMware 基礎結構移至 Azure

當虛構公司 Contoso 將其 VMware 虛擬機器 (Vm) 從內部部署資料中心遷移至 Azure 時，小組會提供兩個選項。 本文著重于 Azure VMware 解決方案，Contoso 認為這是更好的遷移選項。

| 移轉選項 | 結果 |
| --- | --- |
| [Azure Migrate](https://azure.microsoft.com/services/azure-migrate/) | <li>[評估](https://docs.microsoft.com/azure/migrate/tutorial-assess-vmware) 和 [遷移](https://docs.microsoft.com/azure/migrate/tutorial-migrate-vmware) 內部部署 vm。 <li>使用 Azure 基礎結構即服務 (IaaS) 來執行工作負載。 <li>使用 [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)管理 vm。 |
| [Azure VMware 解決方案](https://azure.microsoft.com/overview/azure-vmware) | <li>使用 VMware 混合式雲端擴充功能 (HCX) 或 vMotion 來移動內部部署 Vm。 <li>在 Azure 裸機硬體上執行原生 VMware 工作負載。 <li>使用 vSphere 管理 Vm。 |

在本文中，Contoso 會使用 Azure VMware 解決方案在 Azure 中建立私用雲端，以原生方式存取 VMware vCenter 和 VMware 支援的其他工具，以進行工作負載遷移。 Contoso 可以安心地使用 Azure VMware 解決方案，並瞭解它是 VMware 支援的第一方 Microsoft 產品。

## <a name="business-drivers"></a>商業動機

與商務合作夥伴密切合作，Contoso IT 小組定義了 VMware 遷移至 Azure 的商業驅動程式。 這些驅動程式可能包括：

- **Datacenter 撤離或 shutdown**：在整合或淘汰現有的資料中心時，順暢地移動 VMware 工作負載。
- 嚴重損壞**修復和商務持續性**：使用在 Azure 中部署的 VMware 堆疊，作為內部部署資料中心基礎結構的主要或次要隨選災難復原網站。
- **應用程式現代化**：利用 Azure 生態系統將 Contoso 應用程式現代化，而不需要重建以 VMware 為基礎的環境。
- 實行**DevOps**：帶入 VMware 環境的 Azure DevOps 工具鏈，並以自己的步調現代化應用程式。
- **確保操作持續性**：將 vSphere 型應用程式重新部署至 Azure，同時避免執行程式管理和應用程式重構。 擴充對執行 Windows 和 SQL Server 的繼承應用程式的支援。

## <a name="goals-for-migrating-vmware-on-premises-to-vmware-in-the-cloud"></a>將 VMware 內部部署遷移至雲端中 VMware 的目標

在考慮到其商業驅動程式之後，Contoso 已針對這項遷移的幾個目標進行了以下的鎖定：

- 使用其小組熟悉的 VMware 工具繼續管理其現有的環境，同時使用原生 Azure 服務現代化應用程式。
- 將 Contoso VMware 工作負載從其資料中心順暢地移至 Azure，並將 VMware 環境與 Azure 整合。
- 在遷移之後，Azure 中的應用程式應該具有與目前在 VMware 中相同的效能功能。 在雲端中，應用程式在內部部署時仍會保持嚴重的重要性。

這些目標可支援 Contoso 決定使用 Azure VMware 解決方案，並將其驗證為最佳的遷移方法。

## <a name="benefits-of-running-vmware-workloads-in-azure"></a>在 Azure 中執行 VMware 工作負載的優點

藉由使用 Azure VMware 解決方案，Contoso 現在可以使用通用的作業架構，順暢地跨 VMware 環境和 Azure 來執行、管理及保護應用程式。

Contoso 會以現有的 VMware 投資、技能和工具（包括 VMware vSphere、vSAN 和 vCenter）作為資本。 同時，Contoso 也會取得 Azure 的規模、效能和創新。 此外，它可以：

- 幾分鐘內就能在雲端設定 VMware 基礎結構。
- 以自己的步調現代化應用程式。
- 使用專用、隔離、高效能的基礎結構和獨特的 Azure 產品和服務，增強 VMware 應用程式。
- 將內部部署 Vm 移動或延伸至 Azure，而不需要重構其應用程式。
- 針對全球 Azure 基礎結構上的 VMware 工作負載，取得規模、自動化和快速布建。
- 受益于由 Microsoft 提供的解決方案、由 VMware 驗證，並在 Azure 基礎結構上執行。

## <a name="the-solutions-design"></a>解決方案設計

在 Contoso 關閉其目標和需求之後，公司會設計和審查部署解決方案，並識別遷移程式。

### <a name="current-architecture"></a>目前架構

Contoso 目前的架構功能：

- 部署到 [vSphere](https://www.vmware.com/products/vsphere.html)所管理之內部部署資料中心的 vm。
- 在以 [vCenter](https://www.vmware.com/products/vcenter-server.html)、 [vSAN](https://www.vmware.com/products/vsan.html)和 [NSX](https://www.vmware.com/products/nsx.html)管理的 VMware ESXi 主機叢集上部署的工作負載。

### <a name="proposed-architecture"></a>建議的架構

在其提議的架構中，Contoso 將會：

- 將 [Azure VMware 解決方案私用雲端](https://docs.microsoft.com/azure/azure-vmware/concepts-private-clouds-clusters) 部署到美國西部 Azure 區域。
- 將內部部署資料中心連接到在美國西部執行的 Azure VMware 解決方案，方法是使用虛擬網路，並啟用全球範圍的 [ExpressRoute](https://docs.microsoft.com/azure/azure-vmware/concepts-networking) 。
- 使用 [VMware 混合式雲端擴充功能 (HCX) ](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html)，將 vm 遷移至專用的 Azure VMware 解決方案。

![提議架構的圖表。](./media/contoso-migration-vmware-to-azure/on-premises-stretched-network-expressroute.png)

## <a name="solution-review"></a>解決方案檢閱

Contoso 會藉由將 [優缺點] 清單放在一起，來評估其提議的設計，如下表所示：

| 考量 | 詳細資料 |
| --- | --- |
| **優點** | <li>高效能的裸機 VMware 基礎結構。 <li>完全專用於 Contoso 的基礎結構，實際上與其他客戶的基礎結構隔離。 <li>因為 Contoso 使用的是使用 VMware 的重新裝載，所以沒有任何特殊設定或遷移的複雜性。 <li>Contoso 可以使用舊版 Windows 和 SQL 平臺的 [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/) 和 [擴充安全性更新](https://www.microsoft.com/cloud-platform/windows-server-2008) ，來充分利用軟體保證中的投資。 <li>Contoso 會保留 Azure 中應用程式 Vm 的完整控制權。 <br><br> |
| **缺點** | <li>Contoso 必須繼續將應用程式支援為 VMware Vm，而不是將它們移至受控服務，例如 Azure App Service 和 Azure SQL Database。 <li>Azure VMware 解決方案是根據最少三個大型節點，而不是 Azure IaaS 中的個別 Vm 所設定和定價。 Contoso 必須規劃其容量需求，因為該公司目前使用的內部部署環境會將它限制為 Azure 中其他服務的隨選本質。 |

> [!NOTE]
> 如需價格的相關資訊，請參閱 [Azure VMware 解決方案價格](https://azure.microsoft.com/pricing/details/azure-vmware/)。

## <a name="migration-process"></a>移轉程序

Contoso 會使用 VMware HCX 工具將其 Vm 移至 Azure VMware 解決方案。 Vm 將會在 Azure VMware 解決方案私人雲端中執行。 [VMWARE HCX 遷移方法](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-8A31731C-AA28-4714-9C23-D9E924DBB666.html) 包括執行大量或冷遷移。 vMotion 或複寫輔助的 vMotion (RAV) 是保留給透過即時移轉執行之工作負載的方法。

若要完成此程式，Contoso 小組：

- 規劃其在 Azure 和 ExpressRoute 中的網路功能。
- 使用 Azure 入口網站建立 Azure VMware 解決方案私用雲端。
- 設定網路以包含 ExpressRoute 線路。
- 設定 HCX 元件，以將其內部部署 vSphere 環境連接到 Azure VMware 解決方案私用雲端。
- 使用 VMware HCX 複寫 Vm，然後將它們移至 Azure。

 ![遷移程式的圖表。](./media/contoso-migration-vmware-to-azure/on-premises-vmware-hcx-azure.png)

## <a name="scenarios-steps"></a>案例步驟

> [!div class="checklist"]
>
> - **步驟1：網路規劃**
> - **步驟2：建立 Azure VMware 解決方案私用雲端**
> - **步驟3：設定網路功能**
> - **步驟4：使用 HCX 遷移 Vm**

### <a name="step-1-network-planning"></a>步驟1：網路規劃

Contoso 必須規劃其網路功能，以包含內部部署與 Azure 之間的 Azure 虛擬網路和連線能力。 公司需要提供其內部部署與 Azure 環境之間的高速連線，以及與 Azure VMware 解決方案私用雲端的連線。

此連線會透過 Azure ExpressRoute 傳遞，而且需要一些特定的網路位址範圍和防火牆埠，才能啟用服務。 此高頻寬、低延遲的連線可讓 Contoso 從 Azure VMware 解決方案私用雲端環境存取在其 Azure 訂用帳戶中執行的服務。

Contoso 將需要規劃 IP 位址配置，其中包含其 [虛擬網路](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm)的非重迭位址空間。 該公司必須包含 [ExpressRoute 閘道](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways)的閘道子網。

Azure VMware 解決方案私用雲端會使用另一個 Azure ExpressRoute 連線來連接到 Contoso 的 Azure 虛擬網路。 將啟用 ExpressRoute 全球範圍，以允許從內部部署 Vm [直接](https://docs.microsoft.com/azure/azure-vmware/concepts-networking#on-premises-interconnectivity) 連線到在 Azure VMware 解決方案私人雲端上執行的 vm。 需要 ExpressRoute Premium SKU 才能實現全球範圍。

![使用 Azure VMware 解決方案的 ExpressRoute 全球化圖表。](./media/contoso-migration-vmware-to-azure/adjacency-overview-drawing-double.png)

Azure VMware 解決方案私人雲端至少需要 `/22` 子網的 CIDR 網路位址區塊。 若要連接到內部部署環境和虛擬網路，這必須是非重迭的網路位址區塊。

>[!NOTE]
> 若要瞭解 Azure VMware 解決方案的網路規劃，請參閱 [Azure Vmware 解決方案的網路功能檢查清單](https://docs.microsoft.com/azure/azure-vmware/tutorial-network-checklist/)。

### <a name="step-2-create-an-azure-vmware-solution-private-cloud"></a>步驟2：建立 Azure VMware 解決方案私用雲端

完成其網路和 IP 位址規劃之後，Contoso 會接著專注于在「美國西部」 Azure 區域中設定 Azure VMware 解決方案服務。 藉由使用 Azure VMware 解決方案，Contoso 可以在 Azure 中部署 vSphere 叢集。

Azure VMware 解決方案私用雲端是獨立的 VMware 軟體定義資料中心，支援 ESXi 主機、vCenter、vSAN 和 NSX。 堆疊會在 Azure 區域中專用和隔離的裸機硬體節點上執行。 Azure VMware 解決方案私用雲端的最低初始部署是三部主機。 您可以一次加入一部額外的主機，每個叢集最多 16 部主機。

如需詳細資訊，請參閱 [Azure VMware 解決方案預覽私人雲端和叢集概念](https://docs.microsoft.com/azure/azure-vmware/concepts-private-clouds-clusters)。

Azure VMware 解決方案私用雲端是透過 Azure VMware 解決方案入口網站來管理。 Contoso 在自己的管理網域中有自己的 vCenter server。

若要瞭解如何建立 Azure VMware 解決方案私用雲端，請參閱 [在 azure 中部署 Azure Vmware 解決方案私用雲端](https://docs.microsoft.com/azure/azure-vmware/tutorial-create-private-cloud)。

1. Contoso 小組會先執行下列命令，向 Azure 註冊 Azure VMware 解決方案提供者：

    ```bash
    az provider register -n Microsoft.AVS --subscription <your subscription ID>
    ```

1. 在 Azure 入口網站中，小組會藉由提供方案的網路資訊來建立 Azure VMware 解決方案私用雲端。 然後，小組會選取 [審核] 和 [ **建立**]。 此步驟大約需要兩小時的時間。

    ![[Azure 入口網站] 窗格的螢幕擷取畫面，用來建立 Azure VMware 解決方案私用雲端。](./media/contoso-migration-vmware-to-azure/create-private-cloud.png)

1. 小組會藉由前往資源群組並選取私人雲端資源，來驗證 Azure VMware 解決方案私用雲端部署是否已完成。 狀態會顯示為 [ *成功*]。

    ![[Contoso Azure VMware 解決方案私人雲端] 頁面的螢幕擷取畫面，其中顯示部署成功。](./media/contoso-migration-vmware-to-azure/validate-deployment.png)

### <a name="step-3-configure-networking"></a>步驟3：設定網路功能

Azure VMware 解決方案私用雲端需要虛擬網路。 因為 Azure VMware 解決方案在預覽期間不支援內部部署 vCenter，所以 Contoso 需要額外的步驟，才能整合其內部部署環境。 藉由設定 ExpressRoute 線路和虛擬網路閘道，小組會將其虛擬網路連接至 Azure VMware 解決方案私人雲端。

如需詳細資訊，請參閱 [在 Azure 中設定 VMware 私人雲端的網路](https://docs.microsoft.com/azure/azure-vmware/tutorial-configure-networking)功能。

1. Contoso 小組會先建立具有閘道子網的虛擬網路。

    > [!IMPORTANT]
    > 小組必須使用與建立私人雲端時所使用的位址空間 *不* 重迭的位址空間。

1. 小組會建立 ExpressRoute VPN 閘道，請務必選取正確的 SKU，然後選取 [ **審查 + 建立**]。

    ![[建立虛擬網路閘道] 窗格的螢幕擷取畫面。](./media/contoso-migration-vmware-to-azure/create-virtual-network-gateway.png)

1. 小組會取得授權金鑰，以將 ExpressRoute 連線到虛擬網路。 您可在 Azure 入口網站的 Azure VMware 解決方案私人雲端資源的 [連線能力] 畫面上找到此金鑰。

    ![[Contoso Azure VMware 解決方案] [私人雲端連線] 窗格上 [ExpressRoute] 索引標籤的螢幕擷取畫面。](./media/contoso-migration-vmware-to-azure/request-auth-key.png)

1. 小組會將 ExpressRoute 連線至 VPN 閘道，以將 Azure VMware 解決方案私人雲端連接到 Contoso 虛擬網路。 它會在 Azure 中建立連線來完成這項工作。

    ![[新增連線] 窗格的螢幕擷取畫面，可將 ExpressRoute 連接至虛擬網路。](./media/contoso-migration-vmware-to-azure/add-connection.png)

如需詳細資訊，請參閱 [瞭解如何存取 Azure VMware 解決方案私用雲端](https://docs.microsoft.com/azure/azure-vmware/tutorial-access-private-cloud)。

### <a name="step-4-migrate-by-using-vmware-hcx"></a>步驟4：使用 VMware HCX 進行遷移

若要使用 HCX 將 VMware Vm 移至 Azure，Contoso 小組必須遵循下列高階步驟：

- 安裝和設定 VMware HCX。
- 使用 HCX 執行至 Azure 的遷移。

如需詳細資訊，請參閱 [INSTALL HCX For Azure VMware Solution](https://docs.microsoft.com/azure/azure-vmware/hybrid-cloud-extension-installation)。

<!-- docsTest:ignore L2 -->

#### <a name="install-and-configure-vmware-hcx-for-the-public-cloud"></a>安裝和設定適用于公用雲端的 VMware HCX

[VMWARE HCX](https://cloud.vmware.com/vmware-hcx) 是 vmware 產品，屬於 Azure vmware 解決方案預設安裝的一部分。 預設會安裝 HCX Advanced，但它可以升級至 HCX Enterprise，因為需要額外的功能和功能。

Azure VMware 解決方案會自動化 Azure VMware 解決方案中 HCX 的雲端管理員元件。 它會針對必須在內部部署端和客戶 vCenter 網域中設定的連接器 HCX 設備，提供客戶啟用金鑰和下載連結。 這些元素接著會與 Azure VMware 解決方案雲端設備配對，讓客戶可以利用像是遷移和 L2 延展等服務。

- Contoso 小組會使用 VMware 提供的 OVF 套件來部署 HCX。

   ![[部署 OVF 範本] 視窗的螢幕擷取畫面。](./media/contoso-migration-vmware-to-azure/configure-template.png)

   若要為您的 Azure VMware 解決方案私人雲端安裝和設定 HCX，請參閱 [INSTALL HCX For Azure Vmware solution](https://docs.microsoft.com/azure/azure-vmware/hybrid-cloud-extension-installation)。

- 當小組設定 HCX 時，它已選擇啟用遷移和其他選項，包括損毀修復。

   ![用來設定 HCX 的 [建立服務網格] 視窗的螢幕擷取畫面。](./media/contoso-migration-vmware-to-azure/hcx-manager.png)

   如需詳細資訊，請參閱 [HCX 公用雲端的 HCX 安裝工作流程](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-FDE5473E-6B71-4A71-85B6-8C9BA2B73686.html)。

#### <a name="migrate-vms-to-azure-by-using-hcx"></a>使用 HCX 將 Vm 遷移至 Azure

當內部部署資料中心 (來源) 和 Azure VMware 解決方案私用雲端 (目的地) 已設定 VMware 雲端和 HCX 時，Contoso 就可以開始遷移其 Vm。 小組可以藉由使用多個遷移技術，將 Vm 移入和移出已啟用 VMware HCX 的資料中心。

- Contoso 的 HCX 應用程式已上線，且狀態為綠色。 小組現在已準備好使用 HCX 來遷移和保護 Azure VMware 解決方案 Vm。

  ![[VMware vSphere Web 用戶端] 頁面的螢幕擷取畫面。](./media/contoso-migration-vmware-to-azure/appliance-status.png)

#### <a name="vmware-hcx-bulk-migration"></a>VMware HCX 大量遷移

此遷移方法會使用 VMware vSphere 複寫通訊協定，同時將多個 Vm 同時移至目的地網站。 好處包括：

- 這個方法是設計來平行移動多個 Vm。
- 您可以將遷移設定為根據預先定義的排程來完成。
- Vm 會在來源網站上執行，直到容錯移轉開始為止。 服務中斷相當於重新開機。

#### <a name="vmware-hcx-vmotion-live-migration"></a>VMware HCX vMotion 即時移轉

此遷移方法會使用 VMware vMotion 通訊協定，將單一 VM 移至遠端網站。 好處包括：

- 這個方法是設計來一次移動一個 VM。
- 遷移 VM 狀態時，不會中斷服務。

#### <a name="vmware-hcx-cold-migration"></a>VMware HCX 冷遷移

此遷移方法會使用 VMware 近距離的通訊協定。 當來源 VM 電源關閉時，會自動選取此選項。

#### <a name="vmware-hcx-replication-assisted-vmotion"></a>VMware HCX 複寫-輔助 vMotion

VMware HCX RAV 結合了 VMware HCX 大量遷移的優點，包括平行作業、復原和排程，以及 VMware HCX vMotion 遷移的優點，這在 VM 狀態遷移期間不會有任何停機時間。

## <a name="additional-resources"></a>其他資源

如需其他 VMware 技術檔，請參閱：

- [VMware HCX 檔](https://docs.vmware.com/en/VMware-HCX/index.html)
- [使用 VMware HCX 遷移虛擬機器](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g)
