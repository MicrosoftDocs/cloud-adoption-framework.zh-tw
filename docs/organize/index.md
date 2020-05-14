---
title: 管理組織調整
description: 使用「適用於 Azure 的雲端採用架構」，了解如何建立及維持組織的一致性。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/04/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: organize
ms.custom: organize
ms.openlocfilehash: 4d34515e2e109e77e18a129b490e6c7e1faee593
ms.sourcegitcommit: 60d8b863d431b5d7c005f2f14488620b6c4c49be
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83222057"
---
# <a name="manage-organizational-alignment"></a>管理組織一致性

若沒有妥善組織的人員，就不會有雲端採用。 成功的雲端採用是透過技能熟練的人員執行適當類型的工作、與明確定義的商務目標進行整合，以及在良好管理的環境中交互運作的結果。 若要提供有效的雲端作業模式，建立適當的員工組織結構非常重要。 本文概述了建立和維護適當組織結構的方法，其有四個步驟。

下列練習將協助引導建立登陸區域以支援雲端採用的程序。

<!-- markdownlint-disable MD033 -->

| | |
|---|---|
| ![1](../_Images/icons/1.png)     | <br>[結構類型](#structure-type)：定義最適合您作業模型的組織結構類型。                                |
| ![2](../_Images/icons/2.png)     | <br>[雲端功能](#understand-required-cloud-functions)：了解採用和操作雲端所需的雲端功能。                                |
| ![3](../_Images/icons/3.png)     | <br>[成熟的小組結構](./organization-structures.md)：定義可以提供各種雲端功能的小組。                                |
| ![4](../_Images/icons/4.png)      | [RACI 矩陣](./raci-alignment.md)：明確定義的角色是任何作業模型的重要層面。 使用提供的 RACI 矩陣，將職責、責任、諮詢和知情角色對應到每個小組，以實現雲端作業模型的各種功能。                        |

## <a name="structure-type"></a>結構類型

以下組織結構不一定必須對應到組織圖 (組織圖)。 組織圖通常反應命令和控制管理結構。 相反地，以下組織結構旨在用來擷取角色和責任的一致性。 在敏捷的矩陣組織中，這些結構可能是虛擬團隊 (或 v-teams) 最好的代表。 對於要在組織圖中顯示這些組織結構並沒有任何限制，但沒有必要生成有效的作業模式。

管理組織調整的第一步，是確定如何實現以下組織結構：

- **組織圖調整：** 管理階層、管理員責任和員工調整會與組織結構保持一致。
- **虛擬團隊 (v-teams)：** 管理結構和組織圖會保持不變。 相反地，虛擬團隊將會透過必要的功能建立並指派工作。
- **混合模型：** 更常見的是，組織圖和 v-team 調整的混合模型需要實現轉型目標。

## <a name="understand-required-cloud-functions"></a>了解必要的雲端功能

以下是雲端採用和長期作業模型能夠獲得成功所需的功能清單。 在您熟悉這些功能之後，可以根據人員配置和成熟度對組織結構調整這些功能：

- [雲端策略](./cloud-strategy.md)：針對業務需求來調整技術變更。
- [雲端採用](./cloud-adoption.md)：提供技術解決方案。
- [雲端治理](./cloud-governance.md)：管理風險。
- [集中式 IT](./central-it.md)：現有 IT 人員的支援。
- [雲端作業](./cloud-operations.md)：支援和操作採用的解決方案。
- [雲端卓越中心](./cloud-center-of-excellence.md)：提高採用的品質、速度和彈性。
- [雲端平台](./cloud-platform.md)：操作平台並讓平台更完善。
- [雲端自動化](./cloud-automation.md)：加速採用與創新。
- [雲端安全性](./cloud-security.md)：管理資訊安全性風險。

在某種程度上，上述每項功能在每次雲端採用工作中均有提供，都是明確或根據定義的團隊結構來提供。

隨著採用需求的成長，創造平衡和結構的需求也在增長。 為了滿足這些需求，公司通常會遵循讓組織結構更成熟的過程。

![組織成熟度週期](../_images/ready/org-ready-maturity.png)

[決定組織結構成熟度](./organization-structures.md)一文提供了關於每個成熟度等級的更多細節。

若要隨著時間進行追蹤組織結構決策，請下載並修改 [RACI 試算表範本](https://archcenter.blob.core.windows.net/cdn/fusion/management/raci-template.xlsx)。
