---
title: Azure 資源類型的建議縮寫
description: 在命名資源和資產時，請參閱建議的縮寫以用於各種 Azure 資源類型。
author: BrianBlanchard
ms.author: brblanch
ms.date: 12/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: internal, readiness, fasttrack-edit
ms.openlocfilehash: bc0aae28551efd126e7dc0ff37978a63649cf717
ms.sourcegitcommit: 042fb295ef5623d45066ce38a389dd8d636cbc20
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/14/2021
ms.locfileid: "100492250"
---
# <a name="recommended-abbreviations-for-azure-resource-types"></a>Azure 資源類型的建議縮寫

Azure 工作負載通常是由多個資源和服務所組成。 在資源名稱中包含代表 Azure 資源類型的命名元件，可讓您更輕鬆地以視覺化方式辨識應用程式或服務元件。

這份清單提供適用于各種 Azure 資源類型的建議縮寫，以納入您的命名慣例。 這些縮寫通常用來做為資源名稱中的前置詞，因此每個縮寫會如下所示，後面接著連字號 (`-`) ，但不允許資源名稱中有連字號的資源類型。 如果您的命名慣例更適合您組織的需求，則您的命名慣例可能會將資源類型縮寫放在不同名稱的位置。

<!-- cSpell:ignore osdisk stvm arck ssimp -->
<!-- cSpell:ignoreRegExp [a-z]+- -->

## <a name="general"></a>一般

| 資產類型 | 縮寫 |
|--|--|
| 管理群組 | `mg-` |
| 資源群組 | `rg-` |
| 原則定義 | `policy-` |
| API 管理服務實例 | `apim-` |
| 受控識別 | `id-` |

## <a name="networking"></a>網路

| 資產類型 | 縮寫 |
|--|--|
| 虛擬網路 | `vnet-` |
| 子網路 | `snet-` |
| 虛擬網路對等互連 | `peer-` |
| 網路介面 (NIC) | `nic-` |
| 公用 IP 位址 | `pip-` |
| 負載平衡器 (內部)  | `lbi-` |
| 負載平衡器 (外部)  | `lbe-` |
| 網路安全性群組 (NSG) | `nsg-` |
|  (ASG) 的應用程式安全性群組 | `asg-` |
| 區域網路閘道 | `lgw-` |
| 虛擬網路閘道 | `vgw-` |
| VPN 連線 | `cn-` |
| ExpressRoute 線路 | `erc-` |
| 應用程式閘道 | `agw-` |
| 路由表 | `route-` |
| 使用者定義的路由 (UDR)  | `udr-` |
| 流量管理員設定檔 | `traf-` |
| Front Door | `fd-` |
| CDN 設定檔 | `cdnp-` |
| CDN 端點 | `cdne-` |
| Web 應用程式防火牆 (WAF) 原則 | `waf` |

## <a name="compute-and-web"></a>計算和網路

| 資產類型 | 縮寫 |
|--|--|
| 虛擬機器 | `vm` |
| 虛擬機器擴展集 | `vmss-` |
| 可用性設定組 | `avail-` |
| 受控磁片 (OS)  | `osdisk` |
| 受控磁片 (資料)  | `disk` |
| VM 儲存體帳戶 | `stvm` |
| 已啟用 Azure Arc 的伺服器 | `arcs-` |
| Azure Arc 啟用的 Kubernetes 叢集 | `arck` |
| 容器登錄 | `cr` |
| 容器實例 | `ci-` |
| AKS 叢集 | `aks-` |
| Service Fabric 叢集 | `sf-` |
| App Service 環境 | `ase-` |
| App Service 方案 | `plan-` |
| Web 應用程式 | `app-` |
| 靜態 Web 應用程式 | `stapp` |
| 函數應用程式 | `func-` |
| 雲端服務 | `cld-` |
| 通知中樞 | `ntf-` |
| 通知中樞命名空間 | `ntfns-` |

## <a name="databases"></a>資料庫

| 資產類型 | 縮寫 |
|--|--|
| Azure SQL Database 伺服器 | `sql-` |
| Azure SQL 資料庫 | `sqldb-` |
| Azure Cosmos DB 資料庫 | `cosmos-` |
| Azure Cache for Redis 實例 | `redis-` |
| MySQL 資料庫 | `mysql-` |
| PostgreSQL 資料庫 | `psql-` |
| Azure SQL 資料倉儲 | `sqldw-` |
| Azure Synapse Analytics | `syn-` |
| SQL Server Stretch Database | `sqlstrdb-` |
| SQL 受控執行個體 | `sqlmi-` |

## <a name="storage"></a>儲存體

| 資產類型 | 縮寫 |
|--|--|
| 儲存體帳戶 | `st` |
| Azure StorSimple | `ssimp` |
| Azure Container Registry | `acr` |

## <a name="ai-and-machine-learning"></a>AI 與機器學習

| 資產類型 | 縮寫 |
|--|--|
| Azue 認知搜尋 | `srch-` |
| Azure 認知服務 | `cog-` |
| Azure Machine Learning 工作區 | `mlw-` |

## <a name="analytics-and-iot"></a>分析和 IoT

| 資產類型 | 縮寫 |
|--|--|
| Azure Analysis Services server | `as` |
| Azure Databricks 工作區 | `dbw-` |
| Azure 串流分析 | `asa-` |
| Azure 資料總管叢集 | `dec` |
| Azure Data Factory | `adf-` |
| Data Lake Store 帳戶 | `dls` |
| Data Lake Analytics 帳戶 | `dla` |
| 事件中樞命名空間 | `evhns-` |
| 事件中樞 | `evh-` |
| 事件方格網域 | `evgd-` |
| 事件方格主題 | `evgt-` |
| HDInsight-Hadoop 叢集 | `hadoop-` |
| HDInsight-HBase 叢集 | `hbase-` |
| HDInsight-Kafka 叢集 | `kafka-` |
| HDInsight-Spark 叢集 | `spark-` |
| HDInsight-風暴叢集 | `storm-` |
| HDInsight-ML 服務叢集 | `mls-` |
| IoT 中樞 | `iot-` |
| Power BI Embedded | `pbi-` |
| 時間序列深入解析環境 | `tsi-` |

## <a name="developer-tools"></a>開發人員工具

| 資產類型 | 縮寫 |
|---|---|
| 應用程式設定存放區 | `appcs-` |
| Azure 靜態 Web 應用程式 | `stap-` |

## <a name="integration"></a>整合

| 資產類型 | 縮寫 |
|--|--|
| 整合帳戶 | `ia-` |
| 邏輯應用程式 | `logic-` |
| 服務匯流排 | `sb-` |
| 服務匯流排佇列 | `sbq-` |
| 服務匯流排主題 | `sbt-` |

## <a name="management-and-governance"></a>管理與治理

| 資產類型 | 縮寫 |
|--|--|
| 自動化帳戶 | `aa-` |
| Azure 監視器動作群組 | `ag-` |
| Azure 範疇實例 | `pview-` |
| 藍圖 | `bp-` |
| 藍圖指派 | `bpa-` |
| 金鑰保存庫 | `kv-` |
| Log Analytics 工作區 | `log-` |
| Application Insights | `appi-` |

## <a name="migration"></a>移轉

| 資產類型 | 縮寫 |
|--|--|
| Azure Migrate 專案 | `migr-` |
| 資料庫移轉服務實例 | `dms-` |
| 復原服務保存庫 | `rsv-` |

## <a name="next-steps"></a>下一步

查看 Azure 資源和資產的標記建議。

> [!div class="nextstepaction"]
> [定義您的標記策略](./resource-tagging.md)
