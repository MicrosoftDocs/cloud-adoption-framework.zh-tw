---
title: 適用于 Teradata 的 Azure Synapse Analytics 遷移
description: 使用適用于 Azure 的雲端採用架構，瞭解 Teradata 和遷移至 Azure Synapse Analytics 的分析解決方案。
author: v-hanki
ms.author: brblanch
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: think-tank
ms.openlocfilehash: 9fd53a221a1763b423e9439900b4cb785714db95
ms.sourcegitcommit: c167c45b66cc7324b60c88b8b7aac439f956b65d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2021
ms.locfileid: "102208127"
---
<!-- cSpell:ignore DATEADD DATEDIFF Inmon NUSI Informatica Talend BTEQ FASTEXPORT QUALIFY ORC Parquet "Parallel Data Transporter" Attunity "Qlik Replicate" -->

# <a name="azure-synapse-analytics-solutions-and-migration-for-teradata"></a>適用于 Teradata 的 Azure Synapse Analytics 解決方案和遷移

許多組織已準備好將昂貴的資料倉儲工作（例如基礎結構維護和平臺開發）移至雲端提供者。 組織現在希望利用創新的雲端、基礎結構即服務，以及較新環境（例如 Azure）中的平臺即服務供應專案。

Azure Synapse Analytics 是一項無限制的分析服務，可將企業資料倉儲和大型資料分析整合在一起。 它可讓您自由使用無伺服器隨選或布建資源，以大規模地查詢您的詞彙資料。 瞭解當您將舊版 Teradata 系統移轉至 Azure Synapse 時要做什麼規劃。

雖然 Teradata 和 Azure Synapse 很類似，但它們都是設計來使用大量平行處理技術的 SQL 資料庫，以達到大型資料磁片區的高查詢效能，但有一些基本差異：

- 舊版 Teradata 系統是安裝在內部部署環境，並使用專屬硬體。 Azure Synapse 是以雲端為基礎，並使用 Azure 計算和儲存體資源。
- 升級 Teradata 設定是一項主要工作，牽涉到額外的實體硬體以及可能冗長的資料庫重新設定或傾印和重載。 在 Azure Synapse 中，計算和儲存體資源是分開的，因此您可以使用 Azure 的彈性擴充性來輕鬆地相應增加或減少。
- 若沒有實體系統可支援，您可以視需要暫停或調整 Azure Synapse 的大小，以降低資源使用率和成本。 在 Azure 中，您可以存取全球可用、高度安全且可擴充的雲端環境，其中包含 Azure Synapse 的支援工具和功能生態系統中。

在本文中，我們將探討架構遷移，其目標是在 Azure Synapse 上取得已遷移 Teradata 資料倉儲和資料超市的對等或更高的效能。 我們考慮特別適用于從現有的 Teradata 環境進行遷移的考慮。

概括而言，遷移套裝程式含下表所列的步驟：

| 準備        | 遷移                             | 移轉後 |
| :----------------- | :----------------------------- | :---------------- |
| <li> 定義範圍：我們要遷移什麼？ <li> 建立要遷移的資料和進程的清查。 <li> 定義任何資料模型變更。 <li> 找出最適合使用的 Azure 和協力廠商工具和功能。 <li> 及早在新平臺上訓練員工。 <li> 設定 Azure 目標平臺。</li> |  <li> 從小型和簡單開始。 <li> 盡可能自動化。 <li> 使用 Azure 內建工具和功能來減少遷移工作。 <li> 遷移資料表和 views 的中繼資料。 <li> 遷移相關的歷程記錄資料。 <li> 遷移或重構預存程式和商務程式。 <li> 遷移或重構 ETL/ELT 增量載入進程。</li> | <li> 監視和記錄遷移程式的所有階段。 <li> 使用獲得的體驗來建立範本，以供未來的遷移之用。 <li> 使用新平臺的效能和擴充性，視需要 Reengineer 資料模型。 <li> 測試應用程式和查詢工具。 <li> 基準測試和優化查詢效能。</li> |

當您從舊版 Teradata 環境遷移至 Azure Synapse 時，除了 Teradata 檔中所述的一般主題之外，您還必須考慮一些特定因素。

## <a name="initial-migration-workload"></a>初始遷移工作負載

舊版 Teradata 環境通常會隨著時間而進化，以包含多個主題區域和混合的工作負載。 當您決定要從初始遷移專案開始的位置時，選擇下欄區域有意義：

- 透過快速提供新環境的優點，證明遷移至 Azure Synapse 的可行性。
- 允許內部技術人員體驗新的程式和工具，讓他們可以使用它們來遷移其他區域。
- 根據目前的工具和進程建立範本，以在來源 Teradata 環境的其他遷移中使用。

從 Teradata 環境初始遷移的絕佳候選項，是執行 Power BI/分析工作負載而非 OLTP 工作負載的絕佳候選。 工作負載應該具有可透過最少量的修改（例如星狀或雪花式架構）遷移的資料模型。

針對大小，您在初始練習中遷移的資料量很重要，足以示範 Azure Synapse 環境的功能和優點，並提供簡短的時間來示範價值。 通常符合需求的大小介於1到 10 tb (TB) 。

初始遷移專案的方法可將風險和實行時間降至最低，以限制遷移至資料超市的範圍。 在 Teradata 中，有一個很好的例子，就是 Teradata 資料倉儲的 OLAP 資料庫部分。 這種方法是很好的起點，因為它會限制遷移的範圍，而且通常可以在短時間點上達到。

資料超市的初始遷移範圍只會解決更廣泛的考慮，例如如何遷移 ETL 和歷程記錄資料。 您必須在後續階段中處理這些區域，並使用建立它們所需的資料和程式來回填已遷移的資料超市層。

## <a name="lift-and-shift-approach-vs-phased-approach"></a>隨即轉移方法與階段式方法

無論您為遷移選擇的驅動程式和範圍為何，都可以選擇兩種一般類型的遷移：

- 隨即 **轉移方法：** 使用這種方法時，現有的資料模型（例如星狀架構）會以不變的方式遷移至新的 Azure Synapse 平臺。 重點在於降低風險，並藉由減少達成移往 Azure 雲端環境所需的工作所需的時間來進行遷移。

  這種方法適用于現有的 Teradata 環境，在此環境中，將會遷移單一資料超市，以及資料是否已在設計良好的星星或雪花式架構中。 如果您有時間和成本壓力可移至更新式的雲端環境，這種方法也是不錯的選擇。

- **合併修改的階段式方法：** 如果您的舊版倉儲演進過了一段時間，您可能需要 reengineer 資料倉儲來維持所需的效能，或支援新的資料來源（例如 IoT 串流）。 遷移至 Azure Synapse，以瞭解可調整雲端環境的知名優點，可能會被視為重建程式的一部分。 此程式可能包括變更基礎資料模型，例如從 Inmon 模型移至 Azure 資料保存庫。

  我們建議的方法是先將現有的資料模型依原樣移至 Azure。 然後，利用 Azure 服務的效能和彈性來套用重建變更，而不會影響現有的來源系統。

## <a name="virtual-machine-colocation-to-support-migration"></a>支援遷移的虛擬機器共置

從內部部署 Teradata 環境遷移的選擇性方法，可在 Azure 中利用經濟實惠的雲端儲存體和彈性的擴充性。 首先，在 Azure 虛擬機器上建立與目標 Azure Synapse 環境共存的 Teradata 實例。 然後，您會使用標準的 Teradata 公用程式，例如 Teradata Parallel Transporter 或協力廠商資料複寫工具（例如 Qlik sense 複寫）（例如 (複寫）（先前為 Attunity) ），以有效率地將您要遷移的 Teradata 資料表子集移至 VM 實例。 所有的遷移工作都是在 Azure 環境中進行。

這種方法有幾項優點：

- 在資料的初始複寫之後，來源系統不會受到其他遷移工作的影響。
- 您可以在 Azure 環境中使用熟悉的 Teradata 介面、工具和公用程式。
- 遷移至 Azure 環境之後，您就不會有內部部署來源系統與雲端目標系統之間的網路頻寬可用性任何潛在問題。
- Azure Data Factory 之類的工具可有效率地呼叫公用程式，例如 Teradata Parallel Transporter，以快速且輕鬆地遷移資料。
- 您可以從 Azure 環境內完全協調和控制遷移程式。

## <a name="metadata-migration"></a>中繼資料移轉

使用 Azure 環境的功能自動化和協調遷移程式是合理的。 這種方法會將對現有 Teradata 環境的影響降到最低，但可能已接近完整容量。

Azure Data Factory 是以雲端為基礎的資料整合服務。 您可以使用 Data Factory 在雲端中建立資料驅動的工作流程，以協調及自動進行資料移動和資料轉換。 Data Factory 管線可以從不同的資料存放區內嵌資料。 然後，他們會使用計算服務（例如適用于 Apache Hadoop 的 Azure HDInsight 和 Apache Spark、Azure Data Lake Analytics 和 Azure Machine Learning）來處理及轉換資料。

首先，建立中繼資料來列出您想要遷移的資料表及其位置。 然後，使用 Data Factory 功能來管理遷移程式。

## <a name="design-differences-between-teradata-and-azure-synapse"></a>Teradata 與 Azure Synapse 之間的設計差異

當您規劃從舊版 Teradata 環境遷移至 Azure Synapse 時，請務必考慮兩個平臺之間的設計差異。

### <a name="multiple-databases-vs-a-single-database-and-schemas"></a>多個資料庫與單一資料庫和架構

在 Teradata 環境中，您可能會有多個個別的資料庫用於整個環境的不同部分。 例如，您可能會有用於資料內嵌和臨時表的個別資料庫、適用于核心倉儲資料表的資料庫，以及資料超市的另一個資料庫 (有時稱為 *語義層*) 。 以 Azure Synapse 中的 ETL/ELT 管線處理不同的資料庫，可能需要在不同的資料庫之間執行跨資料庫聯結和移動資料。

Azure Synapse 環境具有單一資料庫。 架構會用來將資料表分成不同的邏輯群組。 建議您在 Azure Synapse 中使用一組架構，以模擬您從 Teradata 遷移的任何不同資料庫。

如果您使用 Teradata 環境中的架構，您可能需要使用新的命名慣例，將現有的 Teradata 資料表和觀點移至新的環境。 例如，您可能會將現有的 Teradata 架構和資料表名稱串連至新的 Azure Synapse 資料表名稱，然後在新的環境中使用架構名稱，以維護原始個別的資料庫名稱。

另一個選項是在基礎資料表上使用 SQL 視圖，以維護其邏輯結構。 使用 SQL 視圖有一些可能的缺點：

- Azure Synapse 中的 Views 是唯讀的，因此您必須對基礎基表上的資料進行任何更新。
- 如果已有多層的視圖，則加入另一層的視圖可能會影響效能。

### <a name="tables"></a>資料表

當您在不同的技術之間遷移資料表時，實際上只會將原始資料和在這兩個環境之間描述的中繼資料移動。 您不會從來源系統移轉索引之類的資料庫元素，因為它們可能不需要，或可能在新的環境中以不同方式執行。

不過，若要瞭解效能優化（例如索引在來源環境中的使用方式），您可以在新環境中將效能優化的地方提供很有用的指示。 例如，如果在來源 Teradata 環境中建立了非唯一的次要索引，您可能會發現在已遷移的 Azure Synapse 環境中建立非叢集索引，或使用其他原生效能優化技術（例如資料表複寫），最好是建立類似的索引。

### <a name="high-availability-database"></a>高可用性資料庫

Teradata 透過選項支援跨節點的資料複寫 `FALLBACK` 。 實際位於節點上的資料表資料列會複寫到系統內的另一個節點。 這種方法可保證當節點失敗時，資料不會遺失，而且會提供容錯移轉案例的基礎。

Azure SQL Database 中高可用性架構的目標是保證您的資料庫已啟動並執行99.99% 的時間。 您不需要考慮維護作業和中斷可能會如何影響您的工作負載。 Azure 會自動處理重要的服務工作，例如修補、備份、Windows 和 SQL 升級，以及基礎硬體、軟體或網路失敗等非計畫事件。

快照集是服務的內建功能，可在 Azure Synapse 中建立還原點。 快照集會針對 Azure Synapse 中的資料儲存體提供自動備份。 您不需要啟用此功能。 目前，個別使用者無法刪除服務用來維護 Sla 的自動還原點以進行復原。

Azure Synapse 會在一天內取得資料倉儲的快照集。 它所建立的還原點可供使用七天。 無法變更保留期限。 Azure Synapse 支援八小時的復原點目標。 您可以從過去七天內所建立的任何快照集，在主要區域中還原資料倉儲。 如果您的組織需要更細微的備份，則可以使用其他使用者定義的選項。

### <a name="unsupported-teradata-table-types"></a>不支援的 Teradata 資料表類型

Teradata 包含對於時間序列和時態性資料的特殊資料表類型的支援。 Azure Synapse 不會直接支援這些資料表類型的語法和某些函式，但是您可以將資料移轉到具有所需資料類型的標準資料表，以及在日期/時間資料行上進行索引編制或分割。

Teradata 藉由使用查詢重寫將篩選準則加入至時態查詢來限制適用的日期範圍，來執行時態性查詢功能。 如果您在來源 Teradata 環境中使用時態性查詢，而您想要將其遷移，則必須將篩選準則加入至相關的時態查詢。

Azure 環境也包含可透過 Azure 時間序列深入解析大規模進行時間序列資料的複雜分析功能。 時間序列深入解析是針對 IoT 資料分析應用程式所設計，而且可能更適合該使用案例。 如需詳細資訊，請參閱 [Azure 時間序列深入](https://azure.microsoft.com/services/time-series-insights/)解析。

### <a name="teradata-data-type-mapping"></a>Teradata 資料類型對應

Azure Synapse 不直接支援某些 Teradata 資料類型。 下表顯示這些資料類型，以及處理這些資料類型的建議方法。 在資料表中，Teradata 資料行類型是儲存在系統目錄中的類型 (例如，在 `DBC.ColumnsV`) 中。

使用來自 Teradata 目錄資料表的中繼資料來判斷是否應該遷移任何這些資料類型，然後規劃您的遷移計畫中的支援資源。 例如，您可以使用類似下一節中的 SQL 查詢，來尋找任何您需要解決的不支援資料類型。

協力廠商提供可自動化遷移的工具和服務，包括平臺之間的對應資料類型。 如果您已在 Teradata 環境中使用協力廠商 ETL 工具（例如 Informatica 或 Talend），您可以使用此工具來執行任何必要的資料轉換。

## <a name="sql-data-manipulation-language-dml-syntax-differences"></a>SQL 資料操作語言 (DML) 語法差異

您應留意 SQL 資料操作語言的一些差異， (DML) Teradata SQL 與 Azure Synapse 之間的語法：

- `QUALIFY`： Teradata 支援 `QUALIFY` 運算子。

   例如：

  `SELECT col1 FROM tab1 WHERE col1='XYZ'`

  協力廠商工具和服務可將資料對應工作自動化：

  `QUALIFY ROW_NUMBER() OVER (PARTITION by col1 ORDER BY col1) = 1;`

  在 Azure Synapse 中，您可以使用下列語法來達到相同的結果：

  `SELECT * FROM (SELECT col1, ROW_NUMBER() OVER (PARTITION by col1 ORDER BY col1) rn FROM tab1 WHERE c1='XYZ' ) WHERE rn = 1;`

- **日期算術：** Azure Synapse 具有和之類 `DATEADD` `DATEDIFF` 的運算子，您可以在或上使用它 `DATE` `DATETIME` 。

   Teradata 支援日期的直接減法：

  - `SELECT DATE1 - DATE2 FROM ...`

  - `LIKE ANY` 語法

    範例：

    `SELECT * FROM CUSTOMER WHERE POSTCODE LIKE ANY ('CV1%', 'CV2%', CV3%') ;`.

    Azure Synapse 語法中的對等專案為：

    `SELECT * FROM CUSTOMER WHERE (POSTCODE LIKE 'CV1%') OR (POSTCODE LIKE 'CV2%') OR (POSTCODE LIKE 'CV3%') ;`

- 視系統設定而定，Teradata 中的字元比較可能預設不會有大小寫。 在 Azure Synapse 中，這些比較一律是案例特定的。

## <a name="functions-stored-procedures-triggers-and-sequences"></a>函數、預存程式、觸發程式和序列

當您從像是 Teradata 的成熟舊版環境遷移資料倉儲時，通常需要將簡單資料表和觀點以外的元素遷移至新的目標環境。 Teradata 中您可能需要遷移至 Azure Synapse 的非資料表元素範例有函數、預存程式、觸發程式和順序。 在遷移的準備階段，您應該建立要遷移之物件的清查。 在專案計劃中，定義處理所有物件的方法，並配置適當的資源來進行遷移。

您可以在 Azure 環境中找到服務，以取代在 Teradata 環境中實作為函式或預存程式的功能。 通常，使用內建 Azure 功能，而不是在 Teradata 函式中進行編碼，是更有效率的作法。

以下是有關遷移函數、預存程式、觸發程式和順序的詳細資訊：

- **函數：** 如同大多數的資料庫產品，Teradata 在 SQL 執行中支援系統函數和使用者定義函數。 當一般系統函式遷移至另一個資料庫平臺（例如 Azure Synapse）時，通常會在新的環境中使用，而且可以在不變更的情況下遷移。 如果系統函數在新的環境中有稍微不同的語法，您通常可以將必要的變更自動化。

  您可能需要在新的環境中，對沒有對等專案的任意使用者自訂函數和系統函數進行編碼。 使用新環境中可用的語言。 Azure Synapse 會使用熱門的 Transact-sql 語言來執行使用者定義函數。

- **預存程式：** 在大部分的新式資料庫產品中，您可以將程式儲存在資料庫中。 預存程式通常包含 SQL 語句和一些程式邏輯。 它可能也會傳回資料或狀態。

  Teradata 提供預存程式語言以建立預存程式。 Azure Synapse 支援使用 T-sql 的預存程式。 如果您將預存程式遷移至 Azure Synapse，您必須使用 T-sql 來將它們重設成。

- **觸發程式：** 您無法在 Azure Synapse 中建立觸發程式，但可以在 Data Factory 中執行觸發程式。

- **順序：** Azure Synapse 順序的處理方式類似于 Teradata 中的處理方式。 使用 `IDENTITY` 資料行或 SQL 程式碼來建立數列中的下一個序號。

## <a name="metadata-and-data-extraction"></a>中繼資料和資料解壓縮

當您規劃如何從 Teradata 環境中解壓縮中繼資料和資料時，請考慮下列資訊：

- **資料定義語言 (DDL) 產生：** 如先前所述，如有必要，您可以編輯現有 `CREATE TABLE` 的 Teradata 和 `CREATE VIEW` 腳本，以建立具有已修改資料類型的對等定義。 在此案例中，您通常必須移除額外的 Teradata 特定子句 (例如 `FALLBACK`) 。

  指定目前資料表和 view 定義的資訊會保留在系統目錄資料表中。 系統目錄資料表是資訊的最佳來源，因為資料表可能是最新的，而且已完成。 使用者維護的檔可能不會與目前的資料表定義同步。

  您可以使用目錄上的 views 來存取訊號，例如 `DBC.ColumnsV` 。 您也可以使用 views 來 `CREATE TABLE` 為 Azure Synapse 中的對等資料表產生對等的資料定義語言 (DDL) 語句。

  協力廠商的遷移和 ETL 工具也會使用類別目錄資訊來達成相同的結果。

- **資料擷取**

  使用標準 Teradata 公用程式（如和），從現有的 Teradata 資料表遷移原始資料 `BTEQ` `FASTEXPORT` 。 在遷移練習中，盡可能有效率地將資料解壓縮，通常很重要。 針對最新版本的 Teradata，我們建議 Teradata Parallel Transporter，此公用程式會使用多個平行 `FASTEXPORT` 資料流程來達到最佳輸送量。

  您可以直接從 Data Factory 呼叫 Teradata Parallel Transporter。 我們建議使用此方法來管理資料移轉程式、Teradata 實例是否在內部部署環境中，或複製到 Azure 環境中的 VM （如先前所述）。

  我們建議用於解壓縮資料的資料格式是分隔的文字檔 (也稱為 *逗點分隔值*) 、優化的資料列單欄式檔案或 Parquet 檔案。

如需從 Teradata 環境遷移資料和 ETL 之程式的詳細資訊，請參閱 Teradata 檔。

## <a name="performance-tuning-recommendations"></a>效能微調建議

在優化的時候，平臺有一些差異。 在下列效能微調建議清單中，Teradata 和 Azure Synapse 之間的較低層級執行差異，以及您遷移的替代專案會反白顯示：

- **資料散發選項：** 在 Azure 中，您可以設定個別資料表的資料散發方法。 這項功能的目的是要減少執行查詢時，在處理節點之間移動的資料量。

  針對大型資料表/大型資料表聯結，雜湊在其中一個或兩個 (都是在理想的情況下，聯結資料行上的) 資料表有助於確保聯結處理可以在本機執行，因為要聯結的資料列已經共置於相同的處理節點上。

  Azure Synapse 提供另一種方法來達成小型資料表/大型資料表聯結的本機聯結 (通常稱為星狀架構模型) 中的 *維度資料表/事實資料表聯結* 。 您會將較小的資料表複寫到所有節點，藉此確保較大資料表的聯結索引鍵值具有可在本機使用的相符維度資料列。 如果資料表很大，則複寫維度資料表的額外負荷相對較低。 在此情況下，最好使用稍早所述的雜湊散發方法。

- **資料索引編制：** Azure Synapse 提供各種索引編制選項，但在 Teradata 中，索引選項的作業和使用方式各不相同。 若要瞭解 Azure Synapse 中的索引編制選項，請參閱在 [Azure Synapse 集區中設計資料表](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-overview)。

  來源 Teradata 環境中的現有索引可以提供資料使用方式的實用指示，並提供在 Azure Synapse 環境中編制索引所需的候選資料行的指示。

- **資料分割：** 在企業資料倉儲中，事實資料表可能包含許多數十億個數據列的資料。 資料分割是將這些資料表中的維護和查詢優化的一種方式。 將資料表分割成不同的部分，可減少一次處理的資料量。 資料表的資料分割是在語句中定義 `CREATE TABLE` 。

  每個資料表只能有一個欄位可以用來進行資料分割。 經常用於資料分割的欄位是日期欄位，因為許多查詢都會依日期或日期範圍進行篩選。 您可以在初始載入之後變更資料表的資料分割。 若要變更資料表的資料分割，請使用語句的新散發重新建立資料表 `CREATE TABLE AS SELECT` 。 如需 Azure Synapse 中資料分割的詳細說明，請參閱 [Azure SYNAPSE SQL 集區中](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-partition)的分割資料表。

- **資料表統計資料：** 您可以藉由在 `COLLECT STATISTICS` ETL/ELT 工作中加入步驟，或在資料表上啟用自動統計資料收集，來確保有關資料表的統計資料是最新的。

- **用於載入資料的 PolyBase：** PolyBase 是最有效率的方法，可用來將大量資料載入至倉儲。 您可以使用 PolyBase 來載入平行資料流程中的資料。

- **工作負載管理的資源類別：** Azure Synapse 會使用資源類別來管理工作負載。 一般情況下，大型資源類別可提供更佳的個別查詢效能。 較小的資源類別可提供更高層級的平行存取。 您可以使用動態管理檢視來監視使用狀況，以協助確保有效率地使用適當的資源。

## <a name="next-steps"></a>下一步

如需有關實施 Teradata 遷移的詳細資訊，請與您的 Microsoft 帳戶代表討論內部部署的內部部署優惠。
