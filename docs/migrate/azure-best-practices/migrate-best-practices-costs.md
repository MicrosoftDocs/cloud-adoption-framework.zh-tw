---
title: 遷移至 Azure 之成本和大小工作負載的最佳做法
description: 使用適用于 Azure 的雲端採用架構，以瞭解將工作負載遷移至 Azure 的成本和大小的最佳作法。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: b22f93d4251f03c87ecdeb6dbef8ea932f48476f
ms.sourcegitcommit: 580a6f66a0d0f3f5b755c68d757a84b2351a432f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/31/2020
ms.locfileid: "87473245"
---
<!-- docsTest:ignore ARO -->

# <a name="best-practices-to-cost-and-size-workloads-migrated-to-azure"></a>遷移至 Azure 之成本和大小工作負載的最佳做法

當您針對移轉進行規劃及設計時，將焦點放在成本可確保您 Azure 移轉的長期成功。 在移轉專案執行期間，所有團隊 (財務、管理和應用程式開發小組等) 都必須了解相關的成本。

- 在遷移之前，請務必擁有每月、每季和每年預算目標的基準，以便估計您在遷移時所花費的金額，並確保其成功。
- 在移轉之後，您應該最佳化成本、持續監視工作負載，並針對未來的使用模式進行規劃。 已移轉的資源一開始可能是某種類型的工作負載，但會根據使用狀況、成本與變動的商業需求隨著時間轉換為另一種類型。

本文說明在遷移之前和之後，準備和管理成本和大小的最佳作法。

> [!IMPORTANT]
> 此文章所述的最佳做法與意見是以此文章撰寫當時可用的 Azure 平台與服務為基礎。 特色與功能會隨著時間改變。 這些建議不一定全都適用於您的部署，因此請選取適合您部署的建議。

## <a name="before-migration"></a>移轉前

將您的工作負載移轉到雲端之前，請預估在 Azure 中執行它們的每月成本。 主動管理雲端成本可協助您遵守您的運營費用預算。 若預算有限制，移轉之前請將此問題納入考慮。 在可能的情況下，考慮將供作負載轉換為 Azure 無伺服器技術以降低成本。

本節中的最佳作法可協助您：

- 預估成本。
- 執行虛擬機器（Vm）和儲存體的適當大小調整。
- 使用 Azure Hybrid Benefit。
- 使用 Azure 保留的虛擬機器執行個體。
- 跨訂用帳戶估計雲端費用。

## <a name="best-practice-estimate-monthly-workload-costs"></a>最佳做法：預估每月工作負載成本

若要預測您的已移轉工作負載每月帳單，您有一些工具可以使用。

<!-- TODO: Change "input costs" -->

- **Azure 定價計算機：** 選取您想要預估的產品，例如 Vm 和儲存體。 然後，將成本輸入至計算機以建立預估。

  ![Azure 定價計算機的螢幕擷取畫面。](./media/migrate-best-practices-costs/pricing.png)
    _圖1： Azure 定價計算機。_

- **Azure Migrate：** 若要預估成本，您必須審查並考慮在 Azure 中執行工作負載所需的所有資源。 若要取得此資料，您可以建立您的資產清查，包括伺服器、VM、資料庫與儲存體。 您可以使用 Azure Migrate 來收集此資訊。

  - Azure Migrate 會探索並評量您的內部部署環境以提供清查。
  - Azure Migrate 可以對應並顯示 Vm 之間的相依性，讓您有完整的圖片。
  - Azure Migrate 評量包含預估的成本。
    - **計算成本：** 當您建立評量時，使用建議的 Azure VM 大小，Azure Migrate 會使用 Azure 計費 API 來計算預估的每月 VM 成本。 估計會考慮作業系統、軟體保證、Azure 保留的虛擬機器執行個體、VM 執行時間、位置和貨幣設定。 它會在評量中彙總所有 VM 的成本，並計算每月總計算成本。
    - **儲存成本：** Azure Migrate 會匯總評量中所有 Vm 的儲存體成本，以計算每月儲存成本總計。 您可以透過彙總連結到特定機器之所有磁碟的每月成本，以計算該機器的每月儲存體成本。

    ![[圖 2] Azure Migrate 的螢幕擷取畫面 ](./media/migrate-best-practices-costs/assess.png)
     _： Azure Migrate 評估。_

**瞭解更多資訊：**

- [使用](https://azure.microsoft.com/pricing/calculator) Azure 定價計算機。
- 閱讀[Azure Migrate 的總覽](https://docs.microsoft.com/azure/migrate/migrate-services-overview)。
- [了解](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation) Azure Migrate 評量。
- 深入瞭解[Azure 資料庫移轉服務](https://docs.microsoft.com/azure/dms/dms-overview)。

## <a name="best-practice-right-size-vms"></a>最佳做法：適當調整 Vm 大小

當您部署 Azure VM 以支援工作負載時，您可以選擇數個選項。 每個 VM 類型都有特定的功能與不同的 CPU、記憶體與磁碟組合。 Vm 的分組方式如下表所示：

| 類型 | 詳細資料 | 使用量 |
| --- | --- | --- |
| **一般用途** | 平衡的 CPU 對記憶體。 | 適用于測試和開發、小型至中型資料庫，以及低至中量的流量網頁伺服器。 |
| **計算最佳化** | CPU 與記憶體的比例高。 | 適用于中型流量 web 伺服器、網路設備、批次處理，以及應用程式伺服器。 |
| **記憶體優化** | 高記憶體對 CPU。 | 適用于關係資料庫、中型至大型快取，以及記憶體內部分析。 |
| **儲存體最佳化** | 高磁片輸送量和 i/o。 | 適用于海量資料和 SQL 和 NoSQL 資料庫。 |
| **GPU 最佳化** | 特製化 VM。 一或多個 GPU。 | 繁重圖形與視訊編輯。 |
| **高效能** | 最快、最強的 CPU。 具有選用高輸送量網路介面（RDMA）的 Vm。 | 關鍵高效能應用程式。 |

- 請務必了解這些 VM 之間的定價差異，以及長期預算影響。
- 每個類型中都有一些 VM 系列。
- 此外，當您選取某個系列中的 VM 時，您只能在該系列中相應增加或相應減少 VM。 例如， `DS2_v2` 實例可以相應增加至 `DS4_v2` ，但無法變更為不同數列的實例，例如 `F2s_v2` 實例。

**瞭解更多資訊：**

- 深入瞭解[VM 類型和大小](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)，以及將大小對應至類型。
- [VM 實例的方案大小](https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs)。
- 查看[虛構 Contoso 公司的範例評](https://docs.microsoft.com/azure/migrate/contoso-migration-assessment)量。

## <a name="best-practice-select-the-right-storage"></a>最佳做法：選取正確的儲存體

調整及管理內部部署儲存體 (SAN 或 NAS) 以及網路以支援它們的成本可能非常高，而且非常耗時。 檔案 (儲存體) 資料通常會移轉到is commonly migrated to 雲端，以協助解決作業與管理難題。 Microsoft 提供數個選項讓您將資料移動到 Azure，而且您需要制訂有關那些選項的決策。 為資料挑選正確的儲存體類型每個月可以為您的組織節省數萬塊錢。 以下是一些考慮：

- 不常存取且不是業務關鍵的資料，不需要放在成本最高的儲存體上。
- 相反地，重要的業務關鍵資料應該放在較高層級的儲存體選項中。
- 在進行移轉規劃期間，請建立資料清查並依重要性分類，以將它對應到最適當的儲存體。 考慮預算與成本，以及效能。 成本不一定是主要因素。 挑選成本最低的選項可能會讓工作負載暴露于效能和可用性風險。

### <a name="storage-data-types"></a>儲存體資料類型

Azure 提供數種儲存體資料類型。

| 資料類型 | 詳細資料 | 使用量 |
| --- | --- | --- |
| **Blob** | 已針對儲存大量非結構化物件（例如文字或二進位資料）進行優化。 <br><br> | 透過 HTTP/HTTPS 從任意位置存取資料。 <br><br> 針對串流與隨機存取案例使用。 例如，直接將影像與文件提供給瀏覽器、串流視訊與音訊，以及存放備份與災害復原資料。 |
| **檔案** | 透過 SMB 3.0 存取的受控檔案共用。 | 當移轉內部部署檔案共用時使用，並提供檔案資料的多個存取/連線。 |
| **磁碟** | 以分頁 Blob 為基礎。 <br><br> 磁片類型：標準（HDD 或 SSD）或 premium （SSD）。 <br><br> 磁片管理：非受控（您管理磁片設定和存放裝置）或受管理（您選取磁片類型，Azure 會為您管理磁片）。 | 使用適用于 Vm 的 premium 磁片。 使用受控磁碟來獲得簡單的管理與規模調整。 |
| **佇列** | 儲存並抓取透過已驗證的呼叫（HTTP 或 HTTPS）存取的大量訊息。 | 將應用程式元件與非同步訊息佇列連接。 |
| **資料表** | 存放資料表。 | 此資料類型是 Azure Cosmos DB 資料表 API 的一部分。 |

### <a name="access-tiers"></a>存取層級

Azure 儲存體提供不同的選項來存取區塊 blob 資料。 選取適當的存取層可確保您以最具成本效益的方式儲存區塊 Blob 資料。

| 存取層 | 詳細資料 | 使用量 |
| --- | --- | --- |
| **經常性** | 儲存成本高於非經常性存取。 較酷的存取費用低。 <br><br> 這是預設層。 | 適用于經常存取的使用中資料。 |
| **超酷** | 儲存成本較熱。 較熱的存取費用更高。 <br><br> 至少存放 30 天。 | 儲存短期。 資料可供使用，但不常存取。 |
| **封存** | 用於個別區塊 Blob。 <br><br> 最具成本效益的儲存體選項。 相較於經常性與非經常性存取層，資料存取成本比較高。 | 適用于可容忍數小時之抓取延遲的資料，而且將會保留在層中至少180天。 |

### <a name="storage-account-types"></a>儲存體帳戶類型

Azure 提供數種儲存體帳戶類型與效能層級。

| 帳戶類型 | 詳細資料 | 使用量 |
| --- | --- | --- |
| **一般用途 v2 標準** | 支援 blob （區塊、頁面和附加）、檔案、磁片、佇列和資料表。 <br><br> 支援經常性、非經常性和封存存取層。 支援區域冗余儲存體（ZRS）。 | 用於大部分案例與大部分類型的資料。 標準儲存體帳戶可以是 HDD 或 SSD 型。 |
| **一般用途 v2 premium** | 支援 Blob 儲存體資料 (分頁 Blob)。 支援經常性、非經常性和封存存取層。 支援 ZRS。 <br><br> 存放在 SSD 上。 | Microsoft 建議為所有 VM 使用。 |
| **一般用途 v1** | 不支援存取分層。 不支援 ZRS。 | 如果應用程式需要 Azure 傳統部署模型，請使用。 |
| **Blob** | 用於存取非結構化物件的特殊化儲存體帳戶。 僅提供區塊 blob 和附加 blob （不含檔案、佇列、資料表或磁片儲存體服務）。 提供與一般用途 v2 相同的持久性、可用性、擴充性和效能。 | 您無法將分頁 blob 儲存在這些帳戶中，因此無法儲存 VHD 檔案。 您可以將存取層設定為經常性或非經常性。 |

### <a name="storage-redundancy-options"></a>儲存體備援選項

儲存體帳戶可以使用不同類型的備援選項來獲得備援能力與高可用性。

| 類型 | 詳細資料 | 使用量 |
| --- | --- | --- |
| **本機備援儲存體 (LRS)** | 透過在單一儲存體單位中複寫到不同的容錯網域與更新網域，來在本地服務故障時維持可用性。 在一個資料中心存放您資料的多個複本。 在特定一年內提供至少99.999999999% （11個9）的物件持久性。 | 請考慮您的應用程式是否儲存可輕鬆重建的資料。 |
| **區域備援儲存體 (ZRS)** | 透過複寫到單一區域中的三個儲存體叢集，來維持在資料中心無法使用時的可用性。 每個儲存體叢集的實體位置都不同，而且都位於自己的可用性區域中。 藉由在多個資料中心或區域中保留多份資料複本，在特定一年內提供至少99.9999999999 百分比（12個9）的物件持久性。 | 當您需要一致性、持久性與高可用性時可以考 慮使用。 當有多個區域永久受影響時，可能無法防止區域性災難。 |
| **異地備援儲存體 (GRS)** | 將資料複寫到與主要區域相距數百英里的次要地區，以保護整個區域中斷的情況。 在特定年份中，至少提供物件的99.99999999999999 百分比（16個9）持久性。 | 除非 Microsoft 起始容錯移轉到次區域的作業，否則複本資料無法使用。 若發生容錯移轉，讀取與寫入存取可用。 |
| **讀取權限異地備援儲存體 (RA-GRS)** | 類似於 GRS。 在特定年份中，至少提供物件的99.99999999999999 百分比（16個9）持久性。 | 允許從用於 GRS 的第二個區域讀取存取，以提供99.99% 的讀取可用性。 |

**瞭解更多資訊：**

- 查看[Azure 儲存體定價](https://azure.microsoft.com/pricing/details/storage)。
- 瞭解[Azure 匯入/匯出](https://docs.microsoft.com/azure/storage/common/storage-import-export-service)。
- 比較[blob、檔案和磁片儲存體資料類型](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks)。
- 深入瞭解[存取層](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers)。
- 查看[不同類型的儲存體帳戶](https://docs.microsoft.com/azure/storage/common/storage-account-overview)。
- 深入瞭解[Azure 儲存體的冗余](https://docs.microsoft.com/azure/storage/common/storage-redundancy)，包括 LRS、ZRS、GRS 和讀取權限 GRS。
- 深入瞭解[Azure 檔案儲存體](https://docs.microsoft.com/azure/storage/files/storage-files-introduction)。

## <a name="best-practice-take-advantage-of-azure-hybrid-benefit"></a>最佳做法：利用 Azure Hybrid Benefit

整合內部部署 Microsoft 軟體與 Azure 的組合，可為您提供競爭和成本優勢。 如果您目前有透過軟體保證的作業系統或其他軟體授權，您可以利用 Azure Hybrid Benefit，將這些授權帶到雲端。

**瞭解更多資訊：**

- [請看一下](https://azure.microsoft.com/pricing/hybrid-benefit)Azure Hybrid Benefit 節約計算機。
- 深入瞭解[適用于 Windows Server 的 Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit)。
- 請參閱[SQL Server Azure vm 的定價指導](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance)方針。

## <a name="best-practice-use-reserved-vm-instances"></a>最佳做法：使用保留的 VM 實例

大部分的雲端平臺會使用隨用隨付付款模型。 此模型帶來缺點，因為您不一定知道工作負載的動態。 當您為工作負載指定清楚的意圖時，您就為基礎結構規劃做出貢獻。

當您使用 Azure 保留的 VM 執行個體時，您會預付一年或三年期的 VM 實例。

- 預付提供所使用資源的折扣。
- 相較于隨用隨付價格，您可以大幅減少 VM、Azure SQL Database 計算、Azure Cosmos DB 或其他資源成本。
- 保留會提供計費折扣，且不會影響資源的執行階段狀態。
- 您可以取消保留執行個體。

![隨用隨付和 Azure Hybrid Benefit 與保留實例比較的螢幕擷取畫面。 ](./media/migrate-best-practices-costs/reserve.png)
_圖3： Azure 保留的 VM 執行個體。_

**瞭解更多資訊：**

- 瞭解[Azure 保留](https://docs.microsoft.com/azure/cost-management-billing/reservations/save-compute-costs-reservations)專案。
- 閱讀[AZURE 保留的 VM 執行個體常見問題](https://azure.microsoft.com/pricing/reserved-vm-instances/#faq)。
- 請參閱[Azure vm 上 SQL Server 的定價指導](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance)方針。

## <a name="best-practice-aggregate-cloud-spending-across-subscriptions"></a>最佳做法：跨訂用帳戶匯總雲端費用

最後，您可能會有一個以上的 Azure 訂用帳戶。 例如，您可能需要額外的訂用帳戶來將開發與生產界限分開，或者您的平台可能需要為每個用戶端使用不同的訂用帳戶。 因此，跨所有訂用帳戶將資料報告彙總到單一平台的功能非常有價值。

若要這樣做，您可以使用 Azure 成本管理和計費 API。 然後，將資料匯總到單一來源（例如 Azure SQL）之後，您就可以使用 Power BI 之類的工具來呈現匯總的資料。 您可以建立彙總的訂用帳戶報告，以及精細的報告。 例如，對於需要主動深入解析成本管理的使用者，您可以根據部門、資源群組或其他資訊來建立特定的成本觀點。 您不需要將完整的 Azure 帳單資料存取權提供給他們。

**瞭解更多資訊：**

- 閱讀[Azure 使用量 api 總覽](https://docs.microsoft.com/azure/billing/billing-consumption-api-overview)。
- 瞭解如何[連接到 Power BI Desktop 中的 Azure 使用量見解](https://docs.microsoft.com/power-bi/desktop-connect-azure-consumption-insights)。
- 瞭解如何[使用角色型存取控制（RBAC）來管理對 Azure 帳單資訊的存取](https://docs.microsoft.com/azure/billing/billing-manage-access)。

## <a name="after-migration"></a>移轉之後

成功遷移您的工作負載以及收集耗用量資料的幾周之後，您就能清楚瞭解資源的成本。 當分析資料時，您可以開始為 Azure 資源群組與資源產生預算基準。 接著，當您了解您的雲端預算花在哪裡之後，您可以分析如何進一步降低您的成本。

## <a name="best-practice-use-azure-cost-management-and-billing"></a>最佳做法：使用 Azure 成本管理和計費

Microsoft 提供 Azure 成本管理和計費，協助您追蹤費用。 此服務能夠：

- 協助您監視及控制 Azure 費用，並將資源的使用最佳化。
- 檢閱您的整個訂用帳戶與其所有資源，並產生建議。
- 提供您完整的 API 來整合外部工具和財務系統以進行報告。
- 追蹤資源使用量，並協助您利用單一、統一的觀點來管理雲端成本。
- 提供豐富的營運與財務見解以協助您做出明智的決策。

透過 Azure 成本管理和計費，您可以：

- 建立財務責任的預算。
  - 您可以針對特定期間（每月、每季或每年）和範圍（訂用帳戶或資源群組），來考慮您取用或訂閱的服務。 例如，您可以針對每月、每季或每年期間建立 Azure 訂用帳戶預算。
    - 建立預算之後，它會顯示在成本分析中。 當您要分析成本和費用時，針對目前的費用來查看您的預算是很重要的。
  - 當達到預算閾值時，您可以選擇傳送電子郵件通知。
  - 您可以將成本管理資料匯出至 Azure 儲存體，以供分析。

  ![成本管理預算的螢幕擷取畫面。 ](./media/migrate-best-practices-costs/budget.png)
  _圖4： Azure 成本管理和計費預算。_

- 進行成本分析來探索及分析組織成本，以協助您瞭解成本的累算方式，並找出支出趨勢。
  - Enterprise 合約的使用者可以使用成本分析。
  - 您可以查看各種範圍的成本分析資料，包括依部門、帳戶、訂用帳戶或資源群組。
  - 您可以取得顯示當月份總成本的成本分析，以及累計每日成本。

  ![Azure 成本管理分析圖5的螢幕擷取畫面 ](./media/migrate-best-practices-costs/analysis.png)
   _： Azure 成本管理和計費分析。_

- 取得 Advisor 建議，其中顯示如何最佳化及改進效率。

**瞭解更多資訊：**

- 閱讀[Azure 成本管理和計費總覽](https://docs.microsoft.com/azure/cost-management-billing/cost-management-billing-overview)。
- 瞭解如何透過[Azure 成本管理和帳單來優化您的雲端投資](https://docs.microsoft.com/azure/cost-management-billing/costs/cost-mgt-best-practices)。
- 深入瞭解[Azure 成本管理和帳單報告](https://docs.microsoft.com/azure/cost-management/use-reports)。
- 取得[從建議中優化成本的教學](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-acm-opt-recommendations)課程。
- 請參閱[Azure 使用量 api](https://docs.microsoft.com/rest/api/consumption/budgets)。

## <a name="best-practice-monitor-resource-utilization"></a>最佳做法：監視資源使用率

在 Azure 中，您只需要針對您使用的資源付費，而不需要針對未使用資源的情況付費。 針對 VM，帳單會在配置 VM 時產生，而且解除配置 VM 之後，您就不會再收到帳單。 記住這一點之後，您應該監視使用中的 Vm，並確認 VM 大小。

持續評估您的 VM 工作負載以決定基準。 例如，如果您的工作負載會在星期一到星期五、上午8點到下午6點使用，但在這些小時外幾乎不會使用，您可以在尖峰時間外降級 Vm。 這可能表示變更 VM 大小，或使用虛擬機器擴展集來自動相應增加或相應減少 VM。 有些公司將 Vm 「延遲」，方法是將它們放在指定何時可以使用的行事曆，以及不需要時。

您可以使用 Microsoft 工具（例如 Azure 成本管理和計費、Azure 監視器和 Azure Advisor）來監視 VM 使用量。 也有第三方工具可用。

> [!NOTE]
> 除了 VM 監視之外，您還應該監視其他網路資源，例如 Azure ExpressRoute 和虛擬網路閘道，以供 underuse 和濫用。

**瞭解更多資訊：**

- 閱讀[Azure 監視器](https://docs.microsoft.com/azure/azure-monitor/overview)和[Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-overview)的總覽。
- [取得 Azure Advisor 的成本建議](https://docs.microsoft.com/azure/advisor/advisor-cost-recommendations)。
- 瞭解如何[從建議中將成本優化](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-acm-opt-recommendations)，並[避免非預期的費用](https://docs.microsoft.com/azure/billing/billing-getting-started)。
- 瞭解[Azure 資源優化（ARO）工具](https://github.com/azure/azure-quickstart-templates/tree/master/azure-resource-optimization-toolkit)組。

## <a name="best-practice-implement-resource-group-budgets"></a>最佳做法：執行資源群組預算

通常，您可能會發現使用資源群組來呈現成本界限很有用。 資源群組預算可協助您追蹤與資源群組關聯的成本。 當您達到或超過預算時，您可以觸發警示並執行各種不同的腳本。

**瞭解更多資訊：**

- 瞭解如何[使用 Azure 預算來管理成本](https://docs.microsoft.com/azure/billing/billing-cost-management-budget-scenario)。
- 請參閱[建立和管理 Azure 預算](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-acm-create-budgets)的教學課程。

## <a name="best-practice-optimize-azure-monitor-retention"></a>最佳做法：優化 Azure 監視器保留

您可以將資源移到 Azure 中並為資源啟用診斷記錄，您可以產生大量的記錄資料。 一般來說，此記錄資料會傳送至對應至 Log Analytics 工作區的儲存體帳戶。 以下是優化 Azure 監視器保留的幾個秘訣：

- 資料保留期間越長，您有的資料就越多。
- 並非所有記錄資料都相等，某些資源將會產生比其他資源更多的資料。
- 由於法規和合規性的緣故，您可能需要保留某些資源的記錄資料，而不是其他資源。
- 在最佳化您的記錄儲存體成本與保存所需的記錄資料之間應該仔細評估。
- 我們建議您在完成遷移之後立即評估和設定記錄，這樣您就不需要支付任何重要記錄的費用。

**瞭解更多資訊：**

- 瞭解[監視使用量和估計成本](https://docs.microsoft.com/azure/azure-monitor/platform/usage-estimated-costs)。

## <a name="best-practice-optimize-storage"></a>最佳做法：優化儲存體

如果您遵循在遷移前選取儲存體的最佳做法，您可能會享受某些優點。 但是，您仍然可以優化額外的儲存成本。 經過一段時間後，blob 和檔案會變得過時。 資料可能無法再使用，但法規需求可能表示您必須將它保存一段時間。 因此，您可能不需要將它存放在用於原始移轉的高效能儲存體上。

找出過時資料並移動到較便宜的儲存體區域對您的每月儲存體預算與成本節省會有大幅影響。 Azure 提供許多方式來協助您找出並存放此類過時資料。

- 利用一般用途 v2 儲存體的存取層，將較不重要的資料從經常性移至非經常性存取和封存層。
- 使用 StorSimple 協助移動以自訂原則為基礎的過時資料。

**瞭解更多資訊：**

- 深入瞭解[存取層](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers)。
- 閱讀[StorSimple 總覽](https://docs.microsoft.com/azure/azure-monitor/overview)。
- 請參閱[StorSimple 定價](https://azure.microsoft.com/pricing/details/storsimple)。

## <a name="best-practice-automate-vm-optimization"></a>最佳做法：自動化 VM 優化

在雲端中執行 VM 的終極目標是最佳化它所使用的 CPU、記憶體與磁碟。 如果您發現未優化的 Vm，或未使用 Vm 的頻率很長，請使用虛擬機器擴展集將它們關閉或縮小它們是合理的。

您可以使用 Azure 自動化、虛擬機器擴展集、自動關機與指令碼或第三方解決方案來最佳化 VM。

**瞭解更多資訊：**

- 瞭解[垂直](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-vertical-scale-reprovision)自動調整。
- [排程 VM 自動啟動](https://azure.microsoft.com/updates/azure-devtest-labs-schedule-vm-auto-start)。
- 瞭解如何[在 Azure 自動化中啟動或停止 vm 數小時](https://docs.microsoft.com/azure/automation/automation-solution-vm-management)。
- 取得[Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-overview)的詳細資訊，以及[Azure 資源優化（ARO）工具](https://github.com/azure/azure-quickstart-templates/tree/master/azure-resource-optimization-toolkit)組。

## <a name="best-practices-use-azure-logic-apps-and-runbooks-with-budgets-api"></a>最佳做法：使用 Azure Logic Apps 和 runbook 搭配預算 API

Azure 提供 REST API，它可以存取您的租用戶帳單資訊。 您可以使用預算 API 來整合外部系統與工作流程 (透過您從 API 資料建置的計量觸發)。 您可以將使用量與資源資料提取到您慣用的資料分析工具中。 您可以將預算 API 與 Azure Logic Apps 和 runbook 整合。

Azure 資源使用情況和 RateCard API 可協助您準確地預測並管理成本。 這些 Api 會實作為資源提供者，並包含在 Azure Resource Manager 所公開的 Api 中。

**瞭解更多資訊：**

- 請參閱[Azure 預算 API](https://docs.microsoft.com/rest/api/consumption/budgets)。
- 深入瞭解[Azure 計費 API](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview)的使用方式。

## <a name="best-practice-implement-serverless-technologies"></a>最佳做法：執行無伺服器技術

VM 工作負載通常會以「原樣」遷移，以避免停機。 通常，Vm 可以裝載斷斷續續的工作、在短時間內執行，或花費數小時的時間。 範例包括執行排定工作的 Vm，例如 Windows 工作排程器或 PowerShell 腳本。 當這些工作未執行時，您仍然必須吸收 VM 與磁碟儲存體成本。

在遷移和徹底檢查這些類型的工作之後，您可以考慮將它們遷移至無伺服器技術，例如 Azure Functions 或批次作業。 這些解決方案可以降低成本，而且您不再需要管理和維護 Vm。

**瞭解更多資訊：**

- 了解 [Azure Functions](https://azure.microsoft.com/services/functions)。
- 了解 [Azure Batch](https://azure.microsoft.com/services/batch)。

## <a name="next-steps"></a>後續步驟

檢閱其他最佳做法：

- 移轉後的安全性及管理[最佳做法](./migrate-best-practices-security-management.md)。
- 移轉後的網路功能[最佳做法](./migrate-best-practices-networking.md)。
