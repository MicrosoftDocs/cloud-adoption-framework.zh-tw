---
title: 檢查您的資料選項
description: 使用適用于 Azure 的雲端採用架構，以瞭解如何判斷裝載工作負載的資料需求。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/15/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 74fc5b90484bbbc8f72568eb83bbca7c8eb25afb
ms.sourcegitcommit: 9a84c2dfa4c3859fd7d5b1e06bbb8549ff6967fa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2020
ms.locfileid: "83755806"
---
<!-- cSpell:ignore HDFS -->

# <a name="review-your-data-options"></a>檢查您的資料選項

當您準備登陸區域環境以進行雲端採用時，您需要判斷裝載工作負載的資料需求。 Azure 資料庫產品和服務支援各種不同的資料儲存體案例和功能。 要如何設定登陸區域環境來支援資料需求，取決於您的工作負載管理、技術和商務需求。

## <a name="identify-data-services-requirements"></a>識別資料服務需求

在登陸區域的評估和準備過程中，您需要識別登陸區域必須支援的資料存放區。 此程序牽涉到評估組成工作負載的每個應用程式和服務，以判斷其資料儲存及存取需求。 在您識別並記錄需求之後，您可以為登陸區域建立原則，以根據您的工作負載需求來控制允許的資源類型。

針對您要部署到登陸區域環境的每個應用程式或服務，請使用下列決策樹作為起點，以協助您判斷要使用的適當資料存放區服務：

![Azure 資料庫服務決策樹](../../_images/ready/data-decision-tree.png)

### <a name="key-questions"></a>重要問題

回答下列有關工作負載的問題，以協助您根據 Azure 資料庫服務決策樹來做出決策：

- **您需要資料庫軟體或主機 OS 的完整控制權或擁有權嗎？** 有些案例會要求您具備軟體設定和主機伺服器的高度控制權或擁有權，才能進行資料庫工作負載。 在這些案例中，您可以部署自訂的基礎結構即服務 (IaaS) 虛擬機器，以完全控制資料服務的部署和設定。 如果您沒有這些需求，平臺即服務（PaaS）資料庫服務可能會降低您的管理和作業成本。
- **您的工作負載會使用關聯式資料庫技術嗎？** 若是如此，您打算使用哪一種技術？ Azure 提供的受控 PaaS 資料庫功能適用於[Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)、[MySQL](https://docs.microsoft.com/azure/mysql/overview)、[PostgreSQL](https://docs.microsoft.com/azure/postgresql/overview) 和 [MariaDB](https://docs.microsoft.com/azure/mariadb/overview)。
- **您的工作負載是否會使用 SQL Server？** 在 Azure 中，您可以在 [Azure 虛擬機器上的 IaaS 型 SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server) 中或 [PaaS 型的 Azure SQL Database 託管服務](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)上執行工作負載。 選擇要使用哪個選項的主要考量是，您是否想管理資料庫、套用修補程式及進行備份，或是您要將這些作業委派給 Azure。 在某些情況下，因為相容性問題，您可能需要使用 IaaS 主控的 SQL Server。 若要深入了解如何為您的工作負載選擇正確選項，請參閱[在 Azure 中選擇適當的 SQL Server 選項](https://docs.microsoft.com/azure/sql-database/sql-database-paas-vs-sql-server-iaas)。
- **您的工作負載會使用索引鍵/值資料庫儲存體嗎？** [Azure Cache for Redis](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-overview) 可提供快取效能高的索引鍵/值資料儲存體解決方案，讓您擁有快速且可擴充的應用程式。 [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction) 也提供一般用途的索引鍵/值儲存體功能。
- **您的工作負載會使用文件或圖形資料嗎？** [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction) 是多模型資料庫服務，可支援各種不同的資料類型和 API。 Azure Cosmos DB 也提供文件和圖形資料庫功能。
- **您的工作負載是否會使用資料行系列資料？** [Azure HDInsight 中的 Apache HBase](https://docs.microsoft.com/azure/hdinsight/hbase/apache-hbase-overview)是建置於 apache Hadoop 上。 它支援無架構資料庫中的大量非結構化和半結構化資料，並由資料行系列組織。
- **您的工作負載需要高容量的資料分析功能嗎？** 您可以使用 [Azure SQL 資料倉儲](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is)來有效儲存及查詢結構化的 PB 規模資料。 針對非結構化的大型資料工作負載，您可以使用[Azure data lake](https://azure.microsoft.com/solutions/data-lake)來儲存及分析 pb 大小的檔案和物件的數兆個。
- **您的工作負載需要搜尋引擎功能嗎？** 您可以使用[Azure 搜尋](https://docs.microsoft.com/azure/search/search-what-is-azure-search)服務來建立 AI 增強的雲端式搜尋索引，以整合到您的應用程式中。
- **您的工作負載會使用時間序列資料嗎？** [Azure 時間序列深入](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-overview)解析是針對儲存、視覺化及查詢大量時間序列資料所建立，例如 IoT 裝置所產生的資料。

> [!NOTE]
> 在 [Azure 應用程式架構指南](https://docs.microsoft.com/azure/architecture/guide/technology-choices/data-store-comparison)中，深入了解如何評估每個應用程式或服務的資料庫選項。

## <a name="common-database-scenarios"></a>一般資料庫案例

下表說明一些常見的使用案例需求，以及用來處理這些需求的建議資料庫服務：

| **案例**                                                                                                                            | **資料服務**                                                                                                                                  |
|-----------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| 我需要有 NoSQL 選項支援的全域分散式多模型資料庫。                                                     | [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction)                                                                        |
| 我需要完全受控的關聯式資料庫，並且要能快速佈建、即時調整規模及包含內建智能和安全性。 | [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)                                               |
| 我需要完全受控、可調整規模的 MySQL 關聯式資料庫，其內建高可用性和安全性，無需額外費用。           | [適用於 MySQL 的 Azure 資料庫](https://docs.microsoft.com/azure/mysql/overview)                                                                       |
| 我需要完全受控、可調整規模的 PostgreSQL 關聯式資料庫，其內建高可用性和安全性，無需額外費用。      | [適用於 PostgreSQL 的 Azure 資料庫](https://docs.microsoft.com/azure/postgresql/overview)                                                             |
| 我打算在雲端中託管企業 SQL Server 應用程式，並對伺服器作業系統擁有完整控制權。                                        | [虛擬機器上的 SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview) |
| 我需要完全受控的彈性資料倉儲，而且每個規模層級都要提供安全性，無需額外費用。                               | [Azure SQL 資料倉儲](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is)                               |
| 我需要能夠支援 Hadoop 叢集或 HDFS 資料的 Data Lake Storage 資源。                                         | [Azure data lake](https://azure.microsoft.com/solutions/data-lake)                                                                                |
| 我的資料需要高輸送量且一致的低延遲存取，以支援快速且可調整的應用程式。                           | [Azure Cache for Redis](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-overview)                                                    |
| 我需要完全受控、可調整規模的 MariaDB 關聯式資料庫，其內建高可用性和安全性，無需額外費用。         | [適用於 MariaDB 的 Azure 資料庫](https://docs.microsoft.com/azure/mariadb/overview)                                                                   |

## <a name="regional-availability"></a>區域可用性

Azure 可讓您以所需的規模提供服務，_隨時隨地_觸及您的客戶和合作夥伴。 規劃雲端部署的關鍵要素是判斷哪個 Azure 區域可託管您的工作負載資源。

大部分資料庫服務都已在大部分的 Azure 區域中正式使用。 但有幾個區域（主要是以政府客戶為目標）僅支援這些產品的一部分。 在決定您要將資料庫資源部署到哪個區域之前，建議您參閱 [[區域] 頁面](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=data-factory,sql-server-stretch-database,redis-cache,database-migration,sql-data-warehouse,postgresql,mariadb,cosmos-db,mysql,sql-database)，以檢查區域可用性的最新狀態。

若要深入了解 Azure 全域基礎結構，請參閱 [Azure 區域頁面](https://azure.microsoft.com/global-infrastructure/regions)。 您也可以查看[依區域提供的產品](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=all)，以瞭解每個 Azure 區域中可用的整體服務的特定詳細資料。

## <a name="data-residency-and-compliance-requirements"></a>資料落地和合規性需求

您的工作負載中通常會有與資料儲存體相關的法律和合約需求。 這些需求可能會因為您組織的位置、託管資料存放區的實體資產管轄權，以及您適用的商務部門而有所不同。 需要考量的資料責任包括資料分類、資料位置，以及共同責任模式下的個別資料保護責任。 如需瞭解這些需求的協助，請參閱[使用 Azure 達成符合規範的資料存放區和安全性](https://azure.microsoft.com/resources/achieving-compliant-data-residency-and-security-with-azure)白皮書。

合規性工作的一部分可能包括控制資料庫資源實際所在的位置。 Azure 區域會在稱為 geographies 的群組中進行排列。 [Azure 地理](https://azure.microsoft.com/global-infrastructure/geographies)可確保符合地理及政治界限內的資料落地、主權、合規性及復原需求。 如果您的工作負載受限於資料主權或其他合規性需求，您必須將儲存體資源部署到合規 Azure 地理位置中的區域。

## <a name="establish-controls-for-database-services"></a>建立資料庫服務的控制項

當您準備登陸區域環境時，您可以建立控制項來限制使用者可以部署的資料存放區。 控制項可協助您管理成本並限制安全性風險，同時還能讓開發人員和 IT 小組部署及設定支援您工作負載所需的資源。

識別並記下登陸區域的需求之後，您可以使用 [Azure 原則](https://docs.microsoft.com/azure/governance/policy/overview)來控制允許使用者建立的資料庫資源。 控制項可以採用[允許或拒絕建立資料庫資源類型](https://docs.microsoft.com/azure/governance/policy/samples/allowed-resource-types)的形式。 例如，您可能會限制使用者只能建立 Azure SQL Database 資源。 您也可以在建立資源時使用原則來控制允許的選項，例如[限制可以布建的 SQL Database sku](https://docs.microsoft.com/azure/governance/policy/samples/allowed-sql-db-skus) ，或只允許在 IaaS VM 上安裝[特定版本的 SQL Server](https://docs.microsoft.com/azure/governance/policy/samples/require-sql-12) 。

原則的範圍可以設定為資源、資源群組、訂用帳戶和管理群組。 您可以將您的原則納入[Azure 藍圖](https://docs.microsoft.com/azure/governance/blueprints/overview)定義中，並在整個雲端資產中重複套用。
