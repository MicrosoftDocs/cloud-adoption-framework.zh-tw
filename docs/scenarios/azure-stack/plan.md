---
title: 規劃您的 Azure Stack Hub 遷移
description: 規劃 Azure Stack Hub 的遷移。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/19/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.openlocfilehash: 64e1ae5cbe6f6ed31575614551f1dafc3275f1a0
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88572658"
---
# <a name="plan-your-azure-stack-hub-migration"></a>規劃您的 Azure Stack Hub 遷移

本文假設您已瞭解如何將 [Azure Stack 整合到您的雲端策略](./index.md) ，而且您的旅程圖會與該文章中的範例相符。

在您直接進入組織的遷移工作之前，請務必適當地設定 Azure 和 Azure Stack Hub 的預期。 這樣做有助於避免在專案中出現陷阱或 setbacks。 成功實施的關鍵是瞭解使用 Azure 的時機，以及何時使用 Azure Stack Hub 的關鍵。

## <a name="digital-estate-alignment"></a>數位資產對齊

當您完成 [數位資產合理化](../../digital-estate/index.md)時，與組織的數位資產一致，就會開始提出一些簡單的問題。

- 需要什麼動機，例如法規需求、資料引力或合規性需求，才能將應用程式和資料保留在內部部署？
- 哪些特定的法規或合規性需求會影響資料中心的決策？
- 資料隱私權會如何影響資料移轉？
- 是否會將遷移定義為現代化旅程？
- 如果有，您是否已定義下一個步驟和遷移之後所需的目標？
- 服務等級協定、復原點目標、復原時間目標和可用性需求有哪些？

針對某些工作負載，您對這些問題的答案將會針對該工作負載的 Azure 值與 Azure Stack Hub 的相關討論。

## <a name="assessment-best-practices"></a>評量最佳作法

藉由運用 [Azure Migrate 評估數位資產](../../plan/contoso-migration-assessment.md)的最佳作法，您可以加速評估和調整數位資產中的工作負載和資產。 這種最佳做法可讓您深入瞭解您的完整 IT 組合。 它也有助於識別容量、規模和設定的技術需求，以引導您的遷移。

藉由使用適當的評量資料，您的雲端採用小組可以進行最睿智做法選擇，並在評估 Azure 中的公用或私用雲端平臺的選項時，建立更清楚的優先順序。

## <a name="planning-best-practices"></a>規劃最佳作法

下列資源可協助您的小組瞭解 Azure 與 Azure Stack Hub 之間的差異：

- [Azure Stack 總覽和藍圖](https://azure.microsoft.com/resources/videos/ignite-2018-azure-stack-overview-and-roadmap/)
- [Azure Stack 容量規劃](/azure/azure-stack/capacity-planning)
- [Azure Stack Hub 資料中心整合逐步解說](/azure-stack/operator/azure-stack-customer-journey)
- [Azure Stack 虛擬機器功能](/azure-stack/user/azure-stack-vm-considerations?view=azs-1910)

當您瞭解每個工作負載的最佳平臺時，您可以將您的決策整合到 [雲端採用方案](../../plan/template.md) 中，以一項一致的方式來管理公用和私人雲端遷移。

## <a name="next-steps"></a>後續步驟

如需雲端採用旅程圖的特定元素指引，請參閱：

- [準備好您的雲端環境以進行 Azure Stack Hub 遷移](./ready.md)
- [評估 Azure Stack Hub 的工作負載](./migrate-assess.md)
- [將工作負載部署到 Azure Stack Hub](./migrate-deploy.md)
- [治理 Azure Stack Hub](./govern.md)
- [管理 Azure Stack Hub](./manage.md)
