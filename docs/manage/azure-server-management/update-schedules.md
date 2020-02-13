---
title: 建立更新排程
description: 使用 Azure 入口網站或新的 PowerShell Cmdlet 模組來管理更新排程。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 2f049fce8c109c3028fa84c5822c0eb1053023bb
ms.sourcegitcommit: 238e7a06b56950cebdcc8f75924849fc995e6ff2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2020
ms.locfileid: "77173346"
---
# <a name="create-update-schedules"></a>建立更新排程

您可以使用 Azure 入口網站或新的 PowerShell Cmdlet 模組來管理更新排程。

若要透過 Azure 入口網站建立更新排程，請參閱[排程更新部署](https://docs.microsoft.com/azure/automation/automation-tutorial-update-management#schedule-an-update-deployment)。

Az. Automation 模組現在支援使用 Azure PowerShell 來設定更新管理。 模組的[版本 1.7.0](https://www.powershellgallery.com/packages/Az/1.7.0)新增[AzAutomationUpdateManagementAzureQuery](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationupdatemanagementazurequery?view=azps-1.7.0) Cmdlet 的支援。 此 Cmdlet 可讓您使用標籤、位置和已儲存的搜尋，為彈性的機器群組設定更新排程。

## <a name="example-script"></a>範例指令碼

本節中的範例腳本說明如何使用標記和查詢來建立可將更新排程套用至的動態電腦群組。 它會執行下列動作。 當您建立自己的腳本時，可以參考特定動作的執行。

- 建立在每個星期六上午8:00 執行的 Azure 自動化更新排程。
- 針對符合下列準則的機器建立查詢：
  - 部署于 `westus`、`eastus`或 `eastus2` Azure 位置
  - 已將 `Owner` 標籤套用至其值設定為 `JaneSmith`
  - 使用設定為的值，將 `Production` 標記套用至 `true`
- 將更新排程套用至查詢的機器，並設定兩個小時的更新視窗。

執行範例腳本之前，您必須使用[disconnect-azaccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.1.0) Cmdlet 來登入。 當您啟動腳本時，請提供下列資訊：

- 目標訂用帳戶識別碼
- 目標資源群組
- 您的 Log Analytics 工作區名稱
- 您的 Azure 自動化帳戶名稱

```powershell

    <#
        .SYNOPSIS
            This script orchestrates the deployment of the solutions and the agents.
        .Parameter SubscriptionName
        .Parameter WorkspaceName
        .Parameter AutomationAccountName
        .Parameter ResourceGroupName

    #>

    param (
        [Parameter(Mandatory=$true)]
        [string]$SubscriptionId,

        [Parameter(Mandatory=$true)]
        [string]$ResourceGroupName,

        [Parameter(Mandatory=$true)]
        [string]$WorkspaceName,

        [Parameter(Mandatory=$true)]
        [string]$AutomationAccountName,

        [Parameter(Mandatory=$false)]
        [string]$scheduleName = "SaturdayCritialSecurity"
    )

    Import-Module Az.Automation

    $startTime = ([DateTime]::Now).AddMinutes(10)
    $schedule = New-AzAutomationSchedule -ResourceGroupName $ResourceGroupName `
        -AutomationAccountName $AutomationAccountName `
        -StartTime $startTime `
        -Name $scheduleName `
        -Description "Saturday patches" `
        -DaysOfWeek Saturday `
        -WeekInterval 1 `
        -ForUpdateConfiguration

    # Using AzAutomationUpdateManagementAzureQuery to create dynamic groups.

    $queryScope = @("/subscriptions/$SubscriptionID/resourceGroups/")

    $query1Location =@("westus", "eastus", "eastus2")
    $query1FilterOperator = "Any"
    $ownerTag = @{"Owner"= @("JaneSmith")}
    $ownerTag.add("Production", "true")

    $DGQuery = New-AzAutomationUpdateManagementAzureQuery -ResourceGroupName $ResourceGroupName `
        -AutomationAccountName $AutomationAccountName `
        -Scope $queryScope `
        -Tag $ownerTag

    $AzureQueries = @($DGQuery)

    $UpdateConfig = New-AzAutomationSoftwareUpdateConfiguration -ResourceGroupName $ResourceGroupName `
        -AutomationAccountName $AutomationAccountName `
        -Schedule $schedule `
        -Windows `
        -Duration (New-TimeSpan -Hours 2) `
        -AzureQuery $AzureQueries `
        -IncludedUpdateClassification Security,Critical
```

## <a name="next-steps"></a>後續步驟

請參閱如何[在 Azure 中](./common-policies.md)執行可協助管理伺服器之通用原則的範例。

> [!div class="nextstepaction"]
> [Azure 中的一般原則](./common-policies.md)
