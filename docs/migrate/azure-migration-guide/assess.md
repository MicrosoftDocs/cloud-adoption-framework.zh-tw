---
title: 評估數位資產
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 評估數位資產
author: matticusau
ms.author: mlavery
ms.date: 08/08/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: fasttrack-new, AQC
ms.localizationpriority: high
ms.openlocfilehash: 27fa0720308910db32a340943f584a4502f52e94
ms.sourcegitcommit: 6f287276650e731163047f543d23581d8fb6e204
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/07/2019
ms.locfileid: "73753576"
---
# <a name="assess-the-digital-estate"></a>評估數位資產

在理想的移轉中，每個資產 (基礎結構、應用程式或資料) 都會與雲端平台相容，並準備好進行移轉。 但事實上，並非所有資產都應該遷移至雲端。 此外，並非每個資產都會與雲端平台相容。 在將工作負載遷移至雲端之前，請務必先評估工作負載和每個相關的資產 (基礎結構、應用程式和資料)。

本節中的資源可協助您評估環境，以判斷其是否適合進行移轉以及要考慮使用什麼方法。

<!-- markdownlint-disable MD025 -->

# <a name="toolstabtools"></a>[工具](#tab/Tools)

下列工具可協助您評估環境，以判斷其是否適合進行移轉以及可使用的最佳方法。 如需如何選擇適當工具以支援移轉工作的實用資訊，請參閱[雲端採用架構的移轉工具決策指南](../../decision-guides/migrate-decision-guide/index.md)。

## <a name="azure-migrate"></a>Azure Migrate

Azure Migrate 服務會評估要移轉至 Azure 的內部部署基礎結構、應用程式和資料。 此服務會評估內部部署資產的移轉適用性、執行以效能為依據的大小調整，並提供在 Azure 中執行內部部署資產的成本估計。 如果您正在考慮進行「隨即移轉」，或者正處於移轉的早期評估階段，此服務很適合您。 完成評估之後，您可以使用 Azure Migrate 來執行移轉。

![Azure Migrate 概觀](./media/assess/azuremigrate-overview-1.png)

### <a name="create-a-new-server-migration-project"></a>建立新的伺服器移轉專案

若要使用 Azure Migrate 開始伺服器移轉評估，請遵循下列步驟：

1. 選取 [Azure Migrate]  。
1. 在 [概觀]  中，按一下 [評估和遷移伺服器]  。
1. 選取 [新增工具]  。
1. 在 [探索、評估和遷移伺服器]  中，按一下 [新增工具]  。
1. 在 [Migrate 專案]  中選取您的 Azure 訂用帳戶，並建立資源群組 (如果您還沒有的話)。
1. 在 [專案詳細資料]  中指定專案名稱，以及您要在其中建立專案的地理位置，然後按 [下一步]  。
1. 在 [選取評量工具]  中，選取 [暫時跳過新增評量工具] > [下一步]  。
1. 在 [選取移轉工具]  中，選取 **[Azure Migrate：伺服器移轉] > [下一步]** 。
1. 在 [檢閱 + 新增工具]  中檢閱設定，然後按一下 [新增工具] 
1. 新增工具之後，工具會出現在 [Azure Migrate 專案] > [伺服器] > [移轉工具]  中。

::: zone target="chromeless"

::: form action="Blade[#blade/Microsoft_Azure_Migrate/AmhResourceMenuBlade/overview]" submitText="Assess and migrate servers" :::

::: zone-end

::: zone target="docs"

### <a name="learn-more"></a>深入了解

- [Azure Migrate 概觀](https://docs.microsoft.com/azure/migrate/migrate-services-overview)
- [將實體或虛擬伺服器遷移至 Azure](https://docs.microsoft.com/azure/migrate/tutorial-migrate-physical-virtual-machines)
- [Azure 入口網站中的 Azure Migrate](https://portal.azure.com/#blade/Microsoft_Azure_Migrate/AmhResourceMenuBlade/overview)

::: zone-end

## <a name="service-map"></a>服務對應

服務對應可自動探索 Windows 和 Linux 系統上的應用程式元件，並對應服務之間的通訊。 您可以藉由服務對應，將伺服器視為提供重要服務的互連系統，藉此來檢視伺服器。 不需要進行任何設定，只要安裝了代理程式，服務對應就會顯示橫跨任何 TCP 連線架構的伺服器、處理序、輸入和輸出連線的延遲，和連接埠之間的連線。

Azure Migrate 會使用服務對應來增強環境中的報告功能和相依性。 [相依性視覺效果](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization)會概述這項整合的完整詳細資料。 如果您使用 Azure 移轉服務，則不需要進行額外步驟就能設定並取得服務對應的好處。 如果您想要針對其他用途或專案使用服務對應，我們提供了下列指示供您參考。

### <a name="enable-dependency-visualization-using-service-map"></a>使用服務對應來啟用相依性視覺效果

若要使用相依性視覺效果，您需要在待分析的每個內部部署機器上，下載及安裝代理程式。

- 必須在每個機器上安裝 [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows)。
- 必須在每部機器上安裝 [Microsoft 相依性代理程式](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-hybrid-cloud#install-the-dependency-agent-on-windows)。
- 此外，如果您有無法連線至網際網路的機器，則必須在該機器上下載並安裝 Log Analytics 閘道。

<!-- markdownlint-disable MD024 -->

### <a name="learn-more"></a>深入了解

- [在 Azure 中使用服務對應解決方案](https://docs.microsoft.com/azure/azure-monitor/insights/service-map)
- [Azure Migrate 和服務對應：相依性視覺效果](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization)

# <a name="scenarios-and-stakeholderstabscenarios"></a>[案例和專案關係人](#tab/Scenarios)

## <a name="scenarios"></a>案例

本指南著重於下列案例︰

- **舊版硬體：** 您正在進行遷移以移除即將終止支援或生命週期即將結束的舊版硬體上所具有的相依性。
- **容量成長：** 您必須增加資產 (基礎結構、應用程式和資料) 的容量，因為目前的基礎結構無法提供。
- **資料中心現代化：** 您必須擴充資料中心或使用雲端技術將資料中心現代化，以確保企業保持最新狀態和競爭力。
- **應用程式或服務現代化：** 您可以更新應用程式以利用雲端原生功能。 即使您的初始目標是重新裝載移轉策略，但能夠建立方案來檢閱應用程式或服務以及進行可能的現代化，是所有移轉的共通程序。

### <a name="organizational-alignment-and-stakeholders"></a>組織配合與專案關係人

專案關係人的完整清單會隨著移轉專案而有所不同。 最好是假設您在開始規劃移轉時並不知道所有的專案關係人，因為通常只有到了專案的特定階段才能識別專案關係人。 不過，在開始進行任何移轉專案之前，您可以先識別財務、IT 基礎結構和應用程式群組中的重要業務主管，因為他們會對組織的整體雲端移轉工作感到興趣。

建立核心雲端策略小組 (圍繞這些重要高階專案關係人來建置) 可協助讓組織準備好採用雲端，並引導整體的雲端移轉工作。 這個小組會負責了解雲端技術和移轉程序、識別移轉的業務理由，以及判斷最適合移轉工作使用的高階解決方案。 他們也有助於識別特定的應用程式和業務專案關係人並與之合作，以確保移轉成功。

如需如何讓組織準備好進行雲端移轉工作的詳細資訊，請參閱雲端採用架構關於[初始組織配合](../../plan/initial-org-alignment.md)的文章。

# <a name="timelinestabtimelines"></a>[時間表](#tab/Timelines)

一般而言，客戶會發現本指南所說明的移轉案例可以在 1 到 6 個月內完成。

在評估移轉時間表時所要考慮的一些因素如下：

- **要遷移的資產 (基礎結構、應用程式和資料)：** 資產的數量和多樣性。
- **員工整備程度：** 員工是否準備好管理新的環境或是否需要訓練？
- **資金：** 您有適當的核准和預算可供完成移轉嗎？
- **變更管理：** 貴公司是否有關於變更實作和核准的特定需求？
- **部門法規：** 您是否必須遵守部門或企業法規？

# <a name="cost-managementtabmanagecost"></a>[成本管理](#tab/ManageCost)

當您評估環境時，這會提供絕佳機會讓您得以納入成本分析步驟。 使用評估活動所收集的資料，您應該就能夠分析和預測成本。 除了任何一次性成本 (例如，增加的資料輸入) 外，此成本預測也應同時納入取用服務成本。

在移轉期間，會影響決策和執行活動的某些因素如下：

- **數位資產大小：** 了解數位資產大小會直接影響決策以及執行移轉所需的資源。
- **帳戶處理模型：** 從結構化的資本支出模型轉移到流暢的營運費用模型。

::: zone target="docs"

下列資源會提供相關資訊：

- [估計雲端成本](../migration-considerations/assess/estimate.md)

::: zone-end
