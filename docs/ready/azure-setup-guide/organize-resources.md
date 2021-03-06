---
title: 有效地組織 Azure 資源
description: 了解有效組織 Azure 資源以簡化資源管理工作的最佳做法。
author: laraaleite
ms.author: brblanch
ms.date: 04/09/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.localizationpriority: high
ms.custom: think-tank, fasttrack-edit, AQC, setup
ms.openlocfilehash: c577c5c0b0362ff1c8dcc51e38d97d17450bd5bf
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102115419"
---
<!-- cSpell:ignore profx fsubscriptions fresource -->

# <a name="organize-your-azure-resources-effectively"></a>有效地組織 Azure 資源

組織雲端式資源對於保護、管理和追蹤工作負載相關的成本而言至關重要。 若要組織您的資源，請定義管理群組階層、遵循妥善考慮的命名慣例，並套用資源標記。

<!-- markdownlint-disable MD024 -->

## <a name="azure-management-groups-and-hierarchy"></a>[Azure 管理群組和階層](#tab/AzureManagementGroupsAndHierarchy)

Azure 提供四個管理範圍層級：管理群組、訂用帳戶、資源群組和資源。 下圖顯示這些層級的關聯性。

   ![管理階層關聯性的圖示](./media/organize-resources/scope-levels.png) *圖 1：四個管理範圍層級之間的關聯性。*

- **管理群組：** 這些群組是可協助您針對多個訂用帳戶管理存取、原則及合規性的容器。 管理群組內的所有訂用帳戶都會自動繼承套用到管理群組的條件。
- **訂用帳戶：** 訂用帳戶會以邏輯方式關聯使用者帳戶以及這些使用者帳戶所建立的資源。 每個訂用帳戶均有可供建立和使用的資源數量限制或配額。 組織可以使用訂用帳戶來管理成本以及由使用者、小組或專案所建立的資源。
- **資源群組：** 資源群組是一個邏輯容器，可在其中部署與管理 Azure 資源 (例如 Web 應用程式、資料庫和儲存體帳戶)。
- **資源：** 資源是所建立服務 (如虛擬機器、儲存體或 SQL 資料庫) 的執行個體。

### <a name="scope-of-management-settings"></a>管理設定的範圍

您可以在任何管理層級套用管理設定，例如原則和 Azure 角色型存取控制。 您選取的層級會決定套用設定的範圍。 較低層級會從較高層級繼承設定。 例如，當您將原則套用到訂用帳戶時，該訂用帳戶中的所有資源群組和資源也都會套用該原則。

通常我們會在較高層級套用重要設定，而在較低層級套用專案特定需求。 例如，您可能想要確定組織的所有資源都部署到特定區域。 若要這樣做，請將原則套用至指定了允許位置的訂用帳戶。 當貴組織中其他使用者新增新的資源群組和資源時，會自動強制執行允許的位置。 請深入了解本指南的控管、安全性和合規性區段中所提供的原則。

如果您只有幾個訂用帳戶，獨立管理這些訂用帳戶相當簡單。 如果您使用的訂用帳戶數量增加，則請考慮建立管理群組階層，以簡化訂用帳戶和資源的管理。 如需詳細資訊，請參閱[組織和管理您的 Azure 訂用帳戶](../azure-best-practices/organize-subscriptions.md)。

在規劃合規性策略時，請與組織中擔任下列角色的人員合作：安全性和合規性、IT 管理、企業架構設計人員、網路、財務和採購。

::: zone target="docs"

### <a name="create-a-management-level"></a>建立管理層級

您可以建立管理群組、其他訂用帳戶或資源群組。

#### <a name="create-a-management-group"></a>建立管理群組

建立管理群組，以協助您針對多個訂用帳戶管理存取、原則及合規性。

1. 請移至[管理群組](https://portal.azure.com/#blade/Microsoft_Azure_ManagementGroups/HierarchyBlade)。
2. 選取 [新增管理群組]。

#### <a name="create-a-subscription"></a>建立訂用帳戶

使用訂用帳戶來管理成本以及由使用者、小組或專案所建立的資源。

1. 請移至 [[訂用帳戶](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)]。
2. 選取 [新增]。

> [!NOTE]
> 亦可透過程式設計方式建立訂閱。 如需詳細資訊，請參閱[以程式設計方式建立 Azure 訂用帳戶](/azure/cost-management-billing/manage/programmatically-create-subscription)。

#### <a name="create-a-resource-group"></a>建立資源群組

建立資源群組，以容納共用相同生命週期、權限及原則的 Web 應用程式、資料庫和儲存體帳戶等資源。

1. 移至 [[資源群組](https://portal.azure.com/#create/Microsoft.ResourceGroup)]。
1. 選取 [新增]。
1. 選取要用來建立資源群組的 [訂用帳戶]。
1. 輸入 [資源群組] 的名稱。
1. 選取資源群組位置的 [區域]。

### <a name="learn-more"></a>深入了解

若要深入了解，請參閱：

- [Azure 基本概念](../considerations/fundamental-concepts.md)
- [建立您的初始訂用帳戶](../azure-best-practices/initial-subscriptions.md)
- [建立額外的 Azure 訂用帳戶以調整您的 Azure 環境](../azure-best-practices/scale-subscriptions.md)
- [組織和管理您的 Azure 訂用帳戶](../azure-best-practices/organize-subscriptions.md)
- [使用 Azure 管理群組來組織資源](/azure/governance/management-groups/overview)
- [了解 Azure 中的資源存取管理](../../govern/resource-consistency/resource-access-management.md)
- [訂用帳戶服務的限制](/azure/azure-resource-manager/management/azure-subscription-service-limits)

::: zone-end

::: zone target="chromeless"

### <a name="actions"></a>動作

**建立管理群組：**

建立管理群組，以協助您針對多個訂用帳戶管理存取、原則及合規性。

1. 請移至 **管理群組**。
1. 選取 [新增管理群組]。

::: form action="OpenBlade[#blade/Microsoft_Azure_ManagementGroups/HierarchyBlade]" submitText="Go to Management groups" :::

**建立其他訂用帳戶：**

使用訂用帳戶來管理成本以及由使用者、小組或專案所建立的資源。

1. 請移至 [**訂用帳戶**]。
1. 選取 [新增]。

::: form action="OpenBlade[#blade/Microsoft_Azure_Billing/SubscriptionsBlade]" submitText="Go to Subscriptions" :::

**建立資源群組：**

建立資源群組，以容納共用相同生命週期、權限及原則的 Web 應用程式、資料庫和儲存體帳戶等資源。

1. 移至 [**資源群組**]。
1. 選取 [新增]。
1. 選取要用來建立資源群組的 [訂用帳戶]。
1. 輸入 [資源群組] 的名稱。
1. 選取資源群組位置的 [區域]。

::: form action="Create[#create/Microsoft.ResourceGroup]" submitText="Create a resource group" :::

::: zone-end

## <a name="naming-standards"></a>[命名標準](#tab/NamingStandards)

良好的命名標準可協助您識別 Azure 入口網站中、帳單陳述式上和自動化指令碼中的資源。 命名策略應該包含業務和營運詳細資料來作為資源名稱的元件：

在業務方面，此策略應該確保資源名稱包含識別小組所需的組織資訊。 您可以與負責資源成本的業務擁有者一起使用資源。

在營運方面，則應確保名稱包含 IT 小組所需的資訊。 請使用可識別工作負載、應用程式、環境和重要性的詳細資料，以及可用來管理資源的其他實用資訊。

不同的資源類型具有不同的[命名規則和限制](/azure/azure-resource-manager/management/resource-name-rules)。 如需詳細資訊和為了支援企業雲端採用工作所特別提出的建議，請參閱雲端採用架構的[命名和標記指引](../azure-best-practices/naming-and-tagging.md)。

下表包含一些 Azure 資源範例類型的命名模式。

::: zone target="docs"

>[!TIP]
> 避免使用任何特殊字元 (`-` 或 `_`) 作為任何名稱的第一個或最後一個字元。 這些字元會導致大部分驗證規則失敗。

::: zone-end

| 單位 | 影響範圍 | 長度 | 大小寫 | 有效字元 | 建議模式 | 範例 |
| --- | --- | --- | --- | --- | --- | --- |
| 資源群組 | 訂用帳戶 | 1-90 | 不區分大小寫 | 英數字元、底線、括號、連字號、句號 (結尾除外) 及 Unicode 字元 | `<service short name>-<environment>-rg` | `profx-prod-rg` |
| 可用性設定組 | 資源群組 | 1-80 | 不區分大小寫 | 英數字元、底線和連字號 | `<service-short-name>-<context>-as` | `profx-SQL-as` |
| Tag | 相關聯的實體 | 512 (名稱)、256 (值) | 不區分大小寫 | 英數字元 | `"Key" : "value"` | `"Department" : "Central IT"` |

## <a name="resource-tags"></a>[資源標記](#tab/ResourceTags)

標記可快速找出資源和資源群組。 您可將標籤套用至 Azure 資源，以便以邏輯方式依照類別組織這些資源。 每個標記都是由一個名稱和一個值所組成。 例如，您可以將「環境」名稱和「生產」值套用至生產環境中的所有資源。 標記應該包含資源相關的工作負載或應用程式、營運需求和擁有權資訊等內容。

套用標記之後，您可以擷取訂用帳戶中具有該標記名稱和值的所有資源。 當您針對計費或管理來組織資源時，標記可協助您從不同的資源群組擷取相關資源。

您也可以將標記用於其他許多項目。 常見使用案例包括：

- **中繼資料和文件：** 系統管理員套用 `ProjectOwner` 之類的標記，即可輕易查看其所處理資源的相關詳細資料。
- **自動化：** 您可能有定期執行的指令碼，其會根據 `ShutdownTime` 或 `DeprovisionDate` 等標記值來採取動作。
- **成本最佳化：** 您可以將資源配置給負責成本的小組和資源。 在 Azure 成本管理 + 計費中，您可以套用成本中心標記做為篩選準則，以根據小組或部門使用量來報告費用。

每個資源或資源群組最多都可以有 50 個標記名稱和值組。 此限制只適用於直接套用至資源群組或資源的標記。

如需更多的標記建議和範例，請參閱雲端採用架構中的[建議的命名和標記慣例](../azure-best-practices/naming-and-tagging.md)。

::: zone target="docs"

### <a name="apply-a-resource-tag"></a>套用資源標記

若要對資源群組套用標記：

1. 移至 [[資源群組](https://portal.azure.com/#blade/HubsExtension/BrowseResourceGroups)]。
1. 選取資源群組。
1. 選取 [指派標記]。
1. 輸入新的名稱和值，或使用下拉式清單來選取現有的名稱和值。

### <a name="learn-more"></a>深入了解

若要深入了解，請參閱[使用標記來組織 Azure 資源](/azure/azure-resource-manager/management/tag-resources)。

::: zone-end

::: zone target="chromeless"

### <a name="action"></a>動作

**套用資源標記：**

若要對資源群組套用標記：

1. 移至 [**資源群組**]。
1. 選取資源群組。
1. 選取 [標記]。
1. 輸入新的名稱和值，或選取現有的名稱和值。

::: form action="OpenBlade[#blade/HubsExtension/Resources/resourceType/Microsoft.Resources%2FSubscriptions%2FResourceGroups]" submitText="Go to resource groups" :::

::: zone-end
