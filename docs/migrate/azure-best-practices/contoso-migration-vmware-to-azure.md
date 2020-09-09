---
title: 將內部部署 VMware 基礎結構移至 Azure
description: 瞭解虛構公司 Contoso 如何將內部部署 VMware Vm 移至 Azure。
author: deltadan
ms.author: abuck
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 7921c16e54e3684e5f2ba43555b54b68cd12c37f
ms.sourcegitcommit: 8b82889dca0091f3cc64116f998a3a878943c6a1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/09/2020
ms.locfileid: "89605151"
---
<!-- docsTest:casing "HCX Bulk Migration" "HCX Cold Migration" -->

# <a name="move-on-premises-vmware-infrastructure-to-azure"></a>將內部部署 VMware 基礎結構移至 Azure

當虛構公司 Contoso (Vm) 從內部部署資料中心遷移至 Azure 時，就會有兩個選項可供小組使用。 本文著重于 Azure VMware 解決方案，Contoso 已判定為更好的遷移選項。

| 移轉選項 | 結果 |
| --- | --- |
| [Azure Migrate](https://azure.microsoft.com/services/azure-migrate/) | <li>[評定](/azure/migrate/tutorial-assess-vmware) 及 [遷移](/azure/migrate/tutorial-migrate-vmware) 內部部署 vm。 <li>使用 Azure 基礎結構即服務 (IaaS) 來執行工作負載。 <li>使用 [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)管理 vm。 |
| [Azure VMware 解決方案](https://azure.microsoft.com/overview/azure-vmware) | <li>使用 VMware 混合式雲端擴充 (HCX) 或 vMotion 來移動內部部署 Vm。 <li>在 Azure 裸機硬體上執行原生 VMware 工作負載。 <li>使用 vSphere 管理 Vm。 |

在本文中，Contoso 會使用 Azure VMware 解決方案，以原生存取 VMware vCenter 和 VMware 針對工作負載遷移所支援的其他工具，在 Azure 中建立私人雲端。 Contoso 可以安心地使用 Azure VMware 解決方案，瞭解它是 VMware 所支援的第一方 Microsoft 產品。

## <a name="business-drivers"></a>商業動機

與商務合作夥伴密切合作，Contoso IT 小組定義了 VMware 遷移至 Azure 的商業驅動程式。 這些驅動程式可包含：

- **資料中心撤離或關機**：當 VMware 工作負載合併或淘汰現有的資料中心時，可順暢地移動這些工作負載。
- 嚴重損壞**修復和商務持續性**：使用部署在 Azure 中的 VMware 堆疊作為內部部署資料中心基礎結構的主要或次要隨選災難復原網站。
- **應用程式現代化**：利用 Azure 生態系統將 Contoso 應用程式現代化，而不需要重建 VMware 環境。
- **實施 DevOps**：將 Azure DevOps 工具鏈帶入 VMware 環境，並以自己的步調將應用程式現代化。
- **確保作業持續性**：將 vSphere 型應用程式重新部署至 Azure，同時避免虛擬程式的轉換和應用程式重構。 擴充對執行 Windows 和 SQL Server 之繼承應用程式的支援。

## <a name="goals-for-migrating-vmware-on-premises-to-vmware-in-the-cloud"></a>將 VMware 內部部署遷移至雲端中 VMware 的目標

考慮到其商務驅動程式，Contoso 針對此種遷移已有幾個目標：

- 使用原生 Azure 服務現代化應用程式時，繼續使用其小組熟悉的 VMware 工具來管理其現有環境。
- 將 Contoso VMware 工作負載從其資料中心順暢地移動至 Azure，並整合 VMware 環境與 Azure。
- 在遷移之後，Azure 中的應用程式應該具有與現今在 VMware 中相同的效能功能。 在內部部署環境中，應用程式會保持在雲端中的重要性。

這些目標可支援 Contoso 使用 Azure VMware 解決方案的決策，並將其驗證為最佳的遷移方法。

## <a name="benefits-of-running-vmware-workloads-in-azure"></a>在 Azure 中執行 VMware 工作負載的優點

藉由使用 Azure VMware 解決方案，Contoso 現在可以使用常見的作業架構，順暢地跨 VMware 環境與 Azure 來執行、管理及保護應用程式。

Contoso 將會利用現有的 VMware 投資、技能和工具，包括 VMware vSphere、vSAN 和 vCenter。 同時，Contoso 會取得 Azure 的規模、效能和創新。 此外，它可以：

- 在雲端中設定 VMware 基礎結構（分鐘）。
- 以自己的步調將應用程式現代化。
- 使用專用、隔離、高效能的基礎結構和獨特的 Azure 產品與服務來增強 VMware 應用程式。
- 將內部部署 Vm 移動或擴充至 Azure，而不需要重構其應用程式。
- 為全球 Azure 基礎結構上的 VMware 工作負載取得規模、自動化和快速布建。
- 受益于 Microsoft 所提供、由 VMware 驗證，並在 Azure 基礎結構上執行的解決方案。

## <a name="the-solutions-design"></a>解決方案設計

在 Contoso 釘選其目標和需求之後，公司會設計和審核部署解決方案，並識別遷移程式。

### <a name="current-architecture"></a>目前架構

Contoso 目前的架構功能：

- 部署至 [vSphere](https://www.vmware.com/products/vsphere.html)所管理之內部部署資料中心的 vm。
- 部署在使用 [vCenter](https://www.vmware.com/products/vcenter-server.html)、 [vSAN](https://www.vmware.com/products/vsan.html)和 [NSX](https://www.vmware.com/products/nsx.html)管理的 VMware ESXi 主機叢集上的工作負載。

### <a name="proposed-architecture"></a>建議的架構

在其建議的架構中，Contoso 會：

- 將 [Azure VMware 解決方案私人雲端](/azure/azure-vmware/concepts-private-clouds-clusters) 部署到美國西部 azure 區域。
- 使用已啟用全球存取範圍的虛擬網路和 [ExpressRoute](/azure/azure-vmware/concepts-networking) ，將內部部署資料中心連線到在美國西部執行的 Azure VMware 解決方案。
- 使用 [VMware 混合式雲端擴充功能 (HCX) ](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html)，將 vm 遷移至專屬的 Azure vmware 解決方案。

![建議的架構圖表。](./media/contoso-migration-vmware-to-azure/on-premises-stretched-network-expressroute.png)

## <a name="solution-review"></a>解決方案檢閱

Contoso 會藉由結合優缺點清單來評估其建議的設計，如下表所示：

| 考量 | 詳細資料 |
| --- | --- |
| **優點** | <li>高效能的裸機 VMware 基礎結構。 <li>專為 Contoso 所專用的基礎結構，實際上與其他客戶的基礎結構隔離。 <li>因為 Contoso 使用的是使用 VMware 的重新裝載，所以沒有特殊的設定或遷移複雜度。 <li>Contoso 可以使用舊版 Windows 和 SQL 平臺的 [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/) 和 [延伸安全性更新](https://www.microsoft.com/cloud-platform/windows-server-2008) ，以充分利用軟體保證的投資。 <li>Contoso 會保留 Azure 中應用程式 Vm 的完整控制權。 <br><br> |
| **缺點** | <li>Contoso 必須繼續將應用程式支援為 VMware Vm，而不是將其移至受管理的服務，例如 Azure App Service 和 Azure SQL Database。 <li>Azure VMware 解決方案的設定與定價最少三個大型節點，而不是根據 Azure IaaS 中的個別 Vm。 Contoso 將需要規劃其容量需求，因為公司目前使用的內部部署環境會限制它免于 Azure 中其他服務的隨選本質。 |

> [!NOTE]
> 如需定價的詳細資訊，請參閱 [Azure VMware 解決方案定價](https://azure.microsoft.com/pricing/details/azure-vmware/)。

## <a name="migration-process"></a>移轉程序

Contoso 會使用 VMware HCX 工具將其 Vm 移至 Azure VMware 解決方案。 Vm 將在 Azure VMware 解決方案私人雲端中執行。 [VMWARE HCX 遷移方法](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-8A31731C-AA28-4714-9C23-D9E924DBB666.html) 包括執行大量或冷遷移。 VMware vMotion 或複寫輔助的 vMotion (RAV) 是一種方法，可針對透過即時移轉執行的工作負載進行保留。

若要完成此程式，Contoso 小組：

- 規劃其在 Azure 和 ExpressRoute 中的網路功能。
- 使用 Azure 入口網站建立 Azure VMware 解決方案私人雲端。
- 設定網路以包含 ExpressRoute 線路。
- 設定 HCX 元件，以將其內部部署 vSphere 環境連接到 Azure VMware 解決方案私人雲端。
- 複寫 Vm，然後使用 VMware HCX 將其移至 Azure。

 ![遷移程式的圖表。](./media/contoso-migration-vmware-to-azure/on-premises-vmware-hcx-azure.png)

## <a name="scenarios-steps"></a>案例步驟

> [!div class="checklist"]
>
> - **步驟1：網路規劃**
> - **步驟2：建立 Azure VMware 解決方案私人雲端**
> - **步驟3：設定網路功能**
> - **步驟4：使用 HCX 遷移 Vm**

### <a name="step-1-network-planning"></a>步驟1：網路規劃

Contoso 需要規劃其網路功能，以包含 Azure 虛擬網路和內部部署與 Azure 之間的連線能力。 公司需要在其內部部署與 Azure 環境之間提供高速連線，以及 Azure VMware 解決方案私人雲端的連線。

此連線會透過 Azure ExpressRoute 傳遞，並需要一些特定的網路位址範圍和防火牆埠才能啟用服務。 這個高頻寬、低延遲的連線可讓 Contoso 從 Azure VMware 解決方案私用雲端環境存取其 Azure 訂用帳戶中執行的服務。

Contoso 將需要規劃 IP 位址配置，其中包含其 [虛擬網路](/azure/virtual-network/virtual-network-vnet-plan-design-arm)的非重迭位址空間。 該公司必須包含 [ExpressRoute 閘道](/azure/expressroute/expressroute-about-virtual-network-gateways)的閘道子網。

Azure VMware 解決方案私人雲端會使用另一個 Azure ExpressRoute 連線連接到 Contoso 的 Azure 虛擬網路。 將會啟用 ExpressRoute 全球存取範圍，以允許從內部部署 Vm [直接](/azure/azure-vmware/concepts-networking#on-premises-interconnectivity) 連線至在 Azure VMware 解決方案私人雲端上執行的 vm。 需要 ExpressRoute Premium SKU，才能實現全球存取範圍。

![Azure VMware 解決方案的 ExpressRoute Global 觸及圖。](./media/contoso-migration-vmware-to-azure/adjacency-overview-drawing-double.png)

Azure VMware 解決方案私人雲端至少需要 `/22` 子網的 CIDR 網路位址區塊。 若要連接到內部部署環境和虛擬網路，這必須是不重迭的網路位址區塊。

>[!NOTE]
> 若要瞭解 Azure VMware 解決方案的網路規劃，請參閱 [Azure Vmware 解決方案的網路檢查清單](/azure/azure-vmware/tutorial-network-checklist/)。

### <a name="step-2-create-an-azure-vmware-solution-private-cloud"></a>步驟2：建立 Azure VMware 解決方案私人雲端

在完成其網路和 IP 位址的規劃後，Contoso 會接著著重于在美國西部 Azure 區域中設定 Azure VMware 解決方案服務。 藉由使用 Azure VMware 解決方案，Contoso 可以在 Azure 中部署 vSphere 叢集。

Azure VMware 解決方案私用雲端是隔離的 VMware 軟體定義資料中心，支援 ESXi 主機、vCenter、vSAN 和 NSX。 此堆疊會在 Azure 區域中專用且獨立的裸機硬體節點上執行。 Azure VMware 解決方案私用雲端的最低初始部署是三部主機。 您可以一次加入一部額外的主機，每個叢集最多 16 部主機。

如需詳細資訊，請參閱 [Azure VMware 解決方案預覽版私用雲端和叢集概念](/azure/azure-vmware/concepts-private-clouds-clusters)。

Azure VMware 解決方案私用雲端是透過 Azure VMware 解決方案入口網站來管理。 Contoso 在自己的管理網域中有自己的 vCenter server。

若要瞭解如何建立 Azure VMware 解決方案私人雲端，請參閱 [在 azure 中部署 Azure Vmware 解決方案私人雲端](/azure/azure-vmware/tutorial-create-private-cloud)。

1. Contoso 小組會先藉由執行下列命令，向 Azure 註冊 Azure VMware 解決方案提供者：

    ```bash
    az provider register -n Microsoft.AVS --subscription <your subscription ID>
    ```

1. 在 Azure 入口網站中，小組會藉由提供方案中的網路資訊來建立 Azure VMware 解決方案私人雲端。 小組接著選取 [ **審核 + 建立**]。 此步驟大約需要兩個小時。

    ![用來建立 Azure VMware 解決方案私人雲端的 [Azure 入口網站] 窗格螢幕擷取畫面。](./media/contoso-migration-vmware-to-azure/create-private-cloud.png)

1. 小組會藉由前往資源群組並選取私用雲端資源，來驗證 Azure VMware 解決方案私人雲端部署是否已完成。 狀態會顯示為 [ *成功*]。

    ![[Contoso Azure VMware 解決方案私人雲端] 頁面的螢幕擷取畫面，其中顯示部署成功。](./media/contoso-migration-vmware-to-azure/validate-deployment.png)

### <a name="step-3-configure-networking"></a>步驟3：設定網路功能

Azure VMware 解決方案私用雲端需要虛擬網路。 因為 Azure VMware 解決方案在預覽期間不支援內部部署 vCenter，所以 Contoso 需要額外的步驟來與其內部部署環境整合。 藉由設定 ExpressRoute 線路和虛擬網路閘道，小組會將其虛擬網路連接到 Azure VMware 解決方案私人雲端。

如需詳細資訊，請參閱 [在 Azure 中設定 VMware 私用雲端的網路](/azure/azure-vmware/tutorial-configure-networking)功能。

1. Contoso 小組會先建立具有閘道子網的虛擬網路。

    > [!IMPORTANT]
    > 小組必須使用與建立私人雲端時所用的位址空間 *不* 重迭的位址空間。

1. 小組會建立 ExpressRoute VPN 閘道，請務必選取正確的 SKU，然後選取 [ **審核 + 建立**]。

    ![[建立虛擬網路閘道] 窗格的螢幕擷取畫面。](./media/contoso-migration-vmware-to-azure/create-virtual-network-gateway.png)

1. 小組會取得授權金鑰，以將 ExpressRoute 連線至虛擬網路。 您可以在 Azure 入口網站的 Azure VMware 解決方案私人雲端資源的 [連線能力] 畫面上找到此金鑰。

    ![[Contoso Azure VMware 解決方案私人雲端連線] 窗格上 [ExpressRoute] 索引標籤的螢幕擷取畫面。](./media/contoso-migration-vmware-to-azure/request-auth-key.png)

1. 小組將 ExpressRoute 連線至 VPN 閘道，以將 Azure VMware 解決方案私人雲端連接到 Contoso 虛擬網路。 它會藉由在 Azure 中建立連接來執行此工作。

    ![將 ExpressRoute 連接至虛擬網路的 [新增連線] 窗格螢幕擷取畫面。](./media/contoso-migration-vmware-to-azure/add-connection.png)

如需詳細資訊，請參閱 [瞭解如何存取 Azure VMware 解決方案私人雲端](/azure/azure-vmware/tutorial-access-private-cloud)。

### <a name="step-4-migrate-by-using-vmware-hcx"></a>步驟4：使用 VMware HCX 進行遷移

若要使用 HCX 將 VMware Vm 移至 Azure，Contoso 團隊將需要遵循下列高階步驟：

- 安裝和設定 VMware HCX。
- 使用 HCX 執行遷移至 Azure。

如需詳細資訊，請參閱 [安裝 HCX For Azure VMware Solution](/azure/azure-vmware/hybrid-cloud-extension-installation)。

<!-- docsTest:casing L2 -->

#### <a name="install-and-configure-vmware-hcx-for-the-public-cloud"></a>安裝和設定適用于公用雲端的 VMware HCX

[VMWARE HCX](https://cloud.vmware.com/vmware-hcx) 是 vmware 產品，屬於 Azure vmware 解決方案預設安裝的一部分。 預設會安裝 HCX Advanced，但它可以升級為 HCX Enterprise，因為需要額外的功能和功能。

Azure VMware 解決方案會將 Azure VMware 解決方案中 HCX 的雲端管理員元件自動化。 它會提供客戶啟用金鑰，並下載連接器 HCX 設備的連結，該應用裝置必須在內部部署端和客戶的 vCenter 網域中設定。 然後，這些元素會與 Azure VMware 解決方案雲端設備配對，讓客戶可以利用諸如遷移和 L2 延展等服務。

- Contoso 團隊將使用 VMware 提供的 OVF 套件來部署 HCX。

   ![[部署 OVF 範本] 視窗的螢幕擷取畫面。](./media/contoso-migration-vmware-to-azure/configure-template.png)

   若要安裝及設定 Azure VMware 解決方案私人雲端的 HCX，請參閱 [安裝 HCX For Azure Vmware solution](/azure/azure-vmware/hybrid-cloud-extension-installation)。

- 小組在設定 HCX 時，選擇啟用遷移和其他選項，包括嚴重損壞修復。

   ![用於設定 HCX 的 [建立服務網格] 視窗的螢幕擷取畫面。](./media/contoso-migration-vmware-to-azure/hcx-manager.png)

   如需詳細資訊，請參閱 [HCX 公用雲端的 HCX 安裝工作流程](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-FDE5473E-6B71-4A71-85B6-8C9BA2B73686.html)。

#### <a name="migrate-vms-to-azure-by-using-hcx"></a>使用 HCX 將 Vm 遷移至 Azure

當內部部署資料中心 (來源) 和 Azure VMware 解決方案私人雲端 (目的地) 都是使用 VMware 雲端和 HCX 來設定時，Contoso 就可以開始遷移其 Vm。 小組可以使用多個遷移技術，將 Vm 移入和移出已啟用 VMware HCX 的資料中心。

- Contoso 的 HCX 應用程式已上線，且狀態為綠色。 小組現在已準備好使用 HCX 來遷移及保護 Azure VMware 解決方案 Vm。

  ![VMware vSphere Web 用戶端頁面的螢幕擷取畫面。](./media/contoso-migration-vmware-to-azure/appliance-status.png)

#### <a name="vmware-hcx-bulk-migration"></a>VMware HCX 大量遷移

此遷移方法會使用 VMware vSphere 的複寫通訊協定，將多個 Vm 同時移至目的地網站。 好處包括：

- 此方法是設計來平行移動多個 Vm。
- 您可以將遷移設定為在預先定義的排程上完成。
- Vm 會在來源網站執行，直到容錯移轉開始為止。 服務中斷相當於重新開機。

#### <a name="vmware-hcx-vmotion-live-migration"></a>VMware HCX vMotion 即時移轉

此遷移方法會使用 VMware vMotion 通訊協定，將單一 VM 移至遠端網站。 好處包括：

- 此方法的設計是一次移動一個 VM。
- 當 VM 狀態遷移時，不會中斷服務。

#### <a name="vmware-hcx-cold-migration"></a>VMware HCX 冷遷移

此遷移方法會使用 VMware 近乎現場的通訊協定。 當來源 VM 電源關閉時，會自動選取此選項。

#### <a name="vmware-hcx-replication-assisted-vmotion"></a>VMware HCX 複寫-輔助 vMotion

VMware HCX RAV 結合 VMware HCX 大量遷移的優點，包括平行作業、復原和排程，以及 VMware HCX vMotion 遷移的優點，包括 VM 狀態遷移期間的零停機時間。

## <a name="additional-resources"></a>其他資源

如需其他 VMware 技術檔，請參閱：

- [VMware HCX 檔](https://docs.vmware.com/en/VMware-HCX/index.html)
- [使用 VMware HCX 來遷移虛擬機器](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-D0CD0CC6-3802-42C9-9718-6DA5FEC246C6.html?hWord=N4IghgNiBcIBIGEAaACAtgSwOYCcwBcMB7AOxAF8g)
