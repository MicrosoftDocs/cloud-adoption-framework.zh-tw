---
title: 使用 SQL 遷移來加速遷移
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 使用 SQL 遷移來加速遷移
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/10/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 4af94af91874ac666f45a917eed003b3cf881c51
ms.sourcegitcommit: 35c162d2d09ec1c4a57d3d57a5db1d56ee883806
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2019
ms.locfileid: "72558216"
---
# <a name="accelerate-migration-with-a-sql-migration"></a>使用 SQL 遷移來加速遷移

遷移整個 SQL Server 實例可以加速工作負載遷移工作。 下列指導方針將擴充[Azure 遷移指南](../azure-migration-guide/index.md)的範圍，方法是將 SQL Server 遷移至工作負載重點的遷移工作之外。 這種方法可以使用單一資料平臺遷移來植入多個工作負載。

## <a name="general-scope-expansion"></a>一般範圍擴充

此範圍擴充所需的大部分工作，都是在遷移工作的必要條件、評估、遷移和優化程式期間發生。

<!-- markdownlint-disable MD026 -->

## <a name="is-this-expanded-scope-right-for-you"></a>這個擴充範圍適合您嗎？

[Azure 遷移指南](../azure-migration-guide/index.md)中所述的建議方法，是將每個資料結構與相關聯的工作負載一起遷移，做為單一遷移工作的一部分。 遷移的反復方法會減少探索、評估和其他可建立封鎖程式的工作，以及企業價值較慢的作業。

不過，某些資料結構可以透過個別的資料平臺遷移更有效率地進行遷移。 以下是一些可能會導致使用此擴充範圍指引的範例：

- **服務結束：** 快速移動 SQL Server 實例以避免服務終止的問題，可以在標準遷移工作之外，證明使用本指南。
- **SQL Server 服務：** 資料結構是更廣泛的解決方案的一部分，需要在虛擬機器上執行 SQL Server。 這對於利用 SQL Server 服務（例如 SQL Server Reporting Services、SQL Server Integration Services 或 SQL Server Analysis Services）的解決方案而言是很常見的。
- 高密度 **、低使用量資料庫：** SQL Server 具有高密度的資料庫。 每個資料庫都有較低的交易量，而且需要少量的計算資源。 應該考慮其他更現代化的解決方案，但 IaaS 方法可能會導致作業成本大幅降低。
- **擁有權總成本：** 適用時，可以將[Azure 混合式權益](https://azure.microsoft.com/pricing/hybrid-benefit)套用至清單價格，以建立 SQL server 的最低擁有成本。 這對於在多重雲端案例中裝載 SQL Server 的客戶而言特別常見。
- **遷移加速器：** 提升 SQL Server 實例的隨即轉移，可以在一個反復專案中移動數個資料庫。 這種方法有時可以讓未來的反復專案更明確地專注于應用程式和 Vm，在單一反覆運算中遷移更多工作負載。
- **VMWare 遷移：** 常見的內部部署架構包括虛擬主機上的應用程式和 Vm，以及裸機上的資料庫。 在遷移此通用架構時，可以 complimented VMWare 主機到 Azure VMWare 服務（AVS）的遷移，以遷移整個 SQL Server 實例以支援這些虛擬主機。 如需免費指引，請參閱[VMWare 主機遷移](./vmware-host.md)。

如果上述準則都不適用於此遷移，則最好繼續進行[標準遷移](../index.md)程式。 在標準程式中，資料結構會反復地與每個工作負載一起遷移。

如果本指南符合您的準則，請繼續進行此擴充的範圍指南，做為[標準遷移](../index.md)程式中的工作。 在必要條件階段中，您可以將工作整合到整體採用方案中。

## <a name="suggested-prerequisites"></a>建議的必要條件

在執行 SQL Server 遷移之前，請先加入資料資產，以擴展**數位資產**。 資料資產會記錄要考慮進行遷移的資料資產清查。 下表概述記錄資料資產的方法。

### <a name="server-inventory"></a>伺服器清查

以下是適用于 SQL server 的伺服器清查範例：

|SQL Server|目的|版本|[程度](../../manage/considerations/criticality.md)|[敏感性](../../govern/policy-compliance/data-classification.md)|資料庫計數|SSIS|SSRS|SSAS|叢集|節點數|
|---------|---------|---------|---------|---------|---------|---------|---------|
|sql-01|核心應用程式|2016|任務關鍵性|高度機密|40|N/A|N/A|N/A|是|3|
|sql-02|核心應用程式|2016|任務關鍵性|高度機密|40|N/A|N/A|N/A|是|3|
|sql-03|核心應用程式|2016|任務關鍵性|高度機密|40|N/A|N/A|N/A|是|3|
|sql-04|BI|2012|高|XX|6|N/A|機密|是-多維度 Cube|否|1|
|sql-05|整合|2008 R2|低|一般|20|是|N/A|N/A|否|1|

### <a name="database-inventory"></a>資料庫清查

以下是上述其中一個 SQL Server 的資料庫清查範例：

|伺服器|資料庫|[程度](../../manage/considerations/criticality.md)|[敏感性](../../govern/policy-compliance/data-classification.md)|DMA 結果|DMA 補救|目標平臺|
|---------|---------|---------|---------|---------|---------|---------|
|sql-01|DB-1|任務關鍵性|高度機密|性|N/A|Azure SQL Database|
|sql-01|DB-2|高|機密|需要變更架構|已執行的變更|Azure SQL Database|
|sql-01|DB-1|高|一般|性|N/A|Azure SQL 受控執行個體|
|sql-01|DB-1|低|高度機密|需要變更架構|已排程變更|Azure SQL 受控執行個體|
|sql-01|DB-1|任務關鍵性|一般|性|N/A|Azure SQL 受控執行個體|
|sql-01|DB-2|高|機密|性|N/A|Azure SQL Database|

### <a name="integration-with-the-cloud-adoption-plan"></a>與雲端採用方案整合

探索完成之後，方案可以包含在[雲端採用方案](../../plan/template.md)中。 在雲端採用方案中，新增每個 SQL Server 實例，以作為[不同的工作負載](../../plan/workloads.md)進行遷移。 在每個工作負載中，資料庫和服務（SSIS、SSAS、SSRS）都可以新增為[資產](../../plan/workloads.md)。 若要將工作負載和資產大量加入採用方案，請參閱[使用 Excel 新增和編輯工作專案](https://docs.microsoft.com/azure/devops/boards/backlogs/office/bulk-add-modify-work-items-excel?view=azure-devops)。

一旦工作負載和資產包含在方案中，小組可以繼續進行標準的遷移程式，使用採用計畫來引導工作。 當採用小組進入評估、遷移和優化程式時，應將下列變更納入考慮。

## <a name="assessment-process-changes"></a>評定程式變更

如果方案中的任何資料庫可以遷移至平臺即服務（PaaS）資料平臺，請使用 Data Migration Assistant 來評估所選資料庫的相容性。 當資料庫需要架構轉換時，建議您在評估過程中完成那些轉換，以避免發生遷移管線中斷的情況。

### <a name="suggested-action-during-the-assessment-process"></a>評估過程中的建議動作

對於可遷移至 PaaS 解決方案的資料庫，下列動作會在評估過程中完成。

- **使用 Data Migration Assistant 進行評估：** 使用 Data Migration Assistant 來偵測可能影響目標 Azure SQL Database 受控實例中資料庫功能的相容性問題，以建議效能和可靠性的改進，以及移動架構、資料和非內含性物件從您的來源伺服器到目標伺服器。 如需詳細資訊，請參閱[Data Migration Assistant](/sql/dma/dma-overview)。
- **補救和轉換：** 根據 Data Migration Assistant 的輸出，轉換來源資料架構以補救相容性問題。 使用相依應用程式測試已轉換的資料結構描述。

## <a name="migrate-process-changes"></a>遷移程序變更

在遷移期間，有多個工具和方法選項。 但是每一種方法都遵循簡單的程式：遷移架構、資料和物件。 將資料同步處理至目標資料來源。

資料結構和服務的目標和來源可以進行這兩個步驟，而不是複雜。 請參閱下列各節，以瞭解根據您的遷移決策的最佳工具選擇。

### <a name="suggested-action-during-the-migrate-process"></a>遷移程序進行期間的建議動作

建議的遷移和同步處理路徑會使用下列三個工具的組合。 下列各節將概述更複雜的遷移和同步處理選項，讓您有更多的目標和來源解決方案。

|遷移選項|目的|
|---------|---------|
|[Azure Database Migration Service](/sql/dma/dma-overview)|Azure DMS 支援從 SQL Server 2005、SQL Server 2008 和 SQL Server 2008 R2、SQL Server 2012、SQL Server 2014、SQL Server 2016 和 SQL Server 2017，大規模地進行線上（最短停機時間）和離線（單次）遷移至 Azure SQL Database 受控實例。|
|[異動複寫](/sql/relational-databases/replication/administration/enhance-transactional-replication-performance)|Azure SQL Database 受控實例的異動複寫支援從下列版本進行遷移： SQL Server 2012 （SP2 CU8、SP3 或更新版本）、SQL Server 2014 （RTM CU10 或更新版本，或 SP1 CU3 或更新版本）、SQL Server 2016、SQL Server 2017|
|[大量載入](/sql/t-sql/statements/bulk-insert-transact-sql)|針對儲存于 SQL Server 2005、SQL Server 2008 和 SQL Server 2008 R2、SQL Server 2012、SQL Server 2014、SQL Server 2016 和 SQL Server 2017 的資料，使用大量載入 Azure SQL Database 受控實例。|

### <a name="guidance-and-tutorials-for-suggested-migration-process"></a>建議的遷移程式的指引和教學課程

選擇使用 DMS 進行遷移的最佳指引是在選擇的來源和目標平臺上。 下表概述使用 DMS 遷移 SQL database 之每個標準方法的教學課程。

|來源  |確定目標  |工具  |遷移類型  |指導方針  |
|---------|---------|---------|---------|---------|
|SQL Server|Azure SQL Database|DMS|離線|[教學課程](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql)|
|SQL Server|Azure SQL Database|DMS|線上|[教學課程](https://docs.microsoft.com/azure/dms/tutorial-sql-server-azure-sql-online)|
|SQL Server|Azure SQL Database 受控執行個體|DMS|離線|[教學課程](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)|
|SQL Server|Azure SQL Database 受控執行個體|DMS|線上|[教學課程](https://docs.microsoft.com/azure/dms/tutorial-sql-server-managed-instance-online)|
|RDS SQL Server|Azure SQL Database （或受控執行個體）|DMS|線上|[教學課程](https://docs.microsoft.com/azure/dms/tutorial-rds-sql-server-azure-sql-and-managed-instance-online)|

### <a name="guidance-and-tutorials-for-various-services-to-equivalent-paas-solutions"></a>適用于對等 PaaS 解決方案之各種服務的指引和教學課程

將資料庫從 SQL server 移至 DMS 之後，可以在許多 PaaS 解決方案中重新裝載架構和資料。 不過，其他必要的服務可能仍在該伺服器上執行。 下列三個教學課程將協助您在 Azure 上將 SSIS、SSAS 和 SSRS 移至對等的 PaaS 服務。

|來源  |確定目標  |工具  |遷移類型  |指導方針  |
|---------|---------|---------|---------|---------|
|SQL Server 整合服務|Data Factory Integration Runtime|Azure Data Factory|離線|[教學課程](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)|
|SQL Server 分析服務-表格式模型|Azure Analysis Service|SSDT|離線|[教學課程](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy)|
|SQL Server 報表服務|Power BI 報表伺服器|Power BI|離線|[教學課程](/power-bi/report-server/migrate-report-server)|

### <a name="guidance-and-tutorials-for-migration-from-sql-server-to-an-iaas-instance-of-sql-server"></a>從 SQL Server 遷移至 IaaS 實例 SQL Server 的指引和教學課程

將資料庫和服務遷移至 PaaS 實例之後，仍然可能會有與 PaaS 不相容的資料結構和服務。 當現有的條件約束阻止遷移資料結構或服務時，下列教學課程可協助將資料組合中的各種資產遷移至 Azure IaaS 解決方案。

這個方法可以用來在 SQL Server 或在來源 SQL Server 上執行的其他服務上遷移資料庫。

|來源  |確定目標  |工具  |遷移類型  |指導方針  |
|---------|---------|---------|---------|---------|
|單一實例 SQL Server|IaaS 上的 SQL Server|多變|離線|[教學課程](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-migrate-sql)|

## <a name="optimization-process-changes"></a>優化處理變更

在優化期間，每個資料結構、服務或 SQL Server 實例都可以進行測試、優化和升級至生產環境。 這是從每個工作負載遷移模型而偏離的最大影響。

在理想的情況下，相依的工作負載、應用程式和 Vm 將會在與 SQL Server 實例相同的反復專案中進行遷移。 當這種理想的情況發生時，可以與資料來源一起測試工作負載。 經過測試之後，資料結構就可以升級至生產環境，同步處理常式也可以終止。

可惜的是，在非工作負載導向的遷移期間，優化程式的最大變更，是在資料庫移轉與工作負載遷移之間有顯著的時間差距時。 當多個資料庫移轉為 SQL Server 遷移的一部分時，這些資料庫可能會並存在雲端和內部部署中，以進行多個反復專案。 在該時間範圍內，需要維護資料同步處理，直到這些相依資產被遷移、測試及升級為止。

在所有相依的工作負載都升級之前，小組會負責支援從來源系統將資料同步處理到目標系統。 這種同步處理會耗用網路頻寬、雲端成本，而且最重要的是人們的時間。 在 SQL Server 遷移「工作負載」和所有相依的工作負載/應用程式中適當地對齊採用方案，將可降低這種昂貴的負擔。

### <a name="suggested-action-during-the-optimization-process"></a>優化程式期間的建議動作

在優化程式期間，必須在每次反覆運算時完成下列工作，直到所有資料結構和服務都升級至生產環境為止。

1. 驗證資料的同步處理。
2. 測試任何遷移的應用程式。
3. 將應用程式和資料結構優化，以調整成本。
4. 將應用程式升階至生產環境。
5. 針對內部部署資料庫的持續內部部署流量進行測試。
6. 終止任何升級至生產環境之資料的同步處理。
7. 終止原始源資料庫。

在步驟5通過之前，無法終止資料庫和同步處理。 在 SQL Server 上的所有資料庫都已完成步驟 1-7 之前，內部部署 SQL Server 應該視為生產環境，而且應該維護所有同步處理。

## <a name="next-steps"></a>後續步驟

回到[擴充的範圍檢查清單](./index.md)，以確保您的移轉方法已完全配合。

> [!div class="nextstepaction"]
> [擴充的範圍檢查清單](./index.md)
