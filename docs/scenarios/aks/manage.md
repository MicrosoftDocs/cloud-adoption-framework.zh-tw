---
title: 管理新式容器解決方案
description: 描述案例對操作管理的影響
author: BrianBlanchard
ms.author: brblanch
ms.date: 03/01/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 90a99c1c7c817b6bb8e578515f1fae8b8d7ae341
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101793662"
---
# <a name="manage-modern-container-solutions-clusters"></a>管理新式容器解決方案叢集

[雲端採用架構提供的核心方法，可](../../manage/index.md)讓您以中立的方式定義雲端的作業管理流程。 其指導方針可協助您建立作業管理基準和其他特定層級的作業。 對於混合使用基礎結構即服務 (IaaS) 、平臺即服務 (PaaS) 和容器化工作負載的組織而言，本指南可能仍適用。 本文概述您需要整合到現有作業以準備進行容器管理的事項。 它也強調了將 Azure Kubernetes Service (AKS) 整合到您的容器管理原則中的優點。

## <a name="business-alignment-for-operations-management-needs"></a>營運管理需求的商務一致性

容器會移除多層基礎結構的相依性，進而改善作業管理功能。 若要瞭解這些作業的改進，您可能必須從商務一致性開始修改整體雲端管理原則。

若要建立適當的作業管理實務，您必須瞭解如何在雲端採用方案中使用容器，以及您想要從這項轉移到容器化工作負載的優點。

- 您是否會在雲端平臺中管理多個技術解決方案，例如容器、IaaS 和 PaaS？
- 集中式小組是否支援容器或 AKS 平臺的作業和管理？ 此責任轉移給個別的工作負載小組嗎？
- 集中式小組是否支援操作和管理每個容器或 pod 中執行的工作負載？ 此責任轉移給個別的工作負載小組嗎？
- 您使用的是任務關鍵性工作負載的容器嗎？
- 您是否只針對較不重要或公用工作負載使用容器來降低成本？
- 您個別工作負載的效能和可靠性有多重要？
- 您容器中的應用程式是否處於不變的狀態？ 您是否需要保存狀態以保護和復原容器中的工作負載？

這些基本問題將塑造如何以最佳方式將容器與 AKS 整合到您的作業管理原則中。

## <a name="operations-baseline"></a>作業基準

執行作業基準可讓您集中存取在雲端環境中操作及管理所有資產所需的工具。 如果您沒有非容器化資產的作業基準，您可以執行 [管理方法中所定義的作業基準](../../manage/azure-server-management/index.md)。

您的作業基準應該包含工具和設定，以提供可見度、監視、操作合規性、優化和保護/復原。

![作業管理基準](../../_images/manage/management-baseline.png)

上述文章中所述的作業基準不會提供對您的容器或 AKS 平臺的支援。 不過，它會提供可延伸以支援容器的工具基礎，例如 Azure 監視器、Azure 備份和其他工具。

如果您在雲端中大部分的組合都裝載在容器中，請考慮將下一節中的特殊平臺作業納入您的作業基準中。

## <a name="platform-operations"></a>平台作業

除非此實行是您組織的第一個或僅部署至雲端，否則您應該有作業基準。 本節將識別您可能想要包含的一些工具，以協助管理容器或 AKS 部署。

### <a name="inventory-and-visibility"></a>清查和可見性

監視容器和 AKS 叢集會使用您的作業基準中包含的工具、儀表板和警示。 不過，您可能需要進行更多設定，以將容器中的資料取得至 operations monitoring 工具，例如 [適用于容器的 Azure 監視器](/azure/azure-monitor/containers/container-insights-overview?bc=/azure/cloud-adoption-framework/_bread/toc.json toc=/azure/cloud-adoption-framework/toc.json)。 請參閱 [適用于容器的 Azure 監視器總覽](/azure/azure-monitor/containers/container-insights-overview?bc=/azure/cloud-adoption-framework/_bread/toc.json toc=/azure/cloud-adoption-framework/toc.json) ，以收集將容器和 AKS platform 作業新增至您的作業基準所需的資料。

設定 Azure 監視器以收集容器上的資料之後，您可以在集中式管理程式中監視下欄區域：

- 識別在不同區域中執行的叢集，最好系結至服務樹狀結構專案，並識別這些叢集上的重要事實
  - 識別叢集節點集區、網路和這些叢集的儲存體拓撲
  - 識別 AKS 版本和節點映射版本分層。
- 識別叢集節點資源使用量 (進程、記憶體和儲存體) 
- 識別節點上正在執行的容器，以及其對節點使用率的貢獻
- 瞭解叢集在平均和最高負載下的行為。 此知識可協助您識別所需的容量，並判斷叢集可承受的負載上限。
- 設定警示，以主動通知您或記錄節點或容器上的 CPU 和記憶體使用率超過閾值，或在叢集的基礎結構或節點健全狀況匯總中發生健全狀況狀態變更時。
- 使用 [查詢](/azure/azure-monitor/containers/container-insights-log-search) 來建立一組通用的警示、儀表板，以及詳細的執行詳細分析

這項資料也會提供容器化平臺上執行之工作負載的詳細資訊，以支援工作負載作業小組：

- 檢閱在和支援 Pod 的標準程序無關之主機上執行的工作負載的資源使用率。
- 與 [Prometheus](/azure/azure-monitor/containers/container-insights-prometheus-integration) 整合以查看應用程式計量。
- 監視部署到 Azure Stack 上的 AKS Engine 內部部署和 AKS Engine 的容器工作負載。
- 監視部署至 Azure Red Hat OpenShift 的容器工作負載。
- 監視部署到已啟用 Azure Arc 的容器工作負載 Kubernetes (preview) 。

### <a name="operations-compliance"></a>作業合規性

修補、調整和調整大小會在容器化環境中有幾個不同的層級進行。 根據您所需的作業方法，操作員可能會位於許多不同的小組。 為了維持作業合規性，操作員將會監視使用量、調整資產大小以平衡效能和成本，以及修補基礎系統以將風險和設定漂移降至最低。 這些都是中央 IT 組織通常會在 IaaS 和 PaaS 解決方案的作業基準中提供的工作。

在 Azure 的叢集環境中，這些工作會在多個層級上執行： AKS 叢集、節點映射和節點作業系統。 所有這些作業工作都將相依于叢集或個別節點集區上執行之工作負載的瞭解和工作關聯性。 下列語句將有助於評估您要做什麼，以及您想要如何操作容器環境。

- 如果 AKS 叢集、節點映射或節點 OS 的大小調整和修補是作為應用程式部署管線的一部分來提供，或是相依于應用程式架構/設定，則最好將作業合規性轉移給工作負載小組，以進行細微控制。 因為工作負載通常會相依于協調流程功能，所以這是最常見的模式，因為未預期的 AKS 版本變更或節點映射變更可能會對工作負載或其執行時間工具造成災難性的變化。
- 針對較不常見的集中式叢集，支援工作負載的組合和各種應用程式，集中式作業小組可能仍會負責操作合規性工作，下列指南將有助於在您的叢集中傳遞這些工作。 定期執行這些工作，以 instills 平臺特定的作業。 中央操作方法有明顯的風險，並且在預先生產環境中仔細測試升級、清除和遵守排程維護，以及不符合規範工作負載的應變計劃，全都都必須準備就緒。 其中一個不良的升級可能是單一失敗點，同樣地，一個無法升級的工作負載可能會導致叢集無法支援。 規劃和管理具有審慎考慮的多租使用者叢集。

針對這兩種叢集類型，請遵循下列所示的升級、節點映射和節點 OS 更新的指引：

- [升級 AKS 叢集](/azure/aks/upgrade-cluster?bc=/azure/cloud-adoption-framework/_bread/toc.json&toc=/azure/cloud-adoption-framework/toc.json)
- [升級節點映像](/azure/aks/node-image-upgrade?bc=/azure/cloud-adoption-framework/_bread/toc.json&toc=/azure/cloud-adoption-framework/toc.json)
- [流程節點 OS 更新](/azure/aks/node-updates-kured?bc=/azure/cloud-adoption-framework/_bread/toc.json&toc=/azure/cloud-adoption-framework/toc.json)
- [修補和升級指引](/azure/architecture/operator-guides/aks/aks-upgrade-practices?bc=/azure/cloud-adoption-framework/_bread/toc.json&toc=/azure/cloud-adoption-framework/toc.json)

### <a name="protect-and-recover"></a>保護和復原

AKS 節點本質上是暫時性的，因此不會以個別還原的方式進行備份。 從事件中復原可能牽涉到將工作負載重新部署到新的節點集區或整個新叢集（視事件範圍而定）。

- 選擇 [在您的叢集中新增執行時間 SLA](/azure/aks/uptime-sla)。
- 針對較高的 Sla，您可能也想要考慮 [多區域 BCDR 最佳做法](/azure/aks/operator-best-practices-multi-region?bc=/azure/cloud-adoption-framework/_bread/toc.json&toc=/azure/cloud-adoption-framework/toc.json) ，以提供額外的保護。
- 因為叢集不應包含狀態，所以會使用現有的作業基準指引來處理外部狀態還原。 如果您將狀態帶到您的叢集中，請確保您遵循 [運算子的儲存體最佳做法](/azure/aks/operator-best-practices-storage?bc=/azure/cloud-adoption-framework/_bread/toc.json&toc=/azure/cloud-adoption-framework/toc.json)，並有策略可針對指定的工作負載 [還原此資料](/azure/aks/operator-best-practices-storage#secure-and-back-up-your-data) 。 使用 [Velero](https://github.com/vmware-tanzu/velero) 之類的工具，就是擴充您的作業基準的平臺特定作業範例。
  - 如果您的應用程式組合不一致地套用狀態，建議中央營運團隊不會嘗試同時維護這兩個解決方案。 相反地，請將所有容器的所需狀態工具鏈標準化，但是將替代復原解決方案的責任轉移給工作負載作業團隊。 這種方法可讓開發人員自由設計，保持較低的中央成本，並提供成本降低的獎勵，讓工作負載小組符合標準。

## <a name="workload-operations"></a>工作負載作業

上述平臺操作一節說明管理 AKS 叢集時的常見交談。 Kubernetes 叢集是要集中管理的技術平臺嗎？ 它們是工作負載工具，應該由擁有每個工作負載的團隊進行管理？ 針對不同的組織，該問題是不同的。 大部分組織的情況都是容器和 AKS 的設計目的，是為了讓工作負載小組能更靈活地操作每個工作負載，並為這些工作負載提供特定功能，以在其架構中用來受益于應用程式的擁有者和客戶。

工作負載作業可以根據您現有的作業基準和平臺特定作業來建立。 您也可以使用完全非集中式的工作負載作業，安全地操作 AKS 叢集。 無論是哪一種情況，當您需要提高作業以專注于特定工作負載的特定結果時，您可以使用 [Azure Well-Architected Framework](/azure/architecture/framework/) 和 [Microsoft Azure Well-Architected 評論](https://aka.ms/architecture/review) ，以非常明確地瞭解您的工作負載所使用的操作程式和工具類型。

## <a name="next-step-your-next-migration-iteration"></a>下一步：您的下一個遷移反復專案

當新式容器遷移完成之後，雲端採用小組就可以開始下一個案例特定的遷移。 或者，如果有其他要遷移的平臺，則可以再次使用此文章系列來引導您下一次的新式容器遷移或部署。

- [新式容器的策略](./strategy.md)
- [規劃新式容器](./plan.md)
- [檢查您的環境或 Azure 登陸區域](./ready.md)
- [將工作負載遷移至新式容器](./migrate.md)
- [使用新式容器解決方案進行創新](/azure/architecture/reference-architectures/containers/aks-start-here?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)
- [管理新式容器解決方案](./govern.md)
- [管理新式容器解決方案](./manage.md)
