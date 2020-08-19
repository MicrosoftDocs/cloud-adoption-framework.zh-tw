---
title: 將 MySQL 資料庫遷移至 Azure
description: 瞭解 Contoso 如何將其內部部署的 MySQL 資料庫遷移至 Azure。
author: deltadan
ms.author: abuck
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: azure-migrate
ms.openlocfilehash: 39b9d4781b5caabed5524577be9819e4544ec665
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88567150"
---
<!-- cSpell:ignore mysqldump InnoDB binlog Navicat -->

# <a name="migrate-mysql-databases-to-azure"></a>將 MySQL 資料庫遷移至 Azure

本文示範虛構公司 Contoso 如何規劃和遷移其內部部署 MySQL 開放原始碼資料庫平臺到 Azure。

## <a name="business-drivers"></a>商業動機

IT 領導小組與商務合作夥伴密切合作，瞭解他們想要使用這種方式達成什麼目標。 他們想要：

- **提高可用性。** Contoso 在其 MySQL 內部部署環境中有可用性問題。 企業要求使用此資料存放區的應用程式更可靠。
- **提高效率。** Contoso 必須移除不必要的程式，並簡化開發人員和使用者的流程。 企業需要快速且不浪費時間或金錢，以更快的速度提供客戶的需求。
- **增加靈活性。** Contoso IT 必須能夠更快因應企業的需求。 它必須比 marketplace 中的變更更快，以實現全球經濟的成功。 It 不得成為企業封鎖程式。
- **規模。** 隨著企業順利成長，Contoso IT 必須提供以相同步調成長的系統。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已針對此次移轉擬定好各項目標。 並用這些目標用來判斷最合適的移轉方法。

| 需求 | 詳細資料 |
| --- | --- |
| **可用性** | 目前內部員工正在使用 MySQL 實例的裝載環境。 Contoso 希望資料庫層有接近99.99% 的可用性。 |
| **延展性** | 內部部署資料庫主機的容量很快地用盡。 Contoso 需要一種方法來調整超過目前限制的實例，或在商務環境變更時縮小以節省成本。 |
| **效能** | Contoso 人力資源 (HR) 部門每天、每週和每月執行各種報告。 當它執行這些報表時，它會遇到與員工面向應用程式明顯的效能問題。 它需要執行報表，而不會影響應用程式效能。 |
| **安全性** | Contoso 必須知道資料庫只能供其內部應用程式存取，而且無法透過網際網路顯示或存取。 |
| **監視** | Contoso 目前使用工具來監視 MySQL 資料庫伺服器的計量，並在 CPU、記憶體或儲存體發生問題時提供通知。 公司想要在 Azure 中擁有相同的功能。 |
| **業務持續性** | HR 資料存放區是 Contoso 日常營運的重要部分。 如果它損毀或需要還原，公司想要盡可能將停機時間降到最低。 |
| **Azure** | Contoso 想要將應用程式移至 Azure，而不在 Vm 上執行。 Contoso 想要使用 Azure 平臺即服務 (適用于資料層的 PaaS) 服務。 |

## <a name="solution-design"></a>解決方案設計

在達到目標和需求後，Contoso 會設計和審核部署解決方案，並識別遷移程式。 此外，也會識別用於遷移的工具和服務。

### <a name="current-application"></a>目前的應用程式

MySQL 資料庫會儲存公司人力資源部門的所有層面所使用的員工資料。 以 [燈泡為基礎](https://wikipedia.org/wiki/LAMP_(software_bundle)) 的應用程式是用來處理員工 HR 要求的前端。 Contoso 有全球各地的100000員工，所以執行時間很重要。

### <a name="proposed-solution"></a>建議的解決方案

使用 Azure 資料庫移轉服務，將資料庫移轉至適用於 MySQL 的 Azure 資料庫實例。 修改所有的應用程式和進程，以使用新的適用於 MySQL 的 Azure 資料庫實例。

### <a name="database-considerations"></a>資料庫考量

<!-- TODO: Verify GraphDBMS term -->
<!-- docsTest:ignore ColumnStore GraphDBMS -->

在解決方案設計過程中，Contoso 會檢查 Azure 中的功能，以裝載其 MySQL 資料。 下列考慮有助於公司決定使用 Azure：

- 類似于 Azure SQL Database，適用於 MySQL 的 Azure 資料庫允許 [防火牆規則](/azure/mysql/concepts-firewall-rules)。
- 適用於 MySQL 的 Azure 資料庫可以搭配 [Azure 虛擬網路](/azure/mysql/concepts-data-access-and-security-vnet) 使用，以防止實例可公開存取。
- 適用於 MySQL 的 Azure 資料庫具有 Contoso 針對其審計員必須符合的必要合規性和隱私權認證。
- 報表和應用程式處理效能將會使用讀取複本來增強。
- 只能將服務公開給內部網路流量的能力， (使用 [Azure Private Link](/azure/mysql/concepts-data-access-security-private-link)沒有公用存取) 。
- Contoso 選擇不移至適用於 MySQL 的 Azure 資料庫，因為未來可能會使用適用于 mariadb 資料行存放區和 GraphDBMS 資料庫模型。
- 除了 MySQL 功能以外，Contoso 還 proponent 了真正的開放原始碼專案，並選擇不使用 MySQL。
- 從應用程式到資料庫的 [頻寬和延遲](/azure/vpn-gateway/vpn-gateway-about-vpngateways) ，會根據所選的閘道而足夠， (Azure ExpressRoute 或站對站 VPN) 。

### <a name="solution-review"></a>解決方案檢閱

Contoso 會透過比較一份優缺點清單，來評估建議設計。

| 考量 | 詳細資料 |
| --- | --- |
| **優點** | 適用於 MySQL 的 Azure 資料庫提供99.99% 的財務支援服務等級協定， (SLA) 以獲得 [高可用性](/azure/mysql/concepts-high-availability)。 <br><br> Azure 可讓您在每季的尖峰負載期間擴大或縮小。 Contoso 可以購買 [保留容量](/azure/mysql/concept-reserved-pricing)來節省更多成本。 <br><br> Azure 提供適用於 MySQL 的 Azure 資料庫的時間點還原和異地還原功能。 <br><br> |
| **缺點** | Contoso 受限於 Azure 中支援的 MySQL 發行版本，目前為10.2 和10.3。 <br><br> 適用於 MySQL 的 Azure 資料庫有一些 [限制](/azure/mysql/concepts-limits)，例如調整儲存體。 |

## <a name="proposed-architecture"></a>建議的架構

![圖表顯示案例架構。 ](./media/contoso-migration-mysql-to-azure/architecture.png)
_圖1：案例架構。_

### <a name="migration-process"></a>移轉程序

#### <a name="preparation"></a>準備

在遷移 MySQL 資料庫之前，您必須確定這些實例符合所有 Azure 必要條件，以進行成功的遷移。

#### <a name="supported-versions"></a>支援的版本

MySQL 會使用 X. Z 版本控制配置。 例如，X 是主要版本，Y 是次要版本，而 Z 是修補程式版本。

Azure 目前支援10.2.25 和10.3.16。

Azure 會自動管理修補程式更新的升級。 範例10.2.21 至10.2.23。 不支援次要和主要版本升級。 例如，不支援從 MySQL 10.2 升級至 MySQL 10.3。 如果您想要從10.2 升級為10.3，請取得傾印，並將其還原至使用新引擎版本建立的伺服器。

#### <a name="network"></a>Network (網路)

Contoso 必須將虛擬網路閘道連線從其內部部署環境設定為其 MySQL 資料庫所在的虛擬網路。 此連接可讓內部部署應用程式在連接字串更新時，透過閘道存取資料庫。

![圖表顯示遷移程式。 ](./media/contoso-migration-mysql-to-azure/migration-process.png)
_圖2：遷移程式。_

#### <a name="migration"></a>移轉

Contoso 管理員會使用 Azure 資料庫移轉服務來遷移資料庫，並遵循 [逐步遷移教學](/azure/dms/tutorial-mysql-azure-mysql-online)課程。 他們可以使用 MySQL 5.6 或5.7 來執行線上、離線和混合式 (預覽版) 的遷移。

> [!NOTE]
> 適用於 MySQL 的 Azure 資料庫支援 MySQL 8.0。 資料庫移轉服務工具尚不支援該版本。

總而言之，他們必須執行下列工作：

- 確定已符合所有的遷移必要條件：
  - MySQL 資料庫伺服器來源必須符合適用於 MySQL 的 Azure 資料庫支援的版本。 適用於 MySQL 的 Azure 資料庫支援 MySQL 社區版、InnoDB 儲存引擎，以及使用相同版本跨來源和目標進行遷移。
  - `my.ini` (Windows) 或 `my.cnf` (Unix) 啟用二進位記錄。 無法啟用二進位記錄，會在 [遷移嚮導] 中出現下列錯誤：「二進位記錄中的錯誤。 變數 binlog_row_image 的值為「基本」。 請將其變更為「完整」。如需詳細資訊，請參閱此 [MySQL 網站](https://go.microsoft.com/fwlink/?linkid=873009`)。
  - 使用者必須具有 `ReplicationAdmin` 角色。
  - 在不包含外鍵和觸發程式的情況下遷移資料庫架構。
- 建立透過 ExpressRoute 或 VPN 連接到內部部署網路的虛擬網路。
- 使用 `Premium` 連線到虛擬網路的 SKU 來建立 Azure 資料庫移轉服務實例。
- 確定實例可以透過虛擬網路存取 MySQL 資料庫。 請確定已在虛擬網路層級、網路 VPN 和裝載 MySQL 的電腦上，允許從 Azure 到 MySQL 的所有連入埠。
- 建立新的資料庫移轉服務專案：

  ![螢幕擷取畫面顯示如何建立新的資料庫移轉服務專案 ](./media/contoso-migration-mysql-to-azure/migration-dms-new-project.png)
   _圖3： Azure 資料庫移轉服務專案。_

#### <a name="migration-by-using-native-tools"></a>使用原生工具進行遷移

除了使用 Azure 資料庫移轉服務以外，Contoso 還可以使用常見的公用程式和工具（例如 MySQL 工作臺、mysqldump、Toad 或 Navicat）來連線到適用於 MySQL 的 Azure 資料庫，並將資料移轉至其中。

- 使用 mysqldump 傾印及還原：
  - 使用 mysqldump 中的 [排除-觸發程式] 選項，以防止在匯入期間執行觸發程式，並改善效能。
  - 您可以使用單一交易選項，將翻譯隔離模式設定為 `REPEATABLE READ` ，並在傾印 `START TRANSACTION` 資料前傳送 SQL 語句。
  - 使用 mysqldump 中的停用金鑰選項，即可在載入之前停用外鍵條件約束。 移除條件約束可提升效能。
  - 使用 Azure Blob 儲存體來儲存備份檔案，並從該處執行還原，以加快還原速度。
  - 更新應用程式連接字串。
  - 遷移資料庫之後，Contoso 必須更新連接字串，以指向新的適用於 MySQL 的 Azure 資料庫。

## <a name="clean-up-after-migration"></a>移轉之後進行清除

遷移之後，Contoso 必須備份內部部署資料庫以進行保留，並淘汰內部部署的 MySQL 資料庫伺服器。

## <a name="review-the-deployment"></a>檢閱部署

對於 Azure 中的遷移後資源，Contoso 必須能執行一切功能並保護其新的基礎結構。

### <a name="security"></a>安全性

Contoso 必須：

- 確定其新的適用於 MySQL 的 Azure 資料庫實例和資料庫都是安全的。 如需詳細資訊，請參閱 [適用於 MySQL 的 Azure 資料庫中的安全性](/azure/mysql/concepts-security)。
- 檢查防火牆和虛擬網路設定。
- 設定 Private Link，以便在 Azure 和內部部署網路內保存所有資料庫流量。
- 啟用 Azure 進階威脅防護。

### <a name="backups"></a>備份

請確定使用異地還原來備份適用于 MySQL 的 Azure 資料庫實例。 如此一來，在發生區域性中斷的情況下，備份可以在配對的區域中使用。

> [!IMPORTANT]
> 請確定適用於 MySQL 的 Azure 資料庫資源有資源鎖定，以防止其遭到刪除。 無法還原已刪除的伺服器。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- 適用於 MySQL 的 Azure 資料庫可以向上或向下調整。 監視伺服器和資料庫的效能非常重要，可確保符合需求，同時將成本降至最低。
- CPU 和儲存體都有相關聯的成本。 有數個定價層可供使用。 確定已針對每個資料工作負載選取適當的定價方案。
- 每個讀取複本會根據選取的計算和儲存體計費。
- 使用保留容量來節省成本。

## <a name="conclusion"></a>結論

在本文中，Contoso 將 MySQL 資料庫遷移至適用於 MySQL 的 Azure 資料庫實例。
