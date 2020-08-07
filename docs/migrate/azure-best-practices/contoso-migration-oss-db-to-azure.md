---
title: 將開放原始碼資料庫遷移到 Azure
description: 瞭解 Contoso 如何將其內部部署開放原始碼資料庫移轉至 Azure。
author: deltadan
ms.author: abuck
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: azure-migrate
ms.openlocfilehash: 9169417f75434a176767fb20182a1bdd99a70bbf
ms.sourcegitcommit: 452e09b543e7b84f943db5b02480ba2d18afd939
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87866191"
---
# <a name="migrate-open-source-databases-to-azure"></a>將開放原始碼資料庫遷移到 Azure

本文示範虛構的公司 Contoso 如何評估、規劃並將其各種內部部署開放原始碼資料庫移轉至 Azure。

Contoso 在考慮遷移至 Azure 時，該公司需要進行技術和財務方面的評量，以判斷其內部部署工作負載是否適合雲端移轉。 尤其是，Contoso 小組想要評定機器和資料庫是否能與移轉作業相容。 此外，它想要估計在 Azure 中執行 Contoso 資源的容量和成本。

## <a name="business-drivers"></a>商業動機

Contoso 在維護其網路上的開放原始碼資料庫工作負載的各式各樣版本時遇到各種問題。 在最新的投資者會議之後，CFO 和 CTO 決定將所有這些工作負載移至 Azure。 這種移動會將其從結構化的資本支出模型轉移到流暢的營運費用模型。

IT 領導小組與商務合作夥伴密切合作，以瞭解商務和技術需求。 他們想要：

- **增加安全性。** Contoso 必須能夠以更及時且有效率的方式來監視及保護所有資料資源。 該公司也想要取得更集中式的報表系統，以設定資料庫存取模式。
- **將計算資源優化。** Contoso 已部署大型的內部部署伺服器基礎結構。 該公司有數個 SQL Server 實例會耗用，但實際上並不會使用以有效率方式配置的基礎 CPU、記憶體和磁片。
- **提高效率。** Contoso 必須移除不必要的程式，並簡化開發人員和使用者的流程。 企業需要快速且不浪費時間或金錢，以更快的速度提供客戶需求。 在遷移之後，應該減少或最小化資料庫管理。
- **增加靈活性。** Contoso IT 必須能夠更快因應企業的需求。 其回應速度必須比 marketplace 中的變更更快，才能在全球經濟中實現成功。 它會不得取得或成為商務封鎖程式。
- **尺度.** 隨著企業順利成長，Contoso IT 必須提供以相同步調成長的系統。
- **瞭解成本。** 商務和應用程式擁有者想要知道，在內部部署環境中執行應用程式時，他們將不會陷入高雲端成本。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已針對各種不同的遷移，將目標固定在一起。 這些目標是用來判斷最佳的遷移方法。

| 需求 | 詳細資料 |
| --- | --- |
| **效能** | 遷移之後，Azure 中的應用程式應該在 Contoso 的內部部署環境中具有與應用程式相同的效能功能。 移至雲端並不表示應用程式效能較不重要。 |
| **相容性** | Contoso 必須瞭解其應用程式和資料庫與 Azure 的相容性。 Contoso 也必須瞭解其 Azure 裝載選項。 |
| **資料來源** | 所有資料庫都會移至 Azure，但不會有任何例外狀況。 根據所使用之 SQL 功能的資料庫和應用程式分析，它們會移至平臺即服務 (PaaS) 或基礎結構即服務 (IaaS) 。 所有資料庫都必須移動。 |
| **應用程式** | 您必須盡可能將應用程式移至雲端。 如果無法移動，他們只會透過私人連線，透過 Azure 網路連接到已遷移的資料庫。 |
| **成本** | Contoso 不僅想要瞭解它的遷移選項，也要瞭解與基礎結構移至雲端後的相關成本。 |
| **管理** | 您必須針對各種部門建立資源管理群組，以及使用資源群組來管理所有遷移的資料庫。 所有資源都必須針對計費需求，以部門資訊來標記。 |
| **限制** | 一開始，並非所有執行應用程式的分公司都有 Azure 的直接 Azure ExpressRoute 連結。 這些辦公室必須透過虛擬網路閘道連接。 |

## <a name="solution-design"></a>解決方案設計

Contoso 已使用[Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-services-overview)來執行其數位資產的[遷移評估](https://docs.microsoft.com/azure/cloud-adoption-framework/plan/contoso-migration-assessment)。

![圖表顯示遷移程式。 ](./media/contoso-migration-oss-db-to-azure/migration-process.png)
_圖1：遷移程式。_

### <a name="solution-review"></a>解決方案檢閱

Contoso 會透過比較一份優缺點清單，來評估建議設計。

| 考量 | 詳細資料 |
| --- | --- |
| **優點** | Azure 會在資料庫工作負載中提供單一窗格。 <br><br> 成本會透過 Azure 成本管理和計費來進行監視。 <br><br> 您可以使用 Azure 計費 API 輕鬆地執行商務計費計費。 <br><br> 伺服器和軟體維護將會縮減為僅限 IaaS 架構的環境。 |
| **缺點** | 由於 IaaS 型 Vm 的需求，仍然需要管理這些機器上的軟體。 |

### <a name="budget-and-management"></a>預算和管理

在進行遷移之前，必須先備妥必要的 Azure 結構，以支援解決方案的管理和計費層面。

針對管理需求，已建立數個[管理群組](https://docs.microsoft.com/azure/governance/management-groups/overview)來支援組織結構。

針對計費需求，會將每個 Azure 資源[標記](https://docs.microsoft.com/azure/azure-resource-manager/management/tag-resources)為適當的帳單標記。

### <a name="migration-process"></a>移轉程序

資料移轉遵循標準且可重複使用的模式。 此套裝程式含根據[Microsoft 最佳作法](https://datamigration.microsoft.com/)的下列步驟：

- 預先遷移：
  - **探索：** 清查資料庫資產和應用程式堆疊。
  - **評估：** 評估工作負載和修正建議。
  - **轉換：** 將來源架構轉換為在目標中工作。
- 移轉：
  - **遷移：** 將來源架構、來源資料和物件遷移至目標。
  - **同步處理資料：** 同步處理資料 (以最短的停機時間) 。
  - 轉換 **：** 將來源剪下至目標。
- 遷移後：
  - **補救應用程式：** 反復對應用程式進行任何必要的變更。
  - **執行測試：** 反復執行功能和效能測試。
  - **優化：** 根據測試，解決效能問題，然後重新測試以確認效能改進。
  - **淘汰資產：** 舊的 Vm 和裝載環境會進行備份和淘汰。

#### <a name="step-1-discovery"></a>步驟1：探索

Contoso 使用 Azure Migrate 來呈現所有 Contoso 環境的相依性。 Azure Migrate 自動探索到 Windows 和 Linux 系統上的應用程式元件，並對應服務之間的通訊。 Azure Migrate 也會顯示 Contoso 伺服器、進程、輸入和輸出連線延遲，以及跨 TCP 連線架構的埠之間的連接。 只有在安裝[Microsoft Monitoring Agent](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows)和[Microsoft Dependency Agent](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-hybrid-cloud#install-the-dependency-agent-on-windows)時，才需要 Contoso。

Contoso 已識別出超過300個必須遷移的資料庫實例。 在這些情況下，大約40% 可移至 PaaS 型服務。 在剩餘的60% 中，必須將它們移至以 IaaS 為基礎的方法，其中 VM 執行各自的資料庫軟體。

#### <a name="step-2-application-assessment"></a>步驟2：應用程式評估

評量的結果顯示 Contoso 主要使用 JAVA、PHP 和 Node.js 應用程式。 公司已識別下列應用程式：

- 100 JAVA 應用程式
- 關於 50 Node.js 應用程式
- 大約25個 PHP 應用程式

#### <a name="step-3-database-assessment"></a>步驟3：資料庫評估

當資料庫已清查時，每種資料庫類型都會經過審查，以判斷將它遷移至 Azure 的方法。 下列指導方針已遵循資料庫移轉。

| 資料庫類型 | 詳細資料 | 目標 | 移轉指南 |
| --- | --- | --- | --- |
| **MySQL** | 在遷移之前，所有支援的版本都升級為支援的版本 | 適用於 MySQL 的 Azure 資料庫 (PaaS)  | [輥](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online)
| **PostgreSQL** | 在遷移之前，所有支援的版本都升級為支援的版本 | 適用於 PostgreSQL 的 Azure 資料庫 (PaaS)  | [輥](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online) |
| **MariaDB** | 在遷移之前，所有支援的版本都升級為支援的版本 | 適用於 MariaDB 的 Azure 資料庫 (PaaS)  | [輥](https://datamigration.microsoft.com/scenario/mariadb-to-azuremariadb?step=1) |

#### <a name="step-4-migration-planning"></a>步驟4：遷移規劃

由於資料庫數量龐大，Contoso 會設定專案管理辦公室來追蹤每個資料庫移轉實例。 [責任與責任](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/migration-considerations/assess/)已指派給每個商務和應用程式小組。

Contoso 也已執行[工作負載準備檢查](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/migration-considerations/assess/evaluate)。 本審查已檢查基礎結構、資料庫和網路元件。

#### <a name="step-5-test-migrations"></a>步驟5：測試遷移

遷移準備的第一個部分牽涉到將每個資料庫的測試遷移至預先安裝環境。 為了節省時間，Contoso 已編寫所有作業的腳本來進行遷移，並記錄每個作業的時機。 為了加速遷移，公司識別出可同時執行的遷移作業。

萬一發生某些非預期的失敗，就會針對每個資料庫工作負載識別任何復原程式。

針對以 IaaS 為基礎的工作負載，公司會事先設定所有必要的協力廠商軟體。

在測試遷移之後，Contoso 使用了各種 Azure[成本預估工具](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/migration-considerations/assess/estimate)，以更精確地瞭解遷移的未來營運成本。

#### <a name="step-6-migration"></a>步驟6：遷移

針對生產環境的遷移，Contoso 識別出所有資料庫移轉的時間範圍，以及在週末期間，可充分執行的工作 (在星期五到午夜的午夜，) 最短的停機時間。

### <a name="clean-up-after-migration"></a>移轉之後進行清除

Contoso 已識別出所有資料庫工作負載的保存時間範圍。 當視窗過期時，資源將會從內部部署基礎結構解除配置。 此套裝程式括從內部部署伺服器移除生產資料，以及在最後一個工作負載時間範圍到期時淘汰主控伺服器。

### <a name="review-the-deployment"></a>檢閱部署

對於 Azure 中的遷移後資源，Contoso 必須能執行一切功能並保護其新的基礎結構。

#### <a name="security"></a>安全性

Contoso 必須：

- 確保其新的 Azure 資料庫工作負載是安全的。 如需詳細資訊，請參閱[Azure SQL Database 和 SQL 受控執行個體安全性功能](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview)。
- 檢查防火牆和虛擬網路設定。
- 設定 Azure 私用連結，讓所有資料庫流量保留在 Azure 和內部部署網路中。
- 啟用 Azure 進階威脅防護。

#### <a name="backups"></a>備份

請確定使用異地還原來備份 Azure 資料庫。 如此一來，如果發生區域中斷，備份就可以在配對的區域中使用。

> [!IMPORTANT]
>請確定 Azure 資源具有[資源鎖定](https://docs.microsoft.com/azure/azure-resource-manager/management/lock-resources)，以防止其遭到刪除。 無法還原已刪除的伺服器。

#### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- 許多 Azure 資料庫工作負載可以相應增加或相應減少。 監視伺服器和資料庫效能非常重要，可確保您符合自己的需求，並盡可能保持成本。
- CPU 和儲存體都有相關聯的成本。 有數個定價層可供選擇。 請確定已針對資料工作負載選取適當的價格方案。
- 每個讀取複本都會根據選取的計算和儲存體來計費。
- 使用保留容量來降低成本。

## <a name="conclusion"></a>結論

在本文中，Contoso 已評估、規劃並將其開放原始碼資料庫移轉至 Azure PaaS 和 IaaS 解決方案。
