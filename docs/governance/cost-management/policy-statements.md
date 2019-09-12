---
title: 成本管理範例原則聲明
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 成本管理範例原則聲明
author: BrianBlanchard
ms.author: brblanch
ms.date: 02/11/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: fc8a691b446b5c40534e7189cb9d381aabd5f402
ms.sourcegitcommit: a26c27ed72ac89198231ec4b11917a20d03bd222
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70828145"
---
# <a name="cost-management-sample-policy-statements"></a>成本管理範例原則聲明

個別的雲端原則聲明是用來解決在風險評估流程中識別出之特定風險的方針。 這些聲明應該會提供風險的簡明摘要，以及如何處理這些風險的方案。 每個聲明定義都應該包含下列資訊：

- **商務風險。** 此原則將解決的風險摘要。
- **原則聲明。** 明確地摘要說明原則需求。
- **設計選項。** IT 小組與開發人員可以在實作原則時使用之可操作的建議、規格或其他指引。

下列範例原則聲明會解決與一般成本相關的商務風險。 這些語句是您在草擬原則聲明以滿足組織需求時可以參考的範例。 這些範例並非禁止使用，而且可能會有數個原則選項來處理每個已識別的風險。 與企業和 IT 小組密切合作，找出您獨特風險的最佳原則。

## <a name="future-proofing"></a>與未來技術的兼容性

**業務風險：** 目前不保證治理小組會投資成本管理專業領域的準則。 不過，您可以預期未來將有這類投資。

**原則聲明：** 您應該將所有部署至雲端的資產與帳單單位和應用程式/工作負載產生關聯。 此原則將確保未來的成本管理成果將會生效。

**設計選項：** 如需有關建立未來證明基礎的詳細資訊，請參閱與在雲端採用架構指引中包含的可[操作設計指南](../journeys/index.md)中建立治理 MVP 相關的討論。

## <a name="budget-overruns"></a>預算超支

**業務風險：** 自助服務部署會造成超支風險。

**原則聲明：** 所有雲端部署都必須配置給已核准預算的計費單位，以及適用於預算限制的機制。

**設計選項：** 在 Azure 中，可以使用 [Azure 成本管理](/azure/cost-management/manage-budgets)來控制預算。

## <a name="underutilization"></a>使用量過低

**業務風險：** 公司已針對雲端服務進行預付，或已承諾每年會花費特定金額。 風險是不會使用已同意的金額，因而導致投資遺失。

**原則聲明：** 每個具有已配置雲端預算的計費單位每年都將召開會議來設定預算、每季都會調整預算，而且每月都會配置時間來檢閱已規劃與實際的支出。 每月都要與計費單位的負責人討論任何大於 20% 的偏差。 基於追蹤目的，請將所有資產都指派給一個計費單位。

**設計選項：**

- 在 Azure 中，已規劃與實際的支出均可透過 [Azure 成本管理](/azure/cost-management/quick-acm-cost-analysis)來管理。
- 有數個選項可讓計費單位用來群組資源。 在 Azure 中，應該與治理小組一同選擇[資源一致性模型](../../decision-guides/resource-consistency/index.md)並套用至所有資產。

## <a name="overprovisioned-assets"></a>過度佈建的資產

**業務風險：** 在傳統內部部署資料中心，常見的做法是部署已額外規劃容量的資產，以便在長遠的未來中隨之增長。 相較於傳統設備，雲端可以更快速地進行調整。 雲端中的資產也會根據技術容量來計費。 對於舊的內部部署做法，會有以人為方式誇大雲端支出的風險。

**原則聲明：** 所有部署至雲端的資產都必須在計劃中註冊，此計劃可以監視使用量，並報告任何超過使用量 50% 的容量。 所有部署至雲端的資產都必須以邏輯方式加以群組或標記，如此一來，治理小組成員就能讓工作負載擁有者參與關於過度佈建資產的任何最佳化。

**設計選項：**

- 在 Azure 中，[Azure Advisor](/azure/advisor/advisor-cost-recommendations) 可以提供最佳化建議。
- 有數個選項可讓計費單位用來群組資源。 在 Azure 中，應該與治理小組一同選擇[資源一致性模型](../../decision-guides/resource-consistency/index.md)並套用至所有資產。

## <a name="overoptimization"></a>過度最佳化

**業務風險：** 有效的成本管理會產生新的風險。 將支出最佳化，正好與系統效能相反。 降低成本時，會造成過度削減支出，而產生不良使用者體驗的風險。

**原則聲明：** 所有直接影響客戶體驗的資產都必須透過群組或標記來識別。 在將任何影響客戶體驗的資產優化之前，雲端治理小組必須根據至少90天的使用量趨勢來調整優化。 記載在將資產最佳化時所考慮的任何季節性或事件驅動的高載。

**設計選項：**

- 在 Azure 中，[Azure 監視器的深入解析功能](/azure/azure-monitor/insights/vminsights-performance)有助於分析系統使用量。
- 有數個選項可根據角色來群組和標記資源。 在 Azure 中，您應該與治理小組一同選擇[資源一致性模型](../../decision-guides/resource-consistency/index.md)，並將此項套用至所有資產。

## <a name="next-steps"></a>後續步驟

使用本文提及的範例做為起點，以開發與您雲端採用方案保持一致的原則來解決特定的業務風險。

若要開始自行開發與成本管理相關的自訂原則聲明，請下載[成本管理範本](./template.md)。

若要加速採用這個專業領域，請選擇最符合您環境的可採取動作的[治理指南](../journeys/index.md)。 然後修改設計，以納入您特定的公司原則決策。

根據風險和承受度建立流程來治理和傳達成本管理原則的遵循情況。

> [!div class="nextstepaction"]
> [建立原則合規性流程](./compliance-processes.md)
