---
title: 藉由將應用程式遷移至 Azure Vm 和 SQL Server 來進行重新裝載 Always On 可用性群組
description: 了解 Contoso 如何將內部部署應用程式移轉至 Azure VM 和 SQL Server Always On 可用性群組，以重新裝載該應用程式。
author: deltadan
ms.author: abuck
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: azure-migrate
ms.openlocfilehash: fe6ce9e2406901779f963fb7c6eb6f0aa29c662e
ms.sourcegitcommit: 26aee3c6f596bb8a9f1e16af93cdf94e41a61dee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2020
ms.locfileid: "87400542"
---
<!-- cSpell:ignore WEBVM SQLVM contosohost vcenter contosodc AOAG SQLAOG SQLAOGAVSET contosoadmin contosocloudwitness MSSQLSERVER BEPOOL contosovmsacc SHAOG NSGs inetpub iisreset -->

# <a name="rehost-an-on-premises-application-with-azure-vms-and-sql-server-always-on-availability-groups"></a>使用 Azure Vm 和 SQL Server 重新裝載內部部署應用程式 Always On 可用性群組

本文示範虛構公司 Contoso 如何在遷移至 Azure 的過程中，重新裝載在 VMware 虛擬機器（Vm）上執行的兩層式 Windows .NET 應用程式。 Contoso 會將應用程式前端 VM 遷移至 Azure VM，並將應用程式資料庫移轉至 Azure SQL Server VM，並在具有 SQL Server Always On 可用性群組的 Windows Server 容錯移轉叢集中執行。

此範例中使用的 SmartHotel360 應用程式是以開放原始碼的形式提供。 如果您想要將它用於自己的測試目的，請從[GitHub](https://github.com/Microsoft/SmartHotel360)下載。

## <a name="business-drivers"></a>商業動機

IT 領導小組與商務合作夥伴密切合作，以瞭解他們想要透過此遷移達成的目標。 他們想要：

- **解決業務成長。** Contoso 正在成長，因此對內部部署系統和基礎結構造成了壓力。
- **提高效率。** Contoso 必須移除不必要的程式，並簡化開發人員和使用者的流程。 企業需要快速且不浪費時間或金錢，以更快的速度提供客戶需求。
- **增加靈活性。** Contoso IT 必須能夠更快因應企業的需求。 其回應速度必須比 marketplace 中的變更更快，才能在全球經濟中實現成功。 它會不得取得或成為商務封鎖程式。
- **尺度.** 隨著企業順利成長，Contoso IT 必須提供以相同步調成長的系統。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已針對此次移轉擬定好各項目標。 這些目標用來判斷最合適的移轉方法：

- 在遷移之後，Azure 中的應用程式應該具有與目前在 VMware 中相同的效能功能。 在雲端中，應用程式在內部部署環境中將維持不變。
- Contoso 不想要投資此應用程式。 這對企業很重要，但以其目前的形式而言，Contoso 只是想要安全地將它移到雲端。
- 應用程式的內部部署資料庫有可用性問題。 Contoso 想要將它部署在 Azure 中，作為具有容錯移轉功能的高可用性叢集。
- Contoso 想要從其目前的 SQL Server 2008 R2 平臺升級至 SQL Server 2017。
- Contoso 不想要使用此應用程式的 Azure SQL Database，而且正在尋找替代方案。

## <a name="solution-design"></a>解決方案設計

完成公司的目標和需求後，Contoso 會設計和審查部署解決方案，並識別遷移程式。 也會識別用於遷移的 Azure 服務。

### <a name="current-architecture"></a>目前架構

- 應用程式會跨兩個 Vm （ `WEBVM` 和 `SQLVM` ）進行分層。
- VM 位於 VMware ESXi 主機 `contosohost1.contoso.com`(6.5 版)。
- VMware 環境是由 `vcenter.contoso.com` 在 VM 上執行的 vCenter Server 6.5 （）所管理。
- Contoso 具有內部部署的資料中心（ `contoso-datacenter` ）與內部部署網域控制站（ `contosodc1` ）。

### <a name="proposed-architecture"></a>建議的架構

在此情節中：

- Contoso 會將應用程式前端遷移 `WEBVM` 至 Azure 基礎結構即服務（IaaS） VM。
  - Azure 中的前端 VM 將會部署在 `ContosoRG` 資源群組中（用於生產資源）。
  - 它會位於主要區域的 Azure 生產網路（ `VNET-PROD-EUS2` ）中（） `East US 2` 。
- 應用程式資料庫將會遷移至 Azure SQL Server VM。
  - 它會位於 Contoso 在主要區域（）中的 Azure 資料庫網路（ `PROD-DB-EUS2` ） `East US 2` 。
  - 它會放在 Windows Server 容錯移轉叢集中，其中包含兩個使用 SQL Server Always On 可用性群組的節點。
  - 在 Azure 中，叢集中的兩個 SQL Server VM 節點將會部署在 `ContosoRG` 資源群組中。
  - VM 節點會位於主要區域的 Azure 生產網路（ `VNET-PROD-EUS2` ）中（ `East US 2` ）。
  - Vm 將會使用 SQL Server 2017 Enterprise edition 執行 Windows Server 2016。 Contoso 沒有此作業系統的授權。 它會使用 Azure Marketplace 中的映射，以向公司的 Azure Enterprise 合約承諾用量提供授權。
  - 除了唯一名稱以外，這兩個 VM 所使用的設定皆相同。
- Contoso 會部署內部負載平衡器，以接聽叢集上的流量，並將其導向至適當的叢集節點。
  - 內部負載平衡器將會部署到 `ContosoNetworkingRG` （用於網路資源）。
- 移轉完成之後，將會解除委任 Contoso 資料中心的內部部署 VM。

    ![顯示案例架構圖表的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/architecture.png)

### <a name="database-considerations"></a>資料庫考量

在解決方案設計過程中，Contoso 會進行 Azure SQL Database 與 SQL Server 的功能比較。 下列考慮可協助公司決定使用執行 SQL Server 的 Azure IaaS VM：

- 如果 Contoso 需要自訂作業系統或資料庫伺服器，或可能想要在相同的 VM 上共置並執行協力廠商應用程式，則使用執行 SQL Server 的 Azure VM 似乎是最佳解決方案。

### <a name="solution-review"></a>解決方案檢閱

Contoso 會藉由將優缺點清單放在一起，來評估其提議的設計。

| 考量 | 詳細資料 |
| --- | --- |
| **優點** | `WEBVM`將會移至 Azure 而不進行變更，讓您輕鬆進行遷移。 <br><br> SQL Server 層將會在 SQL Server 2017 和 Windows Server 2016 上執行。 這會淘汰目前的 Windows Server 2008 R2 作業系統。 執行 SQL Server 2017 可支援 Contoso 的技術需求和目標。 它會在離開 SQL Server 2008 R2 的同時，提供100% 的相容性。 <br><br> Contoso 可以使用 Azure Hybrid Benefit，利用其對軟體保證的投資效益。 <br><br> Azure 中的高可用性 SQL Server 部署提供容錯功能，讓應用程式資料層不再是單一的容錯移轉點。 |
| **缺點** | `WEBVM`正在執行 Windows Server 2008 R2。 Azure 對此作業系統的支援僅限於特定角色 (2018 年 7 月)。 若要深入瞭解，請參閱[適用于 Microsoft Azure 虛擬機器的 Microsoft 伺服器軟體支援](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)。 <br><br> 應用程式的 web 層會保留單一容錯移轉點。 <br><br> Contoso 必須繼續支援 web 層做為 Azure VM，而不是移至受管理的服務，例如 Azure App Service。 <br><br> 透過選擇的解決方案，Contoso 必須繼續管理兩個 SQL Server 的 Vm，而不是移至受控平臺，例如 Azure SQL 受控執行個體。 此外，透過軟體保證，Contoso 可以在 Azure SQL 受控執行個體上交換其現有授權以享有折扣費率。 |

### <a name="azure-services"></a>Azure 服務

| 服務 | 描述 | 成本 |
| --- | --- | --- |
| [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) | Azure 資料庫移轉服務可在停機時間最短的情況之下，從多個資料庫來源順暢地遷移至 Azure 資料平臺。 | 瞭解[支援的區域](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability)和[Azure 資料庫移轉服務的定價](https://azure.microsoft.com/pricing/details/database-migration)。 |
| [Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-overview) | Contoso 會使用 Azure Migrate 來評估其 VMware Vm。 Azure Migrate 會評定機器是否適合移轉。 它會提供在 Azure 中執行的大小調整建議和成本估計。 | 不須額外費用即可使用 Azure Migrate。 視他們決定用於評估和遷移的工具（第一方或獨立軟體廠商）而定，可能會產生費用。 深入瞭解[Azure Migrate 定價](https://azure.microsoft.com/pricing/details/azure-migrate)。 |

## <a name="migration-process"></a>移轉程序

Contoso 管理員會將應用程式 Vm 遷移至 Azure。

- 他們會使用 Azure Migrate 將前端 VM 遷移至 Azure VM：
  - 在第一個步驟中，他們會準備和設定 Azure 元件，並準備內部部署 VMware 基礎結構。
  - 一切就緒後，他們就可以開始複寫 VM。
  - 當複寫已啟用且正常運作之後，他們會使用 Azure Migrate 來遷移 VM。
- 在驗證資料庫之後，他們會使用 Azure 資料庫移轉服務，將資料庫移轉至 Azure 中的 SQL Server 叢集。
  - 在第一個步驟中，他們將需要在 Azure 中布建 SQL Server Vm、設定叢集和內部負載平衡器，以及設定 Always On 可用性群組。
  - 這麼做之後，他們就可以遷移資料庫。
- 在遷移之後，他們會啟用資料庫的 Always On 可用性群組。

    ![顯示遷移程式圖表的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/migration-process.png)

## <a name="prerequisites"></a>必要條件

以下是 Contoso 在此案例中應該準備好的事項。

| 需求 | 詳細資料 |
| --- | --- |
| **Azure 訂用帳戶** | Contoso 已在本系列稍早的文章中建立訂用帳戶。 如果您沒有 Azure 訂用帳戶，請建立[免費帳戶](https://azure.microsoft.com/free)。 <br><br> 如果您建立免費帳戶，您就是訂用帳戶的管理員，並可執行所有動作。 <br><br> 如果您使用現有的訂用帳戶，但您不是系統管理員，請與管理員合作以指派擁有者或參與者許可權給您。 <br><br> |
| **Azure 基礎結構** | Contoso 會如[適用於移轉的 Azure 基礎結構](./contoso-migration-infrastructure.md)中所述，設定 Azure 基礎結構。 <br><br> 深入瞭解 Azure Migrate 的特定[必要條件](./contoso-migration-devtest-to-iaas.md#prerequisites)需求：伺服器遷移。 |
| **內部部署伺服器** | 內部部署 vCenter Server 應執行版本5.5、6.0、6.5 或6.7。 <br><br> 執行5.5、6.0、6.5 或6.7 版本的 ESXi 主機。 <br><br> 一或多部在 ESXi 主機上執行的 VMware VM。 |
| **內部部署 VM** | 檢閱已背書在 Azure 上執行的 [Linux 機器](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)。 |

## <a name="scenario-steps"></a>案例步驟

以下是 Contoso 執行移轉的方式：

> [!div class="checklist"]
>
> - **步驟1：準備 SQL Server Always On 可用性群組叢集。** 建立叢集，以在 Azure 中部署兩個 SQL Server VM 節點。
> - **步驟2：部署並設定叢集。** 準備 Azure SQL Server 叢集。 資料庫會移轉至這個現有的叢集中。
> - **步驟3：部署 Azure Load Balancer。** 部署負載平衡器，以平衡傳輸至 SQL Server 節點的流量。
> - **步驟4：準備 Azure 以進行 Azure Migrate。** 建立 Azure 儲存體帳戶來保存複寫的資料。
> - **步驟5：準備內部部署 VMware 以進行 Azure Migrate。** 為帳戶進行 VM 探索和代理程式安裝的準備工作。 準備內部部署 VM，讓使用者在移轉之後可連線至 Azure VM。
> - **步驟6：將內部部署 Vm 複寫至 Azure。** 啟用對 Azure 複寫 VM 的功能。
> - **步驟7：透過 Azure 資料庫移轉服務遷移資料庫。** 使用 Azure 資料庫移轉服務，將資料庫移轉至 Azure。
> - **步驟8：使用 SQL Server Always On 保護資料庫。** 為叢集建立 Always On 可用性群組。
> - **步驟9：使用 Azure Migrate 遷移 VM。** 執行測試移轉，確定一切都沒問題。 然後執行遷移至 Azure。

## <a name="step-1-prepare-a-sql-server-always-on-availability-group-cluster"></a>步驟1：準備 SQL Server Always On 可用性群組叢集

若要設定叢集，Contoso 管理員會：

1. 選取 Azure Marketplace 中的 SQL Server 2017 Enterprise Windows Server 2016 映射，建立兩個 SQL Server 的 Vm。

    ![顯示 SQL VM SKU 的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/sql-vm-sku.png)

1. 在**建立虛擬機器嚮導**  >  **基本概念**中，他們會設定：

    - Vm 的名稱： `SQLAOG1` 和 `SQLAOG2` 。
    - 因為機器是業務關鍵的，請為 VM 磁片類型啟用 SSD。
    - 指定電腦認證。
    - 他們會將 Vm 部署在資源群組中的主要區域（ `East US 2` ） `ContosoRG` 。

1. 在 [**大小**] 中，它們會從 `D2S v3` 兩個 vm 的實例開始。 它們會在稍後視需要進行調整。
1. 在 [**設定**] 中，他們會執行下列動作：

    - 因為這些 Vm 是應用程式的重要資料庫，所以會使用受控磁片。
    - 它們會將機器放在 `PROD-DB-EUS2` 主要區域（）之生產網路（）的資料庫子網（）中 `VNET-PROD-EUS2` `East US 2` 。
    - 他們會建立 `SQLAOGAVSET` 具有兩個容錯網域和五個更新網域的新可用性設定組（）。

      ![顯示新可用性設定組的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/sql-vm-settings.png)

1. 在**SQL Server 設定**中，它們會將 SQL 連線限制為預設通訊埠1433上的虛擬網路（私人）。 若為驗證，則會使用與網站上所使用的相同認證（ `contosoadmin` ）。

    ![顯示 SQL Server 設定的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/sql-vm-db.png)

**需要其他協助？**

- 取得有關如何布建[SQL SERVER VM](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision#1-configure-basic-settings)的說明。
- 瞭解如何為[不同的 SQL Server sku 設定 vm](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-prereq#create-sql-server-vms)。

## <a name="step-2-deploy-and-set-up-the-cluster"></a>步驟 2︰部署並設定叢集

若要設定叢集，Contoso 管理員會：

1. 設定 Azure 儲存體帳戶作為雲端見證。
1. 將 SQL Server Vm 新增至 Contoso 內部部署資料中心內的 Active Directory 網域。
1. 在 Azure 中建立叢集。
1. 設定雲端見證。
1. 啟用 SQL Always On 可用性群組。

### <a name="set-up-a-storage-account-as-a-cloud-witness"></a>將儲存體帳戶設定為雲端見證

為了設定雲端見證，Contoso 必須要有 Azure 儲存體帳戶，用來保存用於叢集仲裁的 Blob 檔案。 相同的儲存體帳戶也可用來設定多個叢集的雲端見證。

若要建立儲存體帳戶，Contoso 管理員會：

1. 請為帳戶指定可辨識的名稱（ `contosocloudwitness` ）。
1. 使用 LRS 部署一般的所有用途帳戶。
1. 將帳戶放在第三個區域（ `South Central US` ）。 它們會將它放在主要和次要區域之外，讓它在區域失敗時仍可供使用。
1. 將它放在保留基礎結構資源的資源群組中 `ContosoInfraRG` 。

    ![顯示雲端見證帳戶名稱的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/witness-storage.png)

1. 當他們建立儲存體帳戶時，系統會產生該帳戶的主要和次要存取金鑰。 他們必須要有主要存取金鑰才能建立雲端見證。 金鑰會出現在儲存體帳戶名稱底下 >**存取金鑰**]。

    ![顯示存取金鑰的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/access-key.png)

<!-- docsTest:ignore "Failover Cluster feature -->

### <a name="add-sql-server-vms-to-contoso-domain"></a>將 SQL Server VM 新增至 Contoso 網域

1. Contoso 會將 `SQLAOG1` 和新增 `SQLAOG2` 至 `contoso.com` 網域。
1. 在每部 VM 上，管理員會安裝 Windows 容錯移轉叢集功能和工具。

### <a name="set-up-the-cluster"></a>設定叢集

在 Contoso 管理員設定叢集之前，他們會在每部機器上建立 OS 磁片的快照。

![顯示 [建立快照集] 窗格的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/snapshot.png)

1. 他們會執行腳本來建立 Windows 容錯移轉叢集。

    ![顯示建立 Windows 容錯移轉叢集之腳本的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/create-cluster1.png)

1. 建立叢集之後，它們會確認 Vm 顯示為叢集節點。

     ![顯示建立叢集的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/create-cluster2.png)

<!--docsTest:ignore "Failover Cluster Manager" -->

### <a name="configure-the-cloud-witness"></a>設定雲端見證

1. Contoso 管理員會在容錯移轉叢集管理員中使用**仲裁設定向導**來設定雲端見證。
1. 在嚮導中，他們會選擇使用儲存體帳戶建立雲端見證。
1. 設定雲端見證之後，它會出現在 [容錯移轉叢集管理員] 嵌入式管理單元中。

    ![顯示已設定雲端見證的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/cloud-witness.png)

### <a name="enable-sql-server-always-on-availability-groups"></a>啟用 SQL Server Always On 可用性群組

Contoso 管理員現在可以啟用 Always On 可用性群組：

1. 在 SQL Server 組態管理員中，他們為 **SQL Server (MSSQLSERVER)** 服務啟用 **Always On 可用性群組**。

    ![顯示 [啟用 Always On 可用性群組] 核取方塊的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/enable-always-on.png)

1. 他們重新啟動服務讓變更生效。

在啟用 Always On 可用性群組的情況下，Contoso 可以設定將會保護 SmartHotel360 資料庫的 Always On 可用性群組。

**需要其他協助？**

- 閱讀[雲端見證的相關資訊，並為其設定儲存體帳戶](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness)。
- 取得如何[設定叢集和建立可用性群組](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial)的指示。

## <a name="step-3-deploy-azure-load-balancer"></a>步驟3：部署 Azure Load Balancer

Contoso 管理員現在想要部署位於叢集節點前面的內部負載平衡器。 負載平衡器會接聽流量，並將其導向至適當的節點。

![顯示負載平衡的圖表。](./media/contoso-migration-rehost-vm-sql-ag/architecture-lb.png)

若要建立負載平衡器，Contoso 管理員：

1. 在 Azure 入口網站中，移至 [**網路**] [  >  **負載平衡器**]，並設定新的內部負載平衡器： `ILB-PROD-DB-EUS2-SQLAOG` 。
1. 將負載平衡器放在生產網路的資料庫子網（ `PROD-DB-EUS2` ）中（） `VNET-PROD-EUS2` 。
1. 為其指派靜態 IP 位址（ `10.245.40.100` ）。
1. 作為網路元素，請在網路資源群組中部署負載平衡器 `ContosoNetworkingRG` 。

    ![顯示 [建立負載平衡器] 窗格的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/lb-create.png)

部署內部負載平衡器之後，Contoso 管理員必須加以設定。 他們會建立後端位址集區、設定健康情況探查，以及設定負載平衡規則。

### <a name="add-a-back-end-pool"></a>新增後端集區

若要將流量分散到叢集中的 Vm，Contoso 管理員會設定後端位址集區，其中包含將會從負載平衡器接收網路流量之 Vm 的 Nic IP 位址。

1. 在入口網站的負載平衡器設定中，Contoso 會新增後端集區： `ILB-PROD-DB-EUS-SQLAOG-BEPOOL` 。
1. 系統管理員會將集區與可用性設定組產生關聯 `SQLAOGAVSET` 。 集合中的 Vm （ `SQLAOG1` 和 `SQLAOG2` ）會新增至集區。

    ![顯示 [新增後端集區] 畫面的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/backend-pool.png)

### <a name="create-a-health-probe"></a>建立健康狀態探查

Contoso 管理員會建立健康情況探查，讓負載平衡器能夠監視應用程式的健康情況。 探查會根據其對健康狀態檢查的回應，從負載平衡器輪替中動態新增或移除 Vm。

若要建立探查，Contoso 管理員會：

1. 在入口網站的負載平衡器設定中，建立健康情況探查： **SQLAlwaysOnEndPointProbe**。
1. 設定探查以監視 TCP 埠59999上的 Vm。
1. 設定探查之間的間隔為5秒，臨界值為2。 如果有兩個探查失敗，VM 即會被視為狀況不良。

    ![顯示 [新增健全狀況探查] 畫面的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

### <a name="configure-the-load-balancer-to-receive-traffic"></a>設定要接收流量的負載平衡器

現在，Contoso 管理員會設定負載平衡器規則，以定義如何將流量分散至 Vm。

- 前端 IP 位址會處理傳入流量。
- 後端 IP 集區會接收流量。

若要建立規則，Contoso 管理員會：

1. 在入口網站的負載平衡器設定中，新增規則： `SQLAlwaysOnEndPointListener` 。
1. 設定前端接聽程式，以接收 TCP 通訊埠1433上的連入 SQL 用戶端流量。
1. 指定流量將路由至其中的後端集區，以及 Vm 用來接聽流量的埠。
1. 啟用浮動 IP （伺服器直接回傳），這一律為 SQL Server Always On 所需。

    ![顯示健全狀況探查設定的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

**需要其他協助？**

- 取得[Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)的總覽。
- 深入瞭解如何[建立負載平衡器](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-basic-internal-portal)。

## <a name="step-4-prepare-azure-for-azure-migrate"></a>步驟4：準備適用于 Azure Migrate 的 Azure

以下是 Contoso 部署 Azure Migrate 所需的 Azure 元件：

- Vm 在遷移時將位於其中的虛擬網路。
- 用來保存複寫資料的 Azure 儲存體帳戶。

Contoso 管理員會設定這些元件：

1. Contoso 已建立網路/子網，它可以在[部署 Azure 基礎結構](./contoso-migration-rehost-vm-sql-ag.md)時用於 Azure Migrate。

    - SmartHotel360 應用程式是生產應用程式，並 `WEBVM` 會遷移至 `VNET-PROD-EUS2` 主要區域（）中的 Azure 生產網路（） `East US 2` 。
    - `WEBVM`會放在 `ContosoRG` 用於生產資源的資源群組，以及在生產子網（ `PROD-FE-EUS2` ）中。

1. Contoso 管理員會 `contosovmsacc20180528` 在主要區域中建立 Azure 儲存體帳戶（）。

    - 使用具有標準儲存體和 LRS 複寫的一般用途帳戶。

## <a name="step-5-prepare-on-premises-vmware-for-azure-migrate"></a>步驟5：準備內部部署 VMware 以進行 Azure Migrate

以下是 Contoso 管理員在內部部署環境的準備工作：

- VCenter Server 或 vSphere ESXi 主機上的帳戶，可將 VM 探索自動化。
- 內部部署 VM 設定，讓 Contoso 可以在遷移之後連線到複寫的 Azure VM。

### <a name="prepare-an-account-for-automatic-discovery"></a>準備帳戶以進行自動探索

Azure Migrate 需要 VMware 伺服器的存取權：

- 自動探索 VM。
- 協調複寫與遷移。
- 需要至少一個唯讀帳戶。 他們需要可執行作業的帳戶，例如建立和移除磁片以及開啟 Vm。

若要設定此帳戶，Contoso 管理員會：

1. 在 vCenter 層級建立一個角色。
1. 將必要的許可權指派給該角色。

### <a name="prepare-to-connect-to-azure-vms-after-migration"></a>準備在遷移後連接到 Azure Vm

遷移之後，Contoso 會想要連線到 Azure Vm，並允許 Azure 管理 Vm。 若要這樣做，Contoso 管理員會在進行遷移之前執行下列工作：

1. 為了透過網際網路存取，他們會：

    - 請先在內部部署 VM 上啟用 RDP 或 SSH，再進行遷移。
    - 確定已為**公用**設定檔新增 TCP 和 UDP 規則。
    - 檢查作業系統防火牆中是否允許 RDP 或 SSH。

1. 為了透過站對站 VPN 存取，他們：

    - 請先在內部部署 VM 上啟用 RDP 或 SSH，再進行遷移。
    - 檢查作業系統防火牆中是否允許 RDP 或 SSH。
    - 若是 Windows，請將內部部署 VM 上的作業系統 SAN 原則設定為**OnlineAll**。

1. 安裝 Azure 代理程式：

    - [Azure Linux 代理程式](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux)
    - [Azure Windows 代理程式](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows)

1. 其他

   - 若是 Windows，觸發遷移時，VM 上不應該有擱置中的 Windows 更新。 如果有，在更新完成之前，Contoso 管理員將無法登入 VM。
   - 在遷移之後，他們可以勾選 [**開機診斷**] 以查看 VM 的螢幕擷取畫面。 如果無法運作，他們應該確認 VM 正在執行，並檢查這些[疑難排解秘訣](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)。

**需要其他協助？**

瞭解如何[準備 vm 以進行遷移](https://docs.microsoft.com/azure/migrate/prepare-for-migration)。

## <a name="step-6-replicate-the-on-premises-vms-to-azure"></a>步驟6：將內部部署 Vm 複寫至 Azure

Contoso 管理員必須先設定並啟用複寫，才能執行遷移至 Azure。

完成探索之後，他們就可以開始將 VMware Vm 複寫到 Azure。

1. 在 Azure Migrate 專案中，他們會移至 [**伺服器**]  >  **Azure Migrate： [伺服器遷移**]，然後選取 [複寫]。 **Replicate**

    ![顯示 [複寫] 選項的螢幕擷取畫面。](./media/contoso-migration-rehost-vm/select-replicate.png)

1. 在**Replicate**[複寫  >  **來源設定**] 中，  >  **您的電腦已虛擬化嗎？** 他們會選取 **[是]，並 VMware vSphere**。

1. 在**內部部署應用裝置**中，他們會選取已設定的 Azure Migrate 設備的名稱，然後選取 **[確定]**。

    ![顯示 [來源設定] 索引標籤的螢幕擷取畫面。](./media/contoso-migration-rehost-vm/source-settings.png)

1. 在 [**虛擬機器**] 中，他們會選取要複寫的機器。
    - 如果 Contoso 管理員已針對 Vm 執行評估，則可以從評估結果套用 VM 大小調整和磁片類型（premium/standard）建議。 在 [**從 Azure Migrate 評估匯入遷移設定？**] 中，他們會選取 [**是]** 選項。
    - 如果他們未執行評量，或不想使用評估設定，他們會選取 [**否**] 選項。
    - 如果他們選擇使用評估，他們會選取 VM 群組和評量名稱。

    ![顯示選取評量的螢幕擷取畫面。](./media/contoso-migration-rehost-vm/select-assessment.png)

1. 在**虛擬機器**中，它們會視需要搜尋 vm，並檢查要遷移的每個 vm。 然後選取 **[下一步：目標設定]**。

1. 在 [**目標設定**] 中，他們會選取訂用帳戶和要遷移的目的地區域，並指定在遷移後 Azure vm 所在的資源群組。 在**虛擬網路**中，他們會選取 azure 虛擬網路/子網，以在遷移後將 azure vm 加入其中。

1. 在**Azure Hybrid Benefit**中，Contoso 管理員：

    - 如果不想套用 Azure Hybrid Benefit，請選取 [**否**]。 然後選取 **[下一步]**。
    - 如果具有有效軟體保證或 Windows Server 訂用帳戶所涵蓋的 Windows Server 電腦，而且他們想要將其權益套用至所要遷移的機器，請選取 **[是]** 。 然後選取 **[下一步]**。

1. 在 [**計算**] 中，他們會檢查 VM 名稱、大小、OS 磁片類型和可用性設定組。 VM 必須符合 [Azure 需求](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#vmware-requirements)。

    - **VM 大小：** 如果他們使用評估建議，[VM 大小] 下拉式清單會包含建議的大小。 否則，Azure Migrate 會根據 Azure 訂用帳戶中最符合的相符項來挑選大小。 或者，他們也可以選擇手動大小的**AZURE VM 大小。**
    - **OS 磁片：** 他們會指定 VM 的 OS （開機）磁片。 OS 磁碟是具有作業系統開機載入器和安裝程式的磁碟。
    - **可用性設定組：** 如果 VM 在遷移後應位於 Azure 可用性設定組中，它們會指定集合。 此集合必須位於為遷移指定的目標資源群組中。

1. 在 [**磁片**] 中，它們會指定是否應將 VM 磁片複寫到 Azure。 然後，他們會在 Azure 中選取磁片類型（標準 SSD/HDD 或 premium 受控磁片），然後選取 **[下一步]**。
    - 它們可以排除磁片不進行複寫。
    - 如果磁片已排除，則在遷移之後，它們將不會出現在 Azure VM 上。

1. 在 [**審查 + 開始**複寫] 中，他們會檢查設定。 然後，他們**會選取 [** 複寫] 來啟動伺服器的初始複寫。

> [!NOTE]
> 複寫設定可以在複寫開始之前，隨時更新**管理**複寫的  >  **機器**。 在複寫啟動後，就無法變更設定。

## <a name="step-7-migrate-the-database-via-azure-database-migration-service"></a>步驟7：透過 Azure 資料庫移轉服務遷移資料庫

Contoso 管理員會遵循[逐步遷移教學](https://docs.microsoft.com/azure/dms/tutorial-sql-server-azure-sql-online)課程，透過 Azure 資料庫移轉服務來遷移資料庫。 他們可以執行線上、離線和混合式（預覽）的遷移。

總而言之，他們必須執行下列工作：

- 使用 Premium 定價層來建立連接到虛擬網路的 Azure 資料庫移轉服務實例。
- 確定實例可以透過虛擬網路存取遠端 SQL Server。 請確定所有連入埠都可從 Azure 允許在虛擬網路層級、網路 VPN 和裝載 SQL Server 的電腦上 SQL Server。
- 設定實例：
  - 建立遷移專案。
  - 新增來源（內部部署資料庫）。
  - 選取目標。
  - 選取要遷移的資料庫。
  - 設定 [高級設定]。
  - 開始複寫。
  - 解決任何錯誤。
  - 執行最後的轉換。

## <a name="step-8-protect-the-database-with-sql-server-always-on"></a>步驟8：使用 SQL Server Always On 保護資料庫

在上執行的應用程式資料庫 `SQLAOG1` 中，Contoso 管理員現在可以使用 Always On 可用性群組來保護它。 他們會使用 SQL Server Management Studio 來設定 SQL Server Always On，然後使用 Windows 叢集指派接聽程式。

### <a name="create-an-always-on-availability-group"></a>建立 Always On 可用性群組

1. 在 SQL Server Management Studio 中，他們會選取並按住（或按一下滑鼠右鍵） **Always On 高可用性**，以啟動 [**新增可用性群組嚮導]**。
1. 在 [**指定選項**] 中，他們會將可用性群組命名為 `SHAOG` 。 在 [**選取資料庫**] 中，他們會選取 `SmartHotel360` 資料庫。

    ![顯示 [選取資料庫] 窗格的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/aog-1.png)

1. 在 [**指定複本**] 中，他們會新增兩個 SQL 節點做為可用性複本，並將其設定為使用同步認可來提供自動容錯移轉。

     ![顯示 [複本] 索引標籤的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/aog-2.png)

1. 他們會設定群組（ `SHAOG` ）和埠的接聽程式。 內部負載平衡器的 IP 位址會新增為靜態 IP 位址（ `10.245.40.100` ）。

    ![顯示 [建立可用性群組接聽程式] 選項的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/aog-3.png)

1. 在 [選取資料同步處理]**** 中，他們啟用自動植入。 使用此選項時，SQL Server 會自動為群組中的每個資料庫建立次要複本，因此 Contoso 不需要手動備份和還原。 驗證之後，會建立可用性群組。

    ![顯示已建立 Always On 可用性群組的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/aog-4.png)

1. Contoso 在建立群組時發生問題。 它不會使用 Active Directory 的 Windows 整合式安全性，而且需要授與 SQL 登入的許可權，以建立 Windows 容錯移轉叢集角色。

    ![顯示授與許可權給 SQL 登入的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/aog-5.png)

1. 建立群組之後，它會顯示在 SQL Server Management Studio 中。

### <a name="configure-a-listener-on-the-cluster"></a>在叢集上設定接聽程式

在設定 SQL 部署的最後一個步驟中，Contoso 管理員會將內部負載平衡器設定為叢集上的接聽程式，並使接聽程式上線。 他們會使用腳本來執行這項工作。

![顯示叢集接聽程式的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/cluster-listener.png)

### <a name="verify-the-configuration"></a>驗證設定

在所有設定完成後，Contoso 此時在使用移轉後資料庫的 Azure 中已有可運作的可用性群組。 系統管理員藉由連接到 SQL Server Management Studio 中的內部負載平衡器來確認設定。

![顯示內部負載平衡器連線的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/ilb-connect.png)

**需要其他協助？**

- 深入瞭解如何建立[可用性群組](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#create-the-availability-group)[和接聽](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#configure-listener)程式。
- 手動[設定叢集以使用負載平衡器 IP 位址](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener#configure-the-cluster-to-use-the-load-balancer-ip-address)。
- 深入瞭解如何[建立和使用 SAS](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2)。

## <a name="step-9-migrate-the-vm-with-azure-migrate"></a>步驟9：使用 Azure Migrate 遷移 VM

Contoso 管理員會執行快速測試容錯移轉，然後遷移 VM。

### <a name="run-a-test-migration"></a>執行測試移轉

執行測試遷移有助於確保所有專案在遷移前都能如預期般運作。 Contoso 管理員：

1. 執行測試容錯移轉至最新的可用時間點（ `Latest processed` ）。
1. 選取 [**先將機器關機再開始容錯移轉**]，讓 Azure Migrate 嘗試先關閉來源 VM，再觸發容錯移轉。 即使關機失敗，仍會繼續容錯移轉。
1. 測試容錯移轉會執行：

    - 系統會執行先決條件檢查，以確保所有移轉所需的條件都已準備就緒。
    - 容錯移轉會處理資料，以便建立 Azure VM。 如果選取了最新的復原點，則會根據資料建立復原點。
    - 您可以使用上一個步驟中處理的資料來建立 Azure VM。

1. 容錯移轉完成後，Azure VM 複本會出現在 Azure 入口網站中。 他們會確認 VM 為適當的大小、其已連線到正確的網路，而且正在執行中。
1. 確認之後，他們可以清除容錯移轉，並記錄與儲存任何觀察到的結果。

### <a name="run-a-failover"></a>執行容錯移轉

1. 確認測試容錯移轉如預期般運作之後，他們會建立用於遷移的復原方案，並將新增 `WEBVM` 至方案。

     ![顯示建立復原方案的螢幕擷取畫面](./media/contoso-migration-rehost-vm-sql-ag/recovery-plan.png)

1. 他們根據此計畫執行容錯移轉。 他們會選取最新的復原點。 它們會指定 Azure Migrate 應該在觸發容錯移轉之前，先嘗試關閉內部部署 VM。

    ![顯示 [容錯移轉] 窗格的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/failover1.png)

1. 容錯移轉之後，他們可以確認 Azure VM 會如預期般出現在 Azure 入口網站中。

    ![顯示虛擬機器的 [總覽] 窗格的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/failover2.png)

1. 在 Azure 中確認 VM 之後，他們會完成遷移以完成遷移程式、停止 VM 的複寫，以及停止 VM 的 Azure Migrate 計費。

    ![顯示完整遷移專案的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/failover3.png)

### <a name="update-the-connection-string"></a>更新連接字串

在遷移程式的最後一個步驟中，Contoso 管理員會將應用程式的連接字串更新為指向在接聽程式上執行的已遷移資料庫 `SHAOG` 。 這項設定將會在現在於 Azure 中執行的變更 `WEBVM` 。 此設定位於 `web.config` ASP.NET 應用程式的中。

1. Contoso 管理員會在找出檔案 `C:\inetpub\SmartHotelWeb\web.config` ，並變更伺服器的名稱，以反映 Always On 可用性群組的 FQDN： `shaog.contoso.com` 。

    ![顯示 Always On 可用性群組之 FQDN 的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/failover4.png)

1. 在更新並儲存檔案之後，他們會在上重新開機 IIS `WEBVM` 。 它們會 `iisreset /restart` 從命令提示字元使用。
1. 重新開機 IIS 之後，應用程式現在會使用在受控實例上執行的資料庫。

**需要其他協助？**

- 瞭解如何[執行測試容錯移轉](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure)。
- 瞭解如何[建立復原方案](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans)。
- 瞭解如何[容錯移轉至 Azure](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover)。

### <a name="clean-up-after-migration"></a>移轉之後進行清除

在遷移之後，SmartHotel360 應用程式會在 Azure VM 上執行。 SmartHotel360 資料庫位於 Azure SQL 叢集中。

現在，Contoso 必須完成下列清除步驟：

- 從 vCenter 清查中移除內部部署 VM。
- 從本機備份作業中移除 VM。
- 更新內部文件以顯示 VM 的新位置和 IP 位址。
- 檢閱與已解除委任 VM 互動的任何資源。 更新任何相關的設定或文件，以反映新的組態。
- 將兩個新的 Vm （ `SQLAOG1` 和 `SQLAOG2` ）新增至生產環境監視系統。

### <a name="review-the-deployment"></a>檢閱部署

對於 Azure 中的遷移後資源，Contoso 必須能執行一切功能並保護其新的基礎結構。

### <a name="security"></a>安全性

Contoso 安全性小組會審核虛擬機器 `WEBVM` 、 `SQLAOG1` 和， `SQLAOG2` 以判斷是否有任何安全性問題。 他們需要：

- 檢查 VM 的網路安全性群組（Nsg），以控制存取權。 NSG 可用來確保只可以傳遞該應用程式允許的流量。
- 請考慮使用 Azure 磁碟加密和 Azure Key Vault 來保護磁片上的資料。
- 評估透明資料加密。 然後在新的 Always On 可用性群組上執行的 SmartHotel360 資料庫上啟用它。 深入瞭解[透明資料加密](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017)。

如需詳細資訊，請參閱[Azure 中 IaaS 工作負載的安全性最佳作法](https://docs.microsoft.com/azure/security/fundamentals/iaas)。

## <a name="business-continuity-and-disaster-recovery"></a>商務持續性和災害復原

針對商務持續性和災害復原，Contoso 會採取下列動作：

- 為了保護資料的安全，Contoso 會透過 `WEBVM` `SQLAOG1` `SQLAOG2` [Azure VM 備份](https://docs.microsoft.com/azure/backup/backup-azure-vms-introduction)來備份、和 vm 上的資料。
- Contoso 也會瞭解如何使用 Azure 儲存體將 SQL Server 直接備份到 Azure Blob 儲存體。 深入瞭解如何[使用 Azure 儲存體 SQL Server 備份和還原](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-use-storage-sql-server-backup-restore)。
- 為了讓應用程式正常運作，Contoso 會使用 Site Recovery 將 Azure 中的應用程式 Vm 複寫至次要區域。 深入瞭解如何[設定 AZURE VM 的損毀修復至次要 azure 區域](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart)。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- Contoso 擁有其 WEBVM 的現有授權，而且會利用 Azure Hybrid Benefit。 Contoso 會轉換現有的 Azure VM，以便充分利用這個定價。
- Contoso 會使用[Azure 成本管理和帳單](https://docs.microsoft.com/azure/cost-management-billing/cost-management-billing-overview)來確保公司維持在 IT 領導地位所建立的預算內。

## <a name="conclusion"></a>結論

在本文中，Contoso 會在 Azure 中重新裝載 SmartHotel360 應用程式，方法是使用 Azure Migrate 將應用程式前端 VM 遷移至 Azure。 Contoso 會使用 Azure 資料庫移轉服務，將應用程式資料庫移轉至布建在 Azure 中的 SQL Server 叢集，並在 SQL Server Always On 可用性群組中加以保護。
