---
title: 在移轉前預估雲端成本
description: 瞭解可能會影響決策和執行活動的因素，以及各種評估雲端成本的選項。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: a9996ac0cc1b3ab324fb16b5f03f37adccaa84bb
ms.sourcegitcommit: 7d3fc1e407cd18c4fc7c4964a77885907a9b85c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "80432818"
---
# <a name="estimate-cloud-costs"></a>估計雲端成本

在移轉期間，有幾個因素可能會影響決策和執行活動。 為了協助您了解哪一個選項最適合用於不同的情況，本文將討論用來估計雲端成本的各種選項。

## <a name="digital-estate-size"></a>數位資產大小

您的數位資產大小會直接影響移轉決策。 涉及不到 250 個 VM 的移轉，可能會比涉及超過 10,000 個 VM 的移轉容易預估得多。 強烈建議您選取較小的工作負載作為第一次移轉。 這讓您的小組在嘗試估計較大且較複雜的工作負載移轉之前，有機會先了解如何估計簡單移轉工作的成本。

但請注意，儘管是較小、單一工作負載的移轉，仍可能牽涉到數量差異很大的支援資產。 如果您的移轉涉及不到 1,000 個 VM，則 [Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-overview) 之類的工具應該就足以收集關於清查和預測成本的資料。 [數位資產計算](../../../digital-estate/calculate.md)的相關文章會說明其他成本預估工具選項。

針對1000個以上的單元數位資產，您仍然可以將估計細分成四或五個可操作的反復專案，讓預估程式更容易管理。 對於較大的資產，或是在需要較高水準的預測精確度時，您可能需要更完整的方法，例如「雲端採用架構」的[數位資產](../../../digital-estate/index.md)一節所說明的方法。

## <a name="accounting-models"></a>會計模型

會計模型

如果您熟悉傳統的 IT 採購程序，您可能會覺得雲端中的預估有些陌生。 採用雲端技術時，購買作業會從固定的結構化資本支出模型轉變為流動的營運費用模型。 在傳統的資本支出模型中，IT 小組會嘗試為不同程式的多個工作負載整合購買力，以集中化可支援各個解決方案的共用 IT 資產集區。 在營運費用雲端模型中，成本可直接歸因於個別工作負載、小組或業務單位的支援需求。 這種方法可讓您將成本更直接地歸因於支援的內部客戶。 估計成本時，請務必先瞭解 IT 小組將使用的這項新的會計功能有多少。

如果想要將舊版資本支出方法複寫到會計，請使用上述的「[數位資產大小](#digital-estate-size)」一節中建議的任一種方法的輸出，來取得年度成本基準。 接下來，將該年度成本乘以公司的一般硬體重新整理週期。 硬體更新週期是指公司更換過時硬體的速率，通常以年計算。 每年執行率乘以硬體更新週期，會建立類似於資本支出投資模式的成本結構。

## <a name="next-steps"></a>後續步驟

估計成本之後，即可開始進行移轉。 不過，在開始進行任何移轉之前，最好先檢閱[合作關係和支援選項](./partnership-options.md)。

> [!div class="nextstepaction"]
> [了解合作關係選項](./partnership-options.md)
