---
title: 將適用于 mariadb 資料庫移轉至 Azure
description: 瞭解 Contoso 如何將其內部部署適用于 mariadb 資料庫移轉至 Azure。
author: deltadan
ms.author: abuck
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: azure-migrate
ms.openlocfilehash: 84bd6cbef97428aacb81d4d6136439a1c9c1f3f6
ms.sourcegitcommit: bcc73d194c6d00c16ae2e3c7fb2453ac7dbf2526
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/09/2020
ms.locfileid: "86197973"
---
<!-- TODO: Verify GraphDBMS term -->
<!-- cSpell:ignore ColumnStore GraphDBMS mysqldump Navicat phpMyAdmin -->

# <a name="migrate-mariadb-databases-to-azure"></a>將適用于 mariadb 資料庫移轉至 Azure

本文示範虛構公司 Contoso 如何規劃並將其內部部署適用于 mariadb 開放原始碼資料庫平臺遷移至 Azure。

Contoso 會使用適用于 mariadb over MySQL，因為它有無數的儲存引擎、快取和索引效能、功能的開放原始碼支援、延伸模組，以及其分析資料行存放區支援。 其遷移的目標是繼續使用適用于 mariadb，但不必擔心管理支援它所需的環境。

## <a name="business-drivers"></a>商業動機

IT 領導小組與商務合作夥伴密切合作，以了解此次移轉所要實現的目標：

- **增加可用性。** Contoso 在其適用于 mariadb 內部部署環境中有可用性問題，企業需要使用此資料存放區的應用程式更可靠。

- **提高效率。** Contoso 必須移除不必要的程式，並簡化開發人員和使用者的流程。 企業需要快速且不浪費時間或金錢，以更快的速度提供客戶需求。

- **增加靈活性。**  Contoso IT 必須能夠更快因應企業的需求。 它必須能夠以比 marketplace 中的變更更快的速度來實現全球經濟的成功。 它會不得取得或成為商務封鎖程式。

- **尺度.** 隨著企業順利成長，Contoso IT 必須提供能夠同步成長的系統。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已將此遷移的目標固定在一起，並會使用它們來判斷最佳的遷移方法。

| 需求 | 詳細資料 |
| --- | --- |
| **可用性** | 目前內部員工在適用于 mariadb 實例的主控環境中有困難的時間。 Contoso 想要有接近資料庫層的99.99% 可用性。 |
| **延展性** | 內部部署資料庫主機的容量很快就會用盡，Contoso 需要一種方法來調整其目前限制的實例，或在商務環境變更以節省成本時，將其向下調整。 |
| **效能** | Contoso 人力資源 (HR) 部門有數個每日、每週和每月執行的報表。 當他們執行這些報表時，會發現員工面向的應用程式有相當大的效能問題。 他們需要執行報表，而不會影響應用程式效能。 |
| **安全性** | Contoso 必須知道資料庫只能供其內部應用程式存取，而且無法透過網際網路顯示或存取。 |
| **監視** | Contoso 目前使用工具來監視適用于 mariadb 的計量，並在 CPU、記憶體或儲存體發生問題時提供通知。 他們想要在 Azure 中具有這項相同的功能。 |
| **業務持續性** | HR 資料存放區是 Contoso 每日作業的重要部分，如果它已損毀或需要還原，他們會想要將停機時間降到最低。 |
| **Azure** | Contoso 想要將應用程式移至 Azure，而不在 Vm 上執行。 針對資料層使用 Azure PaaS 服務的 Contoso 需求狀態。 |

## <a name="solution-design"></a>解決方案設計

在將目標和需求固定之後，Contoso 會設計和審查部署解決方案，並識別遷移程式，包括用於遷移的工具和服務。

### <a name="current-application"></a>目前的應用程式

適用于 mariadb 主控員工資料，用於公司人力資源部門的所有層面。 以[燈泡為基礎](https://wikipedia.org/wiki/LAMP_(software_bundle))的應用程式是用來做為處理員工 HR 要求的前端。 Contoso 在全球有100000名員工，因此執行時間對其資料庫而言非常重要。

### <a name="proposed-solution"></a>建議的解決方案

- 評估環境以進行遷移相容性。
- 使用常見的開放原始碼工具將資料庫移轉至適用於 MariaDB 的 Azure 資料庫實例。
- 修改所有應用程式和進程，以使用新的適用於 MariaDB 的 Azure 資料庫實例。

### <a name="database-considerations"></a>資料庫考量

在解決方案設計過程中，Contoso 已在 Azure 中審查用來裝載其適用于 mariadb 資料庫的功能。 下列考慮可協助他們決定使用 Azure。

- 類似于 Azure SQL，適用於 MariaDB 的 Azure 資料庫允許[防火牆規則](https://docs.microsoft.com/azure/mariadb/concepts-firewall-rules)。
- 適用於 MariaDB 的 Azure 資料庫可以與[Azure 虛擬網路](https://docs.microsoft.com/azure/mariadb/concepts-data-access-security-vnet)搭配使用，以防止實例公開存取
- 適用於 MariaDB 的 Azure 資料庫具有 Contoso 必須符合其審計員的必要合規性和隱私權認證。
- 系統會使用讀取複本來增強報表和應用程式處理效能。
- 只能 (無公用存取) 使用[私用連結](https://docs.microsoft.com/azure/mariadb/concepts-data-access-security-private-link)，將服務公開到內部網路流量的能力。
- 他們選擇不移到適用於 MySQL 的 Azure 資料庫，因為它們在未來可能會使用適用于 mariadb 資料行存放區和 GraphDBMS 資料庫模型。
- 從應用程式到資料庫的[頻寬和延遲](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)，將足以根據所選的閘道 (ExpressRoute 或站對站 VPN) 。

### <a name="solution-review"></a>解決方案檢閱

Contoso 會透過比較一份優缺點清單，來評估其建議設計。

| 考量 | 詳細資料 |
| --- | --- |
| **優點** | 適用於 MariaDB 的 Azure 資料庫提供99.99% 的財務支援服務等級協定， (SLA) 以取得[高可用性](https://docs.microsoft.com/azure/mariadb/concepts-high-availability)。 <br><br> Azure 提供在每季的尖峰負載時間進行相應增加或減少的功能。 Contoso 可以省更多購買的採購[保留容量](https://docs.microsoft.com/azure/mariadb/concept-reserved-pricing)。 <br><br> Azure 提供適用於 MariaDB 的 Azure 資料庫的時間點還原和異地還原功能。 <br><br> |
| **缺點** | Contoso 僅限於 Azure 中支援的適用于 mariadb 發行版本，目前是10.2 和10.3。 <br><br> 適用於 MariaDB 的 Azure 資料庫確實有一些[限制](https://docs.microsoft.com/azure/mariadb/concepts-limits)，例如相應減少儲存體。 |

## <a name="proposed-architecture"></a>建議的架構

![案例架構 ](./media/contoso-migration-mariadb-to-azure/architecture.png)
 _圖1：案例架構。_

### <a name="migration-process"></a>移轉程序

#### <a name="preparation"></a>準備

在您可以遷移適用于 mariadb 資料庫之前，您必須確定這些實例符合所有 Azure 必要條件，才能成功進行遷移。

支援的版本：

- 適用于 mariadb 會使用 x-y 命名配置。 X 是主要版本，y 是次要版本，而 z 是修補程式版本。
- Azure 目前支援10.2.25 和10.3.16。
- Azure 會自動管理修補程式更新的升級。 例如，10.2.21 至10.2.23。 不支援次要和主要版本升級。 例如，不支援從 MariaDB 10.2 升級至 MariaDB 10.3。 如果您想要從10.2 升級至10.3，請採用資料庫傾印並將它還原至以目標引擎版本建立的伺服器。

網路：

Contoso 必須設定從其內部部署環境到其適用于 mariadb 資料庫所在之虛擬網路的虛擬網路閘道連線。 這可讓內部部署應用程式在連接字串更新時，透過閘道存取資料庫。

  ![遷移程式 ](./media/contoso-migration-mariadb-to-azure/migration-process.png) _圖2：遷移程式。_

#### <a name="migration"></a>遷移

由於適用于 mariadb 與 MySQL 非常類似，因此可以使用相同的通用公用程式和工具（例如 MySQL 工作臺、mysqldump、Toad 或 Navicat）來連線到適用於 MariaDB 的 Azure 資料庫，並將資料移轉至其中。

Contoso 使用下列步驟來遷移其資料庫。

- 執行下列命令並觀察輸出，以判斷內部部署的適用于 mariadb 版本。 在大部分的情況下，您的版本在架構和資料傾印方面應該不太重要。 如果您使用的是應用層級的功能，您應該確定這些應用程式與 Azure 中的目標版本相容。

  ```cmd
    mysql -h localhost -u root -P
  ```

  ![遷移程式 ](./media/contoso-migration-mariadb-to-azure/mariadb_version.png)
   _圖4：判斷內部部署的適用于 mariadb 版本。_

- 在 Azure 中建立新的適用于 mariadb 實例：

  - 開啟 [Azure 入口網站](https://portal.azure.com)。
  - 選取 [**新增資源**]。
  - 搜尋 `MariaDB`。

    ![遷移程式 ](./media/contoso-migration-mariadb-to-azure/azure-mariadb-create.png)
     _圖4： Azure 中的新適用于 mariadb 實例。_

  - 選取 [建立]。
  - 選取您的訂用帳戶和資源群組。
  - 選取伺服器名稱和位置。
  - 選取您的目標版本 (10.2 或 10.3) 。
  - 選取您的計算和儲存體。
  - 輸入系統管理員使用者名稱和密碼。
  - 選取 [檢閱 + 建立]。

    ![遷移程式 ](./media/contoso-migration-mariadb-to-azure/azure_mariadb_create.png)
     _圖5：審查和建立。_

  - 選取 [建立]。
  - 記錄伺服器主機名稱、使用者名稱和密碼。
  - 選取 [連線**安全性**]。
  - 選取 [**新增用戶端 IP** ] (您將從中還原資料庫的來源 ip) 。
  - 選取 [Save] \(儲存\)。

- 執行下列命令，以匯出名為的資料庫 `Employees` 。 針對每個資料庫重複此動作：

    ```cmd
    mysqldump -h localhost -u root -p -–skip-triggers -–single-transaction –-extended-insert -–order-by-primary -–disable-keys Employees > Employees.sql
    ```

- 還原資料庫。 將取代為您適用於 MariaDB 的 Azure 資料庫實例的端點和使用者名稱

  ```cmd
  mysql -h {name}.mariadb.database.azure.com -u user@{name} -p –ssl
  create database employees;
  use database employees;
  source employees.sql;
  ```

- 使用 phpMyAdmin 或類似工具 (MySQL 工作臺、Toad 和 Navicat) ，請檢查每個資料表中的記錄計數來確認還原。
- 更新所有應用程式連接字串，以指向已遷移的資料庫。
- 測試所有應用程式的適當操作

## <a name="clean-up-after-migration"></a>移轉之後進行清除

經過驗證成功的遷移之後，Contoso 必須備份並儲存內部部署資料庫備份檔案，以供保留之用。 淘汰內部部署適用于 mariadb 伺服器。

## <a name="review-the-deployment"></a>檢閱部署

對於 Azure 中的移轉後資源，Contoso 必須能執行一切功能並保護其新的基礎結構。

### <a name="security"></a>安全性

- Contoso 必須確保其新的適用於 MariaDB 的 Azure 資料庫實例和資料庫都是安全的。 [深入了解](https://docs.microsoft.com/azure/mariadb/concepts-security)。
- Contoso 應檢查[防火牆規則](https://docs.microsoft.com/azure/mariadb/concepts-firewall-rules)和虛擬網路設定，以確認連線僅限於需要它的應用程式。
- 設定任何輸出 IP 需求，以允許連線至適用于 mariadb[閘道 IP 位址](https://docs.microsoft.com/azure/mariadb/concepts-connectivity-architecture)。
- 更新所有應用程式，以要求連接到資料庫的[SSL](https://docs.microsoft.com/azure/mariadb/concepts-ssl-connection-security) 。
- 設定[私用連結](https://docs.microsoft.com/azure/mariadb/concepts-data-access-security-private-link)，讓所有資料庫流量保留在 Azure 和內部部署網路中。
- 啟用[Azure 進階威脅防護 (ATP) ](https://docs.microsoft.com/azure/mariadb/concepts-data-access-and-security-threat-protection)。
- 設定 Log Analytics 以監視及警示安全性，並記錄相關的專案。

### <a name="backups"></a>備份

請確定使用異地還原來備份適用於 MariaDB 的 Azure 資料庫資料庫。 這可讓您在區域中斷時，在配對的區域中使用備份。

> [!IMPORTANT]
> 請確定適用於 MariaDB 的 Azure 資料庫資源有[資源鎖定](https://docs.microsoft.com/azure/azure-resource-manager/management/lock-resources)，以防止它被刪除。 已刪除的伺服器無法還原。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- 適用於 MariaDB 的 Azure 資料庫可以相應增加或相應減少，因此伺服器和資料庫的效能監視非常重要，可確保您符合需求，但也至少要保持成本。
- CPU 和儲存體都有相關聯的成本。 有數個定價層可供選擇。 請確定已針對資料工作負載選取適當的價格方案。
- 每個讀取複本都會根據選取的計算和儲存體來計費。
- 使用保留容量來節省成本。

## <a name="conclusion"></a>結論

在本文中，Contoso 會將其適用于 mariadb 資料庫移轉至適用於 MariaDB 的 Azure 資料庫實例。
