---
title: 適用于 Teradata 的分析解決方案
description: 使用適用于 Azure 的雲端採用架構，以瞭解使用 Teradata 的分析解決方案。
author: v-hanki
ms.author: brblanch
ms.date: 07/14/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 85dc03afc7ea0d8c931c24009beb220d03b108e5
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86451736"
---
<!-- cSpell:ignore DATEADD DATEDIFF Attunity Teradata Inmon NUSI Informatica Talend BTEQ FASTEXPORT QUALIFY ORC Parquet "Parallel Data Transporter" "Attunity Replicate" -->

# <a name="analytics-solutions-for-teradata"></a>適用于 Teradata 的分析解決方案

Teradata 資料倉儲系統的現有使用者現在想要利用較新環境（例如雲端、IaaS、PaaS）所提供的創新，並將基礎結構維護和平臺開發之類的工作委派給雲端提供者。

雖然 Teradata 和 Azure Synapse Analytics 之間有相似之處，但這兩個 SQL 資料庫的設計都是使用大量平行處理（MPP）技術，在大型資料磁片區上達到高查詢效能，但方法也有一些基本差異：

- 舊版 Teradata 系統通常會使用專屬的硬體安裝在內部部署環境中，而 Azure Synapse Analytics 會使用雲端式儲存體和計算資源。
- 升級 Teradata 設定是牽涉到額外實體硬體和可能冗長的資料庫重新設定的主要工作。 因為儲存體和計算資源在 Azure 環境中是分開的，所以您可以使用彈性的擴充功能，輕鬆地調整這些資源（向上和向下）。
- Azure Synapse 分析可以視需要暫停或調整大小，以降低資源使用率並因此成本。

若要充分發揮這些優點，必須將現有的（或新的）資料和應用程式遷移到 Azure Synapse 分析平臺，而在許多組織中，這包括從舊版內部部署平臺（例如 Teradata）遷移現有的資料倉儲。 概括而言，基本程式將包含下列步驟：

<!-- markdownlint-disable MD033 -->

| 準備 | 遷移 | 移轉後 |
|---|---|---|
| <li> 定義範圍-要遷移的內容 <li> 建立資料和進程以進行遷移的清查 <li> 定義資料模型變更（如果有的話） <li> 識別要使用的適當 Azure （和協力廠商）工具和功能 <li> 在新平臺上及早訓練員工 <li> 設定 Azure 目標平臺 |  <li> 從小型和簡單開始 <li> 盡可能自動化 <li> 使用 Azure 內建的工具和功能來降低遷移的效率 <li> 遷移資料表和視圖的中繼資料 <li> 遷移要維護的歷程記錄資料 <li> 遷移或重構預存程式和商務程式 <li> 遷移或重構 ETL/ELT 累加式載入進程 |  <li> 監視並記載進程的所有階段 <li> 使用已取得的體驗來建立範本以供未來遷移 <li> 視需要重新設計資料模型，並使用新平臺效能和擴充性 <li> 測試應用程式和查詢工具 <li> 基準測試和優化查詢效能 |

## <a name="migration-scope"></a>移轉範圍

### <a name="choose-the-workload-for-the-initial-migration"></a>選擇初始遷移的工作負載

舊版 Teradata 環境通常會隨著時間而演變，以包含多個主題區域和混合工作負載。 在決定初始遷移專案的起始位置時，您應該選擇能夠執行下列動作的區域：

- 藉由快速提供新環境的優勢，證明遷移至 Azure Synapse 分析的可行性。
- 允許內部技術人員取得相關的程式和工具，以便在其他區域的遷移中使用。
- 針對來源 Teradata 環境和目前已準備好的工具和進程，建立用於進一步遷移練習的範本。

適用于從 Teradata 環境進行初始遷移的絕佳候選項目，這會啟用上面的專案，通常是使用資料模型來執行 BI/分析工作負載（也就是不是 OLTP 工作負載），並可使用最少的修改（通常是星型或雪花式架構）來進行遷移。

就大小而言，在初始練習中遷移的資料磁片區必須夠大，才能示範 Azure Synapse 分析環境的功能和優點，同時還能讓時間更短，通常是在 1-10 TB 的範圍內。

初始遷移專案的其中一種可能的方法是將風險降至最低，並減少初始專案的執行時間，就是將遷移範圍限制為僅限資料超市（例如，Teradata 倉儲的 OLAP 資料庫部分）。 根據定義，這種方法會限制遷移的範圍，而且通常可以在短時間內達成，因此可能是很好的起點。 不過，這種方法不會處理更廣泛的主題，例如 ETL 遷移和歷程記錄資料移轉做為初始遷移專案的一部分。 您必須在專案的後續階段中解決這些問題，因為遷移的資料超市層會重新填入建立它們所需的資料和程式。

## <a name="lift-and-shift-as-is-versus-a-phased-approach-incorporating-changes"></a>隨即轉移原樣與合併變更的分階段方法比較

無論預期的遷移的驅動程式和範圍為何，大致來說都有兩種類型的遷移：

### <a name="lift-and-shift"></a>隨即轉移

在此情況下，現有的資料模型（例如星狀架構）會以不變的方式遷移至新的 Azure Synapse 分析平臺。 此處的重點在於將風險降至最低，並藉由減少必須完成才能達成 Azure 雲端環境優勢的工作，來降低遷移所花費的時間。

這適合用於要遷移單一資料超市的現有 Teradata 環境，或是資料已在設計良好的星型或雪花式架構中，或可能有時間壓力可移至更現代化的環境。

### <a name="phased-approach-incorporating-modifications"></a>合併修改的階段式方法

對於舊版倉儲在一段長時間內演變的情況，可能需要重新設計來維護所需的效能層級，或支援新的資料（例如，IoT 串流）。 遷移至 Azure Synapse 分析以取得可擴充雲端環境的優點，可能會被視為重新工程程式的一部分。 這可能包括基礎資料模型的變更（例如，從 Inmon 模型移至資料保存庫）。

建議的方法是一開始將現有的資料模型移至 Azure 環境（如下文所述，在 Azure 中選擇性地使用 VM Teradata 實例），然後使用 azure 環境的效能和彈性來套用重新工程變更，並在適當的情況下使用 Azure 功能進行變更，而不會影響現有的來源系統。

## <a name="using-a-vm-teradata-instance-as-part-of-a-migration"></a>在遷移過程中使用 VM Teradata 實例

從內部部署 Teradata 環境執行遷移的一個選擇性方法，就是使用 Azure 環境，提供便宜的雲端儲存體和彈性的擴充性，在 Azure 中建立 VM 內的 Teradata 實例，並與目標 Azure Synapse 分析環境共存。

透過這種方法，標準 Teradata 公用程式（例如 Teradata Parallel Data Transporter （或 Attunity replication 等協力廠商資料複寫工具）可用來有效率地移動要遷移至 VM 實例的 Teradata 資料表子集，然後所有的遷移工作都可以在 Azure 環境中進行。 這種方法有幾項優點：

- 資料的初始複寫之後，來源系統不會受到遷移工作的影響。
- 熟悉的 Teradata 介面、工具和公用程式可在 Azure 環境中使用。
- 一旦進入 Azure 環境，內部部署來源系統與雲端目標系統之間就不會有任何潛在的網路頻寬可用性問題。
- Azure Data Factory 之類的工具可以有效率地呼叫公用程式，例如 Teradata Parallel Transporter，以快速且輕鬆地遷移資料。
- 遷移程式已協調並完全控制于 Azure 環境中。

## <a name="use-azure-data-factory-to-implement-a-metadata-driven-migration"></a>使用 Azure Data Factory 來執行中繼資料驅動的遷移

藉由使用 Azure 環境中的功能，來自動化和協調遷移程式是合理的。 這種方法也會將對現有 Teradata 環境的影響降至最低（可能已經接近完整容量）。

Azure Data Factory 是雲端式資料整合服務，可讓您在雲端中建立資料驅動的工作流程，以協調和自動化資料移動和資料轉換。 使用 Azure Data Factory，可以建立並排程資料驅動的工作流程 (稱為管線)，它可以從不同的資料存放區擷取資料。 使用計算服務 (例如，Azure HDInsight Hadoop、Spark、Azure Data Lake Analytics 和 Azure Machine Learning) 可以處理或轉換資料。

藉由建立中繼資料來列出要遷移的資料表及其位置，可以使用 Azure Data Factory 的功能來管理和自動化遷移程式的部分。

## <a name="design-differences-between-teradata-and-azure-synapse-analytics"></a>Teradata 與 Azure Synapse Analytics 之間的設計差異

### <a name="separate-databases-versus-schemas"></a>個別資料庫與架構

在 Teradata 環境中，通常會針對整體環境的個別部分定義多個不同的資料庫，例如，資料內嵌和臨時資料表可能會有個別的資料庫、核心倉儲資料表的資料庫，以及資料超市的另一個資料庫（有時稱為「語義層」）。 ETL/ELT 管線之類的處理可能會執行跨資料庫聯結，並且會在這些不同的資料庫之間移動資料。

Azure Synapse 分析環境具有單一資料庫，而架構則用來將資料表分隔成邏輯上分隔的群組。 因此，建議您在目標 Azure Synapse 分析中使用一系列的架構，以模擬將從 Teradata 環境遷移的個別資料庫。 如果架構已在 Teradata 環境中使用，則可能需要使用新的命名慣例將現有的 Teradata 資料表和 views 移至新的環境（例如，將現有的 Teradata 架構和資料表名稱串連到新的 Azure Synapse Analytics 資料表名稱中，並使用新環境中的架構名稱來維護原始的個別資料庫名稱）。 另一個選項是使用基礎資料表上的 SQL views 來維護邏輯結構，但這種方法有一些可能的缺點：

- Azure Synapse 分析中的 Views 是唯讀的，因此必須在基礎基表上進行資料的任何更新。
- 可能已經有一個圖層（或圖層）存在，而且加入額外的一層視圖可能會影響效能。

### <a name="table-considerations"></a>資料表考慮

在不同技術之間遷移資料表時，通常只會有原始資料，以及描述它在兩個環境之間實際移動的中繼資料。 來源系統中的其他資料庫元素（例如索引）不會遷移，因為這些元素可能不需要，也可能在新的目標環境中以不同方式執行。

不過，請務必瞭解效能優化（例如索引在來源環境中的使用方式），因為這項資訊可以提供有用的指示，指出在新的目標環境中可能會將效能優化加入何處。 例如，如果已在來源 Teradata 環境內建立 NUSI，它可能表示非叢集索引應該在遷移的 Azure Synapse 分析內建立，但其他原生效能優化技術（例如資料表複寫）可能會比直接類似的索引建立更適用。

### <a name="high-availability-for-the-database"></a>資料庫的高可用性

Teradata 透過選項支援跨節點的資料複寫 `FALLBACK` ，其中位於指定節點上的資料表資料列會複寫到系統內的另一個節點。 如果發生節點失敗並提供容錯移轉案例的基礎，則此方法可確保資料不會遺失。

Azure SQL Database 中高可用性架構的目標是確保您的資料庫在99.99% 的時間內啟動並執行，而不需擔心維護作業和中斷的影響。 Azure 會自動處理重要的服務工作，例如修補、備份、Windows 和 SQL 升級，以及未計畫的事件，例如基礎硬體、軟體或網路失敗。

Azure Synapse 分析中的資料儲存體會藉由使用快照來自動備份。 這些快照集是服務的內建功能，可建立還原點。 您不需啟用此功能。 使用者目前無法刪除自動還原點，因為服務會使用這些還原點來維護用於還原的 SLA。

Azure SQL 資料倉儲會在一天內製作資料倉儲的快照集，以建立七天內可用的還原點。 此保留期無法變更。 Azure SQL 資料倉儲支援八小時的復原點目標（RPO）。 您可以透過過去七天內所建立的任一個快照集，在主要區域中還原資料倉儲。 如果需要更細微的備份，則可以使用其他使用者定義的選項。

### <a name="unsupported-teradata-table-types"></a>不支援的 Teradata 資料表類型

Teradata 包括針對時間序列和時態性資料的特殊資料表類型支援。 Azure Synapse 分析中不直接支援這些資料表類型的語法和部分函式，但可以將資料移轉到具有適當資料類型的標準資料表中，以及在日期/時間資料行上編制索引或分割。

Teradata 會透過查詢重寫來執行時態性查詢功能，以在時態性查詢中加入其他篩選準則，以限制適用的日期範圍。 如果這項功能目前在來源 Teradata 環境中使用，而且要進行遷移，則必須將這個額外的篩選新增至相關的時態查詢。

Azure 環境也包含特定功能，可在大規模的時間序列資料上進行複雜的分析，稱為時間序列深入解析。 這是針對 IoT 資料分析應用程式，而且可能更適合此使用案例。 如需詳細資訊，請參閱[Azure 時間序列深入解析](https://azure.microsoft.com/services/time-series-insights/)。

### <a name="teradata-data-type-mapping"></a>Teradata 資料類型對應

Azure Synapse Analytics 不直接支援某些 Teradata 資料類型。 下表顯示這些資料類型，以及處理它們的建議方法。 在資料表中，Teradata 資料行類型是儲存在系統目錄內的類型（例如中的 `DBC.ColumnsV` ）。

使用來自 Teradata 目錄資料表的中繼資料，來判斷是否要遷移其中任何資料類型，並在遷移計畫中允許此情況。 例如，如下所示的 SQL 查詢，可以用來尋找任何需要注意的不支援資料類型。

有協力廠商提供工具和服務來自動化遷移作業，包括如上所述的資料類型對應。 此外，如果 Informatica 或 Talend 等協力廠商 ETL 工具已在 Teradata 環境中使用，則可以執行任何必要的資料轉換。

## <a name="sql-dml-syntax-differences"></a>SQL DML 語法差異

Teradata SQL 與 Azure Synapse 分析之間的 SQL 資料操作語言（DML）語法有一些差異，以在遷移時考慮：

- **合格：** Teradata 支援 `QUALIFY` 運算子。 例如：

  `SELECT col1 FROM tab1 WHERE col1='XYZ'`

  協力廠商工具和服務可以自動化資料對應工作：

  `QUALIFY ROW_NUMBER() OVER (PARTITION by col1 ORDER BY col1) = 1;`

  這可在 Azure Synapse 中透過下列語法來達成：

  `SELECT * FROM (SELECT col1, ROW_NUMBER() OVER (PARTITION by col1 ORDER BY col1) rn FROM tab1 WHERE c1='XYZ' ) WHERE rn = 1;`

- **日期算術：** Azure Synapse 有運算子，例如：

  - `DATEADD`和 `DATEDIFF` （可用於 `DATE` 或 `DATETIME` 欄位）。 Teradata 支援日期的直接減法，例如：

    `SELECT DATE1 - DATE2 FROM ...`

  - `LIKE ANY`語法，例如：

    `SELECT * FROM CUSTOMER WHERE POSTCODE LIKE ANY ('CV1%', 'CV2%', CV3%') ;`.

    Azure Synapse 語法中的對等項是：

    `SELECT * FROM CUSTOMER WHERE (POSTCODE LIKE 'CV1%') OR (POSTCODE LIKE 'CV2%') OR (POSTCODE LIKE 'CV3%') ;`

- 視系統設定而定，Teradata 中的字元比較預設可能不會區分大小寫。 在 Azure Synapse 中，這些比較一律會以案例為特定。

## <a name="functions-stored-procedures-triggers-and-sequences"></a>函數、預存程式、觸發程式和順序

從成熟的舊版資料倉儲環境（例如 Teradata）進行遷移時，通常需要將簡單資料表和 views 以外的元素遷移到新的目標環境。 範例包括函式、預存程式、觸發程式和順序。

在準備階段中，您應該建立要遷移的這些物件的清查，並使用在專案計劃中指派的適當資源配置來定義它們的處理方法。

Azure 環境中的設備可能會取代在 Teradata 環境中實作為函式或預存程式的功能。 在此情況下，使用內建的 Azure 設施，而不是將 Teradata 函式進行編碼，通常會更有效率。

如需每個元素的詳細資訊，請參閱下面的。

### <a name="functions"></a>函式

與大部分的資料庫產品一樣，Teradata 也支援 SQL 執行中的系統函數和使用者定義的函數。 當遷移至另一個資料庫平臺（例如 Azure Synapse）時，一般系統功能已正式運作，而且可以在不變更的情況下進行遷移。 有些系統函數可能有稍微不同的語法，但是在此情況下，可能會自動進行必要的變更。

對於沒有對等的系統函數，或針對任意使用者定義的函式，這些可能需要使用目標環境中可用的語言來重新編碼。 Azure Synapse 會使用熱門的 Transact-sql 語言來執行使用者定義函數。

### <a name="stored-procedures"></a>預存程序

大部分的新式資料庫產品都允許將程式儲存在資料庫中。 Teradata 會針對此用途提供 SPL 語言。 預存程式通常包含 SQL 語句和一些程式邏輯，而且可能會傳回資料或狀態。

SQL Azure 資料倉儲也支援使用 T-sql 的預存程式，因此如果有預存程式要遷移，則必須據此重新編碼。

### <a name="triggers"></a>觸發程序

Azure Synapse 中不支援建立觸發程式，但可在 Azure Data Factory 內執行。

### <a name="sequences"></a>序列

Azure Synapse 序列的處理方式類似于 Teradata、使用 `IDENTITY` 資料行或使用 SQL 程式碼來建立數列中的下一個序號。

## <a name="extracting-metadata-and-data-from-a-teradata-environment"></a>從 Teradata 環境中解壓縮中繼資料和資料

### <a name="data-definition-language-ddl-generation"></a>資料定義語言（DDL）產生

如以上所述，您可以編輯現有 `CREATE TABLE` 的 Teradata 和 `CREATE VIEW` 腳本來建立對等定義（如有必要，請使用修改過的資料類型）。 一般而言，這牽涉到移除額外的 Teradata 特有子句（例如 `FALLBACK` ）。

不過，在現有 Teradata 環境內指定資料表和 views 目前定義的所有資訊都會保留在系統目錄資料表中。 這是這項資訊的最佳來源，因為它保證是最新且完整的。 使用者維護的檔可能不會與目前的資料表定義同步。

這項資訊可以透過 views 存取目錄（例如） `DBC.ColumnsV` ，也可以用來 `CREATE TABLE` 針對 Azure Synapse 中的對等資料表產生對等的 DDL 語句。  

協力廠商遷移和 ETL 工具也會使用目錄資訊來達到相同的結果。

### <a name="data-extraction-from-teradata"></a>從 Teradata 的資料提取

使用標準 Teradata 公用程式（例如和），將原始資料從現有的 Teradata 資料表遷移到 `BTEQ` `FASTEXPORT` 。 一般來說，在遷移練習期間，請務必盡可能有效率地將資料解壓縮，而使用最新版本的 Teradata 時，建議的方法是使用 Teradata Parallel Transporter，這將使用多個平行 `FASTEXPORT` 資料流程來達到最佳輸送量。

Teradata Parallel Transporter 可以直接從 Azure Data Factory 呼叫，這是管理資料移轉程式的建議方法（這適用于內部部署中的 Teradata 實例，還是複製到 Azure 環境內的 VM，如上所述）。  
已解壓縮資料的建議資料格式為分隔的文字檔（也稱為逗點分隔值或類似的），或是優化的資料列單欄式（ORC）或 Parquet 檔案。

如需從 Teradata 環境遷移資料和 ETL 程式的詳細資訊，請參閱相關檔章節2.1。 資料移轉 ETL 和從 Teradata 載入。

## <a name="performance-recommendations-for-teradata-migrations"></a>Teradata 遷移的效能建議

效能微調方法的差異

### <a name="data-distribution-options"></a>資料散發選項

Azure 會針對個別資料表啟用資料散發方法的規格。 其目的是要減少在執行查詢時，必須在處理節點之間移動的資料量。  

對於大型資料表-大型資料表聯結，在聯結資料行上散發一或兩個（最好是兩者）資料表的雜湊會確保聯結處理可以在本機執行，因為聯結的資料列將已在相同的處理節點上共置。

另一種達成小型資料表的本機聯結的方式（在星狀架構模型中，維度資料表通常為事實資料表）是跨所有節點複寫較小的維度資料表，因此可確保較大資料表之聯結索引鍵的任何值，在本機都有相符的維度資料列。 如果資料表不是大型的，則複寫維度資料表的額外負荷相對較低。 在此情況下，如上所述的雜湊散發方法較為適當。

### <a name="data-indexing"></a>資料索引

Azure Synapse 提供數個索引選項，但其作業和使用方式與 Teradata 中所實的不同。 如需不同索引選項的詳細資訊，請參閱[Azure Synapse 集區中的設計資料表](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-overview)。

不過，來源 Teradata 環境內的現有索引可以提供有用的指示，指出資料的目前使用方式，並提供在 Azure Synapse 環境中編制索引的候選項目指示。

### <a name="data-partitioning"></a>資料分割

在企業資料倉儲中，事實資料表可以包含許多數十億個數據列和資料分割，這是一種方式，可將這些資料表的維護和查詢分割成不同的部分，以減少所處理的資料量。 資料表的資料分割規格定義于 `CREATE TABLE` 語句中。

每個資料表只能有一個欄位用於資料分割，而這通常是日期欄位，因為有許多查詢會依日期或日期範圍篩選。 如有需要，您可以在初始載入之後變更資料表的資料分割，方法是使用語句來重新建立具有新散發的資料表 `CREATE TABLE AS SELECT` 。 如需 Azure Synapse 中資料分割的詳細討論，請參閱[在 SYNAPSE SQL 集區中分割資料表](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-partition)。

### <a name="data-table-statistics"></a>資料表統計資料

確保資料表的統計資料是最新的。 藉由建立 `COLLECT STATISTICS` ETL/ELT 作業的步驟，或在資料表上啟用自動統計資料收集，即可達成此目的。

### <a name="polybase-for-data-loading"></a>用於載入資料的 PolyBase

PolyBase 是將大量資料載入倉儲最有效率的方法，因為它可以使用平行載入資料流程。

### <a name="use-resource-classes-for-workload-management"></a>使用資源類別來管理工作負載

Azure Synapse 分析會使用資源類別來管理工作負載。 一般來說，大型資源類別會提供更好的個別查詢效能，而較小的資源類別則會啟用更高層級的平行存取。 您可以透過動態管理檢視來監視使用率，以確保能有效率地使用適當的資源。

## <a name="next-steps"></a>後續步驟

如需有關如何執行 Teradata 遷移的詳細資訊，請與您的 Microsoft 帳戶代表內部部署的遷移供應專案交談。
