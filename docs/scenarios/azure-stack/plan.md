---
title: 規劃 Azure Stack 中樞遷移
description: 規劃 Azure Stack 中樞遷移
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/19/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.openlocfilehash: fd756ec02305c2bdc9a5c6d106f6b5b4c170c60c
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86451828"
---
# <a name="plan-for-azure-stack-hub-migration"></a>規劃 Azure Stack 中樞遷移

本文假設您已瞭解如何將[Azure Stack 整合到您的雲端策略](./index.md)中，而且您的旅程與該文章中的範例相符。

在直接移到遷移的工作之前，請務必適當地設定 Azure 和 Azure Stack Hub 的期望。 這麼做有助於避免錯誤，或稍後在專案中 setbacks。 成功執行的關鍵在於充分瞭解使用 Azure 的時機，以及何時使用 Azure Stack 中樞。

## <a name="digital-estate-alignment"></a>數位資產對齊

這一開始會有幾個簡單的問題，在完成[數位資產合理化](../../digital-estate/index.md)時被詢問。

- 將應用程式和資料保留在內部部署的動機為何，例如法規需求、資料重心或合規性需求？
- 哪些特定的法規或合規性需求會影響要保留在資料中心的決策？
- 資料隱私權對資料移轉的影響為何？
- 是否已將遷移定義為現代化旅程？
- 若是如此，您是否已定義接下來的步驟，以及在遷移之後所需的目標？
- SLA、RPO、RTO 和可用性需求有哪些？

對於某些工作負載而言，這項資訊會針對該工作負載的 Azure 與 Azure Stack 中樞的價值進行討論。

## <a name="assessment-best-practices"></a>評估最佳做法

[使用 Azure Migrate 評估數位資產](../../plan/contoso-migration-assessment.md)的最佳做法，可以加速您的數位資產中工作負載和資產的評估和對齊。 這種最佳作法可讓您深入瞭解您的完整 IT 組合。 它也有助於識別容量、規模和設定的技術需求，以引導您進行遷移。

有了適當的評估資料，小組可以在評估 Azure 中公用或私用雲端平臺的選項時，進行最睿智做法選擇，並建立更清楚的優先順序。

## <a name="planning-best-practices"></a>規劃最佳做法

下列資源可協助您瞭解 Azure 與 Azure Stack 中樞之間的差異：

- [Azure Stack 總覽和藍圖](https://azure.microsoft.com/resources/videos/ignite-2018-azure-stack-overview-and-roadmap/)
- [Azure Stack 容量規劃](https://docs.microsoft.com/azure/azure-stack/capacity-planning)
- [Azure Stack Hub 資料中心整合逐步解說](https://docs.microsoft.com/azure-stack/operator/azure-stack-customer-journey)
- [Azure Stack VM 功能](https://docs.microsoft.com/azure-stack/user/azure-stack-vm-considerations?view=azs-1910)

瞭解每個工作負載的最佳平臺之後，您就可以將這些決策整合到[雲端採用計畫](../../plan/template.md)中，以一項一致的方式來管理公用和私人雲端遷移。

## <a name="next-step-prepare-your-environment-for-azure-stack-hub-migrations"></a>下一步：準備您的環境以進行 Azure Stack Hub 遷移

下列文章清單會帶您前往在雲端採用旅程圖的特定點上找到的指引。 有關[環境就緒](./ready.md)的第一篇文章是建議的下一個步驟。

- [環境就緒](./ready.md)
- [評估 Azure Stack 中樞的工作負載](./migrate-assess.md)
- [將工作負載部署到 Azure Stack 中樞](./migrate-deploy.md)
- [管理 Azure Stack 中樞](./govern.md)
- [管理 Azure Stack Hub](./manage.md)
