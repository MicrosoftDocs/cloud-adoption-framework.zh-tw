---
title: 叢集設計和作業
description: 瞭解適用于叢集設計和作業的雲端採用架構中的 Kubernetes。
author: sabbour
ms.author: asabbour
ms.date: 12/16/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: 6745e271700280b87800fbb76d603242bf561b19
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86448753"
---
<!-- cSpell:ignore asabbour sabbour autoscaler PDBs -->

# <a name="cluster-design-and-operations"></a>叢集設計和作業

識別叢集設定和網路設計。 透過自動化基礎結構布建，來提供更高的擴充性。 藉由規劃商務持續性和災害復原，來維持高可用性。

## <a name="plan-train-and-proof"></a>規劃、定型和證明

當您開始使用時，以下的檢查清單和資源將可協助您規劃叢集設計。 您應該能夠回答下列問題：

<!-- markdownlint-disable MD033 -->

> [!div class="checklist"]
>
> - 您是否已識別出叢集的網路設計需求？
> - 您有不同需求的工作負載嗎？ 您要使用多少個節點集區？

<!-- -->

> [!div class="tdCol2BreakAll"]
>
> | 檢查清單  | 資源 |
> |------------------------------------------------------------------|-----------------------------------------------------------------|
> | **識別網路設計考慮。** 瞭解叢集網路設計考慮、比較網路模型，以及選擇符合您需求的 Kubernetes 網路外掛程式。    | [Kubenet 和 Azure 容器網路介面（CNI）](https://docs.microsoft.com/azure/aks/concepts-network#azure-virtual-networks) <br> [在 Azure Kubernetes Service (AKS) 中使用 kubenet 網路與您自己的 IP 位址範圍](https://docs.microsoft.com/azure/aks/configure-kubenet) <br> [在 Azure Kubernetes Service (AKS) 中設定 Azure CNI 網路](https://docs.microsoft.com/azure/aks/configure-azure-cni) <br> [AKS 叢集的安全網路設計](https://github.com/azure/sg-aks-workshop/blob/master/cluster-design/NetworkDesign.md) |
> | **建立多個節點集區。** 若要支援具有不同計算或儲存體需求的應用程式，您可以選擇性地使用多個節點集區來設定您的叢集。 例如，使用額外的節點集區來為計算密集型應用程式提供 Gpu，或存取高效能 SSD 儲存體。   | [在 Azure Kubernetes Service 中建立和管理叢集的多個節點集區](https://docs.microsoft.com/azure/aks/use-multiple-node-pools) |
> | **決定可用性需求。** 若要為您的應用程式提供更高的可用性層級，可以將叢集分散在可用性區域。 這些區域是在指定區域中實體獨立的資料中心。 當叢集元件分散到多個區域時，您的叢集 cano 會容忍其中一個區域發生失敗。 即使整個資料中心發生問題，您的應用程式和管理作業仍可繼續使用。   | [建立使用可用性區域的 Azure Kubernetes Service （AKS）叢集](https://docs.microsoft.com/azure/aks/availability-zones) |

## <a name="go-to-production-and-apply-best-practices"></a>移至生產環境並套用最佳作法

當您準備應用程式以進行生產時，您應該執行一組最基本的最佳作法。 請在此階段使用以下檢查清單。 您應該能夠回答下列問題：

> [!div class="checklist"]
>
> - 您是否能夠安心重新部署叢集基礎結構？
> - 您是否已套用資源配額？

<!-- -->

> [!div class="tdCol2BreakAll"]
>
> | 檢查清單  | 資源 |
> |---|---|
> | **自動布建叢集。** 透過基礎結構即程式碼，您可以將基礎結構布建自動化，以在嚴重損壞時提供更多的復原能力，並可在需要時快速重新部署基礎結構 | [使用 Azure Kubernetes Service 和 Terraform 建立 Kubernetes 叢集](https://docs.microsoft.com/azure/terraform/terraform-create-k8s-cluster-with-tf-and-aks) |
> | **使用 pod 中斷預算來規劃可用性。** 若要維護應用程式的可用性，請定義 pod 中斷預算（Pdb），以確保在硬體故障或叢集升級期間，叢集中可使用最少的 pod 數目。 | [使用 Pod 中斷預算規劃可用性](https://docs.microsoft.com/azure/aks/operator-best-practices-scheduler#plan-for-availability-using-pod-disruption-budgets) |
> | **在命名空間上強制執行資源配額。** 在命名空間層級規劃和套用資源配額。 您可以在計算資源、儲存體資源和物件計數上設定配額。 | [強制執行資源配額](https://docs.microsoft.com/azure/aks/operator-best-practices-scheduler#enforce-resource-quotas) |

## <a name="optimize-and-scale"></a>優化和調整

應用程式現在已進入生產階段，如何優化您的工作流程，並準備您的應用程式和小組進行調整？ 使用優化和調整檢查清單來準備。 您應該能夠回答下列問題：

> [!div class="checklist"]
>
> - 您是否有針對商務持續性和嚴重損壞修復的計畫？
> - 您的叢集可以調整以符合應用程式需求嗎？
> - 您是否能夠監視叢集和應用程式的健康情況並接收警示？

<!-- -->

> [!div class="tdCol2BreakAll"]
>
> | 檢查清單  | 資源 |
> |------------------------------------------------------------------|-----------------------------------------------------------------|
> | **自動調整叢集以符合應用程式需求。** 若要跟上應用程式的需求，您可能需要調整使用叢集自動調整程式自動執行工作負載的節點數目。 | [設定 Kubernetes cluster 自動調整程式](https://docs.microsoft.com/azure/aks/cluster-autoscaler)    |
> | **規劃商務持續性和嚴重損壞修復。** 規劃多區域部署、建立儲存體遷移計畫，以及啟用容器映射的異地複寫。 | [區域部署的最佳作法](https://docs.microsoft.com/azure/aks/operator-best-practices-multi-region) <br> [Azure Container Registry 異地複寫](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication)  |
> | **設定大規模的監視和疑難排解。** 在 Kubernetes 中設定應用程式的警示和監視。 瞭解預設設定、如何整合更先進的計量，以及如何新增您自己的自訂監視和警示，以可靠地操作您的應用程式。 | [開始使用 Kubernetes 的監視和警示（影片）](https://www.youtube.com/watch?v=W7aN_z-cyUw&list=PLLasX02E8BPCrIhFrc_ZiINhbRkYMKdPT&index=16) <br> [使用適用于容器的 Azure 監視器來設定警示](https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-overview) <br> [查看主要元件的診斷記錄](https://docs.microsoft.com/azure/aks/view-master-logs) <br> [Azure Kubernetes Service （AKS）診斷](https://docs.microsoft.com/azure/aks/concepts-diagnostics)    |
