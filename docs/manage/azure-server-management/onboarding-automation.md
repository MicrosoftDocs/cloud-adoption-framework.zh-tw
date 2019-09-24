---
title: 自動上線和警示設定
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 自動上線和警示設定
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 242c8a1a054507c3b1134b1126ea95e3ead74d84
ms.sourcegitcommit: d19e026d119fbe221a78b10225230da8b9666fe1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2019
ms.locfileid: "71221363"
---
# <a name="automate-onboarding"></a>自動上架

若要改善部署 Azure 伺服器管理服務的效率, 請考慮使用本指南前面幾節所討論的建議, 自動化管理服務的部署。 下列各節提供的腳本和範例範本, 是開發自己的上執行緒序自動化的起點。

## <a name="onboarding-by-using-automation"></a>使用自動化上架

本指南提供支援的 GitHub 存放庫, 其中包含範例程式碼[CloudAdoptionFramework](https://aka.ms/caf/manage/automation-samples), 它會提供範例腳本和 Azure Resource Manager 範本, 協助您將 Azure 伺服器管理服務的部署作業自動化。

這些範例檔案說明如何使用 Azure PowerShell Cmdlet 來自動化下列工作:

1. 建立[Log Analytics 工作區](https://docs.microsoft.com/azure/azure-monitor/platform/manage-access)(或使用現有的工作區 (如果符合需求&mdash;, 請參閱[工作區規劃](./prerequisites.md#log-analytics-workspace-and-automation-account-planning))。

2. 建立自動化帳戶 (或使用現有的帳戶, 如果符合需求&mdash;, 請參閱[工作區規劃](./prerequisites.md#log-analytics-workspace-and-automation-account-planning))。

3. 連結自動化帳戶和 Log Analytics 工作區 (如果是透過入口網站上架, 則不需要)。

4. 啟用工作區的更新管理和變更追蹤和清查。

5. 使用 Azure 原則將 Azure Vm 上架（原則會在 Azure Vm 上安裝 Log Analytics 代理程式和 Dependency Agent）。

6. 藉由在內部部署伺服器上安裝 Log Analytics 代理程式來將其上架。

下表中所述的檔案會在此範例中使用, 您可以自訂這些檔案以支援您自己的部署案例。

| 檔案名稱 | 描述 |
|-----------|-------------|
| New-AMSDeployment.ps1 | 主要的協調腳本會自動上線。 此 PowerShell 腳本需要現有的訂用帳戶, 但它會建立資源群組、位置、工作區和自動化帳戶 (如果不存在)。 |
| Workspace-AutomationAccount.json | 部署工作區和自動化帳戶資源的 Resource Manager 範本。 |
| WorkspaceSolutions.json | 在 Log Analytics 工作區中啟用所需解決方案的 Resource Manager 範本。 |
| ScopeConfig.json | Resource Manager 範本, 其使用具有變更追蹤解決方案之內部部署伺服器的加入宣告模型。 使用加入宣告模型是選擇性的。 |
| Enable-VMInsightsPerfCounters.ps1 | 可讓伺服器 VMInsight 及設定效能計數器的 PowerShell 腳本。 |
| ChangeTracking-Filelist.json | Resource Manager 範本, 定義將由變更追蹤監視的檔案清單。 |

您可以使用下列命令來執行 New-AMSDeployment:

```powershell
.\New-AMSDeployment.ps1 -SubscriptionName '{Subscription Name}' -WorkspaceName '{Workspace Name}' -WorkspaceLocation '{Azure Location}' -AutomationAccountName {Account Name} -AutomationAccountLocation {Account Location}
```

## <a name="next-steps"></a>後續步驟

瞭解如何設定基本警示, 以通知您的小組金鑰管理事件和問題。

> [!div class="nextstepaction"]
> [設定基本警示](./setup-alerts.md)
