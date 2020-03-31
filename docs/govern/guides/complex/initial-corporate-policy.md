---
title: 複雜的企業治理：初始公司原則
description: 使用適用于 Azure 的雲端採用架構來定義初始治理位置、初期風險、初始原則聲明，以及早期強制執行程式。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/05/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 327cbac3de0641b0fc4e8baf1c621dce7d5bc356
ms.sourcegitcommit: afe10f97fc0e0402a881fdfa55dadebd3aca75ab
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/31/2020
ms.locfileid: "80434439"
---
# <a name="governance-guide-for-complex-enterprises-initial-corporate-policy-behind-the-governance-strategy"></a>複雜企業的治理指南：治理策略背後的初始公司原則

下列公司原則會定義初始治理位置，這是本指南的起點。 本文定義初期風險、初始原則聲明，以及強制執行原則聲明的初期流程。

> [!NOTE]
>公司原則不是技術文件，但是會推動許多技術決策。 [概觀](./index.md)中所述的治理 MVP 最終會衍生自這個原則。 實作治理 MVP 之前，貴組織應該根據自己的目標和業務風險來開發公司原則。

## <a name="cloud-governance-team"></a>雲端治理小組

CIO 最近與 IT 治理小組見面，以瞭解個人資料和任務關鍵性原則的歷程記錄，並檢查變更這些原則的影響。 CIO 也討論了雲端對 IT 和公司的整體潛能。

會議之後，有兩個 IT 治理小組成員要求了研究並支援雲端規劃工作的權限。 IT 治理主管認可治理需求以及限制影子 IT 的機會，因此支援這個想法。 如此一來，雲端治理小組就已經出生了。 在接下來的幾個月，他們會從治理的觀點，接手清除在探索雲端期間所造成的許多錯誤。 這會獲得_雲端保管人_的標記。 在稍後的反復專案中，本指南將說明其角色在一段時間內的變更方式。

[!INCLUDE [business-risk](../../../../includes/business-risks.md)]

## <a name="tolerance-indicators"></a>承受度指標

目前的風險承受度很高，且投資雲端治理的意願偏低。 因此，承受度指標可當作早期警告系統來觸發時間和精力的投資。 如果觀察到下列指標，最好事先推進治理策略。

- **成本管理：** 部署的規模超過1000個資產到雲端，或每月支出超過每個月 $10000 美元。
- 身分**識別基準：** 包含具有舊版或協力廠商多重要素驗證需求的應用程式。
- **安全性基準：** 在定義的雲端採用方案中納入受保護的資料。
- **資源一致性：** 在定義的雲端採用計畫中包含任何任務關鍵性應用程式。

[!INCLUDE [policy-statements](../../../../includes/policy-statements.md)]

## <a name="next-steps"></a>後續步驟

此公司原則會準備雲端治理小組來實行治理 MVP，這將成為採用的基礎。 下一步是實作此 MVP。

> [!div class="nextstepaction"]
> [說明的最佳做法](./prescriptive-guidance.md)
