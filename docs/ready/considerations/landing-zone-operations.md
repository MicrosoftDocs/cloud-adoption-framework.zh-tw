---
title: 改善登陸區域作業
description: 改善登陸區域作業。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/15/2020
ms.topic: overview
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: ffdfd350247d2cb1c6ff3cd367fbaf12ab6585e3
ms.sourcegitcommit: 71a4f33546443d8c875265ac8fbaf3ab24ae8ab4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2020
ms.locfileid: "86479617"
---
# <a name="improve-landing-zone-operations"></a>改善登陸區域作業

登陸區域作業提供作業管理的初始基礎。 隨著作業規模的改進，這些改良功能將會重構登陸區域，以符合日益增加的營運品質、可靠性和效能需求。

## <a name="landing-zone-operations-best-practices"></a>登陸區域操作最佳做法

下列連結提供改善登陸區域作業的最佳作法。

- [Azure 伺服器管理](../../manage/azure-server-management/index.md)：整合管理作業所需之雲端原生工具和服務的上線指南。
- [混合式監視](../../manage/monitor/index.md)：許多客戶已對 System Center Operations Manager 投入大量投資。 對於這些客戶，這份混合式監視指南可協助他們比較雲端原生報表工具與 Operations Manager 工具，並與其進行對比。 這種比較可讓您更輕鬆地決定要使用哪些工具來進行作業管理。
- [集中管理作業](../../manage/centralize-operations.md)：使用 Azure 燈塔來集中化跨多個 Azure 租使用者的作業管理。
- [建立操作適用性審查](../../manage/operational-fitness-review.md)：審查環境的操作適用性。
- 工作負載特有的作業最佳做法：
  - [復原檢查清單](https://docs.microsoft.com/azure/architecture/checklist/resiliency-per-service?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)
  - [失敗模式分析](https://docs.microsoft.com/azure/architecture/resiliency/failure-mode-analysis?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)
  - [從全區域的服務中斷復原](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)
  - [從資料損毀或意外刪除復原](https://docs.microsoft.com/azure/architecture/framework/resiliency/data-management?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)

## <a name="four-steps-to-improve-operations-beyond-a-single-landing-zone"></a>改善單一登陸區域以外作業的四個步驟

[管理方法](../../manage/index.md)提供建立作業管理容量的整體指導方針，請參閱[管理方法](../../manage/index.md)。 我們將使用該方法的基本結構，以及該方法的下列步驟，來改善所有登陸區域的登陸區域作業和作業。

<!-- cSpell:ignore caf -->

![管理方法 ](../../_images/manage/caf-manage.png)
 _圖1： CAF 管理方法。_

1. [建立管理基準](../../manage/azure-server-management/index.md)：管理基準會建立作業管理的基礎。 第一個步驟底下的指引可套用至任何登陸區域，以改善初始作業。
2. [定義商務承諾](../../manage/considerations/business-alignment.md)：瞭解登陸區域內每個工作負載的重要性和影響，將會建立「完成定義」，以供任何登陸區域進行任何持續的管理改進。 此程式也會識別每個工作負載的可靠性、效能和作業需求。
3. [擴充 [管理基準](../../manage/best-practices.md)]：您可以套用這組最佳作法，以改善初始基準以外的登陸區域作業。
4. [先進的作業和設計原則](../../manage/design-principles.md)：檢查特定工作負載、平臺或完整登陸區域的設計和作業，以符合更深入的需求。

## <a name="test-driven-development-cycle"></a>以測試為導向的開發週期

開始任何安全性改進之前，請務必瞭解「完成定義」和所有「接受條件」。 如需詳細資訊，請參閱在 Azure 中進行登陸區域和[測試導向開發](./azure-test-driven-development.md)的[測試導向開發](./test-driven-development.md)文章。

![雲端登陸區域的測試導向開發程式 ](../../_images/ready/test-driven-development-process.png)
 _圖2：雲端登陸區域的測試導向開發流程。_

## <a name="next-steps"></a>後續步驟

瞭解如何[改善登陸區域治理](./landing-zone-governance.md)，以支援大規模採用。

> [!div class="nextstepaction"]
> [改善登陸區域控管](./landing-zone-governance.md)
