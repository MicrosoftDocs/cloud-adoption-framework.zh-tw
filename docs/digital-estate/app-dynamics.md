---
title: 使用 AppDynamics 測量商務結果
description: 使用 AppDynamics 來瞭解應用程式的效能和使用者經驗如何影響業務成果。
author: BrianBlanchard
ms.author: wayneme
ms.date: 09/18/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.custom: think-tank
ms.openlocfilehash: 502dba2d970f46d8e929755a9c05be270724f501
ms.sourcegitcommit: d957bfc1fa8dc81168ce9c7d801a8dca6254c6eb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/23/2020
ms.locfileid: "95447093"
---
<!-- docutune:casing "Movie Tickets Online" -->

# <a name="measure-business-outcomes-with-appdynamics"></a>使用 AppDynamics 測量商務結果

測量和量化成功的業務成果是任何雲端採用策略的重要部分。 瞭解應用程式的效能和使用者體驗是測量這些業務成果的關鍵。 不過，精確地測量應用程式效能、使用者體驗和業務衝擊之間的關聯性，通常很難、不准確且耗時。

AppDynamics 可以針對大部分的使用案例供應商業見解，而許多組織會在下列使用案例中，開始使用全方位的雲端採用策略：

- 遷移前和遷移後比較
- 商務健康情況
- 發行驗證
- 區段健全狀況
- 使用者旅程
- 商務旅程
- 轉換漏斗圖

本指南將著重于如何測量遷移前和遷移後比較的業務成果，以及如何在雲端採用旅程期間加速和降低遷移的風險。

## <a name="how-appdynamics-works"></a>AppDynamics 的運作方式

在遷移之前，會將小型的輕量代理程式與您的應用程式一起部署。 代理程式專為各種不同的語言（例如 .NET、JAVA 和 Node.js）所建立。 代理程式會在遷移期間收集效能和診斷資料，並將其傳送至控制器，以相互關聯及分析資訊。 控制器可以位於完全受控的 AppDynamics 環境中，或客戶可以選擇在 Azure 中管理它們。 重要的使用者體驗會識別為「商務交易」，可協助您探索一般應用程式或商務效能的基準。 不論是傳統的伺服器基礎結構、資料庫、中介軟體元件、內部部署或雲端，都能針對整個應用程式和每個商務交易，即時識別所有應用程式元件和相依性。

![AppDynamics 的流程地圖](./media/app-dynamics-flow-map.jpg)

_圖1： AppDynamics 的流程地圖。_

## <a name="appdynamics-identifies-business-metrics"></a>AppDynamics 識別商務計量

AppDynamics 可協助您為您的應用程式定義商業價值、找出他們應符合的關鍵計量以保留其價值，並確認它們是否符合其目標業務成果。 AppDynamics 代理程式會直接從應用程式收集這些資料點和傳統的應用程式效能計量（例如回應時間和記憶體使用量），而不需要變更程式碼。

商務計量與業務成果密切相關。 許多組織都有複雜的計量，可測量獨特的業務成果，而且這些結果可以從會計和彈性相關的效能和客戶參與目標的範圍。 AppDynamics 會收集對您的組織而言是特定且有用的計量，而這些計量可以參與遷移前後的目前商務營運。

**範例︰**

從線上 marketplace 銷售 widget 的公司已在其 web 應用程式中識別出下列重要商務交易：

- 登陸頁面
- 加入至購物車
- 運送中
- 計費
- 確認訂單

這些類型的商務交易是電子商務應用程式的通用交易。 轉換漏斗圖是使用者在這些頁面上所經歷的旅程，並會直接在公司的平臺上導致銷售收入。 當使用者因為頁面效能不良或錯誤而放棄旅程時，這會直接影響公司的基礎收益。

此外，公司已找出下列重要的商務計量：

- 購物車總計
- 客戶區段
- 客戶位置

結合應用程式和商務效能計量有助於清楚地展示其應用程式的效能與其基礎收益有何關聯。 在遷移期間，此層級的可見度和類型的見解很重要。

可設定的儀表板是許多 AppDynamics 工具的其中一種，可將這些深入解析視覺化。 在此即時範例中，我們會看到整體的轉換漏斗圖，以及針對 abandoners 和購物車總計、客戶區段、地點和一般收益詳細資料，對個別頁面效能的影響。

![AppDynamics 業務影響儀表板](./media/app-dynamics-business-impact-dashboard.jpg)

_圖2： AppDynamics 業務影響儀表板。_

## <a name="resources-to-help-identify-business-metrics"></a>協助識別商務計量的資源

雲端採用架構的 [策略](../strategy/index.md) 和 [業務成果](../strategy/business-outcomes/index.md) 章節提供指引和策略，協助您找出組織的業務成果。

## <a name="pre--and-post-migration-comparison"></a>遷移前和遷移後比較

雲端提供龐大的優點和潛力，但遷移的第一個步驟通常不清楚，也不會有完整的風險。 成功的遷移必須比部署成功的功能更多。 瞭解雲端前和雲端前的使用者體驗和企業效能，可協助您在需要時調整和穩定兩者，進而協助產生成功的業務成果，同時強化 Azure 在整個遷移旅程中提供的價值。

若要建立 AppDynamics 如何供應商務和應用程式計量的基礎，請在遷移之前和之後比較這些計量，以評估其成功與否，以及是否符合目標業務成果。

**範例︰**

線上電影票證是虛構的線上 movie 券賣方，正在淘汰其現有的資料中心，並將其工作負載移至 Azure。 他們的容量問題導致商務交易效能不佳，而且會期待 Azure 中的效能優化和產能。

除了改善效能，他們想要確保提升其銷售漏斗圖和成長收入的業務成果。 在遷移過程中，他們會將 AppDynamics 部署到現有的內部部署環境，以清楚瞭解其目前的效能。 在雲端部署過程中，線上的電影票證可以使用 AppDynamics 與 Azure 的原生整合，以瞭解遷移後的效能和業務成果。

在此情況下，他們可以看到從48到79% 的轉換率增加，以及基礎效能、回應時間和票證銷售磁片區的改進。

![AppDynamics 遷移比較](./media/app-dynamics-migration-comparison.jpg)

_圖3： AppDynamics 遷移比較。_

## <a name="next-steps"></a>後續步驟

AppDynamics 可讓組織在其雲端採用策略期間，有獨特的能力來測量業務成果。 造訪 [AppDynamics](https://www.appdynamics.com/product/infrastructure-monitoring/cloud-monitoring/microsoft-azure) ，深入瞭解如何使用 Azure AppDynamics。
