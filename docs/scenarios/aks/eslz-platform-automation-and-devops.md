---
title: 企業級 Azure Kubernetes Service (AKS) platform automation 和 DevOps
description: 瞭解 Azure Kubernetes Service (AKS) platform automation 和 DevOps 的設計建議和考慮。
author: BrianBlanchard
ms.author: brblanch
ms.date: 03/01/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: ff7b2cceab332b76091489a2adb91368ae4251b7
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794684"
---
# <a name="aks-enterprise-scale-platform-automation-and-devops"></a>AKS 企業規模的平臺自動化和 DevOps

作為雲端原生結構，Kubernetes 需要雲端原生方法來部署和操作。 Azure 和 Kubernetes 是開放且可延伸的平臺，具有豐富且妥善架構的 Api，可讓您在整個範圍內自動進行自動化。 依賴自動化和一般 DevOps 的最佳作法，規劃 DevOps 且高度自動化的方法。

## <a name="design-considerations"></a>設計考量

以下是 AKS platform automation 和 DevOps 的一些設計考慮：

- 判斷您的工程和自動化方法時，請考慮 [Azure 服務限制](/azure/azure-resource-manager/management/azure-subscription-service-limits) ，以及持續整合/持續傳遞 (CI/CD) 環境。 如需其他範例，請參閱 [GitHub 使用限制](https://docs.github.com/actions/reference/usage-limits-billing-and-administration)。

- 在保護和保護開發、測試、Q&和生產環境的存取時，請考慮 CI/CD 觀點的安全性選項。 部署會自動發生，因此請適當地對應存取控制。

- 請考慮使用前置詞和尾碼搭配妥善定義的慣例，來唯一識別每個已部署的資源。 這些命名慣例可避免個別部署解決方案的衝突，並提升整體團隊的靈活性和輸送量。

- 清查工作流程，以在一般和數位 Rebar 布建 (的) 制度中，支援工程、更新和部署解決方案。 請考慮根據這些工作流程對應管線，將熟悉度和生產力最大化。

  以下是要考慮的一些範例案例和管線：
  - 部署、修補及升級叢集
  - 部署和升級應用程式
  - 部署和維護附加元件
  - 針對嚴重損壞修復進行容錯移轉
  - 藍綠部署
  - 維護未淡出的環境

- 請考慮使用 [服務網格](/azure/aks/servicemesh-about) ，將更多的安全性、加密和記錄功能新增至您的工作負載。

- 請考慮部署其他資源（例如訂用帳戶、標記和標籤），藉由追蹤和追蹤部署和相關成品來支援您的 DevOps 體驗。

- 考慮 *牛群與寵物* 典範轉變的影響。 預期 pod 和其他方面的 Kubernetes 會是暫時的，並據此調整您的自動化和管線基礎結構。 請勿依賴 IP 位址或其他資源來固定或永久性。

## <a name="design-recommendations"></a>設計建議

以下是 AKS platform automation 和 DevOps 的一些設計建議：

- 依賴管線或動作來：
  - 充分運用整個團隊的已套用實務。
  - 移除重塑滾輪的大部分負擔。
  - 提供整體品質和靈活性的可預測性和深入解析。

- 使用觸發程式和排程的管線，及早且經常地部署。 以觸發程式為基礎的管線確保變更會通過適當的驗證，而排程的管線則會管理變更環境中的行為。

- 不同的應用程式部署基礎結構部署。 核心基礎結構變更低於應用程式。 將每種部署類型視為個別的流程和管線。

- 使用 [雲端原生](/dotnet/architecture/cloud-native/introduction) 概念進行部署。 使用 [基礎結構即程式碼](/azure/devops/learn/what-is-infrastructure-as-code) 來部署包含控制平面的基礎結構，並使用 [Helm](https://helm.sh/) 和 [Kubernetes 運算子](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/) 來部署和維護 Kubernetes 的原生元件。

- 使用 [gitops) 將](/azure/azure-arc/kubernetes/tutorial-use-gitops-connected-cluster) 來部署及維護應用程式。 Gitops) 將使用 Git 儲存機制作為單一真實來源，避免在復原和相關程式期間進行設定漂移和提高生產力和可靠性。

- 將秘密和其他敏感性成品儲存在 GitHub 秘密中，讓動作和其他工作流程部分在執行時視需要讀取這些專案。

- 藉由避免硬式編碼的設定專案和設定，努力達到最大化的部署並行。

- 依賴跨基礎結構和應用程式相關部署的知名慣例。 使用與[閘道管理員](https://github.com/open-policy-agent/gatekeeper)結合的[許可控制器](https://kubernetes.io/blog/2019/03/21/a-guide-to-kubernetes-admission-controllers/)來驗證和強制執行其他已定義原則之間的慣例。

- 以一致的方式使用 [shift](/azure/devops/learn/devops-at-microsoft/shift-left-make-testing-fast-reliable) ：
  - 安全性，方法是在管線早期新增弱點掃描工具，例如容器掃描。
  - 原則，方法是使用原則作為程式碼，並透過「許可控制器」以雲端原生的方式強制執行原則。

- 將每個失敗、錯誤或中斷情形視為自動化和改進整體解決方案品質的機會。 將此方法整合到您的左移和 [網站可靠性工程， (SRE) ](/azure/site-reliability-engineering/) framework。
