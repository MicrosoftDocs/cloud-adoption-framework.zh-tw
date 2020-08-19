---
title: Azure 中的部署加速工具
description: 瞭解 Azure 原生工具如何協助支援部署加速專業領域的成熟原則和流程。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 0f24b44be27111461e2374d12b8bdc75ca0aabb2
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88573610"
---
# <a name="deployment-acceleration-tools-in-azure"></a>Azure 中的部署加速工具

[部署加速專業領域](./index.md)是[雲端治理的五個專業領域](../governance-disciplines.md)之一。 這個專業領域著重於建立原則來治理資產設定或部署的方式。 在雲端治理的五個專業領域中，部署加速專業領域牽涉到部署和設定的一致。 這可能會透過手動活動或完全自動化的 DevOps 活動。 無論是哪一種情況，所牽涉的原則都會保持不變。

對治理有興趣的雲端監管人、雲端守護者和雲端架構設計人員，經常投入許多時間在部署加速專業領域中，這個專業領域跨多個雲端採用工作制訂原則和需求。 此工具鏈中的工具對於雲端治理小組很重要，應該是小組學習路徑的高優先順序。

以下是 Azure 工具清單，可協助您成熟支援此專業領域的原則和流程。

|  | [Azure 原則](/azure/governance/policy/overview) | [Azure 管理群組](/azure/governance/management-groups) | [Azure Resource Manager](/azure/azure-resource-manager/management/overview) | [Azure 藍圖](/azure/governance/blueprints/overview) | [Azure resource graph](/azure/governance/resource-graph/overview) | [Azure 成本管理](/azure/cost-management) \(部分機器翻譯\) |
|---------|---------|---------|---------|---------|---------|---------|
| **實行公司原則**     | 是 | 否  | 否  | 否 | 否 | 否 |
| **跨訂用帳戶套用原則**     | 必要 | 是  | 否  | 否 | 否 | 否 |
| **部署定義的資源**     | 否 | 否  | 是  | 否 | 否 | 否 |
| **建立完全相容的環境**      | 必要 | 必要  | 必要  | 是 | 否 | 否 |
| **稽核原則**      | 是 | 否  | 否  | 否 | 否 | 否 |
| **查詢 Azure 資源**      | 否 | 否  | 否  | 否 | 是 | 否 |
| **資源成本報告**      | 否 | 否  | 否  | 否 | 否 | 是 |

以下是完成特定部署加速目標可能需要的其他工具。 這些工具通常會在治理小組外部使用，但仍會被視為部署加速專業領域的層面。

|  | [Azure 入口網站](https://azure.microsoft.com/features/azure-portal)  | [Azure Resource Manager](/azure/azure-resource-manager/management/overview)  | [Azure 原則](/azure/governance/policy/overview) | [Azure DevOps](/azure/devops/user-guide/what-is-azure-devops) | [Azure 備份](/azure/backup/backup-overview) | [Azure Site Recovery](/azure/site-recovery/site-recovery-overview) |
|---------|---------|---------|---------|---------|---------|---------|
| **手動部署 (單一資產)**     | 是 | 是  | 否  | 無效率 | 否 | 是 |
| **手動部署 (完整環境)**     | 無效率 | 是 | 否  | 無效率 | 否 | 是 |
| **自動化部署 (完整環境)**     | 否  | 是  | 否  | 是  | 否 | 是 |
| **更新單一資產的設定**     | 是 | 是 | 無效率 | 無效率 | 否 | 是，在複寫期間 |
| **完整環境的更新設定**     | 無效率 | 是 | 是 | 是  | 否 | 是，在複寫期間 |
| **管理設定飄移**     | 無效率 | 無效率 | 是  | 是  | 否 | 是，在複寫期間 |
| **建立自動化管線來部署程式碼和設定資產 (DevOps)**     | 否 | 否 | 否 | 是 | 否 | 否 |

除了上述的 Azure 原生工具，客戶通常會使用協力廠商工具來加速部署加速和 DevOps 部署。
