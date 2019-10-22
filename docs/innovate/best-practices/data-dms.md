---
title: 雲端創新：資料移轉服務
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 雲端創新-資料移轉服務
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: c75efe3576bb61ecb116ab22e4946b8d87da3d4a
ms.sourcegitcommit: f3371811a36e12533ecbc3aa936e2a68e0cee25f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/21/2019
ms.locfileid: "72683424"
---
# <a name="collect-data-through-the-migration-and-modernization-of-existing-data-sources"></a>透過遷移和現代化的現有資料來源來收集資料

公司通常會有各種現有的資料可供[大眾化](../considerations/data.md)。 當客戶假設需要使用現有的資料來建立新式解決方案時，第一個步驟可能是資料移轉和現代化，以準備進行開始創造發明和創新。 為了配合雲端採用方案內的現有遷移工作，可以更輕鬆地在[遷移方法](../../migrate/index.md)內執行遷移和現代化。

## <a name="use-of-this-article"></a>本文的使用

本文概述一系列與遷移過程一致的方法，但最符合標準遷移工具鏈。 在遷移方法的評估過程中，雲端採用小組會評估目前的狀態，並希望資產的未來狀態得以遷移。 當該程式是創新作業的一部分時，這兩個雲端採用小組都可以使用這篇文章來做出決定。

## <a name="primary-toolset"></a>主要工具組

當遷移和現代化目前位於內部內部部署的資料時，最常見的 Azure 工具選擇是[資料移轉服務（DMS）](https://docs.microsoft.com/azure/dms) ，這是較廣泛[Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-services-overview)工具鏈的一部分。 針對現有的 SQL Server 資料來源， [Data Migration Assistant （DMA）](/sql/dma/dma-overview)也可以提供評估和遷移較少量資料結構的協助。

為了支援 Oracle 和 NoSQL 遷移，[資料移轉服務（DMS）](https://docs.microsoft.com/azure/dms)也可以用於目標資料庫的特定類型，例如 Oracle to 于 postgresql 或 MongoDB to Cosmos DB。 採用協力廠商工具或自訂遷移腳本以遷移至 Cosmos DB、HDInsight 或 IaaS 型 VM 選項更常見。

## <a name="considerations-and-guidance"></a>考慮和指引

利用 DMS 來遷移和現代化資料時，請務必瞭解目前裝載資料（來源）、版本，以及最能支援客戶假設（目標）之平臺和版本的平臺。 以下是要與遷移小組一起審查的來源和目標配對清單。 每個都包含一個工具選擇，以及根據這些考慮的指南連結。

**遷移類型：** 使用離線遷移時，應用程式停機會在開始遷移時啟動。 若使用線上移轉，則只會在移轉結束時於完全移轉期間停機。 我們建議您瞭解可接受的商務停機時間，並測試離線遷移，以判斷還原時間是否符合此情況;如果沒有，請進行線上遷移。

|來源  |確定目標  |工具  |遷移類型  |指導方針  |
|---------|---------|---------|---------|---------|
|SQL Server|Azure SQL Database|DMS|離線|[教學課程](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql)|
|SQL Server|Azure SQL Database|DMS|線上|[教學課程](https://docs.microsoft.com/azure/dms/tutorial-sql-server-azure-sql-online)|
|SQL Server|Azure SQL Database 受控執行個體|DMS|離線|[教學課程](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)|
|SQL Server|Azure SQL Database 受控執行個體|DMS|線上|[教學課程](https://docs.microsoft.com/azure/dms/tutorial-sql-server-managed-instance-online)|
|RDS SQL Server|Azure SQL Database （或受控執行個體）|DMS|線上|[教學課程](https://docs.microsoft.com/azure/dms/tutorial-rds-sql-server-azure-sql-and-managed-instance-online)|
|MySQL|適用於 MySQL 的 Azure 資料庫|DMS|線上|[教學課程](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online)|
|PostgreSQL|適用於 PostgreSQL 的 Azure 資料庫|DMS|線上|[教學課程](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online)|
|MondoDB|Azure Cosmos DB Mongo API|DMS|離線|[教學課程](https://docs.microsoft.com/azure/dms/tutorial-mongodb-cosmos-db)|
|MongoDB|Azure Cosmos DB Mongo API|DMS|線上|[教學課程](https://docs.microsoft.com/azure/dms/tutorial-mongodb-cosmos-db-online)|
|Oracle|PaaS & IaaS 選項的範圍|協力廠商或 Azure Migrate|各種類型|[決策樹](../../migrate/expanded-scope/data-oracle-migration.md)|
|各種 NoSQL Db|Cosmo DB 或 IaaS 選項|程式性遷移或 Azure Migrate|各種類型|[決策樹](../../migrate/expanded-scope/data-no-sql-migration.md)|
