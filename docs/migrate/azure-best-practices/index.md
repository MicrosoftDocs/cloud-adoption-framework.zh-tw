---
title: Azure 雲端移轉最佳做法檢查清單
description: 探索 Azure 雲端移轉檢查清單，了解如何實作雲端移轉最佳做法搭配使用的 Azure 工具。
keywords: azure 雲端移轉最佳做法, azure 移轉檢查清單, 雲端移轉檢查清單, 雲端移轉最佳做法
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: seo-azure-migrate
ms.openlocfilehash: 81f5fe9bf3f67031ffc35caf960009f0c3491c2c
ms.sourcegitcommit: 580a6f66a0d0f3f5b755c68d757a84b2351a432f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/31/2020
ms.locfileid: "87472786"
---
# <a name="azure-cloud-migration-best-practices-checklist"></a>Azure 雲端移轉最佳做法檢查清單

如果您想遷移至 Azure，建議先從雲端採用架構的 [Azure 移轉指南](../azure-migration-guide/index.md)開始。 該指南能引導您使用一組工具和基本方法，將虛擬機器遷移至雲端。

下列檢查清單提供了 Azure 雲端移轉最佳做法，且不僅僅是基本的雲端原生工具。 下文會概述常見的複雜領域區域，移轉範圍可能會超出 [Azure 移轉指南](../azure-migration-guide/index.md)涵蓋範圍。

## <a name="migration-best-practices-for-business-driven-scope-expansion"></a>擴大業務導向範圍的移轉最佳做法

- **[支援全球市場](./multiple-regions.md)：** 業務是在多個地理區域中營運，其皆具有不同的資料主權需求。 為了符合那些需求，您應該將額外考量納入必要條件檢閱及移轉期間的資產發佈。

## <a name="migration-best-practices-for-technology-driven-scope-expansion"></a>擴大技術導向範圍的移轉最佳做法

- **[VMWare 移轉](./vmware-host.md)：** 遷移 VMWare 主機可以加速整體的移轉流程。 所遷移的每個 VMWare 主機都可以將多個工作負載移至雲端。 在移轉之後，這些 VM 和工作負載可以留在 VMWare 中，也可以遷移至新式雲端功能。
- **[SQL Server 移轉](./sql-migration.md)：** 遷移 SQL Server 的執行個體可以加速整體的移轉進程。 所遷移的每個執行個體都可以移動多個資料庫和服務，而可能加快多個工作負載的速度。
- **[多個資料中心](./multiple-datacenters.md)：** 遷移多個資料中心會大幅提高複雜性。 我們將會在每個移動程序期間 (評量、遷移、最佳化及管理) 討論其他考量，以針對更複雜的環境做好準備。
- **[資料需求超過網路容量](./network-capacity-exceeded.md)：** 公司經常會因現有資料中心的容量、速度或穩定性已不再令人滿意，而選擇移轉至雲端。 不幸的是，那些相同的條件約束都會為移轉程序帶來複雜度，並需要在評量和移轉程序期間進行額外的規劃。
- **[治理或合規性策略](./governance-or-compliance.md)：** 若治理和合規性是移轉成功與否的關鍵，則 IT 治理小組和雲端採用小組便需要取得進一步的共識。

## <a name="additional-migration-best-practices"></a>其他移轉最佳做法

- [針對遷移至 Azure 的工作負載來設定網路](./migrate-best-practices-networking.md)
- [部署移轉基礎結構](./contoso-migration-infrastructure.md)
- [針對遷移到 Azure 的成本及大小工作負載](./migrate-best-practices-costs.md)
- [對 Azure 進行大規模移轉](./contoso-migration-scale.md)

## <a name="next-steps"></a>後續步驟

以下是研究 Azure 移轉最佳做法，相當好的起點。

> [!div class="nextstepaction"]
> [多個資料中心](./multiple-datacenters.md)
