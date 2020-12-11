---
title: 建立初始雲端治理基礎
description: 使用適用于 Azure 的雲端採用架構，藉由建立初始雲端治理基礎來開始進行雲端治理。
author: BrianBlanchard
ms.author: brblanch
ms.date: 02/25/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: internal
ms.openlocfilehash: cdbba0f9047a351af9240712f69e4886ea4750d6
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97020109"
---
# <a name="establish-an-initial-cloud-governance-foundation"></a>建立初始雲端治理基礎

建立雲端治理是很廣泛的反復工作。 在速度與控制之間取得有效的平衡相當困難，尤其是在雲端採用的早期方法期間執行。 雲端採用架構中的治理指導方針，可協助透過敏捷式採用方式提供平衡。

本文提供兩個選項來建立治理的初始基礎。 任一個選項都可確保在採用採用計畫時可以調整並擴充治理條件約束，並更清楚地定義需求。 根據預設，初始基礎會假設隔離和控制的位置。 它在資源組織上的重點也會比資源管理更多。 此輕量起點稱為 *最小可行產品 (MVP)* 進行治理。 MVP 的目標是減少建立初始治理位置的障礙，然後讓解決方案的快速成熟得以解決各種具體風險。

## <a name="already-using-the-cloud-adoption-framework"></a>已在使用雲端採用架構

如果您已遵循雲端採用架構，您可能已部署治理 MVP。 治理是任何作業模型的核心層面。 它存在於雲端採用生命週期的每個方法中。 因此， [雲端採用架構](../index.yml) 會提供指導方針，將治理插入與 [雲端採用計畫](../plan/index.md)的實施相關的活動中。 這項治理整合的其中一個範例是使用藍圖來部署一或多個登陸區域，這些登陸區域存在於 [現成的方法](../ready/index.md) 指引中。 另一個範例是 [相應放大訂閱](../ready/azure-best-practices/scale-subscriptions.md)的指引。 如果您已遵循其中一項建議，則下列 MVP 章節只是您現有的部署決策的評論。 在快速審核之後，請立即跳到 [成熟的初始治理解決方案，並套用最佳作法控制項](./foundation-improvements.md)。

## <a name="establish-an-initial-governance-foundation"></a>建立初始治理基礎

以下是兩個不同的初始治理基礎 (範例也稱為治理 Mvp) ，可將用於治理的音效基礎套用至新的或現有的部署。 選擇最符合您業務需求的 MVP 以開始使用：

- [標準治理指南](./guides/standard/index.md)：以建議的初始雙訂用帳戶模型為基礎的大部分組織指南，其設計目的是在多個區域中部署，但不橫跨公用和主權/政府雲端。
- [適用於複雜企業的治理指南](./guides/complex/index.md)：適用於由多個獨立 IT 業務單位或跨公用和主權/政府雲端管理的企業指南。

## <a name="next-steps"></a>後續步驟

管理治理基礎之後，請套用適合的建議，以改善解決方案並防範有形風險。

> [!div class="nextstepaction"]
> [改善初始治理基礎](./foundation-improvements.md)
