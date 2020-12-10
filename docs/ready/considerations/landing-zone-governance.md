---
title: 改善登陸區域控管
description: 改進登陸區域治理。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: internal
ms.openlocfilehash: 624a73a649e85362355757574dc1d1391297948d
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97013462"
---
# <a name="improve-landing-zone-governance"></a>改善登陸區域控管

登陸區域治理是整體治理的最小單位。 在您的前幾個登陸區域內建立音效治理基礎，可減少稍後在採用生命週期中所需的重構量。 改進登陸區域治理將會整合成本控制、建立基本工具以允許調整規模，並可讓雲端治理小組更輕鬆地在雲端治理的五個專業領域提供。

## <a name="landing-zone-governance-best-practices"></a>登陸區域治理最佳作法

- **初始登陸區域治理：** 建立 [初始治理基礎](../../govern/guides/complex/index.md) 的文章有助於將初始治理工具新增至前幾個登陸區域。 這些做法可協助調整採用和治理，以及實行合理的成本管理。 此方法的開頭為：資源組織、原則定義、RBAC 角色和藍圖定義。
- **[命名和標記標準](../azure-best-practices/naming-and-tagging.md)：** 確保命名和標記的一致性，這是建立音效治理實務的基本資料。
- **[追蹤工作負載之間的成本](../azure-best-practices/track-costs.md)：** 開始在您的第一個登陸區域中追蹤成本。 評估如何跨多個工作負載和角色來套用一致性。
- **[使用多個訂用帳戶進行調整](../azure-best-practices/scale-subscriptions.md)：** 評估此登陸區域和其他登陸區域的擴充方式，因為多個訂用帳戶會成為需求。
- **[組織訂閱](../azure-best-practices/organize-subscriptions.md)：** 瞭解如何組織和管理多個訂閱。

## <a name="four-steps-to-improve-overall-governance"></a>改進整體治理的四個步驟

治理 [方法](../../govern/index.md) 提供建立治理原則、程式和專業領域的整體指引。 我們將使用該方法的基本結構以及該方法的下列步驟，來改善所有登陸區域的登陸區域治理和治理。

1. [瞭解方法](../../govern/methodology.md)：瞭解指引結束狀態治理設計的基本方法。
2. [基準測試](../../govern/benchmark.md)：評估目前和未來狀態，以建立願景並採取行動。
3. [初始治理基礎](../../govern/initial-foundation.md)：小型、容易實施的治理工具組，可為所有登陸區域建立初始基礎。
4. [改進治理基礎](../../govern/foundation-improvements.md)：反復加入治理控制項，以強化所有登陸區域治理。

## <a name="next-steps"></a>後續步驟

雲端採用將會隨著新工作負載的每個 wave 或版本持續擴充。 為了持續滿足這些需求，雲端平臺小組應定期檢查其他登陸區域的最佳做法。

> [!div class="nextstepaction"]
> [查看其他登陸區域的最佳作法](../azure-best-practices/index.md)
