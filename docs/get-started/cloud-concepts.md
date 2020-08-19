---
title: 開始使用：記錄基礎對齊決策
description: 瞭解並記載推動雲端採用旅程所需的初始決策。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.openlocfilehash: 619dd5197bcea95d83b27c166e0f4238abd1a988
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88573933"
---
# <a name="get-started-understand-and-document-foundational-alignment-decisions"></a>開始使用：瞭解及記錄基礎對齊決策

雲端採用旅程可將許多商業、技術和組織的優點發揮到最大。 無論您想要完成什麼工作，如果您的旅程牽涉到雲端，則每個小組都應該瞭解一些初步決策。 當您完成本指南時，請使用 [初始決策範本](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/references/initial-decisions-checklist.docx)記錄這些決策。 範本可協助您快速將參與雲端採用生命週期的團隊成員上線。

> [!NOTE]
> 選取下列任何連結，可能會讓您在 Azure 適用的 Microsoft 雲端採用架構的目錄中彈跳，尋找您稍後將用來協助小組執行相關指引的基本概念。 將此頁面加入書簽，以便經常返回此檢查清單。

## <a name="step-1-understand-how-azure-works"></a>步驟1：瞭解 Azure 的運作方式

如果您選擇 Azure 作為雲端提供者，以支援您的雲端採用旅程，請務必瞭解 [azure 的運作方式](./what-is-azure.md)。

**相關小組、交付專案和支援指引：**

參與雲端採用生命週期的每個人，都應該對 Azure 的運作方式有基本的瞭解。

## <a name="step-2-understand-initial-azure-concepts"></a>步驟2：瞭解初步的 Azure 概念

Azure 建基於一組 [基礎概念](../ready/considerations/fundamental-concepts.md) ，以深入瞭解 azure 實施的技術策略。

**相關小組、交付專案和支援指引：**

Azure 技術策略的所有相關人員都應該瞭解基礎概念中的詞彙和定義。

## <a name="step-3-review-the-portfolio"></a>步驟3：檢查組合

無論您選擇哪一種雲端提供者，所有雲端主機和環境決策都會先瞭解工作負載的組合。 雲端採用架構包含一些工具，可供您瞭解和評估組合。

**交付：**

- 記錄 [初始決策範本](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/references/initial-decisions-checklist.docx)中組合檔的位置、狀態及負責任人員。

**支援交付完成的指導方針：**

- 在開始雲端採用之前，[基本概念](../ready/considerations/fundamental-concepts.md)可協助您瞭解重要的 Azure 主題。
- [Operations management 活頁簿](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx)和企業調整方法可協助您瞭解已轉換至雲端營運團隊的工作負載和資產。
- [雲端採用方案](../plan/plan-intro.md)提供預定在雲端採用的工作負載和資產待處理專案（backlog）。
- [數位資產分析](../digital-estate/approach.md) 是一種方法，可記錄現有的工作負載，以及預定在雲端採用的資產。 在 Azure 中，數位資產最適合用稱為 [Azure Migrate](/azure/migrate/migrate-support-matrix)的工具表示。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端策略小組負責定義查看組合的方式。 | <li> 多個小組會使用下列指導方針來建立這些視圖。 參與雲端採用的每個人都應該知道在何處可以找到「公事包」視圖，以在程式中提供支援決策。 |

## <a name="step-4-define-portfolio-hierarchy-depth-to-align-the-portfolio"></a>步驟4：定義公事包階層深度以對齊組合

在雲端中裝載資產和工作負載可以很簡單，包括單一工作負載和其支援資產。 針對其他客戶，雲端採用策略可能包含數千個工作負載，以及更多支援的資產。 組合階層會提供每個層級的通用名稱，以協助建立組織的通用語言，而不論雲端提供者為何。

**交付：**

- 在 [初始決策範本](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/references/initial-decisions-checklist.docx)中記錄相關的階層需求。

**支援交付完成的指導方針：**

- 瞭解 [組合](../reference/fundamental-concepts/hosting-hierarchy.md) 階層的層級，以配合基本字詞。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端治理小組負責定義、強制執行組合階層，並將其自動化，以塑造雲端中的公司原則。 | <li> 參與雲端採用技術策略的每個人，都應該熟悉組合階層和目前使用的階層層級。 |

## <a name="step-5-establish-a-naming-and-tagging-standard-across-the-portfolio"></a>步驟5：建立跨組合的命名和標記標準

所有現有的工作負載和資產都應正確命名，並根據命名和標記標準來標記。 這些標準應該記載，並且可供所有小組成員參考。 可能的話，也應自動強制執行標準，以確保最小標記需求。

**交付：**

- 在 [初始決策範本](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/references/initial-decisions-checklist.docx)中，記錄命名和標記慣例活頁簿的位置、狀態和負責任的合作物件。

**支援交付完成的指導方針：**

- 建立 [命名和標記標準](../ready/azure-best-practices/naming-and-tagging.md)。
- 填入 [命名和標記慣例追蹤範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/ready/naming-and-tagging-conventions-tracking-template.xlsx) ，以追蹤決策。
- [檢查和更新 Azure 中的現有標記](/azure/azure-resource-manager/management/tag-resources)。
- [在 Azure 中強制執行標記原則](/azure/azure-resource-manager/management/tag-policies)。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端治理小組負責定義、強制執行和自動化命名和標記標準，以確保組合之間的一致性。 | <li> 牽涉到雲端採用技術策略的每個人，都應該先熟悉命名和標記標準，再部署至雲端。 |

## <a name="step-6-create-a-resource-organization-design-to-implement-the-portfolio-hierarchy"></a>步驟6：建立資源組織設計來實行組合階層

為了確保一致地配合組合階層決策，請務必建立資源組織的設計。 這類設計會將雲端提供者的組織工具與支援您的雲端採用計畫所需的組合階層進行對齊。 這項設計將藉由闡明哪些資產可以部署到雲端環境內的特定界限，來引導實行。

**交付：**

- 將 Azure 產品對應至 [初始決策範本](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/references/initial-decisions-checklist.docx)中組合階層的對齊層級。

**支援交付完成的指導方針：**

- 瞭解 [Azure 產品如何支援組合](../reference/fundamental-concepts/hierarchy-azure-tools.md)階層。
- 檢查現有的訂用帳戶，使其符合所選的組合階層。

建立訂用帳戶原則：

- [依設計開始使用兩個](../ready/azure-best-practices/initial-subscriptions.md)訂用帳戶。 新增基本訂用帳戶設計以考慮一般企業需求，例如共用服務或沙箱訂閱。
- 因為需要額外的訂用帳戶來支援雲端採用方案，所以[管理多個](../ready/azure-best-practices/organize-subscriptions.md)訂用帳戶。
- [根據組合階層建立明確的界限](../reference/fundamental-concepts/hierarchy-azure-tools.md#organizing-the-hierarchy-in-azure)。
- 必要時，請在訂用帳戶 [之間移動資源群組和資產](/azure/azure-resource-manager/management/move-resource-group-and-subscription) ，以遵守組織策略。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端治理小組負責定義、實行和自動化整個組合中的資源組織設計。 | <li> 牽涉到雲端採用技術策略的每個人，都應該先熟悉資源組織的設計，再部署至雲端。 |

## <a name="step-7-map-capabilities-teams-and-raci-to-fundamental-concepts"></a>步驟7：將功能、小組和 RACI 對應至基本概念

組合階層的複雜度將有助於通知組織結構和方法，以引導各小組的日常活動。

**交付：**

- 根據這些概念，完成組織對齊的快速入門手冊。

<!-- docsTest:ignore "Get started: Align your organization" -->

**支援交付完成的指導方針：**

- 使用先前的步驟作為指南，以評估 [組合階層責任指引](../reference/fundamental-concepts/hosting-hierarchy.md#hierarchy-accountability-and-guidance)。 判斷哪些功能可能需要由專用組織或虛擬團隊提供。
- 使用 [快速入門：讓您的組織符合](./org-alignment.md) RACI (負責、負責任、諮詢及通知的) 圖表。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端策略小組負責調整虛擬或專屬的組織結構，以確保雲端採用生命週期的成功。 | <li> 參與雲端採用生命週期的每個人，都應該熟悉人員和責任層級的一致性。 |

## <a name="whats-next"></a>後續步驟

在雲端採用架構的這一節中，透過一系列的快速入門手冊來建立這組基本概念。

> [!div class="nextstepaction"]
> [將基本概念套用至其他快速入門手冊](./index.md)
