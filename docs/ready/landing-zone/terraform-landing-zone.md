---
title: 使用 Terraform 來建立登陸區域
description: 瞭解如何使用 Terraform by HashiCorp 來建立登陸區域。
author: arnaudlh
ms.author: brblanch
ms.date: 02/25/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: think-tank
ms.openlocfilehash: 0ad91b2d09e08bf05cca62323e18ed242ef5e1b6
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101785320"
---
<!-- cSpell:ignore eastasia southeastasia vCPUs lalogs tfvars NetworkMonitoring ADAssessment ADReplication AgentHealthAssessment DnsAnalytics KeyVaultAnalytics -->

# <a name="use-terraform-to-build-your-landing-zones"></a>使用 Terraform 來建立登陸區域

Azure 提供原生服務來部署登陸區域。 其他協力廠商工具也可以協助進行這種工作。 客戶和合作夥伴通常用來部署登陸區域的其中一種工具，是由 HashiCorp Terraform。 本節說明如何使用範例登陸區域來部署 Azure 訂用帳戶的基礎治理、計量和安全性功能。

## <a name="purpose-of-the-landing-zone"></a>登陸區域的用途

適用于 Terraform 的雲端採用架構基礎登陸區域會提供強制執行記錄、帳戶處理和安全性的功能。 此登陸區域使用稱為 Terraform 模組的標準元件，在環境中部署的資源之間強制執行一致性。

## <a name="use-standard-modules"></a>使用標準模組

重複使用元件是基礎結構即程式碼的基本原則。 模組有助於定義跨環境內與跨資源部署的標準和一致性。 用來部署第一個登陸區域的模組可在官方 [Terraform](https://registry.terraform.io/modules/aztfmod)登錄中取得。

## <a name="architecture-diagram"></a>架構圖

第一個登陸區域會在您的訂用帳戶中部署下列元件：

![基礎登陸區域使用 Terraform ](../../_images/ready/foundations-terraform-landing-zone.png)
 _圖1：使用 Terraform 的基礎登陸區域。_

<!-- docutune:casing NetworkMonitoring AdAssessment AdReplication AgentHealthAssessment DnsAnalytics KeyVaultAnalytics -->

## <a name="capabilities"></a>功能

部署的元件及其用途包括下列各項：

| 元件 | 責任 |
|---|---|
| 資源群組 | 基礎所需的核心資源群組 |
| 活動記錄 | 審核所有訂用帳戶活動和封存： <li> 儲存體帳戶 <li> Azure 事件中心 |
| 診斷記錄 | 所有作業記錄都會保留一天的特定天數： <li> 儲存體帳戶 <li> 事件中樞 |
| Log Analytics | 儲存作業記錄。 部署適用于深入應用程式最佳作法審核的常見解決方案： <li> NetworkMonitoring <li> AdAssessment <li> Get-adreplication <li> AgentHealthAssessment <li> DnsAnalytics <li> KeyVaultAnalytics |
| Azure 資訊安全中心 | 傳送至電子郵件和電話號碼的安全性防護計量和警示 |

## <a name="use-this-blueprint"></a>使用此藍圖

使用「雲端採用架構基礎」登陸區域之前，請先參閱下列假設、決策和實施指引。

## <a name="assumptions"></a>假設

定義此初始登陸區域時，會考慮下列假設或條件約束。 如果這些假設符合您的條件約束，您可以使用藍圖來建立您的第一個登陸區域。 藍圖也可以加以擴充，以建立符合您特有條件限制的登陸區域藍圖。

- **訂用帳戶限制：** 這項採用工作不太可能會超過訂用帳戶 [限制](/azure/azure-resource-manager/management/azure-subscription-service-limits)。 兩個常見指標為超過 25,000 部 VM 或 10,000 個 vCPU。
- **合規性：** 此登陸區域不需要任何協力廠商合規性需求。
- **架構複雜度：** 架構複雜性不需要額外的生產訂用帳戶。
- **共用服務：** 在 Azure 中，沒有任何現有的共用服務需要將此訂用帳戶視為中樞和輪輻架構中的輪輻。

如果這些假設符合您目前的環境，此藍圖可能是開始建立登陸區域的好方法。

## <a name="design-decisions"></a>設計決策

下列決策會以 CAF Terraform 模組表示：

| 元件              | 決策                                                                                                                                                                                                                                                                | 替代方法                                                                                                                                                                                                                                          |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 記錄和監視 | 使用 Azure 監視器 Log Analytics 工作區。 已布建診斷儲存體帳戶和事件中樞。                                                                                                                                                        |                                                                                                                                                                                                                                                                 |
| 網路                | N/A-網路是在另一個登陸區域中執行。                                                                                                                                                                                                                    | [網路決策](../considerations/networking-options.md)                                                                                                                                                                                                 |
| 身分識別               | 假設訂用帳戶已經與 Azure Active Directory 執行個體相關聯。                                                                                                                                                                        | [身分識別管理最佳做法](/azure/security/fundamentals/identity-management-best-practices)                                                                                                                               |
| 原則                 | 此登陸區域目前假設未套用任何 Azure 原則。                                                                                                                                                                                            |                                                                                                                                                                                                                                                                 |
| 訂用帳戶設計    | N/A-專為單一生產訂用帳戶所設計。                                                                                                                                                                                                                     | [建立初始訂閱](../azure-best-practices/initial-subscriptions.md)                                                                                                                                                                                  |
| 資源群組        | N/A-專為單一生產訂用帳戶所設計。                                                                                                                                                                                                                     | [調整訂用帳戶](../azure-best-practices/scale-subscriptions.md)                                                                                                                                                                                           |
| 管理群組      | N/A-專為單一生產訂用帳戶所設計。                                                                                                                                                                                                                     | [組織訂閱](../azure-best-practices/organize-subscriptions.md)                                                                                                                                                                                     |
| 資料                   | N/A                                                                                                                                                                                                                                                                      | 在 Azure 和[azure 資料存放區](/azure/architecture/guide/technology-choices/data-store-overview)[中選擇正確的 SQL Server 選項](/azure/sql-database/sql-database-paas-vs-sql-server-iaas) |
| 儲存體                | N/A                                                                                                                                                                                                                                                                      | [Azure 儲存體指引](../considerations/storage-options.md)                                                                                                                                                                                                  |
| 命名標準       | 建立環境時，也會建立唯一的前置詞。 需要全域唯一名稱的資源 (例如儲存體帳戶) 使用此前置詞。 自訂名稱會附加隨機尾碼。 標記使用方式會依照下表所述的方式來強制執行。 | [命名和標記最佳做法](../azure-best-practices/naming-and-tagging.md)                                                                                                                                                                              |
| 成本管理        | N/A                                                                                                                                                                                                                                                                      | [追蹤成本](../azure-best-practices/track-costs.md)                                                                                                                                                                                                        |
| 計算                | N/A                                                                                                                                                                                                                                                                      | [計算選項](../considerations/compute-options.md)                                                                                                                                                                                                         |

### <a name="tagging-standards"></a>標記標準

下列所示的最小標記集合必須存在於所有資源和資源群組中：

<!-- TODO: Review capitalization and hyphenation -->
<!-- TODO: Eliminate either "Tag name" or "Key" column -->

| 標籤名稱          | 描述                                                                                        | Key               | 範例值                                    |
|-------------------|----------------------------------------------------------------------------------------------------|-------------------|--------------------------------------------------|
| 業務單位     | 您公司中擁有該資源所屬訂用帳戶或工作負載的最上層部門。 | `BusinessUnit`    | `finance`, `marketing`, `<product-name>`, `corp`, `shared` |
| 成本中心       | 與此資源相關聯的會計成本中心。                                              | `CostCenter`      | `<cost-center-number>`                                     |
| 災害復原 | 應用程式、工作負載或服務的業務關鍵性。                                     | `DR`              | `dr-enabled`, `non-dr-enabled`                   |
| 環境       | 應用程式、工作負載或服務的部署環境。                                   | `Env`             | `prod`, `dev`, `qa`, `staging`, `test`, `training` |
| 擁有人名稱        | 應用程式、工作負載或服務的擁有者。                                                    | `Owner`           | `email`                                            |
| 部署類型   | 定義如何維護資源。                                                    | `DeploymentType`  | `manual`, `terraform`                                |
| 版本           | 已部署之藍圖的版本。                                                                 | `Version`         | `v0.1`                                             |
| 應用程式名稱  | 與資源相關聯的相關聯應用程式、服務或工作負載的名稱。             | `ApplicationName` | `<app-name>`                                       |

## <a name="customize-and-deploy-your-first-landing-zone"></a>自訂和部署您的第一個登陸區域

您可以 [複製 Terraform foundation 登陸區域](https://github.com/azure/caf-terraform-landingzones)。 藉由修改 Terraform 變數，即可輕鬆地開始使用登陸區域。 在我們的範例中，我們使用 **blueprint_foundations tfvars**，因此 Terraform 會自動為您設定此檔案中的值。

讓我們看看不同的變數區段。

在此第一個物件中，我們會在名為的區域中建立兩個資源群組， `southeastasia` `-hub-core-sec` 並在 `-hub-operations` 執行時間加入前置詞。

```hcl
resource_groups_hub = {
    HUB-CORE-SEC    = {
        name = "-hub-core-sec"
        location = "southeastasia"
    }
    HUB-OPERATIONS  = {
        name = "-hub-operations"
        location = "southeastasia"
    }
}
```

接下來，我們會指定可設定基礎的區域。 在這裡， `southeastasia` 會用來部署所有資源。

```hcl
location_map = {
    region1   = "southeastasia"
    region2   = "eastasia"
}
```

然後，我們會指定作業記錄和 Azure 訂用客戶紀錄的保留期限。 這項資料會儲存在個別的儲存體帳戶和事件中樞，其名稱會隨機產生，因為它們必須是唯一的。

```hcl
azure_activity_logs_retention = 365
azure_diagnostics_logs_retention = 60
```

在 tags_hub 中，我們會指定套用至所有所建立資源的最小標記集合。

```hcl
tags_hub = {
    environment     = "DEV"
    owner           = "Arnaud"
    deploymentType  = "Terraform"
    costCenter      = "65182"
    BusinessUnit    = "SHARED"
    DR              = "NON-DR-ENABLED"
}
```

然後，我們會指定 Log Analytics 名稱和一組分析部署的解決方案。 在這裡，我們保留了網路監視、Active Directory 評定和複寫、DNS 分析，以及金鑰保存庫分析。

```hcl

analytics_workspace_name = "lalogs"

solution_plan_map = {
    NetworkMonitoring = {
        "publisher" = "Microsoft"
        "product"   = "OMSGallery/NetworkMonitoring"
    },
    ADAssessment = {
        "publisher" = "Microsoft"
        "product"   = "OMSGallery/ADAssessment"
    },
    ADReplication = {
        "publisher" = "Microsoft"
        "product"   = "OMSGallery/ADReplication"
    },
    AgentHealthAssessment = {
        "publisher" = "Microsoft"
        "product"   = "OMSGallery/AgentHealthAssessment"
    },
    DnsAnalytics = {
        "publisher" = "Microsoft"
        "product"   = "OMSGallery/DnsAnalytics"
    },
    KeyVaultAnalytics = {
        "publisher" = "Microsoft"
        "product"   = "OMSGallery/KeyVaultAnalytics"
    }
}

```

接下來，我們設定了 Azure 安全性中心的警示參數。

```hcl
# Azure Security Center Configuration
security_center = {
    contact_email   = "joe@contoso.com"
    contact_phone   = "+6500000000"
}
```

## <a name="take-action"></a>採取動作

檢查設定之後，您可以部署設定，就像部署 Terraform 環境一樣。 建議您使用 rover，這是可讓您從 Windows、Linux 或 macOS 進行部署的 Docker 容器。 您可以開始使用 [登陸區域](https://github.com/azure/caf-terraform-landingzones)。

## <a name="next-steps"></a>下一步

基礎登陸區域以分解的方式為複雜的環境奠定基礎。 此版本提供一組簡單的功能，可透過將其他模組新增至藍圖或將其他登陸區域分層在其上進行擴充。

將登陸區域分層是將系統分離、為您所使用的每個元件進行版本控制，以及允許基礎結構作為程式碼部署的快速創新和穩定性的好作法。

未來的參考架構將示範中樞和輪輻拓撲的這個概念。

> [!div class="nextstepaction"]
> [複習範例 foundation Terraform 登陸區域](https://github.com/azure/caf-terraform-landingzones)
