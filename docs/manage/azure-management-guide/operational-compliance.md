---
title: Azure 中的作業合規性
description: 了解如何透過作業合規性來降低中斷或遭受攻擊的可能性，進而確保商務穩定性。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: fasttrack-edit, AQC
ms.localizationpriority: high
ms.openlocfilehash: b747a6f0d50fbf2510dc3a5220f72d513e8d1cb3
ms.sourcegitcommit: 60d8b863d431b5d7c005f2f14488620b6c4c49be
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83216702"
---
<!-- cSpell:ignore WSUS getting started -->

# <a name="operational-compliance-in-azure"></a>Azure 中的作業合規性

_作業合規性_是任何雲端管理基準中的第二個專業領域。

![雲端管理基準](../../_images/manage/management-baseline.png)

改善作業合規性可減少發生與設定漂移有關的中斷，或與系統未正確修補有關的弱點。

針對任何企業級環境，下表會概述管理基準的最低建議。

| Process | 工具 | 目的 |
|---|---|---|
| 修補程式管理 | 更新管理 | 更新的管理和排程 |
| 強制執行原則 | Azure 原則 | 強制執行原則以確保環境和來賓的合規性 |
| 環境設定 | Azure 藍圖 | 核心服務的自動化合規性 |
| 資源組態 | Desired State Configuration | 在客體 OS 和環境的某些層面進行自動化設定 |

::: zone target="docs"

## <a name="update-management"></a>更新管理

::: zone-end
::: zone target="chromeless"

## <a name="update-management"></a>[更新管理](#tab/UpdateManagement)

::: zone-end

「更新管理」所管理的電腦會使用下列設定來執行評估和更新部署：

- 適用於 Windows 或 Linux 的 Microsoft Monitoring Agent (MMA)。
- 適用於 Linux 的 PowerShell Desired State Configuration (DSC)。
- Azure 自動化 Hybrid Runbook Worker。
- 適用於 Windows 電腦的 Microsoft Update 或 Windows Server Update Services (WSUS)。

如需詳細資訊，請參閱[更新管理解決方案](https://docs.microsoft.com/azure/automation/automation-update-management)。

> [!WARNING]
> 在使用更新管理之前，您必須先將虛擬機器或整個訂用帳戶上線到 Log Analytics 和 Azure 自動化。
>
> 有兩種方式可以上線：
>
> - [單一 VM](../../manage/azure-server-management/onboard-single-vm.md)
> - [整個訂用帳戶](../../manage/azure-server-management/onboard-at-scale.md)
>
> 您應該先遵循其中一個方式，再繼續進行更新管理。

### <a name="manage-updates"></a>管理更新

若要將原則套用至資源群組：

1. 移至 [Azure 自動化](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Automation%2FAutomationAccounts)。
1. 選取**自動化帳戶**，並選擇其中一個列出的帳戶。
1. 移至 [設定管理]  。
1. **清查**、**變更管理**和**狀態設定**都可用來控制受控 VM 的狀態和作業合規性。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Automation%2FAutomationAccounts]" submitText="Assign Policy" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

::: zone target="docs"

## <a name="azure-policy"></a>Azure 原則

::: zone-end
::: zone target="chromeless"

## <a name="azure-policy"></a>[Azure 原則](#tab/AzurePolicy)

::: zone-end

Azure 原則會用於整個治理流程。 在雲端管理流程中也一樣擁有極高價值。 Azure 原則除了能稽核和修復 Azure 資源外，還可以稽核機器內的設定。 此驗證會由「來賓設定」延伸模組和用戶端執行。 透過用戶端的延伸模組會驗證下列設定：

- 作業系統設定。
- 應用程式設定或目前狀態。
- 環境設定。

Azure 原則來賓設定目前只會稽核機器內的設定。 其不會套用設定。

::: zone target="chromeless"

### <a name="action"></a>動作

將內建原則指派給管理群組、訂用帳戶或資源群組。

::: form action="OpenBlade[#blade/Microsoft_Azure_Policy/PolicyMenuBlade/GettingStarted]" submitText="Assign Policy" :::

::: zone-end

::: zone target="docs"

### <a name="apply-a-policy"></a>套用原則

若要將原則套用至資源群組：

1. 請移至 [Azure 原則](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyMenuBlade/GettingStarted)。
1. 選取 [指派原則]  。

### <a name="learn-more"></a>深入了解

若要深入了解，請參閱：

- [Azure 原則](https://docs.microsoft.com/azure/azure-policy)
- [Azure 原則 - 來賓設定](https://docs.microsoft.com/azure/governance/policy/concepts/guest-configuration)
- [雲端採用架構：原則強制執行決策指南](../../decision-guides/policy-enforcement/index.md)

## <a name="azure-blueprints"></a>Azure 藍圖

::: zone-end
::: zone target="chromeless"

## <a name="azure-blueprints"></a>[Azure 藍圖](#tab/AzureBlueprints)

::: zone-end

有了 Azure 藍圖，雲端架構設計人員和中央 IT 群組就可以定義一組可重複使用的 Azure 資源。 這些資源會實作組織的標準、模式和需求，並加以遵循。

有了 Azure 藍圖，開發小組就可以快速建置並啟動新的環境。 小組也可以確信他們的建置符合組織合規性。 他們會使用一組內建元件 (例如網路功能) 來加速開發和傳遞。

藍圖是以宣告方式來協調部署多種資源範本與其他成品，例如：

- 角色指派。
- 原則指派。
- Azure Resource Manager 範本。
- 資源群組。

套用藍圖可在環境中強制執行作業合規性 (如果雲端治理小組尚未執行此強制動作的話)。

### <a name="create-a-blueprint"></a>建立藍圖

若要建立藍圖：

::: zone target="chromeless"

1. 前往**藍圖 - 使用者入門**。
1. 在 [建立藍圖]  窗格中，選取 [建立]  。
1. 篩選藍圖清單以選取適當的藍圖。
1. 在 [藍圖名稱]  方塊中，輸入藍圖名稱。
1. 選取 [定義位置]  ，然後選擇適當的位置。
1. 選取 [下一步：  成品 >>]，然後檢閱藍圖中包含的成品。
1. 選取 [儲存草稿]  。

::: form action="OpenBlade[#blade/Microsoft_Azure_Policy/BlueprintsMenuBlade/GetStarted]" submitText="Create a blueprint" :::

::: zone-end

::: zone target="docs"

1. 前往[藍圖 - 使用者入門](https://portal.azure.com/#blade/Microsoft_Azure_Policy/BlueprintsMenuBlade/GetStarted)。
1. 在 [建立藍圖]  窗格中，選取 [建立]  。
1. 篩選藍圖清單以選取適當的藍圖。
1. 在 [藍圖名稱]  方塊中，輸入藍圖名稱。
1. 選取 [定義位置]  ，然後選擇適當的位置。
1. 選取 [下一步：  成品 >>]，然後檢閱藍圖中包含的成品。
1. 選取 [儲存草稿]  。

::: zone-end

### <a name="publish-a-blueprint"></a>發佈藍圖

若要將藍圖成品發佈至您的訂用帳戶：

::: zone target="chromeless"

1. 移至**藍圖 - 藍圖定義**。
1. 選取您在先前步驟中建立的藍圖。
1. 檢閱藍圖定義，然後選取 [發佈藍圖]  。
1. 在 [版本]  方塊中輸入版本，例如 "1.0"。
1. 在 [變更附註]  方塊中，輸入您的附註。
1. 選取 [發佈]  。

::: form action="OpenBlade[#blade/Microsoft_Azure_Policy/BlueprintsMenuBlade/Blueprints]" submitText="Blueprint definitions" :::

::: zone-end

::: zone target="docs"

1. 移至[藍圖 - 藍圖定義](https://portal.azure.com/#blade/Microsoft_Azure_Policy/BlueprintsMenuBlade/Blueprints)。
1. 選取您在先前步驟中建立的藍圖。
1. 檢閱藍圖定義，然後選取 [發佈藍圖]  。
1. 在 [版本]  方塊中輸入版本，例如 "1.0"。
1. 在 [變更附註]  方塊中，輸入您的附註。
1. 選取 [發佈]  。

<!-- markdownlint-disable MD024 -->

### <a name="learn-more"></a>深入了解

若要深入了解，請參閱：

- [Azure 藍圖](https://docs.microsoft.com/azure/governance/blueprints)
- [雲端採用架構：資源一致性決策指南](../../decision-guides/resource-consistency/index.md)
- [標準型藍圖範例](https://docs.microsoft.com/azure/governance/blueprints/samples/index#standards-based-blueprint-samples)

::: zone-end
