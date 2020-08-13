---
title: Windows 虛擬桌面規劃
description: 使用適用于 Azure 的雲端採用架構來瞭解 Windows 虛擬桌面遷移的最佳作法，以降低複雜度並將遷移程式標準化。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2010
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: d3da947c4b9c22501b21e2cd96f7ca9647f7c205
ms.sourcegitcommit: 949b87bad28d32df84df190160089f01826f3a31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2020
ms.locfileid: "88196390"
---
# <a name="windows-virtual-desktop-planning"></a>Windows 虛擬桌面規劃

Windows 虛擬桌面和部署案例會遵循與其他遷移工作相同的遷移方法。 這種一致的方法可讓「遷移」處理站或現有的遷移小組採用此程式，而不需要變更非技術的需求。

![雲端採用架構遷移方法的圖表。](../../_images/migrate/methodology.png)

## <a name="plan-your-migration"></a>規劃移轉

與其他遷移一樣，您的小組會評估工作負載、加以部署，然後將它們發行給使用者。 不過，Windows 虛擬桌面包含需要在評估工作負載期間審查 Azure 登陸區域的特定需求。 在第一次部署之前，此程式也需要概念證明。

若要建立您的方案，請參閱 Azure DevOps 中現有遷移待處理專案的「 [雲端採用計畫 DevOps」範本](../../plan/template.md) 。 使用範本來建立活動的詳細計畫。

## <a name="next-steps"></a>後續步驟

如需雲端採用旅程的特定元素指引，請參閱：

- [審查您的環境或 Azure 登陸區域](./ready.md)
- [完成 Windows 虛擬桌面概念證明](./proof-of-concept.md)
- [評估 Windows 虛擬桌面遷移或部署](./migrate-assess.md)
- [部署或遷移 Windows 虛擬桌面執行個體](./migrate-deploy.md)
- [將 Windows 虛擬桌面部署發行至生產環境](./migrate-release.md)
