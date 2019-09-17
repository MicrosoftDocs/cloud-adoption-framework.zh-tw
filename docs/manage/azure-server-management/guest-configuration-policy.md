---
title: 來賓設定原則
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 來賓設定原則
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: d51cef67c96057b1929ed8cc86396f20338e932b
ms.sourcegitcommit: 443c28f3afeedfbfe8b9980875a54afdbebd83a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2019
ms.locfileid: "71029165"
---
# <a name="guest-configuration-policy"></a>來賓設定原則

Azure 原則[來賓](/azure/governance/policy/concepts/guest-configuration)設定延伸模組可讓您在虛擬機器中, 對設定進行審核。 目前只有在 Azure Vm 上才支援來賓設定。

您可以在 Azure 原則入口網站頁面上搜尋「來賓設定」類別, 以尋找來賓設定原則的清單。 您也可以在 PowerShell 視窗中執行此 Cmdlet 來尋找清單:

```powershell
Get-AzPolicySetDefinition | where-object {$_.Properties.metadata.category -eq "Guest Configuration"}
```

> [!NOTE]
> 來賓設定功能會定期更新, 以支援其他原則集。 定期檢查是否有新支援的原則, 並評估其是否適合您的需求。

<!-- TODO: Update these links when available. 

By default, we recommend enabling the following policies:

- [Preview]: Audit to verify password security settings are set correctly inside Linux and Windows machines.
- Audit to verify that certificates are not nearing expiration on Windows VMs.

-->

## <a name="deployment"></a>部署

您可以使用下列範例 PowerShell 腳本來部署這些原則:

- 確認 Windows 和 Linux 電腦中的密碼安全性設定已正確設定。
- 確認 Windows Vm 上的憑證不會接近到期日。

 執行此腳本之前, 您必須使用[disconnect-azaccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.1.0) Cmdlet 來登入。 當您執行腳本時, 您必須提供要套用原則的訂用帳戶名稱。

```powershell
#Assign Guest Configuration policy.
param (
    [Parameter(Mandatory=$true)]
    [string]$SubscriptionName
)

$Subscription = Get-AzSubscription -SubscriptionName $SubscriptionName
$scope = "/subscriptions/" + $Subscription.Id

$PasswordPolicy = Get-AzPolicySetDefinition -Name "3fa7cbf5-c0a4-4a59-85a5-cca4d996d5a6"
$CertExpirePolicy = Get-AzPolicySetDefinition -Name "b6f5e05c-0aaa-4337-8dd4-357c399d12ae"

New-AzPolicyAssignment -Name "PasswordPolicy" -DisplayName "[Preview]: Audit that password security settings are set correctly inside Linux and Windows machines" -Scope $scope -PolicySetDefinition $PasswordPolicy -AssignIdentity -Location eastus

New-AzPolicyAssignment -Name "CertExpirePolicy" -DisplayName "[Preview]: Audit that certificates are not expiring on Windows VMs" -Scope $scope -PolicySetDefinition $CertExpirePolicy -AssignIdentity -Location eastus
```

## <a name="next-steps"></a>後續步驟

瞭解如何針對重要的檔案、服務、軟體和登錄變更[啟用變更追蹤和警示](./enable-tracking-alerting.md)。

> [!div class="nextstepaction"]
> [啟用重大變更的追蹤和警示](./enable-tracking-alerting.md)
