---
title: Netezza 的 Azure Synapse Analytics 遷移
description: 使用適用于 Azure 的雲端採用架構，瞭解 Netezza 的分析解決方案，並將其遷移至 Azure Synapse Analytics。
author: v-hanki
ms.author: brblanch
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: think-tank
ms.openlocfilehash: ad5a3eb7eb845213764bcc82c698417a7c45eb64
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97015536"
---
<!-- docutune:casing Informatica Talend Inmon Attunity Qlik nzLua CBT CBTs NZPLSQL DELIM TABLENAME ORC Parquet nzsql nzunload mpp -->

<!-- cSpell:ignore Informatica Talend Qlik CBTs NZPLSQL CHARINDEX DATEDIFF DELIM STRPOS TABLENAME nzsql nzunload zonemap -->

# <a name="azure-synapse-analytics-solutions-and-migration-for-netezza"></a>適用于 Netezza 的 Azure Synapse Analytics 解決方案和遷移

由於 IBM 對 Netezza 的支援結束，目前使用 Netezza 資料倉儲系統的許多組織都想要利用創新的雲端、基礎結構即服務，以及較新環境（例如 Azure）中的平臺即服務供應專案。 許多組織已準備好將昂貴的工作（例如基礎結構維護和平臺開發）移至雲端提供者。

Azure Synapse Analytics 是一種無限制的分析服務，可將企業資料倉儲和大型資料分析整合在一起。 它可讓您自由使用無伺服器隨選或布建資源，以大規模地查詢您的詞彙資料。 瞭解當您將舊版 Netezza 系統移轉至 Azure Synapse 時要做什麼規劃。

Netezza 和 Azure Synapse 很類似，因為每個都是 SQL database，其設計目的是使用大量平行處理技術，以在大型資料磁片區上達到高查詢效能。 但這兩個平臺在主要層面上不同：

- 舊版 Netezza 系統是安裝在內部部署環境，並使用專屬硬體。 Azure Synapse 是以雲端為基礎，並使用 Azure 計算和儲存體資源。
- 升級 Netezza 設定是一項主要工作，牽涉到額外的實體硬體以及可能冗長的資料庫重新設定或傾印和重載。 在 Azure Synapse 中，儲存體和計算資源是分開的。 您可以使用 Azure 的彈性擴充性，以獨立相應增加或相應減少。
- 若沒有實體系統可支援，您可以視需要暫停或調整 Azure Synapse 的大小，以降低資源使用率和成本。 在 Azure 中，您可以存取全球可用、高度安全且可擴充的雲端環境，其中包含 Azure Synapse 的支援工具和功能生態系統中。

在本文中，我們將探討架構遷移，並提供可讓您在 Azure Synapse 上取得已遷移 Netezza 資料倉儲和資料超市之對等或更高效能的觀點。 我們考慮特別適用于從現有的 Netezza 環境進行遷移的考慮。

概括而言，遷移套裝程式含下表所列的步驟：

| 準備        | 遷移                             | 移轉後 |
| :----------------- | :----------------------------- | :---------------- |
| <ul><li> 定義範圍：我們要遷移什麼？</li><li>建立要遷移的資料和進程的清查。</li><li>定義任何資料模型變更。</li><li>找出最適合使用的 Azure 和協力廠商工具和功能。</li><li>及早在新平臺上訓練員工。</li><li>設定 Azure 目標平臺。</li></ul> |  <ul><li> 從小型和簡單開始。</li><li>盡可能自動化。</li><li>使用 Azure 內建工具和功能來減少遷移工作。</li><li>遷移資料表和 views 的中繼資料。</li><li>遷移相關的歷程記錄資料。</li><li>遷移或重構預存程式和商務程式。</li><li>遷移或重構 ETL 或 ELT 增量載入進程。</li></ul> | <ul><li> 監視和記錄遷移程式的所有階段。</li><li>使用獲得的體驗來建立範本，以供未來的遷移之用。</li><li>使用新平臺的效能和擴充性，視需要 Reengineer 資料模型。</li><li>測試應用程式和查詢工具。</li><li>基準測試和優化查詢效能。</li></ul> |

當您從舊版 Netezza 環境遷移至 Azure Synapse 時，除了 Netezza 檔中所述的一般主題之外，您還必須考慮一些特定因素。

## <a name="initial-migration-workload"></a>初始遷移工作負載

舊版 Netezza 環境通常會隨著時間而進化，以包含多個主題區域和混合的工作負載。 當您決定要從初始遷移專案開始的位置時，選擇下欄區域有意義：

- 透過快速提供新環境的優點，證明遷移至 Azure Synapse 的可行性。
- 允許內部技術人員體驗新的程式和工具，讓他們可以使用它們來遷移其他區域。
- 根據目前的工具和進程建立範本，以用於從來源 Netezza 環境進行額外的遷移。

從支援這些目標的 Netezza 環境初始遷移的絕佳候選，通常是執行 Power BI/分析工作負載，而非 OLTP 工作負載的一種。 工作負載應該具有可透過最少量的修改（例如星狀或雪花式架構）遷移的資料模型。

針對大小，您在初始練習中遷移的資料量很重要，足以示範 Azure Synapse 環境的功能和優點，並提供簡短的時間來示範價值。 通常符合需求的大小是在 1 tb (TB) 到 10 TB 之間。

初始遷移專案的方法可將風險和實行時間降至最低，以限制遷移至資料超市的範圍。 這種方法是很好的起點，因為它會清楚地限制遷移的範圍，而且通常可以在短的時間點上達到。 資料超市的初始遷移只會解決更廣泛的考慮，例如如何遷移 ETL 和歷程記錄資料。 您必須在後續階段中處理這些區域，並使用建立它們所需的資料和程式來回填已遷移的資料超市層。

## <a name="lift-and-shift-approach-vs-phased-approach"></a>隨即轉移方法與階段式方法

無論您為遷移選擇的驅動程式和範圍為何，都可以選擇兩種一般類型的遷移：

- 隨即 **轉移方法：** 使用這種方法時，現有的資料模型（例如星狀架構）會以不變的方式遷移至新的 Azure Synapse 平臺。 在此案例中，重點在於將風險降至最低，以及藉由減少必須完成以達成移往 Azure 雲端環境之優點所需的工作來進行遷移。

  這種方法適用于現有的 Teradata 環境，在此環境中，將會遷移單一資料超市，以及資料是否已在設計良好的星形或雪花式架構中。 如果您有時間和成本壓力可移至更新式的雲端環境，這種方法也是不錯的選擇。

- **合併修改的階段式方法：** 當舊版倉儲演進過一段時間後，您可能需要 reengineer 資料倉儲來維持所需的效能，或支援新的資料來源（例如 IoT 串流）。 遷移至 Azure Synapse，以瞭解可調整雲端環境的知名優點，可能會被視為重建程式的一部分。 此程式可能包括變更基礎資料模型，例如從 Inmon 模型移至 Azure 資料保存庫。

  我們建議的方法是先將現有的資料模型依原樣移至 Azure。 然後，利用 Azure 服務的效能和彈性來套用重建變更，而不會影響現有的來源系統。

## <a name="metadata-migration"></a>中繼資料移轉

使用 Azure 環境的功能自動化和協調遷移程式是合理的。 這種方法會將現有 Netezza 環境的影響降到最低，而這可能已接近完整容量。

Azure Data Factory 是以雲端為基礎的資料整合服務。 您可以使用 Data Factory 在雲端建立資料驅動的工作流程，以協調和自動化資料移動和資料轉換。 Data Factory 管線可以從不同的資料存放區內嵌資料，然後使用 Azure HDInsight 適用于 Apache Hadoop 和 Apache Spark、Azure Data Lake Analytics 和 Azure Machine Learning 等計算服務來處理和轉換資料。 首先，建立中繼資料來列出您想要遷移的資料表及其位置，然後使用 Data Factory 功能來管理遷移程式。

## <a name="design-differences-between-netezza-and-azure-synapse"></a>Netezza 與 Azure Synapse 之間的設計差異

當您規劃從舊版 Netezza 環境遷移至 Azure Synapse 時，請務必考慮兩個平臺之間的設計差異。

### <a name="multiple-databases-vs-a-single-database-and-schemas"></a>多個資料庫與單一資料庫和架構

在 Netezza 環境中，您可能會有多個個別的資料庫用於整個環境的不同部分。 例如，您可能會有用於資料內嵌和臨時表的個別資料庫、適用于核心倉儲資料表的資料庫，以及另一個適用于資料超市的資料庫，有時稱為「 _語義層_」。 以 Azure Synapse 中的 ETL/ELT 管線處理不同的資料庫，可能需要在不同的資料庫之間執行跨資料庫聯結和移動資料。

Azure Synapse 環境具有單一資料庫。 架構會用來將資料表分成不同的邏輯群組。 建議您在目標 Azure Synapse 中使用一系列的架構，以模仿您從 Netezza 遷移的任何個別資料庫。 如果您使用 Netezza 環境中的架構，您可能需要使用新的命名慣例，將現有的 Netezza 資料表和觀點移至新的環境。 例如，您可能會將現有的 Netezza 架構和資料表名稱串連至新的 Azure Synapse 資料表名稱，然後在新的環境中使用架構名稱，以維護原始個別的資料庫名稱。

另一個選項是在基礎資料表上使用 SQL 視圖來維護邏輯結構。 使用 SQL 視圖有一些可能的缺點：

- Azure Synapse 中的 Views 是唯讀的，因此您必須對基礎基表上的資料進行任何更新。
- 如果已有多層的視圖，則加入另一層的視圖可能會影響效能。

### <a name="tables"></a>資料表

當您在不同的技術之間遷移資料表時，實際上只會將原始資料和在這兩個環境之間描述的中繼資料移動。 您不會從來源系統移轉索引之類的資料庫元素，因為它們可能不需要，或可能在新環境中以不同方式執行。

不過，若要瞭解效能優化（例如索引在來源環境中的使用方式），您可以在新環境中將效能優化的地方提供很有用的指示。 例如，如果來源 Netezza 環境中的查詢經常使用區域對應，您可能會在已遷移的 Azure Synapse 環境中建立非叢集索引，或使用其他原生效能優化技術（例如資料表複寫）來建立類似贊的索引，這是很有利的做法。

<!-- docutune:casing "NZ Toolkit" -->

### <a name="unsupported-netezza-database-object-types"></a>不支援的 Netezza 資料庫物件類型

Netezza 會實作為 Azure Synapse 中不直接支援的某些資料庫物件。 不過，Azure Synapse 提供的方法可用來在新的環境中達到相同的功能，如下列清單所述：

- **區域對應：** 在 Netezza 中，會自動為某些資料行類型建立和維護區域對應。 區域對應會在查詢時用於下列資料行類型，以限制要掃描的資料量：

  - `INTEGER` 長度少於8個位元組的資料行
  - 時態性資料行，包括 `DATE` 、 `TIME` 和 `TIMESTAMP`
  - `CHAR`資料行（如果它們是具體化視圖的一部分並包含在子句中） `ORDER BY`

  您可以使用 nz_zonemap 公用程式，找出有區域對應的資料行。 此公用程式是 NZ 工具組的一部分。

  Azure Synapse 不會使用區域對應，但您可以使用使用者定義的索引類型或資料分割來達到類似的結果。

- 叢集 **基礎資料表 (CBTs) ：** 在 Netezza 中，最常見的 CBT 是包含數十億筆記錄的事實資料表。 掃描這類大型資料表需要較長的處理時間，因為可能需要完整的資料表掃描才能取得相關記錄。 藉由以限制的 CBTs 組織記錄，Netezza 可以將記錄分組在相同或鄰近的範圍中。 此程式也會建立區域對應，藉由減少要掃描的資料量來改善效能。

  在 Azure Synapse 中，您可以透過資料分割或使用其他索引類型來達到類似的結果。

- **具體化視圖：** Netezza 建議使用者在具有許多資料行的大型資料表上建立一個或多個具體化視圖，而且查詢中只會定期使用幾個資料行。 當基表中的資料已更新時，系統會自動維護具體化的視圖。

  目前，Microsoft 在 Azure Synapse 中提供具體化視圖的預覽支援，其功能與 Netezza 相同。

- **資料類型對應：** 大部分的 Netezza 資料類型在 Azure Synapse 中都有直接的對等專案。 下表顯示資料類型和建議用來對應資料類型的方法。

  有些協力廠商提供的工具和服務可以自動化遷移工作，包括資料類型對應。 如果 Netezza 環境中已使用 Informatica 或 Talend 這類協力廠商 ETL 工具，您可以使用此工具來執行所需的任何資料轉換。

- **SQL 資料操作語言 (DML) 語法：** 您應該留意 Netezza SQL 與 Azure Synapse 之間的 SQL DML 語法有一些差異。

  以下是一些重要功能，以及它們的差異：

  - `STRPOS`：在 Netezza 中，此函式會傳回 `STRPOS` 字串內子字串的位置。 Azure Synapse 中的對等專案是 `CHARINDEX` 函數，而且引數的順序會反轉。

    在 Netezza 中：

    `SELECT STRPOS('abcdef', 'def') ...`

    會取代為 Azure Synapse 中的下列程式碼：

    `SELECT CHARINDEX('def', 'abcdef') ...`

  - `AGE`： Netezza 支援 `AGE` 運算子來提供兩個時態值之間的間隔 (例如，時間戳記和日期) 。 例如：

    `SELECT AGE ('23-03-1956', '01-01-2019') FROM ...`

    您可以使用 `DATEDIFF` (記下日期表示順序) ，以在 Azure Synapse 中達成相同結果：

    `SELECT DATEDIFF(day, '1956-03-23', '2019-01-01') FROM ...`

  - `NOW()`： Netezza 會 `NOW()` `CURRENT_TIMESTAMP` 在 Azure Synapse 中用來表示。

## <a name="functions-stored-procedures-and-sequences"></a>函數、預存程式和序列

當您從像是 Netezza 的成熟舊版環境遷移資料倉儲時，您通常需要將簡單資料表和觀點以外的元素遷移到新的目標環境。 Netezza 中您可能需要遷移至 Azure Synapse 的非資料表元素範例有函數、預存程式和序列。 在遷移的準備階段，您應該建立要遷移之物件的清查。 在專案計劃中，定義處理所有物件的方法，並為其遷移配置適當的資源。

您可以在 Azure 環境中找到服務，以取代在 Netezza 環境中實作為函式或預存程式的功能。 通常，使用內建 Azure 功能，而不是在 Netezza 函式中進行編碼，是更有效率的作法。

此外，協力廠商提供的工具和服務可將函式、預存程式及 Netezza 的順序自動化。 範例包括 Qlik sense (先前的 Attunity) 和 WhereScape。

以下是有關遷移函式、預存程式和順序的一些其他資訊：

- **函數：** 如同大多數的資料庫產品，Netezza 在 SQL 執行中支援系統函數和使用者定義函數。 當一般系統函式遷移至另一個資料庫平臺（例如 Azure Synapse）時，這些函式通常會在新的環境中提供，而且可以在不變更的情況下遷移。 如果系統函數在新的環境中有稍微不同的語法，您通常可以將必要的變更自動化。

  您可能需要在新的環境中，對沒有對等專案的任意使用者自訂函數和系統函數進行編碼。 使用新環境中可用的語言。 Netezza 使用者定義函數是使用 nzLua 或 c + + 來撰寫程式碼。 Azure Synapse 會使用熱門的 Transact-sql 語言來執行使用者定義函數。

- **預存程式：** 在大部分的新式資料庫產品中，您可以將程式儲存在資料庫中。 預存程式通常包含 SQL 語句和一些程式邏輯。 它可能也會傳回資料或狀態。

  Netezza 會根據 PL/pgSQL 提供預存程式的 NZPLSQL 語言。 Azure Synapse 支援使用 T-sql 的預存程式。 如果您將預存程式遷移至 Azure Synapse，您必須使用 T-sql 來將它們重設成。

- **順序：** 在 Netezza 中，序列是透過語句建立的命名資料庫物件 `CREATE SEQUENCE` 。 物件可以透過方法提供唯一值 `NEXT()` 。 您可以使用值來產生唯一的數位，作為主鍵值的代理鍵值。

  Azure Synapse 不支援 `CREATE SEQUENCE` 。 在 Azure Synapse 中，系統會使用識別資料行或 SQL 程式碼來處理序列，以建立數列中的下一個序號。

## <a name="metadata-and-data-extraction"></a>中繼資料和資料解壓縮

當您規劃如何從 Netezza 環境中解壓縮中繼資料和資料時，請考慮下列資訊：

- **資料定義語言 (DDL) 產生：**`CREATE TABLE` `CREATE VIEW` 如先前所述，您可以視需要編輯現有的 Netezza 和腳本，以建立對等的定義和修改過的資料類型。 這項工作通常牽涉到移除或修改 Netezza 特定的任何子句，例如 `ORGANIZE ON` 。

  在 Netezza 中，指定目前資料表和 view 定義的資訊會保留在系統目錄資料表中。 系統目錄資料表是資訊的最佳來源，因為資料表可能是最新的，而且已完成。 使用者維護的檔可能不會與目前的資料表定義同步。

您可以使用 nz_ddl_table 之類的公用程式來存取 Netezza 中的系統目錄資料表。 您可以使用資料表來產生 `CREATE TABLE` DDL 語句，然後您可以在 Azure Synapse 中針對對等的資料表進行編輯。 協力廠商的遷移和 ETL 工具也會使用類別目錄資訊來達成相同的結果。

- **資料解壓縮：** 您可以使用標準 Netezza 公用程式（例如 nzsql 和 nzunload），以及使用外部資料表，將原始資料解壓縮，以從現有的 Netezza 資料表遷移至一般的分隔檔案。 使用 gzip 壓縮檔案，然後使用 AzCopy 或 Azure 資料傳輸服務（例如 Azure 資料箱）將檔案上傳至 Azure Blob 儲存體。

  在遷移練習期間，請務必盡可能有效率地將資料解壓縮。 Netezza 的建議方法是使用外部資料表，這也是最快的方法。 您可以平行完成多個解壓縮，以將資料解壓縮的輸送量最大化。

以下是外部資料表解壓縮的簡單範例：

  `CREATE EXTERNAL TABLE '/tmp/export_tab1.CSV' USING (DELIM ',') AS SELECT * from <TABLE-NAME>;`

   如果您有足夠的網路頻寬，您可以使用 Data Factory 進程或協力廠商資料移轉或 ETL 產品，將資料從內部部署 Netezza 系統直接解壓縮到 Azure Synapse 資料表或 Azure 資料儲存體。

   解壓縮資料的建議資料格式是分隔的文字檔 (也稱為 _逗點分隔值_) 、優化的資料列單欄式檔案或 Parquet 檔案。

如需有關從 Netezza 環境遷移資料和 ETL 之程式的詳細資訊，請參閱 Netezza 有關資料移轉 ETL 和載入的檔。

## <a name="performance-tuning-recommendations"></a>效能微調建議

當您從 Netezza 環境移至 Azure Synapse 時，您所使用的許多效能微調概念都很熟悉。

例如，這兩個環境的這些概念都相同：

- 資料散發 colocates 要聯結至相同處理節點的資料。
- 使用特定資料行的最小資料類型可節省儲存空間並加速查詢處理。
- 藉由減少轉換資料以進行比對的需求，確保要聯結的資料行資料類型是完全相同的優化聯結處理。
- 確保統計資料是最新的，可協助優化工具產生最佳執行計畫。

平臺之間的差異在於優化。 在下列效能微調建議清單中，Netezza 和 Azure Synapse 之間的較低層級實施差異，以及您遷移的替代方案，會反白顯示：

- **資料散發選項：** 在 Netezza 和 Azure Synapse 中，您可以使用 `CREATE TABLE` 語句來指定散發定義。 用於 `DISTRIBUTE ON` Netezza 和 `DISTRIBUTION =` Azure Synapse。

   Azure Synapse 提供另一種方法來達成小型資料表/大型資料表聯結的本機聯結，通常稱為星狀架構模型中的 _維度資料表/事實資料表聯結_ 。 方法是在所有節點之間複寫較小的維度資料表，藉此確保較大資料表的聯結索引鍵值，會有可在本機使用的相符維度資料列。 如果資料表很大，則複寫維度資料表的額外負荷相對較低。 在此情況下，最好使用稍早所述的雜湊散發方法。

- **資料索引編制：** Azure Synapse 提供各種使用者可定義的索引編制選項，但是在 Netezza 中，這些選項與系統管理的區域對應的操作和使用方式不同。 若要瞭解 Azure Synapse 中的索引編制選項，請參閱 [Azure SYNAPSE SQL 集區中的索引資料表](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-index)。

   來源 Netezza 環境中現有的系統管理區域對應可提供資料使用方式的實用指示，並提供在 Azure Synapse 環境中建立索引的候選資料行指示。

- **資料分割：** 在企業資料倉儲中，事實資料表可能包含許多數十億個數據列的資料。 資料分割是將這些資料表中的維護和查詢優化的一種方式。 將資料表分割成不同的部分，可減少一次處理的資料量。 資料表的資料分割是在語句中定義 `CREATE TABLE` 。

  每個資料表只能有一個欄位可以用來進行資料分割。 經常用於資料分割的欄位是日期欄位，因為許多查詢都會依日期或日期範圍進行篩選。 您可以在初始載入之後變更資料表的資料分割。 若要變更資料表的資料分割，請使用語句的新散發重新建立資料表 `CREATE TABLE AS SELECT` 。 如需 Azure Synapse 中資料分割的詳細說明，請參閱 [Azure SYNAPSE SQL 集區中](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-partition)的分割資料表。

- **用於載入資料的 PolyBase：** PolyBase 是最有效率的方法，可用來將大量資料載入至倉儲。 您可以使用 PolyBase 來載入平行資料流程中的資料。

- **工作負載管理的資源類別：** Azure Synapse 會使用資源類別來管理工作負載。 一般情況下，大型資源類別可提供更佳的個別查詢效能。 較小的資源類別可提供更高層級的平行存取。 您可以使用動態管理檢視來監視使用狀況，以協助確保有效率地使用適當的資源。

## <a name="next-steps"></a>後續步驟

如需有關執行 Netezza 遷移的詳細資訊，請與您的 Microsoft 帳戶代表討論內部部署的內部部署優惠。
