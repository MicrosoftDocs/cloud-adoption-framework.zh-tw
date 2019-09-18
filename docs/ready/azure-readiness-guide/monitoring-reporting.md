---
title: Azure 中的監視和報告
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 了解如何為您的 Azure 管理環境設定監視、報告和警示。
author: timleyden
ms.author: tileyden
ms.date: 04/09/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: fasttrack-edit, AQC
ms.localizationpriority: high
ms.openlocfilehash: dc2df86dae7e820527d5f4b1d26fa69366b55b59
ms.sourcegitcommit: 443c28f3afeedfbfe8b9980875a54afdbebd83a8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2019
ms.locfileid: "71022067"
---
# <a name="monitoring-and-reporting-in-azure"></a>Azure 中的監視和報告

Azure 提供許多服務，這些服務可共同提供一個全方位的解決方案，讓您從應用程式和支援這些服務的 Azure 資源收集遙測、加以分析並採取行動。 此外，這些服務可延伸至監視重要的內部部署資源，以提供混合式監視環境。

# <a name="azure-monitortabazuremonitor"></a>[Azure 監視器](#tab/AzureMonitor)

Azure 監視器針對 Azure 中的所有監視和診斷資料提供了單一的整合中樞。 您可以使用它來取得資源的可見度。 透過 Azure 監視器，您將可找出並修正問題，進而將效能最佳化。 您也可以了解客戶的行為。

- **監視計量並以視覺化方式呈現。** 計量是可從 Azure 資源取得的數值，可協助您了解系統的健康情況。 請自訂儀表板的圖表並使用活頁簿來提供報告。

- **查詢和分析記錄。** 記錄包含來自 Azure 的活動記錄和診斷記錄。 從適用於雲端或內部部署資源的其他監視和管理解決方案收集其他記錄。 Log Analytics 可作為彙總所有資料的中央存放庫。 您可以從該處執行查詢來協助針對問題進行疑難排解，也可以將資料視覺化。

- **設定警示和動作。** 警示會主動通知您重大情況。 您可以根據來自計量、記錄或服務健康情況問題的觸發程序來採取矯正措施。 您可以設定不同的通知和動作，並將資料傳送至 IT 服務管理工具。

::: zone target="docs"

 開始監視：

- [應用程式](https://docs.microsoft.com/azure/application-insights/app-insights-overview)
- [容器](https://docs.microsoft.com/azure/monitoring/monitoring-container-overview)
- [虛擬機器](https://docs.microsoft.com/azure/monitoring/monitoring-service-map)
- [網路](https://docs.microsoft.com/azure/networking/network-monitoring-overview)

若要監視其他資源，請尋找 Azure Marketplace 中的其他解決方案。

若要探索 Azure 監視器，請移至 [Azure 入口網站](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/overview)。

## <a name="learn-more"></a>深入了解

若要深入了解，請參閱 [Azure 監視器文件](https://docs.microsoft.com/azure/monitoring-and-diagnostics)。

::: zone-end

::: zone target="chromeless"

## <a name="action"></a>動作

::: form action="OpenBlade[#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/overview]" submitText="Explore Azure Monitor" :::

::: zone-end

# <a name="azure-service-healthtabazureservicehealth"></a>[Azure 服務健康狀態](#tab/AzureServiceHealth)

Azure 服務健康狀態會提供您所用 Azure 服務和區域的健康情況個人化檢視。 作用中問題的相關資訊會發佈到「服務健康狀態」，協助您了解對您資源的影響。 定期更新可讓您立即掌握問題已解決的資訊。

我們也會將計劃性維護事件發佈至「服務健康狀態」，讓您能夠知道可能影響資源可用性的變更。 設定「服務健康狀態」警示，以便獲知可能影響您所用 Azure 服務和區域的服務問題、計劃性維護或其他變更。

Azure 服務健康狀態包含：

- **Azure 狀態：** Azure 服務健康狀態的全域檢視。
- **服務健康狀態：** Azure 服務健康狀態的個人化檢視。
- **資源健康狀態：** 每個個別資源健康狀態的更深入檢視。

::: zone target="chromeless"

<!-- markdownlint-disable MD024 -->

## <a name="action"></a>動作

若要設定服務健康狀態警示：

1. 請移至 [服務健康狀態]  。
2. 選取 [健康狀態警示]  。
3. 建立服務健康狀態警示。

::: form action="OpenBlade[#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/healthalerts]" submitText="Go to Service Health" :::

::: zone-end

::: zone target="docs"

若要設定服務健康狀態警示，請移至 [Azure 入口網站](https://portal.azure.com/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/healthalerts)。

## <a name="learn-more"></a>深入了解

若要深入了解，請參閱 [Azure 服務健康狀態文件](https://docs.microsoft.com/azure/service-health)。

::: zone-end

# <a name="azure-advisortabazureadvisor"></a>[Azure Advisor](#tab/AzureAdvisor)

Azure Advisor 是免費的個人化雲端顧問，可協助您遵循及實作 Azure 部署的最佳做法。 它會分析資源組態和使用量遙測，然後建議有助於將環境最佳化的解決方案。 建議分為四個類別：

- **高可用性：** 可改善商務關鍵應用程式的持續性。 建議做法可能包括將虛擬機器新增至可用性設定組，或新增異地備援的端點。
- **安全性：** 可偵測可能導致安全性漏洞的威脅和弱點。 建議做法可能包括套用磁碟加密，或啟用網路安全性群組。
- **效能：** 可提升應用程式的速度。 建議做法可能包括藉由建立索引或重新設定流量管理員設定來提升 SQL 查詢效能。
- **成本：** 可最佳化並降低整體 Azure 費用。 建議做法可能包括將未充分使用的虛擬機器調整大小或關閉，或改用 Azure 保留項目以降低擁有權總成本。

Advisor 會依據您所部署的資源以及您在 Azure 中採取的動作來提供建議。 您可以定期查看 Advisor 以獲得最新建議。

::: zone target="chromeless"

## <a name="action"></a>動作

::: form action="OpenBlade[#blade/Microsoft_Azure_Expert/AdvisorBlade]" submitText="Explore Azure Advisor" :::

::: zone-end

::: zone target="docs"

若要探索 Azure Advisor，請移至 [Azure 入口網站](https://portal.azure.com/#blade/Microsoft_Azure_Expert/AdvisorBlade)。

## <a name="learn-more"></a>深入了解

若要深入了解，請參閱 [Azure Advisor 文件](https://docs.microsoft.com/azure/advisor)。

::: zone-end

# <a name="azure-security-centertabazuresecuritycenter"></a>[Azure 資訊安全中心](#tab/AzureSecurityCenter)

Azure 資訊安全中心也會在監視策略中扮演重要角色。 它可協助您監視機器、網路、儲存體、資料服務和應用程式的安全性。 資訊安全中心可藉由使用機器學習和行為分析來協助您識別目標是 Azure 資源的作用中威脅，從而提供先進的威脅偵測。 其也提供威脅防護，可封鎖惡意程式碼或其他不必要的程式碼，並可減少暴露在暴力密碼破解攻擊和其他網路攻擊之下的介面區。

資訊安全中心在識別到威脅時，會觸發附有回應攻擊所需採取步驟的安全性警示。 它也會提供附有所偵測到威脅相關資訊的報告。

Azure 資訊安全中心提供兩個層級：免費層和標準層。 安全性建議等功能可免費使用。 標準層則會提供額外的保護，例如針對混合式雲端工作負載的進階威脅偵測和防護。

::: zone target="chromeless"

## <a name="action"></a>動作

**前 30 天可免費使用標準服務層級。**

為訂用帳戶資源開啟及設定安全性原則後，便可在 [防護]  區段中檢視資源的安全性狀態及任何問題。 您也可以在 [建議]  圖格上檢視這些問題的清單。

::: form action="OpenBlade[#blade/Microsoft_Azure_Security/SecurityMenuBlade/SecurityMenuBlade/0]" submitText="Explore Azure Security Center" :::

::: zone-end

::: zone target="docs"

若要探索 Azure 資訊安全中心，請移至 [Azure 入口網站](https://portal.azure.com/#blade/Microsoft_Azure_Security/SecurityMenuBlade/SecurityMenuBlade/0)。

## <a name="learn-more"></a>深入了解

若要深入了解，請參閱 [Azure 資訊安全中心文件](https://docs.microsoft.com/azure/security-center)。

::: zone-end
