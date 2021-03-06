---
title: 藉由遷移至 Azure Vm 和 Azure SQL 受控實例來重新裝載內部部署應用程式
description: 瞭解 Contoso 如何使用 Azure SQL 受控實例，在 Azure Vm 上如何內部部署應用程式。
author: givenscj
ms.author: abuck
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: think-tank
ms.openlocfilehash: fa17261cf94d1f07fc51f23904b8a30a88036640
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101787037"
---
<!-- cSpell:ignore WEBVM SQLVM OSTICKETWEB OSTICKETMYSQL contosohost vcenter contosodc NSGs agentless SQLMI iisreset -->

# <a name="rehost-an-on-premises-application-by-migrating-to-azure-vms-and-azure-sql-managed-instance"></a>藉由遷移至 Azure Vm 和 Azure SQL 受控實例來重新裝載內部部署應用程式

本文說明虛構公司 Contoso 如何使用 Azure 遷移，將在 VMware 虛擬機器上執行的兩層式 Windows .NET 前端應用程式遷移 (Vm) 至 Azure VM。 它也會說明 Contoso 如何將應用程式資料庫移轉至 Azure SQL 受控實例。

此範例中使用的 SmartHotel360 應用程式是以開放原始碼軟體的形式提供。 如果您想要將它用於您自己的測試用途，請從 [GitHub](https://github.com/Microsoft/SmartHotel360)下載。

## <a name="business-drivers"></a>商業動機

Contoso 的 IT 領導小組與公司的商務合作夥伴密切合作，以瞭解企業想要使用這種方式達成什麼目標。 他們想要：

- **解決業務成長。** Contoso 正在成長。 因此，該公司的內部部署系統和基礎結構所承受的壓力變大了。
- **提高效率。** Contoso 必須移除不必要的程序，並簡化其開發人員和使用者的程序。 企業需要快速且不浪費時間或金錢，讓公司更快提供客戶的需求。
- **增加靈活性。** Contoso IT 必須能夠更快因應企業的需求。 它必須比在 marketplace 中發生的變更更快，讓公司在全球經濟中成功。 Contoso 的 IT 小組不得礙事，或成為企業的絆腳石。
- **規模。** 隨著該公司的業務順利成長，Contoso IT 必須提供能夠同步成長的系統。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已確定此次移轉的目標。 該公司會使用移轉目標來判斷最合適的移轉方法。

- 在遷移之後，Azure 中的應用程式應該在 Contoso 的內部部署 VMware 環境中具有與應用程式相同的效能功能。 移至雲端並不表示應用程式效能較不重要。
- Contoso 不想投資應用程式。 應用程式對企業至關重要且重要，但 Contoso 只想將應用程式的目前形式移至雲端。
- 在應用程式遷移之後，資料庫管理工作應最小化。
- Contoso 不想對此應用程式使用 Azure SQL Database。 且正在尋求替代方案。

## <a name="solution-design"></a>解決方案設計

完成公司的目標和需求後，Contoso 會設計和審核部署解決方案，並識別遷移程式。 也會識別其將用於遷移的 Azure 服務。

### <a name="current-architecture"></a>目前架構

- Contoso 有一個主要的資料中心 (`contoso-datacenter`) 。 資料中心位於美國東部的紐約市。
- Contoso 在全美另有三家地區性分公司。
- 主要資料中心會使用光纖 Metro Ethernet 連線連線到網際網路 (每秒 500 mb 的) 。
- 每家分公司皆使用企業級連線從本機連到網際網路，並透過 IPsec VPN 通道連回主要資料中心。 此設定可讓 Contoso 的整個網路永久連線，並將網際網路連線最佳化。
- 主要資料中心已透過 VMware 完全虛擬化。 Contoso 有兩部 ESXi 6.5 虛擬化主機，均由 vCenter Server 6.5 管理。
- Contoso 使用 Active Directory 來管理身分識別。 Contoso 會使用內部網路上的 DNS 伺服器。
- Contoso 有 () 的內部部署網域控制站 `contosodc1` 。
- 網域控制站會在 VMware VM 上執行。 地區分公司的網域控制站會在實體伺服器上執行。
- SmartHotel360 應用程式會分層至兩個 Vm (`WEBVM` 和 `SQLVM` 位於 VMware ESXi 6.5 版主機 () 上的) `contosohost1.contoso.com` 。
- VMware 環境是由 VM 上執行的 vCenter Server 6.5 () 所管理 `vcenter.contoso.com` 。

![目前 Contoso 架構的圖表。](./media/contoso-migration-rehost-vm-sql-managed-instance/contoso-architecture.png)

### <a name="proposed-architecture"></a>建議的架構

在此案例中，Contoso 想要遷移其兩層內部部署旅遊應用程式，如下所示：

- 將應用程式資料庫 (`SmartHotelDB`) 遷移至 SQL 受控實例。
- 將前端遷移 `WEBVM` 至 AZURE VM。
- 移轉完成後，將會解除委任 Contoso 資料中心的內部部署 VM。

![案例架構的圖表。](./media/contoso-migration-rehost-vm-sql-managed-instance/architecture.png)

### <a name="database-considerations"></a>資料庫考量

在解決方案設計過程中，Contoso 會在 Azure SQL Database 和 SQL 受控實例之間進行功能比較。 下列考慮有助於公司決定使用 SQL 受控實例。

- SQL 受控實例的目標是要提供與最新內部部署 SQL Server 版本幾乎100% 的相容性。 針對執行 SQL Server 內部部署或基礎結構即服務 (IaaS) Vm，並想要將其應用程式遷移至完全受控的服務，並將設計變更降至完全受控的服務，我們建議使用 SQL 受控實例。
- Contoso 打算將大量的應用程式從內部部署遷移至 IaaS。 其中有許多應用程式都是 ISV 提供的。 Contoso 發現，使用 SQL 受控實例有助於確保這些應用程式的資料庫相容性，而不是使用 SQL Database，這可能不受支援。
- Contoso 可以使用完全自動化的 Azure 資料庫移轉服務，對 SQL 受控實例執行隨即轉移。 備妥此服務，Contoso 可以將它重複使用於未來的資料庫移轉。
- SQL 受控實例支援 SQL Server Agent，這是 SmartHotel360 應用程式的重要元件。 Contoso 需要這種相容性。 否則，就必須重新設計應用程式所需的維護計畫。
- 透過軟體保證，Contoso 可以使用適用于 SQL Server 的 Azure 混合式權益，將其現有授權交換至 SQL 受控實例上的折扣費率。 基於這個理由，Contoso 最多可省下30% 的 SQL 受控實例費用。
- SQL 受控實例完全包含在虛擬網路中，因此可為 Contoso 的資料提供更高的隔離和安全性。 Contoso 可以取得公用雲端的優點，同時讓環境與公用網際網路隔離。
- SQL 受控實例支援許多安全性功能。 其中包括永遠加密、動態資料遮罩、資料列層級安全性和威脅偵測。

### <a name="solution-review"></a>解決方案檢閱

Contoso 藉由結合一份優缺點來評估提議的設計。

| 考量 | 詳細資料 |
| --- | --- |
| **優點** | `WEBVM` 將會移至 Azure 而不需要變更，以簡化遷移工作。 <br><br> SQL 受控執行個體可援 Contoso 的技術需求和目標。 <br><br> SQL 受控實例在將公司移離 SQL Server 2008 R2 時，會提供與 Contoso 目前部署的100% 相容性。 <br><br> Contoso 可以利用其軟體保證的投資，並使用適用于 SQL Server 和 Windows Server 的 Azure 混合式權益。 <br><br> Contoso 可以重複使用 Azure 資料庫移轉服務來進行其他未來的遷移。 <br><br> SQL 受控實例具有內建的容錯功能，Contoso 不需要設定。 這項功能可確保資料層不再是單一失敗點。 |
| **缺點** | `WEBVM` 正在執行 Windows Server 2008 R2。 雖然 Azure 支援此作業系統，但不再是支援的平臺。 若要深入瞭解，請參閱 [MICROSOFT SQL Server 產品的支援原則](/troubleshoot/sql/general/support-policy-hardware-virtualization-product)。 <br><br> Web 層會維持單一容錯移轉點，只 `WEBVM` 提供服務。 <br><br> Contoso 必須繼續支援應用程式 web 層作為 VM，而不是移至受控服務（例如 Azure App Service）。 <br><br> 若為資料層，如果 Contoso 想要自訂作業系統或資料庫伺服器，或如果公司想要執行協力廠商應用程式以及 SQL Server，則 SQL 受控實例可能不是最佳解決方案。 在 IaaS VM 上執行 SQL Server 可提供此種彈性。 |

### <a name="migration-process"></a>移轉程序

Contoso 會完成下列步驟，將其 SmartHotel360 應用程式的 web 和資料層遷移至 Azure：

1. Contoso 的 Azure 基礎結構已經備妥，所以只需要為此案例新增幾個特定 Azure 元件即可。
1. 資料層將會使用 Azure 資料庫移轉服務進行遷移。 此服務會跨 Contoso 資料中心與 Azure 之間的站對站 VPN 連線，連接至內部部署 SQL Server VM。 接著，服務會移轉資料庫。
1. Web 層將會透過使用「Azure 遷移」來使用隨即轉移進行遷移。 此程序需要準備內部部署 VMware 環境、設定和啟用複寫，以及將 VM 容錯移轉至 Azure 以便遷移。

     ![遷移架構的圖表。](./media/contoso-migration-rehost-vm-sql-managed-instance/migration-architecture.png)

### <a name="azure-services"></a>Azure 服務

| 服務 | 描述 | 成本 |
| --- | --- | --- |
| [Azure 資料庫移轉服務](/azure/dms/dms-overview) | Azure 資料庫移轉服務可讓您從多個資料庫來源順暢地遷移到 Azure 資料平臺，並減少停機時間。 | 深入瞭解 [支援的區域](/azure/dms/dms-overview#regional-availability) 和 [Azure 資料庫移轉服務定價](https://azure.microsoft.com/pricing/details/database-migration/)。 |
| [Azure SQL 受控執行個體](/azure/azure-sql/managed-instance/sql-managed-instance-paas-overview) | SQL 受控實例是受控資料庫服務，代表 Azure 雲端中的完全受控 SQL Server 實例。 它會使用與最新版 SQL Server 資料庫引擎相同的程式碼，而且具有最新的功能、效能增強功能和安全性修補程式。 | 使用在 Azure 中執行的 SQL 受控實例會根據容量產生費用。 深入瞭解 [SQL 受控實例定價](https://azure.microsoft.com/pricing/details/azure-sql/sql-managed-instance/single/)。 |
| [Azure Migrate](/azure/migrate/migrate-services-overview) | Contoso 會使用 Azure 遷移來評定其 VMware Vm。 Azure Migrate 會評定機器是否適合移轉。 它會提供在 Azure 中執行的大小調整建議和成本估計。 | 不須額外費用即可使用 Azure Migrate。 它們可能會產生費用，視 (第一方或獨立軟體廠商的工具而定，) 他們決定用來進行評量和遷移。 深入瞭解 [Azure 遷移定價](https://azure.microsoft.com/pricing/details/azure-migrate/)。 |

## <a name="prerequisites"></a>必要條件

Contoso 和其他使用者必須符合此案例的下列必要條件。

| 需求 | 詳細資料 |
| --- | --- |
| **Azure 訂用帳戶** | Contoso 已在本系列的第一篇文章中建立訂用帳戶。 如果您沒有 Azure 訂用帳戶，請建立[免費帳戶](https://azure.microsoft.com/free/)。 <br><br> 如果您建立免費帳戶，您就是訂用帳戶的管理員，並可執行所有動作。 <br><br> 如果您使用現有的訂用帳戶，而且您不是訂用帳戶的系統管理員，請與系統管理員合作，將擁有者或參與者許可權指派給所需的資源群組和資源。 |
| **Azure 基礎結構** | Contoso 會如 Azure 基礎結構中所述，設定其 Azure 基礎結構 [以進行遷移](./contoso-migration-infrastructure.md)。 |
| **內部部署伺服器** | 內部部署 vCenter 伺服器應執行5.5、6.0 或6.5 版。 <br><br> ESXi 主機應該執行5.5、6.0 或6.5 版。 <br><br> 一或多部在 ESXi 主機上執行的 VMware VM。 |
| **內部部署 VM** | 檢閱已背書在 Azure 上執行的 [Linux 機器](/azure/virtual-machines/linux/endorsed-distros)。 |
| **Database Migration Service** | 針對 Azure 資料庫移轉服務，您需要相容的內部 [部署 VPN 裝置](/azure/vpn-gateway/vpn-gateway-about-vpn-devices)。 <br><br> 您必須能夠設定內部部署 VPN 裝置。 它必須有對外開放的公用 IPv4 位址。 此位址不能位於 NAT 裝置後方。 <br><br> 請確定您可以存取內部部署 SQL Server 資料庫。 <br><br> Windows 防火牆應該要能存取來源資料庫引擎。 瞭解如何 [設定適用于 database engine 存取的 Windows 防火牆](/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access)。 <br><br> 如果資料庫機器前面有防火牆，請新增規則以允許存取資料庫，以及允許透過 SMB 連接埠 445 存取檔案。 <br><br> 用來連線至來源 SQL Server 實例和目標 SQL 受控實例的認證，必須是系統管理員（sysadmin）伺服器角色的成員。 <br><br> 您的內部部署資料庫中必須有一個網路共用，讓 Azure 資料庫移轉服務可用來備份源資料庫。 <br><br> 請確定執行來源 SQL Server 執行個體的服務帳戶擁有網路共用的寫入權限。 <br><br> 請記下擁有網路共用完整控制權限的 Windows 使用者和密碼。 Azure 資料庫移轉服務會模擬這些使用者認證，以便將備份檔案上傳至 Azure 儲存體容器。 <br><br> SQL Server Express 安裝程序預設會將 TCP/IP 通訊協定設定為 **停用**。 請務必將其啟用。 |

## <a name="scenario-steps"></a>案例步驟

以下說明 Contoso 打算如何設定部署：

> [!div class="checklist"]
>
> - **步驟1：準備 SQL 受控實例。** Contoso 需要現有的受控執行個體，以作為內部部署 SQL Server 資料庫的移轉目的地。
> - **步驟2：準備 Azure 資料庫移轉服務。** Contoso 必須註冊資料庫移轉提供者、建立執行個體，然後建立資料庫移轉服務專案。 Contoso 也必須將共用存取簽章 (SAS) 統一資源識別項 (URI) 用於資料庫移轉服務實例。 SAS URI 可提供 Contoso 儲存體帳戶中資源的委派存取權，讓 Contoso 可以將有限的許可權授與給儲存體物件。 Contoso 會設定 SAS URI，讓 Azure 資料庫移轉服務可以存取服務將 SQL Server 備份檔案上傳至其中的儲存體帳戶容器。
> - **步驟3：準備 Azure 以進行 Azure 遷移：伺服器遷移工具。** Contoso 會將伺服器遷移工具新增至其 Azure 遷移專案。
> - **步驟4：準備內部部署 VMware 以進行 Azure 遷移：伺服器遷移。** Contoso 會準備帳戶以進行 VM 探索，並在遷移後準備連接至 Azure Vm。
> - **步驟5：複寫內部部署 Vm。** Contoso 會設定複寫，並開始將 Vm 複寫至 Azure 儲存體。
> - **步驟6：透過 Azure 資料庫移轉服務遷移資料庫。** Contoso 遷移資料庫。
> - **步驟7：使用 Azure 遷移來遷移 Vm：伺服器遷移。** Contoso 會執行測試遷移，以確定一切都正常運作，然後執行完整遷移以將 VM 移至 Azure。

## <a name="step-1-prepare-a-sql-managed-instance"></a>步驟1：準備 SQL 受控實例

若要設定 SQL 受控實例，Contoso 需要符合下列需求的子網：

- 此子網路必須是專用的。 它必須是空的。 它不能包含任何其他雲端服務。 子網路不能是閘道子網路。
- 在建立受控實例之後，Contoso 不應將資源新增至子網。
- 子網路不能有與其相關聯的網路安全性群組。
- 子網路必須有使用者定義的路由表。 唯一指派的路由應該是 `0.0.0.0/0` 下一個躍點網際網路。
- 如果為虛擬網路指定了選擇性的自訂 DNS，則 `168.63.129.16` 必須將 Azure 中遞迴解析程式的虛擬 IP 位址新增至清單。 瞭解如何 [設定 SQL 受控實例的自訂 DNS](/azure/azure-sql/managed-instance/custom-dns-configure)。
- 子網路不得有相關聯的服務端點 (儲存體或 SQL)。 虛擬網路上應該停用服務端點。
- 子網路必須至少具有 16 個 IP 位址。 瞭解如何 [調整受控實例子網的大小](/azure/azure-sql/managed-instance/vnet-existing-add-subnet)。
- 在 Contoso 的混合式環境中，需要有自訂 DNS 設定。 Contoso 會將 DNS 設定配置為使用公司的其中一或多部 Azure DNS 伺服器。 深入瞭解 [DNS 自訂](/azure/azure-sql/managed-instance/custom-dns-configure)。

### <a name="set-up-a-virtual-network-for-the-managed-instance"></a>設定受控執行個體的虛擬網路

若要設定虛擬網路，Contoso 管理員：

1. 在主要區域中建立新的虛擬網路 (`VNET-SQLMI-EU2`)  (`East US 2`) 。 它會將虛擬網路新增至 `ContosoNetworkingRG` 資源群組。
1. 指派的位址空間 `10.235.0.0/24` 。 他們會確保範圍不會與其企業中的任何其他網路重疊。
1. 將兩個子網新增至網路：
    - `SQLMI-DS-EUS2` (`10.235.0.0/25`).
    - `SQLMI-SAW-EUS2` (`10.235.0.128/29`). 此子網會用來將目錄連結到受控實例。

      ![顯示 [SQL 受控實例：建立虛擬網路] 窗格的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-vnet.png)

1. 部署虛擬網路和子網路之後，他們會將網路對等互連，如下所示：

    - `VNET-SQLMI-EUS2` `VNET-HUB-EUS2` 在) 中 (中樞虛擬網路的對等 `East US 2` 。
    - `VNET-SQLMI-EUS2` `VNET-PROD-EUS2` (生產網路) 的對等。

      ![顯示網路對等互連的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-peering.png)

1. 設定自訂 DNS 設定。 DNS 會先指向 Contoso 的 Azure 網域控制站。 而後指向 Azure DNS。 Contoso Azure 網域控制站的位置如下所示：

    - 位於 `PROD-DC-EUS2` 生產網路中的子網中， `East US 2` (`VNET-PROD-EUS2`) ]。
    - `CONTOSODC3` 位址： `10.245.42.4` 。
    - `CONTOSODC4` 位址： `10.245.42.5` 。
    - Azure DNS 解析程式： `168.63.129.16` 。

      ![顯示網路 DNS 伺服器的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-dns.png)

**需要其他協助？**

- 閱讀 [SQL 受控實例總覽](/azure/azure-sql/managed-instance/sql-managed-instance-paas-overview)。
- 瞭解如何 [建立 SQL 受控實例的虛擬網路](/azure/azure-sql/managed-instance/vnet-existing-add-subnet)。
- 了解如何[設定對等互連](/azure/virtual-network/virtual-network-manage-peering)。
- 了解如何[更新 Azure Active Directory DNS 設定](/azure/active-directory-domain-services/tutorial-create-instance)。

### <a name="set-up-routing"></a>設定路由

受控實例會置於私人虛擬網路中。 Contoso 需要路由表，虛擬網路才能與 Azure 管理服務通訊。 如果虛擬網路無法與管理它的服務通訊，則會變成無法存取。

Contoso 會考量下列因素：

- 路由表包含一組規則 (路由) ，以指定從受控實例傳送的封包應如何在虛擬網路中路由傳送。
- 路由表會與部署受控實例的子網相關聯。 每個離開子網路的封包都會依據相關聯的路由表進行處理。
- 一個子網路只能與一個路由表相關聯。
- 在 Microsoft Azure 中建立路由表，沒有任何額外的費用。

 若要設定路由，Contoso 管理員可以執行下列步驟：

1. 在資源群組中建立使用者定義的路由表 `ContosoNetworkingRG` 。

    ![顯示路由表的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table.png)

1. 為了符合 SQL 受控實例需求，在部署路由表 (`MIRouteTable`) 之後，他們會新增位址前置詞為的路由 `0.0.0.0/0` 。 [下一個躍點類型] 選項會設定為 [網際網路]。

    ![顯示路由表前置詞的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-prefix.png)

1. 將路由表與 `SQLMI-DB-EUS2` 網路) 中的子網 (建立關聯 `VNET-SQLMI-EUS2` 。

    ![顯示路由表子網的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-subnet.png)

**需要其他協助？**

瞭解如何 [設定受控實例的路由](/azure/azure-sql/managed-instance/instance-create-quickstart)。

### <a name="create-a-managed-instance"></a>建立受控執行個體

Contoso 管理員現在可以布建 SQL 受控實例：

1. 因為受控實例會為商務應用程式提供服務，所以會在公司的主要區域中部署受控實例 (`East US 2`) 。 他們會將受控實例新增至 `ContosoRG` 資源群組。
1. 他們會選取執行個體的定價層、大小計算和儲存體。 深入瞭解 [SQL 受控實例定價](https://azure.microsoft.com/pricing/details/azure-sql/sql-managed-instance/single/)。

    ![顯示 [SQL 受控實例] 窗格的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-create.png)

1. 部署受控實例之後，資源群組中會出現兩個新的資源 `ContosoRG` ：

    - SQL 受控實例。
    - 如果 Contoso 有多個受控實例，則為虛擬叢集。

      ![顯示兩個新資源的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-resources.png)

**需要其他協助？**

瞭解如何布建 [受控實例](/azure/azure-sql/managed-instance/instance-create-quickstart)。

## <a name="step-2-prepare-azure-database-migration-service"></a>步驟2：準備 Azure 資料庫移轉服務

若要準備 Azure 資料庫移轉服務，Contoso 管理員需要執行幾項作業：

- 在 Azure 中註冊資料庫移轉服務提供者。
- 授與資料庫移轉服務存取 Azure 儲存體的許可權，以上傳用來遷移資料庫的備份檔案。 若要提供 Azure 儲存體的存取權，請建立 Azure Blob 儲存體容器。 產生 Blob 儲存體容器的 SAS URI。
- 建立 Azure 資料庫移轉服務專案。

他們會完成下列步驟：

1. 在其訂用帳戶下註冊資料庫移轉提供者。 ![顯示資料庫移轉服務註冊的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-subscription.png)

1. 建立 Azure Blob 儲存體容器。 Contoso 會產生 SAS URI，讓 Azure 資料庫移轉服務可以存取它。

    ![顯示產生 SAS URI 的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-sas.png)

1. 建立 Azure 資料庫移轉服務執行個體。

    ![顯示建立實例的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-instance.png)

1. 將資料庫移轉服務實例放在 `PROD-DC-EUS2` 虛擬網路的子網中 `VNET-PROD-DC-EUS2` 。
    - 因為服務必須在可透過 VPN 閘道存取內部部署 SQL Server VM 的虛擬網路中，所以會將該實例放在此處。
    - `VNET-PROD-EUS2` 對等互連至 `VNET-HUB-EUS2` ，而且允許使用遠端閘道。 [ **使用遠端閘道** ] 選項可確保實例可以視需要進行通訊。

        ![顯示設定網路的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-network.png)

**需要其他協助？**

- 瞭解如何 [設定 Azure 資料庫移轉服務](/azure/dms/quickstart-create-data-migration-service-portal)。
- 了解如何[建立和使用 SAS](/azure/storage/common/storage-sas-overview)。

## <a name="step-3-prepare-azure-for-the-azure-migrate-server-migration-tool"></a>步驟3：準備 Azure 以進行 Azure 遷移：伺服器遷移工具

以下是 Contoso 將 VM 移轉至 Azure 時，所需的 Azure 元件：

- 當 Azure Vm 在遷移期間建立時，將位於其中的虛擬網路。
- Azure 遷移：伺服器遷移工具已布建。

Contoso 管理員會設定這些元件：

1. 設定網路。 Contoso 已設定可用於 Azure 遷移的網路： [部署 azure 基礎結構](./contoso-migration-infrastructure.md)時的伺服器遷移。

    - SmartHotel360 應用程式是生產應用程式，而且 Vm 將會遷移到主要區域中的 Azure 生產網路 (`VNET-PROD-EUS2`)  (`East US 2`) 。
    - 這兩個 Vm 都會放置在 `ContosoRG` 用於生產資源的資源群組中。
    - 應用程式前端 VM (`WEBVM`) 將會遷移至前端子網 (`PROD-FE-EUS2` 生產網路) 。
    - 應用程式資料庫 VM (`SQLVM`) 將會遷移至 (`PROD-DB-EUS2` 生產網路) 的資料庫子網。

## <a name="step-4-prepare-on-premises-vmware-for-azure-migrate-server-migration"></a>步驟4：為 Azure 遷移準備內部部署 VMware：伺服器遷移

以下是 Contoso 將 VM 移轉至 Azure 時，所需的 Azure 元件：

- 當 Azure Vm 在遷移期間建立時，將位於其中的虛擬網路。
- 已布建和設定的 Azure 遷移設備。

Contoso 管理員會依照下列步驟來設定這些元件：

1. 設定網路。 Contoso 已設定可用於 Azure 遷移的網路： [部署 azure 基礎結構](./contoso-migration-infrastructure.md)時的伺服器遷移。

    - SmartHotel360 應用程式是生產應用程式，而且 Vm 將會遷移到主要區域中的 Azure 生產網路 (`VNET-PROD-EUS2`)  (`East US 2`) 。
    - 這兩個 Vm 都會放置在 `ContosoRG` 用於生產資源的資源群組中。
    - 應用程式前端 VM (`WEBVM`) 將會遷移至生產網路中的前端子網 (`PROD-FE-EUS2`) 。
    - 應用程式資料庫 VM (`SQLVM`) 將會遷移至生產網路中的資料庫子網 (`PROD-DB-EUS2`) 。

1. 布建 Azure 遷移設備。

    1. 從 Azure 遷移下載 OVA 映射，並將其匯入 VMware。

        ![顯示下載 OVA 檔案的螢幕擷取畫面。](./media/contoso-migration-rehost-vm/migration-download-ova.png)

    1. 依照下列步驟啟動匯入的映射並設定工具：

       1. 設定必要條件。

          ![顯示必要條件設定的螢幕擷取畫面。](./media/contoso-migration-rehost-vm/migration-setup-prerequisites.png)

       1. 將工具指向 Azure 訂用帳戶。

          ![顯示選取訂用帳戶的螢幕擷取畫面](./media/contoso-migration-rehost-vm/migration-register-azure.png)

       1. 設定 VMware vCenter 認證。

          ![顯示設定 VMware vCenter 認證的螢幕擷取畫面。](./media/contoso-migration-rehost-vm/migration-vcenter-server.png)

       1. 新增任何以 Linux 或 Windows 為基礎的認證來進行探索。

          ![顯示設定 Linux 和 Windows 認證的螢幕擷取畫面。](./media/contoso-migration-rehost-vm/migration-credentials.png)

1. 設定完成之後，工具會花一些時間來列舉所有的虛擬機器。 程式完成之後，Contoso 管理員可以看到 Azure 中的 Azure 遷移工具填入的 Vm。

**需要其他協助？**

瞭解如何設定 [Azure 遷移設備](/azure/migrate/migrate-services-overview#azure-migrate-server-migration-tool)。

### <a name="prepare-on-premises-vms"></a>準備內部部署 Vm

在遷移之後，Contoso 想要連線至 Azure Vm，並允許 Azure 管理 Vm。 在遷移之前，Contoso 管理員必須執行下列步驟：

1. 為了透過網際網路存取，他們會：

    - 在遷移之前，先在內部部署 VM 上啟用 RDP 或 SSH。
    - 確定已為 **公用** 設定檔新增 TCP 和 UDP 規則。
    - 檢查作業系統防火牆中是否允許 RDP 或 SSH。

1. 若要透過站對站 VPN 存取，他們：

    - 在遷移之前，先在內部部署 VM 上啟用 RDP 或 SSH。
    - 檢查作業系統防火牆中是否允許 RDP 或 SSH。
    - 若為 Windows，請將內部部署 VM 上的作業系統 SAN 原則設定為 **OnlineAll**。

1. 他們會安裝 Azure 代理程式：

    - [Azure Linux 代理程式](/azure/virtual-machines/extensions/agent-linux)
    - [Azure Windows 代理程式](/azure/virtual-machines/extensions/agent-windows)

1. 其他考量：

   - 若是 Windows，在觸發遷移時，VM 上不應該有暫止的 Windows 更新。 如果有，在更新完成之前，他們將無法登入 VM。
   - 遷移後，他們可以檢查 **開機診斷** 以查看 VM 的螢幕擷取畫面。 如果這不可行，則應確認 VM 是否正在執行，並檢查這些 [疑難排解秘訣](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)。

**需要其他協助？**

瞭解如何 [準備 vm 以進行遷移](/azure/migrate/prepare-for-migration)。

## <a name="step-5-replicate-the-on-premises-vms"></a>步驟5：複寫內部部署 Vm

在 Contoso 管理員可以執行遷移至 Azure 之前，他們必須先設定並啟用複寫。

完成探索之後，他們就可以開始將 VMware Vm 複寫至 Azure。

1. 在 azure 遷移專案中，它們會移至 azure 遷移的 **伺服器**  >  **：伺服器遷移**。 然後 **選取 [** 複寫]。

    ![顯示 [複寫] 選項的螢幕擷取畫面。](./media/contoso-migration-rehost-vm/select-replicate.png)

1. 在 **[** 複寫  >  **來源設定**] 中，您的  >  **電腦虛擬化了嗎？** 他們會選取 **[是，使用 VMware vSphere]**。

1. 在 **內部部署設備** 中，他們會選取已設定的 Azure 遷移設備名稱，然後選取 **[確定]**。

    ![顯示 [來源設定] 索引標籤的螢幕擷取畫面。](./media/contoso-migration-rehost-vm/source-settings.png)

1. 在 **虛擬機器** 中，他們會選取要複寫的機器：
    - 如果他們已執行 Vm 的評量，則可以將 VM 大小調整和磁片類型套用至評定結果 (premium/standard) 建議。 在 [ **從 Azure 遷移評估匯入遷移設定？**] 中，他們會選取 [ **是]** 選項。
    - 如果他們未執行評量，或他們不想使用評量設定，則會選取 [ **否** ] 選項。
    - 如果他們選擇使用評量，他們會選取 VM 群組和評量名稱。

    ![顯示選取評定的螢幕擷取畫面。](./media/contoso-migration-rehost-vm/select-assessment.png)

1. 在 **虛擬機器** 中，他們會視需要搜尋 vm，並檢查每個要遷移的 vm。 然後選取 **[下一步：目標設定]**。

1. 在 [ **目標設定**] 中，他們會選取要遷移至的訂用帳戶和目的地區域。 它們也會指定 Azure Vm 在遷移後所在的資源群組。 在 **虛擬網路** 中，他們會選取 azure vm 在遷移後將加入的 azure 虛擬網路/子網。

1. 在 **Azure 混合式權益** 中，他們：

    - 如果他們不想要套用 Azure 混合式權益，請選取 [ **否** ]。 然後選取 **[下一步]**。
    - 如果他們的 Windows Server 電腦是使用中的軟體保證或 Windows Server 訂用帳戶，而且想要將權益套用至其所要遷移的機器，請選取 **[是]** 。 然後選取 **[下一步]**。

1. 在 **計算** 中，他們會檢查 VM 名稱、大小、OS 磁片類型和可用性設定組。 VM 必須符合 [Azure 需求](/azure/migrate/migrate-support-matrix-vmware#vmware-requirements)。

    - **VM 大小：** 如果他們使用評量建議，則 [VM 大小] 下拉式清單會包含建議的大小。 否則，Azure 遷移會根據 Azure 訂用帳戶中最接近的相符項來挑選大小。 或者，他們可以選擇 **AZURE VM 大小** 的手動大小。
    - **作業系統磁片：** 他們會為 VM 指定作業系統 (開機) 磁片。 OS 磁碟是具有作業系統開機載入器和安裝程式的磁碟。
    - **可用性設定組：** 如果 VM 在遷移後應位於 Azure 可用性設定組中，則會指定該集合。 此集合必須在為遷移所指定的目標資源群組中。

1. 在 **磁片** 中，它們會指定是否應將 VM 磁片複寫至 Azure。 然後，他們會在 Azure 中選取 (標準 SSD/HDD 或 premium 受控磁片) 的磁片類型，然後選取 **[下一步]**。
    - 它們可以將磁片從複寫中排除。
    - 如果將磁片排除，則在遷移後將不會出現在 Azure VM 上。

1. 在 [ **檢查 + 開始** 複寫] 中，他們會檢查設定。 然後 **，他們會選取 [** 複寫] 以啟動伺服器的初始複寫。

> [!NOTE]
> 複寫設定可以在複寫開始之前的任何時間更新，以 **管理** 複寫  >  **機器**。 在複寫啟動後，就無法變更設定。

## <a name="step-6-migrate-the-database-via-azure-database-migration-service"></a>步驟6：透過 Azure 資料庫移轉服務遷移資料庫

Contoso 管理員必須建立資料庫移轉服務專案，然後遷移資料庫。

### <a name="create-an-azure-database-migration-service-project"></a>建立 Azure 資料庫移轉服務專案

1. 系統管理員會建立資料庫移轉服務專案。 他們會選取 **SQL Server** 來源伺服器類型和 **Azure sql 受控實例** 作為目標。

     ![顯示 [新增遷移專案] 窗格的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-project.png)

1. 移轉精靈隨即開啟。

### <a name="migrate-the-database"></a>遷移資料庫

1. 在移轉精靈中，他們會指定內部部署資料庫所在的來源 VM。 他們會提供用於存取資料庫的認證。

    ![顯示 [來源詳細資料] 窗格的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-source.png)

1. 他們會選取要遷移 () 的資料庫 `SmartHotel.Registration` 。

    ![顯示 [選取源資料庫] 窗格的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-source-db.png)

1. 針對目標，他們會在 Azure 中輸入受控實例的名稱，以及存取認證。

    ![顯示 [目標詳細資料] 窗格的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-target-details.png)

1. 在 **新的活動**  >  **執行遷移** 中，他們會指定執行遷移的設定：
    - 來源和目標認證。
    - 要遷移的資料庫。
    - 在內部部署 VM 上建立的網路共用。 Azure 資料庫移轉服務會取得此共用的來源備份。
        - 執行來源 SQL Server 執行個體的服務帳戶必須擁有此共用的寫入權限。
        - 必須使用共用的 FQDN 路徑。
    - SAS URI，可讓 Azure 資料庫移轉服務存取儲存體帳戶容器，服務會將備份檔案上傳至該容器以進行遷移。

        ![顯示 [設定遷移設定] 畫面的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-migration-settings.png)

1. 他們會儲存遷移設定，然後執行遷移。
1. 在 [概觀] 中，他們會監視移轉狀態。

    ![顯示狀態的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor1.png)

1. 當遷移完成時，它們會確認受控實例上有目標資料庫。

    ![顯示驗證資料庫移轉的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor2.png)

## <a name="step-7-migrate-the-vms-with-azure-migrate-server-migration"></a>步驟7：使用 Azure 遷移來遷移 Vm：伺服器遷移

Contoso 管理員會執行快速測試遷移，並確認 VM 是否正常運作。 然後，他們會遷移 VM。

### <a name="run-a-test-migration"></a>執行測試移轉

1. 在 **遷移目標**  >  **伺服器** 中  >  ，**Azure 遷移：伺服器遷移**，他們選取 [**測試遷移的伺服器**]。

     ![顯示 [測試遷移的伺服器] 專案的螢幕擷取畫面。](./media/contoso-migration-rehost-linux-vm/test-migrated-servers.png)

1. 他們會選取並保存 (或以滑鼠右鍵按一下) 要測試的 VM，然後選取 [ **測試遷移**]。

    ![顯示測試遷移專案的螢幕擷取畫面。](./media/contoso-migration-rehost-linux-vm/test-migrate.png)

1. 在 **測試遷移** 中，他們會選取 azure 虛擬網路，azure VM 在遷移後將位於此網路中。 我們建議使用非生產虛擬網路。
1. **測試移轉** 作業隨即啟動。 他們會在入口網站通知中監視作業。
1. 在遷移完成後，他們會在 Azure 入口網站中的 **虛擬機器** 中，查看已遷移的 Azure VM。 機器名稱會具有尾碼 **-Test**。
1. 測試完成後，他們會選取並保存 (或以滑鼠右鍵按一下) 複寫 **機器** 中的 Azure VM，然後選取 [ **清除測試遷移**]。

    ![顯示清除測試遷移專案的螢幕擷取畫面。](./media/contoso-migration-rehost-linux-vm/clean-up.png)

### <a name="migrate-the-vm"></a>移轉 VM

Contoso 管理員現在會執行完整遷移來完成移動。

1. 在 azure 遷移專案中，他們會移至 azure 遷移的 **伺服器**  >  **：伺服器遷移**，然後選取複寫 **伺服器**。

    ![顯示 [複寫伺服器] 專案的螢幕擷取畫面。](./media/contoso-migration-rehost-linux-vm/replicating-servers.png)

1. 在複寫 **機器** 時，他們會選取並保存 (或以滑鼠右鍵按一下) VM，然後選取 [ **遷移**]。
1. 在  >  [遷移] 會 **關閉虛擬機器，並在不遺失資料** 的情況下執行規劃的遷移，然後選取 [**是**  >  **]**。
    - 根據預設，Azure 遷移會關閉內部部署 VM，並執行隨選複寫來同步處理自從上次複寫之後發生的任何 VM 變更。 此動作可確保不會遺失任何資料。
    - 如果他們不想關閉 VM，則會選取 [ **否**]。
1. VM 會啟動移轉作業。 他們會在 Azure 通知中追蹤作業。
1. 作業完成後，他們可以從 [ **虛擬機器** ] 頁面查看和管理 VM。
1. 最後，他們會更新 `WEBVM` 其中一個 Contoso 網域控制站上的 DNS 記錄。

### <a name="update-the-connection-string"></a>更新連接字串

在遷移程式的最後一個步驟中，Contoso 管理員會將應用程式的連接字串更新為指向在 SQL 受控實例上執行的遷移後資料庫。

1. 在 Azure 入口網站中，他們會選取 [**設定**  >  **連接字串**] 來尋找連接字串。

    ![顯示 [連接字串] 選項的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-managed-instance/failover4.png)

1. 他們使用 SQL 受控實例的使用者名稱和密碼來更新字串。
1. 設定字串之後，會取代其應用程式的檔案中目前的連接字串 `web.config` 。
1. 在更新檔案並加以儲存後，他們會 `WEBVM` `iisreset /restart` 在命令提示字元視窗中執行，以重新開機 IIS。
1. 重新開機 IIS 之後，應用程式會使用在 SQL 受控實例上執行的資料庫。
1. 此時，他們可以關閉內部部署 `SQLVM` 機器。 遷移已完成。

**需要其他協助？**

- 瞭解如何 [執行測試容錯移轉](/azure/site-recovery/tutorial-dr-drill-azure)。
- 瞭解如何 [建立復原方案](/azure/site-recovery/site-recovery-create-recovery-plans)。
- 了解如何[容錯移轉至 Azure](/azure/site-recovery/site-recovery-failover)。

## <a name="clean-up-after-migration"></a>移轉之後進行清除

完成遷移之後，SmartHotel360 應用程式會在 Azure VM 上執行，而 SmartHotel360 資料庫則可在 Azure SQL 受控實例中使用。

現在，Contoso 必須執行下列清除工作：

- `WEBVM`從 VCenter Server 清查中移除機器。
- `SQLVM`從 VCenter Server 清查中移除機器。
- `WEBVM` `SQLVM` 從本機備份作業中移除和。
- 更新內部檔以顯示的新位置和 IP 位址 `WEBVM` 。
- `SQLVM`從內部檔中移除。 此外，Contoso 也可以修訂檔，以顯示 `SQLVM` 為已刪除，而不再存在於 VM 清查中。
- 檢閱與已解除委任 VM 互動的任何資源。 更新任何相關的設定或文件，以反映新的組態。

## <a name="review-the-deployment"></a>檢閱部署

對於 Azure 中的遷移後資源，Contoso 必須能執行一切功能並保護其新的基礎結構。

### <a name="security"></a>安全性

Contoso 安全性小組會檢查 Azure Vm 和 SQL 受控實例中是否有任何安全性問題：

- 小組會檢閱用來控制 VM 存取權的網路安全性群組。 網路安全性群組可協助確保只有應用程式允許的流量可以通過。
- Contoso 安全性小組也會考慮使用 Azure 磁碟加密和 Azure KeyVault 來保護磁碟上的資料。
- 小組會在受控實例上啟用威脅偵測。 如果偵測到威脅，威脅偵測會將警示傳送至 Contoso 的安全性小組/服務台系統。 深入瞭解 [SQL 受控實例的威脅偵測](/azure/azure-sql/managed-instance/threat-detection-configure)。

     ![顯示 [SQL 受控實例安全性：威脅偵測] 畫面的螢幕擷取畫面。](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-security.png)

若要深入了解 VM 的安全性做法，請參閱 [Azure 中 IaaS 工作負載的安全性最佳做法](/azure/security/fundamentals/iaas)。

### <a name="business-continuity-and-disaster-recovery"></a>商務持續性和災害復原

針對商務持續性和災害復原，Contoso 會採取下列動作：

- **保持資料安全。** Contoso 會使用 Azure 備份服務來備份 Vm 上的資料。 如需詳細資訊，請參閱 [AZURE VM 備份的總覽](/azure/backup/backup-azure-vms-introduction)。
- **讓應用程式保持正常運作。** Contoso 會使用 Site Recovery，將 Azure 中的應用程式 Vm 複寫至次要區域。 若要深入瞭解，請參閱 [設定 AZURE VM 的損毀修復至次要 Azure 區域](/azure/site-recovery/azure-to-azure-quickstart)。
- **深入了解。** Contoso 會深入瞭解如何管理 SQL 受控實例，其中包括 [資料庫備份](/azure/azure-sql/database/automated-backups-overview)。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- Contoso 具有的現有授權 `WEBVM` 。 為了利用 Azure Hybrid Benefit 的定價，Contoso 會轉換現有的 Azure VM。
- Contoso 會使用 [Azure 成本管理 + 計費](/azure/cost-management-billing/cost-management-billing-overview) 來確保公司維持在 IT 領導階層所建立的預算內。

## <a name="conclusion"></a>結論

在本文中，Contoso 會在 Azure 中如何 SmartHotel360 應用程式，方法是使用 Azure 遷移將應用程式前端 VM 遷移至 Azure。 Contoso 會使用 Azure 資料庫移轉服務，將內部部署資料庫移轉至 SQL 受控實例。
