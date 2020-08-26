---
title: 第一個雲端採用專案
description: 使用適用于 Azure 的雲端採用架構，瞭解雲端採用的流程，以及裝載于雲端的工作負載作業。
author: BrianBlanchard
ms.author: brblanch
ms.date: 5/19/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: strategy
ms.openlocfilehash: 507fdcba779f70b418e5d01a7058345967750af5
ms.sourcegitcommit: 07d56209d56ee199dd148dbac59671cbb57880c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88877019"
---
# <a name="first-cloud-adoption-project"></a>第一個雲端採用專案

其中有學習曲線和與雲端採用規劃相關聯的時間承諾。 即使是有經驗的團隊，適當的規劃也需要時間：讓專案關係人、收集和分析資料的時間、驗證長期決策的時間，以及調整人員、程式和技術的時間。 在最高生產力的採用工作中，計畫會隨著採用而並行成長、每個版本的改進，以及將每個工作負載遷移至雲端。 請務必瞭解雲端採用方案與雲端採用策略之間的差異。 您需要妥善定義的策略，以促進及引導採用雲端採用計畫。

<!-- docutune:ignore "Strategy, Plan, Ready, Adopt, and Operate phases" -->

適用于 Azure 的雲端採用架構概述雲端採用的流程，以及裝載于雲端的工作負載作業。 跨策略、計畫、就緒、採用和操作階段的每個程式都需要稍微擴充技術、業務和營運技巧。 其中有些技能可能來自于導向學習。 但其中有許多都是透過實際操作經驗獲得最有效率的。

以平行方式啟動第一個採用程式與計畫的開發，可提供一些優點：

- 建立成長思維來鼓勵學習和探索。
- 讓團隊有機會開發必要的技能。
- 建立鼓勵新的共同作業方法的情況。
- 找出技能差距和潛在的合作關係需求。
- 提供實際的計畫輸入。

## <a name="first-project-criteria"></a>第一個專案準則

您的第一個採用專案應與您的雲端採用 [動機](./motivations.md) 一致。 可能的話，您的第一個專案應該也會示範定義 [商務結果](./business-outcomes/business-outcome-template.md)的進度。

## <a name="first-project-expectations"></a>第一個專案預期

小組的第一個採用專案可能會導致實際執行部署。 但這不一定是如此。 及早建立適當的期望。 以下是一些應設定的預期：

- 此專案是學習的來源。
- 此專案可能會導致生產環境部署，但可能需要先進行額外的工作。
- 此專案的輸出是提供長期生產解決方案的一組明確需求。

## <a name="first-project-examples"></a>第一個專案範例

為了支援上述準則，此清單提供每個動機類別的第一個專案範例：

- **重大商務活動：** 當重要的商務事件是主要動機時，如 [Azure Site Recovery](../migrate/azure-migration-guide/secure-and-manage.md#replicate-an-azure-vm-to-another-region-with-site-recovery-service) 這類工具的執行可能會是理想的第一個專案。 在遷移期間，您會使用類似 [Azure Migrate](../migrate/azure-migration-guide/migrate.md#azure-migrate) 的工具，快速地遷移資料中心資產。 但是在第一個專案中，您可以先使用 Azure Site Recovery 作為嚴重損壞修復工具。 在 pragmatically 規劃遷移之前，降低資料中心內嚴重損壞修復資產的相依性。

- **遷移動機：** 當遷移是主要動機時，開始遷移非關鍵性工作負載是明智的做法。 [Azure 設定指南](../ready/azure-setup-guide/index.md)和[azure 遷移指南](../migrate/azure-migration-guide/index.md)可以提供遷移第一個工作負載的指引。

- **創新動機：** 當創新是主要動機時，建立目標開發/測試環境可能是絕佳的第一個專案。

<!-- docutune:ignore "data migration services" -->

第一個採用專案的其他範例包括：

- **商務持續性和嚴重損壞修復 (BCDR) ：** 除了 Azure Site Recovery 之外，您還可以將多個 BCDR 策略實作為第一個專案。
- **非生產：** 部署工作負載的非生產實例。
- 封存 **：** 冷儲存體可能會對資料中心資源造成壓力。 將該資料移至雲端是一項穩固的快速勝利。
- **終止支援 (EOS) ：** 遷移已達到終止支援的資產，是建立技術技能的另一個快速勝利。 它也可以從昂貴的支援合約或授權成本來提供一些成本規避。
- **虛擬桌面介面 (VDI) ：** 建立遠端員工的虛擬桌面可提供快速勝利。 在某些情況下，這項第一個採用專案也可能會降低對昂貴私人網路的相依性，以利商用的網際網路連線。
- **開發/測試：** 從內部部署環境中移除開發/測試，以提供開發人員控制、靈活性和自助功能。
- **簡單的應用程式 (少於五個) ：** 現代化和遷移簡單的應用程式，以快速獲得開發人員和操作經驗。
- **效能實驗室：** 當您在實驗室設定中需要高效能的效能時，請使用雲端快速且符合成本效益的方式，在短時間內布建這些實驗室。
- **資料平臺：** 使用可調整的計算來建立 data lake，以供分析、報告或機器學習工作負載使用，並使用傾印/還原方法或資料移轉服務遷移至受控資料庫。

## <a name="next-steps"></a>後續步驟

瞭解 [平衡競爭優先順序](./balance-competing-priorities.md)的策略。

> [!div class="nextstepaction"]
> [平衡競爭優先順序](./balance-competing-priorities.md)
