---
title: 適用于 Azure 的 Microsoft 雲端採用架構檔
description: 取得工具、指引和敘述，協助塑造策略，並在雲端採用生命週期的所有階段推動所需的業務成果。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.custom: homepage
ms.openlocfilehash: 1e2c1cb95e80ba5aa4926807ef505d748013e060
ms.sourcegitcommit: 8b82889dca0091f3cc64116f998a3a878943c6a1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/09/2020
ms.locfileid: "89604218"
---
# <a name="what-is-the-microsoft-cloud-adoption-framework-for-azure"></a>什麼是適用於 Azure 的 Microsoft 雲端採用架構？

適用于 Azure 的 Microsoft 雲端採用架構是經過證實的指導方針，其設計目的是協助您建立及實行您的組織在雲端中成功所需的商務和技術策略。 它提供雲端架構設計人員、IT 專業人員和業務決策者成功達成短期和長期目標所需的最佳作法、檔和工具。

藉由使用適用于 Azure 最佳做法的 Microsoft 雲端採用架構，組織可以更妥善地調整其商務和技術策略，以確保成功。 請觀看下列影片以深入了解。

<!-- markdownlint-disable MD034 -->

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE4tyzr]

<!-- markdownlint-enable MD034 -->

雲端採用架構彙集了 Microsoft 員工、合作夥伴和客戶的雲端採用最佳作法。 它提供一組工具、指引和敘述，協助塑造技術、業務和人員策略，以在採用工作期間推動所需的業務成果。 本指南適用于雲端採用生命週期的下列階段，可讓您在適當的時間輕鬆存取正確的指引。

| <span title="圖示">&nbsp;</span> | <span title="描述">&nbsp;</span> | <span title="圖示">&nbsp;</span> | <span title="描述">&nbsp;</span> |
|--|--|--|--|
| <br> ![策略圖示](./_images/icons/strategy.png) | <br> [策略](./strategy/index.md)： &nbsp; 定義 &nbsp; 商業 &nbsp; 理由 &nbsp; 和預期的採用結果。 | <br> ![規劃圖示](./_images/icons/plan.png) | <br> [方案](./plan/index.md)： &nbsp; &nbsp; 將可採取動作的 &nbsp; 採用 &nbsp; 方案與業務成果保持一致。 |
| <br> ![就緒圖示](./_images/icons/ready.png)       | <br> [就緒](./ready/index.md)：準備雲端環境以進行已規劃的變更。 | <br> ![遷移圖示](./_images/icons/adopt.png) | <br> [遷移](./migrate/index.md)：遷移和現代化現有的工作負載。 |
| <br> ![創新圖示](./_images/icons/innovate.png) | <br> [創新](./innovate/index.md)：開發新的雲端原生或混合式解決方案。 | <br> ![管理圖示](./_images/icons/govern.png) | <br> 治理[：管理](./govern/index.md)環境和工作負載。 |
| <br> ![管理圖示](./_images/icons/manage.png)     | <br> [管理](./manage/index.md)：雲端和混合式解決方案的作業管理。 | <br> ![組織圖示](./_images/icons/organize.png) | <br> [組織](./organize/index.md)：管理環境和工作負載。 |

## <a name="understand-the-lifecycle"></a>了解生命週期

上述每個方法都屬於廣泛的雲端採用生命週期。 雲端採用架構是一種完整的生命週期架構，可在每個採用階段支援客戶，方法是提供方法作為克服常見封鎖的特定方法，如下所示。

![雲端採用架構的總覽](./_images/caf-overview-new.png)

## <a name="intent"></a>Intent

雲端徹底改變企業採購、使用和保護技術資源的方式。 傳統上，企業要承擔從基礎結構到軟體等所有技術層面的擁有權和責任。 移至雲端可讓企業只在需要時才佈建及取用資源。 雖然雲端對於設計選擇提供大量彈性，但企業需要經過實證且一致的方法來採用雲端技術。 適用於 Azure 的 Microsoft 雲端採用架構符合該需求，它能協助指引進行雲端採用的決策。

但雲端採用是一種結束的途徑。 成功的雲端採用開始於選定雲端平台廠商之前。 當業務和 IT 部門的決策者意識到雲端可以加速特定的業務轉型目標時，雲端採用就已開始。 雲端採用架構有助於協調商務、文化和技術變革的策略，以達成所需的業務成果。

雲端採用架構提供適用於 Microsoft Azure 的技術指導方針。 由於企業客戶可能仍在選擇雲端廠商或可能有刻意多重雲端策略，因此，此架構會盡可能針對策略性決策提供雲端中立的指引。

## <a name="intended-audience"></a>目標對象

此指導方針會影響企業的業務、技術和文化。 受影響的角色包括企業營運主管、業務決策者、IT 決策者、財務、企業管理員、IT 作業、IT 安全性和合規性、IT 治理、工作負載開發擁有者和工作負載作業擁有者。 每個角色都會使用自己的專業詞彙，且擁有不同的目標及關鍵效能指標。 單一一組內容無法有效地適用於所有對象。

進入 *雲端架構師*。 雲端架構設計師可引導並促使這些對象共同合作。 我們設計了本指南集合，協助雲端架構設計師促進與正確物件進行正確的交談，並推動決策制定。 雲端所提供的業務轉型取決於雲端架構設計人員角色，以協助引導整個業務和 IT 的決策。

雲端採用架構的每個小節代表雲端架構設計師角色的不同專業化或樣貌。 這些小節也製造在雲端架構設計師小組中分擔雲端架構責任的機會。 例如，治理區段是針對具有緩和技術風險的雲端架構設計人員所設計。 有些雲端提供者將這些專家稱為雲端監管者;我們偏好雲端守護 *者* ，或統稱為 *雲端治理小組*。

## <a name="how-to-use-the-microsoft-cloud-adoption-framework-for-azure"></a>如何使用適用於 Azure 的 Microsoft 雲端採用架構

如果您的企業是 Azure 的新手，請先 [瞭解並記載基本的調整決策](./get-started/cloud-concepts.md)。 當您企業的數位轉型牽涉到雲端時，瞭解這些基本概念將可在程式的每個步驟中協助您。

<!-- docsTest:ignoreNextStep -->

> [!div class="nextstepaction"]
> [開始使用](./get-started/index.md)
