---
title: 企業規模的 Azure Kubernetes Service (AKS) 安全性治理和合規性
description: 瞭解雲端安全性控制生命週期，以及如何設定 AKS 安全性控制、Azure 原則和 AKS 成本管理。
author: BrianBlanchard
ms.author: brblanch
ms.date: 03/01/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 6f7b00443846d2219828784c077b00516211752e
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794920"
---
<!-- docutune:casing "Contoso Financials" -->

# <a name="aks-enterprise-scale-platform-security-governance-and-compliance"></a>AKS 企業規模的平臺安全性治理和合規性

本文將逐步解說 Azure Kubernetes Service (AKS) 安全性治理，以在執行任何解決方案之前思考。

大部分的內容都是技術中立的，因為每個客戶都有不同的執行方式。 本文著重于如何使用 Azure 和一些開放原始碼軟體 (OSS) 來執行解決方案。

## <a name="cloud-security-control-lifecycle"></a>雲端安全性控制生命週期

在雲端原生安全性和治理中，雲端可以是動態的，而大部分的企業則是用於相對穩定的環境。 傳統環境包含在結構化組織模型中的偵測和防護控制項，可與傳統的 IT 功能（例如身分識別、網路、電腦和虛擬機器） (VM) OS 管理。 傳統的安全性與合規性小組會面對整體結構和基礎結構的較不頻繁變更，或是為了達成安全性和風險目標所需的控制項和監視。

在採用雲端和 DevOps 模型的情況下，安全性必須適應變更，因此不會成為商務和進度的障礙。 安全性採用原音包含人員程式，像是建立正確的角色和職責，以及像是掃描、測試和基礎結構即程式碼等自動化和技術工具。 這些工具是 DevOps 管線的一部分，可進行創新和快速傳遞時間，同時仍然符合安全性需求。

這項誘惑是從您今天執行的工作開始，但關鍵重點是如果您嘗試在雲端中執行的工作，與您今天在內部部署的工作完全相同，您將會失敗。 雲端採用是學習、更新技能和提高組織安全性狀態的機會。 您可以藉由在開發到生產過程中簡化安全性，來將安全性定位為啟用程式，而不是封鎖程式。 如果您使用10-20 年舊的工具和程式，您會繼承或引進10-20 年的安全性挑戰、問題和挫折。 雲端採用是您開始新的機會。

從頭開始，先瞭解雲端。

- 可用的雲端服務、控制項和可見度有哪些？
- 如何操作雲端服務和控制項？
- 可用的安全性工具和治理工具有哪些？
- 您要如何在程式和系統中使用安全性和治理工具，如此一來，企業營運可以更靈活地運用新式 DevOps 模型？
- 您要如何確保符合安全性和合規性需求？

從程式觀點來啟動生命週期架構通常是最簡單的方式，無論是建立安全性控制項，也可以管理和運用控制項本身。 針對建立安全性控制，許多客戶會使用常見的架構和標準，例如 NIST 800-53、ISO、CIS 效能評定和 HIPAA/HITRUST。 這些標準可協助建立全方位的架構，並提供指引來協助建立、記錄和審核安全性控制和程式。

若要管理和運用安全性控制，您可以查看各種模型，包括包含雲端和 DevOps 的安全性開發生命週期流程。 這些方法和模型會將安全性控制項視為組織中的任何其他資產，並包括生命週期管理。 下圖顯示組織的安全性控制生命週期管理範例：

:::image type="content" source="./media/security-control-lifecycle.png" alt-text="顯示 Azure 安全性控制生命週期管理設定的圖表。" border="false":::

### <a name="azure-security-control-architecture"></a>Azure 安全性控制架構

所有 Azure 安全性控制的開頭都是 Azure Resource Manager API，並建置於其上。 下圖顯示 Azure 原則引擎如何協助強制執行原則（不論其來源為何）。 這項功能很重要，因為這表示組織不需要在多個位置定義原則。

:::image type="content" source="./media/enterprise-control-plane-architecture.png" alt-text="顯示 Azure 安全性控制架構的圖表。" border="false":::

### <a name="security-control-governance"></a>安全性控制治理

安全性控制架構的主要優點之一，就是它的治理與控制的控制。 除了 Azure 原則，Azure 安全性控制治理還包括 [Azure 安全性中心](/azure/security-center/security-center-introduction)，以及其 [安全分數](/azure/security-center/secure-score-security-controls) 和合規性儀表板功能。 規範的核心是關於治理可檢視性。 下圖顯示範例合規性儀表板：

:::image type="content" source="./media/enterprise-control-plane-governance.png" alt-text="顯示 Azure 安全性控制治理的圖表。" border="false":::

## <a name="azure-security-controls-setup"></a>Azure 安全性控制項設定

本節將逐步解說如何設定 Azure 專屬的最上層安全性控制架構。 本節假設已建立組織安全性控制項。 定義這些安全性控制項不在本文的討論範圍內。

下列各節將說明如何執行下列控制項，以符合虛構 Contoso 財務案例的安全性需求：

- 記錄所有雲端 API 要求以進行 audit reporting。
- 啟用授權的 IP 範圍，以保護對 API 伺服器的存取。
- 使用允許的位置，只在特定區域中建立 AKS 叢集。

這些控制項只是完整 Azure 部署所包含的安全性控制和 Azure 原則的子集。 一旦您瞭解程式之後，就可以重複此程式，以實行您組織所需的任何其他安全性控制項。

### <a name="log-all-cloud-api-requests-for-audit-reporting-purposes"></a>記錄所有雲端 API 要求以供 audit reporting 之用

[Azure 活動記錄](/azure/azure-monitor/essentials/platform-logs-overview) 會捕捉所有 Azure Resource Manager 的互動，包括審核記錄。 挑戰在於活動記錄只會有特定的保留週期。 若要保留記錄以進行 audit reporting，您必須將活動記錄中的資料匯出到更持續的儲存位置，例如 Azure 監視器記錄。

如需有關如何設定 Azure 訂用帳戶之活動記錄收集的教學課程，請參閱 [傳送至 Log Analytics 工作區](/azure/azure-monitor/essentials/activity-log#send-to-log-analytics-workspace)。

下列螢幕擷取畫面顯示 Azure Log Analytics 工作區中的 Azure 活動記錄檔捕獲：

:::image type="content" source="./media/activity-log-capture.png" alt-text="顯示活動記錄檔捕捉的螢幕擷取畫面。" border="false":::

### <a name="enable-azure-security-monitoring-to-check-for-authorized-ip-ranges"></a>啟用 Azure 安全性監視以檢查授權的 IP 範圍

建立任何服務之前，請先查看您的雲端提供者，以取得安全性監視的最佳作法和建議。 雲端提供者不會執行組織所需的所有安全性控制，但關鍵是使用提供的控制項來避免重塑滾輪，並且只在必要時才建立自訂控制項。

在 Azure 中，您可以啟用 Azure 安全性中心來查看安全性監視建議。 您也可以使用 Azure 安全性中心來判斷 AKS 叢集是否已啟用授權的 IP 範圍。

若要啟用 azure 訂用帳戶的 Azure 安全性中心標準，請參閱 [快速入門：設定 Azure 安全性中心](/azure/security-center/security-center-get-started)。

下列螢幕擷取畫面顯示 Azure 安全性中心標準監視：

:::image type="content" source="./media/security-center-standard.png" alt-text="顯示 Azure 安全性中心標準監視的螢幕擷取畫面。":::

### <a name="enforce-an-azure-policy-to-create-aks-clusters-only-in-certain-regions"></a>強制 Azure 原則只在特定區域中建立 AKS 叢集

也請參閱雲端提供者，以強制執行原則。 在 Azure 中，評估「Azure 原則」，這是企業控制平面的重要部分。 您無法使用 Azure 原則來執行任何動作，但金鑰是使用提供的內容來避免重塑滾輪，並且只在必要時才建立自訂控制項。

您可以使用 Azure 原則來執行建立 Azure 資源（例如 AKS 叢集）所允許的位置。 如需詳細資訊和指示，請參閱 [教學課程：建立和管理原則以強制執行合規性](/azure/governance/policy/tutorials/create-and-manage)。

下列螢幕擷取畫面顯示使用 Azure 原則來執行允許的區域位置的步驟。

1. 在 Azure 原則中選取內建的 **允許位置** 原則。

   :::image type="content" source="./media/policy-definitions.png" alt-text="顯示內建 Azure 原則的螢幕擷取畫面。":::

1. 查看原則定義並指派原則。

   :::image type="content" source="./media/assign-policy.png" alt-text="顯示允許的位置原則定義的螢幕擷取畫面。":::

1. 設定原則。

   :::image type="content" source="./media/configure-policy.png" alt-text="顯示 [允許的位置] 設定畫面的螢幕擷取畫面。":::

1. 設定原則參數。

   :::image type="content" source="./media/configure-parameters.png" alt-text="顯示 [允許的位置] 參數畫面的螢幕擷取畫面。":::

1. 查看原則指派。

   :::image type="content" source="./media/policy-assignments.png" alt-text="顯示 Azure 原則指派的螢幕擷取畫面。":::

限制資源建立至特定區域的能力只是許多可用的 Azure 原則之一。

- 如需有關如何使用 Azure 原則的範例，請參閱 [Azure 原則範例](/azure/governance/policy/samples/)。
- 如需 azure 資訊安全中心的 Azure 原則的詳細資訊，請參閱 azure 資訊 [安全中心的 Azure 原則內建定義](/azure/security-center/policy-reference)。

## <a name="cost-governance"></a>成本治理

成本治理是執行原則以控制成本的連續程式。 在 Kubernetes 內容中，有數種方式可讓組織控制及優化成本。 這些工具組括原生 Kubernetes 工具，可管理及管理資源使用量和耗用量，並主動監視和優化基礎結構。

本節說明如何使用 [Kubecost](https://kubecost.com/) 來管理 AKS 叢集成本。 您可以將成本配置的範圍設為部署、服務、標籤、pod 或命名空間，以提供復原或顯示叢集使用者的彈性。

### <a name="install-kubecost"></a>安裝 Kubecost

有幾個 Kubecost 的安裝選項。 如需詳細資訊，請參閱 [安裝 Kubecost](https://docs.kubecost.com/install)。

若要直接安裝 Kubecost，請使用下列命令：

```bash

# Create the Kubecost namespace

kubectl create namespace kubecost

# Install Kubecost into the AKS cluster

kubectl apply -f https://raw.githubusercontent.com/kubecost/cost-analyzer-helm-chart/master/kubecost.yaml --namespace kubecost
```

若要使用 Helm 2 安裝 Kubecost，請使用下列命令：

```bash
helm repo add kubecost https://kubecost.github.io/cost-analyzer/
helm install kubecost/cost-analyzer --namespace kubecost --name kubecost --set kubecostToken="YWxnaWJib25AbWljcm9zb2Z0LmNvbQ==xm343yadf98"
```

若要使用 Helm 3 安裝 Kubecost，請使用下列命令：

```bash
kubectl create namespace kubecost
helm repo add kubecost https://kubecost.github.io/cost-analyzer/
helm install kubecost kubecost/cost-analyzer --namespace kubecost --set kubecostToken="YWxnaWJib25AbWljcm9zb2Z0LmNvbQ==xm343yadf98"
```

幾分鐘後，請檢查以確定 Kubecost 已啟動且正在執行：

```bash
kubectl get pods -n kubecost

# Connect to the Kubecost dashboard UI

kubectl port-forward -n kubecost svc/kubecost-cost-analyzer 9090:9090
```

您現在可以開啟瀏覽器並指向 `http://127.0.0.1:9090` 以開啟 KUBECOST UI。 在 Kubecost UI 中，選取您的叢集以查看成本配置資訊。

### <a name="navigate-kubecost"></a>導覽 Kubecost

Kubecost 會將資源細分為下列類別：

- 每月叢集成本
- 命名空間成本
- 部署資源成本
- 成本效益

選取您的叢集以查看類似下列儀表板的總覽：

:::image type="content" source="./media/kubecost-dashboard.png" alt-text="顯示 Kubecost 儀表板的螢幕擷取畫面。":::

選取左側的 [ **配置** ]，以深入瞭解資源的命名空間成本。 **配置** 會顯示 CPU、記憶體、持續性磁片區和網路的成本。 Kubecost 會從 Azure 定價取得資料，但您也可以為資源設定自訂成本。

:::image type="content" source="./media/kubecost-allocation.png" alt-text="顯示 [Kubecost 配置] 畫面的螢幕擷取畫面。":::

選取左側的 **節省** 成本，以深入瞭解使用量過低的資源。 **節省成本** 可提供您未充分利用的節點和 pod 和已放棄資源的相關資訊，並識別在叢集中過度布建的資源要求。 下列螢幕擷取畫面顯示範例 **節省** 的總覽：

:::image type="content" source="./media/kubecost-savings.png" alt-text="顯示 Kubecost 節省畫面的螢幕擷取畫面。":::

花一點時間流覽 Kubecost 提供的不同視圖和功能。

## <a name="design-considerations"></a>設計考量

AKS 有數個 azure 服務（例如 Azure Active Directory、Azure 儲存體和 Azure 虛擬網路）的介面，在規劃階段中需要特別注意。 AKS 也增加了額外的複雜度，您必須考慮套用相同的安全性、治理和合規性機制和控制項，就像基礎結構的其餘部分一樣。

以下是一些 AKS 安全性治理和合規性的其他設計考慮：

- 決定叢集的控制平面是否可透過網際網路（預設值）或僅在特定虛擬網路內作為私人叢集來存取。

- 使用內建的 [AppArmor](/azure/aks/operator-best-practices-cluster-security#app-armor) Linux 安全性模組來評估，以限制容器可執行檔動作，例如讀取、寫入、執行或系統功能，例如掛接檔案系統。

- 評估使用 [安全運算 (seccomp) ](/azure/aks/operator-best-practices-cluster-security#secure-computing) 在流程層級，以限制容器可執行檔進程呼叫。

- 請考慮使用適用于 [Kubernetes 的 Azure Defender](/azure/security-center/defender-for-kubernetes-introduction) 來偵測威脅。

## <a name="design-recommendations"></a>設計建議

- 使用 Azure 角色型存取控制來限制對 [Kubernetes 叢集配置](/azure/aks/control-kubeconfig-access) 檔的存取。

- [保護 pod 對資源的存取](/azure/aks/developer-best-practices-pod-security#secure-pod-access-to-resources)。 提供最少的許可權，並避免使用根或特殊許可權的提升。

- 使用 [Pod 管理](/azure/aks/operator-best-practices-identity#use-pod-managed-identities) 的身分識別和 [Azure 金鑰保存庫提供者，以取得秘密存放區 CSI 驅動程式](https://github.com/Azure/secrets-store-csi-driver-provider-azure) 來保護秘密、憑證和連接字串。

- 請務必定期 [輪替憑證](/azure/aks/certificate-rotation) ，例如每隔90天。

- 使用 [AKS 節點映射升級](/azure/aks/node-image-upgrade) 以更新 AKS 叢集節點映射（如果可能的話），或在套用更新之後自動重新開機節點的 [kured](/azure/aks/node-updates-kured) 。

- 在 [Azure 安全性中心](/azure/security-center/security-center-introduction)內查看 AKS 建議。

- 使用 [適用于 Kubernetes 的 Azure 原則附加](/azure/aks/use-pod-security-on-azure-policy)元件來監視和強制執行設定。
