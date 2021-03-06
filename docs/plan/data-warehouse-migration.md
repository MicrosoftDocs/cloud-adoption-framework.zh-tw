---
title: 規劃資料倉儲移轉
description: 瞭解資料倉儲遷移的最佳作法、核心資料倉儲遷移程式步驟，以及每個步驟中的工作。
author: v-hanki
ms.author: janet
ms.date: 06/24/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.custom: think-tank
ms.openlocfilehash: 4b1e563cab763a8e40f7da1a43baa7dd2448ea0b
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102116320"
---
<!-- cSpell:ignore Informatica gzipped Attunity -->

# <a name="plan-a-data-warehouse-migration"></a>規劃資料倉儲移轉

資料倉儲遷移對於任何公司都是一項挑戰。 為了順利執行它並避免任何令人困擾的意外成本，您需要徹底研究挑戰、降低風險，並規劃您的遷移，以確保您盡可能做好準備。 概括而言，您的計畫應該涵蓋核心的資料倉儲遷移程式步驟和其中的任何工作。 主要處理步驟如下：

- 預先遷移準備
- 遷移策略和執行
- 移轉後

例如，準備工作包括在技能訓練和技術熟悉方面讓您的資料倉儲遷移團隊做好準備。 它也包括設定概念證明實驗室、瞭解您將如何管理測試和生產環境、取得適當的方式來遷移您的資料和公司防火牆外部的生產系統，以及在資料中心設定遷移軟體，以繼續進行遷移。

為了讓資料倉儲遷移順利進行，您的計畫應該清楚瞭解：

- 您的商務案例，包括其驅動程式、業務優點和風險。
- 遷移團隊角色和責任。
- 啟用成功遷移所需的技能設定和定型。
- 已配置完整遷移的預算。
- 您的遷移策略。
- 如何避免遷移專案中的風險，以避免延遲或重新修改。
- 您現有的資料倉儲系統、其架構、架構、資料磁片區、資料流程、安全性和操作相依性。
- 您現有的內部部署資料倉儲 DBMS 和 Azure Synapse 之間的差異，例如資料類型、SQL 函數、邏輯和其他考慮。
- 需要遷移的內容和優先順序。
- 遷移工作、方法、順序和期限。
- 您將如何控制遷移。
- 如何防止使用者在進行遷移時中斷。
- 您必須在內部部署環境中進行，以避免延遲並啟用遷移。
- 這些工具可讓您將架構、資料和 ETL 處理安全地遷移至 Azure。
- 在遷移期間和之後所需的資料模型設計變更。
- 任何遷移前或遷移後技術變更，以及如何將重做的最小化。
- 遷移後的技術淘汰。
- 您將如何實行測試和品質保證以成功證明。
- 用來評估進度和啟用決策的檢查點。
- 萬一發生問題，您的應變計劃和回復點也會發生。

為了達成這項瞭解，我們必須先準備並開始特定的活動，然後再開始進行任何遷移。 讓我們來看看有什麼需要更詳細的資訊。

## <a name="pre-migration-preparation"></a>預先遷移準備

在您開始進行資料倉儲遷移之前，應該先解決幾件事。

### <a name="key-roles-in-a-data-warehouse-migration-team"></a>資料倉儲遷移團隊中的重要角色

遷移專案中的重要角色包括：

- 企業主
- 專案管理員 (具有 agile 方法體驗，例如 Scrum) 
- 專案協調器
- 雲端工程師
- 資料庫管理員 (現有的資料倉儲 DBMS 和 Azure Synapse) 
- 資料建模者
- ETL 開發人員
- 資料虛擬化專家 (可能是資料庫管理員) 
- 測試工程師
- 商務分析師 (協助測試 BI 工具查詢、報表和分析) 

此外，小組需要您的內部部署基礎結構小組支援。

### <a name="skills-and-training-to-ready-the-team-for-migration"></a>讓團隊做好遷移的技能和訓練

關於技能，在資料倉儲遷移方面很重要。 因此，請確定您的遷移小組的適當成員已在 Azure 雲端基礎、Azure Blob 儲存體、Azure Data Lake Storage、Azure 資料箱、ExpressRoute、Azure 身分識別管理、Azure Data Factory 和 Azure Synapse 中進行訓練。 當您從現有的資料倉儲進行遷移之後，您的資料建模者很可能需要微調您的 Microsoft Azure Synapse 資料模型。

### <a name="assessing-your-existing-data-warehouse"></a>評估您現有的資料倉儲

準備遷移的另一個部分是需要完整評估現有的資料倉儲，以充分瞭解架構、資料存放區、架構、商務邏輯、資料流程、使用的 DBMS 功能、倉儲作業和相依性。 更深入瞭解更好。 深入瞭解系統的運作方式，有助於與所有基底進行通訊和涵蓋。

評量的目的不只是為了確保對整個遷移團隊的目前設定有詳細的瞭解，還可以瞭解目前設定中的優點和缺點。 因此，評估您目前的資料倉儲的結果，可能會影響您的遷移策略（以增益和轉移為範圍）。 例如，如果評量的結果是您的資料倉儲在生命週期結束，則在 Azure Synapse 上將資料移轉至新設計的資料倉儲，以及隨即轉移的方法，將會很清楚。

### <a name="on-premises-preparation-for-data-migration"></a>資料移轉的內部部署準備工作

除了準備和準備您的目標環境的遷移小組，以及評估您目前的設定，也同樣重要的是，在內部部署中設定動作也相當重要，因為生產資料倉儲通常會受到 IT 程式和核准程式的高度控制。 為了避免延遲，請確定您的資料中心基礎結構和營運小組已準備好將您的資料、架構、ETL 作業等遷移至 Azure 雲端。 資料移轉可能會透過下列方式進行：

- AzCopy 至 Azure Blob 儲存體。
- Microsoft Azure ExpressRoute 可將壓縮的資料直接傳輸到 Azure。
- 檔案匯出至 Azure 資料箱。

影響選取這些選項的主要因素是資料磁片區大小 (以 tb 為單位的) 和網路速度 (Mbps 的) 。 需要計算以判斷透過網路遷移資料所需的時間，並考慮資料可能會在您的資料倉儲中壓縮並在您匯出時變成未壓縮。 這種情況可能會減緩資料傳送速率。 使用上述任何一種方法來移動資料時，透過 Gzip 重新壓縮資料。 PolyBase 可以直接處理 gzipped 資料。 如果移動資料需要太長的時間，可能會透過 Azure 資料箱來遷移大型資料磁片區。

此外，若要讓 Azure Data Factory 控制從 Azure 匯出現有資料倉儲資料的執行，您的資料中心必須安裝自我裝載整合執行時間軟體，才能繼續進行遷移。 基於這些需求，如果需要正式核准才能達成此目的，則儘早啟動適當的核准程式以達成此目的，將有助於避免延遲行。

### <a name="azure-preparation-for-schema-and-data-migration"></a>適用于架構和資料移轉的 Azure 準備

就 Azure 端的準備工作而言，必須透過 Microsoft Azure ExpressRoute 或 Microsoft Azure 資料箱來管理資料匯入。 Azure Data Factory 管線是將您的資料載入至 Azure Blob 儲存體，然後使用 PolyBase 從該處載入至 Azure Synapse 的理想方式。 因此，Azure 端需要進行準備以開發這類管線。

替代方法是在 Azure 上使用現有的 ETL 工具（如果它支援 Azure Synapse），這表示在 azure 上設定 azure Marketplace 的工具，並準備管線以匯入您的資料並將其載入至 Azure Blob 儲存體。

## <a name="defining-a-migration-strategy"></a>定義遷移策略

### <a name="migration-goals"></a>移轉目標

在任何策略中，都必須定義一組目標或目標，以表示成功。 然後，您可以設定目標，以達成這些目標，以及負責達成這些目標的人員。 下表顯示在雲端資料倉儲遷移專案中設定目標的遷移目標和對應計量範例：

目標和度量範例的類型：

#### <a name="improve-overall-performance"></a>提升整體效能

- 資料移轉效能
- ELT 效能
- 資料載入效能
- 複雜的查詢效能
- 並行使用者數目

#### <a name="run-at-lower-cost"></a>以較低的成本執行

- 依工作負載計算的成本，例如每小時的計算時數 x 成本：
  - 標準報告
  - 特定查詢
  - Batch ELT 處理
- 儲存體 (接移、生產資料表、索引、暫存空間) 的成本

#### <a name="operate-with-better-availability-and-service-levels"></a>以更高的可用性和服務等級操作

- 服務等級協定
- 高可用性

#### <a name="improve-productively"></a>提高效率

- 工作自動化，減少系統管理人員人數

因此，成功的資料倉儲遷移可以被視為資料倉儲，以快速或更快的速度執行，而且成本比您從遷移的舊版系統更低。 將這些目標的擁有者指派給達成這些目標的責任。 它也可確保測試在概念證明實驗室 (如本指南的「取消風險」一節中所定義) 將會被視為成功，如果測試找出可達成目標的方式。

### <a name="migration-approach"></a>遷移方法

您有數個策略選項可將現有的資料倉儲遷移至 Azure Synapse：

- 依原樣隨即轉移您現有的資料倉儲。
- 簡化現有的資料倉儲，然後將它遷移。
- 在 Azure Synapse 上完全重新設計您的資料倉儲，並遷移您的資料。

評估現有資料倉儲的結果應該會大幅影響您的策略。 良好的評量結果可能會建議隨即轉移策略。 因為低彈性評等的中等結果可能表示需要在遷移前進行簡化。 不良的結果可能表示需要完整的重新設計。

隨即轉移您的架構，並嘗試將移動現有系統的工作降至最低。 如果您現有的 ETL 工具已支援 Azure Synapse，您可能可以最輕鬆地變更目標。 儘管如此，資料表類型、資料類型、SQL 函數、views、預存程式商務邏輯等也會有所差異。這項遷移系列中的較低層級檔會詳細說明這些差異和其相關方式。

在遷移前簡化您現有的資料倉儲，是為了降低遷移的複雜度。 它可能包括：

- 在遷移前移除或封存未使用的資料表，以避免遷移未使用的資料。
- 使用資料虛擬化軟體將實體資料超市轉換成虛擬資料超市，以減少您必須遷移的資料。 轉換也可提升彈性並降低擁有權總成本，因此可在遷移期間將其視為現代化。

您也可以先簡化，然後隨即轉移。

### <a name="migration-scope"></a>移轉範圍

無論您選擇哪一種策略，您都應該清楚地定義遷移的範圍、要遷移的內容，以及是否要以累加方式或全部進行遷移。 累加式遷移的其中一個範例是先遷移您的資料超市，然後再遷移您的資料倉儲。 這種方法可讓您專注于高優先順序的商務領域，同時讓您的小組在遷移資料倉儲本身之前，在每個超市個別遷移時，以累加方式建立專長。

### <a name="defining-what-has-to-be-migrated"></a>定義必須遷移的內容

製作所有需要遷移之專案的清查。 這包括架構、資料、ETL 流程 (管線) 、授權許可權、使用者、BI 工具語義存取層，以及分析應用程式。 本系列的每個較低層級的遷移文章中提供有關遷移清查的詳細資訊。 這些連結如下所示。

- 架構遷移、設計和效能考慮。
- 資料移轉、ETL 處理和負載。
- 存取安全性和資料倉儲作業。
- 視覺化和報表的遷移。
- 將 SQL 問題的影響降到最低。
- 可協助您進行資料倉儲遷移的協力廠商工具。

如果您不確定最佳方法，請在概念證明實驗室中進行測試，以找出最佳的技巧。 如需詳細資訊，請參閱如何將資料倉儲遷移專案的風險取消風險一節。

### <a name="migration-control"></a>遷移控制項

資料倉儲遷移至 Azure Synapse 牽涉到需要進行的工作：

- 內部部署，例如資料匯出。
- 在網路上，例如資料傳輸。
- 在 Azure 雲端中，例如資料轉換、整合和載入。

問題在於，如果腳本和公用程式全都在內部部署和 Azure 環境中獨立開發、測試和執行，則管理這些工作可能會很複雜。 這會增加複雜性，特別是在版本控制、測試管理和遷移執行不協調的情況下。

您應該避免這些複雜性，並透過原始檔控制存放庫從共同的位置控制這些複雜性，以管理從開發到測試和生產環境的變更。 遷移執行牽涉到需要在內部部署、網路和 Azure 中執行的工作。 因為 Azure Synapse 是目標環境，所以控制 Azure 的遷移執行可簡化管理。 使用 Azure Data Factory 建立遷移控制管線，以控制內部部署和 Azure 上的執行。 這會引進自動化並將錯誤降至最低。 Data Factory 會成為遷移協調流程工具，而不只是企業資料整合工具。

從 Azure 上執行的 Microsoft 合作夥伴控制可供使用的其他選項包括資料倉儲自動化工具，以嘗試將遷移自動化。 例如 WhereScape 和 Attunity 等廠商。 大部分的自動化工具都是以轉移的方法為目標。 在此情況下，這類工具可能不支援某些作業，例如預存程式。 這些產品和數個其他產品會在協力廠商工具專屬的個別指南中詳細說明，以協助您遷移至 Azure Synapse。

### <a name="migration-testing"></a>遷移測試

測試所需的第一件事，就是針對每個需要執行以驗證並指出成功的測試，定義一系列的測試和一組必要的結果。 請務必確保所有層面都經過測試，並與您現有和遷移的系統進行比較，包括：

- 結構描述
- 必要時轉換的資料類型
- 在 Azure Synapse 中使用使用者定義的架構來區分資料倉儲和資料超市資料表
- 使用者
- 這些角色的使用者角色和指派
- 資料存取安全性許可權
- 資料隱私權與合規性
- 管理管理功能的許可權
- 資料品質和完整性
- ETL 處理會在資料倉儲中填入 Azure Synapse，並從資料倉儲擴展至任何資料超市，包括測試
- 所有資料表中的所有資料列都是正確的，包括歷程記錄
- 緩時變維度處理
- 變更資料捕獲處理
- 使用跨系統可能不同之函式的計算和匯總
- 所有已知查詢、報表和儀表板的結果
- 效能和延展性
- 分析功能
- 新的隨用隨付環境中的成本

盡可能將測試自動化，讓每個測試都可重複使用，並啟用一致的方法來評估結果。 如果報表和儀表板不一致，則在遷移測試期間能夠比較原始和遷移系統之間的中繼資料歷程，是很重要的，因為它可以反白顯示差異，並找出不容易偵測到的位置。

安全地達成此目的的最佳方式是建立角色、將存取權限指派給角色，然後將使用者附加至角色。 若要存取您剛遷移的資料倉儲，請設定自動化程式來建立新的使用者並指派角色。 請務必將使用者從角色中移除。

將轉換傳達給所有使用者，讓他們知道有哪些變更和預期的情況。

## <a name="de-risking-your-data-warehouse-migration-project"></a>將資料倉儲遷移專案的風險取消風險

資料倉儲遷移的另一個重要因素是將專案的風險減少，以便將成功的可能性最大化。 有幾件事可以完成資料倉儲遷移的風險。 包括：

- 建立概念證明實驗室，讓您的小組可以試用、進行測試、瞭解任何問題，並找出有助於驗證遷移方法的修正和優化，進而改善效能並降低成本。 它也可協助您建立自動化工作的方式、使用內建工具和組建範本來取得最佳作法、學習經驗，以及追蹤所學到的經驗。 這是降低風險並提高成功機率的寶貴方法。 此外，您可以將擁有者指派給負責達成遷移目標和目標的測試，如您的遷移策略中所定義。
- 介紹 BI 工具與資料倉儲和資料超市之間的資料虛擬化。 使用資料虛擬化來引進使用者透明度，以降低資料倉儲遷移的風險，並使用資料虛擬化 BI 工具隱藏使用者的遷移，如下圖所示。

![資料倉儲遷移圖表](../_images/data-warehouse-migration.png)

其目的是要使用自助 BI 工具，以及要遷移之基礎資料倉儲和資料超市的實體架構，來打破商務使用者之間的相依性。 藉由引進資料虛擬化，在資料倉儲和資料超市遷移至 Azure Synapse 的過程中所做的任何架構替代 (例如，將效能優化) 可從商務使用者中隱藏，因為它們只會存取資料虛擬化層中的虛擬資料表。 如果需要結構變更，則只需要變更資料倉儲或資料超市與任何虛擬資料表之間的對應，讓使用者不會察覺這些變更，也不會察覺遷移。

- 在資料倉儲遷移之前，請先封存任何識別為從未使用的現有資料表，因為沒有使用的點遷移資料表。 其中一種可能的方法是將未使用的資料封存到 Azure Blob 儲存體或 Azure Data Lake Storage，並在 Azure Synapse 中建立外部資料表，以供該資料仍在線上。
- 請考慮使用開發版本在 Azure 上引進虛擬機器 (VM) 的可能性 (通常是在此 VM 上執行之現有舊版資料倉儲 DBMS 的免費) 。 這可讓您快速地將現有的資料倉儲架構移至 VM，並將其移至 Azure Synapse，同時在 Azure 雲端上運作。
- 定義遷移順序和相依性。
- 確定您的基礎結構和營運小組已準備好將您的資料移轉到遷移專案中。
- 找出 DBMS 功能的差異，以及專屬商務邏輯可能成為問題的地方。 例如，使用 ELT 處理的預存程式不太容易遷移，也不會包含任何中繼資料歷程，因為轉換是在程式碼中。
- 先考慮遷移資料超市的策略，再接著將資料倉儲作為資料超市來源的資料倉儲。 這是因為它可進行累加式遷移，讓它更容易管理，而且可以根據業務需求來排定遷移的優先順序。
- 在遷移之前，考慮使用資料虛擬化來簡化您目前的資料倉儲架構，例如，將資料超市取代為虛擬資料超市，讓您可以消除資料超市的實體資料存放區和 ETL 作業，而不會在遷移前遺失任何功能。 這樣做會減少要遷移的資料存放區數量、減少資料複本、降低擁有權的總成本，以及提升靈活性。 這需要在遷移您的資料倉儲之前，先從實體切換至虛擬資料超市。 在許多方面，您可以在遷移前考慮這項資料倉儲現代化步驟。

## <a name="next-steps"></a>下一步

如需有關資料倉儲遷移的詳細資訊，請從 Informatica 參加 [Azure 上的虛擬雲端資料倉儲現代化研討會](https://now.informatica.com/Microsoft_CDW_Workshops.html) 。
