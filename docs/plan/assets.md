---
title: 將資產對應至優先順序的工作負載
description: 使用適用于 Azure 的雲端採用架構，以瞭解如何將資產與優先順序的工作負載進行調整。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.openlocfilehash: 7cefe7d64dbda13321b362f298c1ac45aa99ebcc
ms.sourcegitcommit: ea63be7fa94a75335223bd84d065ad3ea1d54fdb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/27/2020
ms.locfileid: "80354936"
---
# <a name="align-assets-to-prioritized-workloads"></a>將資產對應至優先順序的工作負載

工作負載是資產集合的概念描述： Vm、應用程式和資料來源。 前一篇文章[定義和設定優先順序](./workloads.md)，提供收集將定義工作負載之資料的指引。 在遷移之前，該清單中的幾項技術輸入需要額外的驗證。 本文可協助驗證下列輸入：

- **應用程式：** 列出此工作負載中包含的任何應用程式。
- **Vm 和伺服器：** 列出工作負載中包含的任何 Vm 或伺服器。
- **資料來源：** 列出工作負載中包含的任何資料來源。
- 相依性 **：** 列出工作負載中未包含的任何資產相依性。

有數個選項可組合此資料。 以下是幾個最常見的方法。

## <a name="alternative-inputs-migrate-modernize-innovate"></a>替代輸入：遷移、現代化、創新

前幾個資料點的目標是要將相關的技術工作和相依性視為優先處理的協助。 視您想要的轉換而定，您可能需要收集替代的資料點，以支援適當的優先順序。

**遷移：** 針對單純的遷移工作，現有的清查和資產相依性可作為相對複雜度的公平測量。

**現代化：** 當工作負載的目標是將應用程式或其他資產現代化時，這些資料點仍然是複雜的複雜性測量。 不過，將現代化商機的輸入新增至工作負載檔可能是明智的做法。

**創新：** 當資料或商務邏輯在雲端採用工作期間進行實質性變更時，會將其視為*創新*的轉換類型。 當您要建立新的資料或新的商務邏輯時，也是如此。 針對任何創新案例，資產的遷移可能代表所需的最小工作量。 在這些情況下，小組應該設計一組技術資料輸入來測量相對複雜度。

## <a name="azure-migrate"></a>Azure Migrate

Azure Migrate 提供一組群組函式，可加速應用程式、Vm、資料來源和相依性的匯總。 在概念上定義工作負載之後，您可以使用它們作為根據相依性對應來分組資產的基礎。

Azure Migrate 檔提供[如何根據相依性將機器分組](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies)的指引。

## <a name="configuration-management-database"></a>設定-管理資料庫

有些組織在現有的作業管理工具中，擁有妥善維護的設定管理資料庫（CMDB）。 他們也可以使用 CMDB 來提供稍早所討論的輸入資料點。

## <a name="next-steps"></a>後續步驟

根據資產對齊和工作負載定義來[審查合理化決策](./review-rationalization.md)。

> [!div class="nextstepaction"]
> [審查合理化決策](./review-rationalization.md)
