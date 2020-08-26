---
title: 使用 Azure Migrate 在 Azure Vm 上重新裝載應用程式
description: 瞭解 contoso 如何使用 Azure Migrate 服務，將內部部署機器隨即轉移至 Azure，並重新裝載內部部署應用程式。
author: givenscj
ms.author: abuck
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: azure-migrate
ms.openlocfilehash: 80b34b343ca3ee43bbf8fc8eab6d9972fcddda39
ms.sourcegitcommit: 07d56209d56ee199dd148dbac59671cbb57880c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88877529"
---
<!-- cSpell:ignore WEBVM SQLVM OSTICKETWEB OSTICKETMYSQL contosohost vcenter contosodc NSGs agentless -->

# <a name="rehost-an-on-premises-application-on-azure-vms-by-using-azure-migrate"></a>使用 Azure Migrate 在 Azure Vm 上重新裝載內部部署應用程式

本文示範虛構公司 Contoso 如何將應用程式 Vm 遷移至 Azure Vm，以如何在 VMware 虛擬機器上執行的兩層式 Windows .NET 前端應用程式 (Vm) 。

此範例中使用的 SmartHotel360 應用程式是以開放原始碼的形式提供。 如果您想要將它用於您自己的測試用途，您可以從 [GitHub](https://github.com/Microsoft/SmartHotel360)下載。

## <a name="business-drivers"></a>商業動機

IT 領導小組與商務合作夥伴密切合作，瞭解他們想要使用這種方式達成什麼目標。 他們想要：

- **解決業務成長。** Contoso 正在成長，因此對公司的內部部署系統和基礎結構造成了壓力。
- **限制風險。** SmartHotel360 應用程式對 Contoso 公司來說非常重要。 該公司想要將應用程式移至 Azure，但沒有任何風險。
- **擴展。** Contoso 不想要修改應用程式，但想要確保應用程式的穩定性。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已針對此次移轉擬定好各項目標。 它會使用這些目標來判斷最佳的遷移方法：

- 在遷移之後，Azure 中的應用程式應該具有與現今在 VMware 中相同的效能功能。 應用程式在內部部署環境中將會保持在雲端中的重要性。
- 雖然此應用程式對 Contoso 很重要，但該公司目前不想要投資。 Contoso 想要將應用程式安全地移至雲端（以其目前的形式）。
- Contoso 不想變更此應用程式的 ops 模型。 Contoso 想要以現在的相同方式在雲端中與其互動。
- Contoso 不想變更任何應用程式功能。 只有應用程式位置會變更。

## <a name="solution-design"></a>解決方案設計

在建立目標和需求之後，Contoso 會設計和審核部署解決方案。 Contoso 會識別遷移程式，包括將用於遷移的 Azure 服務。

### <a name="current-application"></a>目前的應用程式

- 應用程式會分層至兩個 Vm (`WEBVM` 和 `SQLVM`) 。
- VM 位於 VMware ESXi 主機 `contosohost1.contoso.com`(6.5 版)。
- VMware 環境是由 VM 上執行的 vCenter Server 6.5 () 來管理 `vcenter.contoso.com` 。
- Contoso 有內部部署資料中心 () 搭配內部 `contoso-datacenter` 部署網域控制站 (`contosodc1`) 。

### <a name="proposed-architecture"></a>建議的架構

- 因為應用程式是生產工作負載，所以 Azure 中的應用程式 Vm 會位於生產資源群組中 `ContosoRG` 。
- 應用程式 Vm 將會遷移到主要 Azure 區域 (美國東部 2) 並放置於生產網路 (`VNET-PROD-EUS2`) 。
- Web 前端 VM 會位於生產網路中 () 的前端子網 `PROD-FE-EUS2` 。
- 資料庫 VM 會位於生產網路中 () 的資料庫子網中 `PROD-DB-EUS2` 。
- 移轉完成之後，將會解除委任 Contoso 資料中心的內部部署 VM。

![顯示案例架構的圖表。](./media/contoso-migration-rehost-vm/architecture.png)

### <a name="database-considerations"></a>資料庫考量

在解決方案設計過程中，Contoso 會進行 Azure SQL Database 與 SQL Server 的功能比較。 下列考慮有助於公司決定使用 SQL Server 在 Azure IaaS VM 上執行的：

- 如果 Contoso 需要自訂作業系統和資料庫，或在相同的 VM 上共同找出並執行合作夥伴應用程式，則使用執行 SQL Server 的 Azure VM 似乎是最佳解決方案。
- 使用軟體保證，Contoso 稍後可以使用 SQL Server 的 Azure Hybrid Benefit，以折扣費率在 Azure SQL 受控執行個體上交換現有授權。 這最多可節省30% 的 SQL 受控執行個體。

### <a name="solution-review"></a>解決方案檢閱

Contoso 藉由結合一份優缺點來評估提議的設計。

| 考量 | 詳細資料 |
| --- | --- |
| **優點** | 這兩個應用程式 Vm 都會移至 Azure，而不需要變更，讓遷移變簡單。 <br><br> 因為 Contoso 會針對這兩個應用程式 Vm 使用隨即轉移方法，所以不需要應用程式資料庫的任何特殊設定或遷移工具。 <br><br> Contoso 可以使用 Azure Hybrid Benefit 來利用其軟體保證的投資。 <br><br> Contoso 會保留 Azure 中應用程式 Vm 的完整控制權。 |
| **缺點** | `WEBVM` 和 `SQLVM` 正在執行 Windows Server 2008 R2。 Azure 支援特定角色的作業系統。 [深入了解](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)。 <br><br> 應用程式的 web 和資料層會保留為單一失敗點。 <br><br> `SQLVM` 正在 SQL Server 2008 R2 上執行。 SQL Server 2008 R2 不再存在於主流支援中，但 Azure Vm 支援此功能。 [深入了解](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support)。 <br><br> Contoso 必須繼續支援 Azure Vm 上的應用程式，而不是移至受管理的服務，例如 Azure App Service 或 Azure SQL Database。 |

### <a name="migration-process"></a>移轉程序

Contoso 會使用 Azure Migrate：伺服器遷移工具中的無代理程式方法，將應用程式前端和資料庫 Vm 遷移至 Azure Vm。

- 在第一個步驟中，Contoso 會準備和設定適用于 Azure Migrate 的 Azure 元件：伺服器遷移，並準備內部部署 VMware 基礎結構。
- [Azure 基礎結構](./contoso-migration-infrastructure.md)已就緒，因此 Contoso 只需要透過 Azure Migrate：伺服器遷移工具來設定 vm 的複寫。
- 等一切就緒，Contoso 就可以開始複寫 VM。
- 啟用複寫並正常運作之後，Contoso 會藉由測試遷移並將其容錯移轉至 Azure （如果成功）來遷移 VM。

![顯示遷移程式中步驟的圖表。](./media/contoso-migration-rehost-vm/migration-process-az-migrate.png)

### <a name="azure-services"></a>Azure 服務

| 服務 | 描述 | 成本 |
| --- | --- | --- |
| [Azure Migrate：伺服器移轉](/azure/migrate/contoso-migration-rehost-vm) | 此服務會協調和管理內部部署應用程式和工作負載的遷移，以及 Amazon Web Services (AWS) /Google Cloud Platform (GCP) VM 實例。 | 複寫至 Azure 的期間會產生 Azure 儲存體費用。 系統會建立 azure Vm，並在發生遷移時產生費用，並在 Azure 中執行 Vm。 深入瞭解 [費用和定價](https://azure.microsoft.com/pricing/details/azure-migrate)。  |

## <a name="prerequisites"></a>先決條件

Contoso 和其他使用者必須符合此案例的下列必要條件。

| 需求 | 詳細資料 |
| --- | --- |
| **Azure 訂用帳戶** | Contoso 已在本系列稍早的文章中建立訂用帳戶。 如果您沒有 Azure 訂用帳戶，請建立[免費帳戶](https://azure.microsoft.com/free)。 <br><br> 如果您建立免費帳戶，您就是訂用帳戶的管理員，並可執行所有動作。 <br><br> 如果您使用現有的訂用帳戶，而且您不是系統管理員，請與系統管理員合作，指派擁有者或參與者許可權給您。 <br><br> 如果您需要更細微的權限，請檢閱[此文章](/azure/site-recovery/site-recovery-role-based-linked-access-control)。 |
| **Azure 基礎結構** | [了解](./contoso-migration-infrastructure.md) Contoso 如何設定 Azure 基礎結構。 <br><br> 深入瞭解 Azure Migrate 的特定 [必要條件](./contoso-migration-devtest-to-iaas.md#prerequisites) ：伺服器遷移。 |
| **內部部署伺服器** | 內部部署 vCenter 伺服器應執行5.5、6.0、6.5 或6.7 版。 <br><br> ESXi 主機應該執行5.5、6.0、6.5 或6.7 版。 <br><br> 一或多部在 ESXi 主機上執行的 VMware VM。 |

## <a name="scenario-steps"></a>案例步驟

以下是 Contoso 管理員執行移轉的方式：

> [!div class="checklist"]
>
> - **步驟1：準備 Azure 以進行 Azure Migrate：伺服器遷移。** 他們會將伺服器遷移工具新增至其 Azure Migrate 專案。
> - **步驟2：複寫內部部署 Vm。** 他們會設定複寫，並開始將 Vm 複寫至 Azure 儲存體。
> - **步驟3：使用 Azure Migrate：伺服器遷移來遷移 Vm。** 他們會執行測試遷移，以確定一切都正常運作，然後執行完整遷移以將 Vm 移至 Azure。

## <a name="step-1-prepare-azure-for-azure-migrate-server-migration"></a>步驟1：準備 Azure 以進行 Azure Migrate：伺服器遷移

為了將 Vm 遷移至 Azure，Contoso 需要一個虛擬網路，當 Azure Vm 在遷移期間建立時，就會在該虛擬網路中找到。 它也需要 Azure Migrate：伺服器遷移工具 (OVA 檔) 布建和設定。

1. 設定網路。 Contoso 已在 [部署 Azure 基礎結構](./contoso-migration-infrastructure.md)時，設定一個可用於 Azure Migrate：伺服器遷移的設定。

    - SmartHotel360 應用程式是生產應用程式，而且 Vm 將會遷移到主要區域中的 Azure 生產網路 (`VNET-PROD-EUS2`)  (`East US 2`) 。
    - 這兩個 Vm 都會放置在 `ContosoRG` 用於生產資源的資源群組中。
    - 應用程式前端 VM (`WEBVM`) 將會遷移至生產網路中的前端子網 (`PROD-FE-EUS2`) 。
    - 應用程式資料庫 VM (`SQLVM`) 將會遷移至生產網路中的資料庫子網 (`PROD-DB-EUS2`) 。

1. 布建 Azure Migrate：伺服器遷移工具。

    1. 從 Azure Migrate 下載 OVA 映射，並將其匯入 VMware。

       ![顯示 O V A file 的 [下載] 按鈕的螢幕擷取畫面。](./media/contoso-migration-rehost-vm/migration-download-ova.png)

    1. 啟動匯入的映射並設定工具，包括下列步驟：

       - 設定必要條件。

         ![顯示設定先決條件授權條款之區域的螢幕擷取畫面。](./media/contoso-migration-rehost-vm/migration-setup-prerequisites.png)

       - 將工具指向 Azure 訂用帳戶。

         ![顯示用於向 Azure Migrate 註冊之選項的螢幕擷取畫面。](./media/contoso-migration-rehost-vm/migration-register-azure.png)

       - 設定 VMware vCenter 認證。

         ![顯示指定 vCenter 伺服器之選項的螢幕擷取畫面。](./media/contoso-migration-rehost-vm/migration-vcenter-server.png)

       - 新增任何以 Windows 為基礎的認證進行探索。

         ![探索虛擬機器上的應用程式和相依性之區域的螢幕擷取畫面。](./media/contoso-migration-rehost-vm/migration-credentials.png)

當您完成設定時，工具需要一些時間來列舉所有 Vm。 當此程式完成時，您會看到它們填入 Azure 中的 Azure Migrate 工具。

**需要其他協助嗎？**

瞭解如何設定 [Azure Migrate：伺服器遷移工具](/azure/migrate/migrate-services-overview#azure-migrate-server-migration-tool)。

### <a name="prepare-on-premises-vms"></a>準備內部部署 Vm

在遷移之後，Contoso 想要連線至 Azure Vm，並允許 Azure 管理 Vm。 在遷移之前，Contoso 管理員必須執行下列步驟：

1. 若要透過網際網路存取：

    - 在遷移之前，先在內部部署 VM 上啟用 RDP 或 SSH。
    - 確定已為**公用**設定檔新增 TCP 和 UDP 規則。
    - 檢查作業系統防火牆中是否允許 RDP 或 SSH。

2. 針對透過站對站 VPN 的存取：

    - 在遷移之前，先在內部部署 VM 上啟用 RDP 或 SSH。
    - 檢查作業系統防火牆中是否允許 RDP 或 SSH。
    - 若為 Windows，請將內部部署 VM 上的作業系統 SAN 原則設定為 **OnlineAll**。

3. 安裝 [Azure Windows 代理程式](/azure/virtual-machines/extensions/agent-windows)。

其他考量：

- 若是 Windows，當您觸發遷移時，VM 上不應該有擱置中的 Windows 更新。 如果有，則在更新完成之前，系統管理員將無法登入 VM。
- 遷移之後，系統管理員可以檢查 **開機診斷** 以查看 VM 的螢幕擷取畫面。 如果這不可行，則應確認 VM 是否正在執行，並檢查 [疑難排解秘訣](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)。

**需要其他協助嗎？**

瞭解如何 [準備 vm 以進行遷移](/azure/migrate/prepare-for-migration)。

## <a name="step-2-replicate-the-on-premises-vms"></a>步驟2：複寫內部部署 Vm

在 Contoso 管理員可以執行遷移至 Azure 之前，他們必須先設定並啟用複寫。

完成探索之後，他們就可以開始將 VMware Vm 複寫至 Azure。

1. 在 Azure Migrate 專案中，移至 [**伺服器**  >  **Azure Migrate：伺服器遷移**]。 然後選取 **[** 複寫]。

    ![用於複寫虛擬機器之選取專案的螢幕擷取畫面。](./media/contoso-migration-rehost-vm/select-replicate.png)

2. 在 **[** 複寫  >  **來源設定**] 中，您的  >  **電腦虛擬化了嗎？** 請選取 **[是]，VMware vSphere**。

3. 在 [ **內部部署應用裝置**] 中，選取您所設定 Azure Migrate 設備的名稱，然後選取 **[確定]**。

    ![顯示來源設定的螢幕擷取畫面。](./media/contoso-migration-rehost-vm/source-settings.png)

4. 在 [ **虛擬機器**] 中，選取您要複寫的機器。
    - 如果您已執行 Vm 的評量，您可以從評量結果將 VM 大小和磁片類型套用 (premium 或標準) 建議。 若要這麼做，請在 [從 Azure Migrate 評估匯入移轉設定?] 中，選取 [是] 選項。
    - 如果您未執行評量，或不想使用評量設定，請選取 [ **否** ] 選項。
    - 如果您選擇使用評量，請選取 VM 群組和評量名稱。

    ![顯示選取要遷移虛擬機器之方塊的螢幕擷取畫面。](./media/contoso-migration-rehost-vm/select-assessment.png)

5. 在 **虛擬機器**中，視需要搜尋 vm，並檢查您想要遷移的每個 vm。 然後選取 **[下一步：目標設定]**。

6. 在 [ **目標設定**] 中，選取您要遷移的訂用帳戶和目的地區域。 然後，指定 Azure Vm 在遷移後將位於其中的資源群組。 在 [ **虛擬網路**] 中，選取 azure vm 在遷移後將加入的 azure 虛擬網路或子網。

7. 在 [Azure Hybrid Benefit] 中：

    - 如果您不想套用 Azure Hybrid Benefit，請選取 [否]。 接著，選取 [下一步]  。
    - 如果您有 active 軟體保證或 Windows Server 訂閱所涵蓋的 Windows Server 電腦，而且您想要將權益套用至您要遷移的機器，請選取 **[是]** 。 接著，選取 [下一步]  。

8. 在 [計算] 中，檢閱 VM 名稱、大小、OS 磁碟類型和可用性設定組。 VM 必須符合 [Azure 需求](/azure/migrate/migrate-support-matrix-vmware#vmware-requirements)。

    - **VM 大小：** 如果您使用評量建議，[VM 大小] 下拉式清單將會包含建議的大小。 否則，Azure Migrate 會根據 Azure 訂用帳戶中最接近的相符項來挑選大小。 或者，您可以在 [Azure VM 大小] 中手動選擇大小。
    - **作業系統磁片：** 為 VM 指定作業系統 (開機) 磁片。 作業系統磁片具有作業系統開機載入器和安裝程式。
    - **可用性設定組：** 如果 VM 在遷移後應位於 Azure 可用性設定組中，請指定集合。 此集合必須位於您為遷移指定的目標資源群組中。

9. 在 [磁碟]**** 中，指定是否應將 VM 磁碟複寫至 Azure，並選取 Azure 中的磁碟類型 (標準 SSD/HDD 或進階受控磁碟)。 接著，選取 [下一步]  。

   您可以從複寫排除磁碟。 如果您排除磁片，則在遷移後將不會出現在 Azure VM 上。

10. 在 [ **檢查並開始**複寫] 中，檢查設定，然後 **選取 [** 複寫] 以啟動伺服器的初始複寫。

> [!NOTE]
> 您可以在複寫開始之前、**管理**複寫機器中的任何時間更新複寫設定  >  ** **。 在複寫啟動後，就無法變更設定。

## <a name="step-3-migrate-the-vms-with-azure-migrate-server-migration"></a>步驟3：使用 Azure Migrate：伺服器遷移來遷移 Vm

Contoso 管理員會執行快速測試遷移，然後執行完整遷移來遷移 Vm。

### <a name="run-a-test-migration"></a>執行測試移轉

1. 在 [**遷移目標**  >  **伺服器**  >  **Azure Migrate：伺服器遷移**] 中，選取 [**測試遷移的伺服器**]。

    ![開始測試已遷移伺服器的按鈕螢幕擷取畫面。](./media/contoso-migration-rehost-vm/test-migrated-servers.png)

2. 選取並保存 (或以滑鼠右鍵按一下) 要測試的 VM，然後選取 [ **測試遷移**]。

    ![所選虛擬機器的螢幕擷取畫面，以及啟動遷移測試的按鈕。](./media/contoso-migration-rehost-vm/test-migrate.png)

3. 在 [測試移轉]**** 中，選取 Azure VM 在移轉後將位於其中的 Azure 虛擬網路。 建議您使用非生產虛擬網路。
4. **測試移轉**作業隨即啟動。 請在入口網站通知中監視作業。
5. 移轉完成之後，請在 Azure 入口網站的 [虛擬機器] 中檢視已遷移的 Azure VM。 電腦名稱稱具有 **-Test** 尾碼。
6. 測試完成後，選取並保存 (或以滑鼠右鍵按一下 [複寫 **機器**中的 Azure VM) ]，然後選取 [ **清除測試遷移**]。

    ![清除遷移選取專案的螢幕擷取畫面。](./media/contoso-migration-rehost-vm/clean-up.png)

### <a name="migrate-the-vms"></a>遷移 VM

Contoso 管理員現在會執行完整的遷移。

1. 在 Azure Migrate 專案中 **，選取 [伺服器**  >  **Azure Migrate：伺服器遷移**複寫  >  **伺服器**]。

    ![複寫伺服器的選項螢幕擷取畫面。](./media/contoso-migration-rehost-vm/replicating-servers.png)

2. 在 [複寫 **機器**] 中，選取並按住 (或以滑鼠右鍵按一下) VM，然後選取 [ **遷移**]。
3. 在 [遷移] > [將虛擬機器關機，在沒有資料遺失的情況下執行計劃性移轉] 中，選取 [是] > [確定]。

    根據預設，Azure Migrate 會關閉內部部署 VM，並執行隨選複寫，以同步處理自上次複寫之後發生的任何 VM 變更。 這樣可確保不會遺失任何資料。 如果您不想關閉 VM，請選取 [否]。
4. VM 會啟動移轉作業。 請在 Azure 通知中追蹤該作業。
5. 作業完成後，您可以從 [虛擬機器] 頁面檢視及管理 VM。

**需要其他協助嗎？**

- 瞭解如何 [執行測試遷移](/azure/migrate/tutorial-migrate-vmware#run-a-test-migration)。
- 瞭解如何將 [vm 遷移至 Azure](/azure/migrate/tutorial-migrate-vmware#migrate-vms)。

## <a name="clean-up-after-migration"></a>移轉之後進行清除

當完成遷移時，SmartHotel360 應用層現在會在 Azure Vm 上執行。

現在，Contoso 必須執行以下的清除步驟：

- 完成移轉之後，即停止複寫。
- `WEBVM`從 vCenter 清查中移除機器。
- `SQLVM`從 vCenter 清查中移除機器。
- `WEBVM` `SQLVM` 從本機備份作業中移除和。
- 更新內部檔以顯示 Vm 的新位置和 IP 位址。
- 檢閱與 VM 互動的任何資源，並更新任何相關的設定或文件，以反映新的組態。

## <a name="review-the-deployment"></a>檢閱部署

當應用程式正在執行時，Contoso 需要在 Azure 中完整讓並保護其安全。

### <a name="security"></a>安全性

Contoso 安全性小組會檢查 Azure Vm 以判斷任何安全性問題。 若要控制存取權，小組會檢閱 VM 的網路安全性群組 (NSG)。 Nsg 是用來確保只有應用程式允許的流量可以到達它。 小組也會考慮使用 Azure 磁碟加密和 Key Vault 來保護磁片上的資料。

如需詳細資訊，請參閱 [Azure 中 IaaS 工作負載的安全性最佳作法](/azure/security/fundamentals/iaas)。

## <a name="business-continuity-and-disaster-recovery"></a>商務持續性和災害復原

針對商務持續性和災害復原，Contoso 會採取下列動作：

- 保護資料安全： Contoso 會 [使用 Azure 備份來備份 vm 上的資料](/azure/backup/backup-overview)。
- 讓應用程式保持運作： Contoso 會 [使用 Azure Site Recovery 將 Azure 中的應用程式 vm 複寫至次要區域](/azure/site-recovery/azure-to-azure-quickstart)。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

Contoso 具有其 Vm 的現有授權，而且會利用 Azure Hybrid Benefit。 Contoso 會轉換現有的 Azure VM，以便充分利用這個定價。

Contoso 會啟用 [Azure 成本管理和帳單](/azure/cost-management-billing/cost-management-billing-overview) ，以協助監視和管理 Azure 資源。

## <a name="conclusion"></a>結論

在本文中，Contoso 會在 Azure 中重新裝載 SmartHotel360 應用程式。 系統管理員會使用 Azure Migrate：伺服器遷移工具，將應用程式 Vm 遷移至 Azure Vm。
