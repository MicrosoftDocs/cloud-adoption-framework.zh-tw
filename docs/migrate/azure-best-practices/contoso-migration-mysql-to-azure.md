---
title: 將 MySQL 資料庫遷移至 Azure
description: 瞭解 Contoso 如何將其內部部署 MySQL 資料庫遷移至 Azure。
author: deltadan
ms.author: abuck
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: azure-migrate
ms.openlocfilehash: 0f5735a9b61f2ab59ff129ef37b3ee1e90a4c40d
ms.sourcegitcommit: bcc73d194c6d00c16ae2e3c7fb2453ac7dbf2526
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/09/2020
ms.locfileid: "86197895"
---
<!-- cSpell:ignore mysqldump InnoDB binlog Navicat -->

# <a name="migrate-mysql-databases-to-azure"></a>將 MySQL 資料庫遷移至 Azure

本文示範虛構公司 Contoso 如何規劃並將其內部部署 MySQL 開放原始碼資料庫平臺遷移至 Azure。

## <a name="business-drivers"></a>商業動機

IT 領導小組與商務合作夥伴密切合作，以了解此次移轉所要實現的目標：

- **增加可用性。** Contoso 在其 MySQL 內部部署環境中有可用性問題。 企業需要使用此資料存放區的應用程式更可靠。
- **提高效率。** Contoso 必須移除不必要的程式，並簡化開發人員和使用者的流程。 企業需要快速且不浪費時間或金錢，以更快的速度提供客戶需求。
- **增加靈活性。**  Contoso IT 必須能夠更快因應企業的需求。 它必須能夠以比 marketplace 中的變更更快的速度來實現全球經濟的成功。 It 不得成為商務封鎖程式。
- **尺度.** 隨著企業順利成長，Contoso IT 必須提供能夠同步成長的系統。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已針對此次移轉擬定好各項目標。 並用這些目標用來判斷最合適的移轉方法。

| 需求 | 詳細資料 |
| --- | --- |
| **可用性** | 目前的內部員工與 MySQL 實例的主控環境有困難的時間。 Contoso 想要有接近資料庫層的99.99% 可用性。 |
| **延展性** | 內部部署資料庫主機的容量很快就會用盡，Contoso 需要一種方法來調整其目前限制的實例，或在商務環境變更以節省成本時，將其向下調整。 |
| **效能** | Contoso 人力資源 (HR) 部門會每日、每週和每月執行各種報告。 當他們執行這些報表時，他們會遇到與員工面向應用程式的重大效能問題。 他們需要執行報表，而不會影響應用程式效能。 |
| **安全性** | Contoso 必須知道資料庫只能供其內部應用程式存取，而且無法透過網際網路顯示或存取。 |
| **監視** | Contoso 目前使用工具來監視 MySQL 資料庫伺服器的計量，並在 CPU、記憶體或儲存體發生問題時提供通知。 他們想要在 Azure 中具有這項相同的功能。 |
| **業務持續性** | HR 資料存放區是 Contoso 每日作業的重要部分，如果它已損毀或需要還原，他們會想要盡可能將停機時間降到最低。 |
| **Azure** | Contoso 想要將應用程式移至 Azure，而不在 Vm 上執行。 Contoso 想要針對資料層使用 Azure PaaS 服務。 |

## <a name="solution-design"></a>解決方案設計

在將目標和需求固定之後，Contoso 會設計和審查部署解決方案，並識別遷移程式，包括將用於遷移的工具和服務。

### <a name="current-application"></a>目前的應用程式

MySQL 資料庫會儲存用於公司人力資源部門所有層面的員工資料。 以[燈泡為基礎](https://wikipedia.org/wiki/LAMP_(software_bundle))的應用程式是用來做為處理員工 HR 要求的前端。 Contoso 在全球有100000名員工，因此執行時間非常重要。

### <a name="proposed-solution"></a>建議的解決方案

使用 Azure 資料庫移轉服務將資料庫移轉至適用於 MySQL 的 Azure 資料庫實例，並修改所有應用程式和進程以使用新的適用於 MySQL 的 Azure 資料庫實例。

### <a name="database-considerations"></a>資料庫考量

<!-- TODO: Verify GraphDBMS term -->
<!-- docsTest:ignore ColumnStore GraphDBMS -->

在解決方案設計過程中，Contoso 已複習 Azure 中用來裝載 MySQL 資料的功能。 下列考慮可協助他們決定使用 Azure。

- 類似于 Azure SQL Database，適用於 MySQL 的 Azure 資料庫允許[防火牆規則](https://docs.microsoft.com/azure/mysql/concepts-firewall-rules)。
- 適用於 MySQL 的 Azure 資料庫可以與[Azure 虛擬網路](https://docs.microsoft.com/azure/mysql/concepts-data-access-and-security-vnet)搭配使用，以防止實例可公開存取。
- 適用於 MySQL 的 Azure 資料庫具有 Contoso 必須符合其審計員的必要合規性和隱私權認證。
- 您可以使用讀取複本來增強報表和應用程式處理效能。
- 僅 (沒有公用存取) 使用[私人連結](https://docs.microsoft.com/azure/mysql/concepts-data-access-security-private-link)，才能夠將服務公開到內部網路流量。
- 他們選擇不移到適用於 MySQL 的 Azure 資料庫，因為它們在未來可能會使用適用于 mariadb 資料行存放區和 GraphDBMS 資料庫模型。
- 除了 MySQL 功能以外，Contoso 也是真正的開放原始碼專案 proponent，並選擇不使用 MySQL。
- 從應用程式到資料庫的[頻寬和延遲](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)，將足以根據所選的閘道 (ExpressRoute 或站對站 VPN) 。

### <a name="solution-review"></a>解決方案檢閱

Contoso 會透過比較一份優缺點清單，來評估其建議設計。

| 考量 | 詳細資料 |
| --- | --- |
| **優點** | 適用於 MySQL 的 Azure 資料庫提供99.99% 的財務支援服務等級協定， (SLA) 以取得[高可用性](https://docs.microsoft.com/azure/mysql/concepts-high-availability)。 <br><br> Azure 提供在每季的尖峰負載時間進行相應增加或減少的功能。 Contoso 可以省更多購買的採購[保留容量](https://docs.microsoft.com/azure/mysql/concept-reserved-pricing)。 <br><br> Azure 提供適用於 MySQL 的 Azure 資料庫的時間點還原和異地還原功能。 <br><br> |
| **缺點** | Contoso 將受限於 Azure 中支援的 MySQL 發行版本，目前是10.2 和10.3。 <br><br> 適用於 MySQL 的 Azure 資料庫確實有一些[限制](https://docs.microsoft.com/azure/mysql/concepts-limits)，例如相應減少儲存體。 |

## <a name="proposed-architecture"></a>建議的架構

![案例架構 ](./media/contoso-migration-mysql-to-azure/architecture.png)
 _圖1：案例架構。_

### <a name="migration-process"></a>移轉程序

#### <a name="preparation"></a>準備

在您可以遷移 MySQL 資料庫之前，您必須先確定這些實例符合所有 Azure 必要條件，才能成功進行遷移。

#### <a name="supported-versions"></a>支援的版本

MySQL 會使用 `X.Y.Z` 版本設定配置。 `X`是主要版本， `Y` 是次要版本，而 `Z` 是修補程式版本。

Azure 目前支援10.2.25 和10.3.16。

Azure 會自動管理修補程式更新的升級。 例如，10.2.21 至10.2.23。 不支援次要和主要版本升級。 例如，不支援從 MySQL 10.2 升級至 MySQL 10.3。 如果您想要從10.2 升級至10.3，請使用傾印並將它還原至使用新引擎版本所建立的伺服器。

#### <a name="network"></a>網路

Contoso 必須設定從其內部部署環境到其 MySQL 資料庫所在之虛擬網路的虛擬網路閘道連線。 這可讓內部部署應用程式在連接字串更新時，能夠透過閘道存取資料庫。

![遷移程式 ](./media/contoso-migration-mysql-to-azure/migration-process.png)
 _圖2：遷移程式。_

#### <a name="migration"></a>遷移

Contoso 管理員會使用 Azure 資料庫移轉服務，透過[逐步執行遷移教學](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online)課程來遷移資料庫。 他們可以使用 MySQL 5.6 或5.7，執行線上、離線和混合式 (預覽) 遷移。

> [!NOTE]
> 適用於 MySQL 的 Azure 資料庫支援 MySQL 8.0，但資料庫移轉服務工具尚不支援該版本。

總而言之，他們必須執行下列動作：

- 確保符合所有的遷移必要條件：

  - MySQL 資料庫伺服器來源必須符合適用於 MySQL 的 Azure 資料庫支援的版本。 適用於 MySQL 的 Azure 資料庫支援 MySQL 社區版本、InnoDB 儲存引擎，以及跨來源與目標的相同版本進行遷移。
  - 在 `my.ini` (Windows) 或 `my.cnf` (Unix) 中啟用二進位記錄。 若未這麼做，將會導致遷移嚮導發生下列錯誤： `error in binary logging. Variable binlog_row_image has value 'minimal'. Please change it to 'full'. For more information, see https://go.microsoft.com/fwlink/?linkid=873009` 。
  - 使用者必須擁有 `ReplicationAdmin` 角色。
  - 不搭配外鍵和觸發程式來遷移資料庫架構。

- 建立透過 ExpressRoute 或 VPN 連接到內部部署網路的虛擬網路。

- 使用連線到 VNet 的 SKU 來建立 Azure 資料庫移轉服務實例 `Premium` 。

- 確定實例可以透過虛擬網路存取 MySQL 資料庫。 這會需要確保在虛擬網路層級、網路 VPN 和裝載 MySQL 的機器上，允許從 Azure 到 MySQL 的所有連入埠。

- 建立新的資料庫移轉服務專案：

  ![建立新的資料庫移轉服務專案 ](./media/contoso-migration-mysql-to-azure/migration-dms-new-project.png)
   _圖3： Azure 資料庫移轉專案。_

#### <a name="migration-using-native-tools"></a>使用原生工具進行遷移

Contoso 可以使用一般的公用程式和工具（例如 MySQL 工作臺、mysqldump、Toad 或 Navicat）來連線並將資料移轉至適用於 MySQL 的 Azure 資料庫，這是使用 Azure 資料庫移轉服務的替代方案。

- 使用 mysqldump 傾印和還原：
  - 在 mysqldump 中使用 [排除-觸發程式] 選項。這會防止觸發程式在匯入期間執行，並改善效能。
  - 使用單一交易選項，將轉譯隔離模式設定為 `REPEATABLE READ` ，並在傾印 `START TRANSACTION` 資料之前傳送 SQL 語句。
  - 在載入之前，請使用 mysqldump 中的停用按鍵選項來停用外鍵條件約束。 移除此將會提供效能提升。
  - 使用 Azure Blob 儲存體來儲存備份檔案，並從該處執行還原，以加快還原速度。
  - 更新應用程式連接字串。
  - 一旦遷移資料庫之後，Contoso 必須更新連接字串，以指向新的適用於 MySQL 的 Azure 資料庫。

## <a name="cleanup-after-migration"></a>遷移後的清除

遷移之後，Contoso 必須備份內部部署資料庫以進行保留，並淘汰內部部署的 MySQL 資料庫伺服器。

## <a name="review-the-deployment"></a>檢閱部署

對於 Azure 中的移轉後資源，Contoso 必須能執行一切功能並保護其新的基礎結構。

### <a name="security"></a>安全性

- Contoso 必須確保其新的適用於 MySQL 的 Azure 資料庫實例和資料庫都是安全的。 如需詳細資訊，請參閱[適用於 MySQL 的 Azure 資料庫中的安全性](https://docs.microsoft.com/azure/mysql/concepts-security)。
- 特別是，Contoso 應該檢查防火牆和虛擬網路設定。
- 設定私用連結，讓所有資料庫流量保留在 Azure 和內部部署網路中。
- 啟用 Azure 進階威脅防護。

### <a name="backups"></a>備份

請確定使用異地還原來備份適用於 MySQL 的 Azure 資料庫實例。 這可讓您在區域中斷時，在配對的區域中使用備份。

> [!IMPORTANT]
> 請確定適用於 MySQL 的 Azure 資料庫資源有資源鎖定，以防止它被刪除。 已刪除的伺服器無法還原。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- 適用於 MySQL 的 Azure 資料庫可以相應增加或相應減少，因此監視伺服器和資料庫的效能非常重要，可確保滿足您的需求，同時將成本降至最低。
- CPU 和儲存體都有相關聯的成本。 有數個可用的定價層。 請確定已針對每個資料工作負載選取適當的定價方案。
- 每個讀取複本都會根據選取的計算和儲存體來計費。
- 使用保留容量來節省成本。

## <a name="conclusion"></a>結論

在本文中，Contoso 會將其 MySQL 資料庫遷移至適用於 MySQL 的 Azure 資料庫實例。
