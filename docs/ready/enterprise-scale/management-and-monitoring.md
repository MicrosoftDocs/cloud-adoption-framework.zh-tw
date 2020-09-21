---
title: 管理與監視
description: 探索如何以平台層級進行集中式管理和監視，以操作維護 Microsoft Azure 的企業資產。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 475db193a4d712720dd5c2402eb3be43a80ac0db
ms.sourcegitcommit: 4e12d2417f646c72abf9fa7959faebc3abee99d8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2020
ms.locfileid: "90776392"
---
# <a name="management-and-monitoring"></a>管理與監視

## <a name="plan-platform-management-and-monitoring"></a>規劃平臺管理與監視

本節將探討如何在平台層級進行集中式管理和監視，以操作維護 Azure 企業資產。 更具體來說，它會提供主要的建議，讓中央團隊在大規模的 Azure 平臺中維持操作的可見度。

![顯示管理和監視的圖表。](./media/management-and-monitoring.png)

_圖1：平臺管理與監視。_

<!-- cSpell:ignore syslogs SIEM -->

**設計考慮：**

- 使用 Azure 監視器 Log Analytics 工作區作為系統管理界限。
- 以應用程式為中心的平臺監視，分別涵蓋適用于計量和記錄的經常性存取和冷遙測路徑：
  - 作業系統計量;例如，效能計數器和自訂計量
  - 作業系統記錄檔;例如，Internet Information Services、Windows 事件追蹤和 syslog
  - 資源健康狀態事件
- 安全性審核記錄，並在整個組織的整個 Azure 資產中達成水準安全性鏡頭：
  - 內部部署安全性資訊和事件管理的潛在整合 (SIEM) 系統，例如 qRadar 或 ArcSight
  - Azure 活動記錄
  - Azure Active Directory (Azure AD) 審核報表
  - Azure 診斷服務、記錄和計量;Azure Key Vault audit 事件;網路安全性群組 (NSG) 流量記錄;和事件記錄檔
  - Azure 監視器、Azure 網路監看員、Azure 資訊安全中心和 Azure Sentinel
- Azure 資料保留閾值和封存需求：
  - Azure 監視器記錄的預設保留期間為30天，最多為兩年。
  - Azure AD 報表 (premium) 的預設保留期間為30天。
  - Azure 診斷服務的預設保留期限為90天。

- 操作需求：
  - 具有原生工具（例如 Azure 監視器記錄或協力廠商工具）的營運儀表板
  - 使用集中式角色控制具有特殊許可權的活動
  - 適用于[azure 資源的受控](/azure/active-directory/managed-identities-azure-resources/overview)識別，可存取 azure 服務
  - 用來保護編輯和刪除資源的資源鎖定

**設計建議：**

- 使用單一 [監視器記錄工作區](/azure/azure-monitor/platform/design-logs-deployment) 來集中管理平臺，但角色型存取控制 (RBAC) 、資料主權需求和資料保留原則會強制執行不同的工作區。 集中式記錄對於營運管理小組所需的可見度而言是不可或缺的。 記錄集中的驅動有關變更管理、服務健康狀態、設定，以及 IT 營運的大部分其他層面的報告。 在集中式工作區模型上進行融合可減少系統管理工作，以及可檢視性中的間隙機會。

    在企業規模架構的環境中，集中式記錄主要是與平臺作業相關。 這項強調不會防止針對以 VM 為基礎的應用程式記錄使用相同的工作區。 使用以資源為中心的存取控制模式設定的工作區時，會強制執行細微的 RBAC，以確保應用程式小組只能存取其資源的記錄。 在此模型中，應用程式小組藉由減少其管理額外負荷，受益于使用現有的平臺基礎結構。 對於任何非計算資源（例如 web 應用程式或 Azure Cosmos DB 資料庫），應用程式小組可以使用自己的 Log Analytics 工作區，並設定診斷和計量以在此路由傳送。

<!-- docutune:ignore WORM -->

- 如果記錄保留需求超過兩年，請將記錄匯出至 Azure 儲存體。 使用不可變的儲存體搭配寫入一次、讀取多個原則，讓資料在使用者指定的間隔內不可清除且不可修改。
- 使用 Azure 原則進行存取控制和合規性報告。 Azure 原則能讓您強制執行全組織的設定，以確保一致的原則遵循和快速違規偵測。 如需詳細資訊，請參閱 [瞭解 Azure 原則效果](/azure/governance/policy/concepts/effects)。
- 使用 Azure 原則監視來賓內的虛擬機器 (VM) 設定漂移。 透過原則啟用 [來賓](/azure/governance/policy/concepts/guest-configuration) 設定審核功能，可協助應用程式小組工作負載立即使用功能功能。
- [在 Azure 自動化中使用更新管理](/azure/automation/automation-update-management)作為 Windows 和 Linux vm 的長期修補機制。 透過原則強制執行更新管理設定，可確保所有 Vm 都包含在 patch management 擬訂規則中，並讓應用程式小組能夠管理其 Vm 的修補部署。 它也會為所有 Vm 的中央 IT 小組提供可見度和強制功能。
- 使用網路監看員，透過網路監看員 [NSG 流量記錄 v2](/azure/network-watcher/network-watcher-nsg-flow-logging-overview)主動監視流量流程。 使用[分析](/azure/network-watcher/traffic-analytics)會分析 NSG 流量記錄，以收集虛擬網路內 IP 流量的深入解析，並提供重要的資訊以進行有效的管理和監視。 流量分析提供的資訊包括大部分的通訊主機和應用程式通訊協定、最常交談的主機配對、允許或封鎖的流量、輸入和輸出流量、開啟網際網路埠、大部分封鎖規則、每個 Azure 資料中心的流量分配、虛擬網路、子網或 rogue 網路。
- 使用資源鎖定來防止意外刪除重要的共用服務。
- 使用 [拒絕原則](/azure/governance/policy/concepts/effects#deny) 補充 Azure AD RBAC 指派。 拒絕原則可用來防止將要求傳送至資源提供者，以防止部署和設定不符合所定義之標準的資源。 拒絕原則和 RBAC 指派的組合可確保適當的護欄已準備好可部署和設定*資源，* *以及可部署*和設定的資源。
- 在整體平臺監視解決方案中包含 [服務](/azure/service-health/service-health-overview) 和 [資源](/azure/service-health/resource-health-overview) 健康狀態事件。 從平臺的觀點來追蹤服務和資源健康狀態，是 Azure 中資源管理的重要元件。
- 請勿將原始記錄專案傳送回內部部署監視系統。 相反地，請採用在 azure 中所提供的 *資料會保留在 azure 中*的原則。 如果需要內部部署 SIEM 整合，則 [傳送重大警示](/azure/security-center/continuous-export) ，而不是記錄。

## <a name="plan-for-app-management-and-monitoring"></a>規劃應用程式管理與監視

若要在上一節中展開，此區段將會考慮同盟模型，並說明應用程式小組如何以操作方式維護這些工作負載。

**設計考慮：**

- 應用程式監視可以使用專用的 Log Analytics 工作區。
- 針對部署到虛擬機器的應用程式，記錄應該從平臺的觀點集中儲存到專用的 Log Analytics 工作區。 應用程式小組可以存取其應用程式或虛擬機器上所擁有之 RBAC 的記錄。
- 適用于基礎結構即服務的應用程式效能和健全狀況監視 (IaaS) 和平臺即服務 (PaaS) 資源。
- 跨所有應用程式元件的資料匯總。
- [健康情況模型和運算化](../..//manage/monitor/cloud-models-monitor-overview.md)：
  - 如何測量工作負載及其子系統的健康情況
  - 代表健康情況的流量燈模型
  - 如何在應用程式元件之間回應失敗

**設計建議：**

- 使用集中式 Azure 監視器 Log Analytics 工作區，從 IaaS 和 PaaS 應用程式資源收集記錄和計量，並 [使用 RBAC 控制記錄存取](/azure/azure-monitor/platform/design-logs-deployment#access-control-overview)。
- 使用 [Azure 監視器計量](/azure/azure-monitor/platform/data-platform-metrics) 來進行時間緊迫的分析。 Azure 監視器中的計量會儲存在經過優化的時間序列資料庫中，以分析時間戳記資料。 這些計量非常適用于警示和快速偵測問題。 它們也可以告訴您系統的執行狀況。 它們通常需要與記錄結合，以找出問題的根本原因。
- 使用 [Azure 監視器記錄](/azure/azure-monitor/platform/data-platform-logs) 來取得深入解析和報告。 記錄包含不同類型的資料，這些資料會組織成具有不同屬性集的記錄。 它們適合用來分析來自各種來源的複雜資料，例如效能資料、事件和追蹤。
- 必要時，請使用登陸區域內的共用儲存體帳戶來儲存 Azure 診斷擴充記錄儲存體。
- 使用 [Azure 監視器警示](/azure/azure-monitor/platform/alerts-overview) 來產生操作警示。 Azure 監視器警示會統一計量和記錄的警示，並使用動作和智慧群組等功能來進行先進的管理和修復。
