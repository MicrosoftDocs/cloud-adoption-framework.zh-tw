---
title: 定義您的命名慣例
description: 瞭解 azure 資源和資產的命名考慮，以及查看 Azure 中資源和資產的範例名稱。
author: BrianBlanchard
ms.author: brblanch
ms.date: 12/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: internal, readiness, fasttrack-edit
ms.openlocfilehash: b7ddc0641e1aaecf2828e74234c0daf5c9b1216b
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101786170"
---
# <a name="define-your-naming-convention"></a>定義您的命名慣例

有效的命名慣例會從有關每個資源的重要資訊來撰寫資源名稱。 妥善選擇的名稱可協助您快速識別資源的類型、其相關聯的工作負載、其部署環境，以及裝載該資源的 Azure 區域。 例如，位於美國西部區域的生產 SharePoint 工作負載的公用 IP 資源可能是 `pip-sharepoint-prod-westus-001` 。

![Azure 資源名稱的元件](../../_images/ready/resource-naming.png)

_圖1： Azure 資源名稱的元件。_

## <a name="naming-scope"></a>命名範圍

所有 Azure 資源類型都有一個範圍，可定義資源名稱必須是唯一的層級。 資源的範圍內必須有唯一的名稱。

例如，虛擬網路有資源群組範圍，這表示在指定的資源群組中，只能有一個名為 `vnet-prod-westus-001` 的網路。 其他資源群組可以有自己的虛擬網路，名為 `vnet-prod-westus-001` 。 子網的範圍設定為虛擬網路，因此虛擬網路中的每個子網都必須有不同的名稱。

某些資源名稱（例如具有公用端點或虛擬機器 DNS 標籤的 PaaS 服務）具有全域範圍，因此它們在整個 Azure 平臺中必須是唯一的。

![Azure 資源名稱的範圍層級](../../_images/ready/resource-naming-scope.png)

_圖2： Azure 資源名稱的範圍層級。_

資源名稱有長度限制。 當您開發命名慣例時，請務必平衡名稱中內嵌的內容及其範圍和長度限制。 如需詳細資訊，請參閱 [Azure 資源的命名規則和限制](/azure/azure-resource-manager/management/resource-name-rules)。

### <a name="recommended-naming-components"></a>建議的命名元件

當您建構命名慣例時，應找出您想要在資源名稱中反映的重要資訊片段。 不同資訊會與不同資源類型相關。 下列清單會提供建構資源名稱時實用的資訊範例。

請盡量縮短命名元件的長度，避免超過資源名稱的長度限制。

| 命名元件 | 描述 |
|--|--|--|
| **資源類型** | 表示 Azure 資源或資產類型的縮寫。 此元件通常用來做為名稱中的前置詞或尾碼。 如需詳細資訊，請參閱 [Azure 資源類型的建議縮寫](./resource-abbreviations.md)。 範例：`rg`、`vm` |
| **業務單位** | 您公司中擁有該資源所屬訂用帳戶或工作負載的最上層部門。 在小型組織中，此元件可能是單一公司的頂層組織元素。 範例： `fin` 、 `mktg` 、 `product` 、 `it` 、 `corp` |
| **應用程式或服務名稱** | 資源所屬應用程式、工作負載或服務的名稱。 範例： `navigator` 、 `emissions` 、 `sharepoint` 、 `hadoop` |
| **訂用帳戶類型** | 資源所在訂用帳戶的用途摘要描述。 通常會依部署環境類型或特定工作負載來細分。 範例： `prod` 、 `shared` 、 `client` |
| **部署 &nbsp; 環境** | 資源所支援工作負載的開發週期階段。 範例： `prod` 、 `dev` 、 `qa` 、 `stage` 、 `test` |
| **區域** | 部署資源所在的 Azure 區域。 範例： `westus` 、 `eastus2` 、 `westeu` 、 `usva` 、 `ustx` |

## <a name="example-names-for-common-azure-resource-types"></a>常見 Azure 資源類型的範例名稱

下一節提供企業雲端部署中常見 Azure 資源類型的一些範例名稱。

> [!NOTE]
> 其中一些範例名稱使用三位數的填補配置 (`###`) ，例如 `mktg-prod-001` 。
>
> 當資產在設定管理資料庫 (CMDB) 、IT 資產管理工具或傳統的會計工具中管理時，填補可改善資產的可讀性和排序。 當部署的資產集中管理成為較大型清查或 IT 資產組合的一部分時，填補方法會與這些系統用來管理清查命名的介面對齊。
>
> 可惜的是，傳統的資產填補方法可以證明基礎結構即程式碼的方法有問題，這種方法可能會根據非填補的數位來逐一查看資產。 這種方法在部署或自動化設定管理工作期間很常見。 這些腳本必須定期去除填補，然後將填補的數位轉換成實數，這樣會減緩腳本開發和執行時間。
>
> 選擇適合您組織的方法。 此處顯示的填補內容說明使用一致方法來清查編號的重要性，而不是較佳的方法。 選擇編號配置 (不論是否有填補) ，請評估哪些內容會影響長期作業： CMDB/資產管理解決方案或以程式碼為基礎的清查管理。 然後一致地遵循最符合您操作需求的填補選項。

<!-- cspell:ignore cloudapp azurewebsites servicebus -->

<!-- cspell:ignoreRegExp [a-z]+-[a-z]+ -->
<!-- cspell:ignoreRegExp `[a-z]+` -->
<!-- cspell:ignoreRegExp [a-z]+\d+ -->
<!-- cspell:ignoreRegExp _[a-z]+[\\-] -->

## <a name="example-names-general"></a>範例名稱：一般

| 資產類型 | 影響範圍 | 格式和範例 |
|--|--|--|
| **管理群組** | 營業單位及/或 <br> 環境類型 | _mg- \<business unit> [- \<environment type> ]_ <br><br> <li> `mg-mktg` <li> `mg-hr` <li> `mg-corp-prod` <li> `mg-fin-client` |
| **訂用帳戶** | 帳戶/企業合約 | _\<business&nbsp;unit>-\<subscription&nbsp;type>-\<###>_ <br><br> <li> `mktg-prod-001` <li> `corp-shared-001` <li> `fin-client-001` |
| **資源群組** | 訂用帳戶 | _rg- \<app&nbsp;or&nbsp;service&nbsp;name> -<訂用帳戶 &nbsp; 類型>-\<###>_ <br><br> <li> `rg-mktgsharepoint-prod-001` <li> `rg-acctlookupsvc-shared-001` <li> `rg-ad-dir-services-shared-001` |
| **API 管理服務實例** | 全球 | _apim\<app&nbsp;or&nbsp;service&nbsp;name>_ <br><br> `apim-navigator-prod` |
| **受控識別** | 資源群組 | _識別碼\<app&nbsp;or&nbsp;service&nbsp;name>_ <br><br> <li> `id-appcn-keda-prod-eastus2-001` |

## <a name="example-names-networking"></a>範例名稱：網路

| 資產類型 | 影響範圍 | 格式和範例 |
|--|--|--|
| **虛擬網路** | 資源群組 | _vnet\<subscription&nbsp;type>-\<region>-\<###>_ <br><br> <li> `vnet-shared-eastus2-001` <li> `vnet-prod-westus-001` <li> `vnet-client-eastus2-001` |
| **子網路** | 虛擬網路 | _snet-\<subscription>-\<region>-\<###>_ <br><br> <li> `snet-shared-eastus2-001` <li> `snet-prod-westus-001` <li> `snet-client-eastus2-001` |
| **網路介面 (NIC)** | 資源群組 | _nic-< # # >-\<vm&nbsp;name>-\<subscription>-\<###>_ <br><br> <li> `nic-01-dc1-shared-001` <li> `nic-02-vmhadoop1-prod-001` <li> `nic-02-vmtest1-client-001` |
| **公用 IP 位址** | 資源群組 | _點數\<vm&nbsp;name&nbsp;or&nbsp;app&nbsp;name>-\<environment>-\<region>-\<###>_ <br><br> <li> `pip-dc1-shared-eastus2-001` <li> `pip-hadoop-prod-westus-001` |
| **負載平衡器** | 資源群組 | _磅\<app&nbsp;name&nbsp;or&nbsp;role>-<environment>-\<###>_ <br><br> <li> `lb-navigator-prod-001` <li> `lb-sharepoint-dev-001` |
| **網路安全性群組 (NSG)** | 子網或 NIC | _nsg\<policy&nbsp;name&nbsp;or&nbsp;app&nbsp;name>-\<###>_ <br><br> <li> `nsg-weballow-001` <li> `nsg-rdpallow-001` <li> `nsg-sqlallow-001` <li> `nsg-dnsblocked-001` |
| **局域網路閘道** | 虛擬閘道 | _lgw-\<subscription&nbsp;type>-\<region>-\<###>_ <br><br> <li> `lgw-shared-eastus2-001` <li> `lgw-prod-westus-001` <li> `lgw-client-eastus2-001` |
| **虛擬網路閘道** | 虛擬網路 | _vgw-\<subscription&nbsp;type>-\<region>-\<###>_ <br><br> <li> `vgw-shared-eastus2-001` <li> `vgw-prod-westus-001` <li> `vgw-client-eastus2-001` |
| **站對站連線** | 資源群組 | _cn----- \<local&nbsp;gateway&nbsp;name>\<virtual&nbsp;gateway&nbsp;name>_ <br><br> <li> `cn-lgw-shared-eastus2-001-to-vgw-shared-eastus2-001` <li> `cn-lgw-shared-eastus2-001-to-vgw-shared-westus-001` |
| **VPN 連線** | 資源群組 | _cn- \<subscription1> - ---- \<region1>\<subscription2>-\<region2>-_ <br><br> <li> `cn-shared-eastus2-to-shared-westus` <li> `cn-prod-eastus2-to-prod-westus` |
| **路由表** | 資源群組 | _往\<route&nbsp;table&nbsp;name>_ <br><br> <li> `route-navigator` <li> `route-sharepoint` |
| **DNS 標籤** | 全球 | _\<DNS&nbsp;A&nbsp;record&nbsp;for&nbsp;VM>.\<region>.cloudapp.azure.com_ <br><br> <li> `dc1.westus.cloudapp.azure.com` <li> `web1.eastus2.cloudapp.azure.com` |

## <a name="example-names-compute-and-web"></a>範例名稱：計算和 Web

| 資產類型 | 影響範圍 | 格式和範例 |
|--|--|--|
| **虛擬機器** | 資源群組 | _Vm\<policy name or app name>\<###>_ <br><br> <li> `vmnavigator001` <li> `vmsharepoint001` <li> `vmsqlnode001` <li> `vmhadoop001` |
| **VM 儲存體帳戶** | 全球 | _stvm\<performance type>\<app name or prod name>\<region>\<###>_ <br><br> <li> `stvmstcoreeastus2001` <li> `stvmpmcoreeastus2001` <li> `stvmstplmeastus2001` <li> `stvmsthadoopeastus2001` |
| **Web 應用程式** | 全球 | _\<app name> - \<environment> - \<###> azurewebsites.net_ <br><br> <li> `app-navigator-prod-001.azurewebsites.net` <li> `app-accountlookup-dev-001.azurewebsites.net` |
| **函數應用程式** | 全球 | _func- \<app name> - \<environment> - \<###> . azurewebsites.net_ <br><br> <li> `func-navigator-prod-001.azurewebsites.net` <li> `func-accountlookup-dev-001.azurewebsites.net` |
| **雲端服務** | 全球 | _\<app name> - \<environment> - \<###> cloudapp.net}_ <br><br> <li> `cld-navigator-prod-001.azurewebsites.net` <li> `cld-accountlookup-dev-001.azurewebsites.net` |
| **通知中樞命名空間** | 全球 | _ntfns-\<app name>-\<environment>_ <br><br> <li> `ntfns-navigator-prod` <li> `ntfns-emissions-dev` |
| **通知中樞** | 通知中樞命名空間 | _contoso.ntf\<app name>-\<environment>_ <br><br> <li> `ntf-navigator-prod` <li> `ntf-emissions-dev` |

## <a name="example-names-databases"></a>範例名稱：資料庫

| 資產類型 | 影響範圍 | 格式和範例 |
|--|--|--|
| **Azure SQL Database 伺服器** | 全球 | _.sql\<app name>-\<environment>_ <br><br> <li> `sql-navigator-prod` <li> `sql-emissions-dev` |
| **Azure SQL 資料庫** | Azure SQL Database | _sqldb\<database name>-\<environment>_ <br><br> <li> `sqldb-users-prod` <li> `sqldb-users-dev` |
| **Azure Cosmos DB 資料庫** | 全球 | _cosmos\<app name>-\<environment>_ <br><br> <li> `cosmos-navigator-prod` <li> `cosmos-emissions-dev` |
| **Azure Cache for Redis 實例** | 全球 | _redis\<app name>-\<environment>_ <br><br> <li> `redis-navigator-prod` <li> `redis-emissions-dev` |
| **MySQL 資料庫** | 全球 | _mysql\<app name>-\<environment>_ <br><br> <li> `mysql-navigator-prod` <li> `mysql-emissions-dev` |
| **PostgreSQL 資料庫** | 全球 | _psql\<app name>-\<environment>_ <br><br> <li> `psql-navigator-prod` <li> `psql-emissions-dev` |
| **Azure SQL 資料倉儲** | 全球 | _sqldw-\<app name>-\<environment>_ <br><br> <li> `sqldw-navigator-prod` <li> `sqldw-emissions-dev` |
| **SQL Server Stretch Database** | Azure SQL Database | _sqlstrdb-\<app name>-\<environment>_ <br><br> <li> `sqlstrdb-navigator-prod` <li> `sqlstrdb-emissions-dev` |

## <a name="example-names-storage"></a>範例名稱：儲存體

| 資產類型 | 影響範圍 | 格式和範例 |
|--|--|--|
| **儲存體帳戶 (一般用途)** | 全球 | _st\<storage name>\<###>_ <br><br> <li> `stnavigatordata001` <li> `stemissionsoutput001` |
| **儲存體帳戶 (診斷記錄)** | 全球 | _stdiag\<first 2 letters of subscription name and number>\<region>\<###>_ <br><br> <li> `stdiagsh001eastus2001` <li> `stdiagsh001westus001` |
| **Azure StorSimple** | 全球 | _ssimp\<app name>-\<environment>_ <br><br> <li> `ssimpnavigatorprod` <li> `ssimpemissionsdev` |
| **Azure Container Registry** | 全球 | _Acr\<app name>\<environment>\<###>_ <br><br> <li> `acrnavigatorprod001` |

## <a name="example-names-ai-and-machine-learning"></a>範例名稱： AI 和機器學習

| 資產類型 | 影響範圍 | 格式和範例 |
|--|--|--|
| **Azue 認知搜尋** | 全球 | _srch-\<app name>-\<environment>_ <br><br> <li> `srch-navigator-prod` <li> `srch-emissions-dev` |
| **Azure 認知服務** | 資源群組 | _齒輪\<app name>-\<environment>_ <br><br> <li> `cog-navigator-prod` <li> `cog-emissions-dev` |
| **Azure Machine Learning 工作區** | 資源群組 | _mlw-\<app name>-\<environment>_ <br><br> <li> `mlw-navigator-prod` <li> `mlw-emissions-dev` |

## <a name="example-names-analytics-and-iot"></a>範例名稱：分析和 IoT

| 資產類型 | 影響範圍 | 格式和範例 |
|--|--|--|
| **Azure Data Factory** | 全球 | _放下\<app name>\<environment>_ <br><br> <li> `adf-navigator-prod` <li> `adf-emissions-dev` |
| **Azure 串流分析** | 資源群組 | _asa\<app name>-\<environment>_ <br><br> <li> `asa-navigator-prod` <li> `asa-emissions-dev` |
| **Data Lake Analytics 帳戶** | 全球 | _dla\<app name>\<environment>_ <br><br> <li> `dlanavigatorprod` <li> `dlanavigatorprod` |
| **Data Lake Storage 帳戶** | 全球 | _Dls\<app name>\<environment>_ <br><br> <li> `dlsnavigatorprod` <li> `dlsemissionsdev` |
| **事件中樞** | 全球 | _evh-\<app name>-\<environment>_ <br><br> <li> `evh-navigator-prod` <li> `evh-emissions-dev` |
| **HDInsight-HBase 叢集** | 全球 | _hbase\<app name>-\<environment>_ <br><br> <li> `hbase-navigator-prod` <li> `hbase-emissions-dev` |
| **HDInsight-Hadoop 叢集** | 全球 | _hadoop\<app name>-\<environment>_ <br><br> <li> `hadoop-navigator-prod` <li> `hadoop-emissions-dev` |
| **HDInsight-Spark 叢集** | 全球 | _大家\<app name>-\<environment>_ <br><br> <li> `spark-navigator-prod` <li> `spark-emissions-dev` |
| **IoT 中樞** | 全球 | _iot\<app name>-\<environment>_ <br><br> <li> `iot-navigator-prod` <li> `iot-emissions-dev` |
| **Power BI Embedded** | 全球 | _pbi\<app name>-\<environment>_ <br><br> <li> `pbi-navigator-prod` <li> `pbi-emissions-dev` |

## <a name="example-names-integration"></a>範例名稱：整合

| 資產類型 | 影響範圍 | 格式和範例|
|--|--|--|
| **服務匯流排** | 全球 | _sb- \<app name> - \<environment> . servicebus.windows.net_ <br><br> <li> `sb-navigator-prod` <li> `sb-emissions-dev` |
| **服務匯流排佇列** | 服務匯流排 | _sbq-\<query descriptor>_ <br><br> <li> `sbq-messagequery` |
| **服務匯流排主題** | 服務匯流排 | _sbt\<query descriptor>_ <br><br> <li> `sbt-messagequery` |

## <a name="next-steps"></a>下一步

在命名資源和資產時，請參閱建議的縮寫以用於各種 Azure 資源類型。

> [!div class="nextstepaction"]
> [Azure 資源類型的建議縮寫](./resource-abbreviations.md)
