---
title: 自動上架
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 自動上架
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 83e1cd80bcb821ba1b815291f7f25f875ba66284
ms.sourcegitcommit: 6f287276650e731163047f543d23581d8fb6e204
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/07/2019
ms.locfileid: "73751595"
---
# <a name="automate-onboarding"></a>自動上架

若要改善部署 Azure 伺服器管理服務的效率，請考慮將部署自動化，如本指南先前章節中所述。 下列各節提供的腳本和範例範本，是開發自己的上執行緒序自動化的起點。

本指南提供支援的 GitHub 存放庫範例程式碼[CloudAdoptionFramework](https://aka.ms/caf/manage/automation-samples)。 存放庫提供範例腳本和 Azure Resource Manager 範本，可協助您將 Azure 伺服器管理服務的部署作業自動化。

範例檔案說明如何使用 Azure PowerShell Cmdlet 來自動化下列工作：

- 建立[Log Analytics 工作區](https://docs.microsoft.com/azure/azure-monitor/platform/manage-access)。 （或者，如果符合需求，請使用現有的工作區。 如需詳細資訊，請參閱[工作區規劃](./prerequisites.md#log-analytics-workspace-and-automation-account-planning)。

- 建立自動化帳戶。 （或者，如果符合需求，請使用現有的帳戶。 如需詳細資訊，請參閱[工作區規劃](./prerequisites.md#log-analytics-workspace-and-automation-account-planning)）。

- 連結自動化帳戶和 Log Analytics 工作區。 如果您是使用 Azure 入口網站進行上架，則不需要執行此步驟。

- 啟用工作區的更新管理，以及變更追蹤和清查。

- 使用 Azure 原則將 Azure Vm 上架。 原則會在 Azure Vm 上安裝 Log Analytics 代理程式和 Microsoft Dependency Agent。

- 藉由在內部部署伺服器上安裝 Log Analytics 代理程式來將其上架。

下表中所述的檔案會在此範例中使用。 您可以自訂它們以支援您自己的部署案例。

| 檔案名稱 | 描述 |
|-----------|-------------|
| New-AMSDeployment. ps1 | 主要的協調腳本會自動上線。 它會建立資源群組，以及位置、工作區和自動化帳戶（如果尚未存在）。 此 PowerShell 腳本需要現有的訂用帳戶。 |
| 工作區-AutomationAccount. json | 部署工作區和自動化帳戶資源的 Resource Manager 範本。 |
| WorkspaceSolutions json | Resource Manager 範本，可在 Log Analytics 工作區中啟用您想要的解決方案。 |
| ScopeConfig json | Resource Manager 範本，其使用具有變更追蹤解決方案之內部部署伺服器的加入宣告模型。 使用加入宣告模型是選擇性的。 |
| Enable-VMInsightsPerfCounters. ps1 | 為伺服器啟用 VM 深入解析並設定效能計數器的 PowerShell 腳本。 |
| 變更追蹤-Filelist. json | Resource Manager 範本，定義將由變更追蹤監視的檔案清單。 |

使用下列命令來執行 New-AMSDeployment：

```powershell
.\New-AMSDeployment.ps1 -SubscriptionName '{Subscription Name}' -WorkspaceName '{Workspace Name}' -WorkspaceLocation '{Azure Location}' -AutomationAccountName {Account Name} -AutomationAccountLocation {Account Location}
```

## <a name="next-steps"></a>後續步驟

瞭解如何設定基本警示，以通知您的小組金鑰管理事件和問題。

> [!div class="nextstepaction"]
> [設定基本警示](./setup-alerts.md)
