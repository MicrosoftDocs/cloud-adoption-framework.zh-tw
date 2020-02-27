---
title: 成本管理範例原則聲明
description: 使用適用于 Azure 的雲端採用架構，取得可協助您草擬原則聲明的範例成本管理原則聲明。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 3f9b7076cb50d9526beccfe4faabaa043b010841
ms.sourcegitcommit: af45c1c027d7246d1a6e4ec248406fb9a8752fb5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2020
ms.locfileid: "77708881"
---
# <a name="cost-management-sample-policy-statements"></a>成本管理範例原則聲明

個別雲端原則聲明是解決在風險評估流程中識別出的特定風險方針。 這些聲明應該會提供風險的簡明摘要，以及如何處理這些風險的方案。 每個聲明定義都應該包含下列資訊：

- **業務風險：** 此原則將解決的風險摘要。
- **原則聲明：** 原則需求的清楚摘要說明。
- **設計選項：** IT 小組與開發人員可以在執行原則時使用的可操作建議、規格或其他指引。

下列範例原則聲明會解決與一般成本相關的商務風險。 這些語句是您在草擬原則聲明以滿足組織需求時可以參考的範例。 這些範例並不是為了規範性，而且可能會有數個原則選項來處理每個已識別的風險。 與企業和 IT 小組密切合作，找出您獨特風險的最佳原則。

## <a name="future-proofing"></a>與未來技術的兼容性

**業務風險：** 目前的準則，不保證會向治理小組投資成本管理的專業領域。 不過，您可以預期未來將有這類投資。

**原則聲明：** 您應該將所有部署至雲端的資產與帳單單位和應用程式/工作負載產生關聯。 此原則將確保未來的成本管理成果將會生效。

**設計選項：** 如需有關建立未來證明基礎的詳細資訊，請參閱與在雲端採用架構指引中包含的可[操作設計指南](../guides/index.md)中建立治理 MVP 相關的討論。

## <a name="budget-overruns"></a>預算超支

**業務風險：** 自助式部署會產生超支的風險。

**原則聲明：** 任何雲端部署都必須配置給具有已核准預算的計費單位和預算限制的機制。

**設計選項：** 在 Azure 中，可以使用[Azure 成本管理](https://docs.microsoft.com/azure/cost-management/manage-budgets)來控制預算

## <a name="underutilization"></a>使用量過低

**業務風險：** 公司已預付雲端服務的費用，或已進行年度承諾以支付特定金額。 風險是不會使用已同意的金額，因而導致投資遺失。

**原則聲明：** 每個具有已配置雲端預算的計費單位，每年會達到設定預算、每季調整預算，以及配置時間來審查計畫與實際支出。 每月都要與計費單位的負責人討論任何大於 20% 的偏差。 基於追蹤目的，請將所有資產都指派給一個計費單位。

**設計選項：**

- 在 Azure 中，已規劃與實際的支出均可透過 [Azure 成本管理](https://docs.microsoft.com/azure/cost-management/quick-acm-cost-analysis)來管理。
- 有數個選項可讓計費單位用來群組資源。 在 Azure 中，應該與治理小組一同選擇[資源一致性模型](../../decision-guides/resource-consistency/index.md)並套用至所有資產。

## <a name="overprovisioned-assets"></a>過度佈建的資產

**業務風險：** 在傳統的內部部署資料中心中，常見的作法是部署資產，並在遙遠的未來進行成長，以提供額外的容量規劃。 相較於傳統設備，雲端可以更快速地進行調整。 雲端中的資產也會根據技術容量來計費。 對於舊的內部部署做法，會有以人為方式誇大雲端支出的風險。

**原則聲明：** 任何部署到雲端的資產都必須在可監視使用率的程式中註冊，並報告任何超過50% 使用率的容量。 所有部署至雲端的資產都必須以邏輯方式加以群組或標記，如此一來，治理小組成員就能讓工作負載擁有者參與關於過度佈建資產的任何最佳化。

**設計選項：**

- 在 Azure 中，[Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-cost-recommendations) 可以提供最佳化建議。
- 有數個選項可讓計費單位用來群組資源。 在 Azure 中，應該與治理小組一同選擇[資源一致性模型](../../decision-guides/resource-consistency/index.md)並套用至所有資產。

## <a name="overoptimization"></a>過度最佳化

**業務風險：** 有效的成本管理會產生新的風險。 將支出最佳化，正好與系統效能相反。 降低成本時，會造成過度削減支出，而產生不良使用者體驗的風險。

**原則聲明：** 任何直接影響客戶體驗的資產都必須透過群組或標記來識別。 在將任何影響客戶體驗的資產優化之前，雲端治理小組必須根據至少90天的使用量趨勢來調整優化。 記載在將資產最佳化時所考慮的任何季節性或事件驅動的高載。

**設計選項：**

- 在 Azure 中，[Azure 監視器的深入解析功能](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-performance)有助於分析系統使用量。
- 有數個選項可根據角色來群組和標記資源。 在 Azure 中，您應該與治理小組一同選擇[資源一致性模型](../../decision-guides/resource-consistency/index.md)，並將此項套用至所有資產。

## <a name="next-steps"></a>後續步驟

使用本文提及的範例作為起點，以開發與您雲端採用方案保持一致的原則來解決特定業務風險。

若要開始自行開發與成本管理相關的自訂原則聲明，請下載[成本管理範本](./template.md)。

若要加速採用這個專業領域，請選擇最符合您環境的可採取動作的[治理指南](../guides/index.md)。 然後修改設計，以納入您特定的公司原則決策。

根據風險和承受度建立流程來治理和傳達成本管理原則的遵循情況。

> [!div class="nextstepaction"]
> [建立原則合規性流程](./compliance-processes.md)
