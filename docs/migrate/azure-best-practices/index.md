---
title: Azure 移轉最佳做法
description: 使用適用於 Azure 的雲端採用架構，了解如何實作符合雲端移轉最佳做法的必要工具。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: a7affd1d64e80cfdf85504ed62960de78a4a34a5
ms.sourcegitcommit: 88fbc36cd634c3069e1a841a763a5327c737aa84
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/03/2020
ms.locfileid: "80636402"
---
# <a name="best-practices-for-cloud-migration"></a>雲端移轉的最佳做法

如果您想遷移至 Azure，建議您以雲端採用架構中的 [Azure 移轉指南](../azure-migration-guide/index.md)作為起點。 該指南能引導您使用一組工具和基本方法，將虛擬機器遷移至雲端。 雲端採用架構的這一節，將說明許多超越基本雲端原生工具的最佳做法。

## <a name="cloud-migration-best-practices-checklist"></a>雲端移轉最佳做法檢查清單

下列檢查清單會概述常見的複雜度區域，其可能會需要將移轉範圍擴充至 [Azure 移轉指南](../azure-migration-guide/index.md)的範圍之外。

### <a name="business-driven-scope-expansion"></a>業務導向範圍擴充

- **[支援全球市場](./multiple-regions.md)：** 業務是在多個地理區域中營運，其皆具有不同的資料主權需求。 為了符合那些需求，應該將額外考量納入必要條件檢閱及移轉期間的資產發佈。

### <a name="technology-driven-scope-expansion"></a>技術導向範圍擴充

- **[VMWare 移轉](./vmware-host.md)：** 遷移 VMWare 主機可以加速整體的移轉流程。 所遷移的每個 VMWare 主機都可以使用隨即轉移方法，將多個工作負載移至雲端。 在移轉之後，這些 VM 和工作負載可以留在 VMWare 中，也可以遷移至新式雲端功能。
- **[SQL Server 移轉](./sql-migration.md)：** 遷移 SQL Server 可以加速整體的移轉進程。 所遷移的每個 SQL Server 都可以移動多個資料庫和服務，而可能加快多個工作負載的速度。
- **[多個資料中心](./multiple-datacenters.md)：** 遷移多個資料中心會大幅提高複雜性。 我們將會在評量、遷移、最佳化及管理程序期間討論其他考量，以針對更複雜的環境做好準備。
- **[資料需求超過網路容量](./network-capacity-exceeded.md)：** 公司經常會因現有資料中心的容量、速度或穩定性已不再令人滿意，而選擇移轉至雲端。 不幸的是，那些相同的條件約束都會為移轉程序帶來複雜度，並需要在評量和移轉程序期間進行額外的規劃。
- **[治理或合規性策略](./governance-or-compliance.md)：** 若治理和合規性是移轉成功與否的關鍵，IT 治理小組和雲端採用小組之間便需要取得進一步的共識。

如果您的案例中有上述任何一項複雜性，則＜雲端採用架構＞中的本節很可能可以為您提供在移轉程序中適當對齊範圍所需的指引類型。

每個案例皆會由＜雲端採用架構＞中本節的數篇文章所討論。

## <a name="next-steps"></a>後續步驟

請瀏覽左側目錄以解決特定需求或範圍變更。 此外，清單上的第一個範圍強化：[支援全球市場](./multiple-regions.md)，是檢閱這些案例的理想出發點。

> [!div class="nextstepaction"]
> [支援全球市場](./multiple-regions.md)
