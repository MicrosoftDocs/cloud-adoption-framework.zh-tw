---
title: 雲端採用方案反模式
description: 避免雲端採用規劃反模式，例如選擇透過現代化的取代專案，以及使用錯誤的操作或服務模型。
author: mahia127
ms.author: brblanch
ms.date: 02/19/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.custom: think-tank
ms.openlocfilehash: 6d3063f8ba61ab1d25e9402177e11a01b1d3d25b
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102117268"
---
# <a name="cloud-adoption-plan-antipatterns"></a>雲端採用方案反模式

客戶通常會在規劃雲端採用時遇到反模式，原因有很多：

- 未對齊的作業模型可能會增加上市時間、誤解，以及增加 IT 部門的壓力。
- 當公司認為平臺即服務 (PaaS) 降低成本時，有時會選擇錯誤的服務模型。
- 當組織的架構變更時，可能會產生主要的取代專案。 管理這些專案通常很複雜而且成本更高。

## <a name="antipattern-choose-the-wrong-cloud-operating-model"></a>反模式：選擇錯誤的雲端作業模型

公司的策略性優先順序和其組合的範圍，決定其雲端作業模式。 模型可以有不同類型的責任、登陸區域和焦點。 當模型沒有與公司目標對齊時，可能會產生問題：

- 加快上市時間
- 誤解
- 增加 IT 部門的壓力

### <a name="example-assign-too-much-responsibility-to-a-small-team"></a>範例：將太多責任指派給小型團隊

Corporation 引進了一種作業模型，讓 IT 部門負責在雲端內執行的所有專案。 負責雲端的團隊包含三個人。 這種設定會導致較慢的採用旅程，因為：

- 小組只在完全瞭解其對業務、營運和安全性的影響之後，才核准量值。
- 這些問題不是小組的主要專業領域。

主題專家希望使用雲端服務，因此業務單位會提高壓力。 影子 IT 可能會出現，因為業務單位使用公司信用卡來建立自己的環境。

### <a name="preferred-outcome-compare-models-and-build-a-readiness-plan"></a>慣用結果：比較模型並建立就緒計畫

審視策略性優先順序、組合範圍、需求和條件約束。 藉由 [比較四個最常見的雲端作業模式](../operating-model/compare.md) 與您目前的雲端作業模式，來探索作業模型選項。 識別符合您組織的一或多個雲端作業模型。 然後決定模型。 因為角色會隨著作業模型而變更，所以在移至雲端之前，請先 [建立技能就緒計畫](../plan/adapt-roles-skills-processes.md) 。

## <a name="antipattern-choose-the-wrong-service-model"></a>反模式：選擇錯誤的服務模型

公司有時候會假設 PaaS 解決方案的成本低於基礎結構即服務 (IaaS) 解決方案。 這項假設可能會導致服務模型的選擇不正確。 符合成本的公司通常會在移至雲端的主要原因時，造成這項錯誤，也就是節省成本。 這些公司忘了他們在採用 PaaS 時，也需要變更程式，特別是當他們將特定責任移至雲端提供者時。 切換至 PaaS 導入了協調工作、工程實務和傳遞管線的基本變更。 產生非預期的成本增加和延遲可能會產生。

### <a name="example-choose-paas-over-iaas"></a>範例：透過 IaaS 選擇 PaaS

發行者會啟動程式，將其資料中心遷移至雲端。 主管想要一次將目前的應用程式架構和工具現代化。 原因包括：

- 將成本效率發揮到極致。
- 開發更現代化的應用程式組合。

針對其採用策略，他們選擇透過 IaaS 的 PaaS。 在雲端採用旅程中，一年的採用率較慢。 他們必須變更許多程式、實務和工具，以將 PaaS 視為完整的範圍。 面板看不到與 PaaS 相關的一般影響和優點。 同時，其速度會比以往更慢，而資料中心成本維持不變。

### <a name="preferred-outcome-minimize-disruption-to-your-business"></a>慣用結果：將業務中斷降到最低

若要減少協調工作，請從 IaaS 著手進行初始雲端採用專案。 當您稍後移至雲端，而不是在一開始時，採用新的進程和實務會更容易管理。 先採用 IaaS，特別是在資料中心轉換案例中。 同時，也請啟動雲端技能計畫。

在工作負載已在雲端之後，逐漸現代化並採用 PaaS。 您所獲得的經驗將可協助您更快速地採用 PaaS。 您將需要瞭解現代化的新技能和流程。 您也不會明顯地中斷商務流程。

根據 [雲端合理化](../digital-estate/5-rs-of-rationalization.md)來評估數位資產。 本文說明最常見的遷移和現代化路徑，或五個 Rs：

- 重新裝載
- 重構
- 重新架構
- 重建
- 取代

## <a name="antipattern-replace-architecture"></a>反模式：取代架構

以 PaaS 和軟體即服務 (SaaS) 為基礎的應用程式相當容易維護。 它們通常需要花很多時間來管理。 如此一來，許多公司都會以 SaaS 和雲端原生概念取代舊的複雜架構環境。 此架構變更通常會導致主要的取代專案。 這是管理及執行這些專案的複雜、成本密集的工作。 變更進程和作業模型也牽涉到其他重大風險。

### <a name="example-choose-replacement-over-modernization"></a>範例：選擇取代現代化

公司有大型的 SAP 環境。 IT 部門想要取代這個環境，這會導致數個效能和穩定性問題。 從取代專案開始，用來取代整個環境的到期清單每天會變得更長。

### <a name="preferred-outcome-rationalize-your-digital-estate"></a>慣用結果：合理化您的數位資產

在您取代大型或複雜的應用程式環境之前，請考慮改為現代化以累加方式改進您的環境。 相對較小的應用程式環境變更可能會對效能和可靠性產生巨大的影響。 例如，將裝載平臺變更為 Azure 可以提供穩定性和快速結果。 改進的效能和可靠性結果，以預估的取代成本為一小部分。

在決定創新策略時，請探索不同的現代化選項。 在 (PoC) 的概念證明中評估這些選項。

瞭解您公司的 [數位資產](../digital-estate/index.md)。 判斷哪五個 [合理化](../digital-estate/5-rs-of-rationalization.md) 的最適合現代化或遷移您的資產：

- 重新裝載
- 重構
- 重新架構
- 重建
- 取代

## <a name="next-steps"></a>下一步

- [比較常見的雲端作業模型](../operating-model/compare.md)
- [打造技能就緒計畫](../plan/adapt-roles-skills-processes.md)
- [雲端合理化](../digital-estate/5-rs-of-rationalization.md)
- [什麼是數位資產？](../digital-estate/index.md)