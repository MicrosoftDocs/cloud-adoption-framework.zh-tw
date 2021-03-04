---
title: AKS 企業規模的擴充性
description: 適用于企業規模擴充性的 AKS 指導方針
author: xstabel
ms.author: ctalaver
ms.date: 03/01/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 14db2304c724cf9d17652f7b2bbe0d44dca15d1e
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794937"
---
<!-- docutune:casing "HPA" -->

# <a name="aks-enterprise-scale-scalability"></a>AKS 企業規模的擴充性

AKS 可根據基礎結構需求進行相應放大和縮小， (需要更多或更少容量) 、新增具有 GPU 等特殊功能的節點集區，或應用程式需求，在此情況下，您會有數個因素，例如並行連線數和速率、要求數目，以及 AKS 應用程式的後端延遲。

AKS 最常見的擴充性選項是叢集自動調整程式 (根據 CPU 和記憶體使用率自動新增/移除節點) 或 HPA (水準 pod 自動調整程式) ，讓您的應用程式根據 CPU 和記憶體使用率以及更先進的計量來相應縮小和放大。

## <a name="design-considerations"></a>設計考量

以下是一些要考慮的重要因素：

- 您的應用程式是否需要快速的擴充性， (無時間等待) ？
  - 若要快速布建 pod，請使用虛擬節點，只有 Linux 節點/pod 支援這些節點。
- 工作負載是否為非時間敏感性，而且可以處理中斷？ 考慮使用現成的 [vm](/azure/aks/spot-node-pool)
- 基礎結構 (網路外掛程式、IP 範圍、訂用帳戶限制、配額等) 是否能夠向外擴充？
- 考慮將擴充性自動化

  - 您可以啟用叢集自動調整以調整節點數目。 考慮[](/azure/aks/cluster-autoscaler)將叢集自動[調整規模調整為零](/azure/aks/scale-cluster#scale-user-node-pools-to-0)
  - 水準 pod 自動調整程式會自動調整 pod 數目。
- 考慮使用多重區域和節點集區的擴充性
  - 建立節點集區時，請考慮 [使用 AKS 來設定可用性區域](/azure/aks/availability-zones)。
  - 請考慮使用多個節點集區來支援具有不同需求的應用程式。
  - 使用叢集自動調整程式調整節點集區。
  - 您可以將使用者節點集區調整為零。 請參閱 [限制](/azure/aks/use-multiple-node-pools#limitations)。

## <a name="design-recommendations"></a>設計建議

遵循您設計的最佳作法：

- 使用虛擬機器擴展集，這些案例包括自動調整、多節點集區，以及 Windows 節點集區支援。
  - 請勿在 Azure 入口網站中或使用 Azure CLI，以手動方式啟用或編輯擴充性的設定。
- 如果您需要快速高載自動調整，請選擇使用 Azure 容器實例和 [虛擬節點](/azure/aks/virtual-nodes-portal) 從 AKS 叢集高載，以進行快速、無限的擴充性和每秒的計費。
- 使用叢集 [自動調整程式](/azure/aks/cluster-autoscaler) ，並使用以 VM 為基礎的背景工作節點來調整可預測的 [擴充](/azure/aks/scale-cluster#scale-user-node-pools-to-0) 性。
- 啟用叢集 [自動調整程式](/azure/aks/cluster-autoscaler) 以符合應用程式需求。
  - 您可以為 [多個節點](/azure/aks/cluster-autoscaler#use-the-cluster-autoscaler-with-multiple-node-pools-enabled)集區啟用自動調整。
- 啟用 [水準 pod 自動調整程式 (HPA) ](/azure/aks/concepts-scale#horizontal-pod-autoscaler) ，以減少應用程式的忙碌時間。
  - 您所有的容器和 pod 都必須定義資源要求和限制。
  - HPA 會根據觀察到的資源限制 CPU/記憶體或自訂計量，自動調整 pod 數目。
- 啟用 [適用于容器的 Azure 監視器](/azure/azure-monitor/containers/container-insights-overview) 和即時監視，以監視叢集和工作負載的使用率。
- 當您的應用程式有不同的資源需求時，請使用多個節點集區。
  - 請記住，您可以 [為節點集區指定 VM 大小](/azure/aks/use-multiple-node-pools#specify-a-vm-size-for-a-node-pool)。
- 請考慮找出以 [VM 為基礎的節點](/azure/aks/spot-node-pool) 集區，以找出可處理中斷和收回的非時間緊迫工作負載。
