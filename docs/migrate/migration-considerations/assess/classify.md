---
title: 遷移之前的工作負載分類
description: 在遷移前評量期間分類您的工作負載。
author: BrianBlanchard
ms.author: brblanch
ms.date: 03/03/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: internal
ms.openlocfilehash: 594c85a000f9b8ff8a78fd88d3b26869b2a57659
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101786289"
---
# <a name="workload-classification-before-migration"></a>遷移之前的工作負載分類

在任何遷移程式的每個反復專案中，將會遷移一或多個工作負載，並升階到生產環境。 在其中一個遷移活動之前，請務必分類每個工作負載。 分類有助於闡明治理、安全性、作業和資料管理需求。

下列指導方針以藉由新增重要[作業](../../../manage/considerations/criticality.md#criticality-scale)和[治理](../../../govern/guides/complex/prescriptive-guidance.md#resource-tagging)元素來[定義標記策略](../../../ready/azure-best-practices/resource-tagging.md)中所述的建議標記需求為基礎。

在本文中，我們特別建議您對現有的標記標準新增重要性和資料敏感性。 每個資料點都可協助其他小組瞭解哪些工作負載可能需要額外的注意或支援。

## <a name="data-sensitivity"></a>資料敏感度

如同 [資料分類](../../../govern/policy-compliance/data-classification.md)的文章所述，資料分類會測量資料流程失對企業或客戶造成的影響。 治理和安全性小組會使用資料敏感性或資料分類來作為安全性風險的指標。 在評估期間，雲端採用小組應該評估每個以遷移為目標的工作負載的資料分類，並與支援小組共用該分類。 嚴格處理「公用資料」的工作負載可能不會對支援小組造成任何影響。 不過，隨著資料不斷地移至範圍的「高度機密」，治理和安全性小組可能會有參與工作負載評量的有意興趣。

儘早使用您的安全性與治理小組來定義下列專案：

- 在待處理專案上以機密資料共用任何工作負載的明確程式。
- 瞭解各種不同資料敏感度層級所需的治理需求和安全性基準。
- 任何對資料敏感性的影響可能會有訂用帳戶設計、管理群組階層或登陸區域需求。
- 測試資料分類的任何需求，其中可能包含特定工具或分類的定義範圍。

## <a name="mission-criticality"></a>任務關鍵性

如有關 [工作負載重要性](../../../manage/considerations/criticality.md)的文章中所述，工作負載的重要性是測量企業在中斷期間會受到影響的程度。 此資料點可協助作業管理和安全性小組評估有關中斷和缺口的風險。 在評估期間，雲端採用小組應該評估每個以遷移為目標的工作負載的任務重要性，並與支援小組共用該分類。 「低」或「不支援」的工作負載可能不會對支援的小組造成很大的影響。 不過，當工作負載接近「任務關鍵性」或「全單元」分類時，其操作相依性會變得更明顯。

儘早使用您的安全性和作業小組來定義下列專案：

- 以支援需求在待處理專案上共用任何工作負載的明確程式。
- 瞭解各種不同嚴重性層級的作業管理和資源一致性需求。
- 對訂用帳戶設計、管理群組階層或登陸區域需求的任何影響重要性。
- 記錄重要性的任何需求，其中可能包括特定的流量或使用方式報表、財務分析或其他工具。

## <a name="next-steps"></a>下一步

一旦適當分類工作負載，就可以更輕鬆地 [調整業務優先順序](./business-priorities.md)。

> [!div class="nextstepaction"]
> [在業務優先順序上取得共識](./business-priorities.md)
