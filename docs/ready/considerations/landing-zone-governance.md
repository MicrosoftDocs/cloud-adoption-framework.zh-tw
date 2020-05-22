---
title: 改善登陸區域控管
description: 改善登陸區域控管
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/15/2020
ms.topic: overview
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: fd500f00ecd021a1d05904c675d63a64e8b6764b
ms.sourcegitcommit: 9a84c2dfa4c3859fd7d5b1e06bbb8549ff6967fa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2020
ms.locfileid: "83756527"
---
# <a name="improve-landing-zone-governance"></a>改善登陸區域控管

登陸區域治理是整體治理的最小單位。 在您的前幾個登陸區域內建立音效治理基礎，將可減少未來採用生命週期中所需的重構量。 改善登陸區域治理將會整合成本控制、建立基本工具以允許調整規模，並可讓雲端治理小組更輕鬆地傳遞五個雲端治理專業領域。

## <a name="landing-zone-governance-best-practices"></a>登陸區域治理最佳做法

- **初始登陸治理：** 建立[初始治理基礎](../../govern/guides/complex/index.md)的文章將有助於將初始治理工具新增至前幾個登陸區域。 這些實務會協助調整採用和治理，以及實行音效成本管理。 此方法的開頭為：資源組織、原則定義、RBAC 角色和藍圖定義。
- **[命名和標記標準](../azure-best-practices/naming-and-tagging.md)：** 確保命名和標記的一致性，這是建立音效治理實務的基本資料。
- **[追蹤工作負載之間的成本](../azure-best-practices/track-costs.md)：** 開始追蹤第一個登陸區域中的成本。 評估如何跨多個工作負載和角色來套用一致性。
- **[使用多個訂用帳戶進行調整](../azure-best-practices/scale-subscriptions.md)：** 評估此登陸區域和其他登陸區域的擴充方式，因為有多個訂用帳戶成為需求。
- **[組織訂閱](../azure-best-practices/organize-subscriptions.md)：** 瞭解如何組織和管理多個訂閱。

## <a name="four-steps-to-improve-overall-governance"></a>改善整體治理的四個步驟

[管理方法](../../govern/index.md)提供建立治理原則、流程和專業領域的整體指引。 我們將使用該方法的基本結構，以及該方法的下列步驟，來改善所有登陸區域的登陸區域治理和治理。

1. [瞭解方法](../../govern/methodology.md)：瞭解基本方法以引導結束狀態治理設計。
2. [基準測試](../../govern/benchmark.md)：評估目前和未來的狀態，以建立願景並採取行動。
3. [初始治理基礎](../../govern/initial-foundation.md)：一組小型、容易實行的治理工具，以建立所有登陸區域的初始基礎。
4. [改善治理基礎](../../govern/foundation-improvements.md)：反復新增治理控制項，以加強所有登陸區域治理。

## <a name="next-steps"></a>後續步驟

雲端採用會隨著每一波或發行的新工作負載繼續擴充。 為了滿足這些需求，建議雲端平臺小組定期[查看其他登陸區域的最佳作法](../azure-best-practices/index.md)。

> [!div class="nextstepaction"]
> [查看其他登陸區域的最佳作法](../azure-best-practices/index.md)
