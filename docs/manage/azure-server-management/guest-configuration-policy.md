---
title: 來賓設定原則
description: 使用適用于 Azure 的雲端採用架構，以瞭解如何使用 Azure 原則來賓設定延伸模組來審查 Azure VM 中的設定。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 73373e5cc56ef7e5804151171a22ad9f541f1cd3
ms.sourcegitcommit: 959cb0f63e4fe2d01fec2b820b8237e98599d14f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/11/2020
ms.locfileid: "79094693"
---
# <a name="guest-configuration-policy"></a>來賓設定原則

您可以使用 [Azure 原則[來賓](https://docs.microsoft.com/azure/governance/policy/concepts/guest-configuration)設定] 延伸模組來審查虛擬機器中的設定。 目前只有在 Azure Vm 上才支援來賓設定。

若要尋找來賓設定原則的清單，請在 Azure 原則入口網站頁面上搜尋「來賓設定」。 或在 PowerShell 視窗中執行此 Cmdlet 來尋找清單：

```powershell
Get-AzPolicySetDefinition | where-object {$_.Properties.metadata.category -eq "Guest Configuration"}
```

> [!NOTE]
> 來賓設定功能會定期更新，以支援其他原則集。 定期檢查是否有新支援的原則，並評估其是否有用。

<!-- TODO: Update these links when available. 

By default, we recommend that you enable the following policies:

- [Preview]: Audit to verify that password-security settings are correct on Linux and Windows machines.
- Audit to verify that certificates are not nearing expiration on Windows VMs.

-->

## <a name="deployment"></a>部署

使用下列範例 PowerShell 腳本，將這些原則部署到：

- 確認 Windows 和 Linux 電腦中的密碼安全性設定已正確設定。
- 確認 Windows Vm 上的憑證不會接近到期日。

 執行此腳本之前，請使用[disconnect-azaccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.1.0) Cmdlet 來登入。 當您執行腳本時，必須提供您想要套用原則的訂用帳戶名稱。

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
