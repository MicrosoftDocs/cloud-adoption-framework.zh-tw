---
title: 建議的命名和標記慣例
description: 瞭解專為支援企業雲端採用工作而設計的詳細資源命名和標記建議。
author: BrianBlanchard
ms.author: brblanch
ms.date: 03/05/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: readiness, fasttrack-edit
ms.openlocfilehash: 9c1377bfed389d435403f9f72dd6ac4e48ed8ee8
ms.sourcegitcommit: 8e5b670151cc8da0934037e23a1ef1609c6b2cc2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/09/2020
ms.locfileid: "94378931"
---
<!-- docutune:disable -->
<!-- cSpell:ignore appcs arck cdnp cdne osdisk westeurope usgovia accountlookup messagequery -->

# <a name="recommended-naming-and-tagging-conventions"></a>建議的命名和標記慣例

組織您的雲端資產，以支援營運管理和會計需求。 定義完善的命名和元資料標記慣例有助於快速找出及管理資源。 這些慣例也有助於透過計費和回報會計機制，將雲端使用量成本與商務小組產生關聯。

確切的資源表示和命名是安全性目的不可或缺的。 發生安全性事件時，迅速找出受影響的系統、其潛在的業務衝擊，以及它們所使用的內容，對於做出良好的風險決策而言很重要。 安全性服務（例如 [Azure 資訊安全中心](https://docs.microsoft.com/azure/security-center/security-center-introduction) 和 [Azure Sentinel](https://docs.microsoft.com/azure/sentinel) 參考資源）及其相關聯的記錄/警示資訊（依資源名稱）。

Azure 會定義 [azure 資源的命名規則和限制](/azure/azure-resource-manager/management/resource-name-rules)。 本指引提供詳細的建議，以支援企業雲端採用工作。

變更資源名稱可能很困難。 在開始進行任何大型雲端部署之前，請先建立完整的命名慣例。

> [!NOTE]
> 每個企業都有不同的組織和管理需求。 這些建議會提供在您的雲端採用小組內進行討論的起點。
>
> 當這些討論繼續進行時，請使用下列範本，以在將這些建議符合您特定的業務需求時，掌握您所進行的命名和標記決定。
>
> 下載 [命名和標記慣例追蹤範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/ready/naming-and-tagging-conventions-tracking-template.xlsx)。

## <a name="naming-and-tagging-resources"></a>命名與標記資源

命名和標記策略包含業務和營運詳細資料，這些是資源名稱和中繼資料標籤的元件：

此策略的商務端可確保資源名稱和標記包含識別小組所需的組織資訊。 您可以與負責資源成本的業務擁有者一起使用資源。

在營運方面，可確保名稱和標記包含 IT 小組用來識別工作負載、應用程式、環境和重要性的資訊，以及用來管理資源的其他實用資訊。

## <a name="resource-naming"></a>資源命名

有效的命名慣例會使用重要資源資訊作為資源名稱的一部分，來組合資源名稱。 例如，使用這些 [建議的命名慣例](#example-names)，生產 SharePoint 工作負載的公用 IP 資源的命名方式如下： `pip-sharepoint-prod-westus-001` 。

從名稱中，您可以快速地識別出資源類型、其相關聯的工作負載、其部署環境，以及裝載該資源的 Azure 區域。

### <a name="naming-scope"></a>命名範圍

所有 Azure 資源類型都有一個範圍，可定義資源名稱必須是唯一的層級。 資源的範圍內必須有唯一的名稱。

例如，虛擬網路有資源群組範圍，這表示在指定的資源群組中，只能有一個名為 `vnet-prod-westus-001` 的網路。 其他資源群組可以有自己的虛擬網路，名為 `vnet-prod-westus-001` 。 子網的範圍設定為虛擬網路，因此虛擬網路中的每個子網都必須具有唯一的名稱。

某些資源名稱 (例如具有公用端點或虛擬機器 DNS 標籤的 PaaS 服務) 具有全域範圍，這表示這些名稱在整個 Azure 平台中必須是唯一的。

資源名稱有長度限制。 當您開發命名慣例時，透過其範圍和長度來平衡名稱中的內嵌內容是很重要的。 如需詳細資訊，請參閱 [Azure 資源的命名規則和限制](/azure/azure-resource-manager/management/resource-name-rules)。

### <a name="recommended-naming-components"></a>建議的命名元件

當您建構命名慣例時，應找出您想要在資源名稱中反映的重要資訊片段。 不同資訊會與不同資源類型相關。 下列清單會提供建構資源名稱時實用的資訊範例。

請盡量縮短命名元件的長度，避免超過資源名稱的長度限制。

| 命名元件            | 描述                                                                                                                                                                                                      | 範例                                         |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------|
| 業務單位               | 您公司中擁有該資源所屬訂用帳戶或工作負載的最上層部門。 在小型組織中，此元件可能是單一公司的頂層組織元素。 | `fin`, `mktg`, `product`, `it`, `corp`           |
| 訂用帳戶類型           | 資源所在訂用帳戶的用途摘要描述。 通常會依部署環境類型或特定工作負載來細分。                                                       | `prod`, `shared`, `client`                       |
| 應用程式或服務名稱 | 資源所屬應用程式、工作負載或服務的名稱。                                                                                                                                    | `navigator`, `emissions`, `sharepoint`, `hadoop` |
| 部署環境      | 資源所支援工作負載的開發週期階段。                                                                                                                              | `prod`, `dev`, `qa`, `stage`, `test`             |
| 區域                      | 部署資源所在的 Azure 區域。                                                                                                                                                                 | `westus`, `eastus2`, `westeurope`, `usgovia`     |

### <a name="recommended-resource-type-prefixes"></a>建議的資源類型首碼

每個工作負載都可以包含許多個別資源和服務。 將資源類型首碼併入您的資源名稱，可讓您更輕鬆地以視覺化方式識別應用程式或服務元件。

這份清單建議您定義命名慣例時要使用的 Azure 資源類型首碼。

<!-- cSpell:ignore apim snet traf vmss stvm arcm ntfns sqldb psql sqldw sqlstrdb ssimp srch hbase appi migr -->

### <a name="general"></a>一般

| 資產類型                      | 名稱前置詞 |
|---------------------------------|-------------|
| 管理群組                | mg         |
| 資源群組                  | rg-         |
| 原則定義               | 策略     |
| API 管理服務實例 | apim       |
| 受控識別                | 識別碼         |

### <a name="networking"></a>網路功能

| 資產類型                            | 名稱前置詞 |
|---------------------------------------|-------------|
| 虛擬網路                       | vnet-       |
| 子網路                                | snet-       |
| 虛擬網路對等互連               | 等       |
| 網路介面 (NIC)               | nic-        |
| 公用 IP 位址                     | pip-        |
| 負載平衡器 (內部)               | lbi-        |
| 負載平衡器 (外部)               | lbe-        |
| 網路安全性群組 (NSG)          | nsg-        |
|  (ASG) 的應用程式安全性群組      | asg        |
| 區域網路閘道                 | lgw-        |
| 虛擬網路閘道               | vgw-        |
| VPN 連線                        | cn-         |
| ExpressRoute 線路                  | erc-        |
| 應用程式閘道                   | agw-        |
| 路由表                           | 往      |
| 使用者定義的路由 (UDR)               | udr        |
| 流量管理員設定檔               | traf-       |
| Front Door                            | fd         |
| CDN 設定檔                           | cdnp-       |
| CDN 端點                          | cdne-       |
| Web 應用程式防火牆 (WAF) 原則 | waf         |

### <a name="compute-and-web"></a>計算和網路

| 資產類型 | 名稱前置詞 |
|--|--|
| 虛擬機器 | vm |
| 虛擬機器擴展集 | vmss |
| 可用性設定組 | 可用性/mb |
| 受控磁片 (OS)  | osdisk |
| 受控磁片 (資料)  | disk |
| VM 儲存體帳戶 | stvm |
| Azure Arc 啟用的伺服器 | 圓弧 |
| Azure Arc 啟用的 Kubernetes 叢集 | arck |
| 容器登錄 | 鉻 |
| 容器實例 | ci |
| AKS 叢集 | aks |
| Service Fabric 叢集 | sf |
| App Service 環境 | ase |
| App Service 方案 | 餐 |
| Web 應用程式 | 應用 |
| 函式應用程式 | func |
| 雲端服務 | 無法 |
| 通知中樞 | contoso.ntf |
| 通知中樞命名空間 | ntfns- |

### <a name="databases"></a>資料庫

| 資產類型                     | 名稱前置詞 |
|--------------------------------|-------------|
| Azure SQL Database 伺服器      | .sql        |
| Azure SQL 資料庫             | sqldb-      |
| Azure Cosmos DB 資料庫       | cosmos     |
| Azure Cache for Redis 實例 | redis-      |
| MySQL 資料庫                 | mysql-      |
| PostgreSQL 資料庫            | psql       |
| Azure SQL 資料倉儲       | sqldw-      |
| Azure Synapse Analytics        | syn        |
| SQL Server Stretch Database    | sqlstrdb-   |
| SQL 受控執行個體           | sqlmi      |

### <a name="storage"></a>儲存體

| 資產類型               | 名稱前置詞 |
|--------------------------|-------------|
| 儲存體帳戶          | st          |
| Azure StorSimple         | ssimp       |
| Azure Container Registry | acr         |

### <a name="ai-and-machine-learning"></a>AI 與機器學習

| 資產類型                       | 名稱前置詞 |
|----------------------------------|-------------|
| Azue 認知搜尋           | srch-       |
| Azure 認知服務         | 齒輪        |
| Azure Machine Learning 工作區 | mlw-        |

### <a name="analytics-and-iot"></a>分析和 IoT

|資產類型 |名稱前置詞 | |---------------------------------_ |-------------| |Azure Analysis Services server |as | |Azure Databricks 工作區 |dbw-| |Azure 串流分析 |asa-| |Azure 資料總管叢集 |dec | |Azure Data Factory |adf-| |Data Lake Store 帳戶 |dls | |Data Lake Analytics 帳戶 |dla | |事件中樞 |evh-| |HDInsight-Hadoop 叢集 |hadoop-| |HDInsight-HBase 叢集 |hbase-| |HDInsight-Kafka 叢集 |kafka-| |HDInsight-Spark 叢集 |spark-| |HDInsight-風暴叢集 |風暴-| |HDInsight-ML 服務叢集 |mls-| |IoT 中樞 |iot-| |Power BI Embedded |pbi-| |時間序列深入解析環境 |tsi-|

### <a name="developer-tools"></a>開發人員工具

| 資產類型 | 名稱前置詞 |
|---|---|
| 應用程式設定存放區 | appcs- |

### <a name="integration"></a>整合

| 資產類型          | 名稱前置詞 |
|---------------------|-------------|
| 整合帳戶 | ia         |
| 邏輯應用程式          | 邏輯設計      |
| 服務匯流排         | sb-         |
| 服務匯流排佇列   | sbq-        |
| 服務匯流排主題   | sbt        |

### <a name="management-and-governance"></a>管理和治理

| 資產類型 | 名稱前置詞 |
|--|--|
| 自動化帳戶 | aa |
| Azure 監視器動作群組 | ag |
| 藍圖 | bp |
| 藍圖指派 | bpa |
| 金鑰保存庫 | kv |
| Log Analytics 工作區 | 記錄 |
| Application Insights | appi- |
| 復原服務保存庫 | rsv- |

### <a name="migration"></a>遷移

| 資產類型                          | 名稱前置詞 |
|-------------------------------------|-------------|
| Azure Migrate 專案               | 移       |
| 資料庫移轉服務實例 | dm        |
| 復原服務保存庫             | rsv-        |

<!-- cSpell:enable -->

## <a name="metadata-tags"></a>中繼資料標記

將中繼資料標記套用到雲端資源時，您可以包含資源名稱中無法包含的資產相關資訊。 您可以使用該資訊來對資源執行更複雜的篩選和報告。 建議您讓這些標記包含資源相關的工作負載或應用程式、營運需求和擁有權資訊等內容。 IT 或業務小組可以使用這些資訊來尋找資源，或產生資源使用量和計費的相關報告。

您要套用至資源的標記，以及必要標記或選擇性標記的判定，會因為組織不同而有所差異。 下列清單提供可捕捉資源重要內容和相關資訊的常見標記範例。 請使用此清單作為建立您自有標記慣例的起點。

| 標記名稱                  | 描述                                                                                                                                                                                                          | Key               | 範例值                                              |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|------------------------------------------------------------|
| 應用程式名稱          | 與資源相關聯的應用程式、服務或工作負載名稱。                                                                                                                                       | _ApplicationName_ | _{應用程式名稱}_                                               |
| 核准者名稱             | 負責核准此資源相關成本的人員。                                                                                                                                                     | _人員_        | _電子郵件_                                                  |
| 需要的/核准的預算  | 配置給此應用程式、服務或工作負載的金額。                                                                                                                                                          | _BudgetAmount_    | _{\$}_                                                     |
| 業務單位             | 您公司中擁有該資源所屬訂用帳戶或工作負載的最上層部門。 在小型組織中，此標記可能是單一公司或共用的頂層組織元素。 | _BusinessUnit_    | _財務_ 、 _行銷_ 、 _{產品名稱}_ 、 _CORP_ 、 _共用_ |
| 成本中心               | 與此資源相關聯的會計成本中心。                                                                                                                                                                | _CostCenter_      | _數量_                                                 |
| 災害復原         | 應用程式、工作負載或服務的業務關鍵性。                                                                                                                                                       | _DR_              | _任務關鍵性_ 、 _關鍵_ 、 _重要_                |
| 專案的結束日期   | 排定淘汰應用程式、工作負載或服務的日期。                                                                                                                                         | _日期_         | _更新_                                                   |
| 環境               | 應用程式、工作負載或服務的部署環境。                                                                                                                                                     | _Env_             | _生產_ 、 _開發_ 、 _QA_ 、 _階段_ 、 _測試_                       |
| 擁有人名稱                | 應用程式、工作負載或服務的擁有者。                                                                                                                                                                      | _擁有者_           | _電子郵件_                                                  |
| 要求者名稱            | 要求建立此應用程式的使用者。                                                                                                                                                                 | _要求者_       | _電子郵件_                                                  |
| 服務類別             | 應用程式、工作負載或服務的服務等級協定層級。                                                                                                                                              | _ServiceClass_    | _Dev_ 、 _銅牌_ 、 _銀_ 級、 _金_ 級                          |
| 專案的開始日期 | 初次部署應用程式、工作負載或服務的日期。                                                                                                                                                  | _起始_       | _更新_                                                   |

## <a name="example-names"></a>範例名稱

下一節提供企業雲端部署中常見 Azure 資源類型的一些範例名稱。

<!-- TODO: Use tick marks for names. -->

<!-- cSpell:ignore mktgsharepoint acctlookupsvc vmhadoop vmtest vmsharepoint vmnavigator vmsqlnode stvmstcoreeastus stvmpmcoreeastus stvmstplmeastus stvmsthadoopeastus stnavigatordata stemissionsoutput stdiag stdiagsh ssimpnavigatorprod ssimpemissionsdev dlanavigatorprod dlsnavigatorprod dlaemissionsdev dlsemissionsdev weballow rdpallow sqlallow dnsblocked cloudapp azurewebsites servicebus appcn keda acrnavigatorprod -->

<!-- markdownlint-disable MD024 -->

### <a name="example-names-general"></a>範例名稱：一般

| 資產類型                      | 影響範圍                                 | [格式]                                                      | 範例                                                                                           |
|---------------------------------|---------------------------------------|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| 管理群組                | 業務單位及/或環境類型 | mg\<Business Unit\>\[-\<Environment type\>\]               | <li> mg-mktg <li> mg-小時 <li> mg-corp-生產 <li> mg-fin-用戶端                                       |
| 訂用帳戶                    | 帳號 <br> Enterprise 合約    | \<Business Unit\>-\<Subscription type\>-\<\#\#\#\>          | <li> mktg-prod-001 <li> corp-shared-001 <li> fin-client-001                                        |
| 資源群組                  | 訂用帳戶                          | rg\<App or service name\>-\<Subscription type\>-\<\#\#\#\> | <li> rg-mktgsharepoint-prod-001 <li> rg-acctlookupsvc-share-001 <li> rg-ad-dir-services-shared-001 |
| API 管理服務實例 | 全球                                | apim\<App or service name\>                                | apim-navigator-生產                                                                                |
| 受控識別                | 資源群組                        | 識別碼\<App or service name\>                                  | 識別碼-appcn-keda-生產-eus-001                                                                         |

> [!NOTE]
> 本檔中上述和其他位置的範例名稱會參考三位數填補 (\<\#\#\#\>) 。 即。  mktg-生產- *001*
>
> 在設定管理資料庫 (CMDB) 、IT 資產管理工具或傳統的帳戶處理工具中管理資產時，填補有助於人類的可讀性和排序。 當部署的資產集中管理成為較大型清查或 IT 資產組合的一部分時，填補方法會與這些系統用來管理清查命名的介面對齊。
>
> 可惜的是，傳統的資產填補方法可以證明基礎結構即程式碼的方法有問題，這種方法可能會根據非填補的數位來逐一查看資產。 這種方法在部署或自動化設定管理工作期間很常見。 這些腳本必須定期去除填補，然後將填補的數位轉換成實數，這樣會減緩腳本開發和執行時間。
>
> 您選擇要執行哪一種方法是個人的決策。 本文中的填補旨在說明使用一致方法清查編號的重要性，而不是哪一種方法是較佳的方法。 在決定數位架構 (具有或不含填補) 評估哪些會對長期作業產生較大的影響： CMDB/資產管理解決方案或以程式碼為基礎的清查管理。 然後一致地遵循最符合您操作需求的填補選項。

### <a name="example-names-networking"></a>範例名稱：網路

| 資產類型                   | 影響範圍           | [格式]                                                               | 範例                                                                                                                      |
|------------------------------|-----------------|----------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| 虛擬網路              | 資源群組  | vnet\<Subscription type\>-\<Region\>-\<\#\#\#\>                     | <li> vnet-shared-eastus2-001 <li> vnet-prod-westus-001 <li> vnet-client-eastus2-001 |
| 子網路                       | 虛擬網路 | 于\<subscription\>-\<subregion\>-\<\#\#\#\>                       | <li> snet-shared-eastus2-001 <li> snet-prod-westus-001 <li> snet-client-eastus2-001 |
| 網路介面 (NIC)      | 資源群組  | nic\<\#\#\>-\<vm name\>-\<subscription\>-\<\#\#\#\>                   | <li> nic-01-dc1-shared-001 <li> nic-02-vmhadoop1-prod-001 <li> nic-02-vmtest1-client-001 |
| 公用 IP 位址            | 資源群組  | 點數\<vm name or app name\>-\<Environment\>-\<subregion\>-\<\#\#\#\> | <li> pip-dc1-shared-eastus2-001 <li> pip-hadoop-prod-westus-001 |
| 負載平衡器                | 資源群組  | 磅\<app name or role\>\<Environment\>\<\#\#\#\>                     | <li> lb-navigator-prod-001 <li> lb-sharepoint-dev-001 |
| 網路安全性群組 (NSG) | 子網或 NIC   | nsg\<policy name or app name\>-\<\#\#\#\>                           | <li> nsg-weballow-001 <li> nsg-rdpallow-001 <li> nsg-sqlallow-001 <li> nsg-dnsblocked-001 |
| 區域網路閘道        | 虛擬閘道 | lgw-\<Subscription type\>-\<Region\>-\<\#\#\#\>                      | <li> lgw-shared-eastus2-001 <li> lgw-生產-westus-001 <li> lgw-用戶端-eastus2-001 |
| 虛擬網路閘道      | 虛擬網路 | vgw-\<Subscription type\>-\<Region\>-\<\#\#\#\>                      | <li> vgw-shared-eastus2-001 <li> vgw-生產-westus-001 <li> vgw-用戶端-eastus2-001 |
| 站對站連線      | 資源群組  | cn----- \<local gateway name\>\<virtual gateway name\>                | <li> cn-lgw-shared-eastus2-001--vgw-shared-eastus2-001 <li> cn-lgw-eastus2-001-to-shared-westus-001 |
| VPN 連線               | 資源群組  | cn----- \<subscription1\> \<region1\>\<subscription2\>\<region2\>-     | <li> cn-shared-eastus2-to-shared-westus <li> cn-prod-eastus2-to-prod-westus |
| 路由表                  | 資源群組  | 往\<route table name\>                                           | <li> 路由-導覽器 <li> route-sharepoint |
| DNS 標籤                    | 全球          | \<A record of vm\>。 Cloudapp.azure.com <的區域 \> 。                   | <li> dc1.westus.cloudapp.azure.com <li> web1.eastus2.cloudapp.azure.com |

### <a name="example-names-compute-and-web"></a>範例名稱：計算和 Web

| 資產類型                  | 影響範圍          | [格式]                                                              | 範例                                                                                                                          |
|-----------------------------|----------------|---------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|
| 虛擬機器             | 資源群組 | Vm\<policy name or app name\>\<\#\#\#\>                              | <li> vmnavigator001 <li> vmsharepoint001 <li> vmsqlnode001 <li> vmhadoop001 |
| VM 儲存體帳戶          | 全球         | stvm\<performance type\>\<app name or prod name\>\<region\>\<\#\#\#\> | <li> stvmstcoreeastus2001 <li> stvmpmcoreeastus2001 <li> stvmstplmeastus2001 <li> stvmsthadoopeastus2001 |
| Web 應用程式                     | 全球         | 應用程式- \<App Name\> - \<Environment\> - \<\#\#\#\> . [{azurewebsites.net}]   | <li> app-navigator-prod-001.azurewebsites.net <li> app-accountlookup-dev-001.azurewebsites.net |
| 函式應用程式                | 全球         | func- \<App Name\> - \<Environment\> - \<\#\#\#\> . [{azurewebsites.net}]  | <li> func-navigator-prod-001.azurewebsites.net <li> func-accountlookup-dev-001.azurewebsites.net |
| 雲端服務               | 全球         | 可能- \<App Name\> - \<Environment\> - \<\#\#\#\> . [{cloudapp.net}]        | <li> could-navigator-prod-001.azurewebsites.net <li> could-accountlookup-dev-001.azurewebsites.net |
| 通知中樞            | 資源群組 | contoso.ntf\<App Name\>-\<Environment\>                                    | <li> contoso.ntf-navigator-生產 <li> contoso.ntf-排放-開發 |
| 通知中樞命名空間 | 全球         | ntfns-\<App Name\>-\<Environment\>                                  | <li> ntfns-navigator-生產 <li> ntfns-排放-開發 |

### <a name="example-names-databases"></a>範例名稱：資料庫

| 資產類型                     | 影響範圍              | [格式]                                 | 範例                                                                  |
|--------------------------------|--------------------|----------------------------------------|---------------------------------------------------------------------------|
| Azure SQL Database 伺服器      | 全球             | .sql\<App Name\>-\<Environment\>       | <li> sql-導覽器-生產 <li> sql-排放-開發 |
| Azure SQL 資料庫             | Azure SQL Database | sqldb\<Database Name>-\<Environment\> | <li> sqldb-使用者-生產 <li> sqldb-使用者-開發人員 |
| Azure Cosmos DB 資料庫       | 全球             | cosmos\<App Name\>-\<Environment\>    | <li> cosmos-navigator-生產 <li> cosmos-排放-開發 |
| Azure Cache for Redis 實例 | 全球             | redis\<App Name\>-\<Environment\>     | <li> redis-navigator-prod <li> redis-emissions-dev |
| MySQL 資料庫                 | 全球             | mysql\<App Name\>-\<Environment\>     | <li> mysql-navigator-prod <li> mysql-emissions-dev |
| PostgreSQL 資料庫            | 全球             | psql\<App Name\>-\<Environment\>      | <li> psql-navigator-生產 <li> psql-排放-開發 |
| Azure SQL 資料倉儲       | 全球             | sqldw\<App Name\>-\<Environment\>     | <li> sqldw-navigator-prod <li> sqldw-emissions-dev |
| SQL Server Stretch Database    | Azure SQL Database | sqlstrdb-\<App Name\>-\<Environment\>  | <li> sqlstrdb-navigator-prod <li> sqlstrdb-emissions-dev |

### <a name="example-names-storage"></a>範例名稱：儲存體

| 資產類型                        | 影響範圍  | [格式]                                                                        | 範例                                                              |
|-----------------------------------|--------|-------------------------------------------------------------------------------|-----------------------------------------------------------------------|
| 儲存體帳戶 (一般用途)      | 全球 | 聖\<storage name\>\<\#\#\#\>                                                  | <li> stnavigatordata001 <li> stemissionsoutput001 |
| 儲存體帳戶 (診斷記錄)  | 全球 | stdiag\<first 2 letters of subscription name and number\>\<region\>\<\#\#\#\> | <li> stdiagsh001eastus2001 <li> stdiagsh001westus001 |
| Azure StorSimple                  | 全球 | ssimp\<App Name\>\<Environment\>                                              | <li> ssimpnavigatorprod <li> ssimpemissionsdev |
| Azure Container Registry          | 全球 | Acr\<App Name\>\<Environment\>\<\#\#\#\>                                      | <li> acrnavigatorprod001 |

### <a name="example-names-ai-and-machine-learning"></a>範例名稱： AI 和機器學習

| 資產類型                       | 影響範圍          | [格式]                            | 範例                                                          |
|----------------------------------|----------------|-----------------------------------|-------------------------------------------------------------------|
| Azue 認知搜尋           | 全球         | srch-\<App Name\>-\<Environment\> | <li> srch-navigator-prod <li> srch-emissions-dev |
| Azure 認知服務         | 資源群組 | 齒輪\<App Name\>-\<Environment\>  | <li> 齒輪-navigator-生產 <li> 齒輪-排放-開發 |
| Azure Machine Learning 工作區 | 資源群組 | mlw-\<App Name\>-\<Environment\>  | <li> mlw-navigator-生產 <li> mlw-排放-開發 |

### <a name="example-names-analytics-and-iot"></a>範例名稱：分析和 IoT

| 資產類型                  | 影響範圍          | [格式]                              | 範例                                                              |
|-----------------------------|----------------|-------------------------------------|-----------------------------------------------------------------------|
| Azure Data Factory          | 全球         | 放下\<App Name\>\<Environment\>     | <li> adf-導覽-生產 <li> adf-排放-開發 |
| Azure 串流分析      | 資源群組 | asa\<App Name\>-\<Environment\>    | <li> asa-navigator-prod <li> asa-emissions-dev |
| Data Lake Analytics 帳戶 | 全球         | Dla\<App Name\>\<Environment\>      | <li> dlanavigatorprod <li> dlaemissionsdev |
| Data Lake Storage 帳戶   | 全球         | Dls\<App Name\>\<Environment\>      | <li> dlsnavigatorprod <li> dlsemissionsdev |
| 事件中樞                   | 全球         | evh-\<App Name\>-\<Environment\>    | <li> evh-navigator-prod <li> evh-emissions-dev |
| HDInsight-HBase 叢集   | 全球         | hbase\<App Name\>-\<Environment\>  | <li> hbase-navigator-生產 <li> hbase-排放-開發 |
| HDInsight-Hadoop 叢集  | 全球         | hadoop\<App Name\>-\<Environment\> | <li> hadoop-導覽器-生產 <li> hadoop-排放-開發 |
| HDInsight-Spark 叢集   | 全球         | 大家\<App Name\>-\<Environment\>  | <li> spark-導覽器-生產 <li> spark-排放-開發  |
| IoT 中樞                     | 全球         | iot\<App Name\>-\<Environment\>    | <li> iot-navigator-生產 <li> iot-排放-開發 |
| Power BI Embedded           | 全球         | pbi\<App Name\>\<Environment\>     | <li> pbi-navigator-生產 <li> pbi-排放-開發 |

### <a name="example-names-integration"></a>範例名稱：整合

| 資產類型        | 影響範圍       | [格式]                                                     | 範例                                                      |
|-------------------|-------------|------------------------------------------------------------|---------------------------------------------------------------|
| 服務匯流排       | 全球      | sb- \<App Name\> - \<Environment\> . [{servicebus.windows.net}] | <li> sb-navigator-prod <li> sb-emissions-dev |
| 服務匯流排佇列 | 服務匯流排 | sbq-\<query descriptor\>                                   | <li> sbq-messagequery |
| 服務匯流排主題 | 服務匯流排 | sbt\<query descriptor\>                                   | <li> sbt-messagequery |
