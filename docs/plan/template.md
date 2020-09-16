---
title: 將雲端採用方案部署到 Azure DevOps
description: 瞭解如何使用將雲端採用工作與標準化程式保持一致的範本，快速地將待處理專案部署至 Azure DevOps。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.openlocfilehash: 8c5dd7d7e1b40172c6dc996bc865d72ac532f50c
ms.sourcegitcommit: 4da8118cdac560b795d2d413974c85c49b3189fa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2020
ms.locfileid: "90681413"
---
# <a name="cloud-adoption-plan-and-azure-devops"></a>雲端採用方案和 Azure DevOps

Azure DevOps 是一組雲端式工具，適用于管理反復專案的 Azure 客戶。 它也包含用來管理部署管線的工具，以及 DevOps 的其他重要層面。

在本文中，您將瞭解如何使用範本，快速地將待處理專案部署至 Azure DevOps。 此範本會根據雲端採用架構中的指導方針，將雲端採用工作與標準化的流程保持一致。

## <a name="create-your-cloud-adoption-plan"></a>制定雲端採用計劃

若要部署雲端採用方案，請開啟 [Azure DevOps 示範](https://aka.ms/adopt/plan/generator)產生器。 此工具會將範本部署至您的 Azure DevOps 租使用者。 使用此工具需要下列步驟：

1. 確認 [ **選取的範本** ] 欄位設定為 [ **雲端採用方案**]。 如果未選取，請選取 [ **選擇範本** ] 以選擇正確的範本。
2. 從 [ **選取組織** ] 下拉式清單方塊中選取您的 Azure DevOps 組織。
3. 輸入新專案的名稱。 雲端採用方案在部署到您的 Azure DevOps 租使用者時，將會有此名稱。
4. 選取 [ **建立專案** ]，根據策略和方案範本在您的租使用者中建立新專案。 進度列會顯示部署專案的進度。
5. 當部署完成時，請選取 [ **流覽至專案** ] 以查看您的新專案。

建立專案之後，請繼續進行此系列文章，以瞭解如何修改範本，以符合您的雲端採用方案。

如需此工具的其他支援和指引，請參閱 [Azure DevOps Services 示範](/azure/devops/demo-gen)產生器。

## <a name="bulk-edit-the-cloud-adoption-plan"></a>大量編輯雲端採用方案

方案專案部署完成後，您就可以使用 Microsoft Excel 來修改它。 使用 Microsoft Excel 來建立新的工作負載或資產，比使用 Azure DevOps 瀏覽器體驗更容易。

若要準備工作站進行大量編輯，請參閱 [使用 Microsoft Excel 大量加入或修改工作專案](/azure/devops/boards/backlogs/office/bulk-add-modify-work-items-excel?view=azure-devops)。

有些使用者可能會想要使用 Project 來追蹤其工作、建立待處理專案並指派資源。 以下是將 [專案連接至 Azure DevOps](/azure/devops/boards/backlogs/office/create-your-backlog-tasks-using-project?view=tfs-2018)的步驟。

## <a name="use-the-cloud-adoption-plan"></a>使用雲端採用方案

雲端採用方案會依活動類型來組織活動：

- **Epics：**_長篇故事_代表雲端採用生命週期的整體階段。
- **功能：** 功能是用來在每個階段中組織特定的目標。 比方說，特定工作負載的遷移是一項功能。
- **使用者案例：** 使用者案例群組會根據特定目標，進入活動的邏輯集合。
- 工作 **：** 工作是要完成的實際工作。

在每個圖層上，系統會根據相依性排序活動。 活動會連結至雲端採用架構中的文章，以明確說明目標或工作。

雲端採用方案的清晰觀點來自于 **Epics** 待處理專案（backlog）視圖。 如需變更至 **Epics** 待處理專案（backlog）的說明，請參閱關於 [查看待](/azure/devops/boards/backlogs/define-features-epics?view=azure-devops#view-a-backlog-or-portfolio-backlog)處理專案（backlog）的文章。 從這個觀點來看，您可以輕鬆地規劃和管理完成採用生命週期目前階段所需的工作。

> [!NOTE]
> 雲端採用計畫的目前狀態著重于遷移工作。 與治理、創新或作業相關的工作必須手動填入。

## <a name="align-the-cloud-adoption-plan"></a>調整雲端採用方案

策略方法和計畫方法的總覽頁面都參考 [策略和計畫範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx)。 該範本會組織決策和資料點，以配合採用的特定計劃，將雲端採用方案的範本結合在一起。 在調整新專案之前，考慮完成 [策略方法](../strategy/index.md) 和 [計畫方法](../plan/index.md) 中的練習。

下列文章支援雲端採用方案的對齊：

- [工作負載](./workloads.md)：在雲端遷移長篇中對齊功能，以捕獲要遷移或現代化的每個工作負載。 新增和修改這些功能以開始遷移您的前10個工作負載。
- [資產](./assets.md)：每個資產 (虛擬機器、應用程式或資料) 都會以每個工作負載下的使用者案例表示。 新增和修改這些使用者案例，以配合您的數位資產。
- [合理化](./review-rationalization.md)：當每個工作負載都已定義時，該工作負載的初始假設可能會受到挑戰。 這可能會導致每個資產下的工作變更。
- 建立[發行計畫](./iteration-paths.md)：反復專案路徑會將工作與各種版本和反復專案相配合，以建立發行計畫。
- [建立時間](./timelines.md)軸：定義每個反復專案的開始和結束日期會建立時間軸，以管理整個專案。

這五篇文章有助於開始管理您的採用工作所需的每個調整工作。 下一個步驟可讓您開始進行對齊練習。

## <a name="next-steps"></a>後續步驟

藉由 [定義和排列工作負載的優先順序，](./workloads.md)開始調整您的方案專案。

> [!div class="nextstepaction"]
> [定義工作負載並設定其優先順序](./workloads.md)
