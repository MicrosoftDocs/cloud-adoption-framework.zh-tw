---
title: 資源命名與標記決策指南
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 了解在 Azure 移轉中作為核心服務的資源組織與標記。
author: alexbuckgit
ms.author: abuck
ms.date: 02/11/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: decision-guide
ms.custom: governance
ms.openlocfilehash: e02fa26454c4e10af7acae80bbe3b89e8ccaee84
ms.sourcegitcommit: 617c3f12a3657a8a1393fd08d261dd98eb81b65c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/14/2019
ms.locfileid: "74086817"
---
# <a name="resource-naming-and-tagging-decision-guide"></a>資源命名與標記決策指南

組織雲端資源是 IT 其中一個最重要的工作，除非您只需要簡單的部署。 組織您的資源有三種主要的用途：

- **資源管理：** 您的 IT 小組必須快速找出與特定工作負載、環境、擁有權群組，或其他重要資訊關聯的資源。 對於指派組織角色和存取權限以管理資源而言，組織支援至關重要。
- **自動化：** 除了讓 IT 更容易管理資源之外，適當的組織配置也可讓您運用自動化功能自動建立資源、監視作業，以及建立 DevOps 程序。
- **計量：** IT 人員必須了解哪些工作負載與團隊正在使用哪些資源，才能讓各業務群組留意到雲端資源使用狀況。 若要支援像計費和回報會計這樣的方法，需組織雲端資源以反映擁有權和使用量。

## <a name="tagging-decision-guide"></a>標記決策指南

![規劃符合下列快速連結的標記選項 (從最簡單到最複雜)](../../_images/decision-guides/decision-guide-resource-tagging.png)

跳至：[基準命名慣例](#baseline-naming-conventions) | [資源標記模式](#resource-tagging-patterns) | [深入了解](#learn-more)

您的標記方法可以簡單或複雜，重點範圍包括從支援 IT 團隊管理雲端工作負載，到整合企業所有層面的相關資訊。

IT 會協調標記重點，例如依據工作負載、功能或環境進行標記，這將能降低監視資產時的複雜度，並能更輕鬆地依據營運需求做出管理決策。

包含業務協調焦點 (例如計量、業務所有權，或業務關鍵性) 的標記配置，可能需要投入更多時間來創建能夠反映商業利益的標記標準，並長期維持這些標準。 不過，此程序的結果會是能夠提供增強能力，可將 整體企業的 IT 資產成本與價值納入考量的標記系統。 資產的商業價值和其營運成本之間的關聯，是在更大型的組織內改變成本中心對 IT 部門看法的其中一個優先步驟。

## <a name="baseline-naming-conventions"></a>基準命名慣例

標準化命名慣例是組織雲端裝載資源的起點。 正確結構化的命名系統可讓您基於管理和會計計量目的快速識別資源。 如果您組織的其他部門具已有現有的 IT 命名慣例，請考慮您是否應採用一致的雲端命名慣例，或是否應建立個別的雲端標準。

也請注意，不同的 Azure 資源類型具有不同[命名需求](../../ready/azure-best-practices/naming-and-tagging.md)。 您的命名慣例必須與這些命名需求相容。

## <a name="resource-tagging-patterns"></a>資源標記模式

對於更複雜 (相較於一致命名慣例只能提供的) 組織而言，雲端平台可支援標記資源的能力。

*標記*是附加至資源的中繼資料元素。 標記由成對的「索引鍵/值」字串組成。 這些成對字串中包含的值由您決定，但應用一組一致的全域標記 (作為綜合命名和標記原則) 的一部分，是整體治理原則不可或缺的一部分。

規劃程序時，請使用下列問題協助判斷您的資源標記需要支援的資訊類型：

- 您的命名和標記原則需要整合公司內現有的命名和組織原則嗎？
- 您是否要實作計費或回報會計系統？ 相較於簡單訂用帳戶層級明細允許的詳細程度，您是否必須針對部門、事業群和小組，採用更詳細的方式建立資源與計量資訊之間的關聯？
- 標記是否需要代表像是資源法規合規性要求這樣的詳細資料？ 或是是否需要代表像是運作時間要求、修補排程或安全性要求這樣的運作詳細資料？
- 哪些標記是在依據中央 TI 原則的情況下，所有資源都需要的標記？ 哪些標記是選擇性的？ 是否允許個別團隊實作自己的自訂標記配置？

下面列出的常見標記模式可提供如何使用標記組織雲端資產的範例。 這些模式並獨佔模式，而且可並行使用，能夠依據公司需求提供多種資產組織方式。

<!-- markdownlint-disable MD033 -->

| 標記類型 | 範例 | 說明 |
|-----|-----|-----|
| 函數            | app = catalogsearch1 <br/>tier = web <br/>webserver = apache<br/>env = prod <br/>env = staging <br/>env = dev                 | 根據在工作負載內的用途、部署位置的環境，或其他功能與運作詳細資料，將資源分類。                                 |
| 分類        | confidentiality=private<br/>sla = 24hours                                 | 可依據資源使用方式和對它套用的原則將資源分類                               |
| 會計            | department = finance <br/>project = catalogsearch <br/>region = northamerica | 可允許針對帳單用途將資源與組織內的特定群組建立關聯 |
| 合作關係           | owner = jsmith <br/>contactalias = catsearchowners<br/>stakeholders = user1;user2;user3<br/>                       | 可提供涉及哪些 (IT 之外的) 人員與資源相關或受它影響的相關資訊                      |
| 目的               | businessprocess=support<br/>businessimpact=moderate<br/>revenueimpact=high   | 可將資源與業務功能相結合，為所做的投資選擇提供更妥善的支援  |

<!-- markdownlint-enable MD033 -->

## <a name="learn-more"></a>深入了解

如需有關 Azure 中命名和標記的詳細資訊，請參閱：

- [Azure 資源的命名慣例](/azure/architecture/best-practices/resource-naming)。 請參閱本指南了解建議的 Azure 資源命名慣例。
- [使用標記來組織 Azure 資源](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags?toc=/azure/billing/TOC.json)。 您可以在 Azure 中將標記套用在資源群組和個別資源層級，讓自己能夠根據套用的標記彈性調整會計報表的資料詳細程度。

## <a name="next-steps"></a>後續步驟

資源標記只是雲端採用程序期間，其中一個需針對架構做出決策的核心基礎結構元件。 請瀏覽[決策指南概觀](../index.md)以了解針對其他基礎架構類型訂定決策時，所使用的替代模式或模型。

> [!div class="nextstepaction"]
> [架構相關決策指南](../index.md)
