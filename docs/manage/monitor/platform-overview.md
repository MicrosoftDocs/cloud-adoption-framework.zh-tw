---
title: 雲端監視平臺總覽
description: 深入瞭解兩個監視平臺，協助您瞭解每個平臺如何提供核心監視功能。
author: mgoedtel
ms.author: magoedte
ms.date: 07/31/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 228601766d4c580b73df2e78540833c05ff00924
ms.sourcegitcommit: a7eb2f6c4465527cca2d479edbfc9d93d1e44bf1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94713542"
---
<!-- cSpell:ignore opsman ITSM -->

# <a name="cloud-monitoring-guide-monitoring-platforms-overview"></a>雲端監視指南：監視平臺總覽

Microsoft 提供一系列來自兩種產品的監視功能： System Center Operations Manager，其是針對內部部署所設計，然後擴充至雲端，而 Azure 監視器則是針對雲端所設計，但也可以監視內部部署系統。 這兩個供應專案提供核心監視服務，例如警示、服務執行時間追蹤、應用程式和基礎結構健康情況監視、診斷和分析。

許多組織都採用最新的做法來 DevOps 靈活性和雲端創新，以管理其異類環境。 此外，他們也在意他們能夠做出適當且負責的決策來監視這些工作負載。

本文提供監視平臺的概要說明，協助您瞭解每個平臺如何提供核心監視功能。

## <a name="the-story-of-system-center-operations-manager"></a>System Center Operations Manager 的故事

在2000中，我們輸入了 Microsoft Operations Manager 2000 的 operations management 欄位。 在2007中，我們引進了經過重新設計版本的產品，System Center Operations Manager。 它超越簡單的 Windows server 監視，並著重于強大的端對端服務和應用程式監視，包括不同的平臺、網路裝置，以及其他應用程式或服務相依性。 它是針對內部部署環境所建立的企業級監視平臺，位於與 IBM Tivoli 或 HP Operations Manager 產業相同的類別中。 它已成長為支援監視在 Azure 中執行的計算和平臺資源、Amazon Web Services (AWS) 和其他雲端提供者。

## <a name="the-story-of-azure-monitor"></a>Azure 監視器的故事

當 Azure 在2010中發行時，會提供 Azure 診斷代理程式的監視雲端服務，以提供從 Azure 資源收集診斷資料的方式。 這項功能被視為一般監視工具，而不是企業級監視平臺。

Application Insights 導入了業界的變更，其中雲端、行動裝置和 IoT 裝置的激增成長，以及 DevOps 實務的引進。 它會從 Operations Manager 的應用程式效能監視成長到 Azure 中的服務，並提供豐富的監視功能，讓您以各種語言撰寫的 web 應用程式。 在2015中，Visual Studio 的 Application Insights 預覽已宣佈，之後又稱為 Application Insights。 它會收集應用程式效能、要求和例外狀況，以及追蹤的詳細資料。

在2015中，Azure Operational Insights 已公開推出。 它已提供 Log Analytics 分析服務，該服務會從 Azure、內部部署或其他雲端環境中的電腦收集和搜尋資料，並連接到 System Center Operations Manager。 提供的智慧套件提供各種預先封裝的管理和監視設定，這些設定包含在安全性審核、健全狀況評定和警示管理等案例中，包含一組查詢和分析邏輯、視覺效果和資料收集規則。 之後，Azure Operational Insights 即成為 Log Analytics。

在2016中，已在 Microsoft Ignite 會議宣佈 Azure 監視器的預覽。 它提供了一個通用架構，可從任何使用架構開始的 Azure 服務收集平臺計量、資源診斷記錄，以及訂用帳戶層級的活動記錄事件。 先前，每個 Azure 服務都有自己的監視方法。

在 2018 Ignite 會議中，我們宣佈了擴充的 Azure 監視器品牌，包括原本以獨立功能開發的數個不同服務：

- 原始 **Azure 監視器**，用於收集僅適用于 Azure 平臺資源的平臺計量、資源診斷記錄和活動記錄。
- **Application Insights**，用於監視應用程式。
- **Log Analytics** 是收集和分析記錄資料的主要位置。
- 新的 **整合警示服務**，其結合了先前提及的每個其他服務的警示機制。

- **Azure 網路** 監看員，可針對虛擬網路中的資源進行監視、診斷和查看計量。

## <a name="the-story-of-operations-management-suite-oms"></a>Operations Management Suite (OMS) 的案例

從2015到4月2018日為止，Operations Management Suite (OMS) 是下列 Azure 管理服務的配套，以供授權之用：

- Application Insights
- Azure 自動化
- Azure 備份
- Operational Insights (稍後更名為 Log Analytics) 
- Site Recovery

Oms 已停止時，屬於 OMS 的服務功能不會變更。 它們是在 Azure 監視器下重新編制。

## <a name="infrastructure-requirements"></a>基礎結構需求

### <a name="operations-manager"></a>Operations Manager

Operations Manager 需要大量的基礎結構和維護，才能支援管理群組，這是基本的功能單位。 管理群組至少包含一部或多部管理伺服器、SQL Server 實例、裝載操作和報表資料倉儲資料庫，以及代理程式。 管理群組設計的複雜性取決於多個因素，例如要監視的工作負載範圍，以及支援工作負載的裝置或電腦數目。 如果您需要高可用性和網站復原功能（通常是企業監視平臺的情況），則基礎結構需求和相關聯的維護可能會大幅增加。

![Operations Manager 管理群組的圖表](./media/monitoring-management-guidance-cloud-and-on-premises/operations-manager-management-group-optimized.svg)

### <a name="azure-monitor"></a>Azure 監視器

Azure 監視器是一種軟體即服務 (SaaS) 供應專案，因此其支援的基礎結構會在 Azure 中執行，並由 Microsoft 管理。 它會大規模地執行監視、分析和診斷。 它適用于所有國家雲端。 基礎結構 (收集器、計量和記錄存放區，以及支援 Azure 監視器的分析) 的核心部分會由 Microsoft 維護。

![Azure 監視器的圖表](./media/monitoring-management-guidance-cloud-and-on-premises/azure-monitor-greyed-optimized.svg)

## <a name="data-collection"></a>資料集合

<!-- markdownlint-disable MD024 -->

### <a name="operations-manager"></a>Operations Manager

#### <a name="agents"></a>代理程式

Operations Manager 直接從安裝在 [Windows 電腦](/system-center/scom/plan-planning-agent-deployment?view=sc-om-1807#windows-agent)上的代理程式收集資料。 它可以接受來自 Operations Manager SDK 的資料，但這種方法通常用於以自訂應用程式擴充產品的合作夥伴，而不是用來收集監視資料。 它可以從遠端存取這些其他裝置的 Windows 代理程式上執行的特殊模組，收集其他來源（例如 [Linux 電腦](/system-center/scom/plan-planning-agent-deployment?view=sc-om-1807#linuxunix-agent) 和網路裝置）的資料。

![Operations Manager 代理程式的圖表](./media/monitoring-management-guidance-cloud-and-on-premises/data-collection-opsman-agents-optimized.svg)

Operations Manager 代理程式可以從本機電腦上的多個資料來源收集，例如事件記錄檔、自訂記錄檔和效能計數器。 它也可以執行腳本，以便從本機電腦或從外部來源收集資料。 您可以撰寫自訂腳本來收集無法以其他方式收集的資料，或從無法監視的各種遠端裝置收集資料。

#### <a name="management-packs"></a>管理組件

Operations Manager 會使用工作流程 (規則、監視和物件探索) 來執行所有監視。 這些工作流程會一起封裝在 [管理元件](/system-center/scom/manage-overview-management-pack?view=sc-om-2019) 中，並部署至代理程式。 管理套件適用于各種不同的產品和服務，包括預先定義的規則和監視。 您也可以為自己的應用程式和自訂案例撰寫自己的管理元件。

#### <a name="monitoring-configuration"></a>監視設定

管理元件可包含上百個規則、監視器和物件探索規則。 代理程式會從所有適用的管理元件（由探索規則決定）執行所有的監視設定。 每個監視設定的每個實例都會獨立執行，並立即在它所收集的資料上運作。 這是 Operations Manager 可以達成近乎即時警示的方式，以及受監視資源目前的健康情況狀態。

例如，監視器可能會每隔幾分鐘取樣一個效能計數器。 如果該計數器超過閾值，則會立即設定其目標物件的健全狀況狀態，以立即觸發管理群組中的警示。 排定的規則可能會監看特定事件的建立，並在本機事件記錄檔中建立事件時立即引發警示。

因為這些監視設定會彼此隔離，並從個別的資料來源運作，Operations Manager 在多個來源之間有相關聯的資料挑戰。 收集資料之後也很難對資料進行回應。 您可以執行存取 Operations Manager 資料庫的工作流程，但這不是常見的情況，而且通常用於有限數量的特殊用途工作流程。

![Operations Manager 管理群組的圖表](./media/monitoring-management-guidance-cloud-and-on-premises/operations-manager-management-group-optimized.svg)

### <a name="azure-monitor"></a>Azure 監視器

#### <a name="data-sources"></a>資料來源

Azure 監視器會收集各種來源的資料，包括 Azure 基礎結構和平臺資源、Windows 和 Linux 電腦上的代理程式，以及監視 Azure 儲存體中收集的資料。 任何 REST 用戶端都可以使用 API 將記錄資料寫入 Azure 監視器，而且您可以定義 web 應用程式的自訂計量。 某些計量資料可以路由至不同的位置，視其使用方式而定。 例如，您可能會使用資料進行「快速」的警示，或使用長期趨勢分析搜尋與其他記錄資料。

#### <a name="monitoring-solutions-and-insights"></a>監視解決方案和見解

監視解決方案會使用 Azure 監視器中的記錄平臺，以提供特定應用程式或服務的監視。 它們通常會定義來自代理程式或 Azure 服務的資料收集，並提供記錄查詢和查看來分析該資料。 它們通常不會提供警示規則，這表示您必須根據收集到的資料定義自己的警示準則。

深入解析（例如容器和適用於 VM 的 Azure 監視器的 Azure 監視器）會使用 Azure 監視器的記錄和計量平臺，為 Azure 入口網站中的應用程式或服務提供自訂的監視體驗。 除了自訂收集的資料分析之外，它們可能還會提供健全狀況監視和警示狀況。

#### <a name="monitoring-configuration"></a>監視設定

Azure 監視器會將資料收集與針對該資料所採取的動作分開，以支援雲端環境中的分散式微服務。 它會將多個來源的資料合併成 common data platform，並根據所收集的資料提供分析、視覺效果和警示功能。

Azure 監視器所收集的資料會儲存為記錄檔或計量，而 Azure 監視器的不同功能則依賴其中一種方式。 計量包含時間序列中的數值，適用于近乎即時的警示和快速偵測問題。 記錄檔包含文字或數值資料，而且可以使用功能強大的語言進行查詢，特別適合用來執行複雜的分析。

因為 Azure 監視器會將資料收集與該資料的動作分開，所以在許多情況下可能無法提供近乎即時的警示。 若要對記錄資料發出警示，查詢會依照警示中定義的週期性排程執行。 此行為可讓 Azure 監視器輕鬆地將所有受監視來源的資料相互關聯，而且您可以透過各種方式以互動方式分析資料。 這特別有助於進行根本原因分析，並找出可能發生問題的位置。

## <a name="health-monitoring"></a>健康狀況監視

### <a name="operations-manager"></a>Operations Manager

Operations Manager 中的管理元件包含服務模型，可描述受監視應用程式的元件及其關聯性。 監視會根據代理程式上的資料和腳本，識別每個元件目前的健全狀況狀態。 健全狀況狀態會匯總，讓您可以快速地查看受監視電腦和應用程式的摘要健全狀況狀態。

### <a name="azure-monitor"></a>Azure 監視器

Azure 監視器不會提供可由使用者定義的方法來執行服務模型或監視，以指出任何服務元件目前的健全狀況狀態。 因為監視解決方案是以 Azure 監視器的標準功能為基礎，所以它們不會提供狀態層級監視。 Azure 監視器的下列功能很有説明：

- **Application Insights：** 建立 web 應用程式的複合對應，並提供每個應用程式元件或相依性的健全狀況狀態。 這包括警示狀態，並向下切入至更詳細的應用程式診斷。

- **適用於 VM 的 Azure 監視器：** 會在監視 Windows 和 Linux 虛擬機器時，提供來賓 Azure Vm 的健康情況監視體驗，類似于 Operations Manager。 它會從可用性和效能的觀點來評估重要作業系統元件的健全狀況，以判斷目前的健全狀況狀態。 當它判斷來賓 VM 遇到持續性的資源使用率、磁碟空間容量或與核心作業系統功能相關的問題時，它會產生警示，以將此狀態帶到您的注意力。

- **適用于容器的 Azure 監視器：** 監視 Azure Kubernetes Service 或 Azure 容器實例的效能和健康情況。 它可透過計量 API 從 Kubernetes 中提供的控制器、節點與容器收集記憶體與處理器計量。 它也會收集容器和其映射的容器記錄和清查資料。 根據所收集效能資料的預先定義健康情況準則，可協助您識別資源瓶頸或容量問題是否存在。 您也可以瞭解整體效能，或特定 Kubernetes 物件類型 (pod、節點、控制器或容器) 的效能。

## <a name="analyze-data"></a>分析資料

### <a name="operations-manager"></a>Operations Manager

Operations Manager 提供四種基本方式來分析收集的資料：

- **健全狀況總管：** 協助您找出哪些監視器正在識別健全狀況狀態問題，並瞭解監視的相關知識，以及其相關動作的可能原因。

- **Views：** 提供所收集資料的預先定義視覺效果，例如效能資料圖形或受監視元件的清單，以及其目前的健全狀態。 圖表視圖會以視覺化方式呈現應用程式的服務模型。

- **報表：** 可讓您摘要儲存在 Operations Manager 資料倉儲中的歷程記錄資料。 您可以自訂查看和報表所依據的資料。 但是，不允許對所收集資料進行複雜或互動式分析的功能。

- **Operations Manager 命令 Shell：** 使用一組額外的 Cmdlet 來擴充 Windows PowerShell，並且可以查詢和視覺化收集到的資料。 這包括圖形和其他視覺效果，以原生方式使用 PowerShell，或使用 Operations Manager HTML 式 web 主控台。

### <a name="azure-monitor"></a>Azure 監視器

使用強大的 Azure 監視器分析引擎，您可以互動方式處理記錄資料，並將它們與其他監視資料結合，以進行趨勢和其他資料分析。 視圖和儀表板可讓您以各種不同的方式將查詢資料視覺化 Azure 入口網站，並將其匯入 Power BI。 監視解決方案包含查詢和查看，以呈現其所收集的資料。 適用于容器的 Application Insights、適用於 VM 的 Azure 監視器和 Azure 監視器等見解包含自訂的視覺效果，可支援互動式監視案例。

## <a name="alerting"></a>警示

### <a name="operations-manager"></a>Operations Manager

當符合預先定義的事件、符合效能閾值，以及受監視元件的健全狀況狀態變更時，Operations Manager 會建立警示來回應預先定義的事件。 它包含警示的完整管理，可讓您設定解決方案，並將其指派給各種操作員或系統工程師。 您可以設定通知規則，以指定哪些警示將傳送主動式通知。

管理元件包含各種預先定義的警示規則，適用于受監視的應用程式中不同的重大條件。 您可以調整這些規則，或建立自訂規則以符合您環境的特定需求。

### <a name="azure-monitor"></a>Azure 監視器

使用 Azure 監視器時，您可以根據超過閾值的計量，或根據排程的查詢結果來建立警示。 雖然以計量為基礎的警示可以達到近乎即時的結果，但排定的查詢會有較長的回應時間，取決於資料內嵌和索引的速度。 Azure 監視器中的記錄查詢警示不受限於特定的代理程式，可讓您分析多個工作區中儲存之所有資料的資料。 這些警示也包含特定 Application Insights 應用程式的資料，方法是使用跨工作區查詢。

雖然監視解決方案可以包含警示規則，但您通常會根據自己的需求來建立警示規則。

## <a name="workflows"></a>工作流程

### <a name="operations-manager"></a>Operations Manager

Operations Manager 中的管理元件包含數百個個別工作流程，而且它們會決定要收集哪些資料，以及該資料要執行什麼動作。 例如，規則可能會每隔幾分鐘取樣一個效能計數器，並儲存其結果以進行分析。 監視器可能會取樣相同的效能計數器，並將其值與閾值進行比較，以判斷受監視物件的健全狀況狀態。 另一項規則可能會執行腳本來收集和分析代理程式電腦上的某些資料，然後在傳回特定值時引發警示。

Operations Manager 中的工作流程彼此獨立，因此難以跨多個受監視物件進行分析。 這些監視案例必須以收集之後的資料為基礎，但這是可行的，但可能會很困難，而且並不常見。

### <a name="azure-monitor"></a>Azure 監視器

Azure 監視器會將資料收集與從該資料取得的動作和分析分開。 代理程式和其他資料來源會將記錄資料寫入至 Log Analytics 工作區，並將計量資料寫入至計量資料庫，而不需要分析該資料或其使用方式的知識。 監視會從儲存的資料執行警示和其他動作，可讓您跨所有來源的資料執行分析。

## <a name="extend-the-base-platform"></a>擴充基底平臺

### <a name="operations-manager"></a>Operations Manager

Operations Manager 會在管理元件中執行所有監視邏輯，您可以自行建立或從我們或合作夥伴取得。 當您安裝管理元件時，它會自動在不同的代理程式上探索應用程式或服務的元件，並部署適當的規則和監視。 此管理元件包含健全狀況定義、警示規則、效能和事件集合規則，以及可提供支援基礎結構服務或應用程式的完整監視。

Operations Manager SDK 可讓 Operations Manager 整合協力廠商監視平臺或 IT 服務管理 (ITSM) 軟體。 某些夥伴管理元件也會使用 SDK 來支援監視網路裝置，並提供自訂的呈現體驗，例如方形 Up HTML5 儀表板或與 Microsoft Office Visio 的整合。

### <a name="azure-monitor"></a>Azure 監視器

Azure 監視器從 Azure 資源收集計量和記錄，幾乎不需要設定。 監視解決方案會新增監視應用程式或服務的邏輯，但它們仍可在監視的標準記錄查詢和流覽中運作。 深入解析（例如 Application Insights 和適用於 VM 的 Azure 監視器）使用監視器平臺進行資料收集和處理。 它們也提供其他工具來視覺化及分析資料。 您可以使用核心監視功能（例如記錄查詢和警示），結合見解收集的資料與其他資料。

監視器支援數種方法，可從 Azure 或外部資源收集監視或管理資料。 然後，您可以將資料從度量或記錄存放區解壓縮和轉送到 ITSM 或監視工具。 或者，您可以使用 Azure 監視器 REST API 來執行管理工作。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [監視雲端部署模型](./cloud-models-monitor-overview.md)
