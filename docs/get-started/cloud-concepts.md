---
title: 開始使用：記錄基礎對齊決策
description: 瞭解並記載推動雲端採用旅程所需的初始決策。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.openlocfilehash: 0c59a383f47629dd16cd81bc6865812fcaca4679
ms.sourcegitcommit: 60d8b863d431b5d7c005f2f14488620b6c4c49be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83229804"
---
<!-- docsTest:ignore CAF%20Readiness%20Naming%20and%20Tagging%20tracking%20template -->

# <a name="get-started-understand-and-document-foundational-alignment-decisions"></a>開始使用：瞭解並記載基本的對齊決策

雲端採用旅程可以透過各種旅程來解除鎖定許多商務、技術和組織的優勢。 無論您想要完成什麼動作，如果您的旅程牽涉到雲端，則每個小組都應該瞭解一些初步的決策。 當您逐步執行本指南時，請使用[初始決策範本](https://raw.githubusercontent.com/microsoft/cloudadoptionframework/master/references/initial-decisions-checklist.docx)來記錄這些決策，以快速將參與整體雲端採用生命週期的小組成員上架。

> [!NOTE]
> 按一下下列任何步驟，將會導致跳動雲端採用架構目錄，以涵蓋稍後用來協助小組執行相關指引的基本概念。 將此頁面加入書簽，通常會返回此檢查清單。

## <a name="step-1-understand-how-azure-works"></a>步驟1：瞭解 Azure 的運作方式

如果您已選擇 Azure 做為雲端提供者以支援您的雲端採用旅程，請務必瞭解[azure 的運作方式](./what-is-azure.md)。

**相關小組、交付專案和支援指引：**

所有參與雲端採用生命週期的人員，都應該對 Azure 的運作方式有基本的瞭解。

## <a name="step-2-understand-initial-azure-concepts"></a>步驟2：瞭解初始 Azure 概念

Azure 是以一組[基本概念為基礎](../ready/considerations/fundamental-concepts.md)，必須要有關于技術策略的深入討論，因為這與 azure 的開發特別相關。

**相關小組、交付專案和支援指引：**

所有與 Azure 實行技術策略相關的人員，都應該瞭解基本概念中的詞彙和定義。

## <a name="step-3-review-the-portfolio"></a>步驟3：檢查組合

不論所選擇的雲端提供者為何，所有雲端主機和環境決策都會從瞭解工作負載的整體組合開始著手。 雲端採用架構包含一些工具，可讓您瞭解和評估組合。

**項**

- 針對[初始決策範本](https://raw.githubusercontent.com/microsoft/cloudadoptionframework/master/references/initial-decisions-checklist.docx)中的組合檔，記錄位置、狀態及負責的人員。

**支援交付後完成的指引：**

- [基本概念](../ready/considerations/fundamental-concepts.md)可協助您在開始雲端採用之前，先瞭解重要的 Azure 主題。
- [Operations management 活頁簿](https://raw.githubusercontent.com/microsoft/cloudadoptionframework/master/manage/opsmanagementworkbook.xlsx)和商務調整方法可協助您瞭解已轉換至雲端作業小組的工作負載和資產。
- [雲端採用方案](../plan/plan-intro.md)提供工作負載和資產的待處理專案（backlog），並預定在雲端採用。
- [數位資產分析](../digital-estate/approach.md)是一種方法，用來記載預定在雲端採用的現有工作負載和資產。 在 Azure 中，數位資產最適合以稱為[Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-support-matrix)的工具表示。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端策略小組負責定義一種方式來查看組合。 | <li> 多個小組會使用下列指導方針來建立這些視圖。 所有與雲端採用相關的人員都應該知道哪裡可以找到「公事包」視圖，以支援稍後在程式中的決策。 |

## <a name="step-4-define-portfolio-hierarchy-depth-to-align-the-portfolio"></a>步驟4：定義組合-階層深度以對齊組合

在雲端中裝載資產和工作負載可以很簡單，其中包含單一工作負載及其支援的資產。 針對其他客戶，雲端採用策略可能包含數千個工作負載和更多支援的資產。 公事包階層會針對每個分層階層層級列出一般名稱，以協助建立組織的通用語言，而不論雲端提供者為何。

**項**

- 記錄[初始決策範本](https://raw.githubusercontent.com/microsoft/cloudadoptionframework/master/references/initial-decisions-checklist.docx)中的相關階層需求。

**支援交付後完成的指引：**

- 瞭解[組合](../reference/fundamental-concepts/hosting-hierarchy.md)階層的層級，以對齊基本詞彙。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端治理小組負責定義、強制執行和自動化組合階層，以塑造雲端中的公司原則。 | <li> 所有牽涉到雲端採用之技術策略的人員，都應該熟悉目前所使用之階層的組合階層和層級。 |

## <a name="step-5-establish-a-naming-and-tagging-standard-across-the-portfolio"></a>步驟5：建立跨組合的命名和標記標準

所有現有的工作負載和資產都應該適當地命名，並根據命名和標記標準來標記。 這些標準應記載並提供給所有小組成員參考。 可能的話，也應該自動強制執行標準，以確保最小的標記需求。

**項**

- 在[初始決策範本](https://raw.githubusercontent.com/microsoft/cloudadoptionframework/master/references/initial-decisions-checklist.docx)中，記錄命名和標記標準活頁簿的位置、狀態和合作方。

**支援交付後完成的指引：**

- 建立[命名和標記標準](../ready/azure-best-practices/naming-and-tagging.md)。
- 填入[命名和標記慣例追蹤活頁簿](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/CAF%20Readiness%20Naming%20and%20Tagging%20tracking%20template.xlsx)以追蹤決策。
- [在 Azure 中檢查並更新現有的標記](https://docs.microsoft.com/azure/azure-resource-manager/management/tag-resources)。
- [在 Azure 中強制執行標記原則](https://docs.microsoft.com/azure/azure-resource-manager/management/tag-policies)。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端治理小組負責定義、強制執行及自動化命名和標記標準，以確保組合間的一致性。 | <li> 在雲端採用的技術策略中牽涉到的所有人員，應該先熟悉命名和標記標準，再部署到雲端。 |

## <a name="step-6-create-a-resource-organization-design-to-implement-the-portfolio-hierarchy"></a>步驟6：建立資源組織設計以執行組合階層

為了確保符合組合階層決策的一致，請務必建立資源組織的設計。 這種設計會讓雲端提供者的組織工具與支援您的雲端採用方案所需的組合階層一致。 這項設計會藉由澄清哪些資產可以部署到雲端環境內的特定界限來進行引導。

**項**

- 在[初始決策範本](https://raw.githubusercontent.com/microsoft/cloudadoptionframework/master/references/initial-decisions-checklist.docx)中，將 Azure 產品對應至組合階層的對齊層級。

**支援交付後完成的指引：**

- 瞭解[Azure 產品如何支援組合](../reference/fundamental-concepts/hierarchy-azure-tools.md)階層。
- 檢查現有的訂用帳戶，以對齊所選的組合階層。

建立訂用帳戶原則：

- 從設計開始使用[兩個訂閱](../ready/azure-best-practices/initial-subscriptions.md)。
  - 新增基本訂用帳戶設計，以考慮常見的企業需求，例如共用服務或沙箱訂閱。
- [管理多個訂](../ready/azure-best-practices/organize-subscriptions.md)用帳戶，因為需要額外的訂閱以支援雲端採用方案。
- [根據組合階層建立清楚的界限](../reference/fundamental-concepts/hierarchy-azure-tools.md#organizing-the-hierarchy-in-azure)。
- 必要時，請在訂用帳戶[之間移動資源群組和資產](https://docs.microsoft.com/azure/azure-resource-manager/management/move-resource-group-and-subscription)，以符合組織策略。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端治理小組負責定義、實施和自動化整個組合的資源組織設計。 | <li> 在部署至雲端之前，所有牽涉到雲端採用技術策略的人員，都應該先熟悉資源組織設計。 |

## <a name="step-7-map-capabilities-teams-and-raci-to-fundamental-concepts"></a>步驟7：將功能、小組和 RACI 對應至基本概念

組合階層的複雜性會協助通知組織結構和方法，以引導各種小組的日常活動。

**項**

- 根據這些基本概念來完成組織對齊的快速入門手冊。

<!-- docsTest:ignore Get Align -->

**支援交付後完成的指引：**

- 使用先前的步驟做為評估[組合階層責任指引](../reference/fundamental-concepts/hosting-hierarchy.md#hierarchy-accountability-and-guidance)的指南，以判斷哪些功能可能需要由專用的組織或虛擬小組傳遞。
- 使用[入門：讓您的組織](./org-alignment.md)能夠將組合階層責任指引套用到 RACI 圖表。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端策略小組負責協調虛擬或專用的組織結構，以確保整體雲端採用週期的成功。 | <li> 與雲端採用週期相關的所有人員，都應該熟悉個人和責任層級的對齊方式。 |

## <a name="whats-next"></a>後續步驟

透過雲端採用架構這一節中的《快速入門手冊》一系列，建立這組基本概念。

> [!div class="nextstepaction"]
> [將基本概念套用至其他使用者入門指南](./index.md)
