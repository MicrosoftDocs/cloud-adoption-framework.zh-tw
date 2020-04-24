---
title: 雲端部署模型的監視策略
description: 使用適用于 Azure 的雲端採用架構，瞭解要採用哪些監視策略來進行雲端管理。
author: MGoedtel
ms.author: magoedte
ms.date: 10/04/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
services: azure-monitor
ms.openlocfilehash: 33daaaf5859e0b761a6b53b1afc67df2ddcd1f65
ms.sourcegitcommit: 7d3fc1e407cd18c4fc7c4964a77885907a9b85c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "80527085"
---
<!-- cSpell:ignore savision -->

# <a name="cloud-monitoring-guide-monitoring-strategy-for-cloud-deployment-models"></a>雲端監視指南：雲端部署模型的監視策略

本文包含我們針對每個雲端部署模型所建議的監視策略（根據下列準則）：

- 您必須維護 Operations Manager 或其他企業監視平臺的承諾，因為它已與您的 IT 作業程式、知識和專業知識整合，或某些功能尚未在 Azure 監視器中提供。
- 您必須在內部部署和公用雲端中，或只監視雲端中的工作負載。
- 您的雲端遷移策略包括現代化 IT 作業，並移至我們的雲端監視服務和解決方案。
- 您的重要系統可能有空中或實體隔離，或裝載于私人雲端或實體硬體上，而且這些系統必須受到監視。

我們的策略包括監視基礎結構（計算、儲存體和伺服器工作負載）、應用程式（使用者、例外狀況和用戶端），以及網路資源的支援。 它提供完整的服務導向監視觀點。

## <a name="azure-cloud-monitoring"></a>Azure 雲端監視

Azure 監視器是 Azure 原生平臺服務，可提供用來監視 Azure 資源的單一來源。 其設計適用于下列雲端解決方案：

- 建置於 Azure 上。
- 支援以虛擬機器（VM）工作負載為基礎的商務功能，或使用微服務和其他平臺資源的複雜架構。

它會監視堆疊的所有層，從租使用者服務開始，例如 Azure Active Directory Domain Services，以及訂用帳戶層級事件和 Azure 服務健康狀態。

它也會監視基礎結構資源，例如 Vm、儲存體和網路資源。 在最上層，它會監視您的應用程式。

藉由監視每個相依性，並收集每個相依性可以發出的正確信號，您就可以取得應用程式的可檢視性和所需的重要基礎結構。

下表摘要說明監視堆疊各層的建議方法：

<!-- markdownlint-disable MD033 -->

階層 | 資源 | 影響範圍 | 方法
---|---|---|----
Application | 在 .NET、.NET Core、JAVA、JavaScript 和 node.js 平臺上執行的 web 應用程式，可在 Azure VM、Azure App 服務、Azure Service Fabric、Azure Functions 和 Azure 雲端服務上執行。 | 監視即時 web 應用程式，以自動偵測效能異常、找出程式碼例外狀況和問題，以及收集使用者行為分析。 |  Azure 監視器（Application Insights）。
Azure 資源-平臺即服務（PaaS） | Azure 資料庫服務（例如 SQL 或 MySQL）。 | 適用于 SQL 效能計量的 Azure 資料庫。 | 啟用診斷記錄以將 SQL 資料串流至 Azure 監視器記錄。
Azure 資源-基礎結構即服務（IaaS） | 1. Azure 儲存體<br/> 2. Azure 應用程式閘道<br/> 3. 網路安全性群組<br/> 4. Azure 流量管理員<br/> 5. Azure 虛擬機器<br/> 6. Azure Kubernetes Service/Azure 容器實例 | 1. 容量、可用性和效能。<br/> 2. 效能和診斷記錄（活動、存取、效能和防火牆）。<br/> 3. 在套用規則時監視事件，以及規則套用到拒絕或允許的次數的規則計數器。<br/> 4. 監視端點狀態可用性。<br/> 5. 監視來賓 VM 作業系統（OS）中的容量、可用性和效能。 對應每個 VM 上裝載的應用程式相依性，包括伺服器之間的作用中網路連線的可見度、輸入和輸出連線延遲，以及任何 TCP 連接架構的埠。<br/> 6. 監視容器和容器實例上執行之工作負載的容量、可用性和效能。 | 1. Blob 儲存體的儲存體計量。<br/> 2. 啟用診斷記錄，並設定串流以 Azure 監視器記錄。<br/> 3. 啟用網路安全性群組的診斷記錄，並設定串流以 Azure 監視器記錄。<br/> 4. 啟用流量管理員端點的診斷記錄，並設定串流處理以 Azure 監視器記錄。<br/> 5. 啟用適用於 VM 的 Azure 監視器。<br/> 6. 啟用容器的 Azure 監視器。
網路 | 您的虛擬機器與一或多個端點（另一個 VM、完整功能變數名稱、統一資源識別項或 IPv4 位址）之間的通訊。 | 監視在 VM 與端點之間發生的可連線性、延遲和網路拓撲變更。 | Azure 網路監看員。
Azure 訂用帳戶 | Azure 服務健康狀態和基本資源健康狀態。 | <li> 在服務或資源上執行的系統管理動作。<br/><li> 具有 Azure 服務的服務健全狀況處於降級或無法使用的狀態。<br/><li> 從 Azure 服務的觀點來看，偵測到 Azure 資源的健康情況問題。<br/><li> 使用 Azure 自動調整執行的作業會指出失敗或例外狀況。 <br/><li> 以 Azure 原則執行的作業，表示發生允許或拒絕的動作。<br/><li> Azure 資訊安全中心所產生的警示記錄。 | 在活動記錄中傳遞，以使用 Azure Resource Manager 進行監視和警示。
Azure 租用戶 | Azure Active Directory || 啟用診斷記錄，並設定串流處理以 Azure 監視器記錄。

<!-- markdownlint-enable MD033 -->

## <a name="hybrid-cloud-monitoring"></a>混合式雲端監視

對於許多組織而言，轉換至雲端必須逐漸進行，其中混合式雲端模型是旅程圖中最常見的第一個步驟。 您可以謹慎地選取適當的應用程式和基礎結構子集，以開始進行遷移，同時避免業務中斷。 不過，由於我們提供兩個支援此雲端模型的監視平臺，因此 IT 決策者可能無法確定哪一種是支援其商務和 IT 營運目標的最佳選擇。

在本節中，我們會藉由檢查數個因素來解決不確定性，並提供對要考慮的平臺的瞭解。

請記住下列重要的技術層面：

- 您必須從支援工作負載的 Azure 資源收集資料，並將其轉送至您現有的內部部署或受控服務提供者工具。

- 您需要在 System Center Operations Manager 中維持目前的投資，並將其設定為監視在 Azure 中執行的 IaaS 和 PaaS 資源。 （選擇性）因為您要根據需求來監視兩個具有不同特性的環境，所以您必須判斷與 Azure 監視器整合的方式如何支援您的策略。

- 在您的現代化策略中，若要將單一工具標準化以降低成本和複雜度，您必須認可 Azure 監視器來監視 Azure 和公司網路上的資源。

下表摘要說明根據一組通用準則來監視混合式雲端模型時，Azure 監視器和 System Center Operations Manager 支援的需求。

<!-- markdownlint-disable MD033 -->

|需求 | Azure 監視器 | Operations Manager |
|:--|:---|:---|
|基礎結構需求 | 否 | 是<br> 至少需要管理伺服器和 SQL server，才能裝載運算元據庫和報表資料倉儲資料庫。 當需要高可用性和嚴重損壞修復，而且有多個網站中的機器、不受信任的系統，以及其他複雜的設計考慮時，複雜度就會增加。|
|有限的連線能力-無網際網路<br> 或隔離的網路 | 否 | 是 |
|受限制的連線能力控制網際網路存取 | 是 | 是 |
|有限的連線-經常中斷連線 | 是 | 是 |
|可設定的健全狀況監視 | 否 | 是 |
| Web 應用程式可用性測試（獨立網路） | 是，有限<br> Azure 監視器在此區域提供有限的支援，且需要自訂的防火牆例外。 | 是 |
| Web 應用程式可用性測試（全域散發） | 否 | 是 |
|監視 VM 工作負載 | 是，有限<br> 可以收集 IIS 和 SQL Server 錯誤記錄檔、Windows 事件和效能計數器。 需要建立自訂查詢、警示和視覺效果。 | 是<br> 支援使用可用的管理元件監視大部分的伺服器工作負載。 需要 VM 上的 Log Analytics Windows 代理程式或 Operations Manager 代理程式，回報至公司網路上的管理群組。|
|監視 Azure IaaS | 是 | 是<br> 支援從公司網路監視大部分的基礎結構。 透過 Azure 管理元件，追蹤 Azure Vm、SQL 和儲存體的可用性狀態、計量和警示。|
|監視 Azure PaaS | 是 | 是，有限<br> 根據 Azure 管理元件中支援的功能。 |
|Azure 服務監視 | 是<br> | 是<br> 雖然目前沒有透過管理元件提供的 Azure 服務健康狀態的原生監視，您可以建立自訂工作流程來查詢 Azure 服務健康狀態警示。 使用 Azure REST API 透過現有的通知來取得警示。|
|新式 web 應用程式監視 | 是 | 否 |
|舊版 web 應用程式監視 | 是，有限制，依 SDK 而有所不同<br> 支援監視較舊版本的 .NET 和 JAVA web 應用程式。 | 是，有限 |
|監視 Azure Kubernetes Service 容器 | 是 | 否 |
|監視 Docker 或 Windows 容器 | 是 | 否 |
|網路效能監視 | 是 | 是，有限<br> 支援可用性檢查，並使用來自公司網路的簡易網路管理通訊協定（SNMP），從網路裝置收集基本統計資料。|
|互動式資料分析 | 是 | 否<br> 依賴 SQL Server Reporting Services 預先定義的報表、協力廠商的視覺效果解決方案，或自訂的 Power BI 執行。 Operations Manager 資料倉儲的規模和效能限制。 與 Azure 監視器記錄整合，以作為資料匯總需求的替代方案。 您可以藉由設定 Log Analytics 連接器來達到整合。|
|端對端診斷、根本原因分析，以及及時的疑難排解 | 是 | 是，有限<br> 僅支援內部部署基礎結構和應用程式的端對端診斷和疑難排解。 使用其他 System Center 元件或合作夥伴解決方案。|
|互動式視覺效果（儀表板） | 是 | 是，有限<br> 透過其 HTML5 web 主控台或合作夥伴解決方案（例如，方形 Up 和 Savision）的先進體驗，提供基本的儀表板。 |
|與 IT 或 DevOps 工具整合 | 是 | 是，有限 |

<!-- markdownlint-enable MD033 -->

### <a name="collect-and-stream-monitoring-data-to-third-party-or-on-premises-tools"></a>收集監視資料並串流至協力廠商或內部部署工具

若要從 Azure 基礎結構和平臺資源收集計量和記錄，您必須啟用這些資源的 Azure 診斷記錄。 此外，透過 Azure Vm，您可以藉由啟用 Azure 診斷延伸模組，從來賓 OS 收集計量和記錄。 若要將從 Azure 資源發出的診斷資料轉送到內部部署工具或受控服務提供者，請設定[事件中樞](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-stream-event-hubs)以將資料串流處理到它們。

### <a name="monitor-with-system-center-operations-manager"></a>使用 System Center Operations Manager 進行監視

雖然 System Center Operations Manager 原本是設計為內部部署解決方案，可監視在您的 IT 環境中執行的應用程式、工作負載和基礎結構元件，但其演變為包含雲端監視功能。 它會與 Azure、Office 365 和 Amazon Web Services （AWS）整合。 它可以使用設計和更新的管理元件來監視這些不同的環境，以支援它們。  

對於在 Operations Manager 進行大量投資以達成與 IT 服務管理程式和工具緊密整合的全面監視，或對 Azure 的客戶而言，您可以使用下列問題來瞭解：

- Operations Manager 是否可以繼續傳遞價值，並有其商業意義嗎？
- Operations Manager 的功能讓它適合我們的 IT 組織嗎？
- 整合 Operations Manager 與 Azure 監視器提供我們需要的符合成本效益且全面的監視解決方案嗎？

如果您已經投資 Operations Manager，就不需要專注于規劃遷移來立即取代。 如果 Azure 或其他雲端提供者是以您自己的內部部署網路的延伸模組存在，Operations Manager 可以監視來賓 Vm 和 Azure 資源，就像是在您的公司網路上一樣。 這種方法需要有足夠頻寬的網路與 Azure 虛擬網路之間的可靠網路連線。

若要監視在 Azure 中執行的工作負載，您需要：

- [適用于 Azure 的管理元件](https://www.microsoft.com/download/details.aspx?id=50013)。 它會收集 Azure 服務所發出的效能計量，例如 web 和背景工作角色、Application Insights 可用性測試（web 測試）、Azure 服務匯流排等等。 管理元件會使用 Azure REST API 來監視這些資源的可用性和效能。 某些 Azure 服務類型在管理元件中沒有計量或預先定義的監視，但您仍然可以透過 Azure 管理元件中針對探索到的服務所定義的關聯性來監視它們。

- [Azure SQL Database 的管理元件](https://www.microsoft.com/download/details.aspx?id=38829)，可使用 azure REST API 和 t-SQL 查詢 SQL Server 系統檢視，來監視 azure sql 資料庫和 azure sql Database 伺服器的可用性和效能。

- 若要監視在 VM 上執行的虛擬作業系統和工作負載（例如 SQL Server、IIS 或 Apache Tomcat），您需要下載並匯入支援應用程式、服務和 OS 的管理元件。

知識是在管理元件中定義的，它會說明如何監視個別的相依性和元件。 這兩個 Azure 管理元件都需要在 Azure 和 Operations Manager 中執行一組設定步驟，才能開始監視這些資源。

在應用層，Operations Manager 提供一些舊版 .NET 和 JAVA 的基本應用程式效能監視功能。 如果混合式雲端環境內的特定應用程式以離線或網路隔離模式運作，使其無法與公用雲端服務通訊，Operations Manager 應用程式效能監視（APM）可能是某些有限案例的可行選項。 針對不是在舊版平臺上執行，但裝載于內部部署和任何公用雲端（允許透過防火牆（直接或透過 proxy）通訊至 Azure）的應用程式，請使用 Azure 監視器 Application Insights。 此服務提供深度、程式碼層級的監視，並具有 ASP.NET、ASP.NET Core、JAVA、JavaScript 和 node.js 的一流支援。

對於可從外部連線的任何 web 應用程式，您應該啟用一種稱為「[可用性監視]( https://docs.microsoft.com/azure/azure-monitor/app/monitor-web-app-availability)」的綜合交易。 請務必瞭解您的應用程式或應用程式所依賴的重要 HTTP/HTTPS 端點是否可用且有回應。 使用 Application Insights 可用性監視時，您可以從多個 Azure 資料中心執行測試，並從全球的觀點提供應用程式健康情況的深入解析。

雖然 Operations Manager 能夠監視裝載于 Azure 中的資源，但還是有幾個優點可以納入 Azure 監視器，因為它的優勢克服了 Operations Manager 的限制，而且可以建立強大的基礎來支援最終的從它進行的遷移。 我們將在此探討每個優點和缺點，並建議您在混合式監視策略中包含 Azure 監視器。  

#### <a name="disadvantages-of-using-operations-manager-by-itself"></a>使用 Operations Manager 本身的缺點

- 在 Operations Manager 中分析監視資料，通常是使用從主控台存取的管理元件、從 SQL Server Reporting Services （SSRS）報告，或使用者已建立的自訂視圖所提供的預先定義的視圖來執行。 臨機運算元據分析不是現成可用的。 Operations Manager 報告沒有彈性。 提供監視資料長期保留的資料倉儲不會調整或執行得很好。 如需撰寫 T-sql 語句、開發 Power BI 解決方案，或使用協力廠商解決方案的專長，必須支援 IT 組織中各種不同角色的需求。

- Operations Manager 中的警示不支援複雜運算式或包含相互關聯邏輯。 為了協助減少雜訊，會將警示分組，以顯示兩者之間的關聯性，並識別其原因。

#### <a name="advantages-of-using-operations-manager-with-azure-monitor"></a>搭配 Azure 監視器使用 Operations Manager 的優點

- Azure 監視器是解決 Operations Manager 限制的方式。 它藉由收集重要的效能和記錄資料，來補充 Operations Manager 資料倉儲資料庫。 Azure 監視器提供更佳的分析、效能（查詢大型資料磁片區時），以及比 Operations Manager 資料倉儲的保留期。

  有了 Azure 監視器查詢語言，您就可以建立更複雜且精密的查詢。 您可以在數秒內跨數 tb 的資料執行查詢。 您可以快速地將資料轉換成圓形圖、時間圖表和其他許多視覺效果。 若要分析此資料，您不再受限於使用以 SQL Server Reporting Services、自訂 SQL 查詢或其他因應措施為基礎的 Operations Manager 報表。

- 您可以藉由執行 Azure 監視器警示管理解決方案來提供改良的警示體驗。 Operations Manager 管理群組中產生的警示可以轉送到 Azure 監視器 logs Analytics 工作區。 您可以設定負責將警示從 Operations Manager 轉送到 Azure 監視器記錄的訂用帳戶，只轉送特定警示。 例如，您可以透過單一窗格，只轉送符合您查詢準則以進行問題管理的支援，以及調查失敗或問題根本原因的警示。 此外，您可以將其他記錄資料與 Application Insights 或其他來源相互關聯，以取得可協助改善使用者體驗、增加執行時間，以及縮短解決事件的時間的深入解析。

- 您可以從 Azure 中的簡單或多層式架構監視雲端原生基礎結構和應用程式，而且您可以使用 Operations Manager 來監視內部部署基礎結構。 此監視包括一或多個 Vm、放置在可用性設定組或虛擬機器擴展集內的多個 Vm，或部署到在 Windows Server 或 Linux 容器上執行之 Azure Kubernetes Service （AKS）的容器化應用程式。

- 您可以使用 System Center Operations Manager 健全狀況檢查解決方案，以週期性間隔主動評估 System Center Operations Manager 管理群組的風險和健全狀況。 此解決方案可以取代或補充您已新增至管理群組的任何自訂功能。

- 藉由使用適用於 VM 的 Azure 監視器的「對應」功能，您可以從 Azure Vm 與內部部署 Vm 之間的網路連線監視標準連接計量。 這些計量包括回應時間、每分鐘的要求數、流量輸送量和連結。 您可以識別失敗的連線、疑難排解、執行遷移驗證、執行安全性分析，以及驗證服務的整體架構。 Map 可以自動探索 Windows 和 Linux 系統上的應用程式元件，並對應服務之間的通訊。 這項自動化可協助您識別不知道的連線和相依性、規劃和驗證遷移至 Azure，以及在事件解決期間將推測降至最低。

- 藉由使用網路效能監控，您可以監視下列各項之間的網路連線：
  - 您的公司網路和 Azure。
  - 任務關鍵性多層式應用程式和微服務。
  - 使用者位置和 web 應用程式（HTTP/HTTPS）。

此策略會提供網路層的可見度，而不需要 SNMP。 它也可以顯示在互動式拓撲圖中，這是來源與目的地端點之間路由的逐一躍點拓撲。 這是比嘗試在 Operations Manager 中的網路監視或您的環境中目前使用的其他網路監視工具來達成相同結果的最佳選擇。

### <a name="monitor-with-azure-monitor"></a>使用 Azure 監視器進行監視

雖然遷移至雲端會帶來許多挑戰，但也提供了機會。 它可讓您的組織從一或多個內部部署企業監視工具進行遷移，不僅可能降低資本支出和營運成本，還能受益于雲端監視平臺（例如 Azure 監視器）可在雲端規模提供的優勢。 檢查您的監視和警示需求、現有監視工具的設定，以及轉換至雲端的工作負載。 當您的方案完成後，請設定 Azure 監視器。

- 監視混合式基礎結構和應用程式，從簡單或多層式架構，其中元件裝載于 Azure、其他雲端提供者，以及您的公司網路。 這些元件可能包括一或多個 Vm、放置在可用性設定組或虛擬機器擴展集內的多個 Vm，或部署到在 Windows Server 或 Linux 容器上執行之 Azure Kubernetes Service （AKS）的容器化應用程式。

- 啟用適用於 VM 的 Azure 監視器、容器的 Azure 監視器，以及 Application Insights 來偵測及診斷基礎結構與應用程式之間的問題。 如需更完整的分析，以及從多個元件或支援應用程式的相依性收集的資料相互關聯，您必須使用 Azure 監視器記錄。

- 建立適用于一組核心應用程式和服務元件的智慧型警示，協助減少複雜信號的動態閾值的警示雜訊，並根據機器學習服務演算法使用警示匯總，協助您快速識別問題。

- 定義查詢和儀表板的程式庫，以支援 IT 組織中各種不同角色的需求。

- 定義標準和方法，以啟用跨混合式和雲端資源的監視、每個資源的監視基準、警示閾值等等。  

- 設定角色型存取控制（RBAC），讓您只授與使用者和群組所需的存取權，以從他們負責管理的資源監視資料。

- 包含自動化和自助式服務，可讓每個小組視需要建立、啟用和調整其監視和警示設定。

## <a name="private-cloud-monitoring"></a>私人雲端監視

您可以使用 System Center Operations Manager 全面監視 Azure Stack。 具體而言，您可以監視在租使用者中執行的工作負載、資源層級、虛擬機器，以及裝載 Azure Stack （實體伺服器和網路交換器）的基礎結構。

您也可以使用 Azure Stack 所包含的[基礎結構監視功能](https://docs.microsoft.com/azure/azure-stack/azure-stack-monitor-health)組合來達成整體監視。 這些功能可協助您在 Azure Stack 中查看 Azure Stack 區域和[Azure 監視器服務](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-metrics-azure-data)的健康情況和警示，以提供大部分服務的基本層級基礎結構計量和記錄。

如果您已投資 Operations Manager，請使用 Azure Stack 管理元件來監視 Azure Stack 部署的可用性和健全狀況狀態，包括區域、資源提供者、更新、更新執行、縮放單位、單位節點、基礎結構角色和其實例（由硬體資源組成的邏輯實體）。 此管理元件會使用健康情況和更新資源提供者 REST Api 與 Azure Stack 進行通訊。 若要監視實體伺服器和存放裝置，請使用 OEM 廠商的管理元件（例如，由聯想、Hewlett Packard 或 Dell 提供）。 Operations Manager 可以使用 SNMP 以原生方式監視網路交換器，以收集基本統計資料。 藉由下列兩個基本步驟，可以使用 Azure 管理元件來監視租使用者工作負載。 設定您想要監視的訂用帳戶，然後新增該訂用帳戶的監視。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [收集正確的資料](./data-collection.md)
