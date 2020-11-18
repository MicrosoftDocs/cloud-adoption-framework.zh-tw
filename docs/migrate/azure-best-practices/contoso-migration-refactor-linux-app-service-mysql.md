---
title: 將 Linux 應用程式重構至 Azure App Service、流量管理員和適用於 MySQL 的 Azure 資料庫
description: 使用適用于 Azure 的雲端採用架構，瞭解如何重構 Linux 服務中心應用程式來 Azure App Service 和適用於 MySQL 的 Azure 資料庫。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: df3ca412e1cf405a0927e13e5c030da66f4a0e53
ms.sourcegitcommit: a7eb2f6c4465527cca2d479edbfc9d93d1e44bf1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94713627"
---
<!-- cSpell:ignore WEBVM SQLVM contosohost vcenter contosodc OSTICKETWEB OSTICKETMYSQL osTicket contosoosticket trafficmanager InnoDB binlog DBHOST DBUSER CNAME -->

# <a name="refactor-a-linux-application-by-using-azure-app-service-traffic-manager-and-azure-database-for-mysql"></a>使用 Azure App Service、流量管理員和適用於 MySQL 的 Azure 資料庫重構 Linux 應用程式

本文說明虛構公司 Contoso 如何重構兩層式 [燈泡](https://wikipedia.org/wiki/LAMP_(software_bundle)) 應用程式，並使用 Azure App Service 搭配 GitHub 整合和適用於 MySQL 的 Azure 資料庫，將其從內部部署遷移至 Azure。

osTicket，我們在此範例中使用的服務台應用程式是以開放原始碼的形式提供。 如果您想要將它用於您自己的測試目的，您可以從 [GitHub 中的 osTicket](https://github.com/osTicket/osTicket)存放庫下載該檔案。

## <a name="business-drivers"></a>商業動機

IT 領導小組與商務合作夥伴密切合作，瞭解他們想要達成的目標：

- **解決業務成長。** Contoso 正在成長並轉向新的市場。 需要額外的客戶服務代理程式。
- **規模。** 應該建置解決方案，好讓 Contoso 隨著商務擴展而新增更多客戶服務代理程式。
- **改善復原能力。** 在過去，系統只會影響內部使用者的問題。 使用新的商業模式，外部使用者會受到影響，而 Contoso 需要應用程式隨時保持運作。

## <a name="migration-goals"></a>移轉目標

為了判斷最佳的遷移方法，Contoso 雲端小組已針對此種遷移程式的目標進行了固定：

- 應用程式應該擴展超過目前的內部部署容量和效能。 Contoso 正在移動應用程式，以充分利用 Azure 的隨選縮放。
- Contoso 想要將應用程式程式碼基底移至持續傳遞管線。 當應用程式變更推送至 GitHub 時，Contoso 想要部署這些變更，而不需要作業人員的工作。
- 應用程式必須具有復原能力與容錯移轉的能力。 Contoso 想要在兩個不同的 Azure 區域中部署應用程式，並將它設定為自動調整。
- 將應用程式移到雲端之後，Contoso 想要將資料庫管理工作減到最少。

## <a name="solution-design"></a>解決方案設計

擬定好目標和需求後，Contoso 會設計和檢閱部署解決方案，並識別移轉程序，包括將用於移轉的 Azure 服務。

## <a name="current-architecture"></a>目前架構

- 應用程式會分層至兩部虛擬機器 (Vm)  (`OSTICKETWEB` 和 `OSTICKETMYSQL`) 。
- VM 位於 VMware ESXi 主機 `contosohost1.contoso.com`(6.5 版)。
- VMware 環境是由 VM 上所執行的 VCenter Server 6.5 (`vcenter.contoso.com`) 進行管理。
- Contoso 有內部部署資料中心 (`contoso-datacenter`) 以及內部部署網域控制站 (`contosodc1`)。

![目前架構的圖表。](./media/contoso-migration-refactor-linux-app-service-mysql/current-architecture.png)

## <a name="proposed-architecture"></a>建議的架構

以下是建議的架構：

- 上的 web 層應用程式將會藉 `OSTICKETWEB` 由在兩個 Azure 區域中建立 Azure App Service web 應用程式來進行遷移。 Contoso 團隊將使用 PHP 7.0 Docker 容器來執行 Linux Azure App Service。
- 應用程式程式碼將會移至 GitHub，而且 Azure App Service 的 web 應用程式將會設定為透過 GitHub 持續傳遞。
- Azure App Service 將會部署在主要區域 (`East US 2`) 和次要區域 (`Central US`) 。
- Azure 流量管理員將會在兩個區域中的兩個 web 應用程式前面進行設定。
- 流量管理員將會在優先順序模式下設定，以強制流量通過 `East US 2` 。
- 如果中的 Azure 應用程式伺服器 `East US 2` 離線，則使用者可以在中存取已容錯移轉的應用程式 `Central US` 。
- 應用程式資料庫將會使用 Azure 資料庫移轉服務遷移至適用於 MySQL 的 Azure 資料庫服務。 內部部署資料庫將在本機進行備份，並直接還原到適用於 MySQL 的 Azure 資料庫。
- 資料庫會位於資料庫子網中 () 的主要區域， `East US 2` (`PROD-DB-EUS2` 生產網路 () 的) `VNET-PROD-EUS2` 。
- 由於他們正在遷移生產工作負載，因此應用程式的 Azure 資源會位於生產資源群組中 `ContosoRG` 。
- 流量管理員資源將會部署在 Contoso 的基礎結構資源群組中 `ContosoInfraRG` 。
- 移轉完成之後，將會解除委任 Contoso 資料中心的內部部署 VM。

![案例架構的圖表。](./media/contoso-migration-refactor-linux-app-service-mysql/proposed-architecture.png)

## <a name="migration-process"></a>移轉程序

Contoso 會完成遷移程式，如下所示：

1. 在第一個步驟中，Contoso 管理員會設定 Azure 基礎結構，包括布建 Azure App Service、設定流量管理員，以及布建適用於 MySQL 的 Azure 資料庫實例。
2. 準備好 Azure 基礎結構之後，他們會使用 Azure 資料庫移轉服務來遷移資料庫。
3. 在 Azure 中執行資料庫之後，他們會上傳 GitHub 私用存放庫以進行 Azure App Service 持續傳遞，並使用 osTicket 應用程式載入它。
4. 在 Azure 入口網站中，他們會執行 Azure App Service，將應用程式從 GitHub 載入至 Docker 容器。
5. 他們會調整 DNS 設定，並設定應用程式的自動調整。

![Contoso 遷移程式的圖表。](./media/contoso-migration-refactor-linux-app-service-mysql/migration-process.png)

### <a name="azure-services"></a>Azure 服務

| 服務 | 描述 | 成本 |
| --- | --- | --- |
| [Azure App Service](https://azure.microsoft.com/services/app-service) | 此服務會使用 Azure 平臺即服務 (適用于網站的 PaaS) 來執行及調整應用程式。 | 定價是以實例的大小和所需的功能為基礎。 [深入了解](https://azure.microsoft.com/pricing/details/app-service/windows)。 |
| [Azure 流量管理員](https://azure.microsoft.com/services/traffic-manager) | 使用網域名稱系統 (DNS) 將使用者導向 Azure 或外部網站和服務的負載平衡器。 | 定價是根據收到的 DNS 查詢數目和受監視的端點數目。 | [深入了解](https://azure.microsoft.com/pricing/details/traffic-manager)。 |
| [Azure Database Migration Service](/azure/dms/dms-overview) | Azure 資料庫移轉服務可讓您從多個資料庫來源順暢地遷移到 Azure 資料平臺，並減少停機時間。 | 深入了解[支援的區域](/azure/dms/dms-overview#regional-availability)和[資料庫移轉服務定價](https://azure.microsoft.com/pricing/details/database-migration)。 |
| [適用於 MySQL 的 Azure 資料庫](/azure/mysql) | 資料庫是以開放原始碼 MySQL 資料庫引擎為基礎。 它為應用程式開發和部署，提供完全受控、符合企業需求的社區 MySQL 資料庫。 | 定價是以計算、儲存體和備份需求為基礎。 [深入了解](https://azure.microsoft.com/pricing/details/mysql)。 |

## <a name="prerequisites"></a>先決條件

若要執行此案例，Contoso 必須符合下列必要條件：

| 需求 | 詳細資料 |
| --- | --- |
| **Azure 訂用帳戶** | Contoso 稍早已在本文章系列中建立訂用帳戶。 如果您沒有 Azure 訂用帳戶，請建立[免費帳戶](https://azure.microsoft.com/free)。 <br><br> 如果您建立免費帳戶，您就是訂用帳戶的管理員，並可執行所有動作。 <br><br> 如果您使用現有訂用帳戶，而且您不是系統管理員，則需要與系統管理員合作，讓其指派擁有者或參與者權限給您。 |
| **Azure 基礎結構** | Contoso 會如[適用於移轉的 Azure 基礎結構](./contoso-migration-infrastructure.md)中所述，設定其 Azure 基礎結構。 |

## <a name="scenario-steps"></a>案例步驟

以下是完成遷移的 Contoso 方案：

> [!div class="checklist"]
>
> - **步驟1：** 布建 Azure App Service。 Contoso 管理員會在主要和次要區域中佈建 Web 應用程式。
> - **步驟2：設定流量管理員**。 他們會在 Web 應用程式前面設定流量管理員，以便路由傳送及平衡流量負載。
> - **步驟3：** 布建適用於 MySQL 的 Azure 資料庫。 在 Azure 中，他們會佈建適用於 MySQL 的 Azure 資料庫執行個體。
> - **步驟4：遷移資料庫**。 他們會使用 Azure 資料庫移轉服務來遷移資料庫。
> - **步驟5：設定 GitHub**。 他們會設定應用程式網站和程式碼的本機 GitHub 存放庫。
> - **步驟6：設定 web 應用程式**。 他們會使用 osTicket 網站來設定 web 應用程式。

## <a name="step-1-provision-azure-app-service"></a>步驟1：布建 Azure App Service

Contoso 管理員會在每個區域中布建兩個 web 應用程式 (一個) 使用 Azure App Service。

1. 他們會在主要區域中建立 web 應用程式資源 (`osticket-eus2`)  (`East US 2`) via Azure Marketplace。
2. 他們會將資源放在生產資源群組中 `ContosoRG` 。

    ![在 Linux 中建立 web 應用程式的 [Web 應用程式] 窗格螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app1.png)

3. 他們會在主要區域中建立 App Service 方案 **SVP-EUS2**，並使用標準大小。

     ![建立 App Service 方案的 [新增 App Service 方案] 窗格螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app2.png)

4. 他們會選取具有 PHP 7.0 執行階段堆疊的 Linux OS，這是一個 Docker 容器。

    ![[Web 應用程式] 窗格的螢幕擷取畫面，其中已選取 [Linux OS]、[美國東部 2] 位置和 [PHP 7.0]。](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app3.png)

5. 他們會建立第二個 web 應用程式、 **osticket-cu**，以及適用于 **美國中部** 的 Azure App Service 方案。

    ![選取 [Web 應用程式] 窗格的螢幕擷取畫面，其中包含 Linux OS、美國中部位置和 PHP 7.0。](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app4.png)

**需要其他協助嗎？**

- 瞭解 [Azure App Service web 應用程式](/azure/app-service/overview)。
- 了解 [Linux 上的 Azure App Service](/azure/app-service/containers/app-service-linux-intro)。

## <a name="step-2-set-up-traffic-manager"></a>步驟 2：設定流量管理員

Contoso 管理員會設定流量管理員，將輸入的 web 要求導向至在 osTicket web 層上執行的 web 應用程式。

1. 在 Azure Marketplace 中，他們會建立流量管理員資源 **osticket.trafficmanager.net**。 他們使用優先順序路由，讓 **美國東部 2** 是主要網站。 他們會將資源放在其現有的基礎結構資源群組 **>contosoinfrarg** 中。 請注意，流量管理員為全域服務，無法繫結至特定位置。

    ![[建立流量管理員設定檔] 窗格的螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager1.png)

2. 他們會使用端點來設定流量管理員。 他們會將美國東部2中的 web 應用程式新增為主要網站、 **osticket eus2**，並在美國中部將 web 應用程式新增為次要網站（ **osticket-cu**）。

    ![[流量管理員] 中 [新增端點] 窗格的螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager2.png)

3. 新增端點之後，系統管理員就可以進行監視。

    ![在流量管理員中監視端點的 [端點] 窗格螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager3.png)

**需要其他協助嗎？**

- 了解[流量管理員](/azure/traffic-manager/traffic-manager-overview)。
- 了解如何[將流量路由傳送至優先端點](/azure/traffic-manager/traffic-manager-configure-priority-routing-method)。

## <a name="step-3-provision-azure-database-for-mysql"></a>步驟 3：佈建適用於 MySQL 的 Azure 資料庫

Contoso 管理員會在主要區域（美國東部2）中布建 MySQL 資料庫實例。

1. 在 Azure 入口網站中，他們可會建立適用於 MySQL 的 Azure 資料庫資源。

    ![Azure 入口網站中適用於 MySQL 的 Azure 資料庫連結的螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-1.png)

2. 他們會為 Azure 資料庫新增名稱 **contosoosticket**。 他們會將資料庫新增至生產資源群組 **ContosoRG** ，然後為其指定認證。
3. 內部部署 MySQL 資料庫的版本為 5.7，因此他們會為了相容性而選取這個版本。 他們會使用預設大小，以符合其資料庫需求。

    ![選取版本5.7 的 [MySQL 伺服器] 窗格螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-2.png)

4. 針對 **備份冗余選項**，他們會選取 [ **異地冗余**]。 此選項可讓他們在其次要區域中還原資料庫， (美國中部) 如果發生中斷情況。 只有在布建資料庫時，才能設定此選項。

    ![選取 [Geo-Redundant] 選項的 [備份冗余選項] 窗格螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/db-redundancy.png)

5. 他們會設定連線安全性。 在資料庫中，他們會選取 [連線 **安全性** ]，然後設定防火牆規則，以允許資料庫存取 Azure 服務。

6. 他們會將本機工作站用戶端 IP 位址新增到開始和結束 IP 位址。 這可讓 Web 應用程式存取 MySQL 資料庫，以及存取執行移轉的資料庫用戶端。

    ![[連線安全性] 窗格的螢幕擷取畫面，其中顯示已開啟的 Azure 服務存取，以及選取的用戶端 IP 位址。](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-3.png)

## <a name="step-4-migrate-the-database"></a>步驟 4：遷移資料庫

有幾種方式可以移動 MySQL 資料庫。 每個選項都需要 Contoso 管理員建立目標的適用於 MySQL 的 Azure 資料庫實例。 建立實例之後，他們就可以使用下列兩個路徑中的任一個來遷移資料庫：

- 步驟4a： Azure 資料庫移轉服務
- 步驟4b： MySQL 工作臺備份和還原

### <a name="step-4a-migrate-the-database-via-azure-database-migration-service"></a>步驟4a：透過 Azure 資料庫移轉服務遷移資料庫

Contoso 管理員會依照 [逐步進行遷移教學](/azure/dms/tutorial-mysql-azure-mysql-online)課程，透過 Azure 資料庫移轉服務來遷移資料庫。 他們可以使用 MySQL 5.6 或5.7 來執行線上、離線和混合式 (預覽版) 的遷移。

> [!NOTE]
> 適用於 MySQL 的 Azure 資料庫支援 MySQL 8.0，但資料庫移轉服務工具尚未支援此版本。

簡言之，Contoso 會執行下列動作：

- 它們可確保符合所有的遷移必要條件：
  - MySQL 資料庫伺服器來源必須符合適用於 MySQL 的 Azure 資料庫支援的版本。 適用於 MySQL 的 Azure 資料庫支援 MySQL 社區版、InnoDB 儲存引擎，以及使用相同版本跨來源和目標進行遷移。

  - 它們可在 `my.ini` (Windows) 或 `my.cnf` (Unix) 中啟用二進位記錄。 若未這麼做，將會在 [遷移嚮導] 中產生下列錯誤：

    「二進位記錄中發生錯誤。 變數 binlog_row_image 的值為「基本」。 請將其變更為「完整」。 如需詳細資訊，請參閱 `https://go.microsoft.com/fwlink/?linkid=873009` 。

  - 使用者必須擁有該 `ReplicationAdmin` 角色。

  - 在不包含外鍵和觸發程式的情況下遷移資料庫架構。

- 他們會建立虛擬私人網路， (透過 ExpressRoute 或 VPN 連線到內部部署網路的 VPN) 。

- 他們會使用連線到虛擬網路的 Premium SKU 來建立 Azure 資料庫移轉服務實例。

- 它們可確保 Azure 資料庫移轉服務可以透過虛擬網路存取 MySQL 資料庫。 這需要確保在虛擬網路層級、網路 VPN 和裝載 MySQL 的機器上，允許從 Azure 到 MySQL 的所有連入埠。

- 他們會執行資料庫移轉服務工具，然後執行下列動作：

  a. 建立以 Premium SKU 為基礎的遷移專案。

    ![[MySQL 總覽] 窗格的螢幕擷取畫面，其中有一則訊息，指出已成功建立遷移服務。](./media/contoso-migration-refactor-linux-app-service-mysql/migration-dms-new-project.png)

    ![MySQL 的 [新增遷移專案] 窗格螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/migration-dms-new-project-02.png)

  b. 將來源 (內部部署資料庫) 。

    ![[新增來源詳細資料] 窗格的 [遷移] 窗格螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/migration-dms-source.png)

  c. 選取目標。

    ![[遷移嚮導] 的 [目標詳細資料] 窗格螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/migration-dms-target.png)

  d. 選取要遷移的資料庫。

    ![[對應到目標資料庫] 窗格的 [遷移至目標資料庫] 窗格螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/migration-dms-databases.png)

  e. 設定 [advanced settings]。

    ![[遷移設定] 窗格的螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/migration-dms-settings.png)

  f. 開始複寫並解決任何錯誤。

    ![[伺服器詳細資料] 窗格的螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/migration-dms-monitor.png)

  g. 執行最後的轉換。

    ![OsTicket 詳細資料窗格的螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/migration-dms-cutover.png)

    ![[完成轉換] 窗格的螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/migration-dms-cutover-complete.png)

    ![[遷移活動狀態] 資料表的螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/migration-dms-cutover-complete-02.png)

  h. 恢復任何外鍵和觸發程式。

  i. 修改應用程式以使用新的資料庫。

    ![[遷移活動] 資料表的螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/migration-dms-cutover-apps.png)

### <a name="step-4b-migrate-the-database-mysql-workbench"></a>步驟4b：將資料庫移轉 (MySQL 工作臺) 

1. Contoso 管理員會檢查 [必要條件，並下載 MySQL 工作臺](https://dev.mysql.com/downloads/workbench/?utm_source=tuicool)。
2. 他們會按照[安裝指示](https://dev.mysql.com/doc/workbench/en/wb-installing.html)，安裝適用於 Windows 的 MySQL Workbench。 在其上安裝 MySQL 工作臺的電腦必須可透過網際網路存取 OSTICKETMYSQL VM 和 Azure。
3. 在 MySQL Workbench 中，他們會建立 OSTICKETMYSQL 的 MySQL 連線。

    ![[MySQL 工作臺連接詳細資料] 窗格的螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/workbench1.png)

4. 他們會將資料庫匯出為 `osticket` 本機的獨立檔案。

    ![MySQL 工作臺 [資料匯出] 窗格的螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/workbench2.png)

5. 在本機備份資料庫之後，系統管理員會建立適用於 MySQL 的 Azure 資料庫實例的連接。

    ![MySQL 工作臺的 [設定新連接] 窗格。](./media/contoso-migration-refactor-linux-app-service-mysql/workbench3.png)

6. 現在，他們可以從獨立的檔案匯入 (restore) 適用於 MySQL 的 Azure 資料庫實例中的資料庫。 建立實例的新架構 `osticket` 。

    ![MySQL 工作臺 [資料匯入] 窗格的螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/workbench4.png)

7. 在還原資料之後，系統管理員可以使用 MySQL 工作臺來查詢資料。 資料會顯示在 Azure 入口網站中。

    ![Azure 入口網站的螢幕擷取畫面，其中顯示已還原的資料。](./media/contoso-migration-refactor-linux-app-service-mysql/workbench5.png)

    ![[我的 SQL 資料庫] 分頁的螢幕擷取畫面，其中箭號指向 osticket 資料庫。](./media/contoso-migration-refactor-linux-app-service-mysql/workbench6.png)

8. 系統管理員會更新 web 應用程式上的資料庫資訊。 在 MySQL 執行個體上，他們會開啟 **連接字串**。

    ![MySQL 實例中 [連接字串] 連結的螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/workbench7.png)

9. 在 [連接字串] 清單中，他們會選取 web 應用程式設定，然後選取 [ **按一下以複製**] 進行複製。

    ![MySQL 實例中 web 應用程式設定的螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/workbench8.png)

10. 他們會在 [記事本] 中開啟新檔案、將字串貼到其中，然後更新字串以符合 osTicket 資料庫、MySQL 實例和認證設定。

    ![記事本檔案中所貼上之連接字串的螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/workbench9.png)

11. 他們可以在 Azure 入口網站的 MySQL 實例的 [ **總覽** ] 窗格中，確認伺服器名稱和登入。

    ![[資源群組] 窗格的螢幕擷取畫面，其中顯示 [伺服器名稱] 和 [伺服器管理員登入名稱]。](./media/contoso-migration-refactor-linux-app-service-mysql/workbench10.png)

## <a name="step-5-set-up-github"></a>步驟 5︰設定 GitHub

Contoso 管理員會建立新的私人 GitHub 存放庫，並在適用於 MySQL 的 Azure 資料庫中設定與 osTicket 資料庫的連線。 然後，他們會將 Web 應用程式載入 Azure App Service 中。

1. 他們會流覽至 osTicket software 公用 GitHub 存放庫，並將它派生給 Contoso GitHub 帳戶。

    ![GitHub 儲存機制頁面的螢幕擷取畫面，其中反白顯示 [分叉] 按鈕。](./media/contoso-migration-refactor-linux-app-service-mysql/github1.png)

2. 在派生存放庫之後，他們會移至 *include* 資料夾，然後尋找並選取 *ost-config php* 檔案。

    ![GitHub 中 PHP 檔案的螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/github2.png)

3. 檔案會在瀏覽器中開啟，並加以編輯。

    ![GitHub 中 [檔案編輯] (鉛筆) ] 圖示的螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/github3.png)

4. 在編輯器中，系統管理員會更新資料庫詳細資料，特別是 `DBHOST` 和 `DBUSER` 。

    ![GitHub 中 [檔案編輯] 窗格的螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/github4.png)

5. 他們會認可變更。

    ![醒目提示 [編輯] 窗格上的 [認可變更] 按鈕的螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/github5.png)

6. 針對 (osticket-eus2 和 osticket-cu) 的每個 web 應用程式，在 Azure 入口網站中，他們會選取左窗格上的 [ **應用程式設定** ]，然後修改設定。

    ![反白顯示 Azure 入口網站中的 [應用程式設定] 連結的螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/github6.png)

7. 他們以名稱輸入連接字串 `osticket` ，並將 [記事本] 中的字串複製到 [ **值] 區域** 中。 他們會選取字串旁邊下拉式清單中的 **MySQL**，並儲存設定。

    ![[連接字串] 窗格的螢幕擷取畫面，其中反白顯示 osTicket 連接字串。](./media/contoso-migration-refactor-linux-app-service-mysql/github7.png)

## <a name="step-6-configure-the-web-apps"></a>步驟6：設定 web 應用程式

在遷移程式的最後一個步驟中，Contoso 管理員會使用 osTicket 網站來設定 web 應用程式。

1. 在主要 web 應用程式（osticket-eus2）中，他們會開啟 [ **部署] 選項** ，然後將來源設定為 **GitHub**。

    ![[部署選項] 窗格的螢幕擷取畫面，其中已選取 GitHub 作為來源。](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app1.png)

2. 他們會選取部署選項。

    ![[部署選項] 窗格中 [選項詳細資料] 的螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app2.png)

3. 設定這些選項之後，設定會在 Azure 入口網站中顯示為 *擱置* 。

    ![顯示擱置網站狀態的 [部署選項] 窗格螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app3.png)

4. 更新設定之後，將 osTicket web 應用程式從 GitHub 載入至執行 Azure App Service 的 Docker 容器之後，網站會顯示為使用中 *狀態*。

    ![[部署選項] 窗格的螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app4.png)

5. 他們會針對次要 web 應用程式（osticket-cu）重複上述步驟。
6. 設定好網站之後，即可透過流量管理員設定檔進行存取。 DNS 名稱是 osTicket 應用程式的新位置。 [深入了解](/azure/app-service/app-service-web-tutorial-custom-domain#map-a-cname-record)。

    ![[流量管理員設定檔] 窗格的螢幕擷取畫面，其中顯示 DNS 名稱。](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app5.png)

7. Contoso 想要使用容易記住的 DNS 名稱。 在 [ **新增資源記錄** ] 窗格中，他們會建立別名、 **CNAME** 和完整功能變數名稱（ **osticket.contoso.com**），該名稱會指向網域控制站上 DNS 中的流量管理員名稱。

    ![[新增資源記錄] 窗格的螢幕擷取畫面，其中顯示別名名稱和流量管理員的指標。](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app6.png)

8. 他們會設定 osticket eus2 和 osticket cu web 應用程式，以允許自訂主機名稱。

    ![[Ad 主機名稱] 窗格的螢幕擷取畫面，其中反白顯示 [驗證] 按鈕。](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app7.png)

### <a name="set-up-autoscaling"></a>設定自動調整

最後，Contoso 管理員會為應用程式設定自動調整。 自動調整可確保當代理程式使用應用程式時，應用程式實例會根據商務需求而增加和減少。

1. 在 App Service **應用程式 SVP-EUS2** 中，他們會開啟 **縮放單位**。
2. 他們使用單一規則來設定新的自動調整設定，以在目前實例的 CPU 使用量超過 70% 10 分鐘時，將實例計數增加一個。

    ![第一個區域的自動調整設定頁面螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/autoscale1.png)

3. 他們會在 **應用程式 SVP-cu** 上設定相同的設定，以確保應用程式容錯移轉至次要區域時，會套用相同的行為。 唯一的差別在於它們會將預設實例設定為1，因為這僅適用于容錯移轉。

   ![第二個區域的自動調整設定頁面螢幕擷取畫面。](./media/contoso-migration-refactor-linux-app-service-mysql/autoscale2.png)

## <a name="clean-up-after-migration"></a>移轉之後進行清除

完成遷移之後，osTicket 應用程式會重構，以使用私人 GitHub 存放庫持續傳遞，以在 Azure App Service web 應用程式中執行。 應用程式會在兩個區域中執行，以提高復原能力。 OsTicket 資料庫在遷移至 PaaS 平臺之後，會在適用於 MySQL 的 Azure 資料庫中執行。

若要在遷移後進行清除，Contoso 會執行下列動作：

- 他們會從 vCenter 清查中移除 VMware Vm。
- 他們會從本機備份作業中移除內部部署 VM。
- 他們會更新內部檔以顯示新的位置和 IP 位址。
- 他們會檢查與內部部署 Vm 互動的任何資源，並更新任何相關的設定或檔，以反映新的設定。
- 他們會重新設定監視以指向 `osticket-trafficmanager.net` URL，以追蹤應用程式是否已啟動並執行。

## <a name="review-the-deployment"></a>檢閱部署

當應用程式正在執行時，Contoso 必須完全讓並保護其新的基礎結構。

### <a name="security"></a>安全性

Contoso 安全性小組會檢查應用程式，以判斷任何安全性問題。 它們會識別 osTicket 應用程式與 MySQL 資料庫實例之間的通訊並未針對 SSL 進行設定。 他們會這麼做，以確保資料庫的流量不會遭到駭客入侵。 [深入了解](/azure/mysql/howto-configure-ssl)。

### <a name="backups"></a>備份

- OsTicket web 應用程式不會包含狀態資料，因此不需要備份。
- Contoso 小組不需要設定資料庫的備份。 適用於 MySQL 的 Azure 資料庫會自動建立伺服器備份和存放區。 小組選擇為資料庫使用異地複寫，因此它具有復原能力和生產環境就緒。 備份可以用來將伺服器還原至某個時間點。 [深入了解](/azure/mysql/concepts-backup)。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- PaaS 部署沒有任何授權問題。
- Contoso 將會使用 [Azure 成本管理和帳單](/azure/cost-management-billing/cost-management-billing-overview) ，以確保他們在 IT 領導的預算內保持不變。
