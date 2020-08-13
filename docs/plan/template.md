---
title: 雲端採用方案部署至 Azure DevOps
description: 瞭解如何使用可將雲端採用工作與標準化程式相配合的範本，快速地將待處理專案部署到 Azure DevOps。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
ms.openlocfilehash: b427df31a09444a22a800d751621e7c460627e43
ms.sourcegitcommit: 949b87bad28d32df84df190160089f01826f3a31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2020
ms.locfileid: "88196525"
---
# <a name="cloud-adoption-plan-and-azure-devops"></a>雲端採用方案和 Azure DevOps

Azure DevOps 是一組雲端式工具，適用于管理反復專案的 Azure 客戶。 其中也包含管理部署管線的工具，以及 DevOps 的其他重要層面。

在本文中，您將瞭解如何使用範本，快速地將待處理專案部署到 Azure DevOps。 此範本會根據雲端採用架構中的指導方針，將雲端採用工作與標準化程式保持一致。

## <a name="create-your-cloud-adoption-plan"></a>制定雲端採用計劃

若要部署雲端採用方案，請開啟 [Azure DevOps 示範](https://aka.ms/adopt/plan/generator)產生器。 此工具會將範本部署至您的 Azure DevOps 租使用者。 使用此工具需要下列步驟：

1. 確認 [ **選取的範本** ] 欄位已設定為 [ **雲端採用方案**]。 如果不是，請選取 [ **選擇範本** ] 來選擇正確的範本。
2. 從 [ **選取組織** ] 下拉式清單方塊中選取您的 Azure DevOps 組織。
3. 輸入新專案的 [名稱]。 雲端採用方案部署至您的 Azure DevOps 租使用者時，將會有此名稱。
4. 選取 [ **建立專案** ]，以根據策略和計畫範本，在您的租使用者中建立新的專案。 進度列會顯示部署專案的進度。
5. 當部署完成時，請選取 [ **流覽至專案** ] 以查看您的新專案。

建立專案之後，請繼續進行此文章系列，以瞭解如何修改範本以配合您的雲端採用方案。

如需此工具的其他支援和指引，請參閱 [Azure DevOps Services 示範](https://docs.microsoft.com/azure/devops/demo-gen)產生器。

## <a name="bulk-edit-the-cloud-adoption-plan"></a>大量編輯雲端採用方案

部署計畫專案之後，您可以使用 Microsoft Excel 來進行修改。 使用 Microsoft Excel，在方案中建立新的工作負載或資產，比使用 Azure DevOps 的瀏覽器體驗更容易。

若要準備工作站進行大量編輯，請參閱 [使用 Microsoft Excel 大量加入或修改工作專案](https://docs.microsoft.com/azure/devops/boards/backlogs/office/bulk-add-modify-work-items-excel?view=azure-devops)。

某些使用者可能會想要使用 Project 來追蹤其工作、建立待處理專案和指派資源。 以下是將 [專案連接到 Azure DevOps](https://docs.microsoft.com/azure/devops/boards/backlogs/office/create-your-backlog-tasks-using-project?view=tfs-2018)的步驟。

## <a name="use-the-cloud-adoption-plan"></a>使用雲端採用方案

雲端採用方案會依活動類型來組織活動：

- **Epics：**_長篇故事_代表雲端採用生命週期的整體階段。
- **功能：** 功能是用來組織每個階段內的特定目標。 例如，特定工作負載的遷移是一項功能。
- **使用者故事：** 使用者故事群組會根據特定目標，進入活動的邏輯集合。
- 工作 **：** 工作是要執行的實際工作。

在每個層級，活動會接著根據相依性進行排序。 活動會連結至雲端採用架構中的文章，以明確地闡明目標或工作。

雲端採用方案的清晰視圖來自 **Epics** 待處理專案（backlog）。 如需變更至 **Epics** 待處理專案（backlog）視圖的說明，請參閱關於 [查看待](https://docs.microsoft.com/azure/devops/boards/backlogs/define-features-epics?view=azure-devops#view-a-backlog-or-portfolio-backlog)處理專案（backlog）。 從這個觀點來看，您可以輕鬆地規劃及管理完成採用生命週期目前階段所需的工作。

> [!NOTE]
> 雲端採用方案的目前狀態著重于遷移工作。 與治理、創新或作業相關的工作必須手動填入。

## <a name="align-the-cloud-adoption-plan"></a>配合雲端採用方案

雲端採用週期的策略和規劃階段的總覽頁面，每個都參考 [策略和計畫範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx)。 該範本會組織決策和資料點，以配合採用的特定計劃，將雲端採用方案的範本納入考慮。 如果您還沒有這麼做，您可能想要先完成與 [策略](../strategy/index.md) 和 [規劃](../plan/index.md) 相關的練習，再調整您的新專案。

下列文章支援雲端採用方案的對齊：

- [工作負載](./workloads.md)：調整雲端遷移長篇中的功能，以捕捉要遷移或現代化的每個工作負載。 新增和修改這些功能，以獲取遷移前10個工作負載的工作。
- [資產](./assets.md)：每個資產 (VM、應用程式或資料) 會由每個工作負載下的使用者故事表示。 新增和修改這些使用者故事，使其符合您的數位資產。
- [合理化](./review-rationalization.md)：由於每個工作負載都已定義，因此關於該工作負載的初始假設可能會受到質疑。 這可能會導致每個資產下的工作變更。
- 建立[發行計畫](./iteration-paths.md)：反復專案路徑會藉由使用各種版本和反復專案來協調工作，以建立發行計畫。
- [建立時程表](./timelines.md)：定義每個反復專案的開始和結束日期時，會建立時程表來管理整體專案。

這五篇文章可協助您開始管理您的採用成果所需的每個對齊工作。 下一個步驟可讓您開始進行對齊練習。

## <a name="next-steps"></a>後續步驟

藉由 [定義工作負載並排定其優先順序，](./workloads.md)開始調整您的計畫專案。

> [!div class="nextstepaction"]
> [定義工作負載並設定其優先順序](./workloads.md)
