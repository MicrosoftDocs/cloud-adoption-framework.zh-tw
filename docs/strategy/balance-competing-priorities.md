---
title: 平衡競爭優先順序
description: 探索平衡競爭優先順序的策略。
author: BrianBlanchard
ms.author: brblanch
ms.date: 03/04/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: strategy
ms.custom: internal
ms.openlocfilehash: 06fcd59e26240a01c8b64e391c7e36595cc77de9
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101787938"
---
# <a name="balance-competing-priorities"></a>平衡競爭優先順序

在任何數位轉型旅程上登機的行為，就像是專案關係人的強制函式，橫跨商務和技術團隊。 成功的途徑是以組織的能力平衡競爭優先順序。

類似于其他數位轉換，雲端採用將會在整個採用生命週期中公開競爭的優先順序。 如同其他形式的轉換，在這些優先順序中找出餘額的能力，將會對商務價值的實現產生顯著的影響。 平衡這些競爭優先順序將需要在專案關係人之間進行開放式和有時很難交談 (，有時也會) 個別參與者。

本文概述一些在執行每個方法時通常會討論的競爭優先順序。 我們希望這個先進的認知有助於在開發您的雲端採用策略時，針對這些討論做好更好的準備。

![雲端採用生命週期總覽](../_images/caf-overview.png)

下列各節會對應到上述的雲端採用週期視覺效果的流程。 不過，請務必瞭解雲端採用是反復的 (不是連續的程式) 而且這些競爭優先順序將會出現 (而且有時會在雲端採用旅程的不同點 reemerge) 。

## <a name="general-theme-of-the-cloud-adoption-framework-approach"></a>雲端採用架構方法的一般主題

整合型解決方案和先進的規劃都是以一系列的假設為基礎，可能 (或可能無法) 證明在一段時間內是正確的。 採用雲端通常是企業和技術團隊的新體驗。 就像大部分的新經驗或學習機會一樣，這些假設也有很高的機率會證實為 false。

遵循經過證實的延遲技術決策 agile 準則，是此架構中大部分指引的最優先方法。 該方法遵循一致的模式：建立一般結束狀態、快速移至初始執行、測試和驗證假設，以及及早重構以解決假設。 這種類型的成長思維可將學習最大化，並將風險降至最低，但需要一些不明確的風險。

有時候，可能會有不明確的 scarier (或比 false 假設更危險的) 。 雖然此架構仰賴執行期間的學習和解決方式，但在許多情況下，小組需要針對以分析為基礎或以假設為基礎的方法。 下列各節將嘗試說明每個區段中至少一個「擴充的範圍範例」，以說明第二個更深入的反復專案會有價值的時間。

## <a name="balance-during-the-strategizing-phase"></a>Strategizing 階段期間的平衡

策略方法的核心目標是要開發專案關係人之間的一致。 一旦定義之後，對齊的策略性位置將會推動每個方法的行為，以確保技術決策符合所需的業務成果。 參與專案關係人之間的調整，可建立一組共同的共同優先順序：相對 **于時間與商務影響****的深度**。

**競爭優先順序：**

- **理由深度：** 專案關係人通常想要有深度的財務分析和全面的商業理由，以充分配合策略性的方向。 可惜的是，這種分析層級可能需要經過一段時間，才能進行資料收集和分析。
- **業務影響時間：** 相反地，專案關係人通常會負責在定義的時間範圍內供應商務成果。 花時間分析和評量可能會在技術工作開始之前，讓這些結果具有風險。

**最小範圍：** 若要尋找此餘額，您需要在此程式中及早進行專案關係人討論。 策略方法建議您限制在這種早期工作期間的對齊範圍。 在建議的方法中，專案關係人會專注于調整一組核心動機、可測量的結果，以及高階的業務理由。 然後，專案關係人應快速認可到少數初始專案或試驗，以推動所需的學習機會。

**擴充的範圍範例：** 如果最初的商務分析指出對企業造成負面影響的高風險，則專案關係人可能需要減緩效能，並且更謹慎地在商業理由期間評估更深入的分析。

## <a name="balance-during-the-planning-phase"></a>規劃階段期間的平衡

類似于在 strategizing 階段期間的優先順序，在規劃階段中有個需求，可平衡初步規劃與延遲技術決策的深度。

**競爭優先順序：**

- 有關雲端中的技術實施 **的初步規劃深度** 通常包含大量的假設。 尤其是當小組有技能差距時，環境會受到探索缺口的影響，或者工作負載並沒有明確定義的架構結束狀態。 這些假設全都在詳細的雲端採用方案中都很常見。 需要進行實驗、試驗和定性分析，才能移除這些假設。
- **延遲的技術決策** 會假設您可以在稍後進行技術決策，讓決策更加精確。 下列敏捷式產品規劃準則將有助於延遲技術決策，讓他們能夠在適當的時間進行，以提供足夠的資訊。 不過，這種方法會導致初始計畫中有更高程度的混淆。

**最小範圍：** 在可管理的方案內，建議您採用敏捷式產品開發方法來推動提示動作。 計畫方法建議您執行下列步驟，以達成這項平衡。 使用自動化的探索工具清查完整的數位資產，但使用增量合理化來規劃到下一個1-3 個月的工作。 確保適當的組織對齊，以快速地移動。 為指派的小組建立技能就緒方案。 使用策略和方案範本，快速部署初始待辦專案。

**擴充的範圍範例：** 有時候，雲端採用方案的傳遞可能會回應時間緊迫或高度影響的商務事件。 成功需要在一段固定時間內移動大量資產時，上述步驟通常會遵循更深入的規劃工作。 在這些案例中，成功的關鍵在於規劃是否足以開始進行，然後規劃完整的參與。 這種方法可降低規劃封鎖業務成果的可能性。

## <a name="balance-during-the-readiness-phase"></a>在準備階段期間的平衡

當採用小組準備第一步進入雲端時，通常會在採用和長期作業之間有競爭的優先順序。 小組可能會很難在手邊提供工作，而不是妥善管理。 這在傳統的 IT 環境中是必要的，因為開發平臺的動作需要實體資產和取得週期。 不過，在程式碼中定義整個 IT 平臺時，傳統的開發策略 (像重構) 可減少從一開始就妥善管理的需求。

**競爭優先順序：**

- **長期作業：** 客戶通常會被想要有一個符合功能與目前營運管理、治理和安全性系統功能同位的雲端環境遭到封鎖。 在目前的客戶研究中，超過90% 的客戶所需的支援必須獲得超過此思維。 這項封鎖程式會插入延遲變慢或阻礙業務影響的月數。
- **採用時間：** 雲端式工具（例如 Azure 原則、Azure 藍圖和管理群組）可讓您輕鬆地在 IT 平臺上進行重構。 此外，預先定義的登陸區域也提供固定的位置，以加速部署至已符合許多功能同位需求的環境。 有機會加快上市時間，同時對長期作業的影響降到很低。

**最小範圍：** 現成的方法會概述從快速採用長期作業的直接途徑。 這種方法是從啟用環境重構的工具基本簡介開始。 根據這些工具和環境需求，系統會引導客戶選擇預先定義的登陸區域 (每個都使用基礎結構即程式碼模型) 來傳遞。 然後，您可以在雲端採用過程中重構該程式碼，以改善作業、安全性和管理做法。

<!-- docutune:casing "Govern and Manage methodologies" -->

**擴充的範圍範例：** 對於採用計畫在24個月內進行長期目標 (的團隊而言) 若要裝載 **1000 以上的資產 (應用程式、基礎結構或資料資產) 在雲端中**，建議使用更健全的登陸區域觀點。 在這些情況下，在初始登陸區域交談期間應考慮治理和管理方法。 不過，這種更深入的考慮通常會在雲端採用方案中增加數周或數個月。 為了將對業務成果的影響降到最低，採用小組應該平行地試驗雲端中的實際工作負載，以建立更成熟的登陸區域和中央架構解決方案。

## <a name="balance-during-the-migration-phase"></a>遷移階段期間的平衡

在遷移工作期間，採用小組通常會假設工作負載將在雲端中重新裝載為現行的設定。 這會直接與正向觀點競爭以重新架構每個工作負載，以更充分利用雲端功能。 不過，這兩個不會互斥，而且可以在透過一般進程管理時免費。

**競爭優先順序：**

- **重新裝載：** 客戶通常會將遷移移至在其目前狀態設定中將所有資產複寫到雲端的隨即 _轉移_ 動作。 這會導致 IT 組合內的一些漂移。 這種方法也是淘汰現有資料中心資產的最快方法。
- **重新架構：** 現代化每個工作負載的架構，能將雲端的價值最大化到成本、效能和營運。 不過，這種方法的速度很慢，而且通常需要存取每個應用程式的原始程式碼。

**最小範圍：** 在初期規劃期間，請使用重新裝載選項進行規劃，並清楚瞭解此選項是初始的商業假設，而不是技術決策。 在遷移方法中，雲端採用小組會針對每個已遷移的工作負載挑戰這項假設。 此方法會針對每個工作負載或群組建立遷移處理站的工作負載或工作負載，遵循評定/遷移/升方法。 在評估階段，採用小組會評估每個工作負載的技術符合和架構。 這項評估工作很少會產生單純的隨即轉移方法，因為架構中有許多元件通常會針對重構和現代化進行選取。

**擴充的範圍範例：** 針對要徑任務或高度敏感性的工作負載（例如大型主機或多層式微服務應用程式），評估階段可能需要更深入的工作負載評量。 在這些重新架構的情況下，客戶應該使用 Microsoft Azure Well-Architected 評論和 [Microsoft azure Well-Architected 架構](/azure/architecture/framework) 來調整評量期間的工作負載需求。

## <a name="balance-during-the-innovation-phase"></a>創新階段期間的平衡

真正的客戶對創新，在需要提供規劃的功能集和客戶理解開發流程之間，建立了常見的衝突優先順序。

**競爭優先順序：**

- **功能焦點：** 創新的初始計畫建基於現有的數位資產和雲端功能，以提供一組符合客戶需求的功能。 您可以輕鬆地讓計畫推動技術實行，進而專注于專注開發工作。 這種方法通常會導致暫時的專案關係人滿意度，但會降低推動影響客戶行為之創新的可能性。
- **客戶的理解：** 初始計畫是開發商務端的重要部分，而且應該包含在一般報告中。 不過，以客戶理解的學習、測量和建築是更精確的創新工作成功量值。 專注于客戶的功能更有可能導致短期和長期客戶滿意度和業務衝擊。

**最小範圍：** 創新方法說明如何透過商業價值共識來整合策略和計畫。 然後，本指南將介紹可加速各項創新專業領域的雲端原生工具，並伴隨實施的最佳做法。 最後，流程改進部分會示範如何在雲端採用旅程中採用計畫和策略，以建立客戶的理解方法。 這種方法著重于利用盡可能少的技術來提供創新。

**擴充的範圍範例：** 有時候，創新可能會相依于任務關鍵性或高敏感度的工作負載。 當「客戶」是內部使用者時，開發投入時間可能會是最早反覆運算期間的任務關鍵性和高敏感度。 在這些案例中，採用小組應使用 Microsoft Azure Well-Architected 評論和 Microsoft Azure Well-Architected Framework，在程式初期評估高階的架構設計。

## <a name="balance-during-the-governance-phase"></a>在治理階段進行平衡

雲端治理的作法是在兩個競爭的優先順序之間進行固定的平衡：速度和靈活性與妥善控管的環境。 雲端治理小組將焦點放在透過統一的控制項評估企業的風險，並將變更降至最低。 採用小組著重于推動業務成果，這需要新的風險，並在本質上建立變更。

**競爭優先順序：**

- **妥善控管：** 設計來將風險降至最低的每個控制項，會封鎖一些變更或限制設計選項的層面。 控制對妥善管理的環境而言是不可或缺的。 不過，當控制項是以隔離方式設計和部署時，可能會因為其預期的風險而受到損害。
- **速度和靈活性：** 速度和靈活性是數位經濟中的基本商務需求。 兩者都需要能夠以最少量的障礙來推動創新或採用。 當變更是在隔離治理的情況下進行時，它會產生新的風險，可能會以非預期的方式危害企業。

**最小範圍：** 控管方法會建議您不應該隔離治理和採用。 這種方法一開始會先瞭解治理專業領域，以及有關商務風險、原則和流程的交談。 作為整個雲端採用旅程中的作用中成員，治理小組可以執行一組最小的護欄，以解決雲端採用方案內的有形風險。 經過一段時間之後，治理小組就可以重構並擴充這些護欄，以符合新的風險。 這種方法能將學習和創新最大化，同時將風險降至最低。

**擴充的範圍範例：** 當商務風險很高時（特別是在採用初期），雲端治理小組可能需要加快治理實施的擴充。 您可以使用相同的指導方針和練習來新增此較高層級的治理，但時間可能需要加速。 在某些情況下，在部署第一個登陸區域時，可能甚至需要治理的 advanced 狀態。

## <a name="balance-during-the-management-phase"></a>管理階段的平衡

過去十年來，關於營運管理的 IT 商務模型一直持續演進。 隨著硬體維護作業更進一步地從其核心價值主張中移動，營運管理的觀點也已改變。 因為它會增加專注于供應商務價值，所以營運管理小組會與平衡無 ops/低營運營運和廣泛的投資相衝突。

**競爭優先順序：**

- **廣泛的投資：** 在避免中斷、快速復原和監視環境之間進行平均投資，是作業管理的傳統方法。 這種方法的成本可能很高，有時會重複雲端廠商所提供的支援產品。
- **無 ops 和低營運：** 使用雲端原生作業工具，將先前由全職員工提供的重複和週期性工作降至最低。 減少作業管理模型中的這些操作相依性，可讓這些員工更有價值。 這種方法只會導致欠佳操作支援。

**最小範圍：** 管理方法建議您建立雲端原生、無 ops 的基準。 確認無作業基準不符合所有商務需求，請與企業合作來定義承諾，並更妥善地調整投資。 展開基準以符合所有工作負載的一般需求。 然後讓平臺小組或特定工作負載小組在妥善管理的環境內維護妥善管理的解決方案。

**擴充的範圍範例：** 在大部分的環境中，其商業價值的工作負載很小，會比對營運的深度投資。 在這些案例中，IT 小組可能會想要使用 Microsoft Azure Well-Architected 評論和 Microsoft Azure Well-Architected Framework 來引導更深入的作業。

## <a name="balance-during-the-organization-phase"></a>在組織階段進行平衡

這篇文章中的其中一項重點是反映它的磁片磁碟機，以提供速度和靈活性的商務需求。 這項相同的改變會顯示組織結構圖 (或虛擬小組結構的變更，) 來加強對商務成果的支援。 當 IT 領導人反映小組結構時，通常會解決兩個競爭的優先順序：集中控制和委派的控制項。

**競爭優先順序：**

- **集中式控制：** 這項作業模型著重于集中執行固定原則所需的所有控制項。 在此模型中，它可作為創新、速度和靈活性的封鎖程式。 不過，它可以確保穩定性、合規性和安全性的程度更高。
- **委派的控制項：** 在此分散式作業模型中，假設每個 DevOps 小組或商務應用程式小組都將根據供應商務目標所需的解決方案，提供自己的一組控制項。 在此模型中，它會提供護欄來協助讓團隊保持道路，但盡可能將強制的技術條件約束的數目降至最低。

**最小範圍：** 大部分的組織會在一段時間內經歷一組自然的演進。 組織方法會概述最常見的演進系列。 建議的指導方針是為了讓小組致力於卓越的雲端中心 (CCoE) 結構，以提供委派的控制方法。

**擴充的範圍範例：** 在許多情況下，會觸發集中式控制的需求。 協力廠商合規性需求和暫時性的安全性漏洞是集中控制的兩個觸發程式範例。 在這些情況下，通常需要建立限制原則和固定的固定控制項。 不過，為了讓創新和採用繼續進行，建議中央 IT 小組根據每個工作負載的重要性和敏感性來提供這些控制項。 提供較少控制的環境，但是範圍或風險設定檔，甚至可以在需要控制時提供彈性。

## <a name="next-steps"></a>下一步

瞭解如何 [平衡遷移、創新和實驗](./balance-the-portfolio.md) ，以充分發揮雲端遷移工作的價值。

> [!div class="nextstepaction"]
> [平衡組合](./balance-the-portfolio.md)
