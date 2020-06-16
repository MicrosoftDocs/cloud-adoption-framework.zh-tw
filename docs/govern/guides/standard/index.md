---
title: 標準企業治理指南
description: 跟著虛構的標準企業經歷治理成熟度的各種階段，因為其會根據最佳做法定義最小可行產品 (MVP)。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/05/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: cd5e3d69709dc2fbbf9b79fcca3bf0efc63b021b
ms.sourcegitcommit: d88c1cc3597a83ab075606d040ad659ac4b33324
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/15/2020
ms.locfileid: "84787022"
---
# <a name="standard-enterprise-governance-guide"></a>標準企業治理指南

## <a name="overview-of-best-practices"></a>最佳做法概觀

這個治理指南會遵循虛構公司在治理成熟度各個階段的體驗， 並以實際的客戶體驗為基礎。 最佳做法則會以該虛構公司的條件約束和需求為主。

此概觀會根據最佳做法來定義治理的最簡可行產品 (MVP) 以作為快速起點。 它也提供一些治理改進方法的連結，這些改進會隨著新業務或技術風險的出現而進一步新增更多最佳做法。

> [!WARNING]
> 這個 MVP 是基於一組假設的基準起點。 即便是這一系列最基本的最佳做法，也是以獨特的業務風險和風險承受度推動的公司原則為基礎。 若要查看您是否適用這些假設，請閱讀本文後面[較長的敘述](./narrative.md)。

### <a name="governance-best-practices"></a>治理最佳做法

這些最佳做法可作為組織在多個訂用帳戶間快速且一致地新增治理防護的基礎。

### <a name="resource-organization"></a>資源組織

下圖顯示組織資源的治理 MVP 階層。

![資源組織圖](../../../_images/govern/resource-organization.png)

每個應用程式都應該在管理群組、訂用帳戶，以及資源群組階層的適當區域中部署。 在部署規劃期間，雲端治理小組將在階層中建立必要的節點，使雲端採用小組更具產能。

1. 每種類型環境 (例如生產、開發和測試) 都有一個管理群組。
2. 兩個訂用帳戶，一個供生產工作負載使用，另一個供非生產工作負載使用。
3. 應該在此群組階層的每個層級套用[一致的命名法](../../../ready/azure-best-practices/naming-and-tagging.md)。
4. 在部署資源群組時，應該要考慮其內容的生命週期：一起開發的所有項目要一起管理，並一起淘汰。 如需資源群組最佳做法的詳細資訊，請參閱[這裡](../../../decision-guides/resource-consistency/index.md)。
5. [區域選取](../../../migrate/azure-best-practices/multiple-regions.md)非常重要，因此必須納入考量，以備妥網路、監視、稽核來進行容錯移轉/容錯回復，並確認[所需的 SKU 可在偏好的區域中取得](https://azure.microsoft.com/global-infrastructure/services)。

以下是使用中的這種模式的一個範例：

![中型市場公司的資源組織範例](../../../_images/govern/mid-market-resource-organization.png)

這些模式會提供成長的空間，而不會不必要地使階層複雜化。

[!INCLUDE [governance-of-resources](../../../../includes/governance-of-resources.md)]

## <a name="iterative-governance-improvements"></a>反覆治理改進

一旦部署此 MVP 之後，其他治理層就可以快速地合併到環境中。 以下是一些改進 MVP 以符合特定業務需求的方式：

- [受保護資料的安全性基準](./security-baseline-improvement.md)
- [任務關鍵性應用程式的資源設定](./resource-consistency-improvement.md)
- [成本管理控制](./cost-management-improvement.md)
- [多重雲端演進的控制](./multicloud-improvement.md)

## <a name="what-does-this-guidance-provide"></a>本指南提供哪些內容？

在 MVP 中，從[部署加速](../../deployment-acceleration/index.md)專業領域建立做法和工具，是為了快速套用公司原則。 特別是，MVP 會使用 Azure 藍圖、Azure 原則以及 Azure 管理群組套用幾個基本的公司原則，如這個虛構公司的敘述中所定義。 那些公司原則會使用 Resource Manager 範本與 Azure 原則來套用，以建立小型的身分識別和安全性基準。

![累加式治理 MVP 的範例](../../../_images/govern/governance-mvp.png)

## <a name="incremental-improvement-of-governance-practices"></a>治理做法的累加式改進

經過一段時間之後，這個治理 MVP 將用於改進治理做法。 隨著採用率提高，業務風險也會增加。 雲端採用架構治理模型內的各種專業領域，將會持續變更以管理這些風險。 本系列的後續文章將討論影響虛構公司的公司原則累加式改進。 這些改進會跨三個專業領域進行：

- 成本管理 (採用擴大規模時)。
- 安全性基準 (部署受保護的資料時)。
- 資源一致性 (IT 操作開始支援任務關鍵性工作負載時)。

![累加式治理 MVP 的範例](../../../_images/govern/governance-improvement.png)

## <a name="next-steps"></a>後續步驟

現在您已經熟悉治理 MVP，並且了解要遵循的治理改進，請閱讀對於其他內容的支援敘述。

> [!div class="nextstepaction"]
> [閱讀支持性敘述](./narrative.md)
