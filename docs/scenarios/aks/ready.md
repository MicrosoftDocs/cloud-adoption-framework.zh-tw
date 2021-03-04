---
title: 新式容器的 Azure 登陸區域審核
description: 描述案例對 Azure 登陸區域設計的影響
author: BrianBlanchard
ms.author: brblanch
ms.date: 03/01/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: f0bf2bdb2cc00ec32f6b56942335343a6b94fa73
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794540"
---
# <a name="azure-landing-zone-review-for-modern-containers"></a>新式容器的 Azure 登陸區域審核

[雲端採用架構中的準備方法](../../ready/index.md)，會引導您使用[azure 登陸區域](../../ready/landing-zone/index.md)建立所有 Azure 環境。 Azure 登陸區域提供許多建立在一組[常見設計區域](../../ready/landing-zone/design-areas.md)周圍的[實作為選項](../../ready/landing-zone/implementation-options.md)。

使用 Azure 登陸區域，您可以從小規模的執行開始，並隨著時間擴充。 針對更複雜的環境，您可以從企業規模的執行選項開始。 無論您選擇哪一個實選項，都必須評估任何要用於新式容器解決方案的登陸區域。

## <a name="environmental-considerations-for-orchestrated-containers"></a>協調容器的環境考慮

如果您尚未選取 Azure 登陸區域的執行方法，請參閱 [azure 登陸](../../ready/landing-zone/index.md) 區域文章系列。 然後，請檢查該登陸區域選項最適合最適合新式容器案例的方式。

**開始-小型選項：** 透過 Azure Kubernetes Service (AKS) 的容器協調流程需要進行一些環境設定。 適用于 [azure 登陸區域的 Azure Kubernetes Service (AKS) 叢集的基準架構](/azure/architecture/reference-architectures/containers/aks/secure-baseline-aks?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json) 會提供網路、計算、身分識別、安全性、BCDR 和作業的相關考慮。

**企業規模選項：** 適用于 AKS 的企業規模建築集包含參考實行，可部署企業規模登陸區域以支援您的 AKS 平臺。 結構集檔中包含一系列的最佳作法，可用來評估任何 AKS 環境的生產環境是否就緒。 即使您選取開始小型登陸區域，也適用這些考慮。 請參閱下列文章來評估您的 AKS 環境：

- [身分識別和存取管理](eslz-identity-and-access-management.md)
- [網路拓樸和連線能力](eslz-network-topology-and-connectivity.md)
- [管理與監視](eslz-management-and-monitoring.md)
- [商務持續性和災害復原](eslz-business-continuity-and-disaster-recovery.md)
- [安全性治理和合規性](eslz-security-governance-and-compliance.md)
- [平台自動化和 DevOps](eslz-platform-automation-and-devops.md)

上述兩個選項之間的主要 deference，是以 Azure 資源、訂用帳戶拓撲，以及用於治理的 Azure 原則的方式來表達和實行職責的分隔。 瞭解您組織對集中式與非集中式作業的規劃，以及最適合您組織工作負載的方案。 這兩種模型都可以 flexed，以提供您的組織和工作負載所需的確切經驗，但您會想要從最符合您定義之策略的一開始著手。 確定所有工作負載小組都瞭解所有 IT 群組和成員所需的作業模型和責任。

## <a name="environmental-considerations-for-non-orchestrated-container-solutions"></a>非協調容器解決方案的環境考慮

下列容器服務會以平臺即服務解決方案的形式執行，其需要較少的環境設定。 但是，降低的設定需求會減少容器協調流程的控制權，以及解決方案特定的設定，以將工作負載整合到其他資產，例如 Vm 或其他容器。 這些非協調的解決方案傾向于使其成為工作負載偏差作業策略。

請參閱以下每個產品檔連結中的概念和操作指南，以針對非協調的容器類型評估不同類型的環境設定：

- [App Service](/azure/app-service/)
- [Azure Functions](/azure/azure-functions/functions-overview)
- [容器實例](/azure/container-instances/container-instances-overview)
- [Batch](/azure/batch/batch-technical-overview)

## <a name="next-step-migrate-workload-to-modern-containers"></a>下一步：將工作負載遷移至新式容器

下列文章清單會帶您前往雲端採用旅程中特定點的指導方針，以協助您成功進行雲端採用案例。

- [將工作負載遷移至新式容器](./migrate.md)
- [使用新式容器解決方案進行創新](/azure/architecture/reference-architectures/containers/aks-start-here?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)
- [管理新式容器解決方案](./govern.md)
- [管理新式容器解決方案](./manage.md)
