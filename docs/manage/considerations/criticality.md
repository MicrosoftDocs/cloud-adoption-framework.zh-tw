---
title: 雲端管理的業務重要性
description: 使用適用于 Azure 的雲端採用架構，瞭解工作負載的重要性，並防止對收益和獲利率造成負面影響。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: internal
ms.openlocfilehash: fdb560227db780e02f5b822685610a594a5fcea4
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102114705"
---
# <a name="business-criticality-in-cloud-management"></a>雲端管理的業務重要性

在每個企業中，有少量的工作負載可能太重要而無法成功。 這些工作負載會視為要徑任務。 當這些工作負載發生中斷或效能降低時，對收益和獲利的負面影響可能會在整個公司中感到明顯。

在該頻譜的另一端，某些工作負載可能會在幾個月內進行，而不使用。 這些工作負載的效能或中斷不理想，但影響是隔離且受限的。

瞭解 IT 組合中每個工作負載的重要性，是為雲端管理建立相互承諾的第一步。 下圖說明要遵循的重要性規模和企業所做的標準承諾之間的一般調整。

![重要性和管理層級的對齊](../../_images/manage/cloud-criticality-alignment.png)

## <a name="criticality-scale"></a>重要性規模

任何商務重要性對齊工作的第一個步驟是建立重要性規模。 下表提供用來作為參考或範本的範例尺規，以建立您自己的尺規。

| 重要性 | 商務視圖 |
| --------- | --------- |
| 關鍵任務 |  會影響公司的任務，而且可能會對公司的損益陳述產生顯著的影響。 |
| 單元關鍵性 | 會影響特定業務單位及其收益和損失陳述的任務。 |
| 高 | 可能不會妨礙任務，但會影響高重要性的流程。 在中斷的情況下，可以量化可測量的損失。 |
| 中 | 對處理常式的影響很可能。 損失很低或無數，但品牌損毀或上游損失很可能。 |
| 低 | 無法測量對商務程式的影響。 品牌損毀和上游損失都不太可能。 對單一小組的當地語系化影響很有可能。 |
| 不支援 | 與此工作負載相關聯的商務擁有者、小組或流程，可能會證明對工作負載進行中的管理工作是否有任何投資。 |

企業通常會包含其產業、垂直或特定商務程式特有的其他重要性分類。 其他分類的範例包括：

- **合規性-重大：** 在嚴格管制的產業中，某些工作負載在維護合規性需求方面可能很重要。
- **安全性關鍵：** 某些工作負載可能不會造成任務關鍵性，但中斷可能會導致資料遺失或受保護資訊的非預期存取。
- **安全-重大：** 當員工和客戶的生活或實體安全在中斷期間具有風險時，將工作負載分類為安全關鍵可能是明智的做法。

## <a name="importance-of-accurate-criticality"></a>精確重要性的重要性

在雲端採用程式之後，雲端管理小組會使用此分類來判斷符合一致重要性層級所需的投入量。 在內部部署環境中，作業管理通常會集中購買，並視為必要的業務負擔，幾乎不需要額外的營運成本。 就像所有的雲端服務一樣，作業管理會以每個資產為基礎來購買，以作為每月作業成本。

由於雲端中的作業管理具有明確且直接的成本，因此請務必適當地調整成本和所需的重要性規模。

## <a name="select-a-default-criticality"></a>選取預設的重要性

首次審核組合中的每個工作負載可能相當耗時。 為了確保這種情況不會封鎖您更廣泛的雲端策略，建議您的小組同意將預設的重要性套用至所有工作負載。

根據上述的重要性層級資料表，我們建議您採用 *中等* 重要性作為預設值。 這樣做可讓您的雲端策略小組快速找出需要更高層級重要性的工作負載。

## <a name="use-the-template"></a>使用範本

如果您是使用 [operations management 活頁簿](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx) 來規劃雲端管理，則適用下列步驟。

1. 記錄工作表中的 [重要性] 尺規 `Scale` 。
2. 更新工作表或工作表中的每個工作負載 `Example` `Clean Template` ，以反映資料行中的預設重要性 `Criticality` 。
3. 企業應該輸入正確的值，以反映與預設重要性的任何偏差。

## <a name="next-steps"></a>下一步

當您的小組已定義商務重要性之後，您就可以 [計算並記錄商務衝擊](./impact.md)。

> [!div class="nextstepaction"]
> [計算並記錄商務衝擊](./impact.md)
