---
title: 規劃新式容器
description: 描述案例對規劃的影響
author: BrianBlanchard
ms.author: brblanch
ms.date: 03/01/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: strategy
ms.openlocfilehash: 03e946b343661d76c7203dc1b26d93e8ff3729a6
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794529"
---
# <a name="plan-for-modern-containers"></a>規劃新式容器

[雲端採用架構的計畫方法](../../plan/index.md) 可協助建立整體雲端採用方案，以引導參與雲端式數位轉型的程式和小組。 本指南提供的範本可用來建立您的待處理專案，並計畫在您的小組中建立必要的技能，這些都是根據您在雲端中嘗試執行的作業而定。

標準方案方法著重于 [合理化數位資產的五個 rs](../../digital-estate/5-rs-of-rationalization.md)。  (鬆散轉譯：雲端採用最常見的路徑。 ) 更明確地說，這些計畫是針對最常見的案例量身訂做：重新裝載、重新架構或 rebuild。

## <a name="initial-containers-considerations"></a>初始容器考慮

將您的工作負載封裝在容器中，是必須排程和處理的第一個工作主體。 第二個是規劃這些容器的裝載。

### <a name="containers-without-orchestration"></a>沒有協調流程的容器

有些工作負載是高度獨立的，不一定會受益于大型平臺（例如 Kubernetes）隨附的 advanced 控制項和基礎結構需求。 這是因為您的工作負載是容器化的，並不表示它必須部署至 Kubernetes。 Azure 提供各種不同的解決方案，可在您的組合中執行工作負載，而不需要 AKS 所需的管理和基礎結構層級。 下列解決方案會遵循此方法來規劃：

- [App Service](/azure/app-service/)
- [Azure Functions](/azure/azure-functions/functions-overview)
- [Azure 容器執行個體](/azure/container-instances/container-instances-overview)
- [Batch](/azure/batch/batch-technical-overview)

針對不希望複雜度成長的工作負載，請考慮使用較輕量的解決方案，並配合上述解決方案的用途和限制。

### <a name="containers-with-orchestration"></a>具有協調流程的容器

針對無法在完全受控的 PaaS 平臺中執行，而且必須在基礎結構層級的控制上轉送的工作負載，想要使用像是容器協調器所提供的 advanced 部署功能，或預期會隨著模組化的複雜性成長，請轉至 Azure Kubernetes Service (AKS) 。 AKS 可解決這兩個容器裝載，但也提供廣泛的架構、SRE、安全性、部署、監視及基礎結構選項。

平臺的功能組需要在叢集操作員層級和工作負載層級上學習平臺。 將營運團隊、架構小組和工作負載工程團隊的教育因素納入遷移時程表。 此外，由於 AKS 是一個平臺，請確保工作負載小組瞭解此平臺內的職責區隔與目前的裝載相片順序之間的區分。 它可能在某些方面很類似，但可能會在其他情況下新穎。

針對非常特製化的工作負載或特定的組織需求，Azure 在容器協調流程空間中提供兩個其他主要平臺。

- Azure Red Hat OpenShift
- Azure Service Fabric

如果有理由要探索替代方案，請確定已配置時間來瞭解所有平臺選項的優點和取捨。 Azure 的預設解決方案是 AKS，本檔假設 AKS 是選擇的技術。

## <a name="digital-estate"></a>數位資產

在規劃您的數位資產時，您會想要 [收集清查資料](../../digital-estate/inventory.md) 並 [合理化資產](../../digital-estate/rationalize.md)。 在容器採用方案中，所有資產（例如 Vm、資料和應用程式）都是依支援的工作負載分組。 完成群組和基本合理化之後，您就可以評估這些工作負載，以判斷套件/重新裝載或重新架構選項。

在數位資產評估中，您也需要根據容器持續性來評估資料的計畫。 容器可在持續性或非持續性狀態中執行。 如果發生失敗，持續性狀態容器將會保留資料。 非持續性容器不會維護資料。 如果您選擇的非持續性設定（適用于成熟的 DevOps 團隊）很常見，您將需要考慮將工作負載資料裝載在持續性環境中，例如 Azure SQL Database。

這些考慮將會清楚地說明在雲端中完成採用容器化工作負載所需的動作。

## <a name="modern-container-adoption-plan"></a>新式容器採用方案

標準 [雲端採用方案範本](../../plan/template.md) 適用于一般雲端採用工作所需的工作類型。 但是，您必須將工作新增至您的方案，以將工作負載封裝到容器布建的容器和協調流程中。

## <a name="modern-container-readiness-plan"></a>新式容器就緒方案

除了雲端採用技能方案之外，雲端採用小組可能還需要先開發容器和 Kubernetes 的相關技能，再執行您的計畫：

- [瞭解 Kubernetes 的基本概念](https://aka.ms/LearnAKS)
- [瞭解容器](https://azure.microsoft.com/product-categories/containers/)
- [熟悉 Kubernetes 的最佳作法](https://aka.ms/aks/bestpractices)

請確定已配置工作負載小組的時間，以記錄和執行遷移計畫。 現有的應用程式或外部系統 (相依于此工作負載的相依性和系統) 可能需要以額外的彈性來修改，以支援遷移工作。 這在進入生產階段前和生產環境都是如此。

## <a name="next-step-review-your-environment-or-azure-landing-zone"></a>下一步：檢查您的環境或 Azure 登陸區域

下列文章清單會帶您前往雲端採用旅程中特定點的指導方針，以協助您成功進行雲端採用案例。

- [檢閱環境或 Azure 登陸區域](./ready.md)
- [將工作負載遷移至新式容器](./migrate.md)
- [使用新式容器解決方案進行創新](/azure/architecture/reference-architectures/containers/aks-start-here?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)
- [管理新式容器解決方案](./govern.md)
- [管理新式容器解決方案](./manage.md)
- [請參閱容器和計算決策樹](/azure/architecture/guide/technology-choices/compute-decision-tree)
