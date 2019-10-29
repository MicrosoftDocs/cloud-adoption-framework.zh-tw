---
title: 用於 Azure 中雲端管理的工作負載特製化
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 改善工作負載特有的雲端管理作業
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: fasttrack-edit, AQC
ms.localizationpriority: high
ms.openlocfilehash: 51bdf23ccb2262202dfa095c5e9b57f727d11d3b
ms.sourcegitcommit: 35c162d2d09ec1c4a57d3d57a5db1d56ee883806
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2019
ms.locfileid: "72556984"
---
# <a name="workload-specialization-for-cloud-management"></a>用於雲端管理的工作負載特製化

工作負載特製化是以[平台特製化](./platform-specialization.md)中所述的概念為基礎。

![超越雲端管理基準](../../_images/manage/beyond-the-baseline.png)

- **工作負載作業：** 最大一筆的每一工作負載作業投資。 最高程度的復原能力。 建議用於可帶來商業價值的工作負載 +/- 20%。 通常保留給高度重要性或任務關鍵性的工作負載。
- **平台作業：** 作業投資會分散到許多工作負載。 復原能力的改善會影響所有運用已定義平台的工作負載。 建議用於最高重要性平台 +/- 20%。 通常保留給中度到高度重要性的工作負載。
- **增強的管理基準：** 相對最低的作業投資。 可稍微改善使用其他雲端原生作業工具和流程的商務承諾。

## <a name="high-level-process"></a>高階流程

要進行工作負載特製化作業，就必須有紀律地反覆執行下列四個流程。 [平台特製化](./platform-specialization.md)一文會更詳細地說明每個流程。

- **改善系統設計：** 改善特定工作負載的設計，以便有效地將中斷情形降到最低。
- **自動化補救：** 某些改善並不符合成本效益。 在這種情況下，將補救程序自動化並降低中斷所造成的影響，可能會是更好的做法。
- **擴展解決方案：** 隨著系統設計和自動化補救功能的改善，這些變更就能透過服務類別目錄擴展到整個環境。
- **持續改善：** 您可以使用各種監視工具來探索累進改善方法，以在下一階段的系統設計、自動化和擴展進行處理。

## <a name="cultural-change"></a>文化變革

工作負載特製化通常會觸發文化變革。 傳統 IT 所建置的流程著重在提供管理基準、增強的基準和平台作業。 這些類型的供應項目可以擴展到整個環境。 工作負載特製化與平台特製化在執行方面非常類似。 但是，不同於常見平台，個別工作負載所需的特製化通常不會擴展開來。

需要工作負載特製化時，作業管理通常會超越中央 IT 的觀點來發展。 雲端採用架構所建議的方法是分散雲端管理功能。

在此模型中，監視、部署、DevOps 和其他聚焦在創新的功能等操作工作，會轉移給應用程式開發或業務單位組織。 雲端平台和核心的雲端監控小組仍然會在整個環境中提供管理基準。 這些集中式小組也會引導和指示工作負載特製化小組來操作其工作負載。 但日常的營運責任則落在 IT 之外的雲端管理小組。 這類分散式控制是卓越雲端中心成熟與否的其中一個主要指標。

## <a name="beyond-platform-specialization---application-insights"></a>超越平台特製化 - Application Insights

您必須更詳細的特定工作負載資訊，才能提供明確的工作負載作業。 在持續改善階段期間，您必須將 Application Insights 新增至雲端管理工具鏈。

| 應用程式監視 | Application Insights | 應用程式的監視和診斷 | | 效能、可用性、使用量 | Application Insights | 使用應用程式儀表板、複合地圖、使用量和追蹤進行進階的應用程式監視 |

### <a name="deploy-application-insights"></a>部署 Application Insights

1. 在 Azure 入口網站中，搜尋 **Application Insights**。
2. 按一下 [+ 新增]  來建立 Application Insights 資源，以監視即時 Web 應用程式。
3. 遵循螢幕上的提示進行

如需如何設定應用程式以進行監視的指引，請參閱 [Azure 監視器 Application Insights 中樞](https://docs.microsoft.com/azure/azure-monitor/azure-monitor-app-hub)。

::: zone target="chromeless"

::: form action="OpenBlade[#create/Microsoft.AppInsights]" submitText="Create Application Insight resources" :::

::: zone-end

### <a name="monitor-performance-availability-and-usage"></a>監視效能、可用性和使用量

1. 在 Azure 入口網站中，搜尋 **Application Insights**。
2. 從清單中選擇其中一個 Application Insights 資源

Application Insights 的瀏覽包含各種用於監視效能、可用性、使用量和相依性的選項。 這些應用程式資料的每一個檢視，都能讓您清楚了解持續改善意見反應迴圈。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/microsoft.insights%2Fcomponents]" submitText="Monitor applications" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end
