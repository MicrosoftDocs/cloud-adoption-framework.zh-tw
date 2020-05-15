---
title: 藉由將應用程式遷移至 Azure Vm 和 SQL Server 來進行重新裝載 Always On 可用性群組
description: 了解 Contoso 如何將內部部署應用程式移轉至 Azure VM 和 SQL Server Always On 可用性群組，以重新裝載該應用程式。
author: givenscj
ms.author: abuck
ms.date: 04/02/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: azure-migrate
ms.openlocfilehash: c356554dbdca417708d7eb9698d9729270d8e981
ms.sourcegitcommit: 5d6a7610e556f7b8ca69960ba76a3adfa9203ded
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2020
ms.locfileid: "83401012"
---
<!-- cSpell:ignore givenscj WEBVM SQLVM contosohost vcenter contosodc AOAG SQLAOG SQLAOGAVSET contosoadmin contosocloudwitness MSSQLSERVER BEPOOL contosovmsacc SHAOG NSGs inetpub iisreset -->

# <a name="rehost-an-on-premises-app-with-azure-virtual-machines-and-sql-server-always-on-availability-groups"></a>使用 Azure 虛擬機器和 SQL Server 重新裝載內部部署應用程式 Always On 可用性群組

本文示範虛構公司 Contoso 如何在遷移至 Azure 的過程中，重新裝載在 VMware 虛擬機器（Vm）上執行的兩層式 Windows .NET 應用程式。 Contoso 會將應用程式前端 VM 移轉至 Azure VM，並將應用程式資料庫移轉至 Azure SQL Server VM，在具有 SQL Server AlwaysOn 可用性群組的 Windows Server 容錯移轉叢集中執行。

此範例中使用的 SmartHotel360 應用程式以開放原始碼的形式提供。 如果想將它用於自己的測試目的，您可以從 [github](https://github.com/Microsoft/SmartHotel360) 進行下載。

## <a name="business-drivers"></a>商業動機

IT 領導小組與商務合作夥伴密切合作，以了解此次移轉所要實現的目標：

- **解決業務成長。** Contoso 的業務量日益增多，對內部部署系統和基礎結構造成了壓力。

- **提高效率。** Contoso 必須移除不必要的程序，並且簡化開發人員和使用者的程序。 企業亟需快速且不浪費時間或金錢的 IT 服務，進而更快滿足客戶的需求。

- **增加靈活性。** Contoso IT 必須能夠更快因應企業的需求。 其因應速度必須能夠比市場變化更快，才能更在全球經濟中獲致成功。 它不得礙事，或成為企業的絆腳石。

- **尺度.** 隨著企業順利成長，Contoso IT 必須提供能夠同步成長的系統。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已針對此次移轉擬定好各項目標。 這些目標用來判斷最合適的移轉方法：

- 移轉之後，應用程式不管是在 Azure 或 VMware 中，應具有相同效能。 應用程式不管是在雲端中或在內部部署，都一樣重要。

- Contoso 不想要投資此應用程式。 這對企業很重要，但以其目前的形式而言，Contoso 只想安全地移至雲端。

- 應用程式的內部部署資料庫有可用性方面問題。 Contoso 想要將其部署於 Azure 中，作為具有容錯移轉能力的高可用性叢集。

- Contoso 想要從目前的 SQL Server 2008 R2 平台升級至 SQL Server 2017。

- Contoso 不想對此應用程式使用 Azure SQL Database，且正在尋求替代項目。

## <a name="solution-design"></a>解決方案設計

擬定好目標和需求後，Contoso 會設計和檢閱部署解決方案，並識別移轉程序，包括將用於移轉的 Azure 服務。

### <a name="current-architecture"></a>目前架構

- 應用程式分層至兩個 VM (WEBVM 和 SQLVM)。
- Vm 位於 VMware ESXi 主機**contosohost1.contoso.com** （版本6.5）。
- VMware 環境是由在 VM 上執行的 vCenter Server 6.5 （**vcenter.contoso.com**）所管理。
- Contoso 有內部部署資料中心 (contoso-datacenter) 以及內部部署網域控制站 (**contosodc1**)。

### <a name="proposed-architecture"></a>建議的架構

在此情節中：

- Contoso 會將應用程式前端 WEBVM 移轉至 Azure IaaS VM。
  - Azure 中的前端 VM 將部署在 ContosoRG 資源群組中 (用於生產資源)。
  - 它將位於美國東部 2 主要區域的 Azure 生產網路中 (VNET-PROD-EUS2)。
- 應用程式資料庫將移轉至 Azure SQL Server VM。
  - 它將位於 Contoso 在美國東部 2 主要區域的 Azure 資料庫網路中 (PROD-DB-EUS2)。
  - 它將放置在具有兩個節點、且使用 SQL Server Always On 可用性群組的 Windows Server 容錯移轉叢集中。
  - 在 Azure 中，叢集中的兩個 SQL Server VM 節點將會部署在 ContosoRG 資源群組中。
  - 這些 VM 節點將位於美國東部 2 主要區域的 Azure 生產網路中 (VNET-PROD-EUS2)。
  - VM 會執行具有 SQL Server 2017 Enterprise Edition 的 Windows Server 2016。 Contoso 不具備此作業系統的授權，因此將使用 Azure Marketplace 中以 Azure EA 承諾用量計費的形式提供授權的映像。
  - 除了唯一名稱以外，這兩個 VM 所使用的設定皆相同。
- Contoso 會部署內部負載平衡器，以接聽叢集上的流量，並將其導向至適當的叢集節點。
  - 內部負載平衡器將部署在 ContosoNetworkingRG 中 (用於網路資源)。
- 移轉完成之後，將會解除委任 Contoso 資料中心的內部部署 VM。

    ![案例架構](./media/contoso-migration-rehost-vm-sql-ag/architecture.png)

### <a name="database-considerations"></a>資料庫考量

在解決方案設計過程中，Contoso 會進行 Azure SQL Database 與 SQL Server 的功能比較。 下列考量協助他們決定採用執行 SQL Server 的 Azure IaaS VM：

- 如果 Contoso 需要自訂作業系統或資料庫伺服器，或者想要在相同的 VM 上共置並執行第三方應用程式，使用執行 SQL Server 的 Azure VM 似乎是最佳解決方案。

### <a name="solution-review"></a>解決方案檢閱

Contoso 會透過比較一份優缺點清單，來評估其建議設計。

<!-- markdownlint-disable MD033 -->

**考量** | **詳細資料**
--- | ---
**優點** | WEBVM 會移至 Azure (不需變更)，讓移轉變得更簡單。 <br><br> SQL Server 層將會在 SQL Server 2017 和 Windows Server 2016 上執行。 這會淘汰其目前的 Windows Server 2008 R2 作業系統，且執行 SQL Server 2017 將可支援 Contoso 的技術需求和目標。 IT 從 SQL Server 2008 R2 進行移轉時，可提供 100% 的相容性。 <br><br> Contoso 可以使用 Azure Hybrid Benefit，充分發揮軟體保證的投資效益。 <br><br> Azure 中高可用性 SQL Server 部署可提供容錯功能，使應用程式資料層不再是單一的容錯移轉點。
**缺點** | WEBVM 會執行 Windows Server 2008 R2。 Azure 對此作業系統的支援僅限於特定角色 (2018 年 7 月)。 [深入了解](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)。 <br><br> 應用程式的 Web 層會保留單一容錯移轉點。 <br><br> Contoso 必須繼續支持此 Web 層作為 Azure VM，而非移轉至 Azure App Service 等受控服務。 <br><br> 透過選擇的解決方案，Contoso 必須繼續管理兩個 SQL Server VM，而非移轉至受控平台，例如 Azure SQL Database 受控執行個體。 此外，Contoso 可透過軟體保證，以折扣優惠在 Azure SQL Database 受控執行個體上交換其現有的授權。

<!-- markdownlint-enable MD033 -->

### <a name="azure-services"></a>Azure 服務

**服務** | **描述** | **成本**
--- | --- | ---
[Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) | Azure 資料庫移轉服務能夠從多個資料庫來源無縫移轉到 Azure 資料平台，將停機時間降到最低。 | 深入了解[支援的區域](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability)和[資料庫移轉服務定價](https://azure.microsoft.com/pricing/details/database-migration)。
[Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-services-overview) | Contoso 會使用 Azure Migrate 服務來評定其 VMware VM。 Azure Migrate 會評定機器是否適合移轉。 它會提供在 Azure 中執行的大小調整建議和成本估計。 | 截至 2018 年 5 月，Azure Migrate 是一項免費服務。

## <a name="migration-process"></a>移轉程序

Contoso 管理員會將應用程式 VM 移轉至 Azure。

- 他們會使用 Azure Migrate 將前端 VM 遷移至 Azure VM：
  - 在第一個步驟中，他們會準備以及設定 Azure 元件，然後準備內部部署 VMware 基礎結構。
  - 一切就緒後，他們就可以開始複寫 VM。
  - 複寫功能啟用且正常運作後，他們會將 VM 容錯移轉至 Azure，而加以移轉。
- 一旦使用者驗證了資料庫，就會使用資料移轉服務（DMS），將資料庫移轉至 Azure 中的 SQL Server 叢集。
  - 在第一個步驟中，他們必須在 Azure 中佈建 SQL Server VM、設定叢集與內部負載平衡器，並設定 Always On 可用性群組。
  - 這麼做之後，他們就可以遷移資料庫。
- 移轉完成後，他們會為資料庫啟用 Always On 保護。

    ![移轉程序](./media/contoso-migration-rehost-vm-sql-ag/migration-process.png)

## <a name="prerequisites"></a>先決條件

以下是 Contoso 在此案例中應該準備好的事項。

<!-- markdownlint-disable MD033 -->

| **需求** | **詳細資料** |
| --- | --- |
| **Azure 訂用帳戶** | Contoso 已在本系列稍早的文章中建立訂用帳戶。 如果您沒有 Azure 訂用帳戶，請建立[免費帳戶](https://azure.microsoft.com/pricing/free-trial)。 <br><br> 如果您建立免費帳戶，您就是訂用帳戶的管理員，並可執行所有動作。 <br><br> 如果您使用現有訂用帳戶，而且您不是系統管理員，則需要與系統管理員合作，讓其指派擁有者或參與者權限給您。 |
| **Azure 基礎結構** | Contoso 會如[適用於移轉的 Azure 基礎結構](./contoso-migration-infrastructure.md)中所述，設定 Azure 基礎結構。 <br><br> 深入瞭解 Azure Migrate 的特定[必要條件](https://docs.microsoft.com/azure/migrate/contoso-migration-rehost-linux-vm#prerequisites)需求：伺服器遷移。 |
| **內部部署伺服器** | 內部部署 vCenter 伺服器應執行版本5.5、6.0、6.5 或6.7。 <br><br> 執行5.5、6.0、6.5 或6.7 版本的 ESXi 主機。 <br><br> 一或多部在 ESXi 主機上執行的 VMware VM。 |
| **內部部署 VM** | 檢閱已背書在 Azure 上執行的 [Linux 機器](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)。 |

<!-- markdownlint-enable MD033 -->

## <a name="scenario-steps"></a>案例步驟

以下是 Contoso 執行移轉的方式：

> [!div class="checklist"]
>
> - **步驟1：準備 AOAG 叢集。** 建立叢集，以在 Azure 中部署兩個 SQL Server VM 節點。
> - **步驟2：部署並設定叢集。** 準備 Azure SQL Server 叢集。 資料庫會移轉至這個現有的叢集中。
> - **步驟3：部署 Azure Load Balancer。** 部署負載平衡器，以平衡傳輸至 SQL Server 節點的流量。
> - **步驟4：準備 Azure 以進行 Azure Migrate。** 建立 Azure 儲存體帳戶來保存複寫的資料。
> - **步驟5：準備內部部署 VMware 以進行 Azure Migrate。** 為帳戶進行 VM 探索和代理程式安裝的準備工作。 準備內部部署 VM，讓使用者在移轉之後可連線至 Azure VM。
> - **步驟6：複寫 Vm。** 啟用對 Azure 複寫 VM 的功能。
> - **步驟7：遷移具有資料移轉服務（DMS）的資料庫。** 使用資料移轉服務將資料庫移轉至 Azure。
> - **步驟8：保護資料庫。** 為叢集建立 Always On 可用性群組。
> - **步驟9：使用 Azure Migrate 遷移 vm**執行測試容錯移轉，確認一切如預期般運作。 然後，執行對 Azure 的完整容錯移轉。

## <a name="step-1-prepare-a-sql-server-always-on-availability-group-cluster"></a>步驟1：準備 SQL Server Always On 可用性群組叢集

Contoso 管理員會依照下列方式設定叢集：

1. 他們在 Azure Marketplace 中選取 SQL Server 2017 Enterprise Windows Server 2016 映像，以建立兩個 SQL Server VM。

    ![SQL VM SKU](./media/contoso-migration-rehost-vm-sql-ag/sql-vm-sku.png)

2. 在**建立虛擬機器嚮導**  >  **基本概念**中，他們會設定：

    - VM 的名稱：**SQLAOG1** 和 **SQLAOG2**。
    - 由於機器是攸關業務的重要元件，因此其 VM 磁碟類型可以是 SSD。
    - 他們指定機器認證。
    - 他們在主要美國東部 2 區域，將 VM 部署在 ContosoRG 資源群組中。

3. 在 [大小]**** 中，他們先為兩個 VM 都設定 D2s_V3 SKU。 其後他們將視需要加以調整。
4. 在 [設定]**** 中，他們執行下列作業：

    - 由於這些 VM 對應用程式而言都是重要的資料庫，因此他們使用受控磁碟。
    - 他們將機器放在美國東部 2 主要區域的生產網路中 (**VNET-PROD-EUS2**)，置於資料庫子網路內 (**PROD-DB-EUS2**)。
    - 他們建立新的可用性設定組：**SQLAOGAVSET**，具有兩個容錯網域和五個更新網域。

      ![SQL VM](./media/contoso-migration-rehost-vm-sql-ag/sql-vm-settings.png)

5. 在 [SQL Server 設定]**** 中，他們將 SQL 對虛擬網路 (私人) 的連線限定於預設連接埠 1433。 針對驗證，他們會使用與現場使用的相同認證（**contosoadmin**）。

    ![SQL VM](./media/contoso-migration-rehost-vm-sql-ag/sql-vm-db.png)

**需要其他協助？**

- [取得說明](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision#1-configure-basic-settings)以了解如何佈建 SQL Server VM。
- [了解](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-prereq#create-sql-server-vms)如何為不同的 SQL Server SKU 設定 VM。

## <a name="step-2-deploy-and-set-up-the-cluster"></a>步驟 2︰部署並設定叢集

以下是 Contoso 管理員設定叢集的方式：

1. 他們設定 Azure 儲存體帳戶作為雲端見證。
2. 他們將 SQL Server VM 新增至 Contoso 內部部署資料中心的 Active Directory 網域。
3. 他們在 Azure 中建立叢集。
4. 他們設定雲端見證。
5. 最後，他們啟用 SQL Always On 可用性群組。

### <a name="set-up-a-storage-account-as-cloud-witness"></a>將儲存體帳戶設定為雲端見證

為了設定雲端見證，Contoso 必須要有 Azure 儲存體帳戶，用來保存用於叢集仲裁的 Blob 檔案。 相同的儲存體帳戶也可用來設定多個叢集的雲端見證。

Contoso 管理員會建立儲存體帳戶，如下所示：

1. 他們為帳戶指定可辨識的名稱 (**contosocloudwitness**)。
2. 他們部署具有 LRS 的一般通用帳戶。
3. 他們會將帳戶放在第三個區域：美國中南部。 它們會將它放在主要和次要區域之外，讓它在區域失敗時仍可供使用。
4. 他們將它置於用來保存基礎結構資源的資源群組 **ContosoInfraRG**。

    ![雲端見證](./media/contoso-migration-rehost-vm-sql-ag/witness-storage.png)

5. 當他們建立儲存體帳戶時，系統會產生該帳戶的主要和次要存取金鑰。 他們必須要有主要存取金鑰才能建立雲端見證。 金鑰會出現在儲存體帳戶名稱 > [存取金鑰]**** 下方。

    ![存取金鑰](./media/contoso-migration-rehost-vm-sql-ag/access-key.png)

### <a name="add-sql-server-vms-to-contoso-domain"></a>將 SQL Server VM 新增至 Contoso 網域

1. Contoso 將 SQLAOG1 和 SQLAOG2 新增至 contoso.com 網域。
2. 然後，他們在每個 VM 上安裝 Windows 容錯移轉叢集功能和工具。

### <a name="set-up-the-cluster"></a>設定叢集

在設定叢集之前，Contoso 管理員為每個機器的作業系統磁碟建立快照集。

![建立快照集](./media/contoso-migration-rehost-vm-sql-ag/snapshot.png)

1. 然後，他們執行已整合的指令碼，以建立 Windows 容錯移轉叢集。

    ![建立叢集](./media/contoso-migration-rehost-vm-sql-ag/create-cluster1.png)

2. 在建立叢集之後，他們確認 VM 已呈現為叢集節點。

     ![建立叢集](./media/contoso-migration-rehost-vm-sql-ag/create-cluster2.png)

### <a name="configure-the-cloud-witness"></a>設定雲端見證

1. Contoso 管理員使用容錯移轉叢集管理員中的**仲裁設定精靈**來設定雲端見證。
2. 在嚮導中，他們會選擇使用儲存體帳戶建立雲端見證。
3. 雲端見證設定後，會出現在容錯移轉叢集管理員嵌入式管理單元中。

    ![雲端見證](./media/contoso-migration-rehost-vm-sql-ag/cloud-witness.png)

### <a name="enable-sql-server-always-on-availability-groups"></a>啟用 SQL Server Always On 可用性群組

Contoso 管理員現在已可啟用 Always On：

1. 在 SQL Server 組態管理員中，他們為 **SQL Server (MSSQLSERVER)** 服務啟用 **Always On 可用性群組**。

    ![啟用 Always On](./media/contoso-migration-rehost-vm-sql-ag/enable-always-on.png)

2. 他們重新啟動服務讓變更生效。

在 Always On 啟用後，Contoso 可以設定將會保護 SmartHotel360 資料庫的 Always On 可用性群組。

**需要其他協助？**

- [深入了解](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness)雲端見證以及如何為其設定儲存體帳戶。
- [取得指示](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial)以了解如何設定叢集和建立可用性群組。

## <a name="step-3-deploy-the-azure-load-balancer"></a>步驟 3：部署 Azure Load Balancer

Contoso 管理員此時想要部署位於叢集節點前面的內部負載平衡器。 負載平衡器會接聽流量，並將其導向至適當的節點。

![負載平衡](./media/contoso-migration-rehost-vm-sql-ag/architecture-lb.png)

他們依照下列方式建立負載平衡器︰

1. 在 Azure 入口網站 >**網路**  >  **Load Balancer**中，他們會設定新的內部負載平衡器： **ILB-EUS2-ilb-prod-db-eus-sqlaog-bepool**。
2. 他們將負載平衡器放在生產網路 **VNET-PROD-EUS2** 的資料庫子網路 **PROD-DB-EUS2** 中。
3. 他們為其指派靜態 IP 位址：10.245.40.100。
4. 他們在網路資源群組 **ContosoNetworkingRG** 中部署負載平衡器，作為網路元素。

    ![負載平衡](./media/contoso-migration-rehost-vm-sql-ag/lb-create.png)

部署內部負載平衡器後，他們必須加以設定。 他們會建立後端位址集區、設定健康情況探查，以及設定負載平衡規則。

### <a name="add-a-back-end-pool"></a>新增後端集區

為了將流量分散至叢集中的各個 VM，Contoso 管理員設定了後端位址集區，其中包含將會從負載平衡器接收網路流量之 VM 的 NIC IP 位址。

1. 在入口網站的負載平衡器設定中，Contoso 會新增後端集區： **ILB-EUS-ilb-prod-db-eus-sqlaog-bepool-BEPOOL**。
2. 他們建立了該集區與可用性設定組 SQLAOGAVSET 的關聯。 設定組中的 VM (**SQLAOG1** 和 **SQLAOG2**) 會新增至集區。

    ![後端集區](./media/contoso-migration-rehost-vm-sql-ag/backend-pool.png)

### <a name="create-a-health-probe"></a>建立健康狀態探查

Contoso 管理員建立了健康情況探查，讓負載平衡器能夠監視應用程式的健康情況。 此探查會根據 VM 對健康情況檢查的回應，以動態方式從負載平衡器輪替中新增或移除 VM。

他們依照下列方式建立探查：

1. 在入口網站的負載平衡器設定中，Contoso 建立健康情況探查：**SQLAlwaysOnEndPointProbe**。
2. 他們設定探查以監視 TCP 連接埠 59999 上的 VM。
3. 他們在探查間設定 5 秒的間隔，並將閾值設定為 2。 如果有兩個探查失敗，VM 即會被視為狀況不良。

    ![探查](./media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

### <a name="configure-the-load-balancer-to-receive-traffic"></a>設定要接收流量的負載平衡器

此時，Contoso 管理員必須設定負載平衡器規則，以定義如何將流量分散至 VM。

- 前端 IP 位址會處理傳入流量。
- 後端 IP 集區會接收流量。

他們依照下列方式建立規則：

1. 在入口網站的負載平衡器設定中，他們會新增規則： **SQLAlwaysOnEndPointListener**。
2. 他們設定了前端接聽程式，以在 TCP 1433 上接收傳入的 SQL 用戶端流量。
3. 他們指定流量所將路由到的後端集區，以及 VM 用來接聽流量的連接埠。
4. 他們啟用了浮動 IP (伺服器直接回傳)。 SQL Always On 一律需要此功能。

    ![探查](./media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

**需要其他協助？**

- 對 Azure Load Balancer [取得概觀](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)。
- [了解](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-basic-internal-portal)如何建立負載平衡器。

## <a name="step-4-prepare-azure-for-azure-migrate"></a>步驟4：準備適用于 Azure Migrate 的 Azure

以下是 Contoso 部署 Azure Migrate 所需的 Azure 元件：

- 在容錯移轉時建立 VM 時，這些 VM 所在的 VNet。
- 用來保存複寫資料的 Azure 儲存體帳戶。

Contoso 管理員的設定方式如下：

1. Contoso 已建立可在[部署 Azure 基礎結構](./contoso-migration-rehost-vm-sql-ag.md)時用於 Azure Migrate 的網路/子網。

    - SmartHotel360 應用程式是一個生產應用程式，而 WEBVM 將會移轉至美國東部 2 主要區域中的 Azure 生產網路 (VNET-PROD-EUS2)。
    - WEBVM 將會置於 ContosoRG 資源群組中 (用於生產資源)，並且置於生產子網路中 (PROD-FE-EUS2)。

2. Contoso 管理員會在主要區域中建立 Azure 儲存體帳戶 (contosovmsacc20180528)。

    - 他們使用一般用途的帳戶，並配備標準儲存體和 LRS 複寫。

## <a name="step-5-prepare-on-premises-vmware-for-azure-migrate"></a>步驟5：準備內部部署 VMware 以進行 Azure Migrate

以下是 Contoso 管理員為內部部署所做的準備：

- vCenter Server 或 vSphere ESXi 主機上的一個帳戶，用以自動執行 VM 探索。
- 內部部署 VM 設定，讓 Contoso 在容錯移轉之後可以連線至複寫的 Azure VM。

### <a name="prepare-an-account-for-automatic-discovery"></a>準備帳戶以進行自動探索

Azure Migrate 需要 VMware 伺服器的存取權：

- 自動探索 VM。
- 協調複寫、容錯移轉和容錯回復。
- 需要至少一個唯讀帳戶。 您需要可執行建立和移除磁碟以及開啟 VM 等作業的帳戶。

Contoso 管理員會依照下列方式設定帳戶：

1. 他們在 vCenter 層級建立一個角色。
2. 然後，他們會指派必要權限給角色。

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>準備在容錯移轉後連接到 Azure VM

遷移之後，Contoso 會想要連線到 Azure Vm，並允許 Azure 管理 Vm。 為此，Contoso 管理員在移轉之前執行了下列作業：

1. 為了透過網際網路存取，他們會：

    - 請先在內部部署 VM 上啟用 RDP 或 SSH，再進行遷移。
    - 確定已為**公用**設定檔新增 TCP 和 UDP 規則。
    - 檢查作業系統防火牆中是否允許 RDP 或 SSH。

2. 為了透過站對站 VPN 存取，他們：

    - 請先在內部部署 VM 上啟用 RDP 或 SSH，再進行遷移。
    - 檢查作業系統防火牆中是否允許 RDP 或 SSH。
    - 若是 windows，請將內部部署 VM 上的作業系統 SAN 原則設定為**OnlineAll**。

3. 安裝 Azure 代理程式：

    - [Azure Linux 代理程式](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux)
    - [Azure Windows 代理程式](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows)

4. 其他

   - 若是 windows，觸發遷移時，VM 上不應該有擱置中的 Windows 更新。 如果有，在更新完成之前，他們將無法登入 VM。
   - 在遷移之後，他們可以勾選 [**開機診斷**] 以查看 VM 的螢幕擷取畫面。 若未解決問題，他們應確認 VM 是否執行中，並檢閱這些[疑難排解祕訣](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)。

5. 需要其他協助？

   - [瞭解如何](https://docs.microsoft.com/azure/migrate/contoso-migration-rehost-vm#prepare-vms-for-migration)準備 vm 以進行遷移。

## <a name="step-6-replicate-the-on-premises-vms-to-azure"></a>步驟6：將內部部署 Vm 複寫至 Azure

Contoso 管理員必須先設定並啟用複寫，才能執行移轉至 Azure 的作業。

完成探索之後，您就可以開始將 VMware VM 複寫至 Azure。

1. 在 [Azure Migrate 專案] > [伺服器]  、 **[Azure Migrate：伺服器移轉]** 中，按一下 [複寫]  。

    ![複寫 VM](./media/contoso-migration-rehost-vm/select-replicate.png)

2. 在 [複寫]  > [來源設定]   > [您的電腦虛擬化了嗎]  中，選取 [是，使用 VMware vSphere]  。

3. 在 [內部部署設備]  中，選取您設定的 Azure Migrate 設備名稱 > [確定]  。

    ![來源設定](./media/contoso-migration-rehost-vm/source-settings.png)

4. 在 [虛擬機器]  中，選取您要複寫的機器。
    - 如果您已執行 VM 的評估，您可以套用評估結果中的 VM 大小調整和磁碟類型 (進階/標準) 建議。 若要這麼做，請在 [從 Azure Migrate 評估匯入移轉設定?]  中，選取 [是]  選項。
    - 如果您未執行評估，或不想使用評估設定，請選取 [否]  選項。
    - 如果您選擇使用評估，請選取 VM 群組和評估名稱。

    ![選取評估](./media/contoso-migration-rehost-vm/select-assessment.png)

5. 在 [虛擬機器]  中，視需要搜尋 VM，並檢查您要遷移的每個 VM。 然後按 [下一步：  目標設定]。

6. 在 [目標設定]  中，選取訂用帳戶、您的遷移目標區域，並指定 Azure VM 在移轉後所在的資源群組。 在 [虛擬網路]  中，選取 Azure VM 在移轉後所將加入的 Azure VNet/子網路。

7. 在 [ **Azure Hybrid Benefit**中，選取下列各項：

    - 如果您不想套用 Azure Hybrid Benefit，請選取 [否]  。 然後按一下 [下一步]  。
    - 如果您有 Windows Server 機器涵蓋於有效的軟體保證或 Windows Server 訂用帳戶下，且您想要將權益套用至要移轉的機器，請選取 [是]  。 然後按一下 [下一步]  。

8. 在 [計算]  中，檢閱 VM 名稱、大小、OS 磁碟類型和可用性設定組。 VM 必須符合 [Azure 需求](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#vmware-requirements)。

    - **VM 大小：** 如果您使用評估建議，[VM 大小] 下拉式清單會包含建議的大小。 否則，Azure Migrate 會根據 Azure 訂用帳戶中最接近的相符項來選擇大小。 或者，您可以在 [Azure VM 大小]  中手動選擇大小。
    - **OS 磁片：** 指定 VM 的 OS （開機）磁片。 OS 磁碟是具有作業系統開機載入器和安裝程式的磁碟。
    - **可用性設定組：** 如果 VM 在遷移後應位於 Azure 可用性設定組中，請指定集合。 此設定組必須位於您為移轉指定的目標資源群組中。

9. 在 [磁碟]  中，指定是否應將 VM 磁碟複寫至 Azure，並選取 Azure 中的磁碟類型 (標準 SSD/HDD 或進階受控磁碟)。 然後按一下 [下一步]  。
    - 您可以從複寫排除磁碟。
    - 如果您排除磁碟，則在移轉後磁碟將不會出現在 Azure VM 上。

10. 在 [檢閱並啟動複寫]  中檢閱設定，然後按一下 [複寫]  以啟動伺服器的初始複寫。

> [!NOTE]
> 您可以在複寫開始之前隨時更新複寫設定 (經由 [管理]   > [複寫機器]  )。 在複寫啟動後，就無法變更設定。

## <a name="step-7-migrate-the-database-with-azure-database-migration-service-dms"></a>步驟7：使用 Azure 資料庫移轉服務（DMS）遷移資料庫

Contoso 管理員會使用 Azure 資料庫移轉服務（DMS），透過[逐步遷移教學課程來](https://docs.microsoft.com/azure/dms/tutorial-sql-server-azure-sql-online)遷移 it。 他們可以執行線上、離線和混合式（預覽）的遷移。

總而言之，您必須執行下列動作：

- 使用 `Premium` 連線到 VNet 的 SKU 來建立 Azure 資料庫移轉服務（DMS）。
- 確定 Azure 資料庫移轉服務（DMS）可以透過虛擬網路存取遠端 SQL Server。 這會需要確保所有連入埠都可從 Azure 允許在虛擬網路層級、網路 VPN 和裝載 SQL Server 的電腦上 SQL Server。
- 設定 Azure 資料庫移轉服務：
  - 建立遷移專案。
  - 新增來源（內部部署資料庫）。
  - 選取目標。
  - 選取要遷移的資料庫。
  - 設定 [高級設定]。
  - 開始複寫。
  - 解決任何錯誤。
  - 執行最後的轉換。

## <a name="step-8-protect-the-database-with-always-on"></a>步驟8：使用 Always On 保護資料庫

在應用程式資料庫執行於 **SQLAOG1** 的情況下，Contoso 管理員現在可以使用 Always On 可用性群組加以保護。 他們使用 SQL Management Studio 設定 Always On，然後使用 Windows 叢集指派接聽程式。

### <a name="create-an-always-on-availability-group"></a>建立 Always On 可用性群組

1. 在 SQL Management Studio 中，他們以滑鼠右鍵按一下 [Always On 高可用性]****，以啟動 [新增可用性群組精靈]****。
2. 在 [指定選項]**** 中，他們將可用性群組命名為 **SHAOG**。 在 [選取資料庫]**** 中，他們選取 SmartHotel360 資料庫。

    ![Always On 可用性群組](./media/contoso-migration-rehost-vm-sql-ag/aog-1.png)

3. 在 [指定複本]**** 中，他們新增兩個 SQL 節點作為可用性複本，並將其設定為透過同步認可提供自動容錯移轉。

     ![Always On 可用性群組](./media/contoso-migration-rehost-vm-sql-ag/aog-2.png)

4. 他們設定群組 (**SHAOG**) 和連接埠的接聽程式。 內部負載平衡器的 IP 位址會新增為靜態 IP 位址 (10.245.40.100)。

    ![Always On 可用性群組](./media/contoso-migration-rehost-vm-sql-ag/aog-3.png)

5. 在 [選取資料同步處理]**** 中，他們啟用自動植入。 透過此選項，SQL Server 會自動為群組中的每個資料庫中建立次要複本，因此 Contoso 不需要手動加以備份和還原。 驗證之後，會建立可用性群組。

    ![Always On 可用性群組](./media/contoso-migration-rehost-vm-sql-ag/aog-4.png)

6. Contoso 在建立群組時發生問題。 他們未使用 Active Directory Windows 整合式安全性，因此需要授與權限給 SQL 登入，以建立 Windows 容錯移轉叢集角色。

    ![Always On 可用性群組](./media/contoso-migration-rehost-vm-sql-ag/aog-5.png)

7. 群組建立後，Contoso 可以在 SQL Management Studio 中加以檢視。

### <a name="configure-a-listener-on-the-cluster"></a>在叢集上設定接聽程式

在設定 SQL 部署的最後一個步驟中，Contoso 管理員將內部負載平衡器設定為叢集上的接聽程式，並使接聽程式上線。 他們使用指令碼執行此作業。

![叢集接聽程式](./media/contoso-migration-rehost-vm-sql-ag/cluster-listener.png)

### <a name="verify-the-configuration"></a>驗證組態

在所有設定完成後，Contoso 此時在使用移轉後資料庫的 Azure 中已有可運作的可用性群組。 管理員可藉由在 SQL Management Studio 中連線至內部負載平衡器來確認這一點。

![ILB 連線](./media/contoso-migration-rehost-vm-sql-ag/ilb-connect.png)

**需要其他協助？**

- 了解如何建立[可用性群組](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#create-the-availability-group)和[接聽程式](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#configure-listener)。
- 手動[設定叢集以使用負載平衡器 IP 位址](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener#configure-the-cluster-to-use-the-load-balancer-ip-address)。
- [深入了解](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2)如何建立和使用 SAS。

## <a name="step-9-migrate-the-vm-with-azure-migrate"></a>步驟9：使用 Azure Migrate 遷移 VM

Contoso 管理員會執行快速測試容錯移轉，然後移轉 VM。

### <a name="run-a-test-failover"></a>執行測試容錯移轉

在移轉之前執行測試容錯移轉，有助於確保一切運作正常。

1. 他們會執行測試容錯移轉至最新的可用時間點（**最近處理**的）。
2. 他們會選取 [**先將機器關機再開始容錯移轉**]，讓 Azure Migrate 在觸發容錯移轉之前，先嘗試關閉來源 VM。 即使關機失敗，仍會繼續容錯移轉。
3. 執行測試容錯移轉：

    - 系統會執行先決條件檢查，以確保所有移轉所需的條件都已準備就緒。
    - 容錯移轉會處理資料，以便建立 Azure VM。 如果他們選取最新的復原點，就會從資料建立復原點。
    - 將會使用先前步驟中處理的資料來建立 Azure VM。

4. 容錯移轉完成後，Azure VM 複本會出現在 Azure 入口網站中。 他們會確認 VM 為適當的大小、其已連線到正確的網路，而且正在執行中。
5. 確認之後，他們可以清除容錯移轉，並記錄與儲存任何觀察到的結果。

### <a name="run-a-failover"></a>執行容錯移轉

1. 驗證測試容錯移轉如預期般正常運作之後，Contoso 管理員會建立移轉的復原計畫，並將 WEBVM 新增至計畫。

     ![復原計畫](./media/contoso-migration-rehost-vm-sql-ag/recovery-plan.png)

2. 他們根據此計畫執行容錯移轉。 他們會選取最新的復原點，並指定 Azure Migrate 應該在觸發容錯移轉之前，先嘗試關閉內部部署 VM。

    ![容錯移轉](./media/contoso-migration-rehost-vm-sql-ag/failover1.png)

3. 容錯移轉之後，他們可以確認 Azure VM 會如預期般出現在 Azure 入口網站中。

    ![復原計畫](./media/contoso-migration-rehost-vm-sql-ag/failover2.png)

4. 在 Azure 中確認 VM 之後，他們會完成遷移以完成遷移程式、停止 VM 的複寫，以及停止 VM 的 Azure Migrate 計費。

    ![容錯移轉](./media/contoso-migration-rehost-vm-sql-ag/failover3.png)

### <a name="update-the-connection-string"></a>更新連接字串

在移轉程序的最後一個步驟中，Contoso 管理員會將應用程式的連接字串更新為指向在其 SHAOG 接聽程式上執行的移轉後資料庫。 此組態會在目前執行於 Azure 中的 WEBVM 上變更。 此組態位於ASP 應用程式的 web.config 中。

1. 在中尋找檔案 `C:\inetpub\SmartHotelWeb\web.config` 。 變更伺服器的名稱以反映 AOG 的 FQDN：shaog.contoso.com。

    ![容錯移轉](./media/contoso-migration-rehost-vm-sql-ag/failover4.png)

2. 更新該檔案並加以儲存後，他們會在 WEBVM 上重新啟動 IIS。 他們會從命令提示字元使用來執行此動作 `iisreset /restart` 。
3. IIS 重新啟動後，應用程式此時會使用在 SQL MI 上執行的資料庫。

**需要其他協助？**

- [了解](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure)如何執行測試容錯移轉。
- [了解](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans)如何建立復原計畫。
- [了解](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover)如何容錯移轉至 Azure。

### <a name="clean-up-after-migration"></a>移轉之後進行清除

移轉之後，SmartHotel360 應用程式會在 Azure VM 上執行，而 SmartHotel360 資料庫會位於 Azure SQL 叢集中。

現在，Contoso 必須完成以下的清除步驟：

- 從 vCenter 清查中移除內部部署 VM。
- 從本機備份作業中移除 VM。
- 更新內部文件以顯示 VM 的新位置和 IP 位址。
- 檢閱與已解除委任 VM 互動的任何資源，並更新任何相關的設定或文件，以反映新的組態。
- 新增兩個應新增至生產監視系統的新 VM (SQLAOG1 和 SQLAOG2)。

### <a name="review-the-deployment"></a>檢閱部署

對於 Azure 中的移轉後資源，Contoso 必須能執行一切功能並保護其新的基礎結構。

### <a name="security"></a>安全性

Contoso 安全性小組會檢查 WEBVM、SQLAOG1 和 SQLAOG2 等 Azure VM，以判斷是否有任何的安全疑慮。

- 小組會檢閱 VM 的網路安全性群組 (NSG) 以控制存取權。 NSG 可用來確保只可以傳遞該應用程式允許的流量。
- 小組考慮使用 Azure 磁碟加密和 Key Vault 來保護磁碟上的資料。
- 小組應評估透明資料加密 (TDE)，然後在執行於新的 SQL AOG 的 SmartHotel360 資料庫上加以啟用。 [深入了解](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017)。

如需詳細資訊，請參閱[Azure 中 IaaS 工作負載的安全性最佳作法](https://docs.microsoft.com/azure/security/fundamentals/iaas)。

## <a name="business-continuity-and-disaster-recovery"></a>商務持續性和災害復原

針對商務持續性和災害復原 (BCDR)，Contoso 採取下列動作：

- 為了保護資料的安全，Contoso 會使用 Azure 備份服務，備份 WEBVM、SQLAOG1 和 SQLAOG2 Vm 上的資料。 [深入了解](https://docs.microsoft.com/azure/backup/backup-overview)。
- Contoso 也將了解如何使用 Azure 儲存體將 SQL Server 直接備份至 Blob 儲存體。 [深入了解](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-use-storage-sql-server-backup-restore)。
- 為了讓應用程式繼續運作，Contoso 會使用 Site Recovery 將 Azure 中的應用程式 Vm 複寫至次要區域。 [深入了解](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart)。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- Contoso 為他們的 WEBVM 準備了現有的授權，且將運用 Azure Hybrid Benefit。 Contoso 會轉換現有的 Azure VM，以便充分利用這個定價。
- Contoso 會使用[Azure 成本管理](https://azure.microsoft.com/services/cost-management)，確保他們會在其 IT 領導地位所建立的預算內保持不變。

## <a name="conclusion"></a>結論

在本文中，Contoso 會在 Azure 中重新裝載 SmartHotel360 應用程式，方法是使用 Azure Migrate 服務，將應用程式前端 VM 遷移至 Azure。 Contoso 會使用 Azure 資料庫移轉服務，將應用程式資料庫移轉至 Azure 中布建的 SQL Server 叢集，並在 SQL Server Always On 可用性群組中加以保護。
