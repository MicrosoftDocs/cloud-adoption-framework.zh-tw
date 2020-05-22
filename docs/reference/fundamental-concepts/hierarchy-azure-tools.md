---
title: Azure 產品如何支援組合階層？
description: Azure 產品如何支援組合階層？
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 17d6368eb8e0d55e8ad8107601a690046af1c2e9
ms.sourcegitcommit: 9a84c2dfa4c3859fd7d5b1e06bbb8549ff6967fa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2020
ms.locfileid: "83755626"
---
<!-- markdownlint-disable MD026 -->

# <a name="how-do-azure-products-support-the-portfolio-hierarchy"></a>Azure 產品如何支援組合階層？

在[瞭解和對齊組合](./hosting-hierarchy.md)階層時，組合階層和角色對應的一組定義會針對大部分的組合方法建立範圍的階層。 如該文章所述，您可能不需要每個大綱層級或_範圍_。 將層級的數目降到最低會降低複雜度，因此這些層不應全部視為需求。

本文說明 Azure 中的每個層級或範圍如何透過組織工具、部署和治理工具，以及適用于 Azure 的 Microsoft Cloud 採用架構中的一些解決方案來支援。

## <a name="organizing-the-hierarchy-in-azure"></a>在 Azure 中組織階層

Azure Resource Manager 包含數個組織方法，可協助您在雲端階層的每個層級組織資產。

下圖中的滑動條會示範對齊中的常見變化。 投影片的灰色部分是常見的，但只能用於特定的商務需求。 影像之後的點會描述建議的最佳作法。

![對應至階層的資源組織](../../_images/ready/hierarchy-with-organizing-tools.png)

- **組合：** 企業或業務單位可能不會包含任何技術資產，但可能會影響成本決策。 企業和業務單位會以管理群組階層的根節點來表示。
- **雲端平臺：** 在管理群組階層中，每個環境都有自己的節點。
- **登陸區域和雲端基礎：** 每個登陸區域都會表示為一個訂用帳戶。 同樣地，平臺基礎會包含在自己的訂用帳戶中。 有些訂用帳戶設計可能會針對每個雲端或每個工作負載呼叫訂用帳戶，這會變更每個雲端的組織工具。
- **工作負載：** 每個工作負載都是以資源群組來表示。 資源群組通常用來代表解決方案、部署或其他技術小組的資產。
- **資產：** 每個資產本身都是以 Azure 中的資源表示。

## <a name="organizing-with-tags"></a>使用標記進行組織

與最佳做法的偏差很常見。 您可以藉由標記所有資產來加以記錄。 使用標記來代表階層的每個相關層級。 如需詳細資訊，請參閱[建議的命名和標記慣例](../../ready/azure-best-practices/naming-and-tagging.md)。
