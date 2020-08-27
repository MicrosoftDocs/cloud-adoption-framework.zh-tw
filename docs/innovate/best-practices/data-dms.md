---
title: 遷移資料的創新工具
description: 瞭解 Azure 資料庫移轉服務和其他工具，這些工具可遷移和現代化資料，以準備進行雲端發明和創新。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: f3d2663bdb3c057f5031eeacc79e2068c729cb89
ms.sourcegitcommit: 07d56209d56ee199dd148dbac59671cbb57880c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88884482"
---
# <a name="collect-data-through-the-migration-and-modernization-of-existing-data-sources"></a>透過遷移和現代化現有的資料來源來收集資料

公司通常會有不同類型的現有資料可供 [將大眾化](../considerations/data.md)。 當客戶假設需要使用現有的資料來建立新式解決方案時，第一個步驟可能是資料移轉和現代化，以準備進行發明和創新。 若要與雲端採用方案中現有的遷移工作保持一致，您可以更輕鬆地在 [遷移方法](../../migrate/index.md)內進行遷移和現代化。

## <a name="use-of-this-article"></a>本文的使用

本文概述一系列與遷移方法一致的方法。 您可以最好將這些方法與標準遷移工具鏈一致。

在遷移方法內的評估階段期間，雲端採用小組會針對已遷移的資產，評估目前的狀態和所需的未來狀態。 當該流程是創新工作的一部分時，這兩個雲端採用小組都可以使用這篇文章來協助進行這些評量。

## <a name="primary-toolset"></a>主要工具組

當您將內部部署資料移轉和現代化時，最常見的 Azure 工具選擇是 [Azure 資料庫移轉服務](/azure/dms)。 這項服務是廣泛 [Azure Migrate](/azure/migrate/migrate-services-overview) 工具鏈的一部分。 針對現有的 SQL Server 資料來源， [Data Migration Assistant](/sql/dma/dma-overview) 可協助您評估及遷移少量的資料結構。

若要支援 Oracle 和 NoSQL 遷移，您也可以針對特定類型的來源目標資料庫使用 [資料庫移轉服務](/azure/dms) 。 範例包括將 Oracle 資料庫移轉至於 postgresql 或 MongoDB 資料庫以 Azure Cosmos DB。 更常見的是，採用小組會根據基礎結構即服務 (IaaS) ，使用合作夥伴工具或自訂腳本來遷移至 Azure Cosmos DB、Azure HDInsight 或虛擬機器選項。

## <a name="considerations-and-guidance"></a>考慮和指導方針

當您使用 Azure 資料庫移轉服務來遷移和現代化資料時，請務必瞭解：

- 用於裝載資料來源的目前平臺。
- 目前版本。
- 未來最能支援客戶假設或目標的平臺和版本。

下表顯示要與遷移小組檢查的來源和目標群組。 每一組都包含工具選項以及相關指南的連結。

### <a name="migration-type"></a>遷移類型

若使用離線移轉，當移轉開始時，應用程式也會開始停機。 若使用線上移轉，則只會在移轉結束時於完全移轉期間停機。

建議您決定可接受的商務停機時間，並測試離線遷移。 您可以這麼做，檢查還原時間是否符合可接受的停機時間。 如果還原時間無法接受，請進行線上遷移。

| 來源 | 目標 | 工具 | 遷移類型 | 指引 |
|--|--|--|--|--|
| SQL Server | Azure SQL Database | Database Migration Service | 離線 | [教學課程](/azure/dms/tutorial-sql-server-to-azure-sql) |
| SQL Server | Azure SQL Database | Database Migration Service | 線上 | [教學課程](/azure/dms/tutorial-sql-server-azure-sql-online) |
| SQL Server | Azure SQL 受控執行個體 | Database Migration Service | 離線 | [教學課程](/azure/dms/tutorial-sql-server-to-managed-instance) |
| SQL Server | Azure SQL 受控執行個體 | Database Migration Service | 線上 | [教學課程](/azure/dms/tutorial-sql-server-managed-instance-online) |
| RDS SQL Server | Azure SQL Database 或 Azure SQL 受控執行個體 | Database Migration Service | 線上 | [教學課程](/azure/dms/tutorial-rds-sql-server-azure-sql-and-managed-instance-online) |
| MySQL | 適用於 MySQL 的 Azure 資料庫 | Database Migration Service | 線上 | [教學課程](/azure/dms/tutorial-mysql-azure-mysql-online) |
| PostgreSQL | 適用於 PostgreSQL 的 Azure 資料庫 | Database Migration Service | 線上 | [教學課程](/azure/dms/tutorial-postgresql-azure-postgresql-online) |
| MongoDB | 適用於 MongoDB 的 Azure Cosmos DB API | Database Migration Service | 離線 | [教學課程](/azure/dms/tutorial-mongodb-cosmos-db) |
| MongoDB | 適用於 MongoDB 的 Azure Cosmos DB API | Database Migration Service | 線上 | [教學課程](/azure/dms/tutorial-mongodb-cosmos-db-online) |
