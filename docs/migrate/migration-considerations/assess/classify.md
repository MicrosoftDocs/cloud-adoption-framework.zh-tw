---
title: 遷移前的工作負載分類
description: 在進行預遷移評估期間分類您的工作負載。
author: BrianBlanchard
ms.author: brblanch
ms.date: 03/03/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 0708c394cca50c64813b7eea04a1239586c825af
ms.sourcegitcommit: da7ebd67a0ebf29361f093f00e10217b212a2eb2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/01/2020
ms.locfileid: "80527706"
---
# <a name="workload-classification-before-migration"></a>遷移前的工作負載分類

在任何遷移程式的每個反復專案期間，會將一或多個工作負載遷移並升級至生產環境。 在這兩個遷移活動之前，請務必分類每個工作負載。 分類有助於闡明治理、安全性、作業和資料管理需求。

下列指導方針是藉由新增重要[作業](../../../manage/considerations/criticality.md#criticality-scale)和[治理](../../../govern/guides/complex/prescriptive-guidance.md#resource-tagging)元素，以[命名和標記標準](../../../ready/azure-best-practices/naming-and-tagging.md#metadata-tags)一文中所述的建議標記需求為基礎。

在本文中，我們特別建議您對現有的標記標準增加重要性和資料敏感度。 每個資料點都可協助其他小組瞭解哪些工作負載可能需要額外注意或支援。

## <a name="data-sensitivity"></a>資料敏感度

如[資料分類](../../../govern/policy-compliance/data-classification.md)一文所述，資料分類會測量資料流程失對企業或客戶的影響。 治理和安全性小組會使用資料敏感性或資料分類作為安全性風險的指標。 在評估期間，雲端採用小組應該評估針對遷移目標的每個工作負載的資料分類，並與支援小組共用該分類。 嚴格處理「公用資料」的工作負載可能不會對支援小組造成任何影響。 不過，隨著資料進一步發展到範圍的「高度機密」端，治理和安全性小組在參與工作負載評估時，可能會有有意的興趣。

儘早與您的安全性和治理小組合作，以定義下列專案：

- 以敏感資料共用待處理專案上任何工作負載的清除程式。
- 瞭解各種不同資料敏感度層級所需的治理需求和安全性基準。
- 對資料敏感度的影響可能會對訂用帳戶設計、管理群組階層或登陸區域需求有任何影響。
- 測試資料分類的任何需求，其中可能包含特定工具或定義的分類範圍。

## <a name="mission-criticality"></a>任務重要性

如有關[工作負載重要性](../../../manage/considerations/criticality.md)的文章中所述，工作負載的重要性是測量在中斷期間，企業會受到何種影響的量值。 此資料點可協助作業管理和安全性小組評估有關中斷和缺口的風險。 在評估期間，雲端採用小組應該針對遷移目標的每個工作負載評估任務重要性，並與支援小組共用該分類。 「低」或「不支援」的工作負載可能會對支援小組造成極小的影響。 不過，當工作負載接近「任務關鍵」或「關鍵單位」分類時，其操作相依性就會變得更明顯。

儘早與您的安全性和營運小組合作，以定義下列專案：

- 在具有支援需求的待處理專案上共用任何工作負載的清除程式。
- 瞭解各種不同嚴重性層級的作業管理和資源一致性需求。
- 任何影響的重要性可能有訂用帳戶設計、管理群組階層或登陸區域需求。
- 記錄重要性的任何需求，其中可能包括特定的流量或使用方式報表、財務分析或其他工具。

## <a name="next-steps"></a>後續步驟

一旦適當地分類工作負載，就可以更輕鬆地[配合商務優先順序](./business-priorities.md)。

> [!div class="nextstepaction"]
> [在業務優先順序上取得共識](./business-priorities.md)
