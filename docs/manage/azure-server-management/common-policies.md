---
title: 常見的 Azure 原則範例
description: 使用適用于 Azure 的雲端採用架構，藉由使用 PowerShell Cmdlet 來建立原則，以確保符合治理原則需求。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 9e4829581a642a3fab13d461c98e423a5f777f2b
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88567439"
---
# <a name="common-azure-policy-examples"></a>常見的 Azure 原則範例

[Azure 原則](/azure/governance/policy/overview) 可協助您將治理套用至您的雲端資源。 這種服務可協助您建立護欄，以確保符合治理原則需求的公司範圍。 若要建立原則，請使用 Azure 入口網站或 PowerShell Cmdlet。 本文提供 PowerShell Cmdlet 範例。

> [!NOTE]
> 使用 Azure 原則， () 的強制原則 `DeployIfNotExists` 不會自動部署到現有的 vm。 需要補救才能讓 Vm 符合規範。 如需詳細資訊，請參閱 [使用 Azure 原則補救不符合](/azure/governance/policy/how-to/remediate-resources)規範的資源。

## <a name="common-policy-examples"></a>一般原則範例

下列各節說明一些常用的原則。

### <a name="restrict-resource-regions"></a>限制資源區域

法規和原則合規性通常取決於資源部署所在之實體位置的控制權。 您可以使用內建原則，讓使用者只能在特定允許的 Azure 區域中建立資源。

若要在入口網站中尋找此原則，請在 [原則定義] 頁面上搜尋「位置」。 或者執行這個 Cmdlet 來尋找原則：

```powershell
Get-AzPolicyDefinition | Where-Object { ($_.Properties.policyType -eq 'BuiltIn') `
  -and ($_.Properties.displayName -like '*location*') }
```

下列腳本說明如何指派原則。 變更 `$SubscriptionID` 值以指向您想要指派原則的訂用帳戶。 執行腳本之前，請使用 [disconnect-azaccount](/powershell/module/az.accounts/connect-azaccount?view=azps-2.1.0) Cmdlet 來登入。

```powershell
# Specify the value for $SubscriptionID.
$SubscriptionID = <subscription ID>
$scope = "/subscriptions/$SubscriptionID"

# Replace the -Name GUID with the policy GUID you want to assign.
$AllowedLocationPolicy = Get-AzPolicyDefinition -Name "e56962a6-4747-49cd-b67b-bf8b01975c4c"

# Replace the locations with the ones you want to specify.
$policyParam = '{ "listOfAllowedLocations":{"value":["eastus","westus"]}}'
New-AzPolicyAssignment -Name "Allowed Location" -DisplayName "Allowed locations for resource creation" -Scope $scope -PolicyDefinition $AllowedLocationPolicy -Location eastus -PolicyParameter $policyParam
```

您也可以使用此腳本來套用本文中討論的其他原則。 只要取代以您要套用之原則的 GUID 設定的行中的 GUID `$AllowedLocationPolicy` 。

### <a name="block-certain-resource-types"></a>封鎖某些資源類型

另一個用來控制成本的常見內建原則也可以用來封鎖特定的資源類型。

若要在入口網站中尋找此原則，請在 [原則定義] 頁面上搜尋「允許的資源類型」。 或者執行這個 Cmdlet 來尋找原則：

```powershell
Get-AzPolicyDefinition | Where-Object { ($_.Properties.policyType -eq "BuiltIn") -and ($_.Properties.displayName -like "*allowed resource types") }
```

識別您想要使用的原則之後，您可以在 [ [限制資源區域](#restrict-resource-regions) ] 區段中修改 PowerShell 範例以指派原則。

### <a name="restrict-vm-size"></a>限制 VM 大小

Azure 提供各種不同的 VM 大小來支援各種工作負載。 若要控制您的預算，您可以建立只允許訂用帳戶中的 VM 大小子集的原則。

### <a name="deploy-antimalware"></a>部署反惡意程式碼

您可以使用此原則，將具有預設設定的 Microsoft _IaaSAntimalware_ 擴充功能，部署到未受反惡意程式碼保護的 vm。

原則 GUID 為 `2835b622-407b-4114-9198-6f7064cbe0dc` 。

下列腳本說明如何指派原則。 若要使用腳本，請將 `$SubscriptionID` 值變更為指向您想要指派原則的訂用帳戶。 執行腳本之前，請使用 [disconnect-azaccount](/powershell/module/az.accounts/connect-azaccount?view=azps-2.1.0) Cmdlet 來登入。

```powershell
# Specify the value for $SubscriptionID.
$subscriptionID = <subscription ID>
$scope = "/subscriptions/$subscriptionID"

$antimalwarePolicy = Get-AzPolicyDefinition -Name "2835b622-407b-4114-9198-6f7064cbe0dc"

# Replace location "eastus" with the value that you want to use.
New-AzPolicyAssignment -Name "Deploy Antimalware" -DisplayName "Deploy default Microsoft IaaSAntimalware extension for Windows Server" -Scope $scope -PolicyDefinition $antimalwarePolicy -Location eastus –AssignIdentity

```

## <a name="next-steps"></a>接下來的步驟

瞭解其他可用的伺服器管理工具和服務。

> [!div class="nextstepaction"]
> [Azure 伺服器管理工具和服務](./tools-services.md)
