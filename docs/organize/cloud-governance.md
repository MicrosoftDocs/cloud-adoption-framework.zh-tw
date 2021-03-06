---
title: 瞭解雲端治理功能
description: 瞭解雲端治理小組的功能，包括來源、領域和交付專案。
author: JanetCThomas
ms.author: janet
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: organize
ms.custom: internal
ms.openlocfilehash: 7a8aedbf030d9d41cc3491f6f5a1eee37dff3bf5
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101789859"
---
<!-- docutune:ignore IS -->

# <a name="cloud-governance-functions"></a>雲端治理功能

雲端治理小組確保會正確地評估及管理風險和風險承受度。 此小組可確保企業無法承受的風險適當識別。 此小組的人員會將風險轉換成管理公司原則。

根據所需的業務成果，提供完整雲端治理功能所需的技能包括：

- IT 治理
- 企業架構
- 安全性
- IT 作業
- IT 基礎結構
- 網路
- 身分識別
- 虛擬化
- 商務持續性和災害復原
- IT 內的應用程式擁有者
- 財務擁有者

這些基準函數可協助您識別與目前和未來版本相關的風險。 這些工作可協助您評估風險、瞭解潛在的影響，並做出有關風險承受度的決策。 當您這樣做時，請快速更新方案，以反映 [雲端遷移團隊](./cloud-migration.md)的變更需求。

## <a name="preparation"></a>準備

- 請參閱 [管理方法](../govern/index.md)。
- 採用 [治理基準評估](../govern/benchmark.md)。
- [Azure 中的安全性簡介](/learn/modules/protect-against-security-threats-azure/)：瞭解在雲端中保護基礎結構和資料的基本概念。 瞭解什麼是您的責任，以及 Azure 為您處理的工作。
- 瞭解如何跨群組工作以 [管理成本](../organize/cost-conscious-organization.md)。

## <a name="minimum-scope"></a>最小範圍

- 瞭解計畫所引進的 [商務風險](../govern/policy-compliance/risk-tolerance.md) 。
- 代表 [業務的風險容忍度](../govern/policy-compliance/risk-tolerance.md)。
- 協助建立 [治理 MVP](../govern/guides/index.md)。

牽涉到雲端治理活動中的下列參與者：

- 來自中間管理的領導人和關鍵角色的直接參與者，都應該代表企業並協助評估風險承受度。
- 雲端治理功能是由 [雲端策略小組](./cloud-strategy.md)的延伸模組所提供。 就像 CIO 和企業領導人必須參與雲端策略功能一樣，其直屬員工也預期會參與雲端治理活動。
- 身為業務單位成員且與企業營運領導密切合作的商務員工，應能獲得有關公司和技術風險的決策。
- 資訊技術 (IT) 和資訊安全 (是) 瞭解雲端轉型技術層面的員工，可能會以旋轉容量提供服務，而不是雲端治理功能的一致提供者。

## <a name="deliverable"></a>交付項目

雲端治理的任務是要平衡轉換和風險降低的競爭力。 此外，雲端治理可確保 [雲端遷移小組](./cloud-migration.md) 知道資料和資產分類，以及治理採用的架構指導方針。 治理小組或個人也可以與 [卓越的雲端中心](../organize/cloud-center-of-excellence.md) 合作，以運用自動化的方法來管理雲端環境。

**每月進行中的工作：**

- 瞭解每個版本期間引進的 [商務風險](../govern/policy-compliance/risk-tolerance.md) 。
- 代表 [業務的風險容忍度](../govern/policy-compliance/risk-tolerance.md)。
- 協助增加 [原則和合規性需求](../govern/policy-compliance/index.md)的改進。

**會議步調：**

雲端治理小組每個小組成員的承諾用量，將代表每日排程的大量百分比。 投稿將不受限於會議和意見反應週期。

## <a name="out-of-scope"></a>超出範圍

雲端治理小組會隨著採用規模的發展，與創新的步調保持一致。 如果您的環境具有繁重的合規性、作業或安全性需求，則更是如此。 如果發生這種情況，您可以將一些責任轉移給現有的 IT 小組，以降低治理小組的範圍。

## <a name="next-steps"></a>下一步

有些大型組織有專門專注于 IT 治理的專門團隊。 這些團隊會針對 IT 組合的風險管理進行特殊化。 當這些小組存在時，可以快速加速下列成熟度模型。 但我們鼓勵 IT 治理小組複習雲端治理模型，以瞭解治理在雲端中的變化。 重要文章包括將公司原則延伸至雲端以及五個雲端治理專業領域。

**沒有治理：** 組織通常會移至雲端，而不會有任何明確的治理計畫。 在長時間之前，有關安全性、成本、規模和作業的考慮，會開始觸發關於治理模型需求的交談，以及人員將與該模型相關聯的程式。 開始這些交談之前，請務必先克服「無治理」的反模式。 定義公司原則的章節可協助您加速這些交談。

**封鎖治理：** 當安全性、成本、規模和作業的相關問題未獲得解答時，專案和業務目標通常會遭到封鎖。 缺乏適當的控管會在專案關係人和工程師之間產生恐懼、不確定性和不確定。 提早採取行動，以在追蹤中停止這項操作。 在雲端採用架構中定義的兩個治理指南，可協助您從小規模開始，並設定一開始限制原則，以將一段時間的不確定性和成熟治理降至最低。 選擇複雜的企業指南或標準企業指南。

**自願治理：** 在每個企業中，通常會有美麗的可憐蟲了。 這些 gallant 的人都願意開始著手，説明團隊從錯誤中學習。 這通常是治理的啟動方式，尤其是在小型公司中。 這些美麗可憐蟲了修正某些問題，並將雲端採用小組推入一組一致且妥善管理的最佳做法。

這些人的成果比「沒有治理」或「治理封鎖」的案例更好。 雖然應建議其工作，但此方法不應與治理混淆。 適當的治理需要有一種以上的支援來推動一致性，也就是任何良好治理方法的目標。 雲端治理的五個專業領域中的指導方針可協助開發此專業領域。

**雲端保管者：** 此名字標記對於許多特製化早期階段治理的雲端架構設計人員而言，都是一項榮譽。 當治理實務首次開始時，結果會與治理志願者的結果類似。 但有一個基本差異。 雲端保管者已考慮到方案。 在這個成熟階段，小組會花時間清除他們之前的雲端架構設計人員所做的混亂。 但雲端保管者會將該工作與結構完善的公司原則保持一致。 他們也會使用治理工具，就像治理 MVP 中所述的一樣。

雲端保管者和治理自願之間的另一項基本差異是領導性支援。 自願花了更多的時間，因為他們想要學習和進行學習。 雲端保管者會取得領導階層的支援，以降低其每日的職責，以確保定期分配的時間可以投資于改善雲端治理。

**雲端守護者：** 雲端採用小組會將治理實務集中化並接受，因此專門管理治理的雲端架構設計師角色，也是雲端治理小組的角色。 一般來說，更成熟的做法可讓其他主題專家的注意力獲得協助，以協助強化治理實行所提供的保護。

雖然差異很微妙，但在建置以治理為重點的 IT 文化時，這是一個重要的區別。 雲端守護者會清除創新的雲端結構設計師所製造的混亂，而這兩個角色具有原本就會產生摩擦且對立的目標。 雲端守護者可協助保護雲端，讓其他雲端結構設計師可以更快速地移動並減少混亂。

雲端守護者開始使用更先進的治理方法來加速平臺部署，並協助小組自行維護其環境需求，讓他們可以更快地移動。 這些更先進的函式範例可在治理 MVP 的累加式改進中看到，例如安全性基準的改進。

**雲端加速器：** 雲端守護者和雲端監管者會自然地收集腳本和治理工具，以加速部署環境、平臺，甚至是各種應用程式的元件。 除了集中式治理職責之外，策劃和共用這些腳本會針對這些架構設計人員發展高度尊重。

公開共用策劃腳本的治理工作人員有助於更快速地傳遞技術專案，並將治理內嵌到工作負載的架構中。 此工作負載會影響並支援良好的設計模式，將雲端加速器提升至更高等級的治理專家。

**全球治理：** 當組織相依于全球分散的 IT 需求時，各地理位置的營運和治理可能會有明顯的偏差。 業務單位需求甚至是本機資料主權需求，都可能會導致治理最佳做法干擾所需的作業。 在這些案例中，分層治理模型可提供最低限度的一致性和當地語系化的治理。 關於多層治理的文章會提供更深入的資訊，讓您深入瞭解此等級的成熟度。

每一家公司都是獨一無二的，因此其治理需求。 選擇適合您組織的成熟度層級，並使用雲端採用架構來引導做法、流程和工具，以協助您達到此規範。

隨著雲端治理的成熟，小組能夠以更快的步調來採用雲端。 持續的雲端採用工作通常會觸發 IT 營運中的成熟度。 開發雲端營運小組，或與您的雲端營運團隊進行同步處理，以確保治理是營運開發的一部分。

深入瞭解如何啟動 [雲端治理小組](../get-started/team/cloud-governance.md) 或 [雲端營運團隊](../get-started/team/cloud-operations.md)。

在您建立初步的 [雲端治理基礎](../govern/initial-foundation.md)之後，請在 [治理基礎增強功能](../govern/foundation-improvements.md) 中使用這些最佳作法，以獲得採用計畫並避免風險。
