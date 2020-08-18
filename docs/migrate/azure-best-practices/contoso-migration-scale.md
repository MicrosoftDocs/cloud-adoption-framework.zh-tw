---
title: 對 Azure 進行大規模移轉
description: 使用適用于 Azure 的雲端採用架構，瞭解如何規劃和執行大規模遷移至 Azure。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: azure-migrate
ms.openlocfilehash: 38a2c43d5240dead7d545a406c3b8f9430736ca0
ms.sourcegitcommit: 917188fa930cadddb03f9e9bbcdd7b630e4ee33e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/17/2020
ms.locfileid: "88281659"
---
<!-- docsTest:ignore ARO POC Y/N None/Some/Severe Rehost/Refactor/Rearchitect/Rebuild -->

<!-- cSpell:ignore VHDs autosnooze unsnooze Hanu Scalr -->

# <a name="scale-a-migration-to-azure"></a>對 Azure 進行大規模移轉

本文將示範虛構公司 Contoso 如何大規模執行移轉至 Azure 的工作。 該公司會考慮如何規劃和執行超過3000個工作負載、8000資料庫，以及10000虛擬機器 (Vm) 的遷移。

## <a name="business-drivers"></a>商業動機

IT 領導小組與商務合作夥伴密切合作，以了解此次移轉所要實現的目標：

- **解決業務成長。** Contoso 的業務量日益增多，對內部部署系統和基礎結構造成了壓力。
- **提高效率。** Contoso 必須移除不必要的程序，並且簡化開發人員和使用者的程序。 企業亟需快速且不浪費時間或金錢的 IT 服務，進而更快滿足客戶的需求。
- **增加靈活性。** Contoso IT 必須能夠更快因應企業的需求。 它必須能夠以更快的速度回應 marketplace 中的變更，以實現全球經濟的成功。 它不得取得或成為企業封鎖程式。
- **規模。** 隨著企業順利成長，Contoso IT 小組必須提供能夠同步成長的系統。
- **改進成本模型。** Contoso 想要減少 IT 預算的資本適足要求。 Contoso 想要使用雲端功能來調整規模，並減少對於昂貴硬體的需要。
- **降低授權成本。** Contoso 想要將雲端成本降至最低。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已針對此次移轉擬定好各項目標。 它會使用這些目標來判斷最佳的遷移方法。

| 需求 | 詳細資料 |
| --- | --- |
| 快速移至 Azure | Contoso 希望儘快開始將應用程式和 Vm 移至 Azure。 |
| 編譯完整的詳細目錄 | Contoso 想要完整清查組織中的所有應用程式、資料庫和 Vm。 |
| 評定和分類應用程式 | Contoso 想要充分利用雲端。 根據預設，Contoso 會假設所有服務都是以「平臺即服務」的形式執行， (PaaS) 。 在 PaaS 不適用的情況下，將會使用 (IaaS) 的基礎結構即服務。 |
| 訓練並移至 DevOps | Contoso 想要移至 DevOps 模型。 Contoso 將提供 Azure 和 DevOps 訓練，並且會視需要重新組織團隊。 |

在建立目標和需求之後，Contoso 會審核 IT 使用量，並識別遷移程式。

## <a name="current-deployment"></a>目前的部署

Contoso 已規劃並設定 [Azure 基礎結構](./contoso-migration-infrastructure.md) ，並嘗試進行不同的概念證明 (POC) 遷移組合（如上表所述）。 現在已準備好步屬於大規模的完整遷移至 Azure。 Contoso 想要移轉的內容如下。

| Item | 磁碟區 | 詳細資料 |
| --- | --- | --- |
| 工作負載 | > 3000 應用程式 | <li> 應用程式會在 Vm 上執行。 <li> 應用程式平臺包括 Windows、SQL Server 和 [燈泡](https://wikipedia.org/wiki/LAMP_(software_bundle))。 |
| 資料庫 | 大約8500資料庫 | 資料庫包含 SQL Server、MySQL 和于 postgresql。 |
| VM | > 35000 Vm | Vm 會在 VMware 主機上執行，並由 vCenter server 管理。 |

## <a name="migration-process"></a>移轉程序

Contoso 現在已建立商務驅動程式和遷移目標，可與 [遷移方法](../index.md)一致。 它可以根據遷移波和遷移短期衝刺的應用程式來進行，以反復規劃和執行遷移工作。

## <a name="plan"></a>計畫

Contoso 藉由探索及評估內部部署應用程式、資料和基礎結構，開始進行規劃程式。 以下是 Contoso 將採取的做法：

- Contoso 需要探索應用程式、在應用程式之間對應相依性，以及決定遷移順序和優先順序。
- Contoso 在進行評估時，會建立應用程式和資源的完整清查。 除了新的清查，Contoso 還會使用並更新這些現有的專案：
  - Configuration management 資料庫 (CMDB) 。 它會保存 Contoso 應用程式的技術設定。
  - 服務類別目錄。 它記載了應用程式的操作詳細資料，包括相關聯的業務夥伴和服務等級協定。

### <a name="discover-applications"></a>探索應用程式

Contoso 會在一系列伺服器上執行數千個應用程式。 除了 CMDB 和服務類別目錄之外，Contoso 還需要探索和評量工具。

這些工具必須有機制可將評估資料送入移轉程序中。 評估工具必須提供資料來協助建置 Contoso 實體和虛擬資源的智慧詳細目錄。 資料應該包含設定檔資訊和效能計量。

當探索完成時，Contoso 應該擁有資產的完整清查和與其相關聯的中繼資料。 該公司將使用此清查來定義遷移計畫。

### <a name="identify-classifications"></a>識別分類

Contoso 會識別一些常見類別來分類詳細目錄中的資產。 這些分類對 Contoso 決策進行遷移至關重要。 分類清單可協助您建立遷移優先權，並識別複雜的問題。

| 類別 | 指派的值 | 詳細資料 |
| --- | --- | --- |
| 商務群組 | 商務群組名稱的清單 | 哪個群組負責詳細目錄項目？ |
| POC 候選項目 | Y/N | 應用程式是否可以作為 POC 或早期採用者進行雲端遷移？ |
| 技術債務 | 無/一些/嚴重 | 清查專案是執行中或使用不支援的產品、平臺或作業系統？ |
| 防火牆影響 | Y/N | 應用程式是否與網際網路或外部流量通訊？ 其是否與防火牆整合？ |
| 安全性問題 | Y/N | 應用程式是否有已知的安全性問題？ 應用程式使用未加密的資料或過期的平臺嗎？ |

### <a name="discover-application-dependencies"></a>探索應用程式相依性

在評估過程中，Contoso 需要找出應用程式的執行位置。 它也需要找出應用程式伺服器之間的相依性和連接。 Contoso 會在下列步驟中對應環境：

1. Contoso 會探索伺服器和機器如何對應到個別的應用程式、網路位置和群組。
2. Contoso 清楚地識別有一些相依性且適合快速遷移的應用程式。
3. Contoso 可以使用對應來協助識別更複雜的相依性，以及應用程式伺服器之間的通訊。 接著，Contoso 可以用邏輯方式將這些伺服器分組以代表應用程式，並根據這些群組規劃遷移策略。

完成對應後，Contoso 可以確保在建立遷移計畫時，會識別出所有的應用程式元件並加以考慮。

![相依性對應的圖表。](./media/contoso-migration-scale/dependency-map.png)

### <a name="evaluate-applications"></a>評估應用程式

在探索和評估程式的最後一個步驟中，Contoso 可以評估評量和對應結果，以找出如何在服務類別目錄中遷移每個應用程式。

為了抓住此評估程式，Contoso 在清查中新增了幾個分類。

| 類別 | 指派的值 | 詳細資料 |
| --- | --- | --- |
| 商務群組 | 商務群組名稱的清單 | 哪個群組負責詳細目錄項目？ |
| POC 候選項目 | Y/N | 應用程式是否可以作為 POC 或早期採用者進行雲端遷移？ |
| 技術債務 | 無/一些/嚴重 | 清查專案是執行中或使用不支援的產品、平臺或作業系統？ |
| 防火牆影響 | Y/N | 應用程式是否與網際網路或外部流量通訊？ 其是否與防火牆整合？ |
| 安全性問題 | Y/N | 應用程式是否有已知的安全性問題？ 應用程式使用未加密的資料或過期的平臺嗎？ |
| 移轉策略 | 重新裝載/重構/重新架構/重建 | 應用程式需要何種遷移？ 如何將應用程式部署到 Azure？ [深入了解](./contoso-migration-overview.md#migration-patterns)。 |
| 技術複雜度 | 1-5 | 移轉有多複雜？ 此值應由 Contoso DevOps 和相關合作夥伴來定義。 |
| 業務關鍵性 | 1-5 | 商務應用程式的重要性為何？ 例如，小型工作組應用程式可能會被指派一個分數，而跨組織使用的重要應用程式可能會被指派五個分數。 這個分數會影響移轉的優先順序高低。 |
| 移轉優先順序 | 1/2/3 | 應用程式的遷移優先順序為何？ |
| 移轉風險 | 1-5 | 遷移應用程式的風險層級為何？ Contoso DevOps 和相關合作夥伴應同意此值。 |

### <a name="determine-costs"></a>判斷成本

為了判斷 Azure 遷移的成本和可能節省的成本，Contoso 可以使用 [擁有權總成本 (TCO) 計算機](https://azure.microsoft.com/pricing/tco/calculator) 來計算 AZURE 的 tco，並將其與可比較的內部部署進行比較。

### <a name="identify-assessment-tools"></a>找出評估工具

Contoso 會決定要使用哪一種工具來進行探索、評估和詳細目錄建置。 Contoso 會識別 Azure 工具和服務、原生應用程式工具和腳本，以及合作夥伴工具的混合。 特別是，Contoso 想要知道如何使用 Azure Migrate 來進行大規模評估。

#### <a name="azure-migrate"></a>Azure Migrate

Azure Migrate 服務可協助您探索及評估內部部署 VMware VM，以做好移轉到 Azure 的準備。 Azure Migrate 的用途如下：

1. **探索：** 探索內部部署 VMware Vm。

   Azure Migrate 支援以順序) 從多部 vCenter (server 進行探索，並可在不同的 Azure Migrate 專案中執行探索。

   Azure Migrate 透過執行 Azure Migrate 收集器的 VMware VM 來執行探索。 相同的收集器可以探索不同 vCenter server 上的 Vm，並將資料傳送至不同的專案。

2. **評估準備工作：** 評估內部部署機器是否適合在 Azure 中執行。 評量包含：
   - **大小建議：** 根據內部部署 Vm 的效能歷程記錄，取得 Azure Vm 的大小建議。
   - **預估每月成本：** 取得在 Azure 中執行內部部署機器的估計成本。

3. **識別** 相依性：將內部部署機器的相依性視覺化，以建立最佳的電腦群組以進行評量和遷移。

![Azure Migrate 服務如何運作的圖表。](./media/contoso-migration-scale/azure-migrate.png)

根據此遷移的規模，Contoso 必須正確地使用 Azure Migrate：

- Contoso 會使用 Azure Migrate 來執行應用程式對應用程式的評量。 這種評量可確保 Azure Migrate 將及時的資料傳給 Azure 入口網站。
- Contoso 管理員瞭解如何 [大規模部署 Azure Migrate](/azure/migrate/scale-hyper-v-assessment)。
- Contoso 在下表中摘要記下了 Azure Migrate 的限制。

| 動作 | 限制 |
| --- | --- |
| 建立 Azure Migrate 專案 | 10,000 個 VM |
| 探索 | 10,000 個 VM |
| 評量 | 10,000 個 VM |

Contoso 會按照下列方式使用 Azure Migrate：

- 在 vCenter 中，將 Vm 組織成資料夾。 這可讓系統管理員在對特定資料夾中的 Vm 執行評量時，輕鬆地將焦點放在一起。
- 評估機器之間的相依性。 VM 上必須安裝代理程式才能進行評估。
  
  Contoso 會使用自動化指令碼來安裝所需的 Windows 或 Linux 代理程式。 透過編寫指令碼，Contoso 可以將安裝推送到 vCenter 資料夾內的 VM。

#### <a name="database-tools"></a>資料庫工具

除了 Azure Migrate，Contoso 還會聚焦在使用專為進行資料庫評估的工具。 [Data Migration Assistant](/sql/dma/dma-overview?view=sql-server-2017)等工具將有助於評估 SQL Server 資料庫以進行遷移。

Data Migration Assistant 可協助 Contoso 找出內部部署資料庫是否與一系列的 Azure 資料庫解決方案相容。 這些解決方案包括 Azure SQL Database、SQL Server 在 Azure IaaS VM 上執行，以及 Azure SQL 受控執行個體。

除了資料庫移轉服務，Contoso 還有一些腳本，用來探索和記錄 SQL Server 資料庫。 這些腳本位於 GitHub 存放庫中。

#### <a name="partner-assessment-tools"></a>合作夥伴評估工具

有幾個其他合作夥伴工具可協助 Contoso 評估內部部署環境，以遷移至 Azure。 深入瞭解 [Azure 遷移合作夥伴](https://azure.microsoft.com/migration/partners)。

## <a name="phase-2-migrate"></a>階段 2：遷移

完成評量之後，Contoso 需要找出可將其應用程式、資料和基礎結構移至 Azure 的工具。

### <a name="migration-strategies"></a>移轉策略

Contoso 可以考慮採用四種廣泛的遷移策略。

| 策略 | 詳細資料 | 使用量 |
| --- | --- | --- |
| 重新裝載 | <li> 這通常稱為「隨即 _轉移_ 」，這是可讓您快速將現有應用程式遷移至 Azure 的無程式碼選項。 <li> 應用程式會以雲端的優點進行遷移，而不會有與程式碼變更相關聯的風險或成本。 | <li> Contoso 可以重新裝載不需要變更程式碼的較不具策略性的應用程式。 |
| 重構 | <li> 也稱為重新封裝，此策略需要最基本的應用程式程式碼或設定 *變更，以*將應用程式連線至 Azure PaaS，並利用雲端功能。 | <li> Contoso 可以重構策略性應用程式來保留相同的基本功能，但請將其移到 Azure 平臺上執行（例如 Azure App Service）。 <li> 此策略所需變更的程式碼最少。 <li> 另一方面，Contoso 必須維持 VM 平臺，因為 Microsoft 不會管理這一點。 |
| 重新架構 | <li> 此策略會修改或擴充應用程式的程式碼基底，以優化雲端功能和規模的應用程式架構。 <li> 它會將應用程式將其現代化至彈性、可高度擴充、可獨立部署的架構。 <li> Azure 服務可以加速程式、安心地調整應用程式，並輕鬆管理應用程式。 |
| 重建 | <li> 這種方法會使用雲端原生技術從頭重建應用程式。 <li> Azure PaaS 在雲端提供完整的開發和部署環境。 它消除了軟體授權的一些費用和複雜度。 此外，也不再需要基礎應用程式基礎結構、中介軟體和其他資源。 | <li> Contoso 可以重寫重要的應用程式，以利用無伺服器計算或微服務等雲端技術。 <li> Contoso 將會管理它所開發的應用程式和服務，而 Azure 會管理其他所有專案。 |

資料也必須考慮近來，尤其是 Contoso 所擁有的資料庫數量。 Contoso 的預設方法是使用 PaaS 服務 (例如 Azure SQL Database) 來利用雲端功能。 藉由移至資料庫的 PaaS 服務，Contoso 只需要維護資料。 它會將基礎平臺保留給 Microsoft。

### <a name="evaluate-migration-tools"></a>評估移轉工具

Contoso 主要使用這些 Azure 服務和工具來進行遷移：

- [Azure Migrate](/azure/migrate/migrate-services-overview)：將內部部署虛擬機器和其他資源遷移至 Azure 的服務。
- [Azure 資料庫移轉服務](/azure/dms/dms-overview)：將 SQL Server、MySQL 和 Oracle 等內部部署資料庫遷移到 Azure。

<!-- markdownlint-disable MD024 -->

#### <a name="azure-migrate"></a>Azure Migrate

Azure Migrate 是可協調從 Azure 和內部部署網站遷移至 Azure 的主要 Azure 服務。

Azure Migrate 會協調從內部部署位置到 Azure 的複寫。 複寫在設定和執行時，內部部署機器可以容錯移轉至 Azure，以完成移轉。

Contoso 已經 [完成 POC](./contoso-migration-rehost-vm.md) ，瞭解 Azure Migrate 如何協助 it 遷移至雲端。

##### <a name="use-azure-migrate-at-scale"></a>大規模使用 Azure Migrate

Contoso 計畫執行多個隨即轉移的遷移。 為了確保運作正常，Azure Migrate 會一次複寫大約100個 Vm 的批次。 若要判斷此作業的運作方式，Contoso 必須針對建議的遷移執行容量規劃。

Contoso 必須收集其流量大小的相關資訊。 尤其是：

- 它必須決定要複寫之 Vm 的變更速率。
- 它需要從內部部署網站到 Azure 的網路連線能力。

為了回應容量和數量需求，Contoso 必須根據所需 VM 的每日資料變動率配置足夠的頻寬，以符合其復原點目標 (RPO)。 最後，它必須決定需要多少伺服器來執行部署的 Azure Migrate 元件。

##### <a name="gather-on-premises-information"></a>收集內部部署資訊

Contoso 可以使用 Azure Migrate：

- 從遠端分析 Vm，而不會影響生產環境。 這有助於找出複寫和容錯移轉所需的頻寬和儲存體。
- 不需要在內部部署環境中安裝任何 Site Recovery 元件。

此工具可收集相容與不相容 VM、每個 VM 的磁碟，以及每個磁碟的資料變換等相關資訊。 它也會識別網路頻寬需求，以及成功複寫和容錯移轉所需的 Azure 基礎結構。

Contoso 必須確保它會在符合 Site Recovery 設定伺服器最低需求的 Windows Server 電腦上執行規劃工具。 設定伺服器是複寫內部部署 VMware Vm 所需的 Site Recovery 機。

##### <a name="identify-site-recovery-requirements"></a>識別 Site Recovery 需求

除了正在複寫的 VM，Site Recovery 還需要幾個元件才能移轉 VMware。

| 元件 | 詳細資料 |
| --- | --- |
| 組態伺服器 | <li> 通常是透過 OVF 範本設定的 VMware VM。 <li> 設定伺服器元件會協調內部部署與 Azure 之間的通訊，而且會管理資料複寫。 |
| 處理序伺服器 | <li> 預設會安裝在組態伺服器上。 <li> 進程伺服器元件接收復寫資料;使用快取、壓縮和加密進行優化;然後將它傳送至 Azure 儲存體。 <li> 進程伺服器也會在您要複寫的 Vm 上安裝 Azure Site Recovery 行動服務，並執行內部部署機器的自動探索。 <li> 大規模部署需要額外的獨立處理序伺服器，才能處理大量的複寫流量。 |
| 行動服務 | <li> 行動服務代理程式安裝在將使用 Azure Site Recovery 遷移的每個 VMware VM 上。 |

Contoso 必須了解如何根據容量考量來部署這些元件。

<br>

| 元件 | 容量需求 |
| --- | --- |
| 最大每日變動率 | <li> 單一進程伺服器可處理的每日變更率最多為 2 tb (TB) 。 因為 VM 只能使用一部進程伺服器，所以複寫 VM 支援的每日資料變更率上限為 2 TB。 |
| 最大輸送量 | <li> 一個標準「Azure 儲存體」帳戶每秒最多可以處理 20,000 個要求。 跨複寫 VM 的每秒 i/o 作業 (IOPS) 應該在此限制內。 例如，如果 VM 有5個磁片，而每個磁片產生 120 IOPS (在 VM 上) 的8K 大小，則會在 Azure 每個磁片的 IOPS 限制（500）內。 <li> 所需的儲存體帳戶數目等於來源機器 IOPS 總數除以20000。 複寫的機器只能屬於 Azure 中的單一儲存體帳戶。 |
| 組態伺服器 | Contoso 會根據 Contoso 估計將100複寫至 200 Vm，以及設定 [伺服器的大小需求](/azure/site-recovery/site-recovery-plan-capacity-vmware#size-recommendations-for-the-configuration-server-and-inbuilt-process-server)，來估計它需要下列類型的伺服器機器設定： <li> CPU：16個 vcpu (2 個通訊端 &#215; 8 核心 @ 2.5 GHz)  <li> 記憶體：32 GB <li> 快取磁碟：1 TB <li> 資料變更率：1到 2 TB <br> 除了調整大小需求，Contoso 還必須確保設定伺服器以最佳的方式位於與要遷移之 Vm 相同的網路與 LAN 區段上。 |
| 處理序伺服器 | Contoso 會部署獨立的專用進程伺服器，並能夠將100複寫至 200 Vm： <li> CPU：16個 vcpu (2 個通訊端 &#215; 8 核心 @ 2.5 GHz)  <li> 記憶體：32 GB <li> 快取磁碟：1 TB <li> 資料變更率：1到 2 TB <br> 進程伺服器將會運作困難，因此它位於 ESXi 主機上，可處理複寫所需的磁片 i/o、網路流量和 CPU。 為此，Contoso 會考慮使用專用主機。 |
| 網路功能 | <li> Contoso 已審核目前的站對站 VPN 基礎結構，並決定要執行 Azure ExpressRoute。 這是很重要的，因為它會降低延遲，並提升 Contoso 主要 Azure 區域 (`East US 2`) 的頻寬。 <li> Contoso 將需要仔細監視來自處理序伺服器的資料。 如果資料多載網路頻寬，Contoso 會考慮 [節流進程伺服器頻寬](/azure/site-recovery/site-recovery-plan-capacity-vmware#control-network-bandwidth)。 |
| Azure 儲存體 | <li> 若要進行遷移，Contoso 必須識別正確的目標 Azure 儲存體帳戶類型和數目。 Site Recovery 將 VM 資料複寫至 Azure 儲存體。 <li> Site Recovery 可以複寫至標準或 premium SSD 儲存體帳戶。 <li> 若要決定儲存體，Contoso 必須檢查 [儲存體限制](/azure/virtual-machines/windows/disks-types) ，並考慮預期的成長和未來的使用量增加。 由於遷移的速度和優先順序，Contoso 決定使用 premium Ssd。 <li> Contoso 已決定使用受控磁片來部署至 Azure 的所有 Vm。 所需的 IOPS 將有助於判斷磁片是否為標準 HDD、標準 SSD 或 premium SSD。 |

#### <a name="azure-database-migration-service"></a>Azure 資料庫移轉服務

Azure 資料庫移轉服務是一項完全受控的服務，可讓您從多個資料庫來源順暢地遷移到 Azure 資料平臺，並減少停機時間。 以下是有關服務的一些詳細資料：

- 它會整合現有工具和服務的功能。 它會使用 Data Migration Assistant 來產生評估報表，以找出有關資料庫相容性及任何必要修改的建議。
- 它使用簡單的自我引導式遷移程式與智慧型評量，可在遷移前協助解決潛在的問題。
- 它可以從多個來源大規模遷移到目標 Azure 資料庫。
- 它提供從 SQL Server 2005 到 SQL Server 2017 的支援。

資料庫移轉服務並非唯一的 Microsoft 資料庫移轉工具。 取得[工具和服務的比較](https://blogs.msdn.microsoft.com/datamigration/2017/10/13/differentiating-microsofts-database-migration-tools-and-services)。

##### <a name="use-database-migration-service-at-scale"></a>大規模使用資料庫移轉服務

從 SQL Server 進行遷移時，Contoso 會使用資料庫移轉服務。

布建資料庫移轉服務時，Contoso 需要正確調整其大小，並將它設定為將資料移轉的效能優化。 Contoso 會選取具有四個虛擬核心的業務關鍵層。 此選項可讓服務利用多重個 vcpu 進行平行處理，並加快資料傳送速率。

![顯示 [資料庫移轉服務調整] 的螢幕擷取畫面，其中已選取 [商務關鍵] 選項。](./media/contoso-migration-scale/dms.png)

Contoso 可以使用的另一個調整策略是在資料移轉期間，暫時將 Azure SQL Database 或適用於 MySQL 的 Azure 資料庫目標實例擴大至 Premium 定價層。 當組織使用較低的層級時，這會將可能影響資料傳輸活動的資料庫節流降至最低。

##### <a name="use-other-tools"></a>使用其他工具

除了資料庫移轉服務，Contoso 還可以使用其他工具和服務來識別 VM 資訊：

- 協助手動遷移的腳本。 這些指令碼可於 GitHub 存放庫取得。
- 用於遷移的各種 [合作夥伴工具](https://azure.microsoft.com/migration/partners) 。

## <a name="ready-for-production"></a>準備好用於生產環境

Contoso 將資源移至 Azure 之後，需要簡化這些資源以改善效能，並利用成本管理工具將投資率最大化。 因為 Azure 是使用付費服務，所以 Contoso 必須瞭解系統的執行情況，並確保其大小適當。

### <a name="azure-cost-management-and-billing"></a>Azure 成本管理和計費

為了充分運用雲端投資，Contoso 會利用免費的 Azure 成本管理和計費工具。 此解決方案可讓 Contoso 以透明和精確度來管理雲端費用。 它會提供工具來監視、配置及去除雲端成本。

Azure 成本管理和帳單提供簡單的儀表板報告，以協助進行成本配置、回報和退款。 此工具可找出可供 Contoso 管理及調整的使用量過低的資源，以協助將雲端費用優化。

您可以在 [Azure 成本管理和帳單的總覽](/azure/cost-management-billing/cost-management-billing-overview)中深入瞭解。

![Azure 成本管理中一段時間的成本圖表範例螢幕擷取畫面。](./media/contoso-migration-scale/cost-management.png)

### <a name="native-tools"></a>原生工具

Contoso 也會使用指令碼來找出未使用的資源。

在大型遷移期間，通常會有剩餘的資料片段，例如虛擬硬碟，這會產生費用，但不會對公司提供任何價值。 腳本可在 [GitHub](https://github.com/azure/azure-quickstart-templates/tree/master/azure-resource-optimization-toolkit)存放庫中取得。

Contoso 會利用 Microsoft IT 部門所完成的工作，並考慮將 Azure 資源優化 (ARO) 工具組。 此工具組也會在 GitHub 存放庫中。

Contoso 可以部署已在其訂用帳戶預先設定了 Runbook 和排程的 Azure 自動化帳戶，並開始節省成本。 在啟用或建立排程之後，訂用帳戶會自動進行資源優化，包括新資源的優化。 這可提供非集中式的自動化功能來降低成本。 功能包括：

- 根據低 CPU 使用率 Autosnooze Azure Vm。
- 排程 Azure VM 來進行延遲和取消延遲。
- 使用 Azure 標記以遞增和遞減順序排程 Azure Vm 以進行延遲或取消延遲。
- 依需求大量刪除資源群組。

### <a name="partner-optimization-tools"></a>合作夥伴最佳化工具

Contoso 可以使用 [Hanu](https://hanu.com/insight) 和 [>scalr](https://www.scalr.com/cost-optimization)等合作夥伴工具。

## <a name="phase-4-secure-and-manage"></a>第4階段：保護與管理

在這個階段中，Contoso 會使用 Azure 安全性和管理資源，來管理、保護及監視 Azure 中的雲端應用程式。 這些資源可協助組織在使用 Azure 入口網站中提供的產品時，執行安全且妥善管理的環境。

Contoso 會在遷移期間開始使用這些服務。 透過 Azure 混合式支援，Contoso 會繼續使用許多服務，以獲得跨混合式雲端的一致體驗。

### <a name="security"></a>安全性

Contoso 會依賴 Azure 資訊安全中心進行統一的安全性管理，並在混合式雲端工作負載 Azure 進階威脅防護。

「安全性中心」可讓您在 Azure 中全面掌握雲端應用程式的安全性並對其進行控制。 Contoso 可以快速偵測威脅並採取動作來回應，以及藉由啟用調適性威脅防護來減少安全性風險。

[深入瞭解](https://azure.microsoft.com/services/security-center) 資訊安全中心。

### <a name="monitoring"></a>監視

Contoso 需要能看到剛遷移的應用程式、基礎結構和現在在 Azure 中執行之資料的健康情況和效能。 Contoso 會使用內建的 Azure 雲端監視工具，例如 Azure 監視器、Log Analytics 工作區，以及 Application Insights。

藉由使用這些工具，Contoso 可以輕鬆地從來源收集資料，並深入瞭解。 例如，Contoso 可以測量 VM 的 CPU 磁碟和記憶體使用量、檢視多部 VM 之間的應用程式和網路相依性，並追蹤應用程式效能。 Contoso 會使用這些雲端監視工具來採取行動，並與服務解決方案整合。

深入瞭解 [Azure 監視](/azure/azure-monitor/overview)。

### <a name="business-continuity-and-disaster-recovery"></a>商務持續性和災害復原

Contoso 將需要商務持續性和嚴重損壞修復 (BCDR 的 Azure 資源) 策略。

Azure 提供 [內建的 BCDR 功能](/azure/architecture/resiliency/disaster-recovery-azure-applications) 來協助保護資料，並讓應用程式和服務繼續執行。

除了內建功能外，Contoso 還想要確保其可以從失敗中復原，避免耗費資源的業務中斷情況、符合合規性目標，以及保護資料免於遭受勒索軟體攻擊和人為錯誤。 若要這樣做：

- Contoso 會部署 Azure 備份，以這個符合成本效益的解決方案來備份 Azure 資源。 由於它是內建的，因此 Contoso 可以使用幾個簡單的步驟來設定雲端備份。
- Contoso 會使用 Azure Site Recovery 在其指定的 Azure 區域之間進行複寫、容錯移轉及容錯回復，來設定 Azure Vm 的嚴重損壞修復。 這可確保在主要區域發生中斷時，在 Azure Vm 上執行的應用程式會在 Contoso 選擇的次要區域中保持可用狀態。 [深入了解](/azure/site-recovery/azure-to-azure-quickstart)。

## <a name="conclusion"></a>結論

在本文中，Contoso 已規劃了大規模的 Azure 移轉。 它會將遷移程式分成四個階段。 在完成遷移後，這些階段會從評定和遷移到優化、安全性和管理。

組織必須將遷移專案規劃為整個程式，並將集合細分為對企業有意義的分類和數位，以遷移其系統。 藉由評估資料並套用分類，專案可細分成一系列較小的遷移，可安全且快速地執行。 這些較小移轉的總和很快就會變成成功的大型 Azure 移轉。