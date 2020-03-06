---
title: 評估每個工作負載並精簡方案
description: 這節的 Azure 移轉指南可協助您評估環境，以判斷要遷移的內容，以及要考慮哪些遷移方法。
author: matticusau
ms.author: mlavery
ms.date: 02/25/2020
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: fasttrack-new, AQC
ms.localizationpriority: high
ms.openlocfilehash: 40e62a05101e14fcd7507011d521521e58920ffc
ms.sourcegitcommit: 72a280cd7aebc743a7d3634c051f7ae46e4fc9ae
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2020
ms.locfileid: "78225549"
---
# <a name="assess-each-workload-and-refine-plans"></a>評估每個工作負載並精簡方案

本指南中的資源可協助您評估每個工作負載，並針對每個工作負載的移轉適用性提出挑戰，以及完成移轉選項的相關架構決策。

<!-- markdownlint-disable MD025 -->

# <a name="tools"></a>[工具](#tab/Tools)

如果您未遵循上述連結中的指導方針，您可能會需要資料和評量工具來做出明智的移轉決策。 Azure Migrate 是用來評估**和**遷移至 Azure 的原生工具。 如果您還沒有這些項目，請使用下列步驟來建立新的伺服器移轉專案，並收集必要的資料。

## <a name="azure-migrate"></a>Azure Migrate

Azure Migrate 會評估要移轉至 Azure 的內部部署基礎結構、應用程式和資料。 此服務能夠：

- 評估內部部署資產的移轉適用性。
- 執行以效能為基礎的大小調整。
- 為 Azure 中執行的內部部署資產提供成本預估。

如果您正在可慮使用「隨即移轉」方法，或者正處於移轉的早期評量階段，此服務很適合您。 完成評量之後，您可以使用 Azure Migrate 來執行移轉。

![Azure Migrate 概觀](./media/assess/azure-migrate-overview-1.png)

### <a name="create-a-new-server-migration-project"></a>建立新的伺服器移轉專案

透過下列步驟，使用 Azure Migrate 開始進行伺服器移轉評量：

1. 選取 [Azure Migrate]  。
1. 在 [概觀]  中，選取 [評估和遷移伺服器]  。
1. 選取 [新增工具]  。
1. 在 [探索、評估和遷移伺服器]  中，選取 [新增工具]  。
1. 在 [Migrate 專案]  中選取您的 Azure 訂用帳戶，然後建立資源群組 (如果您還沒有的話)。
1. 在 [專案詳細資料]  中指定專案名稱，以及您要在其中建立專案的地理位置，然後選取 [下一步]  。
1. 在 [選取評量工具]  中，選取 [暫時跳過新增評量工具] > [下一步]  。
1. 在 [選取移轉工具]  中，選取 **[Azure Migrate：伺服器移轉] > [下一步]** 。
1. 在 [檢閱 + 新增工具]  中檢閱設定，然後選取 [新增工具]  。
1. 新增工具之後，工具會出現在 [Azure Migrate 專案] > [伺服器] > [移轉工具]  中。

::: zone target="chromeless"

::: form action="OpenBlade[#blade/Microsoft_Azure_Migrate/AmhResourceMenuBlade/overview]" submitText="Assess and migrate servers" :::

::: zone-end

::: zone target="docs"

### <a name="learn-more"></a>深入了解

- [Azure Migrate 概觀](https://docs.microsoft.com/azure/migrate/migrate-services-overview)
- [將實體或虛擬伺服器遷移至 Azure](https://docs.microsoft.com/azure/migrate/tutorial-migrate-physical-virtual-machines)
- [Azure 入口網站中的 Azure Migrate](https://portal.azure.com/#blade/Microsoft_Azure_Migrate/AmhResourceMenuBlade/overview)

::: zone-end

## <a name="service-map"></a>服務對應

服務對應會自動在 Windows 及 Linux 系統上探索應用程式元件，並對應服務之間的通訊。 您可以藉由服務對應，將伺服器視為提供重要服務的互連系統，藉此來檢視伺服器。 不需要進行任何設定，只要安裝了代理程式，服務對應就會顯示橫跨任何 TCP 連線架構的伺服器、處理序、輸入和輸出連線的延遲，和連接埠之間的連線。

Azure Migrate 會使用服務對應來增強環境中的報告功能和相依性。 如需這項整合的完整詳細資料，請參閱[相依性視覺效果](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization)。 如果您使用 Azure 移轉服務，則不需要進行額外步驟就能設定並取得服務對應的好處。 如果您想要針對其他用途或專案使用服務對應，我們提供了下列指示供您參考。

### <a name="enable-dependency-visualization-using-service-map"></a>使用服務對應來啟用相依性視覺效果

若要使用相依性視覺效果，請在待分析的每個內部部署機器上，下載及安裝代理程式。

- 必須在每個機器上安裝 [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows)。
- 必須在每部機器上安裝 [Microsoft 相依性代理程式](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-hybrid-cloud#install-the-dependency-agent-on-windows)。
- 此外，如果您有無法連線至網際網路的機器，請在這些機器上下載並安裝 Log Analytics 閘道。

<!-- markdownlint-disable MD024 -->

### <a name="learn-more"></a>深入了解

- [在 Azure 中使用服務對應解決方案](https://docs.microsoft.com/azure/azure-monitor/insights/service-map)
- [Azure Migrate 和服務對應：相依性視覺效果](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization)

# <a name="challenge-assumptions"></a>[挑戰假設](#tab/Challenge-Assumptions)

在理想的移轉中，每個資產 (基礎結構、應用程式或資料) 都會與雲端平台相容，並準備好進行移轉或現代化。 但事實上，並非所有工作負載都應該遷移至雲端。 並非每個資產都會與雲端平台相容。 在將工作負載遷移至雲端之前，請先評估每個工作負載和所有相依的資產 (基礎結構、應用程式和資料)。

[雲端採用架構的方案方法](../../plan/index.md)會建議讀者使用[漸進式合理化](../../digital-estate/rationalize.md#incremental-rationalization)及[以十為單位](../../digital-estate/rationalize.md#release-planning)方法來評估和規劃移轉。 本指南也包含[使用 Azure Migrate 評估數位資產](../../plan/contoso-migration-assessment.md)的詳細最佳做法。

上述連結會建議在移轉的規劃階段中，假設是可接受的，而且通常是建議的做法。 但是現在是時候採取行動了。 在遷移至雲端之前，您應該針對每個工作負載，對這些假設提出挑戰。

## <a name="two-steps-of-incremental-rationalization"></a>漸進式合理化的兩個步驟

若要成功實現[合理化](../../digital-estate/rationalize.md#incremental-rationalization)，您需要兩個相同的加權步驟。 這兩個步驟都需要環境中的資料和深入解析。 不過，每一種方法都涉及成功完成移轉工作所需的時間和詳細資料細微性。

- **[以十為單位的版本規劃](../../digital-estate/rationalize.md#release-planning)：** 第一次進行合理化和版本規劃期間，只有其中一個[合理化的 5R 策略](../../digital-estate/5-rs-of-rationalization.md)可用於評量。 根據與[雲端採用策略文件](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)中定義的整體動機最相符的合理化選項來預估和規劃。

- **每個工作負載的詳細評量：** 與「以十為單位」版本規劃相關的假設是可接受的，並且足以用來建立方案。 但是，如果在移轉之前未進行評估，則這些相同的假設可能會造成重大問題。

## <a name="challenge-assumptions-and-update-the-plan"></a>挑戰假設並更新方案

在 Azure Migrate 或您選擇的評估工具中仔細檢閱評量資料。 此資料會提供相容性問題、補救需求、大小調整建議和其他考量的見解。

在移轉之前，請使用該資料，並與產品擁有者、開發小組、系統管理員和其他人進行探索交談，以評估遷移此特定工作負載的可行性。 使用此探索來挑戰有關此工作負載的核心假設。 如果發現的結果會變更移轉或採用計畫，請據以更新方案。

挑戰這些假設的第一個步驟是[檢閱所有 5 R](../../digital-estate/rationalize.md)。

    - 假設的合理化方法適用於此工作負載嗎？ 這是最好的方法嗎？
    - 有任何[複寫物理學](../migration-considerations/migrate/replicate.md#replication-risks---physics-of-replication)會影響此工作負載的移轉嗎？
    - 移轉之前，此工作負載是否需要任何[補救活動](../migration-considerations/assess/evaluate.md)？

這些類型的問題有助於挑戰假設，並引導出每個工作負載的最佳路徑。

如需用於驗證假設的問題和已定義程序的擴充清單，請參閱[評量程序改善的概觀](../migration-considerations/assess/index.md)。

# <a name="scenarios-and-stakeholders"></a>[案例和專案關係人](#tab/Scenarios)

## <a name="scenarios"></a>案例

本指南涵蓋下列案例：

- **舊版硬體：** 遷移以移除即將終止支援或生命週期即將結束的舊版硬體上所具有的相依性。
- **容量成長：** 增加資產 (基礎結構、應用程式和資料) 的容量，因為目前的基礎結構無法提供。
- **資料中心現代化：** 擴充資料中心或使用雲端技術將資料中心現代化，以確保企業保持最新狀態和競爭力。
- **應用程式或服務現代化：** 更新應用程式以利用雲端原生功能。 即使您的初始目標是重新裝載移轉策略，但能夠建立方案來檢閱應用程式或服務以及進行可能的現代化，是所有移轉的共通程序。

### <a name="organizational-alignment-and-stakeholders"></a>組織配合與專案關係人

專案關係人的完整清單會隨著移轉專案而有所不同。 最好是假設您在開始規劃移轉時並不知道所有的專案關係人，因為通常只有到了專案的特定階段才能識別專案關係人。 不過，在開始進行任何移轉專案之前，請先識別財務、IT 基礎結構和應用程式群組中的重要業務主管，因為他們相當重視組織的整體雲端移轉工作。

建立核心雲端策略小組 (圍繞這些重要高階專案關係人來建置) 可協助讓組織準備好採用雲端，並引導整體的雲端移轉工作。 這個小組會負責了解雲端技術和移轉程序、識別移轉的業務理由，以及判斷最適合移轉工作使用的高階解決方案。 他們也有助於識別特定的應用程式和業務專案關係人並與之合作，以確保移轉成功。

如需讓組織準備好進行雲端移轉工作的詳細資訊，請參閱雲端採用架構關於[初始組織配合](../../plan/initial-org-alignment.md)的指引。

# <a name="timelines"></a>[時間表](#tab/Timelines)

客戶通常會發現本指南所說明的移轉案例可以在 1 到 6 個月內完成。

在評估移轉時間表時，請考慮下列因素：

- **要遷移的資產 (基礎結構、應用程式和資料)：** 資產的數量和多樣性。
- **員工整備程度：** 員工是否準備好管理新的環境或是否需要訓練？
- **資金：** 您有適當的核准和預算可供完成移轉嗎？
- **變更管理：** 貴公司是否有關於變更實作和核准的特定需求？
- **部門法規：** 您是否必須遵守部門或企業法規？

# <a name="cost-management"></a>[成本管理](#tab/ManageCost)

評估環境可提供絕佳的機會來包含成本分析步驟。 使用評估活動所收集的資料，您應該就能夠分析和預測成本。 除了任何一次性成本 (例如，增加的資料輸入) 外，此成本預測也應同時納入取用服務成本。

在移轉期間，會影響決策和執行活動的某些因素如下：

- **數位資產大小：** 了解數位資產大小會直接影響決策以及執行移轉所需的資源。
- **帳戶處理模型：** 從結構化的資本支出模型轉移到流暢的營運費用模型。

::: zone target="docs"

下列資源會提供相關資訊：

- [估計雲端成本](../migration-considerations/assess/estimate.md)

::: zone-end
