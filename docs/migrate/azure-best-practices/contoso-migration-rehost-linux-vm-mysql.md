---
title: 將內部部署 Linux 應用程式重新裝載至 Azure Vm 和適用於 MySQL 的 Azure 資料庫
description: 了解 Contoso 如何將內部部署 Linux 應用程式移轉至 Azure VM 和適用於 MySQL 的 Azure 資料庫，以便重新裝載。
author: givenscj
ms.author: abuck
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: azure-migrate
ms.openlocfilehash: cbd6be4f5c0724a4477c43ad1a772c141c08c500
ms.sourcegitcommit: 84d7bfd11329eb4c151c4c32be5bab6c91f376ed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2020
ms.locfileid: "86234747"
---
<!-- cSpell:ignore givenscj OSTICKETWEB OSTICKETMYSQL contosohost vcenter contosodc contosoosticket osticket InnoDB binlog systemctl NSGs -->

# <a name="rehost-an-on-premises-linux-application-to-azure-vms-and-azure-database-for-mysql"></a>將內部部署 Linux 應用程式重新裝載至 Azure Vm 和適用於 MySQL 的 Azure 資料庫

本文說明虛構公司 Contoso 如何重新裝載兩層式[燈泡](https://wikipedia.org/wiki/LAMP_(software_bundle))應用程式，使用 Azure vm 和適用於 MySQL 的 Azure 資料庫將其從內部部署遷移至 Azure。

此範例中使用的服務台應用程式 osTicket 是以開放原始碼的形式提供。 如果想將它用於自己的測試，您可以從 [github](https://github.com/osTicket/osTicket) 進行下載。

## <a name="business-drivers"></a>商業動機

IT 領導小組與商務合作夥伴密切合作，以瞭解他們想要達成的目標：

- **解決業務成長。** Contoso 的業務量日益增多，對內部部署系統和基礎結構造成了壓力。
- **限制風險。** 服務台應用程式對企業至關重要。 Contoso 想要在亳無風險的情況下，將它移至 Azure。
- **延遲.** Contoso 不想立即變更應用程式。 只是想要讓應用程式保持穩定。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已針對此次移轉擬定好各項目標，以便決定最佳移轉方法：

- 在遷移之後，Azure 中的應用程式應該具有與目前在其內部部署 VMware 環境中相同的效能功能。 在雲端中，應用程式在內部部署環境中將維持不變。
- Contoso 不想要投資此應用程式。 這對企業很重要，但以其目前的形式而言，Contoso 只想安全地移至雲端。
- 在完成幾個 Windows 應用程式遷移之後，Contoso 想要瞭解如何在 Azure 中使用以 Linux 為基礎的基礎結構。
- 將應用程式移到雲端之後，Contoso 想要將資料庫管理工作減到最少。

## <a name="proposed-architecture"></a>建議的架構

在此情節中：

- 目前，應用程式會跨兩個 Vm 分層 (`OSTICKETWEB` 和 `OSTICKETMYSQL`) 。
- VM 位於 VMware ESXi 主機 `contosohost1.contoso.com`(6.5 版)。
- VMware 環境是由 VM 上所執行的 VCenter Server 6.5 (`vcenter.contoso.com`) 進行管理。
- Contoso 有內部部署資料中心 (`contoso-datacenter`) 以及內部部署網域控制站 (`contosodc1`)。
- 上的 web 應用程式 `OSTICKETWEB` 將會遷移至 Azure IAAS VM。
- 應用程式資料庫將會遷移至適用於 MySQL 的 Azure 資料庫 PaaS 服務。
- 因為 Contoso 是要移轉生產工作負載，所以資源會位於生產資源群組 `ContosoRG` 中。
- `OSTICKETWEB`資源將會複寫到主要區域 (美國東部 2) ，並放在生產網路 (`VNET-PROD-EUS2`) ：
  - Web VM 會位於前端子網路 (`PROD-FE-EUS2`) 中。
- 應用程式資料庫將會使用[Azure 資料庫移轉服務](https://docs.microsoft.com/azure/dms/dms-overview)遷移至適用於 MySQL 的 Azure 資料庫。
- 移轉完成之後，將會解除委任 Contoso 資料中心的內部部署 VM。

    ![案例架構](./media/contoso-migration-rehost-linux-vm-mysql/architecture.png)

## <a name="migration-process"></a>移轉程序

Contoso 會按照下列方式完成移轉程序：

若要遷移 Web VM：

- 在第一個步驟中，Contoso 會設定部署 Azure Migrate 所需的 Azure 和內部部署基礎結構。
- 他們已經備妥[Azure 基礎結構](./contoso-migration-infrastructure.md)，因此 Contoso 只需要透過 Azure Migrate： Server 遷移工具來新增和設定 vm 的複寫。
- 完成所有準備工作之後，Contoso 就可以開始複寫 VM。
- 當複寫已啟用且正常運作之後，Contoso 會使用 Azure Migrate 完成移動。

若要遷移資料庫：

1. Contoso 會在 Azure 中佈建 MySQL 執行個體。
2. Contoso 會設定 Azure 資料庫移轉服務，以確保能夠存取內部部署資料庫伺服器。
3. Contoso 會將資料庫移轉至適用於 MySQL 的 Azure 資料庫。

    ![移轉程序](./media/contoso-migration-rehost-linux-vm-mysql/migration-process.png)

### <a name="azure-services"></a>Azure 服務

| 服務 | 描述 | 成本 |
| --- | --- | --- |
| [Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-services-overview) | Contoso 會使用 Azure Migrate 服務來評定其 VMware VM。 Azure Migrate 會評定機器是否適合移轉。 它會提供在 Azure 中執行的大小調整建議和成本估計。 | [Azure Migrate](https://azure.microsoft.com/pricing/details/azure-migrate)可以免費使用，但可能會產生費用，取決於您決定用於評估和遷移 (第一方或 ISV) 的工具。 |
| [Azure 資料庫移轉服務](https://docs.microsoft.com/azure/dms/dms-overview) | Azure 資料庫移轉服務可在停機時間最短的情況之下，從多個資料庫來源順暢地遷移至 Azure 資料平臺。 | 瞭解[支援的區域](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability)和[Azure 資料庫移轉服務的定價](https://azure.microsoft.com/pricing/details/database-migration)。 |
| [適用於 MySQL 的 Azure 資料庫](https://docs.microsoft.com/azure/mysql) | 資料庫是以開放原始碼 MySQL 資料庫引擎為基礎。 它為應用程式開發和部署提供完全受控的企業專用的 MySQL 資料庫。 | 深入瞭解適用於 MySQL 的 Azure 資料庫[定價](https://azure.microsoft.com/pricing/details/mysql)和擴充性選項。 |

## <a name="prerequisites"></a>先決條件

以下是 Contoso 在此案例中應該準備好的事項。

| 需求 | 詳細資料 |
| --- | --- |
| **Azure 訂用帳戶** | Contoso 在先前文章期間已建立訂用帳戶。 若您沒有 Azure 訂用帳戶，請建立一個[免費帳戶](https://azure.microsoft.com/free)。 <br><br> 如果您建立免費帳戶，您就是訂用帳戶的管理員，並可執行所有動作。 <br><br> 如果您使用現有訂用帳戶，而且您不是系統管理員，則需要與系統管理員合作，讓其指派擁有者或參與者權限給您。 <br><br> 如果您需要更細微的權限，請檢閱[此文章](https://docs.microsoft.com/azure/site-recovery/site-recovery-role-based-linked-access-control)。 |
| **Azure 基礎結構** | Contoso 會如[適用於移轉的 Azure 基礎結構](./contoso-migration-infrastructure.md)中所述，設定 Azure 基礎結構。 |
| **內部部署伺服器** | 內部部署 vCenter Server 應執行版本5.5、6.0、6.5 或6.7。 <br><br> 執行5.5、6.0、6.5 或6.7 版本的 ESXi 主機。 <br><br> 一或多部在 ESXi 主機上執行的 VMware VM。 |
| **內部部署 VM** | 檢閱已背書在 Azure 上執行的 [Linux 機器](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)。 |

## <a name="scenario-steps"></a>案例步驟

以下是 Contoso 管理員完成移轉的方式：

> [!div class="checklist"]
>
> - **步驟1：準備 Azure 以進行 Azure Migrate：伺服器遷移。** 他們會將伺服器遷移工具加入至其 Azure Migrate 專案。
> - **步驟2：準備內部部署 VMware 以進行 Azure Migrate：伺服器遷移。** 他們會準備帳戶以進行 VM 探索，並準備在遷移後連線到 Azure VM。
> - **步驟3：複寫 Vm。** 他們會設定複寫，並開始將 Vm 複寫至 Azure 儲存體。
> - **步驟4：使用 Azure Migrate：伺服器遷移來遷移應用程式 VM。** 他們會執行測試遷移，確定一切都能正常運作，然後執行完整的遷移，將 VM 移至 Azure。
> - **步驟5：遷移資料庫。** 他們會使用 Azure 資料庫移轉服務來設定遷移。

## <a name="step-1-prepare-azure-for-the-azure-migrate-server-migration-tool"></a>步驟1：準備適用于 Azure Migrate 的 Azure：伺服器遷移工具

以下是 Contoso 將 VM 移轉至 Azure 時，所需的 Azure 元件：

- 在遷移期間建立 Azure Vm 時，將會在其中尋找其所在的 VNet。
- Azure Migrate：伺服器遷移工具 (OVA) 已布建和設定。

他們依照下列方式進行其設定：

1. 設定網路-Contoso 已設定可用於 Azure Migrate 的網路：[部署 Azure 基礎結構](./contoso-migration-infrastructure.md)時的伺服器遷移

2. 布建 Azure Migrate：伺服器遷移工具。

    - 從 Azure Migrate 下載 OVA 映射，並將其匯入 VMware。

        ![下載 OVA 檔案](./media/contoso-migration-rehost-vm/migration-download-ova.png)

    - 啟動匯入的映射並設定工具，包括下列步驟：

      - 設定必要條件。

        ![設定工具](./media/contoso-migration-rehost-vm/migration-setup-prerequisites.png)

      - 將工具指向 Azure 訂用帳戶。

        ![設定工具](./media/contoso-migration-rehost-vm/migration-register-azure.png)

      - 設定 VMware vCenter 認證。

        ![設定工具](./media/contoso-migration-rehost-vm/migration-vcenter-server.png)

      - 新增任何以 Linux 為基礎的認證以供探索。

        ![設定工具](./media/contoso-migration-rehost-vm/migration-credentials.png)

3. 設定完成後，工具會花一些時間來列舉所有虛擬機器。 完成後，您會看到它們填入 Azure 中的 [Azure Migrate] 工具。

**需要其他協助？**

瞭解如何設定[Azure Migrate：伺服器遷移工具](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-server-migration-tool)。

## <a name="step-2-prepare-on-premises-vmware-for-azure-migrate-server-migration"></a>步驟2：準備內部部署 VMware 以進行 Azure Migrate：伺服器遷移

遷移至 Azure 之後，Contoso 想要能夠連線到 Azure 中複寫的 Vm。 若要這麼做，Contoso 管理員還需執行一些作業：

- 若要存取 Azure VM，它們會先在內部部署 Linux VM 上啟用 SSH，再進行遷移。 對於 Ubuntu，可以使用下列命令來完成這項作業： `sudo apt-get ssh install -y` 。

- 執行遷移之後，他們可以勾選 [**開機診斷**] 來查看 VM 的螢幕擷取畫面。

- 如果這個不起任何作用，則需要檢查 VM 是否正常執行，並且檢閱這些[疑難排解祕訣](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)。

- 安裝[Azure Linux 代理程式](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux)。

**需要其他協助？**

- 瞭解如何[準備 vm 以進行遷移](https://docs.microsoft.com/azure/migrate/prepare-for-migration)。

## <a name="step-3-replicate-vm"></a>步驟3：複寫 VM

Contoso 管理員必須先設定並啟用複寫，才能執行移轉至 Azure 的作業。

完成探索後，他們就可以開始將應用程式 VM 複寫到 Azure。

1. 在 [Azure Migrate 專案 >**伺服器**  >  **Azure Migrate： [伺服器遷移**] **Replicate**中，選取 [複寫]。

    ![複寫 VM](./media/contoso-migration-rehost-linux-vm/select-replicate.png)

2. 在 **[** 複寫  >  **來源設定**  >  ] [**您的電腦虛擬化嗎？**] 中，選取 **[是]，並 VMware vSphere**。

3. 在 [內部部署設備] 中，選取您設定的 Azure Migrate 設備名稱 > [確定]。

    ![來源設定](./media/contoso-migration-rehost-linux-vm/source-settings.png)

4. 在 [虛擬機器] 中，選取您要複寫的機器。
    - 如果您已執行 VM 的評估，您可以套用評估結果中的 VM 大小調整和磁碟類型 (進階/標準) 建議。 若要這麼做，請在 [從 Azure Migrate 評估匯入移轉設定?]**** 中，選取 [是]**** 選項。
    - 如果您未執行評估，或不想使用評估設定，請選取 [**否**] 選項。
    - 如果您選擇使用評估，請選取 VM 群組和評估名稱。

    ![選取評估](./media/contoso-migration-rehost-linux-vm/select-assessment.png)

5. 在 [虛擬機器]**** 中，視需要搜尋 VM，並檢查您要遷移的每個 VM。 然後選取 **[下一步：目標設定]**。

6. 在 [目標設定] 中，選取訂用帳戶、您的遷移目標區域，並指定 Azure VM 在移轉後所在的資源群組。 在 [虛擬網路] 中，選取 Azure VM 在移轉後所將加入的 Azure VNet/子網路。

7. 在 [ **Azure Hybrid Benefit**中，選取下列各項：

    - 如果您不想套用 Azure Hybrid Benefit，請選取 [否]。 然後，選取 [下一步]。

8. 在 [計算] 中，檢閱 VM 名稱、大小、OS 磁碟類型和可用性設定組。 VM 必須符合 [Azure 需求](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#vmware-requirements)。

    - **VM 大小：** 如果您使用評估建議，[VM 大小] 下拉式清單會包含建議的大小。 否則，Azure Migrate 會根據 Azure 訂用帳戶中最接近的相符項來選擇大小。 或者，在 [ **AZURE VM 大小**] 中選擇手動大小。
    - **OS 磁片：** 指定 VM 的 OS (開機) 磁片。 OS 磁碟是具有作業系統開機載入器和安裝程式的磁碟。
    - **可用性設定組：** 如果 VM 在遷移後應位於 Azure 可用性設定組中，請指定集合。 此設定組必須位於您為移轉指定的目標資源群組中。

9. 在 [磁碟]**** 中，指定是否應將 VM 磁碟複寫至 Azure，並選取 Azure 中的磁碟類型 (標準 SSD/HDD 或進階受控磁碟)。 然後，選取 [下一步]。
    - 您可以從複寫排除磁碟。
    - 如果您排除磁碟，則在移轉後磁碟將不會出現在 Azure VM 上。

10. 在 [**審查並啟動**複寫] 中，檢查設定，**然後選取 [複寫]，啟動**伺服器的初始複寫。

> [!NOTE]
> 您可以在複寫開始之前隨時更新複寫設定 (經由 [管理]**** > [複寫機器]****)。 在複寫啟動後，就無法變更設定。

## <a name="step-4-migrate-the-vm-with-azure-migrate-server-migration"></a>步驟4：使用 Azure Migrate 遷移 VM：伺服器遷移

Contoso 管理員會執行快速測試遷移，然後進行完整遷移來移動 web VM。

### <a name="run-a-test-migration"></a>執行測試移轉

1. 在 [**遷移目標**  >  **伺服器**  >  **Azure Migrate： [伺服器遷移**] 中，選取 [**測試遷移的伺服器**]。

     ![測試遷移的伺服器](./media/contoso-migration-rehost-linux-vm/test-migrated-servers.png)

2. 選取並按住 (，或以滑鼠右鍵按一下) 要測試的 VM，然後選取 [**測試遷移**]。

    ![測試移轉](./media/contoso-migration-rehost-linux-vm/test-migrate.png)

3. 在 [測試移轉] 中，選取 Azure VM 在移轉後將位於其中的 Azure VNet。 我們建議使用非生產 VNet。
4. **測試移轉**作業隨即啟動。 請在入口網站通知中監視作業。
5. 移轉完成之後，請在 Azure 入口網站的 [虛擬機器] 中檢視已遷移的 Azure VM。 機器名稱會具有尾碼 **-Test**。
6. 測試完成之後，選取並按住 (或以滑鼠右鍵) 按一下 [複寫**機器**中的 Azure VM]，然後選取 [**清除測試遷移**]。

    ![清除移轉](./media/contoso-migration-rehost-linux-vm/clean-up.png)

### <a name="migrate-the-vm"></a>移轉 VM

Contoso 管理員現在會執行完整的遷移，以完成移動。

1. 在 [Azure Migrate 專案 >**伺服器**  >  **Azure Migrate： [伺服器遷移**] 中，選取 [複寫**伺服器**]。

    ![複寫伺服器](./media/contoso-migration-rehost-linux-vm/replicating-servers.png)

2. 在 [複寫**電腦**] 中，選取並按住 (，或以滑鼠右鍵按一下 [) VM > [**遷移**]。
3. 在 [遷移] > [將虛擬機器關機，在沒有資料遺失的情況下執行計劃性移轉] 中，選取 [是] > [確定]。
    - 根據預設，Azure Migrate 會關閉內部部署 VM，並執行隨選複寫，以同步處理上次複寫執行後發生的任何 VM 變更。 這樣可確保不會遺失任何資料。
    - 如果您不想關閉 VM，請選取 [否]。
4. VM 會啟動移轉作業。 請在 Azure 通知中追蹤該作業。
5. 作業完成後，您可以從 [虛擬機器]**** 頁面檢視及管理 VM。

## <a name="step-5-provision-azure-database-for-mysql"></a>步驟5：布建適用於 MySQL 的 Azure 資料庫

Contoso 管理員會在主要區域中布建 MySQL 資料庫實例， (`East US 2`) 。

1. 在 Azure 入口網站中，他們可會建立適用於 MySQL 的 Azure 資料庫資源。

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-1.png)

2. 他們會新增 `contosoosticket` Azure 資料庫的名稱。 他們會將資料庫新增至生產資源群組 `ContosoRG` ，並為其指定認證。
3. 內部部署 MySQL 資料庫的版本為 5.7，因此他們會為了相容性而選取這個版本。 他們會使用預設大小，以符合其資料庫需求。

     ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-2.png)

4. 針對 [備份備援選項]****，他們會選擇使用 [異地備援]****。 此選項可讓他們在次要區域中還原資料庫， (`Central US`) 發生中斷的情況。 他們在佈建資料庫時，只能設定這個選項。

     ![備援性](./media/contoso-migration-rehost-linux-vm-mysql/db-redundancy.png)

5. 在 `VNET-PROD-EUS2` 網路 >**服務端點**中，他們會將服務端點新增 (SQL 服務的資料庫子網) 。

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-3.png)

6. 新增子網路之後，他們會建立虛擬網路規則，以允許從生產網路中的資料庫子網路進行存取。

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-4.png)

## <a name="step-6-migrate-the-database"></a>步驟 6：遷移資料庫

有數種方式可以移動 MySQL 資料庫。 每個選項都需要您建立目標的適用於 MySQL 的 Azure 資料庫實例。 建立之後，您可以使用兩個路徑來執行遷移：

- 6A： Azure 資料庫移轉服務
- 6b： MySQL 工作臺備份和還原

### <a name="step-6a-migrate-the-database-via-azure-database-migration-service"></a>步驟6a：透過 Azure 資料庫移轉服務遷移資料庫

Contoso 管理員會遵循[逐步遷移教學](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online)課程，透過 Azure 資料庫移轉服務來遷移資料庫。 他們可以使用 MySQL 5.6 或5.7，執行線上、離線和混合式 (預覽) 遷移。

> [!NOTE]
> 適用於 MySQL 的 Azure 資料庫支援 MySQL 8.0，但資料庫移轉服務工具尚不支援該版本。

總而言之，您必須執行下列動作：

- 確保符合所有的遷移必要條件：

  - MySQL 伺服器資料庫來源必須符合適用於 MySQL 的 Azure 資料庫支援的版本。 適用於 MySQL 的 Azure 資料庫支援 MySQL 社區版本、InnoDB 儲存引擎，以及跨來源與目標的相同版本進行遷移。
  - 在 `my.ini` (Windows) 或 `my.cnf` (Unix) 中啟用二進位記錄。 若未這麼做，將會導致遷移嚮導發生下列錯誤： `Error in binary logging. Variable binlog_row_image has value 'minimal'. Please change it to 'full. For more information, see https://go.microsoft.com/fwlink/?linkid=873009` 。
  - 使用者必須擁有 `ReplicationAdmin` 角色。
  - 不搭配外鍵和觸發程式來遷移資料庫架構。

- 建立透過 ExpressRoute 或 VPN 連接到內部部署網路的虛擬網路。

- 使用 `Premium` 連線到 VNet 的 SKU 來建立 Azure 資料庫移轉服務實例。

- 確定實例可以透過虛擬網路存取 MySQL 資料庫。 這會需要確保在虛擬網路層級、網路 VPN 和裝載 MySQL 的機器上，允許從 Azure 到 MySQL 的所有連入埠。

- 執行資料庫移轉服務工具：

  - 建立遷移專案。

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/migration-dms-new-project.png)

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/migration-dms-new-project-02.png)

  - 在內部部署資料庫) 中新增來源 (。

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/migration-dms-source.png)

  - 選取目標。

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/migration-dms-target.png)

  - 選取要遷移的資料庫。

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/migration-dms-databases.png)

  - 設定 [高級設定]。

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/migration-dms-settings.png)

  - 啟動複寫並解決任何錯誤。

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/migration-dms-monitor.png)

  - 執行最後的轉換。 ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/migration-dms-cutover.png)

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/migration-dms-cutover-complete.png)

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/migration-dms-cutover-complete-02.png)

  - 恢復任何外鍵和觸發程式。

  - 修改應用程式以使用新的資料庫。

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/migration-dms-cutover-apps.png)

### <a name="step-6b-migrate-the-database-mysql-workbench"></a>步驟6b：將資料庫移轉 (MySQL 工作臺) 

Contoso 管理員會利用 MySQL 工具，使用備份與還原來遷移資料庫。 他們會安裝 MySQL 工作臺，從備份資料庫 `OSTICKETMYSQL` ，然後將它還原到適用於 MySQL 的 Azure 資料庫。

### <a name="install-mysql-workbench"></a>安裝 MySQL Workbench

1. 他們會檢查[必要條件以及下載 MySQL Workbench](https://dev.mysql.com/downloads/workbench/?utm_source=tuicool) (英文)。

2. 他們會按照[安裝指示](https://dev.mysql.com/doc/workbench/en/wb-installing.html)，安裝適用於 Windows 的 MySQL Workbench。

3. 在 MySQL Workbench 中，他們會建立 OSTICKETMYSQL 的 MySQL 連線。

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench1.png)

4. 它們會將資料庫匯出為 `osticket` 本機獨立檔案。

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench2.png)

5. 在本機備份資料庫之後，他們會建立適用於 MySQL 的 Azure 資料庫執行個體連線。

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench3.png)

6. 他們現在可以在適用於 MySQL 的 Azure 資料庫執行個體中，從該獨立檔案匯入 (還原) 資料庫。 為實例建立新的架構 (`osticket`) 。

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench4.png)

### <a name="connect-the-vm-to-the-database"></a>將 VM 連線到資料庫

在遷移程式的最後一個步驟中，Contoso 管理員會將應用程式的連接字串更新為指向在 VM 上執行的應用程式資料庫 `OSTICKETMYSQL` 。

1. 他們會 `OSTICKETWEB` 使用 PuTTY 或另一個 ssh 用戶端，對 VM 進行 SSH 連線。 VM 為私用的，所以會使用私人 IP 位址進行連線。

    ![連線到資料庫](./media/contoso-migration-rehost-linux-vm/db-connect.png)

    ![連線到資料庫](./media/contoso-migration-rehost-linux-vm/db-connect2.png)

2. 他們必須確保 `OSTICKETWEB` vm 可以與 `OSTICKETMYSQL` vm 通訊。 目前，設定會以內部部署 IP 位址進行硬式編碼 `172.16.0.43` 。

    **更新之前：**

    ![更新 IP](./media/contoso-migration-rehost-linux-vm/update-ip1.png)

    **更新之後：**

    ![更新 IP](./media/contoso-migration-rehost-linux-vm/update-ip2.png)

3. 他們會使用重新開機服務 `systemctl restart apache2` 。

    ![重新啟動](./media/contoso-migration-rehost-linux-vm/restart.png)

4. 最後，他們會 `OSTICKETWEB` `OSTICKETMYSQL` 在其中一個 Contoso 網域控制站上，更新和的 DNS 記錄。

    ![更新 DNS](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png)

    ![更新 DNS](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png)

**需要其他協助？**

- 瞭解如何[執行測試遷移](https://docs.microsoft.com/azure/migrate/tutorial-migrate-vmware#run-a-test-migration)。
- 瞭解如何[將 vm 遷移至 Azure](https://docs.microsoft.com/azure/migrate/tutorial-migrate-vmware#migrate-vms)。

## <a name="review-the-deployment"></a>檢閱部署

現在應用程式正在執行，Contoso 必須完全讓並保護其新的基礎結構。

## <a name="clean-up-after-migration"></a>移轉之後進行清除

當遷移完成時，osTicket 應用層會在 Azure Vm 上執行。

現在，Contoso 必須執行下列作業：

- 從 vCenter 清查中移除 VMware VM。
- 從本機備份作業中移除內部部署 VM。
- 更新內部文件以顯示新的位置和 IP 位址。
- 檢閱與內部部署 VM 互動的任何資源，並更新任何相關的設定或文件，以反映新的組態。
- Contoso 使用了 Azure Migrate 服務搭配相依性對應來評估 `OSTICKETWEB` VM 以進行遷移。

### <a name="security"></a>安全性

Contoso 安全性小組會檢閱 VM 和資料庫，判斷是否有任何的安全疑慮。

- 他們會檢閱 VM 的網路安全性群組 (NSG) 來控制存取權。 NSG 可用來確保只可以傳遞該應用程式允許的流量。
- 他們考慮使用 Azure 磁碟加密和 Azure Key Vault 來保護 VM 磁片上的資料。
- VM 與資料庫執行個體之間的通訊並未針對 SSL 進行設定。 他們必須這麼做，才能確保資料庫流量不會遭到駭客入侵。

如需詳細資訊，請參閱[Azure 中 IaaS 工作負載的安全性最佳作法](https://docs.microsoft.com/azure/security/fundamentals/iaas)。

<!-- docsTest:ignore "Quickstart: Set" -->

### <a name="bcdr"></a>BCDR

針對商務持續性和災害復原，Contoso 會採取下列動作：

- **保護資料的安全。** Contoso 會使用[AZURE VM 備份](https://docs.microsoft.com/azure/backup/backup-azure-vms-introduction)來備份應用程式 VM 上的資料。 他們不需要設定資料庫的備份。 適用於 MySQL 的 Azure 資料庫會自動建立及儲存伺服器備份。 他們選擇對資料庫使用異地備援，所以資料庫可復原並已準備好用於生產。

- **讓應用程式保持正常運作。** Contoso 會使用 Site Recovery 將 Azure 中的應用程式 Vm 複寫至次要區域。 如需詳細資訊，請參閱[快速入門：設定 AZURE VM 的損毀修復至次要 azure 區域](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart)。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- 部署資源之後，Contoso 會根據他們在 [Azure 基礎結構](./contoso-migration-infrastructure.md#set-up-tagging)部署期間所做的決定來指派 Azure 標記。
- 他們沒有 Contoso Ubuntu 伺服器相關授權問題。
- Contoso 會使用[Azure 成本管理和帳單](https://docs.microsoft.com/azure/cost-management-billing/cost-management-billing-overview)，確保他們會在其 IT 領導地位所建立的預算內保持不變。
