---
title: 雲端監視指南–雲端部署模型的監視策略
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 選擇何時使用 Azure 監視器或 System Center Operations Manager Microsoft Azure
author: MGoedtel
ms.author: magoedte
ms.date: 10/04/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: operate
services: azure-monitor
ms.openlocfilehash: 9fdef4d5d3d9cd39d16566221262330ef110bb3a
ms.sourcegitcommit: 35c162d2d09ec1c4a57d3d57a5db1d56ee883806
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2019
ms.locfileid: "72548176"
---
# <a name="cloud-monitoring-guide-monitoring-strategy-for-cloud-deployment-models"></a>雲端監視指南：雲端部署模型的監視策略

本文包含我們針對每個雲端部署模型所建議的監視策略（根據下列準則）：

- 您必須維護 Operations Manager 或其他企業監視平臺的承諾，因為與您的 IT 作業程式、知識和專業知識，或是 Azure 監視器中尚未提供特定功能的整合。
- 您必須在內部部署和公用雲端中，或只監視雲端中的工作負載。
- 您的雲端遷移策略包括現代化 IT 作業，並移至我們的雲端監視服務和解決方案。
- 您的重要系統可能是以空中或實體隔離、裝載于私人雲端或實體硬體上，而且需要進行監視。

我們的策略包括監視基礎結構（計算、儲存體和伺服器工作負載）、應用程式（使用者、例外狀況和用戶端）和網路資源的支援，以提供完整的服務導向監視的觀點。

## <a name="azure-cloud-monitoring"></a>Azure 雲端監視

Azure 監視器是 Azure 原生平臺服務，可提供用來監視 Azure 資源的單一來源。 其設計適用于建置於 Azure 上的雲端解決方案，並支援以 VM 工作負載為基礎的商務功能，或使用微服務和其他平臺資源的複雜架構。 它會監視堆疊的所有層，從租使用者服務（例如 Azure Active Directory Domain Services）和訂用帳戶層級事件和 Azure 服務健康狀態開始。 它也會監視基礎結構資源，例如 Vm、儲存體和網路資源，以及最上層的應用程式。 監視每個相依性，並收集每個相依性可發出的正確信號，讓您可檢視性應用程式和所需的重要基礎結構。

下表摘要說明監視堆疊的每個層級的建議方法。

<!-- markdownlint-disable MD033 -->

層次 | 資源 | Scope | 方法
---|---|---|----
Application | 在 .NET、.NET Core、JAVA、JavaScript 和 node.js 平臺上執行的 Web 應用程式，位於 Azure VM、Azure App 服務、Azure Service Fabric、Azure Functions 和 Azure 雲端服務 | 監視即時 web 應用程式，以自動偵測效能異常、找出程式碼例外狀況和問題，以及收集使用者行為分析。 |  Azure 監視器（Application Insights）
Azure 資源-PaaS | 1. Azure 資料庫服務（例如 SQL 或 mySQL） | 1. 適用于 SQL 效能計量的 Azure 資料庫。 | 1. 啟用診斷記錄以將 SQL 資料串流至 Azure 監視器記錄。
Azure 資源-IaaS | 1. Azure 儲存體<br/> 2. Azure 應用程式閘道<br/> 3. 網路安全性群組<br/> 4. Azure 流量管理員<br/> 5. 虛擬機器<br/> 6. Azure Kubernetes Service/Azure 容器實例 | 1. 容量、可用性和效能。<br/> 2. 效能和診斷記錄（活動、存取、效能和防火牆）。<br/> 3. 在套用規則時監視事件，以及規則套用到拒絕或允許的次數的規則計數器。<br/> 4. 監視端點狀態可用性。<br/> 5. 監視來賓 VM OS 中的容量、可用性和效能。 對應每個 VM 上裝載的應用程式相依性，包括伺服器之間的作用中網路連線的可見度、輸入和輸出連線延遲，以及任何 TCP 連接架構的埠。<br/> 6. 監視容器和容器實例上執行之工作負載的容量、可用性和效能。 | 1. Blob 儲存體的儲存體計量。<br/> 2. 啟用診斷記錄，並設定串流以 Azure 監視器記錄。<br/> 3. 啟用網路安全性群組的診斷記錄，並設定串流以 Azure 監視器記錄。<br/> 4. 啟用流量管理員端點的診斷記錄，並設定串流來 Azure 監視器記錄。<br/> 5. 啟用適用於 VM 的 Azure 監視器<br/> 6. 啟用容器的 Azure 監視器
網路| 您的虛擬機器與一或多個端點（另一個 VM、完整功能變數名稱、統一資源識別項或 IPv4 位址）之間的通訊。 | 監視在 VM 與端點之間發生的可連線性、延遲和網路拓撲變更。 | Azure 網路監看員
Azure 訂用帳戶 | Azure 服務健康狀態和基本資源健康狀態 | <li> 在服務或資源上執行的系統管理動作。<br/><li> 具有 Azure 服務的服務健全狀況處於降級或無法使用的狀態。<br/><li> 從 Azure 服務的觀點來看，偵測到 Azure 資源的健康情況問題。<br/><li> 使用 Azure 自動調整執行的作業會指出失敗或例外狀況。 <br/><li> 以 Azure 原則執行的作業，表示發生允許或拒絕的動作。<br/><li> Azure 資訊安全中心所產生的警示記錄。 |在活動記錄中傳遞，以使用 Azure Resource Manager 進行監視和警示。
Azure 租用戶|Azure Active Directory || 啟用診斷記錄，並設定串流處理以 Azure 監視器記錄。

<!-- markdownlint-enable MD033 -->

## <a name="hybrid-cloud-monitoring"></a>混合式雲端監視

對於許多組織而言，雲端必須逐漸進行，其中混合式雲端模型是旅程圖中最常見的第一個步驟。 您可以謹慎地選取適當的應用程式子集和基礎結構，以開始進行遷移，同時避免中斷您的業務。 不過，由於我們提供兩個支援此雲端模型的監視平臺，因此 IT 決策者會感到困惑，這是支援其商務和 IT 營運目標的最佳選擇。 我們將探討幾個因素來解決不確定性，並讓您瞭解所要考慮的平臺。

要考慮的一些重要技術層面如下：

- 您必須從支援工作負載的 Azure 資源收集資料，並將其轉送至您現有的內部部署或受控服務提供者工具。

- 您需要在 System Center Operations Manager 中維持目前的投資，並將其設定為監視在 Azure 中執行的 IaaS 和 PaaS 資源。 （選擇性）因為您要根據需求來監視兩個具有不同特性的環境，所以您會決定與 Azure 監視器的整合，以支援您的策略。

- 在您的現代化策略中，為了降低成本和複雜度而進行標準化，您可以認可 Azure 監視器來監視 Azure 和公司網路上的資源。

下表摘要說明根據一組通用準則來監視混合式雲端模型時，Azure 監視器和 System Center Operations Manager 支援的需求。

<!-- markdownlint-disable MD033 -->

|需求 | Azure Monitor | Operations Manager |
|:--|:---|:---|
|基礎結構需求 | **否** | **是**<br> 至少需要一個管理伺服器，以及一個 SQL server 來裝載運算元據庫和報表資料倉儲資料庫。 需要 HA/DR、多個網站中的機器、不受信任的系統，以及其他複雜的設計考慮時，會變得更複雜。|
|有限的連線能力-無網際網路<br> 或隔離的網路 | **否** | **是** |
|受限制的連線能力控制網際網路存取 | **是** | **是** |
|有限的連線-經常中斷連線 | **是** | **是** |
|可設定的健全狀況監視 | **否** | **是** |
| Web 應用程式可用性測試（獨立網路） | **是，有限**<br> Azure 監視器在此區域提供有限的支援，且需要自訂的防火牆例外。 | **是** |
| Web 應用程式可用性測試（全域散發） | **否** | **是** |
|監視 VM 工作負載 | **是，有限**<br> 可以收集 IIS 和 SQL Server 錯誤記錄檔、Windows 事件和效能計數器。 需要建立自訂查詢、警示和視覺效果。 | **是**<br> 支援使用可用的管理元件監視大部分的伺服器工作負載。 需要 VM 上的 Log Analytics Windows 代理程式或 Operations Manager 代理程式，回報至公司網路上的管理群組。|
|監視 Azure IaaS | **是** | **是**<br> 支援從公司網路監視大部分的基礎結構。 透過 Azure 管理元件，追蹤 Azure Vm、SQL 和儲存體的可用性狀態、計量和警示。|
|監視 Azure PaaS | **是** | **是，有限**<br> 根據 Azure 管理元件中支援的功能。 |
|Azure 服務監視 | **是**<br> | **是**<br> 雖然目前沒有透過管理元件提供的 Azure 服務健康狀態的原生監視，您可以建立自訂工作流程來查詢 Azure 服務健康狀態警示。 使用 Azure REST API 透過現有的通知來取得警示。|
|新式 web 應用程式監視 | **是** | **否** |
|舊版 web 應用程式監視 | **是，受限於 SDK 會有所不同**<br> 支援監視較舊版本的 .NET 和 JAVA web 應用程式。 | **是，有限** |
|監視 Azure Kubernetes Service 容器 | **是** | **否** |
|監視 Docker/Windows 容器 | **是** | **否** |
|網路效能監視 | **是** | **是，有限**<br> 支援可用性檢查，並使用來自公司網路的 SNMP 通訊協定從網路裝置收集基本統計資料。|
|互動式資料分析 | **是** | **否**<br> 依賴 SQL Server Reporting Services 預先定義的報表、協力廠商的視覺效果解決方案，或自訂的 Power BI 執行。 Operations Manager 資料倉儲的規模和效能限制。 與 Azure 監視器記錄整合，以作為資料匯總需求的替代方案。 藉由設定 Log Analytics 連接器來達成整合。|
|端對端診斷、根本原因分析，以及及時的疑難排解 | **是** | **是，有限**<br> 僅支援內部部署基礎結構和應用程式的端對端診斷和疑難排解。 使用其他 System Center 元件或合作夥伴解決方案。|
|互動式視覺效果（儀表板） | **是** | **是，有限**<br> 透過其 HTLM5 web 主控台提供基本的儀表板，或從像是方形 Up 和 Savision 等合作夥伴解決方案獲得先進的經驗。 |
|與 IT/DevOps 工具整合 | **是** | **是，有限** |

<!-- markdownlint-enable MD033 -->

### <a name="collect-and-stream-monitoring-data-to-third-party-or-on-premises-tools"></a>收集監視資料並串流至協力廠商或內部部署工具

若要從 Azure 基礎結構和平臺資源收集計量和記錄，您必須啟用這些資源的 Azure 診斷記錄。 此外，透過 Azure Vm，您可以藉由啟用 Azure 診斷延伸模組，從客體作業系統收集計量和記錄。 若要將從 Azure 資源發出的診斷資料轉送到內部部署工具或受控服務提供者，請設定[事件中樞](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-stream-event-hubs)以將資料串流處理到它們。

### <a name="monitor-with-system-center-operations-manager"></a>使用 System Center Operations Manager 進行監視

雖然 System Center Operations Manager 原本設計為內部部署解決方案，可監視在 IT 環境中執行的應用程式、工作負載和基礎結構，但其演變為包含雲端監視功能並與整合Azure、Office 365 和 Amazon Web Services （AWS）。 它可以透過設計和更新的管理元件來監視這些不同的環境，以支援它們。  

對於在 Operations Manager 進行大量投資的客戶，若要達成與 IT 服務管理程式和工具緊密整合的全面監視，或客戶對 Azure 的新功能，有問題可以理解：

- Operations Manager 是否可以繼續傳遞價值，並能讓它更有商業意義嗎？

- 如果 Operations Manager 中的功能讓它適合我們的 IT 組織嗎？

- 整合 Operations Manager 與 Azure 監視器提供我們需要的符合成本效益且全面的監視解決方案嗎？

如果您已經投資 Operations Manager，就不需要專注于規劃遷移來立即取代。 透過現有的 Azure 或其他雲端提供者作為您自己的內部部署網路的延伸模組，Operations Manager 可以監視來賓 Vm 和 Azure 資源，就像是在您的公司網路上一樣。 這需要有足夠頻寬的網路與 Azure 虛擬網路之間的可靠網路連線。

若要監視在 Azure 中執行的工作負載，您需要：

- 用來收集 Azure 服務所發出的效能計量（例如 web 和背景工作角色）、Application Insights 可用性測試（webtest）、服務匯流排等的[azure 管理元件](https://www.microsoft.com/download/details.aspx?id=50013)。管理元件會使用 Azure REST API 來監視這些資源的可用性和效能。 某些 Azure 服務類型在管理元件中沒有任何計量或任何預先定義的監視，但是仍然可以透過 Azure 管理元件中針對探索到的服務所定義的關聯性進行監視。

- [Azure SQL Database 管理元件](https://www.microsoft.com/download/details.aspx?id=38829)，可使用 azure REST API 和 t-SQL 查詢 SQL Server 系統檢視，來監視 azure sql 資料庫和 azure sql Database 伺服器的可用性和效能。

- 若要監視 VM 上執行的虛擬作業系統和工作負載（例如 SQL Server、IIS 或 Apache Tomcat），您必須下載並匯入支援應用程式、服務和作業系統的管理元件。

知識是在管理元件中定義的，它會說明如何監視個別的相依性和元件。 這兩個 Azure 管理元件都需要在 Azure 和 Operations Manager 中執行一組設定步驟，才能開始監視這些資源。

在應用層，Operations Manager 提供一些舊版 .NET 和 JAVA 的基本應用程式效能監視功能。 如果混合式雲端環境內的特定應用程式以離線或網路隔離模式運作，使其無法與公用雲端服務通訊，Operations Manager 應用程式效能監視（APM）可能是的一個可行選項某些有限的案例。 針對不是在舊版平臺上執行的應用程式，裝載于內部部署和任何公用雲端中，以允許透過防火牆（直接或透過 proxy）通訊至 Azure，請使用 Azure 監視器 Application Insights。 這提供了深度、程式碼層級的監視，並具有 ASP.NET、ASP.NET Core、JAVA、JavaScript 和 node.js 的頂級支援。

對於可從外部連線的任何 web 應用程式，您應該啟用一種稱為「[可用性監視]( https://docs.microsoft.com/azure/azure-monitor/app/monitor-web-app-availability)」的綜合交易。 請務必知道您的應用程式或應用程式所依賴的重要 HTTP/HTTPS 端點是否可用且有回應。 Application Insights 可用性監視可讓您從多個 Azure 資料中心執行測試，並從全球的觀點提供應用程式健康情況的深入解析。

雖然 Operations Manager 能夠監視裝載于 Azure 中的資源，但有幾個優點可以納入 Azure 監視器，因為其優勢會克服 Operations Manager 的限制，並建立強大的基礎來支援最終的遷移從這個。 在這裡，我們將使用我們的建議來檢查每個，以納入混合式監視策略中的 Azure 監視器。  

#### <a name="disadvantage-of-using-operations-manager-by-itself"></a>使用 Operations Manager 本身的缺點

1. 在 Operations Manager 中分析監視資料通常是使用管理元件中定義的預先定義的視圖來執行，這些是從主控台、從 SQL Server Reporting Services （SSRS）報告，或由使用者所建立的自訂視圖中進行存取。 目前無法對資料執行臨機操作分析。 Operations Manager 報告沒有彈性，提供監視資料長期保留的資料倉儲不會調整或執行良好，而撰寫 T-sql 語句、開發 Power BI 解決方案或使用協力廠商解決方案的專業知識則是必須支援 IT 組織中不同角色的需求。

2. Operations Manager 中的警示不會提供複雜運算式的支援，並包含相互關聯邏輯，以協助減少警示雜訊並將警示分組，以顯示兩者之間的關聯性，以協助識別的根本原因。問題.

#### <a name="advantage-of-operations-manager--azure-monitor"></a>Operations Manager + Azure 監視器的優勢

1. Azure 監視器記錄是解決 Operations Manager 限制的解決方案，並補充 Operations Manager 的資料倉儲資料庫，以收集重要的效能和記錄資料。 Azure 監視器提供更佳的分析、查詢大型資料量的效能，以及與 Operations Manager 資料倉儲相比的保留。 其查詢語言可讓您建立更複雜且精密的查詢，並可在數秒內跨數 tb 的資料執行查詢。 您可以快速地將資料轉換成圓形圖、時間圖表和其他許多視覺效果。 您不再受到限制，您可以在 Operations Manager 中使用以 SQL Server Reporting Services、自訂 SQL 查詢為基礎的報表，或是分析此資料的其他因應措施。

2. 藉由執行 Azure 監視器警示管理解決方案，提供改良的警示體驗。 Operations Manager 管理群組中產生的警示可以轉送到 Azure 監視器 Logs Analytics 工作區。 您可以設定負責將警示從 Operations Manager 轉送到 Azure 監視器記錄的訂用帳戶，只轉送特定警示。 例如，您可以透過單一窗格，只轉送符合您查詢準則以進行問題管理的支援，以及調查失敗或問題根本原因的警示。 此外，您可以將其他記錄資料與 Application Insights 或其他來源相互關聯，以取得可協助改善使用者體驗、增加執行時間，以及縮短解決事件的時間的深入解析。

3. 從 Azure 中的簡單或多層式架構監視雲端原生基礎結構和應用程式，並使用 Operations Manager 來監視內部部署基礎結構。 這包括一或多個 Vm、放置在可用性設定組或虛擬機器擴展集內的多個 Vm，或部署到在 Windows Server 或 Linux 容器上執行之 Azure Kubernetes Service （AKS）的容器化應用程式。

4. 使用 System Center Operations Manager 健全狀況檢查解決方案，定期主動評估 System Center Operations Manager 管理群組的風險和健康情況。 這可以取代或補充您已新增至管理群組的任何自訂功能。

5. 透過適用於 VM 的 Azure 監視器的對應功能，它可讓您監視 Azure Vm 與內部部署 Vm 之間網路連線的標準連線計量。 這些計量包括回應時間、每分鐘的要求數、流量輸送量和連結。 您可以識別失敗的連線、疑難排解、執行遷移驗證、執行安全性分析，以及驗證服務的整體架構。 Map 可以自動探索 Windows 和 Linux 系統上的應用程式元件，並對應服務之間的通訊。 這可協助您識別不知道的連線和相依性、規劃和驗證 Azure 的遷移，以及在事件解決期間將推測降到最低。

6. 使用網路效能監控，監視之間的網路連線：

   - 您的公司網路和 Azure。

   - 任務關鍵性多層式應用程式和微服務。

   - 使用者位置和 web 應用程式（HTTP/HTTPs）。

   此策略會提供網路層的可見度，而不需要 SNMP。 它也可以顯示在互動式拓撲圖中，這是來源與目的地端點之間路由的逐一躍點拓撲。 比起嘗試透過 Operations Manager 中的網路監視，或您的環境中目前使用的其他網路監視工具來達成相同的結果，這是較佳的選擇。

### <a name="monitor-with-azure-monitor"></a>使用 Azure 監視器進行監視

雖然遷移至雲端會帶來許多挑戰，但它也包含許多機會。 它可讓您的組織從一或多個內部部署企業監視工具進行遷移，不僅可能降低資本支出和營運成本，還能受益于雲端監視平臺（如 Azure 監視器所提供）的優點雲端規模。 檢查您的監視和警示需求、現有監視工具的設定、工作負載轉換至雲端，並在您的方案完成後設定 Azure 監視器。

- 從簡單或多層式架構監視混合式基礎結構和應用程式，其中元件會裝載于 Azure、其他雲端提供者和您的公司網路。 這包括一或多個 Vm、放置在可用性設定組或虛擬機器擴展集內的多個 Vm，或部署到在 Windows Server 或 Linux 容器上執行之 Azure Kubernetes Service （AKS）的容器化應用程式。

- 啟用適用於 VM 的 Azure 監視器、容器的 Azure 監視器，以及 Application Insights 來偵測及診斷基礎結構與應用程式之間的問題。 如需更完整的分析，以及從多個元件或支援應用程式的相依性收集的資料相互關聯，您必須使用 Azure 監視器記錄。

- 建立可套用至一組核心應用程式和服務元件的智慧型警示、協助減少複雜信號的動態閾值的警示雜訊，並根據機器學習服務演算法使用警示匯總，以協助快速識別問題.

- 定義查詢和儀表板的程式庫，以支援 IT 組織中不同角色的需求。

- 定義標準和方法，以啟用跨混合式和雲端資源的監視、每個資源的監視基準、警示閾值等等。  

- 設定角色型存取控制（RBAC），讓您只授與使用者和群組使用他們負責管理之資源的監視資料所需的存取權數量。

- 包含自動化和自助式服務，讓每個小組可以視需要建立、啟用和調整其監視和警示設定。

## <a name="private-cloud-monitoring"></a>私人雲端監視

您可以使用 System Center Operations Manager 全面監視 Azure Stack。 具體而言，您可以監視在租使用者中執行的工作負載、資源層級、虛擬機器上的 Azure Stack，以及裝載的基礎結構（實體伺服器和網路交換器）。 您也可以使用 Azure Stack 中包含的[基礎結構監視功能](https://docs.microsoft.com/azure/azure-stack/azure-stack-monitor-health)組合來達成整體監視。 這些功能可協助您在 Azure Stack 中查看 Azure Stack 區域和[Azure 監視器服務](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-metrics-azure-data)的健康情況和警示，以提供大部分服務的基本層級基礎結構計量和記錄。

如果您已投資 Operations Manager，請使用 Azure Stack 管理元件來監視 Azure Stack 部署的可用性和健全狀況狀態。 這包括區域、資源提供者、更新、更新執行、縮放單位、單位節點、基礎結構角色和其實例（由硬體資源組成的邏輯實體）。 它會使用健康情況和更新資源提供者 REST Api 來與 Azure Stack 進行通訊。 若要監視實體伺服器和存放裝置，請使用 OEM 廠商的管理元件（例如，由聯想、Hewlett Packard 或 Dell 提供）。 Operations Manager 可以使用 SNMP 通訊協定，以原生方式監視網路交換器，以收集基本統計資料。 藉由下列兩個基本步驟，可以使用 Azure 管理元件來監視租使用者工作負載。 設定您想要監視的訂用帳戶，然後新增該訂用帳戶的監視。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [收集正確的資料](./data-collection.md)
