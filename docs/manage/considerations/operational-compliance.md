---
title: 雲端管理中的作業合規性
description: 使用適用于 Azure 的雲端採用架構，瞭解如何維持符合營運承諾的規範。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 5e2bdbc808b6360691c39f4d124cd3dd7ede6c75
ms.sourcegitcommit: 412b945b3492ff3667c74627524dad354f3a9b85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/18/2020
ms.locfileid: "94879730"
---
# <a name="operational-compliance-in-cloud-management"></a>雲端管理中的作業合規性

作業合規性建基於 [清查和可見度](./inventory.md)的專業領域。 作為雲端管理的第一個可採取動作的步驟，此專業領域著重于週期性遙測評論和補救工作， (主動式和被動補救) 。 此專業領域是維護安全性、治理、效能和成本之間平衡的基石。

## <a name="components-of-operations-compliance"></a>作業合規性的元件

維持營運承諾的合規性需要分析、自動化和人工補救。 有效的作業合規性需要在幾個重要的程式中保持一致性：

- 資源一致性
- 環境一致性
- 資源設定一致性
- 更新一致性
- 補救自動化

### <a name="resource-consistency"></a>資源一致性

雲端管理小組可以對營運合規性採取的最有效步驟是在資源組織和標記中建立一致性。 當資源以一致的方式組織及標記時，其他所有作業工作就變得更容易。 如需更深入的資源一致性指導方針，請參閱 [管理方法](../../govern/index.md)。 具體而言，請參閱 [初始治理基礎文章](../../govern/initial-foundation.md) ，以瞭解如何開始開發資源一致性。

### <a name="environment-consistency"></a>環境一致性

建立一致的環境（或登陸區域）是操作合規性的下一個最重要步驟。 當登陸區域一致，並透過自動化工具強制執行時，診斷和解決操作問題會變得更複雜。 如需更深入的環境一致性指引，請參閱雲端採用生命週期的 [準備階段](../../ready/index.md) 。 該階段中的練習有助於建立可重複的程式，以定義和成熟以程式碼為基礎的環境開發的一致程式碼優先方法。

### <a name="resource-configuration-consistency"></a>資源設定一致性

由於它是以治理和準備就緒的方法為基礎，因此雲端管理應包含持續監視的程式，以及遵循資源一致性需求的評估。 當工作負載變更或採用新的版本時，雲端管理程式必須評估任何設定變更，而這些變更並不容易透過自動化進行管制。

當發現不一致的情況時，會透過更新中的一致性來解決某些問題，而其他專案則可以自動補救。

### <a name="update-consistency"></a>更新一致性

方法的穩定性可能會導致更穩定的作業。 但在雲端管理程式中需要進行一些變更。 尤其是，定期修補和效能變更對於減少中斷和控制成本而言是不可或缺的。

<!-- docutune:ignore "a cloud management methodology" -->

成熟雲端管理方法的其中一個價值，是專注于穩定並控制必要的變更。

任何雲端管理基準都應該包含一種排程、控制，以及可能將必要更新自動化的方法。 這些更新應至少包含修補程式，但也可以包含效能、大小調整，以及更新資產的其他層面。

### <a name="remediation-automation"></a>補救自動化

作為雲端管理的增強基準，某些工作負載可能會受益于自動補救。 當工作負載經常遇到無法透過程式碼或架構變更解決的問題時，自動補救有助於降低雲端管理的負擔並提高使用者滿意度。

許多人認為有足夠的問題可以自動解決，應該透過解決技術債務來解決問題。 當您審慎解決長期解析時，它應該是預設選項。 不過，某些商務案例會讓您難以證明技術債務的解決問題。 當這類解決方案無法調整時，但補救是一項常見且昂貴的負擔，而自動補救是下一個最佳的解決方案。

## <a name="next-steps"></a>後續步驟

[保護和](./protect.md) 復原是雲端管理基準中的下一個要考慮的領域。

> [!div class="nextstepaction"]
> [保護和復原](./protect.md)
