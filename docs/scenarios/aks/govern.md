---
title: 管理新式容器解決方案
description: 將治理實務延伸至新式容器實例
author: BrianBlanchard
ms.author: brblanch
ms.date: 03/01/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.openlocfilehash: a764ac7671eeeea7430b20f48cff223fba554845
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794912"
---
<!-- docutune:ignore "public container registry" -->

# <a name="govern-modern-container-solutions"></a>管理新式容器解決方案

[雲端採用架構提供了一種](../../govern/index.md)系統化的方法，可讓您以有系統的方式大幅改善雲端組合的治理。 本文會示範如何擴充治理方法，以 Kubernetes 部署至 Azure 或其他公用或私人雲端的叢集。

## <a name="initial-governance-foundation"></a>初始治理基礎

治理從 [最初稱為治理 MVP 的初始治理基礎](../../govern/initial-foundation.md)開始。 此基礎會部署在您的雲端環境中提供治理所需的基本 Azure 產品。

初始治理基礎著重于治理的下列層面：

- 基本的混合式網路和連線能力。
- Azure 角色型存取控制 (RBAC) 以進行身分識別和存取控制。
- 針對一致識別資源的命名和標記標準。
- 使用資源群組、訂用帳戶和管理群組來組織資源。
- 強制執行治理原則的 azure 原則和 Azure 藍圖。

初始治理基礎的每一項功能都可用於管理新式容器解決方案實例。 但首先，您必須將幾個元件新增至初始基礎，以將 [Azure 原則套用至您的容器](/azure/governance/policy/concepts/policy-for-kubernetes?bc=/azure/cloud-adoption-framework/_bread/toc.json&toc=/azure/cloud-adoption-framework/toc.json)。 設定之後，您就可以使用 Azure 原則和初始治理基礎來管理下列類型的容器：

- [Azure Kubernetes Service (AKS)](/azure/aks/intro-kubernetes?bc=/azure/cloud-adoption-framework/_bread/toc.json&toc=/azure/cloud-adoption-framework/toc.json)
- [已啟用 Azure Arc 的 Kubernetes](/azure/azure-arc/kubernetes/overview?bc=/azure/cloud-adoption-framework/_bread/toc.json&toc=/azure/cloud-adoption-framework/toc.json)
- [AKS 引擎](https://github.com/Azure/aks-engine/blob/master/docs/README.md)

## <a name="expand-on-governance-disciplines"></a>展開治理專業領域

初始治理基礎可以用來擴充各種治理專業領域，以確保所有 Kubernetes 實例一致且穩定的部署方法。

您可以使用五種不同的觀點來查看 Kubernetes 叢集的治理。

### <a name="azure-resource-governance"></a>Azure 資源管理

第一個是 Azure 資源的觀點。 確保所有叢集都符合您組織的需求。 這包括網路拓撲、私人叢集、適用于 SRE 團隊的 Azure RBAC 角色、診斷設定、區域可用性、節點集區考慮、Azure Container Registry 管理、Azure 負載平衡器選項、AKS 附加元件、診斷設定等等等概念。 此治理可確保組織中叢集的「外觀與風格」和「拓撲」中的一致性。 這也應該延伸至 post 叢集部署啟動載入，例如必須安裝哪些安全性代理程式，以及如何設定它們。

<!-- docutune:casing Bicep CVEs -->

雪花式叢集很難管理任何集中容量。 將群集之間的差異降至最低，讓原則可以一致地套用，而且不建議且可偵測到異常的叢集。 這也可能包含用來部署叢集的技術，例如 ARM、Bicep 或 Terraform。

在管理群組/訂用帳戶層級套用的 Azure 原則有助於提供許多考慮，但並非全部。

### <a name="kubernetes-workload-governance"></a>Kubernetes 工作負載管理

由於 Kubernetes 本身是一個平臺，而第二個則是管理叢集中發生的情況。 這包括命名空間指引、網路原則、Kubernetes RBAC、限制和配額等專案。 這會將治理套用至工作負載，而不是叢集。 每個工作負載都將會是唯一的，因為它們全都能解決不同的商務問題，並會以各種不同的技術來實行。 其中可能沒有許多「單一大小適合所有」治理作法，但您應該考慮治理 OCI 構件的建立/耗用量、供應鏈需求、公用容器登錄使用量、映射隔離程式、部署管線治理。

如果方式要做的話，也請考慮在一般工具和模式周圍進行標準化。 針對 Helm、服務網格、輸入控制器、Gitops) 將操作員、永久性磁片區等技術提出建議。 這裡所包含的也會管理 pod 受控身分識別和來自 Key Vault 的來源秘密。

驅動對遙測存取的強烈期望，以確保工作負載擁有者能夠適當存取所需的計量和資料來改善其產品，同時也確保叢集操作員可以存取系統遙測來改善其服務供應專案。 資料通常需要在兩者之間交叉相互關聯，以確保在必要時能夠適當存取治理原則。

在叢集層級套用的適用于 AKS 的 Azure 原則有助於提供其中一些，但並非全部。

### <a name="cluster-operator-roles-devops-sre"></a>叢集操作員角色 (DevOps，SRE) 

第三個是針對叢集操作員角色進行治理。 SRE 團隊如何與群集互動？ 該小組和工作負載小組之間的關聯性。 它們是一樣的嗎？ 叢集操作員應該有明確定義的適用于叢集分級活動的腳本，例如它們存取叢集的方式、其存取叢集的位置，以及它們對叢集的許可權，以及指派這些許可權的時機。 請確定在此內容中，工作負載操作員與叢集運算子的治理檔、原則和訓練材料中有任何差異。 視您的組織而定，其可能是相同的。

### <a name="cluster-per-workload-or-many-workloads-per-cluster"></a>每個工作負載的叢集，或每個叢集的許多工作負載

第四個是多租使用者的治理。 也就是說，叢集應該包含依定義所擁有之應用程式的「類似群組」，這些都是由相同的工作負載小組所擁有，並且代表一 *組* 相關的工作負載元件。 或者，根據設計，叢集應該具有多個不同的工作負載和工作負載擁有者，且本質上為多租使用者;執行與管理，就像組織內受管理的服務供應專案。 治理策略在每個策略方面都特別不同，因此您應該管理您所選擇的策略是否已強制執行。 如果您必須同時支援這兩種模型，請確定您的治理計畫清楚地定義了哪些原則適用于哪些類型的叢集。

這項選擇應該在 [策略階段](./strategy.md) 進行，因為它對人員配置、預算和創新有很大的影響。

### <a name="stay-current-efforts"></a>隨時掌握最新成果

第五個是圍繞作業，例如節點映射的有效期限 (修補) 和 Kubernetes 版本控制。 誰負責進行節點映射升級、追蹤已套用的修補程式、追蹤和組合 Kubernetes 和 AKS Cve 補救計畫？ 工作負載小組 *需要* 驗證其解決方案適用于叢集升級，如果您的叢集不是最新的，則它們將會不受 Azure 支援。 在 AKS 中有一個強大的治理，也就是 Azure 中其他大部分的平臺，都是很 *重要* 的。 如此一來，就必須與應用程式小組和專用時間（至少每月）進行工作負載驗證，以確保叢集保持最新。 請確定相依于 Kubernetes 的所有團隊都瞭解這項持續投入的需求和成本，最後只要它們在平臺上即可。

### <a name="security-baseline"></a>安全性基準

您可以將下列最佳作法新增至安全性基準，以考慮 AKS 叢集的安全性：

- [安全 pod](/azure/aks/use-pod-security-on-azure-policy?bc=/azure/cloud-adoption-framework/_bread/toc.json&toc=/azure/cloud-adoption-framework/toc.json)
- [保護 pod 之間的流量](/azure/aks/use-network-policies?bc=/azure/cloud-adoption-framework/_bread/toc.json&toc=/azure/cloud-adoption-framework/toc.json)
- 如果未使用私人叢集[，AKS API 的授權 IP 存取](/azure/aks/api-server-authorized-ip-ranges?bc=/azure/cloud-adoption-framework/_bread/toc.json&toc=/azure/cloud-adoption-framework/toc.json)。

### <a name="identity"></a>身分識別

您可以將許多最佳作法套用至您的身分識別基準，以確保 Kubernetes 叢集之間的身分識別和存取管理一致：

- [Azure AD 整合](/azure/aks/managed-aad?bc=/azure/cloud-adoption-framework/_bread/toc.json&toc=/azure/cloud-adoption-framework/toc.json)
- [RBAC 和 Azure AD 整合](/azure/aks/azure-ad-rbac?bc=/azure/cloud-adoption-framework/_bread/toc.json&toc=/azure/cloud-adoption-framework/toc.json)
- [Kubernetes 中的受控識別](/azure/aks/use-managed-identity?bc=/azure/cloud-adoption-framework/_bread/toc.json&toc=/azure/cloud-adoption-framework/toc.json)
- [使用 Azure AD pod 身分識別存取其他 Azure 資源](/azure/aks/use-azure-ad-pod-identity?bc=/azure/cloud-adoption-framework/_bread/toc.json&toc=/azure/cloud-adoption-framework/toc.json)

## <a name="next-step-manage-modern-container-solutions"></a>下一步：管理新式容器解決方案

下列文章將引導您取得雲端採用旅程中特定點的指導方針，以協助您成功進行雲端採用案例。

- [管理新式容器解決方案](./manage.md)
