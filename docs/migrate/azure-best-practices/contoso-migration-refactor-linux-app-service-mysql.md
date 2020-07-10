---
title: 將 Linux 應用程式重構為 Azure App Service、流量管理員和適用於 MySQL 的 Azure 資料庫
description: 使用適用于 Azure 的雲端採用架構，瞭解如何將 Linux 服務台應用程式重構為 Azure App Service 和適用於 MySQL 的 Azure 資料庫。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 4e92222389dea22f22283c2014d89b6ea33008e4
ms.sourcegitcommit: bcc73d194c6d00c16ae2e3c7fb2453ac7dbf2526
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/09/2020
ms.locfileid: "86192229"
---
<!-- cSpell:ignore WEBVM SQLVM contosohost vcenter contosodc OSTICKETWEB OSTICKETMYSQL osTicket contosoosticket trafficmanager InnoDB binlog DBHOST DBUSER CNAME -->

# <a name="refactor-a-linux-application-to-multiple-regions-using-azure-app-service-traffic-manager-and-azure-database-for-mysql"></a>使用 Azure App Service、流量管理員和適用於 MySQL 的 Azure 資料庫，將 Linux 應用程式重構到多個區域

本文說明虛構公司 Contoso 如何重構兩層式[燈泡](https://wikipedia.org/wiki/LAMP_(software_bundle))應用程式，使用 Azure App Service 搭配 GitHub 整合和適用於 MySQL 的 Azure 資料庫，將其從內部部署遷移至 Azure。

osTicket，此範例中使用的服務中心應用程式是以開放原始碼的形式提供。 如果您想要將它用於自己的測試用途，您可以從[GitHub 的 osTicket](https://github.com/osTicket/osTicket)存放庫下載它。

## <a name="business-drivers"></a>商業動機

IT 領導小組與商務合作夥伴密切合作，以瞭解他們想要達成的目標：

- **解決業務成長。** Contoso 正在成長並轉向新的市場。 需要額外的客戶服務代理程式。
- **尺度.** 應該建置解決方案，好讓 Contoso 隨著商務擴展而新增更多客戶服務代理程式。
- **改善復原能力。** 在過去，系統只會影響內部使用者的問題。 有了新的商務模型，外部使用者會受到影響，而 Contoso 需要應用程式隨時都能正常運作。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已針對此次移轉擬定好各項目標，以便決定最佳移轉方法：

- 應用程式應該擴展超過目前的內部部署容量和效能。 Contoso 正在移動應用程式，以充分利用 Azure 的隨選縮放。
- Contoso 想要將應用程式程式碼基底移至持續傳遞管線。 當應用程式變更推送至 GitHub 時，Contoso 想要部署那些變更，而不需要作業人員的工作。
- 應用程式必須先具備成長與容錯移轉的能力，才算是具備復原性。 Contoso 想要在兩個不同的 Azure 區域中部署應用程式，並將它設定為自動調整。
- 將應用程式移到雲端之後，Contoso 想要將資料庫管理工作減到最少。

## <a name="solution-design"></a>解決方案設計

擬定好目標和需求後，Contoso 會設計和檢閱部署解決方案，並識別移轉程序，包括將用於移轉的 Azure 服務。

## <a name="current-architecture"></a>目前架構

- 應用程式會跨兩個 Vm 分層 (`OSTICKETWEB` 和 `OSTICKETMYSQL`) 。
- VM 位於 VMware ESXi 主機 `contosohost1.contoso.com`(6.5 版)。
- VMware 環境是由 VM 上所執行的 VCenter Server 6.5 (`vcenter.contoso.com`) 進行管理。
- Contoso 有內部部署資料中心 (`contoso-datacenter`) 以及內部部署網域控制站 (`contosodc1`)。

![目前架構](./media/contoso-migration-refactor-linux-app-service-mysql/current-architecture.png)

## <a name="proposed-architecture"></a>建議的架構

以下是建議的架構：

- 上的 web 層應用程式 `OSTICKETWEB` 將透過在兩個 Azure 區域中建立 Azure App Service 來進行遷移。 適用于 Linux 的 Azure App Service 將使用 PHP 7.0 docker 容器來執行。
- 應用程式代碼將會移至 GitHub，而 Azure App Service web 應用程式將會設定為使用 GitHub 進行持續傳遞。
- Azure App Service 將會部署在主要區域 (`East US 2`) 和次要區域 () 中 `Central US` 。
- 在這兩個區域中，流量管理員將會設定於兩個 Web 應用程式前面。
- 流量管理員將會在優先順序模式下設定，以強制流量通過 `East US 2` 。
- 如果中的 Azure 應用程式伺服器 `East US 2` 離線，使用者就可以在中存取故障的應用程式 `Central US` 。
- 應用程式資料庫將會使用 Azure 資料庫移轉服務遷移至適用於 MySQL 的 Azure 資料庫服務。 內部部署資料庫將在本機進行備份，並直接還原到適用於 MySQL 的 Azure 資料庫。
- 資料庫會位於主要區域中， (`East US 2`) 在 `PROD-DB-EUS2` 生產網路 () 的資料庫子網中 () `VNET-PROD-EUS2` ：
- 因為他們正在遷移生產工作負載，所以應用程式的 Azure 資源會位於生產資源群組中 `ContosoRG` 。
- 流量管理員資源會部署在 Contoso 的基礎結構資源群組中 `ContosoInfraRG` 。
- 移轉完成之後，將會解除委任 Contoso 資料中心的內部部署 VM。

![案例架構](./media/contoso-migration-refactor-linux-app-service-mysql/proposed-architecture.png)

## <a name="migration-process"></a>移轉程序

Contoso 會按照下列方式完成移轉程序：

1. 在第一個步驟中，Contoso 管理員會設定 Azure 基礎結構，包括布建 Azure App Service、設定流量管理員，以及布建適用於 MySQL 的 Azure 資料庫實例。
2. 準備好 Azure 基礎結構之後，他們會使用 Azure 資料庫移轉服務來遷移資料庫。
3. 在 Azure 中執行資料庫之後，他們會針對具有持續傳遞的 Azure App Service 提供 GitHub 私人存放庫，並使用 osTicket 應用程式將它載入。
4. 在 Azure 入口網站中，他們會將應用程式從 GitHub 載入至執行 Azure App Service 的 docker 容器。
5. 他們會調整 DNS 設定，並設定應用程式的自動調整。

![移轉程序](./media/contoso-migration-refactor-linux-app-service-mysql/migration-process.png)

### <a name="azure-services"></a>Azure 服務

| 服務 | 描述 | 成本 |
| --- | --- | --- |
| [Azure App Service](https://azure.microsoft.com/services/app-service) | 服務會使用適用於網站的 Azure PaaS 服務，執行和調整應用程式。 | 定價是以執行個體的大小和所需的功能為基礎。 [深入了解](https://azure.microsoft.com/pricing/details/app-service/windows)。 |
| [流量管理員](https://azure.microsoft.com/services/traffic-manager) | 使用 DNS 將使用者導向 Azure 或外部網站和服務的負載平衡器。 | 定價是以收到的 DNS 查詢數目和已監視的端點數目為基礎。 | [深入了解](https://azure.microsoft.com/pricing/details/traffic-manager)。 |
| [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) | Azure 資料庫移轉服務可在停機時間最短的情況之下，從多個資料庫來源順暢地遷移至 Azure 資料平臺。 | 深入了解[支援的區域](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability)和[資料庫移轉服務定價](https://azure.microsoft.com/pricing/details/database-migration)。 |
| [適用於 MySQL 的 Azure 資料庫](https://docs.microsoft.com/azure/mysql) | 資料庫是以開放原始碼 MySQL 資料庫引擎為基礎。 它提供完全受控、符合企業需求的社區 MySQL 資料庫，供應用程式開發和部署之用。 | 定價是以計算、儲存和備份需求為基礎。 [深入了解](https://azure.microsoft.com/pricing/details/mysql)。 |

## <a name="prerequisites"></a>必要條件

以下是 Contoso 要執行此案例所需的項目。

| 需求 | 詳細資料 |
| --- | --- |
| **Azure 訂用帳戶** | Contoso 稍早已在本文章系列中建立訂用帳戶。 若您沒有 Azure 訂用帳戶，請建立一個[免費帳戶](https://azure.microsoft.com/free)。 <br><br> 如果您建立免費帳戶，您就是訂用帳戶的管理員，並可執行所有動作。 <br><br> 如果您使用現有訂用帳戶，而且您不是系統管理員，則需要與系統管理員合作，讓其指派擁有者或參與者權限給您。 |
| **Azure 基礎結構** | Contoso 會如[適用於移轉的 Azure 基礎結構](./contoso-migration-infrastructure.md)中所述，設定其 Azure 基礎結構。 |

## <a name="scenario-steps"></a>案例步驟

以下是 Contoso 完成移轉的方式：

> [!div class="checklist"]
>
> - **步驟1：布建 Azure App Service。** Contoso 管理員會在主要和次要區域中佈建 Web 應用程式。
> - **步驟2：設定流量管理員。** 他們會在 Web 應用程式前面設定流量管理員，以便路由傳送及平衡流量負載。
> - **步驟3：布建 MySQL。** 在 Azure 中，他們會佈建適用於 MySQL 的 Azure 資料庫執行個體。
> - **步驟4：遷移資料庫。** 他們會使用 Azure 資料庫移轉服務來遷移資料庫。
> - **步驟5：設定 GitHub。** 他們會設定應用程式網站/程式碼的本機 GitHub 存放庫。
> - **步驟6：部署 web 應用程式。** 他們會從 GitHub 部署 Web 應用程式。

## <a name="step-1-provision-azure-app-service"></a>步驟1：布建 Azure App Service

Contoso 管理員會使用 Azure App Service 佈建兩個 Web 應用程式 (每個區域一個)。

1. 他們會在主要區域中建立 web 應用程式資源 (`osticket-eus2`) ， (透過 `East US 2` Azure Marketplace) 。
2. 它們會將資源放在生產資源群組中 `ContosoRG` 。

    ![Azure Web 應用程式](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app1.png)

3. 他們會 `APP-SVP-EUS2` 使用標準大小，在主要區域中建立新的 App Service 方案 () 。

     ![Azure 應用程式](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app2.png)

4. 他們會選取具有 PHP 7.0 執行時間堆疊的 Linux OS，這是一個 docker 容器。

    ![Azure 應用程式](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app3.png)

5. 他們會建立第二個 web 應用程式 (`osticket-cus`) 和 Azure App Service 方案 `Central US` 。

    ![Azure 應用程式](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app4.png)

**需要其他協助？**

- 深入瞭解[Azure App Service web 應用程式](https://docs.microsoft.com/azure/app-service/overview)。
- 了解 [Linux 上的 Azure App Service](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro)。

## <a name="step-2-set-up-traffic-manager"></a>步驟 2：設定流量管理員

Contoso 管理員會設定流量管理員，將輸入的 Web 要求導向至在 osTicket Web 層上執行的 Web 應用程式。

1. 他們會從 Azure Marketplace 建立流量管理員資源 (`osticket.trafficmanager.net`) 。 它們會使用優先順序路由，因此 `East US 2` 是主要網站。 它們會將資源放在其基礎結構資源群組 (`ContosoInfraRG`) 中。 請注意，流量管理員為全域服務，無法繫結至特定位置。

    ![流量管理員](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager1.png)

2. 現在，他們會使用端點來設定流量管理員。 他們會在中新增 web 應用程式 `East US 2` 作為主要網站 (`osticket-eus2`) ，並在中將 web 應用程式當做 `Central US` 次要網站 (`osticket-cus`) 。

    ![在流量管理員中新增端點](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager2.png)

3. 新增端點之後，它們可以進行監視。

    ![監視流量管理員中的端點](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager3.png)

**需要其他協助？**

- 了解[流量管理員](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)。
- 了解如何[將流量路由傳送至優先端點](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-configure-priority-routing-method)。

## <a name="step-3-provision-azure-database-for-mysql"></a>步驟 3：佈建適用於 MySQL 的 Azure 資料庫

Contoso 管理員會在主要區域中布建 MySQL 資料庫實例， (`East US 2`) 。

1. 在 Azure 入口網站中，他們可會建立適用於 MySQL 的 Azure 資料庫資源。

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-1.png)

2. 他們會新增 `contosoosticket` Azure 資料庫的名稱。 他們會將資料庫新增至生產資源群組 `ContosoRG` ，並為其指定認證。
3. 內部部署 MySQL 資料庫的版本為 5.7，因此他們會為了相容性而選取這個版本。 他們會使用預設大小，以符合其資料庫需求。

     ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-2.png)

4. 針對 [備份備援選項]****，他們會選擇使用 [異地備援]****。 此選項可讓他們在次要區域中還原資料庫， (`Central US`) 發生中斷的情況。 他們在佈建資料庫時，只能設定這個選項。

    ![備援性](./media/contoso-migration-refactor-linux-app-service-mysql/db-redundancy.png)

5. 他們會設定連線安全性。 在 [資料庫 > 連線**安全性**] 中，他們會設定防火牆規則，以允許資料庫存取 Azure 服務。

6. 他們會將本機工作站用戶端 IP 位址新增到開始和結束 IP 位址。 這可讓 Web 應用程式存取 MySQL 資料庫，以及存取執行移轉的資料庫用戶端。

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-3.png)

## <a name="step-4-migrate-the-database"></a>步驟 4：遷移資料庫

有數種方式可以移動 MySQL 資料庫。 每個選項都需要您建立目標的適用於 MySQL 的 Azure 資料庫實例。 建立之後，您可以使用兩個路徑來執行遷移：

- 步驟4a： Azure 資料庫移轉服務
- 步驟4b： MySQL 工作臺備份和還原

### <a name="step-4a-migrate-the-database-via-azure-database-migration-service"></a>步驟4a：透過 Azure 資料庫移轉服務遷移資料庫

Contoso 管理員會遵循[逐步遷移教學](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online)課程，透過 Azure 資料庫移轉服務來遷移資料庫。 他們可以使用 MySQL 5.6 或5.7，執行線上、離線和混合式 (預覽) 遷移。

> [!NOTE]
> 適用於 MySQL 的 Azure 資料庫支援 MySQL 8.0，但資料庫移轉服務工具尚不支援此版本。

總而言之，您必須執行下列動作：

- 確保符合所有的遷移必要條件：
  - MySQL 資料庫伺服器來源必須符合適用於 MySQL 的 Azure 資料庫支援的版本。 適用於 MySQL 的 Azure 資料庫支援 MySQL 社區版本、InnoDB 儲存引擎，以及跨來源與目標的相同版本進行遷移。
  - 在 `my.ini` (Windows) 或 `my.cnf` (Unix) 中啟用二進位記錄。 執行此動作時，將會導致 `error in binary logging. Variable binlog_row_image has value 'minimal'. Please change it to 'full'. For more information, see https://go.microsoft.com/fwlink/?linkid=873009` 遷移嚮導。
  - 使用者必須擁有 `ReplicationAdmin` 角色。
  - 不搭配外鍵和觸發程式來遷移資料庫架構。
- 建立透過 ExpressRoute 或 VPN 連接到內部部署網路的虛擬網路。
- 使用 `Premium` 連線到 VNet 的 SKU 來建立 Azure 資料庫移轉服務。
- 確定 Azure 資料庫移轉服務可以透過虛擬網路存取 MySQL 資料庫。 這會需要確保在虛擬網路層級、網路 VPN 和裝載 MySQL 的機器上，允許從 Azure 到 MySQL 的所有連入埠。
- 執行資料庫移轉服務工具：
  - 建立以**PREMIUM SKU**為基礎的遷移專案。

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/migration-dms-new-project.png)

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/migration-dms-new-project-02.png)

  - 在內部部署資料庫) 中新增來源 (。

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/migration-dms-source.png)

  - 選取目標。

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/migration-dms-target.png)

  - 選取要遷移的資料庫。

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/migration-dms-databases.png)

  - 設定 [高級設定]。

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/migration-dms-settings.png)

  - 啟動複寫並解決任何錯誤。

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/migration-dms-monitor.png)

  - 執行最後的轉換。

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/migration-dms-cutover.png)

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/migration-dms-cutover-complete.png)

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/migration-dms-cutover-complete-02.png)

  - 恢復任何外鍵和觸發程式。

  - 修改應用程式以使用新的資料庫。

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/migration-dms-cutover-apps.png)

### <a name="step-4b-migrate-the-database-mysql-workbench"></a>步驟4b：將資料庫移轉 (MySQL 工作臺) 

1. 他們會檢查[必要條件以及下載 MySQL Workbench](https://dev.mysql.com/downloads/workbench/?utm_source=tuicool) (英文)。
2. 他們會按照[安裝指示](https://dev.mysql.com/doc/workbench/en/wb-installing.html)，安裝適用於 Windows 的 MySQL Workbench。 其安裝所在的電腦必須可供 `OSTICKETMYSQL` VM 和 Azure 透過網際網路存取。
3. 在 MySQL 工作臺中，他們會建立與的 MySQL 連線 `OSTICKETMYSQL` 。

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench1.png)

4. 它們會將資料庫匯出為 `osticket` 本機獨立檔案。

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench2.png)

5. 在本機備份資料庫之後，他們會建立適用於 MySQL 的 Azure 資料庫執行個體連線。

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench3.png)

6. 他們現在可以在適用於 MySQL 的 Azure 資料庫執行個體中，從該獨立檔案匯入 (還原) 資料庫。 為實例建立新的架構 (`osticket`) 。

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench4.png)

7. 還原資料之後，您可以使用 MySQL 工作臺進行查詢，並出現在 Azure 入口網站中。

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench5.png)

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench6.png)

8. 最後，他們必須更新 Web 應用程式上的資料庫資訊。 在 MySQL 執行個體上，他們會開啟**連接字串**。

     ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench7.png)

9. 在字串清單中，他們會找出 Web 應用程式設定，然後選取加以複製。

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench8.png)

10. 他們會開啟 [記事本] 視窗並將字串貼入新檔案，然後加以更新它，以符合 osticket 資料庫、MySQL 執行個體和認證設定。

     ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench9.png)

11. 他們可以在 Azure 入口網站的 MySQL 執行個體中，從 [概觀]**** 確認伺服器名稱和登入。

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench10.png)

## <a name="step-5-set-up-github"></a>步驟 5︰設定 GitHub

Contoso 管理員會建立新的私人 GitHub 存放庫，並在適用於 MySQL 的 Azure 資料庫中設定與 osTicket 資料庫的連接。 然後，他們會將 Web 應用程式載入 Azure App Service 中。

1. 他們會瀏覽至 OsTicket 軟體公用 GitHub 存放庫，並將它分支處理至 Contoso GitHub 帳戶。

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github1.png)

2. 分叉之後，他們會流覽至 `include` 資料夾，並尋找檔案 `ost-config.php` 。

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github2.png)

3. 檔案會在瀏覽器中開啟，他們會加以編輯。

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github3.png)

4. 在編輯器中，他們會更新資料庫詳細資料，特別是針對 `DBHOST` 和 `DBUSER` 。

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github4.png)

5. 他們會接著認可變更。

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github5.png)

6. 針對每個 (和) 的 web 應用程式 `osticket-eus2` `osticket-cus` ，他們會修改 Azure 入口網站中的**應用程式設定**。

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github6.png)

7. 他們會輸入名稱為的連接字串 `osticket` ，並將字串從 [記事本] 複製到 [**值] 區域**。 他們會選取字串旁邊下拉式清單中的 **MySQL**，並儲存設定。

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github7.png)

## <a name="step-6-configure-the-web-apps"></a>步驟6：設定 web 應用程式

在移轉程序的最後一個步驟中，Contoso 管理員會使用 osTicket 網站來設定 Web 應用程式。

1. 在 () 的主要 web 應用程式中 `osticket-eus2` ，他們會開啟 [**部署選項**] 並將來源設定為 [ **GitHub**]。

    ![設定應用程式](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app1.png)

2. 他們會選取部署選項。

    ![設定應用程式](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app2.png)

3. 設定好選項之後，Azure 入口網站中的組態會顯示為擱置。

    ![設定應用程式](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app3.png)

4. 更新設定並將 osTicket web 應用程式從 GitHub 載入至執行 Azure App Service 的 docker 容器之後，網站就會顯示為 [使用中]。

    ![設定應用程式](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app4.png)

5. 他們會針對次要 web 應用程式 () 重複上述步驟 `osticket-cus` 。
6. 設定好網站之後，即可透過流量管理員設定檔進行存取。 DNS 名稱是 osTicket 應用程式的新位置。 [深入了解](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain#map-a-cname-record)。

    ![設定應用程式](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app5.png)

7. Contoso 想要有好記的 DNS 名稱。 他們會 `osticket.contoso.com` 在其網域控制站的 DNS 中，建立 (CNAME) 的別名記錄，以指向流量管理員名稱。

    ![設定應用程式](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app6.png)

8. 他們會設定 `osticket-eus2` 和 `osticket-cus` web 應用程式，以允許自訂主機名稱。

    ![設定應用程式](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app7.png)

### <a name="set-up-autoscaling"></a>設定自動調整

最後，他們會為應用程式設定自動調整。 這可確保當代理程式使用應用程式時，應用程式實例會根據商務需求而增加和減少。

1. 在 app service 中 `APP-SRV-EUS2` ，他們會開啟 [**縮放單位**]。
2. 他們以單一規則設定新的自動調整設定，以便在目前執行個體的 CPU 百分比超過 70% 達到 10 分鐘時，將執行個體計數遞增一。

    ![Autoscale](./media/contoso-migration-refactor-linux-app-service-mysql/autoscale1.png)

3. 他們會在上進行相同的設定， `APP-SRV-CUS` 以確保當應用程式故障切換到次要區域時，會套用相同的行為。 唯一的差異在於他們將預設執行個體設定為 1，因為這僅適用於容錯移轉。

   ![Autoscale](./media/contoso-migration-refactor-linux-app-service-mysql/autoscale2.png)

## <a name="clean-up-after-migration"></a>移轉之後進行清除

當遷移完成時，osTicket 應用程式會重構為在 Azure App Service web 應用程式中執行，並使用私人 GitHub 存放庫進行持續傳遞。 應用程式會在兩個區域中執行，以提高復原能力。 OsTicket 資料庫會在遷移至 PaaS 平臺之後適用於 MySQL 的 Azure 資料庫中執行。

若要進行清除，Contoso 必須執行下列動作：

- 從 vCenter 清查中移除 VMware VM。
- 從本機備份作業中移除內部部署 VM。
- 更新內部文件以顯示新的位置和 IP 位址。
- 檢閱與內部部署 VM 互動的任何資源，並更新任何相關的設定或文件，以反映新的組態。
- 重新設定監視以指向 `osticket-trafficmanager.net` URL，以追蹤應用程式已啟動且正在執行。

## <a name="review-the-deployment"></a>檢閱部署

現在應用程式正在執行，Contoso 必須完全讓並保護其新的基礎結構。

### <a name="security"></a>安全性

Contoso 安全性小組已審查應用程式，以判斷是否有任何安全性問題。 他們發現 osTicket 應用程式與 MySQL 資料庫實例之間的通訊並未針對 SSL 進行設定。 他們必須這麼做，才能確保資料庫流量不會遭到駭客入侵。 [深入了解](https://docs.microsoft.com/azure/mysql/howto-configure-ssl)。

### <a name="backups"></a>備份

- OsTicket web 應用程式不包含狀態資料，因此不需要備份。
- 他們不需要設定資料庫的備份。 適用於 MySQL 的 Azure 資料庫會自動建立伺服器備份和存放區。 他們選擇對資料庫使用異地備援，所以資料庫可復原並已準備好用於生產。 備份可以用來將伺服器還原至某個時間點。 [深入了解](https://docs.microsoft.com/azure/mysql/concepts-backup)。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- PaaS 部署沒有任何授權問題。
- Contoso 會使用[Azure 成本管理和帳單](https://docs.microsoft.com/azure/cost-management-billing/cost-management-billing-overview)，確保他們會在其 IT 領導地位所建立的預算內保持不變。
