---
title: 遷移新式容器的工作負載
description: 藉由將多個 web 應用程式遷移至容器解決方案，減少雲端平臺的相依性並可能降低基礎結構的耗用量
author: BrianBlanchard
ms.author: brblanch
ms.date: 03/01/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 810a62968a6703e110dfad04a9f6c0256b2109cf
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101793664"
---
# <a name="migrate-workloads-to-modern-containers"></a>將工作負載遷移至新式容器

大部分新式容器選項都需要重新架構或重新部署應用程式。 但 Azure Kubernetes Service (AKS) 的協調流程功能，可讓 AKS 解決方案整合至標準的遷移程式。

將現有工作負載從內部部署資料中心遷移至 Azure 中的 Kubernetes 叢集，會有明顯且不斷成長的趨勢。 這種方法有可能會降低遷移後基礎結構的使用量。 更重要的是，遷移至容器可提供組合中更大的可攜性，讓工作負載更容易在公用和私人雲端之間移動。 當組織有龐大的 web 應用程式集合時，最常遇到此趨勢。

## <a name="one-migrate-approach"></a>一個遷移方法

您可以遷移至 Azure Kubernetes Service (AKS) 來加速雲端中的容器，作為 [雲端採用架構的一個遷移案例](../index.md)的一部分。 一般而言，遷移至 Azure 會使用 Azure 遷移和合作夥伴工具來評估工作負載、遷移工作負載，以及將工作負載發行至雲端。 您可以將這三個步驟的程式套用至 AKS 遷移，不過，您可能需要一些其他工具來協助進行遷移步驟。

### <a name="assess-workloads"></a>評估工作負載

您需要清查工作負載及其目前的容器化狀態。 在容器內操作工作負載的功能和效能時，無法遷移工作負載。 與應用程式擁有者合作，以配置時間來執行容器化、驗證結果，以及建立工作的影像建立管線。 請注意獨特的相依性，例如 Windows 特定的需求 (例如 Gmsa) 、本機檔案系統使用方式、快取執行詳細資料、單一執行和相依性（例如資料庫）。

雖然集中式小組可以將容器化工作導向整個組織，但請考慮更多專案管理功能和技術需求收集和監督程式，應用程式擁有者必須高度參與此程式。

### <a name="migrate-containers-and-workloads"></a>遷移容器和工作負載

在遷移時，請確定您的目標 Kubernetes 版本位於支援的 AKS 視窗內。 如果使用較舊的版本，它可能不在支援的範圍內，而且需要 AKS 支援升級版本。 如需詳細資訊，請參閱 [AKS 支援的 Kubernetes 版本](/azure/aks/supported-kubernetes-versions)。

如同任何遷移，請決定要比較容易的維護時段，並清楚瞭解如何進行遷移的所有感興趣的專案關係人。 在適當的情況下追蹤和儀表板的遷移。 如果無法協調下一階段的遷移，則在沒有停機時間的情況下，允許額外的規劃、成本和複雜性。 如果發現不需要停機時需要停機，請將該變更傳達給您的專案關係人。 對該變更執行影響分析，以確保會記載風險並據此進行同意。

所有遷移 (甚至是) 的停機時間，都可能需要修改現有的應用程式，並提供支援遷移的額外彈性。 確保應用程式小組完全參與規劃工作負載遷移的最早速度。 例如，您可能需要在目前的工作負載中部署其他 DNS、連接字串和設定切換功能，才能完成遷移。

目前，您必須使用數個開放原始碼工具的其中一個，才能完成將容器和工作負載複寫至 Azure 的動作：

如果您是從現有的 Kubernetes 平臺 (AKS 引擎、ACS 或其他 Kubernetes 執行) ，您可能會考慮使用一些開放原始碼工具來協助進行遷移。 在這些情況下，您已經有在 Kubernetes 中運作的工作負載，而 AKS 中的重新裝載通常更簡單。 執行任何遷移之前，請先驗證 AKS 中的所有功能。

- [Velero](https://velero.io)
- [Azure kube CLI 擴充功能](https://github.com/yaron2/azure-kube-cli)
- [Reshifter](https://github.com/mhausenblas/reshifter)
- 從 [AKS 引擎](/azure-stack/user/azure-stack-kubernetes-aks-engine-overview) 遷移至 AKS
- 從 [Azure container service (acs) ](https://azure.microsoft.com/updates/azure-container-service-will-retire-on-january-31-2020/) 遷移至 AKS
- 將現有資源移至不同區域

在遷移時，請確定您的目標 Kubernetes 版本位於支援的 AKS 視窗內。 如果使用較舊的版本，它可能不在支援的範圍內，而且需要 AKS 支援升級版本。 如需詳細資訊，請參閱 [AKS 支援的 Kubernetes 版本](/azure/aks/supported-kubernetes-versions)。 可能的話，請一律嘗試遷移至相同版本的 Kubernetes。 這表示您可以在現有系統中進行就地升級，或根據您的優先順序規劃遷移後升級。

## <a name="next-step-innovate-using-modern-container-solutions"></a>下一步：使用新式容器解決方案進行創新

下列文章將帶您前往雲端採用旅程中特定點的指引，並協助您在雲端採用案例中成功。

- [使用新式容器解決方案進行創新](/azure/architecture/reference-architectures/containers/aks-start-here?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)
- [管理新式容器解決方案](./govern.md)
- [管理新式容器解決方案](./manage.md)
