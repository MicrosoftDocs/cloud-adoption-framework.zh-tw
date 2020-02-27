---
title: 將治理設計與公司原則保持一致
description: 使用適用于 Azure 的雲端採用架構，瞭解如何建立符合原則需求的架構選擇和設計模式。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.openlocfilehash: b68710606f0b361caec66e390e3ac826c1944a2e
ms.sourcegitcommit: af45c1c027d7246d1a6e4ec248406fb9a8752fb5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2020
ms.locfileid: "77708388"
---
# <a name="align-your-cloud-governance-design-guide-with-corporate-policy"></a>配合公司原則來調整您的雲端治理設計指南

在根據您的[已識別風險](./policy-definition.md)來[定義雲端原則](./business-risk.md)之後，您必須產生可採取行動的指引，與 IT 人員和開發人員參考的這些原則保持一致。 起草雲端治理設計指南可讓您根據為[五個治理專業領域](../governance-disciplines.md)所產生的原則聲明，指定特定的結構化、技術和流程選擇。

雲端治理設計指南應該為最符合您原則需求之雲端部署的每個核心基礎結構元件，建立架構選擇和設計模式。 除此之外，您應該提供將會支援每個設計決策之技術、工具和流程的高階說明。

雖然您的風險分析和原則聲明可能會有某種程度的不受雲端平臺限制，但您的設計指南應該提供您的 IT 和開發小組在建立和部署雲端式工作負載時所能使用的平臺特定的執行詳細資料。 進行設計決策和提供指引時，專注於您所選擇平台的架構、工具和功能。

雲端設計指南應該納入與每個基礎結構元件相關聯的一些技術詳細資料，它們不一定是廣泛的技術文件或規格。 請確定您的指南會處理您的原則聲明，並以方便員工瞭解和參考的格式，清楚陳述設計決策。

<!-- markdownlint-enable MD033 -->

## <a name="use-the-actionable-governance-guides"></a>使用可操作的治理指南

如果您打算使用 Azure 平臺進行雲端採用，雲端採用架構會提供可[操作的治理指南](../guides/index.md)，說明雲端採用架構治理模型的累加方法。 這些敘述指南涵蓋一系列常見的採用案例，包括企業風險、容錯需求，以及進入建立治理最低可行產品（MVP）的原則聲明。 這些指南代表 Azure 中雲端採用程式的真實世界客戶體驗合成。

雖然每個雲端採用都有唯一的目標、優先順序及挑戰，這些範例應該提供良好的範本，將您的原則轉換成指引。 挑選最接近您的情況的案例作為起點，並加以塑造以符合您的特定原則需求。

## <a name="next-steps"></a>後續步驟

設計指引就位時，建立原則遵循流程以確保原則合規性。

> [!div class="nextstepaction"]
> [建立原則遵循流程](./processes.md)
