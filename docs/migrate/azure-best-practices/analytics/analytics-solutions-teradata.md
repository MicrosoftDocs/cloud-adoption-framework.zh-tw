---
title: 適用于 Teradata 的 Azure Synapse 分析遷移
description: 使用適用于 Azure 的雲端採用架構來瞭解分析解決方案，以 Teradata 及遷移至 Azure Synapse Analytics。
author: v-hanki
ms.author: brblanch
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: bc78b38f93f86dae0f8e0b650712cda7406574c7
ms.sourcegitcommit: 76edf563a08ff7dc81c3fc2dc6c8972ab3b4c55b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2020
ms.locfileid: "88237095"
---
<!-- cSpell:ignore DATEADD DATEDIFF Attunity Teradata Inmon NUSI Informatica Talend BTEQ FASTEXPORT QUALIFY ORC Parquet "Parallel Data Transporter" "Attunity Replicate" -->

# <a name="azure-synapse-analytics-solutions-and-migration-for-teradata"></a>適用于 Teradata 的 Azure Synapse 分析解決方案和遷移

許多組織都已準備好將昂貴的資料倉儲工作（例如基礎結構維護和平臺開發）轉移到雲端提供者的步驟。 組織現在想要利用創新的雲端、基礎結構即服務，以及 Azure 等較新環境中的平臺即服務供應專案。  

Azure Synapse 分析是一種無限制的分析服務，可將企業資料倉儲和海量資料分析整合在一起。 它可讓您自由地使用無伺服器隨選或布建的資源，以大規模查詢您的詞彙資料。 瞭解當您將舊版 Teradata 系統移轉至 Azure Synapse 時要規劃的事項。

雖然 Teradata 和 Azure Synapse 很類似，但兩者都是 SQL 資料庫，其設計目的是要使用大量平行處理技術來達到大型資料磁片區上的高查詢效能，它們有一些基本差異：

- 舊版 Teradata 系統會安裝在內部部署環境中，並使用專屬硬體。 Azure Synapse 是以雲端為基礎，並使用 Azure 儲存體和計算資源。
- 升級 Teradata 設定是一項主要工作，涉及額外的實體硬體以及可能冗長的資料庫重新設定或傾印和重載。 在 Azure Synapse 中，儲存體和計算資源是分開的，因此您可以使用 Azure 的彈性擴充性，輕鬆地單獨相應增加或相應減少。
- 如果沒有實體系統可支援，您可以視需要暫停或調整 Azure Synapse 的大小，以降低資源使用率和成本。 在 Azure 中，您可以存取全球可用、高度安全且可調整的雲端環境，其中包含支援工具和功能生態系統中的 Azure Synapse。

在本文中，我們將探討架構遷移，其目標是要在 Azure Synapse 上取得您遷移的 Teradata 資料倉儲和資料超市的對等或提升效能。 我們會考慮特別適用于從現有的 Teradata 環境進行遷移的考慮。

大致來說，遷移套裝程式含下表所列的步驟：

<!-- markdownlint-disable MD033 -->

| 準備        | 遷移                             | 移轉後 |
| :----------------- | :----------------------------- | :---------------- |
| <ul><li> 定義範圍：我們要遷移什麼？</li><li>建立要遷移之資料和進程的清查。</li><li>定義任何資料模型變更。</li><li>識別要使用的最佳 Azure 和協力廠商工具和功能。</li><li>在新平臺上及早訓練員工。</li><li>設定 Azure 目標平臺。</li></ul> |  <ul><li>從小型和簡單著手。</li><li>盡可能自動化。</li><li>使用 Azure 內建工具和功能來減少遷移工作。</li><li>遷移資料表和視圖的中繼資料。</li><li>遷移相關的歷程記錄資料。</li><li>遷移或重構預存程式和商業流程。</li><li>遷移或重構 ETL/ELT 累加式載入進程。</li></ul> | <ul><li> 監視並記載遷移程式的所有階段。</li><li>使用已取得的經驗來建立範本，以供未來遷移。</li><li>如有需要，請使用新平臺的效能和擴充性來 Reengineer 資料模型。</li><li>測試應用程式和查詢工具。</li><li>基準測試和優化查詢效能。</li></ul> |

當您從舊版 Teradata 環境遷移至 Azure Synapse 時，除了 Teradata 檔中所述的一般主題以外，您還必須考慮一些特定的因素。 

## <a name="initial-migration-workload"></a>初始遷移工作負載

舊版 Teradata 環境通常會隨著時間而演變，以包含多個主題區域和混合工作負載。 當您決定要從何處開始進行初始的遷移專案時，選擇下欄區域是合理的：

- 藉由快速提供新環境的優點，證明遷移至 Azure Synapse 的可行性。
- 可讓內部技術人員取得新程式和工具的經驗，讓他們可以使用它們來遷移其他區域。
- 根據目前用來從來源 Teradata 環境進行其他遷移的工具和進程，建立範本。

從支援這些目標的 Teradata 環境初始遷移的理想候選，通常是執行 Power BI/分析工作負載，而不是 OLTP 工作負載。 工作負載應該具有可使用最少修改來遷移的資料模型，例如星形或雪花式架構。

針對大小，請務必在初始練習中遷移的資料量夠大，以示範 Azure Synapse 環境的功能和優點，並提供簡短的時機來示範價值。 通常符合需求的大小為 1 tb (TB) 到 10 Tb 的範圍。

最小化風險和執行時間的初始遷移專案的方法，是將遷移範圍限制為數據超市。 在 Teradata 中，有一個很好的例子，就是 Teradata 資料倉儲的 OLAP 資料庫部分。 這種方法是很好的起點，因為它會限制遷移的範圍，而且通常可以在短的時間點上達到。 

資料超市的初始遷移範圍僅無法解決更廣泛的問題，例如如何遷移 ETL 和歷程記錄資料。 您必須在後續階段中解決這些區域，並使用建立它們所需的資料和程式來回填遷移的資料超市層。

## <a name="lift-and-shift-approach-vs-phased-approach"></a>隨即轉移方法與分階段方法

無論您為遷移選擇的驅動程式和範圍為何，您都可以從兩種一般的遷移類型中選擇：

- 隨即**轉移方法**：在這種方法中，現有的資料模型（例如星狀架構）會原封不動地遷移至新的 Azure Synapse 平臺。 重點是要將風險降至最低，並減少完成移至 Azure 雲端環境所需的工作，藉此降低遷移所花費的時間。

  這種方法適用于現有的 Teradata 環境，其中會遷移單一資料超市，以及資料是否已在設計良好的星型或雪花式架構中。 如果您有時間和成本壓力，可以移至更現代化的雲端環境，這種方法也是不錯的選擇。

- **合併修改的階段式方法**：如果您的舊版倉儲隨著時間進化，您可能需要 reengineer 資料倉儲以維持所需的效能，或支援 IoT 串流之類的新資料來源。 遷移至 Azure Synapse 以取得可調整雲端環境的知名優點，可能會被視為重建程式的一部分。 此流程可能包括變更基礎資料模型，例如從 Inmon 模型移至 Azure 資料保存庫。

  我們建議的方法是，一開始將現有的資料模型移至 Azure。 然後，利用 Azure 服務的效能和彈性來套用重建變更，而不會影響現有的來源系統。

## <a name="virtual-machine-colocation-to-support-migration"></a>支援遷移的虛擬機器共置

從內部部署 Teradata 環境進行遷移的選擇性方法，會利用 Azure 中成本低廉的雲端儲存體和彈性的擴充性。 首先，您會在與目標 Azure Synapse 環境共存的 Azure 虛擬機器上建立 Teradata 實例。 然後，您可以使用標準的 Teradata 公用程式，例如 Teradata Parallel Transporter 或協力廠商資料複寫工具（例如 (Qlik sense）) 先前由 Attunity 進行複寫，以有效率地將您要遷移的 Teradata 資料表子集移至 VM 實例。 所有的遷移工作都是在 Azure 環境中進行。 

這種方法有幾項優點：

- 資料的初始複寫之後，來源系統不會受到其他遷移工作的影響。
- 熟悉的 Teradata 介面、工具和公用程式可在 Azure 環境中取得。
- 在遷移轉移到 Azure 環境之後，您不會有內部部署來源系統與雲端目標系統之間網路頻寬可用性的任何潛在問題。
- Azure Data Factory 之類的工具可以有效率地呼叫像是 Teradata 平行 Transporter 的公用程式，以快速輕鬆地遷移資料。
- 從 Azure 環境中完全協調和控制遷移程式。

## <a name="metadata-migration"></a>中繼資料移轉

使用 Azure 環境的功能來自動化和協調遷移程式是合理的。 這種方法會將對現有 Teradata 環境的影響降至最低，這可能已經接近完整的容量。

Azure Data Factory 是以雲端為基礎的資料整合服務。 您可以使用 Data Factory 在雲端建立資料驅動的工作流程，以協調及自動化資料移動和資料轉換。 Data Factory 管線可以從不同的資料存放區內嵌資料。 然後，他們會使用適用于 Apache Hadoop 的 Azure HDInsight 和 Apache Spark、Azure Data Lake Analytics 和 Azure Machine Learning 等計算服務來處理和轉換資料。 

首先建立中繼資料，其中列出您要遷移的資料表及其位置。 然後，使用 Data Factory 功能來管理遷移程式。

## <a name="design-differences-between-teradata-and-azure-synapse"></a>Teradata 與 Azure Synapse 之間的設計差異

當您規劃從舊版 Teradata 環境遷移至 Azure Synapse 時，請務必考慮兩個平臺之間的設計差異。

### <a name="multiple-databases-vs-a-single-database-and-schemas"></a>多個資料庫與單一資料庫和架構的比較

在 Teradata 環境中，您可能會有多個個別的資料庫，供整體環境的不同部分使用。 例如，您可能會有個別的資料庫用於資料內嵌和臨時資料表、用於核心倉儲資料表的資料庫，以及另一個用於資料超市的資料庫 (有時稱為「 *語義層* 」) 。 在 Azure Synapse 中處理不同的資料庫做為 ETL/ELT 管線，可能需要執行跨資料庫聯結，並在不同的資料庫之間移動資料。

Azure Synapse 環境有單一資料庫。 架構會用來將資料表分隔成邏輯上分隔的群組。 我們建議您在 Azure Synapse 中使用一系列的架構，以模擬您從 Teradata 遷移的任何不同資料庫。 

如果您使用 Teradata 環境中的架構，您可能需要使用新的命名慣例，將現有的 Teradata 資料表和 views 移到新的環境。 例如，您可以將現有的 Teradata 架構和資料表名稱串連到新的 Azure Synapse 資料表名稱中，然後在新的環境中使用架構名稱來維護原始的個別資料庫名稱。 

另一個選項是使用基礎資料表上的 SQL views 來維護其邏輯結構。 使用 SQL views 有一些可能的缺點：

- Azure Synapse 中的 Views 是唯讀的，因此您必須對基礎基表上的資料進行任何更新。
- 如果已有視圖層級，則新增另一層視圖可能會影響效能。

### <a name="tables"></a>資料表

當您在不同的技術之間遷移資料表時，您實際上只會在兩個環境之間移動原始資料和描述它的中繼資料。 您不會將資料庫元素（例如來源系統中的索引）遷移到這些專案，因為這些專案可能不需要，也可能在新的環境中以不同方式執行。

不過，若要瞭解在來源環境中使用索引的效能優化，可能會有很大的指示，讓您在新環境中優化效能。 例如，如果在來源 Teradata 環境中建立了一個非唯一的次要索引，您可能會發現在遷移的 Azure Synapse 環境中建立非叢集索引，或使用其他原生效能優化技術（如資料表複寫），可能比較適合建立類似的索引。

### <a name="high-availability-database"></a>高可用性資料庫

Teradata 透過選項支援跨節點的資料複寫 `FALLBACK` 。 實際上位於節點上的資料表資料列會複寫到系統內的另一個節點。 這種方法可保證當節點失敗時，資料不會遺失，而且它會提供容錯移轉案例的基礎。

Azure SQL Database 中高可用性架構的目標，是要確保您的資料庫已啟動並執行99.99% 的時間。 您不需要考慮維護作業和中斷可能對您的工作負載有何影響。 Azure 會自動處理重要的服務工作，例如修補、備份、Windows 和 SQL 升級，以及基礎硬體、軟體或網路失敗之類的非計畫事件。

快照集是服務的內建功能，可在 Azure Synapse 中建立還原點。 快照集可為 Azure Synapse 中的資料儲存提供自動備份。 您不需要啟用此功能。 目前，個別使用者無法刪除自動還原點，此服務會使用此端點來維護修復的 Sla。

Azure Synapse 會在一天內取得資料倉儲的快照集。 它所建立的還原點可供使用七天。 無法變更保留期限。 Azure Synapse 支援八小時的復原點目標。 您可以從過去七天內所建立的任何快照集，在主要區域中還原資料倉儲。 如果您的組織需要更細微的備份，則可以使用其他使用者定義的選項。

### <a name="unsupported-teradata-table-types"></a>不支援的 Teradata 資料表類型

Teradata 包括針對時間序列和時態性資料的特殊資料表類型支援。 Azure Synapse 不直接支援這些資料表類型的語法和部分函式，但您可以將資料移轉至具有所需資料類型的標準資料表，以及在日期/時間資料行上建立索引或分割的功能。

Teradata 會使用查詢重寫來執行時態性查詢功能，以將篩選準則加入至時態查詢，以限制適用的日期範圍。 如果您在來源 Teradata 環境中使用時態查詢，而且想要加以遷移，則必須將篩選準則加入至相關的時態查詢。

Azure 環境也包含透過 Azure 時間序列深入解析大規模針對時間序列資料進行複雜分析的功能。 時間序列深入解析是針對 IoT 資料分析應用程式所設計，而且可能更適合該使用案例。 如需詳細資訊，請參閱 [Azure 時間序列深入解析](https://azure.microsoft.com/services/time-series-insights/)。

### <a name="teradata-data-type-mapping"></a>Teradata 資料類型對應

某些 Teradata 資料類型不會直接在 Azure Synapse 中受到支援。 下表顯示這些資料類型，以及處理它們的建議方法。 在資料表中，Teradata 資料行類型是儲存在系統目錄中的類型 (例如， `DBC.ColumnsV`) 。

使用來自 Teradata 目錄資料表的中繼資料來判斷是否應該遷移其中任何一種資料類型，然後在您的遷移計畫中規劃支援的資源。 例如，您可以使用下一節中的 SQL 查詢來尋找您需要解決的任何不支援的資料類型。

協力廠商提供可自動化遷移的工具和服務，包括在平臺之間對應資料類型。 如果您已經在 Teradata 環境中使用協力廠商 ETL 工具，例如 Informatica 或 Talend，您可以使用工具來執行任何必要的資料轉換。

## <a name="sql-data-manipulation-language-dml-syntax-differences"></a>SQL 資料操作語言 (DML) 語法差異

您應該注意 SQL 資料操作語言中的幾項差異， (Teradata SQL 與 Azure Synapse 之間的 DML) 語法：

- `QUALIFY`： Teradata 支援 `QUALIFY` 運算子。 

   例如：

  `SELECT col1 FROM tab1 WHERE col1='XYZ'`

  協力廠商工具和服務可以自動化資料對應工作：

  `QUALIFY ROW_NUMBER() OVER (PARTITION by col1 ORDER BY col1) = 1;`

  在 Azure Synapse 中，您可以使用下列語法來達到相同的結果：

  `SELECT * FROM (SELECT col1, ROW_NUMBER() OVER (PARTITION by col1 ORDER BY col1) rn FROM tab1 WHERE c1='XYZ' ) WHERE rn = 1;`

- **日期算術**： Azure Synapse 具有和之類 `DATEADD` `DATEDIFF` 的運算子，您可以在或上使用它 `DATE` `DATETIME` 。 

   Teradata 支援日期的直接減法：
   
   - `SELECT DATE1 - DATE2 FROM ...`

  - `LIKE ANY` 語法

    範例：

    `SELECT * FROM CUSTOMER WHERE POSTCODE LIKE ANY ('CV1%', 'CV2%', CV3%') ;`.

    Azure Synapse 語法中的對等項是：

    `SELECT * FROM CUSTOMER WHERE (POSTCODE LIKE 'CV1%') OR (POSTCODE LIKE 'CV2%') OR (POSTCODE LIKE 'CV3%') ;`

- 視系統設定而定，Teradata 中的字元比較預設可能不會區分大小寫。 在 Azure Synapse 中，這些比較一律是案例特定的。

## <a name="functions-stored-procedures-triggers-and-sequences"></a>函數、預存程式、觸發程式和順序

當您從成熟的舊版環境（例如 Teradata）遷移資料倉儲時，通常需要將簡單資料表和 views 以外的元素遷移到新的目標環境。 Teradata 中您可能需要遷移至 Azure Synapse 的非資料表元素範例包括函式、預存程式、觸發程式和順序。 在遷移的準備階段期間，您應該建立要遷移之物件的清查。 在專案計劃中，定義處理所有物件的方法，並配置適當的資源來進行遷移。

您可能會在 Azure 環境中找到服務，以取代在 Teradata 環境中實作為函式或預存程式的功能。 通常，使用內建的 Azure 功能會更有效率，而不是 Teradata 函數的編碼。

以下是有關遷移函數、預存程式、觸發程式和順序的詳細資訊：

- **函數**：如同大部分的資料庫產品，TERADATA 支援 SQL 執行中的系統函數和使用者定義函數。 當一般系統函數遷移至另一個資料庫平臺（例如 Azure Synapse）時，通常會在新的環境中使用，而且可以在不變更的情況下進行遷移。 如果系統函數在新的環境中有稍微不同的語法，您通常可以自動執行必要的變更。

  您可能需要在新的環境中，對任何沒有對等的使用者自訂函數和系統函數進行編碼。 使用新環境中可用的語言。 Azure Synapse 會使用熱門的 Transact-sql 語言來執行使用者定義函數。

- **預存程式**：在大部分的現代化資料庫產品中，您可以將程式儲存在資料庫中。 預存程式通常包含 SQL 語句和一些程式邏輯。 它也可能會傳回資料或狀態。

  Teradata 提供預存程式語言來建立預存程式。 Azure Synapse 支援使用 T-sql 的預存程式。 如果您將預存程式遷移至 Azure Synapse，則必須使用 T-sql 來將其編碼。

- **觸發**程式：您無法在 Azure Synapse 中建立觸發程式，但您可以在 Data Factory 中執行觸發程式。

- **順序**： Azure Synapse 序列的處理方式類似于在 Teradata 中處理它們。 使用 `IDENTITY` 資料行或 SQL 程式碼來建立數列中的下一個序號。

## <a name="metadata-and-data-extraction"></a>中繼資料和資料解壓縮

當您規劃如何從 Teradata 環境中解壓縮中繼資料和資料時，請考慮下列資訊：

- **資料定義語言 (DDL) 產生**：如先前所述，您可以編輯現有的 Teradata `CREATE TABLE` 和 `CREATE VIEW` 腳本，以視需要使用修改過的資料類型來建立對等定義。 在此案例中，您通常必須移除額外的 Teradata 特有子句 (例如 `FALLBACK`) 。

  指定目前資料表和 view 定義的資訊會保留在系統目錄資料表中。 系統目錄資料表是資訊的最佳來源，因為資料表可能是最新的，而且已完成。 使用者維護的檔可能不會與目前的資料表定義同步。

  您可以使用目錄上的 views 來存取訊號，例如 `DBC.ColumnsV` 。 您也可以使用 views 來為 `CREATE TABLE` Azure Synapse 中的對等資料表，產生對等的資料定義語言 (DDL) 語句。  

  協力廠商遷移和 ETL 工具也會使用目錄資訊來達到相同的結果。

- **資料擷取**

  使用標準 Teradata 公用程式（例如和），從現有的 Teradata 資料表遷移原始資料 `BTEQ` `FASTEXPORT` 。 在遷移練習中，將資料盡可能有效率地解壓縮通常是很重要的。 我們針對最新版本的 Teradata 所建議的方法是使用 Teradata Parallel Transporter，這是使用多個平行 `FASTEXPORT` 串流來達到最佳輸送量的公用程式。

  您可以直接從 Data Factory 呼叫 Teradata Parallel Transporter。 如果 Teradata 實例是內部部署或複製到 Azure 環境中的 VM，建議使用這種方法來管理資料移轉程式，如先前所述。  

  我們建議用來解壓縮資料的資料格式是分隔的文字檔， (也稱為 *逗號分隔值*) 、優化的資料列單欄式檔案或 Parquet 檔。

如需從 Teradata 環境遷移資料和 ETL 程式的詳細資訊，請參閱 Teradata 檔。

## <a name="performance-tuning-recommendations"></a>效能微調建議

在優化方面，這些平臺有一些差異。 在下列的效能微調建議清單中，Teradata 與 Azure Synapse 之間的較低層級執行差異，以及您的遷移替代專案會反白顯示：

- **資料散發選項**：在 Azure 中，您可以設定個別資料表的資料散發方法。 此功能的目的是要減少在執行查詢時，于處理節點之間移動的資料量。  

  對於大型資料表/大型資料表聯結，雜湊散發在其中一個或兩個 (理想的情況下，聯結資料行上) 資料表都有助於確保聯結處理可以在本機執行，因為要聯結的資料列已經共置在相同的處理節點上。

  Azure Synapse 提供另一種方式來達成小型資料表/大型資料表聯結的本機聯結 (通常稱為星狀架構模型) 中的 *維度資料表/事實資料表聯結* 。 您會在所有節點之間複寫較小的資料表，藉此確保較大資料表的聯結索引鍵的任何值都有一個可在本機使用的相符維度資料列。 如果資料表不大，複寫維度資料表的額外負荷相對較低。 在此情況下，最好使用稍早所述的雜湊散發方法。

- **資料索引**： Azure Synapse 提供各種索引編制選項，但在 Teradata 的索引選項中，其選項不同于作業和使用量。 若要深入瞭解 Azure Synapse 中的索引選項，請參閱 [在 Azure Synapse 集區中設計資料表](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/ql-data-warehouse-tables-overview)。

  來源 Teradata 環境中的現有索引可以提供有用的指示，說明如何使用資料，並提供在 Azure Synapse 環境中編制索引的候選資料行指示。

- **資料分割**：在企業資料倉儲中，事實資料表可能包含許多數十億的資料列。 資料分割是在這些資料表中優化維護和查詢的一種方式。 將資料表分割成不同的部分，可減少一次處理的資料量。 資料表的資料分割是在語句中定義 `CREATE TABLE` 。

  每個資料表只能有一個欄位可用於資料分割。 經常用於資料分割的欄位是日期欄位，因為許多查詢會依日期或日期範圍進行篩選。 您可以在初始載入之後變更資料表的資料分割。 若要變更資料表的資料分割，請以使用語句的新散發來重新建立資料表 `CREATE TABLE AS SELECT` 。 如需 Azure Synapse 中資料分割的詳細說明，請參閱 [Azure SYNAPSE SQL 集區中的分割區資料表](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-partition)。

- **資料表統計資料**：您可以藉由 `COLLECT STATISTICS` 在 ETL/ELT 作業中加入步驟，或在資料表上啟用自動統計資料收集，確保資料表的統計資料是最新的。

- **用於資料載入的 polybase**： polybase 是最有效率的方法，可用來將大量資料載入倉儲中。 您可以使用 PolyBase 來載入平行資料流程中的資料。

- **適用于工作負載管理的資源類別**： Azure Synapse 會使用資源類別來管理工作負載。 一般來說，大型資源類別會提供更好的個別查詢效能。 較小的資源類別可提供較高層級的平行存取。 您可以使用動態管理檢視來監視使用率，以協助確保有效率地使用適當的資源。

## <a name="next-steps"></a>後續步驟

如需有關如何執行 Teradata 遷移的詳細資訊，請與您的 Microsoft 帳戶代表內部部署的遷移供應專案交談。
