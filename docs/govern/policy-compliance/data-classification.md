---
title: 什麼是資料分類？
description: 資料分類可讓您判斷並指派組織資料的價值，並提供共同管理的起點。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: ffa318f44a6fe3a856598581ca7cc2de37fbd6f5
ms.sourcegitcommit: 7d3fc1e407cd18c4fc7c4964a77885907a9b85c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "80997370"
---
<!-- markdownlint-disable MD026 -->

# <a name="what-is-data-classification"></a>什麼是資料分類？

資料分類可讓您判斷並指派組織資料的價值，並提供共同管理的起點。 資料分類程式會依敏感度和業務影響來分類資料，以找出風險。 當資料經過分類時，您可以用保護敏感或重要資料的方式來管理它，以免遭竊或遺失。

## <a name="understand-data-risks-then-manage-them"></a>瞭解資料風險，然後加以管理

您必須先瞭解任何風險，才可進行管理。 對於資料安全性缺口責任，必須先了解資料分類。 資料分類是將中繼資料特性與數位資產中的每個資產產生關聯的程式，以識別與該資產相關聯的資料類型。

任何識別為遷移或部署至雲端之潛在候選的資產，都應具有記載的中繼資料，以記錄資料分類、業務重要性和計費責任。 這三個分類點對於了解和降低風險很有幫助。

## <a name="classifications-microsoft-uses"></a>Microsoft 使用的分類

下列是 Microsoft 使用的分類清單。 根據您的產業或現有的安全性需求，資料分類標準可能已存在於您的組織內。 如果沒有標準，您可能會想要使用此範例分類，以進一步瞭解您自己的數位資產和風險設定檔。

- **非企業：** 個人生活中不屬於 Microsoft 的資料。
- **公用：** 免費提供並核准公開使用的商務資料。
- **一般：** 不適用於公用物件的商務資料。
- **機密：** 如果 overshared，可能會對 Microsoft 造成傷害的商務資料。
- **高度機密：** 如果 overshared，可能會對 Microsoft 造成大量傷害的商務資料。

## <a name="tagging-data-classification-in-azure"></a>在 Azure 中標記資料分類

資源標記是用來儲存中繼資料的好方法，而且您可以使用這些標記將資料分類資訊套用至已部署的資源。 雖然以分類來標記雲端資產不是正式資料分類程式的替代方案，但它提供了管理資源和套用原則的重要工具。 [Azure 資訊保護](https://docs.microsoft.com/azure/information-protection/what-is-information-protection)是一個絕佳的解決方案，可協助您將_資料_本身分類，不論其位於何處（內部部署、Azure 或其他位置）。 請將它視為整體分類策略的一部分。

如需 Azure 中資源標記的詳細資訊，請參閱[使用標記來組織您的 azure 資源](https://docs.microsoft.com/azure/azure-resource-manager/management/tag-resources)。

## <a name="next-steps"></a>後續步驟

在其中一個可採取動作的治理指南中套用資料分類。

> [!div class="nextstepaction"]
> [選擇可採取動作的治理指南](../guides/index.md)
