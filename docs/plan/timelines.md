---
title: 雲端採用方案中的時程表
description: 使用適用于 Azure 的雲端採用架構，以瞭解如何根據您的雲端採用方案來預估時程表。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.openlocfilehash: e5407f2f78942d22c5fa07b277f600d5440ddc04
ms.sourcegitcommit: 60d8b863d431b5d7c005f2f14488620b6c4c49be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83214016"
---
# <a name="timelines-in-a-cloud-adoption-plan"></a>雲端採用方案中的時程表

在此系列的上一篇文章中，工作負載和工作已指派給[發行和](./iteration-paths.md)反復專案。 這些指派會摘要本文中的時間軸估計。

工作分解結構常用於連續的專案管理工具中。 它們代表相依工作在一段時間內的完成方式。 這類結構在工作的本質上很好用。 在雲端採用中發現的工作相互相關性，使這類結構變得更容易管理。 若要填滿此間隙，您可以藉由隱藏複雜性來估計以反復專案路徑指派為基礎的時間軸。

## <a name="estimate-timelines"></a>預估時間表

若要開發時程表，請從版本開始。 這些發行目標會針對任何業務影響建立目標日期。 反復專案有助於將這些版本與特定的持續時間進行對齊。

如果時間軸需要更細微的里程碑，請使用反復專案指派來表示里程碑。 若要執行這項指派，請假設工作負載相關工作的最後一個實例可以做為最終里程碑。 小組通常也會將最後的工作標記為里程碑。

針對任何層級的資料細微性，請使用反復專案的最後一天做為每個里程碑的日期。 這會將工作負載採用的完成結合到特定日期。 您可以在試算表或順序專案管理工具（例如 Microsoft Project）中追蹤日期。

## <a name="delivery-plans-in-azure-devops"></a>Azure DevOps 中的傳遞計畫

<!-- docsTest:ignore "Microsoft Delivery Plans" -->

如果您使用 Azure DevOps 來管理您的雲端採用方案，請考慮使用[Microsoft 傳遞計畫延伸](https://marketplace.visualstudio.com/items?itemname=ms.vss-plans)模組。 此延伸模組可以快速建立以反復專案和發行指派為基礎之時程表的視覺標記法。
