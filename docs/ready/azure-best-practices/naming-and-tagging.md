---
title: 建議的命名和標記慣例
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 本文提供詳細的資源命名與標記建議，主要用於支援企業的雲端採用工作。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/01/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: readiness
ms.openlocfilehash: 3a99398d5ae180efe9dca4cadf0554d92c6380b2
ms.sourcegitcommit: 91ece6ba373a4d0d573cca7e616f0b67337b0d1b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/15/2020
ms.locfileid: "76023356"
---
# <a name="recommended-naming-and-tagging-conventions"></a>建議的命名和標記慣例

以協助操作管理和支援會計需求的方式組織雲端式資產，是大型雲端採用的常見挑戰。 藉由將妥善定義的命名和中繼資料標記慣例套用到雲端託管的資源，IT 人員就可以快速尋找及管理資源。 妥善定義的名稱和標記也有助於使用計費 (chargeback) 和回報 (showback) 會計機制，讓雲端的使用成本與業務小組保持一致。

適用于[Azure 資源的命名規則和限制](https://docs.microsoft.com/azure/architecture/best-practices/resource-naming)的 Azure 架構中心指引提供一般建議和平臺限制。 下列討論會使用更詳細的建議來擴充該指引，特別是針對支援企業雲端採用工作。

資源名稱可能難以變更。 開始進行任何大型雲端部署之前，請先建立完整命名慣例的優先順序。

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

### <a name="resource-naming"></a>資源命名

有效的命名慣例會使用重要資源資訊作為資源名稱的一部分，來組合資源名稱。 例如，若使用[本文稍後](#sample-naming-convention)討論的建議命名慣例，在實際執行的 SharePoint 工作負載上，公用 IP 資源的名稱會類似這樣：`pip-sharepoint-prod-westus-001`。

從名稱中，您可以快速地識別出資源類型、其相關聯的工作負載、其部署環境，以及裝載該資源的 Azure 區域。

#### <a name="naming-scope"></a>命名範圍

所有 Azure 資源類型的範圍都會定義資源名稱必須是唯一的層級。 資源在其範圍內必須有唯一的名稱。

例如，虛擬網路有資源群組範圍，這表示在指定的資源群組中，只能有一個名為 `vnet-prod-westus-001` 的網路。 但其他資源群組可以有自己名為 `vnet-prod-westus-001` 的虛擬網路。 再舉另一個例子，子網路會包含在虛擬網路的範圍內，這表示虛擬網路中的每個子網都必須有唯一的名稱。

某些資源名稱 (例如具有公用端點或虛擬機器 DNS 標籤的 PaaS 服務) 具有全域範圍，這表示這些名稱在整個 Azure 平台中必須是唯一的。

資源名稱有長度限制。 當您開發命名慣例時，透過其範圍和長度來平衡名稱中的內嵌內容是很重要的。 若要深入了解命名規則中各資源類型允許的字元、範圍和名稱長度，請參閱 [Azure 資源的命名慣例](https://docs.microsoft.com/azure/architecture/best-practices/resource-naming)。

#### <a name="recommended-naming-components"></a>建議的命名元件

當您建構命名慣例時，應找出您想要在資源名稱中反映的重要資訊片段。 不同資訊會與不同資源類型相關。 下列清單會提供建構資源名稱時實用的資訊範例。

請盡量縮短命名元件的長度，避免超過資源名稱的長度限制。

| 命名元件 | 說明 | 範例 |
| --- | --- | --- |
| 業務單位 | 您公司中擁有該資源所屬訂用帳戶或工作負載的最上層部門。 在小型組織中，此元件可能是單一公司的頂層組織元素。 | *fin*, *mktg*, *product*, *it*, *corp* |
| 訂用帳戶類型 | 資源所在訂用帳戶的用途摘要描述。 通常會依部署環境類型或特定工作負載來細分。 | *prod,* s*hared, client* |
| 應用程式或服務名稱 | 資源所屬應用程式、工作負載或服務的名稱。 | *navigator*, *emissions*, *sharepoint*, *hadoop* |
| 部署環境 | 資源所支援工作負載的開發週期階段。 | *prod, dev, qa, stage, test* |
| 地區 | 部署資源所在的 Azure 區域。 | *westus, eastus2, westeurope, usgovia* |

#### <a name="recommended-resource-type-prefixes"></a>建議的資源類型首碼

每個工作負載都可以包含許多個別資源和服務。 將資源類型首碼併入您的資源名稱，可讓您更輕鬆地以視覺化方式識別應用程式或服務元件。

下列清單提供當您定義命名慣例時，可使用的建議 Azure 資源類型首碼。

| 資源類型                       | 資源名稱首碼 |
| ----------------------------------- | -------------------- |
| 資源群組                      | rg-                  |
| Azure 虛擬網路               | vnet-                |
| 虛擬網路閘道             | vnetgw-              |
| 閘道連線                  | cn-                  |
| 子網路                              | snet-                |
| 網路安全性群組              | nsg-                 |
| 路由表                         | 料               |
| Azure 虛擬機器              | vm-                  |
| VM 儲存體帳戶                  | stvm                 |
| 公用 IP                           | pip-                 |
| Azure Load Balancer                 | lb-                  |
| NIC                                 | nic-                 |
| Azure Key Vault                     | kv                  |
| Azure Kubernetes Service            | aks                 |
| Azure 服務匯流排                   | sb-                  |
| Azure 服務匯流排佇列            | sbq-                 |
| Azure 服務匯流排主題            | sbt                 |
| Azure App Service 方案             | 圖                |
| Azure Web Apps                      | 相關                 |
| Azure Functions                     | func                |
| Azure 雲端服務                | 會                 |
| Azure SQL Database 伺服器           | server                 |
| Azure SQL Database                  | sqldb-               |
| Azure Cosmos DB                     | cosmos              |
| Azure Cache for Redis               | redis-               |
| 適用於 MySQL 的 Azure 資料庫            | mysql-               |
| 適用於 PostgreSQL 的 Azure 資料庫       | psql                |
| Azure SQL 資料倉儲            | sqldw-               |
| SQL Server Stretch Database         | sqlstrdb-            |
| Azure 儲存體                       | st                   |
| Azure StorSimple                    | ssimp                |
| Azure 搜尋服務                        | srch-                |
| Azure 認知服務            | 齒輪                 |
| Azure Machine Learning 工作區    | mlw-                 |
| Azure Data Lake 儲存體             | dls                  |
| Azure Data Lake Analytics           | dla                  |
| Azure HDInsight - Spark             | hdis-                |
| Azure HDInsight - Hadoop            | hdihd-               |
| Azure HDInsight - R Server          | hdir-                |
| Azure HDInsight - HBase             | hdihb-               |
| Power BI Embedded                   | pbi                 |
| Azure 串流分析              | asa-                 |
| Azure Data Factory                  | adf                 |
| Azure 事件中樞                    | evh-                 |
| Azure IoT 中心                       | iot                 |
| Azure 通知中樞             | pubnames.ntf                 |
| Azure 通知中樞命名空間   | ntfns-               |

### <a name="metadata-tags"></a>中繼資料標記

將中繼資料標記套用到雲端資源時，您可以包含資源名稱中無法包含的資產相關資訊。 您可以使用該資訊來對資源執行更複雜的篩選和報告。 建議您讓這些標記包含資源相關的工作負載或應用程式、營運需求和擁有權資訊等內容。 IT 或業務小組可以使用這些資訊來尋找資源，或產生資源使用量和計費的相關報告。

您要套用至資源的標記，以及必要標記或選擇性標記的判定，會因為組織不同而有所差異。 下列清單提供可捕捉資源重要內容和相關資訊的常見標記範例。 請使用此清單作為建立您自有標記慣例的起點。

| 標記名稱                  | 說明                                                                                                                                                                                                    | 索引鍵               | 範例值                                   |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|-------------------------------------------------|
| 應用程式名稱          | 與資源相關聯的應用程式、服務或工作負載名稱。                                                                                                                                 | *ApplicationName* | *{應用程式名稱}*                                    |
| 核准者名稱             | 負責核准此資源相關成本的人員。                                                                                                                                               | *Approver*        | *{電子郵件}*                                       |
| 需要的/核准的預算  | 配置給此應用程式、服務或工作負載的金額。                                                                                                                                                    | *BudgetAmount*    | *{\$}*                                          |
| 業務單位             | 您公司中擁有該資源所屬訂用帳戶或工作負載的最上層部門。 在小型組織中，此標記可能是單一公司或共用的頂層組織元素。 | *BusinessUnit*    | *FINANCE, MARKETING,{產品名稱}, CORP, SHARED* |
| 成本中心               | 與此資源相關聯的會計成本中心。                                                                                                                                                          | *CostCenter*      | *{數字}*                                      |
| 災害復原         | 應用程式、工作負載或服務的業務關鍵性。                                                                                                                                                | *DR*              | *Mission-critical, Critical, Essential*         |
| 專案的結束日期   | 排定淘汰應用程式、工作負載或服務的日期。                                                                                                                                  | *EndDate*         | *{日期}*                                        |
| 環境               | 應用程式、工作負載或服務的部署環境。                                                                                                                                              | *Env*             | *Prod, Dev, QA, Stage, Test*                    |
| 擁有人名稱                | 應用程式、工作負載或服務的擁有者。                                                                                                                                                                | *擁有者*           | *{電子郵件}*                                       |
| 要求者名稱            | 要求建立此應用程式的使用者。                                                                                                                                                          | *Requestor*       | *{電子郵件}*                                       |
| 服務類別             | 應用程式、工作負載或服務的服務等級協定層級。                                                                                                                                       | *ServiceClass*    | *Dev, Bronze, Silver, Gold*                     |
| 專案的開始日期 | 初次部署應用程式、工作負載或服務的日期。                                                                                                                                           | *StartDate*       | *{日期}*                                        |

## <a name="sample-naming-convention"></a>命名慣例的範例

下一節會針對部署企業雲端期間部署的一般 Azure 資源類型，提供命名配置的範例。

<!-- markdownlint-disable MD033 -->

### <a name="subscriptions"></a>訂閱

| 資產類型   | 範圍                        | [格式]                                             | 範例                                     |
|--------------|------------------------------|----------------------------------------------------|----------------------------------------------|
| 訂閱 | 帳戶/Enterprise 合約 | \<業務單位\>-\<訂用帳戶名稱\>-\<\#\#\#\> | <ul><li>mktg-prod-001 </li><li>corp-shared-001 </li><li>fin-client-001</li></ul> |

### <a name="resource-groups"></a>資源群組

| 資產類型     | 範圍        | [格式]                                                     | 範例                                                                            |
|----------------|--------------|------------------------------------------------------------|-------------------------------------------------------------------------------------|
| 資源群組 | 訂閱 | rg\<應用程式或服務名稱\>-\<訂用帳戶類型\>-\<\#\#\#\> | <ul><li>rg-mktgsharepoint-prod-001 </li><li>rg-acctlookupsvc-share-001 </li><li>rg-ad-dir-services-shared-001</li></ul> |

### <a name="virtual-networking"></a>虛擬網路

| 資產類型               | 範圍           | [格式]                                                                | 範例                                                                                              |
|--------------------------|-----------------|-----------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| Azure 虛擬網路          | 資源群組  | vnet-\<訂用帳戶類型\>-\<區域\>-\<\#\#\#\>                      | <ul><li>vnet-shared-eastus2-001 </li><li>vnet-prod-westus-001 </li><li>vnet-client-eastus2-001</li></ul>                                  |
| 虛擬網路的虛擬閘道     | 虛擬網路 | vnetgw-v-\<訂用帳戶類型\>-\<區域\>-\<\#\#\#\>                 | <ul><li>vnet-gw-v-shared-eastus2-001 </li><li>vnet-gw-v-prod-westus-001 </li><li>vnet-gw-v-client-eastus2-001</li></ul>                   |
| 虛擬網路的區域閘道       | 虛擬閘道 | vnetgw-l-\<訂用帳戶類型\>-\<區域\>-\<\#\#\#\>                 | <ul><li>vnet-gw-l-shared-eastus2-001 </li><li>vnet-gw-l-prod-westus-001 </li><li>vnet-gw-l-client-eastus2-001</li></ul>                   |
| 站對站連線 | 資源群組  | cn-\<區域閘道名稱\>-to-\<虛擬閘道名稱\>                 | <ul><li>cn-l-gw-shared-eastus2-001-to-v-gw-shared-eastus2-001 </li><li>cn-l-gw-shared-eastus2-001-to-shared-westus-001</li></ul> |
| 虛擬網路連線         | 資源群組  | cn-\<訂用帳戶 1\>\<區域 1\>-to-\<訂用帳戶 2\>\<區域 2\>-      | <ul><li>cn-shared-eastus2-to-shared-westus </li><li>cn-prod-eastus2-to-prod-westus</li></ul>                                     |
| 子網路                   | 虛擬網路 | snet-\<訂用帳戶\>-\<子區域\>-\<\#\#\#\>                       | <ul><li>snet-shared-eastus2-001 </li><li>snet-prod-westus-001 </li><li>snet-client-eastus2-001</li></ul>                                  |
| 網路安全性群組   | 子網或 NIC   | nsg-\<原則名稱或應用程式名稱\>-\<\#\#\#\>                             | <ul><li>nsg-weballow-001 </li><li>nsg-rdpallow-001 </li><li>nsg-sqlallow-001 </li><li>nsg-dnsbloked-001</li></ul>                                  |
| 公用 IP                | 資源群組  | pip-\<VM 名稱或應用程式名稱\>-\<環境\>-\<子區域\>-\<\#\#\#\> | <ul><li>pip-dc1-shared-eastus2-001 </li><li>pip-hadoop-prod-westus-001</li></ul>                                                 |

### <a name="azure-virtual-machines"></a>Azure 虛擬機器

| 資產類型         | 範圍          | [格式]                                                              | 範例                                                                             |
|--------------------|----------------|---------------------------------------------------------------------|--------------------------------------------------------------------------------------|
| Azure 虛擬機器    | 資源群組 | vm\<原則名稱或應用程式名稱\>\<\#\#\#\>                              | <ul><li>vmnavigator001 </li><li>vmsharepoint001 </li><li>vmsqlnode001 </li><li>vmhadoop001</li></ul>                              |
| VM 儲存體帳戶 | 全球         | stvm\<效能類型\>\<應用程式名稱或產品名稱\>\<區域\>\<\#\#\#\> | <ul><li>stvmstcoreeastus2001 </li><li>stvmpmcoreeastus2001 </li><li>stvmstplmeastus2001 </li><li>stvmsthadoopeastus2001</li></ul> |
| DNS 標籤          | 全球         | \<M 的記錄\>.[\<區域\>.cloudapp.azure.com]                  | <ul><li>dc1.westus.cloudapp.azure.com </li><li>web1.eastus2.cloudapp.azure.com</li></ul>                        |
| Azure Load Balancer      | 資源群組 | lb-\<應用程式名稱或角色\>\<環境\>\<\#\#\#\>                    | <ul><li>lb-navigator-prod-001 </li><li>lb-sharepoint-dev-001</li></ul>                                          |
| NIC                | 資源群組 | nic-\<\#\#\>-\<VM 名稱\>-\<訂用帳戶\>\<\#\#\#\>                  | <ul><li>nic-01-dc1-shared-001 </li><li>nic-02-vmhadoop1-prod-001 </li><li>nic-02-vmtest1-client-001</li></ul>            |

### <a name="paas-services"></a>PaaS 服務

| 資產類型           | 範圍  | [格式]                                                              | 範例                                                                                 |
|----------------------|--------|---------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| Azure Web Apps       | 全球 | 應用程式\<應用程式名稱\>-\<環境\>-\<\#\#\#\>。[{azurewebsites.net}] | <ul><li>azapp-navigator-prod-001.azurewebsites.net </li><li>app-accountlookup-dev-001.azurewebsites.net</li></ul> |
| Azure Functions      | 全球 | func-\<應用程式名稱\>-\<環境\>-\<\#\#\#\>。[{azurewebsites.net}] | <ul><li>azfun-navigator-prod-001.azurewebsites.net </li><li>func-accountlookup-dev-001.azurewebsites.net</li></ul> |
| Azure 雲端服務 | 全球 | \<應用程式名稱\>-\<環境\>-\<\#\#\#\>。[{cloudapp.net}]       | <ul><li>azcs-navigator-prod-001.azurewebsites.net </li><li>could-accountlookup-dev-001.azurewebsites.net</li></ul>   |

### <a name="azure-service-bus"></a>Azure 服務匯流排

| 資產類型         | 範圍       | [格式]                                                     | 範例                           |
|--------------------|-------------|------------------------------------------------------------|------------------------------------|
| Azure 服務匯流排        | 全球      | sb-\<應用程式名稱\>-\<環境\>.[{servicebus.windows.net}] | <ul><li>sb-navigator-prod </li><li>sb-emissions-dev</li></ul> |
| Azure 服務匯流排佇列 | 服務匯流排 | sbq-\<查詢描述項\>                                   | <ul><li>sbq-messagequery</li></ul>                   |
| Azure 服務匯流排主題 | 服務匯流排 | sbt-\<查詢描述項\>                                   | <ul><li>sbt-messagequery</li></ul>                   |

### <a name="databases"></a>資料庫

| 資產類型                          | 範圍              | [格式]                                | 範例                                       |
|-------------------------------------|--------------------|---------------------------------------|------------------------------------------------|
| Azure SQL Database 伺服器           | 全球             | sql\<應用程式名稱\>-\<環境\>      | <ul><li>sql-navigator-生產環境 </li><li>sql-排放-開發</li></ul>           |
| Azure SQL Database                  | Azure SQL Database | sqldb-\<資料庫名稱 >-\<環境\>| <ul><li>sqldb-使用者-生產環境 </li><li>sqldb-使用者-開發人員</li></ul>               |
| Azure Cosmos DB                     | 全球             | cosmos-\<應用程式名稱\>-\<環境\>   | <ul><li>cosdb-navigator-prod </li><li>cosdb-emissions-dev</li></ul>       |
| Azure Cache for Redis               | 全球             | redis-\<應用程式名稱\>-\<環境\>    | <ul><li>redis-navigator-prod </li><li>redis-emissions-dev</li></ul>       |
| 適用於 MySQL 的 Azure 資料庫            | 全球             | mysql-\<應用程式名稱\>-\<環境\>    | <ul><li>mysql-navigator-prod </li><li>mysql-emissions-dev</li></ul>       |
| 適用於 PostgreSQL 的 Azure 資料庫       | 全球             | psql-\<應用程式名稱\>-\<環境\>     | <ul><li>psql-navigator-生產環境 </li><li>psql-排放-開發</li></ul>         |
| Azure SQL 資料倉儲            | 全球             | sqldw-\<應用程式名稱\>-\<環境\>    | <ul><li>sqldw-navigator-prod </li><li>sqldw-emissions-dev</li></ul>       |
| SQL Server Stretch Database         | Azure SQL Database | sqlstrdb-\<應用程式名稱\>-\<環境\> | <ul><li>sqlstrdb-navigator-prod </li><li>sqlstrdb-emissions-dev</li></ul> |

### <a name="storage"></a>儲存體

| 資產類型                              | 範圍  | [格式]                                                                        | 範例                                   |
|-----------------------------------------|--------|-------------------------------------------------------------------------------|--------------------------------------------|
| Azure 儲存體帳戶 - 一般用途     | 全球 | st\<儲存體名稱\>\<\#\#\#\>                                                  | <ul><li>stnavigatordata001 </li><li>stemissionsoutput001</li></ul>    |
| Azure 儲存體帳戶 - 診斷記錄 | 全球 | stdiag\<訂用帳戶名稱的前兩個字母和數字\>\<區域\>\<\#\#\#\> | <ul><li>stdiagsh001eastus2001 </li><li>stdiagsh001westus001</li></ul> |
| Azure StorSimple                        | 全球 | ssimp\<應用程式名稱\>\<環境\>                                              | <ul><li>ssimpnavigatorprod </li><li>ssimpemissionsdev</li></ul>       |

### <a name="ai--machine-learning"></a>AI + 機器學習服務

| 資產類型                       | 範圍          | [格式]                            | 範例                               |
|----------------------------------|----------------|-----------------------------------|----------------------------------------|
| Azure 搜尋服務                     | 全球         | srch-\<應用程式名稱\>-\<環境\> | <ul><li>srch-navigator-prod </li><li>srch-emissions-dev</li></ul> |
| Azure 認知服務         | 資源群組 | 齒輪-\<應用程式名稱\>-\<環境\>   | <ul><li>齒輪-navigator-生產環境 </li><li>齒輪-排放-開發</li></ul>     |
| Azure Machine Learning 工作區 | 資源群組 | mlw-\<應用程式名稱\>-\<環境\>   | <ul><li>mlw-navigator-生產環境 </li><li>mlw-排放-開發</li></ul>     |

### <a name="analytics"></a>分析

| 資產類型                | 範圍  | [格式]                             | 範例                                 |
|---------------------------|--------|------------------------------------|------------------------------------------|
| Azure Data Factory        | 全球 | adf-\<應用程式名稱\>\<環境\>    | <ul><li>adf-navigator-生產環境 </li><li>adf-排放-開發</li></ul>     |
| Azure Data Lake 儲存體   | 全球 | dls\<應用程式名稱\>\<環境\>     | <ul><li>dlsnavigatorprod </li><li>dlsemissionsdev</li></ul>         |
| Azure Data Lake Analytics | 全球 | dla\<應用程式名稱\>\<環境\>     | <ul><li>dlanavigatorprod </li><li>dlaemissionsdev</li></ul>         |
| Azure HDInsight - Spark   | 全球 | hdis-\<應用程式名稱\>-\<環境\>  | <ul><li>hdis-navigator-prod </li><li>hdis-emissions-dev </li></ul>  |
| Azure HDInsight - Hadoop  | 全球 | hdihd-\<應用程式名稱\>-\<環境\> | <ul><li>hdihd-hadoop-prod </li><li>hdihd-emissions-dev</li></ul>    |
| Azure HDInsight - R Server| 全球 | hdir-\<應用程式名稱\>-\<環境\>  | <ul><li>hdir-navigator-prod </li><li>hdir-emissions-dev</li></ul>   |
| Azure HDInsight - HBase   | 全球 | hdihb-\<應用程式名稱\>-\<環境\> | <ul><li>hdihb-navigator-prod </li><li>hdihb-emissions-dev</li></ul> |
| Power BI Embedded         | 全球 | pbi-\<應用程式名稱\>\<環境\>    | <ul><li>pbi-navigator-生產環境 </li><li>pbi-排放-開發</li></ul> |

### <a name="data-streams--internet-of-things-iot"></a>資料流程/物聯網（IoT）

| 資產類型                         | 範圍          | [格式]                             | 範例                                 |
|------------------------------------|----------------|------------------------------------|------------------------------------------|
| Azure 串流分析             | 資源群組 | asa-\<應用程式名稱\>-\<環境\>   | <ul><li>asa-navigator-prod </li><li>asa-emissions-dev</li></ul>     |
| Azure IoT 中心                      | 全球         | iot\<應用程式名稱\>-\<環境\>   | <ul><li>iot-navigator-生產環境 </li><li>iot-排放-開發</li></ul>     |
| Azure 事件中樞                   | 全球         | evh-\<應用程式名稱\>-\<環境\>   | <ul><li>evh-navigator-prod </li><li>evh-emissions-dev</li></ul>     |
| Azure 通知中樞            | 資源群組 | pubnames.ntf-\<應用程式名稱\>-\<環境\>   | <ul><li>pubnames.ntf-navigator-生產環境 </li><li>pubnames.ntf-排放-開發</li></ul>     |
| Azure 通知中樞命名空間  | 全球         | ntfns-\<應用程式名稱\>-\<環境\> | <ul><li>ntfns-navigator-生產環境 </li><li>ntfns-排放-開發</li></ul> |
