---
title: 商務調整-雲端管理和營運
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 商務調整-雲端管理和營運
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 9c6884d57b9238f58921b14ec3ddbbe3623506af
ms.sourcegitcommit: 35c162d2d09ec1c4a57d3d57a5db1d56ee883806
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2019
ms.locfileid: "72558138"
---
# <a name="create-business-alignment-in-cloud-management"></a>在雲端管理中建立商務對齊

在內部部署環境中，IT 資產（應用程式、虛擬機器、VM 主機、磁片、伺服器、裝置和資料來源）由 IT 管理，以支援工作負載作業。 就 IT 方面而言，工作負載是可支援特定商務作業的 IT 資產集合。 在內部部署環境中，IT 管理提供了程式設計，可將這些 IT 資產的中斷降到最低，以儘量減少對支援業務營運的中斷。 當您移至雲端時，管理和作業會轉移一些時間，創造出更緊密的商務調整的機會。

## <a name="business-vernacular"></a>商務日常用語

建立商務對齊的第一個步驟是詞彙對齊。 IT 管理（例如大部分的工程職業相似性）包含相當多的術語或高度的技術術語。 這些詞彙可能會對商務專案關係人造成混淆，並且難以將管理服務對應到商業價值。

幸運的是，開發雲端採用策略和雲端採用方案的程式，會建立詞彙重新對應的概念。 它也會在與業務合作的關係下，建立重新思考承諾與營運管理的絕佳機會。 下列文章系列會逐步解說三個特定詞彙的這種新方法，可以改善與商務專案關係人的交談：

- **[重要性](./criticality.md)** ：將工作負載對應至商務處理常式。 排名重要性，以專注于投資。
- **[影響](./impact.md)** ：瞭解潛在中斷的影響，以協助評估雲端管理的投資報酬率。
- **[承諾](./commitment.md)** ：藉由建立及記載**與企業**簽訂的合約，來開發真正的合作關係。

> [!NOTE]
> 每個詞彙底下都是傳統的 IT 術語，例如 SLA、RTO 和 RPO。 對應商務和 IT 詞彙會在承諾用量文章中更詳細地涵蓋。

## <a name="ops-management-planning-workbook"></a>Ops 管理規劃活頁簿

為了協助捕捉此交談的結果，我們的 GitHub 存放庫中提供[Ops 管理活頁簿](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx)。 此活頁簿不會執行 SLA 或成本計算。 它只是用來捕捉這些量值的指南，並預測遺失避免工作的報酬。

或者，如果解決方案已部署至雲端，則可以直接在 Azure 中標記這些相同的工作負載和相關聯的資產。

## <a name="next-steps"></a>後續步驟

藉由定義[工作負載重要性](./criticality.md)，開始建立商務對齊。

> [!div class="nextstepaction"]
> [定義工作負載重要性](./criticality.md)
