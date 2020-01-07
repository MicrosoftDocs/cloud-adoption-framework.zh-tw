---
title: 藉由遷移 SQL Server 的實例來加速遷移
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 遷移整個 SQL Server 實例可以加速工作負載遷移工作。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/10/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: e499e499cf1639bf9ce1118dcb93254268e9cb54
ms.sourcegitcommit: 3c325764ad8229b205d793593ff344dca3a0579b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/23/2019
ms.locfileid: "75328917"
---
# <a name="accelerate-migration-by-migrating-multiple-databases-or-entire-sql-servers"></a>藉由遷移多個資料庫或整個 SQL Server 來加速遷移

遷移整個 SQL Server 實例可以加速工作負載遷移工作。 下列指導方針藉由將 SQL Server 的實例遷移至工作負載焦點的遷移工作，來擴充[Azure 遷移指南](../azure-migration-guide/index.md)的範圍。 這種方法可以使用單一資料平臺遷移來植入多個工作負載。 此範圍擴充所需的大部分工作都是在遷移工作的必要條件、評估、遷移和優化程式期間進行。

<!-- markdownlint-disable MD026 -->

## <a name="is-this-expanded-scope-right-for-you"></a>這個擴充範圍適合您嗎？

[Azure 遷移指南](../azure-migration-guide/index.md)中建議的方法是在單一遷移工作中，將每個資料結構與相關聯的工作負載一起遷移。 遷移的反復方法會減少探索、評估和其他可建立封鎖程式的工作，以及企業價值較慢的作業。

不過，某些資料結構可以透過個別的資料平臺遷移更有效率地進行遷移。 以下是一些範例：

- **服務結束：** 在較大的遷移工作中，將 SQL Server 實例快速移動為隔離的反復專案，可以避免服務結束的問題。 本指南將協助您在更廣泛的遷移程式中整合 SQL Server 的遷移。 不過，如果您要遷移/升級 SQL Server，而不受任何其他雲端採用工作的影響，則[SQL Server 生命週期結束總覽](/sql/sql-server/end-of-support/sql-server-end-of-life-overview)或[SQL Server 遷移檔](/sql/sql-server/migrate/index)文章可能會提供更清楚的指引。
- **SQL Server 服務：** 資料結構是更廣泛的解決方案的一部分，需要在虛擬機器上執行 SQL Server。 這對於使用 SQL Server 服務（例如 SQL Server Reporting Services、SQL Server Integration Services 或 SQL Server Analysis Services）的解決方案而言是很常見的。
- 高密度 **、低使用量資料庫：** SQL Server 的實例具有高密度的資料庫。 其中每個資料庫都有較低的交易磁片區，而且幾乎不需要計算資源的方式。 您應該考慮其他更現代化的解決方案，但基礎結構即服務（IaaS）方法可能會導致作業成本大幅降低。
- **擁有權總成本：** 適用時，您可以將[Azure 混合式權益](https://azure.microsoft.com/pricing/hybrid-benefit)套用至清單價格，以建立 SQL Server 實例的最低擁有成本。 這對於在多重雲端案例中裝載 SQL Server 的客戶而言特別常見。
- **遷移加速器：** 「隨即轉移」 SQL Server 實例的遷移可以在一個反復專案中移動數個資料庫。 這種方法有時可以讓未來的反復專案更明確地專注于應用程式和 Vm，這表示您可以在單一反覆運算中遷移更多工作負載。
- **VMware 遷移：** 常見的內部部署架構包括虛擬主機上的應用程式和 Vm，以及裸機上的資料庫。 在此案例中，您可以遷移整個 SQL Server 實例，以支援將 VMware 主機遷移至 Azure VMware 服務。 如需詳細資訊，請參閱[VMware 主機遷移](./vmware-host.md)。

如果上述準則皆不適用於這項遷移，則最好繼續進行[標準遷移](../index.md)程式。 在標準程式中，資料結構會與每個工作負載一起反復遷移。

如果本指南符合您的準則，請繼續進行此擴充的範圍指南，做為[標準遷移](../index.md)程式中的工作。 在必要條件階段中，您可以將工作整合到整體採用方案。

## <a name="suggested-prerequisites"></a>建議的必要條件

在執行 SQL Server 遷移之前，請先加入資料資產，以擴展數位資產。 資料資產會記錄您要考慮進行遷移的資料資產清查。 下表概述記錄資料資產的方法。

### <a name="server-inventory"></a>伺服器清查

以下是伺服器清查的範例：

|SQL Server|目的|版本|[重要性](../../manage/considerations/criticality.md)|[敏感性](../../govern/policy-compliance/data-classification.md)|資料庫計數|SSIS|SSRS|SSAS|叢集|節點數|
|---------|---------|---------|---------|---------|---------|---------|---------|---------|---------|---------|
|sql-01|核心應用程式|2016|關鍵任務|高度機密|40|N/A|N/A|N/A|是|3|
|sql-02|核心應用程式|2016|關鍵任務|高度機密|40|N/A|N/A|N/A|是|3|
|sql-03|核心應用程式|2016|關鍵任務|高度機密|40|N/A|N/A|N/A|是|3|
|sql-04|BI|2012|高|XX|6|N/A|Confidential|是-多維度 cube|否|1|
|sql-05|整合|2008 R2|低|一般|20|是|N/A|N/A|否|1|

### <a name="database-inventory"></a>資料庫清查

以下是上述其中一部伺服器的資料庫清查範例：

|伺服器|資料庫|[重要性](../../manage/considerations/criticality.md)|[敏感性](../../govern/policy-compliance/data-classification.md)|Data Migration Assistant （DMA）結果|DMA 補救|目標平台|
|---------|---------|---------|---------|---------|---------|---------|
|sql-01|DB-1|關鍵任務|高度機密|相容|N/A|Azure SQL Database|
|sql-01|DB-2|高|Confidential|需要變更架構|已執行的變更|Azure SQL Database|
|sql-01|DB-3|高|一般|相容|N/A|Azure SQL 受控執行個體|
|sql-01|DB-4|低|高度機密|需要變更架構|已排程變更|Azure SQL 受控執行個體|
|sql-01|DB-5|關鍵任務|一般|相容|N/A|Azure SQL 受控執行個體|
|sql-01|DB-6|高|Confidential|相容|N/A|Azure SQL Database|

### <a name="integration-with-the-cloud-adoption-plan"></a>與雲端採用方案整合

在此探索程式完成後，您可以將它納入[雲端採用方案](../../plan/template.md)。 在雲端採用方案中，新增每個 SQL Server 實例，以作為[不同的工作負載](../../plan/workloads.md)進行遷移。 在每個工作負載中，資料庫和服務（SSIS、SSAS、SSRS）都可以新增為[資產](../../plan/workloads.md)。 若要將工作負載和資產大量加入採用方案，請參閱[使用 Excel 新增和編輯工作專案](https://docs.microsoft.com/azure/devops/boards/backlogs/office/bulk-add-modify-work-items-excel?view=azure-devops)。

在方案中包含工作負載和資產之後，您和您的小組可以使用採用方案繼續進行標準的遷移程式。 當採用小組進入評估、遷移和優化程式時，請考慮下列各節所討論的變更。

## <a name="assessment-process-changes"></a>評定程式變更

如果方案中的任何資料庫可以遷移至平臺即服務（PaaS）資料平臺，請使用 DMA 評估所選資料庫的相容性。 當資料庫需要架構轉換時，您應該在評估程式中完成那些轉換，以避免發生遷移管線中斷的情況。

### <a name="suggested-action-during-the-assessment-process"></a>評估過程中的建議動作

對於可以遷移至 PaaS 解決方案的資料庫，在評估程式期間會完成下列動作。

- **使用 DMA 進行評估：** 使用 Data Migration Assistant 來偵測可能影響目標 Azure SQL Database 受控實例中資料庫功能的相容性問題。 使用 DMA 來建議效能和可靠性改進，以及將架構、資料和非內含性物件從來源伺服器移至目標伺服器。 如需詳細資訊，請參閱[Data Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview)。
- **補救和轉換：** 根據 DMA 的輸出，轉換來源資料架構以補救相容性問題。 使用相依應用程式測試已轉換的資料結構描述。

## <a name="migrate-process-changes"></a>遷移程序變更

在遷移期間，您可以從許多不同的工具和方法中選擇。 但是每一種方法都遵循簡單的程式：遷移架構、資料和物件。 然後將資料同步處理至目標資料來源。

資料結構和服務的目標和來源可以進行這兩個步驟，而不是複雜。 下列各節可協助您根據您的遷移決策，瞭解最佳的工具選擇。

### <a name="suggested-action-during-the-migrate-process"></a>遷移程序進行期間的建議動作

建議的遷移和同步處理路徑會使用下列三個工具的組合。 下列各節將概述更複雜的遷移和同步處理選項，讓您有更多的目標和來源解決方案。

|移轉選項|目的|
|---------|---------|
|[Azure Database Migration Service](https://docs.microsoft.com/sql/dma/dma-overview)|支援線上（最短停機時間）和離線（一次）大規模遷移至 Azure SQL Database 受控實例。 支援從： SQL Server 2005、SQL Server 2008 和 SQL Server 2008 R2、SQL Server 2012、SQL Server 2014、SQL Server 2016 和 SQL Server 2017 進行遷移。|
|[異動複寫](https://docs.microsoft.com/sql/relational-databases/replication/administration/enhance-transactional-replication-performance)|Azure SQL Database 受控實例的異動複寫支援從下列版本進行遷移： SQL Server 2012 （SP2 CU8、SP3 或更新版本）、SQL Server 2014 （RTM CU10 或更新版本，或 SP1 CU3 或更新版本）、SQL Server 2016、SQL Server 2017。|
|[大量載入](https://docs.microsoft.com/sql/t-sql/statements/bulk-insert-transact-sql)|針對儲存在中的資料，使用大量載入 Azure SQL Database 受控實例： SQL Server 2005、SQL Server 2008 和 SQL Server 2008 R2、SQL Server 2012、SQL Server 2014、SQL Server 2016 和 SQL Server 2017。|

### <a name="guidance-and-tutorials-for-suggested-migration-process"></a>建議的遷移程式的指引和教學課程

選擇使用 Azure 資料庫移轉服務來進行遷移的最佳指引，會放在所選的來源和目標平臺上。 下錶鏈接至使用 Azure 資料庫移轉服務遷移 SQL database 之每個標準方法的教學課程。

|來源  |確定目標  |工具  |遷移類型  |指導方針  |
|---------|---------|---------|---------|---------|
|SQL Server|Azure SQL Database|Database Migration Service|離線|[教學課程](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql)|
|SQL Server|Azure SQL Database|Database Migration Service|線上|[教學課程](https://docs.microsoft.com/azure/dms/tutorial-sql-server-azure-sql-online)|
|SQL Server|Azure SQL Database 受控執行個體|Database Migration Service|離線|[教學課程](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)|
|SQL Server|Azure SQL Database 受控執行個體|Database Migration Service|線上|[教學課程](https://docs.microsoft.com/azure/dms/tutorial-sql-server-managed-instance-online)|
|RDS SQL Server|Azure SQL Database （或受控實例）|Database Migration Service|線上|[教學課程](https://docs.microsoft.com/azure/dms/tutorial-rds-sql-server-azure-sql-and-managed-instance-online)|

### <a name="guidance-and-tutorials-for-various-services-to-equivalent-paas-solutions"></a>適用于對等 PaaS 解決方案之各種服務的指引和教學課程

將資料庫從 SQL Server 實例移至 Azure 資料庫移轉服務之後，您可以在許多 PaaS 解決方案中重新裝載架構和資料。 不過，其他必要的服務可能仍在該伺服器上執行。 下列三個教學課程有助於將 SSIS、SSAS 和 SSRS 移至 Azure 上的對等 PaaS 服務。

|來源  |確定目標  |工具  |遷移類型  |指導方針  |
|---------|---------|---------|---------|---------|
|SQL Server Integration Services|Azure Data Factory 整合執行時間|Azure Data Factory|離線|[教學課程](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)|
|SQL Server Analysis Services-表格式模型|Azure Analysis Services|SQL Server Data Tools|離線|[教學課程](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy)|
|SQL Server Reporting Services|Power BI 報表伺服器|Power BI|離線|[教學課程](https://docs.microsoft.com/power-bi/report-server/migrate-report-server)|

### <a name="guidance-and-tutorials-for-migration-from-sql-server-to-an-iaas-instance-of-sql-server"></a>從 SQL Server 遷移至 IaaS 實例 SQL Server 的指引和教學課程

將資料庫和服務遷移至 PaaS 實例之後，您可能還是會有與 PaaS 不相容的資料結構和服務。 當現有的條件約束阻止遷移資料結構或服務時，下列教學課程可協助將資料組合中的各種資產遷移至 Azure IaaS 解決方案。

使用此方法可在 SQL Server 的實例上遷移資料庫或其他服務。

|來源  |確定目標  |工具  |遷移類型  |指導方針  |
|---------|---------|---------|---------|---------|
|單一實例 SQL Server|IaaS 上的 SQL Server|多變|離線|[教學課程](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-migrate-sql)|

## <a name="optimization-process-changes"></a>優化處理變更

在優化期間，您可以對生產環境中的每個資料結構、服務或 SQL Server 實例進行測試、優化和升級。 這是從每個工作負載遷移模型而偏離的最大影響。

在理想的情況下，您會在與 SQL Server 實例相同的反復專案中遷移相依的工作負載、應用程式和 Vm。 當這種情況發生時，您可以測試工作負載和資料來源。 測試之後，您可以將資料結構升級至生產環境，並終止同步處理常式。

現在讓我們來看一下資料庫移轉與工作負載遷移之間有顯著時間差距的案例。 可惜的是，在非工作負載驅動的遷移期間，這可能是優化程式的最大變更。 當您將多個資料庫移轉為 SQL Server 遷移的一部分時，這些資料庫可能會並存于雲端和內部部署中，以進行多個反復專案。 在這段時間內，您必須維護資料同步處理，直到這些相依資產被遷移、測試及升級為止。

在所有相依的工作負載都升級之前，您和您的小組會負責支援從來源系統將資料同步處理到目標系統。 這種同步處理會耗用網路頻寬、雲端成本，而且最重要的是人們的時間。 在 SQL Server 遷移工作負載與所有相依的工作負載和應用程式之間適當地對齊採用方案，可以降低這種昂貴的負荷。

### <a name="suggested-action-during-the-optimization-process"></a>優化程式期間的建議動作

在優化程式期間，請在每次反覆運算時完成下列工作，直到所有資料結構和服務都升級至生產環境為止。

1. 驗證資料的同步處理。
2. 測試任何遷移的應用程式。
3. 將應用程式和資料結構優化，以調整成本。
4. 將應用程式升階至生產環境。
5. 針對內部部署資料庫的持續內部部署流量進行測試。
6. 終止任何升級至生產環境之資料的同步處理。
7. 終止原始源資料庫。

在步驟5通過之前，您無法終止資料庫和同步處理。 在 SQL Server 實例上的所有資料庫都已完成所有七個步驟之前，您應該將內部部署的 SQL Server 實例視為生產環境。 應維持所有同步處理。

## <a name="next-steps"></a>後續步驟

回到[擴充的範圍檢查清單](./index.md)，以確保您的移轉方法已完全配合。

> [!div class="nextstepaction"]
> [擴充的範圍檢查清單](./index.md)
