---
title: Azure 移轉最佳做法
description: Azure 移轉最佳做法簡介
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 5cc4a0cfdfe605fdd374055e782db2f0ce47c13f
ms.sourcegitcommit: 2362fb3154a91aa421224ffdb2cc632d982b129b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/28/2020
ms.locfileid: "76807286"
---
# <a name="azure-migration-best-practices"></a>Azure 移轉最佳做法

Azure 提供數個 Azure 工具，幫助您執行移轉工作。 本節的雲端採用架構設計可協助實作這些工具，配合移轉最佳做法使用。 這些最佳做法會配合下圖所示雲端採用架構移轉模型內的其中一個流程進行。

展開左側目錄的任何流程，查看通常在進行該流程期間所需的最佳做法。

![雲端採用架構移轉模型](../../_images/operational-transformation-migrate.png)

> [!NOTE]
> 數位資產規劃和資產評量，代表兩個不同的移轉規劃和評量層級：
>
> - **數位資產規劃：** 在規劃期間規劃或合理化數位資產，以建立整體移轉待辦項目。 不過，在移轉工作負載之前，必須先驗證此計畫所根據的一些假設和詳細資料。
> - **資產評量：** 在移轉工作負載之前，先評估工作負載的個別資產，以評估雲端相容性，並了解架構和大小的條件約束。 此流程會驗證初始假設，並提供移轉個別資產所需的詳細資料。
