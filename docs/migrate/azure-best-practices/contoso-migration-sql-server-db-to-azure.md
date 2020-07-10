---
title: 將 SQL Server 資料庫移轉至 Azure
description: 瞭解 Contoso 如何將其內部部署 SQL 資料庫遷移至 Azure。
author: deltadan
ms.author: abuck
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: azure-migrate
ms.openlocfilehash: 44269a67e406abe6f67c2b5e233d4ea676f45163
ms.sourcegitcommit: bcc73d194c6d00c16ae2e3c7fb2453ac7dbf2526
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/09/2020
ms.locfileid: "86198067"
---
<!-- cSpell:ignore BACPAC FILESTREAM -->

# <a name="migrate-sql-server-databases-to-azure"></a>將 SQL Server 資料庫移轉至 Azure

本文示範虛構的公司 Contoso 如何評估、規劃並將其各種內部部署 SQL Server 資料庫移轉至 Azure。

Contoso 在考慮遷移至 Azure 時，該公司需要進行技術和財務方面的評量，以判斷其內部部署工作負載是否適合雲端移轉。 尤其是，Contoso 小組想要評定機器和資料庫是否能與移轉作業相容。 此外，它想要估計在 Azure 中執行 Contoso 資源的容量和成本。

## <a name="business-drivers"></a>商業動機

Contoso 有各種問題，可維護其網路上存在的 SQL Server 工作負載的所有版本。 在最新的投資者會議之後，CFO 和 CTO 已決定將所有這些工作負載移至 Azure。 這可讓他們從結構化的資本支出模型轉移到流暢的營運費用模型。

IT 領導小組與商務合作夥伴密切合作，以瞭解商務和技術需求：

- **提高安全性：** Contoso 必須能夠以更及時且有效率的方式來監視及保護所有資料資源。 他們也想要針對資料庫存取模式，取得更集中式的報表系統設定。

- **優化計算資源：** Contoso 已部署大型的內部部署伺服器基礎結構。 它們有數個 SQL Server 實例會取用，但不會真正使用以有效率方式配置的基礎 CPU、記憶體和磁片。

- **提高效率：** Contoso 必須移除不必要的程式，並簡化開發人員和使用者的流程。 企業亟需快速且不浪費時間或金錢的 IT 服務，進而更快滿足客戶的需求。 在遷移之後，資料庫管理應該縮減並（或）最小化。

- **提高靈活性：** Contoso IT 必須能夠更快地回應企業的需求。 其因應速度必須能夠比市場變化更快，才能更在全球經濟中獲致成功。 它不得礙事，或成為企業的絆腳石。

- **調整：** 隨著企業順利成長，Contoso IT 必須提供能夠以相同步調成長的系統。 有數個舊版硬體環境無法進一步升級，且已超過或接近終止支援。

- **成本：** 相較于在內部部署環境中執行應用程式，商務和應用程式擁有者想要知道它們不會卡在高雲端成本。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已針對各種不同的遷移，將目標固定在一起。 這些目標是用來判斷最佳的遷移方法。

| 需求 | 詳細資料 |
| --- | --- |
| **效能** | 遷移之後，Azure 中的應用程式應該在 Contoso 的內部部署環境中具有與應用程式相同的效能功能。 移至雲端並不表示應用程式效能較不重要。 |
| **相容性** | Contoso 必須瞭解其應用程式和資料庫與 Azure 的相容性。 Contoso 也必須瞭解其 Azure 裝載選項。 |
| **資料來源** | 所有資料庫都會移至 Azure，但不會有任何例外狀況。 根據所使用之 SQL 功能的資料庫和應用程式分析，它們會移至 PaaS、IaaS 或受控實例。 所有資料庫都必須移動。 |
| **應用程式** | 您必須盡可能將應用程式移至雲端。 如果無法移動，則只能透過私人連線，將其連線到 Azure 網路上已遷移的資料庫。 |
| **成本** | Contoso 不僅想要了解其移轉選項，還想了解在移到雲端之後與基礎結構相關的成本。 |
| **管理** | 您必須針對各種部門建立資源管理群組，以及使用資源群組來管理所有遷移的 SQL 資料庫。 所有資源都必須加上部門資訊的標籤，以取得收費要求。 |
| **限制** | 一開始，並非所有執行應用程式的分公司都會有直接的 ExpressRoute 連結至 Azure，因此這些辦公室必須透過虛擬網路閘道進行連接。 |

## <a name="solution-design"></a>解決方案設計

Contoso 已使用[服務對應](https://docs.microsoft.com/azure/azure-monitor/insights/service-map)功能的[Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-services-overview) ，來執行其數位資產的[遷移評估](https://docs.microsoft.com/azure/cloud-adoption-framework/plan/contoso-migration-assessment)。

評量會導致多個工作負載分散于多個部門。 遷移專案的整體大小將需要完整的專案管理 office (PMO) ，以管理通訊、資源和排程規劃的細節。

![移轉程序](./media/contoso-migration-sql-server-db-to-azure/migration-process.png)

### <a name="solution-review"></a>解決方案檢閱

Contoso 會透過比較一份優缺點清單，來評估其建議設計。

| 考量 | 詳細資料 |
| --- | --- |
| **優點** | Azure 會在資料庫工作負載中提供單一的半透明窗格 <br><br> 成本會透過 Azure 成本管理和計費來進行監視。 <br><br> 商務費用-您可以使用 Azure 計費 API 輕鬆執行計費。 <br><br> 伺服器和軟體維護將會縮減為僅限 IaaS 架構的環境。 |
| **缺點** | 由於 IaaS 型虛擬機器的需求，因此仍然需要管理這些機器上的軟體。 |

### <a name="budget-and-management"></a>預算和管理

在進行遷移之前，必須先備妥必要的 Azure 結構，以支援解決方案的管理和計費層面。

針對管理需求，已建立數個[管理群組](https://docs.microsoft.com/azure/governance/management-groups/overview)來支援組織結構。

針對計費需求，會將每個 Azure 資源[標記](https://docs.microsoft.com/azure/azure-resource-manager/management/tag-resources)為適當的帳單標記。

### <a name="migration-process"></a>移轉程序

資料移轉遵循標準的可重複模式。 這牽涉到以[Microsoft 最佳做法](https://datamigration.microsoft.com/)為基礎的下列步驟：

- 預先遷移：
  - **探索：** 清查資料庫資產和應用程式堆疊。
  - **評估：** 評估工作負載和修正建議。
  - **轉換：** 將來源架構轉換為在目標中工作。
- 移轉：
  - **遷移：** 將來源架構、來源資料和物件遷移至目標。
  - **同步處理資料：** 同步處理資料 (以最短的停機時間) 。
  - 轉換 **：** 將來源剪下至目標。
- 遷移後：
  - **補救應用程式：** 對您的應用程式進行反復的進行和必要變更。
  - **執行測試：** 反復執行功能和效能測試。
  - **優化：** 根據測試，解決效能問題，然後重新測試以確認效能改進。
  - **淘汰資產：** 舊的 Vm 和裝載環境會進行備份和淘汰。

#### <a name="step-1-discovery"></a>步驟1：探索

Contoso 使用 Azure Migrate 與服務對應來呈現所有 Contoso 環境的相依性。 Azure Migrate 自動探索到 Windows 和 Linux 系統上的應用程式元件，並對應服務之間的通訊。 使用 Azure Migrate 的服務對應功能，會顯示 Contoso 伺服器、進程、輸入和輸出連線延遲之間的連線，以及其 TCP 連線架構間的埠之間的連接。 只有在安裝[Microsoft Monitoring Agent](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows)和[Microsoft dependency](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-hybrid-cloud#install-the-dependency-agent-on-windows) agent 時，才需要 Contoso。

Contoso 也會將 Data Migration Assistant 新增至其 Azure Migrate 專案。 藉由選取此工具，他們就能夠評估要遷移至 Azure 的資料庫。

![Data Migration Assistant](./media/contoso-migration-sql-server-db-to-azure/add-dma.png)

#### <a name="step-2-application-assessment"></a>步驟2：應用程式評估

<!-- docsTest:ignore "mainly .NET-based" "non-.NET-based" -->

評估的結果提供 Contoso 主要使用的可見度。不過，以網路為基礎的應用程式多年來，不同的專案已使用其他技術（例如 PHP 和 Node.js）。 廠商購買的系統也引進了非 .NET 型的應用程式。 他們已識別下列內容：

- ~ 800 Windows .NET 應用程式
- ~ 50 PHP 應用程式
- 25 Node.js 應用程式
- 10個 JAVA 應用程式

#### <a name="step-3-database-assessment"></a>步驟3：資料庫評估

探索到每個資料庫工作負載時，會執行 Data Migration Assistant (DMA) 工具來判斷所使用的功能。 DMA 藉由偵測可能影響新版本 SQL Server 或 Azure SQL Database 中資料庫功能的相容性問題，協助 Contoso 評估其資料庫移轉至 Azure。

Contoso 會遵循這些步驟來評估其資料庫，然後將結果資料上傳至 Azure Migrate：

1. 下載 DMA。
1. 建立評估專案。
1. 在 [DMA] 中，登入 Azure Migrate 專案並同步評估摘要。

![Azure Migrate 和 DMA](./media/contoso-migration-sql-server-db-to-azure/azure-migrate-dma.png)

DMA 針對您的目標環境建議效能和可靠性，並可讓他們將其架構、資料和非內含性物件從來源伺服器移至目標伺服器。

深入瞭解[Data Migration Assistant](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem?view=sql-server-2017)

Contoso 使用 DMA 來執行評估，然後直接將資料上傳至 Azure Migrate。

![將 DMA 上傳至 Azure Migrate](./media/contoso-migration-sql-server-db-to-azure/upload-db-data.png)

現在將資料庫資訊載入 Azure Migrate，Contoso 已識別出超過1000個必須遷移的資料庫實例。 在這些情況下，大約40% 可移至 Azure SQL Database。 剩餘的60百分比必須移至在 Azure 虛擬機器或 Azure SQL 受控執行個體上執行的 SQL Server。 在這些60百分比中，大約10% 需要以虛擬機器為基礎的方法，其餘的實例將會移至 Azure SQL 受控執行個體。

當 DMA 無法在資料來源上執行時，資料庫移轉會遵循下列指導方針。

> [!NOTE]
> Contoso 會在評估階段中探索各種開放原始碼資料庫。 他們會分別遵循[本指南](./contoso-migration-oss-db-to-azure.md)來進行遷移計畫。

<!-- docsTest:ignore "custom .NET" -->

#### <a name="step-4-migration-planning"></a>步驟4：遷移規劃

在手邊取得資訊時，Contoso 會使用下列指導方針來決定要用於每個資料庫的遷移方法。

| 目標 | 資料庫使用量 | 詳細資料 | 線上移轉 | 離線移轉 | 大小上限 | 移轉指南 |
| --- | --- | --- | --- | ---| --- | --- |
| Azure SQL Database (PaaS) | 僅 SQL Server (資料)  | 這些資料庫只是使用基本資料表、資料行、預存程式和函數 | [Data Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview)，[異動複寫](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transactional-replication) | [BACPAC](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database)、 [bcp](https://docs.microsoft.com/sql/tools/bcp-utility?view=sql-server-ver15) | 1 TiB | [連結](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql) |
| Azure SQL 受控執行個體 | SQL Server (先進的功能)  | 這些資料庫會使用觸發程式和其他[先進的概念](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#service-broker)，例如自訂 .net 類型、服務代理程式等等。 | [Data Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview)，[異動複寫](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transactional-replication) | [BACPAC](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database)、 [bcp](https://docs.microsoft.com/sql/tools/bcp-utility?view=sql-server-ver15)、[原生備份/還原](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started-restore) | 2 TiB-8 TiB | [連結](https://docs.microsoft.com/azure/dms/tutorial-sql-server-managed-instance-online) |
| Azure 虛擬機器 (IaaS) 上的 SQL Server | SQL Server (協力廠商整合)  | SQL Server 必須具有[不支援的 SQL 受控執行個體功能](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#service-broker) (跨實例服務代理程式、密碼編譯提供者、緩衝集區、100以下的相容性層級、資料庫鏡像、FILESTREAM、PolyBase、需要存取檔案共用的任何專案、外部腳本、擴充預存程式，以及其他) 或安裝的協力廠商軟體，以支援資料庫的活動。 | [異動複寫](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transactional-replication) | [BACPAC](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database)， [bcp](https://docs.microsoft.com/sql/tools/bcp-utility?view=sql-server-ver15)，[快照](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transactional-replication)式複寫，[原生備份/還原](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started-restore)，將實體機器轉換成 VM | 4 GiB-64 TiB | [連結](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-migrate-sql) |

由於資料庫數量龐大，Contoso 建立了專案管理 office (PMO) 來追蹤每個資料庫移轉實例。 [責任與責任](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/migration-considerations/assess/)已指派給每個商務和應用程式小組。

Contoso 也已執行[工作負載準備檢查](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/migration-considerations/assess/evaluate)。 本審查已檢查基礎結構、資料庫和網路元件。

#### <a name="step-5-test-migrations"></a>步驟5：測試遷移

遷移準備的第一個部分牽涉到將每個資料庫的測試遷移至預先安裝環境。 為了節省時間，他們將所有的作業都編寫成腳本，並記錄每個作業的時機。 為了加速遷移，他們發現可以同時執行哪些遷移作業。

萬一發生某些非預期的失敗，就會針對每個資料庫工作負載識別任何復原程式。

針對以 IaaS 為基礎的工作負載，他們會事先設定所有必要的協力廠商軟體。

在測試遷移之後，Contoso 可以使用各種 Azure[成本估計工具](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/migration-considerations/assess/estimate)，以更精確地瞭解其遷移的未來營運成本。

#### <a name="step-6-migration"></a>步驟6：遷移

針對生產環境的遷移，Contoso 識別出所有資料庫移轉的時間範圍，以及在週末期間，可充分執行的工作 (在星期五到午夜的午夜，) 最短的停機時間。

根據其記載的測試程式，他們會盡可能地透過腳本執行每次遷移，以限制任何手動工作來將錯誤降至最低。

如果在視窗期間發生任何遷移失敗，則會在下一個遷移視窗中復原並重新排程。

### <a name="clean-up-after-migration"></a>移轉之後進行清除

Contoso 已識別出所有資料庫工作負載的保存時間範圍。 當視窗過期時，將會從內部部署基礎結構淘汰資源。

其中包括：

- 從內部部署伺服器移除生產資料。
- 當最後一個工作負載時間範圍到期時，淘汰主控伺服器。

### <a name="review-the-deployment"></a>檢閱部署

對於 Azure 中的移轉後資源，Contoso 必須能執行一切功能並保護其新的基礎結構。

#### <a name="security"></a>安全性

- Contoso 必須確保其新的 Azure 資料庫工作負載是安全的。 [深入了解](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview)。
- 特別是，Contoso 應該檢查防火牆和虛擬網路設定。
- 設定[私用連結](https://docs.microsoft.com/azure/azure-sql/database/private-endpoint-overview)，讓所有資料庫流量保留在 Azure 和內部部署網路中。
- 啟用 Azure SQL Database 的[Azure 進階威脅防護](https://docs.microsoft.com/azure/azure-sql/database/threat-detection-overview)。

#### <a name="backups"></a>備份

- 請確定使用異地還原來備份 Azure 資料庫。 這可讓您在區域中斷時，在配對的區域中使用備份。
- **重要事項：** 請確定 Azure 資源具有[資源鎖定](https://docs.microsoft.com/azure/azure-resource-manager/management/lock-resources)，以防止其遭到刪除。 已刪除的伺服器無法還原。

#### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- 許多 Azure 資料庫工作負載都可以相應增加或相應減少，因此伺服器和資料庫的效能監視非常重要，可確保您符合需求，但也至少要保持成本。
- CPU 和儲存體都有相關聯的成本。 有數個定價層可供選擇。 請確定已針對資料工作負載選取適當的價格方案。
- [彈性](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers-dtu)集區是針對具有相容資源使用模式的資料庫來執行。
- 每個讀取複本的計費依據為選取的計算和儲存體
- 使用保留容量來節省成本。

## <a name="conclusion"></a>結論

在本文中，Contoso 已評估、規劃並將其 Microsoft SQL Server 工作負載遷移至 Azure。
