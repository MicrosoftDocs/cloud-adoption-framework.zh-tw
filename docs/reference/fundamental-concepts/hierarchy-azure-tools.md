---
title: Azure 產品如何支援組合階層？
description: Azure 產品如何支援組合階層？
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: c4ab25ee9cf9a72f6b5a5750157601510d5d050d
ms.sourcegitcommit: 60d8b863d431b5d7c005f2f14488620b6c4c49be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83229960"
---
<!-- markdownlint-disable MD026 -->

# <a name="how-do-azure-products-support-the-portfolio-hierarchy"></a>Azure 產品如何支援組合階層？

在[瞭解和對齊組合](./hosting-hierarchy.md)階層時，組合階層和角色對應的一組定義會針對大部分的組合方法建立範圍的階層。 如該文章所述，您可能不需要以下所述的每個層級或_範圍_。 將層級的數目降到最低會降低複雜度，因此這些層不應全部視為必要的最佳作法。

本文說明如何透過組織工具、部署和治理工具，以及雲端採用架構中的一些解決方案，在 Azure 中支援各層級或範圍。

## <a name="organizing-the-hierarchy-in-azure"></a>在 Azure 中組織階層

Azure Resource Manager 包含數個組織方法，可協助您在雲端階層的每個層級組織資產。

> [!NOTE]
> 下列滑動軸示範對齊中的常見變化。 投影片的灰色部分是常見的，但只有在特定商務需求需要時才會使用。 下圖後面的點僅描述建議的最佳作法。

![對應至階層的資源組織](../../_images/ready/hierarchy-with-organizing-tools.png)

- **組合：** 企業或業務單位可能不會包含任何技術資產，但可能會影響成本決策。 企業和業務單位應該會在管理群組階層的根節點中表示。
- **雲端平臺：** 每個環境都應該在管理群組階層中有自己的節點。
- **登陸區域和平臺基礎：** 每個登陸區域都應該表示為一個訂用帳戶。 同樣地，平臺基礎應該包含在自己的訂用帳戶中。 有各種不同的訂用帳戶設計，可針對每個雲端或每個工作負載呼叫訂用帳戶，這會變更每個雲端的組織工具。
- **工作負載：** 每個工作負載都應該以資源群組來表示。 資源群組通常也用來表示解決方案、部署或其他技術群組的資產。
- **資產：** 每個資產本身都是以 Azure 中的資源表示。

### <a name="organize-with-tags"></a>使用標記進行組織

最佳做法的變異很常見。 使用標籤來標記所有資產，以代表階層的每個相關層級，就應該記錄上述最佳做法的偏差。 如需詳細資訊，請參閱[建議的命名和標記慣例](../../ready/azure-best-practices/naming-and-tagging.md)。
