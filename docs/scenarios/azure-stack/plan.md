---
title: 規劃您的 Azure Stack 中樞遷移
description: 規劃您的 Azure Stack 中樞遷移。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/19/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.openlocfilehash: d1336210469ca45ac2b60dd00a56f5697facd279
ms.sourcegitcommit: 76edf563a08ff7dc81c3fc2dc6c8972ab3b4c55b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2020
ms.locfileid: "88237180"
---
# <a name="plan-your-azure-stack-hub-migration"></a>規劃您的 Azure Stack 中樞遷移

本文假設您已瞭解如何將 [Azure Stack 整合到您的雲端策略](./index.md) 中，而且您的旅程與該文章中的範例相符。

在您直接進入組織的遷移工作之前，請務必適當地設定 Azure 和 Azure Stack 中樞的預期。 這麼做有助於避免錯誤，或稍後在專案中 setbacks。 成功執行的關鍵在於充分瞭解使用 Azure 的時機，以及何時使用 Azure Stack 中樞。

## <a name="digital-estate-alignment"></a>數位資產對齊

您的組織數位資產的調整從幾個簡單的問題開始，到您完成 [數位資產合理化](../../digital-estate/index.md)時提出。

- 推動應用程式和資料內部部署的動機有哪些？例如法規需求、資料重心或合規性需求？
- 哪些特定的法規或合規性需求會影響保留在資料中心的決策？
- 資料隱私權會如何影響資料移轉？
- 是否已將遷移定義為現代化旅程？
- 若是如此，您是否已定義接下來的步驟，以及在遷移之後所需的目標？
- 什麼是服務等級協定、復原點目標、復原時間目標和可用性需求？

對於某些工作負載，這些問題的答案將會針對該工作負載的 Azure 與 Azure Stack 中樞的價值進行討論。

## <a name="assessment-best-practices"></a>評估最佳做法

藉由套用 [Azure Migrate 評估數位資產](../../plan/contoso-migration-assessment.md)的最佳做法，您可以加速評估和調整您數位資產中的工作負載和資產。 這種最佳作法可讓您深入瞭解您的完整 IT 組合。 它也有助於識別容量、規模和設定的技術需求，以引導您進行遷移。

藉由使用適當的評估資料，您的雲端採用小組可以進行最睿智做法選擇，並在評估 Azure 中公用或私用雲端平臺的選項時，建立更清楚的優先順序。

## <a name="planning-best-practices"></a>規劃最佳做法

下列資源可協助您的小組瞭解 Azure 與 Azure Stack 中樞之間的差異：

- [Azure Stack 總覽和藍圖](https://azure.microsoft.com/resources/videos/ignite-2018-azure-stack-overview-and-roadmap/)
- [Azure Stack 容量規劃](https://docs.microsoft.com/azure/azure-stack/capacity-planning)
- [Azure Stack Hub 資料中心整合逐步解說](https://docs.microsoft.com/azure-stack/operator/azure-stack-customer-journey)
- [Azure Stack 虛擬機器功能](https://docs.microsoft.com/azure-stack/user/azure-stack-vm-considerations?view=azs-1910)

當您瞭解每個工作負載的最佳平臺時，您可以將您的決策整合到 [雲端採用計畫](../../plan/template.md) 中，以一項一致的方式來管理公用和私人雲端遷移。

## <a name="next-steps"></a>後續步驟

如需雲端採用旅程的特定元素指引，請參閱：

- [準備好您的雲端環境來進行 Azure Stack Hub 遷移](./ready.md)
- [評估 Azure Stack Hub 的工作負載](./migrate-assess.md)
- [將工作負載部署到 Azure Stack Hub](./migrate-deploy.md)
- [治理 Azure Stack Hub](./govern.md)
- [管理 Azure Stack Hub](./manage.md)
