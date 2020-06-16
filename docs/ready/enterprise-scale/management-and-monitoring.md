---
title: 管理與監視
description: 管理與監視
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: b83e0c6139b2d2fd3b89c5345063a7ffad7f7876
ms.sourcegitcommit: d88c1cc3597a83ab075606d040ad659ac4b33324
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/15/2020
ms.locfileid: "84792463"
---
# <a name="management-and-monitoring"></a>管理與監視

## <a name="planning-platform-management-and-monitoring"></a>規劃平臺管理和監視

本節將探討如何在平台層級上，使用集中式管理和監視，來維護 Microsoft Azure 的企業資產。 更明確地說，它會針對中央團隊提供重要的建議，以維護大規模 Azure 平臺內的操作可見度。

![管理與監視](./media/management-and-monitoring.png)

_圖1：平臺管理和監視。_

<!-- cSpell:ignore syslogs SIEM -->

**設計考慮：**

- 使用 Azure 監視器 Log Analytics 工作區作為系統管理界限。

- 以應用程式為中心的平臺監視，分別包含計量和記錄檔的經常性存取和冷遙測路徑：

  - 作業系統計量（效能計數器和自訂計量）

  - 作業系統記錄（Internet Information Services （IIS）、Windows 事件追蹤和 syslog）

  - 資源健康狀態事件

- 安全性審核記錄，並在客戶的整個 Azure 資產中達到水準安全性鏡頭：

  - 與內部部署安全性資訊和事件管理（SIEM）系統（例如 ServiceNow 或 ArcSight）的潛在整合

  - Azure 活動記錄

  - Azure AD audit 報告

  - Azure 診斷服務、記錄和計量;Azure Key Vault audit 事件;網路安全性群組（NSG）流量記錄;和事件記錄檔

  - Azure 監視器、網路監看員、資訊安全中心和 Azure Sentinel

- Azure 資料保留閾值和封存需求：

  - Azure 監視器記錄檔的預設保留期限為30天，最多兩年

  - Azure Active Directory 報表（premium）的預設保留期限為30天

  - Azure 診斷服務的預設保留期限為90天

- 操作需求：

  - 具有原生工具（例如 Azure 監視器記錄或協力廠商工具）的操作儀表板

  - 使用集中式角色控制特殊許可權活動

  - 適用于[azure 資源的受控](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)識別以存取 azure 服務

  - 用來保護編輯和刪除資源的資源鎖定

**設計建議：**

- 使用單一 [[監視記錄] 工作區](https://docs.microsoft.com/azure/azure-monitor/platform/design-logs-deployment)來集中管理平臺，但角色型存取控制（RBAC）和資料主權需求會強制不同的工作區。 集中式記錄對於營運管理小組所需的可見度而言非常重要。 記錄集中的磁片磁碟機會報告有關變更管理、服務健康情況、設定和 IT 作業的大部分其他層面。 在集中式工作區模型上進行整合，可減少系統管理工作，以及可檢視性中的間隙機會。

在企業級架構的環境中，集中式記錄主要關心平臺作業。 這不會妨礙使用相同的工作區來進行 VM 型應用程式記錄。 在以資源為中心的存取控制模式中設定工作區時，會強制執行細微的 RBAC，以確保應用程式小組只能存取其資源的記錄。 在此模型中，應用程式團隊藉由減少其管理額外負荷，受益于現有平臺基礎結構的使用。 針對任何非計算資源（例如 web 應用程式或 Azure Cosmos DB 資料庫），應用程式小組可以使用自己的 Log Analytics 工作區，並將診斷和計量設定為在此路由傳送。

<!-- docsTest:ignore WORM -->

- 如果記錄保留需求超過兩年，則將記錄匯出至 Azure 儲存體。 使用不可變的儲存體搭配 WORM （一次寫入，多次讀取）原則，讓資料在使用者指定的間隔內無法擦寫且不可修改。

- 使用 Azure 原則進行存取控制和合規性報告。 這可讓您強制執行整個組織的設定，以確保一致的原則遵循和快速違規偵測。 如需詳細資訊，請參閱[瞭解 Azure 原則效果](https://docs.microsoft.com/azure/governance/policy/concepts/effects)。

- 使用 Azure 原則監視來賓內虛擬機器（VM）設定漂移。 透過原則啟用[來賓](https://docs.microsoft.com/azure/governance/policy/concepts/guest-configuration)設定審核功能可協助應用程式小組工作負載輕鬆地立即使用功能功能。

- [在 Azure 自動化中使用更新管理](https://docs.microsoft.com/azure/automation/automation-update-management)，做為 Windows 和 Linux vm 的長期修補機制。
 透過原則強制執行更新管理設定可確保所有 Vm 都包含在 patch management 擬訂規則中，讓應用程式小組能夠管理其 Vm 的修補程式部署，並為中央 IT 提供所有 Vm 之間的可見度和強制功能。

- 使用 Azure 網路監看員透過[網路監看員 NSG 流量記錄 v2](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview)主動監視流量。 使用[分析](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)會分析 NSG 流量記錄，以收集有關虛擬網路內 IP 流量的深入解析，並提供重要的資訊來進行有效的管理和監視。 流量分析提供的資訊包括大部分的通訊主機和應用程式通訊協定、最常交談的主機組、允許或封鎖的流量、輸入和輸出流量、開啟網際網路埠、大部分封鎖規則、每個 Azure 資料中心的流量分配、虛擬網路、子網或 rogue 網路。

- 使用資源鎖定來防止意外刪除重要的共用服務。

- 使用[拒絕原則](https://docs.microsoft.com/azure/governance/policy/concepts/effects#deny)來補充 Azure AD RBAC 指派。 拒絕原則是用來防止將要求傳送給資源提供者，以避免部署和設定不符合所定義標準的資源。 [拒絕原則] 和 [RBAC 指派] 的組合可確保已備妥適當的防護 rails，以強制可部署和設定資源的**人員** **，以及可**部署和設定的資源。

- 將[服務](https://docs.microsoft.com/azure/service-health/service-health-overview)和[資源](https://docs.microsoft.com/azure/service-health/resource-health-overview)健康狀態事件納入為整體平臺監視解決方案的一部分。 從平臺的觀點來追蹤服務和資源健康狀態，是 Azure 中資源管理的重要元件。

- 請勿將原始記錄檔專案傳送回內部部署監視系統。 取而代之的是，採用*azure 中的資料會保留在 azure 中*的原則。 如果需要內部部署 SIEM 整合，則[傳送重大警示](https://docs.microsoft.com/azure/security-center/continuous-export)，而不是記錄。

## <a name="planning-for-app-management-and-monitoring"></a>規劃應用程式管理和監視

若要在上一節中展開，本節將考慮客戶應用程式工作負載的同盟管理和監視，並說明應用程式小組可以如何操作維護這些工作負載。

**設計考慮：**

- 應用程式監視可以使用專用的 Log Analytics 工作區。

- 對於部署到虛擬機器的應用程式，記錄應該從平臺的觀點集中儲存到專用的 Log Analytics 工作區，而應用程式小組可以存取以其應用程式/虛擬機器上的 RBAC 為依據的記錄。

- 基礎結構即服務（IaaS）和平臺即服務（PaaS）資源的應用程式效能與健康情況監視

- 跨所有應用程式元件的資料匯總

- [健全狀況模型和運算化](https://docs.microsoft.com/azure/cloud-adoption-framework/manage/monitor/cloud-models-monitor-overview)：

  - 如何測量工作負載及其子系統的健全狀況
  - 代表健全狀況的流量光線模型
  - 如何在應用程式元件之間回應失敗

**設計建議：**

- 使用集中式 Azure 監視器 Log Analytics 工作區，從 IaaS 和 PaaS 應用程式資源收集記錄和計量，並[使用 RBAC 控制記錄存取](https://docs.microsoft.com/azure/azure-monitor/platform/design-logs-deployment#access-control-overview)。

- 使用[Azure 監視器度量](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics)來進行時間緊迫的分析。 Azure 監視器中的計量儲存在時間序列資料庫中，並已優化以分析時間戳記資料。 這可讓計量適用于警示和快速偵測問題。 它們可以告訴您系統的執行方式，但它們通常需要與記錄結合，以識別問題的根本原因。

- 使用[Azure 監視器記錄](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs)來取得見解和報告。 記錄包含不同類型的資料，會組織成具有不同屬性集的記錄，而且它們有助於分析來自某個範圍來源（例如效能資料、事件和追蹤）的複雜資料。

- 必要時，請使用登陸區域內的共用儲存體帳戶來進行 Azure 診斷延伸模組記錄儲存體。

- 使用[Azure 監視器警示](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)來產生操作警示。 Azure 監視器警示會統一計量和記錄的警示，並使用動作和智慧群組等功能來進行先進的管理和修復。
