---
title: 檢查您的儲存體選項
description: 使用適用于 Azure 的雲端採用架構，瞭解如何檢查 Azure 工作負載的儲存體選項。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/15/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 04435f6ef825a5a0f3b65125bcf5ea66397897ce
ms.sourcegitcommit: afe10f97fc0e0402a881fdfa55dadebd3aca75ab
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/31/2020
ms.locfileid: "80433853"
---
<!-- cSpell:ignore HDFS databox Avere HANA ACLs Isilon DFSR Cloudera -->

# <a name="review-your-storage-options"></a>檢查您的儲存體選項

儲存體功能對於支援裝載於雲端中的工作負載和服務非常重要。 在雲端採用移轉整備程度準備過程中，請參閱這篇文章以協助您規劃及解決您的儲存體需求。

## <a name="select-storage-tools-and-services-to-support-your-workloads"></a>選取儲存體工具和服務以支援您的工作負載

[Azure 儲存體](https://docs.microsoft.com/azure/storage)是 Azure 平台的受控服務，可提供雲端儲存體。 Azure 儲存體是由數個核心服務和支援功能所組成。 Azure 中的儲存體是高可用性、安全、耐用、可擴充和備援性儲存體。 請檢閱這裡所述的案例和考量，以選擇相關的 Azure 服務和正確的架構，符合貴組織的工作負載、治理和資料儲存體需求。

<!-- For each application or service you'll deploy to your landing zone environment, use the following decision tree as a starting point to help you determine your storage resources requirements:

![Azure storage decision tree](../../_images/ready/storage-decision-tree.png)
-->

### <a name="key-questions"></a>重要問題

回答下列有關工作負載的問題，以協助您根據 Azure 儲存體決策樹來做出決策：

- **您的工作負載是否需要磁碟儲存體來支援基礎結構即服務 (IaaS) 虛擬機器的部署？** [Azure 磁碟儲存體](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview)為 IaaS 虛擬機器提供虛擬磁碟功能。
- **您是否需要提供可下載的影像、文件或其他媒體作為工作負載的一部分？** [Azure Blob 儲存體](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)提供[裝載靜態檔案](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website)的功能，該檔案之後可透過網際網路進行下載。 您可以將裝載於 Blob 儲存體中的資產設為公用，或者您可以透過 Azure Active Directory (Azure AD)、共用金鑰或共用存取簽章，[將資產限制為授權的使用者](https://docs.microsoft.com/azure/storage/common/storage-auth)。
- **您需要儲存虛擬機器記錄、應用程式記錄檔和分析資料的位置嗎？** 您可以使用 Azure Blob 儲存體來[儲存 Azure 監視器記錄資料](https://docs.microsoft.com/azure/storage/common/storage-analytics)。
- **您是否需要提供備份、災害復原或封存工作負載相關資料的位置？** Azure 磁碟儲存體會使用 Azure Blob 儲存體來提供[備份和災害復原功能](https://docs.microsoft.com/azure/virtual-machines/windows/backup-and-disaster-recovery-for-azure-iaas-disks)。 您也可以使用 Blob 儲存體作為備份其他資源的位置，例如內部部署或裝載於 IaaS VM 的 [SQL Server 資料](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service?view=sql-server-2017)。
- **您需要支援巨量資料分析工作負載嗎？** [Azure Data Lake Storage Gen 2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction) 建置於 Azure Blob 儲存體之上。 Data Lake Storage Gen 2 可以支援大型企業 Data Lake 功能。 它也可以處理儲存數 PB 的資訊，同時維持數百 GB 的輸送量。
- **您是否需要提供雲端原生檔案共用？** Azure 有兩個主要服務，可提供雲端託管的檔案共用： Azure NetApp Files 和 Azure 檔案儲存體。 [Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction) 提供適用於一般企業工作負載 (例如 SAP) 的高效能 NFS 共用。 [Azure 檔案儲存體](https://docs.microsoft.com/azure/storage/files/storage-files-introduction)提供可透過 SMB 3.0 和 HTTPS 存取的檔案共用。
- **您是否需要支援適用於內部部署高效能運算 (HPC) 工作負載的混合式雲端儲存體？** [Avere vFXT for Azure](https://docs.microsoft.com/azure/avere-vfxt/avere-vfxt-overview) 是一種混合式快取解決方案，可讓您使用雲端式儲存體來擴充內部部署儲存體功能。 針對包含 1000 到 40000 CPU 核心計算伺服器陣列的大量讀取 HPC 工作負載，Avere vFXT for Azure 已最佳化。 Avere vFXT for Azure 可以與內部部署硬體網路連接儲存裝置 (NAS)、Azure Blob 儲存體或兩者整合。
- **您需要執行大規模的封存，並將您的內部部署資料同步處理至雲端嗎？** [Azure 資料箱](https://docs.microsoft.com/azure/databox-family)產品的設計，是為了協助您將大量資料從內部部署環境移至雲端。 [Azure 資料箱閘道](https://docs.microsoft.com/azure/databox-online/data-box-gateway-overview)是位於內部部署環境的虛擬裝置。 資料箱閘道可協助您管理前往雲端的大規模資料移轉。 如果您需要在將資料移至雲端之前進行分析、轉換或篩選，您可以使用 [Azure Data Box Edge](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview)，這是部署至內部部署環境的已啟用 AI 實體邊緣計算裝置。 Data Box Edge 加速處理，並將資料安全傳輸至 Azure。
- **您要將現有的內部部署檔案共用擴充為使用雲端儲存空間嗎？** [Azure 檔案同步](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide)可讓您使用 Azure 檔案儲存體服務，作為裝載於內部部署 Windows Server 電腦上檔案共用的延伸模組。 同步處理服務會將 Windows Server 轉換成 Azure 檔案共用的快速快取。 它可讓您存取共用的內部部署電腦使用 Windows Server 上可用的任何通訊協定。

## <a name="common-storage-scenarios"></a>常見的儲存體案例

Azure 針對不同的儲存體功能提供多項產品和服務。 除了本文稍早所示的儲存體需求決策樹之外，下表描述一系列可能的儲存體案例，以及建議的 Azure 服務來解決案例的需求：

### <a name="block-storage-scenarios"></a>封鎖儲存體案例

<!-- markdownlint-disable MD033 -->

| **案例** | **建議的 Azure 服務** | **建議服務的考量** |
|---|---|---|
| 我有裸機伺服器或 VM (Hyper-V 或 VMware)，並具有執行 LOB 應用程式的直接連接儲存體。 | [Azure 磁碟儲存體 (進階 SSD)](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#premium-ssd) | 對於生產服務，進階 SSD 選項會提供一致的低延遲，加上高 IOPS 和輸送量。 |
| 我有伺服器將裝載 Web 和行動裝置應用程式。 | [Azure 磁碟儲存體 (標準 SSD)](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#standard-ssd) | 針對生產環境中的 CPU 繫結 Web 和應用程式伺服器，標準 SSD IOPS 和輸送量可能已足夠 (成本低於進階 SSD)。 |
| 我有一個企業 SAN 或全部 Flash 陣列 (AFA)。 | [Azure 磁碟儲存體 (進階或 Ultra SSD)](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types) <br/><br/> [Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction) | Ultra SSD 是 NVMe 型，並以高 IOPS 和頻寬提供快速的延遲。 Ultra SSD 最多可延展至 64 TiB。 進階 SSD 和 Ultra SSD 的選擇取決於尖峰延遲、IOPS 和擴充性需求。 |
| 我擁有高可用性 (HA) 叢集伺服器 (例如 SQL Server FCI 或 Windows Server 容錯移轉叢集)。 | [Azure 檔案儲存體 (進階)](https://docs.microsoft.com/azure/storage/files/storage-files-planning#storage-tiers)<br/> [Azure 磁碟儲存體 (進階或 Ultra SSD)](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types) | 叢集工作負載需要多個節點來裝載相同的容錯移轉或 HA 基礎共用儲存體。 進階檔案共用提供可透過 SMB 掛接的共用儲存體。 您也可以使用[合作夥伴解決方案](https://azuremarketplace.microsoft.com/marketplace/apps/sios_datakeeper.sios-datakeeper-8?tab=Overview)，在進階 SSD 或 Ultra SSD 上設定共用區塊儲存體。 |
| 我有關聯式資料庫或資料倉儲工作負載 (例如 SQL Server 或 Oracle)。 | [Azure 磁碟儲存體 (進階或 Ultra SSD)](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types) | 進階 SSD 與 Ultra SSD 的選擇取決於尖峰延遲、IOPS 和擴充性需求。 Ultra SSD 也會藉由移除儲存體集區設定的擴充性需求來降低複雜性 (請參閱[詳細資料](https://azure.microsoft.com/blog/mission-critical-performance-with-ultra-ssd-for-sql-server-on-azure-vm) (英文))。 |
| 我有一個 NoSQL 叢集 (例如 Cassandra 或 MongoDB)。 | [Azure 磁碟儲存體 (進階 SSD)](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#premium-ssd) | Azure 磁碟儲存體進階 SSD 供應項目提供一致的低延遲，加上高 IOPS 和輸送量。 |
| 我正在執行具有持續性磁碟區的容器。 | [Azure 檔案儲存體 (標準或進階)](https://docs.microsoft.com/azure/storage/files/storage-files-planning) <br/><br/> [Azure 磁碟儲存體 (標準、進階或 Ultra SSD)](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types) | 檔案 (RWX) 和封鎖 (RWO) 磁碟區驅動程式選項可用於 Azure Kubernetes Service (AKS) 與自訂 Kubernetes 部署。 持續性磁碟區可以對應到 Azure 磁碟儲存體磁碟或受控 Azure 檔案儲存體共用。 根據持續性磁碟區的工作負載需求，選擇進階與標準選項。 |
| 我有 Data Lake (例如 HDFS 資料的 Hadoop 叢集)。 | [Azure Data Lake Storage Gen 2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction) <br/><br/> [Azure 磁碟儲存體 (標準或進階 SSD)](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types) | Azure Blob 儲存體的 Data Lake Storage Gen 2 功能為平行分析提供了伺服器端 HDFS 相容性和 PB 規模。 它也提供 HA 和可靠性。 如有需要，Cloudera 之類的軟體可以在主要/背景工作節點上使用進階或標準 SSD。 |
| 我有 SAP 或 SAP HANA 部署。 | [Azure 磁碟儲存體 (進階或 Ultra SSD)](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types) | Ultra SSD 已進行最佳化，以提供第 1 層 SAP 工作負載的快速延遲。 Ultra SSD 現為預覽階段。 與 M 系列結合的進階 SSD 可提供公開上市 (GA) 選項。 |
| 我有一個災害復原網站，其具有從主要伺服器同步處理的嚴格 RPO/RTO。 | [Azure 分頁 Blob](https://docs.microsoft.com/azure/storage/blobs/storage-blob-pageblob-overview) | 複寫軟體會使用 Azure 分頁 Blob 來啟用 Azure 的低成本複寫，而不需要計算 VM，直到容錯移轉發生為止。 如需詳細資訊，請參閱 [Azure 磁碟儲存體文件](https://docs.microsoft.com/azure/virtual-machines/windows/backup-and-disaster-recovery-for-azure-iaas-disks)。 **注意：** 分頁 blob 最多支援 8 TB。 |

### <a name="file-and-object-storage-scenarios"></a>檔案和物件儲存體案例

| **案例** | **建議的 Azure 服務** | **建議服務的考量** |
|---|---|---|
| 我使用 Windows 檔案伺服器。 | [Azure 檔案](https://docs.microsoft.com/azure/storage/files/storage-files-planning) <br/><br/> [Azure 檔案同步](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning) | 使用 Azure 檔案同步，您可以在雲端式 Azure 檔案共用上儲存鮮少使用的資料，同時快取您最常使用的內部部署檔案，以進行快速、本機存取。 您也可以使用多網站同步處理，讓檔案在多部伺服器之間保持同步。 如果您打算將工作負載遷移至僅限雲端的部署，Azure 檔案儲存體可能就已足夠。 |
| 我有企業 NAS (例如 NetApp 檔或 Dell-EMC Isilon)。 | [Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction) <br/><br/> [Azure 檔案儲存體 (進階)](https://docs.microsoft.com/azure/storage/files/storage-files-planning#storage-tiers) | 如果您有內部部署的 NetApp，請考慮使用 Azure NetApp Files 將您的部署遷移至 Azure。 如果您使用或將遷移至 Windows Server 或 Linux 伺服器，或您具有檔案共用的基本功能需求，請考量使用 Azure 檔案儲存體。 若要繼續進行內部部署存取，請使用 Azure 檔案同步透過雲端階層處理機制，將 Azure 檔案共用與內部部署檔案共用同步。 |
| 我有一個檔案共用 (SMB 或 NFS)。 | [Azure 檔案儲存體 (標準或進階)](https://docs.microsoft.com/azure/storage/files/storage-files-planning) <br/><br/> [Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction) | 進階與標準 Azure 檔案儲存體層的選擇取決於 IOPS、輸送量和您的延遲一致性需求。 如果您有內部部署的 NetApp，請考量使用 Azure NetApp Files。 如果您需要將存取控制清單 (ACL) 和時間戳記遷移至雲端，Azure 檔案同步可以將所有這些設定帶入您的 Azure 檔案共用，作為便利的移轉路徑。 |
| 我有適用於數 PB 資料的內部部署物件儲存體系統 (例如 Dell-EMC ECS)。 | [Azure Blob 儲存體](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) |  Azure Blob 儲存體提供進階、經常性存取、非經常性存取和封存層，以符合您的工作負載效能和成本需求。 |
| 我有一個 DFSR 部署，或另一種方式來處理分公司。 | [Azure 檔案](https://docs.microsoft.com/azure/storage/files/storage-files-planning) <br/><br/> [Azure 檔案同步](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning) | Azure 檔案同步提供多網站同步處理，讓檔案在雲端中的多部伺服器和原生 Azure 檔案共用之間保持同步。 使用雲端階層處理，移至內部部署的固定儲存體使用量。 雲端階層處理會將您的伺服器轉換成相關檔案的快取，同時在 Azure 檔案共用中調整幾乎不存取的資料。 |
| 我有磁帶媒體櫃（內部部署或異地），用於備份和嚴重損壞修復或長期資料保留。 | [Azure Blob 儲存體 (非經常性存取層或封存層)](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers) | Azure Blob 儲存體封存層的可能成本最低，但可能需要數小時才能將離線資料複製到非經常性存取層、經常性存取層或進階層儲存體，以允許存取。 非經常性存取層以低成本提供即時存取。 |
| 我已設定檔案或物件儲存體來接收我的備份。 | [Azure Blob 儲存體 (非經常性存取層或封存層)](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers) <br/>[Azure 檔案同步](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning) | 若要使用最低成本儲存體來備份長期保留的資料，請將資料移至 Azure Blob 儲存體，並使用非經常性存取層和封存層。 若要針對伺服器（內部部署或 Azure VM）上的檔案資料啟用快速的嚴重損壞修復，請使用 Azure 檔案同步，將共用同步至個別的 Azure 檔案共用。使用 Azure 檔案共用快照集，您可以還原較舊的版本，並將其同步至已連線的伺服器，或在 Azure 檔案共用中以原生方式存取它們。 |
| 我對災害復原網站執行資料複寫。 | [Azure 檔案](https://docs.microsoft.com/azure/storage/files/storage-files-planning) <br/><br/> [Azure 檔案同步](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning) | Azure 檔案同步會移除災害復原伺服器的需求，並將檔案儲存在原生 Azure SMB 共用中。 快速的災害復原會快速重建故障內部部署伺服器上的任何資料。 您甚至可以讓多個伺服器位置保持同步，或使用雲端階層處理僅儲存相關的內部部署資料。 |
| 我在中斷連線的情況下管理資料傳輸。 | [Azure Data Box Edge 或 Azure 資料箱閘道](https://docs.microsoft.com/azure/databox-online) | 使用 Data Box Edge 或資料箱閘道，您可以在中斷連線的情況下複製資料。 當閘道離線時，它會將您複製的所有檔案儲存在快取中，然後在您連線時上傳它們。 |
| 我管理對雲端進行中的資料管線。 | [Azure Data Box Edge 或 Azure 資料箱閘道](https://docs.microsoft.com/azure/databox-online) | 將資料從經常產生資料的系統移至雲端，只要將資料直接複製到儲存體閘道即可。 如果他們需要稍後再存取該資料，則會直接放在該處。 |
| 我有大量的資料會在同一時間抵達。 | [Azure Data Box Edge 或 Azure 資料箱閘道](https://docs.microsoft.com/azure/databox-online) | 管理同時抵達的大量資料，例如當自發車拉回車庫，或 gene 排序機器完成其分析時。 以快速的本機速度將所有資料複製到資料箱閘道，然後讓閘道在您的網路允許的情況下上傳它。

### <a name="plan-based-on-data-workloads"></a>根據資料工作負載進行規劃

| **案例** | **建議的 Azure 服務** | **建議服務的考量** |
|---|---|---|
| 我想要開發新的雲端原生應用程式，其需要保存非結構化資料。 | [Azure Blob 儲存體](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) | |
| 我需要將資料從內部部署 NetApp 執行個體遷移至 Azure。 | [Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction) | |
| 我需要將資料從內部部署 Windows 檔案伺服器執行個體遷移至 Azure。 | [Azure 檔案](https://docs.microsoft.com/azure/storage/files/storage-files-planning) | |
| 我需要將檔案資料移至雲端，但繼續主要從內部部署存取資料。 | [Azure 檔案](https://docs.microsoft.com/azure/storage/files/storage-files-planning) <br/><br/> [Azure 檔案同步](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning) | |
| 我需要支援「高載計算」-NFS/SMB 大量讀取、檔案型工作負載，以及在雲端中執行計算時，位於內部部署的資料資產。 | [Avere vFXT for Azure](https://docs.microsoft.com/azure/avere-vfxt/avere-vfxt-overview) | IaaS 相應放大 NFS/SMB 檔案快取 |
| 我需要移動使用本機磁碟或 iSCSI 的內部部署應用程式。 | [Azure 磁碟儲存體](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) | |
| 我需要遷移具有持續性磁碟區的容器型應用程式。 | [Azure 磁碟儲存體](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) <br/><br/> [Azure 檔案](https://docs.microsoft.com/azure/storage/files/storage-files-planning) | |
| 我需要將不是 Windows Server 或 NetApp 的檔案共用移到雲端。 | [Azure 檔案](https://docs.microsoft.com/azure/storage/files/storage-files-planning) <br/><br/> [Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction) | 通訊協定支援區域可用性效能需求快照和複製功能價格敏感度 |
| 我需要從內部部署傳輸數 TB 到數 PB 的資料至 Azure。 | [Azure Data Box Edge](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview) | |
| 我必須先處理資料，再將其傳送至 Azure。 | [Azure Data Box Edge](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview) | |
| 我需要使用本機快取，以自動化方式支援連續的資料擷取。 | [Azure 資料箱閘道服務](https://docs.microsoft.com/azure/databox-online/data-box-gateway-overview) | |

## <a name="learn-more-about-azure-storage-services"></a>深入了解 Azure 儲存體服務

找出最符合您需求的 Azure 工具之後，請使用下表所連結的詳細文件，熟悉這些服務：

| **服務** | **說明** |
|---|---|
| [Azure Blob 儲存體](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) | Azure Blob 儲存體是 Microsoft 針對雲端推出的物件儲存體解決方案。 Blob 儲存體經過最佳化，已能妥善儲存大量的非結構化資料。 非結構化資料是指不符合特定資料模型或定義的資料，例如文字或二進位資料。<br/><br/>Blob 儲存體設計用來：<ul><li>直接提供映像或文件給瀏覽器。</li><li>儲存檔案供分散式存取。</li><li>串流傳輸視訊和音訊。</li><li>寫入記錄檔。</li><li>儲存備份和還原、災害復原和封存資料。</li><li>儲存資料供內部部署或 Azure 託管服務進行分析。</li></ul> |
| [Azure Data Lake Storage Gen 2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction) | Blob 儲存體支援 Azure Data Lake Storage Gen2，這是適用於雲端的 Microsoft 企業巨量資料分析解決方案。 Azure Data Lake Storage Gen2 提供階層式檔案系統和 Blob 儲存體的各項優點，包括低成本、分層式儲存體、高可用性、強式一致性，以及災害復原功能。 |
| [Azure 磁碟儲存體](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) | Azure 磁碟儲存體提供持續性、高效能的區塊儲存體，以支援 Azure 虛擬機器。 Azure 磁片具有高耐用性、安全性，並提供業界唯一的單一實例 SLA，適用于使用 premium 或 ultra Ssd 的 Vm （[深入瞭解磁片類型](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types)）。 Azure 磁碟可透過對應至 Azure 虛擬機器容錯網域的可用性設定組和可用性區域，提供高可用性。 此外，Azure 磁碟會作為 Azure 中的最上層資源來管理。 根據預設，系統會提供角色型存取控制 (RBAC)、原則和標記等 Azure Resource Manager 功能。 |
| [Azure 檔案](https://docs.microsoft.com/azure/storage/files/storage-files-planning) | Azure 檔案儲存體提供完全受控的原生 SMB 檔案共用作為服務，而不需要執行 VM。 您可以將 Azure 檔案儲存體共用掛接為任何 Azure VM 或內部部署機器的網路磁碟機。 |
| [Azure 檔案同步](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning) | 您可使用 Azure 檔案同步將組織的檔案共用集中在 Azure 檔案服務中，同時保有內部部署檔案伺服器的靈活度、效能及相容性。 Azure 檔案同步會將 Windows Server 轉換成 Azure 檔案共用的快速快取。 |
| [Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction) | Azure NetApp Files 服務是企業等級、高效能、計量檔案儲存體服務。 Azure NetApp Files 支援任何工作負載類型，預設為高度可用。 您可以選取服務和效能層級，並且透過服務設定快照集。 |
| [Azure Data Box Edge](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview) | Azure Data Box Edge 是內部部署網路裝置，可將資料移入和移出 Azure。 Data Box Edge 具有已啟用 AI 的邊緣計算，以便在上傳期間預先處理資料。 「資料箱閘道」是該裝置的虛擬版本，但是具備相同的資料傳輸功能。 |
| [Azure 資料箱閘道服務](https://docs.microsoft.com/azure/databox-online/data-box-gateway-overview) | Azure 資料箱閘道是可讓您順利將資料傳送到 Azure 的儲存體解決方案。 資料箱閘道是虛擬裝置，其基礎是佈建在虛化環境中或 Hypervisor 中的虛擬機器。 虛擬裝置位於內部部署環境，您可以使用 NFS 和 SMB 通訊協定將資料寫入其中。 裝置接著會將您的資料傳輸至 Azure 區塊 Blob 或 Azure 分頁 Blob，或 Azure 檔案儲存體。 |
| [Avere vFXT for Azure](https://docs.microsoft.com/azure/avere-vfxt/avere-vfxt-overview) | Avere vFXT for Azure 是一個適用於資料密集高效能運算 (HPC) 工作的檔案系統快取解決方案。 利用雲端運算的擴充性，讓您的資料隨時隨地都能存取，&mdash;即使是儲存在您自己的內部部署硬體中的資料也一樣。 |

## <a name="data-redundancy-and-availability"></a>資料備援和可用性

Azure 儲存體有各種不同的備援選項，可根據客戶需求來協助確保持久性和高可用性：本地備援儲存體 (LRS)、區域備援儲存體 (ZRS)、異地備援儲存體 (GRS)，以及讀取權限異地備援儲存體 (RA-GRS).

若要深入了解這些功能，以及如何為您的使用案例決定最佳的備援選項，請參閱 [Azure 儲存體備援](https://docs.microsoft.com/azure/storage/common/storage-redundancy)。 此外，儲存體服務的服務等級協定 (SLA) 會提供財務支援的保證。 如需詳細資訊，請參閱[受控磁碟 SLA](https://azure.microsoft.com/support/legal/sla/managed-disks/v1_0)、[虛擬機器 SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8)和[儲存體帳戶 SLA](https://azure.microsoft.com/support/legal/sla/storage/v1_4)。

如需針對 Azure 磁碟規劃正確解決方案的協助，請參閱 [Azure 磁碟儲存體的備份和災害復原](https://docs.microsoft.com/azure/virtual-machines/windows/backup-and-disaster-recovery-for-azure-iaas-disks)。

## <a name="security"></a>安全性

為了協助您保護雲端中的資料，Azure 儲存體針對待用和傳輸中的資料，提供了數個資料安全性和加密的最佳做法。 您可以：

- 使用 RBAC 和 Azure AD 保護儲存體帳戶。
- 使用用戶端加密、HTTPS 或 SMB 3.0，保護應用程式和 Azure 之間傳輸中資料的安全。
- 設定當使用儲存體服務加密來將資料寫入 Azure 儲存體時自動加密資料。
- 使用共用存取簽章，將委派存取權授與 Azure 儲存體中的資料物件。
- 使用分析來追蹤當某人存取 Azure 中的儲存體時使用的驗證方法。

這些安全性功能適用於 Azure Blob 儲存體 (區塊和頁面)，以及適用於 Azure 檔案儲存體。 取得 [Azure 儲存體安全性指南](https://docs.microsoft.com/azure/storage/blobs/security-recommendations)中的詳細儲存體安全性指南。

[儲存體服務加密](https://docs.microsoft.com/azure/storage/storage-service-encryption)提供待用加密，並保護資料安全，以符合組織安全性和合規性承諾。 所有 Azure 區域中的所有受控磁碟、快照集和映像預設都會啟用儲存體服務加密。 從 2017 年 6 月 10 日開始，所有新受控磁碟、快照集、映像和寫入至現有受控磁碟的新資料都會使用 Microsoft 所管理的金鑰進行自動待用加密。 如需詳細資訊，請造訪[受控磁碟常見問題集](https://docs.microsoft.com/azure/virtual-machines/windows/faq-for-disks#managed-disks-and-storage-service-encryption)。

Azure 磁碟加密可讓您使用儲存在 [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault) 中的金鑰，將連接到 IaaS VM 的受控磁碟加密為待用和傳輸中的作業系統和資料磁碟。 對於 Windows，磁碟機是使用業界標準的 [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) 加密技術來加密。 對於 Linux，磁碟是使用 [dm-crypt](https://wikipedia.org/wiki/Dm-crypt) (英文) 子系統來加密。 加密程序會與 Azure Key Vault 整合，可讓您控制和管理磁碟加密金鑰。 如需詳細資訊，請參閱 [Windows 和 Linux IaaS 虛擬機器適用的 Azure 磁碟加密](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview)。

## <a name="regional-availability"></a>區域可用性

您可以使用 Azure 依照您所需要的規模，將服務提供給身居*世界不同角落*的客戶及合作夥伴。 [受控磁碟](https://azure.microsoft.com/global-infrastructure/services/?products=managed-disks)和 [Azure 儲存體](https://azure.microsoft.com/global-infrastructure/services/?products=storage)區域可用性頁面會顯示提供這些服務的區域。 事先檢查服務的區域可用性，可協助您針對您的工作負載和客戶需求做出正確的決策。

受控磁碟可在所有具有進階 SSD 和標準 SSD 供應項目的 Azure 區域中使用。 雖然 Ultra SSD 目前處於公開預覽狀態，但是只在一個可用性區域 (美國東部 2 區域) 中提供。 當您規劃需要 Ultra SSD 的任務關鍵性、最上層工作負載時，請確認區域可用性。

經常性存取層和非經常性存取層 Blob 儲存體、Data Lake Storage Gen2 和 Azure 檔案儲存體都可在所有 Azure 區域中使用。 封存 Blob 儲存體、進階檔案共用和進階區塊 Blob 儲存體僅限於特定區域。 我們建議您參閱區域頁面，以檢查區域可用性的最新狀態。

若要深入了解 Azure 全域基礎結構，請參閱 [Azure 區域頁面](https://azure.microsoft.com/global-infrastructure/regions)。 您也可以參閱依[依區域提供的產品](https://azure.microsoft.com/global-infrastructure/services/?products=storage)頁面，以取得每個 Azure 區域中可用服務的特定詳細資料。

## <a name="data-residency-and-compliance-requirements"></a>資料落地和合規性需求

您的工作負載中通常會有與資料儲存體相關的法律和合約需求。 這些需求可能會因為您組織的位置、託管資料存放區的實體資產管轄權，以及您適用的商務部門而有所不同。 需要考量的資料責任包括資料分類、資料位置，以及共同責任模式下的個別資料保護責任。 如需了解這些需求的協助，請參閱[利用 Azure 達成符合規範的資料落地和安全性](https://azure.microsoft.com/resources/achieving-compliant-data-residency-and-security-with-azure)白皮書。

合規性工作的一部分可能包括控制資料庫資源實際所在的位置。 Azure 區域會在稱為 geographies 的群組中進行排列。 [Azure 地理](https://azure.microsoft.com/global-infrastructure/geographies)可確保符合地理及政治界限內的資料落地、主權、合規性及復原需求。 如果您的工作負載受限於資料主權或其他合規性需求，您必須將儲存體資源部署到符合規範的 Azure 地理位置中的區域。
