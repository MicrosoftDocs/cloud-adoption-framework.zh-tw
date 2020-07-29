---
title: 對 Azure 進行大規模移轉
description: 使用適用于 Azure 的雲端採用架構，瞭解如何規劃和執行大規模的遷移至 Azure。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: azure-migrate
ms.openlocfilehash: adde78f5eddf1229f6b73cdf0dfafb618843072f
ms.sourcegitcommit: 65e8d2fc3ef31f2bb11a50f7c7a2d1eb116a6632
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/28/2020
ms.locfileid: "87254971"
---
<!-- docsTest:ignore ARO POC Y/N None/Some/Severe Rehost/Refactor/Rearchitect/Rebuild -->

<!-- cSpell:ignore VHDs autosnooze unsnooze Hanu Scalr -->

# <a name="scale-a-migration-to-azure"></a>對 Azure 進行大規模移轉

本文將示範虛構公司 Contoso 如何大規模執行移轉至 Azure 的工作。 其正在思考如何規劃和執行超過 3,000 個工作負載、8,000 個資料庫和超過 10,000 個 VM 的移轉。

## <a name="business-drivers"></a>商業動機

IT 領導小組與商務合作夥伴密切合作，以了解此次移轉所要實現的目標：

- **解決業務成長。** Contoso 的業務量日益增多，對內部部署系統和基礎結構造成了壓力。
- **提高效率。** Contoso 必須移除不必要的程序，並且簡化開發人員和使用者的程序。 企業亟需快速且不浪費時間或金錢的 IT 服務，進而更快滿足客戶的需求。
- **增加靈活性。** Contoso IT 必須能夠更快因應企業的需求。 其因應速度必須能夠比市場變化更快，才能更在全球經濟中獲致成功。 它不得礙事，或成為企業的絆腳石。
- **尺度.** 隨著企業順利成長，Contoso IT 小組必須提供能夠同步成長的系統。
- **改進成本模型。** Contoso 想要減少 IT 預算的資本適足要求。 Contoso 想要使用雲端功能來調整規模，並減少對於昂貴硬體的需要。
- **降低授權成本。** Contoso 想要將雲端成本降至最低。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已針對此次移轉擬定好各項目標。 並用這些目標用來判斷最合適的移轉方法。

| 需求 | 詳細資料 |
| --- | --- |
| 快速移至 Azure | Contoso 想要儘快開始將應用程式和 Vm 移至 Azure。 |
| 編譯完整的詳細目錄 | Contoso 想要完整清查組織中的所有應用程式、資料庫和 Vm。 |
| 評估和分類應用程式 | Contoso 想要充分利用雲端。 根據預設，Contoso 假設所有服務都會以 PaaS 的形式執行。 PaaS 不適合時，則會使用 IaaS。 |
| 訓練並移至 DevOps | Contoso 想要移至 DevOps 模型。 Contoso 會提供 Azure 與 DevOps 訓練，並視需要重組小組。 |

確立目標和需求後，Contoso 會檢閱 IT 磁碟使用量，並識別移轉程序。

## <a name="current-deployment"></a>目前的部署

在規劃和設定 [Azure 基礎結構](./contoso-migration-infrastructure.md)並試驗完上表所述的不同概念證明 (POC) 移轉組合之後，Contoso 已準備就緒，可以開始對 Azure 進行大規模的完整移轉。 Contoso 想要移轉的內容如下。

| 項目 | 磁碟區 | 詳細資料 |
| --- | --- | --- |
| 工作負載 | > 3000 應用程式 | <li> 應用程式會在 Vm 上執行。 <li> 應用程式平臺包括 Windows、SQL Server 和[燈泡](https://wikipedia.org/wiki/LAMP_(software_bundle))。 |
| 資料庫 | ~ 8500 資料庫 | 資料庫包括 SQL Server、MySQL 和于 postgresql。 |
| VM | > 35000 Vm | Vm 會在 VMware 主機上執行，並由 vCenter 伺服器管理。 |

## <a name="migration-process"></a>移轉程序

既然 Contoso 已將商務驅動程式和遷移目標保持在最下層，它可以符合[遷移方法](../index.md)，並建立遷移波浪和遷移短期衝刺的應用，以反復規劃和執行遷移工作。

## <a name="plan"></a>計畫

Contoso 會探索並評估內部部署應用程式、資料和基礎結構，以開始進行規劃流程。 以下是 Contoso 將採取的做法：

- Contoso 需要探索應用程式、對應應用程式之間的相依性，以及決定遷移順序和優先順序。
- 當 Contoso 評估時，它會建立應用程式和資源的完整清查。 除了新的清查，Contoso 還會使用並更新現有的設定管理資料庫（CMDB）和服務類別目錄。
  - CMDB 會保存 Contoso 應用程式的技術設定。
  - 服務類別目錄記載應用程式的作業詳細資料，包括相關聯的商業夥伴和服務等級協定（Sla）。

### <a name="discover-applications"></a>探索應用程式

Contoso 會在一系列伺服器上執行數千個應用程式。 除了 CMDB 和服務類別目錄以外，Contoso 還需要探索和評定工具。

- 這些工具必須有機制可將評估資料送入移轉程序中。
- 評估工具必須提供資料來協助建置 Contoso 實體和虛擬資源的智慧詳細目錄。 資料中應該包含設定檔資訊與效能計量。
- 探索完成時，Contoso 應該會有資產及其相關聯中繼資料的完整詳細目錄。 此詳細目錄會用來定義移轉方案。

### <a name="identify-classifications"></a>識別分類

Contoso 會識別一些常見類別來分類詳細目錄中的資產。 這些分類對於 Contoso 決定進行遷移而言非常重要。 分類清單可協助建立移轉優先順序，並找出複雜的問題。

| 類別 | 指派的值 | 詳細資料 |
| --- | --- | --- |
| 商務群組 | 商務群組名稱的清單 | 哪個群組負責詳細目錄項目？ |
| POC 候選項目 | Y/N | 應用程式是否可以做為雲端遷移的 POC 或早期採用者？ |
| 技術債務 | 無/一些/嚴重 | 清查專案執行或使用不支援的產品、平臺或作業系統？ |
| 防火牆影響 | Y/N | 應用程式是否會與網際網路或外部流量通訊？ 其是否與防火牆整合？ |
| 安全性問題 | Y/N | 應用程式是否有已知的安全性問題？ 應用程式使用未加密的資料或過期的平臺嗎？ |

### <a name="discover-application-dependencies"></a>探索應用程式相依性

在評估過程中，Contoso 必須找出應用程式的執行位置，並找出應用程式伺服器之間的相依性和連線。 Contoso 會透過幾個步驟來對應環境。

1. 在第一個步驟中，Contoso 會探索伺服器和電腦如何對應至個別的應用程式、網路位置和群組。
2. 透過這項資訊，Contoso 可以清楚地識別有少數相依性且適合快速遷移的應用程式。
3. Contoso 可以使用對應來協助識別更複雜的相依性，以及應用程式伺服器之間的通訊。 Contoso 接著可以將這些伺服器以邏輯方式分組以代表應用程式，並根據這些群組規劃遷移策略。

完成對應之後，Contoso 可以確保在建立遷移計畫時，識別並考慮所有應用程式元件。

![相依性對應](./media/contoso-migration-scale/dependency-map.png)

### <a name="evaluate-applications"></a>評估應用程式

作為探索和評估程式的最後一個步驟，Contoso 可以評估評估和對應結果，以瞭解如何在服務類別目錄中遷移每個應用程式。

為了擷取此評價程序，其在詳細目錄中新增了幾個額外的分類。

| 類別 | 指派的值 | 詳細資料 |
| --- | --- | --- |
| 商務群組 | 商務群組名稱的清單 | 哪個群組負責詳細目錄項目？ |
| POC 候選項目 | Y/N | 應用程式是否可以做為雲端遷移的 POC 或早期採用者？ |
| 技術債務 | 無/一些/嚴重 | 清查專案執行或使用不支援的產品、平臺或作業系統？ |
| 防火牆影響 | Y/N | 應用程式是否會與網際網路或外部流量通訊？ 其是否與防火牆整合？ |
| 安全性問題 | Y/N | 應用程式是否有已知的安全性問題？ 應用程式使用未加密的資料或過期的平臺嗎？ |
| 移轉策略 | 重新裝載/重構/重新架構/重建 | 應用程式需要何種遷移？ 如何將應用程式部署到 Azure？ [深入了解](./contoso-migration-overview.md#migration-patterns)。 |
| 技術複雜度 | 1-5 | 移轉有多複雜？ 此值應由 Contoso DevOps 和相關合作夥伴來定義。 |
| 業務關鍵性 | 1-5 | 商務應用程式有多重要？ 例如，小型工作組應用程式可能會被指派一個分數，而組織間使用的重要應用程式可能會被指派五個分數。 這個分數會影響移轉的優先順序高低。 |
| 移轉優先順序 | 1/2/3 | 應用程式的遷移優先順序為何？ |
| 移轉風險 | 1-5 | 遷移應用程式的風險層級為何？ 此值應由 Contoso DevOps 和相關合作夥伴來一致決定。 |

### <a name="determine-costs"></a>判斷成本

為了判斷成本和 Azure 遷移的可能節約，Contoso 可以使用[擁有權總成本（TCO）計算機](https://azure.microsoft.com/pricing/tco/calculator)來計算並比較 AZURE 的 TCO 與可比較的內部部署。

### <a name="identify-assessment-tools"></a>找出評估工具

Contoso 會決定要使用哪一種工具來進行探索、評估和詳細目錄建置。 Contoso 會識別 Azure 工具和服務、原生應用程式工具和腳本，以及合作夥伴工具的混合。 特別是，Contoso 想要知道如何使用 Azure Migrate 來進行大規模評估。

#### <a name="azure-migrate"></a>Azure Migrate

Azure Migrate 服務可協助您探索及評估內部部署 VMware VM，以做好移轉到 Azure 的準備。 Azure Migrate 的用途如下：

1. **探索：** 探索內部部署 VMware Vm。
    - Azure Migrate 支援從多部 vCenter server 進行探索（順序），而且可以在不同的 Azure Migrate 專案中執行探索。
    - Azure Migrate 會透過執行 Azure Migrate 收集器的 VMware VM 來執行探索。 相同的收集器也可探索不同 vCenter Server 上的 VM，並將資料傳送至不同專案。

2. **評估準備就緒：** 評估內部部署機器是否適合在 Azure 中執行。 評估項目包括：
    - **大小建議：** 根據內部部署 Vm 的效能歷程記錄，取得 Azure Vm 的大小建議。
    - **預估每月成本：** 取得在 Azure 中執行內部部署機器的估計成本。

3. **識別**相依性：將內部部署機器的相依性視覺化，以建立最佳的機器群組來進行評估和遷移。

![Azure Migrate](./media/contoso-migration-scale/azure-migrate.png)

**大規模遷移：**

Contoso 必須使用 Azure Migrate 來正確提供這項移轉的規模。

- Contoso 會使用 Azure Migrate 來對應用程式逐一進行評估。 這可確保 Azure Migrate 會將資料即時傳回 Azure 入口網站。
- Contoso 管理員會瞭解如何[大規模部署 Azure Migrate](https://docs.microsoft.com/azure/migrate/scale-hyper-v-assessment)。
- Contoso 在下表中摘要記下了 Azure Migrate 的限制。

| 動作 | 限制 |
| --- | --- |
| 建立 Azure Migrate 專案 | 10,000 個 VM |
| 探索 | 10,000 個 VM |
| 評量 | 10,000 個 VM |

Contoso 會按照下列方式使用 Azure Migrate：

- 在 vCenter 中，Contoso 會將 Vm 組織成資料夾。 這可方便他們在對特定資料夾中的 VM 執行評估時集中焦點。
- 評估電腦之間的相依性。 VM 上必須安裝代理程式才能進行評估。
  - Contoso 會使用自動化指令碼來安裝所需的 Windows 或 Linux 代理程式。
  - 透過編寫指令碼，Contoso 可以將安裝推送到 vCenter 資料夾內的 VM。

#### <a name="database-tools"></a>資料庫工具

除了 Azure Migrate，Contoso 還會聚焦在使用專為進行資料庫評估的工具。 [Data Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) 等工具有助於評估要移轉的 SQL Server 資料庫。

Data Migration Assistant （DMA）可協助 Contoso 找出內部部署資料庫是否與 azure 資料庫解決方案的範圍相容，例如 Azure SQL Database、在 Azure IaaS VM 上執行 SQL Server，以及 Azure SQL 受控執行個體。

除了資料庫移轉服務以外，Contoso 還有一些其他腳本，可用來探索和記錄 SQL Server 資料庫。 這些指令碼全都位於 GitHub 存放庫。

#### <a name="partner-assessment-tools"></a>合作夥伴評估工具

有數個其他合作夥伴工具可協助 Contoso 評估要遷移至 Azure 的內部部署環境。 深入瞭解[Azure 遷移合作夥伴](https://azure.microsoft.com/migration/partners)。

## <a name="phase-2-migrate"></a>階段 2：遷移

完成其評估之後，Contoso 必須找出可將應用程式、資料和基礎結構移至 Azure 的工具。

### <a name="migration-strategies"></a>移轉策略

Contoso 可以考慮四個廣泛的移轉策略。

| 策略 | 詳細資料 | 使用方式 |
| --- | --- | --- |
| 重新裝載 | <li> 這通常稱為「隨即_轉移_」，這是將現有應用程式快速遷移至 Azure 的無程式碼選項。 <li> 應用程式會以現有的方式遷移，並具有雲端的優點，而不會有任何與程式碼變更相關的風險或成本。 | <li> Contoso 可以重新裝載較不具策略性的應用程式，而不需要變更程式碼。 |
| 重構 | <li> 也稱為「重新封裝」，此策略需要最少的應用程式程式碼或設定變更，才能將應用程式連線至 Azure PaaS，並更充分利用雲端功能。 | <li> Contoso 可以重構策略性應用程式來保留相同的基本功能，但請將其移至在 Azure 平臺上執行，例如 Azure App Service。 <li> 此策略所需變更的程式碼最少。 <li> 但另一方面，Contoso 必須維護 VM 平台，因為 Microsoft 不會幫它管理。 |
| 重新架構 | <li> 此策略會修改或擴充應用程式的程式碼基底，以將應用程式架構優化以達到雲端功能和規模。 <li> 它會將應用程式將其現代化成彈性、可高度擴充且獨立部署的架構。 <li> Azure 服務可以加速程式、放心調整應用程式，並輕鬆管理應用程式。 |
| 重建 | <li> 這種方法會使用雲端原生技術從頭重建應用程式。 <li> Azure 平台即服務 (PaaS) 會在雲端提供完整的開發與部署環境。 它消除了軟體授權的部分費用和複雜度，並不再需要基礎應用程式基礎結構、中介軟體和其他資源。 | <li> Contoso 可以從頭重寫關鍵應用程式，以利用無伺服器計算或微服務等雲端技術。 <li> Contoso 會管理其開發的應用程式和服務，而 Azure 會管理所有其他專案。 |

資料也必須考慮近來，尤其是 Contoso 所擁有的資料庫數量。 Contoso 的預設方法是使用 PaaS 服務 (例如 Azure SQL Database) 來利用雲端功能。 透過移至資料庫的 PaaS 服務，Contoso 將只需要維護資料，基礎平台則留給 Microsoft 負責。

### <a name="evaluate-migration-tools"></a>評估移轉工具

Contoso 主要是使用幾個 Azure 服務和工具來進行遷移：

- ** [Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-services-overview)：** 將內部部署虛擬機器和其他資源遷移至 Azure 的服務。
- ** [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)：** 協調損毀修復，並將內部部署 Vm 遷移至 Azure。
- ** [Azure 資料庫移轉服務](https://docs.microsoft.com/azure/dms/dms-overview)：** 將內部部署資料庫（例如 SQL Server、MySQL 和 Oracle）遷移至 Azure。

<!-- markdownlint-disable MD024 -->

#### <a name="azure-migrate"></a>Azure Migrate

Azure Migrate 是主要的 Azure 服務，可協調從 Azure 內部和從內部部署網站到 Azure 的遷移。

1. Azure Migrate 會協調從內部部署位置到 Azure 的複寫。
2. 複寫在設定和執行時，內部部署機器可以容錯移轉至 Azure，以完成移轉。

Contoso 已[完成 POC](./contoso-migration-rehost-vm.md) ，以瞭解 Azure Migrate 如何協助他們遷移至雲端。

##### <a name="use-azure-migrate-at-scale"></a>大規模使用 Azure Migrate

Contoso 打算執行多個隨即轉移的遷移。 為了確保運作正常，Azure Migrate 會一次複寫大約100個 Vm 的批次。 為了判斷此功能的作用，Contoso 必須針對建議的遷移執行容量規劃。

- Contoso 必須收集其流量大小的相關資訊。 尤其是：
  - Contoso 需要判斷其所要複寫 VM 的變動率。
  - Contoso 也需要將內部部署網站對 Azure 的網路連線能力納入考量。
- 為了回應容量和數量需求，Contoso 必須根據所需 VM 的每日資料變動率配置足夠的頻寬，以符合其復原點目標 (RPO)。
- 最後，他們必須判斷執行部署所需之 Azure Migrate 元件所需的伺服器數目。

###### <a name="gather-on-premises-information"></a>收集內部部署資訊

Contoso 可以使用 Azure Migrate：

- Contoso 可以使用此工具來遠端剖析 VM，而不會對生產環境造成影響。 這有助於找出複寫和容錯移轉所需的頻寬和儲存體。
- Contoso 不需安裝任何 Site Recovery 內部部署元件，即可執行此工具。
- 此工具可收集相容與不相容 VM、每個 VM 的磁碟，以及每個磁碟的資料變換等相關資訊。 其也可識別網路頻寬需求，以及要成功進行複寫和容錯移轉所需的 Azure 基礎結構。
- Contoso 必須確保在符合 Site Recovery 設定伺服器最低需求的 Windows Server 電腦上執行 planner 工具。 組態伺服器是複寫內部部署 VMware VM 時所需的 Site Recovery 機器。

###### <a name="identify-site-recovery-requirements"></a>識別 Site Recovery 需求

除了正在複寫的 VM，Site Recovery 還需要幾個元件才能移轉 VMware。

| 元件 | 詳細資料 |
| --- | --- |
| 組態伺服器 | <li> 通常是使用 OVF 範本設定的 VMware VM。 <li> 組態伺服器元件會協調內部部署與 Azure 之間的通訊，以及管理資料複寫。 |
| 處理序伺服器 | <li> 預設會安裝在組態伺服器上。 <li> 進程伺服器元件會接收復寫資料;以快取、壓縮和加密進行優化;然後將它傳送至 Azure 儲存體。 <li> 進程伺服器也會在您想要複寫的 Vm 上安裝 Azure Site Recovery 行動服務，並執行內部部署機器的自動探索。 <li> 大規模部署需要額外的獨立處理序伺服器，才能處理大量的複寫流量。 |
| 行動服務 | <li> 行動服務代理程式會安裝在將使用 Azure Site Recovery 遷移的每個 VMware VM 上。 |

Contoso 必須了解如何根據容量考量來部署這些元件。

<br>

| 元件 | 容量需求 |
| --- | --- |
| 最大每日變動率 | <li> 單一處理序伺服器可以處理多達 2 TB 的每日變動率。 因為 VM 只能使用一部進程伺服器，所以複寫 VM 支援的每日資料變更率上限為 2 TB。 |
| 最大輸送量 | <li> 標準 Azure 儲存體帳戶每秒可處理最多20000個要求，而複寫 VM 上的每秒 i/o 作業數（IOPS）應在此限制內。 例如，如果某個 VM 有 5 個磁碟，且該 VM 上的每個磁碟會產生 120 IOPS (8K 大小)，則它會保持在 Azure 的每個磁碟 IOPS 限制 (500) 內。 <li> 所需的儲存體帳戶數目等於來源機器 IOPS 總數，除以20000。 複寫的機器只能屬於 Azure 中的單一儲存體帳戶。 |
| 組態伺服器 | 根據 Contoso 對複寫 100-200 Vm 的估計值，以及設定[伺服器的大小需求](https://docs.microsoft.com/azure/site-recovery/site-recovery-plan-capacity-vmware#size-recommendations-for-the-configuration-server-and-inbuilt-process-server)，contoso 估計需要設定伺服器電腦，如下所示： <li> CPU：16個 vcpu （2個通訊端 &#215; 8 核心 @ 2.5 GHz） <li> 記憶體：32 GB <li> 快取磁碟：1 TB <li> 資料變動率：1 TB 至 2 TB。 <br> 除了調整大小需求，Contoso 必須確保設定伺服器的最佳位置，是在與要遷移的 Vm 相同的網路和 LAN 區段上。 |
| 處理序伺服器 | Contoso 會部署獨立的專用處理序伺服器，並使其能夠複寫 100 至 200 個 VM： <li> CPU：16個 vcpu （2個通訊端 &#215; 8 核心 @ 2.5 GHz） <li> 記憶體：32 GB <li> 快取磁碟：1 TB <li> 資料變動率：1 TB 至 2 TB。 <br> 進程伺服器會正常運作，因此它位於 ESXi 主機上，可處理複寫所需的磁片 i/o、網路流量和 CPU。 為此，Contoso 會考慮使用專用主機。 |
| 網路功能 | <li> Contoso 已審查目前的站對站 VPN 基礎結構，並決定要執行 Azure ExpressRoute。 這是很重要的，因為它會降低延遲，並改善 Contoso 主要 Azure 區域（）的頻寬 `East US 2` 。 <li> Contoso 將需要仔細監視來自處理序伺服器的資料。 如果資料多載網路頻寬，Contoso 會考慮[節流處理伺服器頻寬](https://docs.microsoft.com/azure/site-recovery/site-recovery-plan-capacity-vmware#control-network-bandwidth)。 |
| Azure 儲存體 | <li> 若要進行遷移，Contoso 必須識別目標 Azure 儲存體帳戶的正確類型和數目。 Site Recovery 會將 VM 資料複寫至 Azure 儲存體。 <li> Site Recovery 可以複寫至標準或 premium SSD 儲存體帳戶。 <li> 若要決定儲存體，Contoso 必須檢查[儲存體限制](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types)，並考慮預期的成長和未來增加的使用量。 由於遷移的速度和優先順序，Contoso 決定使用 premium Ssd。 <li> Contoso 已決定針對部署至 Azure 的所有 Vm 使用受控磁片。 所需的 IOPS 有助於判斷磁片是否為標準 HDD、標準 SSD 或 premium SSD。 |

#### <a name="azure-database-migration-service"></a>Azure 資料庫移轉服務

Azure 資料庫移轉服務是完全受控的服務，可在停機時間最短的情況之下，從多個資料庫來源順暢地遷移至 Azure 資料平臺。

- 資料庫移轉服務整合了現有工具和服務的功能。 它會使用 Data Migration Assistant （DMA）來產生評估報告，以找出有關資料庫相容性和任何必要修改的建議。
- 資料庫移轉服務使用簡單的自我引導式遷移程式，並提供智慧型評估，協助解決遷移前的潛在問題。
- 資料庫移轉服務可以大規模地從多個來源遷移至目標 Azure 資料庫。
- 資料庫移轉服務提供從 SQL Server 2005 到 SQL Server 2017 的支援。

資料庫移轉服務並非唯一的 Microsoft 資料庫移轉工具。 取得[工具和服務的比較](https://blogs.msdn.microsoft.com/datamigration/2017/10/13/differentiating-microsofts-database-migration-tools-and-services)。

##### <a name="use-database-migration-service-at-scale"></a>大規模使用資料庫移轉服務

從 SQL Server 遷移時，Contoso 會使用資料庫移轉服務。

- 當布建資料庫移轉服務時，Contoso 必須正確地調整其大小，並將其設定為將資料移轉的效能優化。 Contoso 會選取「有 4 個虛擬核心的商務關鍵層」，從而讓服務利用多個 vCPU 來進行平行處理，提升資料轉送速度。

    ![資料庫移轉服務調整](./media/contoso-migration-scale/dms.png)

- Contoso 可以使用的另一個調整策略，是在資料移轉期間，暫時將 Azure SQL Database 或適用於 MySQL 的 Azure 資料庫目標實例相應增加至 Premium 定價層。 這可在使用較低層級時，最小化可能影響資料傳輸活動的資料庫節流。

##### <a name="use-other-tools"></a>使用其他工具

除了資料庫移轉服務以外，Contoso 還可以使用其他工具和服務來識別 VM 資訊。

- 有指令碼可協助進行手動移轉。 這些指令碼可於 GitHub 存放庫取得。
- 有許多[合作夥伴工具](https://azure.microsoft.com/migration/partners)也可用來移轉。

## <a name="ready-for-production"></a>備妥可用於生產環境

在 Contoso 將資源移至 Azure 之後，他們必須簡化它們以提升效能，並利用成本管理工具將 ROI 最大化。 由於 Azure 是按使用付費的服務，因此 Contoso 必須了解系統的效能，並確保其規模適當。

### <a name="azure-cost-management-and-billing"></a>Azure 成本管理和計費

為了充分利用雲端投資，Contoso 將使用免費的 Azure 成本管理和計費工具。

- 此解決方案可讓 Contoso 以透明度和精確度來管理雲端費用。 它會提供工具來監視、配置及去除雲端成本。
- Azure 成本管理和計費提供簡單的儀表板報告，以協助進行成本配置、回報和退款。
- Azure 成本管理和計費可以識別未充分使用的資源，讓 Contoso 可以管理及調整，藉此將雲端支出優化。
- 深入瞭解[Azure 成本管理和計費](https://docs.microsoft.com/azure/cost-management-billing/cost-management-billing-overview)。

![Azure 成本管理](./media/contoso-migration-scale/cost-management.png)

### <a name="native-tools"></a>原生工具

Contoso 也會使用指令碼來找出未使用的資源。

- 大型移轉期間通常會遺留部分資料 (例如虛擬硬碟 (VHD))，這些遺留物會產生費用，卻未能對公司提供任何價值。 指令碼可於 GitHub 存放庫取得。
- Contoso 會利用 Microsoft IT 部門所完成的工作，並考慮實行 Azure 資源優化（ARO）工具組。
- Contoso 可以部署已在其訂用帳戶預先設定了 Runbook 和排程的 Azure 自動化帳戶，並開始節省成本。 在啟用或建立排程之後，訂用帳戶便會自動進行 Azure 資源最佳化，包括對新資源進行最佳化。
- 這可提供非集中式的自動化功能來降低成本。 功能包括：
  - 根據低 CPU 使用率 Autosnooze Azure Vm。
  - 排程 Azure VM 來進行延遲和取消延遲。
  - 使用 Azure 標記以遞增和遞減順序排程 Azure VM 來進行延遲或取消延遲。
  - 依需求大量刪除資源群組。
- 透過此[GitHub](https://github.com/azure/azure-quickstart-templates/tree/master/azure-resource-optimization-toolkit)存放庫開始使用 Azure 資源優化（ARO）工具組。

### <a name="partner-optimization-tools"></a>合作夥伴最佳化工具

您可以使用 [Hanu](https://hanu.com/insight) 和 [Scalr](https://www.scalr.com/cost-optimization) 等合作夥伴工具。

## <a name="phase-4-secure-and-manage"></a>第4階段：保護和管理

在此階段中，Contoso 會使用 Azure 安全性和管理資源來管理、保護及監視 Azure 中的雲端應用程式。 這些資源可協助您執行安全且便於管理的環境，同時使用 Azure 入口網站所提供的產品。 Contoso 可以在移轉期間開始使用這些服務；透過 Azure 混合式支援，繼續使用許多服務來取得一致的混合式雲端體驗。

### <a name="security"></a>安全性

Contoso 會依賴 Azure 資訊安全中心進行統一的安全性管理，並 Azure 進階威脅防護跨混合式雲端工作負載。

- 此資訊安全中心可讓您在 Azure 中全面瞭解雲端應用程式的安全性，並對其進行控制。
- Contoso 可以快速偵測威脅並採取動作來回應，以及藉由啟用調適性威脅防護來減少安全性風險。

[深入了解](https://azure.microsoft.com/services/security-center)資訊安全中心。

### <a name="monitoring"></a>監視

Contoso 需要能夠看到新遷移的應用程式、基礎結構，以及目前執行 Azure 的資料的健全狀況和效能。 Contoso 會使用內建的 Azure 雲端監視工具，例如 Azure 監視器、Log Analytics 工作區和 Application Insights。

- Contoso 可以使用這些工具，輕鬆地從來源收集資料，並取得豐富的見解。 例如，Contoso 可以測量 VM 的 CPU 磁碟和記憶體使用量、檢視多部 VM 之間的應用程式和網路相依性，並追蹤應用程式效能。
- Contoso 會使用這些雲端監視工具來採取行動，並與服務解決方案整合。
- 深入瞭解[Azure 監視](https://docs.microsoft.com/azure/azure-monitor/overview)。

### <a name="business-continuity-and-disaster-recovery"></a>商務持續性和災害復原

Contoso 必須針對其 Azure 資源備有商務持續性和災害復原 (BCDR) 策略。

- Azure 提供[內建的 BCDR 功能](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications)來保護資料，並讓應用程式和服務保持執行狀態。
- 除了內建功能外，Contoso 還想要確保其可以從失敗中復原，避免耗費資源的業務中斷情況、符合合規性目標，以及保護資料免於遭受勒索軟體攻擊和人為錯誤。 作法：
  - Contoso 會部署 Azure 備份，以這個符合成本效益的解決方案來備份 Azure 資源。 因為它是內建的，所以 Contoso 可以用幾個簡單的步驟來設定雲端備份。
  - Contoso 會使用 Azure Site Recovery 來設定 Azure VM 的災害復原，以在 Azure 區域之間提供複寫、容錯移轉和容錯回復能力。 這可確保在主要區域發生中斷時，在 Contoso 選擇的次要區域中，執行于 Azure Vm 上的應用程式仍可供使用。 [深入了解](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart)。

## <a name="conclusion"></a>結論

在本文中，Contoso 已規劃了大規模的 Azure 移轉。 其將移轉程序分成四個階段。 從評估和移轉，到移轉完成後的最佳化、安全性與管理。 在大部分的情況下，請務必將遷移專案規劃為整個程式，但將集合細分成符合企業效益的分類和數位，以遷移組織中的系統。 藉由評估資料並套用分類，專案可以拆解成一系列較小的移轉，執行時便可安全迅速。 這些較小移轉的總和很快就會變成成功的大型 Azure 移轉。
