---
title: 雲端採用架構移轉模型
description: 雲端採用架構移轉模型
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: baeac7d27ff7aa71c011a2578968811820ebe22e
ms.sourcegitcommit: 959cb0f63e4fe2d01fec2b820b8237e98599d14f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/11/2020
ms.locfileid: "79094752"
---
# <a name="cloud-adoption-framework-migration-model"></a>雲端採用架構移轉模型

雲端採用架構的此小節說明其移轉模型背後的原理。 此內容盡可能嘗試保持廠商中立的立場，同時逐步引導您進行雲端移轉的程序和活動，無論您選擇的雲端廠商是哪個皆適用。

## <a name="understand-migration-motivations"></a>了解移轉動機

雲端移轉巧妙地偽裝成技術實作，但其實是組合管理工作。 在移轉程序期間，您會決定移動一些資產、投資其他資產，以及淘汰過時或未使用的資產。 在此程序中的一部分，一些資產會被最佳化、重構或完全取代。 這些決策的每個都應該配合您雲端移轉背後的動機。 最成功的移轉還會進一步將這些決策配合所要的業務成果。

雲端採用架構移轉模型有賴您的組織已完成雲端採用業務整備程序。 請務必檢閱雲端採用架構中的[規劃](../../strategy/index.md)和[準備](../../ready/index.md)指導方針，以決定雲端移轉的商業誘因或其他理由，以及所需的組織規劃或大規模執行移轉程序之前所需的訓練。

> [!NOTE]
> 雖然業務規劃很重要，成長心態也同樣重要。 在雲端策略小組進行更廣泛的業務規劃工作期間，建議雲端採用小組應同時開始移轉第一個工作負載，作為更大規模移轉工作的先驅。 此初始移轉可讓小組獲得移轉中所牽涉的業務和技術問題的實務經驗。

## <a name="envision-an-end-state"></a>構想最終狀態

開始進行移轉工作之前，請務必建立最終狀態的粗略願景。 下圖顯示基礎結構、應用程式和資料的內部部署起點，此起點定義您的「數位資產」  。 在移轉程序期間，那些資產是使用[合理化的五 R 策略](../../digital-estate/5-rs-of-rationalization.md)中所述五個移轉策略的其中一個來轉換。

![移轉選項的資訊圖](../../_images/migrate/migration-options.png)

移轉和現代化工作的範圍涵蓋使用基礎結構即服務 (IaaS) 功能 (不需要程式碼和應用程式變更) 的簡單「重新裝載」  (也稱為「隨即移轉」  )、包含少量變更的「重構」  ，以及為了修改和擴充程式碼與應用程式功能進行「重新建構」  ，以利用雲端技術。

雲端原生策略和平台即服務 (PaaS) 策略使用 Azure 平台供應項目和受控服務，來「重建」  內部部署工作負載。 在移轉程序中的一部分，有對等完全受控軟體即服務 (SaaS) 雲端式供應項目的工作負載，通常可由這些服務完全「取代」  。

> [!NOTE]
> 在雲端採用架構的公開預覽期間，架構的此小節強調重新裝載策略。 雖然在可用時，PaaS 和 SaaS 解決方案會作為替代方案討論，但使用 IaaS 的虛擬機器型工作負載移轉是主要焦點。
>
> 此內容的其他章節和未來反覆項目將會以其他方法擴充。 如需擴充您的移轉範圍的概略討論，以包含更複雜的移轉策略，請參閱《平衡組合》一文。

## <a name="incremental-migration"></a>累加移轉

雲端採用架構移轉模型是以累加雲端轉換程序為基礎。 它假設您的組織會從範圍有限的初始雲端移轉工作 (通常稱為第一個工作負載) 開始。 此工作會隨著您的營運小組精簡並改善移轉程序，而反覆擴充以包含更多工作負載。

[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) 等雲端移轉工具可遷移由數以萬計 VM 組成的整個資料中心。 不過，企業和現有 IT 作業很少能處理這種步調快速的變更。 因此，許多組織將移轉工作分解成多個反覆項目，每個反覆項目都移動一個工作負載 (或工作負載集合)。

此累加模型背後的原理是根據執行程序和下列資訊圖中的必要條件參考。

![雲端採用架構移轉模型](../../_images/migrate/methodology.png)

這些原則的一致應用是代表您雲端移轉程序的最終目標，不應將它視為必要的起始點。 隨著您的移轉工作成熟，請參考此小節中的指導方針，以協助定義支援您組織需求的最佳程序。

## <a name="next-steps"></a>後續步驟

藉由[調查移轉的必要條件](./prerequisites/index.md)，開始學習此模型。

> [!div class="nextstepaction"]
> [移轉的必要條件](./prerequisites/index.md)
