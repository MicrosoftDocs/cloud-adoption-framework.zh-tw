---
title: 建議的命名和標記慣例
description: 瞭解詳細的資源命名與標記建議，特別是針對支援企業雲端採用工作。
author: BrianBlanchard
ms.author: brblanch
ms.date: 03/05/2020
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: readiness, fasttrack-edit
ms.openlocfilehash: 9e60e84659828efdc9802c45cf2f91ad945c8cda
ms.sourcegitcommit: 5d7e93540a679252f1c7207e62cb2ee7213a6ae9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2020
ms.locfileid: "80069783"
---
<!-- cSpell:ignore eastus westus westeurope usgovia accountlookup messagequery -->

# <a name="recommended-naming-and-tagging-conventions"></a>建議的命名和標記慣例

組織您的雲端資產，以支援作業管理和帳戶處理需求。 妥善定義的命名和元資料標記慣例有助於快速找出及管理資源。 這些慣例也有助於透過計費和回報帳戶處理機制，將雲端使用量成本與商務團隊產生關聯。

適用于[Azure 資源的命名規則和限制](https://docs.microsoft.com/azure/architecture/best-practices/resource-naming)的 Azure 架構中心指引提供一般建議和平臺限制。 下列討論會使用更詳細的建議來擴充該指引，特別是針對支援企業雲端採用工作。

變更資源名稱可能很棘手。 開始進行任何大型雲端部署之前，請先建立完整的命名慣例。

> [!NOTE]
> 每個企業都有不同的組織和管理需求。 這些建議提供一個起點，供您在雲端採用小組中討論。
>
> 當這些討論繼續進行時，請使用下列範本來捕捉您在將這些建議與特定業務需求對齊時所進行的命名和標記決策。
>
> 下載[命名和標記慣例追蹤範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/CAF%20Readiness%20Naming%20and%20Tagging%20tracking%20template.xlsx)。

## <a name="naming-and-tagging-resources"></a>命名與標記資源

命名和標記策略包含業務和營運詳細資料，這些是資源名稱和中繼資料標籤的元件：

- 此策略的商務端可確保資源名稱和標籤包含識別小組所需的組織資訊。 您可以與負責資源成本的業務擁有者一起使用資源。
- 在營運方面，可確保名稱和標記包含 IT 小組用來識別工作負載、應用程式、環境和重要性的資訊，以及用來管理資源的其他實用資訊。

## <a name="resource-naming"></a>資源命名

有效的命名慣例會使用重要資源資訊作為資源名稱的一部分，來組合資源名稱。 例如，使用這些[建議的命名慣例](#example-names)，實際執行 SharePoint 工作負載的公用 IP 資源會命名如下： `pip-sharepoint-prod-westus-001`。

從名稱中，您可以快速地識別出資源類型、其相關聯的工作負載、其部署環境，以及裝載該資源的 Azure 區域。

### <a name="naming-scope"></a>命名範圍

所有 Azure 資源類型的範圍會定義資源名稱必須是唯一的層級。 資源在其範圍內必須有唯一的名稱。

例如，虛擬網路有資源群組範圍，這表示在指定的資源群組中，只能有一個名為 `vnet-prod-westus-001` 的網路。 但其他資源群組可以有自己名為 `vnet-prod-westus-001` 的虛擬網路。 再舉另一個例子，子網路會包含在虛擬網路的範圍內，這表示虛擬網路中的每個子網都必須有唯一的名稱。

某些資源名稱 (例如具有公用端點或虛擬機器 DNS 標籤的 PaaS 服務) 具有全域範圍，這表示這些名稱在整個 Azure 平台中必須是唯一的。

資源名稱有長度限制。 當您開發命名慣例時，透過其範圍和長度來平衡名稱中的內嵌內容是很重要的。 若要深入了解命名規則中各資源類型允許的字元、範圍和名稱長度，請參閱 [Azure 資源的命名慣例](https://docs.microsoft.com/azure/architecture/best-practices/resource-naming)。

### <a name="recommended-naming-components"></a>建議的命名元件

當您建構命名慣例時，應找出您想要在資源名稱中反映的重要資訊片段。 不同資訊會與不同資源類型相關。 下列清單會提供建構資源名稱時實用的資訊範例。

請盡量縮短命名元件的長度，避免超過資源名稱的長度限制。

| 命名元件            | 描述                                                                                                                                                                                                      | 範例                                         |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------|
| 業務單位               | 您公司中擁有該資源所屬訂用帳戶或工作負載的最上層部門。 在小型組織中，此元件可能是單一公司的頂層組織元素。 | _fin_, _mktg_, _product_, _it_, _corp_           |
| 訂用帳戶類型           | 資源所在訂用帳戶的用途摘要描述。 通常會依部署環境類型或特定工作負載來細分。                                                       | _生產_、_共用_、_用戶端_                       |
| 應用程式或服務名稱 | 資源所屬應用程式、工作負載或服務的名稱。                                                                                                                                    | _navigator_, _emissions_, _sharepoint_, _hadoop_ |
| 部署環境      | 資源所支援工作負載的開發週期階段。                                                                                                                              | _生產_、_開發_、 _qa_、_階段_、_測試_             |
| 區域                      | 部署資源所在的 Azure 區域。                                                                                                                                                                 | _westus_、 _eastus2_、 _westeurope_、 _usgovia_     |

### <a name="recommended-resource-type-prefixes"></a>建議的資源類型首碼

每個工作負載都可以包含許多個別資源和服務。 將資源類型首碼併入您的資源名稱，可讓您更輕鬆地以視覺化方式識別應用程式或服務元件。

下列清單提供當您定義命名慣例時，可使用的建議 Azure 資源類型首碼。

<!-- cSpell:disable -->

### <a name="general"></a>一般

| 資產類型                      | 名稱前置詞 |
|---------------------------------|-------------|
| 資源群組                  | rg-         |
| 原則定義               | 策略     |
| API 管理服務實例 | apim       |

### <a name="networking"></a>網路功能

| 資產類型                       | 名稱前置詞 |
|----------------------------------|-------------|
| 虛擬網路                  | vnet-       |
| 子網路                           | snet-       |
| 網路介面 (NIC)          | nic-        |
| 公用 IP 位址                | pip-        |
| 負載平衡器（內部）         | lbi-        |
| 負載平衡器（外部）         | lbe-        |
| 網路安全性群組 (NSG)     | nsg-        |
| 應用程式安全性群組（ASG） | asg        |
| 區域網路閘道            | lgw-        |
| 虛擬網路閘道          | vgw-        |
| VPN 連線                   | cn-         |
| 應用程式閘道              | agw-        |
| 路由表                      | 料      |
| 流量管理員設定檔          | traf-       |

### <a name="compute-and-web"></a>計算和 Web

| 資產類型                  | 名稱前置詞 |
|-----------------------------|-------------|
| 虛擬機器             | vm          |
| 虛擬機器擴展集   | vmss       |
| 可用性設定組            | 激勵      |
| VM 儲存體帳戶          | stvm        |
| Azure Arc 連線電腦 | arcm-       |
| 容器實例          | aci        |
| AKS 叢集                 | aks        |
| Service Fabric 叢集      | sf         |
| App service 環境     | ase        |
| App Service 方案            | 圖       |
| Web 應用程式                     | 相關        |
| 函式應用程式                | func       |
| 雲端服務               | 會        |
| 通知中樞           | pubnames.ntf        |
| 通知中樞命名空間 | ntfns-      |

### <a name="databases"></a>資料庫

| 資產類型                     | 名稱前置詞 |
|--------------------------------|-------------|
| Azure SQL Database 伺服器      | server        |
| Azure SQL 資料庫             | sqldb-      |
| Cosmos DB 資料庫             | cosmos     |
| Azure Cache for Redis 實例 | redis-      |
| MySQL 資料庫                 | mysql-      |
| PostgreSQL 資料庫            | psql       |
| Azure SQL 資料倉儲       | sqldw-      |
| Azure Synapse Analytics        | syn        |
| SQL Server Stretch Database    | sqlstrdb-   |

### <a name="storage"></a>儲存體

| 資產類型       | 名稱前置詞 |
|------------------|-------------|
| 儲存體帳戶  | st          |
| Azure StorSimple | ssimp       |

### <a name="ai--machine-learning"></a>AI + 機器學習服務

| 資產類型                       | 名稱前置詞 |
|----------------------------------|-------------|
| Azue 認知搜尋           | srch-       |
| Azure 認知服務         | 齒輪        |
| Azure Machine Learning 工作區 | mlw-        |

### <a name="analytics-and-iot"></a>分析和 IoT

| 資產類型                      | 名稱前置詞 |
|---------------------------------|-------------|
| Azure Analysis Services server  | 一旦         |
| Azure Databricks 工作區      | dbw-        |
| Azure 串流分析          | asa-        |
| Azure Data Factory              | adf        |
| Data Lake Store 帳戶         | dls         |
| Data Lake Analytics 帳戶     | dla         |
| 事件中樞                       | evh-        |
| HDInsight-Hadoop 叢集      | hadoop     |
| HDInsight-HBase 叢集       | hbase      |
| HDInsight-Kafka 叢集       | kafka      |
| HDInsight-Spark 叢集       | 引起      |
| HDInsight-風暴叢集       | 因      |
| HDInsight-ML 服務叢集 | mls        |
| IoT 中樞                         | iot        |
| Power BI Embedded               | pbi        |

### <a name="integration"></a>整合

| 資產類型        | 名稱前置詞 |
|-------------------|-------------|
| 邏輯應用程式        | 邏輯設計      |
| 服務匯流排       | sb-         |
| 服務匯流排佇列 | sbq-        |
| 服務匯流排主題 | sbt        |

### <a name="management-and-governance"></a>管理與治理

| 資產類型              | 名稱前置詞 |
|-------------------------|-------------|
| 藍圖               | 最佳         |
| 金鑰保存庫               | kv         |
| Log Analytics 工作區 | 日誌        |
| Application Insights    | appi google-api       |
| 復原服務保存庫 | rsv-        |

### <a name="migration"></a>移轉

| 資產類型                          | 名稱前置詞 |
|-------------------------------------|-------------|
| Azure Migrate 專案               | migr-       |
| 資料庫移轉服務實例 | dms        |
| 復原服務保存庫             | rsv-        |

<!-- cSpell:enable -->

## <a name="metadata-tags"></a>中繼資料標記

將中繼資料標記套用到雲端資源時，您可以包含資源名稱中無法包含的資產相關資訊。 您可以使用該資訊來對資源執行更複雜的篩選和報告。 建議您讓這些標記包含資源相關的工作負載或應用程式、營運需求和擁有權資訊等內容。 IT 或業務小組可以使用這些資訊來尋找資源，或產生資源使用量和計費的相關報告。

您要套用至資源的標記，以及必要標記或選擇性標記的判定，會因為組織不同而有所差異。 下列清單提供可捕捉資源重要內容和相關資訊的常見標記範例。 請使用此清單作為建立您自有標記慣例的起點。

| 標記名稱                  | 描述                                                                                                                                                                                                          | Key               | 範例值                                              |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|------------------------------------------------------------|
| 應用程式名稱          | 與資源相關聯的應用程式、服務或工作負載名稱。                                                                                                                                       | _ApplicationName_ | _{應用程式名稱}_                                               |
| 核准者名稱             | 負責核准此資源相關成本的人員。                                                                                                                                                     | _Approver_        | _{電子郵件}_                                                  |
| 需要的/核准的預算  | 配置給此應用程式、服務或工作負載的金額。                                                                                                                                                          | _BudgetAmount_    | _{\$}_                                                     |
| 業務單位             | 您公司中擁有該資源所屬訂用帳戶或工作負載的最上層部門。 在小型組織中，此標記可能是單一公司或共用的頂層組織元素。 | _BusinessUnit_    | _財務_，_行銷_， _{產品名稱}_ ， _CORP_，_共用_ |
| 成本中心               | 與此資源相關聯的會計成本中心。                                                                                                                                                                | _CostCenter_      | _{數字}_                                                 |
| 災害復原         | 應用程式、工作負載或服務的業務關鍵性。                                                                                                                                                       | _DR_              | _任務關鍵性_、_關鍵_、_重要_                |
| 專案的結束日期   | 排定淘汰應用程式、工作負載或服務的日期。                                                                                                                                         | _EndDate_         | _{日期}_                                                   |
| 環境               | 應用程式、工作負載或服務的部署環境。                                                                                                                                                     | _Env_             | _生產_、_開發_、 _QA_、_階段_、_測試_                       |
| 擁有人名稱                | 應用程式、工作負載或服務的擁有者。                                                                                                                                                                      | _擁有者_           | _{電子郵件}_                                                  |
| 要求者名稱            | 要求建立此應用程式的使用者。                                                                                                                                                                 | _Requestor_       | _{電子郵件}_                                                  |
| 服務類別             | 應用程式、工作負載或服務的服務等級協定層級。                                                                                                                                              | _ServiceClass_    | _開發_、_銅_、_銀_級、_金_級                          |
| 專案的開始日期 | 初次部署應用程式、工作負載或服務的日期。                                                                                                                                                  | _StartDate_       | _{日期}_                                                   |

## <a name="example-names"></a>範例名稱

下一節提供企業雲端部署中常見 Azure 資源類型的一些範例名稱。

<!-- cSpell:disable -->

<!-- markdownlint-disable MD024 MD033 -->

### <a name="example-names-general"></a>範例名稱：一般

| 資產類型                      | 影響範圍                              | [格式]                                                      | 範例                                                                                                                |
|---------------------------------|------------------------------------|-------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------|
| 訂用帳戶                    | 帳戶 <br/>Enterprise 合約 | \<業務單位\>-\<訂用帳戶名稱\>-\<\#\#\#\>          | <ul><li>mktg-prod-001 </li><li>corp-shared-001 </li><li>fin-client-001</li></ul>                                        |
| 資源群組                  | 訂用帳戶                       | rg\<應用程式或服務名稱\>-\<訂用帳戶類型\>-\<\#\#\#\> | <ul><li>rg-mktgsharepoint-prod-001 </li><li>rg-acctlookupsvc-share-001 </li><li>rg-ad-dir-services-shared-001</li></ul> |
| API 管理服務實例 | 全域                             | apim-\<應用程式或服務名稱\>                                | apim-navigator-生產環境                                                                                                     |

### <a name="example-names-networking"></a>範例名稱：網路

| 資產類型                   | 影響範圍           | [格式]                                                               | 範例                                                                                                                      |
|------------------------------|-----------------|----------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| 虛擬網路              | 資源群組  | vnet-\<訂用帳戶類型\>-\<區域\>-\<\#\#\#\>                     | <ul><li>vnet-shared-eastus2-001 </li><li>vnet-prod-westus-001 </li><li>vnet-client-eastus2-001</li></ul>                      |
| 子網路                       | 虛擬網路 | snet-\<訂用帳戶\>-\<子區域\>-\<\#\#\#\>                       | <ul><li>snet-shared-eastus2-001 </li><li>snet-prod-westus-001 </li><li>snet-client-eastus2-001</li></ul>                      |
| 網路介面 (NIC)      | 資源群組  | nic-\<\#\#\>-\<VM 名稱\>-\<訂用帳戶\>\<\#\#\#\>                   | <ul><li>nic-01-dc1-shared-001 </li><li>nic-02-vmhadoop1-prod-001 </li><li>nic-02-vmtest1-client-001</li></ul>                 |
| 公用 IP 位址            | 資源群組  | pip-\<VM 名稱或應用程式名稱\>-\<環境\>-\<子區域\>-\<\#\#\#\> | <ul><li>pip-dc1-shared-eastus2-001 </li><li>pip-hadoop-prod-westus-001</li></ul>                                              |
| 負載平衡器                | 資源群組  | lb-\<應用程式名稱或角色\>\<環境\>\<\#\#\#\>                     | <ul><li>lb-navigator-prod-001 </li><li>lb-sharepoint-dev-001</li></ul>                                                        |
| 網路安全性群組 (NSG) | 子網或 NIC   | nsg-\<原則名稱或應用程式名稱\>-\<\#\#\#\>                           | <ul><li>nsg-weballow-001 </li><li>nsg-rdpallow-001 </li><li>nsg-sqlallow-001 </li><li>nsg-dnsbloked-001</li></ul>             |
| 區域網路閘道        | 虛擬閘道 | lgw-\<訂用帳戶類型\>-\<區域\>-\<\#\#\#\>                      | <ul><li>lgw-共用-eastus2-001 </li><li>lgw-生產-westus-001 </li><li>lgw-用戶端-eastus2-001</li></ul>                         |
| 虛擬網路閘道      | 虛擬網路 | vgw-\<訂用帳戶類型\>-\<區域\>-\<\#\#\#\>                      | <ul><li>vgw-共用-eastus2-001 </li><li>vgw-生產-westus-001 </li><li>vgw-用戶端-eastus2-001</li></ul>                         |
| 站對站連線      | 資源群組  | cn-\<區域閘道名稱\>-to-\<虛擬閘道名稱\>                | <ul><li>cn-lgw-shared-eastus2-001 到 vgw-shared-eastus2-001 </li><li>cn-lgw-shared-eastus2-001 到 shared-westus-001</li></ul> |
| VPN 連線               | 資源群組  | cn-\<訂用帳戶 1\>\<區域 1\>-to-\<訂用帳戶 2\>\<區域 2\>-     | <ul><li>cn-shared-eastus2-to-shared-westus </li><li>cn-prod-eastus2-to-prod-westus</li></ul>                                  |
| 路由表                  | 資源群組  | 路由\<路由表名稱\>                                           | <ul><li>lb-navigator-prod-001 </li><li>lb-sharepoint-dev-001</li></ul>                                                        |
| DNS 標籤                    | 全域          | \<M 的記錄\>.[\<區域\>.cloudapp.azure.com]                   | <ul><li>dc1.westus.cloudapp.azure.com </li><li>web1.eastus2.cloudapp.azure.com</li></ul>                                      |

### <a name="example-names-compute-and-web"></a>範例名稱： Compute 和 Web

| 資產類型                  | 影響範圍          | [格式]                                                              | 範例                                                                                                                          |
|-----------------------------|----------------|---------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|
| 虛擬機器             | 資源群組 | vm\<原則名稱或應用程式名稱\>\<\#\#\#\>                              | <ul><li>vmnavigator001 </li><li>vmsharepoint001 </li><li>vmsqlnode001 </li><li>vmhadoop001</li></ul>                              |
| VM 儲存體帳戶          | 全域         | stvm\<效能類型\>\<應用程式名稱或產品名稱\>\<區域\>\<\#\#\#\> | <ul><li>stvmstcoreeastus2001 </li><li>stvmpmcoreeastus2001 </li><li>stvmstplmeastus2001 </li><li>stvmsthadoopeastus2001</li></ul> |
| Web 應用程式                     | 全域         | 應用程式\<應用程式名稱\>-\<環境\>-\<\#\#\#\>。[{azurewebsites.net}]   | <ul><li>app-navigator-prod-001.azurewebsites.net </li><li>app-accountlookup-dev-001.azurewebsites.net</li></ul>                   |
| 函式應用程式                | 全域         | func-\<應用程式名稱\>-\<環境\>-\<\#\#\#\>。[{azurewebsites.net}]  | <ul><li>func-navigator-prod-001.azurewebsites.net </li><li>func-accountlookup-dev-001.azurewebsites.net</li></ul>                 |
| 雲端服務               | 全域         | \<應用程式名稱\>-\<環境\>-\<\#\#\#\>。[{cloudapp.net}]        | <ul><li>could-navigator-prod-001.azurewebsites.net </li><li>could-accountlookup-dev-001.azurewebsites.net</li></ul>                   |
| 通知中樞            | 資源群組 | pubnames.ntf-\<應用程式名稱\>-\<環境\>                                    | <ul><li>pubnames.ntf-navigator-生產環境 </li><li>pubnames.ntf-排放-開發</li></ul>                                                                   |
| 通知中樞命名空間 | 全域         | ntfns-\<應用程式名稱\>-\<環境\>                                  | <ul><li>ntfns-navigator-生產環境 </li><li>ntfns-排放-開發</li></ul>                                                               |

### <a name="example-names-databases"></a>範例名稱：資料庫

| 資產類型                     | 影響範圍              | [格式]                                 | 範例                                                                  |
|--------------------------------|--------------------|----------------------------------------|---------------------------------------------------------------------------|
| Azure SQL Database 伺服器      | 全域             | sql\<應用程式名稱\>-\<環境\>       | <ul><li>sql-navigator-生產環境 </li><li>sql-排放-開發</li></ul>           |
| Azure SQL 資料庫             | Azure SQL Database | sqldb-\<資料庫名稱 >-\<環境\> | <ul><li>sqldb-使用者-生產環境 </li><li>sqldb-使用者-開發人員</li></ul>               |
| Cosmos DB 資料庫             | 全域             | cosmos-\<應用程式名稱\>-\<環境\>    | <ul><li>cosmos-navigator-生產環境 </li><li>cosmos-排放-開發</li></ul>     |
| Azure Cache for Redis 實例 | 全域             | redis-\<應用程式名稱\>-\<環境\>     | <ul><li>redis-navigator-prod </li><li>redis-emissions-dev</li></ul>       |
| MySQL 資料庫                 | 全域             | mysql-\<應用程式名稱\>-\<環境\>     | <ul><li>mysql-navigator-prod </li><li>mysql-emissions-dev</li></ul>       |
| PostgreSQL 資料庫            | 全域             | psql-\<應用程式名稱\>-\<環境\>      | <ul><li>psql-navigator-生產環境 </li><li>psql-排放-開發</li></ul>         |
| Azure SQL 資料倉儲       | 全域             | sqldw-\<應用程式名稱\>-\<環境\>     | <ul><li>sqldw-navigator-prod </li><li>sqldw-emissions-dev</li></ul>       |
| SQL Server Stretch Database    | Azure SQL Database | sqlstrdb-\<應用程式名稱\>-\<環境\>  | <ul><li>sqlstrdb-navigator-prod </li><li>sqlstrdb-emissions-dev</li></ul> |

### <a name="example-names-storage"></a>範例名稱：儲存體

| 資產類型                        | 影響範圍  | [格式]                                                                        | 範例                                                              |
|-----------------------------------|--------|-------------------------------------------------------------------------------|-----------------------------------------------------------------------|
| 儲存體帳戶（一般用途）     | 全域 | st\<儲存體名稱\>\<\#\#\#\>                                                  | <ul><li>stnavigatordata001 </li><li>stemissionsoutput001</li></ul>    |
| 儲存體帳戶（診斷記錄） | 全域 | stdiag\<訂用帳戶名稱的前兩個字母和數字\>\<區域\>\<\#\#\#\> | <ul><li>stdiagsh001eastus2001 </li><li>stdiagsh001westus001</li></ul> |
| Azure StorSimple                  | 全域 | ssimp\<應用程式名稱\>\<環境\>                                              | <ul><li>ssimpnavigatorprod </li><li>ssimpemissionsdev</li></ul>       |

### <a name="example-names-ai--machine-learning"></a>範例名稱： AI + Machine Learning

| 資產類型                       | 影響範圍          | [格式]                            | 範例                                                          |
|----------------------------------|----------------|-----------------------------------|-------------------------------------------------------------------|
| Azue 認知搜尋           | 全域         | srch-\<應用程式名稱\>-\<環境\> | <ul><li>srch-navigator-prod </li><li>srch-emissions-dev</li></ul> |
| Azure 認知服務         | 資源群組 | 齒輪-\<應用程式名稱\>-\<環境\>  | <ul><li>齒輪-navigator-生產環境 </li><li>齒輪-排放-開發</li></ul>   |
| Azure Machine Learning 工作區 | 資源群組 | mlw-\<應用程式名稱\>-\<環境\>  | <ul><li>mlw-navigator-生產環境 </li><li>mlw-排放-開發</li></ul>   |

### <a name="example-names-analytics-and-iot"></a>範例名稱：分析和 IoT

| 資產類型                  | 影響範圍          | [格式]                              | 範例                                                              |
|-----------------------------|----------------|-------------------------------------|-----------------------------------------------------------------------|
| Azure Data Factory          | 全域         | adf-\<應用程式名稱\>\<環境\>     | <ul><li>adf-navigator-生產環境 </li><li>adf-排放-開發</li></ul>       |
| Azure 串流分析      | 資源群組 | asa-\<應用程式名稱\>-\<環境\>    | <ul><li>asa-navigator-prod </li><li>asa-emissions-dev</li></ul>       |
| Data Lake Analytics 帳戶 | 全域         | dla\<應用程式名稱\>\<環境\>      | <ul><li>dlanavigatorprod </li><li>dlaemissionsdev</li></ul>           |
| Data Lake Storage 帳戶   | 全域         | dls\<應用程式名稱\>\<環境\>      | <ul><li>dlsnavigatorprod </li><li>dlsemissionsdev</li></ul>           |
| 事件中樞                   | 全域         | evh-\<應用程式名稱\>-\<環境\>    | <ul><li>evh-navigator-prod </li><li>evh-emissions-dev</li></ul>       |
| HDInsight-HBase 叢集   | 全域         | hbase\<應用程式名稱\>-\<環境\>  | <ul><li>hbase-navigator-生產環境 </li><li>hbase-排放-開發</li></ul>   |
| HDInsight-Hadoop 叢集  | 全域         | hadoop\<應用程式名稱\>-\<環境\> | <ul><li>hadoop-navigator-生產環境 </li><li>hadoop-排放-開發</li></ul> |
| HDInsight-Spark 叢集   | 全域         | spark\<應用程式名稱\>-\<環境\>  | <ul><li>spark-導覽器-生產環境 </li><li>spark-排放-開發 </li></ul>  |
| IoT 中樞                     | 全域         | iot\<應用程式名稱\>-\<環境\>    | <ul><li>iot-navigator-生產環境 </li><li>iot-排放-開發</li></ul>       |
| Power BI Embedded           | 全域         | pbi-\<應用程式名稱\>\<環境\>     | <ul><li>pbi-navigator-生產環境 </li><li>pbi-排放-開發</li></ul>       |

### <a name="example-names-integration"></a>範例名稱：整合

| 資產類型        | 影響範圍       | [格式]                                                     | 範例                                                      |
|-------------------|-------------|------------------------------------------------------------|---------------------------------------------------------------|
| 服務匯流排       | 全域      | sb-\<應用程式名稱\>-\<環境\>.[{servicebus.windows.net}] | <ul><li>sb-navigator-prod </li><li>sb-emissions-dev</li></ul> |
| 服務匯流排佇列 | 服務匯流排 | sbq-\<查詢描述項\>                                   | <ul><li>sbq-messagequery</li></ul>                            |
| 服務匯流排主題 | 服務匯流排 | sbt-\<查詢描述項\>                                   | <ul><li>sbt-messagequery</li></ul>                            |
