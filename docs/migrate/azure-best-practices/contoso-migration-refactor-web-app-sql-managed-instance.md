---
title: 藉由將應用程式遷移至 Azure App Service 並 Azure SQL Database 受控執行個體，加以重構
description: 瞭解 Contoso 如何重新裝載內部部署應用程式，方法是將它遷移至 Azure App Service web 應用程式並 Azure SQL Database 受控執行個體。
author: givenscj
ms.author: abuck
ms.date: 04/02/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: azure-migrate
ms.openlocfilehash: fd8c7e81d457ee3a63186217fda72223272ad3b7
ms.sourcegitcommit: 070e6a60f05519705828fcc9c5770c3f9f986de5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83815254"
---
<!-- cSpell:ignore givenscj WEBVM SQLVM contosohost vcenter contosodc smarthotel SQLMI SHWCF SHWEB -->

# <a name="refactor-an-on-premises-app-to-an-azure-app-service-web-app-and-azure-sql-managed-instance"></a>將內部部署應用程式重構至 Azure App Service web 應用程式和 Azure SQL 受控執行個體

本文示範虛構公司 Contoso 如何在移轉至 Azure 的過程中，重構在 VMware VM 上執行的兩層式 Windows .NET 應用程式。 他們會將應用程式前端 VM 遷移至 Azure App Service web 應用程式。 此外也說明 Contoso 如何將應用程式資料庫移轉至 Azure SQL Database 受控執行個體。

此範例中使用的 SmartHotel360 應用程式以開放原始碼的形式提供。 如果想將它用於自己的測試目的，您可以從 [github](https://github.com/Microsoft/SmartHotel360) 進行下載。

## <a name="business-drivers"></a>商業動機

IT 領導小組與商務合作夥伴密切合作，以了解此次移轉所要實現的目標：

- **解決業務成長。** Contoso 的業務量日益增多，對內部部署系統和基礎結構造成了壓力。
- **提高效率。** Contoso 必須移除不必要的程序，並且簡化開發人員和使用者的程序。 企業亟需快速且不浪費時間或金錢的 IT 服務，進而更快滿足客戶的需求。
- **增加靈活性。**  Contoso IT 必須能夠更快因應企業的需求。 其因應速度必須能夠比市場變化更快，才能更在全球經濟中獲致成功。 它不得礙事，或成為企業的絆腳石。
- **尺度.** 隨著企業順利成長，Contoso IT 必須提供能夠同步成長的系統。
- **降低成本。** Contoso 想要將授權費用降至最低。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已針對此次移轉擬定好各項目標。 並用這些目標用來判斷最合適的移轉方法。

<!-- markdownlint-disable MD033 -->

**需求** | **詳細資料**
--- | ---
**應用程式** | Azure 中的應用程式應與現在一樣重要。 <br><br> 其效能應與目前在 VMware 中的效能相同。 <br><br> 該小組不想在這個應用程式上投注資源。 目前，管理員只會將應用程式安全地移至雲端。 <br><br> 該小組想要停止支援目前執行應用程式的 Windows Server 2008 R2。 <br><br> 該小組想要從 SQL Server 2008 R2 移至新型 PaaS 資料庫平台，這樣可減少管理需求。 <br><br> Contoso 想要盡可能運用其在 SQL Server 授權及軟體保證中所做的投資。 <br><br> 此外，Contoso 想要緩解 Web 層上單一失敗點的情形。
**限制** | 應用程式由相同 VM 上執行的 ASP.NET 應用程式和 WCF 服務所組成。 他們想要將其分成兩個使用 Azure App Service 的 Web 應用程式。
**Azure** | Contoso 想要將應用程式移至 Azure，但不想在 VM 上加以執行。 Contoso 想要使用 Web 和資料層的 Azure PaaS 服務。
**DevOps** | Contoso 想要移轉至 DevOps 模型，使用 Azure DevOps 來處理其建置和發行管線。

<!-- markdownlint-enable MD033 -->

## <a name="solution-design"></a>解決方案設計

擬定好目標和需求後，Contoso 會設計和檢閱部署解決方案，並識別移轉程序，包括將用於移轉的 Azure 服務。

### <a name="current-app"></a>目前的應用程式

- SmartHotel360 內部部署應用程式會分層至兩個 VM (WEBVM 和 SQLVM)。
- Vm 位於 VMware ESXi 主機**contosohost1.contoso.com** （版本6.5）。
- VMware 環境是由在 VM 上執行的 vCenter Server 6.5 （**vcenter.contoso.com**）所管理。
- Contoso 有內部部署資料中心 (contoso-datacenter) 以及內部部署網域控制站 (**contosodc1**)。
- 移轉完成之後，將會解除委任 Contoso 資料中心的內部部署 VM。

### <a name="proposed-solution"></a>建議的解決方案

- 針對應用程式 Web 層，Contoso 決定使用 Azure App Service。 透過此 PaaS 服務，僅須變更一些組態即可部署應用程式。 Contoso 將使用 Visual Studio 來進行變更，並部署兩個 Web 應用程式。 一個用於網站，另一個用於 WCF 服務。
- 為了符合 DevOps 管線的需求，Contoso 已針對使用 Git 存放庫的原始碼管理選取了 Azure DevOps。 他們將使用自動化組建和發行來建置程式碼，並將其部署至 Azure App Service。

### <a name="database-considerations"></a>資料庫考量

在解決方案設計過程中，Contoso 會進行 Azure SQL Database 與 SQL Server 受控執行個體的功能比較。 下列考量協助他們決定使用受控執行個體。

- 受控執行個體的目標是為最新版內部部署 SQL Server 提供幾乎 100% 的相容性。 如果在內部部署環境或在 IaaS VM 上執行 SQL Server 的客戶想要在幾乎不需要變更設計的情況下，將其應用程式遷移至完全受控服務，Microsoft 建議使用受控執行個體。
- Contoso 計劃將大量應用程式從內部部署環境遷移到 IaaS。 有許多都是由 ISV 提供。 Contoso 意識到使用受控執行個體將有助於確保這些應用程式的資料庫相容性，而不是使用可能不支援的 SQL Database。
- Contoso 可以使用完全自動化的 Azure 資料庫移轉服務，輕鬆地將轉移轉移至受控執行個體。 備妥此服務，Contoso 可以將它重複使用於未來的資料庫移轉。
- SQL 受控執行個體支援 SQL Server Agent，也就是 SmartHotel360 應用程式的重要問題。 Contoso 需要此相容性，否則就必須重新設計應用程式所需的維護方案。
- Contoso 可以透過軟體保證使用適用於 SQL Server 的 Azure Hybrid Benefit，以折扣優惠在 SQL Database 受控執行個體上交換執行個體的現有授權。 這可讓 Contoso 最多節省 30% 的受控執行個體。
- SQL 受控執行個體完全包含在虛擬網路中，因此可為 Contoso 的資料提供更高的隔離和安全性。 Contoso 可享有公用雲端的優勢，同時讓環境與公用網際網路保持隔離。
- 受控執行個體支援許多安全性功能，包括永遠加密、動態資料遮罩、資料列層級的安全性，以及威脅偵測。

### <a name="solution-review"></a>解決方案檢閱

Contoso 會透過比較一份優缺點清單，來評估其建議設計。

<!-- markdownlint-disable MD033 -->

**考量** | **詳細資料**
--- | ---
**優點** | SmartHotel360 應用程式的程式碼無需變更即可移轉至 Azure。 <br><br> Contoso 可以使用 SQL Server 和 Windows Server 的 Azure Hybrid Benefit 來妥善運用軟體保證中的投資。 <br><br> 移轉之後，即無須支援 Windows Server 2008 R2。 如需詳細資訊，請參閱 [Microsoft 生命週期原則](https://aka.ms/lifecycle)。 <br><br> Contoso 可以使用多個執行個體來設定應用程式的 Web 層，使它不再是單一失敗點。 <br><br> 資料庫不會再依賴過時的 SQL Server 2008 R2。 <br><br> SQL 受控執行個體可援 Contoso 的技術需求和目標。 <br><br> 受控執行個體將提供與其目前部署的 100% 相容性，同時將其從 SQL Server 2008 R2 中移走。 <br><br> 他們可以使用 SQL Server 和 Windows Server 的 Azure Hybrid Benefit，來妥善運用軟體保證中的投資。 <br><br> 他們可以重複使用 Azure 資料庫移轉服務進行其他未來的移轉作業。 <br><br> SQL 受控執行個體具有 Contoso 不需設定的內建容錯功能。 這可確保資料層不再是單一的容錯移轉點。
**缺點** | Azure App Service 只支援對每個 Web 應用程式部署一個應用程式。 這表示必須佈建兩個 Web 應用程式 (一個用於網站，一個用於 WCF 服務)。 <br><br> 在資料層中，如果 Contoso 想要自訂作業系統或資料庫伺服器，或是想要執行第三方應用程式以及 SQL Server，受控執行個體可能不是最佳解決方案。 在 IaaS VM 上執行 SQL Server 可提供此種彈性。

<!-- markdownlint-enable MD033 -->

## <a name="proposed-architecture"></a>建議的架構

![案例架構](./media/contoso-migration-refactor-web-app-sql-managed-instance/architecture.png)

### <a name="migration-process"></a>移轉程序

1. Contoso 會使用 Azure 資料庫移轉服務（DMS）布建 Azure SQL Database 受控執行個體，並將 SmartHotel360 資料庫移轉至其中。
2. Contoso 會佈建並設定 Web Apps，然後對其部署 SmartHotel360 應用程式。

    ![移轉程序](./media/contoso-migration-refactor-web-app-sql-managed-instance/migration-process.png)

### <a name="azure-services"></a>Azure 服務

**服務** | **說明** | **成本**
--- | --- | ---
[Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) | Azure 資料庫移轉服務能夠從多個資料庫來源無縫移轉到 Azure 資料平台，將停機時間降到最低。 | 深入了解[支援的區域](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability)和[資料庫移轉服務定價](https://azure.microsoft.com/pricing/details/database-migration)。
[Azure SQL Database 受控執行個體](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) | 受控執行個體是一種受控資料庫服務，可代表 Azure 雲端中的完全受控 SQL Server 執行個體。 它會使用與最新版 SQL Server 資料庫引擎相同的程式碼，並擁有最新的功能、效能增強功能和安全性修補程式。 | 使用在 Azure 中執行的 SQL Database 受控執行個體會根據所用容量產生費用。 深入了解[受控執行個體定價](https://azure.microsoft.com/pricing/details/sql-database/managed)。
[Azure App Service](https://docs.microsoft.com/azure/app-service/overview) | 使用受完整管理的平台建立強大的雲端應用程式 | 根據大小、位置和使用期間計算費用。 [深入了解](https://azure.microsoft.com/pricing/details/app-service/windows)。
[Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines) | 為應用程式開發提供持續整合和持續部署 (CI/CD) 管線。 管線一開始會有一個用於管理應用程式程式碼的 Git 存放庫、一個用於產生套件及其他建置成品的建置系統，以及一個用來在開發、測試及生產環境中部署變更的「發行管理」系統。

## <a name="prerequisites"></a>Prerequisites

以下是 Contoso 在此情況下所須執行的動作：

<!-- markdownlint-disable MD033 -->

**需求** | **詳細資料**
--- | ---
**Azure 訂用帳戶** | Contoso 已在前文說明的步驟中建立訂用帳戶。 如果您沒有 Azure 訂用帳戶，請建立[免費帳戶](https://azure.microsoft.com/pricing/free-trial)。 <br><br> 如果您建立免費帳戶，您就是訂用帳戶的管理員，並可執行所有動作。 <br><br> 如果您使用現有訂用帳戶，而且您不是系統管理員，則需要與系統管理員合作，讓其指派擁有者或參與者權限給您。
**Azure 基礎結構** | [了解](./contoso-migration-infrastructure.md) Contoso 如何設定 Azure 基礎結構。

<!--markdownlint-enable MD033 -->

## <a name="scenario-steps"></a>案例步驟

以下是 Contoso 執行移轉的方式：

> [!div class="checklist"]
>
> - **步驟1：設定 SQL Database 受控執行個體。** Contoso 需要現有的受控執行個體，以作為內部部署 SQL Server 資料庫的移轉目的地。
> - **步驟2：使用 Azure 資料庫移轉服務（DMS）進行遷移。** Contoso 會使用 Azure 資料移轉服務來遷移應用程式資料庫。
> - **步驟3：布建 web 應用程式。** Contoso 會佈建兩個 Web 應用程式。
> - **步驟4：設定 Azure DevOps。** Contoso 會建立新的 Azure DevOps 專案，並匯入 Git 存放庫。
> - **步驟5：設定連接字串。** Contoso 會設定連接字串，以便 Web 層的 Web 應用程式、WCF 服務的 Web 應用程式和 SQL 執行個體進行通訊。
> - **步驟6：設定組建和發行管線。** 在最後一個步驟中，Contoso 會設定組建和發行管線來建立應用程式，並將其部署至兩個不同的 web 應用程式。

## <a name="step-1-set-up-a-sql-database-managed-instance"></a>步驟1：設定 SQL Database 受控執行個體

若要設定 Azure SQL Database 受控執行個體，Contoso 需要一個符合下列要求的子網路：

- 此子網路必須是專用的。 它必須空白，不得包含任何其他雲端服務。 子網路不能是閘道子網路。
- 建立受控執行個體後，Contoso 就不得在該子網路中新增資源。
- 子網路不能有與其相關聯的網路安全性群組。
- 子網路必須有使用者定義的路由表。 唯一指派的路由應該是 0.0.0.0/0 下一個躍點網際網路。
- 如果為虛擬網路指定了選擇性的自訂 DNS，則 `168.63.129.16` 必須將 Azure 中遞迴解析程式的虛擬 IP 位址新增到清單中。 瞭解如何[設定 Azure SQL Database 受控執行個體的自訂 DNS](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns)。
- 子網路不得有相關聯的服務端點 (儲存體或 SQL)。 虛擬網路上應該停用服務端點。
- 子網路必須至少具有 16 個 IP 位址。 了解如何[調整受控執行個體子網路的大小](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vnet-subnet)。
- 在 Contoso 的混合式環境中，需要有自訂 DNS 設定。 Contoso 會將 DNS 設定配置為使用公司的其中一或多部 Azure DNS 伺服器。 深入瞭解[DNS 自訂](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns)。

### <a name="set-up-a-virtual-network-for-the-managed-instance"></a>設定受控執行個體的虛擬網路

Contoso 管理員會設定虛擬網路，如下所示：

1. 他們會在主要美國東部 2 區域中建立新的虛擬網路 (**VNET-SQLMI-EU2**)。 它會將此虛擬網路新增至 **ContosoNetworkingRG** 資源群組。
2. 他們會指派 10.235.0.0/24 位址空間。 他們會確保範圍不會與其企業中的任何其他網路重疊。
3. 他們會將兩個子網路新增到網路：
    - **互連 vnet-sqlmi-eus2-DS-EUS2** （10.235.0.0.25）。
    - **互連 vnet-sqlmi-eus2-EUS2** （10.235.0.128/29）。 此子網路用來將目錄連結到受控執行個體。

      ![受控執行個體：建立虛擬網路](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-vnet.png)

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

- 取得 [SQL Database 受控執行個體](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance)的概觀。
- 了解如何[建立 SQL Database 受控執行個體的虛擬網路](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vnet-subnet)。
- 了解如何[設定對等互連](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering)。
- 了解如何[更新 Azure Active Directory DNS 設定](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-instance)。

### <a name="set-up-routing"></a>設定路由

受控實例會放置在私人虛擬網路中。 Contoso 需要路由表，以供虛擬網路與 Azure 管理服務通訊。 如果虛擬網路無法與管理它的服務通訊，則會變成無法存取。

Contoso 會考量下列因素：

- 路由表包含一組規則 (路由)，可指定從受控執行個體傳送的封包應如何在虛擬網路中路由傳送。
- 路由表與受控實例部署所在的子網相關聯。 每個離開子網路的封包都會依據相關聯的路由表進行處理。
- 一個子網路只能與一個路由表相關聯。
- 在 Microsoft Azure 中建立路由表，沒有任何額外的費用。

 若要設定路由，Contoso 管理員可執行下列動作：

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

## <a name="step-2-migrate-with-azure-database-migration-service-dms"></a>步驟2：使用 Azure 資料庫移轉服務（DMS）進行遷移

Contoso 管理員會使用 Azure 資料庫移轉服務（DMS），透過[逐步遷移教學課程來](https://docs.microsoft.com/azure/dms/tutorial-sql-server-azure-sql-online)遷移 it。 他們可以執行線上、離線和混合式（預覽）遷移。

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

## <a name="step-3-provision-web-apps"></a>步驟3：布建 web 應用程式

完成資料庫移轉之後，Contoso 管理員現在即可佈建這兩個 Web 應用程式。

1. 在入口網站中選取 [Web 應用程式]****。

    ![Web 應用程式](./media/contoso-migration-refactor-web-app-sql-managed-instance/web-app1.png)

2. 他們會提供應用程式名稱 (**SHWEB-EUS2**)、在 Windows 上執行，並將它放在生產資源群組 (**ContosoRG**)。 他們會建立新的 Web 應用程式與 Azure App Service 方案。

    ![Web 應用程式](./media/contoso-migration-refactor-web-app-sql-managed-instance/web-app2.png)

3. 佈建好 Web 應用程式之後，他們會重複此程序以建立 WCF 服務的 Web 應用程式 (**SHWCF-EUS2**)

    ![Web 應用程式](./media/contoso-migration-refactor-web-app-sql-managed-instance/web-app3.png)

4. 完成之後，他們會瀏覽應用程式的位址，以檢查應用程式是否已成功建立。

## <a name="step-4-set-up-azure-devops"></a>步驟 4：設定 Azure DevOps

Contoso 需要為應用程式建置 DevOps 基礎結構和管線。 為此，Contoso 管理員會建立新的 DevOps 專案、匯入程式碼，然後設定建置和發行管線。

1. 在 Contoso Azure DevOps 帳戶中，他們會建立一個新專案 (**ContosoSmartHotelRefactor**)，然後選取 [Git]**** 來進行版本控制。

    ![新增專案](./media/contoso-migration-refactor-web-app-sql-managed-instance/vsts1.png)

2. 其會匯入目前保有其應用程式程式碼的 Git 存放庫。 它位於[公用 GitHub 存放庫](https://github.com/Microsoft/SmartHotel360-Registration)中，您可以下載它。

    ![下載應用程式程式碼](./media/contoso-migration-refactor-web-app-sql-managed-instance/vsts2.png)

3. 其會在匯入程式碼後，將 Visual Studio 連線至存放庫，並使用 Team Explorer 複製程式碼。

    ![連線至專案](./media/contoso-migration-refactor-web-app-sql-managed-instance/devops1.png)

4. 將存放庫複製到開發人員機器後，他們開啟應用程式的解決方案檔案。 Web 應用程式和 wcf 服務各在檔案中擁有不同的專案。

    ![解決方案檔案](./media/contoso-migration-refactor-web-app-sql-managed-instance/vsts4.png)

## <a name="step-5-configure-connection-strings"></a>步驟 5：設定連接字串

Contoso 管理員必須確定 Web 應用程式和資料庫皆可進行通訊。 若要這樣做，須在程式碼和 Web 應用程式中設定連接字串。

1. 在 WCF 服務的 Web 應用程式中 (**SHWCF-EUS2**) > [設定]**** >  [應用程式設定]****，新增名為 **DefaultConnection** 的連接字串。
2. 此連接字串取自 **SmartHotel-Registration** 資料庫，並且應以正確的認證進行更新。

    ![連接字串](./media/contoso-migration-refactor-web-app-sql-managed-instance/string1.png)

3. 他們使用 Visual Studio，從解決方案檔案中開啟 **SmartHotel.Registration.wcf** 專案。 WCF 服務**SmartHotel**之 web.config 檔案的**connectionStrings**區段，應使用連接字串進行更新。

     ![連接字串](./media/contoso-migration-refactor-web-app-sql-managed-instance/string2.png)

4. **SmartHotel**之 web.config 檔案的**用戶端**區段應變更為指向 WCF 服務的新位置中。 這是裝載服務端點的 WCF Web 應用程式 URL。

    ![連接字串](./media/contoso-migration-refactor-web-app-sql-managed-instance/strings3.png)

5. 程式碼進行變更後，管理員必須認可這些變更。 他們在 Visual Studio 中使用 Team Explorer 進行認可和同步。

## <a name="step-6-set-up-build-and-release-pipelines-in-azure-devops"></a>步驟 6：在 Azure DevOps 中設定建置和發行管線

Contoso 管理員現在會設定 Azure DevOps 以執行建置和發行程序。

1. 在 Azure DevOps 中，他們會選取 [**建立和發行**  >  **新管線**]。

    ![新增管線](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline1.png)

2. 他們會選取 [Azure Repos Git]**** 及相關的存放庫。

    ![Git 和存放庫](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline2.png)

3. 在 [選取範本]**** 中，他們為其組建選取 ASP.NET 範本。

     ![ASP.NET 範本](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline3.png)

4. **ContosoSmartHotelRefactor-ASP.NET-CI** 會用來作為組建名稱。 他們選取 [儲存並排入佇列]****。

     ![儲存並排入佇列](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline4.png)

5. 這會啟動第一個組建。 他們選取組建編號以查看程序。 完成之後，他們可以看到程序回饋，選取 [成品]**** 即可檢閱組建結果。

    ![檢閱](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline5.png)

6. [Drop]**** 資料夾會包含組建結果。

    - 這兩個 zip 檔案是包含應用程式的套件。
    - 這些檔案會用於部署至 Azure App Service 的發行管線中。

     ![構件](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline6.png)

7. 他們會選取 [**發行**  >  **+ 新增管線**]。

    ![新增管線](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline7.png)

8. 他們選取 Azure App Service 的部署範本。

    ![Azure App Service 部署範本](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline8.png)

9. 他們會將發行管線命名為 **ContosoSmartHotel360Refactor**，並指定 WCF Web 應用程式的名稱 (SHWCF-EUS2) 作為 [階段]**** 名稱。

    ![環境](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline9.png)

10. 在階段底下，他們選取 [1 個作業，1 個工作]**** 以設定 WCF 服務的部署。

    ![部署 WCF](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline10.png)

11. 他們確認訂用帳戶已選取並授權，然後選取 [App Service 名稱]****。

     ![選取 App Service 名稱](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline11.png)

12. 在管線 > [成品]**** 上，他們會選取 [+ 新增成品]****，然後選擇使用 [ContosoSmarthotel360Refactor]**** 管線來進行建置。

     ![Build](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline12.png)

13. 他們在已核取的成品上選取閃電圖示，以啟用持續部署觸發程序。

     ![閃電圖示](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline13.png)

14. 持續部署觸發程序應設定為 [已啟用]****。

    ![持續部署已啟用](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline14.png)

15. 現在，他們會回到 [1 個作業，1 個工作] 階段，然後選取 [部署 Azure App Service]****。

    ![部署 Azure App Service](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline15.png)

16. 在 [選取檔案或資料夾]**** 中，他們找出在建置期間建立的 **SmartHotel.Registration.Wcf.zip** 檔案，然後選取 [儲存]****。

    ![儲存 WCF](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline16.png)

17. 他們會選取 [**管線**  >  **階段**]、[ **+ 新增**] 以新增**shweb-eus2 的環境-EUS2**。 他們會選取另一個 Azure App Service 部署。

    ![新增環境](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline17.png)

18. 他們重複此程序，將 Web 應用程式 (**SmartHotel.Registration.Web.zip**) 檔案發佈至正確的 Web 應用程式。

    ![發佈 Web 應用程式](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline18.png)

19. 完成儲存後，發行管線會顯示如下。

     ![發行管線摘要](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline19.png)

20. 他們回到 [組建]****，並選取 [觸發程序]**** > [啟用持續整合]****。 這會使管線在變更認可至程式碼以及完整組建和發行發生時進行整合。

    ![啟用持續整合](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline20.png)

21. 他們選取 [儲存並排入佇列]****，以執行完整管線。 此時會觸發新的組建，進而建立 Azure App Service 的第一個應用程式版本。

    ![儲存管線](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline21.png)

22. Contoso 管理員可以依循來自 Azure DevOps 的建置和發行管線程序。 組件完成後，就會開始發行。

    ![組建和發行應用程式](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline22.png)

23. 管線完成後，兩個站台即完成部署，且應用程式已啟動並在線上執行。

    ![完成發行](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline23.png)

此時，應用程式已成功移轉至 Azure。

## <a name="clean-up-after-migration"></a>移轉之後進行清除

移轉之後，Contoso 必須完成以下的清除步驟：

- 從 vCenter 清查中移除內部部署 VM。
- 從本機備份作業中移除 VM。
- 更新內部文件以顯示 SmartHotel360 應用程式的新位置。 顯示在 Azure SQL 受控執行個體中執行的資料庫，以及在兩個 web 應用程式中執行的前端。
- 檢閱與已解除委任 VM 互動的任何資源，並更新任何相關的設定或文件，以反映新的組態。

## <a name="review-the-deployment"></a>檢閱部署

對於 Azure 中的移轉後資源，Contoso 必須能執行一切功能並保護其新的基礎結構。

### <a name="security"></a>安全性

- Contoso 必須確保其新的 **SmartHotel-Registration** 資料庫安全無虞。 [深入了解](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview)。
- 特別是，Contoso 應將 Web 應用程式更新為搭配使用 SSL 與憑證。

### <a name="backups"></a>備份

- Contoso 必須查看 Azure SQL 受控執行個體資料庫的備份需求。 [深入了解](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups)。
- Contoso 也必須了解如何管理 SQL Database 備份和還原。 [深入了解](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups)自動備份。
- Contoso 應考慮實作容錯移轉群組，為該資料庫提供區域性容錯移轉。 [深入了解](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview)。
- 考量到復原能力，Contoso 須考慮在主要的美國東部 2 和美國中部區域部署 Web 應用程式。 Contoso 可以設定流量管理員，以確保發生區域中斷時的容錯移轉。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- 部署好所有資源之後，Contoso 應根據[基礎結構規劃](./contoso-migration-infrastructure.md#set-up-tagging)來指派 Azure 標記。
- 所有授權費用都會併入 Contoso 使用的 PaaS 服務中。 這將會從 EA 中扣除。
- Contoso 會使用[Azure 成本管理](https://azure.microsoft.com/services/cost-management)，確保他們會在其 IT 領導地位所建立的預算內保持不變。

## <a name="conclusion"></a>結論

在本文中，Contoso 已透過將應用程式前端 VM 移轉至兩個 Azure App Service Web 應用程式，來重構 SmartHotel360 應用程式。 已將應用程式資料庫移轉至 Azure SQL 受控執行個體。
