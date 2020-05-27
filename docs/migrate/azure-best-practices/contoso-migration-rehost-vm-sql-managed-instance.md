---
title: 將內部部署應用程式移轉至 Azure VM 和 Azure SQL Database 受控執行個體，以重新裝載此應用程式
description: 了解 Contoso 如何在 Azure VM 和 Azure SQL Database 受控執行個體上重新裝載內部部署應用程式。
author: givenscj
ms.author: abuck
ms.date: 04/02/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: azure-migrate
ms.openlocfilehash: fd859de8d7388a0cbd7c55255e005d98e6e87418
ms.sourcegitcommit: bd9872320b71245d4e9a359823be685e0f4047c5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/26/2020
ms.locfileid: "83861577"
---
<!-- cSpell:ignore givenscj WEBVM SQLVM OSTICKETWEB OSTICKETMYSQL contosohost vcenter contosodc NSGs agentless SQLMI iisreset -->

# <a name="rehost-an-on-premises-app-on-an-azure-vm-and-sql-database-managed-instance"></a>在 Azure VM 和 SQL Database 受控執行個體上重新裝載內部部署應用程式

本文說明虛構公司 Contoso 如何使用 Azure Migrate 服務，將在 VMware Vm 上執行的兩層式 Windows .NET 前端應用程式遷移至 Azure VM。 此外也說明 Contoso 如何將應用程式資料庫移轉至 Azure SQL Database 受控執行個體。

此範例中使用的 SmartHotel360 應用程式以開放原始碼的形式提供。 如果想將它用於自己的測試目的，您可以從 [github](https://github.com/Microsoft/SmartHotel360) 進行下載。

## <a name="business-drivers"></a>商業動機

Contoso 的 IT 領導小組已與該公司的企業合作夥伴密切合作，以了解這些企業想從此次移轉實現什麼目標：

- **解決業務成長。** Contoso 正在成長。 因此，該公司的內部部署系統和基礎結構所承受的壓力變大了。
- **提高效率。** Contoso 必須移除不必要的程序，並簡化其開發人員和使用者的程序。 該公司需要快速且不浪費時間或金錢的 IT 服務，以便能夠更快滿足客戶需求。
- **增加靈活性。** Contoso IT 必須能夠更快因應企業的需求。 其因應速度必須能夠比市場變化更快，才能讓該公司在全球經濟中獲致成功。 Contoso 的 IT 小組不得礙事，或成為企業的絆腳石。
- **尺度.** 隨著該公司的業務順利成長，Contoso IT 必須提供能夠同步成長的系統。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已確定此次移轉的目標。 該公司會使用移轉目標來判斷最合適的移轉方法。

- 移轉過後，應用程式不管是在 Azure 或 Contoso 的內部部署 VMware 環境中，都應具有相同的效能。 移到雲端，並不表示應用程式效能比較不重要。
- Contoso 不想要投資應用程式。 此應用程式對企業很重要，但是 Contoso 只想要以目前的格式將應用程式移至雲端。
- 在應用程式遷移之後，資料庫管理工作應會減到最少。
- Contoso 不想對此應用程式使用 Azure SQL Database， 且正在尋求替代方案。

## <a name="solution-design"></a>解決方案設計

擬定好目標和需求後，Contoso 會設計和檢閱部署解決方案，並識別移轉程序，包括將用於移轉的 Azure 服務。

### <a name="current-architecture"></a>目前架構

- Contoso 有一個主要的資料中心 (**contoso-datacenter**)。 該資料中心位於美國東部的紐約市。
- Contoso 在全美另有三家地區性分公司。
- 主要資料中心透過光纖都會乙太網路連線 (500 MBps) 連到網際網路。
- 每家分公司皆使用企業級連線從本機連到網際網路，並透過 IPsec VPN 通道連回主要資料中心。 此設定可讓 Contoso 的整個網路永久連線，並將網際網路連線最佳化。
- 主要資料中心已透過 VMware 完全虛擬化。 Contoso 有兩部 ESXi 6.5 虛擬化主機，均由 vCenter Server 6.5 管理。
- Contoso 使用 Active Directory 來管理身分識別。 Contoso 會使用內部網路上的 DNS 伺服器。
- Contoso 有一個內部部署網域控制站 (**contosodc1**)。
- 網域控制站會在 VMware VM 上執行。 地區分公司的網域控制站會在實體伺服器上執行。
- SmartHotel360 應用程式橫跨兩層 VM (**WEBVM** 和** SQLVM**)，而且位於 VMware ESXi 6.5 版主機 (**contosohost1.contoso.com**)。
- VMware 環境是由在 VM 上執行的 vCenter Server 6.5 （**vcenter.contoso.com**）所管理。

![目前的 Contoso 架構](./media/contoso-migration-rehost-vm-sql-managed-instance/contoso-architecture.png)

### <a name="proposed-architecture"></a>建議的架構

在此案例中，Contoso 想要遷移其兩層式內部部署旅遊應用程式，如下所示：

- 將應用程式資料庫 (**SmartHotelDB**) 遷移到 Azure SQL Database 受控執行個體。
- 將前端 **WebVM** 移轉至 Azure VM。
- 移轉完成後，將會解除委任 Contoso 資料中心的內部部署 VM。

![案例架構](./media/contoso-migration-rehost-vm-sql-managed-instance/architecture.png)

### <a name="database-considerations"></a>資料庫考量

在解決方案設計過程中，Contoso 會進行 Azure SQL Database 與 SQL Server 受控執行個體的功能比較。 下列考慮可協助他們決定使用受控執行個體。

- 受控執行個體的目標是為最新版內部部署 SQL Server 提供幾乎 100% 的相容性。 對於在內部部署或 IaaS Vm 上執行 SQL Server 的客戶，Microsoft 建議受控執行個體，而且想要將其應用程式遷移至完全受控的服務，並使用最少的設計變更。
- Contoso 計劃將大量應用程式從內部部署環境遷移到 IaaS。 有許多都是由 ISV 提供。 Contoso 會發現使用受控執行個體有助於確保這些應用程式的資料庫相容性，而不是使用可能不支援的 SQL Database。
- Contoso 可以使用完全自動化的 Azure 資料庫移轉服務，執行隨即轉移至受控執行個體。 備妥此服務，Contoso 可以將它重複使用於未來的資料庫移轉。
- SQL 受控執行個體支援 SQL Server Agent，這是 SmartHotel360 應用程式的重要元件。 Contoso 需要此相容性，否則就必須重新設計應用程式所需的維護方案。
- Contoso 可以透過軟體保證使用適用於 SQL Server 的 Azure Hybrid Benefit，以折扣優惠在 SQL Database 受控執行個體上交換執行個體的現有授權。 這可讓 Contoso 最多節省 30% 的受控執行個體。
- SQL 受控執行個體完全包含在虛擬網路中，因此可為 Contoso 的資料提供更高的隔離和安全性。 Contoso 可享有公用雲端的優勢，同時讓環境與公用網際網路保持隔離。
- 受控執行個體支援許多安全性功能，包括永遠加密、動態資料遮罩、資料列層級安全性和威脅偵測。

### <a name="solution-review"></a>解決方案檢閱

Contoso 會透過比較一份優缺點清單，來評估建議設計。

<!-- markdownlint-disable MD033 -->

| **考量** | **詳細資料** |
| --- | --- |
| **優點** | WEBVM 會移至 Azure (不需變更)，讓移轉變得更簡單。 <br><br> SQL 受控執行個體可援 Contoso 的技術需求和目標。 <br><br> 受控執行個體將提供與其目前部署的 100% 相容性，同時將其從 SQL Server 2008 R2 中移走。 <br><br> 他們可以使用 SQL Server 和 Windows Server 的 Azure Hybrid Benefit，來妥善運用軟體保證中的投資。 <br><br> 他們可以重複使用 Azure 資料庫移轉服務進行其他未來的移轉作業。 <br><br> SQL 受控執行個體具有 Contoso 不需設定的內建容錯功能。 這可確保資料層不再是單一的容錯移轉點。 |
| **缺點** | WEBVM 會執行 Windows Server 2008 R2。 雖然 Azure 支援此作業系統，但它不再是支援的平台。 [深入了解](https://support.microsoft.com/help/956893)。 <br><br> Web 層會保留只有 WEBVM 提供服務的單一容錯移轉點。 <br><br> Contoso 必須繼續支持此應用程式 Web 層作為 VM，而不是轉向 Azure App Service 等受控服務。 <br><br> 在資料層中，如果 Contoso 想要自訂作業系統或資料庫伺服器，或是想要執行第三方應用程式以及 SQL Server，受控執行個體可能不是最佳解決方案。 在 IaaS VM 上執行 SQL Server 可提供此種彈性。 |

<!-- markdownlint-enable MD033 -->

### <a name="migration-process"></a>移轉程序

Contoso 會完成下列步驟，以將 SmartHotel360 應用程式的 Web 和資料層遷移至 Azure：

1. Contoso 的 Azure 基礎結構已經備妥，所以只需要為此案例新增幾個特定 Azure 元件即可。
2. 資料層會使用 Azure 資料庫移轉服務進行移轉。 此服務會透過 Contoso 資料中心與 Azure 之間的站對站 VPN 連線，來連線至內部部署 SQL Server VM。 接著，服務會移轉資料庫。
3. Web 層將使用 [Azure Migrate 的隨即轉移來進行遷移。 此程序需要準備內部部署 VMware 環境、設定和啟用複寫，以及將 VM 容錯移轉至 Azure 以便遷移。

     ![移轉架構](./media/contoso-migration-rehost-vm-sql-managed-instance/migration-architecture.png)

### <a name="azure-services"></a>Azure 服務

| 服務 | 描述 | 成本 |
| --- | --- | --- |
| [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) | Azure 資料庫移轉服務能夠從多個資料庫來源無縫移轉到 Azure 資料平台，將停機時間降到最低。 | 瞭解[支援的區域](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability)和 | [資料庫移轉服務定價](https://azure.microsoft.com/pricing/details/database-migration)。 |
| [Azure SQL Database 受控執行個體](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) | 受控執行個體是一種受控資料庫服務，可代表 Azure 雲端中的完全受控 SQL Server 執行個體。 它會使用與最新版 SQL Server 資料庫引擎相同的程式碼，並擁有最新的功能、效能增強功能和安全性修補程式。 | 使用在 Azure 中執行的 SQL Database 受控執行個體會根據所用容量產生費用。 深入了解[受控執行個體定價](https://azure.microsoft.com/pricing/details/sql-database/managed)。 |
| [Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-services-overview) | Contoso 會使用 Azure Migrate 服務來 | 評估其 VMware Vm。 Azure Migrate 評估遷移的適用性 | 的電腦。 它提供在中執行的大小調整和成本預估 | Azure。 | 截至 2018 年 5 月，Azure Migrate 是一項免費服務。 |

## <a name="prerequisites"></a>必要條件

Contoso 和其他使用者都必須符合此案例的下列先決條件：

<!-- markdownlint-disable MD033 -->

| 規格需求 | 詳細資料 |
| --- | --- |
| **Azure 訂用帳戶** | 當您在這系列的第一篇文章中執行評量時，您應該已經建立訂用帳戶。 如果您沒有 Azure 訂用帳戶，請建立[免費帳戶](https://azure.microsoft.com/pricing/free-trial)。 <br><br> 如果您建立免費帳戶，您就是訂用帳戶的管理員，並可執行所有動作。 <br><br> 如果您使用現有的訂用帳戶，而且您不是訂用帳戶的系統管理員，則必須與管理員合作，將所需資源群組和資源的擁有者或參與者許可權指派給您。 |
| **Azure 基礎結構** | Contoso 會如[適用於移轉的 Azure 基礎結構](./contoso-migration-infrastructure.md)中所述，設定其 Azure 基礎結構。 |
| **內部部署伺服器** | 內部部署 vCenter 伺服器應執行版本5.5、6.0 或6.5。 <br><br> 執行5.5、6.0 或6.5 版的 ESXi 主機。 <br><br> 一或多部在 ESXi 主機上執行的 VMware VM。 |
| **內部部署 VM** | 檢閱已背書在 Azure 上執行的 [Linux 機器](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)。 |
| **資料庫移轉服務** | 要使用 Azure 資料庫移轉服務，您必須要有[相容的內部部署 VPN 裝置](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices)。 <br><br> 您必須能夠設定內部部署 VPN 裝置。 它必須有對外開放的公用 IPv4 位址。 此位址不能位於 NAT 裝置後方。 <br><br> 請確定您可以存取內部部署 SQL Server 資料庫。 <br><br> Windows 防火牆應該要能存取來源資料庫引擎。 了解如何[設定用於 Database Engine 存取的 Windows 防火牆](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access)。 <br><br> 如果資料庫機器前面有防火牆，請新增規則以允許存取資料庫，以及允許透過 SMB 連接埠 445 存取檔案。 <br><br> 用來連線至來源 SQL Server 執行個體和目標受控執行個體的認證，必須是 sysadmin 伺服器角色的成員。 <br><br> 內部部署資料庫中必須有可供 Azure 資料庫移轉服務用來備份來源資料庫的網路共用。 <br><br> 請確定執行來源 SQL Server 執行個體的服務帳戶擁有網路共用的寫入權限。 <br><br> 請記下擁有網路共用完整控制權限的 Windows 使用者和密碼。 Azure 資料庫移轉服務會模擬這些使用者認證，以將備份檔案上傳至 Azure 儲存體容器。 <br><br> SQL Server Express 安裝程序預設會將 TCP/IP 通訊協定設定為**停用**。 請務必將其啟用。 |

<!-- markdownlint-enable MD033 -->

## <a name="scenario-steps"></a>案例步驟

以下說明 Contoso 打算如何設定部署：

> [!div class="checklist"]
>
> - **步驟1：準備 SQL Database 受控執行個體。** Contoso 需要現有的受控執行個體，以作為內部部署 SQL Server 資料庫的移轉目的地。
> - **步驟2：準備 Azure 資料庫移轉服務。** Contoso 必須註冊資料庫移轉提供者、建立執行個體，然後建立 Azure 資料庫移轉服務專案。 Contoso 也必須為 Azure 資料庫移轉服務設定共用存取簽章 (SAS) 統一資源識別項 (URI)。 SAS URI 可提供 Contoso 儲存體帳戶資源的委派存取權，以便 Contoso 對儲存體物件授與有限的權限。 Contoso 會設定 SAS URI，以便 Azure 資料庫移轉服務存取儲存體帳戶容器 (此為服務在上傳 SQL Server 備份檔案時的上傳目的地)。
> - **步驟3：準備 Azure 以進行 Azure Migrate：伺服器遷移。** 他們將伺服器移轉工具新增至其 Azure Migrate 專案。
> - **步驟4：準備內部部署 VMware 以進行 Azure Migrate：伺服器遷移。** 他們會準備帳戶以進行 VM 探索，並準備在遷移後連線到 Azure Vm。
> - **步驟5：複寫 Vm。** 他們要設定複寫，然後開始將 VM 複寫至 Azure 儲存體。
> - **步驟6：使用 Azure 資料庫移轉服務遷移資料庫。** Contoso 遷移資料庫。
> - **步驟7：使用 Azure Migrate：伺服器遷移來遷移 VM。** 他們會執行測試遷移，確定一切都能正常運作，然後執行完整遷移來將 Vm 移至 Azure。

## <a name="step-1-prepare-a-sql-database-managed-instance"></a>步驟 1：準備 SQL Database 受控執行個體

若要設定 Azure SQL Database 受控執行個體，Contoso 需要一個符合下列要求的子網路：

- 此子網路必須是專用的。 它必須空白，不得包含任何其他雲端服務。 子網路不能是閘道子網路。
- 建立受控執行個體後，Contoso 就不得在該子網路中新增資源。
- 子網路不能有與其相關聯的網路安全性群組。
- 子網路必須有使用者定義的路由表。 唯一指派的路由應該是 0.0.0.0/0 下一個躍點網際網路。
- 如果為虛擬網路指定了選擇性的自訂 DNS，則 `168.63.129.16` 必須將 Azure 中遞迴解析程式的虛擬 IP 位址新增到清單中。 瞭解如何[設定 Azure SQL Database 受控執行個體的自訂 DNS](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns)。
- 子網路不得有相關聯的服務端點 (儲存體或 SQL)。 虛擬網路上應該停用服務端點。
- 子網路必須至少具有 16 個 IP 位址。 了解如何[調整受控執行個體子網路的大小](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration)。
- 在 Contoso 的混合式環境中，需要有自訂 DNS 設定。 Contoso 會將 DNS 設定配置為使用公司的其中一或多部 Azure DNS 伺服器。 深入瞭解[DNS 自訂](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns)。

### <a name="set-up-a-virtual-network-for-the-managed-instance"></a>設定受控執行個體的虛擬網路

Contoso 管理員會設定虛擬網路，如下所示：

1. 他們會在主要美國東部 2 區域中建立新的虛擬網路 (**VNET-SQLMI-EU2**)。 它會將此虛擬網路新增至 **ContosoNetworkingRG** 資源群組。
2. 他們會指派 10.235.0.0/24 位址空間。 他們會確保範圍不會與其企業中的任何其他網路重疊。
3. 他們會將兩個子網路新增到網路：
    - **互連 vnet-sqlmi-eus2-DS-EUS2** （10.235.0.0.25）。
    - **互連 vnet-sqlmi-eus2-EUS2** （10.235.0.128/29）。 此子網路用來將目錄連結到受控執行個體。

      ![受控執行個體 - 建立虛擬網路](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-vnet.png)

4. 部署虛擬網路和子網路之後，他們會將網路對等互連，如下所示：

    - 對等互連 **VNET-SQLMI-EUS2** 與 **VNET-HUB-EUS2** (美國東部 2 的中樞虛擬網路)。
    - 對等**vnet-互連 vnet-sqlmi-eus2-EUS2**與**VNET-生產 EUS2** （生產網路）。

      ![網路對等互連](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-peering.png)

5. 他們可設定自訂 DNS 設定。 DNS 會先指向 Contoso 的 Azure 網域控制站。 而後指向 Azure DNS。 Contoso Azure 網域控制站的位置如下所示：

    - 位於美國東部2生產網路（**EUS2**）中的**EUS2**子網中。
    - **CONTOSODC3**位址：10.245.42.4。
    - **CONTOSODC4**位址：10.245.42.5。
    - Azure DNS 解析程式：168.63.129.16。

      ![網路 DNS 伺服器](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-dns.png)

**需要其他協助？**

- 閱讀[SQL Database 受控執行個體的總覽](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance)。
- 了解如何[建立 SQL Database 受控執行個體的虛擬網路](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration)。
- 了解如何[設定對等互連](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering)。
- 了解如何[更新 Azure Active Directory DNS 設定](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-instance)。

### <a name="set-up-routing"></a>設定路由

受控執行個體會置於私人虛擬網路中。 Contoso 需要路由表，以供虛擬網路與 Azure 管理服務通訊。 如果虛擬網路無法與管理它的服務通訊，則會變成無法存取。

Contoso 會考量下列因素：

- 路由表包含一組規則 (路由)，可指定從受控執行個體傳送的封包應如何在虛擬網路中路由傳送。
- 路由表與受控實例部署所在的子網相關聯。 每個離開子網路的封包都會依據相關聯的路由表進行處理。
- 一個子網路只能與一個路由表相關聯。
- 在 Microsoft Azure 中建立路由表，沒有任何額外的費用。

 若要設定路由，Contoso 管理員會執行下列動作：

1. 他們會在 **ContosoNetworkingRG** 資源群組中建立使用者定義的路由表。

    ![路由表](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table.png)

2. 若要符合受控執行個體需求，在部署路由表 (**MIRouteTable**) 之後，他們會新增位址前置詞為 0.0.0.0/0 的路由。 [下一個躍點類型]**** 選項會設定為 [網際網路]****。

    ![路由表前置詞](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-prefix.png)

3. 他們會讓路由表與 **SQLMI-DB-EUS2** 子網路 (在 **VNET-SQLMI-EUS2** 網路中) 建立關聯。

    ![路由表子網路](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-subnet.png)

**需要其他協助？**

了解如何[設定受控執行個體的路由](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started)。

### <a name="create-a-managed-instance"></a>建立受控執行個體

Contoso 管理員現在可以佈建 SQL Database 受控執行個體：

1. 因為受控執行個體會提供一個商務應用程式，所以他們會在公司的主要美國東部 2 區域中部署受控執行個體。 他們會將受控執行個體新增至 **ContosoRG** 資源群組。
2. 他們會選取執行個體的定價層、大小計算和儲存體。 深入了解[受控執行個體定價](https://azure.microsoft.com/pricing/details/sql-database/managed)。

    ![受控執行個體](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-create.png)

3. 部署受控執行個體之後，**ContosoRG** 資源群組中會出現兩個新的資源：

    - 虛擬叢集 (假使 Contoso 有多個受控執行個體)。
    - SQL Server Database 受控執行個體。

      ![受控執行個體](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-resources.png)

**需要其他協助？**

了解如何[佈建受控執行個體](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started)。

## <a name="step-2-prepare-the-azure-database-migration-service"></a>步驟2：準備 Azure 資料庫移轉服務

若要準備 Azure 資料庫移轉服務，Contoso 管理員需要執行一些作業：

- 在 Azure 中註冊 Azure 資料庫移轉服務提供者。
- 將 Azure 儲存體的存取權提供給 Azure 資料庫移轉服務，以便上傳用來移轉資料庫的備份檔案。 為了提供 Azure 儲存體的存取權，他們會建立 Azure Blob 儲存體容器。 他們會產生 Blob 儲存體容器的 SAS URI。
- 建立 Azure 資料庫移轉服務專案。

然後，他們會完成下列步驟：

1. 他們會在其訂用帳戶之下註冊資料庫移轉提供者。
    ![資料庫移轉服務 - 註冊](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-subscription.png)

2. 他們會建立 Blob 儲存體容器。 Contoso 會產生 SAS URI，以便 Azure 資料庫移轉服務加以存取。

    ![資料庫移轉服務 - 產生 SAS URI](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-sas.png)

3. 他們會建立 Azure 資料庫移轉服務執行個體。

    ![資料庫移轉服務 - 建立執行個體](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-instance.png)

4. 他們會將 Azure 資料庫移轉服務執行個體置於 **VNET-PROD-DC-EUS2** 虛擬網路的 **PROD-DC-EUS2** 子網路中。
    - Azure 資料庫移轉服務會放在該處，因為此服務必須在可透過 VPN 閘道存取內部部署 SQL Server VM 的虛擬網路中。
    - **Vnet-生產 EUS2**會對等互連至**VNET 中樞-EUS2** ，而且可以使用遠端閘道。 [使用遠端閘道]**** 選項可確保 Azure 資料庫移轉服務可視需要進行通訊。

        ![資料庫移轉服務 - 設定網路](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-network.png)

**需要其他協助？**

- 了解如何[設定 Azure 資料移轉服務](https://docs.microsoft.com/azure/dms/quickstart-create-data-migration-service-portal)。
- 了解如何[建立和使用 SAS](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2)。

## <a name="step-3-prepare-azure-for-the-azure-migrate-server-migration-tool"></a>步驟3：準備 Azure 以進行 Azure Migrate：伺服器遷移工具

以下是 Contoso 將 VM 移轉至 Azure 時，所需的 Azure 元件：

- 在遷移期間建立 Azure Vm 時，將會在其中尋找其所在的 VNet。
- Azure Migrate：伺服器遷移工具已布建。

他們依照下列方式進行其設定：

1. **設定網路：** Contoso 已設定可用於 Azure Migrate 的網路：[部署 Azure 基礎結構](./contoso-migration-infrastructure.md)時的伺服器遷移

    - SmartHotel360 應用程式為生產應用程式，而且 VM 會被移轉至美國東部 2 主要區域的 Azure 生產網路 (VNET-PROD-EUS2)。
    - 這兩個 VM 會置於可作為生產資源的 ContosoRG 資源群組中。
    - 應用程式前端 VM (WEBVM) 將移轉至生產網路中的前端子網路 (PROD-FE-EUS2)。
    - 應用程式前端 VM (SQLVM) 將移轉至生產網路中的資料庫子網路 (PROD-DB-EUS2)。

## <a name="step-4-prepare-on-premises-vmware-for-azure-migrate-server-migration"></a>步驟4：準備內部部署 VMware 以進行 Azure Migrate：伺服器遷移

以下是 Contoso 將 VM 移轉至 Azure 時，所需的 Azure 元件：

- 在遷移期間建立 Azure Vm 時，將會在其中尋找其所在的 VNet。
- Azure Migrate：已布建並設定伺服器遷移工具（OVA）。

他們依照下列方式進行其設定：

1. 設定網路-Contoso 已設定可用於 Azure Migrate 的網路：[部署 Azure 基礎結構](./contoso-migration-infrastructure.md)時的伺服器遷移

    - SmartHotel360 應用程式為生產應用程式，而且 VM 會被移轉至美國東部 2 主要區域的 Azure 生產網路 (VNET-PROD-EUS2)。
    - 這兩個 VM 會置於可作為生產資源的 ContosoRG 資源群組中。
    - 應用程式前端 VM (WEBVM) 將移轉至生產網路中的前端子網路 (PROD-FE-EUS2)。
    - 應用程式前端 VM (SQLVM) 將移轉至生產網路中的資料庫子網路 (PROD-DB-EUS2)。

2. 布建 Azure Migrate：伺服器遷移工具。

    - 從 Azure Migrate 下載 OVA 映射，並將其匯入 VMWare。

        ![下載 OVA 檔案](./media/contoso-migration-rehost-vm/migration-download-ova.png)

    - 啟動匯入的映射並設定工具，包括下列步驟：

      - 設定必要條件。

        ![設定工具](./media/contoso-migration-rehost-vm/migration-setup-prerequisites.png)

      - 將工具指向 Azure 訂用帳戶。

        ![設定工具](./media/contoso-migration-rehost-vm/migration-register-azure.png)

      - 設定 VMWare vCenter 認證。

        ![設定工具](./media/contoso-migration-rehost-vm/migration-vcenter-server.png)

      - 新增任何以 Linux 或 Windows 為基礎的認證以供探索。

        ![設定工具](./media/contoso-migration-rehost-vm/migration-credentials.png)

3. 設定完成後，工具會花一些時間來列舉所有虛擬機器。 完成後，您會看到它們填入 Azure 中的 [Azure Migrate] 工具。

**需要其他協助？**

瞭解如何設定[Azure Migrate：伺服器遷移工具](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-server-migration-tool)。

### <a name="prepare-on-premises-vms"></a>準備內部部署 Vm

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

   - 瞭解如何[準備 vm 以進行遷移](https://docs.microsoft.com/azure/migrate/contoso-migration-rehost-vm#prepare-vms-for-migration)。

## <a name="step-5-replicate-the-on-premises-vms"></a>步驟5：複寫內部部署 Vm

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

## <a name="step-6-migrate-the-database-using-the-azure-database-migration-service"></a>步驟6：使用 Azure 資料庫移轉服務遷移資料庫

Contoso 管理員必須建立 Azure 資料庫移轉服務專案，然後移轉資料庫。

### <a name="create-an-azure-database-migration-service-project"></a>建立 Azure 資料庫移轉服務專案

1. 他們會建立 Azure 資料庫移轉服務專案。 他們會選取 [SQL Server]**** 來源伺服器類型，並以 [Azure SQL Database 受控執行個體]**** 作為目標。

     ![資料庫移轉服務 - 新增移轉專案](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-project.png)

2. 移轉精靈隨即開啟。

### <a name="migrate-the-database"></a>遷移資料庫

1. 在移轉精靈中，他們會指定內部部署資料庫所在的來源 VM。 他們會提供用於存取資料庫的認證。

    ![資料庫移轉服務 - 來源詳細資料](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-source.png)

2. 他們會選取要遷移的資料庫（**SmartHotel**）：

    ![資料庫移轉服務 - 選取來源資料庫](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-source-db.png)

3. 對此目標，他們會輸入 Azure 中的受控執行個體名稱，以及存取認證。

    ![資料庫移轉服務 - 目標詳細資料](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-target-details.png)

4. 在 [**新增活動**] [執行] [  >  **遷移**] 中，他們會指定執行遷移的設定：
    - 來源和目標認證。
    - 要遷移的資料庫。
    - 在內部部署 VM 上建立的網路共用。 Azure 資料庫移轉服務會將來源備份擷取到此共用。
        - 執行來源 SQL Server 執行個體的服務帳戶必須擁有此共用的寫入權限。
        - 必須使用共用的 FQDN 路徑。
    - SAS URI，此 URI 可供 Azure 資料庫移轉服務存取它將上傳備份以供移轉的目標儲存體帳戶容器。

        ![資料庫移轉服務 - 設定移轉設定](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-migration-settings.png)

5. 他們會儲存移轉設定，然後執行移轉。
6. 在 [概觀]**** 中，他們會監視移轉狀態。

    ![資料庫移轉服務 - 監視](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor1.png)

7. 完成移轉後，他們會確認受控執行個體上有目標資料庫。

    ![資料庫移轉服務 - 驗證資料庫移轉](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor2.png)

## <a name="step-7-migrate-the-vms-with-azure-migrate-server-migration"></a>步驟7：使用 Azure Migrate 遷移 Vm：伺服器遷移

Contoso 管理員會執行快速測試遷移，並確認 VM 正常運作，然後再遷移 VM。

### <a name="run-a-test-migration"></a>執行測試移轉

1. 在 [移轉目標]   > [伺服器]   >  **[Azure Migrate：伺服器移轉]** 中，按一下 [測試遷移的伺服器]  。

     ![測試遷移的伺服器](./media/contoso-migration-rehost-linux-vm/test-migrated-servers.png)

2. 以滑鼠右鍵按一下要測試的 VM，然後按一下 [測試遷移]  。

    ![測試移轉](./media/contoso-migration-rehost-linux-vm/test-migrate.png)

3. 在 [測試移轉]  中，選取 Azure VM 在移轉後將位於其中的 Azure VNet。 我們建議使用非生產 VNet。
4. **測試移轉**作業隨即啟動。 請在入口網站通知中監視作業。
5. 移轉完成之後，請在 Azure 入口網站的 [虛擬機器]  中檢視已遷移的 Azure VM。 機器名稱會具有尾碼 **-Test**。
6. 測試完成之後，以滑鼠右鍵按一下 [複寫機器]  中的 Azure VM，然後按一下 [清除測試移轉]  。

    ![清除移轉](./media/contoso-migration-rehost-linux-vm/clean-up.png)

### <a name="migrate-the-vms"></a>遷移 VM

Contoso 管理員現在會執行完整的遷移，以完成移動。

1. 在 [Azure Migrate 專案] > [伺服器]   >  **[Azure Migrate：伺服器移轉]** 中，按一下 [複寫伺服器]  。

    ![複寫伺服器](./media/contoso-migration-rehost-linux-vm/replicating-servers.png)

2. 在 [複寫機器]  中，以滑鼠右鍵按一下 VM > [遷移]  。
3. 在 [遷移]   > [將虛擬機器關機，在沒有資料遺失的情況下執行計劃性移轉]  中，選取 [是]   > [確定]  。
    - 根據預設，Azure Migrate 會關閉內部部署 VM，並執行隨選複寫，以同步處理上次複寫執行後發生的任何 VM 變更。 這樣可確保不會遺失任何資料。
    - 如果您不想要關閉 VM，請選取 [**否**]。
4. VM 會啟動移轉作業。 請在 Azure 通知中追蹤該作業。
5. 作業完成後，您可以從 [虛擬機器]  頁面檢視及管理 VM。
6. 最後，他們會在其中一個 Contoso 網域控制站上更新**WEBVM**的 DNS 記錄。

### <a name="update-the-connection-string"></a>更新連接字串

在移轉程序的最後一個步驟中，Contoso 管理員會將應用程式的連接字串更新為指向在 Contoso 受控執行個體上執行的遷移後資料庫。

1. 在 Azure 入口網站中，他們會藉由選取 [**設定**] [連接字串] 來尋找連接字串  >  ** **。

    ![連接字串](./media/contoso-migration-rehost-vm-sql-managed-instance/failover4.png)

2. 他們會使用 SQL Database 受控執行個體的使用者名稱與密碼來更新此字串。
3. 設定此字串之後，他們會取代其應用程式的 web.config 檔案中目前的連接字串。
4. 更新該檔案並加以儲存後，他們會在命令提示字元視窗中執行 `iisreset /restart`，以在 WEBVM 上重新啟動 IIS。
5. 重新啟動 IIS 後，應用程式會使用在 SQL Database 受控執行個體上執行的資料庫。
6. 此時，他們可以關閉內部部署 SQLVM 機器。 已完成移轉。

**需要其他協助？**

- 瞭解如何[執行測試容錯移轉](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure)。
- 瞭解如何[建立復原方案](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans)。
- 了解如何[容錯移轉至 Azure](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover)。

## <a name="clean-up-after-migration"></a>移轉之後進行清除

移轉完成時，SmartHotel360 應用程式會在 Azure VM 上執行，而 SmartHotel360 資料庫可適用於 Azure SQL Database 受控執行個體。

現在，Contoso 必須執行下列清除工作：

- 從 vCenter Server 清查中移除 WEBVM 機器。
- 從 vCenter Server 清查中移除 SQLVM 機器。
- 從本機備份作業移除 WEBVM 和 SQLVM。
- 更新內部文件以顯示 WEBVM 的新位置和 IP 位址。
- 從內部文件中移除 SQLVM。 或者，Contoso 可以修改文件，將 SQLVM 顯示為已刪除且已不在 VM 清查中。
- 檢閱與已解除委任 VM 互動的任何資源。 更新任何相關的設定或文件，以反映新的組態。

## <a name="review-the-deployment"></a>檢閱部署

對於 Azure 中的遷移後資源，Contoso 必須能執行一切功能並保護其新的基礎結構。

### <a name="security"></a>安全性

Contoso 安全性小組會檢閱 Azure VM 和 SQL Database 受控執行個體，檢查其實作是否有任何的安全疑慮：

- 小組會檢閱用來控制 VM 存取權的網路安全性群組。 網路安全性群組有助於確保只可以傳遞該應用程式允許的流量。
- Contoso 安全性小組也會考慮使用 Azure 磁碟加密和 Azure KeyVault 來保護磁碟上的資料。
- 小組會在受控執行個體上啟用威脅偵測。 如果偵測到威脅，威脅偵測會將警示傳送至 Contoso 的安全性小組/服務台系統。 深入了解[受控執行個體的威脅偵測](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-threat-detection)。

     ![受控執行個體安全性 - 威脅偵測](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-security.png)

若要深入了解 VM 的安全性做法，請參閱 [Azure 中 IaaS 工作負載的安全性最佳做法](https://docs.microsoft.com/azure/security/fundamentals/iaas)。

### <a name="bcdr"></a>BCDR

針對商務持續性和災害復原 (BCDR)，Contoso 採取下列動作：

- 保護資料安全：Contoso 會使用 Azure 備份服務來備份 VM 上的資料。 [深入了解](https://docs.microsoft.com/azure/backup/backup-overview)。
- 保持應用程式啟動及執行：Contoso 會使用 Site Recovery，在 Azure 中將應用程式 VM 複寫至次要區域。 [深入了解](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart)。
- Contoso 深入了解如何管理 SQL 受控執行個體，包括[資料庫備份](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups)。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- Contoso 有 WEBVM 的現有授權。 為了利用 Azure Hybrid Benefit 的定價，Contoso 會轉換現有的 Azure VM。
- Contoso 會使用[Azure 成本管理](https://azure.microsoft.com/services/cost-management)，確保他們會在其 IT 領導地位所建立的預算內保持不變。

## <a name="conclusion"></a>結論

在本文中，Contoso 會在 Azure 中重新裝載 SmartHotel360 應用程式，方法是使用 Azure Migrate 服務，將應用程式前端 VM 遷移至 Azure。 Contoso 會使用 Azure 資料庫移轉服務，將內部部署資料庫遷移至 Azure SQL Database 受控執行個體。
