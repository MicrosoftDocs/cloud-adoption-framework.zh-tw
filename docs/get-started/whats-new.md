---
title: 新功能
description: 深入瞭解適用于 Azure 的 Microsoft Cloud 採用架構的最新更新。
author: JanetCThomas
ms.author: janet
ms.date: 07/21/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.openlocfilehash: 619709a29e5effc542662987864d4c755aeca496
ms.sourcegitcommit: 622a7c5f1b47c9ad0a1c1ed3caa98bad6cf9d9c2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "87115077"
---
<!-- markdownlint-disable MD024 -->

# <a name="whats-new-in-the-microsoft-cloud-adoption-framework-for-azure"></a>適用于 Azure 的 Microsoft Cloud 採用架構的新功能

以下是最近對雲端採用架構所做的變更清單。

此架構與客戶、合作夥伴和內部 Microsoft 小組共同合作。 新的和更新的內容會在可用時發行。 這些版本可讓您測試、驗證及精簡指引和我們。 我們鼓勵您與我們合作，為 Azure 打造雲端採用架構。

## <a name="july-17-2020"></a>2020年7月17日
此版本新增了一些新案例，讓雲端採用更具可採取動作。

### <a name="migration-scenarios"></a>移轉案例

新的[遷移案例總覽頁面](../scenarios/index.md)以 CAF 遷移方法為基礎，示範 Azure 如何提供「#OneMigrate」承諾。 它提供將多個第一個和協力廠商案例遷移至 Azure 的方法。 這包括三個新的遷移案例：

| 發行項 | 說明 |
|---|---|
| [Windows 虛擬桌面](../scenarios/wvd/index.md) | 此案例可提高產能，並加速各種工作負載的移轉，以支援使用者體驗。 |
| [Azure Stack](../scenarios/azure-stack/index.md) | 瞭解如何使用 Azure Stack 中樞在資料中心內部署 Azure。 |

### <a name="analytics-in-caf"></a>CAF 中的分析
分析解決方案現已包含在 Microsoft Cloud 採用架構中。 這些新主題著重在雲端採用旅程圖中啟用分析解決方案的最佳作法。

| 發行項 | 說明 |
|---|---|
| [適用于 Teradata、Netezza、Exadata 的分析解決方案](../migrate/azure-best-practices/analytics/analytics-solutions-overview.md) | 瞭解如何將舊版內部部署環境（包括 Teradata、Netezza 和 Exadata）遷移至現代化分析解決方案。 |
| [Azure Synapse 的高可用性](../migrate/azure-best-practices/analytics/azure-synapse.md) | 瞭解新式雲端式基礎結構的其中一個主要優點，內建高可用性和嚴重損壞修復。 |
| [架構遷移資料定義語言（DDL）](../migrate/azure-best-practices/analytics/schema-migration-ddl.md) | 準備遷移現有資料時，瞭解資料庫物件和相關聯的進程。 |

### <a name="ai-in-caf"></a>CAF 中的 AI
人工智慧（AI）解決方案和最佳作法現在已整合到 Microsoft Cloud 採用架構中。 這些 AI 解決方案可協助加速創新，並提供有關客戶需求的預測、自動化商務程式、探索資訊、尋找與客戶互動的新方法，並在您的雲端採用旅程圖期間提供更佳的體驗。

| 發行項 | 說明 |
|---|---|
| [負責任的 AI](../strategy/responsible-ai.md) | 瞭解您在執行 AI 解決方案時應考慮的 AI 原則，並瞭解如何建立負責的 AI 策略。 |
| [Azure 創新指南：使用 AI 進行創新](../innovate/innovation-guide/predict.md) | 瞭解如何使用 AI 進行創新，並根據您的需求尋找最佳解決方案。 |
| [雲端採用架構中的 AI](../innovate/ai/index.md) | 請參閱包含工具、程式和內容 (最佳做法、設定範本和架構指引) 的規範性架構，以簡化大規模採用 AI 和雲端原生的做法。 |
| [使用 Azure Machine Learning 的 MLOps](../manage/mlops-machine-learning.md) | 深入瞭解 Machine Learning 作業（MLOps）的最佳作法。 |
| [使用 AI 來創新](../innovate/best-practices/predict.md) | 瞭解 AI 解決方案（Machine Learning、AI 應用程式和代理程式、知識挖掘）以及可加速數位家發明的最佳做法。 | 

## <a name="june-15-2020"></a>2020 年 6 月 15 日

雲端環境的適當設定通常是雲端採用期間的第一個和最常見的技術封鎖程式。 此版本主要著重于加速部署雲端環境的指導方針。 為了克服這種常見的封鎖程式，Azure 的雲端採用架構引進了**azure 登陸區域**。

| 發行項 | 說明 |
|---|---|
| [Azure 登陸區域](../ready/landing-zone/index.md) | Azure 登陸區域會建立一組通用的設計區域和執行選項，以加速與雲端採用方案和雲端作業模式的環境建立。 這篇新文章會更清楚地定義 Azure 登陸區域。 |
| [Azure 登陸區域：設計區域](../ready/landing-zone/design-areas.md) | 所有 Azure 登陸區域共用一組通用的8個設計區域。 在部署任何 Azure 登陸區域之前，客戶應該考慮每一種設計，以做出重要的決策。 |
| [Azure 登陸區域：執行選項](../ready/landing-zone/implementation-options.md) | 根據您的雲端採用方案和雲端作業模式，選擇最佳的 Azure 登陸區域執行選項。 |

現有的 CAF 藍圖定義和 CAF Terraform 模組會提供 Azure 登陸區域執行的起點。 不過，有些客戶需要更豐富的執行選項，才能符合企業級雲端採用方案的需求。 此版本將**CAF 企業規模**新增至 Azure 登陸區域的執行選項，以填滿該需求。 以下列出幾篇文章，讓您開始使用 CAF 企業級架構和參考的實體系。

| 發行項 | 說明 |
|---|---|
| [企業規模總覽](../ready/enterprise-scale/index.md) | 企業規模總覽 |
| [實作企業級登陸區域](../ready/enterprise-scale/implementation.md) | 快速的執行選項和 GitHub 範例 |
| [企業規模架構](../ready/enterprise-scale/architecture.md) | 瞭解企業規模背後的架構 |
| [企業規模的設計原則](../ready/enterprise-scale/design-principles.md) | 瞭解架構設計原則，這會在執行期間引導決策，以評估此方法是否符合您的雲端操作模型 |
| [企業規模的設計指導方針](../ready/enterprise-scale/design-guidelines.md) | 評估企業級的指導方針，以滿足 Azure 登陸區域的常見設計區域 |
| [實作指導方針](../ready/enterprise-scale/implementation-guidelines.md) | 在部署之前，檢查企業規模執行所需的活動 |

合作夥伴是成功採用雲端的重要層面。 在整個雲端採用架構指引中，我們新增了參考，以顯示重要角色合作夥伴的扮演，以及客戶如何更能吸引合作夥伴。 如需已驗證的 CAF 合作夥伴清單，請參閱[CAF 對齊的合作夥伴優惠](https://aka.ms/adopt/partneroffers)、 [Azure 專家 MSP 合作夥伴](https://www.microsoft.com/solution-providers/search?cacheId=9c2fed4f-f9e2-42fb-8966-4c565f08f11e)或[Advanced 專家合作夥伴](https://www.microsoft.com/azure/partners/advspec)。

## <a name="may-15-2020"></a>2020 5 月15日

根據意見反應，我們建立了新的內容，讓您開始使用雲端採用架構。 新的快速入門手冊可協助您根據想要完成的工作來流覽架構。 我們也建立了新的登陸頁面，讓您更輕鬆地找到可支援雲端採用成功旅程的指引、工具、學習模組和程式。

| 發行項 | 說明 |
|---|---|
| [Cloud Adoption Framework for Azure](../index.yml) | 雲端採用架構登陸頁面已經過重新設計，可讓您更輕鬆地找到可支援雲端採用成功旅程的指引、工具、學習模組和程式。 |
| [開始使用雲端採用架構](./index.md) | 從這裡開始，選擇與您的雲端採用目標一致的快速入門手冊。 這些常見案例會透過適用於 Azure 的 Microsoft 雲端採用架構來提供藍圖。|
| [瞭解並記載基本的對齊決策](./cloud-concepts.md) | 深入瞭解雲端採用所牽涉到的每個小組都應該瞭解的初始決策。 |
| [瞭解並對齊組合階層](../reference/fundamental-concepts/hosting-hierarchy.md) | 瞭解「公事包」階層如何顯示您的工作負載和支援服務如何彼此配合。 |
| [Azure 產品如何支援組合階層？](../reference/fundamental-concepts/hierarchy-azure-tools.md) | 瞭解支援您的組合階層的 Azure 工具和解決方案。 |
| [管理組織一致性](../organize/index.md) | 建立妥善的組織結構，使其成為雲端的有效操作模型。 |

## <a name="april-14-2020"></a>2020 年 4 月 14 日

我們已將所有雲端採用工具和樣板集中在一個位置，讓它們更容易尋找。

| 發行項 | 說明 |
|----------|-------------|
| [工具和範本](../reference/tools-templates.md) | 尋找可協助您加速雲端採用旅程的工具、範本和評量。 |

## <a name="april-4-2020"></a>2020年4月4日

持續反復使用遷移方法和準備方法，以更緊密地配合 Microsoft 客戶、合作夥伴和內部程式的意見反應。

### <a name="migrate-updates"></a>遷移更新

| 發行項                                                                                                                 | 說明                                                                                                                                                                                |
|-------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [遷移方法](../migrate/index.md)                       | 這些變更會簡化遷移工作的階段（評估工作負載、部署工作負載和發行工作負載）。 這些變更也會移除有關遷移待處理專案的詳細資料。 移除這些詳細資料並參考計畫、就緒和採用方法，會為各種不同的雲端採用方案建立彈性，以更符合方法。 |
| 已更新目錄 | Azure 遷移指南和程式改良功能的目錄已更新，以反映方法的變更。 |

### <a name="ready-updates"></a>準備更新

| 發行項                                                                                                                 | 說明                                                                                                                                                                                |
|-------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [重構登陸區域](../ready/landing-zone/refactor.md)                       | **新文章：** 這篇文章從現成的方法研討會進行繪製，示範從初始範本開始的理論，使用決策樹和重構來擴充登陸區域，並移至未來的企業就緒狀態。 |
| [擴充登陸區域](../ready/considerations/index.md)                       | **新文章：** 以重構文章的 [平行反復專案] 區段為基礎，顯示各種類型的登陸區域擴充如何將共用原則內嵌到支援的平臺。 此總覽的原始內容已移至目錄中的[基本 [登陸考慮](../ready/considerations/basic-considerations.md)] 節點。 |
| [登陸區域的測試驅動開發 (TDD)](../ready/considerations/test-driven-development.md)                       | **新文章：** 藉由採用測試導向的開發週期來引導您進行登陸區域的開發和重構，重構方法會大幅提升。 |
| [Azure 中的登陸區域 TDD](../ready/considerations/azure-test-driven-development.md)                       | **新文章：** Azure 治理工具為 TDD 迴圈或紅色/綠色測試提供了豐富的平臺。 |
| [改善登陸區域安全性](../ready/considerations/landing-zone-security.md)                       | **新文章：** 本章節中的最佳作法總覽，與 TDD 週期相關。 |
| [改善登陸區域作業](../ready/considerations/landing-zone-operations.md)                       | **新文章：** 管理方法中的最佳做法清單，並轉換成該模組化方法來改善作業、可靠性和效能。 |
| [改善登陸區域控管](../ready/considerations/landing-zone-governance.md)                       | **新文章：** 與管理方法相關的最佳作法清單，並轉換成該模組化方法來改善控管、成本管理和規模。 |
| [開始使用企業規模](../ready/enterprise-scale/index.md)                       | **新文章：** 示範當客戶從企業級登陸區域範本開始時，顯示程式中差異的方法。 本文可協助客戶瞭解支援這項決策的限定詞。 |
| 目錄更新                       | 目錄已更新，以反映新的文章。 |

## <a name="march-27-2020"></a>2020年3月27日

我們已新增您在採用 Azure 時應建立之初始訂用帳戶的相關指引。

### <a name="ready-updates"></a>準備更新

| 發行項                                                                                                                 | 說明                                                                                                                                                                                |
|-------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [建立您的初始 Azure 訂用帳戶](../ready/azure-best-practices/initial-subscriptions.md)                       | **新文章：** 建立初始生產和非生產訂用帳戶，並決定是否要建立沙箱訂閱，以及是否要包含共用服務。 |
| [建立額外的訂用帳戶來調整您的 Azure 環境](../ready/azure-best-practices/scale-subscriptions.md) | 瞭解建立其他訂用帳戶、在訂用帳戶之間移動資源，以及建立新訂閱之秘訣的原因。                                                   |
| [組織和管理多個 Azure 訂用帳戶](../ready/azure-best-practices/organize-subscriptions.md)             | 建立管理群組階層，以協助組織、管理及控制您的 Azure 訂用帳戶。                                                                                         |

## <a name="march-20-2020"></a>2020年3月20日

我們新增了規範性指導方針，其中包含依角色分類的工具、程式和內容，以在 Kubernetes 上成功部署應用程式，從概念證明到生產環境，然後再進行調整和優化。

### <a name="kubernetes"></a>Kubernetes

| 發行項                                                                                     | 說明                                                                                                                                                                           |
|---------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [應用程式開發與部署](../innovate/kubernetes/application-development.md) | **新文章：** 提供規劃應用程式開發、設定 CI/CD 管線，以及執行 Kubernetes 的網站可靠性工程的檢查清單、資源和最佳作法。 |
| [叢集設計和作業](../innovate/kubernetes/cluster-design-operations.md) | **新文章：** 針對 Kubernetes 的叢集設定、網路設計、未來的擴充性、商務持續性和嚴重損壞修復，提供檢查清單、資源和最佳作法。 |
| [叢集和應用程式安全性](../innovate/kubernetes/cluster-application-security.md) | **新文章：** 提供檢查清單、資源和最佳作法，以 Kubernetes 安全性規劃、生產和調整。 |

## <a name="march-2-2020"></a>2020年3月2日

<!-- docsTest:ignore "Strategy, Plan, Ready, and Migrate" -->

為了回應透過雲端採用架構的多個區段（包括策略、規劃、準備和遷移）的持續性考慮，我們進行了下列更新。 這些更新的設計可讓您在繼續進行遷移旅程時，更輕鬆地瞭解規劃和採用的改進。

### <a name="strategy-updates"></a>策略更新

| 發行項                                                                       | 說明                                                                                                                                    |
|-------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| [平衡組合](../strategy/balance-the-portfolio.md)                 | 本文章已移至稍早在策略方法中顯示。 這可讓您瞭解稍早在生命週期中的想法。 |
| [平衡 &nbsp; 競爭 &nbsp; 優先順序](../strategy/balance-competing-priorities.md) | **新文章：** 概述各種方法之間的優先順序平衡，以協助通知您的策略。                                         |

### <a name="plan-updates"></a>規劃更新

| 發行項                                                                       | 說明                                                                                                                                                                           |
|-------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [評估 &nbsp; 最佳 &nbsp; 做法](../plan/contoso-migration-assessment.md) | 這篇文章已移至計畫方法的新「最佳作法」一節。 這可讓您瞭解稍早在生命週期中評估本機環境的做法。 |

### <a name="ready-updates"></a>準備更新

| 發行項                                                                   | 說明                                                                                                              |
|---------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| [什麼 &nbsp; 是 &nbsp; &nbsp; 登陸 &nbsp; 區域？](../ready/landing-zone/index.md)                 | **新文章：** 定義「登陸區域」一詞。                                                      |
| 第一個登陸區域         | **新文章：** 擴充各種登陸區域的比較。                                                     |
| [CAF 遷移登陸區域](../ready/landing-zone/migrate-landing-zone.md) | 將藍圖定義與第一個登陸區域的選取範圍隔開。         |
| [CAF Terraform 模組](../ready/landing-zone/terraform-landing-zone.md) | 已移至準備方法的新「登陸區域」一節，以提升登陸區域交談中的 Terraform。 |

### <a name="migration-updates"></a>遷移更新

| 發行項                                                                                          | 說明                                                                                                                                                             |
|--------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [概觀](../migrate/azure-migration-guide/index.md)                                            | 以更清楚的指南描述和較少的步驟進行更新。                                                                                                        |
| [評估](../migrate/azure-migration-guide/assess.md)                                             | 新增「具挑戰性的假設」一節，以示範此層級的評估如何與計畫方法中所述的累加評估方法搭配運作。 |
| [評估程式期間的分類](../migrate/migration-considerations/assess/classify.md) | **新文章：** 概述在遷移之前分類每個資產和工作負載的重要性。                                                                    |
| [移轉](../migrate/azure-migration-guide/migrate.md)                                           | 已在協力廠商工具選項中新增 UnifyCloud 的參考，以回應第1層會議的意見反應。                                                         |
| [測試、 &nbsp; 優化 &nbsp; 及 &nbsp; 升級](../migrate/azure-migration-guide/optimize-and-transform.md)        | 將本文的標題與其他進程改進建議一致。                                                                                           |
| [評估總覽](../migrate/migration-considerations/assess/index.md)                           | 已更新，以說明此階段的評量著重于評估特定工作負載和相關資產的技術。                               |
| [規劃檢查清單](../migrate/migration-considerations/prerequisites/planning-checklist.md)    | 已更新，以在規劃遷移工作時清楚瞭解作業的重要性，以確保在遷移後進行妥善管理的工作負載。                  |

<!-- docsTest:ignoreNextStep -->
