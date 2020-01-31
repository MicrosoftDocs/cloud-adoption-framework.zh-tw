---
title: 大型主機遷移：大型主機應用程式遷移
description: 將應用程式從大型主機環境遷移至 Azure，這是經過實證、高度可用且可調整的基礎結構，適用於目前在大型主機上執行的系統。
author: njray
ms.author: v-nanra
ms.date: 12/26/2018
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: a662edd4ae7c6a854014b37cac2ccb0fab4c474e
ms.sourcegitcommit: 2362fb3154a91aa421224ffdb2cc632d982b129b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/28/2020
ms.locfileid: "76808816"
---
# <a name="mainframe-application-migration"></a>大型主機應用程式移轉

將應用程式從大型主機環境移轉至 Azure 時，大部分的小組都會採行務實做法：適時適地盡可能重複使用，然後再進行分階段部署，以重寫或取代應用程式。

應用程式移轉通常涉及下列一或多項策略：

- **重新裝載：** 您可以從大型主機移動現有程式碼、程式和應用程式，然後重新編譯程式碼，以在裝載于雲端實例的大型主機模擬器中執行。 此外，此方法通常會先將應用程式移至雲端式模擬器，然後再將資料庫移轉至雲端式資料庫。 在轉換資料和檔案時需要一些工程和重構。

    或者，您可以使用傳統的主機服務提供者來重新裝載。 使用雲端的主要好處之一，是可以將基礎結構管理工作外包。 您可以找到為您裝載大型主機工作負載的資料中心提供者。 此模式可換得時間、避免廠商鎖定，以及節省臨時成本。

- **淘汰：** 所有不再需要的應用程式都應該在遷移之前淘汰。

- **重建：** 有些組織選擇使用現代化技術完全重寫程式。 由於這種方法的增加成本和複雜度，並不像是隨即轉移方法一般。 進行此類型的移轉後，一般通常會使用程式碼轉換引擎開始取代模組和程式碼。

- **取代：** 這種方法會將大型主機功能取代為雲端中的對等功能。 軟體即服務 (SaaS) 是選項之一，它採用專為企業需求而建立的解決方案，例如財務、人力資源、製造或企業資源規劃等領域。 此外，目前也已有許多產業特定應用程式，可用來解決先前由自訂大型主機解決方案負責解決的問題。

建議您先規劃最初要移轉的工作負載，然後再確認移動相關聯的應用程式、舊版程式碼基底和資料庫的的需求。

## <a name="mainframe-emulation-in-azure"></a>Azure 中的大型主機模擬

Azure 雲端服務可以模擬傳統的大型主機環境，讓您重複使用現有的大型主機程式碼和應用程式。 可模擬的常見伺服器元件包括線上交易處理 (OLTP)、批次和資料擷取系統。

### <a name="oltp-systems"></a>OLTP 系統

許多大型主機都有 OLTP 系統，為廣大的使用者處理數千、甚或數以百萬計的更新。 這些應用程式通常會使用交易處理和螢幕表單處理軟體，例如客戶資訊控制系統（CICS）、資訊管理系統（IMS）和終端機介面處理器（TIP）。

將 OLTP 應用程式移至 Azure 時，您可以使用 Azure 上的虛擬機器 (VM)，將大型主機交易處理 (TP) 監視器的模擬器以基礎結構即服務 (IaaS) 的形式執行。 畫面處理和表單功能也可以由 Web 伺服器實作。 這種方法可以與資料庫 Api 結合，例如 ActiveX data objects （ADO）、開放式資料庫連接（ODBC），以及用於資料存取和交易的 JAVA 資料庫連接（JDBC）。

### <a name="time-constrained-batch-updates"></a>有時間限制的批次更新

許多大型主機系統都會執行數百萬筆帳戶記錄每個月或年度的更新，例如銀行、保險和政府機關使用的帳戶記錄。 大型主機藉由提供高輸送量的資料處理系統來處理這些類型的工作負載。 大型主機批次作業通常是循序的，且倚賴大型主機骨幹所提供每秒輸入/輸出作業 (IOPS) 來提升效能。

雲端式批次環境會使用平行計算和高速網路來提升效能。 如果您需要將批次效能最佳化，可以妥善利用 Azure 提供的多種計算、儲存體和網路功能選項。

### <a name="data-ingestion-systems"></a>資料擷取系統

大型主機會從零售業、金融服務業、製造業和處理其他解決方案擷取大型批次的資料進行處理。 透過 Azure，您可以使用簡單的命令列公用程式 (例如 [AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy)) 將資料複製到儲存體位置，或從中複製資料。 您也可以使用 [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) 服務從不同的資料存放區擷取資料，以建立和排程資料驅動的工作流程。

除了模擬環境外，Azure 還提供平台即服務 (PaaS) 和分析服務，用以增強現有的大型主機環境。

## <a name="migrate-oltp-workloads-to-azure"></a>將 OLTP 工作負載移轉至 Azure

隨即轉移方法是將現有應用程式快速遷移至 Azure 的無程式碼選項。 每個應用程式都會依現狀移轉，因而提供使用雲端時無需承擔程式碼變更風險或費用的好處。 在 Azure 上使用大型主機交易處理 (TP) 監視器的模擬器時，可支援此方法。

您可以從不同的廠商取得 TP 監視器，並在虛擬機器上執行，這是 Azure 上的基礎結構即服務 (IaaS) 選項。 下列執行前後圖表顯示 IBM DB2 (關聯式資料庫管理系統 (DBMS)) 所支援的線上應用程式在 IBM z/OS 大型主機上移轉的情形。 DB2 for z/OS 會使用虛擬儲存體存取方法 (VSAM) 檔案來儲存資料，並使用索引循序存取方法 (ISAM) 來處理一般檔案。 此架構也會使用 CICS 進行交易監視。

![使用模擬軟體將大型主機環境遷移至 Azure 的「隨即轉移」](../../_images/mainframe-migration/mainframe-vs-azure.png)

在 Azure 上，模擬環境可用來執行 TP 管理員和使用 JCL 的批次作業。 在資料層中，DB2 會取代為 [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)，但也可以使用 Microsoft SQL Server、DB2 LUW 或 Oracle Database。 模擬器支援 IMS、VSAM 和 SEQ。 大型主機的系統管理工具會由 Azure 服務，以及其他廠商在 Vm 中執行的軟體所取代。

畫面處理和表單輸入功能通常會使用 Web 伺服器來實作，而這些伺服器可以與資料庫 API (例如 ADO、ODBC 和 JDBC) 結合，以進行資料存取和交易。 所應使用的確切 Azure IaaS 元件組合，取決於您慣用的作業系統。 例如：

- 以 Windows 為基礎的 Vm： Internet Information Server （IIS），以及用於螢幕處理和商務邏輯的 ASP.NET。 針對資料存取和交易，請使用 ADO.NET。

- 以 Linux 為基礎的 Vm：可用的 JAVA 應用程式伺服器，例如用於螢幕處理的 Apache Tomcat 和以 JAVA 為基礎的商務功能。 針對資料存取和交易，請使用 JDBC。

## <a name="migrate-batch-workloads-to-azure"></a>將批次工作負載移轉至 Azure

Azure 中的批次作業不同於大型主機上的一般批次環境。 大型主機批次作業通常是循序的，且倚賴大型主機骨幹所提供 IOPS 來提升效能。 雲端式批次環境會使用平行計算和高速網路來提升效能。

若要使用 Azure 將批次效能最佳化，請考慮使用[計算](https://docs.microsoft.com/azure/virtual-machines/windows/overview)、[儲存體](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)、[網路](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux)和[監視](https://docs.microsoft.com/azure/azure-monitor/overview)選項，如下所示。

### <a name="compute"></a>計算

使用︰

- 具有最高時脈速度的 VM。 大型電腦應用程式通常使用單一執行緒，且大型主機 CPU 具有相當高的時脈速度。

- 具有高記憶體容量的 VM 可支援資料和應用程式工作區域的快取。

- 如果應用程式支援多個執行緒，則具有較高密度個 vcpu 的 Vm 可以利用多執行緒處理。

- 平行處理，因為 Azure 可輕易相應放大以進行平行處理，為批次執行提供更多計算能力。

### <a name="storage"></a>儲存體

使用︰

- [Azure PREMIUM ssd](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)或[azure ultra ssd](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) ，以取得最大的可用 IOPS。

- 將多個磁碟等量分割，使每個儲存體大小有更多 IOPS。

- 分割儲存體，以將 IO 分散到多個 Azure 儲存體裝置。

### <a name="networking"></a>網路

- 使用 [Azure 加速網路](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell)，以盡可能降低延遲。

### <a name="monitoring"></a>監視

- 使用 [Azure 監視器](https://docs.microsoft.com/azure/azure-monitor/overview)、[Azure Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) 甚至 Azure 記錄等監視工具，讓系統管理員能夠監視批次執行的過度效能，並協助消除瓶頸。

## <a name="migrate-development-environments"></a>移轉開發環境

雲端的分散式架構依賴一組不同的開發工具，以提供現代化實務和程式設計語言的優勢。 為了簡化這項轉換，您可以將開發環境與其他專為模擬 IBM z/OS 環境而設計的工具搭配使用。 下列清單顯示 Microsoft 和其他廠商提供的選項：

| 元件        | Azure 選項                                                                                                                                  |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| z/OS             | Windows、Linux 或 UNIX                                                                                                                      |
| CICS             | Micro Focus、Oracle、GT Software (Fujitsu)、TmaxSoft、Raincode 和 NTT Data 所提供的 Azure 服務，或使用 Kubernetes 重寫 |
| IMS              | Micro Focus 和 Oracle 所提供的 Azure 服務                                                                                  |
| 組合器        | Raincode 和 TmaxSoft 所提供的 Azure 服務；或 COBOL、C 或 Java，或對應至作業系統功能               |
| JCL              | JCL、PowerShell 或其他指令碼工具                                                                                                   |
| COBOL            | COBOL、C 或 Java                                                                                                                            |
| Natural          | Natural、COBOL、C 或 Java                                                                                                                  |
| FORTRAN 和 PL/I | FORTRAN、PL/I、COBOL、C 或 Java                                                                                                           |
| REXX 和 PL/I    | REXX、PowerShell 或其他指令碼工具                                                                                                  |

## <a name="migrate-databases-and-data"></a>移轉資料庫和資料

應用程式移轉通常牽涉到重新裝載資料層。 您可以將 SQL Server、開放原始碼和其他關係資料庫移轉至 Azure 上完全受控的解決方案，例如[Azure SQL Database 受控執行個體](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance)、[適用于于 postgresql 的 Azure 資料庫服務](https://docs.microsoft.com/azure/postgresql/overview)，以及[azure 資料庫移轉服務](https://docs.microsoft.com/azure/dms/dms-overview)的[適用於 MySQL 的 Azure 資料庫](https://docs.microsoft.com/azure/mysql/overview)。

例如，如果大型主機資料層使用下列項目，則可以進行移轉：

- IBM DB2 或 IMS 資料庫，請在 Azure 上使用 Azure SQL Database、SQL Server、DB2 LUW 或 Oracle Database。

- VSAM 和其他一般檔案，請對 Azure SQL、SQL Server、DB2 LUW 或 Oracle 使用索引循序存取方法 (ISAM) 一般檔案。

- Generation Date Group (GDG)，請移轉至 Azure 上使用具有 GDG 類似功能的命名慣例和副檔名的檔案。

IBM 資料層包含數個您也必須移轉的重要元件。 例如，在移轉資料庫時，您也應移轉集區中包含的資料集合，因為其中包含 dbextents，這是 z/OS VSAM 資料集。 移轉必須包含目錄以識別資料在儲存體集區中的位置。 此外，您的移轉計劃必須考量資料庫記錄，其中包含在資料庫上執行作業的記錄。 一個資料庫可以有一個、兩個 (雙重或替代) 或四個 (雙重和替代) 記錄。

資料庫移轉也包含下列元件：

- **資料庫管理員：** 提供資料庫中資料的存取權。 在 z/OS 環境中，資料庫管理員會在其本身的分割區中執行。
- **應用程式**要求者：接受來自應用程式的要求，然後再將它們傳遞給應用程式伺服器。
- **線上資源介面卡：** 包含用於 CICS 交易的應用程式要求者元件。
- **Batch 資源介面卡：** 執行 z/OS 批次應用程式的應用程式要求者元件。
- **互動式 SQL （ISQL）：** 以 CICS 應用程式和介面的形式執行，讓使用者可以輸入 SQL 語句或運算子命令。
- **CICS 應用程式：** 使用 CICS 的可用資源和資料來源，在 CICS 的控制下執行。
- **Batch 應用程式：** 執行進程邏輯，而不需要與使用者進行互動式通訊，例如，產生大量資料更新或從資料庫產生報表。

## <a name="optimize-scale-and-throughput-for-azure"></a>最佳化 Azure 的規模和輸送量

一般來說，大型主機會相應增加，而雲端會向外延展。若要將在 Azure 上執行的大型主機應用程式調整規模和輸送量優化，請務必瞭解如何將應用程式分開和隔離。 z/OS 大型主機會使用名為「邏輯分割區」(LPARS) 的功能，來隔離及管理單一執行個體上特定應用程式的資源。

例如，大型主機可能會對具有相關 COBOL 程式的 CICS 區域使用一個邏輯分割區 (LPAR)，並且對 DB2 使用另一個 LPAR。 其他 LPAR 通常用於開發、測試和預備環境。

在 Azure 上，使用不同的 Vm 來提供此用途較為常見。 Azure 架構通常會為應用程式層部署 VM、為資料層部署不同一組 VM，且再部署另一組用於開發，依此類推。 每個處理層都可以使用最適合該環境的 Vm 和功能類型來進行優化。

此外，每一層也都可提供適當的災害復原服務。 例如，生產環境和資料庫 VM 可能需要熱復原或暖復原，而開發和測試 VM 則支援冷復原。

下圖顯示使用主要和次要站台的可能 Azure 部署。 在主要網站中，會以高可用性部署生產、預備和測試 Vm。 次要站台供備份和災害復原之用。

![使用主要和次要站台的可能 Azure 部署](../../_images/mainframe-migration/migration-backup-dr.png)

## <a name="perform-a-staged-mainframe-to-azure"></a>執行從大型主機至 Azure 的分段移轉

要將解決方案從大型主機移至 Azure 可能需執行*分段*移轉，藉以讓某些應用程式先移轉，而讓其他應用程式暫時或永久保留在大型主機上。 使用此方法時，系統通常必須允許應用程式和資料庫在大型主機與 Azure 之間交互操作。

常見的案例是，將應用程式移至 Azure，但將應用程式所使用的資料保留在大型主機上。 您可以使用特定軟體讓 Azure 上的應用程式可從大型主機存取資料。 幸運的是，有許多解決方案都提供 Azure 與現有大型主機環境的整合功能、混合式案例的支援，以及長時間的漸進移轉。 Microsoft 合作夥伴、獨立軟體廠商和系統整合商皆可協助您展開此程序。

其中一個選項是[Microsoft Host Integration Server](https://docs.microsoft.com/host-integration-server)，這是一種解決方案，可提供 Azure 中的應用程式所需的分散式關係資料庫架構（DRDA），以存取保留在大型主機上的 DB2 資料。 將大型主機整合至 Azure 的其他選項包括 IBM、Attunity、Codit、其他廠商所提供的解決方案，和開放原始碼選項。

## <a name="partner-solutions"></a>合作夥伴解決方案

如果您考慮進行大型主機移轉，合作夥伴生態系統將可協助您。

Azure 提供經過實證、高度可用且可調整的基礎結構，適用於目前在大型主機上執行的系統。 某些工作負載比較容易移轉。 其他依賴舊式系統軟體 (例如 CICS 與 IMS) 的工作負載，可以使用合作夥伴解決方案重新裝載，再分階段移轉至 Azure。 無論您如何選擇，Microsoft 與合作夥伴都很樂意協助您達成 Azure 的最佳化，同時保有大型主機系統軟體功能。

## <a name="learn-more"></a>了解更多

如需詳細資訊，請參閱下列資源：

- [開始使用 Azure](https://docs.microsoft.com/azure)

- [在 Azure 上部署 IBM DB2 pureScale](https://azure.microsoft.com/resources/deploy-ibm-db2-purescale-on-azure)

- [Host Integration Server 檔](https://docs.microsoft.com/host-integration-server)
