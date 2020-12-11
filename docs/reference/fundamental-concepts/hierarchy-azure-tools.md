---
title: Azure 產品如何支援組合階層？
description: Azure 產品如何支援組合階層？
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: internal
ms.openlocfilehash: dbd0326bf8e0436cab6072c3fd943d725b496f44
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97025855"
---
# <a name="how-do-azure-products-support-the-portfolio-hierarchy"></a>Azure 產品如何支援組合階層？

在 [瞭解並對齊組合](./hosting-hierarchy.md)階層時，一組組合階層和角色對應的定義會針對大部分的組合方法建立範圍階層。 如該文章所述，您可能不需要每個大綱層級或 _範圍_。 將圖層數目降到最低可減少複雜度，因此不應該將這些圖層視為需求。

本文說明 Azure 中的每個層級或階層範圍如何透過組織工具、部署和治理工具，以及適用于 Azure 的 Microsoft 雲端採用架構中的一些解決方案來支援。

## <a name="organizing-the-hierarchy-in-azure"></a>在 Azure 中組織階層

Azure Resource Manager 包含數個組織方法，可協助組織雲端階層各層級的資產。

下圖中的滑動軸會示範對齊的一般變化。 投影片的灰色部分很常見，但只能用於特定的商務需求。 影像之後的點會描述建議的最佳做法。

![對應到階層的資源組織](../../_images/ready/hierarchy-with-organizing-tools.png)

- **組合：** 企業或營業單位可能不會包含任何技術資產，但可能會影響成本決策。 企業和企業單位會以管理群組階層的根節點表示。
- **雲端平臺：** 每個環境在管理群組階層中都有自己的節點。
- **登陸區域和雲端基礎：** 每個登陸區域都會以訂用帳戶表示。 同樣地，平臺基礎也包含在其自己的訂用帳戶中。 某些訂用帳戶設計可能會針對每個雲端或每個工作負載呼叫訂用帳戶，這會變更每個雲端的組織工具。
- **工作負載：** 每個工作負載都會表示為資源群組。 資源群組通常用來代表解決方案、部署或資產的其他技術群組。
- **資產：** 每個資產原本都會表示為 Azure 中的資源。

## <a name="organizing-with-tags"></a>使用標記進行組織

與最佳作法的偏差很常見。 您可以藉由標記所有資產來記錄它們。 使用標記來代表階層的每個相關層級。 如需詳細資訊，請參閱 [建議的命名和標記慣例](../../ready/azure-best-practices/naming-and-tagging.md)。
