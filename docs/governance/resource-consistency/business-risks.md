---
title: 資源一致性動機和商務風險
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 資源一致性動機和商務風險
author: alexbuckgit
ms.author: abuck
ms.date: 02/11/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 42510f62cb3f673698832403126901789b05e978
ms.sourcegitcommit: a26c27ed72ac89198231ec4b11917a20d03bd222
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70826910"
---
# <a name="resource-consistency-motivations-and-business-risks"></a>資源一致性動機和商務風險

本文討論客戶通常會在雲端治理策略中採用資源一致性專業領域的原因。 它也提供幾個能驅使原則聲明之潛在商務風險的範例。

<!-- markdownlint-disable MD026 -->

## <a name="is-resource-consistency-relevant"></a>資源一致性有相關性嗎？

談到部署資源和工作負載，雲端提供比多數傳統內部部署資料中心更高的靈活度和彈性。 不過，這些潛在的雲端優勢也伴隨潛在管理缺點，可能嚴重危害您雲端採用的成功。 您已經部署哪些資產？ 哪些小組擁有何種資產？ 您有足夠支援工作負載的資源嗎？ 如何知道工作負載是否狀況良好？

資源一致性非常重要，可確保資源會以可重複的方式一致地部署、更新及設定，而且服務中斷會盡可能降到最低並在最短時間內補救。

資源一致性專業領域渉及識別及降低與您雲端部署作業層面相關的商務風險。 資源一致性包含應用程式、工作負載和資產效能的監視。 它也包含符合規模需求所需的工作、提供嚴重損壞修復功能、降低效能服務等級協定（SLA）違規，並透過自動修復主動避免這些 SLA 違規。

初始測試部署可能只需採用某些粗略命名和標記標準來支援您的資源一致性需求。 隨著您的雲端採用日趨成熟並部署更複雜任務關鍵性資產，在資源一致性專業領域的投資需求會快速增加。

## <a name="business-risk"></a>商務風險

資源一致性專業領域會嘗試解決核心作業商務風險。 在您規劃和實作雲端部署時，與您的企業和 IT 小組一起識別這些風險，並監視它們的關聯性。

組織之間的風險會有所不同，但以下是常見的風險，可做為您的雲端治理小組討論的起點：

- **不必要的營運成本。** 已淘汰、未使用或在低需求期間過度佈建的資源會增加不必要作業成本。
- **布建不足資源。** 高於預期需求的資源可能會造成業務中斷，因為雲端資源無法負荷需求。
- **管理效率不佳。** 缺乏一致的資源相關命名和標記中繼資料可能導致 IT 人員難以尋找管理工作所需資源或辨識資產相關所有權及會計資訊。 這會導致管理效率不足的情況，其可能會增加成本並減緩 IT 對於服務中斷或其他作業問題的回應能力。
- **業務中斷。** 造成您組織既有服務等級協定 (SLA) 違規的服務中斷可能會導致公司的企業損失或其他財務影響。

## <a name="next-steps"></a>後續步驟

使用[雲端管理範本](./template.md)，記錄了目前的雲端採用方案可能引進的業務風險。

一旦建立對於實際商務風險的了解，下一步是記錄風險的業務承受度與用來監視承受度的指標和關鍵計量。

> [!div class="nextstepaction"]
> [了解指標、計量及風險承受度](./metrics-tolerance.md)
