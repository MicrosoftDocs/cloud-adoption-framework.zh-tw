---
title: 數位資產的清查資料
description: 瞭解如何製作支援特定商務功能的 IT 資產清查清單，以供稍後分析和合理化。
author: BrianBlanchard
ms.author: brblanch
ms.date: 12/10/2018
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.custom: internal
ms.openlocfilehash: 40903a7ecb4584eef1fd0bcd1a6497a6c5554d2d
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97022831"
---
# <a name="gather-inventory-data-for-a-digital-estate"></a>收集數位資產的清查資料

開發清查是 [數位資產規劃](./index.md)的第一步。 在此程式中，系統會收集支援特定商務功能的 IT 資產清單，以供稍後分析和合理化。 本文假設最適合用來進行分析的方法最適合進行規劃。 如需詳細資訊，請參閱[數位資產規劃方法](./approach.md)。

## <a name="take-inventory-of-a-digital-estate"></a>清查數位資產

支援數位資產的清查會隨著所需的數位轉型和對應的轉型旅程而改變。

- **雲端遷移：** 我們通常會建議您在雲端遷移期間，從掃描工具收集清查，以建立所有虛擬機器和伺服器的集中清單。 某些工具也可以建立網路對應和相依性，以協助定義工作負載的對齊。

- **應用程式創新：** 在具備雲端功能的應用程式創新工作期間，會從客戶開始進行清查。 對應出客戶從頭到尾的體驗，是不錯的著手之處。 對應到應用程式、Api、資料和其他資產的對應，會建立詳細的清查以進行分析。

- **資料創新：** 具備雲端功能的資料創新工作著重于產品或服務。 清查也包含中斷市場之機會的對應，以及所需的功能。

- **安全性：** 清查提供安全性瞭解，以協助您評估、保護及監視組織的資產。

## <a name="accuracy-and-completeness-of-an-inventory"></a>清查的精確度和完整性

清查在第一個反復專案中很少會完成。 我們強烈建議雲端策略小組與專案關係人和 power users 保持一致，以驗證清查。 可能的話，請使用其他工具（例如網路和相依性分析）來識別正在傳送的流量，但不在清查中的資產。

## <a name="next-steps"></a>後續步驟

清查清查並經過驗證之後，就可以合理化。 清查合理化是數位資產規劃的下一步。

> [!div class="nextstepaction"]
> [合理化數位資產](./rationalize.md)
