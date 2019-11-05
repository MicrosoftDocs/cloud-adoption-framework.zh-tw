---
title: 使用 Terraform 來建立您的登陸區域
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 瞭解如何使用 Terraform 來建立您的登陸區域。
author: arnaudlh
ms.author: arnaul
ms.date: 10/16/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: deebe6db08d573872f67d79f734d1f65a85c6904
ms.sourcegitcommit: bf9be7f2fe4851d83cdf3e083c7c25bd7e144c20
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/04/2019
ms.locfileid: "73561694"
---
# <a name="use-terraform-to-build-your-landing-zones"></a>使用 Terraform 來建立您的登陸區域

Azure 提供原生服務來部署您的登陸區域。 其他協力廠商工具也可以協助您進行這種作業。 客戶和合作夥伴通常用來部署登陸區域的一種工具，是 Hashicorp 的 Terraform。 本節說明如何使用原型登陸區域來部署 Azure 訂用帳戶的基本記錄、計量和安全性功能。

## <a name="purpose-of-the-landing-zone"></a>登陸區域的用途

適用于 Terraform 的雲端採用架構基礎登陸區域具有一組有限的責任和功能，可強制執行記錄、帳戶處理和安全性。 此登陸區域會使用稱為 Terraform 模組的標準元件，對環境中部署的所有資源強制執行一致性。

## <a name="use-standard-modules"></a>使用標準模組

重複使用元件是基礎結構即程式碼的基本原則。 模組有助於定義跨環境內和跨環境之資源部署的標準和一致性。 用來部署第一個登陸區域的模組可在官方[Terraform](https://registry.terraform.io/search?q=aztfmod)登錄中取得。

## <a name="architecture-diagram"></a>架構圖表

第一個登陸區域會在您的訂用帳戶中部署下列元件：

![使用 Terraform 的基礎登陸區域](../../_images/ready/foundations-terraform-landingzone.png)

## <a name="capabilities"></a>功能

已部署的元件及其用途包括下列各項：

| 元件 | 宣言 |
|---------|---------|
| 資源群組 | 基礎所需的核心資源群組 |
| 活動記錄 | 審核所有訂用帳戶活動和封存： </br> -儲存體帳戶 </br> -Azure 事件中樞 |  
| 診斷記錄 | 保留特定天數的所有作業記錄： </br> -儲存體帳戶 </br> -事件中樞 |
| Log Analytics | 儲存所有作業記錄 </br> 針對深度應用程式的最佳做法審查部署常見的解決方案： </br> -NetworkMonitoring </br> - ADAssessment </br> -Get-adreplication </br> - AgentHealthAssessment </br> - DnsAnalytics </br> - KeyVaultAnalytics
| Azure 資訊安全中心 | 傳送至電子郵件和電話號碼的安全性防護計量和警示 |

## <a name="use-this-blueprint"></a>使用此藍圖

使用雲端採用架構基礎登陸區域之前，請先參閱下列假設、決策和實施指引。

## <a name="assumptions"></a>假設

定義此初始登陸區域時，會考慮下列假設或條件約束。 如果這些假設符合您的條件約束，您可以使用藍圖來建立您的第一個登陸區域。 藍圖也可以加以擴充，以建立符合您特有條件限制的登陸區域藍圖。

- **訂用帳戶限制：** 這種採用成果不太可能會超過訂用帳戶[限制](https://docs.microsoft.com/azure/azure-subscription-service-limits)。 兩個常見指標為超過 25,000 部 VM 或 10,000 個 vCPU。
- **合規性：** 此登陸區域不需要協力廠商合規性需求。
- **架構複雜度：** 架構複雜度不需要額外的生產訂用帳戶。
- **共用服務：** 在 Azure 中，沒有任何現有的共用服務需要將此訂用帳戶視為中樞和輪輻架構中的輪輻。

如果這些假設符合您目前的環境，此藍圖可能是開始建立登陸區域的好方法。

## <a name="design-decisions"></a>設計決策

下列決策會在 Terraform 登陸區域中呈現：

| 元件 | 決策 | 替代方法 |
| --- | --- | --- |
|記錄和監視 | 使用 Azure 監視器 Log Analytics 工作區。 已布建診斷儲存體帳戶和事件中樞。 |         |
|網路 | N/A-網路會在另一個登陸區域中執行。 |[網路決策](../considerations/networking-options.md) |
|身分識別 | 假設訂用帳戶已經與 Azure Active Directory 執行個體相關聯。 | [身分識別管理最佳做法](https://docs.microsoft.com/azure/security/azure-security-identity-management-best-practices) |
| 原則 | 此登陸區域目前假設不會套用任何 Azure 原則。 | |
|訂用帳戶設計 | N/A - 專為單一生產訂用帳戶所設計。 | [調整訂用帳戶](../azure-best-practices/scaling-subscriptions.md) |
| 管理群組 | N/A - 專為單一生產訂用帳戶所設計。 |[調整訂用帳戶](../azure-best-practices/scaling-subscriptions.md) |
| 資源群組 | N/A - 專為單一生產訂用帳戶所設計。 | [調整訂用帳戶](../azure-best-practices/scaling-subscriptions.md) |
| 資料 | N/A | 在 Azure 和[Azure 資料存放區](https://docs.microsoft.com/azure/architecture/guide/technology-choices/data-store-overview)[中選擇正確的 SQL Server 選項](https://docs.microsoft.com/azure/sql-database/sql-database-paas-vs-sql-server-iaas) |
|儲存體|N/A|[Azure 儲存體指引](../considerations/storage-options.md) |
| 命名標準 | 建立環境時，也會建立唯一的前置詞。 需要全域唯一名稱的資源（例如儲存體帳戶）會使用此前置詞。 自訂名稱會附加一個隨機尾碼。 依照下表所述，會強制執行標記使用方式。 | [命名和標記最佳做法](../azure-best-practices/naming-and-tagging.md) |
| 成本管理 | N/A | [追蹤成本](../azure-best-practices/track-costs.md) |
| 計算 | N/A | [計算選項](../considerations/compute-options.md) |

### <a name="tagging-standards"></a>標記標準

所有資源和資源群組都必須有下列一組最小標記：

| 標籤名稱 | 說明 | 金鑰 | 範例值 |
|--|--|--|--|
| 業務單位 | 您公司中擁有該資源所屬訂用帳戶或工作負載的最上層部門。 | BusinessUnit | 財務，行銷，{產品名稱}，CORP，共用 |
| 成本中心 | 與此資源相關聯的會計成本中心。| CostCenter | Number |
| 災害復原 | 應用程式、工作負載或服務的業務關鍵性。 | 災難 | 啟用 DR、未啟用 DR |
| Environment | 應用程式、工作負載或服務的部署環境。 |  Env | 生產、開發、QA、階段、測試、訓練 |
| 擁有者名稱 | 應用程式、工作負載或服務的擁有者。| 擁有者 | 電子郵件 |
| 部署類型 | 定義維護資源的方式。 | deploymentType | Manual、Terraform |
| 版本 | 已部署藍圖的版本。 | 版本 | v 0。1 |
| 應用程式名稱 | 與資源相關聯的相關聯應用程式、服務或工作負載的名稱。 | ApplicationName | 「應用程式名稱」 |

## <a name="customize-and-deploy-your-first-landing-zone"></a>自訂和部署您的第一個登陸區域

您可以[複製 Terraform foundation 登陸區域](https://github.com/microsoft/CloudAdoptionFramework/tree/master/ready)。 藉由修改 Terraform 變數，即可輕鬆地開始使用登陸區域。 在我們的範例中，我們會使用**blueprint_foundations tfvars**，因此 Terraform 會自動為您設定此檔案中的值。

讓我們看看不同的變數區段。

在第一個物件中，我們會在名為 `-hub-core-sec` 的 `southeastasia` 區域中建立兩個資源群組 `-hub-operations`，並在執行時間新增一個前置詞。

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

接下來，我們會指定可設定基礎的區域。 在這裡，會使用 `southeastasia` 來部署所有資源。

```hcl
location_map = {
    region1   = "southeastasia"
    region2   = "eastasia"
}
```

然後，我們會指定作業記錄和 Azure 訂用客戶紀錄的保留期限。 這項資料會儲存在個別的儲存體帳戶和事件中樞，其名稱是隨機產生的，因為它們必須是唯一的。

```hcl
azure_activity_logs_retention = 365
azure_diagnostics_logs_retention = 60
```

在 tags_hub 中，我們會指定套用至所有已建立資源的最小標記集合。

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

然後，我們會指定 log analytics 名稱和一組分析部署的解決方案。 在這裡，我們保留了網路監視、Active Directory （AD）評估和複寫、DNS 分析和金鑰保存庫分析。

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

接下來，我們已設定 Azure 資訊安全中心的警示參數。

```hcl
# Azure Security Center Configuration
security_center = {
    contact_email   = "joe@contoso.com"
    contact_phone   = "+6500000000"
}
```

## <a name="get-started"></a>開始使用

在您檢查設定之後，就可以部署設定，就像部署 Terraform 環境一樣。 建議您使用 rover，這是允許從 Windows、Linux 或 MacOS 進行部署的 Docker 容器。 您可以開始使用[Rover GitHub 存放庫](https://github.com/aztfmod/rover)。

## <a name="next-steps"></a>後續步驟

基礎登陸區域會以分解的方式，為複雜的環境奠定基礎。 這個版本提供一組簡單的功能，可透過下列方式擴充：

- 將其他模組新增至藍圖。
- 將其他登陸區域分層在其上。

分層登陸區域是一個好方法，用來將系統分離、為您所使用的每個元件設定版本，以及為您的基礎結構即程式碼部署提供快速的創新和穩定性。

未來的參考架構將會針對中樞與輪輻拓撲示範此概念。

> [!div class="nextstepaction"]
> [查看基礎 Terraform 登陸區域範例](https://github.com/microsoft/CloudAdoptionFramework/tree/master/ready)
