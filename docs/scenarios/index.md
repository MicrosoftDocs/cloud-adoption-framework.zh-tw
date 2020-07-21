---
title: Azure 採用的一種移轉方法
description: 請遵循 Azure Migrate 的一種移轉方法，來遷移和現代化整個 IT 組合。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/10/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: f8135b59ed583a2c790b1f78a6b1f940fe4a0acd
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86451424"
---
<!-- docsTest:ignore "One Migration" -->
<!-- cSpell:ignore HANA -->

# <a name="one-migration-approach-to-migrating-the-it-portfolio"></a>遷移 IT 組合的一種移轉方法

Azure 和 Azure Migrate 都因能夠裝載 Microsoft 技術而聞名。 但您可能不知道 Azure 有能力支援 Windows 和 SQL Server 以外的移轉。 「遷移方法」中所獲得的移轉案例會示範以一組相同的一致方針和程序，來同時遷移 Microsoft 和第三方技術。

## <a name="migration-scenarios"></a>移轉案例

以下影像和清單概述數個移轉案例，這些案例遵循相同的反覆方法來進行移轉和現代化。

![雲端採用架構移轉模型](../_images/migrate/one-migrate.png)

### <a name="links-to-migration-scenarios"></a>移轉案例的連結

| | | | |
|---------|---------|---------|---------|
| **虛擬機器** | [虛擬機器](../migrate/azure-best-practices/contoso-migration-rehost-vm.md) | [Linux 伺服器](../migrate/azure-best-practices/contoso-migration-rehost-linux-vm.md) | [虛擬桌面](./wvd/index.md) |
| **應用程式** | [ASP.NET](../migrate/azure-best-practices/contoso-migration-refactor-web-app-sql.md) | [Java](https://docs.microsoft.com/azure/java/migration-overview?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) | [PHP](../migrate/azure-best-practices/contoso-migration-refactor-linux-app-service-mysql.md) |
| **Data** | [SQL Server](../migrate/azure-best-practices/contoso-migration-rehost-vm-sql-managed-instance.md) | [開放原始碼資料庫](../migrate/azure-best-practices/sql-migration.md) | 分析 |
| **混合式** | [Azure Stack](./azure-stack/index.md) | [VMware](../migrate/azure-best-practices/vmware-host.md) | |
| **其他案例** | [保護工作負載](../migrate/azure-best-practices/migrate-best-practices-security-management.md) | [大型主機](../infrastructure/mainframe-migration/index.md) | NetApp 和 SAP HANA |

## <a name="migrate-methodology"></a>遷移方法

在每個移轉案例中，相同的一致程序會引導您進行將現有工作負載移至雲端的移轉工作。

![雲端採用架構移轉模型](../_images/migrate/methodology.png)

建構移轉波浪以引導多個工作負載的發行。 透過規劃和整備方法來建立雲端採用方案和 Azure 登陸區域，有助於新增移轉波浪結構。

在每此反覆執行期間，請遵循遷移方法來評估工作負載、部署工作負載和發行工作負載。 若要修改這些程序以符合您的特定案例，請按一下上方所列的任何移轉案例。

## <a name="next-steps"></a>後續步驟

如果您未遷移特定案例，則請遵循[四個步驟的 CAF 移轉程序](../migrate/index.md)來開始。
