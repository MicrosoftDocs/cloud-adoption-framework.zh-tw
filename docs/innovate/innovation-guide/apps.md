---
title: Azure 創新：透過應用程式參與
description: 了解 Azure 服務如何協助您輕鬆地將現有 Web 和 API 應用程式現代化，並建立雲端原生應用程式。
author: billyclaymyersmsft
ms.author: wimyers
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.custom: fasttrack-edit, AQC
ms.localizationpriority: high
ms.openlocfilehash: 1ea314a40af6ce271a3563773acce783ac956201
ms.sourcegitcommit: 08d6d5bda45814745fc181b0a07bcb8c415bf342
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/14/2020
ms.locfileid: "86373300"
---
<!-- cSpell:ignore billyclaymyersmsft wimyers functionapp -->

# <a name="engage-customers-through-apps"></a>透過應用程式與客戶互動

透過應用程式來創新包含兩個步驟，分別是將裝載於內部部署環境的現有應用程式現代化，以及使用容器或無伺服器技術建置雲端原生應用程式。 Azure 提供了 PaaS 服務 (例如，Azure App Service)，協助您輕鬆地將以 .NET、.NET Core、Java、Node.js、Ruby、Python 或 PHP 撰寫的現有 Web 和 API 應用程式現代化，以便部署到 Azure 中。

透過擁有開放標準的容器模型，只要使用 Azure Kubernetes Service、Azure 容器執行個體和用於容器的 Web App 等受控服務，就能很容易地建置微服務或將現有應用程式容器化，並將這些項目部署到 Azure 上。 無伺服器技術 (例如 Azure Functions 和 Azure Logic Apps) 會利用使用量模型 (用多少付多少) 協助您專注於建置應用程式，而不必部署和管理基礎結構。

<!-- markdownlint-disable MD025 -->

## <a name="deliver-value-faster"></a>[更快地帶來價值](#tab/DeliverValueFaster)

雲端式解決方案的優點之一，就是能夠更快地收集意見反應，並開始為使用者帶來價值。 無論該使用者是外部客戶還是貴公司內的使用者，獲得關於應用程式意見反應的速度都是越快越好。

### <a name="azure-app-service"></a>Azure App Service

Azure App Service 會為應用程式提供裝載環境，讓您不必承擔管理基礎結構和修補作業系統的重任。 提供此環境是為了能夠自動調整規模以符合使用者的需求，同時又不會超過您所定義的限制以防止成本失控。

Azure App Service 可為 ASP.NET、ASP.NET Core、Java、Ruby、Node.js、PHP 和 Python 等語言提供最頂級的支援。 如果您需要裝載其他執行階段堆疊，用於容器的 Web App 可讓您輕鬆快速地在 App Service 中裝載 Docker 容器，藉此將您的自訂程式碼堆疊裝載於某個可讓您甩開伺服器事務的環境中。

#### <a name="action"></a>動作

若要設定或監視 Azure App Service 部署：

1. 移至 [應用程式服務]。
2. 設定新的服務：選取 [新增] 並依照提示執行。
3. 管理現有服務：從裝載的應用程式清單中選取所需應用程式。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Web%2FSites]" submitText="Go to App Services" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

### <a name="azure-cognitive-services"></a>Azure 認知服務

您可以利用 Azure 認知服務，透過一組 API 直接將進階智慧注入到您的應用程式中，從而讓您利用 Microsoft 支援的 AI 和 Machine Learning 演算法。

<!-- markdownlint-disable MD024 -->

#### <a name="action"></a>動作

若要設定或監視 Azure 認知服務部署：

1. 移至 [認知服務]。
2. 設定新的服務：選取 [新增] 並依照提示執行。
3. 管理現有服務：從裝載的服務清單中選取所需服務。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.CognitiveServices%2FAccounts]" submitText="Go to Cognitive Services" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

### <a name="azure-bot-service"></a>Azure Bot 服務

Azure Bot Service 可新增自然的 Bot 介面，該介面使用 AI 和 Machine Learning 來為客戶建立新的互動，進而擴充標準應用程式。

#### <a name="action"></a>動作

若要設定或監視 Azure Bot Service 部署：

1. 移至 [Bot Service]。
2. 設定新的服務：選取 [新增] 並依照提示執行。
3. 管理現有服務：從裝載的服務清單中選取所需 Bot。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.BotService%2FBotServices]" submitText="Go to Bot Services" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

### <a name="azure-devops"></a>Azure DevOps

在您的創新旅途中，您最後會找到自己通往 DevOps 的道路。 Microsoft 早已擁有稱為 Team Foundation Server (TFS) 的內部部署產品。 在我們自己的創新旅途中，Microsoft 開發了 Azure DevOps，這是一項雲端式服務，可提供建置和發行工具來為您的發行支援許多語言和目的地。 如需詳細資訊，請參閱 [Azure DevOps](https://docs.microsoft.com/azure/devops)。

### <a name="visual-studio-app-center"></a>Visual Studio App Center

隨著行動應用程式日益普及，人們也越來越需要有一種平台可讓其在具有各種設定的實際裝置上進行自動化測試。 Visual Studio App Center 不僅提供了這樣的地方，讓您可以在 iOS、Android、Windows 和 macOS 上測試應用程式，還提供監視平台，讓您得以輕鬆快速地運用 Azure Application Insights 來分析遙測資料。 如需詳細資訊，請參閱 [Visual Studio App Center](https://docs.microsoft.com/appcenter)。

Visual Studio App Center 也會提供通知服務，讓您不必個別連絡每個通知服務，就能使用單一呼叫來對您在各平台的應用程式傳送通知。 如需詳細資訊，請參閱 [Visual Studio App Center Push (ACP)](https://docs.microsoft.com/appcenter/push)。

### <a name="learn-more"></a>深入了解

- [App Service 概觀](https://docs.microsoft.com/azure/app-service/overview)
- [用於容器的 Web App：執行自訂容器](https://docs.microsoft.com/azure/app-service/containers/quickstart-docker)
- [Azure Functions 簡介](https://docs.microsoft.com/azure/azure-functions/functions-overview)
- [適用於 .NET 和 .NET Core 開發人員的 Azure](https://docs.microsoft.com/dotnet/azure/?view=azure-dotnet)
- [Azure SDK for Python 文件](https://docs.microsoft.com/azure/python)
- [適用於 Java 雲端開發人員的 Azure](https://docs.microsoft.com/azure/java/?view=azure-java-stable)
- [在 Azure 中建立 PHP Web 應用程式](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-php)
- [Azure SDK for JavaScript 文件](https://docs.microsoft.com/azure/javascript)
- [Azure SDK for Go 文件](https://docs.microsoft.com/azure/go)
- [DevOps 解決方案](https://azure.microsoft.com/solutions/devops)

## <a name="create-cloud-native-apps"></a>[建立雲端原生的應用程式](#tab/CloudNative)

### <a name="what-are-cloud-native-applications"></a>什麼是雲端原生應用程式？

雲端原生應用程式專為雲端規模與效能最佳化全新打造。 其會根據微服務架構進行鬆散結合、使用受控服務、可供觀察，並利用持續傳遞來獲得可靠性及加快上市時間。 通常，其具有可攜性，並可在公用、私人和混合式雲端等動態環境上執行。 雲端原生應用程式通常透過下列一或多種方法來建置：

- 微服務
- 無伺服器
- 容器

### <a name="microservices"></a>微服務

微服務是一種軟體架構風格，應用程式由小型獨立模組所組成，並透過定義良好的 API 協定互相通訊。 這些服務模組是高度分離的建置組塊，小到足以實作單一功能。 微服務可協助您：

- 獨立建置服務。
- 自發調整服務。
- 為部署和程式設計語言使用最適合的方法。
- 隔離失敗點。
- 更快地帶來價值。

### <a name="microservices-azure-kubernetes-service-aks"></a>微服務：Azure Kubernetes Service (AKS)

使用完全受控的 Kubernetes 服務，視需求處理叢集資源的佈建、升級和調整。 AKS 可讓您輕鬆地部署和管理容器化應用程式。 其提供無伺服器 Kubernetes、整合的持續整合與持續傳遞 (CI/CD) 體驗，以及企業級的安全性與治理。 在單一平台集結您的開發與營運團隊，好整以暇地快速建置、提供及調整應用程式。

#### <a name="action"></a>動作

若要設定或監視 AKS 服務：

1. 移至 **Azure Kubernetes Service**。
2. 設定新的服務：選取 [新增] 並依照提示執行。
3. 管理現有服務：從清單中選取所需的 Kubernetes Service。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResource/resourceType/Microsoft.ContainerService%2FManagedClusters]" submitText="Go to Azure Kubernetes Services" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

### <a name="serverless-solutions"></a>無伺服器解決方案

使用完全受控平台建置雲端原生應用程式，而不需要佈建及管理基礎結構，因為系統會為您處理規模調整、可用性和效能相關作業。 Azure 無伺服器解決方案的優點包括：

- 提升開發人員速度。
- 提升小組效能。
- 提升組織影響力。

### <a name="serverless-solutions-azure-functions"></a>無伺服器解決方案：Azure Functions

Azure Functions 會提供平台供您在雲端中執行小段程式碼或函式。 函式可讓您開始將程式碼重構成微服務架構。

Azure Functions 執行階段支援多種語言，包括 C#、Java、JavaScript 和 Python。 如需完整清單，請參閱 [Azure Functions 中支援的語言](https://docs.microsoft.com/azure/azure-functions/supported-languages)。

函式的另一個優點是能由不同的動作和事件加以觸發，例如 HTTPTriggers、TimerTriggers，以及來自其他 Azure 服務 (像是 Blob 儲存體、EventGrid 和 ServiceBus) 的觸發程序。 如需觸發程序和繫結的詳細資訊，請參閱 [Azure Functions 觸發程序和繫結概念](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings)。

#### <a name="action"></a>動作

若要設定或監視 Azure Functions 部署：

1. 移至 [函式應用程式]。
2. 設定新的函數應用程式：選取 [新增] 並依照提示執行。
3. 管理現有的函數應用程式：從函式應用程式清單中選取所需的應用程式。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Web%2FSites/kind/functionapp]" submitText="Go to Azure Functions" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

### <a name="serverless-solutions-azure-logic-apps"></a>無伺服器解決方案：Azure Logic Apps

整合資料和應用程式，而不是在不同的系統之間編寫複雜的整合程式碼。 使用 Azure Logic Apps 以視覺化方式建立無伺服器工作流程，並使用您自己的 API、無伺服器功能或現成可用的軟體即服務 (SaaS) 連接器，包括 Salesforce、Microsoft Office 365 和 Dropbox。

#### <a name="action"></a>動作

若要設定或監視 Azure 邏輯應用程式：

1. 移至 [Logic Apps]。
2. 設定新的邏輯應用程式：選取 [新增] 並依照提示執行。
3. 管理現有的邏輯應用程式：從清單中選取所需的邏輯應用程式。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Logic%2FWorkflows]" submitText="Go to Azure Logic Apps" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

### <a name="serverless-solutions-api-management"></a>無伺服器解決方案：API 管理

Azure API 管理是完全受控的服務，其使用量模型的設計與實作方式完全適用於無伺服器應用程式，可用於發佈、保護、轉換、維護並監視 API。

#### <a name="action"></a>動作

若要設定或監視 API 管理服務：

1. 移至 [API 管理服務]。
2. 設定新的服務：選取 [新增] 並依照提示執行。
3. 管理現有服務：從清單中選取所需的服務。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.ApiManagement%2FService]" submitText="Go to API Management services" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

### <a name="containers"></a>容器

針對將應用程式組合現代化，Azure 提供了各種容器服務，可將現有應用程式遷移至容器，以及建置雲端原生微服務應用程式，讓您更快地為使用者帶來價值。 使用端對端開發人員和 CI/CD 工具來開發、更新和部署容器化的應用程式。 使用與 Azure Active Directory 整合的完全受控 Kubernetes 容器協調流程服務，大規模地管理容器。 無論您處在應用程式現代化旅途的哪一站，都可以加快容器化應用程式的開發腳步，同時滿足安全性需求。

### <a name="containers-azure-container-instances"></a>容器：Azure Container Instances

在受控、無伺服器的 Azure 環境中，視需要執行 Docker 容器。 Azure 容器執行個體是適用於任何可在隔離容器中運作的案例解決方案，而不需要協調流程。 在 Container Instances 中執行您的工作負載時，您可以專注於應用程式的設計與建置，而不必費心管理執行應用程式的基礎結構。

#### <a name="action"></a>動作

若要設定或監視容器執行個體：

1. 移至 [容器執行個體]。
2. 設定新的容器執行個體：選取 [新增] 並依照提示執行。
3. 管理現有的容器執行個體：從清單中選取所需的容器執行個體。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.ContainerInstance%2FContainerGroups]" submitText="Go to Container instances" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

### <a name="containers-azure-red-hat-openshift"></a>容器：Azure Red Hat OpenShift

Azure Red Hat OpenShift 提供靈活、自助的完全受控 OpenShift 叢集部署。 保持遵循法規，並專注於您的應用程式開發，將主機、基礎結構和應用程式節點的修補、更新及監視作業交給 Microsoft 和 Red Hat。 您可選擇自己的登錄、網路、儲存體或 CI/CD 解決方案。 或者透過自動化原始程式碼管理、容器和應用程式建置、部署、調整、健康狀態管理等服務，使用內建解決方案來迅速開始使用。

#### <a name="learn-more"></a>深入了解

- [Azure Red Hat OpenShift](https://docs.microsoft.com/azure/openshift/intro-openshift)

## <a name="isolate-points-of-failure"></a>[隔離失敗點](#tab/IsolatePointsOfFailure)

當您開始從初始測試階段進行轉換時，請評估用來隔離和移除失敗點的方法。 由於 Azure 雲端平台具有分散本質，您可以將應用程式設計為既能讓失敗降到最低，又能改善效能。

### <a name="azure-front-door"></a>Azure Front Door

Azure Front Door 提供了可擴充且安全的進入點，讓您可以在全球各地遞送應用程式。 Azure Front Door 結合了流量最佳化功能，以便提供最佳效能和立即的全域容錯移轉。 如果您需要傳輸層安全性 (TLS) 通訊協定終止 (SSL 卸載) 或每一 HTTP/HTTPS 要求的應用程式層處理，您應該使用 Azure Front Door，而非 Azure 流量管理員。

#### <a name="action"></a>動作

若要設定或監視 Front Door：

1. 移至 [Front Door]。
2. 設定新的 Front Door：選取 [新增] 並依照提示執行。
3. 管理現有 Front Door：從清單中選取所需的 Front Door。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Network%2FFrontDoors]" submitText="Go to Front Doors" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

### <a name="traffic-manager"></a>流量管理員

流量管理員提供可根據各種規則來路由傳送的 DNS 型負載平衡。 這項功能有助於確保任何已部署的服務在失敗時能夠復原。 您也可以堆疊流量管理員，以同時使用失敗型路由和效能型路由，從而根據地理位置來提供最佳體驗。

#### <a name="action"></a>動作

若要設定或監視流量管理員設定檔：

1. 移至 [流量管理員設定檔]。
2. 設定新的設定檔：選取 [新增] 並依照提示執行。
3. 管理現有設定檔：從清單中選取所需的設定檔。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Network%2FTrafficManagerProfiles]" submitText="Go to Traffic Manager profiles" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

### <a name="azure-content-delivery-network"></a>Azure 內容傳遞網路

Azure 提供分散式內容傳遞網路 (CDN)，可讓您將資產快取到接近使用者的位置，以確保能及時傳遞資產。 此快取有助於改善客戶的體驗。 在內容下載期間，也可預防在 CDN 端點與裝載應用程式的資料中心之間發生的網路問題所造成的問題。 Azure CDN 也可供未裝載於 Azure 中的應用程式使用。

#### <a name="action"></a>動作

若要設定或監視 Azure CDN 設定檔：

1. 移至 [CDN 設定檔]。
2. 設定新的設定檔：選取 [新增] 並依照提示執行。
3. 管理現有設定檔：從清單中選取所需的設定檔。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Cdn%2FProfiles]" submitText="Go to CDN profiles" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

### <a name="learn-more"></a>深入了解

- [Azure Front Door](https://docs.microsoft.com/azure/frontdoor/front-door-overview)
- [流量管理員](https://docs.microsoft.com/azure/traffic-manager)
- [內容傳遞網路](https://docs.microsoft.com/azure/cdn)
