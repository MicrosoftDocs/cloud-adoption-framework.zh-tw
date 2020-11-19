---
title: 遷移資產
description: 藉由識別要使用的適當工具，包括原生工具、第三方工具和專案管理工具，來起始移轉到 Azure。
author: matticusau
ms.author: mlavery
ms.date: 09/02/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: fasttrack-new, AQC
ms.localizationpriority: high
ms.openlocfilehash: 2abab02b5d73bd69b33c8124fb4500129e97e545
ms.sourcegitcommit: a7eb2f6c4465527cca2d479edbfc9d93d1e44bf1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94712420"
---
# <a name="deploy-workloads-and-assets-infrastructure-apps-and-data"></a>部署工作負載和資產 (基礎結構、應用程式和資料)

在此旅程階段中，您會使用評估階段的輸出來起始環境的移轉。 本指南可協助您找出適當的工具以達到已完成狀態。 您將探索原生工具、協力廠商工具和專案管理工具。

## <a name="native-migration-tools"></a>[原生移轉工具](#tab/Tools)

下列各節說明可用來執行或輔助移轉的原生 Azure 工具。 如需如何選擇適當工具以支援移轉工作的相關資訊，請參閱[雲端採用架構的移轉工具決策指南](../../decision-guides/migrate-decision-guide/index.md)。

### <a name="azure-migrate"></a>Azure Migrate

Azure Migrate 提供統一且可擴充的移轉體驗。 Azure Migrate 提供一站式的專屬體驗，可讓您在評估和移轉至 Azure 的各個階段追蹤您的移轉旅程。 它可讓您選擇使用自己所選的工具，並在這些工具間追蹤移轉的進度。

Azure Migrate 是集中式的中樞，可評估內部部署伺服器、基礎結構、應用程式與資料，並將其遷移至 Azure。 這個攔截器提供的功能包括：

- 統合平台的評估、移轉和進度追蹤。
- 增強的評估及移轉功能：
  - 包含 Hyper-v 和 VMware 的內部部署伺服器。
  - VMware 虛擬機器對 Azure 的無代理程式移轉。
  - 資料庫移轉至 Azure SQL Database 或 SQL 受控執行個體
  - Web 應用程式
  - 虛擬桌面基礎結構 (VDI) 遷移至 Azure 中的 Windows 虛擬桌面
  - 使用 Azure 資料箱產品的大型資料集合
- ISV 整合的可擴充方法 (例如 Cloudamize)。

若要使用 Azure Migrate 執行移轉，請遵循下列步驟：

1. 在 [所有服務] 下方搜尋 Azure Migrate。 選取 [Azure Migrate] 以繼續作業。
1. 選取 [新增工具] 以啟動移轉專案。
1. 選取要裝載移轉的訂用帳戶、資源群組和地理位置。
1. 選取 選取評估工具 > **Azure Migrate：伺服器評量 > 下一步** 。
1. 選取 [檢閱 + 新增工具]，並驗證設定。 選取 [新增工具] 以起始用來建立移轉專案的作業，並註冊選取的解決方案。

> [!NOTE]
> 如需您案例的特定指引，請參閱教學課程和 Azure Migrate [文件](/azure/migrate/migrate-services-overview)。
>

#### <a name="learn-more"></a>深入了解

- [關於 Azure Migrate](/azure/migrate/migrate-services-overview)
- [Azure Migrate 教學課程 - 將實體或虛擬化伺服器遷移至 Azure](/azure/migrate/tutorial-migrate-physical-virtual-machines)

### <a name="azure-database-migration-service"></a>Azure 資料庫移轉服務

Azure 資料庫移轉服務是一個完全受控的服務，可讓您從多個資料庫來源無縫移轉到 Azure 資料平台，將停機時間降到最低 (線上移轉)。 資料庫移轉服務會執行所有必要步驟。 您可以確信程序會使用 Microsoft 所建議的最佳做法，而安心起始移轉專案。

#### <a name="create-an-azure-database-migration-service-instance"></a>建立 Azure 資料庫移轉服務執行個體

如果這是您第一次使用 Azure 資料庫移轉服務，您必須為 Azure 訂用帳戶註冊資源提供者：

1. 依序選取 [所有服務] > [訂用帳戶]，然後選擇目標訂用帳戶。
1. 選取 [資源提供者]。
1. 搜尋 `migration`，然後在 [Microsoft.DataMigration] 右側選取 [註冊]。

::: zone target="chromeless"

::: form action="OpenBlade[#blade/Microsoft_Azure_Billing/SubscriptionsBlade]" submitText="Go to Subscriptions" :::

::: zone-end

註冊資源提供者之後，您可以建立 Azure 資料庫移轉服務的執行個體。

1. 選取 [+ 建立資源]，然後在 Marketplace 中搜尋 [Azure 資料庫移轉服務]。
1. 完成 [建立移轉服務] 精靈，然後選取 [建立]。

服務現已準備就緒，可以將支援的來源資料庫遷移至目標平台，例如 SQL Server、MySQL、PostgreSQL 或 MongoDB。

::: zone target="chromeless"

::: form action="Create[#create/Microsoft.AzureDMS]" submitText="Create an Azure Database Migration Service instance" :::

::: zone-end

::: zone target="docs"

如需詳細資訊，請參閱

- [Azure 資料庫移轉服務概觀](/azure/dms/dms-overview)
- [建立 Azure 資料庫移轉服務的執行個體](/azure/dms/quickstart-create-data-migration-service-portal)
- [Azure 入口網站中的 Azure Migrate](https://portal.azure.com/#blade/Microsoft_Azure_ManagementGroups/HierarchyBlade)
- [Azure 入口網站：建立移轉專案](https://portal.azure.com/#create/Microsoft.AzureMigrate)

::: zone-end

### <a name="azure-app-service-migration-assistant"></a>Azure App Service Migration Assistant

Azure App Service Migration Assistant 是 [大型應用程式套件的一部分](https://azure.microsoft.com/services/azure-migrate/)，可協助組織將其轉換至雲端。 Migration Assistant 提供引導式、類似精靈的使用者體驗，其會執行兩個工作：

1. 其會在 Web 應用程式上執行預先移轉相容性檢查，以評估安裝在 Windows Server 上的特定 Web 應用程式，來判斷是否可以在不修改 Web 應用程式的情況下，進行 Azure App Service 移轉。
1. 如果評定後確定可移轉 Web 應用程式，Migration Assistant 就會執行移轉。 您必須向 Migration Assistant 提供 Azure 帳戶存取權、選取要使用的資源群組、選取 Web 應用程式的名稱，以及其他詳細資料。

或者，Migration Assistant 會產生一個 Azure Resource Manager 範本，您可以用其來以更自動化且可重複的方式來遷移 Web 應用程式。

#### <a name="migrate-a-web-app-to-azure-app-service"></a>將 Web 應用程式遷移至 Azure App Service

Migration Assistant 會從收集 Azure 帳戶的重要詳細資料來展開移轉程序，然後執行移轉。

首先，您要登入 Azure 帳戶，並使用唯一的程式碼來建立 Migration Assistant 工作階段與帳戶的關聯。 接下來，您要選擇訂用帳戶、資源群組和網站的網域名稱。 您可以選擇建立新的 Azure App Service 方案來裝載應用程式，或選取現有的方案。 這項選擇將影響裝載應用程式的地理區域。 您也會有機會建立此移轉工作與現有 Azure Migrate 專案的關聯。 最後，您可以選擇略過資料庫設定，或選擇設定混合式連接來啟用資料庫連接。

在 Migration Assistant 收集並驗證您的選取專案之後，其會在選取的區域和資源群組中建立所需的 Azure App Service 資源。 其會壓縮 Web 應用程式的來源檔案，並使用 Azure App Service 部署 API 來部署。 最後，其會執行選用的移轉步驟，例如協助設定混合式連線。

成功移轉之後，您即必須執行所有移轉後工作。 這些情況可能包括：

- 手動將 web.config 檔案中的應用程式設定和連接字串移至 Azure App Service。
- 將資料從內部部署 SQL Server 執行個體移轉至 Azure SQL Database。
- 設定 SSL 憑證。
- 設定自訂網域名稱。
- 設定 Azure Active Directory 中的權限。

您也可以決定變更 Azure App Service 主控方案和其他設定，例如自動調整和部署位置。

如需詳細資訊，請參閱：

[將 ASP.NET 應用程式移轉至 Azure](/learn/paths/migrate-dotnet-apps-azure)

### <a name="data-migration-assistant"></a>Data Migration Assistant

Data Migration Assistant (DMA) 可藉由偵測可能對您新版 SQL Server or Azure SQL Database 中的資料庫功能造成影響的相容性問題，來協助您升級至新式資料平台。 DMA 可為您的目標環境提供效能和可靠性改善的建議，並可讓您將結構描述、資料和非內含的物件從來源伺服器移至目標伺服器。

Data Migration Assistant 已與 Azure Migrate 整合，可讓您在 Azure Migrate 儀表板中追蹤所有評估進度。 藉由新增 Azure Migrate，從 Azure Migrate 啟動 DMA：資料庫評估工具，並選取 DMA 中的 [上傳至 Azure Migrate] 按鈕，將您的資料庫評量新增至 Azure Migrate。

> [!NOTE]
> 針對大型移轉 (就資料庫的數量和大小而言)，我們建議使用 Azure 資料庫移轉服務，此服務可大規模移轉資料庫。
>

請使用下列步驟開始使用 Data Migration Assistant：

1. 從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=53595)下載並安裝 Data Migration Assistant。
1. 選取 [新增 (+)] 圖示，然後選取 [評估] 專案類型。
1. 設定來源和目標伺服器類型，然後選取 [建立]。
1. 視需要設定評估選項 (建議使用所有預設值)。
1. 新增要評估的資料庫。
1. 選取 [下一步] 開始進行評估。
1. 檢視 Data Migration Assistant 的結果。

對於企業，我們建議依照[使用 DMA 評估企業及整合評估報告](/sql/dma/dma-consolidatereports)中所述的方法來評估多個伺服器、結合報告，然後使用提供的 Power BI 報告來分析結果。

如需詳細資訊，包括詳細的使用步驟，請參閱：

- [Data Migration Assistant 概觀](/sql/dma/dma-overview)
- [使用 DMA 評估企業及整合評估報告](/sql/dma/dma-consolidatereports)
- [使用 Power BI 分析由 Data Migration Assistant 建立的整合評估報告](/sql/dma/dma-powerbiassesreport)

### <a name="sql-server-migration-assistant"></a>SQL Server 移轉小幫手

Microsoft SQL Server 移轉小幫手 (SSMA) 工具的設計目的，是要自動地將資料庫從 Microsoft Access、DB2、MySQL、Oracle 和 SAP ASE 移轉至 SQL Server。 一般概念是使用這些工具進行收集、評估，然後再進行檢閱，但由於每個來源系統的程序有所差異，建議您查看詳細的 [SQL Server 移轉小幫手](/sql/ssma/sql-server-migration-assistant) 文件。

如需詳細資訊，請參閱

- [SQL Server 移轉小幫手概觀](/sql/ssma/sql-server-migration-assistant)

### <a name="database-experimentation-assistant"></a>資料庫測試助理

資料庫測試助理 (DEA) 是適用於 SQL Server 升級的新款 A/B 測試解決方案。 它可協助您針對指定的工作負載評估 SQL 的目標版本。 從舊版 SQL Server (SQL Server 2005 和更新版本) 升級至任何新版 SQL Server 的客戶，可以使用這些分析計量。

資料庫測試助理包含下列工作流程活動：

- **擷取：** SQL Server a/B 測試的第一個步驟是在來源伺服器上擷取追蹤。 來源伺服器通常是生產伺服器。
- **重新執行：** SQL Server a/B 測試的第二個步驟是重新執行擷取到目標伺服器的追蹤檔案。 然後，從重新執行收集大量追蹤，以進行分析。
- **分析：** 最後一個步驟是使用重新執行追蹤來產生分析報告。 分析報告可協助您深入了解建議變更的效能影響。

如需詳細資訊，請參閱

- [資料庫測試助理概觀](/sql/dea/database-experimentation-assistant-overview)

### <a name="azure-cosmos-db-data-migration-tool"></a>Azure Cosmos DB 資料移轉工具

Azure Cosmos DB 資料移轉工具，將資料從各種來源匯入到 Azure Cosmos DB 集合和資料表。 您可以從 JSON 檔案、CSV 檔案、SQL、MongoDB、Azure 資料表儲存體、Amazon DynamoDB 及 Azure Cosmos DB SQL API 集合匯入資料。 針對 SQL API 從單一分割區集合移轉到多重分割區集合時，也可以使用資料移轉工具。

如需詳細資訊，請參閱

- [Azure Cosmos DB 資料移轉工具](/azure/cosmos-db/import-data)

## <a name="third-party-migration-tools"></a>[第三方移轉工具](#tab/third-party-tools)

有數個第三方移轉工具和 ISV 服務可協助您進行移轉程序。 每項工具各有不同的優點和功能。 這些工具包括：

### <a name="unifycloud"></a>UnifyCloud

UnifyCloud 是一種 ISV 服務，可提供評量、移轉和現代化的自動化工具。

[深入了解](https://www.unifycloud.com)

### <a name="cloudamize"></a>Cloudamize

Cloudamize 是一項 ISV 服務，可用於移轉策略的所有階段。

[深入了解](https://www.cloudamize.com)

### <a name="zerto"></a>Zerto

Zerto 提供可處理 Microsoft Hyper-v 和 VMware vSphere 環境的虛擬複寫功能。

[深入了解](https://www.zerto.com/modernize)

### <a name="carbonite"></a>Carbonite

Carbonite 提供伺服器和資料移轉解決方案，可在任何實體、虛擬或雲端式環境之間互相移轉工作負載。

[深入了解](https://www.carbonite.com/data-protection/data-migration-software)

### <a name="movere"></a>Movere

Movere 是一項探索解決方案，可提供規劃雲端移轉所需的資料和深入解析，並讓您安心地持續最佳化、監視和分析 IT 環境。

[深入了解](https://www.movere.io)

### <a name="azure-cosmos-db-partners"></a>Azure Cosmos DB 合作夥伴

您可從各式各樣資深的系統整合者合作夥伴和工具中選擇，以針對 NoSQL 資料庫需求支援 Azure Cosmos DB 移轉。

[深入了解](/azure/cosmos-db/partners-migration-cosmosdb#migration-tools)

請造訪 [Azure 移轉中心](https://azure.microsoft.com/migration/support)，查看有哪些組織提供現成可用的合作夥伴技術解決方案以因應您的移轉案例，並深入了解其他第三方移轉工具和支援服務。

造訪 [Azure Database 移轉指南](https://datamigration.microsoft.com)，了解原生和合作夥伴的一系列資料庫移轉選項及逐步指引。

## <a name="project-management-tools"></a>[專案管理工具](#tab/project-management-tools)

不受追蹤和管理的專案較可能會發生問題。 為確保能有成功的結果，我們認為您必須使用專案管理工具。 目前有許多不同的工具可供使用，而您組織中的專案管理人員可能已有偏好的工具。

Azure DevOps 是在雲端移轉期間建議用來管理專案的工具。 為了加速使用 Azure DevOps，雲端採用架構會包含自動部署專案範本的工具。 該範本包括在移轉過程中經常執行的各項工作。 使用 [雲端採用方案和 Azure DevOps](/azure/architecture/cloud-adoption/plan/template)中的指示來部署範本。 您可以修改範本以反映要遷移的[工作負載](/azure/architecture/cloud-adoption/plan/workloads)和[資產](/azure/architecture/cloud-adoption/plan/assets)。

Microsoft 也提供下列各種專案管理工具，可搭配使用以提供更廣泛的功能：

- [Microsoft Planner](https://tasks.office.com)：以簡單的視覺化方式組織團隊合作。
- [Microsoft Project](https://products.office.com/project/project-and-portfolio-management-software)：專案組合管理、資源容量管理、財務管理、時程表和排程管理。
- [Microsoft Teams](https://products.office.com/microsoft-teams)：小組共同作業與通訊工具。 Teams 也整合了 Planner 和其他工具來改善共同作業。
- [Azure DevOps Services](/azure/devops/user-guide/what-is-azure-devops?view=azure-devops)：使用 Azure DevOps 不需要雲端採用架構規劃範本。 您可以使用沒有範本的服務以程式碼管理基礎結構，或使用工作項目和面板來執行專案管理。 日趨成熟後，您的組織可以開始運用 CI/CD 功能。

這些專案管理工具並不是唯一可用的工具。 另有許多第三方工具也廣泛用於專案管理社群中。

### <a name="set-up-for-devops"></a>設定 DevOps

當您遷移至雲端技術時，這將是您為組織設定 DevOps 和 CI/CD 的絕佳機會。 即使您的組織僅管理基礎結構，當您開始以程式碼管理基礎結構，並使用 DevOps 的業界模式和實務時，您將可透過 CI/CD 管線開始提高靈活性，因而能夠更快速地因應變更、成長、發行，甚或復原案例。

Azure DevOps 提供必要的功能，並且可整合 Azure、內部部署環境甚或其他雲端。 如需詳細資訊，請參閱 [Azure DevOps](https://azure.microsoft.com/services/devops)。 如需引導式訓練，請參閱[快速入門：使用 Azure DevOps 的 CI/CD](https://microsoft.github.io/PartsUnlimited/pandp/200.1x-PandP-CICDQuickstartwithVSTS.html)。

### <a name="suggested-skills"></a>建議的技能

Microsoft Learn 是新的學習方法。 針對雲端採用所帶來的新技術責任做好準備並不容易。 Microsoft Learn 提供了更有價值的學習方法，可協助您更快達成目標。 獲得學分和等級，並達成更多目標！

以下是在 Microsoft Learn 上量身打造的學習路徑範例，可補充雲端採用架構中 DevOps 指引設定的不足之處。

[透過 Azure DevOps 建置應用程式](/learn/paths/build-applications-with-azure-devops)：與他人共用作業，使用 Azure Pipelines 和 GitHub 建置應用程式。 在管線中執行自動化測試，驗證程式碼品質。 掃描來源程式碼和第三方元件，找出可能的弱點。 定義用來建置應用程式的多個管線。 使用 Microsoft 裝載的代理程式和您自己建置的代理程式一同建置應用程式。

## <a name="cost-management"></a>[成本管理](#tab/ManageCost)

當您將資源移轉至雲端環境時，請務必定期執行成本分析。 定期成本分析有助於您避免產生非預期的使用費用，因為移轉程序對您的服務可能會有額外的使用需求。 您也可以視需要調整資源的大小，以平衡成本和工作負載，這在[最佳化和轉換](./optimize-and-transform.md)一節中會詳細討論。
