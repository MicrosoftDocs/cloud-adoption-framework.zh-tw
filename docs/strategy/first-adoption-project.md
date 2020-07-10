---
title: 第一個雲端採用專案
description: 使用適用于 Azure 的雲端採用架構來瞭解雲端採用的流程，以及裝載于雲端的工作負載作業。
author: BrianBlanchard
ms.author: brblanch
ms.date: 5/19/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: strategy
ms.openlocfilehash: 35df4edd8437cea20f0bc397be901443c21015a5
ms.sourcegitcommit: bcc73d194c6d00c16ae2e3c7fb2453ac7dbf2526
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/09/2020
ms.locfileid: "86194641"
---
# <a name="first-cloud-adoption-project"></a>第一個雲端採用專案

有一個學習曲線和一個與雲端採用規劃相關的時間承諾。 即使是經驗豐富的小組，適當的規劃也會花時間：協調專案關係人的時間、收集和分析資料的時間、驗證長期決策的時間，以及調整人員、程式和技術的時間。 在最具生產力的採用成果中，規劃會與採用平行成長，並隨著每個版本和每個工作負載遷移至雲端而改善。 請務必瞭解雲端採用方案與雲端採用策略之間的差異。 您需要妥善定義的策略，以促進並引導雲端採用方案的執行。

<!-- docsTest:ignore "Strategy, Plan, Ready, Adopt, and Operate phases" -->

適用于 Azure 的雲端採用架構概述雲端採用的程式，以及裝載于雲端的工作負載操作。 跨策略、規劃、準備、採用及操作階段的每個程式，都需要稍微擴充技術、商業和營運技能。 其中一些技巧可能來自于引導式學習。 但其中有許多都是透過實際操作經驗最有效率的方式來取得。

與計畫的開發同時開始第一個採用程式，可提供一些優點：

- 建立成長的思維方式來鼓勵學習和探索。
- 提供機會讓小組開發必要的技能。
- 建立鼓勵共同作業之新方法的情況。
- 找出技能差距和潛在的合作關係需求。
- 提供有形的輸入給方案。

## <a name="first-project-criteria"></a>第一個專案準則

您的第一個採用專案應符合雲端採用的[動機](./motivations.md)。 可能的話，您的第一個專案也應該針對定義的[商務結果](./business-outcomes/business-outcome-template.md)示範進度。

## <a name="first-project-expectations"></a>第一個專案預期

小組的第一個採用專案可能會產生某種類型的生產環境部署。 但這種情況並非總是如此。 及早建立適當的期望。 以下是幾個要設定的期望：

- 此專案是學習的來源。
- 此專案可能會導致生產環境部署，但可能需要先額外投入。
- 此專案的輸出是提供長期生產解決方案的一組明確需求。

## <a name="first-project-examples"></a>第一個專案範例

為了支援上述準則，此清單提供每個動機類別的第一個專案範例：

- **重要的商務活動：** 當重要的商務事件是主要動機時， [Azure Site Recovery](../migrate/azure-migration-guide/migrate.md#azure-site-recovery)之類的工具可能是個不錯的第一個專案。 在遷移期間，您可以使用此工具來快速遷移資料中心資產。 但在第一個專案中，您可以單純地使用它做為嚴重損壞修復工具，以降低資料中心內嚴重損壞修復資產的相依性。

- **遷移動機：** 當遷移是主要動機時，最好是開始進行非關鍵工作負載的遷移。 《 [Azure 設定指南》](../ready/azure-setup-guide/index.md)和[azure 遷移指南](../migrate/azure-migration-guide/index.md)可提供您第一個工作負載的遷移指引。

- **創新動機：** 當創新是主要動機時，建立目標開發/測試環境可能是絕佳的第一個專案。

<!-- docsTest:ignore "data migration services" -->

第一個採用專案的其他範例包括：

- **商務持續性和嚴重損壞修復 (BCDR) ：** 除了 Azure Site Recovery 以外，您還可以將多個 BCDR 策略實作為第一個專案。
- **非生產：** 部署工作負載的非生產實例。
- 封存 **：** 冷儲存體可能會對資料中心資源造成負擔。 將該資料移至雲端是一種穩固的快速勝利。
- ** (EOS) 結束支援：** 遷移已達到支援終止的資產，是建立技術技能的另一個快速勝利。 它也可以從昂貴的支援合約或授權成本提供一些成本規避。
- **虛擬桌面介面 (VDI) ：** 為遠端員工建立虛擬桌面可以提供快速勝利。 在某些情況下，第一次採用專案也可以減少昂貴私人網路的相依性，以利商用公用網際網路連線。
- **開發/測試：** 從內部部署環境移除開發/測試，為開發人員提供控制、彈性和自助功能。
- **簡單的應用程式 (不到五個) ：** 現代化和遷移簡單的應用程式，以快速取得開發人員和操作體驗。
- **效能實驗室：** 當您需要在實驗室設定中進行大規模的效能時，請在短時間內使用雲端快速且符合成本效益地布建這些實驗室。
- **資料平臺：** 使用傾印/還原方法或資料移轉服務，建立具有可擴充計算的 data lake 來分析、報告或機器學習工作負載，以及遷移至受控資料庫。

## <a name="next-steps"></a>後續步驟

瞭解[平衡競爭優先順序](./balance-competing-priorities.md)的策略。

> [!div class="nextstepaction"]
> [平衡競爭優先順序](./balance-competing-priorities.md)
