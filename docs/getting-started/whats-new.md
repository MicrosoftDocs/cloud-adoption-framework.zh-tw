---
title: 新功能
description: 深入瞭解適用于 Azure 的 Microsoft Cloud 採用架構的最新更新。
author: JanetCThomas
ms.author: janet
ms.date: 03/27/2020
ms.topic: overview
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.openlocfilehash: 6bde40ffaa84955811b2687b2b518d93d0c21589
ms.sourcegitcommit: ea63be7fa94a75335223bd84d065ad3ea1d54fdb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/27/2020
ms.locfileid: "80357195"
---
<!-- markdownlint-disable MD024 -->

# <a name="whats-new-in-the-microsoft-cloud-adoption-framework-for-azure"></a>適用于 Azure 的 Microsoft Cloud 採用架構的新功能

以下是最近對雲端採用架構所做的變更清單。

此架構與客戶、合作夥伴和內部 Microsoft 小組共同合作。 新的和更新的內容會在可用時發行。 這些版本可讓您測試、驗證及精簡指引和我們。 我們鼓勵您與我們合作，為 Azure 打造雲端採用架構。

## <a name="march-27-2020"></a>2020年3月27日

我們已新增您在採用 Azure 時應建立之初始訂用帳戶的相關指引。

### <a name="ready-updates"></a>準備更新

| 發行項                                                                                                                 | 描述                                                                                                                                                                                |
|-------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [建立您的初始 Azure 訂用帳戶](../ready/azure-best-practices/initial-subscriptions.md)                       | **新文章：** 建立初始生產和非生產訂用帳戶，並決定是否要建立沙箱訂閱，以及是否要包含共用服務。 |
| [建立額外的訂用帳戶來調整您的 Azure 環境](../ready/azure-best-practices/scale-subscriptions.md) | 瞭解建立其他訂用帳戶、在訂用帳戶之間移動資源，以及建立新訂閱之秘訣的原因。                                                   |
| [組織和管理多個 Azure 訂用帳戶](../ready/azure-best-practices/organize-subscriptions.md)             | 建立管理群組階層，以協助組織、管理及控制您的 Azure 訂用帳戶。                                                                                         |

## <a name="march-20-2020"></a>2020年3月20日

我們新增了規範性指導方針，其中包含依角色分類的工具、程式和內容，以在 Kubernetes 上成功部署應用程式，從概念證明到生產環境，然後再進行調整和優化。

### <a name="kubernetes"></a>Kubernetes

| 發行項                                                                                     | 描述                                                                                                                                                                           |
|---------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [應用程式開發和部署](../innovate/kubernetes/application-development.md) | **新文章：** 提供規劃應用程式開發、設定 DevOps 管線，以及執行 Kubernetes 的網站可靠性工程的檢查清單、資源和最佳作法。 |
| [叢集設計和作業](../innovate/kubernetes/cluster-design-operations.md) | **新文章：** 針對 Kubernetes 的叢集設定、網路設計、未來的擴充性、商務持續性和嚴重損壞修復，提供檢查清單、資源和最佳作法。 |
| [叢集和應用程式安全性](../innovate/kubernetes/cluster-application-security.md) | **新文章：** 提供檢查清單、資源和最佳作法，以 Kubernetes 安全性規劃、生產和調整。 |

## <a name="march-2-2020"></a>2020年3月2日

為了回應透過雲端採用架構的多個區段（包括策略、規劃、準備和遷移）的持續性考慮，我們進行了下列更新。 這些更新的設計可讓您在繼續進行遷移旅程時，更輕鬆地瞭解規劃和採用的改進。

### <a name="strategy-updates"></a>策略更新

| 發行項                                                                       | 描述                                                                                                                                    |
|-------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| [平衡組合](../strategy/balance-the-portfolio.md)                 | 本文章已移至稍早在策略方法中顯示。 這可讓您瞭解稍早在生命週期中的想法。 |
| [&nbsp;競爭&nbsp;優先順序的平衡](../strategy/balance-competing-priorities.md) | **新文章：** 概述各種方法之間的優先順序平衡，以協助通知您的策略。                                         |

### <a name="plan-updates"></a>規劃更新

| 發行項                                                             | 描述                                                                                                                                                                           |
|---------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [評量&nbsp;最佳&nbsp;實務](../plan/contoso-migration-assessment.md) | 這篇文章已移至計畫方法的新「最佳作法」一節。 這可讓您瞭解稍早在生命週期中評估本機環境的做法。 |

### <a name="ready-updates"></a>準備更新

| 發行項                                                                   | 描述                                                                                                              |
|---------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| [&nbsp;&nbsp;登陸&nbsp;區域的&nbsp;為何？](../ready/landing-zone/index.md)                 | **新文章：** 定義「登陸區域」一詞。                                                                          |
| [第一個登陸區域](../ready/landing-zone/first-landing-zone.md)         | **新文章：** 擴充各種登陸區域的比較。                                                     |
| [遷移登陸區域](../ready/landing-zone/migrate-landing-zone.md)     | 將雲端採用架構藍圖的定義與第一個登陸區域的選擇隔開。         |
| [Terraform 登陸區域](../ready/landing-zone/terraform-landing-zone.md) | 已移至準備方法的新「登陸區域」一節，以提升登陸區域交談中的 Terraform。 |

### <a name="migration-updates"></a>遷移更新

| 發行項                                                                                          | 描述                                                                                                                                                             |
|--------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [概觀](../migrate/azure-migration-guide/index.md)                                            | 以更清楚的指南描述和較少的步驟進行更新。                                                                                                        |
| [評估](../migrate/azure-migration-guide/assess.md)                                             | 新增「具挑戰性的假設」一節，以示範此層級的評估如何與計畫方法中所述的累加評估方法搭配運作。 |
| [評估程式期間的分類](../migrate/migration-considerations/assess/classify.md) | **新文章：** 概述在遷移之前分類每個資產和工作負載的重要性。                                                                    |
| [移轉](../migrate/azure-migration-guide/migrate.md)                                           | 已在協力廠商工具選項中新增 UnifyCloud 的參考，以回應第1層會議的意見反應。                                                         |
| [測試、&nbsp;優化、&nbsp;和&nbsp;升階](../migrate/azure-migration-guide/optimize-and-transform.md)        | 將本文的標題與其他進程改進建議一致。                                                                                           |
| [評估總覽](../migrate/migration-considerations/assess/index.md)                           | 已更新，以說明此階段的評量著重于評估特定工作負載和相關資產的技術。                               |
| [規劃檢查清單](../migrate/migration-considerations/prerequisites/planning-checklist.md)    | 已更新，以在規劃遷移工作時清楚瞭解作業的重要性，以確保在遷移後進行妥善管理的工作負載。                  |

<!-- test:ignoreNextStep -->
