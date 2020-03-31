---
title: Azure 中的部署加速工具
description: 瞭解 Azure 原生工具如何協助成熟的原則和流程，以支援部署加速治理專業領域。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: b6fac6af93c68f22561b578cfe598bc9d847c902
ms.sourcegitcommit: afe10f97fc0e0402a881fdfa55dadebd3aca75ab
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/31/2020
ms.locfileid: "80434474"
---
# <a name="deployment-acceleration-tools-in-azure"></a>Azure 中的部署加速工具

[部署加速](./index.md)是[五個雲端治理專業領域](../governance-disciplines.md)其中之一。 這個專業領域著重於建立原則來治理資產設定或部署的方式。 在雲端治理的五個專業領域中，部署加速專業領域牽涉到部署和設定的對齊。 這可能會透過手動活動或完全自動化的 DevOps 活動。 不論是哪一種情況，牽涉到的原則都會維持不變。

對治理有興趣的雲端監管人、雲端守護者和雲端架構設計人員，經常投入許多時間在部署加速專業領域中，這個專業領域跨多個雲端採用工作制訂原則和需求。 此工具鏈中的工具對雲端治理小組而言非常重要，而且必須是小組學習路徑的高優先順序。

以下為 Azure 工具的清單，可協助使支援此治理專業領域的原則和流程臻至成熟。

|  | [Azure 原則](https://docs.microsoft.com/azure/governance/policy/overview) | [Azure 管理群組](https://docs.microsoft.com/azure/governance/management-groups) | [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) | [Azure 藍圖](https://docs.microsoft.com/azure/governance/blueprints/overview) | [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview) | [Azure 成本管理](https://docs.microsoft.com/azure/cost-management) |
|---------|---------|---------|---------|---------|---------|---------|
|執行公司原則     |是 |否  |否  |否 | 否 |否 |
|跨訂用帳戶套用原則     |必要項 |是  |否  |否 | 否 |否 |
|部署定義的資源     |否 |否  |是  |否 | 否 |否 |
|建立完全相容的環境      |必要項 |必要項  |必要項  |是 | 否 |否 |
|稽核原則      |是 |否  |否  |否 | 否 |否 |
|查詢 Azure 資源      |否 |否  |否  |否 |是 |否 |
|資源成本報告      |否 |否  |否  |否 |否 |是 |

以下是達成特定部署加速目標所需的其他工具。 這些工具通常是在治理小組外部使用，但是仍然可視為部署加速專業領域的一個層面。

|  | [Azure 入口網站](https://azure.microsoft.com/features/azure-portal)  | [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)  | [Azure 原則](https://docs.microsoft.com/azure/governance/policy/overview) | [Azure DevOps](https://docs.microsoft.com/azure/devops/index) | [Azure 備份](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) | [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) |
|---------|---------|---------|---------|---------|---------|---------|
|手動部署 (單一資產)     | 是 | 是  | 否  | 無效率 | 否 | 是 |
|手動部署 (完整環境)     | 無效率 | 是 | 否  | 無效率 | 否 | 是 |
|自動化部署 (完整環境)     | 否  | 是  | 否  | 是  | 否 | 是 |
|更新單一資產的設定     | 是 | 是 | 無效率 | 無效率 | 否 | 是 - 在複寫期間 |
|完整環境的更新設定     | 無效率 | 是 | 是 | 是  | 否 | 是 - 在複寫期間 |
|管理設定飄移     | 無效率 | 無效率 | 是  | 是  | 否 | 是 - 在複寫期間 |
|建立自動化管線來部署程式碼和設定資產 (DevOps)     | 否 | 否 | 否 | 是 | 否 | 否 |

除了上述的 Azure 原生工具，客戶通常會使用第三方工具來輔助部署加速與 DevOps 部署。
