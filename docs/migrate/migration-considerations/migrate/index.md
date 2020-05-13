---
title: 執行移轉
description: 取得說明在 Azure 中，遷移工作負載時可能牽涉到各種活動的文章總覽。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 6325e577453598bee8092ee3ba49c6dc1057c9e3
ms.sourcegitcommit: 60d8b863d431b5d7c005f2f14488620b6c4c49be
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83214220"
---
# <a name="deploy-workloads"></a>部署工作負載

評估工作負載之後，您可以將其部署至雲端或分段發行。 這一系列的文章說明此階段的移轉工作可能涉及的各種活動。

## <a name="objective"></a>目標

移轉的目標是將單一工作負載移轉至雲端。

## <a name="definition-of-_done_"></a>對「完成」  的定義

當工作負載已在雲端中預備完畢且準備好進行測試 (包括工作負載所需的所有相依資產皆已可運作) 時，移轉階段便已完成。 在最佳化程序期間，工作負載會針對生產環境用途進行準備。

視您的測試和發行程序而定，針對「完成」  的定義可能會有所不同。 此系列文章的下一篇文章會涵蓋[升級模型上的決定](./promotion-models.md)，並能協助您了解將已移轉的工作負載升級至生產環境的最佳時機。

## <a name="accountability-during-migration"></a>移轉期間的責任

雲端採用小組必須負責處理整個移轉程序。 不過，雲端策略小組成員也具有一些責任，如下一節所列。

## <a name="responsibilities-during-migration"></a>移轉期間的職責

除了高層級的責任之外，還有個人或群組必須直接負責的動作。 以下是需要指派給負責之合作對象的幾個活動：

- **補救。** 解決會防止工作負載順利移轉至雲端的任何相容性問題。
  - 如在必要條件文章中針對[技術複雜度和變更管理](../prerequisites/technical-complexity.md)所討論的內容所述，應該預先對此活動的執行方式做出決定。 特別是，雲端採用小組是否會在和實際移轉作業相同的短期衝刺期間完成這些補救？ 或者，是否會在個別的反覆項目期間使用波或原廠模型來完成補救工作？ 如果此基本程序問題無法由小組的所有成員提供解答，便應該重新造訪[必要條件](../prerequisites/index.md)一節。
- **複寫。** 在雲端中建立每個資產的複本，以搭配雲端中的資源同步 VM、資料及應用程式。
  - 視升級模型而定，可能會需要不同的工具以完成此活動。
- **預備。** 當工作負載的所有資產皆已複寫及確認之後，該工作負載便可以針對業務變更方案的業務測試及執行進行預備。

## <a name="next-steps"></a>後續步驟

在對移轉程序取得基本認識之後，您便可以開始[決定升級模型](./promotion-models.md)。

> [!div class="nextstepaction"]
> [決定升級模型](./promotion-models.md)
