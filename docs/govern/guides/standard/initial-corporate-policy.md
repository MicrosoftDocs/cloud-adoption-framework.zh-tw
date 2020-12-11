---
title: 標準企業治理：初始公司原則
description: 使用適用于 Azure 的雲端採用架構，來定義初始標準治理位置、初期風險、初始原則聲明，以及早期強制執行的流程。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/05/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: internal
ms.openlocfilehash: 8313b926ff9a65d9210e9d9adaa66f4ae9862aa7
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97020738"
---
# <a name="standard-enterprise-governance-guide-initial-corporate-policy-behind-the-governance-strategy"></a>標準企業治理指南：治理策略背後的初始公司原則

下列公司原則會定義初始治理位置，也就是本指南的起點。 本文定義初期風險、初始原則聲明，以及強制執行原則聲明的初期流程。

> [!NOTE]
> 公司原則不是技術文件，但是會推動許多技術決策。 [概觀](./index.md)中所述的治理 MVP 最終會衍生自這個原則。 實作治理 MVP 之前，貴組織應該根據自己的目標和業務風險來開發公司原則。

## <a name="cloud-governance-team"></a>雲端治理小組

在此敘述中，雲端治理小組是由兩個系統管理員所組成，這些系統管理員已辨識出治理的需求。 在接下來的幾個月，他們將會繼承清除公司雲端的治理，並以 *雲端* 監管者的職稱贏得他們的工作。 在後續的反復專案中，此標題可能會變更。

[!INCLUDE [business-risk](../../../../includes/business-risks.md)]

## <a name="tolerance-indicators"></a>承受度指標

目前的風險承受度很高，且投資雲端治理的意願偏低。 因此，承受度指標可當作早期警告系統來觸發更多時間和精力的投資。 如果觀察到下列指標，您應該反復地改善治理策略。

- **成本管理：** 部署規模超過預先決定的資源數量限制或每月成本。
- **安全性基準：** 在定義的雲端採用計畫中納入受保護的資料。
- **資源一致性：** 在定義的雲端採用計畫中納入任何任務關鍵性應用程式。

[!INCLUDE [policy-statements](../../../../includes/policy-statements.md)]

## <a name="next-steps"></a>後續步驟

此公司原則會準備雲端治理小組，以實行治理 MVP，這將是採用的基礎。 下一步是實作此 MVP。

> [!div class="nextstepaction"]
> [說明的最佳作法](./prescriptive-guidance.md)
