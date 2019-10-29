---
title: Azure 中的增強管理基準
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 管理基準的常見改善
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: fasttrack-edit, AQC
ms.localizationpriority: high
ms.openlocfilehash: 85e289867ac69f3403d964078a7c3f3b2a6c96a7
ms.sourcegitcommit: f3371811a36e12533ecbc3aa936e2a68e0cee25f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/21/2019
ms.locfileid: "72683689"
---
# <a name="enhanced-management-baseline-in-azure"></a>Azure 中的增強管理基準

前三個雲端管理專業領域描述的是管理基準。 本指南中的前述文章概述了雲端管理服務的最簡可行產品 (MVP)，簡稱管理基準。 本文會概述管理基準的一些常見改善。

管理基準的目的是要建立一致的供應項目，以便為**所有*** 受支援的工作負載提供最低程度的商務承諾。 這個常見的可重複管理供應項目基準可讓小組以最小的偏差來提供高度最佳化的作業管理機制。 不過，您可能需要比標準供應項目還要好的商務承諾。 下圖和下列項目符號會顯示讓您得以超越管理基準的三種方式。

![超越雲端管理基準](../../_images/manage/beyond-the-baseline.png)

- **工作負載作業：**
  - 最大一筆的每一工作負載作業投資。
  - 最高程度的復原能力。
  - 建議用於大約 20% 可帶來商業價值的工作負載。
  - 通常保留給高度重要性或任務關鍵性的工作負載。
- **平台作業：**
  - 作業投資會分散到許多工作負載。
  - 復原能力的改善會影響所有使用已定義平台的工作負載。
  - 建議用於最高重要性平台 +/- 20%。
  - 通常保留給中度重要性到高度重要性的工作負載。
- **增強的管理基準：**
  - 相對最低的作業投資。
  - 可稍微改善使用其他雲端原生作業工具和流程的商務承諾。

工作負載作業和平台作業都必須變更設計和架構方面的原則。 這些變更需要不少時間，而且可能會導致營運費用增加。 為了減少需要這類投資的工作負載數目，增強的管理基準可提供足夠的商務承諾改善。

下表概述客戶增強的管理基準中常見的一些流程、工具和潛在影響。

|專業領域  |Process  |工具  |潛在影響| 深入了解 |
|---------|---------|---------|---------|---------|
|清查和可見性|服務變更追蹤|Azure Resource Graph|若能更加深入地了解 Azure 服務的變更，可能會有助於更快地偵測到負面影響或更快速地補救|[Azure Resource Graph 的概觀](https://docs.microsoft.com/azure/governance/resource-graph/overview)|
|清查和可見性|IT 服務管理 (ITSM) 整合|IT 服務管理連接器|自動化的 ITSM 連線能更快產生認知|[Azure ITSM 連接器](https://docs.microsoft.com/azure/azure-monitor/platform/itsmc-overview)|
|作業合規性|作業自動化|Azure 自動化|將作業合規性自動化，以更快、更精確地回應變更|請參閱下方|
|作業合規性|多重雲端作業|Azure 自動化 Hybrid Runbook Worker|將跨越多個雲端的作業自動化|[混合式 Runbook 概觀](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker)|
|作業合規性|來賓自動化|期望狀態設定 (DSC)|透過程式碼來設定來賓作業系統，以減少錯誤和設定漂移|[DSC 概觀](/powershell/scripting/dsc/overview/overview)|
|保護和復原|安全性缺口通知|Azure 資訊安全中心|擴大保護範圍，納入安全性缺口復原觸發程序|請參閱下方|

::: zone target="docs"

## <a name="azure-automation"></a>Azure 自動化

::: zone-end
::: zone target="chromeless"

## <a name="azure-automationtabazureautomation"></a>[Azure 自動化](#tab/AzureAutomation)

::: zone-end

Azure 自動化會提供集中式系統來讓您管理自動化控制。 在 Azure 自動化中，您可以執行簡單的補救、調整和最佳化流程以回應環境計量，從而降低與手動處理事件相關聯的額外負荷。 最重要的是，自動化補救可以近乎即時地傳遞，大幅減少商業流程的中斷時間。 研究最常發生的業務中斷可找出環境內可加以自動化的活動。

### <a name="runbooks"></a>Runbook

Runbook 是用來提供自動化補救的基本程式碼單位。 Runbook 包含用於補救或從事件中復原的指示。

若要建立或管理 Runbook：

1. 移至 [Azure 自動化](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Automation%2FAutomationAccounts)。
2. 選擇所列出的其中一個**自動化帳戶**。
3. 尋找入口網站瀏覽的 [程式自動化]  區段。
4. 該區段中的選項可讓您建立或管理 Runbook、排程和其他自動化補救功能。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Automation%2FAutomationAccounts]" submitText="Assign Policy" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end
::: zone target="docs"

## <a name="azure-security-center"></a>Azure 資訊安全中心

::: zone-end
::: zone target="chromeless"

## <a name="azure-security-centertabazuresecuritycenter"></a>[Azure 資訊安全中心](#tab/AzureSecurityCenter)

::: zone-end

Azure 資訊安全中心也會在「保護和復原」策略中扮演重要角色。 它可協助您監視機器、網路、儲存體、資料服務和應用程式的安全性。 Azure 資訊安全中心可藉由使用機器學習和行為分析來協助您識別目標是 Azure 資源的作用中威脅，從而提供先進的威脅偵測。 其也提供威脅防護，可封鎖惡意程式碼或其他不必要的程式碼，並可減少暴露在暴力密碼破解攻擊和其他網路攻擊之下的介面區。

Azure 資訊安全中心在識別到威脅時，會觸發附有回應攻擊所需採取步驟的安全性警示。 它也會提供附有所偵測到威脅相關資訊的報告。

Azure 資訊安全中心提供兩個層級：免費層和標準層。 安全性建議等功能可於免費層取得。 標準層則會提供額外的保護，例如針對混合式雲端工作負載的進階威脅偵測和防護。

::: zone target="chromeless"

### <a name="action"></a>動作

**前 30 天可免費使用標準服務層級。**

為訂用帳戶資源啟用及設定安全性原則後，便可在 [防護]  區段中檢視資源的安全性狀態及任何問題。 您也可以在 [建議]  圖格上檢視這些問題的清單。

::: form action="OpenBlade[#blade/Microsoft_Azure_Security/SecurityMenuBlade/SecurityMenuBlade/0]" submitText="Explore Azure Security Center" :::

::: zone-end

::: zone target="docs"

若要探索 Azure 資訊安全中心，請移至 [Azure 入口網站](https://portal.azure.com/#blade/Microsoft_Azure_Security/SecurityMenuBlade/SecurityMenuBlade/0)。

### <a name="learn-more"></a>深入了解

若要深入了解，請參閱 [Azure 資訊安全中心文件](https://docs.microsoft.com/azure/security-center)。

::: zone-end
