---
title: 讓遷移後的應用程式準備好進行生產環境升階
description: 使用適用于 Azure 的雲端採用架構來瞭解針對生產環境升級準備遷移的應用程式時所需的驗證。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 2ad912c7bc2e61465a81e278714f5018c2373f7f
ms.sourcegitcommit: 7d3fc1e407cd18c4fc7c4964a77885907a9b85c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "80429222"
---
# <a name="prepare-a-migrated-application-for-production-promotion"></a>讓遷移後的應用程式準備好進行生產環境升階

在工作負載升階後，生產環境的使用者流量會路由傳送至遷移後的資產。 整備活動可讓您有機會針對該流量來準備工作負載。 以下是一些有助於引導整備活動的業務和技術考量。

## <a name="validate-the-business-change-plan"></a>驗證業務變更方案

當業務使用者或客戶利用技術解決方案來執行驅動業務的程序時，就會發生轉換。 整備是驗證[業務變更方案](./business-change-plan.md)的絕佳機會，並可確保會對涉及的業務和技術小組進行適當的訓練。 尤其是請確定您已正確傳達變更方案的下列技術相關層面：

- 終端使用者訓練已完成 (或至少已規劃)。
- 任何中斷時段都已確實傳達並通過核准。
- 生產資料已經過同步處理並由使用者驗證過。
- 驗證升階和採用時機；確保時間表和變更已傳達給終端使用者。

## <a name="final-technical-readiness-tests"></a>最終的技術整備測試

「就緒」** 是生產發行前的最後一個步驟。 這表示此步驟也是測試工作負載的最後一次機會。 以下是此階段進行期間所建議的幾項測試：

- **網路隔離測試。** 測試和監視網路流量以確保有適當隔離，而且沒有未預期的網路弱點。 也請驗證在移轉期間所提供的任何網路路由都不會遇到非預期的流量。
- **相依性測試。** 請確定所有工作負載應用程式相依性都已遷移，而且可以從遷移後的資產存取。
- **商務持續性和災害復原 (BCDR) 測試。** 請驗證是否已建立任何備份和復原 SLA。 可能的話，請從 BCDR 解決方案對資產執行完整復原。
- **終端使用者路由測試。** 請驗證終端使用者流量的流量模式和路由。 請確保網路效能符合預期。
- **最後的效能檢查。** 請確定效能測試已完成，並已獲得終端使用者認可。 請執行任何自動化的效能測試。

## <a name="final-business-validation"></a>最終業務驗證

在驗證過業務變更方案和技術整備之後，下列最終步驟可以完成業務驗證：

- **成本驗證 (方案與實際)。** 測試時可能會變更大小和架構。 請確定實際的部署價格仍與原始方案一致。
- **傳達和執行移轉方案。** 在移轉之前，請先傳達移轉事宜並據此執行。

## <a name="next-steps"></a>後續步驟

完成所有整備活動之後，就可以[將工作負載升階](./promote.md)。

> [!div class="nextstepaction"]
> [要將遷移後的資源升階至生產環境需要什麼？](./promote.md)
