---
title: 透過 Azure Migrate 在 Azure 虛擬機器上重新裝載內部部署開發/測試環境
description: 瞭解 Contoso 如何如何內部部署開發/測試環境，方法是透過隨即轉移方法和 Azure Migrate 服務，將內部部署機器遷移至 Azure。
author: deltadan
ms.author: abuck
ms.date: 07/1/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: azure-migrate
ms.openlocfilehash: d307e29cbe6e3d9e28ab93bab3b70a331661e542
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88571026"
---
<!-- docsTest:ignore SmartHotel360 -->
<!-- cSpell:ignore vcenter contosohost contosodc NSGs agentless osTicket WEBVMDEV SQLVMDEV OSTICKETWEBDEV OSTICKETMYSQLDEV -->

# <a name="rehost-an-on-premises-devtest-environment-on-azure-virtual-machines-via-azure-migrate"></a>透過 Azure Migrate 在 Azure 虛擬機器上重新裝載內部部署開發/測試環境

本文將示範虛構公司 Contoso 如何針對 VMware 虛擬機器上執行的兩個應用程式如何其開發/測試環境， (Vm 遷移至 Azure 虛擬機器) 。

此範例中使用的 [SmartHotel360](https://github.com/Microsoft/SmartHotel360) 和 [osTicket](https://github.com/osTicket/osTicket) 應用程式是開放原始碼。 您可以針對自己的測試目的下載它們。

## <a name="migration-options"></a>移轉選項

Contoso 有數個選項可將開發/測試環境移至 Azure：

| 移轉選項 | 結果 |
| --- | --- |
| [Azure Migrate](/azure/migrate/migrate-services-overview) | [評定](/azure/migrate/tutorial-assess-vmware) 及 [遷移](/azure/migrate/tutorial-migrate-vmware) 內部部署 vm。 <br><br> 使用 Azure 基礎結構即服務 (IaaS) 來執行開發/測試伺服器。 <br><br> 使用 [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager)管理 vm。 |
| [Azure DevTest Labs](/azure/devtest-labs/devtest-lab-overview) | 快速布建開發與測試環境。 <br><br> 使用配額和原則將浪費降至最低。 <br><br> 設定自動關機將成本降至最低。 <br><br> 建立 Windows 和 Linux 環境。 |

> [!NOTE]
> 閱讀 Contoso 如何 [使用 DevTest Labs 將其開發/測試環境移至 Azure](./contoso-migration-devtest-to-labs.md)。

## <a name="business-drivers"></a>商業動機

開發領導小組已概述要如何透過此遷移來達成目標。 它的目標是快速將開發/測試功能從內部部署資料中心移出，而不再購買硬體來開發軟體。 它也可讓開發人員在不需要介入的情況下，建立及執行其環境。

> [!NOTE]
> Contoso 會在其環境中使用 [隨用隨付開發/測試訂](https://azure.microsoft.com/offers/ms-azr-0023p) 用帳戶供應專案。 小組中的每個 active Visual Studio 訂閱者都可以使用訂用帳戶虛擬機器隨附的 Microsoft 軟體進行開發/測試，而不需額外付費。 Contoso 只會支付執行 Vm 的 Linux 費率。 這包括具有 SQL Server、SharePoint Server 或其他軟體的 Vm，通常以較高的費率計費。

## <a name="migration-goals"></a>移轉目標

Contoso 開發小組已將此遷移的目標釘選。 這些目標是用來判斷最合適的移轉方法：

- Contoso 想要快速移出其內部部署的開發/測試環境。
- 在遷移之後，Contoso 在 Azure 中的開發/測試環境應該具有 VMware 中目前系統的增強功能。
- 作業模型會使用自助布建，從布建到 DevOps 的服務移動。

## <a name="solution-design"></a>解決方案設計

在達到目標和需求後，Contoso 會設計和審核部署解決方案，並識別遷移程式。 此流程包含 Contoso 將用於遷移的 Azure 服務。

### <a name="current-application"></a>目前的應用程式

- 這兩個應用程式的開發/測試 Vm 是在 Vm 上執行 (`WEBVMDEV` 、 `SQLVMDEV` 、 `OSTICKETWEBDEV` `OSTICKETMYSQLDEV`) 。 在將程式碼升級至生產環境 Vm 之前，這些 Vm 會用來進行開發。
- VM 位於 VMware ESXi 主機 `contosohost1.contoso.com`(6.5 版)。
- VMware 環境是由 VM 上所執行的 VCenter Server 6.5 (`vcenter.contoso.com`) 進行管理。
- Contoso 有內部部署資料中心 () 搭配內部 `contoso-datacenter` 部署網域控制站 (`contosodc1`) 。

### <a name="proposed-architecture"></a>建議的架構

- 因為 Vm 是用於開發/測試，所以會位於 `ContosoDevRG` Azure 的資源群組中。
- Vm 將會遷移到主要 Azure 區域 (`East US 2`) 並置於開發虛擬網路 (`VNET-DEV-EUS2`) 。
- Web 前端 Vm 會位於開發網路中 () 的前端子網中 `DEV-FE-EUS2` 。
- 資料庫 VM 將位於開發網路中 () 的資料庫子網中 `DEV-DB-EUS2` 。
- 移轉完成之後，將會解除委任 Contoso 資料中心的內部部署 VM。

  ![建議的案例架構圖表，其中包含內部部署和虛擬機器。](./media/contoso-migration-devtest-to-iaas/architecture.png)
  
  _圖1：建議的架構。_

### <a name="database-considerations"></a>資料庫考量

為了支援進行中的開發，Contoso 決定繼續使用現有的 Vm，並將其遷移至 Azure。 未來，Contoso 將採用平臺即服務 (PaaS) 服務，例如 [Azure SQL Database](/azure/azure-sql/azure-sql-iaas-vs-paas-what-is-overview) 和 [適用於 MySQL 的 Azure 資料庫](/azure/mysql/overview)。

- 資料庫 Vm 會依原樣遷移，而不會有任何變更。
- 使用 Azure 開發/測試訂用帳戶供應專案時，執行 Windows Server 和 SQL Server 的電腦將不會產生授權費用。 避免費用會將計算成本維持在最低限度。
- 在未來，Contoso 會想要將其開發與 PaaS 服務整合。

### <a name="solution-review"></a>解決方案檢閱

Contoso 藉由結合一份優缺點來評估提議的設計。

| 考量 | 詳細資料 |
| --- | --- |
| **優點** | 所有開發 Vm 都會移至 Azure，而不需要變更，讓遷移變簡單。 <br><br> 因為 Contoso 會針對這兩組 Vm 使用隨即轉移方法，所以應用程式資料庫不需要特殊的設定或遷移工具。 <br><br> Contoso 可以利用其 Azure 開發/測試訂用帳戶的投資來節省授權費用。 <br><br> Contoso 會保留 Azure 中應用程式 Vm 的完整控制權。 <br><br> 開發人員將會提供訂用帳戶的許可權，讓他們能夠建立新的資源，而不需要等待它回應其要求。 |
| **缺點** | 遷移只會移動 Vm，但不會移至 PaaS 服務來進行開發。 這表示 Contoso 需要開始支援其 Vm 的作業，包括安全性修補程式。 這是過去所維護的，所以 Contoso 需要為這項新的作業工作尋找解決方案。 <br><br> 以雲端為基礎的解決方案可為開發人員提供過度布建系統的保護。 開發人員可以立即布建他們的系統，但它們可能會建立成本低廉且不包含在預算中的資源。 |

> [!NOTE]
> Contoso 可以使用 [DevTest Labs](/azure/devtest-labs/devtest-lab-overview)來解決其清單中的缺點。

### <a name="migration-process"></a>移轉程序

Contoso 會使用 Azure Migrate：伺服器遷移工具中的無代理程式方法，將其開發前端和資料庫移轉至 Azure Vm。

- Contoso 會準備和設定適用于 Azure Migrate 的 Azure 元件：伺服器遷移，並準備內部部署 VMware 基礎結構。
- [Azure 基礎結構](./contoso-migration-infrastructure.md)已就緒，因此 Contoso 只需要透過 Azure Migrate：伺服器遷移工具來設定 vm 的複寫。
- 等一切就緒，Contoso 就可以開始複寫 VM。
- 啟用複寫並正常運作之後，Contoso 會藉由測試遷移來遷移 Vm，如果成功，則會將其容錯移轉至 Azure。
- 在 Azure 中啟動並執行開發 Vm 之後，Contoso 會重新設定其開發工作站，以指向目前在 Azure 中執行的 Vm。

![遷移程式的圖表。](./media/contoso-migration-devtest-to-iaas/migration-process-az-migrate.png)

_圖2：遷移程式的總覽。_

### <a name="azure-services"></a>Azure 服務

| 服務 | 描述 | 成本 |
| --- | --- | --- |
| [Azure Migrate：伺服器移轉](/azure/migrate) | 服務會協調和管理內部部署應用程式和工作負載，以及 AWS 或 GCP VM 實例的遷移。 | 複寫至 Azure 的期間會產生 Azure 儲存體費用。 系統會建立 azure Vm，並在發生遷移時產生費用，並在 Azure 中執行 Vm。 [深入了解](https://azure.microsoft.com/pricing/details/azure-migrate)費用和定價。 |

## <a name="prerequisites"></a>必要條件

這是 Contoso 執行此案例所需的內容：

| 需求 | 詳細資料 |
| --- | --- |
| **Azure 開發/測試訂用帳戶** | Contoso 會建立 [Azure 開發/測試訂](https://azure.microsoft.com/offers/ms-azr-0023p) 用帳戶，以利用最多可降低80% 的成本。 <br><br> 如果您沒有 Azure 訂用帳戶，請建立[免費帳戶](https://azure.microsoft.com/free)。 <br><br> 如果您建立免費帳戶，您就是訂用帳戶的系統管理員，而且可以執行所有動作。 <br><br> 如果您使用現有的訂用帳戶，但您不是系統管理員，請與系統管理員合作，指派擁有者或參與者許可權給您。 <br><br> 如果您需要更細微的許可權，請參閱 [使用角色型存取控制來管理 Site Recovery 存取 (RBAC) ](/azure/site-recovery/site-recovery-role-based-linked-access-control)。 |
| **Azure 基礎結構** | 瞭解 Contoso 如何 [設定 Azure 基礎結構](./contoso-migration-infrastructure.md)。 <br><br> 深入瞭解 Azure Migrate 的特定 [必要條件](#prerequisites) ：伺服器遷移。 |
| **內部部署伺服器** | 內部部署 vCenter 伺服器應執行5.5、6.0、6.5 或6.7 版。 <br><br> ESXi 主機應該執行5.5、6.0、6.5 或6.7 版。 <br><br> 一或多部在 ESXi 主機上執行的 VMware VM。 |

## <a name="scenario-steps"></a>案例步驟

以下是 Contoso 管理員執行移轉的方式：

> [!div class="checklist"]
>
> - **步驟1：準備 Azure 以進行 Azure Migrate：伺服器遷移。** 他們會將伺服器遷移工具新增至其 Azure Migrate 專案。
> - **步驟2：準備內部部署 VMware 以進行 Azure Migrate：伺服器遷移。** 他們會準備帳戶以進行 VM 探索，並準備在遷移後連接到 Azure Vm。
> - **步驟3：複寫 Vm。** 他們會設定複寫，並開始將 Vm 複寫至 Azure 儲存體。
> - **步驟4：使用 Azure Migrate：伺服器遷移來遷移 Vm。** 他們會執行測試遷移，以確定一切都正常運作，然後執行完整遷移以將 Vm 移至 Azure。

## <a name="step-1-prepare-azure-for-the-azure-migrate-server-migration-tool"></a>步驟1：準備 Azure 以進行 Azure Migrate：伺服器遷移工具

Contoso 需要將 Vm 遷移至虛擬網路，當 Azure Vm 建立、布建和設定時，會透過 Azure Migrate：伺服器遷移工具來保存。

1. 設定網路： Contoso 已設定可用於 Azure Migrate：在 [部署 Azure 基礎結構](./contoso-migration-infrastructure.md)時進行伺服器遷移的網路。

    - 要遷移的 Vm 會用於開發。 它們會遷移至 Azure 開發虛擬網路， (`VNET-DEV-EUS2` 在主要美國東部2區域中) 。
    - 這兩個 Vm 會放在 `ContosoDevRG` 用於開發資源的資源群組中。
    - 應用程式前端 Vm (`WEBVMDEV` 和 `OSTICKETWEBDEV`) 會 `DEV-FE-EUS2` 在開發虛擬網路中遷移至前端子網 () 。
    - 應用程式資料庫 VM (`SQLVMDEV` 和 `OSTICKETMYSQLDEV`) 將會遷移至開發虛擬網路中的資料庫子網 (`DEV-DB-EUS2`) 。

2. 布建 Azure Migrate：伺服器遷移工具。

    1. 從 Azure Migrate 下載。OVA 映射，並將其匯入 VMware。

       ![下載畫面的螢幕擷取畫面。OVA 檔案。](./media/contoso-migration-devtest-to-iaas/migration-download-ova.png)

       _圖3：下載。OVA 檔案。_

    1. 啟動匯入的映射並設定工具，包括下列步驟：

       - 設定必要條件。

         ![設定必要條件之區段的螢幕擷取畫面。](./media/contoso-migration-devtest-to-iaas/migration-setup-prerequisites.png)

         _圖4：設定必要條件。_

       - 將工具指向 Azure 訂用帳戶。

         ![設定 Azure Migrate 探索之區段的螢幕擷取畫面。](./media/contoso-migration-devtest-to-iaas/migration-register-azure.png)

         _圖5： Azure 訂用帳戶。_

       - 設定 VMware vCenter 認證。

         ![設定 VMware vCenter 認證之區段的螢幕擷取畫面。](./media/contoso-migration-devtest-to-iaas/migration-vcenter-server.png)

         _圖6：設定 VMware vCenter 認證。_

       - 新增任何以 Windows 為基礎的認證進行探索。

         ![探索 Vm 上的應用程式和相依性之區段的螢幕擷取畫面。](./media/contoso-migration-devtest-to-iaas/migration-credentials.png)

         _圖7：新增以 Windows 為基礎的認證來進行探索。_

3. 當您完成設定時，工具需要一些時間來列舉所有 Vm。 當此程式完成時，您會看到它們填入 Azure 中的 Azure Migrate 工具。

**需要其他協助嗎？**

瞭解如何 [設定 Azure Migrate：伺服器遷移工具](/azure/migrate)。

### <a name="prepare-on-premises-vms"></a>準備內部部署 Vm

在遷移之後，Contoso 想要連線至 Azure Vm，並允許 Azure 管理 Vm。 為此，Contoso 管理員在移轉之前執行了下列作業：

1. 為了透過網際網路存取，他們會：

    - 在遷移之前，先在內部部署 VM 上啟用 RDP 或 SSH。
    - 確定已針對設定檔新增 TCP 和 UDP 規則 `Public` 。
    - 檢查作業系統防火牆中是否允許 RDP 或 SSH。
    - 透過下列命令安裝 SSH： `sudo apt-get ssh install -y` 。

2. 為了透過站對站 VPN 存取，他們：

    - 在遷移之前，先在內部部署 VM 上啟用 RDP 或 SSH。
    - 檢查作業系統防火牆中是否允許 RDP 或 SSH。
    - 若為 Windows，請將內部部署 VM 上的作業系統 SAN 原則設定為 `OnlineAll` 。

3. 安裝 [Azure Windows 代理程式](/azure/virtual-machines/extensions/agent-windows) 和 [azure Linux 代理程式](/azure/virtual-machines/extensions/agent-linux)。

若為 Windows，當您觸發遷移時，VM 上不應該有擱置中的 Windows 更新。 如果有，則在更新完成之前，系統管理員將無法登入 VM。 遷移之後，系統管理員可以檢查 **開機診斷** 以查看 VM 的螢幕擷取畫面。 如果這不可行，則應確認 VM 是否正在執行，並檢查 [疑難排解秘訣](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)。

**需要其他協助嗎？**

瞭解如何 [準備 vm 以進行遷移](/azure/migrate/prepare-for-migration)。

## <a name="step-3-replicate-the-on-premises-vms"></a>步驟 3：複寫內部部署 VM

Contoso 管理員必須先設定並啟用複寫，才能執行移轉至 Azure 的作業。 完成探索後，他們就可以開始將 VMware Vm 複寫至 Azure。

1. 在 Azure Migrate 專案中，移至 [**伺服器**  >  **Azure Migrate：伺服器遷移**]。 然後選取 **[** 複寫]。

    ![顯示 [遷移工具] 底下 [複寫] 按鈕的螢幕擷取畫面。](./media/contoso-migration-devtest-to-iaas/select-replicate.png)

    _圖8：複寫 Vm。_

2. 在 **[** 複寫  >  **來源設定**] 中，您的  >  **電腦虛擬化了嗎？** 請選取 **[是]，VMware vSphere**。

3. 在 [ **內部部署應用裝置**] 中，選取您所設定 Azure Migrate 設備的名稱，然後選取 **[確定]**。

    ![顯示來源設定和設備名稱方塊的螢幕擷取畫面。](./media/contoso-migration-devtest-to-iaas/source-settings.png)

    _圖9：來源設定。_

4. 在 [ **虛擬機器**] 中，選取您要複寫的機器。
    - 如果您已執行 Vm 的評量，您可以從評量結果將 VM 大小和磁片類型套用 (premium 或標準) 建議。 若要這麼做，請在 [從 Azure Migrate 評估匯入移轉設定?] 中，選取 [是] 選項。
    - 如果您未執行評量，或不想使用評量設定，請選取 [ **否** ] 選項。
    - 如果您選擇使用評量，請選取 VM 群組和評量名稱。

      ![顯示虛擬機器評量選取專案的螢幕擷取畫面。](./media/contoso-migration-devtest-to-iaas/select-assessment.png)

      _圖10：如何設定必要條件。_

5. 在 **虛擬機器**中，視需要搜尋 vm，並檢查您想要遷移的每個 vm。 然後選取 **[下一步：目標設定]**。

6. 在 [ **目標設定**] 中，選取您要遷移的訂用帳戶和目的地區域。 然後，指定 Azure Vm 在遷移後將位於其中的資源群組。 在 [ **虛擬網路**] 中，選取 azure vm 在遷移後將加入的 azure 虛擬網路或子網。

7. 在 **Azure Hybrid Benefit**中，如果您不想要套用 Azure Hybrid Benefit，請選取 [ **否** ]。 然後選取 [下一步]  。 如果您有 active 軟體保證或 Windows Server 訂用帳戶所涵蓋的 Windows Server 電腦，而且您想要將權益套用至您要遷移的機器，請選取 **[是]** 。 然後選取 [下一步]  。

      > [!NOTE]
      > 在 Contoso 的案例中，系統管理員會選取 [ **否** ] Azure Hybrid Benefit，因為這是 Azure 開發/測試訂用帳戶。 這表示他們只需支付計算費用。 [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit) 只能用於具有軟體保證優點的生產系統。

8. 在 [計算] 中，檢閱 VM 名稱、大小、OS 磁碟類型和可用性設定組。 VM 必須符合 [Azure 需求](/azure/migrate/migrate-support-matrix-vmware#vmware-requirements)。

    - **VM 大小：** 如果您使用評量建議，此下拉式清單會包含建議的大小。 否則，Azure Migrate 會根據 Azure 訂用帳戶中最接近的相符項來選取大小。 您可以改為選擇手動大小，而不是 **AZURE VM 大小**。
    - **作業系統磁片：** 為 VM 指定作業系統 (開機) 磁片。 作業系統磁片具有作業系統開機載入器和安裝程式。
    - **可用性設定組：** 如果 VM 在遷移後應位於 Azure 可用性設定組中，請指定集合。 此集合必須位於您為遷移指定的目標資源群組中。

9. 在 [ **磁片**] 中，指定是否應將 VM 磁片複寫至 azure，並在 azure 中選取 (標準 SSD/HDD 或 premium 受控磁片) 的磁片類型。 然後選取 [下一步]  。 您可以從複寫排除磁碟。 如果您這麼做，則在遷移後將不會出現在 Azure VM 上。

10. 在 [ **檢查並開始**複寫] 中，檢查設定 **並選取 [複寫]，以** 啟動伺服器的初始複寫。

> [!NOTE]
> 您可以在複寫開始之前的任何時間更新複寫設定，以**管理**複寫  >  **機器**。 在複寫啟動後，就無法變更設定。

## <a name="step-4-migrate-the-vms"></a>步驟 4：遷移 VM

Contoso 管理員會執行快速測試遷移，然後執行完整遷移來遷移 Vm。

### <a name="run-a-test-migration"></a>執行測試移轉

1. 在 [**遷移目標**  >  **伺服器**  >  **Azure Migrate：伺服器遷移**] 中，選取 [**測試遷移的伺服器**]。

    ![顯示測試遷移伺服器之選取範圍的螢幕擷取畫面。](./media/contoso-migration-devtest-to-iaas/test-migrated-servers.png)

    _圖11：測試遷移的伺服器。_

2. 選取並保存 (或以滑鼠右鍵按一下) 要測試的 VM，然後選取 [ **測試遷移**]。

    ![顯示用於測試遷移之按鈕的螢幕擷取畫面。](./media/contoso-migration-devtest-to-iaas/test-migrate.png)

    _圖12：測試遷移。_

3. 在 [測試移轉]**** 中，選取 Azure VM 在移轉後將位於其中的 Azure 虛擬網路。 建議您使用非生產虛擬網路。
4. **測試移轉**作業隨即啟動。 請在入口網站通知中監視作業。
5. 移轉完成之後，請在 Azure 入口網站的 [虛擬機器] 中檢視已遷移的 Azure VM。 電腦名稱稱具有 **-Test** 尾碼。
6. 測試完成後，選取並保存 (或以滑鼠右鍵按一下 [複寫 **機器**中的 Azure VM) ]，然後選取 [ **清除測試遷移**]。

    ![顯示清除測試遷移之選項的螢幕擷取畫面。](./media/contoso-migration-devtest-to-iaas/clean-up.png)

    _圖13：清除測試遷移。_

### <a name="migrate-the-vms"></a>遷移 VM

Contoso 管理員現在會執行完整的遷移。

1. 在 Azure Migrate 專案中 **，選取 [伺服器**  >  **Azure Migrate：伺服器遷移**複寫  >  **伺服器**]。

    ![顯示用於複寫伺服器之選項的螢幕擷取畫面。](./media/contoso-migration-devtest-to-iaas/replicating-servers.png)

    _圖14：複寫伺服器。_

2. 在 [複寫 **機器**] 中，選取並按住 (或以滑鼠右鍵按一下) VM，然後選取 [ **遷移**]。
3. 在 [遷移]**** > [將虛擬機器關機，在沒有資料遺失的情況下執行計劃性移轉]**** 中，選取 [是]**** > [確定]****。 根據預設，Azure Migrate 會關閉內部部署 VM，並執行隨選複寫來同步處理自從上次複寫之後發生的任何 VM 變更。 這樣可確保不會遺失任何資料。 如果您不想關閉 VM，請選取 [否]。
4. VM 會啟動移轉作業。 請在 Azure 通知中追蹤該作業。
5. 作業完成後，您可以從 [虛擬機器] 頁面檢視及管理 VM。

**需要其他協助嗎？**

瞭解如何 [執行測試遷移](/azure/migrate/tutorial-migrate-vmware#run-a-test-migration) ，以及如何將 [Vm 遷移至 Azure](/azure/migrate/tutorial-migrate-vmware#migrate-vms)。

## <a name="clean-up-after-migration"></a>移轉之後進行清除

當完成遷移時，SmartHotel360 和 osTicket 應用程式的開發 Vm 會在 Azure Vm 上開始執行。

現在，Contoso 必須完成以下的清除步驟：

- 完成移轉之後，即停止複寫。
- `WEBVMDEV` `SQLVMDEV` `OSTICKETWEBDEV` `OSTICKETMYSQLDEV` 從 vCenter 清查中移除、、和 vm。
- 從本機備份作業中移除所有 Vm。
- 更新內部檔以顯示 Vm 的新位置和 IP 位址。
- 檢閱與 VM 互動的任何資源，並更新任何相關的設定或文件，以反映新的組態。

## <a name="review-the-deployment"></a>檢閱部署

現在應用程式正在執行，Contoso 現在需要在 Azure 中完整讓並保護其安全。

### <a name="security"></a>安全性

Contoso 安全性小組會檢查 Azure Vm 以判斷任何安全性問題。 若要控制存取權，小組會檢閱 VM 的網路安全性群組 (NSG)。 Nsg 是用來確保只有應用程式允許的流量可以到達它。 小組也會考慮使用 Azure 磁碟加密和 Azure Key Vault 來保護磁片上的資料。

如需詳細資訊，請參閱 [Azure 中 IaaS 工作負載的安全性最佳作法](/azure/security/fundamentals/iaas)。

## <a name="business-continuity-and-disaster-recovery"></a>商務持續性和災害復原

針對商務持續性和嚴重損壞修復，Contoso 會採取下列動作：保持資料安全。 Contoso 會使用 Azure 備份服務來備份 Vm 上的資料。 如需詳細資訊，請參閱 [AZURE VM 備份的總覽](/azure/backup/backup-azure-vms-introduction)。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

Contoso 會確保所有開發/測試訂用帳戶建立的 Azure 資源都能節省80%。 系統管理員會啟用 [Azure 成本管理和帳單](/azure/cost-management-billing/cost-management-billing-overview) ，以協助監視和管理 Azure 資源。

## <a name="conclusion"></a>結論

在本文中，Contoso 會在 Azure 中重新裝載用於其 SmartHotel360 和 osTicket 應用程式的開發 Vm。 系統管理員會使用 Azure Migrate：伺服器遷移工具，將應用程式 Vm 遷移至 Azure Vm。
