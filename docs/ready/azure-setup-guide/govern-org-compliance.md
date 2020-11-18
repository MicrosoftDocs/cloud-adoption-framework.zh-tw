---
title: Azure 中的控管、安全性和合規性
description: 使用適用於 Azure 的雲端採用架構，了解如何設定 Azure 環境的控管、安全性和合規性。
author: tvuylsteke
ms.author: kfollis
ms.date: 09/27/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: fasttrack-edit, AQC, setup
ms.localizationpriority: high
ms.openlocfilehash: ab3b3e0d4012113e2b132c8df1e0bfe89758252d
ms.sourcegitcommit: a7eb2f6c4465527cca2d479edbfc9d93d1e44bf1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94711689"
---
# <a name="governance-security-and-compliance-in-azure"></a>Azure 中的控管、安全性和合規性

在建立公司原則並規劃治理策略時，您可以使用工具和服務 (例如 Azure 原則、Azure 藍圖和 Azure 資訊安全中心) 來強制執行組織的治理決策並使其自動化。 開始治理規劃之前，請使用[治理基準測試工具](https://cafbaseline.com)來找出貴組織的雲端治理方法中可能出現的差距。 如需有關開發治理流程的詳細資訊，請參閱[管理方法](../../govern/index.md)。

## <a name="azure-blueprints"></a>[Azure 藍圖](#tab/AzureBlueprints)

Azure 藍圖可讓雲端架構設計師和中央資訊技術人員定義一組可重複使用的 Azure 資源，其中實作並遵循組織的標準、模式和需求。 Azure 藍圖可讓開發小組在知道他們是以符合組織合規性進行建置的情況下，快速地建置及建立新環境，並使用內建元件 (像是網路) 加速開發和交貨。

藍圖是以宣告方式來協調部署多種資源範本與其他成品，例如：

- 角色指派。
- 原則指派。
- Azure Resource Manager 範本。
- 資源群組。

### <a name="create-a-blueprint"></a>建立藍圖

若要建立藍圖：

::: zone target="chromeless"

1. 移至 **[藍圖：開始使用]** 。
1. 在 [建立藍圖] 區段中，選取 [建立]。
1. 篩選藍圖清單以選取適當的藍圖。
1. 輸入 [藍圖名稱]，然後選取適當的 [定義位置]。
1. 選取 下一步：**成品 >>** ，然後檢閱藍圖中包含的成品。
1. 選取 **[儲存草稿]** 。

::: form action="OpenBlade[#blade/Microsoft_Azure_Policy/BlueprintsMenuBlade/GetStarted]" submitText="Create a blueprint" :::

::: zone-end

::: zone target="docs"

1. 在 Azure 入口網站中，移至 [藍圖：開始使用](https://portal.azure.com/#blade/Microsoft_Azure_Policy/BlueprintsMenuBlade/GetStarted)。
1. 在 [建立藍圖] 區段中，選取 [建立]。
1. 篩選藍圖清單以選取適當的藍圖。
1. 輸入 [藍圖名稱]，然後選取適當的 [定義位置]。
1. 選取 下一步：**成品 >>** ，然後檢閱藍圖中包含的成品。
1. 選取 **[儲存草稿]** 。

::: zone-end

### <a name="publish-a-blueprint"></a>發佈藍圖

若要將藍圖成品發佈至您的訂用帳戶：

::: zone target="chromeless"

1. 移至 **藍圖：藍圖定義**。
1. 選取您在先前步驟中建立的藍圖。
1. 檢閱藍圖定義，然後選取 [發佈藍圖]。
1. 提供 **版本** (例如 _1.0_) 和任何 **變更附註**，然後選取 [發佈]。

::: form action="OpenBlade[#blade/Microsoft_Azure_Policy/BlueprintsMenuBlade/Blueprints]" submitText="Blueprint definitions" :::

::: zone-end

::: zone target="docs"

1. 在 Azure 入口網站中，移至 [藍圖：藍圖定義](https://portal.azure.com/#blade/Microsoft_Azure_Policy/BlueprintsMenuBlade/Blueprints)。
1. 選取您在先前步驟中建立的藍圖定義。
1. 檢閱藍圖定義，然後選取 [發佈藍圖]。
1. 提供 **版本** (例如 _1.0_) 和任何 **變更附註**，然後選取 [發佈]。

::: zone-end

::: zone target="docs"

### <a name="learn-more"></a>深入了解

若要深入了解，請參閱：

- [Azure 藍圖](/azure/governance/blueprints)
- [雲端採用架構：資源一致性決策指南](../../decision-guides/resource-consistency/index.md)
- [標準型藍圖範例](/azure/governance/blueprints/samples/index#standards-based-blueprint-samples)

::: zone-end

## <a name="azure-policy"></a>[Azure 原則](#tab/AzurePolicy)

Azure 原則是一項服務，您可用來建立、指派和管理原則。 這些原則會對您的資源強制執行規則，讓這些資源能符合公司標準和服務等級協定的規範。 Azure 原則會掃描您的資源，以識別不符合所實作原則的資源。 例如，您可以讓原則只允許環境中執行特定虛擬機器 (VM) 大小。 當您實作此原則時，其會評估環境中現有的 VM 和任何新部署的 VM。 原則評估作業會產生合規性事件供您用於監視和報告。

考慮一般原則，以便：

- 對資源和資源群組強制執行標記。
- 針對已部署的資源限制區域。
- 針對特定資源限制高成本的 SKU。
- 稽核重要選擇性功能的使用情形，例如 Azure 受控磁碟。

::: zone target="chromeless"

### <a name="action"></a>動作

將內建原則指派給管理群組、訂用帳戶或資源群組。

::: form action="OpenBlade[#blade/Microsoft_Azure_Policy/PolicyMenuBlade/GettingStarted]" submitText="Assign Policy" :::

::: zone-end

::: zone target="docs"

### <a name="apply-a-policy"></a>套用原則

若要將原則套用至資源群組：

1. 請移至 [Azure 原則](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyMenuBlade/GettingStarted)。
1. 選取 [指派原則]。

### <a name="learn-more"></a>深入了解

若要深入了解，請參閱：

- [Azure 原則](/azure/governance/policy)
- [雲端採用架構：原則強制執行決策指南](../../decision-guides/policy-enforcement/index.md)

::: zone-end

## <a name="azure-security-center"></a>[Azure 資訊安全中心](#tab/AzureSecurityCenter)

Azure 資訊安全中心在控管策略中扮演重要角色。 它可協助您掌控安全性，因為它會：

- 為您的工作負載提供統一的安全性檢視。
- 從各種來源 (包括防火牆和其他合作夥伴解決方案) 收集、搜尋及分析安全性資料。
- 提供可採取行動的安全性建議，以在遭到惡意探索前修正問題。
- 用於對混和式雲端工作負載套用安全性原則，以確保符合安全性標準。

許多安全性功能 (例如，安全性原則和建議) 均可免費使用。 部分更進階的功能 (像是 Just-In-Time VM 存取和混合式工作負載支援) 則是在資訊安全中心的標準層中提供使用。 Just-In-Time VM 存取可協助您控制針對 Azure VM 上管理連接埠的存取，以減少網路的受攻擊面。

> [!TIP]
> 根據預設，Azure 資訊安全中心會在每個訂用帳戶中啟用。 但建議您啟用從虛擬機器收集資料的功能，以允許 Azure 資訊安全中心安裝其代理程式並開始蒐集資料。

::: zone target="docs"

若要探索 Azure 資訊安全中心，請移至 [Azure 入口網站](https://portal.azure.com/#blade/Microsoft_Azure_Security/SecurityMenuBlade/SecurityMenuBlade/0)。

### <a name="learn-more"></a>深入了解

若要深入了解，請參閱：

- [Azure 資訊安全中心](/azure/security-center)
- [Just-In-Time 虛擬機器存取](/azure/security-center/security-center-just-in-time#how-does-just-in-time-access-work)
- [資訊安全中心定價層](https://azure.microsoft.com/pricing/details/security-center)
- [雲端採用架構：安全性基準專業領域](../../govern/security-baseline/index.md)

::: zone-end

::: zone target="chromeless"

### <a name="action"></a>動作

::: form action="OpenBlade[#blade/Microsoft_Azure_Security/SecurityMenuBlade/SecurityMenuBlade/0]" submitText="Explore Azure Security Center" :::

::: zone-end
