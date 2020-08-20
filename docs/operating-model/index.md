---
title: 了解雲端作業模型
description: 瞭解雲端作業模型，以及其對您的雲端採用策略有何影響。
author: BrianBlanchard
ms.author: brblanch
ms.date: 08/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.custom: operating-model
ms.openlocfilehash: e1c5a12ce47474ccdbfe65c7faa92454b470addd
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88574783"
---
<!-- docsTest:ignore GRC -->
<!-- cspell:ignore reimagine -->

# <a name="understand-cloud-operating-models"></a>了解雲端作業模型

採用雲端可建立機會來重新審視您操作技術系統的方式。 本文系列將澄清雲端作業模式，以及影響您雲端採用策略的考量。 但首先，讓我們來澄清*雲端做業模型*這個字詞。

## <a name="define-your-operating-model"></a>定義您的作業模型

在部署您的雲端架構之前，請務必瞭解您要如何在雲端中作業。 瞭解您的策略方向、人員組織和治理、風險和合規性 (GRC) 需求，可協助定義您的未來狀態雲端作業模型。 接著，Azure 登陸區域可以提供各種不同的架構和執行選項，以支援您的作業模型。 接下來的幾篇文章將分享幾個基本的字詞，並根據實際的客戶經驗提供常見作業模型的範例，這兩者共同可以引導您決定要實作的正確 Azure 登陸區域。

## <a name="what-is-an-operating-model"></a>什麼是作業模型？

在雲端技術存在之前，技術小組已建立作業模型來定義技術支援企業的方式。 任何公司的 IT 營運模式都有幾項因素，但仍保持一致：*與商務策略、人員的組織、變更管理 (或採用流程)、作業管理、治理/合規性和安全性*的一致性。 每個因素都是長期技術作業不可或缺的一項。

當技術作業轉移至雲端時，這些重要的流程仍然相關，但可能會以某些方式變更。 目前的作業模型，主要著重在實際透過資本支出週期投資的實體資產。 這些資產是用來支援企業維護商務營運所需的工作負載。 大部分作業模型的任務是藉由投資基礎實體資產的穩定性，為工作負載的穩定性設定優先順序。

## <a name="how-is-a-cloud-operating-model-different"></a>雲端作業模型有何不同？

硬體堆疊中的備援是永不結束的迴圈。 實體硬體中斷。 效能降低。 硬體的降低很少會與組織資本支出規劃週期的可預測預算週期一致。 在雲端中作業會將焦點轉移到數位資產，藉以中斷硬體重新整理和午夜修補程式的跑步機：作業系統、應用程式和資料。 從實體轉換成數位也會轉移技術作業模型。

當您的作業模型轉移到雲端時，您仍然需要相同的人員和流程，但他們也會將焦點放在更高層級的作業上。 如果您的人員不再專注於伺服器執行時間，則他們的成功計量將會變更。 如果安全性不再受資料中心的四個牆保護，則您的威脅設定檔會變更。 當採購不再是創新的絆腳石時，您管理變更的步調也會變更。

「雲端作業模型」是流程和程序的集合，可定義您想要如何在雲端作業的技術。

## <a name="purpose-of-a-cloud-operating-model"></a>雲端作業模型的用途

當硬體被當作最常見的作業單位來移除時，焦點就會轉移到數位資產及其支援的工作負載。 因此，作業模型的目的是從保持燈的狀態轉移，以確保作業一致。

[Microsoft Azure Well-Architected Framework](/azure/architecture/framework/) 會在一組常見的架構原則中，分解工作負載考量的絕佳工作：成本最佳化、營運卓越、效能效率、可靠性及安全性。

當移至更高層級的作業時，這些常見的架構原則有助於重構雲端作業模型的目的。 我們如何確保組合中的所有資產和工作負載都能平衡這些架構原則？ 調整這些原則的應用程式所需的流程為何？

## <a name="reimagine-your-operating-model"></a>重新構想您的作業模型

如果您已更新作業模型，以移除實體資產的每個採購、變更、作業或保護的參考，剩下的內容為何？ 針對某些組織，其作業模型現在是全新的平板電腦。 對於大部分的組織而言，過去幾年來開發的條件約束現在會縮減。 不論是哪一種情況，都有機會思考您想要如何在雲端作業。

為了協助您想像未來的狀態作業模型，這些文章討論了以下主題：

- [定義您的雲端作業模型](./define.md)
- [比較常見的雲端作業模型](./compare.md)
- [使用 Azure 登陸區域來執行您的作業模型](../ready/landing-zone/implementation-options.md)

## <a name="next-steps"></a>後續步驟

瞭解雲端採用架構如何協助您定義作業模型。

> [!div class="nextstepaction"]
> [比較常見的雲端作業模型](./compare.md)
