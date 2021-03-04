---
title: 將內部部署 Linux 應用程式重新裝載至 Azure Vm
description: 瞭解 Contoso 如何藉由遷移至 Azure Vm 來如何內部部署 Linux 應用程式。
author: deltadan
ms.author: abuck
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: think-tank
ms.openlocfilehash: b98b90a7eb08fd5452050c7047825fa0788b724b
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101791729"
---
<!-- cSpell:ignore OSTICKETWEB OSTICKETMYSQL OSTICKETWEB OSTICKETMYSQL contosohost vcenter contosodc osTicket binlog systemctl NSGs distros -->

# <a name="rehost-an-on-premises-linux-application-to-azure-vms"></a>將內部部署 Linux 應用程式重新裝載至 Azure Vm

本文說明虛構公司 Contoso 如何使用 Azure 基礎結構即服務 (IaaS) 虛擬機器 (Vm) ，來如何兩層式 [燈泡](https://wikipedia.org/wiki/LAMP_software_bundle) 應用程式。

此範例中使用的服務台應用程式 osTicket 是以開放原始碼軟體的形式提供。 如果您想要將它用於您自己的測試用途，您可以從 [GitHub](https://github.com/osTicket/osTicket)下載。

## <a name="business-drivers"></a>商業動機

IT 領導小組與商務合作夥伴密切合作，以了解此次移轉所要實現的目標：

- **解決業務成長。** Contoso 的業務量日益增多，對內部部署系統和基礎結構造成了壓力。
- **限制風險。** 服務台應用程式對 Contoso 的業務而言是不可或缺的。 Contoso 想要在亳無風險的情況下，將它移至 Azure。
- **擴展。** Contoso 不想立即變更應用程式。 它想要確保應用程式的穩定性。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已將此遷移的目標釘選為判斷最佳的遷移方法：

- 在遷移之後，Azure 中的應用程式應該具有與現今公司內部部署 VMware 環境中相同的效能功能。 應用程式在內部部署環境中將會保持在雲端中的重要性。
- Contoso 不想要投資此應用程式。 這對企業很重要，但以其目前的形式而言，Contoso 只會想要將它安全地移至雲端。
- Contoso 不想變更此應用程式的 ops 模型。 它想要以現在的相同方式與雲端中的應用程式互動。
- Contoso 不想變更應用程式功能。 只有應用程式位置會變更。
- Contoso 想要瞭解如何在 Azure 中使用以 Linux 為基礎的基礎結構，以完成幾個 Windows 應用程式遷移。

## <a name="solution-design"></a>解決方案設計

在達到目標和需求後，Contoso 會設計和審核部署解決方案，並識別遷移程式。 也會識別 Contoso 將用於遷移的 Azure 服務。

### <a name="current-application"></a>目前的應用程式

- OsTicket 應用程式會分層至兩個 Vm (`OSTICKETWEB` 和 `OSTICKETMYSQL`) 。
- VM 位於 VMware ESXi 主機 `contosohost1.contoso.com`(6.5 版)。
- VMware 環境是由 vCenter Server 6.5 () 管理 `vcenter.contoso.com` ，並在 VM 上執行。
- Contoso 有內部部署資料中心 () 搭配內部 `contoso-datacenter` 部署網域控制站 (`contosodc1`) 。

### <a name="proposed-architecture"></a>建議的架構

- 因為應用程式是生產工作負載，所以 Azure 中的 Vm 會位於生產資源群組中 `ContosoRG` 。
- Vm 將會遷移到主要區域 (美國東部 2) 並放置於生產網路 (`VNET-PROD-EUS2`) ：
  - Web VM 會位於前端子網路 (`PROD-FE-EUS2`) 中。
  - 資料庫 VM 會位於資料庫子網 (`PROD-DB-EUS2`) 。
- 移轉完成之後，將會解除委任 Contoso 資料中心的內部部署 VM。

![案例架構的圖表。](./media/contoso-migration-rehost-linux-vm/architecture.png)

### <a name="solution-review"></a>解決方案檢閱

Contoso 藉由結合一份優缺點來評估提議的設計。

| 考量 | 詳細資料 |
| --- | --- |
| **優點** | 這兩個應用程式 Vm 都會移至 Azure，而不需要變更，以簡化遷移工作。 <br><br> 因為 Contoso 會針對這兩個應用程式 Vm 使用隨即轉移方法，所以應用程式資料庫不需要任何特殊的設定或遷移工具。 <br><br> Contoso 會保留 Azure 中應用程式 Vm 的完整控制權。 <br><br> 應用程式 Vm 會執行 Ubuntu 16.04-TLS，這是背書的 Linux 散發套件。 深入瞭解 [Azure 上的背書 Linux 發行](/azure/virtual-machines/linux/endorsed-distros)版。 |
| **缺點** | 應用程式的 web 和資料層會保留單一容錯移轉點。 <br><br> Contoso 必須繼續支援應用程式作為 Azure Vm，而不是移至受控服務，例如 Azure App Service 和適用于 MySQL 的 Azure 資料庫。 <br><br> Contoso 藉由將簡單的 VM 遷移作業保持簡單，讓公司無法充分利用 [適用于 MySQL 的 Azure 資料庫](/azure/mysql/overview)所提供的功能。 這些功能包括內建的高可用性、可預測的效能、簡單調整、自動備份和內建安全性。 |

### <a name="migration-process"></a>移轉程序

Contoso 會按照下列方式完成移轉程序：

- 在第一個步驟中，Contoso 會準備及設定 azure 遷移的 Azure 元件：伺服器遷移，並準備內部部署 VMware 基礎結構。
- 公司已有 [azure 基礎結構](./contoso-migration-infrastructure.md) ，因此只需要透過 azure 遷移：伺服器遷移工具來設定 vm 的複寫。
- 等一切就緒，Contoso 就可以開始複寫 VM。
- 複寫功能啟用且正常運作後，Contoso 會將 VM 容錯移轉至 Azure，而加以遷移。

![遷移程式的圖表。](./media/contoso-migration-rehost-linux-vm/migration-process-az-migrate.png)

### <a name="azure-services"></a>Azure 服務

| 服務 | 描述 | 成本 |
| --- | --- | --- |
| [Azure Migrate：伺服器移轉](../index.md) | 此服務會協調和管理內部部署應用程式和工作負載的遷移，以及 Amazon Web Services (AWS) 和 Google Cloud Platform (GCP) VM 實例。 | 複寫至 Azure 的期間會產生 Azure 儲存體費用。 在進行遷移時，會建立 Azure Vm 並產生費用。 深入瞭解 [費用和定價](https://azure.microsoft.com/pricing/details/azure-migrate/)。 |

## <a name="prerequisites"></a>必要條件

以下是 Contoso 在此案例中應該準備好的事項。

需求 | 詳細資料 |
| --- | --- |
| **Azure 訂用帳戶** | Contoso 已在本系列稍早的文章中建立訂用帳戶。 如果您沒有 Azure 訂用帳戶，請建立[免費帳戶](https://azure.microsoft.com/free/)。 <br><br> 如果您建立免費帳戶，您就是訂用帳戶的管理員，並可執行所有動作。 <br><br> 如果您使用現有的訂用帳戶，而且您不是系統管理員，請與系統管理員合作，指派擁有者或參與者許可權給您。 <br><br> 如果您需要更細微的許可權，請參閱 [使用 azure 角色型存取控制管理 Site Recovery 存取 (AZURE RBAC) ](/azure/site-recovery/site-recovery-role-based-linked-access-control)。 |
| **Azure 基礎結構** | 瞭解 [Contoso 如何設定 Azure 基礎結構](./contoso-migration-infrastructure.md)。 <br><br> 深入瞭解 Azure 遷移的特定 [必要條件](./contoso-migration-devtest-to-iaas.md#prerequisites) ：伺服器遷移。 |
| **內部部署伺服器** | 內部部署 vCenter 伺服器應執行5.5、6.0 或6.5 版。 <br><br> 執行5.5、6.0 或6.5 版本的 ESXi 主機。 <br><br> 一或多部在 ESXi 主機上執行的 VMware VM。 |
| **內部部署 VM** | 複習經背書可在 Azure 上執行的[Linux 散發版本](/azure/virtual-machines/linux/endorsed-distros)。 |

## <a name="scenario-steps"></a>案例步驟

以下是 Contoso 完成移轉的方式：

> [!div class="checklist"]
>
> - **步驟1：準備 azure 以進行 Azure 遷移：伺服器遷移。** 將 Azure 遷移：伺服器遷移工具新增至 Azure 遷移專案。
> - **步驟2：準備內部部署 VMware 以進行 Azure 遷移：伺服器遷移。** 準備帳戶以進行 VM 探索，並準備在遷移後連接到 Azure Vm。
> - **步驟3：複寫 Vm。** 設定複寫，並開始將 Vm 複寫至 Azure 儲存體。
> - **步驟4：使用 Azure 遷移來遷移 Vm：伺服器遷移。** 執行測試遷移以確定一切都正常運作，然後執行遷移以將 Vm 移至 Azure。

## <a name="step-1-prepare-azure-for-the-azure-migrate-server-migration-tool"></a>步驟1：準備 Azure 以進行 Azure 遷移：伺服器遷移工具

以下是 Contoso 將 VM 移轉至 Azure 時，所需的 Azure 元件：

- 當 Azure Vm 在遷移期間建立時，將位於其中的虛擬網路。
- Azure 遷移：伺服器遷移工具已布建。

他們會設定這些元件，如下所示：

1. 設定網路。 Contoso 已設定可用於 Azure 遷移的網路：公司[部署 azure 基礎結構](./contoso-migration-infrastructure.md)時的伺服器遷移

    - SmartHotel360 應用程式是生產應用程式。 Vm 將會遷移到主要區域中的 Azure 生產網路 (`VNET-PROD-EUS2`)  (`East US 2`) 。
    - 這兩個 Vm 都會放置在 `ContosoRG` 用於生產資源的資源群組中。
    - 應用程式前端 VM (`OSTICKETWEB`) 將會遷移至生產網路中的前端子網 (`PROD-FE-EUS2`) 。
    - 應用程式資料庫 VM (`OSTICKETMYSQL`) 將會遷移至生產網路中的資料庫子網 (`PROD-DB-EUS2`) 。

1. 布建 Azure 遷移：伺服器遷移工具。 網路和儲存體帳戶準備就緒之後，Contoso 現在會建立復原服務保存庫 (`ContosoMigrationVault`) ，並將它放在 `ContosoFailoverRG` 主要區域 () 的資源群組中 `East US 2` 。

    ![顯示 Azure 遷移伺服器遷移工具的螢幕擷取畫面](./media/contoso-migration-rehost-linux-vm/server-migration-tool.png)

**需要其他協助？**

瞭解如何 [設定 Azure 遷移：伺服器遷移工具](/azure/migrate/)。

## <a name="step-2-prepare-on-premises-vmware-for-azure-migrate-server-migration"></a>步驟2：準備內部部署 VMware 以進行 Azure 遷移：伺服器遷移

在遷移至 Azure 之後，Contoso 希望能夠連線到 Azure 中的已複寫 Vm。 Contoso 管理員需要執行幾項工作：

- 若要透過網際網路存取 Azure VM，請在移轉前，先在內部部署 Linux VM 上啟用 SSH。 若為 Ubuntu，可以使用下列命令來完成此步驟： `sudo apt-get ssh install -y` 。
- 安裝 [Azure Linux 代理程式](/azure/virtual-machines/extensions/agent-linux)
- 執行遷移之後，他們就可以檢查 **開機診斷** 以查看 VM 的螢幕擷取畫面。
- 如果無法運作，他們必須檢查 VM 是否正在執行，並檢查這些 [疑難排解秘訣](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)。

**需要其他協助？**

瞭解如何 [準備 vm 以進行遷移](/azure/migrate/prepare-for-migration)。

## <a name="step-3-replicate-the-on-premises-vms"></a>步驟 3：複寫內部部署 VM

Contoso 管理員必須先設定並啟用複寫，才能執行移轉至 Azure 的作業。

完成探索之後，開始將 VMware Vm 複寫至 Azure。

1. 在 azure 遷移專案中，移至 [**伺服器**  >  **azure 遷移：伺服器遷移**]， 然後選取 [複寫]。

    ![顯示 [複寫] 選項的螢幕擷取畫面。](./media/contoso-migration-rehost-linux-vm/select-replicate.png)

2. 在 **[** 複寫  >  **來源設定**] 中，您的  >  **電腦虛擬化了嗎？** 請選取 **[是，使用 VMware vSphere]**。

3. 在 **內部部署設備** 中，選取您設定的 Azure 遷移設備名稱，然後選取 **[確定]**。

    ![顯示 [來源設定] 索引標籤的螢幕擷取畫面。](./media/contoso-migration-rehost-linux-vm/source-settings.png)

4. 在 [虛擬機器] 中，選取您要複寫的機器。
    - 如果您已執行 VM 的評估，您可以套用評估結果中的 VM 大小調整和磁碟類型 (進階/標準) 建議。 在 [ **從 Azure 遷移評估匯入遷移設定**] 中，選取 [ **是]** 選項。
    - 如果您未執行評量，或不想使用評量設定，請選取 [ **否** ] 選項。
    - 如果您選擇使用評量，請選取 VM 群組和評量名稱。

    ![顯示選取評定的螢幕擷取畫面。](./media/contoso-migration-rehost-linux-vm/select-assessment.png)

5. 在 **虛擬機器** 中，視需要搜尋 vm，然後選取您想要遷移的每個 vm。 然後選取 **[下一步：目標設定]**。

6. 在 [ **目標設定**] 中，選取您要遷移的訂用帳戶和目的地區域。 指定 Azure Vm 在遷移後將位於其中的資源群組。 在 [ **虛擬網路**] 中，選取 azure vm 在遷移後將加入的 azure 虛擬網路/子網。

7. 在 [Azure Hybrid Benefit] 中：

    - 如果您不想套用 Azure Hybrid Benefit，請選取 [否]。 然後選取 [下一步]。
    - 如果您有使用中的軟體保證或 Windows Server 訂閱所涵蓋的 Windows Server 電腦，而且您想要將權益套用至您要遷移的機器，請選取 **[是]** 。 然後選取 [下一步]。

8. 在 [計算] 中，檢閱 VM 名稱、大小、OS 磁碟類型和可用性設定組。 VM 必須符合 [Azure 需求](/azure/migrate/migrate-support-matrix-vmware#vmware-requirements)。

    - **VM 大小：** 如果您使用評量建議，[VM 大小] 下拉式清單將會包含建議的大小。 否則，Azure 遷移會根據 Azure 訂用帳戶中最接近的相符項來挑選大小。 或者，您可以在 [Azure VM 大小] 中手動選擇大小。
    - **作業系統磁片：** 為 VM 指定作業系統 (開機) 磁片。 OS 磁碟是具有作業系統開機載入器和安裝程式的磁碟。
    - **可用性設定組：** 如果 VM 在遷移後應位於 Azure 可用性設定組中，請指定集合。 此設定組必須位於您為移轉指定的目標資源群組中。

9. 在 [ **磁片**] 中，指定是否應將 VM 磁片複寫至 Azure。 在 Azure 中選取 (標準 SSD/HDD 或 premium 受控磁片) 的磁片類型。 然後選取 [下一步]。
    - 您可以從複寫排除磁碟。
    - 如果您排除磁片，則在遷移後將不會出現在 Azure VM 上。

10. 在 [ **檢查 + 開始** 複寫] 中，檢查設定。 然後選取 **[** 複寫] 以啟動伺服器的初始複寫。

> [!NOTE]
> 您可以在複寫開始之前隨時更新複寫設定，以 **管理** 複寫  >  **機器**。 在複寫啟動後，就無法變更設定。

## <a name="step-4-migrate-the-vms"></a>步驟 4：遷移 VM

Contoso 管理員會執行快速測試遷移，然後進行遷移以移動 Vm。

### <a name="run-a-test-migration"></a>執行測試移轉

1. 在 [**遷移目標**  >  **伺服器**  >  **： Azure 遷移：伺服器遷移**] 中，選取 [**測試遷移的伺服器**]。

     ![顯示 [測試遷移的伺服器] 選項的螢幕擷取畫面。](./media/contoso-migration-rehost-linux-vm/test-migrated-servers.png)

1. 選取並保存 (或以滑鼠右鍵按一下) 要測試的 VM。 然後選取 [ **測試遷移**]。

    ![顯示測試遷移專案的螢幕擷取畫面。](./media/contoso-migration-rehost-linux-vm/test-migrate.png)

1. 在 [測試移轉] 中，選取 Azure VM 在移轉後將位於其中的 Azure 虛擬網路。 建議您使用非生產虛擬網路。
1. **測試移轉** 作業隨即啟動。 請在入口網站通知中監視作業。
1. 移轉完成之後，請在 Azure 入口網站的 [虛擬機器] 中檢視已遷移的 Azure VM。 機器名稱會具有尾碼 **-Test**。
1. 測試完成後，請選取並保存 (或以滑鼠右鍵按一下) 複寫 **機器** 中的 Azure VM。 然後選取 [ **清除測試遷移**]。

    ![顯示清除測試遷移專案的螢幕擷取畫面。](./media/contoso-migration-rehost-linux-vm/clean-up.png)

### <a name="migrate-the-vms"></a>遷移 VM

Contoso 管理員現在會執行完整遷移來完成移動。

1. 在 azure 遷移專案中，移至 [**伺服器**  >  **azure 遷移：伺服器遷移**]，然後選取 [複寫 **伺服器**]。

    ![顯示 [複寫伺服器] 選項的螢幕擷取畫面。](./media/contoso-migration-rehost-linux-vm/replicating-servers.png)

1. 在 [複寫 **機器**] 中，選取並按住 (或以滑鼠右鍵按一下) VM，然後選取 [ **遷移**]。
1. 在 [遷移] > [將虛擬機器關機，在沒有資料遺失的情況下執行計劃性移轉] 中，選取 [是] > [確定]。
    - 根據預設，Azure 遷移會關閉內部部署 VM，並執行隨選複寫來同步處理自從上次複寫之後發生的任何 VM 變更。 此動作可確保不會遺失任何資料。
    - 如果您不想關閉 VM，請選取 [否]。
1. VM 會啟動移轉作業。 請在 Azure 通知中追蹤該作業。
1. 作業完成後，您可以從 [虛擬機器] 頁面檢視及管理 VM。

### <a name="connect-the-vm-to-the-database"></a>將 VM 連線到資料庫

在遷移程式的最後一個步驟中，Contoso 管理員會將應用程式的連接字串更新為指向在 VM 上執行的應用程式資料庫 `OSTICKETMYSQL` 。

1. `OSTICKETWEB`使用 PuTTY 或另一個 ssh 用戶端，對 VM 進行 SSH 連線。 VM 是私用的，因此請使用私人 IP 位址進行連接。

    ![顯示 [連接到虛擬機器] 窗格的螢幕擷取畫面。](./media/contoso-migration-rehost-linux-vm/db-connect.png)

    ![顯示資料庫連接的螢幕擷取畫面。](./media/contoso-migration-rehost-linux-vm/db-connect2.png)

1. 請確定 `OSTICKETWEB` vm 可以與 vm 進行通訊 `OSTICKETMYSQL` 。 目前，設定會以內部部署 IP 位址進行硬式編碼 `172.16.0.43` 。

    **更新之前：**

    ![顯示更新之前的 IP 螢幕擷取畫面。](./media/contoso-migration-rehost-linux-vm/update-ip1.png)

    **更新之後：**

    ![顯示更新之後 IP 的螢幕擷取畫面。](./media/contoso-migration-rehost-linux-vm/update-ip2.png)

1. 使用 **systemctl restart apache2** 重新開機服務。

    ![顯示服務重新開機的螢幕擷取畫面。](./media/contoso-migration-rehost-linux-vm/restart.png)

1. 最後， `OSTICKETWEB` `OSTICKETMYSQL` 在其中一個 Contoso 網域控制站上更新和的 DNS 記錄。

    ![顯示更新 DNS 記錄的螢幕擷取畫面。](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png)

    ![顯示更新 DNS 記錄的螢幕擷取畫面。](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png)

**需要其他協助？**

- 瞭解如何 [執行測試遷移](/azure/migrate/tutorial-migrate-vmware#run-a-test-migration)。
- 瞭解如何將 [vm 遷移至 Azure](/azure/migrate/tutorial-migrate-vmware#migrate-vms)。

## <a name="clean-up-after-migration"></a>移轉之後進行清除

當完成遷移時，osTicket 應用層現在會在 Azure Vm 上執行。

現在，Contoso 必須執行下列工作：

- 從 vCenter 清查中移除內部部署 VM。
- 從本機備份作業中移除內部部署 VM。
- 更新內部檔，以顯示和的新位置和 IP 位址 `OSTICKETWEB` `OSTICKETMYSQL` 。
- 檢查與 Vm 互動的任何資源。 更新任何相關的設定或文件，以反映新的組態。
- Contoso 使用 Azure 遷移服務搭配管理 VM 來評定要進行遷移的 Vm。 系統管理員應從 VMware ESXi server 移除遷移 VM 和 web Vm。

## <a name="review-the-deployment"></a>檢閱部署

當應用程式正在執行時，Contoso 必須完全讓並保護其新的基礎結構。

### <a name="security"></a>安全性

Contoso 安全性小組會審核 `OSTICKETWEB` 和 `OSTICKETMYSQL` vm，以判斷是否有任何安全性問題。

- 小組會檢閱 VM 的網路安全性群組 (NSG) 以控制存取權。 NSG 可用來確保只可以傳遞該應用程式允許的流量。
- 小組也會考慮使用 Azure 磁片加密和 Azure Key Vault 來保護 VM 磁片上的資料。

如需詳細資訊，請參閱 [Azure 中 IaaS 工作負載的安全性最佳作法](/azure/security/fundamentals/iaas)。

### <a name="business-continuity-and-disaster-recovery"></a>商務持續性和災害復原

針對商務持續性和災害復原，Contoso 會採取下列動作：

- **保持資料安全。** Contoso 會使用 [AZURE VM 備份](/azure/backup/backup-azure-vms-introduction)來備份 vm 上的資料。

- **讓應用程式保持正常運作。** Contoso 會使用 Site Recovery，將 Azure 中的應用程式 Vm 複寫至次要區域。 如需詳細資訊，請參閱 [快速入門：針對 AZURE VM 設定損毀修復至次要 Azure 區域](/azure/site-recovery/azure-to-azure-quickstart)。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- 部署資源之後，Contoso 會指派 [Azure 基礎結構部署](./contoso-migration-infrastructure.md#set-up-tagging)期間定義的 Azure 標記。
- Contoso 沒有 Ubuntu 伺服器相關授權問題。
- Contoso 會使用 [Azure 成本管理 + 計費](/azure/cost-management-billing/cost-management-billing-overview) 來確保公司維持在 IT 領導階層所建立的預算內。
