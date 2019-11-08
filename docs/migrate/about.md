---
title: 雲端移轉
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 雲端移轉內容的簡介
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: ab28d38aafdddc0206b9fc7abc98cb489bba5d1b
ms.sourcegitcommit: 6f287276650e731163047f543d23581d8fb6e204
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/07/2019
ms.locfileid: "73751278"
---
# <a name="cloud-migration-in-the-microsoft-cloud-adoption-framework-for-azure"></a>在適用於 Azure 的 Microsoft 雲端採用架構中進行雲端移轉

雲端移轉是將現有數位資產移動至雲端平台的程序。 在此方法中，現有資產以最少的修改複寫至雲端。 當應用程式或工作負載開始在雲端運作時，使用者就從現有解決方案轉換到雲端解決方案。 雲端移轉通常是有效平衡雲端組合的一個方式。 這通常是短期內最快且最敏捷的方法。 相反地，若沒有其他未來的修改，雲端的一些優點可能就無法透過此方法實現。 企業和中型市場客戶使用此方法來加速變革的腳步、避免計劃資本支出和降低進行中的營運成本。

## <a name="cloud-migration-guidance"></a>雲端移轉指引

此節關於雲端採用架構的指導方針有兩個目的：

- 提供可採取行動的移轉指南，代表客戶通常會遇到的一般體驗。 每個指南都封裝了雲端移轉成功所需的程序和工具。 若有必要，設計指引可以專門針對 Azure。 這些指南中的所有其他建議均可在無從驗證的雲端或多重雲端方法中運用。
- 透過開發程序、角色和值則及變更管理控制項的詳細指引，協助讀者建立可滿足各種業務需求的個人化移轉方案，包括移轉至多個公用雲端。

此內容適用於雲端採用小組。 這也與開發雲端移轉強大基礎的雲端架構設計師息息相關。

## <a name="intended-audience"></a>目標對象

雲端採用架構的指引會影響企業的業務、技術和文化。 這一節會大幅影響應用程式擁有者、變更管理人員 (例如 PMO 和敏捷式管理人員)、財務和企業營運領導者，以及雲端採用小組。 對於這些角色有不同程度的相依性，將需要雲端架構設計師使用此指引才能達成。 對這些小組的接觸可能只需要進行一次；不過，在某些情況下，與其他這些角色的互動則需要不斷進行。

雲端架構設計師可引導並促使這些對象共同合作。 此系列指南的內容旨在協助雲端架構設計師促進與正確對象進行正確的對話，藉以推動必要的決策。 由雲端推動的企業轉型必須由雲端架構設計師居中協助指導整個業務和 IT 的決策。

本節**中的雲端架構設計人員特製化：** 雲端採用架構的每個部分都代表雲端架構設計師角色的不同特製化或變體。 本節專為雲端架構設計師而設計，其具備現有內部部署環境相關專業領域知識，以及了解該環境對移轉選項有何影響。

**責任分隔：** 在許多企業中，會有責任區隔，以限制存取公司環境的生產系統或區段。 在這種情況下，移轉程序會變得更複雜。 在某些情況下，這些角色和職責可能需要多個雲端架構設計師，才能橫跨整個移轉程序。

**合作選項** 在上述每個程序中，小組將學習技術執行的新技能和方法。 在執行期間，擴充現有小組成員之間的技術技能是一個選項。 雇用其他人員。 與第三方合作通常可明顯加速作業及降低風險。 [合作選項](./migration-considerations/assess/partnership-options.md)可協助引導選擇最佳合作選項的決策。

## <a name="next-steps"></a>後續步驟

對於想要完全遵循本指南的讀者，此內容將有助於開發強大的雲端移轉策略。 本指南會引導讀者了解此類策略的理論和實作。

第一個步驟是逐步執行[Azure 遷移指南](./azure-migration-guide/index.md)，以瞭解在常見使用案例中進行遷移所需的一組標準工具和方法。 之後，可以透過[複雜的移轉案例](./expanded-scope/index.md)、[移轉最佳做法](./azure-best-practices/index.md)和[移轉考量](./migration-considerations/index.md)，擴充基準指引以符合讀者的需求。

> [!div class="nextstepaction"]
> [Azure 移轉指南](./azure-migration-guide/index.md)
