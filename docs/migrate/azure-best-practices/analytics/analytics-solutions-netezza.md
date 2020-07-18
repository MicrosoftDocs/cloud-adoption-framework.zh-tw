---
title: 適用于 Netezza 的分析解決方案
description: 使用適用于 Azure 的雲端採用架構，以瞭解使用 Netezza 的分析解決方案。
author: v-hanki
ms.author: brblanch
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 4de63bd39c9b5e22d5848532aef956374fa32414
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86451735"
---
<!-- cSpell:ignore Netezza Informatica Talend InMon zonemap CBTs Attunity Wherescape nzlua CBT NZPLSQL DELIM TABLENAME ORC Parquet nzsql nzunload mpp -->

# <a name="analytics-solutions-for-netezza"></a>適用于 Netezza 的分析解決方案

由於 IBM 的支援結束，Netezza 資料倉儲系統的許多現有使用者現在想要利用較新環境（例如雲端、IaaS 或 PaaS）所提供的創新，並將基礎結構維護和平臺開發之類的工作委派給雲端提供者。

雖然 Netezza 和 Azure Synapse Analytics 之間有相似之處，但這兩者都是 SQL 資料庫，其設計目的是要使用大量平行處理（MPP）技術，以在非常大的資料磁片區上達到高查詢效能，但方法也有一些基本差異：

- 舊版 Netezza 系統是使用專屬的硬體安裝在內部部署環境，而 Azure Synapse Analytics 則是使用 Azure 儲存體和計算資源，以雲端為基礎。
- 升級 Netezza 設定是一個主要工作，涉及額外的實體硬體以及可能冗長的資料庫重新設定或傾印和重載。 因為儲存體和計算資源在 Azure 環境中是分開的，所以您可以使用彈性的擴充功能，輕鬆地相應增加（向上和向下）。
- Azure Synapse 分析可以視需要暫停或調整大小，以降低資源使用率並因此成本。 Microsoft Azure 是全球可用、高度安全、可調整的雲端環境，其中包含支援工具和功能生態系統內的 Azure Synapse。

以下探討架構遷移的觀點，以取得您在 Azure Synapse 上遷移的 Netezza 資料倉儲和資料超市的對等或更佳效能。 本檔中所包含的主題特別適用于從現有的 Netezza 環境進行遷移。

## <a name="design-considerations"></a>設計考量

### <a name="migration-scope"></a>移轉範圍

### <a name="preparation-for-migration"></a>準備進行遷移

從 Netezza 環境進行遷移時，除了「第1節：設計和效能」檔中所述的一般主題以外，還有一些必須列入考慮的特定主題。

### <a name="choosing-the-workload-for-the-initial-migration"></a>選擇初始遷移的工作負載

舊版 Netezza 環境通常會隨著時間而演變，以包含多個主題區域和混合工作負載。 在決定初始遷移專案的起始位置時，選擇能夠執行下列動作的區域是合理的：

- 藉由快速提供新環境的優點，證明遷移至 Azure Synapse 的可行性。
- 讓內部技術人員能夠取得相關的程式和工具，以便在其他區域的遷移中使用。
- 針對來源 Netezza 環境和目前已準備好的工具和進程，建立用於進一步遷移練習的範本。

從 Netezza 環境進行初始遷移的絕佳候選項目，這通常是執行 BI/分析工作負載，而不是 OLTP 工作負載，而資料模型可使用最少的修改來遷移，通常是開始或雪花式架構。

就大小而言，在初始練習中遷移的資料磁片區必須夠大，才能示範 Azure Synapse 環境的功能和優點，同時還能讓時間更短地示範，通常是在 1-10 TB 的範圍內。

初始遷移專案的一種可行方法，就是將風險降至最低並減少初始專案的執行時間，只是將遷移範圍將到資料超市。 根據定義，這種方法會限制遷移的範圍，而且通常可以在短時間內達成，因此可能是很好的起點。 不過，這不會處理更廣泛的主題，例如 ETL 遷移和歷程記錄資料移轉做為初始遷移專案的一部分。 這些主題必須在專案的後續階段中解決，因為遷移的資料超市層會重新填入建立它們所需的資料和進程。

### <a name="lift-and-shift-as-is-versus-a-phased-approach-incorporating-changes"></a>隨即轉移原樣與合併變更的分階段方法比較

無論預定遷移的驅動程式和範圍為何，通常會有兩種類型的遷移：

- 隨即**轉移：** 在此情況下，現有的資料模型（例如星狀架構）會以不變的方式遷移至新的 Azure Synapse 平臺。 此處的重點在於將風險降至最低，並藉由減少必須完成才能達成 Azure 雲端環境優勢的工作，來降低遷移所花費的時間。

  這種方法適合用於遷移單一資料超市的現有 Netezza 環境，或資料已在設計良好的星型或雪花式架構中，或需要花費時間和成本壓力來移至更現代化的雲端環境。

- **合併修改的階段式方法：** 當舊版倉儲在一段時間後演變時，您可能需要重新設計它來維護所需的效能，或支援新的資料來源，例如 IoT 串流。 遷移至 Azure Synapse 以取得可調整雲端環境的知名優點，可能會被視為重新工程程式的一部分。 此流程可能包括變更基礎資料模型，例如從 Inmon 模型移至 Azure 資料保存庫。

  建議的方法是一開始將現有的資料模型移至 Azure，然後利用 Azure 的效能和彈性來套用重新工程變更，並在適當的情況下使用 Azure 功能來進行變更，而不會影響現有的來源系統。

### <a name="use-azure-data-factory-to-implement-a-metadata-driven-migration"></a>使用 Azure Data Factory 來執行中繼資料驅動的遷移

藉由使用 Azure 環境中的功能，來自動化和協調遷移程式是合理的。 這種方法也會將對現有 Netezza 環境的影響降至最低（可能已經接近完整容量）。

Azure Data Factory 是雲端式資料整合服務，可讓您在雲端中建立資料驅動的工作流程，以協調和自動化資料移動和資料轉換。 使用 Azure Data Factory，可以建立並排程資料驅動的工作流程 (稱為管線)，它可以從不同的資料存放區擷取資料。 使用計算服務 (例如，Azure HDInsight Hadoop、Spark、Azure Data Lake Analytics 和 Azure Machine Learning) 可以處理或轉換資料。

藉由建立中繼資料來列出要遷移的資料表和其位置，您可以使用 Azure Data Factory 功能來管理遷移程式。

### <a name="design-differences-between-netezza-and-azure-synapse"></a>Netezza 與 Azure Synapse 之間的設計差異

**多個資料庫與單一資料庫和架構：**

在 Netezza 環境中，有時會有多個不同的資料庫供整體環境的個別部分使用。 例如，資料內嵌和臨時資料表可能會有個別的資料庫、核心倉儲資料表的資料庫，以及資料超市的另一個資料庫（有時稱為「語義層」）。 ETL/ELT 管線之類的處理可能會執行跨資料庫聯結，並且會在這些不同的資料庫之間移動資料。

Azure Synapse 環境有單一資料庫，而架構則用來將資料表分隔成邏輯上分隔的群組。 因此，建議您在目標 Azure Synapse 中使用一系列的架構，以模擬將從 Netezza 環境遷移的任何不同資料庫。 如果架構正在 Netezza 環境中使用，則您可能需要使用新的命名慣例，將現有的 Netezza 資料表和 views 移至新的環境（例如，將現有的 Netezza 架構和資料表名稱串連到新的 Azure Synapse 資料表名稱，並在新的環境中使用架構名稱來維護原始的個別資料庫名稱）。 另一個選項是使用基礎資料表上的 SQL views 來維護邏輯結構，但這種方法有一些可能的缺點：

- Azure Synapse 中的 Views 是唯讀的，因此，必須在基礎基表上進行資料的任何更新。
- 可能已經有一個圖層（或圖層）存在，而且加入額外的一層視圖可能會影響效能。

**資料表考慮：**

在不同技術之間遷移資料表時，通常只會在兩個環境之間實際移動的原始資料（以及描述它的中繼資料）。 來源系統（例如索引）中的其他資料庫元素不會遷移，因為可能不需要這些專案，或在新的目標環境中可能會以不同的方式執行。

不過，請務必瞭解效能優化（例如索引在來源環境中的使用方式），因為這項資訊可以提供有用的指示，指出在新的目標環境中可能會將效能優化加入何處。 例如，如果來源 Netezza 環境中的查詢經常使用區域對應，則可能表示非叢集索引應該在遷移的 Azure Synapse 內建立，但其他原生效能優化技術（例如資料表複寫）可能會更適用于類似的索引建立。

<!-- docsTest:ignore "NZ Toolkit" -->

**不支援的 Netezza 資料庫物件類型：**

Netezza 會在 Azure Synapse 中執行一些不直接支援的資料庫物件，但通常有方法可以在新環境中達到相同的功能：

- **區域對應：** 在 Netezza 中，會針對某些資料行類型自動建立和維護區域對應，並在查詢時用來限制要掃描的資料量。 它們是在下列資料行類型上建立的：

  - `INTEGER`長度為8個位元組或更少的資料行。
  - 時態性資料行（例如 `DATE` 、 `TIME` 和 `TIMESTAMP` ）。
  - `CHAR`資料行，如果這些是具體化視圖的一部分，並在子句中提及 `ORDER BY` 。 您可以使用 nz_zonemap 公用程式（NZ 工具組的一部分）找出哪些資料行具有區域對應。

  Azure Synapse 不包含區域對應，但可使用其他（使用者定義）索引類型或資料分割來達成類似的結果。

- 叢集**基表（CBT）：** 在 Netezza 中，CBTs 最常用於事實資料表，其中包含數十億筆記錄。 掃描這類龐大的資料表需要大量的處理時間，因為需要完整資料表掃描才能取得相關記錄。 透過在限制的 CBT 上組織記錄可讓 Netezza 將記錄群組在相同或附近的範圍中，而此程式也會建立區域對應，藉由減少要掃描的資料量來改善效能。

  在 Azure Synapse 中，您可以使用資料分割或使用其他索引來達到類似的效果。

- **具體化視圖：** Netezza 支援具體化視圖，並建議在具有許多資料行的大型資料表上建立其中一個或多個，其中只有幾個資料行經常用於查詢中。 當基表中的資料是更新時，系統會自動維護具體化 views。 從2019日起，Microsoft 已宣佈 Azure Synapse 將支援具有與 Netezza 相同功能的具體化 views。 這項功能現已提供預覽。

- **Netezza 資料類型對應：** 大部分的 Netezza 資料類型在 Azure Synapse 中具有直接的對等用法。 以下表格顯示這些資料型別，以及對應這些類型的建議方法。

  有協力廠商提供工具和服務來自動化遷移作業，包括如上所述的資料類型對應。 此外，如果 Informatica 或 Talend 等協力廠商 ETL 工具已在 Netezza 環境中使用，則可以執行任何必要的資料轉換。

- **SQL DML 語法差異：** Netezza SQL 與 Azure Synapse 之間的 SQL 資料操作語言（DML）語法有一些差異，以在遷移時留意：

  <!-- TODO: This query should probably be a code snippet that the user can copy and use. -->

  ![SQL 查詢](../../../_images/analytics/sql-query-netezza.png)

### <a name="functions-stored-procedures-and-sequences"></a>函數、預存程式和順序

從成熟的舊版資料倉儲環境（例如 Netezza）進行遷移時，通常需要將簡單資料表和 views 以外的元素遷移到新的目標環境。 在 Netezza 中的範例包括函式、預存程式和順序。 在準備階段中，您應該建立要遷移的這些物件的清查，並使用在專案計劃中指派的適當資源配置來定義它們的處理方法。

這可能是 Azure 環境中的一些功能，會取代在 Netezza 環境中實作為函式或預存程式的功能。 在此情況下，使用內建的 Azure 設施，而不是將 Netezza 函式進行編碼，通常會更有效率。

協力廠商提供的工具和服務可自動進行這些作業（如需範例，請參閱 Attunity 或 Wherescape 遷移產品）。

- **函數：** 與大部分的資料庫產品一樣，Netezza 也支援 SQL 執行中的系統函數和使用者定義的函數。 當遷移至另一個資料庫平臺（例如 Azure Synapse）時，一般系統功能已正式運作，而且可以在不變更的情況下進行遷移。 有些系統函數可能有稍微不同的語法，但是在此情況下，可能會自動進行必要的變更。

  對於沒有對等的系統函數，對於任意使用者定義的函式，您可能需要使用目標環境中的可用語言來重新編碼這些函數。 Netezza 使用者定義函式是以 nzlua 或 c + + 語言編碼，而 Azure Synapse 會使用常用的 Transact-sql 語言來執行使用者定義函數。

- **預存程式：** 大部分的新式資料庫產品都允許將程式儲存在資料庫中。 Netezza 會針對此用途提供 NZPLSQL 語言。 NZPLSQL 是以于 postgresql PL/pgSQL 為基礎。 預存程式通常包含 SQL 語句和一些程式邏輯，而且可能會傳回資料或狀態。

  Azure SQL 資料倉儲也支援使用 T-sql 的預存程式，因此如果您要遷移預存程式，則必須據此重新編碼。

- **順序：** 在 Netezza 中，序列是透過 create sequence 所建立的已命名資料庫物件，可以透過 next value for method 提供唯一的值。 這些可以用來產生唯一的數位，可用來做為主鍵值的代理金鑰值。

  在 Azure Synapse 中，沒有 `CREATE SEQUENCE` ，因此序列是透過使用識別欄位來處理，或使用 SQL 程式碼來建立數列中的下一個序號。

### <a name="extracting-metadata-and-data-from-a-netezza-environment"></a>從 Netezza 環境中解壓縮中繼資料和資料

- **產生 DDL：** 您可以編輯現有的 Netezza create table 和 create view script 來建立對等定義（如有必要，請使用修改過的資料類型），如上所述。 一般而言，這牽涉到移除或修改任何額外的 Netezza 特定子句（例如 `ORGANIZE ON` ）。

  不過，在現有 Netezza 環境內指定資料表和 views 目前定義的所有資訊都會保留在系統目錄資料表中。 這是此資訊的最佳來源，因為它系結至最新狀態並已完成。 使用者維護的檔可能不會與目前的資料表定義同步。

  這項資訊可以透過 nz_ddl_table 的公用程式來存取，而且可以用來產生 create table DDL 語句，然後在 Azure Synapse 中針對對等資料表進行編輯。

  協力廠商遷移和 ETL 工具也會使用目錄資訊來達到相同的結果。

- **從 Netezza 進行資料解壓縮：** 要從現有的 Netezza 資料表中遷移的原始資料，可以使用標準 Netezza 公用程式（例如 nzsql、nzunload 和透過外部資料表）解壓縮到以一般分隔的檔案。 這些檔案可以使用 gzip 進行壓縮，並透過 AzCopy 或使用 Azure 資料傳輸設施（例如 Azure 資料箱）上傳至 Azure Blob 儲存體。

  在遷移練習期間，請務必盡可能有效率地解壓縮資料。 建議的 Netezza 方法是使用外部資料表方法，這是最快速的方法。 可以平行執行多個抽取，以最大化資料解壓縮的輸送量。 外部資料表解壓縮的簡單範例如下所示：

  `CREATE EXTERNAL TABLE '/tmp/export_tab1.CSV' USING (DELIM ',') AS SELECT * from <TABLENAME>;`

如果有足夠的網路頻寬，您可以使用 Azure Data Factory 處理常式或協力廠商資料移轉或 ETL 產品，將資料直接從內部部署 Netezza 系統解壓縮到 Azure Synapse 資料表或 Azure 資料儲存體。

已解壓縮資料的建議資料格式為分隔的文字檔（也稱為逗點分隔值或類似的），或是優化的資料列單欄式（ORC）或 Parquet 檔案。

如需從 Netezza 環境遷移資料和 ETL 程式的詳細資訊，請參閱相關聯的檔 < 第2.1 節：資料移轉 ETL 和載入 Netezza。

### <a name="performance-recommendations-for-netezza-migrations"></a>Netezza 遷移的效能建議

- **效能微調方法概念的相似之處：** 從 Netezza 環境中移動時，許多 Azure SQL 資料倉儲的效能微調概念都很熟悉。 例如：

  - 使用資料散發來共置要聯結至相同處理節點的資料。
  - 針對給定資料行使用最小的資料類型，將會節省儲存空間並加速查詢處理。
  - 確保要聯結之資料行的資料類型完全相同，將可減少轉換資料以進行比對的需求，藉此優化聯結處理。
  - 確保統計資料是最新的，可協助優化工具產生最佳的執行計畫。

- **效能微調方法的差異：** 本節將重點放在 Netezza 與 Azure Synapse 之間的較低層級執行差異，以進行效能調整。

- **資料散發選項：** `CREATE TABLE`Netezza 和 Azure Synapse 中的語句都允許透過 `DISTRIBUTE ON` 適用于 Netezza 和 Azure Synapse 的散發定義進行指定 `DISTRIBUTION =` 。

  相較于 Netezza，Azure Synapse 提供額外的方法來達成小型資料表的本機聯結-大型資料表聯結（通常是開始架構模型中的維度資料表），這是跨所有節點複寫較小的維度資料表，因此可確保較大資料表的聯結索引鍵的任何值，在本機都有相符的維度資料列。 如果資料表不大，複寫維度資料表的額外負荷會相對較低。 在此情況下，如上所述的雜湊散發方法較為適當。

- **資料索引：** Azure Synapse 提供許多使用者可定義的索引選項，但在 Netezza 中，系統管理的區域對應的作業和使用方式不同。 瞭解不同的索引選項，如中所述 <!-- TODO verify link: https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-datawarehouse-tables-index -->

  來源 Netezza 環境中現有的系統管理區域對應，可以提供資料目前使用方式的實用指示，並提供在 Azure Synapse 環境中編制索引的候選資料行指示。

- **資料分割：** 在企業資料倉儲中，事實資料表可以包含許多數十億個數據列和資料分割，這是一種方式，可將這些資料表分割成不同的部分，以減少所處理的資料量，藉此優化這些資料表的維護和查詢。 資料表的資料分割規格是在 create table 語句中定義。

  每個資料表只能有一個欄位用於資料分割，而這通常是日期欄位，因為有許多查詢會依日期或日期範圍篩選。 如有必要，您可以在初始載入之後變更資料表的資料分割，方法是使用語句來重新建立具有新散發的資料表 `CREATE TABLE AS SELECT` 。 請參閱 <!-- TODO verify link https://docs.microsoft.com/en-us/azure/sql-datawarehouse/sql-data-warehouse-tables-partition --> 如需 Azure Synapse 中資料分割的詳細討論。

- **用於資料載入的 PolyBase：** PolyBase 是將大量資料載入倉儲中最有效率的方法，因為它可以使用平行載入資料流程

- **使用資源類別來管理工作負載：** Azure Synapse 會使用資源類別來管理工作負載。 一般來說，大型資源類別會提供更好的個別查詢效能，而較小的資源類別則會啟用更高的平行存取層級。 您可以透過動態管理檢視來監視使用率，以確保能有效率地使用適當的資源。

## <a name="next-steps"></a>後續步驟

如需有關如何執行 Netezza 遷移的詳細資訊，請與您的 Microsoft 帳戶代表內部部署的遷移供應專案交談。
