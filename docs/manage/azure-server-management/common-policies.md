---
title: 常見的 Azure 原則範例
description: 使用適用于 Azure 的雲端採用架構，藉由使用 PowerShell Cmdlet 建立原則，以確保符合治理原則需求。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 3f6d5cbc2485a8e5a3752f24659e8873abb40a8d
ms.sourcegitcommit: afe10f97fc0e0402a881fdfa55dadebd3aca75ab
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/31/2020
ms.locfileid: "80430605"
---
# <a name="common-azure-policy-examples"></a>常見的 Azure 原則範例

[Azure 原則](https://docs.microsoft.com/azure/governance/policy/overview)可以協助您將治理套用至您的雲端資源。 此服務可協助您建立護欄，以確保公司範圍符合治理原則需求。 若要建立原則，請使用 Azure 入口網站或 PowerShell Cmdlet。 本文提供 PowerShell Cmdlet 範例。

> [!NOTE]
> 使用 Azure 原則，強制原則（**deployIfNotExists**）不會自動部署至現有的 vm。 必須進行補救以保持 Vm 合規性。 如需詳細資訊，請參閱[使用 Azure 原則補救不相容的資源](https://docs.microsoft.com/azure/governance/policy/how-to/remediate-resources)。

## <a name="common-policy-examples"></a>一般原則範例

下列各節說明一些常用的原則。

### <a name="restrict-resource-regions"></a>限制資源區域

法規和原則合規性通常取決於資源部署所在的實體位置控制。 您可以使用內建原則，讓使用者只能在特定允許的 Azure 區域中建立資源。

若要在入口網站中尋找此原則，請在 [原則定義] 頁面上搜尋「位置」。 或執行此 Cmdlet 來尋找原則：

```powershell
Get-AzPolicyDefinition | Where-Object { ($_.Properties.policyType -eq "BuiltIn") -and ($_.Properties.displayName -like "*location*") }
```

下列腳本顯示如何指派原則。 變更 [`$SubscriptionID`] 值，以指向您想要指派原則的訂用帳戶。 執行腳本之前，請使用[disconnect-azaccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.1.0) Cmdlet 來登入。

```powershell
#Specify the value for $SubscriptionID.
$SubscriptionID = <subscription ID>
$scope = "/subscriptions/$SubscriptionID"

#Replace the -Name GUID with the policy GUID you want to assign.
$AllowedLocationPolicy = Get-AzPolicyDefinition -Name "e56962a6-4747-49cd-b67b-bf8b01975c4c"

#Replace the locations with the ones you want to specify.
$policyParam = '{"listOfAllowedLocations":{"value":["eastus","westus"]}}'
New-AzPolicyAssignment -Name "Allowed Location" -DisplayName "Allowed locations for resource creation" -Scope $scope -PolicyDefinition $AllowedLocationPolicy -Location eastus -PolicyParameter $policyParam
```

您也可以使用此腳本來套用本文中討論的其他原則。 只要以您想要套用之原則的 GUID 取代程式程式碼中的 GUID，以設定 `$AllowedLocationPolicy`。

### <a name="block-certain-resource-types"></a>封鎖特定資源類型

另一個用來控制成本的常見內建原則也可以用來封鎖特定的資源類型。

若要在入口網站中尋找此原則，請在 [原則定義] 頁面上搜尋「允許的資源類型」。 或執行此 Cmdlet 來尋找原則：

```powershell
Get-AzPolicyDefinition | Where-Object { ($_.Properties.policyType -eq "BuiltIn") -and ($_.Properties.displayName -like "*allowed resource types") }
```

識別您想要使用的原則之後，您可以在 [[限制資源區域](#restrict-resource-regions)] 區段中修改 PowerShell 範例來指派原則。

### <a name="restrict-vm-size"></a>限制 VM 大小

Azure 提供各式各樣的 VM 大小來支援各種工作負載。 若要控制您的預算，您可以建立只允許訂用帳戶中的 VM 大小子集的原則。

### <a name="deploy-antimalware"></a>部署反惡意程式碼

您可以使用此原則，將具有預設設定的 Microsoft *IaaSAntimalware*擴充功能部署至未受反惡意程式碼保護的 vm。

原則 GUID `2835b622-407b-4114-9198-6f7064cbe0dc`。

下列腳本顯示如何指派原則。 若要使用腳本，請將 `$SubscriptionID` 值變更為指向您想要指派原則的訂用帳戶。 執行腳本之前，請使用[disconnect-azaccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.1.0) Cmdlet 來登入。

```powershell
#Specify the value for $SubscriptionID.
$SubscriptionID = <subscription ID>
$scope = "/subscriptions/$SubscriptionID"

$AntimalwarePolicy = Get-AzPolicyDefinition -Name "2835b622-407b-4114-9198-6f7064cbe0dc"

#Replace location "eastus" with the value that you want to use.
New-AzPolicyAssignment -Name "Deploy Antimalware" -DisplayName "Deploy default Microsoft IaaSAntimalware extension for Windows Server" -Scope $scope -PolicyDefinition $AntimalwarePolicy -Location eastus –AssignIdentity

```

## <a name="next-steps"></a>後續步驟

瞭解其他可用的伺服器管理工具和服務。

> [!div class="nextstepaction"]
> [Azure 伺服器管理工具和服務](./tools-services.md)
