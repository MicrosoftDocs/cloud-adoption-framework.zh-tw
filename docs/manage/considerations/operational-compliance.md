---
title: 雲端管理中的營運合規性
description: 使用適用于 Azure 的雲端採用架構，以瞭解如何維持營運承諾的合規性。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 894208ff08a0100e8d5d8d5d9df3eff592773426
ms.sourcegitcommit: 0ea426f2f471eb7310c6f09478be1306cf7bf0d8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2020
ms.locfileid: "78340973"
---
# <a name="operational-compliance-in-cloud-management"></a>雲端管理中的營運合規性

營運合規性是以[清查和可見度](./inventory.md)的專業領域為基礎。 這個專業領域是雲端管理的第一個可行步驟，著重于週期性遙測審查和補救工作（主動和回應式補救）。 這個專業領域是維護安全性、治理、效能和成本之間平衡的基石。

## <a name="components-of-operations-compliance"></a>作業合規性的元件

維護符合營運承諾的合規性需要分析、自動化和人工補救。 有效的作業合規性需要幾個重要程式中的一致性：

- 資源一致性
- 環境一致性
- 資源設定一致性
- 更新一致性
- 補救自動化

### <a name="resource-consistency"></a>資源一致性

雲端管理小組可進行作業合規性的最有效步驟，就是在資源組織和標記中建立一致性。 當資源一致地組織並加上標籤時，其他所有作業工作會變得更容易。 如需更深入的資源一致性指引，請參閱雲端採用週期的[治理階段](../../govern/index.md)。 具體而言，[最初的治理基礎文章](../../govern/initial-foundation.md)會示範如何開始開發資源一致性。

### <a name="environment-consistency"></a>環境一致性

建立一致的環境（或登陸區域）是操作合規性的下一個最重要步驟。 當登陸區域一致並透過自動化工具強制執行時，診斷和解決操作問題會明顯不復雜。 如需有關環境一致性的更深入指引，請參閱雲端採用生命週期的[準備階段](../../ready/index.md)。 該階段中的練習會協助建立可重複的程式，以定義和成熟以程式碼為基礎的環境開發的一致方法。

### <a name="resource-configuration-consistency"></a>資源設定一致性

由於雲端管理是根據治理和準備就緒方法，因此應包含持續進行監視及評估其遵循資源一致性需求的程式。 當工作負載變更或採用新版本時，雲端管理程式一定要評估任何設定變更，而不容易透過自動化進行管制。

發現不一致時，部分會因更新中的一致性而被解決，而其他則可以自動補救。

### <a name="update-consistency"></a>更新一致性

方法的穩定性可能會導致更穩定的作業。 但是雲端管理程式中需要進行一些變更。 特別是，定期修補和效能變更對於減少中斷和控制成本而言是不可或缺的。

成熟雲端管理方法的許多價值之一，是專注于穩定並控制必要的變更。

任何雲端管理基準都應該包含排程、控制及可能將必要更新自動化的方法。 這些更新應至少包含修補程式，但也可以包含效能、調整大小，以及更新資產的其他層面。

### <a name="remediation-automation"></a>補救自動化

作為雲端管理的增強基準，某些工作負載可能會因自動補救而受益。 當工作負載經常遇到無法透過程式碼或架構變更來解決的問題時，自動補救可以協助降低雲端管理的負擔，並提高使用者滿意度。

許多人都認為，任何可自動化的問題都應該透過解決技術債務來解決。 當長期解析非常謹慎時，它應該是預設選項。 不過，許多商務案例都難以證明在解決技術債務時的大量投資。 如果無法證明這種解決方式，但補救是一種常見且昂貴的負擔，則自動補救是下一個最佳解決方案。

## <a name="next-steps"></a>後續步驟

[保護和](./protect.md)復原是雲端管理基準中要考慮的下一個領域。

> [!div class="nextstepaction"]
> [保護與復原](./protect.md)
