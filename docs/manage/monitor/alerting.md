---
title: 雲端監視和警示
description: 使用適用于 Azure 的雲端採用架構，瞭解如何在 Microsoft Azure 中判斷何時使用 Azure 監視器或 System Center Operations Manager。
author: MGoedtel
ms.author: brblanch
ms.date: 08/05/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: think-tank
ms.openlocfilehash: d18deef4f25b16bb975c9f3e10b8f86a721a7680
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97015910"
---
<!-- cSpell:ignore multisignal -->

# <a name="cloud-monitoring-guide-alerting"></a>雲端監視指南：警示

多年來，IT 組織一直努力對抗企業中部署的監視工具所建立的警示疲勞。 許多系統會產生大量的警示，通常視為沒有意義，而其他警示則是相關的，但被忽略或忽略。 如此一來，IT 和開發人員作業就會努力滿足內部或外部客戶承諾的服務等級品質。 為了確保可靠性，您必須瞭解基礎結構和應用程式的狀態。 若要將服務降低和中斷降到最低，或是減少或減少事件數目，您需要快速找出原因。

## <a name="successful-alerting-strategy"></a>成功的警示策略

*您無法修正不知道的問題。*

重要事項的警示。 它是藉由收集和測量正確的計量和記錄來助力之下。 當符合條件時，您也需要能夠儲存、匯總、視覺化、分析和起始自動化回應的監視工具。 只有當您完全瞭解服務和應用程式的組合時，才能改善其可檢視性。 您可以將該組合對應至監視平臺要套用的詳細監視設定。 這項設定包括可預測的失敗狀態 (的徵兆，而不是造成警示的失敗) 原因。

請考慮下列準則來判斷徵兆是否為警示的適當候選：

- **這會有什麼影響？** 問題的徵兆是否為影響應用程式整體健全狀況的真實問題或問題？ 例如，您是否在意資源上的 CPU 使用率很高？ 或者，在該資源上的 SQL database 實例上執行的特定 SQL 查詢，在持續的期間內耗用高 CPU 使用率？ 因為 CPU 使用率狀況是真正的問題，所以您應該對其發出警示。 但是您不需要通知小組，因為它並不會指向第一個位置造成的狀況。 警示和通知 SQL 查詢程式的使用問題，都是相關且可採取動作的。
- **這是緊急的嗎？** 問題是否真的存在，是否需要注意？ 如果是的話，應該立即通知負責的小組。
- **您的客戶是否受到影響？** 服務或應用程式的使用者是否受此問題的影響？
- **其他相依系統是否受到影響？** 是否有相關相依性的警示，而且可能相互關聯，以避免通知不同的小組都有相同的問題？

當您一開始開發監視設定時，請詢問這些問題。 在非生產環境中測試和驗證假設，然後部署到生產環境。 監視設定衍生自已知的失敗模式、模擬失敗的測試結果，以及小組不同成員的體驗。

在您的監視設定發行之後，您可以深入瞭解什麼是運作中的內容。 考慮高警示量、由監視未察覺的問題，以及使用者注意到的問題，以及在此評估過程中採取的最佳動作。 在持續進行的持續監視改進程式過程中，識別要執行的變更，以改善服務傳遞。 這並不只是評估警示雜訊或遺漏警示，也是您監視工作負載的效率。 這就是您的警示原則、程式和整體文化特性的有效性，以判斷您是否正在進行改善。

System Center Operations Manager 和 Azure 監視器都會根據靜態或甚至是動態閾值，以及在它們之上設定的動作來支援警示。 範例包括電子郵件、SMS 和語音通話等簡單通知的警示。 這兩項服務也支援 IT 服務管理 (ITSM) 整合、將事件記錄的建立自動化，以及提升至正確的支援小組，或使用 webhook 的任何其他警示管理系統。

可能的話，您可以使用任何一項服務來自動執行復原動作。 這包括 System Center Orchestrator、Azure 自動化、Azure Logic Apps，或在彈性工作負載的情況下自動調整。 當通知負責任的小組是最常見的警示動作時，自動校正動作可能也適用。 這種自動化有助於簡化整個事件管理程式。 將這些復原工作自動化，也可以降低人為錯誤的風險。

## <a name="azure-monitor-alerting"></a>Azure 監視器警示

如果您是以獨佔方式使用 Azure 監視器，請在考慮速度、成本和儲存磁片區時遵循這些指導方針。

根據您所使用的功能和設定，您可以將監視資料儲存在六個存放庫中的任何一個：

- **Azure 監視器計量資料庫：** 時間序列資料庫主要用於 Azure 監視器平臺計量，但也會將 Application Insights 計量資料鏡像至其中。 輸入此資料庫的資訊具有最快的警示時間。

- **Application Insights 資源：** 以記錄檔形式儲存大部分 Application Insights 遙測的資料庫。

- **Azure 監視器 Log Analytics 工作區：** Azure 記錄資料的主要存放區。 其他工具可以將資料路由傳送給它，並可在 Azure 監視器記錄檔中進行分析。 由於內嵌和索引編制，記錄警示查詢會有較高的延遲。 此延遲通常是5-10 分鐘，但在某些情況下可能較高。

- **活動記錄：** 用於所有活動記錄和服務健康狀態事件。 可能會有專用的警示。 保存在您的訂用帳戶中的物件上發生的訂用帳戶層級事件，如同從這些物件的外部所見。 其中一個範例可能是設定原則或存取或刪除資源時。

- **Azure 儲存體：** Azure 診斷和其他監視工具所支援的一般用途儲存體。 這是適用于監視遙測長期保留的低成本選項。 這項服務中儲存的資料不支援警示。

- **事件中樞：** 通常用來將資料串流至內部部署或其他合作夥伴的監視或 ITSM 工具。

Azure 監視器有四種類型的警示，每個警示都與儲存資料的存放庫有所關聯：

- 計量[警示](/azure/azure-monitor/platform/alerts-metric)： Azure 監視器中度量資料的警示。 當受監視的值超過使用者定義的臨界值時，以及當它恢復為「正常」狀態時，就會發生警示。

- [記錄查詢警示](/azure/azure-monitor/platform/alerts-log-query)：可以從 Application Insights 或 Azure 監視器記錄檔中的記錄資料發出警示。 它也可以根據跨工作區查詢來發出警示。

- [活動記錄警示](/azure/azure-monitor/platform/alerts-activity-log)：活動記錄中專案的警示，但 Azure 服務健康狀態資料除外。

- [Azure 服務健康狀態警示](/azure/azure-monitor/platform/alerts-activity-log-service-notifications)：一種特殊類型的警示，只用于來自活動記錄的 Azure 服務健康狀態問題，例如中斷和即將進行規劃的維護。 請注意，這種類型的警示是透過 [Azure 服務健康狀態](/azure/service-health/service-health-overview)，也就是 Azure 監視器的隨附服務來設定。

### <a name="enable-alerting-through-partner-tools"></a>透過合作夥伴工具啟用警示

如果您使用外部警示解決方案，請盡可能地路由傳送 Azure 事件中樞，這是 Azure 監視器的最快路徑。 您必須支付內嵌至事件中樞的費用。 如果成本是問題且速度不是，您可以使用 Azure 儲存體作為較不昂貴的替代方案。 只要確定您的監視或 ITSM 工具可以讀取 Azure 儲存體，以將資料解壓縮。

Azure 監視器包含與其他監視平臺整合的支援，以及 ITSM 軟體（例如 ServiceNow）。 您可以使用 Azure 警示，並在您的事件管理或 DevOps 流程要求的情況下，于 Azure 外部觸發動作。 如果您想要 Azure 監視器警示並將回應自動化，您可以根據您的案例和需求，使用 Azure Functions、Azure Logic Apps 或 Azure 自動化來起始自動化動作。

### <a name="specialized-azure-monitoring-offerings"></a>特製化的 Azure 監視供應專案

[管理解決方案](/azure/azure-monitor/insights/solutions-inventory) 通常會將其資料儲存 Azure 監視器記錄。 針對容器適用於 VM 的 Azure 監視器和 Azure 監視器兩個例外狀況。 下表說明以特定資料類型和儲存位置為基礎的警示體驗。

| 解決方案 | 資料類型 | 警示行為 |
|---| ---| --- |
| 適用於容器的 Azure 監視器 | 從節點和 pod 計算的平均效能資料會寫入計量資料庫。 | 如果您想要根據測量的使用量效能變化來發出警示，請建立計量警示，並在一段時間後匯總。 |
| | 使用從節點、控制器、容器和 pod 百分位數的計算效能資料會寫入工作區。 容器記錄和清查資訊也會寫入工作區。 | 如果您想要根據叢集和容器的測量使用率變化來收到警示，請建立記錄查詢警示。 您也可以根據 pod-階段計數和狀態節點計數來設定記錄查詢警示。 |
| 適用於 VM 的 Azure 監視器 | 健康情況準則是儲存在計量資料庫中的計量。 | 健全狀況狀態從狀況良好變更為狀況不良時，會產生警示。 此警示僅支援設定為傳送 SMS 或電子郵件通知的動作群組。 |
| | 系統會將對應和客體作業系統效能記錄資料寫入 Log Analytics 工作區。 | 建立記錄查詢警示。 |

### <a name="fastest-speed-driven-by-cost"></a>速度最快，以成本推動

延遲是驅動警示的最重要決策，以及對影響服務的問題快速解決的問題之一。 如果您需要在五分鐘內進行近乎即時的警示，請先評估，如果您在遙測上有預設儲存的警示，請先進行評估。 一般而言，此策略也是最便宜的選項，因為您所使用的工具已經將其資料傳送至該位置。

話雖如此，這項規則有一些重要的註腳。

**來賓 OS 遙測** 具有多個可進入系統的路徑。

- 警示此資料最快的方式是將其匯入為自訂計量。 若要這麼做，請使用 Azure 診斷擴充功能，然後使用計量警示。 不過，自訂計量目前為預覽狀態，而且 [比其他選項的成本更高](https://azure.microsoft.com/pricing/details/monitor)。

- 最便宜的，但在某些內嵌延遲的情況下，是將它傳送至 Log Analytics 工作區。 在 VM 上執行 Log Analytics 代理程式是取得所有客體作業系統計量和記錄資料到工作區的最佳方式。

- 您可以在相同的 VM 上執行 Azure 診斷擴充功能和 Log Analytics 代理程式，以將其作為度量傳送給儲存體，並以 Azure 監視器登入。 然後，您可以更快速地發出警示，但也可以在結合其他遙測資料時，使用客體作業系統資料作為更複雜查詢的一部分。

**從內部部署匯入資料：** 如果您嘗試在 Azure 和內部部署中執行的機器之間進行查詢和監視，您可以使用 Log Analytics 代理程式來收集客體作業系統資料。 然後，您可以使用稱為「 [記錄](/azure/azure-monitor/platform/alerts-metric-logs) 」的功能來收集計量，並將其儲存為 Azure 監視器中的度量。 這個方法會在 Azure 監視器記錄中略過內嵌程式的一部分，而資料可供稍後使用。

### <a name="minimize-alerts"></a>最小化警示

如果您使用適用於 VM 的 Azure 監視器的解決方案，並找出監視效能使用率可接受的預設健全準則，請勿根據相同的效能計數器建立重迭的計量或記錄查詢警示。

如果您不是使用適用於 VM 的 Azure 監視器，請探索下列功能，讓建立警示和管理通知的工作更容易：

> [!NOTE]
> 這些功能僅適用于計量警示、根據傳送至 Azure 監視器計量資料庫之資料的警示。 這些功能不適用於其他類型的警示。 如先前所述，計量警示的主要目標是速度。 如果在不到五分鐘內收到警示，則不是主要考慮，您可以改用記錄查詢警示。

- [動態閾值](/azure/azure-monitor/platform/alerts-dynamic-thresholds)：動態閾值會查看資源在一段時間內的活動，並建立「正常行為」臨界值的上限和下限。 當受監視的計量超出這些閾值時，您會收到警示。

- [Multisignal 警示](https://azure.microsoft.com/blog/monitor-at-scale-in-azure-monitor-with-multi-resource-metric-alerts)：您可以建立計量警示，以使用兩個不同資源類型的兩個不同輸入的組合。 例如，如果您想要在 VM 的 CPU 使用率超過90% 時引發警示，而特定 Azure 服務匯流排佇列中的訊息數目超過特定數量，則您可以在不建立記錄查詢的情況下進行。 這項功能僅適用于兩個信號。 如果您有更複雜的查詢，請將計量資料送入 Log Analytics 工作區，並使用記錄查詢。

- [多資源警示](https://azure.microsoft.com/blog/monitor-at-scale-in-azure-monitor-with-multi-resource-metric-alerts)： Azure 監視器允許適用于所有 VM 資源的單一計量警示規則。 這項功能可節省您的時間，因為您不需要為每個 VM 建立個別的警示。 這種警示類型的定價相同。 無論您建立50警示來監視 50 Vm 的 CPU 使用率，還是會有一個警示可監視所有 50 Vm 的 CPU 使用率，這會產生相同數量的費用。 您也可以搭配動態閾值來使用這些類型的警示。

使用這些功能可將警示通知和基礎警示的管理降至最低，藉此節省時間。

### <a name="limits-on-alerts"></a>警示的限制

請務必記下 [您可以建立的警示數目限制](/azure/azure-resource-manager/management/azure-subscription-service-limits#azure-monitor-limits)。 某些限制 (但並非所有) 都可以藉由呼叫支援來增加。

### <a name="best-query-experience"></a>最佳查詢體驗

如果您要尋找所有資料的趨勢，請將所有資料匯入 Azure 監視器記錄檔中，除非它已在 Application Insights 中。 您可以跨這兩個工作區建立查詢，因此不需要在兩者之間移動資料。 您也可以將活動記錄和 Azure 服務健康狀態資料匯入 Log Analytics 工作區。 您需支付此內嵌和儲存體的費用，但您會將所有資料都放在單一位置，以供分析和查詢。 這種方法也可讓您建立複雜的查詢準則，並對其發出警示。
