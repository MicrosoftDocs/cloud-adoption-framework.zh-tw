---
title: 叢集設計和作業
description: 瞭解適用于叢集設計和作業的雲端採用架構中的 Kubernetes。
author: sabbour
ms.author: asabbour
ms.date: 12/16/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: 87761112f0722f46c6b60233145638715287737b
ms.sourcegitcommit: 57b757759b676a22f13311640b8856557df36581
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2020
ms.locfileid: "94994355"
---
<!-- cSpell:ignore autoscaler PDBs -->

# <a name="cluster-design-and-operations"></a>叢集設計和作業

識別叢集設定和網路設計。 藉由自動化基礎結構布建，來提供未來的調整能力。 藉由規劃商務持續性和災害復原，來維持高可用性。

## <a name="plan-train-and-proof"></a>規劃、定型和證明

當您開始使用時，下列檢查清單和資源將協助您規劃叢集設計。 您應該可以回答下列問題：

> [!div class="checklist"]
>
> - 您是否已找出叢集的網路設計需求？
> - 您是否有不同需求的工作負載？ 您要使用多少個節點集區？

<!-- -->

> | 檢查清單  | 資源 |
> |------------------------------------------------------------------|-----------------------------------------------------------------|
> | **找出網路設計考慮。** 瞭解叢集網路設計考慮、比較網路模型，並選擇符合您需求的 Kubernetes 網路外掛程式。 針對 (CNI) 網路的 Azure 容器網路介面，請將所需的 IP 位址數目視為每個節點的最大 pod 數目 (預設值為 30) 和節點數目。 新增升級期間所需的一個節點。 選擇負載平衡器服務時，請考慮在有太多服務時使用輸入控制器，以減少公開端點的數目。 針對 Azure CNI，服務 CIDR 在虛擬網路與所有連線的虛擬網路之間必須是唯一的，以確保適當的路由。 | <li> [Kubenet 和 Azure CNI 網路](/azure/aks/concepts-network#azure-virtual-networks) <li> [在 Azure Kubernetes Service (AKS) 中使用 kubenet 網路與您自己的 IP 位址範圍](/azure/aks/configure-kubenet) <li> [在 Azure Kubernetes Service (AKS) 中設定 Azure CNI 網路](/azure/aks/configure-azure-cni) <li> [AKS 叢集的安全網路設計](https://github.com/Azure/sg-aks-workshop/blob/master/cluster-design/NetworkDesign.md)|
> | **建立多個節點集區。** 若要支援具有不同計算或儲存體需求的應用程式，您可以選擇性地使用多個節點集區設定您的叢集。 例如，使用額外的節點集區來提供 Gpu 給計算密集型應用程式，或存取高效能 SSD 儲存體。 | <li> [&nbsp; &nbsp; 在 Azure Kubernetes Service 中建立和管理叢集的 &nbsp; 多個節點集區](/azure/aks/use-multiple-node-pools) |
> | **決定可用性需求。** Azure Kubernetes Service 的最少兩個 pod 可確保應用程式的高可用性，以防 pod 失敗或重新開機。 使用三個或多個 pod 來處理 pod 失敗期間的負載，並重新啟動。 針對叢集設定，至少需要可用性設定組或虛擬機器擴展集中的2個節點，才能符合服務等級協定99.95%。 使用至少三個 pod，以確保在節點失敗和重新開機期間進行 pod 排程。 若要為您的應用程式提供更高的可用性層級，您可以將叢集分散到可用性區域。 這些區域在特定區域內是以實體分隔的資料中心。 當叢集元件分散到多個區域時，您的叢集可容忍這些區域中的其中一個失敗。 即使整個資料中心發生中斷，您的應用程式和管理作業仍可供使用。 | <li> [建立 Azure Kubernetes Service (AKS) 使用可用性區域的叢集](/azure/aks/availability-zones) |

## <a name="go-to-production-and-apply-best-practices"></a>移至生產環境並套用最佳作法

當您準備應用程式以進行生產時，您應該執行一組最小的最佳作法。 在這個階段使用下列檢查清單。 您應該能夠回答下列問題：

> [!div class="checklist"]
>
> - 您是否能夠安心地重新部署叢集基礎結構？
> - 您是否已套用資源配額？

<!-- -->

> | 檢查清單  | 資源                                                                                                     |
> |------------------------------------------------------------------|-----------------------------------------------------------------|
> | **自動布建叢集。** 您可以使用基礎結構即程式碼，將基礎結構布建自動化，以在災難期間提供更多的復原能力，並視需要靈活地快速重新部署基礎結構。  | <li> [使用 Azure Kubernetes Service 和 Terraform 建立 Kubernetes 叢集](/azure/terraform/terraform-create-k8s-cluster-with-tf-and-aks)|
> | **使用 pod 中斷預算來規劃可用性。** 若要維護應用程式的可用性，請將 pod 中斷預算定義 (Pdb) ，以確保在硬體故障或叢集升級期間，叢集中有最少的 pod 數目。 | <li> [&nbsp; &nbsp; &nbsp; 使用 &nbsp; pod 中斷 &nbsp; 預算規劃可用性](/azure/aks/operator-best-practices-scheduler#plan-for-availability-using-pod-disruption-budgets)  |
> | **在命名空間上強制執行資源配額。** 在命名空間層級規劃及套用資源配額。 您可以設定計算資源、儲存體資源和物件計數的配額。| <li> [強制執行資源配額](/azure/aks/operator-best-practices-scheduler#enforce-resource-quotas)  |

## <a name="optimize-and-scale"></a>優化和調整

應用程式現在已在生產環境中，您要如何優化您的工作流程，並準備您的應用程式和小組進行調整？ 使用「優化」和「調整」檢查清單來準備。 您應該能夠回答下列問題：

> [!div class="checklist"]
>
> - 您是否有商務持續性和嚴重損壞修復的計畫？
> - 您的叢集是否可調整以符合應用程式需求？
> - 您是否能夠監視叢集和應用程式健康情況並收到警示？

<!-- -->

> | 檢查清單 | 資源 |
> |--|--|
> | **自動調整叢集以符合應用程式需求。** 為了滿足應用程式的需求，您可能需要使用叢集自動調整程式來調整自動執行工作負載的節點數目。 | <li> [設定 Kubernetes 叢集自動調整程式](/azure/aks/cluster-autoscaler) |
> | **規劃商務持續性和嚴重損壞修復。** 規劃多區域部署、建立儲存體遷移計畫，以及啟用容器映射的異地複寫。 | <li> [區域部署的最佳作法](/azure/aks/operator-best-practices-multi-region) <li> [Azure Container Registry 異地複寫](/azure/container-registry/container-registry-geo-replication) |
> | **大規模設定監視和疑難排解。** 在 Kubernetes 中設定應用程式的警示和監視。 瞭解預設設定、如何整合更先進的計量，以及如何新增您自己的自訂監視和警示來可靠地操作您的應用程式。 | <li> [開始使用 Kubernetes (影片的監視和警示) ](https://www.youtube.com/watch?v=W7aN_z-cyUw&list=PLLasX02E8BPCrIhFrc_ZiINhbRkYMKdPT&index=16) <li> [使用容器的 Azure 監視器設定警示](/azure/azure-monitor/insights/container-insights-overview) <li> [檢查 &nbsp; &nbsp; &nbsp; 主要元件的診斷記錄](/azure/aks/view-master-logs) <li> [Azure Kubernetes Service (AKS) 診斷](/azure/aks/concepts-diagnostics) |
