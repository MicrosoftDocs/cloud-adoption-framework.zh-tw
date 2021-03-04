---
title: Azure 採用的一種移轉方法
description: 請遵循 Azure Migrate 的一種移轉方法，來遷移和現代化整個 IT 組合。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/21/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: internal
ms.openlocfilehash: 80dc2b74e6abc3593c64985a8e853fb852a8ffd3
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101784946"
---
<!-- docutune:ignore "One Migration" -->
<!-- cSpell:ignore HANA -->

# <a name="the-one-migration-approach-to-migrating-the-it-portfolio"></a>遷移 IT 組合的一種移轉方法

Azure 和 Azure Migrate 都因能夠裝載 Microsoft 技術而聞名。 但您可能不知道 Azure 有能力支援 Windows 和 SQL Server 以外的移轉。 遷移方法中所獲得的「一種移轉」案例會示範以一組相同的一致方針和程序，來同時遷移 Microsoft 和第三方技術。

## <a name="migration-scenarios"></a>移轉案例

以下圖表和表格概述數個案例，這些案例遵循相同的反覆遷移方法來進行移轉和現代化。

![雲端採用架構移轉模型的圖表，會顯示您將需要的 V M、應用程式、資料和混合式資源。](../_images/migrate/one-migrate.png)

| | | | |
|---------|---------|---------|---------|
| **虛擬機器** | [虛擬機器](../migrate/azure-best-practices/contoso-migration-rehost-vm.md) | [Linux 伺服器](../migrate/azure-best-practices/contoso-migration-rehost-linux-vm.md) | [虛擬桌面](./wvd/index.md) |
| **應用程式** | [ASP.NET](../migrate/azure-best-practices/contoso-migration-refactor-web-app-sql.md) | [Java](/azure/java/migration-overview?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) | [PHP](../migrate/azure-best-practices/contoso-migration-refactor-linux-app-service-mysql.md) |
| **Data** | [SQL Server](../migrate/azure-best-practices/contoso-migration-rehost-vm-sql-managed-instance.md) | [開放原始碼資料庫](../migrate/azure-best-practices/sql-migration.md) | [分析](../migrate/azure-best-practices/analytics/analytics-solutions-overview.md) |
| **混合式** | [Azure Stack](./azure-stack/index.md) | [VMware](../migrate/azure-best-practices/vmware-host.md) | |
| **技術平台** | SAP (傳統 & HANA) | Kubernetes | [大型主機](../infrastructure/mainframe-migration/index.md) |
| **其他案例** | [保護工作負載](../migrate/azure-best-practices/migrate-best-practices-security-management.md) | [多租用戶環境](/azure/lighthouse/how-to/migration-at-scale?bc=/azure/cloud-adoption-framework/_bread/toc.json&toc=/azure/cloud-adoption-framework/toc.json) | NetApp |

## <a name="migrate-methodology"></a>遷移方法

在上述每個移轉案例中，當您將現有的工作負載遷移至雲端時，相同的基本流程將會引導您，如下所示：

![雲端採用架構移轉模型的圖表，會顯示移轉波浪和移轉工作。](../_images/migrate/methodology.png)

在每個案例中，建構移轉波浪以引導多個工作負載的版本。 透過規劃和整備方法來建立雲端採用方案和 Azure 登陸區域，有助於新增移轉波浪結構。

在每此反覆執行期間，請遵循遷移方法來評估、部署和發行工作負載。 若要修改這些程序以符合您組織的特定案例，請選取表格中所列的任何移轉案例。

## <a name="next-steps"></a>後續步驟

如果您未遷移特定案例，則請遵循[四個步驟的雲端採用架構移轉程序](../migrate/index.md)來開始。
