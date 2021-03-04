---
title: 資源命名與標記決策指南
description: 了解要組織雲端式資源時，對資源的命名和標記方法和選項，以作為 Azure 雲端採用架構的一部分。
author: alexbuckgit
ms.author: abuck
ms.date: 02/11/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: decision-guide
ms.custom: internal
ms.openlocfilehash: b11f734353eb52e34da459ee9acdb172aebea030
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101790301"
---
# <a name="resource-naming-and-tagging-decision-guide"></a>資源命名與標記決策指南

組織雲端資源是 IT 的重要工作之一，除非您只需要簡單的部署。 基於下列原因，請使用命名和標記標準來組織您的資源：

- **資源管理：** 您的 IT 小組必須快速找出與特定工作負載、環境、擁有權群組，或其他重要資訊關聯的資源。 對於指派組織角色和存取權限以管理資源而言，組織支援至關重要。
- **成本管理和最佳化：** IT 人員必須了解每個小組正在使用哪些工作負載和資源，才能讓各業務群組留意到雲端資源使用狀況。 成本相關標籤支援下列主題：

  - [雲端帳戶處理模型](../../strategy/cloud-accounting.md)
  - [ROI 計算](../../strategy/financial-models.md#return-on-investment)
  - [成本追蹤](../../ready/azure-best-practices/track-costs.md)
  - [預算](/azure/cost-management-billing/costs/tutorial-acm-create-budgets?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)
  - [警示](/azure/cost-management-billing/costs/cost-mgt-alerts-monitor-usage-spending?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)
  - [週期性支出追蹤和報告](../../govern/cost-management/compliance-processes.md)
  - [實作後最佳化](../../govern/cost-management/discipline-improvement.md#operate-and-post-implementation)
  - [成本最佳化策略](../../govern/guides/complex/cost-management-improvement.md#incremental-improvement-of-best-practices)
- **作業管理：** 作業管理小組在商務承諾和 SLA 方面的可見度，是進行中作業的重要層面。 為了妥善管理，必須標記[任務重要性](../../manage/considerations/criticality.md)。
- **安全性：** 當漏洞或其他安全性問題發生時，資料和安全性影響的分類是小組的重要資料點。 若要安全地作業，必須標記[資料分類](../../govern/policy-compliance/data-classification.md)。
- **控管與法規合規性：** 維護資源間的一致性，有助於在已同意原則間找出歧異。 [資源標記的規範性指導](../../govern/guides/complex/prescriptive-guidance.md#resource-tagging) 方針會示範下列其中一種模式如何在部署治理實務時提供協助。 另有類似的模式可讓您使用標籤來評估法規合規性。
- **自動化：** 除了讓 IT 更容易管理資源之外，適當的組織配置也可讓您運用自動化功能自動建立資源、監視作業，以及建立 DevOps 程序。
- **工作負載最佳化：** 標記有助於識別模式並解決多種問題。 標籤也有助於識別支援單一工作負載所需的資產。 標記所有與每個工作負載相關聯的資產，可讓您更深入分析任務關鍵性工作負載，以制定良好的架構決策。

## <a name="tagging-decision-guide"></a>標記決策指南

![規劃符合下列快速連結的標記選項 (從簡單到最複雜)](../../_images/decision-guides/decision-guide-resource-tagging.png)

跳至：[基準命名慣例](#baseline-naming-conventions) | [資源標記模式](#resource-tagging-patterns) | [深入了解](#learn-more)

您的標記方法可以簡單或複雜，重點範圍包括從支援 IT 團隊管理雲端工作負載，到整合企業所有層面的相關資訊。

經 IT 人員調整的標記重點 (例如，依據工作負載、應用程式、功能或環境進行標記) 可降低監視資產時的複雜度，並能更輕鬆地依據營運需求做出管理決策。

包含業務協調焦點 (例如計量、業務所有權，或業務關鍵性) 的標記配置，可能需要投入更多時間來創建能夠反映商業利益的標記標準，並長期維持這些標準。 這項投資會產生一個標記系統，為整體業務的 IT 資產成本和價值提供改良的會計。 資產的商業價值和其營運成本之間的關聯，是在更大型的組織內改變成本中心對 IT 部門看法的其中一個優先步驟。

## <a name="baseline-naming-conventions"></a>基準命名慣例

標準化命名慣例是組織雲端裝載資源的起點。 正確結構化的命名系統可讓您基於管理和會計計量目的快速識別資源。 如果您組織的其他部門具已有現有的 IT 命名慣例，請考慮您是否應採用一致的雲端命名慣例，或是否應建立個別的雲端標準。

> [!NOTE]
> 每個 Azure 資源的[命名規則和限制](/azure/azure-resource-manager/management/resource-name-rules)各有不同。 您的命名慣例必須符合這些規則。

## <a name="resource-tagging-patterns"></a>資源標記模式

對於更複雜 (相較於一致命名慣例只能提供的) 組織而言，雲端平台可支援標記資源的能力。

標籤標記是附加至資源的中繼資料元素。 標記由成對的「索引鍵/值」字串組成。 這些成對字串中包含的值由您決定，但應用一組一致的全域標記 (作為綜合命名和標記原則) 的一部分，是整體治理原則不可或缺的一部分。

規劃程序時，請使用下列問題協助判斷您的資源標記需要支援的資訊類型：

- 您的命名和標記原則需要整合公司內現有的命名和組織原則嗎？
- 您是否要實作計費或回報會計系統？ 相較於簡單訂用帳戶層級明細允許的詳細程度，您是否必須針對部門、事業群和小組，採用更詳細的方式建立資源與計量資訊之間的關聯？
- 標記是否需要代表像是資源法規合規性要求這樣的詳細資料？ 或是是否需要代表像是運作時間要求、修補排程或安全性要求這樣的運作詳細資料？
- 哪些標記是在依據集中式 IT 原則的情況下，所有資源都需要的標記？ 哪些標記是選擇性的？ 是否允許個別團隊實作自己的自訂標記配置？

下面列出的常見標記模式可提供如何使用標記組織雲端資產的範例。 這些模式並獨佔模式，而且可並行使用，能夠依據公司需求提供多種資產組織方式。

<!-- cSpell:ignore catalogsearch northamerica jsmith contactalias catsearchowners businessprocess businessimpact revenueimpact -->

| 標記類型 | 範例 | 描述 |
|--|--|--|
| 函數 | `app` = `catalogsearch1` <br> `tier` = `web` <br> `webserver` = `apache` <br> `env` = `prod` <br> `env` = `staging` <br> `env` = `dev` | 根據在工作負載內的用途、部署位置的環境，或其他功能與運作詳細資料，將資源分類。 |
| 分類 | `confidentiality` = `private` <br> `SLA` = `24hours` | 可依據資源使用方式和對其套用的原則將資源分類。 |
| 會計 | `department` = `finance` <br> `program` = `business-initiative` <br> `region` = `northamerica` | 可允許針對帳單用途將資源與組織內的特定群組建立關聯。 |
| 合作關係 | `owner` = `jsmith` <br> `contactalias` = `catsearchowners` <br> `stakeholders` = `user1;user2;user3` | 可提供涉及哪些 (IT 之外的) 人員與資源相關或受其影響的相關資訊。 |
| 目的 | `businessprocess` = `support` <br> `businessimpact` = `moderate` <br> `revenueimpact` = `high` | 可將資源與業務功能相結合，為所做的投資選擇提供更妥善的支援。 |

## <a name="learn-more"></a>深入了解

如需有關 Azure 中命名和標記的詳細資訊，請參閱：

- [Azure 資源的命名慣例](../../ready/azure-best-practices/naming-and-tagging.md)。 請參閱本指南了解建議的 Azure 資源命名慣例。
- [使用標記來組織 Azure 資源和管理階層](/azure/azure-resource-manager/management/tag-resources)。 您可以在 Azure 中將標記套用在資源群組和個別資源層級，讓自己能夠根據套用的標記彈性調整會計報表的資料詳細程度。

## <a name="next-steps"></a>後續步驟

資源標記只是雲端採用程序期間，其中一個需針對架構做出決策的核心基礎結構元件。 請瀏覽架構決策指南概觀，以了解在為其他類型的基礎結構制定設計決策時，使用的替代模式或模型。

> [!div class="nextstepaction"]
> [架構相關決策指南](../index.md)
