---
title: 商務調整：雲端管理和作業
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 商務調整-雲端管理和營運
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: deeb663a344bb3eb4e8ecc9af307da5ec1348b7a
ms.sourcegitcommit: bf9be7f2fe4851d83cdf3e083c7c25bd7e144c20
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/04/2019
ms.locfileid: "73565209"
---
# <a name="create-business-alignment-in-cloud-management"></a>在雲端管理中建立商務對齊

在內部部署環境中，IT 資產（應用程式、虛擬機器、VM 主機、磁片、伺服器、裝置和資料來源）由 IT 管理，以支援工作負載作業。 就 IT 方面而言，工作負載是可支援特定商務作業的 IT 資產集合。 為了協助支援商務營運，IT 管理提供了專為降低這些資產中斷而設計的處理常式。 當組織移到雲端時，管理和作業會轉移一些，以創造更緊密的商務調整。

## <a name="business-vernacular"></a>商務日常用語

建立商務對齊的第一個步驟是確保詞彙對齊。 IT 管理（例如大部分的工程職業相似性）已積累一系列的術語，或具有高度技術性的術語。 這類詞彙可能會對商務專案關係人造成混淆，並且難以將管理服務對應到商業價值。

幸運的是，開發雲端採用策略和雲端採用方案的程式，會建立一種重新對應這些詞彙的絕佳機會。 此程式也會建立機會來重新思考對營運管理的承諾，與企業合作。 下列文章系列會引導您完成這三個特定詞彙的新方法，協助改善商務專案關係人之間的交談： 

- **[重要性](./criticality.md)：** 將工作負載對應至商務程式。 排名重要性，以專注于投資。
- **[影響](./impact.md)：** 瞭解潛在中斷的影響，以協助評估雲端管理的投資報酬率。
- **[承諾](./commitment.md)用量：** 藉由建立及記載*與企業*簽訂的合約，來開發真正的合作關係。

> [!NOTE]
> 這些詞彙的基礎是傳統的 IT 術語，例如 SLA、RTO 和 RPO。 在[承諾](./commitment.md)用量文章中，將會更詳細地說明對應特定商務和 IT 詞彙。

## <a name="ops-management-planning-workbook"></a>Ops 管理規劃活頁簿

為了協助捕捉此交談針對詞彙對齊所產生的決策，我們的 GitHub 網站上提供[Ops 管理活頁簿](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx)。 此活頁簿不會執行 SLA 或成本計算。 它只是用來協助捕捉這類量值，並預測遺失避免的成果。

或者，如果解決方案已部署至雲端，則可以直接在 Azure 中標記這些相同的工作負載和相關聯的資產。

## <a name="next-steps"></a>後續步驟

藉由定義[工作負載重要性](./criticality.md)，開始建立商務對齊。

> [!div class="nextstepaction"]
> [定義工作負載重要性](./criticality.md)
