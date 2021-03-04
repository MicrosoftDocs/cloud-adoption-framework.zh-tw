---
title: Azure 管理概觀
description: 利用管理 Azure 生產環境所需基本工具的相關資訊，了解適用於 Azure 的雲端採用架構。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.localizationpriority: high
ms.custom: internal, fasttrack-edit, AQC
ms.openlocfilehash: 53ffd13e4968e520874689ca1ad796f90cb8ccb3
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101787454"
---
::: zone target="docs"

# <a name="azure-management-guide-before-you-start"></a>Azure 管理指南：開始之前

::: zone-end

::: zone target="chromeless"

# <a name="before-you-start"></a>開始之前

::: zone-end

Azure 管理指南可協助 Azure 客戶建立管理基準，以制定整個 Azure 的資源一致性。 本指南會概述任何 Azure 生產環境所需的基本工具，尤其是裝載了敏感性資料的環境。 如需詳細資訊、最佳做法，以及與準備雲端環境相關的考量，請參閱雲端採用架構的[移轉整備程度](../index.md)一節。

## <a name="scope-of-this-guide"></a>本指南的範圍

本指南會指導您該如何建立管理基準工具。 其內容也會概述相關辦法，讓您得以延伸基準或建立超越基準的復原能力。

> [!div class="checklist"]
>
> - **清查和可見性：** 建立跨多個雲端的資產清查。 開發對於每個資產執行狀態的可見性。
> - **作業合規性：** 建立控管規定和流程，確保每個狀態都已完成正確設定，並在妥善管理的環境中執行。
> - **保護和復原：** 確保所有受控資產都有受到保護，並可使用基準管理工具加以復原。
> - **增強的基準選項：** 評估可能符合商務需求的常見附加基準。
> - **平台作業：** 使用妥善定義的服務目錄和集中管理的平台來延伸管理基準。
> - **工作負載作業：** 延伸管理基準，以納入對於任務關鍵性工作負載的關注。

## <a name="management-baseline"></a>管理基準

管理基準是一組應套用至每個環境資產的最基本工具和流程。 管理基準中可以納入數個額外的選項。 接下來的幾篇文章將焦點放在所需的最基本選項 (而不是所有可用的選項)，以加速雲端管理功能。

::: zone target="docs"

下一步是[清查和可見性](./inventory.md)。

::: zone-end

::: zone target="chromeless"

本指南會提供互動式步驟讓您試用所推出的功能。 若要回到您先前離開的地方，請使用階層連結來瀏覽。

::: zone-end
