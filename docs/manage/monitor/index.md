---
title: 雲端監視指南
description: 了解 Azure 監視器、System Center Operations Manager，以及用於監視每個雲端部署模型的建議策略。
author: MGoedtel
ms.author: magoedte
ms.date: 10/05/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 1c59389754f8c109158ca8ac6aca54fe8cd92a1a
ms.sourcegitcommit: d81a822575820115d9814b0fc6c05ae33e535825
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/14/2020
ms.locfileid: "92058762"
---
# <a name="cloud-monitoring-guide-introduction"></a>雲端監視指南：簡介

雲端徹底改變企業採購和使用技術資源的方式。 在過去，企業要承擔從基礎結構到軟體等所有技術層級的擁有權和責任。 現在，雲端為企業提供了視需要佈建和取用資源的潛力。

雖然雲端對於設計選擇提供幾乎無限的彈性，但企業仍在尋找經過實證且一致的方法來採用雲端技術。 每個企業對於雲端採用皆有不同的目標與進度，因此幾乎不可能使用一體適用的採用方法。

![雲端採用策略的圖表](./media/monitoring-management-guidance-cloud-and-on-premises/introduction-cloud-adoption.png)

此數位化轉型也使您有機會實現基礎結構、工作負載和應用程式的現代化。 根據商務策略和目標，採用混合式雲端模型可能包含在從內部部署到完全在雲端中運作的移轉過程中。 在此旅程期間，IT 團隊面臨的挑戰是採用並實現雲端的快速價值。 IT 團隊也必須了解如何有效地監視遷移至 Azure 的應用程式或服務，並繼續提供有效的 IT 作業和 DevOps。

專案關係人想要使用雲端式軟體即服務 (SaaS) 監視和管理工具。 他們需要了解提供哪些服務和解決方案，可以實現端到端的可見性、降低成本，並減少對傳統軟體式 IT 作業工具基礎結構和維護的關注。

不過，IT 團隊通常偏好使用他們已投入大量資金的工具。 此方法可支援其服務作業流程監視兩個雲端模型，並以轉換至 SaaS 型供應項目作為最終目標。 IT 團隊偏好此方法不僅是因為切換需要時間、規劃、資源和資金。 也是因為不清楚哪些產品或 Azure 服務適用於或可用於實現轉換。

本指南的目標是要提供詳細的參考，以協助企業 IT 經理、業務決策者、應用程式架構設計人員和應用程式開發人員了解：

- Azure 監視平台針對其功能進行概述和比較。
- 用於監視混合、私人和 Azure 原生工作負載的最佳解決方案。
- 建議的端對端監視方法，用於監視基礎結構和應用程式。 此方法包括可部署的解決方案，可將這些常見的工作負載遷移至 Azure。

本文不是使用或配置個別 Azure 服務和解決方案的操作指南，但的確會在適用或可用時引用這些來源。 閱讀本文後，您將了解如何遵循最佳做法和模式來讓工作負載成功運作。

如果您不熟悉 Azure 監視器和 System Center Operations Manager，並希望深入了解其獨特之處和彼此之間如何進行比較，請參閱[監視平台概觀](./platform-overview.md)。

## <a name="audience"></a>適用對象

本指南主要適用於企業系統管理員、IT 作業、IT 安全性和合規性、應用程式架構設計人員、工作負載開發擁有者，以及工作負載作業擁有者。

## <a name="how-this-guide-is-structured"></a>指南結構

本文是系列文章的其中一篇。 下列文章應依序一起閱讀：

- 簡介 (本文)
- [雲端部署模型的監視策略](./cloud-models-monitor-overview.md)
- [收集正確的資料](./data-collection.md)
- [警示](./alerting.md)

## <a name="products-and-services"></a>產品與服務

有一些軟體和服務可協助您監視和管理各種裝載於 Azure、您的公司網路或其他雲端提供者的資源。 其中包括：

- [System Center Operations Manager](/system-center/scom/welcome)
- [Azure 監視器](/azure/azure-monitor/overview) (包含 Log Analytics 和 Application Insights)
- [Azure 原則](/azure/governance/policy/overview)和 [Azure 藍圖](/azure/governance/blueprints/overview)
- [Azure Arc](/azure/azure-arc/)
- [Azure 自動化](/azure/automation/automation-intro)
- [Azure Logic Apps](/azure/logic-apps/logic-apps-overview)
- [Azure 事件中樞](/azure/event-hubs/event-hubs-about)

本指南的第一個版本涵蓋我們目前的監視平台：Azure 監視器和 System Center Operations Manager。 其中也會概述用於監視每個雲端部署模型的建議策略。 其中也包含第一組監視建議，從資料收集和警示開始。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [雲端部署模型的監視策略](./cloud-models-monitor-overview.md)
