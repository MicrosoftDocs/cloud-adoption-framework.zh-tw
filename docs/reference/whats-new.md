---
title: 新功能
description: 瞭解適用于 Azure Microsoft Cloud 採用架構的更新。
author: BrianBlanchard
ms.author: brblanch
ms.date: 03/02/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: reference
ms.openlocfilehash: 0e93f0279ba210be594b3b5af40cf16657f224e3
ms.sourcegitcommit: 959cb0f63e4fe2d01fec2b820b8237e98599d14f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/11/2020
ms.locfileid: "79095305"
---
# <a name="whats-new-in-the-microsoft-cloud-adoption-framework"></a>Microsoft Cloud 採用架構的新功能

## <a name="fulfilling-the-vision"></a>履行願景

架構已達到公開上市 (GA) 階段。 不過，我們仍在積極地與客戶、合作夥伴及內部團隊一起建立此架構。 為了鼓勵更多人加入，相關內容會在可供使用時發行。 這些公開版本可讓您進行測試、驗證，然後逐漸精簡指導方針。

下列版本資訊中會捕捉到重大變更。

## <a name="migration-update-03022020"></a>遷移更新：03/02/2020

此版本透過多種方法來回應有關遷移方法持續性的意見反應，包括策略、規劃、準備及遷移。 此版本的目標是讓讀者更容易瞭解規劃和採用的持續改進，因為客戶會繼續進行遷移旅程。 為了滿足此版本的精神，已進行下列變更：

### <a name="new-articles"></a>新文章

- [平衡競爭優先順序](../strategy/balance-competing-priorities.md)：概述每個方法中發生的優先順序餘額，以通知客戶策略
- [評估程式期間的分類](../migrate/migration-considerations/assess/classify.md)：概述在遷移之前分類每個資產和工作負載的重要性。

### <a name="restructure-landing-zone-process"></a>重建登陸區域進程

第一個登陸區域已從準備就緒指南中提取，以作為其本身的區段。 這種方法可讓我們擴充登陸區域和登陸區域選項的概念。 這也會導致建立幾篇新文章：

- [何謂登陸區域？](../ready/landing-zone/index.md)：定義「登陸區域」一詞
- [第一個登陸區域](../ready/landing-zone/first-landing-zone.md)：擴展各種登陸區域的比較
- [遷移登陸區域](../ready/landing-zone/migrate-landing-zone.md)：移動先前的登陸區域藍圖一文，以區隔 CAF 藍圖的定義與第一個登陸區域的選擇，以允許更多登陸區域選項。
- [Terraform 登陸區域](../ready/landing-zone/terraform-landing-zone.md)文章：從準備方法的「擴充範圍」一節移至準備方法的新「登陸區域」一節，將 Terraform 視為登陸區域交談中的第一個類別公民。
- 在目錄中，將基本登陸區域考慮群組在「基本登陸區域考慮」底下。
- 將安全性最佳做法從遷移方法移至新登陸區域區段呼叫「改善登陸區域安全性（敏感性資料）」，以在生命週期中將讀者公開到本指引。

### <a name="refinements-to-the-azure-migration-guide"></a>改善 Azure 遷移指南

使用者流量模式會建議在其第一次遷移期間，實施者和架構設計人員可能會使用這一節的內容。 為了進一步支援此物件，遷移指南的重點已經過調整，以配合將讀者公開給第一次遷移時所使用之工具的原始目的。

- [總覽](../migrate/azure-migration-guide/index.md)：更清楚的指南描述，並減少步驟數。
- [評估](../migrate/azure-migration-guide/assess.md)：更清楚地說明此階段的評量著重于評估特定工作負載和相關資產的技術。 新增挑戰的假設一節，以示範此層級的評估如何與計畫方法中第一次提及的累加評估方法搭配運作。
- [遷移](../migrate/azure-migration-guide/migrate.md)：為了回應第1層會議的意見反應，已將 UnifyCloud 的參考新增至協力廠商工具選項。
- [測試、優化和升級](../migrate/azure-migration-guide/optimize-and-transform.md)：將本文的標題與其他處理常式改進建議進行對齊。

### <a name="refinements-to-migration-process-improvements"></a>改善遷移程式改進

- [評估總覽](../migrate/migration-considerations/assess/index.md)：更清楚地說明此階段的評量著重于評估特定工作負載和相關資產的技術。
- [規劃檢查清單](../migrate/migration-considerations/prerequisites/planning-checklist.md)：在規劃遷移工作時，請先清楚瞭解作業的重要性，以確保妥善管理的工作負載（遷移後）。

### <a name="placement-of-related-articles"></a>相關文章的位置

下列每篇文章清單都已移動。 流量會重新導向至新的位置。

- [評估最佳做法](../plan/contoso-migration-assessment.md)文章：從遷移方法的「最佳做法」一節移至計畫方法的新「最佳作法」一節。 這項移動會向讀者展示在生命週期稍早評估本機環境的實務。
- [平衡公事包](../strategy/balance-the-portfolio.md)文章：從遷移方法的「擴充範圍」一節移至策略方法。 這項移動會在稍早的生命週期中，將讀者公開到這個思考的流程。

### <a name="alignment-of-the-changes"></a>變更的對齊

- [準備好總覽](../ready/index.md)：更新 [準備好] 總覽中的四個步驟，以反映精簡的流程
- [遷移總覽](../migrate/index.md)：更新「遷移總覽」中的步驟，以反映精簡的流程
- [遷移方法圖形](../migrate/index.md)：更新為方法影像以反映變更
- [總覽圖形](../index.md)：用來反映變更的總覽圖形更新
