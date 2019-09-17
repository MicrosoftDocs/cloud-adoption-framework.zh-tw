---
title: 雲端監視指南–雲端部署模型的監視策略
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 選擇何時使用 Azure 監視器或 System Center Operations Manager Microsoft Azure
author: MGoedtel
ms.author: magoedte
ms.date: 07/31/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: operate
services: azure-monitor
ms.openlocfilehash: 3659f5e965e5a80c3b490f8b106a4cc30f1711a9
ms.sourcegitcommit: 443c28f3afeedfbfe8b9980875a54afdbebd83a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2019
ms.locfileid: "71030708"
---
# <a name="cloud-monitoring-guide-monitoring-strategy-for-cloud-deployment-models"></a>雲端監視指南：雲端部署模型的監視策略

本文包含我們針對每個雲端部署模型所建議的監視策略 (根據下列準則):

- 您需要持續承諾 Operations Manager 或其他企業監視平臺。 這是因為與您的 IT 作業程式、知識和專業知識的整合, 或因為 Azure 監視器中尚未提供特定功能。
- 您必須在內部部署和公用雲端中, 或只監視雲端中的工作負載。
- 您的雲端遷移策略包括現代化 IT 作業, 並移至我們的雲端監視服務和解決方案。
- 您的重要系統可能是以空中或實體隔離、裝載于私人雲端或實體硬體上, 而且需要進行監視。

我們的策略包括監視基礎結構 (計算、儲存體和伺服器工作負載)、應用程式 (使用者、例外狀況和用戶端) 和網路資源的支援, 以提供完整的服務導向監視的觀點。

## <a name="azure-cloud-monitoring"></a>Azure 雲端監視

Azure 監視器是平台服務，提供監視 Azure 資源的單一來源。 其設計適用于建置於 Azure 上的雲端解決方案, 並支援以 VM 工作負載為基礎的商務功能, 或使用微服務和其他平臺資源的複雜架構。 它會監視堆疊的所有層, 從租使用者服務 (例如 Azure Active Directory Domain Services) 和訂用帳戶層級事件和 Azure 服務健康狀態開始。 它也會監視基礎結構資源, 例如 Vm、儲存體和網路資源, 以及最上層的應用程式。 監視每個相依性, 並收集每個相依性可發出的正確信號, 讓您可檢視性應用程式和所需的重要基礎結構。

下表摘要說明監視堆疊的每個層級的建議方法。

<!-- markdownlint-disable MD033 -->

圖層 | Resource | `Scope` | 方法
---|---|---|----
應用程式 | 在 .NET、.NET Core、JAVA、JavaScript 和 node.js 平臺上執行的 Web 應用程式, 位於 Azure VM、Azure App 服務、Azure Service Fabric、Azure Functions 和 Azure 雲端服務 | 監視即時 web 應用程式, 以自動偵測效能異常、找出程式碼例外狀況和問題, 以及收集可用性遙測。 |  Application Insights
容器 | Azure Kubernetes Service/Azure 容器實例 | 監視在容器和容器實例上執行之工作負載的容量、可用性和效能。 | 適用於容器的 Azure 監視器
客體作業系統 | Linux 和 Windows VM 作業系統 | 監視容量、可用性和效能。 對應每個 VM 上裝載的相依性, 包括伺服器之間的作用中網路連線的可見度、輸入和輸出連線延遲, 以及跨任何 TCP 連接架構的埠。 | 適用於 VM 的 Azure 監視器
Azure 資源-PaaS | Azure 資料庫服務 (例如 SQL 或 mySQL) | 適用于 SQL 效能計量的 Azure 資料庫。 | 啟用診斷記錄, 以將 SQL 資料串流至 Azure 監視器記錄。
Azure 資源-IaaS | 1.Azure 儲存體<br/> 2.Azure 應用程式閘道<br/> 3.Azure Key Vault<br/> 4.網路安全性群組<br/> 5.Azure 流量管理員 | 1.容量、可用性和效能。<br/> 2.效能和診斷記錄 (活動、存取、效能和防火牆)。<br/> 3.監視您的金鑰保存庫的存取方式和時間, 以及依據者。<br/> 4.監視套用規則時的事件, 以及規則套用到拒絕或允許的次數的規則計數器。<br/>5.監視端點狀態可用性。 | 1.Blob 儲存體的儲存體計量。<br/> 2.啟用診斷記錄, 並設定串流處理以 Azure 監視器記錄。<br/> 3.啟用診斷記錄，並設定串流處理以 Azure 監視器記錄，並啟用[Azure Key Vault 分析解決方案](https://docs.microsoft.com/azure/azure-monitor/insights/azure-key-vault)。 <br/> 4.啟用網路安全性群組的診斷記錄, 並設定串流以 Azure 監視器記錄。<br/> 5.啟用流量管理員端點的診斷記錄, 並設定串流處理以 Azure 監視器記錄。
網路| 您的虛擬機器與一或多個端點 (另一個 VM、完整功能變數名稱、統一資源識別項或 IPv4 位址) 之間的通訊。 | 監視在 VM 與端點之間發生的可連線性、延遲和網路拓撲變更。 | Azure 網路監看員
Azure 訂用帳戶 | Azure 服務健康狀態和基本資源健康狀態 | <li> 在服務或資源上執行的系統管理動作。<br/><li> 具有 Azure 服務的服務健全狀況處於降級或無法使用的狀態。<br/><li> 從 Azure 服務的觀點來看, 偵測到 Azure 資源的健康情況問題。<br/><li> 使用 Azure 自動調整執行的作業會指出失敗或例外狀況。 <br/><li> 以 Azure 原則執行的作業, 表示發生允許或拒絕的動作。<br/><li> Azure 資訊安全中心所產生的警示記錄。 |在活動記錄中傳遞, 以使用 Azure Resource Manager 進行監視和警示。
Azure 租用戶|Azure Active Directory || 啟用診斷記錄, 並設定串流處理以 Azure 監視器記錄。

<!-- markdownlint-enable MD033 -->

## <a name="hybrid-cloud-monitoring"></a>混合式雲端監視

這一節目前正在開發中，提供一組完整的建議來解決您對此雲端模型的關注，而且很快就會提供。  

## <a name="private-cloud-monitoring"></a>私人雲端監視

您可以使用 System Center Operations Manager 全面監視 Azure Stack。 具體而言, 您可以監視在租使用者中執行的工作負載、資源層級、虛擬機器上的 Azure Stack, 以及裝載的基礎結構 (實體伺服器和網路交換器)。 您也可以使用 Azure Stack 中包含的[基礎結構監視功能](https://docs.microsoft.com/azure/azure-stack/azure-stack-monitor-health)組合來達成整體監視。 這些功能可協助您在 Azure Stack 中查看 Azure Stack 區域和[Azure 監視器服務](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-metrics-azure-data)的健康情況和警示, 以提供大部分服務的基本層級基礎結構計量和記錄。

如果您已投資 Operations Manager, 請使用 Azure Stack 管理元件來監視 Azure Stack 部署的可用性和健全狀況狀態。 這包括區域、資源提供者、更新、更新執行、縮放單位、單位節點、基礎結構角色和其實例 (由硬體資源組成的邏輯實體)。 它會使用健康情況和更新資源提供者 REST Api 來與 Azure Stack 進行通訊。 若要監視實體伺服器和存放裝置, 請使用 OEM 廠商的管理元件 (例如, 由聯想、Hewlett Packard 或 Dell 提供)。 Operations Manager 可以使用 SNMP 通訊協定, 以原生方式監視網路交換器, 以收集基本統計資料。 藉由下列兩個基本步驟, 可以使用 Azure 管理元件來監視租使用者工作負載。 設定您想要監視的訂用帳戶, 然後新增該訂用帳戶的監視。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [收集正確的資料](./data-collection.md)
