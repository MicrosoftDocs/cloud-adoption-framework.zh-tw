---
title: 部署加速範例原則聲明
description: 使用適用于 Azure 的雲端採用架構，取得範例部署加速原則聲明，以協助您草擬原則聲明。
author: alexbuckgit
ms.author: abuck
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: internal
ms.openlocfilehash: bb19bfa0cf8d521b33e570288774128830e9515b
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97021452"
---
# <a name="deployment-acceleration-sample-policy-statements"></a>部署加速範例原則聲明

個別雲端原則聲明是解決在風險評估流程中識別出的特定風險方針。 這些聲明應該會提供風險的簡明摘要，以及如何處理這些風險的方案。 每個聲明定義都應該包含下列資訊：

- **技術風險：** 此原則將解決的風險摘要。
- **原則聲明：** 原則需求的清楚摘要說明。
- **設計選項：** 可操作的建議、規格或其他指引，可供 IT 小組和開發人員在執行原則時使用。

下列範例原則聲明會解決常見的設定相關商務風險。 這些語句是在草擬原則聲明以解決組織需求時可參考的範例。 這些範例並非禁止使用，而且可能會有數個原則選項可處理每個已識別的風險。 與商務和 IT 小組密切合作，為您的唯一風險組合找出最佳原則。

## <a name="reliance-on-manual-deployment-or-configuration-of-systems"></a>依賴手動部署或設定系統

**技術風險：** 在部署或設定期間依賴人為介入，會提高人為錯誤的可能性，並減少系統部署和設定的重複性和可預測性。 它通常也會導致系統資源的部署變慢。

**原則聲明：** 所有部署至雲端的資產都應盡可能使用範本或自動化腳本進行部署。

**可能的設計選項：** [Azure Resource Manager 範本](/azure/azure-resource-manager/templates/overview) 可讓您使用基礎結構即程式碼，將您的資源部署到 Azure。 您也可以使用 [Terraform](/azure/terraform/terraform-overview) 作為一致的內部部署和雲端式部署工具。

## <a name="lack-of-visibility-into-system-issues"></a>缺少系統問題的可見性

**技術風險：** 商務系統的監視和診斷不足，可防止作業人員在系統中斷之前找出並修復問題，而且可能會大幅增加適當解決中斷的所需時間。

**原則聲明：** 將會執行下列原則：

- 系統將針對所有生產系統和元件找出關鍵計量和診斷量值，而且會將監視與診斷工具套用至這些系統，並由操作人員定期監控。
- 作業會考慮使用非生產環境中的監視和診斷工具（例如預備和 QA）來識別系統問題，然後才在生產環境中進行。

**可能的設計選項：** [Azure 監視器](/azure/azure-monitor)（包括 Log Analytics 和 Application Insights）提供收集和分析遙測資料的工具，可協助您瞭解應用程式的執行情況，並主動識別影響它們的問題以及它們所依賴的資源。 此外， [Azure 活動記錄](/azure/azure-monitor/platform/activity-logs-overview) 會報告在平台層級進行的所有變更，並應針對不符合規範的變更進行監視和審核。

## <a name="configuration-security-reviews"></a>設定安全性檢閱

**技術風險：** 經過一段時間後，新的安全性威脅或考慮可能會增加未經授權存取安全資源的風險。

**原則聲明：** 雲端治理程式必須包含設定管理小組的每月審查，以找出雲端資產設定應該避免的惡意執行者或使用模式。

**可能的設計選項：** 建立每月安全性審核會議，其中包括治理小組成員，以及負責設定雲端應用程式和資源的 IT 人員。 請檢查現有的安全性資料和計量，以在目前的部署加速原則和工具之間建立差距，並更新原則來補救任何新風險。

## <a name="next-steps"></a>後續步驟

使用本文提及的範例作為起點，以開發與您雲端採用方案保持一致的原則來解決特定業務風險。

若要開始開發您自己的自訂身分識別基準原則聲明，請下載身分 [識別基準專業版範本](../identity-baseline/template.md)。

若要加速採用此專業領域，請選擇最符合您環境的可 [操作治理指南](../guides/index.md) 。 然後修改設計，以納入您特定的公司原則決策。

根據風險和承受度建立流程來治理和傳達部署加速原則的遵循情況。

> [!div class="nextstepaction"]
> [建立原則合規性流程](./compliance-processes.md)
