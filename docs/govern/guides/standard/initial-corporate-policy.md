---
title: 標準 Enterprise 指南：治理策略背後的初始公司原則
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 標準 Enterprise 指南：治理策略背後的初始公司原則
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/05/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 4da4899038ca86bec2f1d003f9eaaee293119a09
ms.sourcegitcommit: 443c28f3afeedfbfe8b9980875a54afdbebd83a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2019
ms.locfileid: "71029160"
---
# <a name="standard-enterprise-guide-initial-corporate-policy-behind-the-governance-strategy"></a>標準 Enterprise 指南：治理策略背後的初始公司原則

下列公司原則會定義初始治理位置，這是本指南的起點。 本文定義初期風險、初始原則聲明，以及強制執行原則聲明的初期流程。

> [!NOTE]
>公司原則不是技術文件，但是會推動許多技術決策。 [概觀](./index.md)中所述的治理 MVP 最終會衍生自這個原則。 實作治理 MVP 之前，貴組織應該根據自己的目標和業務風險來開發公司原則。

## <a name="cloud-governance-team"></a>雲端治理小組

在此敘述中，雲端治理小組是由兩個系統管理員所組成，他們已識別治理的需求。 在接下來的幾個月，他們將會繼承清理公司雲端的治理作業，並為他們賺取_雲端保管人_的職稱。 在後續的反復專案中，此標題可能會變更。

[!INCLUDE [business-risk](../../../../includes/business-risks.md)]

## <a name="tolerance-indicators"></a>承受度指標

目前的風險承受度很高，且投資雲端治理的意願偏低。 因此，承受度指標可當作早期警告系統來觸發更多時間和精力的投資。 如果和在觀察到下列指標時，您應該反復改善治理策略。

- **成本管理：** 部署到雲端的規模超過 100 項資產，或每月支出超過每個月 $1,000 美元。
- **安全性基準：** 在定義的雲端採用計劃中納入受保護的資料。
- **資源一致性：** 在定義的雲端採用計劃中納入任何任務關鍵性應用程式。

[!INCLUDE [policy-statements](../../../../includes/policy-statements.md)]

## <a name="next-steps"></a>後續步驟

此公司原則會準備雲端治理小組來實行治理 MVP，這將成為採用的基礎。 下一步是實作此 MVP。

> [!div class="nextstepaction"]
> [說明的規範性指導方針](./prescriptive-guidance.md)
