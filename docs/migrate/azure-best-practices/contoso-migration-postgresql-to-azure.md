---
title: 將于 postgresql 資料庫移轉至 Azure
description: 瞭解 Contoso 如何將其內部部署于 postgresql 資料庫移轉至 Azure。
author: deltadan
ms.author: abuck
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 9f9dfee1aca21acbbf0f840b79d61501ad90c73a
ms.sourcegitcommit: 8b82889dca0091f3cc64116f998a3a878943c6a1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/09/2020
ms.locfileid: "89603864"
---
<!-- cSpell:ignore BYOK postgres psql dvdrental vpngateways -->

# <a name="migrate-postgresql-databases-to-azure"></a>將于 postgresql 資料庫移轉至 Azure

本文示範虛構公司 Contoso 如何規劃和遷移其內部部署于 postgresql 開放原始碼資料庫平臺到 Azure。

## <a name="business-drivers"></a>商業動機

IT 領導小組與商務合作夥伴密切合作，瞭解他們想要使用這種方式達成什麼目標。 他們想要：

- **將大型資料自動化。** Contoso 會使用於 postgresql 來進行數個大型資料和 AI 計畫。 該公司想要建立可擴充的可重複管線，將許多分析工作負載自動化。
- **提高效率。** Contoso 必須移除不必要的程式，並簡化開發人員和使用者的流程。 企業需要快速且不浪費時間或金錢，以更快的速度提供客戶的需求。
- **增加靈活性。** Contoso IT 必須能夠更快因應企業的需求。 它必須比 marketplace 中的變更更快，以實現全球經濟的成功，而且不會成為企業封鎖程式。
- **規模。** 隨著企業順利成長，Contoso IT 必須提供可依相同步調成長的系統。
- **提高安全性。** Contoso 發現法規問題會讓公司根據審核、記錄和合規性需求來調整其內部部署策略。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已將此遷移的目標釘選出來，並將使用它們來決定最佳的遷移方法。

| 需求 | 詳細資料 |
| --- | --- |
| **升級** | Contoso 想要確保在有最新的修補程式可供使用時，但該公司不想管理這些更新。 |
| **整合** | Contoso 想要將資料庫中的資料與適用于機器學習的資料和 AI 管線整合。 |
| **備份與還原** | Contoso 正在尋找在資料更新失敗或因任何原因而損毀時進行時間點還原的能力。 |
| **Azure** | Contoso 想要監視系統，並根據效能和安全性引發警示。 |
| **效能** | 在某些情況下，Contoso 會在不同的地理區域中有平行的資料處理管線，而且必須從這些區域讀取資料。 |

## <a name="solution-design"></a>解決方案設計

在達到目標和需求後，Contoso 會設計和審核部署解決方案，並識別遷移程式。 此外，也會識別用於遷移的工具和服務。

### <a name="current-environment"></a>目前的環境

于 postgresql 9.6.7 是在 Contoso 資料中心 () 的實體 Linux 機器上執行 `sql-pg-01.contoso.com` 。 Contoso 已經有一個具有站對站虛擬網路閘道的 Azure 訂用帳戶，可使用內部部署資料中心網路。

### <a name="proposed-solution"></a>建議的解決方案

- 使用 Azure 資料庫移轉服務，將資料庫移轉至適用於 PostgreSQL 的 Azure 資料庫實例。
- 修改所有的應用程式和進程，以使用新的適用於 PostgreSQL 的 Azure 資料庫實例。
- 使用連接至適用於 PostgreSQL 的 Azure 資料庫實例的 Azure Data Factory 來建立新的資料處理管線。

### <a name="database-considerations"></a>資料庫考量

在解決方案設計過程中，Contoso 會檢查 Azure 中裝載其于 postgresql 資料的功能。 下列考慮有助於公司決定使用 Azure：

- 類似于 Azure SQL Database，適用於 PostgreSQL 的 Azure 資料庫支援防火牆規則。
- 適用於 PostgreSQL 的 Azure 資料庫可以搭配虛擬網路使用，以防止實例可公開存取。
- 適用於 PostgreSQL 的 Azure 資料庫具有 Contoso 必須符合的必要合規性認證。
- 與 DevOps 和 Azure Data Factory 的整合可讓您建立自動化的資料處理管線。
- 使用讀取複本可以增強處理效能。
- 支援將您自己的金鑰用於資料加密 (BYOK) 。
- 只能將服務公開給內部網路流量的能力， (使用 Azure Private Link 的無公用存取) 。
- 從應用程式到資料庫的 [頻寬和延遲](/azure/vpn-gateway/vpn-gateway-about-vpngateways) ，會根據所選的閘道而足夠， (Azure ExpressRoute 或站對站 VPN) 。

### <a name="solution-review"></a>解決方案檢閱

Contoso 藉由結合一份優缺點來評估其提議的設計。

| 考量 | 詳細資料 |
|--- | --- |
| **優點** | 目前所有必要和使用中的功能都可以在適用於 PostgreSQL 的 Azure 資料庫中使用。 <br><br> |
| **缺點** | Contoso 仍需要手動從于 postgresql 的主要版本進行遷移。 |

## <a name="proposed-architecture"></a>建議的架構

![案例架構的圖表。](./media/contoso-migration-postgresql-to-azure/architecture.png)

_圖1：案例架構。_

### <a name="migration-process"></a>移轉程序

#### <a name="preparation"></a>準備

在 Contoso 可以遷移其于 postgresql 資料庫之前，可確保 Contoso 的實例符合所有 Azure 必要條件，以進行成功的遷移。

#### <a name="supported-versions"></a>支援的版本

只支援遷移至相同或更高的版本。 支援將于 postgresql 9.5 遷移至適用於 PostgreSQL 的 Azure 資料庫9.6 或10，但不支援從于 postgresql 11 遷移至於 postgresql 9.6。

Microsoft 的目標是支援適用於 PostgreSQL 的 Azure 資料庫單一伺服器中的 _n-2_ 版于 postgresql 引擎。 這些版本會是目前在 Azure 上的主要版本 (_n_) 和兩個先前的主要版本 (_-2_) 。

如需支援版本的最新更新，請參閱 [支援的于 postgresql 主要版本](/azure/postgresql/concepts-supported-versions)。

> [!NOTE]
> 不支援自動主要版本升級。 例如，不會自動從于 postgresql 9.5 升級至於 postgresql 9.6。 若要升級至下一個主要版本，請傾印資料庫並將它還原到使用目標引擎版本建立的伺服器。

#### <a name="network"></a>網路

Contoso 必須將虛擬網路閘道連線從其內部部署環境設定為其適用於 PostgreSQL 的 Azure 資料庫資料庫所在的虛擬網路。 此連接可讓內部部署應用程式存取資料庫，但不能遷移至雲端。

#### <a name="assessment"></a>評量

Contoso 將需要評估目前的資料庫是否有複寫問題。 這些問題包括：

- 源資料庫版本與遷移至目標資料庫版本相容。
- 主鍵必須存在於所有要複寫的資料表。
- 資料庫名稱不能包含分號 (`;`) 。
- 使用相同名稱來遷移多個資料表，但不同的情況可能會導致無法預期的行為。

  ![遷移程式的圖表。 ](./media/contoso-migration-postgresql-to-azure/migration-process.png)
  _圖2：遷移程式。_

#### <a name="migration"></a>移轉

Contoso 可以透過數種方式來執行遷移：

- [傾印和還原](/azure/postgresql/howto-migrate-using-dump-and-restore)
- [Azure 資料庫移轉服務](/azure/dms/tutorial-postgresql-azure-postgresql-online)
- [匯入/匯出](/azure/postgresql/howto-migrate-using-export-and-import)

Contoso 已選取 Azure 資料庫移轉服務，可讓公司在需要執行主要到主要升級時，重複使用遷移專案。 由於單一資料庫移轉服務活動最多隻容納四個資料庫，Contoso 會使用下列步驟來設定數個工作。

若要準備，請設定虛擬網路來存取資料庫。 使用 [VPN 閘道](/azure/vpn-gateway/vpn-gateway-about-vpngateways) 以各種方式建立虛擬網路連線。

### <a name="create-an-azure-database-migration-service-instance"></a>建立 Azure 資料庫移轉服務執行個體

1. 在 [ [Azure 入口網站](https://portal.azure.com)中，選取 [ **新增資源**]。
1. 搜尋 **Azure 資料庫移轉服務**，然後選取它。
1. 選取 [+ 新增] 。
1. 選取服務的訂用帳戶和資源群組。
1. 輸入實例的名稱。
1. 選取最接近 Contoso 資料中心或 VPN 閘道的位置。
1. 選取 [ **Azure** ] 作為服務模式。
1. 選取定價層。
1. 選取 [檢閱 + 建立]。

    ![[建立遷移服務] 畫面的螢幕擷取畫面。](./media/contoso-migration-postgresql-to-azure/azure_migration_service_create.png)
    _圖3：複習和建立。_

1. 選取 [建立]。

### <a name="create-an-azure-database-for-postgresql-instance"></a>建立「適用於 PostgreSQL 的 Azure 資料庫」執行個體

1. 在內部部署伺服器上，設定檔案 `postgresql.conf` 。

1. 將伺服器設定為接聽 Azure 資料庫移轉服務將用來存取伺服器和資料庫的適當 IP 位址。
    - 設定 `listen_addresses` 變數。
1. 啟用 SSL。
    1. 設定 `ssl=on` 變數。
    1. 確認 Contoso 針對支援 TLS 1.2 的伺服器使用公開簽署的 SSL 憑證。 否則，資料庫移轉服務工具將會引發錯誤。
1. 更新 `pg_hba.conf` 檔案。
    - 加入資料庫移轉服務實例特定的專案。
1. 您必須在來源伺服器上，藉由修改每部伺服器的檔案值來啟用邏輯複寫 `postgresql.conf` 。
    1. `wal_level` = `logical`
    1. `max_replication_slots` = [最多可遷移的資料庫數目]
        - 例如，如果 Contoso 想要遷移四個資料庫，則會將值設定為4。
    1. `max_wal_senders` = [同時執行的資料庫數目]
        - 建議值為10。

1. 遷移 `User` 必須具有 `REPLICATION` 源資料庫的角色。

1. 將資料庫移轉服務實例 IP 位址新增至檔案 `PostgreSQLpg_hba.conf` 。

1. 若要匯出資料庫架構，請執行下列命令：

    ```cmd
    pg_dump -U postgres -s dvdrental > dvdrental_schema.sql
    ```

1. 複製檔案、將複本命名為 `dvdrental_schema_foreign.sql` ，然後移除所有非外鍵和觸發程式相關專案。
1. 從檔案中移除所有的外鍵和觸發程式相關專案 `dvdrental_schema.sql` 。

1. 匯入資料庫架構 (步驟 1) ：

      ```cmd
        psql -h {host}.postgres.database.azure.com -d dvdrental -U username -f dvdrental_schema.sql
      ```

### <a name="migration"></a>移轉

1. 在 Azure 入口網站中，Contoso 會移至其資料庫移轉服務資源。
1. 如果服務未啟動，請選取 [ **啟動服務**]。
1. 選取 [ **新增遷移專案**]。

    ![顯示反白顯示 [新增遷移專案] 選項的螢幕擷取畫面。](./media/contoso-migration-postgresql-to-azure/azure_migration_service_new_project.png)

    _圖4：啟動新的遷移。_

1. 選取**New Activity**[  >  **線上資料移轉**] 新活動。
1. 輸入名稱。
1. 選取 [ **于 postgresql** ] 作為來源。
1. 在 [目標] 中，選取 [ **適用於 PostgreSQL 的 Azure 資料庫** ]，然後選取 [ **儲存**]。

    ![顯示 [新增遷移專案] 窗格的螢幕擷取畫面。](./media/contoso-migration-postgresql-to-azure/azure_migration_service_new_project02.png)

    _圖5：已反白顯示新的遷移專案。_

1. 輸入來源資訊，然後選取 [ **儲存**]。

    ![顯示輸入來源資訊的螢幕擷取畫面。](./media/contoso-migration-postgresql-to-azure/azure_migration_service_source.png)
    _圖6：輸入來源資訊。_

1. 輸入目標資訊，然後選取 [ **儲存**]。

    ![顯示選取目標資訊的螢幕擷取畫面。](./media/contoso-migration-postgresql-to-azure/azure_migration_service_target.png)
    _圖7：選取目標資訊。_

1. 選取要遷移的資料庫。 每個資料庫的架構都應該已在先前遷移。 然後選取 [儲存]。

    ![顯示選取資料庫的螢幕擷取畫面。](./media/contoso-migration-postgresql-to-azure/azure_migration_service_db.png)
    _圖8：選取資料庫。_

1. 設定 [advanced] 設定，然後選取 [ **儲存**]。

    ![顯示設定 [advanced settings] 的螢幕擷取畫面。](./media/contoso-migration-postgresql-to-azure/azure_migration_service_advanced.png)
    _圖9：設定 advanced settings。_

1. 提供活動的名稱，然後選取 [ **執行**]。

    ![顯示命名和執行活動的螢幕擷取畫面。](./media/contoso-migration-postgresql-to-azure/azure_migration_service_summary.png)
    _圖10：命名和執行活動。_

1. 監視移轉。 如果發生任何失敗，請重試。 例如，如果缺少外鍵參考，則為。
1. `Full load completed`符合資料表計數之後，請選取 [**開始**轉換]。

    ![顯示監視以開始轉換的螢幕擷取畫面。](./media/contoso-migration-postgresql-to-azure/azure_migration_service_complete.png)
    _圖11：監視遷移以開始轉換。_

1. 停止來源伺服器中的所有交易。
1. 選取 [**確認**] 核取方塊，然後選取 [套用 **]。**

    ![顯示正在執行轉換的螢幕擷取畫面。](./media/contoso-migration-postgresql-to-azure/azure_migration_service_cutover.png)
    _圖12：執行轉換。_

1. 等候轉換完成。

    ![顯示完成轉換的螢幕擷取畫面。](./media/contoso-migration-postgresql-to-azure/azure_migration_service_finished.png)
    _圖13：正在完成轉換。_

      > [!NOTE]
      > 先前的資料庫移轉服務步驟也可以透過 [Azure CLI](/azure/dms/tutorial-postgresql-azure-postgresql-online)執行。

1. 匯入資料庫架構 (步驟 2) ：

      ```cmd
        psql -h {host}.postgres.database.azure.com -d dvdrental -U username -f dvdrental_schema_foreign.sql
      ```

1. 重新設定任何使用內部部署資料庫的應用程式或進程，以指向新的適用於 PostgreSQL 的 Azure 資料庫資料庫實例。

1. 在遷移後，Contoso 會在完成遷移之後，確保它也會設定跨區域讀取複本（如有必要）。

## <a name="clean-up-after-migration"></a>移轉之後進行清除

遷移之後，Contoso 需要備份內部部署資料庫以進行保留，並在清除過程中淘汰舊的于 postgresql 伺服器。

## <a name="review-the-deployment"></a>檢閱部署

對於 Azure 中的遷移後資源，Contoso 必須能執行一切功能並保護其新的基礎結構。

### <a name="security"></a>安全性

Contoso 必須：

- 確定于 postgresql 實例和資料庫的新 Azure 資料庫是安全的。 如需詳細資訊，請參閱 [適用於 PostgreSQL 的 Azure 資料庫-單一伺服器中的安全性](/azure/postgresql/concepts-security)。
- 請檢查 [防火牆規則](/azure/postgresql/concepts-firewall-rules) 和虛擬網路設定，以確認連線只限于需要它的應用程式。
- 針對資料加密執行 [BYOK](/azure/postgresql/concepts-data-encryption-postgresql) 。
- 更新所有應用程式，以要求對資料庫進行 [SSL](/azure/postgresql/concepts-ssl-connection-security) 連接。
- 設定 [Private Link](/azure/postgresql/concepts-data-access-and-security-private-link) ，以便在 Azure 和內部部署網路內保存所有資料庫流量。
- 啟用 [Azure 進階威脅防護 (ATP) ](/azure/postgresql/concepts-data-access-and-security-threat-protection)。
- 將 Log Analytics 設定為監視安全性以及相關記錄專案的警示。

### <a name="backups"></a>備份

請確定使用異地還原備份適用於 PostgreSQL 的 Azure 資料庫資料庫。 如此一來，在發生區域性中斷的情況下，備份可以在配對的區域中使用。

> [!IMPORTANT]
> 請確定適用於 PostgreSQL 的 Azure 資料庫資源具有資源鎖定，以防止其遭到刪除。 無法還原已刪除的伺服器。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- 適用於 PostgreSQL 的 Azure 資料庫可以向上或向下調整。 伺服器和資料庫的效能監視很重要，可確保符合需求，同時維持最少的成本。
- CPU 和儲存體都有相關聯的成本。 有幾個定價層可供選擇。 請確定已為數據工作負載選取適當的定價方案。
- 每個讀取複本會根據選取的計算和儲存體計費。

## <a name="conclusion"></a>結論

在本文中，Contoso 會將其于 postgresql 資料庫移轉至適用於 PostgreSQL 的 Azure 資料庫實例。
