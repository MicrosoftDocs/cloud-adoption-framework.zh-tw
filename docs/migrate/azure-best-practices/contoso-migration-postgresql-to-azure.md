---
title: 將于 postgresql 資料庫移轉至 Microsoft Azure
description: 瞭解 Contoso 如何將其內部部署于 postgresql 資料庫移轉至 Azure。
author: deltadan
ms.author: abuck
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: azure-migrate
ms.openlocfilehash: 8de202b5f6f8f0420541792898bf609360e79d28
ms.sourcegitcommit: bcc73d194c6d00c16ae2e3c7fb2453ac7dbf2526
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/09/2020
ms.locfileid: "86197716"
---
<!-- cSpell:ignore BYOK postgres psql dvdrental -->

# <a name="migrate-postgresql-databases-to-microsoft-azure"></a>將于 postgresql 資料庫移轉至 Microsoft Azure

本文示範虛構公司 Contoso 如何規劃並將其內部部署于 postgresql 開放原始碼資料庫平臺遷移至 Azure。

## <a name="business-drivers"></a>商業動機

IT 領導小組與商務合作夥伴密切合作，以了解此次移轉所要實現的目標：

- **Big data：** Contoso 會使用於 postgresql 來處理其中幾個 big data 和 AI 計畫，他們想要能夠建立可調整的可重複管線，以將許多分析工作負載自動化。
- **提高效率：** Contoso 必須移除不必要的程式，並簡化開發人員和使用者的流程。 企業需要快速且不浪費時間或金錢，以更快的速度提供客戶需求。
- **提高靈活性：** Contoso IT 必須能夠更快地回應企業的需求。 它必須能夠以比 marketplace 中的變更更快的速度來實現全球經濟的成功，而且不會成為商務封鎖程式。
- **調整：** 隨著企業順利成長，Contoso IT 必須提供能夠以相同步調成長的系統。
- **提高安全性：** Contoso 發現法規問題會使他們根據審核、記錄和合規性需求來調整其內部部署策略。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已將此遷移的目標固定在一起，並會使用它們來判斷最佳的遷移方法。

| 需求 | 詳細資料 |
| --- | --- |
| **升級** | Contoso 會想要確保他們在有安裝最新的修補程式時，但不想要管理這些更新。 |
| **整合** | Contoso 會想要將資料庫中的資料與用於 Machine Learning 的資料和 AI 管線整合。 |
| **備份與還原** | 當資料更新失敗或因為任何原因而損毀時，Contoso 會尋求還原時間點的功能。 |
| **Azure** | 他們想要能夠監視系統，並根據效能和安全性引發警示。 |
| **效能** | 在某些情況下，它們會在不同的地理區域中具有平行資料處理管線，且必須從這些區域讀取資料。 |

## <a name="solution-design"></a>解決方案設計

在將目標和需求固定之後，Contoso 會設計和審查部署解決方案，並識別遷移程式，包括用於遷移的工具和服務。

### <a name="current-environment"></a>目前的環境

于 postgresql 9.6.7 正在實體 Linux 機器上執行， (`sql-pg-01.contoso.com` 在 Contoso 資料中心內) 。 Contoso 已有 Azure 訂用帳戶，且具有站對站虛擬網路閘道可供內部部署資料中心網路使用。

### <a name="proposed-solution"></a>建議的解決方案

- 使用 Azure 資料庫移轉服務 o 將資料庫移轉至適用於 PostgreSQL 的 Azure 資料庫實例。
- 修改所有應用程式和進程，以使用新的適用於 PostgreSQL 的 Azure 資料庫實例。
- 使用連接至適用於 PostgreSQL 的 Azure 資料庫實例的 Azure Data Factory，建立新的資料處理管線。

### <a name="database-considerations"></a>資料庫考量

在解決方案設計過程中，Contoso 已在 Azure 中審查功能來裝載其于 postgresql 資料。 下列考慮可協助他們決定使用 Azure。

- 類似于 Azure SQL Database，適用於 PostgreSQL 的 Azure 資料庫支援防火牆規則。
- 適用於 PostgreSQL 的 Azure 資料庫可以與虛擬網路搭配使用，以防止實例可公開存取。
- 適用於 PostgreSQL 的 Azure 資料庫具有 Contoso 必須符合的必要合規性認證。
- 與 DevOps 和 Azure Data Factory 整合可讓您建立自動化資料處理管線。
- 您可以使用讀取複本來增強處理效能。
- 支援攜帶您自己的金鑰 (BYOK) 進行資料加密。
- 只能 (無公用存取) 使用私用連結，將服務公開到內部網路流量的能力。
- 從應用程式到資料庫的[頻寬和延遲](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)，將足以根據所選的閘道 (ExpressRoute 或站對站 VPN) 。

### <a name="solution-review"></a>解決方案檢閱

Contoso 會透過比較一份優缺點清單，來評估其建議設計。

| 考量 | 詳細資料 |
|--- | --- |
| **優點** | 目前所有必要和使用中的功能都可在適用於 PostgreSQL 的 Azure 資料庫中取得。 <br><br> |
| **缺點** | Contoso 仍然需要手動從于 postgresql 的主要版本進行遷移。 |

## <a name="proposed-architecture"></a>建議的架構

![案例架構 ](./media/contoso-migration-postgresql-to-azure/architecture.png)
 _圖1：案例架構。_

### <a name="migration-process"></a>移轉程序

#### <a name="preparation"></a>準備

在您可以遷移于 postgresql 資料庫之前，請確定這些實例符合成功遷移的所有 Azure 必要條件。

#### <a name="supported-versions"></a>支援的版本

僅支援遷移到相同或更高版本。 支援將于 postgresql 9.5 遷移至適用於 PostgreSQL 的 Azure 資料庫9.6 或10，但不支援從于 postgresql 11 遷移至於 postgresql 9.6。

Microsoft 的目標是在適用於 PostgreSQL 的 Azure 資料庫單一伺服器中支援_n-2_版的于 postgresql 引擎。 版本會是 Azure 上目前的主要版本 (_n_) 以及兩個先前的主要版本 (_-2_) 。

如需支援版本的最新更新，[請參閱這裡](https://docs.microsoft.com/azure/postgresql/concepts-supported-versions)。

> [!NOTE]
> 不支援自動主要版本升級。 例如，不會從于 postgresql 9.5 自動升級為于 postgresql 9.6。 若要升級至下一個主要版本，請傾印資料庫，並將它還原至以目標引擎版本建立的伺服器。

#### <a name="network"></a>網路

Contoso 必須設定從其內部部署環境到其適用於 PostgreSQL 的 Azure 資料庫資料庫所在之虛擬網路的虛擬網路閘道連線。 這可讓內部部署應用程式存取資料庫，但不能遷移至雲端。

#### <a name="assessment"></a>評量

Contoso 將需要針對複寫問題評估目前的資料庫。 這些問題包括：

- 源資料庫版本與遷移至目標資料庫版本相容。
- 請確定所有要複寫的資料表上都有主鍵。
- 資料庫名稱不能包含分號 (`;`) 。
- 以相同名稱但大小寫不同的多個資料表進行遷移，可能會導致無法預期的行為。

  ![遷移程式 ](./media/contoso-migration-postgresql-to-azure/migration-process.png)
   _圖2：遷移程式。_

#### <a name="migration"></a>遷移

Contoso 可以透過數種方式來執行遷移，包括：

- [傾印和還原](https://docs.microsoft.com/azure/postgresql/howto-migrate-using-dump-and-restore)

- [Azure 資料庫移轉服務](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online)

- [匯入/匯出](https://docs.microsoft.com/azure/postgresql/howto-migrate-using-export-and-import)

Contoso 已選取 Azure 資料庫移轉服務，可讓他們在需要執行主要升級時，重複使用遷移專案。 因為單一資料庫移轉服務活動最多隻能容納四個資料庫，所以 Contoso 會使用下列步驟來設定數個作業：

若要準備，請設定虛擬網路 (VNet) 來存取資料庫。 您可以使用[VPN 閘道](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)，以各種方式建立 VNet 連線。

<!-- docsTest:ignore "Azure Database Migration Services" -->

- 建立 Azure 資料庫移轉服務實例：
  - 在 [ [Azure 入口網站](https://portal.azure.com)中，選取 [**新增資源**]。
  - 搜尋**Azure 資料庫移轉服務**並加以選取。
  - 選取 [+ 新增]。
  - 選取服務的 [訂用帳戶] 和 [資源群組]。
  - 輸入實例的名稱。
  - 選取最接近您資料中心或 VPN 閘道的位置。
  - 針對服務模式選取 [ **Azure** ]。
  - 選取定價層。
  - 選取 [檢閱 + 建立]。

    ![遷移程式 ](./media/contoso-migration-postgresql-to-azure/azure_migration_service_create.png)
     _圖3：審查和建立。_

  - 選取 [建立]。

- 建立適用於 PostgreSQL 的 Azure 資料庫實例。

- 在內部部署伺服器上，設定檔案 `postgresql.conf` 。

  - 將伺服器設定為接聽 Azure 資料庫移轉服務將用來存取伺服器和資料庫的適當 IP 位址。
    - 設定 `listen_addresses` 變數。
  - 啟用 SSL。
    - 設定 `ssl=on` 變數。
    - 請確認您使用的是公開簽署的 SSL 憑證，適用于支援 TLS 1.2 的伺服器。 否則，資料庫移轉服務工具會引發錯誤。
  - 更新 `pg_hba.conf` 檔案。
    - 加入資料庫移轉服務實例特定的專案。
  - 您必須針對每部伺服器修改檔案中的值，以在來源伺服器上啟用邏輯複寫 `postgresql.conf` 。
    - `wal_level` = `logical`
    - `max_replication_slots`= [至少要遷移的資料庫數目上限]
      - 例如，如果您想要遷移四個資料庫，請將值設定為4。
    - `max_wal_senders`= [同時執行的資料庫數目]
      - 建議的值是10。

- 遷移 `User` 必須擁有 `REPLICATION` 源資料庫的角色。

- 將資料庫移轉服務實例的 IP 位址新增至檔案 `PostgreSQLpg_hba.conf` 。

- 匯出資料庫架構。

  - 執行下列命令：

    ```cmd
    pg_dump -U postgres -s dvdrental > dvdrental_schema.sql
    ```

  - 複製檔案、將複本命名為 `dvdrental_schema_foreign.sql` ，然後移除所有非外鍵和觸發程式相關的專案。
  - 從檔案中移除所有外鍵和觸發程式相關的專案 `dvdrental_schema.sql` 。

- 匯入資料庫架構 (步驟 1) ：

  ```cmd
    psql -h {host}.postgres.database.azure.com -d dvdrental -U username -f dvdrental_schema.sql
    ```

- 移轉：

  - 在 [Azure 入口網站中，流覽至您的資料庫移轉服務資源。
  - 如果服務未啟動，請選取 [**啟動服務**]。
  - 選取 [新增] [**遷移專案**]。

    ![新的遷移專案會反白顯示 [ ](./media/contoso-migration-postgresql-to-azure/azure_migration_service_new_project.png)
     _圖 4]：開始新的遷移。_

  - 選取 [**新增活動**] [  >  **線上資料移轉**]。
  - 輸入名稱。
  - 選取 [**于 postgresql** ] 作為來源。
  - 針對 [目標]，選取 [**適用於 PostgreSQL 的 Azure 資料庫**]。

    ![新的遷移專案會反白顯示 [ ](./media/contoso-migration-postgresql-to-azure/azure_migration_service_new_project02.png)
     _圖 5]：新的遷移專案會反白顯示。_

  - 選取 [Save] \(儲存\)。
  - 輸入來源資訊。

    ![新的遷移專案會反白顯示 [ ](./media/contoso-migration-postgresql-to-azure/azure_migration_service_source.png)
     _圖 6]：輸入來源資訊。_

  - 選取 [Save] \(儲存\)。
  - 輸入目標資訊。

    ![新的遷移專案會反白顯示 [ ](./media/contoso-migration-postgresql-to-azure/azure_migration_service_target.png)
     _圖7：選取目標資訊]。_

  - 選取 [Save] \(儲存\)。
  - 選取您想要遷移的資料庫。 先前應已遷移每個資料庫的架構。

    ![新的遷移專案會反白顯示 [ ](./media/contoso-migration-postgresql-to-azure/azure_migration_service_db.png)
     _圖 8]：選取 [資料庫]。_

  - 選取 [Save] \(儲存\)。
  - 設定 [advanced] 設定。

    ![新的遷移專案會反白顯示 [ ](./media/contoso-migration-postgresql-to-azure/azure_migration_service_advanced.png)
     _圖9：設定 advanced settings]。_

  - 選取 [Save] \(儲存\)。
  - 提供活動的名稱，然後選取 [**執行**]。

    ![新的遷移專案會反白顯示 ](./media/contoso-migration-postgresql-to-azure/azure_migration_service_summary.png)
     _[圖 10]：命名和執行活動。_

  - 監視移轉。 如果發生任何錯誤，您可能需要重試 (例如，如果缺少外鍵參考) 。
  - 一旦 `Full load completed` 符合您的資料表計數，請選取 [**開始**切換]。

    ![新的遷移專案會反白顯示 ](./media/contoso-migration-postgresql-to-azure/azure_migration_service_complete.png)
     _[圖11：監視遷移以開始_轉換]。

  - 停止來源伺服器的所有交易。
  - 選取 [**確認**] 核取方塊，然後選取 [套用 **]。**

    ![新的遷移專案會反白顯示 ](./media/contoso-migration-postgresql-to-azure/azure_migration_service_cutover.png)
     _[圖12：執行正在切換的]。_

  - 等待轉換完成。

    ![新的遷移專案會反白顯示 ](./media/contoso-migration-postgresql-to-azure/azure_migration_service_finished.png)
     _[圖 13]：正在完成轉換。_

  > [!NOTE]
  > 上述的資料庫移轉服務步驟也可以透過[Azure CLI](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online)來執行。

- 匯入資料庫架構 (步驟 2) ：

  ```cmd
    psql -h {host}.postgres.database.azure.com -d dvdrental -U username -f dvdrental_schema_foreign.sql
    ```

- 重新設定任何使用內部部署資料庫的應用程式或進程，以指向新的適用於 PostgreSQL 的 Azure 資料庫資料庫實例。

- 針對後續遷移，請確定您也已在完成遷移之後，設定跨區域讀取複本（如有需要）。

## <a name="clean-up-after-migration"></a>移轉之後進行清除

遷移之後，Contoso 必須備份內部部署資料庫以進行保留，並在清除程式中淘汰舊的于 postgresql 伺服器。

## <a name="review-the-deployment"></a>檢閱部署

對於 Azure 中的移轉後資源，Contoso 必須能執行一切功能並保護其新的基礎結構。

### <a name="security"></a>安全性

- Contoso 必須確保其新的適用於 PostgreSQL 的 Azure 資料庫實例和資料庫都是安全的。 [深入了解](https://docs.microsoft.com/azure/postgresql/concepts-security)。
- Contoso 應檢查[防火牆規則](https://docs.microsoft.com/azure/postgresql/concepts-firewall-rules)和虛擬網路設定，以確認連線僅限於需要它的應用程式。
- 針對資料加密執行[BYOK](https://docs.microsoft.com/azure/postgresql/concepts-data-encryption-postgresql) 。
- 更新所有應用程式，以要求連接到資料庫的[SSL](https://docs.microsoft.com/azure/postgresql/concepts-ssl-connection-security) 。
- 設定[私用連結](https://docs.microsoft.com/azure/postgresql/concepts-data-access-and-security-private-link)，讓所有資料庫流量保留在 Azure 和內部部署網路中。
- 啟用[Azure 進階威脅防護 (ATP) ](https://docs.microsoft.com/azure/postgresql/concepts-data-access-and-security-threat-protection)。
- 設定 Log Analytics 以監視及警示安全性，並記錄相關的專案。

### <a name="backups"></a>備份

請確定使用異地還原來備份適用於 PostgreSQL 的 Azure 資料庫資料庫。 這可讓您在區域中斷時，在配對的區域中使用備份。

> [!IMPORTANT]
> 請確定適用於 PostgreSQL 的 Azure 資料庫資源有資源鎖定，以防止它被刪除。 已刪除的伺服器無法還原。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- 適用於 PostgreSQL 的 Azure 資料庫可以向上或向下調整。 伺服器和資料庫的效能監視非常重要，可確保您符合需求，同時保持最低成本。
- CPU 和儲存體都有相關聯的成本。 有數個定價層可供選擇。 請確定已針對資料工作負載選取適當的價格方案。
- 每個讀取複本都會根據所選的計算和儲存體來計費。

## <a name="conclusion"></a>結論

在本文中，Contoso 會將其于 postgresql 資料庫移轉至適用於 PostgreSQL 的 Azure 資料庫實例。
