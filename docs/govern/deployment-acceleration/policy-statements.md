---
title: 部署加速範例原則聲明
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 部署加速範例原則聲明
author: alexbuckgit
ms.author: abuck
ms.date: 09/17/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: a307c29a640332fdf82a69ec06eab27589f77304
ms.sourcegitcommit: bf9be7f2fe4851d83cdf3e083c7c25bd7e144c20
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/04/2019
ms.locfileid: "73566358"
---
# <a name="deployment-acceleration-sample-policy-statements"></a>部署加速範例原則聲明

每個雲端原則聲明都是一個指導方針，用來解決在風險評估流程中識別到的特定風險。 這些聲明應該會提供風險的簡明摘要，以及如何處理這些風險的方案。 每個聲明定義都應該包含下列資訊：

- **技術風險：** 此原則將解決的風險摘要。
- **原則聲明：** 原則需求的清楚摘要說明。
- **設計選項：** IT 小組與開發人員可以在執行原則時使用的可操作建議、規格或其他指引。

下列範例原則聲明會解決與一般設定相關的商務風險。 這些語句是您在草擬原則聲明以滿足組織需求時可以參考的範例。 這些範例並非禁止使用，而且可能會有數個原則選項來處理每個已識別的風險。 與企業和 IT 小組密切合作，找出您獨特風險的最佳原則。

## <a name="reliance-on-manual-deployment-or-configuration-of-systems"></a>依賴手動部署或設定系統

**技術風險：** 在部署或設定期間依賴人為介入，會增加人為錯誤的可能性，並減少系統部署和設定的可重複性和可預測性。 它通常也會導致系統資源的部署變慢。

**原則聲明：** 所有部署至雲端的資產都應該盡可能使用範本或自動化腳本進行部署。

**潛在的設計選項：** [Azure Resource Manager 範本](https://docs.microsoft.com/azure/azure-resource-manager/template-deployment-overview)提供了基礎結構即程式碼的方法，可將您的資源部署至 Azure。 您也可以使用[Terraform](https://docs.microsoft.com/azure/terraform/terraform-overview)作為一致的內部部署和雲端式部署工具。

## <a name="lack-of-visibility-into-system-issues"></a>缺少系統問題的可見性

**技術風險：** 商務系統的監視和診斷不足，讓作業人員無法在發生系統中斷之前識別及修復問題，而且可能會大幅增加正確解決中斷所需的時間。

**原則聲明：** 將會執行下列原則：

- 系統將針對所有生產系統和元件找出關鍵計量和診斷量值，而且會將監視與診斷工具套用至這些系統，並由操作人員定期監控。
- 作業會考慮使用非生產環境（例如預備和 QA）中的監視和診斷工具，在生產環境中發生系統問題之前加以識別。

**潛在的設計選項：** [Azure 監視器](https://docs.microsoft.com/azure/azure-monitor)（也包括 Log Analytics 和 Application Insights）提供收集和分析遙測的工具，以協助您瞭解應用程式的執行和主動識別影響它們的問題以及它們所依賴的資源。 此外， [Azure 活動記錄](https://docs.microsoft.com/azure/azure-monitor/platform/activity-logs-overview)會報告在平台層級進行的所有變更，並且應該針對不相容的變更進行監視/審核。

## <a name="configuration-security-reviews"></a>設定安全性檢閱

**技術風險：** 經過一段時間後，新的安全性威脅或疑慮可能會增加未經授權存取安全資源的風險。

**原則聲明：** 雲端治理程式必須包含每月審查與設定管理小組，以找出雲端資產設定應該避免的惡意執行者或使用模式。

**潛在的設計選項：** 建立每月的安全性審查會議，其中包括治理小組成員，以及負責設定雲端應用程式和資源的 IT 人員。 請參閱現有的安全性資料和計量，以在目前的部署加速原則和工具中建立間距，並更新原則以補救任何新的風險。

## <a name="next-steps"></a>後續步驟

使用本文提及的範例作為起點，以開發與您雲端採用方案保持一致的原則來解決特定業務風險。

若要開始自行開發與身分識別管理相關的自訂原則聲明，請下載[身分識別基準範本](../identity-baseline/template.md)。

若要加速採用這個專業領域，請選擇最符合您環境的可採取動作的[治理指南](../guides/index.md)。 然後修改設計，以納入您特定的公司原則決策。

根據風險和承受度建立流程來治理和傳達部署加速原則的遵循情況。

> [!div class="nextstepaction"]
> [建立原則合規性流程](./compliance-processes.md)
