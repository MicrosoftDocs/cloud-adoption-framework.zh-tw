---
title: 改善登陸區域作業
description: 改進登陸區域作業。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 572e14e1b5fb06662e37eeb16566f69cb0372a55
ms.sourcegitcommit: a7eb2f6c4465527cca2d479edbfc9d93d1e44bf1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94713882"
---
# <a name="improve-landing-zone-operations"></a>改善登陸區域作業

登陸區域作業可提供作業管理的初始基礎。 隨著營運調整，這些改良功能將會重構登陸區域，以符合日益增加的營運、可靠性和效能需求。

## <a name="landing-zone-operations-best-practices"></a>登陸區域作業的最佳作法

下列連結提供改善登陸區域作業的最佳做法。

- [Azure 伺服器管理](../../manage/azure-server-management/index.md)：加入管理作業所需的雲端原生工具和服務的上線指南。
- [混合式監視](../../manage/monitor/index.md)：許多客戶已在 System Center Operations Manager 中進行大量投資。 針對這些客戶，此混合式監視指南可以協助他們比較和對比雲端原生報表工具與 Operations Manager 工具。 這種比較可讓您更輕鬆地決定要用於作業管理的工具。
- [集中管理作業](../../manage/centralize-operations.md)：使用 Azure Lighthouse 將 operations management 集中在多個 Azure 租使用者。
- [建立操作健身審核](../../manage/operational-fitness-review.md)：檢查環境中的操作適用性。
- 工作負載特定的作業最佳做法：
  - [復原檢查清單](/azure/architecture/checklist/resiliency-per-service?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)
  - [失敗模式分析](/azure/architecture/resiliency/failure-mode-analysis?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)
  - [從全區域的服務中斷復原](/azure/architecture/resiliency/recovery-loss-azure-region?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)
  - [從資料損毀或意外刪除復原](/azure/architecture/framework/resiliency/data-management?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)

## <a name="four-steps-to-improve-operations-beyond-a-single-landing-zone"></a>改善單一登陸區域以外作業的四個步驟

[管理方法](../../manage/index.md)提供建立作業管理容量的整體指引，請參閱[管理方法](../../manage/index.md)。 我們將使用該方法的基本結構以及該方法的下列步驟，來改善所有登陸區域的登陸區域作業和作業。

![管理方法 ](../../_images/manage/caf-manage.png)
 _圖1：雲端採用架構的管理方法。_

1. [建立管理基準](../../manage/azure-server-management/index.md)：管理基準可建立作業管理的基礎。 第一個步驟下的指導方針可套用至任何登陸區域，以改善初始作業。
2. [定義商務承諾](../../manage/considerations/business-alignment.md)：瞭解登陸區域內每個工作負載的重要性和影響，將會針對任何登陸區域的任何進行中管理改進，建立「完成定義」。 此程式也會識別每個工作負載的可靠性、效能和作業需求。
3. [展開管理基準](../../manage/best-practices.md)：可以套用這組最佳作法，以改善初始基準以外的登陸區域作業。
4. [先進的作業和設計原則](../../manage/design-principles.md)：檢查特定工作負載、平臺或完整登陸區域的設計和作業，以符合更深入的要求。

## <a name="test-driven-development-cycle"></a>測試導向的開發週期

在開始進行任何安全性改進之前，請務必先瞭解「完成定義」和所有「接受準則」。 如需詳細資訊，請參閱在 Azure 中對登陸區域和[測試導向開發](./azure-test-driven-development.md)進行[測試導向開發的相關](./test-driven-development.md)文章。

![雲端登陸區域的測試導向開發程式 ](../../_images/ready/test-driven-development-process.png)
 _圖2：雲端登陸區域的測試導向開發程式。_

## <a name="next-steps"></a>後續步驟

瞭解如何 [改善登陸區域治理](./landing-zone-governance.md) ，以支援大規模採用。

> [!div class="nextstepaction"]
> [改善登陸區域控管](./landing-zone-governance.md)
