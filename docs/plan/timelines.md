---
title: 雲端採用方案中的時程表
description: 使用適用于 Azure 的雲端採用架構，瞭解如何根據您的雲端採用方案來預估時程表。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.custom: internal
ms.openlocfilehash: 527db15029a897f0c4d66b9b1a4aff28d3b60305
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102115997"
---
# <a name="timelines-in-a-cloud-adoption-plan"></a>雲端採用方案中的時程表

在這一系列的前一篇文章中，已將工作負載和工作指派給 [發行與](./iteration-paths.md)反復專案。 這些指派摘要了本文中的時間軸預估。

工作細目結構常用於連續的專案管理工具。 它們代表相依的工作將如何在一段時間內完成。 這類結構適用于本質上的工作順序。 在雲端採用中發現的工作相互相關性，使得這類結構難以管理。 若要填滿此間距，您可以藉由隱藏複雜性，以根據反復專案路徑指派來預估時間軸。

## <a name="estimate-timelines"></a>預估時間表

若要開發時程表，請從發行開始。 這些發行目標會建立任何商務衝擊的目標日期。 反復專案有助於將這些版本與特定時間持續時間對齊。

如果時間軸需要更細微的里程碑，請使用反復專案指派來指出里程碑。 若要進行這項指派，請假設工作負載相關工作的最後一個實例可以作為最終里程碑。 小組通常也會將最後的工作標記為里程碑。

若為任何層級的資料細微性，請使用反覆運算的最後一天做為每個里程碑的日期。 這會將工作負載採用的完成作業與特定日期結合。 您可以使用試算表或依序的專案管理工具（例如 Microsoft Project）來追蹤日期。

## <a name="delivery-plans-in-azure-devops"></a>Azure DevOps 中的傳遞方案

<!-- docutune:casing "Microsoft Delivery Plans" -->

如果您是使用 Azure DevOps 來管理您的雲端採用方案，請考慮使用 [Microsoft 傳遞方案擴充](https://marketplace.visualstudio.com/items?itemname=ms.vss-plans)功能。 此延伸模組可以快速建立以反復專案和發行指派為基礎之時程表的視覺標記法。
