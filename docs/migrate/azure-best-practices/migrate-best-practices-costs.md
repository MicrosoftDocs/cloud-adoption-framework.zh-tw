---
title: 已遷移至 azure 的成本與大小調整工作負載
description: 使用適用于 Azure 的雲端採用架構，以瞭解將工作負載遷移至 Azure 的成本和大小的最佳作法。
author: BrianBlanchard
ms.author: brblanch
ms.date: 12/08/2018
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: adc74b83bf3da456e657d890a06ba6b1bcf516ed
ms.sourcegitcommit: 5d6a7610e556f7b8ca69960ba76a3adfa9203ded
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2020
ms.locfileid: "83401231"
---
<!-- docsTest:ignore ARO -->

# <a name="best-practices-for-costing-and-sizing-workloads-migrated-to-azure"></a>針對移轉到 Azure 之工作負載評估成本及調整大小的最佳做法

當您針對移轉進行規劃及設計時，將焦點放在成本可確保您 Azure 移轉的長期成功。 在移轉專案執行期間，所有團隊 (財務、管理和應用程式開發小組等) 都必須了解相關的成本。

- 在移轉之前，根據每月、每季與每年預算目標預估您的移轉費用是成功的重要要素。
- 在移轉之後，您應該最佳化成本、持續監視工作負載，並針對未來的使用模式進行規劃。 已移轉的資源一開始可能是某種類型的工作負載，但會根據使用狀況、成本與變動的商業需求隨著時間轉換為另一種類型。

此文章說明在移轉之前與之後評估成本並調整大小的最佳做法。

> [!IMPORTANT]
> 此文章所述的最佳做法與意見是以此文章撰寫當時可用的 Azure 平台與服務為基礎。 特色與功能會隨著時間改變。 這些建議不一定全都適用於您的部署，因此請選取適合您部署的建議。

## <a name="before-migration"></a>移轉前

將您的工作負載移轉到雲端之前，請預估在 Azure 中執行它們的每月成本。 主動管理雲端成本可協助您遵守您的運營費用預算。 若預算有限制，移轉之前請將此問題納入考慮。 在可能的情況下，考慮將供作負載轉換為 Azure 無伺服器技術以降低成本。

此節中的最佳做法可協助您預估成本、針對 VM 與儲存體執行適當的大小調整作業、運用 Azure Hybrid 優勢、使用保留的 VM，以及預估訂用帳戶的雲端費用。

## <a name="best-practice-estimate-monthly-workload-costs"></a>最佳做法：預估每月工作負載成本

若要預測您的已移轉工作負載每月帳單，您有一些工具可以使用。

<!-- TODO: Change "input costs" -->
- **Azure 定價計算機：** 選取您想要估計的產品，例如 Vm 和儲存體，然後將成本輸入至計算機以建立預估。

 ![Azure 定價計算機](./media/migrate-best-practices-costs/pricing.png) _Azure 定價計算機_

- **Azure Migrate：** 若要預估成本，您必須審查並考慮在 Azure 中執行工作負載所需的所有資源。 若要取得此資料，您可以建立您的資產清查，包括伺服器、VM、資料庫與儲存體。 您可以使用 Azure Migrate 來收集此資訊。

- Azure Migrate 會探索並評量您的內部部署環境以提供清查。
- Azure Migrate 可以對應 VM 之間的相依性並顯示此資訊，以便您可以對其有完整了解。
- Azure Migrate 評量包含預估的成本。
  - 計算成本：當您建立評量時，使用建議的 Azure VM 大小，Azure Migrate 會使用 Azure 計費 API 來計算預估的每月 VM 成本。 預估程序考慮了作業系統、軟體保證、保留執行個體、VM 運作時間、位置與貨幣設定。 它會在評量中彙總所有 VM 的成本，並計算每月總計算成本。
  - 儲存體成本： Azure Migrate 藉由匯總評量中所有 Vm 的儲存體成本，計算每月儲存成本總計。 您可以透過彙總連結到特定機器之所有磁碟的每月成本，以計算該機器的每月儲存體成本。

    ![Azure Migrate](./media/migrate-best-practices-costs/assess.png)
    _Azure Migrate 評量_

**瞭解更多資訊：**

- [使用](https://azure.microsoft.com/pricing/calculator) Azure 定價計算機。
- [取得 Azure Migrate 概觀](https://docs.microsoft.com/azure/migrate/migrate-services-overview)。
- [了解](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation) Azure Migrate 評量。
- [深入了解](https://docs.microsoft.com/azure/dms/dms-overview) Azure 資料庫移轉服務。

## <a name="best-practice-right-size-vms"></a>最佳做法：適當調整 Vm 大小

當您部署 Azure VM 以支援工作負載時，您可以選擇數個選項。 每個 VM 類型都有特定的功能與不同的 CPU、記憶體與磁碟組合。 VM 會依下列方式分組：

| **型別** | **詳細資料** | **使用** |
| --- | --- | --- |
| **一般用途** | 平衡的 CPU 對記憶體。 | 適用於測試和開發、小型至中型資料庫，低至中流量 Web 伺服器。 |
| **計算最佳化** | CPU 與記憶體的比例高。 | 適用於中流量 Web 伺服器、網路設備、批次處理、應用程式伺服器。 |
| **記憶體優化** | 高記憶體對 CPU。 | 適用於關聯式資料庫、中型到大型快取、記憶體內分析。 |
| **儲存體最佳化** | 高磁碟輸送量及 IO。 | 適用於巨量資料、SQL 與 NoSQL 資料庫。 |
| **GPU 最佳化** | 特製化 VM。 一或多個 GPU。 | 繁重圖形與視訊編輯。 |
| **高效能** | 最快、最強的 CPU。 具有選用高輸送量網路介面 (RDMA) 的 VM | 關鍵高效能應用程式。 |

- 請務必了解這些 VM 之間的定價差異，以及長期預算影響。
- 每個類型中都有一些 VM 系列。
- 此外，當您選取某個系列中的 VM 時，您只能在該系列中相應增加或相應減少 VM。 例如，DSv2\_2 可以相應增加到 DSv2\_4，但它無法變更為不同的系列，例如 Fsv2\_2。

**瞭解更多資訊：**

- [深入了解](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) VM 類型與大小調整，以及將大小對應到類型。
- [規劃](https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs) VM 大小調整。
- [檢閱](https://docs.microsoft.com/azure/migrate/contoso-migration-assessment) 虛構 Contoso 公司的範例評量。

## <a name="best-practice-select-the-right-storage"></a>最佳做法：選取正確的儲存體

調整及管理內部部署儲存體 (SAN 或 NAS) 以及網路以支援它們的成本可能非常高，而且非常耗時。 檔案 (儲存體) 資料通常會移轉到is commonly migrated to 雲端，以協助解決作業與管理難題。 Microsoft 提供數個選項讓您將資料移動到 Azure，而且您需要制訂有關那些選項的決策。 為資料挑選正確的儲存體類型每個月可以為您的組織節省數萬塊錢。 一些考量：

- 不常存取且不是業務關鍵的資料，不需要放在成本最高的儲存體上。
- 相反地，重要的業務關鍵資料應該放在較高層級的儲存體選項中。
- 在進行移轉規劃期間，請建立資料清查並依重要性分類，以將它對應到最適當的儲存體。 考慮預算與成本，以及效能。 成本不應該是主要決策制訂因素。 挑選最便宜的選項可能會讓工作負載面臨效能與可用性風險。

### <a name="storage-data-types"></a>儲存體資料類型

Azure 提供數種儲存體資料類型。

<!-- markdownlint-disable MD033 -->

| **資料類型** | **詳細資料** | **使用量** |
| --- | --- |  --- |
| **Blob** | 已針對儲存大量非結構化物件（例如文字或二進位資料）進行優化。 <br><br> | 透過 HTTP/HTTPS 從任意位置存取資料。 | 針對串流與隨機存取案例使用。 例如，直接將影像與文件提供給瀏覽器、串流視訊與音訊，以及存放備份與災害復原資料。 |
| **檔案** | 透過 SMB 3.0 存取受控檔案共用 | 當移轉內部部署檔案共用時使用，並提供檔案資料的多個存取/連線。 |
| **磁碟** | 以分頁 Blob 為基礎。 <br><br> 磁片類型（速度）：標準（HDD 或 SSD）或 Premium （SSD）。 <br><br> 磁片管理：非受控（您管理磁片設定和存放裝置）或受管理（您選取磁片類型，Azure 會為您管理磁片）。 | 為 VM 使用進階磁碟。 使用受控磁碟來獲得簡單的管理與規模調整。 |
| **佇列** | 存放及擷取大量訊息 (透過已驗 (HTTP 或 HTTPS) 存取) | 使用非同步訊息佇列連結應用程式元件。 |
| **資料表** | 存放資料表。 | 現在是 Azure Cosmos DB 資料表 API 的一部分。 |

<!--markdownlint-enable MD033 -->

### <a name="access-tiers"></a>存取層級

Azure 儲存體提供不同的選項，以存取區塊 Blob 資料。 選取適當的存取層可確保您以最具成本效益的方式儲存區塊 Blob 資料。

<!-- markdownlint-disable MD033 -->

| **型別** | **詳細資料** | **使用量** |
| --- | --- | --- |
| **經常性** | 相較於非經常性存取層有較高的儲存體成本。 相較於非經常性存取層有較低的存取費用。 <br><br> 這是預設層。 | 針對經常存取之作用中用途使用。 |
| **非經常性** | 相較於經常性存取層有較低的儲存體成本。 相較於經常性存取層有較高的存取費用。 <br><br> 至少存放 30 天。 | 短期存放，資料可用但不經常存取。 |
| **封存** | 用於個別區塊 Blob。 <br><br> 最具成本效益的儲存體選項。 相較於經常性與非經常性存取層，資料存取成本比較高。 | 用於可容許數小時擷取延遲且依然會存在於該層至少 180 天的資料。 |

<!--markdownlint-enable MD033 -->

### <a name="storage-account-types"></a>儲存體帳戶類型

Azure 提供數種儲存體帳戶類型與效能層級。

<!-- markdownlint-disable MD033 -->

| **帳戶類型** | **詳細資料** | **使用量** |
| --- | --- | --- |
| **一般用途 v2 標準** | 支援 Blob (區塊、分頁、附加)、檔案、磁碟、佇列與資料表。 <br><br> 支援經常性、非經常性與封存存取層。 支援 ZRS。 | 用於大部分案例與大部分類型的資料。 標準儲存體帳戶可以是 HDD 型或 SSD 型。 |
| **一般用途 v2 進階** | 支援 Blob 儲存體資料 (分頁 Blob)。 支援經常性、非經常性與封存存取層。 支援 ZRS。 <br><br> 存放在 SSD 上。 | Microsoft 建議為所有 VM 使用。 |
| **一般用途 v1** | 不支援存取分層。 不支援 ZRS | 在應用程式需要 Azure 傳統部署時使用。 |
| **Blob** | 用於存取非結構化物件的特殊化儲存體帳戶。 提供區塊 Blob 並只附加 Blob (不附加檔案、佇列、資料表或磁碟儲存體服務)。 提供與一般用途 v2 相同的持久性、可用性、延展性與效能。 | 您無法將分頁 blob 儲存在這些帳戶中，因此無法儲存 VHD 檔案。 您可以將存取層設定為經常性或非經常性。 |

<!--markdownlint-enable MD033 -->

### <a name="storage-redundancy-options"></a>儲存體備援選項

儲存體帳戶可以使用不同類型的備援選項來獲得備援能力與高可用性。

| **型別** | **詳細資料** | **使用量** |
| --- | --- | --- |
| **本機備援儲存體 (LRS)** | 透過在單一儲存體單位中複寫到不同的容錯網域與更新網域，來在本地服務故障時維持可用性。 在一個資料中心存放您資料的多個複本。 在指定的一年內提供至少99.999999999% （11個9）的物件持久性。 | 當您的應用程式存放可輕鬆重建的資料可以考慮使用。 |
| **區域冗余儲存體（ZRS）** | 透過複寫到單一區域中的三個儲存體叢集，來維持在資料中心無法使用時的可用性。 每個儲存體叢集的實體位置都不同，而且都位於自己的可用性區域中。 藉由在多個資料中心或區域中保留多個資料複本，在指定的一年內提供至少99.9999999999 百分比（12個9）的持久性。 | 當您需要一致性、持久性與高可用性時可以考 慮使用。 當多個區域都受到永久影響時，可能無法針對區域性災害提供保護。 |
| **異地備援儲存體 (GRS)** | 透過將資料複寫到與主區域距離數百公里遠的次區域，來在提供整個區域的服務都無法使用時的可用性。 在指定的一年內，至少提供物件的99.99999999999999 百分比（16個9）持久性。 | 除非 Microsoft 起始容錯移轉到次區域的作業，否則複本資料無法使用。 若發生容錯移轉，讀取與寫入存取可用。 |
| **讀取權限異地多餘儲存體（RA-GRS）** | 類似於 GRS。 在指定的一年內，至少提供物件的99.99999999999999 百分比（16個9）持久性。 | 允許從用於 GRS 的第二個區域讀取存取，以提供99.99% 的讀取可用性。 |

**瞭解更多資訊：**

- [檢閱](https://azure.microsoft.com/pricing/details/storage) Azure 儲存體定價。
- [了解](https://docs.microsoft.com/azure/storage/common/storage-import-export-service) 如何使用 Azure 匯入/匯出將大量資料移轉到 Azure Blob 與檔案。
- [比較](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks) Blob、檔案與磁碟儲存體資料類型。
- [深入了解](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers)存取層。
- [檢閱](https://docs.microsoft.com/azure/storage/common/storage-account-overview)不同類型的儲存體帳戶。
- 瞭解[Azure 儲存體的冗余](https://docs.microsoft.com/azure/storage/common/storage-redundancy)，包括 LRS、ZRS、GRS 和讀取權限 GRS。
- 深入瞭解[Azure 檔案儲存體](https://docs.microsoft.com/azure/storage/files/storage-files-introduction)。

## <a name="best-practice-take-advantage-of-azure-hybrid-benefits"></a>最佳做法：利用 Azure 混合式權益

由於長期以來在 Windows Server 與 SQL Server 等系統的軟體投資，Microsoft 身處獨特位置，可為透過其他雲端提供者不見得提供的實質折扣，為客戶提供雲端中的價值。

整合式 Microsoft 內部部署/Azure 產品組合可提供競爭與成本優勢。 若您目前有透過軟體保證 (SA) 取得的作業系統或其他軟體授權，您可以透過 Azure Hybrid Benefit 在雲端使用那些授權。

**瞭解更多資訊：**

- [查看](https://azure.microsoft.com/pricing/hybrid-benefit) Hybrid Benefit 節費計算機。
- [深入了解](https://azure.microsoft.com/pricing/hybrid-benefit)適用於 Windows Server 的 Hybrid Benefit。
- [檢閱](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance) SQL Server Azure VM 的定價指導方針。

## <a name="best-practice-use-reserved-vm-instances"></a>最佳做法：使用保留的 VM 實例

大部分的雲端平台都是設定為隨用隨付。 此模型有缺點，因為您不見得知道動態工作負載為何。 當您為工作負載指定清楚的意圖時，您就為基礎結構規劃做出貢獻。

使用 Azure 保留的 VM 執行個體，您可以預付一年或三年期的 VM 實例。

- 預付提供所使用資源的折扣。
- 使用隨用隨付價格時，您可以大幅減少 VM、SQL 資料庫計算、Azure Cosmos DB 或其他資源成本，最高可節省 72%。
- 保留會提供計費折扣，且不會影響資源的執行階段狀態。
- 您可以取消保留執行個體。

![保留執行個體](./media/migrate-best-practices-costs/reserve.png)
_Azure 保留的 VM_

**瞭解更多資訊：**

- 瞭解[Azure 保留](https://docs.microsoft.com/azure/cost-management-billing/reservations/save-compute-costs-reservations)專案。
- 閱讀[保留實例常見問題](https://azure.microsoft.com/pricing/reserved-vm-instances/#faq)。
- 請參閱[適用于 SQL Server Azure vm 的定價指導](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance)方針。

## <a name="best-practice-aggregate-cloud-spend-across-subscriptions"></a>最佳做法：跨訂用帳戶匯總雲端支出

您終究會有多個 Azure 訂用帳戶。 例如，您可能需要額外的訂用帳戶來將開發與生產界限分開，或者您的平台可能需要為每個用戶端使用不同的訂用帳戶。 因此，跨所有訂用帳戶將資料報告彙總到單一平台的功能非常有價值。

若要這樣做，您可以使用 Azure 成本管理 API。 接著，將資料彙總到單一來源 (例如 Azure SQL) 之後，您可以使用諸如 Power BI 的工具來呈現彙總的資料。 您可以建立彙總的訂用帳戶報告，以及精細的報告。 例如，對於需要主動深入解析成本管理的使用者，您可以根據部門、資源群組或其他資訊來建立特定的成本觀點。 您不需要將完整的 Azure 帳單資料存取權提供給他們。

**瞭解更多資訊：**

- [取得 Azure 使用量 API 概觀](https://docs.microsoft.com/azure/billing/billing-consumption-api-overview)。
- [了解](https://docs.microsoft.com/power-bi/desktop-connect-azure-consumption-insights)如何在 Power BI Desktop 中連線到 Azure 使用量見解。
- [了解如何](https://docs.microsoft.com/azure/billing/billing-manage-access)使用角色型存取控制 (RBAC) 管理 Azure 帳單資訊存取權。

## <a name="after-migration"></a>移轉之後

成功移轉您的工作負載並收集使用量資料數週之後，您將會資源成本的概念有清楚的了解。

- 當分析資料時，您可以開始為 Azure 資源群組與資源產生預算基準。
- 接著，當您了解您的雲端預算花在哪裡之後，您可以分析如何進一步降低您的成本。

此節中的最佳做法包括使用 Azure 成本管理來進行成本預算編列及分析、監視資源及實作資源群組預算，以及最佳化監視、儲存體與 VM。

## <a name="best-practice-use-azure-cost-management"></a>最佳做法：使用 Azure 成本管理

Microsoft 提供 Azure 成本管理來協助您追蹤費用：

- 協助您監視及控制 Azure 費用，並將資源的使用最佳化。
- 檢閱您的整個訂用帳戶與其所有資源，並產生建議。
- 提供完整 API 以整合外部工具與財務系統來進行報告建立。
- 使用單一的整合式檢視來追蹤資源使用量並管理雲端成本。
- 提供豐富的營運與財務見解以協助您做出明智的決策。

在成本管理中，您可以：

- **建立預算：** 建立財務責任的預算。
  - 您可以納入您使用或訂閱的服務一段時間 (每月、每季、每年) 與範圍 (訂用帳戶/資源群組)。 例如，您可以針對每月、每季或每年期間建立 Azure 訂用帳戶預算。
    - 建立預算之後，即會在成本分析中顯示它。 根據目前的費用檢視您的預算是分析成本與費用時必須執行的前幾個步驟之一。
  - 達到預算閾值時，可以傳送電子郵件通知。
  - 您可以將成本管理資料匯出到 Azure 儲存體進行分析。

    ![成本管理預算](./media/migrate-best-practices-costs/budget.png)
    _Azure 成本管理預算_

- **進行成本分析：** 取得成本分析來探索及分析組織成本，以協助您瞭解成本的累算方式，並找出支出趨勢。
  - EA 使用者可以使用成本分析功能。
  - 您可以檢視各種範圍的成本分析資料，包括依部門、帳戶、訂用帳戶或資源群組來檢視。
  - 您可以取得顯示當月份總成本的成本分析，以及累計每日成本。

    ![Azure 成本管理分析 ](./media/migrate-best-practices-costs/analysis.png)
     _圖： Azure 成本管理分析_
- **取得建議：** 取得 Advisor 建議，告訴您如何優化和提升效率。

**瞭解更多資訊：**

- 取得[Azure 成本管理](https://docs.microsoft.com/azure/cost-management-billing/cost-management-billing-overview)的總覽。
- 瞭解如何[使用 Azure 成本管理來優化您的雲端投資](https://docs.microsoft.com/azure/cost-management-billing/costs/cost-mgt-best-practices)。
- 瞭解如何[使用 Azure 成本管理報表](https://docs.microsoft.com/azure/cost-management/use-reports)。
- 取得[從建議中優化成本的教學](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-acm-opt-recommendations)課程。
- 請參閱[Azure 使用量 API](https://docs.microsoft.com/rest/api/consumption/budgets)。

## <a name="best-practice-monitor-resource-utilization"></a>最佳做法：監視資源使用率

在 Azure 中，您只需要針對您使用的資源付費，而不需要針對未使用資源的情況付費。 針對 VM，帳單會在配置 VM 時產生，而且解除配置 VM 之後，您就不會再收到帳單。 將此謹記在心，您應該監視使用中的 VM，並檢查 VM 大小調整。

- 持續評估您的 VM 工作負載以決定基準。
- 例如，若您的工作負載在週一到週五上午 8 點到下午 6 點的使用量非常高，但在那些時間以外的時間幾乎沒有使用量，您可以在尖峰時間以外將 VM 降級。 這可能表示變更 VM 大小，或使用虛擬機器擴展集來自動相應增加或相應減少 VM。
- 某些公司依行事曆設定何時需要 VM、何時不需要。
- 除了 VM 監視之外，您也應該監視過度使用或低使用量的其他網路資源 (例如 ExpressRoute 與虛擬網路閘道)。
- 您可以使用 Microsoft 工具 (例如 Azure 成本管理、Azure 監視器與 Azure Advisor) 來監視 VM 使用量。 也有第三方工具可用。

**瞭解更多資訊：**

- 取得 [Azure 監視器](https://docs.microsoft.com/azure/azure-monitor/overview) 與 [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-overview) 概觀。
- [取得](https://docs.microsoft.com/azure/advisor/advisor-cost-recommendations) Advisor 成本建議。
- [了解如何[從建議最佳化成本](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-acm-opt-recommendations)，以及[防止非預期的費用](https://docs.microsoft.com/azure/billing/billing-getting-started)。
- 了解 [Azure Resource Optimization (ARO) Toolkit](https://github.com/azure/azure-quickstart-templates/tree/master/azure-resource-optimization-toolkit)。

## <a name="best-practice-implement-resource-group-budgets"></a>最佳做法：執行資源群組預算

通常，資源群組是用來代表成本界限。 搭配這種使用模式，Azure 小組繼續開發新的加強方式來追蹤及分析不同層級的資源費用，包括建立資源群組與資源預算的能力。

- 資源群組預算可協助您追蹤與資源群組關聯的成本。
- 當達到或超出預算時，您可以觸發警示並執行各種劇本。

**瞭解更多資訊：**

- [了解如何](https://docs.microsoft.com/azure/billing/billing-cost-management-budget-scenario)使用 Azure Budgets 來管理成本。
- [依照教學課程](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-acm-create-budgets)建立及管理 Azure 預算。

## <a name="best-practice-optimize-azure-monitor-retention"></a>最佳做法：優化 Azure 監視器保留

您可以將資源移到 Azure 中並為資源啟用診斷記錄，您可以產生大量的記錄資料。 一般而言，此記錄資料會傳送到對應到 Log Analytics 工作區的儲存體帳戶。

- 資料保留期間越長，您有的資料就越多。
- 並非所有記錄資料都相等，某些資源將會產生比其他資源更多的資料。
- 由於法規與合規性，針對某些資源保留記錄資料的時間長度可能會比其他資源長。
- 在最佳化您的記錄儲存體成本與保存所需的記錄資料之間應該仔細評估。
- 我們建議在完成移轉之後立即評估並設定，這樣您才不會花錢保存不重要的記錄。

**瞭解更多資訊：**

- [了解](https://docs.microsoft.com/azure/azure-monitor/platform/usage-estimated-costs)如何監視使用量及估計成本。

## <a name="best-practice-optimize-storage"></a>最佳做法：優化儲存體

若您在移轉之前依照最佳做法來選取儲存體，您可能可以獲得一些好處。 不過，可能有您仍可以最佳化的其他儲存體成本。 Blob 與檔案會隨著時間而變成過時。 資料可能無法再使用，但法規需求可能表示您必須將它保存一段時間。 因此，您可能不需要將它存放在用於原始移轉的高效能儲存體上。

找出過時資料並移動到較便宜的儲存體區域對您的每月儲存體預算與成本節省會有大幅影響。 Azure 提供許多方式來協助您找出並存放此類過時資料。

- 利用一般用途 v2 儲存體的存取層，將較不重要的資料從經常性移至非經常性存取和封存層。
- 使用 StorSimple 來協助根據自訂原則移動過時資料。

**瞭解更多資訊：**

- [深入了解](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers)存取層。
- [取得 StorSimple 概觀](https://docs.microsoft.com/azure/azure-monitor/overview) 與 [StorSimple 定價](https://azure.microsoft.com/pricing/details/storsimple)。

## <a name="best-practice-automate-vm-optimization"></a>最佳做法：自動化 VM 優化

在雲端中執行 VM 的終極目標是最佳化它所使用的 CPU、記憶體與磁碟。 若您發現未最佳化的 VM，或經常有 VM 未使用的時間，可以將 VM 關機或使用虛擬機器擴展集將其降級。

您可以使用 Azure 自動化、虛擬機器擴展集、autoshutdown，以及腳本式或協力廠商解決方案來優化 VM。

**瞭解更多資訊：**

- [了解如何](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-vertical-scale-reprovision)使用垂直自動調整規模。
- [排定](https://azure.microsoft.com/updates/azure-devtest-labs-schedule-vm-auto-start) VM 自動啟動。
- [了解如何](https://docs.microsoft.com/azure/automation/automation-solution-vm-management) 在 Azure 自動化中開始或停止 VM 離線時間。
- 取得[Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-overview)的詳細資訊，以及[Azure 資源優化（ARO）工具](https://github.com/azure/azure-quickstart-templates/tree/master/azure-resource-optimization-toolkit)組。

## <a name="best-practices-use-logic-apps-and-runbooks-with-budgets-api"></a>最佳做法：使用 Logic Apps 和 runbook 搭配預算 API

Azure 提供 REST API，它可以存取您的租用戶帳單資訊。

- 您可以使用預算 API 來整合外部系統與工作流程 (透過您從 API 資料建置的計量觸發)。
- 您可以將使用量與資源資料提取到您慣用的資料分析工具中。
- Azure 資源使用情況和 RateCard API 可協助您準確地預測並管理成本。
- API 是實作為資源提供者並包括 Azure Resource Manager 所提供的 API。
- 預算 API 可以與 Azure Logic Apps 與 Runbook 整合。

**瞭解更多資訊：**

- [深入了解](https://docs.microsoft.com/rest/api/consumption/budgets)預算 API。
- 使用帳單 API [取得](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview) Azure 使用量見解。

## <a name="best-practice-implement-serverless-technologies"></a>最佳做法：執行無伺服器技術

VM 工作負載通常會依「現況」移轉以避免停機。 VM 通常可能裝載間歇性工作，因此需要短時間執行，或者需要數小時執行。 例如，執行已排定工作 (例如 Windows 工作排程器或 PowerShell 指令碼) 的 VM。 當這些工作未執行時，您仍然必須吸收 VM 與磁碟儲存體成本。

在移轉之後，徹底檢閱這些類型的工作之後，您可能可以考慮將其移轉到無伺服器技術，例如 Azure Functions 或 Azure Batch 作業。 使用此解決方案之後，您再也不需要管理及維護 VM，因此可節省額外的成本。

**瞭解更多資訊：**

- 了解 [Azure Functions](https://azure.microsoft.com/services/functions)。
- 了解 [Azure Batch](https://azure.microsoft.com/services/batch)。

## <a name="next-steps"></a>後續步驟

檢閱其他最佳做法：

- 移轉後的安全性及管理[最佳做法](./migrate-best-practices-security-management.md)。
- 移轉後的網路功能[最佳做法](./migrate-best-practices-networking.md)。
