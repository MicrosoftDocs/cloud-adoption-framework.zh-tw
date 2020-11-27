---
title: Azure 中的增強管理基準
description: 使用「適用於 Azure 的雲端採用架構」來了解管理基準的常見改善項目。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: fasttrack-edit, AQC
ms.localizationpriority: high
ms.openlocfilehash: fa57d16b874c32fda3fc874e82d32c0e1a42c708
ms.sourcegitcommit: 412b945b3492ff3667c74627524dad354f3a9b85
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/18/2020
ms.locfileid: "94880886"
---
# <a name="enhanced-management-baseline-in-azure"></a>Azure 中的增強管理基準

前三個雲端管理專業領域描述的是管理基準。 本指南中的前述文章概述了雲端管理服務的最簡可行產品 (MVP)，亦稱為管理基準。 本文會概述管理基準的一些常見改善。

管理基準的目的是要建立一致的供應項目，以便為「所有」受支援的工作負載提供最低程度的商務承諾。 透過這個常見的可重複管理供應項目基準，小組可以最小的偏差來提供高度最佳化的作業管理機制。

不過，您可能需要比標準供應項目還要好的商務承諾。 下圖和下列清單會顯示讓您得以超越管理基準的三種方式。

![超越雲端管理基準](../../_images/manage/beyond-the-baseline.png)

- **增強的管理基準：**
  - 在組合中大部分的工作負載具有共同需求時，將增強功能新增至管理基準。
  - 改善使用其他雲端原生作業工具和流程的商務承諾。
  - 基準增強功能對特定工作負載的架構不應造成影響。
- **工作負載作業：**
  - 最大一筆的每一工作負載作業投資。
  - 最高程度的復原能力。
  - 建議用於大約 20% 可帶來商業價值的工作負載。
  - 通常保留給高度重要性或任務關鍵性的工作負載。
- **平台作業：**
  - 作業投資會分散到許多工作負載。
  - 復原能力的改善會影響所有使用已定義平台的工作負載。
  - 建議以 20% 的最重要平台為目標。
  - 通常保留給中度重要性到高度重要性的工作負載。

工作負載作業和平台作業都必須變更設計和架構方面的原則。 這些變更需要不少時間，而且可能會導致營運費用增加。 為了減少需要這類投資的工作負載數目，增強的管理基準可提供足夠的商務承諾改善。

<!-- docutune:casing "IT Service Management" "IT Service Management Connector" ITSMC "Free and Standard" -->

此資料表概述客戶增強的管理基準中有哪些流程、工具和潛在影響：

| 專業領域  | Process  | 工具 | 潛在影響 | 深入了解 |
|---|---|---|---|---|
| 清查和可見性 | 服務變更追蹤 | Azure Resource Graph | 若能更加深入地了解 Azure 服務的變更，可能會有助於更快地偵測到負面影響或更快速地補救 | [Azure Resource Graph 的概觀](/azure/governance/resource-graph/overview) |
| 清查和可見性 | IT 服務管理 (ITSM) 整合 | IT 服務管理連接器 | 自動化的 ITSM 連線能更快產生認知。 | [IT 服務管理連接器 (ITSMC)](/azure/azure-monitor/platform/itsmc-overview) |
| 作業合規性 | 作業自動化 | Azure 自動化 | 將作業合規性自動化，以更快、更精確地回應變更。 | 請參閱下列各節 |
| 作業合規性 | 效能自動化 | Azure 自動化 | 透過效能預期自動執行作業合規性，以解決資源特有的調整或大小調整常見問題。 | 請參閱下列各節 |
| 作業合規性 | 多重雲端作業 | Azure 自動化 Hybrid Runbook Worker | 將跨越多個雲端的作業自動化。 | [混合式 Runbook 背景工作概觀](/azure/automation/automation-hybrid-runbook-worker) |
| 作業合規性 | 來賓自動化 | 期望狀態設定 (DSC) | 透過程式碼來設定客體作業系統，以減少錯誤和設定漂移。 | [DSC 概觀](/powershell/scripting/dsc/overview/overview) |
| 保護和復原 | 安全性缺口通知 | Azure 資訊安全中心 | 擴大保護範圍，納入安全性缺口復原觸發程序。 | 請參閱下列各節 |

::: zone target="docs"

## <a name="azure-automation"></a>Azure 自動化

::: zone-end
::: zone target="chromeless"

## <a name="azure-automation"></a>[Azure 自動化](#tab/AzureAutomation)

::: zone-end

Azure 自動化會提供集中式系統來讓您管理自動化控制。 在 Azure 自動化中，您可以根據環境計量執行簡單的補救、調整和最佳化程序。 這些程序會降低與手動事件處理相關聯的額外負荷。

最重要的是，自動化補救可以近乎即時地傳遞，大幅減少商業流程的中斷時間。 研究最常發生的業務中斷可找出環境內可加以自動化的活動。

### <a name="runbooks"></a>Runbook

Runbook 是用來提供自動化補救的基本程式碼單位。 Runbook 包含用於補救或從事件中復原的指示。

若要建立或管理 Runbook：

1. 移至 **Azure 自動化**。
1. 選取 **自動化帳戶**，並選擇其中一個列出的帳戶。
1. 移至 [程序自動化]。
1. 有了這個選項，您就可以建立或管理 Runbook、排程和其他自動化補救功能。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Automation%2FAutomationAccounts]" submitText="Go to Azure Automation" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end
::: zone target="docs"

## <a name="azure-security-center"></a>Azure 資訊安全中心

::: zone-end
::: zone target="chromeless"

## <a name="azure-security-center"></a>[Azure 資訊安全中心](#tab/AzureSecurityCenter)

::: zone-end

Azure 資訊安全中心也會在「保護和復原」策略中扮演重要角色。 它可協助您監視機器、網路、儲存體、資料服務和應用程式的安全性。

Azure 資訊安全中心可藉由使用機器學習和行為分析來協助您識別目標是 Azure 資源的作用中威脅，從而提供先進的威脅偵測。 其也提供威脅防護，可封鎖惡意程式碼和其他不必要的程式碼，並可減少暴露在暴力密碼破解攻擊和其他網路攻擊之下的介面區。

Azure 資訊安全中心會在識別到威脅時觸發安全性警示，並提供回應攻擊所需的步驟。 也會提供所偵測到威脅的相關資訊報告。

Azure 資訊安全中心提供兩個層級：免費和標準。 安全性建議等功能可於免費層取得。 標準層則會提供額外的保護，例如針對混合式雲端工作負載的進階威脅偵測和防護。

::: zone target="chromeless"

### <a name="action"></a>動作

#### <a name="try-standard-tier-for-free-for-your-first-30-days"></a>前 30 天可免費使用標準服務層級

為訂用帳戶資源啟用及設定安全性原則後，便可在 [防護] 窗格中檢視資源的安全性狀態及任何問題。 您也可以在 [建議] 圖格上檢視這些問題的清單。

::: form action="OpenBlade[#blade/Microsoft_Azure_Security/SecurityMenuBlade/SecurityMenuBlade/0]" submitText="Explore Azure Security Center" :::

::: zone-end

::: zone target="docs"

若要探索 Azure 資訊安全中心，請移至 [Azure 入口網站](https://portal.azure.com/#blade/Microsoft_Azure_Security/SecurityMenuBlade/SecurityMenuBlade/0)。

### <a name="learn-more"></a>深入了解

若要深入了解，請參閱 [Azure 資訊安全中心文件](/azure/security-center)。

::: zone-end
