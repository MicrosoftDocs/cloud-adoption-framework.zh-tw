---
title: 自動上線
description: 使用上架範例檔案，協助您考慮將 Azure 伺服器管理服務部署自動化，以提升效率。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: b3c32b6d6afa9b640dd379230865728fbd53c925
ms.sourcegitcommit: af521583b98153f7157895b7ba9de71183d437b0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89176475"
---
# <a name="automate-onboarding"></a>自動上線

若要改善 Azure 伺服器管理服務的部署效率，請考慮依照本指南前面幾節所述，將部署自動化。 下列各節所提供的腳本和範例範本是開始開發您自己的上架程式自動化的起點。

本指南包含範例程式碼（ [CloudAdoptionFramework](https://aka.ms/caf/manage/automation-samples)）的支援 GitHub 存放庫。 存放庫提供範例腳本和 Azure Resource Manager 範本，可協助您自動化 Azure 伺服器管理服務的部署。

範例檔案說明如何使用 Azure PowerShell Cmdlet 將下列工作自動化：

- 建立 [Log Analytics 工作區](/azure/azure-monitor/platform/manage-access)。  (或，如果符合需求，請使用現有的工作區。 如需詳細資訊，請參閱 [工作區規劃](./prerequisites.md#log-analytics-workspace-and-automation-account-planning)。

- 建立自動化帳戶，或使用符合需求的現有帳戶。 如需詳細資訊，請參閱 [工作區規劃](./prerequisites.md#log-analytics-workspace-and-automation-account-planning)。

- 連結自動化帳戶和 Log Analytics 工作區。 如果您是使用 Azure 入口網站上架，就不需要執行此步驟。

- 啟用工作區的更新管理、變更追蹤和清查。

- 使用 Azure 原則將 Azure Vm 上架。 原則會在 Azure Vm 上安裝 Log Analytics 代理程式和 Microsoft Dependency Agent。

- 使用[Azure 原則](https://docs.microsoft.com/azure/backup/backup-azure-auto-enable-backup)自動啟用適用于 Vm 的 Azure 備份

- 在內部部署伺服器上安裝 Log Analytics 代理程式，以將其上架。

下表所述的檔案會在此範例中使用。 您可以自訂它們以支援您自己的部署案例。

| 檔案名稱 | 描述 |
|-----------|-------------|
| `New-AMSDeployment.ps1` | 主要的協調腳本，它會自動上架。 它會建立資源群組、位置、工作區和自動化帳戶（如果尚未存在）。 此 PowerShell 腳本需要現有的訂用帳戶。 |
| `Workspace-AutomationAccount.json` | 部署工作區和自動化帳戶資源的 Resource Manager 範本。 |
| `WorkspaceSolutions.json` | Resource Manager 範本，可在 Log Analytics 工作區中啟用您想要的解決方案。 |
| `ScopeConfig.json` | Resource Manager 範本，其使用內部部署伺服器的加入宣告模型與變更追蹤解決方案。 使用加入宣告模型是選擇性的。 |
| `Enable-VMInsightsPerfCounters.ps1` | PowerShell 腳本，可啟用伺服器的 VM 深入解析，並設定效能計數器。 |
| `ChangeTracking-FileList.json` | Resource Manager 範本，定義將由變更追蹤監視的檔案清單。 |

使用下列命令來執行 `New-AMSDeployment.ps1` ：

```powershell
.\New-AMSDeployment.ps1 -SubscriptionName '{Subscription Name}' -WorkspaceName '{Workspace Name}' -WorkspaceLocation '{Azure Location}' -AutomationAccountName {Account Name} -AutomationAccountLocation {Account Location}
```

## <a name="next-steps"></a>後續步驟

瞭解如何設定基本警示，以通知您的金鑰管理事件和問題的小組。

> [!div class="nextstepaction"]
> [設定基本警示](./setup-alerts.md)
