---
title: 定性和量化資料的意見反應
description: 了解如何使用 Azure 工具，從 GitHub 中裝載的 Web 應用程式和 API 中收集量化和質化的意見反應。
author: BrianBlanchard
ms.author: brblanch
ms.date: 01/27/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.localizationpriority: high
ms.custom: internal, fasttrack-edit, AQC, seo-caf-innovate
keywords: 量化資料、量化意見反應、定性意見反應、測試意見反應、客戶意見反應
ms.openlocfilehash: df83743be2399ddb2651582df482b3b987c659c9
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102112971"
---
# <a name="prepare-for-customer-feedback"></a>針對客戶意見反應做好準備

創新能否成功，關鍵在於使用者的採用、參與和保留。 原因為何？

要建置新的創新解決方案，不只是為使用者提供其所需或自認為所需的項目而已。 重點是要列出可加以測試和改善的假設。 該項測試有兩種形式：

- **量化 (測試意見反應)：** 此意見反應會測量我們希望看見的動作。
- **定性 (客戶意見反應)：** 此意見反應讓我們知道這些計量在客戶心目中所代表的意義。

量化資料是以數位為基礎，使用可量化的度量程式。 量化意見反應可提供資料的數值深入解析，這有助於快速收集客戶的大量答案。 量化意見反應的範例是多個選擇的問題和數值的使用者參與資料。 您可以深入瞭解意見反應，以取得更多關於客戶想法或意見的答案和深入解析。 定性意見反應的範例是客戶問卷，其中包含開放式問題。 客戶意見反應的這兩種方法都會提供寶貴的見解，以改善貴公司的產品和服務。

在整合意見反應迴圈之前，您需要有解決方案的共用存放庫。 集中式存放庫可讓您記錄所傳來的專案意見反應，並對這些意見反應採取行動。 [GitHub](https://github.com) 是開放原始碼軟體的居所。 其也是其中一個最常用來裝載商業開發應用程式，所用原始程式碼存放庫的平台。 關於[建置 GitHub 存放庫](/azure/devops/pipelines/repos/github)的文章可協助您開始使用您的存放庫。

下列每個 Azure 工具都會與 GitHub 中裝載的專案整合 (或相容)：

## <a name="quantitative-feedback-for-web-apps"></a>[適用於 Web 應用程式的定量意見反應](#tab/Quantitative-Apps)

Application Insights 是一種監視工具，可針對應用程式的使用量提供近乎即時的定量意見反應。 此意見反應有助於測試和驗證您目前的假設，以形成待辦項目的下一個功能或使用者劇本。

### <a name="action"></a>動作

若要檢視關於應用程式的定量資料：

1. 前往 **Application Insights**。
   - 如果應用程式未出現在清單中，請選取 [新增]，並遵循提示來開始設定 Application Insights。
   - 如果所需的應用程式已在清單中，請選取該應用程式。
1. [概觀] 窗格中會有一些關於應用程式的統計資料。 選取 [應用程式儀表板]，以針對與假設比較有關的資料建置自訂儀表板。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Insights%2FComponents]" submitText="Go to Application Insights" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

::: zone target="docs"

若要檢視應用程式相關資料，請移至 [Azure 入口網站](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Insights%2FComponents)。

::: zone-end

### <a name="learn-more"></a>深入了解

- [設定 Azure 監視器](/azure/azure-monitor/azure-monitor-app-hub)
- [開始使用 Azure 監視器 Application Insights](/azure/azure-monitor/app/tutorial-users)
- [建置遙測儀表板](/azure/azure-monitor/app/tutorial-app-dashboards)

## <a name="quantitative-feedback-for-apis"></a>[適用於 API 的定量意見反應](#tab/Quantitative-APIs)

連線經濟改變了企業的創新方式。 市場和產業遭遇動盪的速度比以往還快。 動盪的形式很多，企業必須奮力對抗「創新者困境」：如何設定變更步調，又不至於在持續的商業活動上摔一跤。

對外，企業會使用 API 來變更其與客戶和合作夥伴的互動方式。 對內，企業會使用 API 來順暢地連接企業的各個組成部分。 API 經濟的四個營運用建置組塊為：社交、行動、分析和雲端。 應用程式和服務可以透過符合成本效益的方式快速地連結起來，以產生更廣大的價值主張。

<!-- markdownlint-disable MD024 -->

### <a name="action"></a>動作

若要記錄關於 API 的定量資料：

1. 移至 [API 管理服務]。
2. 從清單中選取所需的 API。
3. 在 [監視] 區段中選取 [診斷設定]。

若要檢視關於 API 的定量資料：

1. 移至 [API 管理服務]。
2. 從清單中選取所需的 API。
3. 在 [監視] 區段中選取 [應用程式]。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.ApiManagement%2FService]" submitText="Go to API Management Services" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

::: zone target="docs"

若要開啟 API 管理服務，請移至 [Azure 入口網站](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.ApiManagement%2FService)。

::: zone-end

### <a name="learn-more"></a>深入了解

- [使用 Azure 監視器來取得關於 API 的意見反應](/azure/api-management/api-management-howto-use-azure-monitor)

## <a name="qualitative-feedback"></a>[定性意見反應](#tab/Qualitative)

您可以在待辦項目 (或面板) 將意見反應記錄為使用者劇本。 您也可以在這裡以可採取動作的工作形式來追蹤相關工作。 Azure Boards 可以直接整合到 GitHub 中，以便在意見反應-工作管理和任何原始程式碼之間獲得順暢的體驗。

::: zone target="docs"

### <a name="action"></a>動作

Azure Board 和 Azure Pipelines 需要與 GitHub 和 Azure 不同的入口網站。 開始使用 [Azure DevOps Services](/azure/devops/user-guide/what-is-azure-devops)。

::: zone-end

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

### <a name="action"></a>動作

若要建立 DevOps 專案：

1. 移至 [Azure DevOps Projects]。
2. 選取 [建立 DevOps 專案]。
3. 選取 [執行階段、架構和服務]。

::: form action="OpenBlade[#blade/HubsExtension/BrowseResource/resourceType/Microsoft.VisualStudio%2FAccount%2FProject]" submitText="Go to Azure DevOps Projects" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

### <a name="learn-more"></a>深入了解

這些文章將協助您使用 Azure Boards 搭配 GitHub 來集中管理意見反應：

- [開始使用 Azure Boards](/azure/devops/boards/get-started/)
- [Azure Boards 和 GitHub](/azure/devops/boards/github/)

## <a name="close-the-loop-with-pipelines"></a>[使用管線關閉迴圈](#tab/pipelines)

對意見反應採取行動不一定表示新增客戶所要求的功能。 但是每個資料點都應該會導致某些變更。 該變更可能是您對事物的想法改變。 也可能是與所要求功能完全不同的技術變更。 無論是哪種情況，部署管線和工具 (如 Azure Pipelines) 都可讓您快速發佈這些變更，以便經常與客戶分享。

### <a name="action"></a>動作

若要在管線中檢視目前的部署：

1. 移至 [應用程式服務]。
2. 從清單中選取所需的應用程式。
3. 在 [應用程式服務] 窗格的 [部署] 區段中選取 [部署中心]。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Web%2FSites]" submitText="Go to App Services" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

::: zone target="docs"

若要在 App Service 中檢視您的應用程式，請移至 [Azure 入口網站](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Web%2FSites)。

::: zone-end

### <a name="learn-more"></a>深入了解

開始建置您的部署管線：

- [建立您的第一個管線](/azure/devops/pipelines/create-first-pipeline)
- [`GitHub Release` 工作](/azure/devops/pipelines/tasks/utility/github-release)
