---
title: Azure 原則來賓設定延伸模組
description: 使用適用于 Azure 的雲端採用架構，瞭解如何使用 Azure 原則的來賓設定延伸模組來審核 Azure VM 中的設定。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: internal
ms.openlocfilehash: cc67a0d31b32522a32c331e9fcd798e259b17e11
ms.sourcegitcommit: 9d76f709e39ff5180404eacd2bd98eb502e006e0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/17/2021
ms.locfileid: "100631911"
---
# <a name="azure-policy-guest-configuration-extension"></a>Azure 原則來賓設定延伸模組

您可以使用 [Azure 原則的來賓設定延伸](/azure/governance/policy/concepts/guest-configuration) 模組來審核虛擬機器中的設定。 目前只有 Azure Vm 支援來賓設定。

若要尋找來賓設定原則的清單，請在 Azure 原則入口網站頁面上搜尋「 **來賓** 設定」，或在 PowerShell 視窗中執行此 Cmdlet 來尋找清單：

```powershell
Get-AzPolicySetDefinition | where-object {$_.Properties.metadata.category -eq "Guest Configuration"}
```

> [!NOTE]
> 來賓設定功能會定期更新，以支援其他原則集。 定期檢查是否有新的支援原則，並評估是否有用。

## <a name="deployment"></a>部署

使用下列範例 PowerShell 腳本，將這些原則部署至：

- 確認已正確設定 Windows 和 Linux 電腦中的密碼安全性設定。
- 確認憑證在 Windows Vm 上未接近到期。

 執行此腳本之前，請使用 [disconnect-azaccount](/powershell/module/az.accounts/connect-azaccount) Cmdlet 來登入。 當您執行腳本時，您必須提供要套用原則的訂用帳戶名稱。

```powershell

    # Assign Guest Configuration policy.

    param (
        [Parameter(Mandatory=$true)]
        [string] $SubscriptionName
    )

    $Subscription = Get-AzSubscription -SubscriptionName $SubscriptionName
    $scope = "/subscriptions/" + $Subscription.Id

    $PasswordPolicy = Get-AzPolicySetDefinition -Name "3fa7cbf5-c0a4-4a59-85a5-cca4d996d5a6"
    $CertExpirePolicy = Get-AzPolicySetDefinition -Name "b6f5e05c-0aaa-4337-8dd4-357c399d12ae"

    New-AzPolicyAssignment -Name "PasswordPolicy" -DisplayName "[Preview]: Audit that password security settings are set correctly inside Linux and Windows machines" -Scope $scope -PolicySetDefinition $PasswordPolicy -AssignIdentity -Location eastus

    New-AzPolicyAssignment -Name "CertExpirePolicy" -DisplayName "[Preview]: Audit that certificates are not expiring on Windows VMs" -Scope $scope -PolicySetDefinition $CertExpirePolicy -AssignIdentity -Location eastus

```

## <a name="next-steps"></a>下一步

瞭解如何針對重要的檔案、服務、軟體和登錄變更 [啟用變更追蹤和警示](./enable-tracking-alerting.md) 。

> [!div class="nextstepaction"]
> [啟用重大變更的追蹤和警示](./enable-tracking-alerting.md)
