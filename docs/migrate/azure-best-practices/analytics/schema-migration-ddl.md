---
title: 架構遷移資料定義語言
description: 瞭解當您將架構遷移至 Azure Synapse Analytics 時， (Ddl) 的資料定義語言的設計考慮和效能選項。
author: v-hanki
ms.author: brblanch
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: think-tank
ms.openlocfilehash: f73d5e5a17f7180da186b4c428ae2bcc4e9fb923
ms.sourcegitcommit: c167c45b66cc7324b60c88b8b7aac439f956b65d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2021
ms.locfileid: "102208144"
---
# <a name="data-definition-languages-for-schema-migration"></a>架構遷移的資料定義語言

本文說明當您將架構遷移至 Azure Synapse Analytics 時， (Ddl) 的資料定義語言的設計考慮和效能選項。

## <a name="design-considerations"></a>設計考量

當您規劃架構遷移時，請參閱這些設計考慮。

### <a name="preparation-for-migration"></a>準備遷移

當您準備將現有資料移轉至 Azure Synapse Analytics 時，請務必明確地定義練習的範圍 (特別是) 的初始遷移專案。 事先花在瞭解資料庫物件和相關進程如何遷移的時間，可能會在專案之後減少投入時間和風險。

建立要遷移之資料庫物件的清查。 根據來源平臺，此清查將包含下列部分或所有物件：

- 資料表
- 檢視
- 索引
- 函式
- 預存程序
- 資料散發和資料分割

這些物件的基本資訊應該包含資料列計數、實體大小、資料壓縮比例和物件相依性等度量。 這項資訊應該可透過對來源系統中系統目錄資料表的查詢取得。 系統中繼資料是這項資訊的最佳來源。 外部檔可能已過時，且不會與自初始執行後已套用至資料結構的變更同步。

您也可以從查詢記錄中分析實際的物件使用方式，或使用 Microsoft 合作夥伴的工具，例如 Attunity 可見度來提供協助。 某些資料表可能不需要遷移，因為它們不再用於生產查詢中。

資料大小和工作負載資訊對 Azure Synapse Analytics 很重要，因為它有助於定義適當的設定。 其中一個範例是必要的平行存取層級。 瞭解資料和工作負載的預期成長可能會影響建議的目標設定，而且也是最好的作法，也就是使用這項資訊。

當您使用資料磁片區來估計新目標平臺所需的儲存體時，請務必瞭解源資料庫上的資料壓縮率（如果有的話）。 只要採用來源系統上使用的儲存體數量，就可能會成為調整大小的錯誤基礎。 監視和中繼資料資訊可協助您判斷目前系統中的編制索引、資料複寫、記錄或其他進程的未壓縮原始資料大小和負荷。

要遷移之資料表的未壓縮原始資料大小，是估計新目標 Azure Synapse Analytics 環境所需儲存體的絕佳起點。

新的目標平臺也會包含壓縮因數和索引額外負荷，但這些可能會與來源系統不同。 Azure Synapse Analytics 儲存體定價也包含七天的快照集備份。 相較于現有的環境，這可能會對所需的儲存體整體成本產生影響。

您可以延遲資料模型的效能調整，直到延遲的遷移程式，以及資料倉儲中的實際資料量。 不過，我們建議您稍早執行一些效能微調選項。

例如，在 Azure Synapse Analytics 中，您可以將小型維度資料表定義為複寫資料表，以及將大型事實資料表定義為叢集資料行存放區索引，是合理的。 同樣地，在來源環境中定義的索引可讓您清楚地指出哪些資料行可能會在新環境中建立索引的優點。 當您一開始在載入資料表之前定義資料表時，使用這項資訊會在程式之後節省時間。

當遷移專案進行時，在 Azure Synapse Analytics 中測量您自己資料的壓縮比率和索引額外負荷是很好的作法。 此量值可讓您進行未來的容量規劃。

在遷移之前，您可以藉由降低複雜性來簡化您現有的資料倉儲，以簡化遷移。 這種工作可能包括：

- 在遷移前移除或封存未使用的資料表，以避免遷移未使用的資料。 封存到 Azure Blob 儲存體，並將資料定義為外部資料表，可能會讓資料保持較低的成本。
- 使用資料虛擬化軟體將實體資料超市轉換成虛擬資料超市，以減少您必須遷移的資料。 這項轉換也可提升靈活性，並降低擁有權總成本。 在遷移期間，您可能會將其視為現代化。

遷移練習的一個目標，也可能是藉由變更基礎資料模型來將倉儲現代化。 其中一個範例是從 Inmon 樣式的資料模型移至資料保存庫的方法。 您應該在準備階段中決定這一點，並納入轉換至遷移計畫的策略。

在此案例中，建議的方法是先將資料模型依原樣遷移至新的平臺，然後轉換至 Azure Synapse Analytics 中的新模型。 使用平臺的擴充性和效能特性來執行轉換，而不會影響來源系統。

### <a name="data-model-migration"></a>資料模型遷移

根據來源系統的平臺和來源，部分或全部部分的資料模型可能已在星形或雪花式架構表單中。 若是如此，您可以直接將其遷移至 Azure Synapse Analytics。 此案例是最簡單且最低的風險遷移，以達成目標。 依原樣遷移也可以是更複雜遷移的第一個階段，包括轉換到新的基礎資料模型（例如資料保存庫），如先前所述。

任何一組關聯式資料表和視圖都可以遷移至 Azure Synapse Analytics。 針對大型資料集的分析查詢工作負載，星形或雪花式資料模型通常可提供最佳的整體效能。 如果源資料模型尚未採用此形式，可能值得使用遷移程式來 reengineer 模型。

如果遷移專案包含對資料模型所做的任何變更，最好的作法就是在新的目標環境中執行這些變更。 也就是說，先遷移現有的模型，然後使用 Azure Synapse Analytics 的強大功能和彈性，將資料轉換為新的模型。 這種方法可將對現有系統的影響降到最低，並使用 Azure Synapse Analytics 的效能和擴充性，以快速且符合成本效益的方式進行任何變更。

您可以將現有的系統移轉為數個層級;例如，資料內嵌/暫存層、資料倉儲層，以及報表或資料超市層。 每一層都包含關聯式資料表和觀點。 雖然您可以將這些功能遷移至 Azure Synapse Analytics，但是使用 Azure 生態系統的一些特性和功能可能更符合成本效益且更可靠。 例如：

- **資料內嵌和預備：** 您可以使用 Azure Blob 儲存體搭配 PolyBase 的一部分 ETL (解壓縮、轉換、載入) 或 ELT (解壓縮、載入、轉換) 程式，而不是使用關聯式資料表來進行快速平行的資料載入。

- **資料內嵌和預備：** 您可以使用 Azure Blob 儲存體搭配 PolyBase，以快速進行 ETL 的部分 (解壓縮、轉換、載入) 或 ELT (解壓縮、載入、轉換) 進程，而不是關聯式資料表。
- **報表層和資料超市：** Azure Synapse Analytics 的效能特性可能會讓您不必實際將匯總資料表具現化，以用於報告用途或資料超市。 您可以將這些功能實作為核心資料倉儲或協力廠商資料虛擬化層的觀點。 在基本層級中，您可以完成歷程記錄資料的資料移轉程式，也可能會有累加式更新，如下圖所示：

   ![說明新式資料倉儲的圖表。](../../../_images/analytics/schema-migration-ddl.png)
    _圖1：新式資料倉儲。_

如果您可以使用這些或類似的方法，將會減少要遷移的資料表數目。 某些處理常式可能已簡化或消除，進而減少遷移工作負載。 這些方法的適用性取決於個別的使用案例。 但一般原則是考慮盡可能使用 Azure 生態系統的功能和功能來減少遷移工作負載，並建立符合成本效益的目標環境。 這也適用于其他功能，例如備份/還原和工作流程管理與監視。

Microsoft 合作夥伴提供的產品和服務可協助您在某些情況下，對資料倉儲的遷移和自動化部分進行處理。 如果現有的系統納入協力廠商 ETL 產品，則可能已支援 Azure Synapse Analytics 作為目標環境。 現有的 ETL 工作流程可以重新導向至新的目標資料倉儲。

### <a name="data-marts-physical-or-virtual"></a>資料超市：實體或虛擬

如果組織使用較舊的資料倉儲環境來建立資料超市，以提供其部門或商務功能，並提供良好的臨機操作自助查詢和報告效能，這是常見的作法。 資料超市通常包含資料倉儲的子集，其中包含原始資料的匯總版本。 它的表單通常是維度資料模型，可讓使用者輕鬆地查詢資料，並從 Tableau、MicroStrategy 或 Microsoft Power BI 等方便使用的工具獲得快速回應時間。

資料超市的其中一種用法是以可用形式公開資料，即使基礎倉儲資料模型是不同 (例如資料保存庫) 。 這種方法也稱為三層式模型。

您可以針對組織內的個別營業單位使用不同的資料超市，以實行穩固的資料安全性制度。 例如，您可以允許使用者存取與其相關的特定資料超市，以及消除、模糊化或匿名敏感性資料。

如果這些資料超市實作為實體資料表，這些資料超市需要額外的儲存體資源來裝載它們以及進行額外的處理，以定期建立和重新整理它們。 實體資料表顯示超市中的資料只是最後一個重新整理作業的最新資料，因此可能不適用於高度變動性的資料儀表板。

隨著低成本、可擴充的大量平行處理 (MPP) 架構，您可以提供資料超市功能，而不需要將超市具現化為一組實體資料表。 您可以使用下列其中一種方法將資料超市虛擬化，透過 Azure Synapse Analytics 達成此目的：

- 主要資料倉儲上的 SQL 資料檢視。
- 使用 Azure Synapse Analytics 或協力廠商虛擬化產品（例如 Denodo）等功能的虛擬化層。

這種方法可簡化或消除額外儲存和匯總處理的需求。 它會減少要遷移的資料庫物件總數。

資料倉儲方法的另一個優點是在大型資料磁片區上執行作業（例如聯結和匯總）的容量。 例如，在虛擬化階層內執行匯總和聯結邏輯，並在虛擬化視圖中顯示外部報告，將建立這些視圖所需的強大處理推送至資料倉儲。

選擇實作為實體或虛擬資料超市實行的主要驅動因素如下：

- 更靈活。 虛擬資料超市比實體資料表和相關聯的 ETL 進程更容易變更。
- 降低擁有權總成本，因為虛擬化執行中的資料存放區和資料複本較少。
- 消除 ETL 作業，以在虛擬化環境中遷移和簡化資料倉儲架構。
- 效能。 在過去，實體資料超市更可靠。 虛擬化產品現在正在實行智慧型快取技術來減輕這個問題。

您也可以使用資料虛擬化，在遷移專案期間，以一致的方式向使用者顯示資料。

### <a name="data-mapping"></a>資料對應

#### <a name="key-and-integrity-constraints-in-azure-synapse-analytics"></a>Azure Synapse Analytics 中的關鍵和完整性條件約束

目前不會在 Azure Synapse Analytics 中強制使用 Primary key 和 foreign key 條件約束。 不過，您可以 `PRIMARY KEY` 在語句中包含子句的定義 `CREATE TABLE` `NOT ENFORCED` 。 這表示協力廠商報告產品可以使用資料表的中繼資料來瞭解資料模型中的索引鍵，因此產生最有效率的查詢。

#### <a name="data-type-support-in-azure-synapse-analytics"></a>Azure Synapse Analytics 中的資料類型支援

某些較舊的資料庫系統包含 Azure Synapse Analytics 中不直接支援的資料類型支援。 您可以使用支援的資料類型來儲存資料，或將資料轉換為支援的資料類型，以處理這些資料類型。

以下是支援的資料類型清單（以字母順序列出）：

- `bigint`
- `binary [ (n) ]`
- `bit`
- `char [ (n) ]`
- `date`
- `datetime`
- `datetime2 [ (n) ]`
- `datetimeoffset [ (n) ]`
- `decimal [ (precision [, scale ]) ]`
- `float [ (n) ]`
- `int`
- `money`
- `nchar [ (n) ]`
- `numeric [ (precision [ , scale ]) ]`
- `nvarchar [ (n | MAX) ]`
- `real [ (n) ]`
- `smalldatetime`
- `smallint`
- `smallmoney`
- `time [ (n) ]`
- `tinyint`
- `uniqueidentifier`
- `varbinary [ (n | MAX) ]`
- `varchar [ (n | MAX) ]`

下表列出目前不支援的常見資料類型，以及將它們儲存在 Azure Synapse Analytics 中的建議方法。

| 不支援的資料類型 | 因應措施 |
|--|--|
| `geometry` | `varbinary` |
| `geography` | `varbinary` |
| `hierarchyid` | `nvarchar(4000)` |
| `image` | `varbinary` |
| `text` | `varchar` |
| `ntext` | `nvarchar` |
| `sql_variant` | 將資料行分割成數個強型別資料行 |
| `table` | 轉換成臨時表 |
| `timestamp` | 改編要使用的程式碼和函式 `datetime2` `CURRENT_TIMESTAMP` |
| `xml` | `varchar` |
| 使用者定義型別 | 盡可能轉換回原生資料類型 |

#### <a name="potential-data-issues"></a>潛在的資料問題

根據來源環境而定，某些問題可能會在您遷移資料時發生問題：

- 在 `NULL` 不同的資料庫產品中處理資料的方式可能會有些許差異。 範例包括定序順序和空白字元字串的處理。
- `DATE`、 `TIME` 、 `INTERVAL` 和 `TIME ZONE` 資料以及相關聯的函式可能會隨著產品而異。

請徹底測試這些測試，以判斷是否要在目標環境中達成所需的結果。 遷移練習可以找出目前屬於現有來源系統一部分的錯誤或不正確的結果，而遷移程式是修正異常狀況的好機會。

#### <a name="best-practices-for-defining-columns-in-azure-synapse-analytics"></a>在 Azure Synapse Analytics 中定義資料行的最佳作法

較舊的系統通常會包含效率不佳資料類型的資料行。 例如，您可能會發現某個欄位定義為 `VARCHAR(20)` 實際資料值符合欄位的時間 `CHAR(5)` 。 或者， `INTEGER` 如果所有值都符合欄位，您可能會發現使用欄位 `SMALLINT` 。 資料類型不足可能會導致儲存體和查詢效能的效率不佳，尤其是在大型事實資料表中。

在遷移練習期間檢查和合理化目前的資料定義是很好的時機。 您可以使用 SQL 查詢來尋找資料欄位中的最大數值或字元長度，並比較結果與資料類型，來自動化這些工作。

一般來說，將資料表的總定義資料列長度最小化是很好的作法。 為了獲得最佳查詢效能，您可以使用每個資料行的最小資料類型，如先前所述。 從 Azure Synapse Analytics 中的外部資料表載入資料的建議方法是使用 PolyBase 公用程式，其支援的最大定義資料列長度為 1 mb (MB) 。 PolyBase 不會載入資料列超過 1 MB 的資料表，因此您必須改用[ `bcp` 公用程式](/sql/tools/bcp-utility)。

針對最有效率的聯結執行，請將聯結兩端的資料行定義為相同的資料類型。 如果維度資料表的索引鍵定義為 `SMALLINT` ，則使用該維度之事實資料表中的對應參考資料行也應定義為 `SMALLINT` 。

避免以大的預設大小定義字元欄位。 如果欄位內的資料大小上限為50個字元，請使用 `VARCHAR(50)` 。 同樣地， `NVARCHAR` 如果將足夠，請不要使用 `VARCHAR` 。 `NVARCHAR` 儲存 Unicode 資料以允許不同的語言字元集。 `VARCHAR` 會儲存 ASCII 資料並佔用較少的空間。

## <a name="summary-of-design-recommendations"></a>設計建議的摘要

請勿遷移不必要的物件或進程。 在適當的目標 Azure 環境中使用內建功能和函式，以減少實際要遷移的物件和進程數目。 請考慮使用虛擬化層來減少或消除您將遷移的實體資料超市數量，以及將處理推送至資料倉儲的次數。

盡可能自動化，並使用來源系統中系統目錄的中繼資料，為目標環境產生 Ddl。 可能的話，也會自動產生檔。 WhereScape 等 Microsoft 合作夥伴可以提供特殊的工具和服務，以協助進行自動化。

在目標平臺上執行任何必要的資料模型變更或資料對應優化。 您可以更有效率地在 Azure Synapse Analytics 中進行這些變更。 這種方法可減少可能已接近完整容量之來源系統的影響。

## <a name="performance-options"></a>效能選項

本節說明可讓您用來改善資料模型效能的 Azure Synapse Analytics 中可用的功能。

### <a name="general-approach"></a>一般方法

平臺的功能會在即將遷移的資料庫上執行效能微調。 索引、資料分割和資料散發是這類效能調整的範例。 當您準備進行遷移時，記錄微調可以捕獲並顯示可在 Azure Synapse Analytics 目標環境中套用的優化。

例如，在資料表上存在非唯一索引時，可以指出索引中使用的欄位經常用於篩選、分組或聯結。 這在新的環境中仍是如此，因此當您選擇要為其編制索引的欄位時，請記住這點。 如需有關 Teradata 或 Netezza 環境的詳細資訊，請參閱 Azure Synapse 分析文章， [以瞭解適用于 Teradata 和解決方案的解決方案和遷移](./analytics-solutions-teradata.md) ， [以及 Netezza 的遷移](./analytics-solutions-netezza.md)。

使用目標 Azure Synapse Analytics 環境的效能和擴充性，來試驗不同的效能選項，例如資料散發。 判斷替代方法的最佳選擇 (例如，針對大型維度資料表) 複寫與雜湊散發。 這並不表示必須從外部來源重載資料。 使用不同的資料分割或分佈選項透過語句來建立任何資料表的複本，可讓您快速且輕鬆地在 Azure Synapse Analytics 中測試替代方法 `CREATE TABLE AS SELECT` 。

使用 Azure 環境所提供的監視工具來瞭解查詢的執行方式，以及可能發生瓶頸的位置。 協力廠商 Microsoft 合作夥伴也可以使用工具來提供監視儀表板和自動化的資源管理和警示。

Azure Synapse Analytics 和資源中的每個 SQL 作業（例如記憶體或該查詢所使用的 CPU）都會記錄在系統資料表中。 一系列動態管理檢視可簡化這項資訊的存取。

下列各節說明 Azure SQL 資料倉儲中用來微調查詢效能的重要選項。 現有的環境會包含目標環境中可能優化的相關資訊。

### <a name="temporary-tables"></a>暫存資料表

Azure Synapse Analytics 支援僅對其建立所在之會話可見的臨時表。 它們存在於使用者會話的持續時間內，並會在會話結束時自動卸載。

若要建立臨時表，請在資料表名稱前面加上雜湊字元 (`#`) 。 您可以使用所有一般的索引和散發選項與臨時表，如下一節所述。

臨時表有一些限制：

- 不允許將其重新命名。
- 不允許對其進行流覽或分割。
- 不允許變更許可權。

臨時表通常用於 ETL/ELT 處理中，其中暫時性的中繼結果會在轉換過程中使用。

### <a name="table-distribution-options"></a>資料表散發選項

Azure Synapse Analytics 是 MPP 資料庫系統，可跨多個處理節點平行執行，以達到效能和擴充性。

在多節點環境中執行 SQL 查詢的理想處理案例是平衡工作負載，並為所有節點提供相等的資料量來處理。 這種方法也可讓您將必須在節點之間移動的資料量降到最低或減少，以滿足查詢。

達成理想的案例可能很困難，因為在一般分析查詢中通常會有匯總，而且在多個資料表之間有多個聯結，如同事實和維度資料表。

影響查詢處理方式的其中一種方式，是使用 Azure Synapse Analytics 內的散發選項來指定每個資料表的個別資料列的儲存位置。 例如，假設有兩個大型資料表聯結在資料行上 `CUSTOMER_ID` 。 藉由在每次執行聯結時透過資料行散發兩個數據表 `CUSTOMER_ID` ，您可以確保聯結的每一側的資料都已共置於相同的處理節點上。 這個方法不需要在節點之間移動資料。 資料表的散發規格是在語句中定義 `CREATE TABLE` 。

下列各節說明可用的散發選項，以及使用這些選項的建議。 如果有必要，您可以在初始載入之後變更資料表的散發：使用語句來重新建立具有新散發的資料表 `CREATE TABLE AS SELECT` 。

#### <a name="round-robin"></a>迴圈

迴圈配置資源資料表分佈是預設選項，會將資料平均分散到系統中的各個節點。 這種方法適用于快速載入資料，以及磁片區中相對較低且沒有明顯適合雜湊的資料。 它經常用於臨時表，作為 ETL 或 ELT 程式的一部分。

#### <a name="hashed"></a>散 列

系統會將資料列指派給雜湊值區，這是以套用至使用者定義索引鍵的雜湊演算法為基礎的工作，如 `CUSTOMER_ID` 上述範例所示。 然後，此值區會指派給特定的節點，而且在相同的值上雜湊散發的所有資料列都會在相同的處理節點上結束。

這個方法適用于經常聯結或匯總在索引鍵上的大型資料表。 要聯結的其他大型資料表應該盡可能在相同的索引鍵上進行雜湊處理。 如果雜湊索引鍵有多個候選項目，請選擇最常聯結的一個。

雜湊資料行不應包含 null，且通常不會是日期，因為許多查詢會依日期進行篩選。 如果雜湊的索引鍵是整數值而非或，則雜湊通常更有效率 `CHAR` `VARCHAR` 。 避免選擇具有高度扭曲範圍之值的索引鍵，例如少數的索引鍵值代表大量百分比的資料列。

#### <a name="replicated"></a>複寫

選擇複寫為數據表的散發選項，將會在每個計算節點上複寫該資料表的完整複本，以便進行查詢處理。

這種方法適用于相對較小型的資料表 (通常小於 2 GB 的壓縮) ，其相對靜態且通常會透過等聯結聯結至較大的資料表。 這些資料表通常是星狀架構中的維度資料表。

### <a name="indexing"></a>編製索引

Azure Synapse Analytics 包含在大型資料表中索引資料的選項，可減少取得記錄所需的資源和時間：

- 叢集資料行存放區索引
- 叢集索引
- 非叢集索引

如果 `HEAP` 資料表沒有索引選項，則不會有任何索引選項的非索引選項。 使用索引是在改善的查詢時間與較長的載入時間，以及更多儲存空間使用量之間的取捨。 索引通常 `SELECT` `UPDATE` `DELETE` `MERGE` 會在影響少量資料列的大型資料表上加速、、和作業，而且可以將完整的資料表掃描降至最低。

在資料 `UNIQUE` `PRIMARY KEY` 行上定義或條件約束時，會自動建立索引。

#### <a name="clustered-columnstore-index"></a>叢集資料行存放區索引

叢集資料行存放區索引是 Azure Synapse Analytics 中的預設索引選項。 它提供最佳的壓縮和查詢效能給大型資料表。 針對少於60000000個數據列的較小資料表，這些索引不會有效，因此您應該使用 `HEAP` 選項。 同樣地，如果資料表中的資料是暫時性的，而且是 ETL/ELT 進程的一部分，則堆積或臨時表可能更有效率。

#### <a name="clustered-index"></a>叢集索引

如果需要根據強式篩選準則，從大型資料表定期取得單一資料列或少量的資料列，叢集索引可能會比叢集資料行存放區索引更有效率。 每個資料表只允許有一個叢集索引。

#### <a name="non-clustered-index"></a>非叢集索引

非叢集索引類似于叢集索引，因為它們可以根據篩選準則來加速單一資料列或少量資料列的抓取。 在內部，非叢集索引會與資料分開儲存，而且可以在資料表上定義多個非叢集索引。 不過，每個額外的索引都需要更多的儲存空間，而且會減少資料插入或載入的輸送量。

#### <a name="heap"></a>堆積

堆積資料表在資料載入時，不會產生與索引建立和維護相關的額外負荷。 它們有助於在進程期間快速載入暫時性資料，包括 ELT 進程。 快取也可協助您在之後立即讀取資料。 因為叢集資料行存放區索引在60000000資料列底下沒有效率，所以堆積資料表也有助於儲存資料列小於此數量的資料表。

### <a name="data-partitioning"></a>資料分割

在企業資料倉儲中，事實資料表可包含許多數十億個數據列。 資料分割是一種方式，可將這些資料表的維護和查詢分割成不同的部分，以減少執行查詢時所處理的資料量。 資料表的資料分割規格是在語句中定義 `CREATE TABLE` 。

每個資料表只能使用一個欄位進行資料分割。 這通常是日期欄位，因為許多查詢都會依日期或日期範圍進行篩選。 您可以在初始載入之後變更資料表的資料分割（如有必要），方法是透過語句使用新的散發來重新建立資料表 `CREATE TABLE AS SELECT` 。

**查詢優化的資料分割：** 如果針對大型事實資料表的查詢經常依特定資料行進行篩選，則該資料行上的資料分割可以大幅減少執行查詢所需處理的資料量。 常見的範例是使用 date 欄位將資料表分割成較小的群組。 每個群組都會包含一天的資料。 當查詢包含 `WHERE` 篩選日期的子句時，只需要存取符合日期篩選的資料分割。

**資料表維護優化的資料分割：** 資料倉儲環境通常會維護詳細事實資料的滾動視窗。 例如，有五年的銷售交易。 藉由在銷售日期進行資料分割，移除超出滾動時間範圍的舊資料會變得更有效率。 卸載最舊的資料分割會更快，且使用的資源比刪除每個個別資料列更少。

### <a name="statistics"></a>統計資料

當查詢提交至 Azure Synapse Analytics 時，它會先由查詢最佳化工具進行處理。 優化工具會判斷最適合用來有效率地執行查詢的內部方法。

優化工具會根據以成本為基礎的演算法來比較可使用的各種查詢執行計畫。 成本預估的精確度取決於可用的統計資料。 確保統計資料是最新狀態是個不錯的作法。

在 Azure Synapse Analytics 中，如果此 `AUTO_CREATE_STATISTICS` 選項為開啟狀態，則會觸發自動更新統計資料。 您也可以透過命令手動建立或更新統計資料 `CREATE STATISTICS` 。

當內容大幅變更時重新整理統計資料，例如每日更新。 此重新整理可以併入 ETL 程式。

資料庫中的所有資料表都應至少在一個資料行上收集統計資料。 它可確保優化工具可以使用資料列計數和資料表大小等基本資訊。 其他應該會收集統計資料的資料行是 `JOIN` 、 `DISTINCT` 、 `ORDER BY` 和 `GROUP BY` 處理中指定的資料行。

### <a name="workload-management"></a>工作負載管理

Azure Synapse Analytics 併入了跨混合工作負載管理資源使用率的完整功能。 針對不同的工作負載類型（例如查詢與資料載入）建立資源類別，可協助您管理工作負載。 它會設定並存執行的查詢數目，以及指派給每個查詢的計算資源上的限制。 記憶體和平行存取之間會有取捨：

- 較小的資源類別會減少每個查詢的記憶體上限，但會增加並行。
- 較大的資源類別會增加每個查詢的記憶體上限，但會減少並行。

### <a name="performance-recommendations"></a>效能建議

使用效能改進方法（例如索引或資料散發）來測量新目標環境中類似方法的候選項目，但基準測試可確認它們在 Azure Synapse Analytics 中是必要的。 `COLLECT STATISTICS`在 ETL/ELT 進程中建立步驟，以確保統計資料是最新的，或選擇自動建立統計資料。

瞭解 Azure Synapse Analytics 中可用的微調選項，以及相關公用程式的效能特性，例如 PolyBase 以快速進行平行資料載入。 使用這些選項來建立有效率的端對端執行。

使用 Azure 環境的彈性、擴充性和效能，就地執行任何資料模型變更或效能微調選項。 這種努力會降低對現有來源系統的影響。

瞭解 Azure Synapse Analytics 中可用的動態管理檢視。 這些 views 提供整個系統的資源使用量資訊，以及個別查詢的詳細執行資訊。

瞭解 Azure 資源類別並適當地加以配置，以確保能夠有效率地管理混合的工作負載和平行存取。

請考慮在 Azure Synapse Analytics 環境中使用虛擬化層。 它可以防護商務使用者和報告工具在倉儲中的執行變更。

研究合作夥伴提供的遷移工具和服務，例如 Microsoft 遷移、WhereScape 和 Datometry hyper-q 的 Qlik sense 複寫。 這些服務可以自動化部分的遷移程式，並減少遷移專案所耗用的時間和風險。
