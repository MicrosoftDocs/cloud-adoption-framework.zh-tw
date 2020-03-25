---
title: 軟體定義網路：僅限 PaaS
description: 瞭解雲端中軟體定義網路功能的僅限 PaaS 架構模型的優點和限制。
author: rotycenh
ms.author: abuck
ms.date: 02/11/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: decision-guide
ms.custom: governance
ms.openlocfilehash: fd1ffbbc6ad871e2302d6e582caec7977d20a4df
ms.sourcegitcommit: 25cd1b3f218d0644f911737a6d5fd259461b2458
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/24/2020
ms.locfileid: "80225678"
---
# <a name="software-defined-networking-paas-only"></a>軟體定義網路：僅限 PaaS

當您實作平台即服務 (PaaS) 資源時，部署程序會利用對於該網路的有限控制項，自動建立假設的基礎網路，包括負載平衡、連接埠封鎖，以及連線至其他 PaaS 服務。

在 Azure 中，可以將數個 PaaS 資源類型[部署到](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)或[連線到](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview)虛擬網路，讓這些資源能與您現有的虛擬網路基礎結構整合。 其他服務（例如[App Service 環境](https://docs.microsoft.com/azure/app-service/environment/intro)、 [Azure Kubernetes Service （AKS）](https://docs.microsoft.com/azure/aks/intro-kubernetes)和[Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview) ）必須部署在虛擬網路內。 不過，在許多情況下，僅限 PaaS 的網路架構，僅依賴 PaaS 資源所提供的預設原生網路功能，足以符合工作負載的連線能力和流量管理需求。

如果您考慮使用僅限 PaaS 的網路架構，請務必驗證符合您需求的必要假設。

## <a name="paas-only-assumptions"></a>僅限 PaaS 的假設

部署僅限 PaaS 網路架構的假設如下：

- 正在部署的應用程式是獨立應用程式，或只相依于不需要虛擬網路的其他 PaaS 資源。
- 您的 IT 作業小組可以更新其工具、訓練和流程，以支援獨立 PaaS 應用程式的管理、設定和部署。
- PaaS 應用程式不屬於將要包含 IaaS 資源的更廣泛雲端移轉工作。

這些假設是對應於部署僅限 PaaS 網路的最小限定詞。 雖然此方法可能符合單一應用程式部署的需求，但每個雲端採用小組都應該考慮這些長期問題：

- 此部署會擴大規模而要求存取其他非 PaaS 資源嗎？
- 所規劃的其他 PaaS 部署超過目前的解決方案嗎？
- 組織是否有其他未來雲端移轉的計劃？

這些問題的答案並不會阻止小組選擇 PaaS 僅限選項，但是應在做出最後決定前加以考量。
