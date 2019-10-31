---
title: 適用於複雜企業的治理指南
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 適用於複雜企業的治理指南
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: be75f4315d90c09c277846f8608ca0552f1d9853
ms.sourcegitcommit: e0a783dac15bc4c41a2f4ae48e1e89bc2dc272b0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2019
ms.locfileid: "73058694"
---
# <a name="governance-guide-for-complex-enterprises"></a>適用於複雜企業的治理指南

## <a name="overview-of-best-practices"></a>最佳做法概觀

這個治理指南會遵循虛構公司在治理成熟度各個階段的體驗， 並以實際的客戶體驗為基礎。 建議的最佳做法則會以該虛構公司的條件約束和需求為主。

此概觀會根據最佳做法來定義治理的最簡可行產品 (MVP) 以作為快速起點。 它也提供一些治理改進方法的連結，這些改進會隨著新業務或技術風險的出現而進一步新增更多最佳做法。

> [!WARNING]
> 這個 MVP 是基於一組假設的基準起點。 即便是這一系列最佳做法，也是以獨特的業務風險和風險承受度推動的公司原則。 若要查看您是否適用這些假設，請閱讀本文後面[較長的敘述](./narrative.md)。

### <a name="governance-best-practices"></a>治理最佳做法

這些最佳做法可作為組織在多個 Azure 訂用帳戶間快速且一致地新增治理防護的基礎。

### <a name="resource-organization"></a>資源組織

下圖顯示組織資源的治理 MVP 階層。

![資源組織圖](../../../_images/govern/resource-organization.png)

每個應用程式都應該在管理群組、訂用帳戶，以及資源群組階層的適當區域中部署。 在部署規劃期間，雲端治理小組將在階層中建立必要的節點，使雲端採用小組更具產能。

1. 定義每個營業單位的管理群組，其具有能詳細反映地理位置和環境類型 (例如生產或預先生產環境) 的階層。
2. 針對個別業務單位或地理位置的每一個唯一組合，建立生產和非生產訂用帳戶。 在建立多個訂用帳戶時，請務必謹慎。 如需詳細資訊，請參閱[這裡](../../../decision-guides/subscriptions/index.md)。
3. 在此群組階層的每個層級套用[一致的命名法](../../../ready/considerations/naming-and-tagging.md)。
4. 在部署資源群組時，應該要考慮其內容的生命週期：一起開發的所有項目要一起管理，並一起淘汰。 如需資源群組最佳做法的詳細資訊，請參閱[這裡](../../../decision-guides/resource-consistency/index.md)。
5. [區域選取](../../../decision-guides/regions/index.md)非常重要，因此必須納入考量，以備妥網路、監視、稽核來進行容錯移轉/容錯回復，並確認[所需的 SKU 可在偏好的區域中取得](https://azure.microsoft.com/global-infrastructure/services)。

![大型企業資源組織圖](../../../_images/govern/large-enterprise-resource-organization.png)

這些模式會提供成長的空間，而不會不必要地使階層複雜化。

[!INCLUDE [governance-of-resources](../../../../includes/caf-governance-of-resources.md)]

<!-- See comments for suggestion to possibly add here -->

## <a name="incremental-governance-improvements"></a>累加式治理改進

一旦部署此 MVP 之後，其他治理層就可以快速地合併到環境中。 以下是一些改進 MVP 以符合特定業務需求的方式：

- [受保護資料的安全性基準](./security-baseline-improvement.md)
- [任務關鍵性應用程式的資源設定](./resource-consistency-improvement.md)
- [成本管理控制](./cost-management-improvement.md)
- [累加式多重雲端改進的控制項](./multicloud-improvement.md)

<!-- markdownlint-disable MD026 -->

## <a name="what-does-this-guidance-provide"></a>本指南提供哪些內容？

在 MVP 中，從[部署加速](../../deployment-acceleration/index.md)專業領域建立做法和工具是為了快速套用公司原則。 特別是，MVP 會使用 Azure 藍圖、Azure 原則以及 Azure 管理群組套用幾個基本的公司原則，如這個虛構公司的敘述中所定義。 那些公司原則會使用 Azure Resource Manager 範本與 Azure 原則來套用，以建立小型的身分識別和安全性基準。

![累加式治理 MVP 的範例](../../../_images/govern/governance-mvp.png)

## <a name="incremental-improvements-to-governance-practices"></a>治理做法的漸進式改進

經過一段時間之後，這個治理 MVP 將用於累加式改進治理做法。 隨著採用率提高，業務風險也會增加。 雲端採用架構治理模型內的各種專業領域，將會持續調整以管理這些風險。 本系列的後續文章將討論影響虛構公司的公司原則變更。 這些變更會跨四個專業領域進行：

- 身分識別基準 (隨著移轉相依性在敘述中而改變)。
- 成本管理 (採用擴大規模時)
- 安全性基準 (部署受保護的資料時)。
- 資源一致性 (IT 操作開始支援任務關鍵性工作負載時)。

![累加式治理 MVP 的範例](../../../_images/govern/governance-improvement-large.png)

## <a name="next-steps"></a>後續步驟

現在您已經熟悉治理 MVP，並且了解即將發生的治理變更，請閱讀對於其他內容的支援敘述。

> [!div class="nextstepaction"]
> [閱讀支持性敘述](./narrative.md)
