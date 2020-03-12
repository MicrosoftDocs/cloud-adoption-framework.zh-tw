---
title: 對 Azure 進行大規模移轉
description: 使用適用于 Azure 的雲端採用架構，瞭解如何規劃和執行大規模的遷移至 Azure。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/08/2018
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: azure-migrate
ms.openlocfilehash: 8c3ee0a75efa74aa1599399358bac267c5ffe1de
ms.sourcegitcommit: 959cb0f63e4fe2d01fec2b820b8237e98599d14f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/11/2020
ms.locfileid: "79091922"
---
# <a name="scale-a-migration-to-azure"></a>對 Azure 進行大規模移轉

本文將示範虛構公司 Contoso 如何大規模執行移轉至 Azure 的工作。 其正在思考如何規劃和執行超過 3,000 個工作負載、8,000 個資料庫和超過 10,000 個 VM 的移轉。

## <a name="business-drivers"></a>商業動機

IT 領導小組與商務合作夥伴密切合作，以了解此次移轉所要實現的目標：

- **因應業務成長。** Contoso 的業務量日益增多，對內部部署系統和基礎結構造成了壓力。
- **提高效率。** Contoso 必須移除不必要的程序，並且簡化開發人員和使用者的程序。 企業亟需快速且不浪費時間或金錢的 IT 服務，進而更快滿足客戶的需求。
- **提高靈活性。** Contoso IT 必須能夠更快因應企業的需求。 其因應速度必須能夠比市場變化更快，才能更在全球經濟中獲致成功。 它不得礙事，或成為企業的絆腳石。
- **調整。** 隨著企業順利成長，Contoso IT 小組必須提供能夠同步成長的系統。
- **改進成本模型。** Contoso 想要減少 IT 預算的資本適足要求。 Contoso 想要使用雲端功能來調整規模，並減少對於昂貴硬體的需要。
- **降低授權成本。** Contoso 想要將雲端成本降至最低。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已針對此次移轉擬定好各項目標。 並用這些目標用來判斷最合適的移轉方法。

**需求** | **詳細資料**
--- | ---
**快速移至 Azure** | Contoso 想要盡快開始將應用程式和 VM 移至 Azure。
**編譯完整的詳細目錄** | Contoso 想要獲得組織中所有應用程式、資料庫和 VM 的完整詳細目錄。
**評估應用程式並分類** | Contoso 想要充分利用雲端。 根據預設，Contoso 假設所有服務都會以 PaaS 的形式執行。 PaaS 不適合時，則會使用 IaaS。
**訓練並移至 DevOps** | Contoso 想要移至 DevOps 模型。 Contoso 會提供 Azure 與 DevOps 訓練，並視需要重組小組。

確立目標和需求後，Contoso 會檢閱 IT 磁碟使用量，並識別移轉程序。

## <a name="current-deployment"></a>目前的部署

在規劃和設定 [Azure 基礎結構](./contoso-migration-infrastructure.md)並試驗完上表所述的不同概念證明 (POC) 移轉組合之後，Contoso 已準備就緒，可以開始對 Azure 進行大規模的完整移轉。 Contoso 想要移轉的內容如下。

<!--markdownlint-disable MD033 -->

**Item** | **數量** | **詳細資料**
--- | --- | ---
**工作負載** | 超過 3,000 個應用程式 | 在 VM 上執行的應用程式。<br/><br/>  應用程式是 Windows、SQL 架構與 OSS LAMP。
**資料庫** | 大約 8,500 個 | 資料庫包含 SQL Server、MySQL、PostgreSQL。
**VM** | 超過 35,000 個 | 在 VMware 主機上執行、並由 vCenter Server 管理的 VM。

<!--markdownlint-enable MD033 -->

## <a name="migration-process"></a>移轉程序

Contoso 已確立商業誘因和移轉目標，因此決定採用四個方法來完成移轉程序：

- **第1階段：評估。** 探索目前的資產，並了解其是否適合移轉至 Azure。
- **第2階段：遷移。** 將資產移至 Azure。 要如何將應用程式和物件移至 Azure 取決於應用程式以及其想要實現的目標。
- **第3階段：優化。** 將資源移至 Azure 之後，Contoso 需要改善並簡化資源以獲得最大效能和效率。
- **第4階段：保護和管理。** 一切準備就緒後，Contoso 現在會使用 Azure 安全性和管理資源與服務來管理、保護和監視其在 Azure 中的雲端應用程式。

在整個組織中，這些階段並非是一系列的。 Contoso 移轉專案的每個部分都會落在評估及移轉程序的不同階段。 最佳化、安全性和管理則是隨時都在進行。

## <a name="phase-1-assess"></a>階段 1：評估

Contoso 會藉由探索及評估內部部署應用程式、資料和基礎結構，來啟動整個程序。 以下是 Contoso 將採取的做法：

- Contoso 需要探索應用程式、跨應用程式來對應相依性，以及決定移轉順序和優先順序。
- Contoso 由於要進行評估，所以會建置出應用程式和資源的完整詳細目錄。 連同這份新的詳細目錄，Contoso 還會使用並更新現有的組態管理資料庫 (CMDB) 和服務類別目錄。
  - CMDB 保存了 Contoso 應用程式的技術組態。
  - 服務類別目錄記載了應用程式的操作詳細資料，包括相關聯的業務夥伴及服務等級協定 (SLA)。

### <a name="discover-apps"></a>探索應用程式

Contoso 會在各種伺服器中執行數千個應用程式。 除了 CMDB 和服務類別目錄，Contoso 還需要探索及評估工具。

- 這些工具必須有機制可將評估資料送入移轉程序中。
- 評估工具必須提供資料來協助建置 Contoso 實體和虛擬資源的智慧詳細目錄。 資料中應該包含設定檔資訊與效能計量。
- 探索完成時，Contoso 應該會有資產及其相關聯中繼資料的完整詳細目錄。 此詳細目錄會用來定義移轉方案。

### <a name="identify-classifications"></a>識別分類

Contoso 會識別一些常見類別來分類詳細目錄中的資產。 這些分類對於 Contoso 決定進行遷移而言非常重要。 分類清單可協助建立移轉優先順序，並找出複雜的問題。

**類別目錄** | **指派的值** | **詳細資料**
--- | --- | ---
商務群組 | 商務群組名稱的清單 | 哪個群組負責詳細目錄項目？
POC 候選項目 | Y/N | 應用程式是否可作為雲端移轉的 POC 或早期採用者？
技術債務 | 無/一些/嚴重 | 詳細目錄項目是否執行或使用不受支援的產品、平台或作業系統？
防火牆影響 | Y/N | 應用程式是否與網際網路/外部流量進行通訊？  其是否與防火牆整合？
安全性問題 | Y/N | 應用程式是否有已知的安全性問題？  應用程式是否使用未加密資料或過期平台？

### <a name="discover-app-dependencies"></a>探索應用程式相依性

評估程序進行期間，Contoso 必須識別應用程式執行所在的位置，並了解應用程式伺服器之間的相依性和連線。 Contoso 會透過幾個步驟來對應環境。

1. 在第一個步驟中，Contoso 會探索伺服器和機器與個別應用程式、網路位置和群組的對應情形。
2. 透過這項資訊，Contoso 可以清楚地識別有少數相依性且適合快速遷移的應用程式。
3. Contoso 可以使用此一對應來找出應用程式伺服器之間更複雜的相依性和通訊。 接著，Contoso 可以根據邏輯來群組這些伺服器以代表應用程式，，並根據這些群組來規劃移轉策略。

對應完成後，Contoso 即可確保所有應用程式元件均已找出，並於建置移轉方案時納入考量。

![相依性對應](./media/contoso-migration-scale/dependency-map.png)

### <a name="evaluate-apps"></a>評估應用程式

在探索及評估程序的最後一個步驟中，Contoso 可以就評估和對應結果進行評價，以了解如何遷移服務類別目錄中的每個應用程式。

為了擷取此評價程序，其在詳細目錄中新增了幾個額外的分類。

**類別目錄** | **指派的值** | **詳細資料**
--- | --- | ---
商務群組 | 商務群組名稱的清單 | 哪個群組負責詳細目錄項目？
POC 候選項目 | Y/N | 應用程式是否可作為雲端移轉的 POC 或早期採用者？
技術債務 | 無/一些/嚴重 | 詳細目錄項目是否執行或使用不受支援的產品、平台或作業系統？
防火牆影響 | Y/N | 應用程式是否與網際網路/外部流量進行通訊？  其是否與防火牆整合？
安全性問題 | Y/N | 應用程式是否有已知的安全性問題？  應用程式是否使用未加密資料或過期平台？
移轉策略 | 重新裝載/重構/重新架構/重建 | 應用程式需要何種移轉？ 應用程式會如何部署到 Azure 中？ [詳細資訊](./contoso-migration-overview.md#migration-patterns)。
技術複雜度 | 1-5 | 移轉有多複雜？ 此值應由 Contoso DevOps 和相關合作夥伴來定義。
業務關鍵性 | 1-5 | 此應用程式對業務有多重要？ 例如，小型工作群組應用程式可能會獲得 1 分，跨組織使用的重要應用程式可能會獲得 5 分。 這個分數會影響移轉的優先順序高低。
移轉優先順序 | 1/2/3 | 應用程式的移轉優先順序為何？
移轉風險 | 1-5 | 應用程式的移轉風險層級為何？ 此值應由 Contoso DevOps 和相關合作夥伴來一致決定。

### <a name="figure-out-costs"></a>了解成本

為了了解 Azure 移轉成本和可能節省金額，Contoso 可以使用[擁有權總成本 (TCO) 計算機](https://azure.microsoft.com/pricing/tco/calculator)來計算 Azure 的 TCO 並與類似的內部部署進行比較。

### <a name="identify-assessment-tools"></a>找出評估工具

Contoso 會決定要使用哪一種工具來進行探索、評估和詳細目錄建置。 Contoso 會識別 Azure 工具和服務、原生應用程式工具和指令碼以及合作夥伴工具的搭配。 特別是，Contoso 想要知道如何使用 Azure Migrate 來進行大規模評估。

#### <a name="azure-migrate"></a>Azure Migrate

Azure Migrate 服務可協助您探索及評估內部部署 VMware VM，以做好移轉到 Azure 的準備。 Azure Migrate 的用途如下：

1. 探索：探索內部部署 VMware VM。
    - Azure Migrate 支援從多部 vCenter Server 循序探索，並可在不同的 Azure Migrate 專案中執行探索。
    - Azure Migrate 會透過執行 Migrate 收集器的 VMware VM 來執行探索。 相同的收集器也可探索不同 vCenter Server 上的 VM，並將資料傳送至不同專案。
2. 評估整備程度：評估內部部署機器是否適合在 Azure 中執行。 評估項目包括：
    - 大小建議：根據內部部署 VM 的效能歷程記錄，取得 Azure VM 的大小建議。
    - 預估每月成本：取得在 Azure 中執行內部部署機器的估計成本。
3. 識別相依性：將內部部署機器的相依性視覺化，以建立最佳的機器群組來進行評估和遷移。

![Azure Migrate](./media/contoso-migration-scale/azure-migrate.png)

##### <a name="migrate-at-scale"></a>大規模遷移

Contoso 必須使用 Azure Migrate 來正確提供這項移轉的規模。

- Contoso 會使用 Azure Migrate 來對應用程式逐一進行評估。 這可確保 Azure Migrate 會將資料即時傳回 Azure 入口網站。
- Contoso 系統管理員要了解如何[大規模部署 Azure Migrate](https://docs.microsoft.com/azure/migrate/how-to-scale-assessment)
- Contoso 在下表中摘要記下了 Azure Migrate 的限制。

**動作** | **限制**
--- | ---
建立 Azure Migrate 專案 | 10,000 個 VM
探索 | 10,000 個 VM
評量 | 10,000 個 VM

Contoso 會按照下列方式使用 Azure Migrate：

- 在 vCenter 中，Contoso 會將 VM 組織到資料夾中。 這可方便他們在對特定資料夾中的 VM 執行評估時集中焦點。
- Azure Migrate 會使用 Azure 服務對應來評估機器之間的相依性。 VM 上必須安裝代理程式才能進行評估。
  - Contoso 會使用自動化指令碼來安裝所需的 Windows 或 Linux 代理程式。
  - 透過編寫指令碼，Contoso 可以將安裝推送到 vCenter 資料夾內的 VM。

#### <a name="database-tools"></a>資料庫工具

除了 Azure Migrate，Contoso 還會聚焦在使用專為進行資料庫評估的工具。 [Data Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) 等工具有助於評估要移轉的 SQL Server 資料庫。

Data Migration Assistant (DMA) 可協助 Contoso 了解內部部署資料庫是否與各種 Azure 資料庫解決方案相容，例如 Azure SQL Database、在 Azure IaaS VM 上執行的 SQL Server 以及 Azure SQL 受控執行個體。

除了 DMS，Contoso 還會使用一些其他指令碼來探索並記錄 SQL Server 資料庫。 這些指令碼全都位於 GitHub 存放庫。

#### <a name="partner-assessment-tools"></a>合作夥伴評估工具

Contoso 另有數個合作夥伴工具可協助其評估要移轉至 Azure 的內部部署環境。 [深入了解](https://azure.microsoft.com/migration/partners) Azure 移轉合作夥伴。

## <a name="phase-2-migrate"></a>階段 2：遷移

評估完成後，Contoso 必須找出工具來將其應用程式、資料和基礎結構移至 Azure。

### <a name="migration-strategies"></a>移轉策略

Contoso 可以考慮四個廣泛的移轉策略。

<!--markdownlint-disable MD033 -->

**策略** | **詳細資料** | **使用量**
--- | --- | ---
**重新裝載** | 這通常稱為「隨即_轉移_」，這是將現有應用程式快速遷移至 Azure 的免費程式碼選項。<br/><br/> 應用程式皆依原狀移轉，可直接享有雲端優勢，而無須承擔變更程式碼的相關風險或成本。 | Contoso 可以重新裝載較不具策略性的應用程式，而不必變更程式碼。
**重構** | 此策略也稱為「重新封裝」，只需要稍微變更應用程式程式碼或組態，就能將應用程式連線至 Azure PaaS，並利用雲端功能。 | Contoso 可以重構策略性應用程式，以便既保留相同的基本功能，又將其移到 Azure 平台上執行，例如 Azure App Service。<br/><br/> 此策略所需變更的程式碼最少。<br/><br/> 但另一方面，Contoso 必須維護 VM 平台，因為 Microsoft 不會幫它管理。
**重新架構** | 此策略可修改或延伸應用程式程式碼基底，將應用程式架構最佳化以獲得雲端功能和規模。<br/><br/> 其可將應用程式現代化為能夠復原、大幅調整及獨立部署的架構。<br/><br/> Azure 服務可加快處理速度、放心調整應用程式規模，並輕鬆管理應用程式。
**重建** | 此策略會使用雲端原生技術從頭重建應用程式。<br/><br/> Azure 平台即服務 (PaaS) 會在雲端提供完整的開發與部署環境。 它會消除某些軟體授權支出和複雜性，並讓您不需要基礎應用程式基礎結構、中介軟體和其他資源。 | Contoso 可以從頭撰寫重要應用程式以利用雲端技術，例如無伺服器電腦或微服務。<br/><br/> Contoso 會管理其開發的應用程式和服務，Azure 則管理其他所有項目。

<!--markdownlint-enable MD033 -->

資料也必須考慮近來，尤其是 Contoso 所擁有的資料庫數量。 Contoso 的預設方法是使用 PaaS 服務 (例如 Azure SQL Database) 來利用雲端功能。 透過移至資料庫的 PaaS 服務，Contoso 將只需要維護資料，基礎平台則留給 Microsoft 負責。

### <a name="evaluate-migration-tools"></a>評估移轉工具

Contoso 主要是使用幾個 Azure 服務和工具來進行遷移：

- [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)：協調災害復原，並將內部部署 VM 遷移至 Azure。
- [Azure 資料庫移轉服務](https://docs.microsoft.com/azure/dms/dms-overview)：將 SQL Server、MySQL 和 Oracle 等內部部署資料庫遷移到 Azure。

#### <a name="azure-site-recovery"></a>Azure Site Recovery

在協調災害復原以及從 Azure 內進行的移轉，以及從內部部署網站至 Azure 的移轉時，Azure Site Recovery 會是主要的 Azure 服務。

1. Site Recovery 可讓您協調從內部部署網站複寫至 Azure。
2. 複寫在設定和執行時，內部部署機器可以容錯移轉至 Azure，以完成移轉。

Contoso 已[完成 POC](./contoso-migration-rehost-vm.md) 來了解 Site Recovery 如何協助他們遷移到雲端。

##### <a name="use-site-recovery-at-scale"></a>大規模使用 Site Recovery

Contoso 打算執行多次增益與轉移遷移。 為確保其可行性，Site Recovery 每批次會複寫大約 100 個 VM。 為了了解其運作方式，Contoso 必須針對建議的 Site Recovery 移轉進行容量規劃。

- Contoso 必須收集其流量大小的相關資訊。 尤其是：
  - Contoso 需要判斷其所要複寫 VM 的變動率。
  - Contoso 也需要將內部部署網站對 Azure 的網路連線能力納入考量。
- 為了回應容量和數量需求，Contoso 必須根據所需 VM 的每日資料變動率配置足夠的頻寬，以符合其復原點目標 (RPO)。
- 最後，他們必須了解需要多少部伺服器才能執行進行部署所需的 Site Recovery 元件。

###### <a name="gather-on-premises-information"></a>收集內部部署資訊

Contoso 可以使用 [Site Recovery 部署規劃工具](https://docs.microsoft.com/azure/site-recovery/site-recovery-deployment-planner)來完成下列步驟：

- Contoso 可以使用此工具來遠端剖析 VM，而不會對生產環境造成影響。 這有助於找出複寫和容錯移轉所需的頻寬和儲存體。
- Contoso 不需安裝任何 Site Recovery 內部部署元件，即可執行此工具。
- 此工具可收集相容與不相容 VM、每個 VM 的磁碟，以及每個磁碟的資料變換等相關資訊。 其也可識別網路頻寬需求，以及要成功進行複寫和容錯移轉所需的 Azure 基礎結構。
- Contoso 必須確認這一點，然後在符合 Site Recovery 組態伺服器最低需求的 Windows Server 機器上執行規劃工具。 組態伺服器是複寫內部部署 VMware VM 時所需的 Site Recovery 機器。

###### <a name="identify-site-recovery-requirements"></a>識別 Site Recovery 需求

除了正在複寫的 VM，Site Recovery 還需要幾個元件才能移轉 VMware。

<!--markdownlint-disable MD033 -->

**元件** | **詳細資料**
--- | ---
**組態伺服器** | 通常為使用 OVF 範本所設定的 VMware VM。<br/><br/> 組態伺服器元件會協調內部部署與 Azure 之間的通訊，以及管理資料複寫。
**處理序伺服器** | 預設會安裝在組態伺服器上。<br/><br/> 處理序伺服器元件負責接收複寫資料，以快取、壓縮和加密進行最佳化，然後將複寫資料傳送至 Azure 儲存體。<br/><br/> 處理序伺服器也會在您要複寫的 VM 上安裝 Azure Site Recovery 行動服務，並自動探索內部部署機器。<br/><br/> 大規模部署需要額外的獨立處理序伺服器，才能處理大量的複寫流量。
**行動服務** | 要使用 Site Recovery 遷移的每個 VMware VM 上都會安裝行動服務代理程式。

<!--markdownlint-enable MD033 -->

Contoso 必須了解如何根據容量考量來部署這些元件。

<!--markdownlint-disable MD033 -->

**元件** | **容量需求**
--- | ---
**最大每日變動率** | 單一處理序伺服器可以處理多達 2 TB 的每日變動率。 因為 VM 只能使用一部進程伺服器，所以複寫 VM 支援的每日資料變更率上限為 2 TB。
**最大輸送量** | 標準 Azure 儲存體帳戶每秒可處理最多 20,000 個要求，複寫中 VM 的每秒輸入/輸出作業 (IOPS) 數應保持在此限制內。 例如，如果某個 VM 有 5 個磁碟，且該 VM 上的每個磁碟會產生 120 IOPS (8K 大小)，則它會保持在 Azure 的每個磁碟 IOPS 限制 (500) 內。<br/><br/> 請注意，所需的儲存體帳戶數目等於來源機器 IOPS 總數除以 20,000。 複寫的機器只能屬於 Azure 中的單一儲存體帳戶。
**組態伺服器** | 根據 Contoso 所估計的一起複寫 100=200 VM 以及[組態伺服器調整大小需求](https://docs.microsoft.com/azure/site-recovery/site-recovery-plan-capacity-vmware#size-recommendations-for-the-configuration-server-and-inbuilt-process-server)，Contoso 估計值所需的組態伺服器機器如下：<br/><br/> CPU：16個 vcpu （2個&#215;通訊端8核心 @ 2.5 GHz）<br/><br/> 記憶體：32 GB<br/><br/> 快取磁碟：1 TB<br/><br/> 資料變動率：1 TB 至 2 TB。<br/><br/> 除了調整大小需求，Contoso 還必須確定該組態伺服器完美地位在和所要遷移 VM 相同的網路與 LAN 區段上。
**處理序伺服器** | Contoso 會部署獨立的專用處理序伺服器，並使其能夠複寫 100 至 200 個 VM：<br/><br/> CPU：16個 vcpu （2個&#215;通訊端8核心 @ 2.5 GHz）<br/><br/> 記憶體：32 GB<br/><br/> 快取磁碟：1 TB<br/><br/> 資料變動率：1 TB 至 2 TB。<br/><br/> 處理序伺服器會竭力運作，因此其位置應該落在 ESXi 主機上，以便應付複寫所需的磁碟 I/O、網路流量和 CPU。 為此，Contoso 會考慮使用專用主機。
**網路功能** | Contoso 已檢閱目前的站對站 VPN 基礎結構，並決定實作 Azure ExpressRoute。 此實作很重要，因為它會減少延遲，並提升通往 Contoso 主要美國東部 2 Azure 區域的頻寬。<br/><br/> **監視：** Contoso 將需要仔細監視來自進程伺服器的資料。 如果資料負載超過網路頻寬處理能力，Contoso 會考慮[將處理序伺服器的頻寬節流](https://docs.microsoft.com/azure/site-recovery/site-recovery-plan-capacity-vmware#control-network-bandwidth)。
**Azure 儲存體** | 為了移轉，Contoso 必須識別目標 Azure 儲存體帳戶的正確類型和數目。 Site Recovery 會將 VM 資料複寫至 Azure 儲存體。<br/><br/> Site Recovery 可以複寫至標準或進階的 (SSD) 儲存體帳戶。<br/><br/> 為了決定儲存體，Contoso 必須檢閱[儲存體限制](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types)，並考慮一段時間後的預期成長和使用量增加。 由於遷移的速度和優先順序，Contoso 決定使用 premium Ssd。<br/><br/>
Contoso 已決定對部署至 Azure 的所有 VM 使用受控磁碟。 所需的 IOPS 將會決定磁碟會是標準 HDD、標準 SSD 還是進階 (SSD)。<br/><br/>

<!--markdownlint-enable MD033 -->

#### <a name="azure-database-migration-service"></a>Azure 資料庫移轉服務

Azure 資料庫移轉服務是一個完全受控的服務，能夠從多個資料庫來源無縫移轉到 Azure 資料平台，將停機時間降到最低。

- DMS 會整合現有工具和服務的功能。 它會使用 Data Migration Assistant (DMA) 來產生評估報告，點出有關資料庫相容性的建議和任何必要的修改。
- DMS 會使用具有智慧評估功能的簡單自助式導引移轉程序，先解決潛在問題再進行移轉。
- DMS 可以大規模地從多種來源遷移到目標 Azure 資料庫。
- DMS 可提供從 SQL Server 2005 到 SQL Server 2017 的支援。

DMS 不是唯一的 Microsoft 資料庫移轉工具。 取得[工具和服務的比較](https://blogs.msdn.microsoft.com/datamigration/2017/10/13/differentiating-microsofts-database-migration-tools-and-services)。

##### <a name="use-dms-at-scale"></a>大規模使用 DMS

從 SQL Server 移轉時，Contoso 會使用 DMS。

- 在佈建 DMS 時，Contoso 必須正確設定其大小，而且設定為將資料移轉的效能最佳化。 Contoso 會選取「有 4 個虛擬核心的商務關鍵層」，從而讓服務利用多個 vCPU 來進行平行處理，提升資料轉送速度。

    ![DMS 規模調整](./media/contoso-migration-scale/dms.png)

- Contoso 的另一個規模調整策略，是在資料移轉期間暫時將 Azure SQL 或 MySQL 資料庫目標執行個體相應增加到進階層 SKU。 在使用較低層級的 SKU 時，這麼做可以降低資料庫節流對資料轉送活動的影響。

##### <a name="use-other-tools"></a>使用其他工具

除了 DMS，Contoso 還可以使用其他工具和服務來識別 VM 資訊。

- 有指令碼可協助進行手動移轉。 這些指令碼可於 GitHub 存放庫取得。
- 有許多[合作夥伴工具](https://azure.microsoft.com/migration/partners)也可用來移轉。

## <a name="phase-3-optimize"></a>階段 3：最佳化

Contoso 將資源移至 Azure 後，必須加以簡化以提高效能，同時使用成本管理工具來獲得最大 ROI。 由於 Azure 是按使用付費的服務，因此 Contoso 必須了解系統的效能，並確保其規模適當。

### <a name="azure-cost-management"></a>Azure 成本管理

為了充分利用其雲端投資，Contoso 會利用免費的 Azure 成本管理工具。

- 此授權解決方案由 Microsoft 的子公司 Cloudyn 所建置，可讓 Contoso 透明且準確地管理雲端支出。 它會提供工具來監視、配置及去除雲端成本。
- Azure 成本管理會提供簡單的儀表板報告，以協助進行成本配置、回報和退款。
- 成本管理可以識別未充分使用的資源，以便 Contoso 管理及調整，藉此最佳化雲端支出。
- [深入了解](https://docs.microsoft.com/azure/cost-management/overview) Azure 成本管理。

![成本管理](./media/contoso-migration-scale/cost-management.png)

### <a name="native-tools"></a>原生工具

Contoso 也會使用指令碼來找出未使用的資源。

- 大型移轉期間通常會遺留部分資料 (例如虛擬硬碟 (VHD))，這些遺留物會產生費用，卻未能對公司提供任何價值。 指令碼可於 GitHub 存放庫取得。
- Contoso 會利用 Microsoft IT 部門所完成的工作，並考慮實行 Azure 資源優化（ARO）工具組。
- Contoso 可以部署已在其訂用帳戶預先設定了 Runbook 和排程的 Azure 自動化帳戶，並開始節省成本。 在啟用或建立排程之後，訂用帳戶便會自動進行 Azure 資源最佳化，包括對新資源進行最佳化。
- 這可提供非集中式的自動化功能來降低成本。 功能包括：
  - 根據低 CPU 自動延遲 Azure VM。
  - 排程 Azure VM 來進行延遲和取消延遲。
  - 使用 Azure 標記以遞增和遞減順序排程 Azure VM 來進行延遲或取消延遲。
  - 依需求大量刪除資源群組。
- 您可以在此 [GitHub 存放庫](https://github.com/Azure/azure-quickstart-templates/tree/master/azure-resource-optimization-toolkit)開始使用 ARO 工具組。

### <a name="partner-optimization-tools"></a>合作夥伴最佳化工具

您可以使用 [Hanu](https://hanu.com/insight) 和 [Scalr]( https://www.scalr.com/cost-optimization) 等合作夥伴工具。

## <a name="phase-4-secure-and-manage"></a>第4階段：保護和管理

在此階段中，Contoso 會使用 Azure 安全性和管理資源，來管理、保護及監視 Azure 中的雲端應用程式。 這些資源可協助您執行安全且便於管理的環境，同時使用 Azure 入口網站所提供的產品。 Contoso 可以在移轉期間開始使用這些服務；透過 Azure 混合式支援，繼續使用許多服務來取得一致的混合式雲端體驗。

### <a name="security"></a>安全性

Contoso 會仰賴 Azure 資訊安全中心為混合式雲端工作負載提供統一的安全性管理和進階威脅防護。

- 資訊安全中心可讓您完整檢視並控制 Azure 雲端應用程式的安全性。
- Contoso 可以快速偵測威脅並採取動作來回應，以及藉由啟用調適性威脅防護來減少安全性風險。

[深入了解](https://azure.microsoft.com/services/security-center)資訊安全中心。

### <a name="monitoring"></a>監視

針對剛剛遷移的應用程式、基礎結構和目前執行 Azure 的資料，Contoso 需要了解其健康情況和效能。 Contoso 會使用內建的 Azure 雲端監視工具，例如 Azure 監視器、Log Analytics 工作區和 Application Insights。

- 透過這些工具，Contoso 可以輕鬆從來源收集資料並取得豐富見解。 例如，Contoso 可以測量 VM 的 CPU 磁碟和記憶體使用量、檢視多部 VM 之間的應用程式和網路相依性，並追蹤應用程式效能。
- Contoso 會使用這些雲端監視工具來採取行動，並與服務解決方案整合。
- [深入了解](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) Azure 監視。

### <a name="business-continuity-and-disaster-recovery"></a>商務持續性和災害復原

Contoso 必須針對其 Azure 資源備有商務持續性和災害復原 (BCDR) 策略。

- Azure 可提供[內建的 BCDR 功能](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications)，讓資料保持安全並讓應用程式/服務正常運作。
- 除了內建功能外，Contoso 還想要確保其可以從失敗中復原，避免耗費資源的業務中斷情況、符合合規性目標，以及保護資料免於遭受勒索軟體攻擊和人為錯誤。 作法：
  - Contoso 會部署 Azure 備份，以這個符合成本效益的解決方案來備份 Azure 資源。 因為它是內建的，所以 Contoso 可以用幾個簡單的步驟來設定雲端備份。
  - Contoso 會使用 Azure Site Recovery 來設定 Azure VM 的災害復原，以在 Azure 區域之間提供複寫、容錯移轉和容錯回復能力。 這種做法可確保 Azure VM 上執行的應用程式，能在主要區域發生中斷時在 Contoso 選擇的次要區域中繼續運作。 [詳細資訊](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart)。

## <a name="conclusion"></a>結論

在本文中，Contoso 已規劃了大規模的 Azure 移轉。 其將移轉程序分成四個階段。 從評估和移轉，到移轉完成後的最佳化、安全性與管理。 一般來說，在規劃移轉專案時務必要將其當作整個程序來處理，但在組織內遷移系統時，要將系統組合拆解成對企業有意義的分類和數字。 藉由評估資料並套用分類，專案可以拆解成一系列較小的移轉，執行時便可安全迅速。 這些較小移轉的總和很快就會變成成功的大型 Azure 移轉。
