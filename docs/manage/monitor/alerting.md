---
title: 雲端監視指南：警示
description: 使用適用于 Azure 的雲端採用架構，瞭解如何在 Microsoft Azure 中判斷何時使用 Azure 監視器或 System Center Operations Manager。
author: MGoedtel
ms.author: magoedte
ms.date: 06/26/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: operate
services: azure-monitor
ms.openlocfilehash: c6f48ae433746906d64023bd72f34c21a3163373
ms.sourcegitcommit: 959cb0f63e4fe2d01fec2b820b8237e98599d14f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/11/2020
ms.locfileid: "79091287"
---
# <a name="cloud-monitoring-guide-alerting"></a>雲端監視指南：警示

多年來，IT 組織會努力對抗企業中部署的監視工具所建立的警示疲勞。 許多系統會產生大量的警示，通常會被視為無意義，而其他警示則是相關的，但可能被忽略或略過。 因此，IT 和開發人員作業會努力符合內部或外部客戶所承諾的服務等級品質。 若要確保可靠性，必須瞭解基礎結構和應用程式的狀態。 若要將服務降低和中斷降到最低，或減少或減少事件的影響，您需要快速找出原因。

## <a name="successful-alerting-strategy"></a>成功的警示策略

*您無法修正不知道的問題。*

對重要事項發出警示。 它的助力之下方式是收集並測量正確的計量和記錄。 您也需要能夠在符合條件時儲存、匯總、視覺化、分析及起始自動化回應的監視工具。 只有當您完全瞭解服務和應用程式的組合時，才可以改善其可檢視性。 您可以將該組合對應至監視平臺所要套用的詳細監視設定。 這項設定包括可預測的失敗狀態（徵兆，而不是失敗的原因），對發出警示是合理的。

請考慮下列原則，判斷徵兆是否為警示的適當候選項：

- **這有什麼影響嗎？** 問題的徵兆是真正問題還是影響應用程式整體健全狀況的問題？ 例如，您是否在意資源上的 CPU 使用率很高？ 或者，在該資源上的 SQL database 實例上執行的特定 SQL 查詢，在一段時間內耗用過高的 CPU 使用率？ 因為 CPU 使用率狀況是真正的問題，所以您應該對其發出警示。 但是，您不需要通知小組，因為它並不能説明您找出造成此狀況的原因。 針對 SQL 查詢程式使用率問題發出警示和通知，兩者都是相關且可採取動作的。
- **這是緊急的嗎？** 這是真正的問題，是否需要緊急注意？ 若是如此，應立即通知負責的小組。
- **您的客戶會受到影響嗎？** 服務或應用程式的使用者是否因問題而受到影響？
- **其他相依系統是否受到影響？** 是否有相關相依性的警示，而且可能相互關聯，以避免通知不同的小組處理相同的問題？

當您一開始正在開發監視設定時，請詢問這些問題。 在非生產環境中測試和驗證假設，然後再部署到生產環境。 監視設定衍生自已知的失敗模式、模擬失敗的測試結果，以及來自不同小組成員的經驗。

在您的監視設定發行之後，您可以深入瞭解什麼是運作中和什麼不是。 請考慮高警示量、監視未察覺的問題，但使用者會注意到，以及在本評估過程中採取的最佳動作。 識別要執行的變更，以改善服務傳遞，做為持續的持續監視改進程式的一部分。 它不只是用來評估警示雜訊或錯過警示，也是您監視工作負載的效率。 這就是您的警示原則、程式和整體文化特性的有效性，以判斷您是否正在改善。

System Center Operations Manager 和 Azure 監視器都支援以靜態或甚至是動態閾值以及在其上設定的動作為基礎的警示。 範例包括電子郵件、SMS 及簡單通知的語音通話警示。 這兩項服務也支援 IT 服務管理（ITSM）整合、自動建立事件記錄，以及呈報給正確的支援小組，或任何其他使用 webhook 的警示管理系統。

可能的話，您可以使用任何一項服務來自動化復原動作。 這些包括 System Center Orchestrator、Azure 自動化、Azure Logic Apps，或在彈性工作負載的情況下自動調整。 當通知責任小組是最常見的警示動作時，自動校正動作也可能適用。 這種自動化可協助簡化整個事件管理流程。 自動化這些復原工作也可以降低人為錯誤的風險。

## <a name="azure-monitor-alerting"></a>Azure 監視器警示

如果您是以獨佔方式使用 Azure 監視器，請在考慮速度、成本和存放磁片區時，遵循這些指導方針。

根據您所使用的功能和設定，您可以將監視資料儲存在六個存放庫中的任何一個：

- **Azure 監視器計量資料庫：** 主要用於 Azure 監視器平臺計量的時間序列資料庫，但也具有鏡像至其中的 Application Insights 計量資料。 輸入此資料庫的資訊具有最快速的警示時間。

- **Application Insights 記錄檔存放區：** 以記錄格式儲存大部分 Application Insights 遙測資料的資料庫。

- **Azure 監視器記錄檔存放區：** Azure 記錄資料的主要存放區。 其他工具可以將資料路由傳送給它，並可在 Azure 監視器記錄中進行分析。 由於內嵌和編制索引，記錄警示查詢的延遲較高。 此延遲通常是5-10 分鐘，但在某些情況下可能會更高。

- **活動記錄存放區：** 用於所有活動記錄和服務健康情況事件。 可以進行專用警示。 保留在您的訂用帳戶中的物件上發生的訂閱層級事件，如同從這些物件外部所見。 其中一個範例可能是設定原則或存取或刪除資源時。

- **Azure 儲存體：** Azure 診斷和其他監視工具支援的一般用途儲存體。 對於長期保留的監視遙測而言，這是一個低成本選項。 此服務中儲存的資料不支援警示。

- **事件中樞：** 通常用來將資料串流至內部部署或其他合作夥伴的監視或 ITSM 工具。

Azure 監視器有四種類型的警示，每個都與儲存資料的儲存機制稍有關聯：

- [度量警示](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric)： Azure 監視器計量資料庫中的資料警示。 當受監視的值超過使用者定義的閾值，然後當它恢復為「正常」狀態時，就會發生警示。

- [記錄查詢警示](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log-query)：可用於 Application Insights 或 Azure 記錄存放區中的內容警示。 它也可以根據跨工作區查詢來發出警示。

- [活動記錄警示](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)：活動記錄存放區中專案的警示，但服務健康狀態資料除外。

- [服務健康狀態警示](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log-service-notifications)：特殊類型的警示，僅用於來自活動記錄存放區的服務健康狀態問題，例如中斷和近期規劃的維護。 請注意，這種類型的警示是透過[Azure 服務健康狀態](https://docs.microsoft.com/azure/service-health/service-health-overview)（隨附于 Azure 監視器的服務）所設定。

### <a name="enable-alerting-through-partner-tools"></a>透過合作夥伴工具啟用警示

如果您使用外部警示解決方案，您可以透過 Azure 事件中樞來路由傳送，這是 Azure 監視器最快的路徑。 您必須支付內嵌到事件中樞的費用。 如果成本是問題，而速度不是，您可以使用 Azure 儲存體為較不昂貴的替代方案。 只要確定您的監視或 ITSM 工具可以讀取 Azure 儲存體以解壓縮資料。

Azure 監視器包括與其他監視平臺整合的支援，以及 ServiceNow 之類的 ITSM 軟體。 您可以依照事件管理或 DevOps 流程的需求，使用 Azure 警示，並在 Azure 外部觸發動作。 如果您想要 Azure 監視器警示並將回應自動化，可以根據您的案例和需求，使用 Azure Functions、Azure Logic Apps 或 Azure 自動化來起始自動化動作。

### <a name="specialized-azure-monitoring-offerings"></a>特製化的 Azure 監視供應專案

[管理解決方案](https://docs.microsoft.com/azure/azure-monitor/insights/solutions-inventory)通常會將其資料儲存在 Azure 記錄存放區中。 這兩個例外狀況為容器適用於 VM 的 Azure 監視器和 Azure 監視器。 下表描述以特定資料類型和儲存位置為基礎的警示體驗。

解決方法| 資料類型 | 警示行為
:---|:---|:---
適用於容器的 Azure 監視器 | 從節點和 pod 計算的平均效能資料會寫入計量存放區。 | 如果您想要根據測量的使用量效能變化來警示，請建立計量警示，並在一段時間內匯總。
|| 使用從節點、控制器、容器和 pod 百分位數的計算效能資料，會寫入至記錄存放區。 容器記錄和清查資訊也會寫入至記錄存放區。 | 如果您想要根據叢集和容器的測量使用率變化來警示，請建立記錄查詢警示。 您也可以根據 pod-階段計數和狀態節點計數來設定記錄查詢警示。
適用於 VM 的 Azure 監視器 | 健康情況準則是指寫入計量存放區的計量。 | 當健全狀況狀態從狀況良好變更為狀況不良時，就會產生警示。 此警示僅支援設定為傳送 SMS 或電子郵件通知的動作群組。
|| 對應和客體作業系統效能記錄檔資料會寫入記錄存放區。 | 建立記錄查詢警示。

### <a name="fastest-speed-driven-by-cost"></a>成本最快的速度

延遲是其中一個最重要的決策驅動警示，以及可快速解決影響服務的問題。 如果您在五分鐘內需要近乎即時的警示，請先評估，如果您有或可以在預設儲存資料的遙測上收到警示。 一般而言，此策略也是最便宜的選項，因為您所使用的工具已經將其資料傳送到該位置。

話雖如此，此規則有一些重要的註腳。

**客體作業系統遙測**有多個路徑可進入系統。

- 若要對此資料發出警示，最快的方式是將它匯入為自訂計量。 若要這麼做，請使用 Azure 診斷延伸模組，然後使用計量警示。 不過，自訂計量目前為預覽狀態，而且[比其他選項更昂貴](https://azure.microsoft.com/pricing/details/monitor)。

- 成本最低但最慢的方法是將它傳送至 Azure logs Kusto 存放區。 在 VM 上執行 Log Analytics 代理程式是取得所有客體作業系統計量和記錄資料到此存放區的最佳方式。

- 您可以同時在相同的 VM 上執行延伸模組和代理程式，將它傳送到這兩個存放區。 接著，您可以快速發出警示，但當您結合其他遙測時，也會使用客體作業系統資料做為更複雜搜尋的一部分。

**從內部部署匯入資料：** 如果您嘗試在 Azure 和內部部署環境中執行的機器之間進行查詢和監視，您可以使用 Log Analytics 代理程式來收集客體作業系統資料。 然後，您可以使用稱為「[記錄](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric-logs)」的功能來將計量簡化到計量存放區。 這個方法會略過在 Azure 記錄存放區中內嵌程式的一部分，因此資料會在計量資料庫中更快提供。

### <a name="minimize-alerts"></a>最小化警示

如果您使用適用於 VM 的 Azure 監視器之類的解決方案，並尋找監視效能使用率可接受的預設健康情況準則，請勿根據相同的效能計數器建立重迭的計量或記錄查詢警示。

如果您未使用適用於 VM 的 Azure 監視器，請藉由探索下列功能，讓建立警示和管理通知的作業更容易：

> [!NOTE]
> 這些功能僅適用于計量警示，以傳送至 Azure 監視器公制資料庫的資料為基礎的警示。 這些功能不適用於其他類型的警示。 如先前所述，度量警示的主要目標是速度。 如果在不到五分鐘的時間內收到警示並不是主要考慮，您可以改用記錄查詢警示。

- [動態閾值](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-dynamic-thresholds)：動態閾值會在一段時間內查看資源的活動，並建立上限和較低的「正常行為」閾值。 當受監視的計量超出這些臨界值時，您會收到警示。

- [Multisignal 警示](https://azure.microsoft.com/blog/monitor-at-scale-in-azure-monitor-with-multi-resource-metric-alerts)：您可以建立度量警示，以使用來自兩個不同資源類型的兩個不同輸入組合。 例如，如果您想要在 VM 的 CPU 使用率超過90% 時引發警示，而特定 Azure 服務匯流排佇列中的訊息數目超過特定數量，則您可以不建立記錄查詢來執行此動作。 這項功能只適用于兩個信號。 如果您有更複雜的查詢，請將計量資料摘要到 Azure 監視器記錄存放區，並使用記錄查詢。

- [多資源警示](https://azure.microsoft.com/blog/monitor-at-scale-in-azure-monitor-with-multi-resource-metric-alerts)： Azure 監視器允許套用至所有 VM 資源的單一計量警示規則。 這項功能可以節省您的時間，因為您不需要為每個 VM 建立個別警示。 這種警示類型的定價相同。 無論您是建立50警示來監視 50 Vm 的 CPU 使用率，或是針對所有50的 Vm，監視 cpu 使用率的一個警示，成本都相同。 您也可以搭配使用這些類型的警示與動態臨界值。

搭配使用時，這些功能可以將警示通知和基礎警示的管理降到最低，藉此節省時間。

### <a name="alerts-limitations"></a>警示限制

請務必記下您可以建立的警示數目[限制](https://docs.microsoft.com/azure/azure-subscription-service-limits#azure-monitor-limits)。 有些限制（但不是全部）可以藉由呼叫支援來增加。

### <a name="best-query-experience"></a>最佳查詢體驗

如果您要尋找所有資料的趨勢，請將您的所有資料匯入到 Azure 記錄檔中，除非它已經在 Application Insights 中。 您可以跨這兩個工作區建立查詢，因此不需要在兩者之間移動資料。 您也可以將活動記錄檔和服務健康狀態資料匯入 Log Analytics 工作區。 您需要為這項內嵌和儲存體付費，但您可以在一處取得所有資料，以進行分析和查詢。 此方法也可讓您建立複雜的查詢準則並對其發出警示。
