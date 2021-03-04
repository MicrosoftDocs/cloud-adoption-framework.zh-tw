---
title: 雲端策略反模式
description: 避免造成雲端採用專案失敗。 採取步驟來清楚定義和溝通雲端採用 Kpi 和動機。
author: mahia127
ms.author: brblanch
ms.date: 02/19/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: strategy
ms.custom: think-tank
ms.openlocfilehash: 455d4011ea01de622e829541d9bab49b651fe391
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102117264"
---
# <a name="cloud-strategy-antipatterns"></a>雲端策略反模式

客戶通常會在雲端採用的策略階段遇到反模式。 這些反模式可以讓 it 和商務策略的調整更加困難。 它們也會讓您難以測量雲端專案的成功與否。

## <a name="antipattern-adopt-the-cloud-without-establishing-goals"></a>反模式：在不建立目標的情況下採用雲端

許多公司都宣佈雲端優先或僅限雲端的策略。 但是，它們並不清楚地定義了這些策略所要達到的目標。 少數雲端專案可能會成功，但沒有具體的 Kpi 和目標。 無法測量沒有指標或指定目標的專案效能。

### <a name="example-migrate-to-the-cloud-without-defining-goals"></a>範例：在不定義目標的情況下遷移至雲端

公司最接近的競爭者會啟動僅限雲端的策略。 競爭對手的目標是要讓雲端中的所有系統都在一年內，以加速業務。 公司不想要追蹤背後的進度。 Corporation 的總監開始策略性地討論如何快速採用雲端。 但是，它們不會定義任何具體的成功準則，例如降低成本或提升系統效能。

公司的第一個系統會遷移至雲端。 Director 無法檢查其雲端策略是否成功，因為它們絕對不會定義他們想要達到的目標。

### <a name="preferred-outcome-define-goals-and-kpis"></a>慣用結果：定義目標和 Kpi

在討論您採用雲端的原因時，請定義具體的 Kpi。 然後，您將能夠測量策略的成功程度。 您也會知道是否可以針對其他專案使用相同的策略。 瞭解 [我們為何要移至雲端？](../strategy/motivations.md) 若要深入瞭解雲端採用的動機，請參閱。

## <a name="antipattern-fail-to-communicate-motivations"></a>反模式：無法傳達動機

當公司內的動機或雲端採用觸發程式未對齊時，雲端採用旅程可能會失敗。 企業可能會看到採用雲端的主要優點，但無法傳達這些採用觸發程式。 當這些動機影響公司的 IT 策略（而不只是其商務策略）時，就會出現這個問題。 如果沒有對齊或記載的動機，雲端旅程通常會失敗。

### <a name="example-fail-to-communicate-benefits"></a>範例：無法溝通優點

當 corporation 的管理面板宣佈雲端優先的 IT 策略時，會開始使用雲端。 但是，此面板不會說明策略如何受益于該公司。 IT 和營業單位並不確定他們是採用雲端的原因。 這種不確定性會導致缺少焦點，這表示部門無法因應這種常見的目標。 關於採用雲端的任何猶豫會增加，尤其是在此情況下。 因為 IT 策略的定義不佳，所以該策略在進行審查時，會導致更多問題的答案。

### <a name="preferred-outcome-define-and-communicate-reasons-for-cloud-adoption"></a>慣用結果：定義和溝通雲端採用的原因

決定您想要採用雲端的原因。 然後清楚定義您的原因，並在整個公司進行通訊。 然後，您的 IT 和營業單位將可更輕鬆地接受雲端採用。 您也可以使用這些動機來影響雲端採用旅程的後續階段中的技術決策。 在證明移至雲端之前，請先複習 [雲端遷移的誤解](../strategy/cloud-migration-business-case.md)。

## <a name="next-steps"></a>下一步

- [為何要移至雲端？](../strategy/motivations.md)
- [建置雲端移轉的商業論證](../strategy/cloud-migration-business-case.md)