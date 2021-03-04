---
title: 適用于 AKS 的企業規模管理與監視
description: 描述此企業規模案例如何改善 AKS 的管理和監視
author: BrianBlanchard
ms.author: pidebrui
ms.date: 03/01/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: ca2e137dec3c9b2e0874003c617f783fb1e8b046
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794845"
---
# <a name="management-and-monitoring-for-aks-enterprise-scale-scenario"></a>適用于 AKS 企業規模案例的管理和監視

Kubernetes 是一種相當年輕的技術，它會快速演進，而且有很深刻的生態系統。 因此管理它可能是個挑戰。 藉由適當地設計包含管理和監視的解決方案，您可以在卓越的營運和客戶成功方面發揮作用。

## <a name="design-considerations"></a>設計考量

請考慮下列因素：

- 請注意 [AKS 限制](/azure/aks/quotas-skus-regions)。 使用多個 AKS 實例來調整超過這些限制。
- 請注意，在叢集中以邏輯方式隔離工作負載，並在不同的叢集上隔離工作負載。
- 請留意依工作負載控制資源耗用量的方式。
- 請留意協助 Kubernetes 瞭解工作負載健康情況的方式。
- 請留意各種不同的虛擬機器大小，以及使用其中一種的影響。 較大的 Vm 可以處理更多負載。 當較小的 Vm 無法進行規劃和未規劃的維護時，其他的 Vm 可以更容易取代。 也請留意節點集區的概念、擴展集中的 Vm，以允許在相同叢集中有不同大小的虛擬機器。
- 請留意監視和記錄 AKS 的方式。 Kubernetes 由各種元件和監視和記錄所組成，可提供其健康情況、趨勢及潛在問題的見解。
- 建立監視和記錄功能時，Kubernetes 或在最上層執行的應用程式可能會產生許多事件。 警示有助於區分記錄檔專案的歷程記錄，以及需要立即採取行動的記錄專案。
- 請留意您應該進行的更新和升級。 Kubernetes 層級有主要、次要和修補程式版本。 客戶應該套用這些更新，根據上游 Kubernetes 中的原則保持在支援的狀態。 在背景工作主機層級作業系統核心修補程式可能需要重新開機（客戶應該這麼做），以及升級至新的 OS 版本。
- 請留意叢集的資源限制以及個別的工作負載。
- 監視最佳作法的使用。
- 請留意使用 Azure 來管理多個叢集的選項，以及在 Azure 外部 Kubernetes 認證叢集的選項。

## <a name="design-recommendations"></a>設計建議

- 瞭解 AKS 限制：
  - [配額和區域限制](/azure/aks/quotas-skus-regions)
  - [AKS 服務限制](/azure/azure-resource-manager/management/azure-subscription-service-limits#azure-kubernetes-service-limits)
- 在命名空間層級使用邏輯隔離來分隔應用程式、小組、環境、業務單位。 多租使用者[和叢集隔離](/azure/aks/operator-best-practices-cluster-isolation)。 此外，節點集區可協助具有不同節點規格的節點，以及像 Kubernetes 升級[多個節點](/azure/aks/use-multiple-node-pools)集區一樣的維護
- 在命名空間層級規劃及套用資源配額。 如果 Pod 未定義資源要求和限制，則拒絕該部署。 監視資源使用量，並視需要調整配額。 [基本排程器功能](/azure/aks/operator-best-practices-scheduler)
- 將健康情況探查新增至您的服務。 請確定 pod 包含 `livenessProbe` 、 `readinessProbe` 和 `startupProbe` [AKS 健康情況探查](/azure/application-gateway/ingress-controller-add-health-probes)。
- 使用夠大的 VM 大小來包含多個容器實例，如此您就能獲得更多的密度，但您的叢集無法處理失敗節點的工作負載。
- 使用監視解決方案。 預設會設定[適用于容器的 Azure 監視器](/azure/azure-monitor/containers/container-insights-overview)，並可讓您輕鬆存取許多深入資訊。 如果您想要深入瞭解或體驗使用 Prometheus，您可以使用 [Prometheus 整合](/azure/azure-monitor/containers/container-insights-prometheus-integration) 。 如果您也想要在 AKS 上執行監視應用程式，您也應該使用 Azure 監視器來監視該應用程式。
- 使用警示系統可在需要直接採取行動的情況時提供通知。 [計量警示](/azure/azure-monitor/containers/container-insights-metric-alerts) \(部分機器翻譯\)
- 使用 [自動節點集區調整](/azure/aks/cluster-autoscaler) 功能搭配 [水準 pod 自動調整程式](/azure/aks/concepts-scale#horizontal-pod-autoscaler) 來滿足應用程式需求，並減少尖峰時數的負載。
- 使用 Azure Advisor 來取得有關成本、安全性、可靠性、卓越的營運和效能的最佳做法建議。 此外，也可以使用 [Azure 安全性中心](/azure/security-center/defender-for-kubernetes-introduction) 來防止和偵測映射弱點之類的威脅。
- 使用 azure [Arc](/azure/azure-arc/kubernetes/overview) enabled Kubernetes，在 azure 中使用 azure 原則、資訊安全中心、gitops) 將等來管理非 AKS Kubernetes 叢集。
