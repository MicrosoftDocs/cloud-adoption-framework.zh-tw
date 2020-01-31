---
title: 解除委任已淘汰的資產
description: 解除委任已淘汰的資產
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 933a9c42a55e58e5a58f9ef1c308b006e30e1abf
ms.sourcegitcommit: 2362fb3154a91aa421224ffdb2cc632d982b129b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/28/2020
ms.locfileid: "76801931"
---
# <a name="decommission-retired-assets"></a>解除委任已淘汰的資產

將工作負載升階至生產環境之後，就不再需要以先前裝載生產工作負載的資產來支援商務作業。 此時，較舊的資產會被視為已淘汰。 您可以將淘汰後的資產可以解除委任，以降低營運成本。 解除委任資源的方式很簡單，只要關閉資產的電源並適當處置資產即可。 可惜的是，解除委任資源有時可能會產生不當的結果。 下列指引可協助您適當地解除委任已淘汰的資源，且盡可能不造成業務中斷。

## <a name="cost-savings-realization"></a>實現成本節約

如果節省成本是移轉的主要動機，解除委任將是重要步驟。 資產在解除委任之前，會繼續取用電力、環境支援，以及其他會推升成本的資源。 解除委任資產之後，就可以開始實現成本節約。

## <a name="continued-monitoring"></a>持續監視

在移轉的工作負載升階之後，應繼續監視要淘汰的資產，以驗證不會有額外的生產流量路由傳送至錯誤的資產。

## <a name="testing-windows-and-dependency-validation"></a>測試時間範圍和相依性驗證

即使有最理想的規劃，生產工作負載仍可能包含對認定已淘汰之資產的相依性。 在這種情況下，關閉已淘汰的資產可能會導致非預期的系統失敗。 因此，在處理任何資產的終止時，應採用與系統維護活動相同的嚴格等級。 您應建立適當的測試和中斷時間範圍，以加速終止資源。

## <a name="holding-period-and-data-validation"></a>保存期間和資料驗證

在移轉的複寫過程中，遺漏資料並非罕見的情形。 非定期使用的舊資料更是如此。 在已淘汰的資產關閉後，將資產保留一段時間作為資料的暫時備份，是明智的做法。 公司在終結已淘汰的資產之前，應至少有 30 天的保存和測試期間。

## <a name="next-steps"></a>後續步驟

已淘汰的資產解除委任後，移轉即完成。 您可以藉此機會改善移轉程序，而[回顧](./retrospective.md)可以讓雲端採用小組檢討發行過程，以期能學習並改善。

> [!div class="nextstepaction"]
> [回顧](./retrospective.md)
