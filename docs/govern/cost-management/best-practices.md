---
title: Azure 資源的成本和大小調整
description: 使用適用于 Azure 的雲端採用架構，瞭解在 Azure 中調整資源成本和大小的最佳做法。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: internal
ms.openlocfilehash: 167d95733dbecab3b668e234e314765a414fc6be
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102113192"
---
<!-- docutune:casing ARO -->

# <a name="best-practices-for-costing-and-sizing-resources-hosted-in-azure"></a>Azure 中託管資源的成本和大小最佳作法

在提供治理的專業領域時，成本管理是企業層級的週期性主題。 藉由優化和管理成本，您可以確保 Azure 環境的長期成功。 所有小組都 (（例如財務、管理和應用程式開發團隊）) 瞭解相關聯的成本，並定期進行審查。

## <a name="best-practices-by-team-and-accountability"></a>小組和責任的最佳作法

整個企業的成本管理是雲端治理和雲端操作功能。 所有成本管理決策都會導致支援工作負載的資產變更。 當這些變更影響工作負載的架構時，需要其他考慮，以將對使用者和商務功能的影響降至最低。 設定或開發該工作負載的雲端採用小組可能會持有完成這些變更類型的責任。

集中式與非集中式小組應共同合作，以確保下列高階行為會跨您的組合來實行：

- **標記對於所有治理都很重要。** 確定所有工作負載和資源都遵循 [適當的命名和標記慣例](../../ready/azure-best-practices/naming-and-tagging.md) ，並 [使用 Azure 原則來強制執行標記慣例](/azure/governance/policy/tutorials/govern-tags)。 這可協助您的集中式治理小組制定明智的成本管理決策。
- **授權調整：** 適當配置 Azure 混合式權益和 Azure 保留的 VM 實例，將可大幅降低您的資產組合中資產的每單位成本。 這些類型的授權決策通常是由中央採購功能所建立和維護。 不過，非集中式工作負載小組可能會想要獲得購買和配置授權的諮詢，以將其個別工作負載的成本降至最低。
- **找出適當大小的機會。** 查看整個環境中目前的資源使用率和效能需求。 修改每個資源，以使用可支援每個資源之效能需求的最小實例或 SKU。
- **關閉並取消布建未使用的資源：** 未使用的資產會在雲端環境中增加成本。 識別並終止任何新增至成本的資源，但不會新增至商業價值。
- **水準縮放。** 使用多個小型實例可讓您更輕鬆地調整單一大型實例的路徑。 這可讓調整規模自動化，以建立成本優化。

## <a name="operational-cost-management-best-practices"></a>營運成本管理最佳做法

下列最佳作法通常是由雲端治理或雲端作業小組的成員，根據修補和其他排程維護程式來完成。 這些最佳作法會對應到本文稍後的可採取動作的指導方針。

- **標記對於所有治理都很重要：** 確定所有工作負載和資源都遵循 [適當的命名和標記慣例](../../ready/azure-best-practices/naming-and-tagging.md) ，並 [使用 Azure 原則來強制執行標記慣例](/azure/governance/policy/tutorials/govern-tags)。
- **授權調整：** 對於大量工作負載的最立即成本影響，將來自于規劃完善的授權取得策略。 Azure 混合式權益、Azure 保留的 VM 實例、點 Vm 和其他購買策略的購買和配置，將可快速降低整個雲端組合的成本。
- **找出適當的大小機會：** 在整個環境中檢查目前的資源使用率和效能需求，以找出在一段時間內保持使用量過低的資源， (通常超過90天的) 。
- **適當大小的已布建 sku：** 修改使用量過低的資源，以使用可支援每個資源之效能需求的最小實例或 SKU。
- **Vm 的自動關機：** 當 VM 不是持續使用時，請考慮自動關機。 VM 將不會被刪除或解除委任，但在重新開啟之前，將會停止耗用計算和記憶體成本。
- **自動關閉所有非生產資產：** 如果 VM 是非生產環境的一部分，特別是開發環境，請建立自動關機原則來降低未使用的成本。 可能的話，請使用 Azure DevTest Labs 作為自助選項，協助開發人員自行負責成本。
- **關閉並解除委任未使用的資源：** 沒錯，我們說過兩次。 如果資源未在90天內使用，且沒有明顯的執行時間需求，請將其關閉。 更重要的是，如果電腦已停止或關閉超過90天，則取消布建並刪除該資源。 透過備份或其他機制，驗證是否符合任何資料保留原則。
- **清除孤立的磁片：** 刪除未使用的儲存體，尤其是不再連接至任何 Vm 的 VM 儲存體。
- **適當大小的冗余：** 如果資源不需要高度的冗余，請移除異地多餘的儲存體。
- **調整自動調整參數：** 操作監視可能會發現各種資產的使用模式。 當這些使用模式對應至用來驅動自動調整行為的參數時，作業小組通常會調整自動調整參數，以符合季節性需求或預算配置變更。 請參閱工作負載成本管理最佳作法，以瞭解重要的預防措施。

## <a name="workload-cost-management-best-practices"></a>工作負載成本管理最佳做法

在進行架構變更之前，請先參閱工作負載的技術主管。 使用 [Microsoft azure Well-Architected 審核](/assessments/?id=azure-architecture-review) 和 [Microsoft azure Well-Architected 架構](/azure/architecture/framework/) 來輔助工作負載的檢查，以引導有關下列架構變更類型的決策。

- **Azure App Service。** 確認任何 Premium 層 App Service 方案的生產需求。 如果沒有了解工作負載和基礎資產設定的商務需求，就很難判斷是否需要進階層方案。
- **水準縮放。** 使用多個小型實例可讓您更輕鬆地調整單一大型實例的路徑。 這可讓調整規模自動化，以建立成本優化。 技術小組必須先確認應用程式具有等冪性，工作負載才能水準調整。 水準調整可能需要變更應用程式的各層級的程式碼和設定。
- **自動調整.** 在所有應用程式服務上啟用自動調整，以允許高載數目的較小 Vm。 啟用自動調整具有相同的等冪需求，這需要您瞭解工作負載架構。 工作負載和支援資產必須經核准，才能在任何操作變更之前，由採用小組進行水準調整和自動調整。
- **實施無伺服器技術：** VM 工作負載通常會以「原樣」遷移，以避免停機。 VM 通常可能裝載間歇性工作，因此需要短時間執行，或者需要數小時執行。 例如，執行已排定工作 (例如 Windows 工作排程器或 PowerShell 指令碼) 的 VM。 當這些工作未執行時，您仍然必須吸收 VM 與磁碟儲存體成本。 遷移之後，請考慮將工作負載的層重新架構至無伺服器技術，例如 Azure 函數或 Azure Batch 作業。

## <a name="actionable-best-practices"></a>可行的最佳做法

本文的其餘部分提供策略性範例，讓雲端治理或雲端營運團隊可以遵循這些實務，以將整個企業成本優化。

## <a name="before-adoption"></a>採用之前

將您的工作負載移轉到雲端之前，請預估在 Azure 中執行它們的每月成本。 主動管理雲端成本可協助您遵守您的運營費用預算。 本節中的最佳做法可協助您預估成本，並在將工作負載部署至雲端之前，對 Vm 和儲存體執行適當的大小調整。

## <a name="best-practice-estimate-monthly-workload-costs"></a>最佳做法：預估每月工作負載成本

若要預測 Azure 資源的每月帳單，您可以使用數個工具。

<!-- TODO: Change "input costs" -->

- **Azure 定價計算機：** 選取您要評估的產品（例如 Vm 和儲存體），然後將成本輸入計算機以建立估價。

    ![Azure 定價計算機 ](../../migrate/azure-best-practices/media/migrate-best-practices-costs/pricing.png) *的 azure 定價計算機。*

- **Azure 遷移：** 若要預估成本，您必須檢查並考慮在 Azure 中執行工作負載所需的所有資源。 若要取得此資料，您可以建立您的資產清查，包括伺服器、VM、資料庫與儲存體。 您可以使用 Azure Migrate 來收集此資訊。
  - Azure Migrate 會探索並評量您的內部部署環境以提供清查。
  - Azure Migrate 可以對應 VM 之間的相依性並顯示此資訊，以便您可以對其有完整了解。
  - Azure Migrate 評量包含預估的成本。
    - **計算成本：** 使用您在建立評量時建議的 Azure VM 大小，Azure 遷移會使用 Azure 計費 Api 來計算每月預估的 VM 成本。 預估會考慮作業系統、軟體保證、Azure 保留的 VM 實例、VM 運作時間、位置和貨幣設定。 它會在評量中彙總所有 VM 的成本，並計算每月總計算成本。
    - **儲存體成本：** Azure 遷移會匯總評估中所有 Vm 的儲存體成本，以計算每月總儲存體成本。 您可以透過彙總連結到特定機器之所有磁碟的每月成本，以計算該機器的每月儲存體成本。

    ![Azure 遷移 ](../../migrate/azure-best-practices/media/migrate-best-practices-costs/assess.png)
     *azure 遷移評量。*

**瞭解更多資訊：**

- 使用 [Azure 定價計算機](https://azure.microsoft.com/pricing/calculator/)。
- 閱讀 [Azure 遷移總覽](/azure/migrate/migrate-services-overview)。
- 深入瞭解 [Azure 遷移評](/azure/migrate/concepts-assessment-calculation)量。
- 深入瞭解 [Azure 資料庫移轉服務](/azure/dms/dms-overview)。

## <a name="best-practice-right-size-vms"></a>最佳做法：適當的 Vm 大小

當您部署 Azure VM 以支援工作負載時，您可以選擇數個選項。 每個 VM 類型都有特定的功能與不同的 CPU、記憶體與磁碟組合。 VM 會依下列方式分組：

| 類型 | 詳細資料 | 使用方式 |
|---|---|---|
| **一般用途** | 平衡的 CPU 對記憶體。 | 適用于測試和開發、小型至中型資料庫、低至中度的流量 web 伺服器。 |
| **計算最佳化** | CPU 對記憶體的比例高。 | 適用于中流量 web 伺服器、網路設備、批次處理、應用程式伺服器。 |
| **記憶體優化** | 高記憶體對 CPU。 | 適用於關聯式資料庫、中型到大型快取、記憶體內分析。 |
| **儲存體最佳化** | 高磁片輸送量和 i/o。 | 適用于大型資料、SQL 和 NoSQL 資料庫。 |
| **GPU 最佳化** | 特製化 VM。 單一或多個 GPU。 | 繁重圖形與視訊編輯。 |
| **高效能** | 最快、最強的 CPU。 具有選用的高輸送量網路介面 (RDMA) 的 Vm。 | 關鍵高效能應用程式。 |

- 請務必了解這些 VM 之間的定價差異，以及長期預算影響。
- 每一種類型在其中都有數系列的 Vm。
- 此外，當您選取某個系列中的 VM 時，您只能在該系列中相應增加或相應減少 VM。 例如， `DS2_v2` 實例可以擴大至 `DS4_v2` ，但無法變更為不同系列（例如實例）的實例 `F2S_v2` 。

**瞭解更多資訊：**

- 深入瞭解 [VM 類型和大小調整](/azure/virtual-machines/sizes)，以及將大小對應至類型。
- 規劃 [VM 大小調整](/azure/cloud-services/cloud-services-sizes-specs)。
- 查看 [虛構 Contoso 公司的範例評](../../plan/contoso-migration-assessment.md)量。

## <a name="best-practice-select-the-right-storage"></a>最佳做法：選取正確的儲存體

調整及管理內部部署儲存體 (SAN 或 NAS) 以及網路以支援它們的成本可能非常高，而且非常耗時。 檔案 (儲存體) 資料通常會移轉到is commonly migrated to 雲端，以協助解決作業與管理難題。 Microsoft 提供數個選項讓您將資料移動到 Azure，而且您需要制訂有關那些選項的決策。 為資料挑選正確的儲存體類型每個月可以為您的組織節省數萬塊錢。 一些考量：

- 未存取的資料很多，而且不是業務關鍵，不應放置在成本最高的儲存體上。
- 相反地，重要的業務關鍵資料應該放在較高層級的儲存體選項中。
- 在採用規劃期間，請依重要性取得資料清單並依重要性進行分類，以便將其對應至最適合的儲存體。 考慮預算與成本，以及效能。 成本不應該是主要決策制訂因素。 挑選最便宜的選項可能會讓工作負載面臨效能與可用性風險。

### <a name="storage-data-types"></a>儲存體資料類型

Azure 提供數種儲存體資料類型。

| 資料類型 | 詳細資料 | 使用方式 |
| ---|---|---|
| **Blob** | 針對儲存大量非結構化物件（例如文字或二進位資料）進行優化。 | 透過 HTTP/HTTPS 從任意位置存取資料。 <br><br> 針對串流與隨機存取案例使用。 例如，直接將影像與文件提供給瀏覽器、串流視訊與音訊，以及存放備份與災害復原資料。 |
| **檔案** | 透過 SMB 3.0 存取的受控檔案共用。 | 在遷移內部部署檔案共用時使用，以及提供檔案資料的多個存取與連接。 |
| **磁碟** | 以分頁 Blob 為基礎。 <br><br> 磁片類型 (速度) ：標準 HDD、標準 SSD、premium SSD 或 ultra 磁片。 <br><br> 磁片管理：非受控 (您管理磁片設定和儲存體) 或受控 (您選取磁片類型，Azure 會為您管理) 的磁片。 | 針對 Vm 使用 premium 磁片。 使用受控磁碟來獲得簡單的管理與規模調整。 |
| **佇列** | 透過 HTTP 或 HTTPS)  (的經過驗證的呼叫，儲存和取出大量的訊息。 | 使用非同步訊息佇列來連接應用程式元件。 |
| **資料表** | 存放資料表。 | 現在是 Azure Cosmos DB 資料表 API 的一部分。 |

### <a name="access-tiers"></a>存取層級

Azure 儲存體提供不同的選項來存取區塊 blob 資料。 選取適當的存取層可確保您以最具成本效益的方式儲存區塊 Blob 資料。

| 存取層 | 詳細資料 | 使用方式 |
| --- | --- | --- |
| **經常性** | 儲存成本較高，存取和交易成本較低 <br><br> 這是預設的存取層。 | 針對經常存取之作用中用途使用。 |
| **非經常性** | 儲存體成本較低，存取和交易成本較高。 <br><br> 至少存放 30 天。 | 短期存放，資料可用但不經常存取。 |
| **封存** | 用於個別區塊 Blob。 <br><br> 最具成本效益的儲存體選項。 最低儲存體成本、最高存取和交易成本。 | 使用可容忍數小時的抓取延遲時間，而且將在封存層中至少有180天的資料。 |

### <a name="storage-account-types"></a>儲存體帳戶類型

Azure 提供數種儲存體帳戶類型與效能層級。

| 帳戶類型 | 詳細資料 | 使用方式 |
| --- | --- | --- |
| **一般用途 v2 標準層** | 支援 Blob (區塊、分頁、附加)、檔案、磁碟、佇列與資料表。 <br><br> 支援經常性存取、非經常性存取和封存存取層。 支援區域冗余儲存體 (ZRS) 。 | 用於大部分案例與大部分類型的資料。 標準儲存體帳戶可以是 HDD 或 SSD 型。 |
| **一般用途 v2 Premium 層** | 支援 Blob 儲存體資料 (分頁 blob) 。 支援經常性存取、非經常性存取和封存存取層。 支援 ZRS。 <br><br> 存放在 SSD 上。 | Microsoft 建議為所有 VM 使用。 |
| **一般用途 v1** | 不支援存取分層。 不支援 ZRS | 如果應用程式需要 Azure 傳統部署模型，請使用。 |
| **Blob** | 用於存取非結構化物件的特殊化儲存體帳戶。 只 (沒有檔案、佇列、資料表或磁片儲存體服務) 時，才會提供區塊 blob 和附加 blob。 提供與一般用途 v2 相同的持久性、可用性、擴充性和效能。 | 您無法將分頁 blob 儲存在這些帳戶中，因此無法儲存 VHD 檔案。 您可以將存取層設定為經常性存取或非經常性存取層。 |

### <a name="storage-redundancy-options"></a>儲存體備援選項

儲存體帳戶可以使用不同類型的備援選項來獲得備援能力與高可用性。

| 類型 | 詳細資料 | 使用方式 |
| --- | --- | --- |
| **本機備援儲存體 (LRS)** | 透過在單一儲存體單位中複寫到不同的容錯網域與更新網域，來在本地服務故障時維持可用性。 在一個資料中心存放您資料的多個複本。 提供至少 99.999999999% (11 9 的物件在指定一年的) 耐久性。 | 請考慮您的應用程式是否儲存可輕鬆重建的資料。 |
| **區域備援儲存體 (ZRS)** | 透過複寫到單一區域中的三個儲存體叢集，來維持在資料中心無法使用時的可用性。 每個儲存體叢集實際上都是分開的，位於自己的可用性區域中。 藉由在多個資料中心或區域中保留多個資料複本，在指定的一年內提供至少 99.9999999999% (12 9 的物件持久性) 。 | 考慮您是否需要一致性、持久性和高可用性。 當多個區域都受到永久影響時，可能無法針對區域性災害提供保護。 |
| **異地備援儲存體 (GRS)** | 藉由將資料複寫到與主要區域相距數百英里的次要區域，來防止整個區域中斷。 提供至少 99.99999999999999% (16 9 的物件在指定的年份) 持久性。 | 除非 Microsoft 起始容錯移轉到次區域的作業，否則複本資料無法使用。 若發生容錯移轉，讀取與寫入存取可用。 |
| **讀取權限異地備援儲存體 (RA-GRS)** | 類似於 GRS。 提供至少 99.99999999999999% (16 9 的物件在指定的年份) 持久性。 | 允許從用於 GRS 的第二個區域讀取存取，以提供和99.99% 的讀取可用性。 |

**瞭解更多資訊：**

- 查看 [Azure 儲存體定價](https://azure.microsoft.com/pricing/details/storage/)。
- 瞭解如何使用 [azure 匯入/匯出服務](/azure/import-export/storage-import-export-service) ，安全地將大量資料匯入到 Azure Blob 儲存體和 azure 檔案服務。
- 比較 [blob、檔案和磁片儲存體資料類型](/azure/storage/common/storage-introduction)。
- 深入瞭解 [存取層](/azure/storage/blobs/storage-blob-storage-tiers)。
- 檢查 [不同類型的儲存體帳戶](/azure/storage/common/storage-account-overview)。
- 深入瞭解 [Azure 儲存體冗余](/azure/storage/common/storage-redundancy)，包括 LRS、ZRS、GRS 和讀取權限 GRS。
- 深入瞭解 [Azure](/azure/storage/files/storage-files-introduction)檔案儲存體。

## <a name="after-adoption"></a>採用後

在採用之前，成本預測取決於工作負載擁有者和雲端採用小組所制定的決策。 雖然治理小組可以協助影響這些決策，但是治理小組可能不會採取任何動作。

一旦資源在生產環境中，就可以匯總資料並在環境層級分析趨勢。 此資料將協助治理小組根據實際的使用模式和目前的狀態架構，獨立進行大小調整和使用決策。

- 分析資料以產生 Azure 資源群組和資源的預算基準。
- 識別可讓您減少大小、停止或暫停資源的使用模式，以進一步降低成本。

本節中的最佳作法包括使用 Azure 混合式權益和 Azure 保留的虛擬機器實例、降低訂用帳戶的雲端費用、使用 Azure 成本管理 + 計費來進行成本預算和分析、監視資源和執行資源群組預算，以及優化監視、儲存體和 Vm。

## <a name="best-practice-take-advantage-of-azure-hybrid-benefit"></a>最佳做法：利用 Azure 混合式權益

由於長期以來在 Windows Server 與 SQL Server 等系統的軟體投資，Microsoft 身處獨特位置，可為透過其他雲端提供者不見得提供的實質折扣，為客戶提供雲端中的價值。

整合式 Microsoft 內部部署/Azure 產品組合可提供競爭與成本優勢。 如果您目前有透過軟體保證 (SA) 的作業系統或其他軟體授權，您可以透過 Azure 混合式權益將這些授權帶到雲端。

**瞭解更多資訊：**

- [請看看](https://azure.microsoft.com/pricing/hybrid-benefit/) Azure 混合式權益節省計算機。
- 深入瞭解 [適用于 Windows Server 的 Azure 混合式權益](https://azure.microsoft.com/pricing/hybrid-benefit/)。
- 請參閱 [SQL Server Azure vm 的定價指導](/azure/azure-sql/virtual-machines/windows/pricing-guidance)方針。

## <a name="best-practice-use-azure-reserved-vm-instances"></a>最佳做法：使用 Azure 保留的 VM 實例

大部分的雲端平台都是設定為隨用隨付。 此模型有缺點，因為您不見得知道動態工作負載為何。 當您為工作負載指定清楚的意圖時，您就為基礎結構規劃做出貢獻。

使用 Azure 保留的 VM 實例時，您必須預付一年或三年期的保留實例。

- 預付提供所使用資源的折扣。
- 您可以透過隨用隨付價格高達72%，大幅降低 VM 計算、SQL Database 計算、Azure Cosmos DB 或其他資源的成本。
- 保留的實例會提供計費折扣，且不會影響資源的執行時間狀態。
- 您可以取消保留執行個體。

![Azure 保留的虛擬機器實例 ](../../migrate/azure-best-practices/media/migrate-best-practices-costs/reserve.png)
 *圖1： azure 保留的 vm。*

**瞭解更多資訊：**

- 瞭解 [Azure 保留](/azure/cost-management-billing/reservations/save-compute-costs-reservations)專案。
- 閱讀 [保留實例的常見問題](https://azure.microsoft.com/pricing/reserved-vm-instances/#faq)。
- 請參閱 [SQL Server Azure vm 的定價指導](/azure/azure-sql/virtual-machines/windows/pricing-guidance)方針。

## <a name="best-practice-aggregate-cloud-spend-across-subscriptions"></a>最佳做法：跨訂用帳戶匯總雲端支出

您終究會有多個 Azure 訂用帳戶。 例如，您可能需要額外的訂用帳戶來將開發與生產界限分開，或者您的平台可能需要為每個用戶端使用不同的訂用帳戶。 因此，跨所有訂用帳戶將資料報告彙總到單一平台的功能非常有價值。

若要這樣做，您可以使用 Azure 成本管理 + 計費 Api。 然後，將資料匯總成單一來源（例如 Azure SQL Database）之後，您就可以使用 Power BI 之類的工具來呈現匯總的資料。 您可以建立彙總的訂用帳戶報告，以及精細的報告。 例如，對於需要對成本管理進行主動式見解的使用者，您可以根據部門、資源群組或其他資訊來建立特定的成本觀點。 您不需要將完整的 Azure 帳單資料存取權提供給他們。

**瞭解更多資訊：**

- 閱讀 [Azure 使用量 api 總覽](/azure/cost-management-billing/manage/consumption-api-overview)。
- 瞭解如何 [在 POWER BI Desktop 中連線到 Azure 使用量見解](/power-bi/connect-data/desktop-connect-azure-consumption-insights)。
- 瞭解如何 [使用 azure 角色型存取控制 (AZURE RBAC) 來管理 azure 帳單資訊的存取權 ](/azure/cost-management-billing/manage/manage-billing-access)。

## <a name="best-practice-monitor-resource-utilization"></a>最佳做法：監視資源使用量

在 Azure 中，您只需要針對您使用的資源付費，而不需要針對未使用資源的情況付費。 針對 VM，帳單會在配置 VM 時產生，而且解除配置 VM 之後，您就不會再收到帳單。 將此謹記在心，您應該監視使用中的 VM，並檢查 VM 大小調整。

- 持續評估您的 VM 工作負載以決定基準。
- 例如，若您的工作負載在週一到週五上午 8 點到下午 6 點的使用量非常高，但在那些時間以外的時間幾乎沒有使用量，您可以在尖峰時間以外將 VM 降級。 這可能表示變更 VM 大小，或使用虛擬機器擴展集來自動相應增加或相應減少 VM。
- 某些公司依行事曆設定何時需要 VM、何時不需要。
- 除了 VM 監視之外，您也應該監視過度使用或低使用量的其他網路資源 (例如 ExpressRoute 與虛擬網路閘道)。
- 您可以使用 Microsoft 工具（例如 Azure 成本管理 + 計費、Azure 監視器和 Azure Advisor）來監視 VM 使用量。 也有第三方工具可用。

**瞭解更多資訊：**

- 閱讀 [Azure 監視器](/azure/azure-monitor/overview) 和 [azure Advisor](/azure/advisor/advisor-overview)的總覽。
- 取得 [Azure Advisor 成本建議](/azure/advisor/advisor-cost-recommendations)。
- 瞭解如何 [從建議中將成本優化](/azure/cost-management-billing/costs/tutorial-acm-opt-recommendations)，並 [防止非預期的費用](/azure/cost-management-billing/cost-management-billing-overview)。
- 瞭解 [ (ARO) 工具組的 Azure 資源優化](https://github.com/azure/azure-quickstart-templates/tree/master/azure-resource-optimization-toolkit)。

## <a name="best-practice-reduce-nonproduction-costs"></a>最佳做法：減少非生產成本

開發、測試和品質保證 (在開發週期期間需要 QA) 環境。 可惜的是，這些環境通常會在不再有用時保持布建的時間。 定期審核未使用的非生產環境，可能會對成本產生立即的影響。

此外，請考慮任何非生產環境的一般成本降低：

- 減少非生產資源，以使用較低成本的 B 系列 Vm 和標準儲存體。
- 使用點 Vm 減少非生產計算成本。
- 套用 Azure 原則，以針對任何非生產資源要求資源層級的成本降低。

**瞭解更多資訊：**

- [使用標記](/azure/azure-resource-manager/management/tag-resources) 來識別要調整大小或終止的開發、測試或 QA 目標。
- [自動關機 vm](/azure/cost-management-billing/cost-management-billing-overview#consider-cost-cutting-features-like-auto-shutdown-for-vms) 會設定 vm 的夜間終止時間。 使用這項功能將會每晚停止非生產的 Vm，要求開發人員在準備好繼續開發時重新開機這些 Vm。
- 現成的[vm](/azure/virtual-machines/spot-vms)可讓 it 以大幅節省成本的方式利用未使用的 Azure 容量。 不過，在 Azure 需要取回容量的任何時間點，Azure 基礎結構將會收回 Vm 的位置。
- 鼓勵開發小組使用 [Azure DevTest Labs](/azure/devtest-labs/devtest-lab-overview) 來建立自己的成本控制方法，並避免在先前的步驟中影響標準自動關機的時間。

## <a name="best-practice-use-azure-cost-management--billing"></a>最佳做法：使用 Azure 成本管理 + 計費

Microsoft 提供 Azure 成本管理 + 計費來協助您追蹤費用：

- 協助您監視及控制 Azure 費用，並將資源的使用最佳化。
- 檢閱您的整個訂用帳戶與其所有資源，並產生建議。
- 提供完整的 API 來整合外部工具與財務系統以進行報告。
- 使用單一的整合式檢視來追蹤資源使用量並管理雲端成本。
- 提供豐富的營運與財務見解以協助您做出明智的決策。

在 [Azure 成本管理 + 計費] 中，您可以：

- **建立預算：** 建立財務責任的預算。
  - 您可以納入您使用或訂閱的服務一段時間 (每月、每季、每年) 與範圍 (訂用帳戶/資源群組)。 例如，您可以針對每月、每季或每年期間建立 Azure 訂用帳戶預算。
    - 建立預算之後，即會在成本分析中顯示它。 根據目前的費用檢視您的預算是分析成本與費用時必須執行的前幾個步驟之一。
  - 達到預算閾值時，可以傳送電子郵件通知。
  - 您可以將成本管理資料匯出至 Azure 儲存體，以進行分析。

    ![查看 Azure 成本管理中的預算 + ](../../migrate/azure-best-practices/media/migrate-best-practices-costs/budget.png)
     *Azure 成本管理 + 計費的計費預算。*

- **進行成本分析：** 取得成本分析來探索及分析組織成本，以協助您瞭解成本如何累積，並找出花費趨勢。
  - EA 使用者可以使用成本分析功能。
  - 您可以查看各種範圍的成本分析資料，包括依部門、帳戶、訂用帳戶或資源群組。
  - 您可以取得顯示當月份總成本的成本分析，以及累計每日成本。

    ![Azure 成本管理 + 計費分析 ](../../migrate/azure-best-practices/media/migrate-best-practices-costs/analysis.png)
     *圖： Azure 成本管理 + 帳單分析。*

- **取得建議：** 取得建議程式的建議，告訴您如何優化及改善效率。

**瞭解更多資訊：**

- 閱讀 [Azure 成本管理 + 計費總覽](/azure/cost-management-billing/cloudyn/overview)。
- 瞭解如何 [使用 Azure 成本管理和計費來優化您的雲端投資](/azure/cost-management-billing/costs/cost-mgt-best-practices)。
- 瞭解如何使用 [Azure 成本管理 + 計費報表](/azure/cost-management-billing/cloudyn/use-reports)。
- 請參閱 [從建議中將成本優化](/azure/cost-management-billing/costs/tutorial-acm-opt-recommendations)的教學課程。
- 查看 [Azure 使用量 api](/rest/api/consumption/budgets)。

## <a name="best-practice-implement-resource-group-budgets"></a>最佳做法：實行資源群組預算

通常，資源群組是用來代表成本界限。 搭配這種使用模式，Azure 小組繼續開發新的加強方式來追蹤及分析不同層級的資源費用，包括建立資源群組與資源預算的能力。

- 資源群組預算可協助您追蹤與資源群組關聯的成本。
- 當達到或超出預算時，您可以觸發警示並執行各種劇本。

**瞭解更多資訊：**

- 瞭解如何 [使用 Azure 預算來管理成本](/azure/cost-management-billing/manage/cost-management-budget-scenario)。
- 請參閱 [建立和管理 Azure 預算](/azure/cost-management-billing/costs/tutorial-acm-create-budgets)的教學課程。

## <a name="best-practice-review-azure-advisor-recommendations"></a>最佳做法：審查 Azure Advisor 的建議

Azure Advisor 成本建議會找出可降低成本的機會。 當預算出現高或使用率偏低時，請使用這份報告來尋找可快速調整成本的立即機會。

**瞭解更多資訊：**

- 請[參閱 Azure Advisor 成本建議](/azure/advisor/advisor-cost-recommendations)以立即採取行動。

## <a name="best-practice-optimize-azure-monitor-retention"></a>最佳做法：將 Azure 監視器保留優化

您可以將資源移到 Azure 中並為資源啟用診斷記錄，您可以產生大量的記錄資料。 一般而言，此記錄資料會傳送到對應到 Log Analytics 工作區的儲存體帳戶。

- 資料保留期間越長，您有的資料就越多。
- 並非所有記錄資料都相等，某些資源將會產生比其他資源更多的資料。
- 由於法規與合規性，針對某些資源保留記錄資料的時間長度可能會比其他資源長。
- 在優化記錄儲存體成本和保留所需的記錄資料之間取得平衡。
- 我們建議在完成移轉之後立即評估並設定，這樣您才不會花錢保存不重要的記錄。

**瞭解更多資訊：**

- 瞭解如何 [監視使用量和估計成本](/azure/azure-monitor/usage-estimated-costs)。

## <a name="best-practice-optimize-storage"></a>最佳做法：將儲存體優化

如果您遵循在採用之前選取儲存體的最佳作法，您可能會享受一些優點。 您可能會將一些額外的儲存成本優化。 經過一段時間後，blob 和檔案就會變成過時。 資料可能無法再使用，但法規需求可能表示您必須將它保存一段時間。 因此，您可能不需要將它儲存在用於原始採用的高效能儲存體上。

找出過時資料並移動到較便宜的儲存體區域對您的每月儲存體預算與成本節省會有大幅影響。 Azure 提供許多方式來協助您找出並存放此類過時資料。

- 將較不重要的資料從經常性存取層移至非經常性存取層和封存層，以利用一般用途 v2 儲存體的存取層。
- 使用 StorSimple 來協助根據自訂原則移動過時資料。

**瞭解更多資訊：**

- 深入瞭解 [存取層](/azure/storage/blobs/storage-blob-storage-tiers)。
- 閱讀 [StorSimple 總覽](/azure/azure-monitor/overview)。
- 檢查 [StorSimple 定價](https://azure.microsoft.com/pricing/details/storsimple/)。

## <a name="best-practice-automate-vm-optimization"></a>最佳做法：自動化 VM 優化

在雲端中執行 VM 的終極目標是最佳化它所使用的 CPU、記憶體與磁碟。 若您發現未最佳化的 VM，或經常有 VM 未使用的時間，可以將 VM 關機或使用虛擬機器擴展集將其降級。

您可以使用 Azure 自動化、虛擬機器擴展集、自動關機與指令碼或第三方解決方案來最佳化 VM。

**瞭解更多資訊：**

- 瞭解 [垂直](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-vertical-scale-reprovision)自動調整。
- 查看 [Azure DevTest Labs：排程 VM 自動啟動](https://azure.microsoft.com/updates/azure-devtest-labs-schedule-vm-auto-start/)。
- 瞭解如何 [在 Azure 自動化中啟動或停止 vm 的停機時間](/azure/automation/automation-solution-vm-management)。
- 取得 [Azure Advisor](/azure/advisor/advisor-overview)的詳細資訊，以及 [azure 資源優化 (ARO) 工具](https://github.com/azure/azure-quickstart-templates/tree/master/azure-resource-optimization-toolkit)組。

## <a name="best-practice-use-logic-apps-and-runbooks-with-budgets-api"></a>最佳做法：使用邏輯應用程式和 runbook 搭配預算 API

Azure 提供可存取您租使用者帳單資訊的 REST API。

- 您可以使用預算 API 來整合外部系統與工作流程 (透過您從 API 資料建置的計量觸發)。
- 您可以將使用量與資源資料提取到您慣用的資料分析工具中。
- Azure 資源使用方式 API 和 Azure 資源 RateCard API 可協助您準確地預測和管理您的成本。
- 這些 Api 會實作為資源提供者，並包含在 Azure Resource Manager 所公開的 Api 中。
- 預算 API 可以與 Azure 邏輯應用程式和 Azure 自動化 runbook 整合。

**瞭解更多資訊：**

- 深入瞭解 [預算 API](/rest/api/consumption/budgets)。
- 使用 Azure 計費 Api 取得 Azure 使用量的[見解](/azure/cost-management-billing/manage/consumption-api-overview#usage-details-api)。

## <a name="next-steps"></a>下一步

瞭解最佳做法之後，請檢查 [成本管理工具鏈](./toolchain.md) ，找出可協助您執行這些最佳作法的 Azure 工具和功能。

> [!div class="nextstepaction"]
> [適用於 Azure 成本管理工具鏈](./toolchain.md)
