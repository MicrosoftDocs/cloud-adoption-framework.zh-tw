---
title: Azure 中的資源一致性工具
description: Azure 中的資源一致性工具
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: a2f553285f9d44085cc816c2db34f76fcb02235d
ms.sourcegitcommit: 2362fb3154a91aa421224ffdb2cc632d982b129b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/28/2020
ms.locfileid: "76805178"
---
# <a name="resource-consistency-tools-in-azure"></a>Azure 中的資源一致性工具

[資源一致性](./index.md)是[五個雲端治理專業領域](../governance-disciplines.md)的其中之一。 這個專業領域著重於訂定與環境、應用程式或工作負載之作業管理相關的原則。 在雲端治理的五個專業領域中，資源一致性專業領域牽涉到監視應用程式、工作負載和資產效能。 它也包含符合規模需求、補救效能 SLA 違規，以及主動避免透過自動補救進行效能 SLA 違規所需的工作。

以下為 Azure 工具的清單，可協助使支援此治理專業領域的原則和流程臻至成熟。

| 工具 | [Azure 入口網站](https://azure.microsoft.com/features/azure-portal)  | [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)  | [Azure 藍圖](https://docs.microsoft.com/azure/governance/blueprints/overview) | [Azure 自動化](https://docs.microsoft.com/azure/automation/automation-intro) | [Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) | [Azure 備份](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) | [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) |
|---------|---------|---------|---------|---------|---------|---------|---------|
| 部署資源                             | 是 | 是 | 是 | 是 | 否  | 否 | 否 |
| 管理資源                             | 是 | 是 | 是 | 是 | 否  | 否 | 否 |
| 使用範本部署資源             | 否  | 是 | 否  | 是 | 否  | 否 | 否 |
| 協調環境部署          | 否  | 否  | 是 | 否  | 否  | 否 | 否 |
| 定義資源群組                       | 是 | 是 | 是 | 否  | 否  | 否 | 否 |
| 管理工作負載和帳戶擁有者           | 是 | 是 | 是 | 否  | 否  | 否 | 否 |
| 管理資源的條件式存取       | 是 | 是 | 是 | 否  | 否  | 否 | 否 |
| 設定 RBAC 使用者                         | 是 | 否  | 否  | 否  | 是 | 否 | 否 |
| 將角色和權限指派給資源 | 是 | 是 | 是 | 否  | 是 | 否 | 否 |
| 定義資源間的相依性        | 否  | 是 | 是 | 否  | 否  | 否 | 否 |
| 套用存取控制                         | 是 | 是 | 是 | 否  | 是 | 否 | 否 |
| 存取可用性和延展性          | 否  | 否  | 否  | 是 | 否  | 否 | 否 |
| 將標記套用到資源                      | 是 | 是 | 是 | 否  | 否  | 否 | 否 |
| 指派 Azure 原則規則                    | 是 | 是 | 是 | 否  | 否  | 否 | 否 |
| 套用自動化的補救方法                  | 否  | 否  | 否  | 是 | 否  | 否 | 否 |
| 管理計費                               | 是 | 否  | 否  | 否  | 否  | 否 | 否 |
| 規劃災害復原的資源         | 是 | 是 | 是 | 否  | 否  | 是 | 是 |
|在發生中斷或 SLA 違規期間復原資料     | 否 | 否  | 否  | 否  | 否  | 是 | 是 |
|在發生中斷或 SLA 違規期間復原應用程式和資料     | 否 | 否  | 否  | 否  | 否  | 是 | 是 |

除了這些資源一致性工具和功能外，您還必須監視已部署的資源，以了解效能和健康情況問題。 [Azure 監視器](https://docs.microsoft.com/azure/azure-monitor/overview)是 Azure 中的預設監視和報告解決方案。 Azure 監視器提供監視雲端資源的功能。 此清單會顯示哪些功能可解決常見的監視需求。

| 工具 | [Azure 入口網站](https://azure.microsoft.com/features/azure-portal) | [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) | [Log Analytics](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview) | [Azure 監視器 REST API](https://docs.microsoft.com/rest/api/monitor) |
|----------------------------------------------------|--------------|----------------------|---------------|------------------------|
| 記錄虛擬機器的遙測資料                 | 否           | 否                   | 是           | 否                     |
| 記錄虛擬網路的遙測資料              | 否           | 否                   | 是           | 否                     |
| 記錄 PaaS 服務的遙測資料                   | 否           | 否                   | 是           | 否                     |
| 記錄應用程式的遙測資料                     | 否           | 是                  | 否            | 否                     |
| 設定報告和警示                       | 是          | 否                   | 否            | 是                    |
| 排程一般報表或自訂分析        | 否           | 否                   | 否            | 否                     |
| 以視覺化方式檢視和分析記錄和效能資料     | 是          | 否                   | 否            | 否                     |
| 與內部部署或第三方監視解決方案整合     | 否           | 否                   | 否            | 是                    |

規劃部署時，您將需要考量記錄資料儲存位置，以及如何將雲端式[報告和監視服務](../../decision-guides/logging-and-reporting/index.md)與現有的處理程序和工具整合。

> [!NOTE]
> 組織也會使用第三方 DevOps 工具來監視工作負載和資源。 如需詳細資訊，請參閱[DevOps 工具整合](https://azure.microsoft.com/products/devops-tool-integrations)。

## <a name="next-steps"></a>後續步驟

了解如何在 Azure 中建立、指派和管理[原則定義](https://docs.microsoft.com/azure/governance/policy)。
