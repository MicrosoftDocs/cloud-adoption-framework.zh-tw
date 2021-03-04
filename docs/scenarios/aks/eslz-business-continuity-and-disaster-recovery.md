---
title: 適用于 Azure Kubernetes Service 的企業規模商務持續性和嚴重損壞修復
description: 描述此企業規模案例如何改善 Azure Kubernetes Service 的商務持續性和嚴重損壞修復。
author: JeffMitchell
ms.author: jemitche
ms.date: 02/18/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: fa72f05e479ff56a7163a747ea5cca49c08e7199
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794357"
---
# <a name="enterprise-scale-business-continuity-and-disaster-recovery-for-azure-kubernetes-service"></a>適用于 Azure Kubernetes Service 的企業規模商務持續性和嚴重損壞修復

您的組織必須設計適合的 Azure Kubernetes Service (AKS) 平台層級的功能，以符合其特定需求。 這些應用程式服務的復原時間目標 (RTO) 和復原點目標 (RPO) 相關的需求。 針對 AKS 嚴重損壞修復，有多項考慮需要解決。 您的第一個步驟是為您的基礎結構和應用程式定義服務等級協定 (SLA) 。 深入瞭解 [Azure Kubernetes Service (AKS) 的 SLA ](https://azure.microsoft.com/support/legal/sla/kubernetes-service/v1_1/)。 如需每月執行時間計算的詳細資訊，請參閱 **SLA 詳細資料** 一節。

## <a name="design-considerations"></a>設計考量

請考慮下列因素：

- AKS 叢集應使用節點集區中的多個節點，以提供應用程式的最小可用性層級。

- 設定 [pod 要求和限制](/azure/aks/developer-best-practices-resource-management#define-pod-resource-requests-and-limits)。 設定這些限制可讓 Kubernetes：

  - 有效率地將 CPU 和記憶體資源提供給 pod。

  - 在節點上擁有較高的容器密度。

  限制也可提高可靠性，因為較佳的硬體使用方式會降低成本。

- [可用性區域](/azure/aks/availability-zones)或可用性設定組的 AKS 適用性。

  - 選擇支援可用性區域的地區。

  - 可用性區域只能在建立節點集區時進行設定，之後無法再變更。 多重區域支援僅適用于節點集區。

  - 如需完整的區域權益，所有服務相依性也必須支援區域。 如果相依的服務不支援區域，可能是因為區域失敗而導致服務失敗。

  - 若要提高可用性區域的可用性，可在不同的配對區域中執行多個 AKS 叢集。 如果 Azure 資源支援異地冗余，請提供重複服務將會有其次要區域的位置。

- 您應該留意 AKS 中的嚴重損壞 [修復的指導方針](/azure/aks/operator-best-practices-multi-region) 。 然後考慮是否要將它們套用至您用於 Azure Dev Spaces 的 AKS 叢集。

- 一致地為應用程式和資料建立備份。

  - 您可以有效率地複寫非具狀態的服務。

  - 如果您需要在叢集中儲存狀態 (不建議) ，請務必在配對的區域中經常備份資料。

- 叢集更新與維護。

  - 一律讓您的叢集保持在最新狀態。

  - 請留意發行和淘汰程式。

  - 事先規劃您的更新與維護。

- 發生容錯移轉時的網路連線能力。

  - 根據您的需求，選擇可根據您的需求，將流量分散到多個區域或區域的流量路由器。 此架構會部署 [Azure 負載平衡器](/azure/load-balancer/load-balancer-overview) ，因為它可以跨區域散發非 web 流量。

  - 如果您需要將流量分散到不同區域，請考慮使用 [Azure Front 大門](/azure/frontdoor/front-door-overview)。

- 規劃和未計畫的容錯移轉。

  - 設定每個 Azure 服務時，請選擇支援損毀修復的功能。 例如，在此架構中，請啟用 [Azure Container Registry](/azure/container-registry/container-registry-intro) 以進行異地複寫。 如果區域停止運作，您仍然可以從複寫的區域提取映射。

- 維護工程 DevOps 功能以達成服務等級目標。

- 判斷您是否需要 Kubernetes API 伺服器端點的 [財務支援 SLA](/azure/aks/uptime-sla) 。

## <a name="design-recommendations"></a>設計建議

以下是您設計的最佳作法：

- 將三個節點用於系統節點集區。 若為使用者節點集區，請從不小於兩個節點開始。 如果您需要更高的可用性，請設定更多節點。

- 將您的應用程式放在個別的節點集區中，以將其與系統服務隔離。 如此一來，Kubernetes 服務就會在專用節點上執行，而不會與其他服務競爭。 使用 [標記、標籤和污點](/azure/aks/use-multiple-node-pools#specify-a-taint-label-or-tag-for-a-node-pool) 來識別節點集區，以排程您的工作負載。

- 叢集的一般維護（例如進行即時更新）對於可靠性而言很重要。 在 [AKS 上留意受支援的 Kubernetes 版本](/azure/aks/supported-kubernetes-versions) ，並事先規劃您的更新。 此外，建議透過探查來監視 pod 的健康情況。

- 盡可能 [移除容器內的服務狀態](/azure/aks/operator-best-practices-multi-region#remove-service-state-from-inside-containers)。 相反地，請使用支援多區域複寫的 Azure 平臺即服務 (PaaS) 。

- 確定 pod 資源。 強烈建議部署指定 pod 資源需求。 然後，排程器可以適當地排程 pod。 當 pod 未排程時，可靠性 depreciates 會大幅提高。

- 在部署中設定多個複本，以處理像是硬體故障的中斷情形。 針對更新和升級之類的規劃事件，中斷預算可以確保所需的 pod 複本數目，以處理預期的應用程式負載。

- 您的應用程式可能會使用 [Azure 儲存體來儲存](/azure/aks/operator-best-practices-multi-region#create-a-storage-migration-plan) 其資料。 因為您的應用程式會分散到不同區域中的多個 AKS 叢集，所以您需要將儲存體保持同步。 以下是複寫儲存體的兩個常見方式：

  - 以基礎結構為基礎的非同步複寫

  - 以應用程式為基礎的非同步複寫

- 預估 [pod 限制](/azure/aks/quotas-skus-regions#service-quotas-and-limits)。 測試及建立基準。 以相等的要求和限制值開始。 然後，逐漸調整這些值，直到您已建立可能會在叢集中造成不穩定的閾值為止。 您可以在部署資訊清單中指定 Pod 限制。

  內建功能提供在服務架構中處理失敗與中斷之複雜工作的解決方案。 這些設定有助於簡化設計和部署自動化。 當組織已定義 SLA、RTO 和 RPO 的標準時，可使用內建的服務來 Kubernetes 和 Azure，以達成其商務目標。

- 設定 [pod 中斷預算](/azure/aks/operator-best-practices-scheduler#plan-for-availability-using-pod-disruption-budgets)。 這項設定會檢查部署中有多少複本可在更新或升級事件期間停止執行。

- 在服務命名空間上[強制執行資源配額](/azure/aks/operator-best-practices-scheduler#enforce-resource-quotas)。 命名空間上的資源配額可確保在部署上正確設定 pod 要求和限制。

  - 在叢集層級設定資源配額可能會在部署沒有適當要求和限制的夥伴服務時發生問題。

- 定期執行最新版的[ `kube-advisor` 開放原始碼工具](/azure/aks/operator-best-practices-scheduler#regularly-check-for-cluster-issues-with-kube-advisor)，以偵測叢集中的問題。

- 將您的容器映射儲存在 [Azure Container Registry](/azure/aks/operator-best-practices-multi-region#enable-geo-replication-for-container-images) 中，並將登錄異地複寫到每個 AKS 區域。

- AKS 可以用來做為免費服務，但該階層不會提供有財務支援的 SLA。 若要取得 SLA，您必須在您購買的產品上新增執行時間 SLA。 建議所有生產叢集使用此選項。 針對預先生產叢集，保留沒有此選項的叢集。 當 Kubernetes API 伺服器 SLA 與可用性區域結合時，會增加到99.95%。 您的節點集區和其他資源會在其本身的 SLA 中涵蓋。

- 使用多個區域和對等互連位置進行 [ExpressRoute](/azure/expressroute/expressroute-introduction) 連線。

  如果發生影響 Azure 區域或對等提供者位置的中斷情形，則重複的混合式網路架構可協助確保不中斷的跨單位連線能力。

- 具有 [全域虛擬網路對等](/azure/aks/operator-best-practices-multi-region#interconnect-regions-with-global-virtual-network-peering)互連的互連區域。 如果叢集需要彼此通訊，可以透過虛擬網路對等互連來連線兩個虛擬網路。 這項技術會將虛擬網路彼此互連，以提供跨 Microsoft 骨幹網路的高頻寬，甚至在不同的地理區域。

- 使用 [分割的 TCP 型任意傳播通訊協定](/azure/aks/operator-best-practices-multi-region#application-routing-with-azure-front-door-service)，Azure Front 可確保您的終端使用者能立即連接到最接近的 Front 大門狀態。 Azure Front 的其他功能包括：

  - TLS 終止

  - 自訂網域

  - Web 應用程式防火牆

  - URL 重寫

  - 工作階段親和性

  檢查應用程式流量的需求，以瞭解哪個解決方案最適合。
