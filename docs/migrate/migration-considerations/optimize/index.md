---
title: 將已移轉的工作負載最佳化
description: 將已移轉的工作負載最佳化
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: aed8ba9d97cfbc236d378066569b466302b66239
ms.sourcegitcommit: 72a280cd7aebc743a7d3634c051f7ae46e4fc9ae
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2020
ms.locfileid: "78225438"
---
# <a name="optimize-migrated-workloads"></a>將已移轉的工作負載最佳化

在將工作負載及其支援資產移轉至雲端後，必須對該工作負載進行升級至生產環境的準備。 此程序中的活動會準備工作負載，評估相依資產的大小，然後協助企業為已移轉的雲端式工作負載做好用於生產環境用途的準備。

最佳化的目的是準備好已移轉的工作負載，以升級至生產環境用途。

## <a name="definition-of-done"></a>對「完成」  的定義

當工作負載已正確設定、指定大小，並且部署至生產環境之中時，表示最佳化程序已完成。

## <a name="accountability-during-optimization"></a>最佳化期間的責任

雲端採用小組必須負責處理整個最佳化程序。 不過，雲端策略小組、雲端作業小組以及雲端治理小組的成員也應為此程序內的活動負責。

## <a name="responsibilities-during-optimization"></a>最佳化期間的職責

除了高層級的責任之外，還有個人或群組必須直接負責的動作。 以下是需要指派給負責之合作對象的幾個活動：

- **業務測試。** 解決會防止工作負載順利移轉至雲端的任何相容性問題。
  - 企業內的進階使用者應深入參與對已移轉工作負載的測試。 視所嘗試的最佳化程度而定，可能需要進行多個測試循環。
- **業務變更方案。** 基於移轉工作的結果而開發使用者採用方案、對業務程序進行變更，以及修改業務 KPI 及學習計量。
- **建立基準並加以最佳化。** 對業務測試和自動化測試進行研究，以建立效能基準。 雲端採用小組會根據使用方式調整已部署的資產大小，以依據預期的生產需求平衡成本與效能。
- **針對生產環境進行準備。** 針對工作負載持續的生產環境用途準備工作負載和環境。
- **升級。** 將生產環境流量重新導向至已移轉且已最佳化的工作負載。 此活動代表發行週期的完成。

除了核心活動之外，還有幾個平行活動需要特定的指派和執行計畫：

- **解除委任。** 一般而言，可以透過將先前的生產環境資產解除委任並正確處置，以實現透過移轉來節省成本的目的。
- **追溯性。** 每個發行都能提供進行深度學習，以及採納成長心態的機會。 當每個發行週期完成之後，雲端採用小組應評估在移轉期間所使用的程序，以找出可改善的區塊。

## <a name="next-steps"></a>後續步驟

在對最佳化程序取得基本認識之後，您便可以[針對候選工作負載建立業務變更方案](./business-change-plan.md)來開始該程序。

> [!div class="nextstepaction"]
> [業務變更方案](./business-change-plan.md)
