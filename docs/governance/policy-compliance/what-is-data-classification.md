---
title: 什麼是資料分類？
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 什麼是資料分類？
author: BrianBlanchard
ms.author: brblanch
ms.date: 02/11/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 2adebbe372df2e112f590182a72d6e0d7c10a6c5
ms.sourcegitcommit: a26c27ed72ac89198231ec4b11917a20d03bd222
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70824492"
---
<!-- markdownlint-disable MD026 -->

# <a name="what-is-data-classification"></a>什麼是資料分類？

資料分類可讓您判斷並指派組織資料的價值，而且是治理的常見起點。 資料分類程式會依敏感度和業務影響來分類資料，以找出風險。 一旦將資料分類之後，就可以用保護敏感或重要資料免于遭竊或遺失的方式進行管理。

## <a name="understand-data-risks-then-manage-them"></a>瞭解資料風險，然後加以管理

您必須先瞭解任何風險，才可進行管理。 對於資料安全性缺口責任，必須先了解資料分類。 資料分類是將中繼資料特性與數位資產中的每個資產建立關聯的過程，這能夠識別與該資產建立關聯的資料類型。

Microsoft 建議已識別將移轉或部署到雲端的任何潛在候選資產都應該記錄中繼資料，以便記錄資料分類、業務重要性和帳單責任。 這三個分類點對於了解和降低風險很有幫助。

## <a name="microsofts-data-classification"></a>Microsoft 的資料分類

下列是 Microsoft 使用的分類清單。 根據您的產業或現有安全性需求，組織中可能已存在資料分類標準。 如果沒有標準存在，我們邀請您使用此範例分類，以進一步瞭解您自己的數位資產和風險設定檔。

- **非企業：** 個人生活中不屬於 Microsoft 的資料。
- **公用︰** 免費提供並核准公開使用的商務資料。
- **一般：** 不適用於公用物件的商務資料。
- **機密：** 如果 overshared，可能會對 Microsoft 造成傷害的商務資料。
- **高度機密：** 如果 overshared，可能會對 Microsoft 造成大量傷害的商務資料。

## <a name="tagging-data-classification-in-azure"></a>在 Azure 中標記資料分類

每個雲端服務提供者都應提供用於記錄任何資產中繼資料的機制。 在 Azure 的案例中，資源標記是中繼資料儲存的建議方法，而且這些標記可用來將資料分類資訊套用至已部署的資源。 雖然以分類來標記雲端資產不是正式資料分類程式的替代方案，但它提供了管理資源和套用原則的重要工具。

如需 Azure 資源標記的詳細資訊，請參閱[使用標記來組織您的 Azure 資源](/azure/azure-resource-manager/resource-group-using-tags)一文。

## <a name="next-steps"></a>後續步驟

在其中一個可採取動作的治理指南中套用資料分類。

> [!div class="nextstepaction"]
> [選擇可採取動作的治理指南](../journeys/index.md)
