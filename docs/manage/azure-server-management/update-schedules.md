---
title: 建立更新排程
description: 使用適用于 Azure 的雲端採用架構，瞭解如何使用 Azure 入口網站或新的 PowerShell Cmdlet 模組來管理更新排程。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: internal
ms.openlocfilehash: 1df49da24679b6dd241f4fc7e550411660afad60
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97016692"
---
# <a name="create-update-schedules"></a>建立更新排程

您可以使用 Azure 入口網站或新的 PowerShell Cmdlet 模組來管理更新排程。

若要透過 Azure 入口網站建立更新排程，請參閱 [排程更新部署](/azure/automation/update-management/deploy-updates#schedule-an-update-deployment)。

此 `Az.Automation` 模組現在支援使用 Azure PowerShell 來設定更新管理。 模組的[版本 1.7.0](https://www.powershellgallery.com/packages/Az/1.7.0)新增[AzAutomationUpdateManagementAzureQuery](/powershell/module/az.automation/new-azautomationupdatemanagementazurequery?view=azps-1.7.0) Cmdlet 的支援。 此 Cmdlet 可讓您使用標籤、位置和已儲存的搜尋，來設定彈性電腦群組的更新排程。

## <a name="example-script"></a>範例指令碼

本節中的範例腳本說明如何使用標記和查詢來建立可套用更新排程的動態電腦群組。 它會執行下列動作。 當您建立自己的腳本時，可以參考特定動作的執行方式。

- 建立會在每個星期六上午8:00 執行的 Azure 自動化更新排程。
- 針對符合下列準則的任何電腦建立查詢：
  - 部署在 `westus` 、 `eastus` 或 `eastus2` Azure 位置。
  - 具有 `Owner` 將值設定為的標記套用 `JaneSmith` 。
  - 具有 `Production` 將值設定為的標記套用 `true` 。
- 將更新排程套用到查詢的電腦，並設定兩小時的更新時段。

執行範例腳本之前，您必須使用 [disconnect-azaccount](/powershell/module/az.accounts/connect-azaccount?view=azps-2.1.0) Cmdlet 登入。 當您啟動腳本時，請提供下列資訊：

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
        [string] $SubscriptionId,

        [Parameter(Mandatory=$true)]
        [string] $ResourceGroupName,

        [Parameter(Mandatory=$true)]
        [string] $WorkspaceName,

        [Parameter(Mandatory=$true)]
        [string] $AutomationAccountName,

        [Parameter(Mandatory=$false)]
        [string] $scheduleName = "SaturdayCriticalSecurity"
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
    $ownerTag = @{ "Owner"= @("JaneSmith") }
    $ownerTag.Add("Production", "true")

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

請參閱如何 [在 Azure 中](./common-policies.md) 執行可協助管理伺服器的一般原則範例。

> [!div class="nextstepaction"]
> [Azure 中常見的原則](./common-policies.md)
