---
title: 雲端管理中的商務一致性
description: 使用適用于 Azure 的雲端採用架構，瞭解如何更妥善地管理您的雲端作業，並開發更緊密的商務調整。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 9b2d610942615c3f86e9e5cc0dbbb194d8192293
ms.sourcegitcommit: 412b945b3492ff3667c74627524dad354f3a9b85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/18/2020
ms.locfileid: "94879985"
---
# <a name="create-business-alignment-in-cloud-management"></a>在雲端管理中建立商務一致性

在內部部署環境中，IT 資產 (應用程式、虛擬機器、VM 主機、磁片、伺服器、裝置和資料來源) 由它管理，以支援工作負載作業。 在 IT 方面，工作負載是 IT 資產的集合，可支援特定的商務作業。 為了協助支援商務營運，IT 管理提供的流程是為了將這些資產的中斷情況降到最低。 當組織移至雲端時，管理和作業會稍微改變，創造出機會開發更緊密的商務調整。

## <a name="business-vernacular"></a>商務日常用語

建立商務對齊的第一個步驟是確保詞彙對齊。 IT 管理（如同大部分的工程職業相似性）已積累一系列的術語或高度技術性的條款。 這類詞彙可能會對商務專案關係人造成混淆，並使其難以將管理服務對應至商務價值。

幸運的是，開發雲端採用策略和雲端採用計畫的程式，會建立可重新對應這些條款的絕佳機會。 此程式也會創造機會，以與企業合作來重新思考對營運管理的承諾。 下列文章系列將逐步引導您完成三個特定詞彙的新方法，以協助改善商務專案關係人之間的交談：

- **[重要性](./criticality.md)：** 將工作負載對應至商務處理常式。 排名重要性以專注于投資。
- **[影響](./impact.md)：** 瞭解潛在中斷的影響，以協助評估雲端管理的投資報酬率。
- **[承諾](./commitment.md)用量：** 藉由建立及記錄企業的合約，來開發真正的合作關係。

> [!NOTE]
> 這些詞彙的基礎為傳統的 IT 詞彙，例如 SLA、RTO 和 RPO。 如需有關對應特定商務和 IT 詞彙的詳細資訊，請參閱 [雲端管理中的商務承諾](./commitment.md)。

## <a name="operations-management-workbook"></a>Operations management 活頁簿

為了協助您捕捉此交談有關詞彙對齊的決策，我們的 GitHub 網站上會提供 [operations management 活頁簿](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx) 。 此活頁簿不會執行 SLA 或成本計算。 它只是用來協助您捕獲這類量值，並根據避免的工作來預測報酬率。

或者，如果解決方案已部署至雲端，則可以直接在 Azure 中標記這些相同的工作負載和相關聯的資產。

## <a name="next-steps"></a>後續步驟

藉由定義 [工作負載的重要性](./criticality.md)，開始建立商務調整。

> [!div class="nextstepaction"]
> [定義工作負載重要性](./criticality.md)
