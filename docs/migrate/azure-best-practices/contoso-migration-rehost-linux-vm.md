---
title: 在 Azure Vm 上重新裝載內部部署 Linux 應用程式
description: 使用適用于 Azure 的雲端採用架構，以瞭解如何藉由將內部部署 Linux 應用程式遷移至 Azure Vm 來進行重新裝載。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: site-recovery
ms.openlocfilehash: b44a5d9c8e0447488abd500afeaa70fb87a819b1
ms.sourcegitcommit: 5411c3b64af966b5c56669a182d6425e226fd4f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/13/2020
ms.locfileid: "79311943"
---
<!-- cSpell:ignore SQLVM OSTICKETWEB OSTICKETMYSQL contosohost contosodc vcenter WEBVM systemctl NSGs -->

# <a name="rehost-an-on-premises-linux-app-to-azure-vms"></a>將內部部署 Linux 應用程式重新裝載至 Azure VM

本文說明虛構公司 Contoso 如何使用 Azure IaaS VM 重新裝載兩層式 Linux 型Apache MySQL PHP (LAMP) 應用程式。

此範例中使用的服務台應用程式 osTicket 以開放原始碼的形式提供。 如果想將它用於自己的測試目的，您可以從 [github](https://github.com/osTicket/osTicket) 進行下載。

## <a name="business-drivers"></a>商業動機

IT 領導小組與商務合作夥伴密切合作，以了解此次移轉所要實現的目標：

- **因應業務成長。** Contoso 的業務量日益增多，對內部部署系統和基礎結構造成了壓力。
- **限制風險。** 服務台應用程式對 Contoso 的業務影響甚大。 Contoso 想要在亳無風險的情況下，將它移至 Azure。
- **擴充。** Contoso 不想立即變更應用程式。 只想要確保應用程式的穩定性。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已針對此次移轉擬定好各項目標，以決定最佳移轉方法：

- 移轉之後，應用程式不管是在 Azure 或內部部署 VMware 環境中，都應具有相同的效能。 應用程式不管是在雲端中或在內部部署，都一樣重要。
- Contoso 不想要投資此應用程式。 這對企業很重要，但以其目前的格式而言，Contoso 只想安全地移至雲端。
- Contoso 不想變更這個應用程式的 ops 模型。 而想要按照目前的方式，在雲端中與應用程式互動。
- Contoso 不想變更應用程式功能。 只會變更應用程式位置。
- Contoso 已完成幾次 Windows 應用程式移轉，所以想要了解如何在 Azure 中使用以 Linux 為基礎的基礎結構。

## <a name="solution-design"></a>解決方案設計

擬定好目標和需求後，Contoso 會設計和檢閱部署解決方案，並識別移轉程序，包括 Contoso 會用於移轉的 Azure 服務。

### <a name="current-app"></a>目前的應用程式

- OSTicket 應用程式橫跨兩層 VM (**OSTICKETWEB** 和 **OSTICKETMYSQL**)。
- VM 位於 VMware ESXi 主機 **contosohost1.contoso.com** (6.5 版)。
- VMware 環境是由 VM 上執行的 vCenter Server 6.5 (**vcenter.contoso.com**) 進行管理。
- Contoso 有內部部署資料中心 (**contoso-datacenter**) 以及內部部署網域控制站 (**contosodc1**)

### <a name="proposed-architecture"></a>建議的架構

- 因為應用程式為生產工作負載，所以 Azure 中的 VM 會位於生產資源群組 **ContosoRG** 中。
- VM 將會遷移到主要區域 (美國東部 2)，並且放在生產網路 (VNET-PROD-EUS2) 中：
  - Web VM 會位於前端子網路 (PROD-FE-EUS2) 中。
  - 資料庫 VM 會位於資料庫子網路 (PROD-DB-EUS2) 中。
- 移轉完成之後，將會解除委任 Contoso 資料中心的內部部署 VM。

![案例架構](./media/contoso-migration-rehost-linux-vm/architecture.png)

### <a name="solution-review"></a>解決方案檢閱

Contoso 會透過比較一份優缺點清單，來評估建議設計。

<!-- markdownlint-disable MD033 -->

**考量** | **詳細資料**
--- | ---
**優點** | 這兩個應用程式 VM 都會移至 Azure (不需變更)，讓移轉變簡單。<br/><br/> 因為 Contoso 會針對這兩個應用程式 Vm 使用隨即轉移方法，所以應用程式資料庫不需要任何特殊設定或遷移工具。<br/><br/> Contoso 會保留 Azure 中應用程式 VM 的完整控制權。 <br/><br/> 應用程式 VM 會執行 Ubuntu 16.04-TLS，這是經過背書的 Linux 散發套件。 [詳細資訊](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)。
**缺點** | 應用程式的 Web 和資料層會保留單一容錯移轉點。 <br/><br/> Contoso 必須繼續支持此應用程式作為 Azure VM，而不是轉向適用於 MySQL 的 Azure App Service 與 Azure Database 等受控服務。<br/><br/> Contoso 知道，透過隨即轉移 VM 遷移來簡化工作，並不會充分利用[適用於 MySQL 的 Azure 資料庫](https://docs.microsoft.com/azure/mysql/overview)所提供的功能（內建的高可用性、可預測的效能、簡單的調整、自動備份和內建的安全性）。

<!-- markdownlint-enable MD033 -->

### <a name="migration-process"></a>移轉程序

Contoso 會按照下列方式進行遷移：

- 在第一個步驟中，Contoso 會準備以及設定 Azure Migrate 伺服器移轉的 Azure 元件，然後準備內部部署 VMware 基礎結構。
- 他們已備妥 [Azure 基礎結構](./contoso-migration-infrastructure.md)，因此 Contoso 只需要透過 Azure Migrate 伺服器移轉工具來新增 VM 複寫的設定。
- 等一切就緒，Contoso 就可以開始複寫 VM。
- 複寫功能啟用且正常運作後，Contoso 會將 VM 容錯移轉至 Azure，而加以遷移。

![移轉程序](./media/contoso-migration-rehost-linux-vm/migration-process-az-migrate.png)

### <a name="azure-services"></a>Azure 服務

**服務** | **說明** | **成本**
--- | --- | ---
[Azure Migrate 伺服器移轉](https://docs.microsoft.com/azure/migrate/contoso-migration-rehost-linux-vm) | 該服務會協調和管理您的內部部署應用程式和工作負載，以及 AWS/GCP VM 執行個體的移轉。 | 複寫至 Azure 的期間會產生 Azure 儲存體費用。 發生容錯移轉時，系統會建立 Azure VM 並產生費用。 [深入了解](https://azure.microsoft.com/pricing/details/azure-migrate)費用和定價。

## <a name="prerequisites"></a>Prerequisites

以下是 Contoso 在此案例中應該準備好的事項。

<!-- markdownlint-disable MD033 -->

**需求** | **詳細資料**
--- | ---
**Azure 訂用帳戶** | Contoso 已在本系列稍早的文章中建立訂用帳戶。 如果您沒有 Azure 訂用帳戶，請建立[免費帳戶](https://azure.microsoft.com/pricing/free-trial)。<br/><br/> 如果您建立免費帳戶，您就是訂用帳戶的管理員，並可執行所有動作。<br/><br/> 如果您使用現有訂用帳戶，而且您不是系統管理員，則需要與系統管理員合作，讓其指派擁有者或參與者權限給您。<br/><br/> 如果您需要更細微的權限，請檢閱[此文章](https://docs.microsoft.com/azure/site-recovery/site-recovery-role-based-linked-access-control)。
**Azure 基礎結構** |  [了解](./contoso-migration-infrastructure.md) Contoso 如何設定 Azure 基礎結構。<br/><br/> 深入瞭解 Azure Migrate 伺服器移轉的特定[必要條件](https://docs.microsoft.com/azure/migrate/contoso-migration-rehost-linux-vm#prerequisites)需求。
**內部部署伺服器** | 內部部署 vCenter 伺服器應執行 5.5、6.0 或 6.5 版<br/><br/> 執行 5.5、6.0 或 6.5 版的 ESXi 主機<br/><br/> 一或多部在 ESXi 主機上執行的 VMware VM。
**內部部署 VM** | 檢閱已背書在 Azure 上執行的 [Linux 機器](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)。

<!-- markdownlint-enable MD033 -->

## <a name="scenario-steps"></a>案例步驟

以下是 Contoso 完成移轉的方式：

> [!div class="checklist"]
>
> - **步驟1：準備 Azure 以進行 Azure Migrate Server 遷移。** 他們將伺服器移轉工具新增至其 Azure Migrate 專案。
> - **步驟2：準備內部部署 VMware 以進行 Azure Migrate Server 遷移。** 他們會準備帳戶以便探索 VM，並且準備在容錯移轉後連線至 Azure VM。
> - **步驟3：複寫 Vm。** 他們要設定複寫，然後開始將 VM 複寫至 Azure 儲存體。
> - **步驟4：使用 Azure Migrate 伺服器遷移來遷移 Vm。** 他們會執行測試容錯移轉，確定一切都能正常運作，然後執行完整的容錯移轉，以便將 VM 遷移至 Azure。

## <a name="step-1-prepare-azure-for-the-azure-migrate-server-migration-tool"></a>步驟1：準備適用于 Azure Migrate 伺服器遷移工具的 Azure

以下是 Contoso 將 VM 移轉至 Azure 時，所需的 Azure 元件：

- 容錯移轉期間建立 Azure VM 時，這些 VM 所在的 VNet。
- Azure Migrate 伺服器移轉工具佈建。

他們依照下列方式進行其設定：

1. **設定網路：** Contoso 已設定網路，可在[部署 Azure 基礎結構](./contoso-migration-infrastructure.md)時用於 Azure Migrate 伺服器遷移

    - SmartHotel360 應用程式為生產應用程式，而且 VM 會被移轉至美國東部 2 主要區域的 Azure 生產網路 (VNET-PROD-EUS2)。
    - 這兩個 VM 會置於可作為生產資源的 ContosoRG 資源群組中。
    - 應用程式前端 VM (WEBVM) 將移轉至生產網路中的前端子網路 (PROD-FE-EUS2)。
    - 應用程式前端 VM (SQLVM) 將移轉至生產網路中的資料庫子網路 (PROD-DB-EUS2)。

2. **提供 Azure Migrate 伺服器遷移工具：** 網路和儲存體帳戶準備就緒之後，Contoso 現在會建立復原服務保存庫（ContosoMigrationVault），並將它放在主要美國東部2區域的 ContosoFailoverRG 資源群組中。

    ![Azure Migrate 伺服器移轉工具](./media/contoso-migration-rehost-linux-vm/server-migration-tool.png)

**需要其他協助？**

[深入了解](https://docs.microsoft.com/azure/migrate)設定 Azure Migrate 伺服器移轉工具。

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>準備在容錯移轉後連接到 Azure VM

在容錯移轉至 Azure 之後，Contoso 想要連線到 Azure 中複寫的 Vm。 若要這樣做，Contoso 管理員必須遵循下列步驟：

- 若要透過網際網路存取 Azure VM，請在移轉前，先在內部部署 Linux VM 上啟用 SSH。 若為 Ubuntu，可以使用下列命令來完成此動作：**Sudo apt-get ssh install -y**。
- 執行移轉 (容錯移轉) 之後，可以勾選 [開機診斷] 來檢視 VM 的螢幕擷取畫面。
- 如果這個不起任何作用，則需要檢查 VM 是否正常執行，並且檢閱這些[疑難排解祕訣](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)。

**需要其他協助？**

- [深入了解](https://docs.microsoft.com/azure/migrate/contoso-migration-rehost-linux-vm#prepare-vms-for-migration)準備 VM 以進行移轉

## <a name="step-3-replicate-the-on-premises-vms"></a>步驟 3：複寫內部部署 VM

Contoso 管理員必須先設定並啟用複寫，才能執行移轉至 Azure 的作業。

完成探索之後，您就可以開始將 VMware VM 複寫至 Azure。

1. 在 Azure Migrate 專案 >**伺服器**， **Azure Migrate：伺服器遷移** 中，**選取** 複寫。

    ![複寫 VM](./media/contoso-migration-rehost-linux-vm/select-replicate.png)

2. 在 [複寫] > [來源設定] > [您的電腦虛擬化了嗎] 中，選取 [是，使用 VMware vSphere]。

3. 在 [內部部署設備] 中，選取您設定的 Azure Migrate 設備名稱 > [確定]。

    ![來源設定](./media/contoso-migration-rehost-linux-vm/source-settings.png)

4. 在 [虛擬機器] 中，選取您要複寫的機器。
    - 如果您已執行 VM 的評估，您可以套用評估結果中的 VM 大小調整和磁碟類型 (進階/標準) 建議。 若要這麼做，請在 [從 Azure Migrate 評估匯入移轉設定?] 中，選取 [是] 選項。
    - 如果您未執行評估，或不想使用評估設定，請選取 [否] 選項。
    - 如果您選擇使用評估，請選取 VM 群組和評估名稱。

    ![選取評估](./media/contoso-migration-rehost-linux-vm/select-assessment.png)

5. 在 [虛擬機器] 中，視需要搜尋 VM，並檢查您要遷移的每個 VM。 然後選取 **[下一步：目標設定]** 。

6. 在 [目標設定] 中，選取訂用帳戶、您的遷移目標區域，並指定 Azure VM 在移轉後所在的資源群組。 在 [虛擬網路] 中，選取 Azure VM 在移轉後所將加入的 Azure VNet/子網路。

7. 在  **Azure Hybrid Benefit**中，選取下列各項：

    - 如果您不想套用 Azure Hybrid Benefit，請選取 [否]。 然後選取 [下一步]。
    - 如果您有 Windows Server 機器涵蓋於有效的軟體保證或 Windows Server 訂用帳戶下，且您想要將權益套用至要移轉的機器，請選取 [是]。 然後選取 [下一步]。

8. 在 [計算] 中，檢閱 VM 名稱、大小、OS 磁碟類型和可用性設定組。 VM 必須符合 [Azure 需求](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#vmware-requirements)。

    - **VM 大小：** 如果您使用評估建議，[VM 大小] 下拉式清單會包含建議的大小。 否則，Azure Migrate 會根據 Azure 訂用帳戶中最接近的相符項來選擇大小。 或者，您可以在 [Azure VM 大小] 中手動選擇大小。
    - **OS 磁片：** 指定 VM 的 OS （開機）磁片。 OS 磁碟是具有作業系統開機載入器和安裝程式的磁碟。
    - **可用性設定組：** 如果 VM 在遷移後應位於 Azure 可用性設定組中，請指定集合。 此設定組必須位於您為移轉指定的目標資源群組中。

9. 在 [磁碟] 中，指定是否應將 VM 磁碟複寫至 Azure，並選取 Azure 中的磁碟類型 (標準 SSD/HDD 或進階受控磁碟)。 然後選取 [下一步]。
    - 您可以從複寫排除磁碟。
    - 如果您排除磁碟，則在移轉後磁碟將不會出現在 Azure VM 上。

10. 在 [**審查並啟動**複寫] 中，檢查設定，**然後選取 [複寫]，啟動**伺服器的初始複寫。

> [!NOTE]
> 您可以在複寫開始之前隨時更新複寫設定 (經由 [管理] > [複寫機器])。 在複寫啟動後，就無法變更設定。

## <a name="step-4-migrate-the-vms"></a>步驟 4：遷移 VM

Contoso 管理員會執行一次快速的容錯移轉測試，然後再執行一次完整的容錯移轉來遷移 VM。

### <a name="run-a-test-failover"></a>執行測試容錯移轉

1. 在 [**遷移目標**] > **伺服器** > **Azure Migrate： [伺服器遷移**] 中，選取 [**測試遷移的伺服器**]。

     ![測試遷移的伺服器](./media/contoso-migration-rehost-linux-vm/test-migrated-servers.png)

2. 以滑鼠右鍵按一下要測試的 VM，然後選取 [**測試遷移**]。

    ![測試移轉](./media/contoso-migration-rehost-linux-vm/test-migrate.png)

3. 在 [測試移轉] 中，選取 Azure VM 在移轉後將位於其中的 Azure VNet。 我們建議使用非生產 VNet。
4. **測試移轉**作業隨即啟動。 請在入口網站通知中監視作業。
5. 移轉完成之後，請在 Azure 入口網站的 [虛擬機器] 中檢視已遷移的 Azure VM。 機器名稱會具有尾碼 **-Test**。
6. 測試完成之後，以滑鼠右鍵按一下 [複寫**機器**] 中的 [Azure VM]，然後選取 [**清除測試遷移**]。

    ![清除移轉](./media/contoso-migration-rehost-linux-vm/clean-up.png)

### <a name="migrate-the-vms"></a>遷移 VM

Contoso 管理員現在會執行一次完整的容錯移轉，以完成移轉。

1. 在 Azure Migrate 專案 >**伺服器** > **Azure Migrate： 伺服器遷移** 中，選取 複寫**伺服器**。

    ![複寫伺服器](./media/contoso-migration-rehost-linux-vm/replicating-servers.png)

2. 在 [複寫機器] 中，以滑鼠右鍵按一下 VM > [遷移]。
3. 在 [遷移] > [將虛擬機器關機，在沒有資料遺失的情況下執行計劃性移轉] 中，選取 [是] > [確定]。
    - 根據預設，Azure Migrate 會關閉內部部署 VM，並執行隨選複寫，以同步處理上次複寫執行後發生的任何 VM 變更。 這樣可確保不會遺失任何資料。
    - 如果您不想關閉 VM，請選取 [否]
4. VM 會啟動移轉作業。 請在 Azure 通知中追蹤該作業。
5. 作業完成後，您可以從 [虛擬機器] 頁面檢視及管理 VM。

### <a name="connect-the-vm-to-the-database"></a>將 VM 連線到資料庫

在遷移程式的最後一個步驟中，Contoso 管理員會更新應用程式的連接字串，以指向在**OSTICKETMYSQL** VM 上執行的應用程式資料庫。

1. 他們會使用 Putty 或另一個 SSH 用戶端，對 **OSTICKETWEB** VM 進行 SSH 連線。 VM 為私用的，所以會使用私人 IP 位址進行連線。

    ![連線到資料庫](./media/contoso-migration-rehost-linux-vm/db-connect.png)

    ![連線到資料庫](./media/contoso-migration-rehost-linux-vm/db-connect2.png)

2. 他們必須確定 **OSTICKETWEB** VM 可以與 **OSTICKETMYSQL** VM 通訊。 組態目前已利用內部部署 IP 位址 172.16.0.43 進行硬式編碼。

    **更新之前：**

    ![更新 IP](./media/contoso-migration-rehost-linux-vm/update-ip1.png)

    **更新之後：**

    ![更新 IP](./media/contoso-migration-rehost-linux-vm/update-ip2.png)

3. 他們會使用 **systemctl restart apache2** 重新啟動服務。

    ![重新啟動](./media/contoso-migration-rehost-linux-vm/restart.png)

4. 最後，他們會在其中一個 Contoso 網域控制站上，更新 **OSTICKETWEB** 和 **OSTICKETMYSQL** 的 DNS 記錄。

    ![更新 DNS](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png)

    ![更新 DNS](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png)

**需要其他協助？**

- [了解](https://docs.microsoft.com/azure/migrate/tutorial-migrate-vmware#run-a-test-migration)如何執行測試容錯移轉。
- [深入了解](https://docs.microsoft.com/azure/migrate/tutorial-migrate-vmware#migrate-vms)將 VM 移轉至 Azure。

## <a name="clean-up-after-migration"></a>移轉之後進行清除

移轉完成之後，osTicket 應用程式層現在會在 Azure VM 上執行。

現在，Contoso 必須進行清除，如下所示：

- 從 vCenter 清查中移除內部部署 VM。
- 從本機備份作業中移除內部部署 VM。
- 更新其內部文件以顯示新位置，以及 OSTICKETWEB 和 OSTICKETMYSQL 的 IP 位址。
- 檢閱與 VM 互動的任何資源，並更新任何相關的設定或文件，以反映新的組態。
- Contoso 使用了 Azure Migrate 服務搭配相依性對應，評定要進行遷移的 VM。 系統管理員應該從 VM 中移除 Microsoft Monitoring Agent，以及他們針對此目的所安裝的 Microsoft Dependency Agent。

## <a name="review-the-deployment"></a>檢閱部署

應用程式正在執行中，Contoso 必須能發揮一切功能並保護其新的基礎結構。

### <a name="security"></a>安全性

Contoso 安全性小組會檢閱 OSTICKETWEB 和 OSTICKETMYSQLVM，判斷是否有任何安全疑慮。

- 小組會檢閱 VM 的網路安全性群組 (NSG) 以控制存取權。 NSG 可用來確保只可以傳遞該應用程式允許的流量。
- 小組也會考慮使用磁碟加密和 Azure Key Vault 來保護 VM 磁碟上的資料。

如需詳細資訊，請參閱[Azure 中 IaaS 工作負載的安全性最佳作法](https://docs.microsoft.com/azure/security/fundamentals/iaas)。

### <a name="bcdr"></a>BCDR

針對商務持續性和災害復原，Contoso 會採取下列動作：

- **保護資料安全。** Contoso 會使用 Azure 備份服務來備份 VM 上的資料。 [詳細資訊](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。
- **保持應用程式啟動及執行。** Contoso 會使用 Site Recovery，在 Azure 中將應用程式 VM 複寫至次要區域。 [詳細資訊](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart)。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- 部署資源之後，Contoso 會指派 [Azure 基礎結構部署](./contoso-migration-infrastructure.md#set-up-tagging)期間定義的 Azure 標記。
- Contoso 沒有 Ubuntu 伺服器相關授權問題。
- Contoso 會啟用 Microsoft 子公司 Cloudyn 授權的 Azure 成本管理。 它是一種多雲端成本管理解決方案，可協助您使用和管理 Azure 和其他雲端資源。 [深入了解](https://docs.microsoft.com/azure/cost-management/overview) Azure 成本管理。
