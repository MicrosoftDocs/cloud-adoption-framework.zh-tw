---
title: 移轉 SQL Server 資料庫到 Azure
description: 瞭解 Contoso 如何將其內部部署 SQL 資料庫遷移至 Azure。
author: deltadan
ms.author: abuck
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: think-tank
ms.openlocfilehash: 22cc5f779cbdbac94ac7de0e661b11d40f9c24de
ms.sourcegitcommit: 54f01dd0eafa23c532e54c821954ba682357f686
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/13/2021
ms.locfileid: "98174126"
---
<!-- cSpell:ignore BACPAC FILESTREAM -->

# <a name="migrate-sql-server-databases-to-azure"></a>移轉 SQL Server 資料庫到 Azure

本文示範虛構公司 Contoso 如何評估、規劃及遷移其各種內部部署 SQL Server 資料庫至 Azure。

Contoso 在考慮遷移至 Azure 時，該公司需要進行技術和財務方面的評量，以判斷其內部部署工作負載是否適合雲端移轉。 尤其是，Contoso 小組想要評定機器和資料庫是否能與移轉作業相容。 此外，它還想要估計在 Azure 中執行 Contoso 資源的容量和成本。

## <a name="business-drivers"></a>商業動機

Contoso 在維護所有存在於其網路上 SQL Server 工作負載的廣泛版本時，有各種問題。 在最新投資者的會議之後，CFO 和 CTO 決定將所有這些工作負載移至 Azure。 這可讓他們從結構化的資本支出模型轉移至流動的營運費用模型。

IT 領導小組與商務合作夥伴密切合作，以瞭解商務和技術需求：

- **提高安全性：** Contoso 必須能夠以更及時且有效率的方式監視和保護所有資料資源。 他們也會想要在資料庫存取模式上取得更集中的報告系統設定。

- **優化計算資源：** Contoso 已部署大型的內部部署伺服器基礎結構。 它們有數個 SQL Server 實例取用，但實際上並不會使用以有效率的方式配置的基礎 CPU、記憶體和磁片。

- **提高效率：** Contoso 必須移除不必要的程式，並簡化開發人員和使用者的流程。 企業亟需快速且不浪費時間或金錢的 IT 服務，進而更快滿足客戶的需求。 在遷移之後，資料庫管理應該減少及/或最小化。

- **提高靈活性：** Contoso IT 必須能夠更快回應企業的需求。 其因應速度必須能夠比市場變化更快，才能更在全球經濟中獲致成功。 它不得礙事，或成為企業的絆腳石。

- **調整規模：** 隨著企業順利成長，Contoso IT 必須提供能夠以相同步調成長的系統。 有幾個舊版的硬體環境無法進一步升級，且已超過或即將結束支援。

- **成本：** 相較于在內部部署環境中執行應用程式，商務和應用程式擁有者希望知道他們不會陷入高雲端成本。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已針對各種不同的遷移，將目標釘選在一起。 這些目標是用來決定最佳的遷移方法。

| 需求 | 詳細資料 |
| --- | --- |
| **效能** | 在遷移之後，Azure 中的應用程式在 Contoso 的內部部署環境中應該具有與應用程式相同的效能功能。 移至雲端並不表示應用程式效能較不重要。 |
| **相容性** | Contoso 必須瞭解其應用程式和資料庫與 Azure 的相容性。 Contoso 也需要瞭解其 Azure 裝載選項。 |
| **資料來源** | 所有資料庫都會移至 Azure，而不會發生例外狀況。 根據所使用 SQL 功能的資料庫和應用程式分析，這些功能將移至 PaaS、IaaS 或受控實例。 所有資料庫都必須移動。 |
| **應用程式** | 應用程式必須盡可能移至雲端。 如果無法移動，則只允許透過私人連線，透過 Azure 網路連接到已遷移的資料庫。 |
| **成本** | Contoso 不僅想要了解其移轉選項，還想了解在移到雲端之後與基礎結構相關的成本。 |
| **管理** | 您必須為各部門建立資源管理群組，以及使用資源群組來管理所有已遷移的 SQL 資料庫。 所有資源都必須以部門資訊標記，以取得退款需求。 |
| **限制** | 一開始，並非所有執行應用程式的分公司都有 Azure 的直接 ExpressRoute 連結，因此這些辦公室必須透過虛擬網路閘道進行連線。 |

## <a name="solution-design"></a>解決方案設計

Contoso 已經使用[Azure Migrate](/azure/migrate/migrate-services-overview)執行其數位資產的[遷移評](../..//plan/contoso-migration-assessment.md)量。

評量會導致多個工作負載分散在多個部門。 遷移專案的整體規模將需要完整的 project management office (PMO) ，以管理通訊、資源和排程規劃的細節。

![移轉程序](./media/contoso-migration-sql-server-db-to-azure/migration-process.png)

### <a name="solution-review"></a>解決方案檢閱

Contoso 會透過比較一份優缺點清單，來評估其建議設計。

| 考量 | 詳細資料 |
| --- | --- |
| **優點** | Azure 會在資料庫工作負載中提供單一的透明窗格 <br><br> 系統會透過 Azure 成本管理和帳單來監視成本。 <br><br> 商務費-向後計費將可讓 Azure 計費 API 輕鬆執行。 <br><br> 伺服器和軟體維護只會縮減為以 IaaS 為基礎的環境。 |
| **缺點** | 由於 IaaS 型虛擬機器的需求，仍需要在這些電腦上管理軟體。 |

### <a name="budget-and-management"></a>預算和管理

在進行遷移之前，必須先備妥必要的 Azure 結構，以支援解決方案的管理和計費方面。

為了管理需求，已建立數個 [管理群組](/azure/governance/management-groups/overview) 以支援組織結構。

針對計費需求，每個 Azure 資源都會以適當的計費標記 [標記](/azure/azure-resource-manager/management/tag-resources) 。

### <a name="migration-process"></a>移轉程序

資料移轉會遵循標準的可重複模式。 這包含以 [Microsoft 最佳作法](https://datamigration.microsoft.com/)為基礎的下列步驟：

- 預先遷移：
  - **探索：** 清查資料庫資產和應用程式堆疊。
  - **評定：** 評估工作負載和修正建議。
  - **轉換：** 將來源架構轉換成可在目標中運作。
- 移轉：
  - **遷移：** 將來源架構、來源資料和物件遷移至目標。
  - **同步資料：** 同步處理資料 (盡可能縮短停機時間) 。
  - 轉換 **：** 剪下來源到目標。
- 遷移後：
  - **補救應用程式：** 反復進行和必要的應用程式變更。
  - **執行測試：** 反復執行功能和效能測試。
  - **優化：** 根據測試，解決效能問題，然後重新測試以確認效能改進。
  - **淘汰資產：** 舊的 Vm 和裝載環境會進行備份和淘汰。

#### <a name="step-1-discovery"></a>步驟1：探索

Contoso 使用 Azure Migrate 來呈現 Contoso 環境之間的相依性。 Azure Migrate 自動探索 Windows 和 Linux 系統上的應用程式元件，並對應服務之間的通訊。 Azure Migrate 也顯示 Contoso 伺服器、進程、輸入和輸出連線延遲，以及其 TCP 連接架構之間的埠之間的連接。

Contoso 也將 Data Migration Assistant 新增至其 Azure Migrate 專案。 您可以藉由選取這項工具來評估要遷移至 Azure 的資料庫。

![Data Migration Assistant](./media/contoso-migration-sql-server-db-to-azure/add-dma.png)

#### <a name="step-2-application-assessment"></a>步驟2：應用程式評定

<!-- docutune:casing "mainly .NET-based" "non-.NET-based" -->

評量的結果提供了 Contoso 所使用的可見度。不過，以 .NET 為基礎的應用程式，多年來，許多專案都使用了 PHP 和 Node.js 等其他技術。 廠商購買的系統也引進了非以 .NET 為基礎的應用程式。 它們已識別下列各項：

- ~ 800 Windows .NET 應用程式
- ~ 50 PHP 應用程式
- 25 Node.js 的應用程式
- 10個 JAVA 應用程式

#### <a name="step-3-database-assessment"></a>步驟3：資料庫評量

探索到每個資料庫工作負載時，會執行 Data Migration Assistant (DMA) 工具，以判斷正在使用哪些功能。 DMA 可以偵測可能影響新版本 SQL Server 或 Azure SQL Database 中資料庫功能的相容性問題，以協助 Contoso 評定其資料庫移轉至 Azure。

Contoso 遵循下列步驟來評估其資料庫，然後將結果資料上傳至 Azure Migrate：

1. 下載 DMA。
1. 建立評定專案。
1. 在 DMA 中，登入 Azure Migrate 專案並同步處理評量摘要。

![Azure Migrate 和 DMA](./media/contoso-migration-sql-server-db-to-azure/azure-migrate-dma.png)

DMA 針對您的目標環境建議效能和可靠性改善，並可讓他們將其架構、資料和非內含性物件從來源伺服器移至目標伺服器。

深入瞭解 [Data Migration Assistant](/sql/dma/dma-assesssqlonprem?view=sql-server-2017)

Contoso 使用 DMA 來執行評量，然後將資料直接上傳至 Azure Migrate。

![將 DMA 上傳至 Azure Migrate](./media/contoso-migration-sql-server-db-to-azure/upload-db-data.png)

現在將資料庫資訊載入 Azure Migrate，Contoso 已經識別出超過1000個必須遷移的資料庫實例。 在這些實例中，大約40% 可移至適用于 Azure 的 SQL Database。 剩下的60% 必須移至 Azure 虛擬機器上執行的 SQL Server 或 Azure SQL 受控執行個體。 在這些60% 中，大約10% 需要以虛擬機器為基礎的方法，其餘的實例將會移至 Azure SQL 受控執行個體。

當 DMA 無法在資料來源上執行時，會遵循下列指導方針來進行資料庫移轉。

> [!NOTE]
> Contoso 在評量階段期間發現了各種開放原始碼資料庫。 它們分別是 [將開放原始碼資料庫移轉至 Azure](./contoso-migration-oss-db-to-azure.md) ，以進行遷移規劃。

<!-- docutune:casing "custom .NET" -->

#### <a name="step-4-migration-planning"></a>步驟4：遷移規劃

在手邊的資訊中，Contoso 會使用下列指導方針來決定要針對每個資料庫使用哪種遷移方法。

| 目標 | 資料庫使用方式 | 詳細資料 | 線上移轉 | 離線移轉 | 大小上限 | 移轉指南 |
| --- | --- | --- | --- | ---| --- | --- |
| Azure SQL Database (PaaS) | 僅 SQL Server (資料)  | 這些資料庫只是使用基本資料表、資料行、預存程式和函數 | [Data Migration Assistant](/sql/dma/dma-overview)， [異動複寫](/azure/sql-database/sql-database-managed-instance-transactional-replication) | [BACPAC](/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database)、 [bcp](/sql/tools/bcp-utility?view=sql-server-ver15) | 1 TiB | [連結](/azure/dms/tutorial-sql-server-to-azure-sql) |
| Azure SQL 受控執行個體 | SQL Server (advanced 功能)  | 這些資料庫會使用觸發程式和其他 [先進的概念](/azure/sql-database/sql-database-managed-instance-transact-sql-information#service-broker) ，例如自訂的 .net 類型、服務代理程式等。 | [Data Migration Assistant](/sql/dma/dma-overview)， [異動複寫](/azure/sql-database/sql-database-managed-instance-transactional-replication) | [BACPAC](/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database)、 [bcp](/sql/tools/bcp-utility?view=sql-server-ver15)、 [原生備份/還原](/azure/sql-database/sql-database-managed-instance-get-started-restore) | 2 TiB-8 TiB | [連結](/azure/dms/tutorial-sql-server-managed-instance-online) |
| Azure 虛擬機器上的 SQL Server (IaaS)  | SQL Server (協力廠商整合)  | SQL Server 必須有 [不支援的 SQL 受控執行個體功能](/azure/sql-database/sql-database-managed-instance-transact-sql-information#service-broker) (的跨實例服務代理程式、密碼編譯提供者、緩衝集區、低於100的相容性層級、資料庫鏡像、FILESTREAM、PolyBase、需要存取檔案共用、外部腳本、擴充預存程式的任何專案，) 以及安裝的協力廠商軟體，以支援資料庫的活動。 | [異動複寫](/azure/sql-database/sql-database-managed-instance-transactional-replication) | [BACPAC](/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database)、 [bcp](/sql/tools/bcp-utility?view=sql-server-ver15)、 [快照](/azure/sql-database/sql-database-managed-instance-transactional-replication)式複寫、 [原生備份/還原](/azure/sql-database/sql-database-managed-instance-get-started-restore)、將實體機器轉換為 VM | 4 GiB-64 TiB | [連結](/azure/virtual-machines/windows/sql/virtual-machines-windows-migrate-sql) |

由於有大量資料庫，Contoso 建立了專案管理辦公室 (PMO) 來追蹤每個資料庫移轉實例。 [責任和責任](../..//migrate/migration-considerations/assess/index.md) 已指派給每個商務和應用程式小組。

Contoso 也會執行 [工作負載準備](../..//migrate/migration-considerations/assess/evaluate.md)工作。 本評論檢查了基礎結構、資料庫和網路元件。

#### <a name="step-5-test-migrations"></a>步驟5：測試遷移

遷移準備的第一個部分牽涉到將每個資料庫移轉至預先安裝環境的測試。 為了節省時間，它們編寫了所有作業的腳本，並記錄每個作業的時間。 為了加速遷移，他們識別出可同時執行的遷移作業。

如果發生非預期的失敗，就會針對每個資料庫工作負載識別任何復原程式。

針對以 IaaS 為基礎的工作負載，他們會事先設定所有必要的協力廠商軟體。

在測試遷移之後，Contoso 可以使用各種 Azure [成本估計工具](../..//migrate/migration-considerations/assess/estimate.md) ，以更精確地瞭解其遷移的未來營運成本。

#### <a name="step-6-migration"></a>步驟6：遷移

在生產環境中，Contoso 會找出所有資料庫移轉的時間範圍，以及在週末期間可充分執行的工作時間範圍 (在最短的停機時間內，最短的業務停機時間) 。

根據其記載的測試程式，他們會盡可能地透過腳本執行每個遷移，以限制任何手動工作來將錯誤降至最低。

如果任何遷移在視窗期間失敗，系統就會在下一個遷移視窗中回復並重新排程。

### <a name="clean-up-after-migration"></a>移轉之後進行清除

Contoso 識別出所有資料庫工作負載的封存視窗。 當視窗過期時，資源將會從內部部署基礎結構淘汰。

這包括：

- 從內部部署伺服器移除生產資料。
- 當最後一個工作負載時間範圍過期時，淘汰主控伺服器。

### <a name="review-the-deployment"></a>檢閱部署

對於 Azure 中的移轉後資源，Contoso 必須能執行一切功能並保護其新的基礎結構。

#### <a name="security"></a>安全性

- Contoso 必須確保其新的 Azure 資料庫工作負載是安全的。 [深入了解](/azure/sql-database/sql-database-security-overview)。
- 尤其是，Contoso 應該檢查防火牆和虛擬網路設定。
- 設定 [Private Link](/azure/azure-sql/database/private-endpoint-overview) 以便將所有資料庫流量保留在 Azure 和內部部署網路內。
- 啟用 Azure SQL Database 的 [Azure 進階威脅防護](/azure/azure-sql/database/threat-detection-overview) 。

#### <a name="backups"></a>備份

- 確定 Azure 資料庫已使用異地還原進行備份。 這可在區域中斷時，將備份用於配對的區域中。
- **重要事項：** 請確定 Azure 資源具有 [資源鎖定](/azure/azure-resource-manager/management/lock-resources) ，以防止其遭到刪除。 已刪除的伺服器無法還原。

#### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- 許多 Azure 資料庫工作負載都可以相應增加或減少，因此，伺服器和資料庫的效能監視很重要，可確保您符合需求，同時維持最少的成本。
- CPU 和儲存體都有相關聯的成本。 有幾個定價層可供選擇。 請確定已為數據工作負載選取適當的定價方案。
- [彈性](/azure/sql-database/sql-database-service-tiers-dtu) 集區是針對具有相容資源使用模式的資料庫來執行。
- 每個讀取複本會根據所選的計算和儲存體計費
- 使用保留容量來節省成本。

## <a name="conclusion"></a>結論

在本文中，Contoso 已評估、規劃並將其 Microsoft SQL Server 工作負載遷移至 Azure。
 
您已開發 Azure DevOps 專案，以供您在 SQL 遷移旅程圖中進行研究，並符合雲端採用架構。 此專案將引導您完成所需的重要決策。 [選取此連結](https://azuredevopsdemogenerator.azurewebsites.net/?name=sqlmigration) 以流覽至 Azure DevOps 專案。
