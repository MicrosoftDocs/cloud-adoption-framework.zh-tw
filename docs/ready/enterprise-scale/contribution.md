---
title: 貢獻指南
description: 貢獻指南。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 9c5dc6a16fb6e24b498921f0e21c29d45545f8d6
ms.sourcegitcommit: bcc73d194c6d00c16ae2e3c7fb2453ac7dbf2526
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/09/2020
ms.locfileid: "86194760"
---
# <a name="contribution-guide"></a>貢獻指南

## <a name="enterprise-scale-committee"></a>企業規模的委員會

企業規模的委員會及其成員是企業級存放庫及其語言、設計和 Contoso 實施的主要 caretakers。

目前的委員會成員：

<!-- docsTest:disable -->
<!-- cSpell:disable -->

- Uday Pandya
- Callum 棺木打開
- Kristian Nese
- Victor Arzate
- Johan Dahlbom
- Lyon 截止
- Niels Buit

<!-- docsTest:enable --->
<!-- cSpell:enable -->

## <a name="committee-member-responsibilities"></a>委員會成員責任

委員會成員負責審查和核准 (Rfc 的意見要求，) 提議新功能或設計變更。

最初的企業規模委員會是由 Microsoft 員工所組成。 此社區預期會成長，而新的社區成員會在一段時間後加入委員會成員。 成員資格取決於貢獻程度和專業知識。 以有意義的方式參與專案的人員將會據此辨識。

一間委員會成員可以提名一個強大的社區成員，隨時加入委員會。 提名應以 Rfc 的形式提交，詳述為何該個人符合資格，以及它們的貢獻方式。 在討論過 RFC 之後，將需要匿名投票，才能確認新的委員會成員。

## <a name="contribution-scope-for-enterprise-scale-architecture-guidelines"></a>企業規模架構指導方針的貢獻範圍

此存放庫的貢獻範圍是隨著平臺發展，而新的服務和功能會在生產環境中與客戶進行驗證，設計指導方針則受限於架構整體內容中的更新。 使用預留位置範本提交提取要求 (Pr) 以取得檔更新。

## <a name="contribution-scope-for-contoso-reference-implementation"></a>Contoso 參考實行的貢獻範圍

當新的服務、資源、資源屬性和 API 版本出現時，必須據以更新「執行指南」和「參考」執行。 程式碼投稿主要著重于 Contoso 執行 Microsoft Azure 原則定義和指派。

<!-- cSpell:ignore azops apiversion northeurope -->

## <a name="how-to-submit-a-pr-to-the-upstream-repo"></a>如何將 PR 提交至上游存放庫

1. 藉由執行下列命令，建立以上游/主機為基礎的新分支：

    ```shell
    git checkout -b feature upstream/master
    ```

2. 從您想要包含在 PR 中的工作分支簽出檔案：

    ```shell
    #substitute file name as appropriate. below example
    git checkout feature: .\.github\workflows\azops-push.yml
    ```

3. 將您的 Git 分支推送至您的原始來源：

    ```shell
    git push origin -u
    ```

4. 建立從上游到遠端分支的 PR `master` 。

## <a name="writing-azure-resource-manager-templates-for-contoso-implementation"></a>為 Contoso 的執行撰寫 Azure Resource Manager 範本

沒有適當或錯誤的方式可撰寫 Resource Manager 範本和參數檔案。 Resource Manager 是一種語言，而且每個人都有不同的書寫樣式。 少數的範本和參數檔案在一組開發人員之間會以類似的方式撰寫。 沒有清楚的樣式定義可管理設定中的程式碼，以及叢集要轉譯的範本與參數檔。 使用參數和物件做為參數的指引 (不含任何架構) 也會受到轉譯，而且沒有任何一種適用的撰寫樣式。

為了讓多個開發人員能夠大規模地簡化開發和單元測試，我們採用了一種特定的撰寫範本樣式，方法是將範本從其參數檔案完全分離。 這種極簡的單一範本-all 方法會將所有資源屬性顯現出為參數檔案中的複雜物件，而且我們可以根據平臺已發行的資源架構，在檔案中強制執行嚴格的架構驗證。 這個方法會清楚地分隔範本和參數。 在呼叫或時，參數檔案基本上是資源的 RESTful 表示 `Get-AzResource` 法 `az resource show` 。

- Template.js于

```json
"resources": [{
        "condition": "[bool(equals(variables('resourceType'),'Microsoft.Authorization/policyDefinitions'))]",
        "type": "Microsoft.Authorization/policyDefinitions",
        "name": "[variables('policyDefinitions').name]",
        "apiVersion": "[variables('apiversion')[variables('resourceType')]]",
        "location": "northeurope",
        "properties": "[variables('policyDefinitions').Properties]"
    }],
```

您可以使用[一般多資源範本](https://github.com/uday31in/AzOps/blob/main/template/template.json)，以確保錯誤修正會併入最新的 API 版本。

- Template.parameters.js于

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "input": {
            "value": {
                <copy-paste-value-of-powershell/cli-output-here>
            }
        }
    }
}
```

藉由呼叫函式 `Get-AzResource` 並提供 `resourceId` 給現有的資源，來抓取資源定義。

```powershell
# Replace the resourceId in the command below before executing it

Get-AzResource -ResourceId '/providers/Microsoft.Management/managementGroups/contoso/providers/Microsoft.Authorization/policyDefinitions/DINE-Diagnostics-ActivityLog' | ConvertTo-Json -depth 100
```

<!-- docsTest:ignore "Policy definition" -->

進行設計決策時，應該考慮下列優缺點：

- 優點：

  - 不會再撰寫 Resource Manager 範本。 已寫入最後一個 Resource Manager 範本。
  - 無論是透過 Azure 入口網站、Azure CLI、PowerShell 或協力廠商工具來建立和更新資源，都能以一致的方式在其生命週期中進行匯出。
  - 在 Git 中儲存的設定與目前的設定相比，可以更輕鬆地偵測到漂移。基本上，我們要比較兩個 JSON 檔。
  - 您可以管理用戶端與伺服器端上的簡單資源之間的隱含相依性。 Azure 在資源之間不會有許多迴圈相依性，而且可以根據已發行的資源架構來處理隱含相依性。 例如，虛擬機器可能相依于核心層級虛擬交換器，但不會反過來 (例如，原則定義-> 原則指派-> 角色指派-> 補救或虛擬網路 > ExpressRoute 或 Key Vault > Azure SQL) 。

- 缺點：

  撰寫參數檔案的複雜物件時，可能會喪失智慧。 藉由先抓取目前資源的基底定義或透過入口網站建立資源，即可減輕這個一次性活動。 您無法使用 Azure-合作夥伴-客戶-使用方式-屬性來追蹤範本部署。 這不在企業規模的範圍內。

目前用於資源部署的 Resource Manager 範本不會有任何問題，也不會預期重寫它們。 管線會繼續部署它們，並偵測設定漂移。 我們無法協調它們，因為平臺不允許匯出部署範本來完成此動作。 因為這些參數，在 Pr 內提交的所有範本都必須符合您所部署之內容的原則。

在提交 PR 之前，請先閱讀下一節，但不要提交具有範本和參數檔案的 PR 來部署資源（例如 Azure Key Vault 或 Azure 監視器記錄），而不需要將其包裝在原則內。

## <a name="contributing-to-policy-definitions-policy-assignments-and-role-definitions-and-assignments-for-contoso-implementation"></a>為 Contoso 執行提供原則定義、原則指派和角色定義和指派的貢獻

一旦您的參數符合上一節所述的標準，且已準備好您的資源，請考慮是否應將資源部署在管理群組範圍或訂用帳戶範圍 (連線或管理訂用帳戶) 。 雖然管線可以在四個範圍的任何一個部署範本，但它不會部署在資源群組層級做為登陸區域範本的一部分。 最小的長條是包裝在原則定義內的訂用帳戶層級部署範本。

- 可行事項：
  - 如果您有可在登陸區域內部署的資源，請將其包裝在 `DeployIfNotExists` 原則中; 此作業的指派應位於管理群組範圍內。
  - 如果登陸區域內的資源部署計數只是一個 (例如，登陸區域內的虛擬網路，或是新 Azure 區域) 的虛擬中樞，則此原則應該會有目標為訂用帳戶範圍的存在範圍。
  - 在理想的情況下，所有原則定義都應該建立在端對端範本中定義的根目錄。

- 請勿提交具有範本和參數檔案的 PR 來部署資源 (例如 key vault) ，或在端對端登陸區域中所述的外部建立您自己的管理群組階層。

範例：

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "input": {
      "value": {
        "Name": "Tailspin",
        "DisplayName": "Tailspin",
        "ParentId": "/providers/Microsoft.Management/managementGroups/3fc1081d-6105-4e19-b60c-1ec1252cf560",
        "Children": [
          {
            "Id": "/providers/Microsoft.Management/managementGroups/Tailspin-bu1",
            "Name": "Tailspin-bu1",
            "DisplayName": "Tailspin-bu1",
            "properties": {
              "policyAssignments" :[
              ],
              "roleAssignments": [
              ]
            }
          }
        ],
        "properties": {
          "policyDefinitions": [
                <copy-paste of Json representation of the resource>
          ],
          "policyAssignments" :[
          ],
          "roleDefinitions": [
          ],
          "roleAssignments": [
          ]
        }
      }
    }
  }
}
```

## <a name="contributing-new-azure-policy-definitions-for-contoso-implementation"></a>為 Contoso 執行提供新的 Azure 原則定義

若要參與符合企業規模架構的原則定義，請使用下列工具和建議：

[Visual Studio 的 Azure 原則延伸模組](https://docs.microsoft.com/azure/governance/policy/how-to/extension-for-vscode)

使用此延伸模組來查閱原則別名，以及審查資源和原則。

## <a name="explore-available-resource-properties-with-corresponding-policy-aliases"></a>使用對應的原則別名探索可用的資源屬性

針對 Azure PowerShell：

```powershell
# List all available providers
Get-AzPolicyAlias -ListAvailable

# Get aliases for a specific resource provider
(Get-AzPolicyAlias -NamespaceMatch 'Microsoft.Network').Aliases.Name
```

對於 Azure CLI：

```bash
# List all available providers

az provider list --query [*].namespace

# Get aliases for a specific resource provider

az provider show --namespace Microsoft.Network --expand "resourceTypes/aliases" --query "resourceTypes[].aliases[].name"
```

## <a name="contributing-new-azure-policy-assignments"></a>參與新的 Azure 原則指派

指派所有原則時，必須考慮下列事項：

- 請特別瞭解指派的意圖。 它屬於兩個訂用帳戶 (連線和管理) 還是管理群組？
- 訂用帳戶內的資源散發為何？
- 使用的區域為何，以及每個訂用帳戶是否允許/使用多個區域？
- 允許哪些資源類型不會影響指派原則的位置？
- 針對多個服務相同或相似用途的原則，是否可以將它們組合成原則計畫？
- 原則的基本原理是什麼？ 稽核原則是否應改成強制執行？
- 針對 `DeployIfNotExists` 原則，您是否遵循角色型存取控制定義所使用之最低許可權的原則？

<!-- cSpell:ignore don'ts -->

## <a name="code-of-conduct"></a>管理辦法

我們致力於與我們的熱衷於社區建立強大且有效率的合作。 我們聽到您的聲音和清除。 我們正致力於使用 dos 和不規定的原則和指導方針。
