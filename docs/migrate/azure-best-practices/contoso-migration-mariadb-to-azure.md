---
title: 將適用于 mariadb 資料庫移轉至 Azure
description: 瞭解 Contoso 如何將其內部部署適用于 mariadb 資料庫移轉至 Azure。
author: deltadan
ms.author: abuck
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: a96c4afaa31a7cc399cee0e249e04348623e847d
ms.sourcegitcommit: c1d6c1c777475f92a3f8be6def84f1779648a55c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/21/2020
ms.locfileid: "92334675"
---
<!-- cSpell:ignore mysqldump -->

# <a name="migrate-mariadb-databases-to-azure"></a>將適用于 mariadb 資料庫移轉至 Azure

本文示範虛構公司 Contoso 如何規劃和遷移其內部部署適用于 mariadb 開放原始碼資料庫平臺到 Azure。

Contoso 會使用適用于 mariadb 而不是 MySQL，因為其：

- 許多儲存引擎選項。
- 快取和索引效能。
- 具有功能和擴充功能的開放原始碼支援。
- 分析工作負載的資料行存放區引擎。

公司的遷移目標是繼續使用適用于 mariadb，但不必擔心如何管理支援它所需的環境。

## <a name="business-drivers"></a>商業動機

IT 領導小組與商務合作夥伴密切合作，瞭解他們想要使用這種方式達成什麼目標。 他們想要：

- **提高可用性。** Contoso 在其適用于 mariadb 內部部署環境中有可用性問題。 企業要求使用此資料存放區的應用程式更可靠。
- **提高效率。** Contoso 必須移除不必要的程式，並簡化開發人員和使用者的流程。 企業需要快速且不浪費時間或金錢，以更快的速度提供客戶的需求。
- **增加靈活性。** Contoso IT 必須能夠更快因應企業的需求。 它必須比 marketplace 中的變更更快，以實現全球經濟的成功。 它不得取得或成為企業封鎖程式。
- **規模。** 隨著企業順利成長，Contoso IT 必須提供以相同步調成長的系統。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已針對此次移轉擬定好各項目標。 並用這些目標用來判斷最合適的移轉方法。

| 需求 | 詳細資料 |
| --- | --- |
| **可用性** | 目前，內部員工對適用于 mariadb 實例的裝載環境有很難的時間。 Contoso 希望資料庫層有接近99.99% 的可用性。 |
| **延展性** | 內部部署資料庫主機的容量很快地用盡。 Contoso 需要一種方法來調整超過目前限制的實例，或在商務環境變更時縮小以節省成本。 |
| **效能** | Contoso 人力資源 (HR) 部門有數個報表，每日、每週和每月執行一次。 當它執行這些報表時，會注意到與員工面向的應用程式有相當大的效能問題。 它需要執行報表，而不會影響應用程式效能。 |
| **Security** | Contoso 必須知道資料庫只能供其內部應用程式存取，而且無法透過網際網路顯示或存取。 |
| **監視** | Contoso 目前使用工具來監視適用于 mariadb 資料庫的計量，並在 CPU、記憶體或儲存體發生問題時提供通知。 公司想要在 Azure 中擁有相同的功能。 |
| **業務持續性** | HR 資料存放區是 Contoso 日常營運的重要部分。 如果它損毀或需要還原，公司想要將停機時間降到最低。 |
| **Azure** | Contoso 想要將應用程式移至 Azure，而不在 Vm 上執行。 Contoso 需求狀態為使用 Azure 平臺即服務 (適用于資料層的 PaaS) 服務。 |

## <a name="solution-design"></a>解決方案設計

在達到目標和需求後，Contoso 會設計和審核部署解決方案，並識別遷移程式。 此外，也會識別用於遷移的工具和服務。

### <a name="current-application"></a>目前的應用程式

適用于 mariadb 資料庫主控員工資料，用於公司人力資源部門的所有層面。 以 [燈泡為基礎](https://wikipedia.org/wiki/LAMP_(software_bundle)) 的應用程式是用來處理員工 HR 要求的前端。 Contoso 的全球員工為100000，因此其資料庫的執行時間相當重要。

### <a name="proposed-solution"></a>建議的解決方案

- 評估環境的遷移相容性。
- 使用常見的開放原始碼工具，將資料庫移轉至適用於 MariaDB 的 Azure 資料庫實例。
- 修改所有的應用程式和進程，以使用新的適用於 MariaDB 的 Azure 資料庫實例。

### <a name="database-considerations"></a>資料庫考量

在解決方案設計過程中，Contoso 會檢查 Azure 中裝載其適用于 mariadb 資料庫的功能。 下列考慮有助於公司決定使用 Azure：

- 類似于 Azure SQL Database，適用於 MariaDB 的 Azure 資料庫允許 [防火牆規則](/azure/mariadb/concepts-firewall-rules)。
- 適用於 MariaDB 的 Azure 資料庫可以搭配 [Azure 虛擬網路](/azure/mariadb/concepts-data-access-security-vnet) 使用，以防止實例可公開存取。
- 適用於 MariaDB 的 Azure 資料庫具有 Contoso 針對其審計員必須符合的必要合規性和隱私權認證。
- 報表和應用程式處理效能將會使用讀取複本來增強。
- 只能將服務公開給內部網路流量的能力， (使用 [Azure Private Link](/azure/mariadb/concepts-data-access-security-private-link)的無公用存取) 。
- Contoso 選擇不移至適用於 MySQL 的 Azure 資料庫，因為未來可能會使用適用于 mariadb 資料行存放區和圖形資料庫模型。
- 從應用程式到資料庫的 [頻寬和延遲](/azure/vpn-gateway/vpn-gateway-about-vpngateways) ，會根據所選的閘道而足夠， (Azure ExpressRoute 或站對站 VPN) 。

### <a name="solution-review"></a>解決方案檢閱

Contoso 會透過比較一份優缺點清單，來評估建議設計。

| 考量 | 詳細資料 |
| --- | --- |
| **優點** | 適用於 MariaDB 的 Azure 資料庫提供99.99% 的財務支援服務等級協定， (SLA) 以獲得 [高可用性](/azure/mariadb/concepts-high-availability)。 <br><br> Azure 可讓您在每季的尖峰負載期間擴大或縮小。 Contoso 可以購買 [保留容量](/azure/mariadb/concept-reserved-pricing)來節省更多成本。 <br><br> Azure 提供適用於 MariaDB 的 Azure 資料庫的時間點還原和異地還原功能。 <br><br> |
| **缺點** | Contoso 受限於 Azure 中支援的適用于 mariadb 發行版本，目前為10.2 和10.3。 <br><br> 適用於 MariaDB 的 Azure 資料庫有一些 [限制](/azure/mariadb/concepts-limits)，例如調整儲存體。 |

## <a name="proposed-architecture"></a>建議的架構

![圖表顯示案例架構。 ](./media/contoso-migration-mariadb-to-azure/architecture.png)
_圖1：案例架構。_

### <a name="migration-process"></a>移轉程序

#### <a name="preparation"></a>準備

在遷移您的適用于 mariadb 資料庫之前，您必須確定這些實例符合所有 Azure 必要條件，以進行成功的遷移。

支援的版本：

- 適用于 mariadb 會使用 x.x.x.x 命名配置。 例如，x 是主要版本，y 是次要版本，而 z 是修補程式版本。
- Azure 目前支援10.2.25 和10.3.16。
- Azure 會自動管理修補程式更新的升級。 範例10.2.21 至10.2.23。 不支援次要和主要版本升級。 例如，不支援從 MariaDB 10.2 升級至 MariaDB 10.3。 如果您想要從10.2 升級為10.3，請取得資料庫傾印，並將其還原至使用目標引擎版本建立的伺服器。

網路：

Contoso 必須將虛擬網路閘道連線從其內部部署環境設定為其適用于 mariadb 資料庫所在的虛擬網路。 此連接可讓內部部署應用程式在連接字串更新時，透過閘道存取資料庫。

  ![圖表顯示遷移程式。](./media/contoso-migration-mariadb-to-azure/migration-process.png)
  _圖2：遷移程式。_

#### <a name="migration"></a>移轉

由於適用于 mariadb 類似于 MySQL，因此 Contoso 可以使用相同的常用公用程式和工具（例如 MySQL 工作臺、mysqldump、Toad 或 Navicat）來連線到適用於 MariaDB 的 Azure 資料庫，並將資料移轉至。

Contoso 會使用下列步驟來遷移其資料庫。

- 藉由執行下列命令並觀察輸出，來判斷內部部署適用于 mariadb 版本。 在大部分的情況下，您的版本在架構和資料傾印方面都不會有太大的影響。 如果您是在應用層級使用功能，請確定這些應用程式與 Azure 中的目標版本相容。

  ```cmd
    mysql -h localhost -u root -P
  ```

  ![螢幕擷取畫面顯示如何判斷內部部署的適用于 mariadb 版本。 ](./media/contoso-migration-mariadb-to-azure/mariadb_version.png)
  _圖3：判斷內部部署的適用于 mariadb 版本。_

- 在 Azure 中建立新的適用于 mariadb 實例：

  - 開啟 [Azure 入口網站](https://portal.azure.com)。
  - 選取 [ **新增資源**]。
  - 搜尋 `MariaDB`。

    ![螢幕擷取畫面顯示 Azure 中的新適用于 mariadb 實例。 ](./media/contoso-migration-mariadb-to-azure/azure-mariadb-create.png)
    _圖4： Azure 中的新適用于 mariadb 實例。_

  - 選取 [建立]。
  - 選取您的訂用帳戶和資源群組。
  - 選取伺服器名稱和位置。
  - 選取您的目標版本，也就是10.2 或10.3。
  - 選取您的計算和儲存體。
  - 輸入系統管理員使用者名稱和密碼。
  - 選取 [檢閱 + 建立]。

    ![螢幕擷取畫面顯示 [建立適用于 mariadb 伺服器] 畫面。 ](./media/contoso-migration-mariadb-to-azure/azure_mariadb_create.png)
    _圖5：檢查和建立。_

  - 選取 [建立]。
  - 記錄伺服器主機名稱、使用者名稱和密碼。
  - 選取 [ **連接安全性**]。
  - 選取 [ **新增用戶端 ip** ] (您將從) 還原資料庫的 ip。
  - 選取 [儲存]。

- 執行下列命令，以匯出名為的資料庫 `Employees` 。 針對每個資料庫重複執行：

    ```cmd
    mysqldump -h localhost -u root -p -–skip-triggers -–single-transaction –-extended-insert -–order-by-primary -–disable-keys Employees > Employees.sql
    ```

- 還原資料庫。 以適用於 MariaDB 的 Azure 資料庫實例和使用者名稱的端點取代：

  ```cmd
  mysql -h {name}.mariadb.database.azure.com -u user@{name} -p –ssl
  create database employees;
  use database employees;
  source employees.sql;
  ```

- 使用 phpMyAdmin 或類似的工具（例如 MySQL 工作臺、Toad 和 Navicat），藉由檢查每個資料表中的記錄計數來確認還原。
- 更新所有應用程式連接字串，以指向已遷移的資料庫。
- 測試所有應用程式是否正常運作。

## <a name="clean-up-after-migration"></a>移轉之後進行清除

經過驗證成功的遷移之後，Contoso 需要備份並儲存內部部署資料庫備份檔案，以供保留之用。 淘汰內部部署適用于 mariadb 伺服器。

## <a name="review-the-deployment"></a>檢閱部署

對於 Azure 中的遷移後資源，Contoso 必須能執行一切功能並保護其新的基礎結構。

### <a name="security"></a>安全性

Contoso 必須：

- 確定其新的適用於 MariaDB 的 Azure 資料庫實例和資料庫都是安全的。 如需詳細資訊，請參閱 [適用於 MariaDB 的 Azure 資料庫中的安全性](/azure/mariadb/concepts-security)。
- 請檢查 [防火牆規則](/azure/mariadb/concepts-firewall-rules) 和虛擬網路設定，以確認連線只限于需要它的應用程式。
- 設定任何輸出 IP 需求，以允許連線至適用于 mariadb [閘道 IP 位址](/azure/mariadb/concepts-connectivity-architecture)。
- 更新所有應用程式，以要求對資料庫進行 [SSL](/azure/mariadb/concepts-ssl-connection-security) 連接。
- 設定 [Private Link](/azure/mariadb/concepts-data-access-security-private-link) ，以便在 Azure 和內部部署網路內保存所有資料庫流量。
- 啟用 [Azure 進階威脅防護 (ATP) ](/azure/mariadb/concepts-data-access-and-security-threat-protection)。
- 設定 Log Analytics 來監視和傳送有關安全性的警示以及相關的記錄專案。

### <a name="backups"></a>備份

確定適用於 MariaDB 的 Azure 資料庫實例是使用異地還原進行備份。 如此一來，在發生區域性中斷的情況下，備份可以在配對的區域中使用。

> [!IMPORTANT]
> 請確定適用於 MariaDB 的 Azure 資料庫實例具有 [資源鎖定](/azure/azure-resource-manager/management/lock-resources) ，以防止其遭到刪除。 無法還原已刪除的伺服器。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- 適用於 MariaDB 的 Azure 資料庫可以向上或向下調整。 伺服器和資料庫的效能監視很重要，可確保您符合您的需求，但也至少會保留成本。
- CPU 和儲存體都有相關聯的成本。 有幾個定價層可供選擇。 請確定已為數據工作負載選取適當的定價方案。
- 每個讀取複本會根據選取的計算和儲存體計費。
- 使用保留容量來節省成本。

## <a name="conclusion"></a>結論

在本文中，Contoso 會將其適用于 mariadb 資料庫移轉至適用於 MariaDB 的 Azure 資料庫實例。
