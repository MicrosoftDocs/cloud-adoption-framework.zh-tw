---
title: 將應用程式遷移至 Azure Vm，並 SQL Server Always On 可用性群組，以重新裝載應用程式
description: 了解 Contoso 如何將內部部署應用程式移轉至 Azure VM 和 SQL Server Always On 可用性群組，以重新裝載該應用程式。
author: deltadan
ms.author: abuck
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: c633fe36e4817ad92501085b8309acf945397bc3
ms.sourcegitcommit: 8b82889dca0091f3cc64116f998a3a878943c6a1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/09/2020
ms.locfileid: "89602568"
---
<!-- cSpell:ignore WEBVM SQLVM contosohost vcenter contosodc AOAG SQLAOG SQLAOGAVSET contosoadmin contosocloudwitness MSSQLSERVER BEPOOL contosovmsacc SHAOG NSGs inetpub iisreset -->

# <a name="rehost-an-on-premises-application-with-azure-vms-and-sql-server-always-on-availability-groups"></a>使用 Azure Vm 和 SQL Server Always On 可用性群組重新裝載內部部署應用程式

本文示範虛構公司 Contoso 如何如何在 VMware 虛擬機器上執行的兩層式 Windows .NET 應用程式， (Vm) 作為遷移至 Azure 的一部分。 Contoso 會將應用程式前端 VM 遷移至 Azure VM，並將應用程式資料庫移轉至 Azure SQL Server VM，並在具有 SQL Server Always On 可用性群組的 Windows Server 容錯移轉叢集中執行。

此範例中使用的 SmartHotel360 應用程式是以開放原始碼的形式提供。 如果您想要將它用於您自己的測試用途，請從 [GitHub](https://github.com/Microsoft/SmartHotel360)下載。

## <a name="business-drivers"></a>商業動機

IT 領導小組與商務合作夥伴密切合作，瞭解他們想要使用這種方式達成什麼目標。 他們想要：

- **解決業務成長。** Contoso 正在成長，因此對內部部署系統和基礎結構造成了壓力。
- **提高效率。** Contoso 必須移除不必要的程式，並簡化開發人員和使用者的流程。 企業需要快速且不浪費時間或金錢，以更快的速度提供客戶的需求。
- **增加靈活性。** Contoso IT 必須能夠更快因應企業的需求。 它必須比 marketplace 中的變更更快，以實現全球經濟的成功。 它不得取得或成為企業封鎖程式。
- **規模。** 隨著企業順利成長，Contoso IT 必須提供以相同步調成長的系統。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已針對此次移轉擬定好各項目標。 這些目標用來判斷最合適的移轉方法：

- 在遷移之後，Azure 中的應用程式應該具有與現今在 VMware 中相同的效能功能。 應用程式在內部部署環境中將會保持在雲端中的重要性。
- Contoso 不想要投資此應用程式。 這對企業很重要，但以其目前的形式而言，Contoso 只想要將它安全地移至雲端。
- 應用程式的內部部署資料庫發生可用性問題。 Contoso 想要在 Azure 中將其部署為具有容錯移轉功能的高可用性叢集。
- Contoso 想要從目前的 SQL Server 2008 R2 平臺升級至 SQL Server 2017。
- Contoso 不想對此應用程式使用 Azure SQL Database，且正在尋找替代方案。

## <a name="solution-design"></a>解決方案設計

完成公司的目標和需求後，Contoso 會設計和審核部署解決方案，並識別遷移程式。 也會識別其將用於遷移的 Azure 服務。

### <a name="current-architecture"></a>目前架構

- 應用程式會分層至兩個 Vm (`WEBVM` 和 `SQLVM`) 。
- VM 位於 VMware ESXi 主機 `contosohost1.contoso.com`(6.5 版)。
- VMware 環境是由 `vcenter.contoso.com` VM 上執行的 vCenter Server 6.5 () 來管理。
- Contoso 有內部部署資料中心 () 搭配內部 `contoso-datacenter` 部署網域控制站 (`contosodc1`) 。

### <a name="proposed-architecture"></a>建議的架構

在此情節中：

- Contoso 會將應用程式前端遷移 `WEBVM` 至 Azure 基礎結構即服務 (IaaS) VM。
  - Azure 中的前端 VM 將部署在資源群組中， `ContosoRG` (用於生產資源) 。
  - 它會位於主要區域中的 Azure 生產網路 (`VNET-PROD-EUS2`)  (`East US 2`) 。
- 應用程式資料庫將會遷移至 Azure SQL Server VM。
  - 它會位於 Contoso 的 Azure 資料庫網路 (`PROD-DB-EUS2`) 在主要區域 (`East US 2`) 。
  - 它會放在 Windows Server 容錯移轉叢集中，其中有兩個使用 SQL Server Always On 可用性群組的節點。
  - 在 Azure 中，叢集中的兩個 SQL Server VM 節點將會部署在 `ContosoRG` 資源群組中。
  - VM 節點將位於主要區域中的 Azure 生產網路 (`VNET-PROD-EUS2`)  (`East US 2`) 。
  - Vm 將會執行具有 SQL Server 2017 Enterprise edition 的 Windows Server 2016。 Contoso 沒有此作業系統的授權。 它會使用 Azure Marketplace 中的影像，以提供公司 Azure Enterprise 合約承諾用量的授權。
  - 除了唯一名稱以外，這兩個 VM 所使用的設定皆相同。
- Contoso 會部署內部負載平衡器，以接聽叢集中的流量，並將它導向至適當的叢集節點。
  - 內部負載平衡器會部署在 `ContosoNetworkingRG` 用於網路資源) 的 (中。
- 移轉完成之後，將會解除委任 Contoso 資料中心的內部部署 VM。

    ![顯示案例架構圖表的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/architecture.png)

### <a name="database-considerations"></a>資料庫考量

在解決方案設計過程中，Contoso 會進行 Azure SQL Database 與 SQL Server 的功能比較。 下列考慮有助於公司決定要使用執行 SQL Server 的 Azure IaaS VM：

- 如果 Contoso 需要自訂作業系統或資料庫伺服器，或可能想要在相同的 VM 上共置和執行協力廠商應用程式，則使用執行 SQL Server 的 Azure VM 似乎是最佳解決方案。

### <a name="solution-review"></a>解決方案檢閱

Contoso 藉由結合一份優缺點來評估其提議的設計。

| 考量 | 詳細資料 |
| --- | --- |
| **優點** | `WEBVM` 將會移至 Azure 而不需要變更，以簡化遷移工作。 <br><br> SQL Server 層將會在 SQL Server 2017 和 Windows Server 2016 上執行。 這會淘汰目前的 Windows Server 2008 R2 作業系統。 執行 SQL Server 2017 可支援 Contoso 的技術需求和目標。 它提供100% 的相容性，同時遠離 SQL Server 2008 R2。 <br><br> Contoso 可以使用 Azure Hybrid Benefit 來利用其軟體保證的投資。 <br><br> Azure 中的高可用性 SQL Server 部署提供容錯功能，因此應用程式資料層不再是單一容錯移轉點。 |
| **缺點** | `WEBVM` 正在執行 Windows Server 2008 R2。 Azure 對此作業系統的支援僅限於特定角色 (2018 年 7 月)。 若要深入瞭解，請參閱 [Microsoft Azure 虛擬機器的 Microsoft 伺服器軟體支援](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)。 <br><br> 應用程式的 web 層仍是單一容錯移轉點。 <br><br> Contoso 必須繼續支援作為 Azure VM 的 web 層，而不是移至受控服務（例如 Azure App Service）。 <br><br> 使用選擇的解決方案，Contoso 必須繼續管理兩個 SQL Server Vm，而不是移至受控平臺，例如 Azure SQL 受控執行個體。 此外，透過軟體保證，Contoso 可以在 Azure SQL 受控執行個體上交換其現有的授權以取得折扣費率。 |

### <a name="azure-services"></a>Azure 服務

| 服務 | 描述 | 成本 |
| --- | --- | --- |
| [Azure 資料庫移轉服務](/azure/dms/dms-overview) | Azure 資料庫移轉服務可讓您從多個資料庫來源順暢地遷移到 Azure 資料平臺，並減少停機時間。 | 深入瞭解 [支援的區域](/azure/dms/dms-overview#regional-availability) 和 [Azure 資料庫移轉服務定價](https://azure.microsoft.com/pricing/details/database-migration)。 |
| [Azure Migrate](/azure/migrate/migrate-overview) | Contoso 會使用 Azure Migrate 來評定其 VMware Vm。 Azure Migrate 會評定機器是否適合移轉。 它會提供在 Azure 中執行的大小調整建議和成本估計。 | 不須額外費用即可使用 Azure Migrate。 它們可能會產生費用，視 (第一方或獨立軟體廠商的工具而定，) 他們決定用來進行評量和遷移。 深入瞭解 [Azure Migrate 定價](https://azure.microsoft.com/pricing/details/azure-migrate)。 |

## <a name="migration-process"></a>移轉程序

Contoso 管理員會將應用程式 Vm 遷移至 Azure。

- 他們會使用 Azure Migrate 將前端 VM 遷移至 Azure VM：
  - 在第一個步驟中，他們會準備及設定 Azure 元件，並準備內部部署 VMware 基礎結構。
  - 一切就緒後，他們就可以開始複寫 VM。
  - 啟用複寫並正常運作之後，他們會使用 Azure Migrate 來遷移 VM。
- 在驗證資料庫之後，他們會使用 Azure 資料庫移轉服務，將資料庫移轉至 Azure 中的 SQL Server 叢集。
  - 在第一個步驟中，他們必須在 Azure 中布建 SQL Server Vm、設定叢集和內部負載平衡器，以及設定 Always On 可用性群組。
  - 有了這項功能，他們就可以遷移資料庫。
- 遷移之後，他們會啟用資料庫的 Always On 可用性群組。

    ![顯示遷移程式圖表的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/migration-process.png)

## <a name="prerequisites"></a>先決條件

以下是 Contoso 在此案例中應該準備好的事項。

| 需求 | 詳細資料 |
| --- | --- |
| **Azure 訂用帳戶** | Contoso 已在本系列稍早的文章中建立訂用帳戶。 如果您沒有 Azure 訂用帳戶，請建立[免費帳戶](https://azure.microsoft.com/free)。 <br><br> 如果您建立免費帳戶，您就是訂用帳戶的管理員，並可執行所有動作。 <br><br> 如果您使用現有的訂用帳戶，而且您不是系統管理員，請與系統管理員合作，指派擁有者或參與者許可權給您。 <br><br> |
| **Azure 基礎結構** | Contoso 會如[適用於移轉的 Azure 基礎結構](./contoso-migration-infrastructure.md)中所述，設定 Azure 基礎結構。 <br><br> 深入瞭解 Azure Migrate：伺服器遷移的特定 [必要條件](./contoso-migration-devtest-to-iaas.md#prerequisites) 需求。 |
| **內部部署伺服器** | 內部部署 vCenter Server 應執行5.5、6.0、6.5 或6.7 版。 <br><br> 執行5.5、6.0、6.5 或6.7 版本的 ESXi 主機。 <br><br> 一或多部在 ESXi 主機上執行的 VMware VM。 |
| **內部部署 VM** | 檢閱已背書在 Azure 上執行的 [Linux 機器](/azure/virtual-machines/linux/endorsed-distros)。 |

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

若要設定叢集，Contoso 管理員：

1. 在 Azure Marketplace 中選取 SQL Server 2017 企業版 Windows Server 2016 映射，以建立兩個 SQL Server Vm。

    ![顯示 SQL VM SKU 的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/sql-vm-sku.png)

1. 在**建立虛擬機器嚮導**的  >  **基本概念**中，他們會設定：

    - Vm 的名稱： `SQLAOG1` 和 `SQLAOG2` 。
    - 因為機器是業務關鍵性的，所以請為 VM 磁片類型啟用 SSD。
    - 指定電腦認證。
    - 他們會將 Vm 部署在主要區域中， (`East US 2`) 在 `ContosoRG` 資源群組中。

1. 就 **大小**而言，它們是從 `D2S v3` 兩個 vm 的實例開始。 稍後會視需要進行調整。
1. 在 [ **設定**] 中，他們會執行下列動作：

    - 因為這些 Vm 是應用程式的重要資料庫，所以會使用受控磁片。
    - 它們會將電腦放在資料庫子網中， (`PROD-DB-EUS2` 主要區域中的生產網路)  (`VNET-PROD-EUS2`)  (`East US 2`) 。
    - 他們會建立新的可用性設定組 (`SQLAOGAVSET`) 包含兩個容錯網域和五個更新網域。

      ![顯示新可用性設定組的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/sql-vm-settings.png)

1. 在 **SQL Server 設定**中，它們會將虛擬網路的 SQL 連線能力限制在預設通訊埠1433上 (私用) 。 針對驗證，他們使用的認證與在網站上使用的 (`contosoadmin`) 相同。

    ![顯示 SQL Server 設定的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/sql-vm-db.png)

**需要其他協助嗎？**

- 取得有關如何布建 [SQL SERVER VM](/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision#1-configure-basic-settings)的說明。
- 瞭解如何 [針對不同的 SQL Server sku 設定 vm](/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-prereq#create-sql-server-vms)。

## <a name="step-2-deploy-and-set-up-the-cluster"></a>步驟 2︰部署並設定叢集

若要設定叢集，Contoso 管理員：

1. 設定 Azure 儲存體帳戶作為雲端見證。
1. 將 SQL Server Vm 新增至 Contoso 內部部署資料中心內的 Active Directory 網域。
1. 在 Azure 中建立叢集。
1. 設定雲端見證。
1. 啟用 SQL Always On 可用性群組。

### <a name="set-up-a-storage-account-as-a-cloud-witness"></a>將儲存體帳戶設定為雲端見證

為了設定雲端見證，Contoso 必須要有 Azure 儲存體帳戶，用來保存用於叢集仲裁的 Blob 檔案。 相同的儲存體帳戶也可用來設定多個叢集的雲端見證。

若要建立儲存體帳戶，Contoso 管理員：

1. 請為帳戶 () 指定可辨識的名稱 `contosocloudwitness` 。
1. 使用 LRS 部署一般的全用途帳戶。
1. 將帳戶放在第三個區域中 (`South Central US`) 。 他們將它放在主要和次要區域外部，使其在區域失敗時仍可供使用。
1. 將它放在保存基礎結構資源的資源群組中 `ContosoInfraRG` 。

    ![顯示雲端見證帳戶名稱的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/witness-storage.png)

1. 當他們建立儲存體帳戶時，系統會產生該帳戶的主要和次要存取金鑰。 他們必須要有主要存取金鑰才能建立雲端見證。 金鑰會出現在儲存體帳戶名稱下 > **存取金鑰**。

    ![顯示存取金鑰的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/access-key.png)

<!-- docsTest:casing "Failover Cluster feature" -->

### <a name="add-sql-server-vms-to-contoso-domain"></a>將 SQL Server VM 新增至 Contoso 網域

1. Contoso 會將 `SQLAOG1` 和新增 `SQLAOG2` 至 `contoso.com` 網域。
1. 在每部 VM 上，系統管理員會安裝 Windows 容錯移轉叢集功能和工具。

### <a name="set-up-the-cluster"></a>設定叢集

在 Contoso 管理員設定叢集之前，他們會在每部機器上建立 OS 磁片的快照集。

![顯示 [建立快照集] 窗格的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/snapshot.png)

1. 他們會執行腳本來建立 Windows 容錯移轉叢集。

    ![顯示建立 Windows 容錯移轉叢集之腳本的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/create-cluster1.png)

1. 建立叢集之後，他們會確認 Vm 顯示為叢集節點。

     ![顯示建立叢集的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/create-cluster2.png)

<!--docsTest:casing "Failover Cluster Manager" -->

### <a name="configure-the-cloud-witness"></a>設定雲端見證

1. Contoso 管理員會使用容錯移轉叢集管理員中的 [仲裁設定 **]** 來設定雲端見證。
1. 在嚮導中，他們選擇使用儲存體帳戶建立雲端見證。
1. 設定雲端見證之後，它會出現在容錯移轉叢集管理員嵌入式管理單元中。

    ![顯示已設定雲端見證的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/cloud-witness.png)

### <a name="enable-sql-server-always-on-availability-groups"></a>啟用 SQL Server Always On 可用性群組

Contoso 管理員現在可以啟用 Always On 可用性群組：

1. 在 SQL Server 組態管理員中，他們為 **SQL Server (MSSQLSERVER)** 服務啟用 **Always On 可用性群組**。

    ![顯示 [啟用 Always On 可用性群組] 核取方塊的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/enable-always-on.png)

1. 他們重新啟動服務讓變更生效。

在啟用 Always On 可用性群組的情況下，Contoso 可以設定將會保護 SmartHotel360 資料庫的 Always On 可用性群組。

**需要其他協助嗎？**

- 閱讀雲端見證的相關資訊 [，並為其設定儲存體帳戶](/windows-server/failover-clustering/deploy-cloud-witness)。
- 取得如何 [設定叢集和建立可用性群組](/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial)的指示。

## <a name="step-3-deploy-azure-load-balancer"></a>步驟3：部署 Azure Load Balancer

Contoso 管理員現在想要部署位於叢集節點前面的內部負載平衡器。 負載平衡器會接聽流量，並將其導向至適當的節點。

![顯示負載平衡的圖表。](./media/contoso-migration-rehost-vm-sql-ag/architecture-lb.png)

若要建立負載平衡器，Contoso 管理員：

1. 在 Azure 入口網站中，移至 [**網路**  >  **負載平衡器**]，然後設定新的內部負載平衡器： `ILB-PROD-DB-EUS2-SQLAOG` 。
1. 將負載平衡器放在 (`PROD-DB-EUS2` 生產網路)  () 的資料庫子網中 `VNET-PROD-EUS2` 。
1.  () 將靜態 IP 位址指派給它 `10.245.40.100` 。
1. 以網路功能元素的方式，在網路資源群組中部署負載平衡器 `ContosoNetworkingRG` 。

    ![顯示 [建立負載平衡器] 窗格的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/lb-create.png)

部署內部負載平衡器之後，Contoso 管理員必須設定它。 他們建立後端位址集區、設定健康情況探查，以及設定負載平衡規則。

### <a name="add-a-back-end-pool"></a>新增後端集區

為了將流量分散至叢集中的 Vm，Contoso 管理員設定了後端位址集區，其中包含將會從負載平衡器接收網路流量之 Vm 的 Nic IP 位址。

1. 在入口網站的負載平衡器設定中，Contoso 會新增後端集區： `ILB-PROD-DB-EUS-SQLAOG-BEPOOL` 。
1. 系統管理員會將集區與可用性設定組產生關聯 `SQLAOGAVSET` 。 集合中的 Vm (`SQLAOG1` 和 `SQLAOG2`) 都會新增至集區。

    ![顯示 [新增後端集區] 畫面的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/backend-pool.png)

### <a name="create-a-health-probe"></a>建立健康狀態探查

Contoso 管理員會建立健康情況探查，讓負載平衡器能夠監視應用程式健全狀況。 探查會根據虛擬機器對健康狀態檢查的回應，以動態方式從負載平衡器輪替中新增或移除 Vm。

若要建立探查，Contoso 管理員：

1. 在入口網站的負載平衡器設定中，建立健康情況探查： **>sqlalwaysonendpointprobe**。
1. 設定探查以監視 TCP 埠59999上的 Vm。
1. 將探查之間的間隔設定為5秒，而臨界值為2。 如果有兩個探查失敗，VM 即會被視為狀況不良。

    ![顯示 [新增健康情況探查] 畫面的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

### <a name="configure-the-load-balancer-to-receive-traffic"></a>設定要接收流量的負載平衡器

現在，Contoso 管理員會設定負載平衡器規則，以定義如何將流量分散至 Vm。

- 前端 IP 位址會處理傳入流量。
- 後端 IP 集區會接收流量。

若要建立規則，Contoso 管理員：

1. 在入口網站的負載平衡器設定中，新增規則： `SQLAlwaysOnEndPointListener` 。
1. 設定前端接聽程式，以接收 TCP 埠1433上的連入 SQL 用戶端流量。
1. 指定要將流量路由傳送到其中的後端集區，以及 Vm 用來接聽流量的埠。
1. 啟用浮動 IP (直接伺服器傳回) ，SQL Server Always On 一律需要這項功能。

    ![顯示健康情況探查設定的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

**需要其他協助嗎？**

- 取得 [Azure Load Balancer](/azure/load-balancer/load-balancer-overview)的總覽。
- 瞭解如何 [建立負載平衡器](/azure/load-balancer/tutorial-load-balancer-basic-internal-portal)。

## <a name="step-4-prepare-azure-for-azure-migrate"></a>步驟4：準備 Azure 以進行 Azure Migrate

以下是 Contoso 部署 Azure Migrate 所需的 Azure 元件：

- 在遷移 Vm 時，將在其中尋找 Vm 的虛擬網路。
- 用來保存複寫資料的 Azure 儲存體帳戶。

Contoso 管理員會設定這些元件：

1. Contoso 已建立可在 [部署 Azure 基礎結構](./contoso-migration-rehost-vm-sql-ag.md)時用於 Azure Migrate 的網路/子網。

    - SmartHotel360 應用程式是生產應用程式， `WEBVM` 將會遷移到主要區域中的 Azure 生產網路 (`VNET-PROD-EUS2`)  (`East US 2`) 。
    - `WEBVM` 將放在 `ContosoRG` 用於生產資源的資源群組中，而在生產子網中， (`PROD-FE-EUS2`) 。

1. Contoso 管理員會 `contosovmsacc20180528` 在主要區域中建立 () 的 Azure 儲存體帳戶。

    - 使用一般用途的帳戶搭配標準儲存體和 LRS 複寫。

## <a name="step-5-prepare-on-premises-vmware-for-azure-migrate"></a>步驟5：針對 Azure Migrate 準備內部部署 VMware

以下是 Contoso 管理員在內部部署環境中的準備工作：

- VCenter Server 或 vSphere ESXi 主機上的帳戶，可將 VM 探索自動化。
- 內部部署 VM 設定，讓 Contoso 在遷移後可以連線到複寫的 Azure VM。

### <a name="prepare-an-account-for-automatic-discovery"></a>準備帳戶以進行自動探索

Azure Migrate 需要存取 VMware 伺服器才能：

- 自動探索 VM。
- 協調複寫和遷移。
- 需要至少一個唯讀帳戶。 他們需要可執行諸如建立和移除磁片以及開啟 Vm 等作業的帳戶。

若要設定帳戶，Contoso 管理員：

1. 在 vCenter 層級建立一個角色。
1. 將必要的許可權指派給該角色。

### <a name="prepare-to-connect-to-azure-vms-after-migration"></a>準備在遷移後連接至 Azure Vm

在遷移之後，Contoso 想要連線至 Azure Vm，並允許 Azure 管理 Vm。 若要這樣做，Contoso 管理員會在遷移之前執行下列工作：

1. 為了透過網際網路存取，他們會：

    - 在遷移之前，先在內部部署 VM 上啟用 RDP 或 SSH。
    - 確定已為**公用**設定檔新增 TCP 和 UDP 規則。
    - 檢查作業系統防火牆中是否允許 RDP 或 SSH。

1. 為了透過站對站 VPN 存取，他們：

    - 在遷移之前，先在內部部署 VM 上啟用 RDP 或 SSH。
    - 檢查作業系統防火牆中是否允許 RDP 或 SSH。
    - 若為 Windows，請將內部部署 VM 上的作業系統 SAN 原則設定為 **OnlineAll**。

1. 安裝 Azure 代理程式：

    - [Azure Linux 代理程式](/azure/virtual-machines/extensions/agent-linux)
    - [Azure Windows 代理程式](/azure/virtual-machines/extensions/agent-windows)

1. 其他

   - 若是 Windows，在觸發遷移時，VM 上不應該有暫止的 Windows 更新。 如果有，Contoso 管理員將無法登入 VM，直到更新完成為止。
   - 遷移後，他們可以檢查 **開機診斷** 以查看 VM 的螢幕擷取畫面。 如果沒有作用，則應確認 VM 是否正在執行，並檢查這些 [疑難排解秘訣](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)。

**需要其他協助嗎？**

瞭解如何 [準備 vm 以進行遷移](/azure/migrate/prepare-for-migration)。

## <a name="step-6-replicate-the-on-premises-vms-to-azure"></a>步驟6：將內部部署 Vm 複寫至 Azure

在 Contoso 管理員可以執行遷移至 Azure 之前，他們必須先設定並啟用複寫。

完成探索之後，他們就可以開始將 VMware Vm 複寫至 Azure。

1. 在 Azure Migrate 專案中，他們會移至 [**伺服器**  >  **Azure Migrate：伺服器遷移**] ** **，然後選取 [複寫]。

    ![顯示 [複寫] 選項的螢幕擷取畫面。](./media/contoso-migration-rehost-vm/select-replicate.png)

1. 在 **[** 複寫  >  **來源設定**] 中，您的  >  **電腦虛擬化了嗎？** 他們會選取 **[是]，VMware vSphere**。

1. 在 **內部部署設備**中，他們會選取已設定 Azure Migrate 設備的名稱，然後選取 **[確定]**。

    ![顯示 [來源設定] 索引標籤的螢幕擷取畫面。](./media/contoso-migration-rehost-vm/source-settings.png)

1. 在 **虛擬機器**中，他們會選取要複寫的機器。
    - 如果 Contoso 系統管理員已執行 Vm 的評量，則可以將 VM 大小調整和磁片類型套用至評定結果 (premium/standard) 建議。 在 [ **從 Azure Migrate 評量匯入遷移設定？**] 中，他們會選取 [ **是]** 選項。
    - 如果他們未執行評量，或不想使用評量設定，則會選取 [ **否** ] 選項。
    - 如果他們選擇使用評量，他們會選取 VM 群組和評量名稱。

    ![顯示選取評定的螢幕擷取畫面。](./media/contoso-migration-rehost-vm/select-assessment.png)

1. 在 **虛擬機器**中，他們會視需要搜尋 vm，並檢查每個 vm 以進行遷移。 然後選取 **[下一步：目標設定]**。

1. 在 [ **目標設定**] 中，他們會選取要遷移的訂用帳戶和目的地區域，並指定 Azure vm 在遷移後將位於其中的資源群組。 在 **虛擬網路**中，他們會選取 azure vm 在遷移後將加入的 azure 虛擬網路/子網。

1. 在 **Azure Hybrid Benefit**中，Contoso 管理員：

    - 如果不想套用 Azure Hybrid Benefit，請選取 [ **否** ]。 然後選取 **[下一步]**。
    - 如果 Windows Server 電腦具有 active 軟體保證或 Windows Server 訂用帳戶的涵蓋範圍，請選取 **[是]** ，且他們想要將權益套用至它們所要遷移的機器。 然後選取 **[下一步]**。

1. 在 **計算**中，他們會檢查 VM 名稱、大小、OS 磁片類型和可用性設定組。 VM 必須符合 [Azure 需求](/azure/migrate/migrate-support-matrix-vmware#vmware-requirements)。

    - **VM 大小：** 如果他們使用評量建議，則 [VM 大小] 下拉式清單會包含建議的大小。 否則，Azure Migrate 會根據 Azure 訂用帳戶中最接近的相符項來挑選大小。 或者，他們可以選擇**AZURE VM 大小**的手動大小。
    - **作業系統磁片：** 他們會為 VM 指定作業系統 (開機) 磁片。 OS 磁碟是具有作業系統開機載入器和安裝程式的磁碟。
    - **可用性設定組：** 如果 VM 在遷移後應位於 Azure 可用性設定組中，則會指定該集合。 此集合必須在為遷移所指定的目標資源群組中。

1. 在 **磁片**中，它們會指定是否應將 VM 磁片複寫至 Azure。 然後，他們會在 Azure 中選取 (標準 SSD/HDD 或 premium 受控磁片) 的磁片類型，然後選取 **[下一步]**。
    - 它們可以將磁片從複寫中排除。
    - 如果將磁片排除，則在遷移後將不會出現在 Azure VM 上。

1. 在 [ **檢查 + 開始**複寫] 中，他們會檢查設定。 然後 **，他們會選取 [** 複寫] 以啟動伺服器的初始複寫。

> [!NOTE]
> 複寫設定可以在複寫開始之前的任何時間更新，以**管理**複寫  >  **機器**。 在複寫啟動後，就無法變更設定。

## <a name="step-7-migrate-the-database-via-azure-database-migration-service"></a>步驟7：透過 Azure 資料庫移轉服務遷移資料庫

Contoso 管理員會依照 [逐步進行遷移教學](/azure/dms/tutorial-sql-server-azure-sql-online)課程，透過 Azure 資料庫移轉服務來遷移資料庫。 他們可以執行線上、離線和混合式 (預覽版) 的遷移。

總而言之，他們必須執行下列工作：

- 使用 Premium 定價層來建立連接到虛擬網路的 Azure 資料庫移轉服務實例。
- 確定實例可以透過虛擬網路存取遠端 SQL Server。 請確定 Azure 中允許的所有連入埠都能在虛擬網路層級、網路 VPN 和裝載 SQL Server 的電腦上 SQL Server。
- 設定實例：
  - 建立遷移專案。
  - 將來源 (內部部署資料庫) 。
  - 選取目標。
  - 選取要遷移的資料庫。
  - 設定 [advanced settings]。
  - 開始複寫。
  - 解決任何錯誤。
  - 執行最後的轉換。

## <a name="step-8-protect-the-database-with-sql-server-always-on"></a>步驟8：使用 SQL Server Always On 保護資料庫

在上執行的應用程式資料庫 `SQLAOG1` 中，Contoso 管理員現在可以使用 Always On 可用性群組來保護它。 他們使用 SQL Server Management Studio 設定 SQL Server Always On，然後使用 Windows 叢集指派接聽程式。

### <a name="create-an-always-on-availability-group"></a>建立 Always On 可用性群組

1. 在 SQL Server Management Studio 中，他們會選取並按住 (，或以滑鼠右鍵按一下) **Always On 高可用性** ]，以啟動 [ **新增可用性群組] Wizard**。
1. 在 [ **指定選項**] 中，它們會為可用性群組命名 `SHAOG` 。 在 [ **選取資料庫**] 中，他們會選取 `SmartHotel360` 資料庫。

    ![顯示 [選取資料庫] 窗格的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/aog-1.png)

1. 在 [ **指定複本**] 中，它們會將兩個 SQL 節點新增為可用性複本，並將它們設定為使用同步認可來提供自動容錯移轉。

     ![顯示 [複本] 索引標籤的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/aog-2.png)

1. 他們會設定群組 (`SHAOG`) 和埠的接聽程式。 內部負載平衡器的 IP 位址會以靜態 IP 位址的形式新增 (`10.245.40.100`) 。

    ![顯示 [建立可用性群組接聽程式] 選項的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/aog-3.png)

1. 在 [選取資料同步處理]**** 中，他們啟用自動植入。 使用這個選項時，SQL Server 會自動為群組中的每個資料庫建立次要複本，所以 Contoso 不需要手動備份和還原它們。 驗證之後，會建立可用性群組。

    ![顯示 Always On 可用性群組已建立的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/aog-4.png)

1. Contoso 在建立群組時發生問題。 它不是使用 Active Directory Windows 整合式安全性，而且必須授與許可權給 SQL 登入，以建立 Windows 容錯移轉叢集角色。

    ![顯示授與許可權給 SQL 登入的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/aog-5.png)

1. 群組建立之後，就會出現在 SQL Server Management Studio 中。

### <a name="configure-a-listener-on-the-cluster"></a>在叢集上設定接聽程式

在設定 SQL 部署的最後一個步驟中，Contoso 管理員會將內部負載平衡器設定為叢集上的接聽程式，並使接聽程式上線。 他們使用腳本來執行這項工作。

![顯示叢集接聽程式的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/cluster-listener.png)

### <a name="verify-the-configuration"></a>驗證組態

在所有設定完成後，Contoso 此時在使用移轉後資料庫的 Azure 中已有可運作的可用性群組。 系統管理員會藉由連接到 SQL Server Management Studio 中的內部負載平衡器來驗證設定。

![顯示內部負載平衡器連線的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/ilb-connect.png)

**需要其他協助嗎？**

- 瞭解如何建立[可用性群組](/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#create-the-availability-group)和接聽[程式。](/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#configure-listener)
- 手動[設定叢集以使用負載平衡器 IP 位址](/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener#configure-the-cluster-to-use-the-load-balancer-ip-address)。
- 深入瞭解如何 [建立和使用 SAS](/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2)。

## <a name="step-9-migrate-the-vm-with-azure-migrate"></a>步驟9：使用 Azure Migrate 遷移 VM

Contoso 管理員會執行快速測試容錯移轉，然後遷移 VM。

### <a name="run-a-test-migration"></a>執行測試移轉

執行測試遷移有助於確保一切在遷移前都能如預期般運作。 Contoso 管理員：

1. 執行測試容錯移轉至最新的可用時間點 (`Latest processed`) 。
1. 選取 [ **先將機器關機再開始容錯移轉** ]，讓 Azure Migrate 嘗試先關閉來源 VM，再觸發容錯移轉。 即使關機失敗，仍會繼續容錯移轉。
1. 測試容錯移轉會執行：

    - 系統會執行先決條件檢查，以確保所有移轉所需的條件都已準備就緒。
    - 容錯移轉會處理資料，以便建立 Azure VM。 如果選取了最新的復原點，則會根據資料建立復原點。
    - 使用上一個步驟中處理的資料建立 Azure VM。

1. 容錯移轉完成後，Azure VM 複本會出現在 Azure 入口網站中。 他們會確認 VM 為適當的大小、其已連線到正確的網路，而且正在執行中。
1. 確認之後，他們可以清除容錯移轉，並記錄與儲存任何觀察到的結果。

### <a name="run-a-failover"></a>執行容錯移轉

1. 驗證測試容錯移轉如預期般運作之後，他們會建立復原計畫來進行遷移，並新增 `WEBVM` 至方案。

     ![顯示建立復原方案的螢幕擷取畫面](./media/contoso-migration-rehost-vm-sql-ag/recovery-plan.png)

1. 他們根據此計畫執行容錯移轉。 他們會選取最新的復原點。 他們指定 Azure Migrate 應該在觸發容錯移轉之前，先嘗試關閉內部部署 VM。

    ![顯示 [容錯移轉] 窗格的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/failover1.png)

1. 容錯移轉之後，他們可以確認 Azure VM 會如預期般出現在 Azure 入口網站中。

    ![顯示虛擬機器的 [總覽] 窗格的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/failover2.png)

1. 在 Azure 中確認 VM 之後，他們會完成遷移以完成遷移程式、停止 VM 複寫，以及停止 VM 的 Azure Migrate 計費。

    ![顯示完整遷移專案的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/failover3.png)

### <a name="update-the-connection-string"></a>更新連接字串

在遷移程式的最後一個步驟中，Contoso 管理員會將應用程式的連接字串更新為指向在接聽程式上執行的遷移後資料庫 `SHAOG` 。 這項設定會在現在於 Azure 中執行的上變更 `WEBVM` 。 這項設定位於 `web.config` ASP.NET 應用程式的中。

1. Contoso 管理員會在中找到檔案 `C:\inetpub\SmartHotelWeb\web.config` ，並變更伺服器的名稱，以反映 Always On 可用性群組的 FQDN： `shaog.contoso.com` 。

    ![顯示 Always On 可用性群組之 FQDN 的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-ag/failover4.png)

1. 更新檔案並加以儲存後，就會重新開機 IIS `WEBVM` 。 它們會 `iisreset /restart` 從命令提示字元使用。
1. 重新開機 IIS 之後，應用程式現在會使用在受控實例上執行的資料庫。

**需要其他協助嗎？**

- 瞭解如何 [執行測試容錯移轉](/azure/site-recovery/tutorial-dr-drill-azure)。
- 瞭解如何 [建立復原方案](/azure/site-recovery/site-recovery-create-recovery-plans)。
- 瞭解如何 [容錯移轉至 Azure](/azure/site-recovery/site-recovery-failover)。

### <a name="clean-up-after-migration"></a>移轉之後進行清除

在遷移之後，SmartHotel360 應用程式會在 Azure VM 上執行。 SmartHotel360 資料庫位於 Azure SQL 叢集中。

現在，Contoso 必須完成這些清除步驟：

- 從 vCenter 清查中移除內部部署 VM。
- 從本機備份作業中移除 VM。
- 更新內部文件以顯示 VM 的新位置和 IP 位址。
- 檢閱與已解除委任 VM 互動的任何資源。 更新任何相關的設定或文件，以反映新的組態。
- 新增兩個新的 Vm (`SQLAOG1` ，並 `SQLAOG2`) 到生產監視系統。

### <a name="review-the-deployment"></a>檢閱部署

對於 Azure 中的遷移後資源，Contoso 必須能執行一切功能並保護其新的基礎結構。

### <a name="security"></a>安全性

Contoso 安全性小組會檢查虛擬機器 `WEBVM` 、 `SQLAOG1` 和， `SQLAOG2` 以判斷任何安全性問題。 他們必須：

- 檢查 (Nsg) 的網路安全性群組，讓 VM 控制存取權。 NSG 可用來確保只可以傳遞該應用程式允許的流量。
- 請考慮使用 Azure 磁碟加密和 Azure Key Vault 來保護磁片上的資料。
- 評估透明資料加密。 然後在新的 Always On 可用性群組上執行的 SmartHotel360 資料庫上啟用它。 深入瞭解 [透明資料加密](/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017)。

如需詳細資訊，請參閱 [Azure 中 IaaS 工作負載的安全性最佳作法](/azure/security/fundamentals/iaas)。

## <a name="business-continuity-and-disaster-recovery"></a>商務持續性和災害復原

針對商務持續性和災害復原，Contoso 會採取下列動作：

- 為了保護資料安全，Contoso 會透過 `WEBVM` `SQLAOG1` `SQLAOG2` [Azure VM 備份](/azure/backup/backup-azure-vms-introduction)來備份、和 vm 上的資料。
- Contoso 也將瞭解如何使用 Azure 儲存體將 SQL Server 直接備份至 Azure Blob 儲存體。 深入瞭解如何 [使用 Azure 儲存體來 SQL Server 備份和還原](/azure/virtual-machines/windows/sql/virtual-machines-windows-use-storage-sql-server-backup-restore)。
- 為了讓應用程式保持運作，Contoso 會使用 Site Recovery 將 Azure 中的應用程式 Vm 複寫至次要區域。 深入瞭解如何 [針對 AZURE VM 設定損毀修復至次要 azure 區域](/azure/site-recovery/azure-to-azure-quickstart)。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- Contoso 擁有其 WEBVM 的現有授權，而且會利用 Azure Hybrid Benefit。 Contoso 會轉換現有的 Azure VM，以便充分利用這個定價。
- Contoso 會使用 [Azure 成本管理和帳單](/azure/cost-management-billing/cost-management-billing-overview) 來確保公司維持在 IT 領導階層所建立的預算內。

## <a name="conclusion"></a>結論

在本文中，Contoso 會使用 Azure Migrate 將應用程式前端 VM 遷移至 Azure，以在 Azure 中重新裝載 SmartHotel360 應用程式。 Contoso 會使用 Azure 資料庫移轉服務，將應用程式資料庫移轉至在 Azure 中布建的 SQL Server 叢集，並在 SQL Server Always On 可用性群組中加以保護。
