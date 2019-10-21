---
title: 雲端採用方案中的時程表
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 雲端採用方案中的時程表
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.openlocfilehash: c5bafd4504a0df9570bf65846a5a077e54e158ca
ms.sourcegitcommit: 35c162d2d09ec1c4a57d3d57a5db1d56ee883806
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2019
ms.locfileid: "72549022"
---
# <a name="timelines-in-a-cloud-adoption-plan"></a>雲端採用方案中的時程表

在此系列的上一篇文章中，工作負載和工作已指派給[發行和](./iteration-paths.md)反復專案。 這些指派會摘要本文中的時間軸估計。

工作分解結構（WBS）通常會用於順序專案管理工具中。 它們代表相依工作在一段時間內的完成方式。 這類結構在工作的本質上很好用。 在雲端採用中發現的工作相互相關性，使這類結構變得更容易管理。 若要填滿此間隙，您可以藉由隱藏複雜性來估計以反復專案路徑指派為基礎的時間軸。

## <a name="estimate-timelines"></a>估計時間軸

若要開發時程表，請從版本開始。 這些發行目標會針對任何業務影響建立目標日期。 反復專案有助於將這些版本與特定的持續時間進行對齊。

如果時間軸需要更細微的里程碑，請使用反復專案指派來表示里程碑。 若要執行這項指派，請假設工作負載相關工作的最後一個實例可以做為最終里程碑。 小組通常也會將最後的工作標記為里程碑。

針對任何層級的資料細微性，請使用反復專案的最後一天做為每個里程碑的日期。 這會將工作負載採用的完成結合到特定日期。 您可以在試算表或順序專案管理工具（例如 Microsoft Project）中追蹤日期。

## <a name="delivery-plans-in-azure-devops"></a>Azure DevOps 中的傳遞計畫

如果您使用 Azure DevOps 來管理您的雲端採用方案，請考慮使用[Microsoft 傳遞計畫](https://marketplace.visualstudio.com/items?itemName=ms.vss-plans)延伸模組。 此延伸模組可以快速建立以反復專案和發行指派為基礎之時程表的視覺標記法。
