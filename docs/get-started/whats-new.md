---
title: 新功能
description: 瞭解適用于 Azure 的 Microsoft 雲端採用架構的最新更新。
author: JanetCThomas
ms.author: janet
ms.date: 07/21/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.openlocfilehash: 4d29e1512578cc3312a0f4eda32629368fb5927d
ms.sourcegitcommit: 12fa4597633ca8e04efbae7d0bd7526d3581618e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/20/2020
ms.locfileid: "88661957"
---
<!-- cSpell:ignore MSPs -->
<!-- markdownlint-disable MD024 -->

# <a name="whats-new-in-the-microsoft-cloud-adoption-framework-for-azure"></a>適用于 Azure 的 Microsoft 雲端採用架構中的新功能

以下是雲端採用架構最近所做的變更清單。

此架構與客戶、合作夥伴和內部的 Microsoft 團隊共同建立合作。 新的和更新的內容會在推出時發行。 這些版本可讓您與我們一起測試、驗證和精簡指導方針。 建議您與我們合作來打造適用于 Azure 的雲端採用架構。

## <a name="july-17-2020"></a>2020 年 7 月 17 日

此版本新增了許多新案例，讓雲端採用更具可採取動作。

### <a name="migration-scenarios"></a>移轉案例

新的 [遷移案例總覽頁面](../scenarios/index.md) 建置於 CAF 遷移方法上，以示範 Azure 如何提供「#OneMigrate」承諾。 它提供將多個第一個和協力廠商案例遷移至 Azure 的方法。 這包括三種新的遷移案例：

| 發行項 | 描述 |
|---|---|
| [Windows 虛擬桌面](../scenarios/wvd/index.md) | 此案例可提高產能，並加速各種工作負載的移轉，以支援使用者體驗。 |
| [Azure Stack](../scenarios/azure-stack/index.md) | 瞭解如何使用 Azure Stack Hub 在您的資料中心部署 Azure。 |

### <a name="analytics-in-caf"></a>CAF 中的分析

分析解決方案現已包含在 Microsoft 雲端採用架構中。 這些新的主題強調在您的雲端採用旅程期間啟用分析解決方案的最佳作法。

| 發行項 | 描述 |
|---|---|
| [適用于 Teradata、Netezza、Exadata 的分析解決方案](../migrate/azure-best-practices/analytics/analytics-solutions-overview.md) | 瞭解如何將舊版的內部部署環境（包括 Teradata、Netezza 和 Exadata）遷移至新式分析解決方案。 |
| [Azure Synapse 的高可用性](../migrate/azure-best-practices/analytics/azure-synapse.md) | 瞭解新式雲端式基礎結構的其中一個主要優點，內建高可用性和嚴重損壞修復。 |
| [ (DDL) 的架構遷移資料定義語言 ](../migrate/azure-best-practices/analytics/schema-migration-ddl.md) | 瞭解準備遷移現有資料時的資料庫物件和相關聯的進程。 |

### <a name="ai-in-caf"></a>CAF 中的 AI

人工智慧 (AI) 解決方案和最佳作法現在已整合到 Microsoft 雲端採用架構中。 這些 AI 解決方案可協助您利用有關客戶需求的預測來加速創新、將商務程式自動化、探索資訊、尋找新的方式來與客戶互動，並在您的雲端採用旅程期間提供更佳的體驗。

| 發行項 | 描述 |
|---|---|
| [負責任的 AI](../strategy/responsible-ai.md) | 瞭解您在實施 AI 解決方案時應考慮的 AI 原則，並瞭解如何建立負責任的 AI 策略。 |
| [Azure 創新指南：使用 AI 創新](../innovate/innovation-guide/predict.md) | 瞭解如何使用 AI 進行創新，並根據您的實所需求找出最佳解決方案。 |
| [雲端採用架構中的 AI](../innovate/ai/index.md) | 請參閱包含工具、程式和內容 (最佳做法、設定範本和架構指引) 的規範性架構，以簡化大規模採用 AI 和雲端原生的做法。 |
| [使用 Azure Machine Learning 的 MLOps](../manage/mlops-machine-learning.md) | 瞭解 Machine Learning 作業 (MLOps) 的最佳做法。 |
| [使用 AI 來創新](../innovate/best-practices/predict.md) | 瞭解 AI 解決方案 (Machine Learning、AI 應用程式和代理程式、知識挖掘) 以及可加速數位發明的最佳作法。 |

## <a name="june-15-2020"></a>2020 年 6 月 15 日

雲端環境的正確設定通常是雲端採用期間的第一個和最常見的技術封鎖程式。 此版本著重于加速部署雲端環境的指引。 為了克服這種常見的封鎖程式，適用于 Azure 的雲端採用架構引進了 **azure 登陸區域**。

| 發行項 | 描述 |
|---|---|
| [Azure 登陸區域](../ready/landing-zone/index.md) | Azure 登陸區域會建立一組常用的設計區域和實行選項，以加速環境建立，使其符合雲端採用方案和雲端作業模式。 這篇新文章更清楚地定義 Azure 登陸區域。 |
| [Azure 登陸區域：設計區域](../ready/landing-zone/design-areas.md) | 所有 Azure 登陸區域都共用一組通用的8個設計區域。 在部署任何 Azure 登陸區域之前，客戶應考慮這些設計，以做出重要的決策。 |
| [Azure 登陸區域：實行選項](../ready/landing-zone/implementation-options.md) | 根據您的雲端採用方案和雲端作業模式，選擇最佳的 Azure 登陸區域實行選項。 |

現有的 CAF 藍圖定義和 CAF Terraform 模組提供 Azure 登陸區域執行的起點。 不過，有些客戶需要更豐富的實作選項，以符合企業規模雲端採用方案的需求。 此版本會將 **CAF 企業規模** 新增至 Azure 登陸區域的執行選項，以滿足該需求。 以下列出一些文章，可協助您開始使用 CAF 企業規模的架構和參考。

| 發行項 | 描述 |
|---|---|
| [企業規模總覽](../ready/enterprise-scale/index.md) | 企業規模總覽 |
| [實作企業級登陸區域](../ready/enterprise-scale/implementation.md) | 快速的執行選項和 GitHub 範例 |
| [企業規模架構](../ready/enterprise-scale/architecture.md) | 瞭解企業規模背後的架構 |
| [企業規模的設計原則](../ready/enterprise-scale/design-principles.md) | 瞭解架構設計原則，這些原則會在執行期間引導決策，以評估此方法是否符合您的雲端作業模型 |
| [企業規模的設計指導方針](../ready/enterprise-scale/design-guidelines.md) | 評估企業規模的指導方針，以滿足 Azure 登陸區域的一般設計區域 |
| [實作指導方針](../ready/enterprise-scale/implementation-guidelines.md) | 在部署之前，請先檢查企業規模部署所需的活動 |

合作夥伴是成功採用雲端的重要層面。 在雲端採用架構指引中，我們新增了參考，以顯示夥伴扮演的重要角色，以及客戶如何更妥善地與合作夥伴互動。 如需已驗證的 CAF 夥伴清單，請參閱 [CAF 對齊的合作夥伴](https://aka.ms/adopt/partneroffers)供應專案、 [Azure 專家受控服務提供者 (Msp) ](https://www.microsoft.com/azure/partners/azureexpertmsp?filters=all)或 [Advanced 專家合作夥伴](https://www.microsoft.com/azure/partners/advspec)。

## <a name="may-15-2020"></a>2020 5 月15日

根據意見反應，我們已建立新的內容，讓您開始使用雲端採用架構。 新的「快速入門手冊」可協助您根據想要完成的工作來流覽架構。 我們也建立了新的登陸頁面，讓您更輕鬆地找到可支援雲端採用旅程的指引、工具、學習模組和程式。

| 發行項 | 描述 |
|---|---|
| [適用於 Azure 的雲端採用架構](../index.yml) | 雲端採用架構登陸頁面已經過重新設計，可讓您更輕鬆地找到可支援雲端採用旅程的指引、工具、學習模組和程式。 |
| [開始使用雲端採用架構](./index.md) | 從這裡開始，選擇與您的雲端採用目標一致的使用者入門指南。 這些常見案例會透過適用於 Azure 的 Microsoft 雲端採用架構來提供藍圖。 |
| [瞭解並記載基礎調整決策](./cloud-concepts.md) | 瞭解每個小組參與的雲端採用應瞭解的初始決策。 |
| [瞭解並對齊組合階層](../reference/fundamental-concepts/hosting-hierarchy.md) | 瞭解組合階層如何顯示您的工作負載和支援服務都能彼此配合。 |
| [Azure 產品如何支援組合階層？](../reference/fundamental-concepts/hierarchy-azure-tools.md) | 瞭解支援您組合階層的 Azure 工具與解決方案。 |
| [管理組織一致性](../organize/index.md) | 建立妥善的組織結構，使其成為適用于雲端的有效運作模式。 |

## <a name="april-14-2020"></a>2020 年 4 月 14 日

我們已將所有雲端採用工具和樣板集中在一處，讓他們更容易尋找。

| 發行項 | 描述 |
|----------|-------------|
| [工具和範本](../reference/tools-templates.md) | 尋找可協助您加速雲端採用旅程的工具、範本和評量。 |

## <a name="april-4-2020"></a>2020年4月4日

持續反復進行改進至遷移方法和準備方法，以更緊密地與 Microsoft 客戶、合作夥伴及內部計畫的意見反應。

### <a name="migrate-updates"></a>遷移更新

| 發行項                                                                                                                 | 描述                                                                                                                                                                                |
|-------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [遷移方法](../migrate/index.md)                       | 這些變更簡化了遷移工作的階段， (評估工作負載、部署工作負載，以及) 的發行工作負載。 這些變更也會移除有關遷移待處理專案的詳細資料。 移除這些詳細資料，並參考計畫、就緒和採用方法，反而會為各種不同的雲端採用方案建立彈性，以更妥善配合方法。 |
| 更新的目錄 | Azure 遷移指南和流程改善的目錄已更新，以反映方法的變更。 |

### <a name="ready-updates"></a>準備更新

| 發行項                                                                                                                 | 描述                                                                                                                                                                                |
|-------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [重構登陸區域](../ready/landing-zone/refactor.md)                       | **新文章：** 本文從現成的方法研討會進行繪製，示範從初始範本開始，使用決策樹和重構來擴展登陸區域，並移往未來的企業就緒狀態。 |
| [擴充登陸區域](../ready/considerations/index.md)                       | **新文章：** 以重構文章的「平行反覆運算」一節為基礎，顯示各種類型的登陸區域擴充如何將共用原則內嵌至支援的平臺。 本總覽的原始內容已移至目錄中的 [基本登陸考慮](../ready/considerations/basic-considerations.md) 節點。 |
| [登陸區域的測試驅動開發 (TDD)](../ready/considerations/test-driven-development.md)                       | **新文章：** 重構方法透過採用測試導向的開發週期，來引導登陸區域開發和重構，進而獲得更大的改進。 |
| [Azure 中的登陸區域 TDD](../ready/considerations/azure-test-driven-development.md)                       | **新文章：** Azure 治理工具提供了一種豐富的平臺，可用於 TDD 週期或紅色/綠色的測試。 |
| [改善登陸區域安全性](../ready/considerations/landing-zone-security.md)                       | **新文章：** 此節中的最佳作法簡介，與 TDD 迴圈相關。 |
| [改善登陸區域作業](../ready/considerations/landing-zone-operations.md)                       | **新文章：** 管理方法中的最佳做法清單，轉換成該模組化方法，以改善作業、可靠性和效能。 |
| [改善登陸區域控管](../ready/considerations/landing-zone-governance.md)                       | **新文章：** 與治理方法相關的最佳做法清單，轉換成該模組化方法以改善治理、成本管理和規模。 |
| [開始使用企業規模](../ready/enterprise-scale/index.md)                       | **新文章：** 示範當客戶從企業規模登陸區域範本開始時，顯示程式差異的方法。 本文可協助客戶瞭解支援這項決策的限定詞。 |
| 目錄更新                       | 目錄已更新以反映新的文章。 |

## <a name="march-27-2020"></a>2020年3月27日

我們已新增您在採用 Azure 時應建立之初始訂用帳戶的相關指引。

### <a name="ready-updates"></a>準備更新

| 發行項                                                                                                                 | 描述                                                                                                                                                                                |
|-------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [建立您的初始 Azure 訂用帳戶](../ready/azure-best-practices/initial-subscriptions.md)                       | **新文章：** 建立您的初始生產和非生產訂用帳戶，並決定是否要建立沙箱訂用帳戶，以及包含共用服務的訂用帳戶。 |
| [建立額外的訂用帳戶以調整 Azure 環境](../ready/azure-best-practices/scale-subscriptions.md) | 瞭解建立其他訂用帳戶、在訂用帳戶之間移動資源，以及建立新訂閱秘訣的原因。                                                   |
| [組織和管理多個 Azure 訂用帳戶](../ready/azure-best-practices/organize-subscriptions.md)             | 建立管理群組階層，以協助組織、管理及管理您的 Azure 訂用帳戶。                                                                                         |

## <a name="march-20-2020"></a>2020年3月20日

我們新增了規範性指導方針，包括依角色分類的工具、程式和內容，以在 Kubernetes 上推動應用程式成功部署，從概念證明到生產環境，再接著調整和優化。

### <a name="kubernetes"></a>Kubernetes

| 發行項                                                                                     | 描述                                                                                                                                                                           |
|---------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [應用程式開發與部署](../innovate/kubernetes/application-development.md) | **新文章：** 提供規劃應用程式開發、設定 CI/CD 管線，以及為 Kubernetes 實施網站可靠性工程的檢查清單、資源和最佳作法。 |
| [叢集設計和作業](../innovate/kubernetes/cluster-design-operations.md) | **新文章：** 提供檢查清單、資源和最佳作法，適用于叢集設定、網路設計、未來的調整規模、商務持續性，以及 Kubernetes 的嚴重損壞修復。 |
| [叢集和應用程式安全性](../innovate/kubernetes/cluster-application-security.md) | **新文章：** 提供檢查清單、資源和最佳作法，以 Kubernetes 安全性規劃、生產和調整。 |

## <a name="march-2-2020"></a>2020年3月2日

<!-- docsTest:ignore "Strategy, Plan, Ready, and Migrate" -->

為了回應透過雲端採用架構的多個區段（包括策略、規劃、準備和遷移）的持續性方法的相關意見反應，我們進行了下列更新。 這些更新的設計可讓您在繼續進行遷移旅程時，更輕鬆地瞭解規劃和採用的改進。

### <a name="strategy-updates"></a>策略更新

| 發行項                                                                       | 描述                                                                                                                                    |
|-------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| [平衡組合](../strategy/balance-the-portfolio.md)                 | 已將本文移至稍早在策略方法中顯示。 這可讓您在生命週期的先前思考流程中看到。 |
| [平衡 &nbsp; 競爭 &nbsp; 優先順序](../strategy/balance-competing-priorities.md) | **新文章：** 概述各方法之間的優先順序平衡，以協助通知您的策略。                                         |

### <a name="plan-updates"></a>規劃更新

| 發行項                                                                       | 描述                                                                                                                                                                           |
|-------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [評量 &nbsp; 最佳 &nbsp; 作法](../plan/contoso-migration-assessment.md) | 將本文移至方案方法的新「最佳做法」一節。 這可讓您瞭解在生命週期中稍早評估本機環境的做法。 |

### <a name="ready-updates"></a>準備更新

| 發行項                                                                   | 描述                                                                                                              |
|---------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| [什麼 &nbsp; 是 &nbsp; &nbsp; 登陸 &nbsp; 區域？](../ready/landing-zone/index.md)                 | **新文章：** 定義詞彙登陸區域。                                                      |
| 第一個登陸區域         | **新文章：** 擴充不同登陸區域的比較。                                                     |
| [CAF 遷移登陸區域](../ready/landing-zone/migrate-landing-zone.md) | 以第一個登陸區域的選取範圍分隔藍圖定義。         |
| [CAF Terraform 模組](../ready/landing-zone/terraform-landing-zone.md) | 移至「就緒」方法的新「登陸區域」區段，以提升登陸區域對話中的 Terraform。 |

### <a name="migration-updates"></a>遷移更新

| 發行項                                                                                          | 描述                                                                                                                                                             |
|--------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [概觀](../migrate/azure-migration-guide/index.md)                                            | 以較清楚的指南描述和較少的步驟來更新。                                                                                                        |
| [評估](../migrate/azure-migration-guide/assess.md)                                             | 新增「具挑戰性的假設」一節，以示範此等級的評定如何與計畫方法中所述的累加式評估方法搭配運作。 |
| [評定進程期間的分類](../migrate/migration-considerations/assess/classify.md) | **新文章：** 概述在遷移前分類每個資產和工作負載的重要性。                                                                    |
| [移轉](../migrate/azure-migration-guide/migrate.md)                                           | 新增協力廠商工具選項中的 UnifyCloud 參考，以回應第1層會議的意見反應。                                                         |
| [測試、 &nbsp; 優化 &nbsp; 及 &nbsp; 升階](../migrate/azure-migration-guide/optimize-and-transform.md)        | 將這篇文章的標題與其他程式改進建議保持一致。                                                                                           |
| [評定總覽](../migrate/migration-considerations/assess/index.md)                           | 已更新以說明此階段中的評量著重于評估特定工作負載和相關資產的技術。                               |
| [規劃檢查清單](../migrate/migration-considerations/prerequisites/planning-checklist.md)    | 已更新為在規劃遷移工作時，瞭解作業一致性的重要性，以確保在遷移後可妥善管理的工作負載。                  |

<!-- docsTest:ignoreNextStep -->
