---
title: 遷移資料的創新工具
description: 瞭解 Azure 資料庫移轉服務和其他工具，以遷移和現代化資料，以準備進行雲端開始創造發明和創新。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: 52cbb07489b3b053a2db19a14b47c53b5a67a79a
ms.sourcegitcommit: bcc73d194c6d00c16ae2e3c7fb2453ac7dbf2526
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/09/2020
ms.locfileid: "86193706"
---
# <a name="collect-data-through-the-migration-and-modernization-of-existing-data-sources"></a>透過遷移和現代化的現有資料來源來收集資料

公司通常會有不同種類的現有資料可以[將大眾化](../considerations/data.md)。 當客戶假設需要使用現有的資料來建立新式解決方案時，第一個步驟可能是資料移轉和現代化，以準備進行開始創造發明和創新。 為了配合雲端採用方案內的現有遷移工作，您可以更輕鬆地在[遷移方法](../../migrate/index.md)中進行遷移和現代化。

## <a name="use-of-this-article"></a>本文的使用

本文概述一系列與遷移過程一致的方法。 您可以將這些方法最佳地配合標準遷移工具鏈。

在遷移方法的評估過程中，雲端採用小組會針對已遷移的資產，評估目前狀態和所需的未來狀態。 當該程式是創新成果的一部份時，這兩個雲端採用小組都可以使用這篇文章來協助進行這些評量。

## <a name="primary-toolset"></a>主要工具組

當您遷移並現代化內部部署資料時，最常見的 Azure 工具選擇是[Azure 資料庫移轉服務](https://docs.microsoft.com/azure/dms)。 這項服務屬於更廣泛的[Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-services-overview)工具鏈。 針對現有的 SQL Server 資料來源， [Data Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview)可以協助您評估和遷移少量的資料結構。

若要支援 Oracle 和 NoSQL 遷移，您也可以針對特定類型的來源對目標資料庫使用[資料庫移轉服務](https://docs.microsoft.com/azure/dms)。 範例包括 Oracle to 于 postgresql 和 MongoDB to Azure Cosmos DB。 採用小組更常使用合作夥伴工具或自訂腳本，根據基礎結構即服務 (IaaS) ，遷移至 Azure Cosmos DB、Azure HDInsight 或虛擬機器選項。

## <a name="considerations-and-guidance"></a>考慮和指引

當您使用 Azure 資料庫移轉服務來遷移和現代化資料時，請務必瞭解：

- 目前用來裝載資料來源的平臺。
- 目前版本。
- 未來最能支援客戶假設或目標的平臺和版本。

下表顯示與遷移小組一起審查的來源和目標配對。 每一組都包含工具選擇和相關指南的連結。

### <a name="migration-type"></a>遷移類型

若使用離線移轉，當移轉開始時，應用程式也會開始停機。 若使用線上移轉，則只會在移轉結束時於完全移轉期間停機。

我們建議您決定可接受的商務停機時間，並測試離線遷移。 您會這麼做，以檢查還原時間是否符合可接受的停機時間。 如果無法接受還原時間，請執行線上遷移。

| 來源  | 目標  | 工具  | 遷移類型 | 指導方針 |
|---|---|---|---|---|
| SQL Server | Azure SQL Database | Database Migration Service | 離線 | [教學課程](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql) |
| SQL Server | Azure SQL Database | Database Migration Service | 線上 | [教學課程](https://docs.microsoft.com/azure/dms/tutorial-sql-server-azure-sql-online) |
| SQL Server | Azure SQL 受控執行個體 | Database Migration Service | 離線 | [教學課程](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance) |
| SQL Server | Azure SQL 受控執行個體 | Database Migration Service | 線上 | [教學課程](https://docs.microsoft.com/azure/dms/tutorial-sql-server-managed-instance-online) |
| RDS SQL Server | Azure SQL Database 或 Azure SQL 受控執行個體 | Database Migration Service | 線上 | [教學課程](https://docs.microsoft.com/azure/dms/tutorial-rds-sql-server-azure-sql-and-managed-instance-online) |
| MySQL | 適用於 MySQL 的 Azure 資料庫 | Database Migration Service | 線上 | [教學課程](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online) |
| PostgreSQL | 適用於 PostgreSQL 的 Azure 資料庫 | Database Migration Service | 線上 | [教學課程](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online) |
| MongoDB | Azure Cosmos DB Mongo API | Database Migration Service | 離線 | [教學課程](https://docs.microsoft.com/azure/dms/tutorial-mongodb-cosmos-db) |
| MongoDB | Azure Cosmos DB Mongo API | Database Migration Service | 線上 | [教學課程](https://docs.microsoft.com/azure/dms/tutorial-mongodb-cosmos-db-online) |
