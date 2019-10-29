---
title: Azure 創新指南：針對客戶意見反應做好準備
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 針對客戶意見反應做好準備
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.custom: fasttrack-edit, AQC
ms.localizationpriority: high
ms.openlocfilehash: ec17c66fa92abc292a19a8e74c215111d8638a05
ms.sourcegitcommit: 910efd3e686bd6b9bf93951d84253b43d4cc82b5
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2019
ms.locfileid: "72769373"
---
::: zone target="docs"

# <a name="azure-innovation-guide-prepare-for-customer-feedback"></a>Azure 創新指南：針對客戶意見反應做好準備

::: zone-end

::: zone target="chromeless"

# <a name="prepare-for-customer-feedback"></a>針對客戶意見反應做好準備

::: zone-end

創新能否成功，關鍵在於使用者的採用、參與和保留。 原因為何？

要建置新的創新解決方案，不只是為使用者提供其所需或自認為所需的項目而已。 重點是要列出可加以測試和改善的假設。 該項測試有兩種形式：定量 (測試意見反應)，意思是我們希望看到的動作。 定性 (客戶意見反應)，讓我們知道這些計量在客戶心目中所代表的意義。 這兩種形式的測試結合在一起，可讓我們知道下一個假設以及解決方案的下一次改善方式。 這些意見反應迴圈是[「建置-測量-學習」流程](../considerations/adoption.md)的基礎，而此流程則是這項方法的核心。

在整合意見反應迴圈之前，您必須為解決方案準備好共用存放庫。 集中式存放庫可讓您記錄所傳來的專案意見反應，並對這些意見反應採取行動。  [GitHub](https://github.com/) 是開放原始碼軟體的居所。 其也是其中一個最常用來裝載商業開發應用程式所用原始程式碼存放庫的平台。 關於[建置 GitHub 存放庫](https://docs.microsoft.com/azure/devops/pipelines/repos/github?view=azure-devops&tabs=yaml)的文章可協助您開始使用您的存放庫。

下列每個 Azure 工具都會與 GitHub 中裝載的專案整合 (或相容)：

## <a name="quantitative-feedback-for-web-appstabquantitative-apps"></a>[適用於 Web 應用程式的定量意見反應](#tab/Quantitative-Apps)

Application Insights 是一種監視工具，可針對應用程式的使用量提供近乎即時的定量意見反應。 此意見反應有助於測試和驗證您目前的假設，以形成待辦項目上的下一個功能或使用者劇本。

### <a name="action"></a>動作

若要檢視關於應用程式的定量資料：

1. 移至 [App Insights]  。
2. 如果應用程式未出現在清單中，請按一下 [新增 +]  連結，並遵循提示來開始設定 App Insights。
3. 如果所需的應用程式已在清單上，則請選取該應用程式。
4. [概觀]  窗格中會有一些關於應用程式的統計資料。 [應用程式儀表板]  連結可讓您建置自訂儀表板，以查看與假設比較有關的資料。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/microsoft.insights%2Fcomponents]" submitText="Go to App Insights" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

::: zone target="docs"

若要檢視關於應用程式的資料，請移至 [Azure 入口網站](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/microsoft.insights%2Fcomponents)。

::: zone-end

### <a name="learn-more"></a>深入了解

- [設定 Azure 監視器](https://docs.microsoft.com/azure/azure-monitor/learn/quick-monitor-portal)
- [開始使用 Azure 監視器 App Insights](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-users)
- [建置遙測儀表板](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-app-dashboards)

## <a name="quantitative-feedback-for-apistabquantitative-apis"></a>[適用於 API 的定量意見反應](#tab/Quantitative-APIs)

連線經濟改變了企業的創新方式。  市場和產業遭遇動盪的速度比以往還快。 動盪的形式很多，企業必須奮力對抗 **「創新者困境」** ：如何設定變更步調，又不至於在持續的商業活動上摔一跤。

對外，企業會使用 API 來變更其與客戶和合作夥伴的互動方式。 對內，企業會使用 API 來順暢地連接企業的各個組成部分。 API 經濟的四個營運用建置組塊為：社交、行動、分析和雲端。 應用程式和服務可以透過符合成本效益的方式快速地連結起來，以產生更廣大的價值主張。

<!-- markdownlint-disable MD024 -->

### <a name="action"></a>動作

若要記錄關於 API 的定量資料：

1. 移至 [API 管理服務]  。
2. 從清單中選取所需的 API。
3. 從 [監視]  區段選擇 [診斷設定]  。

若要檢視關於 API 的定量資料：

1. 移至 [API 管理服務]  。
2. 從清單中選取所需的 API。
3. 從 [監視]  區段中選擇 [應用程式]  。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.ApiManagement%2Fservice]" submitText="Go to API Management Service" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

::: zone target="docs"

若要開啟 API 管理服務，請移至 [Azure 入口網站](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.ApiManagement%2Fservice)。

::: zone-end

### <a name="learn-more"></a>深入了解

本文可協助您使用 [Azure 監視器來取得關於 API 的意見反應](https://docs.microsoft.com/azure/api-management/api-management-howto-use-azure-monitor)

## <a name="qualitative-feedbacktabqualitative"></a>[定性意見反應](#tab/Qualitative)

您可以在待辦項目 (或面板) 將意見反應記錄為使用者劇本。 您也可以在這裡以可採取動作的工作形式來追蹤相關工作。 Azure Boards 可以直接整合到 GitHub 中，以便在意見反應-工作管理和任何原始程式碼之間獲得順暢的體驗。

::: zone target="docs"

### <a name="action"></a>動作

Azure Boards 和 Azure Pipelines 需要來自 GitHub 和 Azure 的不同入口網站。
若要開始使用任一工具，請移至 [Azure DevOps](https://dev.azure.com/)

::: zone-end

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

### <a name="action"></a>動作

若要建立 DevOps 專案：

1. 移至 [Azure DevOps 專案]  。
2. 按一下 [建立 DevOps 專案]  。
3. 選擇 [執行階段、架構和服務]  。

::: form action="OpenBlade[#blade/HubsExtension/BrowseResource/resourceType/microsoft.visualstudio%2Faccount%2Fproject]" submitText="Go to Azure DevOps Project" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

### <a name="learn-more"></a>深入了解

下列連結可協助您使用 Azure Boards 搭配 GitHub 來集中和管理意見反應：

- [開始使用 Azure Boards](https://docs.microsoft.com/azure/devops/boards/boards/kanban-quickstart?view=azure-devops)
- [Azure Boards 和 GitHub](https://docs.microsoft.com/azure/devops/boards/boards/kanban-quickstart?view=azure-devops)

## <a name="close-the-loop-with-pipelinestabpipelines"></a>[使用管線關閉迴圈](#tab/pipelines)

對意見反應採取行動不一定會導致客戶所要求的功能。 但是，每個資料點都應該會導致某些變更。 該變更可能是您對事物的想法。 也可能是與所要求功能完全不同的技術變更。 無論是哪種情況，部署管線和工具 (如 Azure Pipelines) 都可讓您快速發佈這些變更，以便經常與客戶分享。

若要查看與 Azure 中所裝載應用程式有關的任何使用中部署：

### <a name="action"></a>動作

若要在管線中檢視目前的部署：

1. 移至 [App Service]  。
2. 從清單中選取所需的應用程式。
3. 從 [應用程式服務] 窗格的 [部署]  區段中選擇 [部署中心]  。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Web%2Fsites]" submitText="Go to App Service" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

::: zone target="docs"

若要在 App Service 中檢視您的應用程式，請移至 [Azure 入口網站](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Web%2Fsites)。

::: zone-end

### <a name="learn-more"></a>深入了解

以下是可供您開始建置部署管線的額外連結：

- [建立您的第一個管線](https://docs.microsoft.com/azure/devops/pipelines/create-first-pipeline?view=azure-devops&tabs=tfs-2018-2)
- [GitHub 發行工作](https://docs.microsoft.com/azure/devops/pipelines/tasks/utility/github-release?view=azure-devops)
