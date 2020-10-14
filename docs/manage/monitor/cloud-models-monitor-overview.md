---
title: 雲端部署模型的監視策略
description: 使用「適用于 Azure 的雲端採用架構」來瞭解要採用的雲端管理監視策略。
author: MGoedtel
ms.author: magoedte
ms.date: 10/09/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: ca376b204b7c6decc7e75a50d5438c6e41905317
ms.sourcegitcommit: d81a822575820115d9814b0fc6c05ae33e535825
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/14/2020
ms.locfileid: "92058779"
---
<!-- cSpell:ignore Savision -->

# <a name="cloud-monitoring-guide-monitoring-strategy-for-cloud-deployment-models"></a>雲端監視指南：雲端部署模型的監視策略

本文包含我們針對每個雲端部署模型建議的監視策略，根據下列準則：

- 您必須維持對 Operations Manager 或其他企業監視平臺的承諾，因為它已與您的 IT 營運流程、知識和專業知識整合，或某些功能尚未在 Azure 監視器中提供。
- 您必須監視內部部署和公用雲端中的工作負載，或只監視雲端中的工作負載。
- 您的雲端遷移策略包括現代化 IT 作業，並移至我們的雲端監視服務和解決方案。
- 您可能有很重要的系統，這些系統是以空調或實體方式隔離，或是裝載在私用雲端或實體硬體上，而且這些系統都需要進行監視。

我們的策略包括支援監視基礎結構 (計算、儲存體和伺服器工作負載) 、應用程式 (使用者、例外狀況和用戶端) ，以及網路資源。 它提供完整的服務導向監視觀點。

## <a name="azure-cloud-monitoring"></a>Azure 雲端監視

Azure 監視器是 Azure 原生平臺服務，提供監視 Azure 資源的單一來源。 它是專為下列雲端解決方案所設計：

- 是以 Azure 為基礎。
- 支援以虛擬機器 (VM) 工作負載或使用微服務和其他平臺資源的複雜架構為基礎的商務功能。

它會監視堆疊的所有層，從租使用者服務開始，例如 Azure Active Directory Domain Services，以及訂用帳戶層級事件和 Azure 服務健康狀態。

它也會監視基礎結構資源，例如 Vm、儲存體和網路資源。 在最上層，它會監視您的應用程式。

藉由監視每個相依性，並收集每個相依性可發出的正確信號，您就可以可檢視性應用程式和所需的金鑰基礎結構。

下表摘要說明監視堆疊各層的建議方法：

| 層 | 資源 | 影響範圍 | 方法 |
|---|---|---|---|
| 應用程式 | 在 .NET、.NET Core、JAVA、JavaScript 和 Node.js 平臺上，于 Azure VM 上執行的 web 應用程式，Azure App Service、Azure Service Fabric、Azure Functions 和 Azure 雲端服務。 | 監視即時 web 應用程式以自動偵測效能異常、識別程式碼例外狀況和問題，以及收集使用者行為分析。 | Application Insights (Azure 監視器) 的功能。 |
| Azure 資源-平臺即服務 (PaaS)  | Azure 資料庫服務 (例如 SQL 或 MySQL) 。 | 適用于 SQL 效能度量的 Azure 資料庫。 | 啟用診斷記錄，以將 SQL 資料串流至 Azure 監視器記錄。 |
| Azure 資源-基礎結構即服務 (IaaS)  | 1. Azure 儲存體 <br> 2. Azure [負載平衡服務](/azure/architecture/guide/technology-choices/load-balancing-overview#azure-load-balancing-services) <br> 3. 網路安全性群組 <br> 4. Azure 虛擬機器 <br> 5. Azure Kubernetes Service/Azure 容器實例 | 1. 容量、可用性和效能。 <br> 2. 效能和診斷記錄 (活動、存取、效能和防火牆) 。 <br> 3. 套用規則時監視事件，以及將規則套用至拒絕或允許的次數規則計數器。 <br> 4. 監視來賓 VM 作業系統 (OS) 中的容量、可用性和效能。 對應每個 VM 上裝載的應用程式相依性，包括伺服器之間的作用中網路連線可見度、輸入和輸出連線延遲，以及任何 TCP 連接架構之間的埠。 <br> 5. 監視容器和容器實例上執行之工作負載的容量、可用性和效能。 | 針對第一個資料行中的專案1到5，系統會自動收集平臺計量和活動記錄，並可在 Azure 監視器中使用以進行分析和警示。 <br> 設定診斷設定，以將資源記錄轉送至 Azure 監視器記錄。 <br> 啟用 [適用於 VM 的 Azure 監視器](/azure/azure-monitor/insights/vminsights-overview)。 <br> 啟用 [容器的 Azure 監視器](/azure/azure-monitor/insights/container-insights-overview)。 |
| 網路 | 虛擬機器與一或多個端點之間的通訊 (另一個 VM、完整功能變數名稱、統一資源識別項或 IPv4 位址) 。 | 監視 VM 與端點之間發生的連線能力、延遲和網路拓撲變更。 | Azure 網路監看員。 |
| Azure 訂用帳戶 | 從 Azure 服務的觀點來 Azure 服務健康狀態和基本資源健康狀態。 | <li> 對服務或資源執行的系統管理動作。 <li> Azure 服務的服務健康狀態為「已降級」或「無法使用」狀態。 <li> 從 Azure 服務的觀點來看，Azure 資源偵測到健康情況問題。 <li> 使用 Azure 自動調整來執行的作業，指出失敗或例外狀況。 <li> 使用 Azure 原則執行的作業，表示發生允許或拒絕的動作。 <li> Azure 資訊安全中心所產生的警示記錄。 | 使用 Azure 監視器在活動記錄中傳遞，以進行監視和警示。 |
| Azure 租用戶 | Azure Active Directory | Azure AD audit 記錄和登入記錄檔。 | 啟用診斷記錄，並設定串流處理以 Azure 監視器記錄。 |

## <a name="hybrid-cloud-monitoring"></a>混合式雲端監視

對於許多組織而言，轉換至雲端必須逐漸進行，其中混合式雲端模型是旅程圖中最常見的第一個步驟。 您可以謹慎地選取適當的應用程式和基礎結構子集來開始進行遷移，同時避免業務中斷。 不過，因為我們提供了兩個支援此雲端模型的監視平臺，所以 IT 決策者可能不確定哪一個是支援其業務和 IT 營運目標的最佳選擇。

在本節中，我們會藉由檢查數個因素並提供瞭解所要考慮的平臺，來解決不確定性。

請記住下列主要技術層面：

- 您必須從支援工作負載的 Azure 資源收集資料，並將其轉送至您現有的內部部署或受控服務提供者工具。

- 您必須維持 System Center Operations Manager 目前的投資，並將其設定為監視在 Azure 中執行的 IaaS 和 PaaS 資源。 （選擇性）因為您要根據需求監視具有不同特性的兩個環境，所以您需要判斷與 Azure 監視器的整合如何支援您的策略。

- 作為現代化策略的一部分，以標準化單一工具來降低成本和複雜性，您需要認可 Azure 監視器，以在 Azure 和您的公司網路上監視資源。

下表摘要說明根據一組通用準則來監視混合式雲端模型 Azure 監視器和 System Center Operations Manager 支援的需求。

| 需求 | Azure 監視器 | Operations Manager |
|---|---|---|
| 基礎結構需求 | 否 | 是 <br><br> 至少需要一台管理伺服器和 SQL server，才能裝載運算元據庫和報表資料倉儲資料庫。 當需要高可用性和嚴重損壞修復，而且有多個網站中的機器、不受信任的系統和其他複雜的設計考慮時，複雜性就會增加。 |
| 有限的連線能力-沒有網際網路或隔離的網路 | 否 | 是 |
| 有限的連線能力控制網際網路存取 | 是 | 是 |
| 有限的連接-經常中斷連線 | 是 | 是 |
| 可設定的健全狀況監視 | 否 | 是 |
| Web 應用程式可用性測試 (隔離網路)  | 是，有限 <br><br> Azure 監視器在此區域的支援有限，且需要自訂防火牆例外。 | 是 |
| Web 應用程式可用性測試 (全域散發)  | 否 | 是 |
| 監視 VM 工作負載 | 是，有限 <br><br> 可以收集 IIS 和 SQL Server 錯誤記錄檔、Windows 事件和效能計數器。 需要建立自訂查詢、警示和視覺效果。 | 是 <br><br> 支援使用可用的管理元件監視大部分的伺服器工作負載。 需要 VM 上的 Log Analytics Windows 代理程式或 Operations Manager 代理程式，並回報至公司網路上的管理群組。 |
| 監視 Azure IaaS | 是 | 是 <br><br> 支援從公司網路監視大部分的基礎結構。 透過 Azure 管理元件追蹤 Azure Vm、SQL 和儲存體的可用性狀態、計量和警示。 |
| 監視 Azure PaaS | 是 | 是，有限 <br><br> 根據 Azure 管理元件所支援的功能。 |
| Azure 服務監視 | 是 | 是 <br><br> 雖然目前沒有透過管理元件提供 Azure 服務健康狀態的原生監視，您也可以建立自訂工作流程來查詢服務健康狀態警示。 使用 Azure REST API 透過您現有的通知取得警示。 |
| 新式 web 應用程式監視 | 是 | 否 |
| 舊版 web 應用程式監視 | 是、受限、因 SDK 而異 <br><br> 支援監視較舊版本的 .NET 和 JAVA web 應用程式。 | 是，有限 |
| 監視 Azure Kubernetes Service 容器 | 是 | 否 |
| 監視 Docker 或 Windows 容器 | 是 | 否 |
| 網路效能監視 | 是 | 是，有限 <br><br> 支援可用性檢查，並使用來自公司網路的簡易網路管理通訊協定 (SNMP) ，收集網路裝置的基本統計資料。 |
| 互動式資料分析 | 是 | 否 <br><br> 依賴 SQL Server Reporting Services 的預先定義或自訂報表、協力廠商的視覺效果解決方案，或自訂 Power BI 的執行。 Operations Manager 資料倉儲有調整規模和效能限制。 將與 Azure 監視器記錄整合，以做為資料匯總需求的替代方案。 您可以設定 Log Analytics 連接器以達成整合。 |
| 端對端診斷、根本原因分析和及時疑難排解 | 是 | 是，有限 <br><br> 僅支援內部部署基礎結構和應用程式的端對端診斷和疑難排解。 使用其他 System Center 元件或合作夥伴解決方案。 |
|  (儀表板的互動式視覺效果)  | 是 | 是，有限 <br><br> 透過其 HTML5 web 主控台或來自合作夥伴解決方案的先進體驗（例如，平方向上和 Savision），提供重要的儀表板。 |
| 與 IT 或 DevOps 工具整合 | 是 | 是，有限 |

### <a name="collect-and-stream-monitoring-data-to-third-party-or-on-premises-tools"></a>收集監視資料並將其串流至協力廠商或內部部署工具

若要從 Azure 基礎結構和平臺資源收集計量和記錄，您必須啟用這些資源的 Azure 診斷記錄。 此外，使用 Azure Vm，您可以藉由啟用 Azure 診斷擴充功能，從來賓 OS 收集計量和記錄。 若要將從 Azure 資源發出的診斷資料轉送至內部部署工具或受控服務提供者，請設定 [事件中樞](/azure/azure-monitor/platform/diagnostic-logs-stream-event-hubs) 將資料串流至這些資料。

### <a name="monitor-with-system-center-operations-manager"></a>使用 System Center Operations Manager 監視

雖然 System Center Operations Manager 原本是設計成內部部署解決方案，以便在您的 IT 環境中執行的應用程式、工作負載和基礎結構元件之間進行監視，但仍會演變為包含雲端監視功能。 它會與 Azure、Microsoft 365 和 Amazon Web Services (AWS) 整合。 它可以透過設計和更新的管理元件來監視這些不同的環境，以支援這些環境。  

如果客戶對 Operations Manager 進行了大幅的投資，以達成與 IT 服務管理程式和工具緊密整合的全面監視，或針對 Azure 的新客戶，則可瞭解如何提出下列問題：

- Operations Manager 是否能繼續提供價值，並能使其成為商業意義？
- Operations Manager 的功能使它適合我們的 IT 組織嗎？
- Operations Manager 與 Azure 監視器整合可提供符合成本效益和全面的監視解決方案嗎？

如果您已經投資 Operations Manager，就不需要將焦點放在規劃遷移來立即取代。 有了 Azure 或其他雲端提供者（以您自己的內部部署網路延伸的形式存在），Operations Manager 可以監視來賓 Vm 和 Azure 資源，就像是在您的公司網路上一樣。 這種方法需要您的網路與 Azure 中具有足夠頻寬的虛擬網路之間有可靠的網路連線。

若要監視在 Azure 中執行的工作負載，您需要：

- [適用于 Azure 的 System Center Operations Manager 管理元件](https://www.microsoft.com/download/details.aspx?id=50013)。 它會收集 Azure 服務（例如 web 和背景工作角色）所發出的效能計量、Application Insights 可用性測試 (web 測試) 、Azure 服務匯流排等。 管理元件會使用 Azure REST API 來監視這些資源的可用性和效能。 某些 Azure 服務類型在管理元件中沒有計量或預先定義的監視器，但您仍然可以透過 Azure 管理元件中針對已探索服務所定義的關聯性來監視它們。

- 用來 [Azure SQL Database 的管理元件](https://www.microsoft.com/download/details.aspx?id=38829) ，可使用 azure REST API 和 t-SQL 查詢來 SQL Server 系統檢視，來監視 azure sql 資料庫和 azure sql Database 伺服器的可用性和效能。

- 若要監視 VM 上執行的虛擬作業系統和工作負載，例如 SQL Server、IIS 或 Apache Tomcat，您需要下載並匯入支援應用程式、服務和作業系統的管理元件。

知識定義于管理元件中，其描述如何監視個別的相依性和元件。 這兩個 Azure 管理元件都需要在 Azure 中執行一組設定步驟，並 Operations Manager，才能開始監視這些資源。

在應用層，Operations Manager 為某些舊版的 .NET 和 JAVA 提供基本的應用程式效能監視功能。 如果您的混合式雲端環境中的特定應用程式以離線或網路隔離模式運作，使其無法與公用雲端服務通訊，則 Operations Manager 的應用程式效能監視 (APM) 可能是特定有限案例的可行選項。 如果應用程式不是在舊版平臺上執行，而是裝載在內部部署和任何公用雲端中，以允許透過防火牆進行通訊 (直接或透過 proxy) 到 Azure，請使用 Azure 監視器 Application Insights。 這種服務提供深度的程式碼層級監視，並具備 ASP.NET、ASP.NET Core、JAVA、JavaScript 和 Node.js 的頂級支援。

針對可從外部連線的任何 web 應用程式，您應該啟用一種稱為 [可用性監視](/azure/azure-monitor/app/monitor-web-app-availability)的綜合交易類型。 請務必瞭解您的應用程式或應用程式所依賴的重要 HTTP/HTTPS 端點是否可用且有回應。 透過 Application Insights 可用性監視，您可以從多個 Azure 資料中心執行測試，並從全球觀點深入瞭解應用程式的健康情況。

雖然 Operations Manager 可監視 Azure 中裝載的資源，但有數個優點可包含 Azure 監視器，因為它的強項可克服 Operations Manager 的限制，而且可以建立強大的基礎來支援從它的最終遷移。 在這裡，我們將探討每個優點和缺點，並建議您將 Azure 監視器納入混合式監視策略中。  

#### <a name="disadvantages-of-using-operations-manager-by-itself"></a>使用 Operations Manager 本身的缺點

- 使用從主控台存取的管理元件所提供的預先定義的視圖（從 SQL Server Reporting Services (SSRS) 報表，或從使用者建立的自訂視圖），通常會執行 Operations Manager 中的監視資料分析。 臨機運算元據分析並非現成可用。 Operations Manager 報告沒有彈性。 提供監視資料長期保留的資料倉儲無法調整或執行效能不佳。 撰寫 T-sql 語句、開發 Power BI 解決方案或使用協力廠商解決方案的專業知識，必須支援 IT 組織中各種不同角色的需求。

- Operations Manager 中的警示不支援複雜運算式或包含相互關聯邏輯。 為了協助減少雜訊，會將警示分組以顯示它們之間的關聯性，並識別其原因。

#### <a name="advantages-of-using-operations-manager-with-azure-monitor"></a>使用 Operations Manager 搭配 Azure 監視器的優點

- Azure 監視器是解決 Operations Manager 限制的方法。 它會收集重要的效能和記錄資料，以補充 Operations Manager 的資料倉儲資料庫。 Azure 監視器可提供更佳的分析、在查詢大型資料磁片區) 時的效能 (，以及比 Operations Manager 資料倉儲的保留。

  使用 Azure 監視器記錄查詢語言，您可以建立更複雜且更複雜的查詢。 您可以在數秒內跨數 tb 的資料執行查詢。 您可以快速地將資料轉換成圓形圖、時間圖表以及許多其他視覺效果。 若要分析此資料，您不再受限於使用以 SQL Server Reporting Services、自訂 SQL 查詢或其他因應措施為基礎的 Operations Manager 報表。

- 您可以藉由實施 Azure 監視器警示管理解決方案，來提供改良的警示體驗。 Operations Manager 管理群組中產生的警示可轉送至 Azure 監視器 Logs Analytics 工作區。 您可以設定負責將警示從 Operations Manager 轉送到 Azure 監視器記錄的訂用帳戶，以轉寄特定的警示。 例如，您可以只轉寄符合您的準則的警示，以支援問題管理的趨勢，以及透過單一窗格來調查失敗或問題的根本原因。 此外，您可以將其他記錄資料與 Application Insights 或其他來源相互關聯，以深入瞭解可協助改善使用者體驗、增加執行時間，並縮短解決事件的時間。

- 您可以使用 Azure 監視器，從 Azure 中的簡單或多層式架構監視雲端原生基礎結構和應用程式，也可以使用 Operations Manager 來監視內部部署基礎結構。 此監視包括一或多個 Vm、放置於可用性設定組或虛擬機器擴展集中的多個 Vm，或部署到在 Windows Server 或 Linux 容器上執行的 Azure Kubernetes Service (AKS) 的容器化應用程式。

    如果您需要全面監視在 Azure Vm 上執行的 Microsoft 或協力廠商工作負載，而且您有無法單獨根據記錄檔或效能資料來評估的 advanced 案例，請使用 System Center Operations Manager。 其管理元件可提供先進的邏輯，包括服務和健康狀態模型，以判斷工作負載的操作健全狀況。

- 藉由使用適用於 VM 的 Azure 監視器的對應功能，您可以監視 Azure Vm 和內部部署 Vm 之間的網路連線計量標準連線計量。 這些計量包括回應時間、每分鐘的要求數、流量輸送量和連結。 您可以識別失敗的連接、進行疑難排解、執行遷移驗證、執行安全性分析，以及驗證服務的整體架構。 Map 可以自動探索 Windows 和 Linux 系統上的應用程式元件，並對應服務之間的通訊。 這項自動化可協助您找出您不知道的連線和相依性、規劃並驗證 Azure 的遷移，並將事件解決期間的推測降至最低。

- 藉由使用網路效能監控，您可以監視下列各項之間的網路連線能力：
  - 您的公司網路和 Azure。
  - 任務關鍵性多層式應用程式和微服務。
  -  (HTTP/HTTPS) 的使用者位置和 web 架構應用程式。

    此策略會提供網路層的可見度，而不需要 SNMP。 它也可以出現在互動式拓撲圖中，也就是來源和目的地端點之間路由的逐一躍點拓撲。 比起嘗試使用 Operations Manager 中的網路監視，或在您的環境中目前使用的其他網路監視工具來達成相同結果，這是較好的選擇。

### <a name="monitor-with-azure-monitor"></a>使用 Azure 監視器進行監視

雖然遷移至雲端帶來了許多挑戰，但它也提供了機會。 它可讓您的組織從一或多個內部部署企業監視工具遷移，不僅可能降低資本支出和營運成本，還能從雲端監視平臺（例如 Azure 監視器可在雲端規模提供的優點）中獲益。 檢查您的監視和警示需求、現有監視工具的設定，以及轉換至雲端的工作負載。 當您的方案完成之後，請設定 Azure 監視器。

- 從 Azure、其他雲端提供者和您的公司網路之間託管元件的簡單或多層式架構，監視混合式基礎結構和應用程式。 這些元件可能包括一或多個 Vm、放置在可用性設定組或虛擬機器擴展集中的多個 Vm，或部署到在 Windows Server 或 Linux 容器上執行的 Azure Kubernetes Service (AKS) 的容器化應用程式。

- 啟用容器適用於 VM 的 Azure 監視器、Azure 監視器，以及 Application Insights 偵測和診斷基礎結構和應用程式之間的問題。 若要對從多個元件或支援應用程式之相依性收集的資料進行更徹底的分析和相互關聯，您必須使用 Azure 監視器記錄。

- 建立適用于一組核心應用程式和服務元件的智慧型警示、使用複雜信號的動態閾值來協助減少警示雜訊，以及使用以機器學習演算法為基礎的警示匯總，協助您快速找出問題。

- 定義查詢和儀表板的程式庫，以支援 IT 組織中各種不同角色的需求。

- 定義在混合式和雲端資源之間啟用監視的標準和方法、每個資源的監視基準、警示閾值等等。  

- 設定 (RBAC) 的角色型存取控制，因此您只授與使用者和群組監視所管理資源的資料所需的存取權。

- 包含自動化和自助服務，讓每個小組都能視需要建立、啟用和調整其監視和警示設定。

## <a name="private-cloud-monitoring"></a>私用雲端監視

您可以使用 System Center Operations Manager 來達成 Azure Stack 的整體監視。 具體而言，您可以監視在租使用者中執行的工作負載、資源層級、虛擬機器上執行的工作負載，以及裝載 Azure Stack (實體伺服器和網路交換器) 的基礎結構。

您也可以使用 Azure Stack 中包含的 [基礎結構監視功能](/azure/azure-stack/azure-stack-monitor-health) 組合來進行全面監視。 這些功能可協助您查看 Azure stack 區域的健康情況和警示，以及 Azure Stack 中的 [Azure 監視器服務](/azure/azure-stack/user/azure-stack-metrics-azure-data) ，以提供適用于大部分服務的基本層級基礎結構計量和記錄。

如果您已投資 Operations Manager，請使用 Azure Stack 管理元件來監視 Azure Stack 部署的可用性和健康情況狀態，包括區域、資源提供者、更新、更新執行、縮放單位、單位節點、基礎結構角色，以及其 (由硬體資源) 組成的邏輯實體。 此管理元件會使用健康情況和更新資源提供者 REST Api 與 Azure Stack 進行通訊。 若要監視實體伺服器和存放裝置，請使用 OEM 廠商的管理元件 (例如，由聯想、Hewlett Packard 或 Dell) 提供。 Operations Manager 可以透過使用 SNMP，以原生方式監視網路交換器以收集基本統計資料。 您可以透過下列兩個基本步驟，使用 Azure 管理元件來監視租使用者工作負載。 設定您想要監視的訂用帳戶，然後新增該訂用帳戶的監視器。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [收集正確的資料](./data-collection.md)
