---
title: 叢集和應用程式安全性
description: 瞭解適用于叢集和應用程式安全性的雲端採用架構中的 Kubernetes。
author: sabbour
ms.author: asabbour
ms.date: 03/20/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: 4af9516fe50068a76c4b85ae64bbb907410ee7ab
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86448804"
---
<!-- cSpell:ignore asabbour sabbour kured -->

# <a name="cluster-and-application-security"></a>叢集和應用程式安全性

熟悉 Kubernetes security essentials，並回顧叢集和應用程式安全性指引的安全設定。

## <a name="plan-train-and-proof"></a>規劃、定型和證明

當您開始使用時，以下的檢查清單和資源將可協助您規劃叢集作業和安全性。 您應該能夠回答下列問題：

> [!div class="checklist"]
>
> - 您已複習過 Kubernetes 叢集的安全性與威脅模型嗎？
> - 您的叢集是否已啟用角色型存取控制？

<!-- markdownlint-disable MD033 -->

> [!div class="tdCol2BreakAll"]
>
> | 檢查清單  | 資源 |
> |------------------------------------------------------------------|-----------------------------------------------------------------|
> | **熟悉安全性基本知識白皮書。** 安全 Kubernetes 環境的主要目標，是要確保其執行的應用程式受到保護、可以快速識別及解決安全性問題，而且未來將會防止類似的問題。 | [保護 Kubernetes 的最終指南（白皮書）](https://clouddamcdnprodep.azureedge.net/gdc/gdc8LXmoZ/original)     |
> | **檢查叢集節點的安全性強化設定。** 安全性強化主機 OS 可減少攻擊的介面區，並允許安全地部署容器。 | [AKS 虛擬機器主機中的安全性強化](https://docs.microsoft.com/azure/aks/security-hardened-vm-host-image)     |
> | **設定叢集角色型存取控制（RBAC）。** 此控制機制可讓您指派權限給使用者或使用者群組，以執行像是建立或修改資源，或檢視執行中應用程式工作負載的記錄等動作。 | [瞭解 Kubernetes 中的角色型存取控制（RBAC）（影片）](https://www.youtube.com/watch?v=G3R24JSlGjY&list=PLLasX02E8BPCrIhFrc_ZiINhbRkYMKdPT&index=12) <br> [將 Azure AD 與 Azure Kubernetes Service 整合](https://docs.microsoft.com/azure/aks/azure-ad-integration) <br> [叢集組態檔的限制存取](https://docs.microsoft.com/azure/aks/control-kubeconfig-access)   |

## <a name="deploy-to-production-and-apply-best-practices"></a>部署到生產環境並套用最佳作法

當您準備應用程式以進行生產時，您應該執行一組最基本的最佳作法。 請在此階段使用以下檢查清單。 您應該能夠回答下列問題：

> [!div class="checklist"]
>
> - 您是否已針對輸入、輸出和 pod 內通訊設定網路安全性規則？
> - 您的叢集設定為自動套用節點安全性更新嗎？
> - 您是否正在為叢集和容器工作負載執行安全性掃描解決方案？

<!-- markdownlint-disable MD033 -->

> [!div class="tdCol2BreakAll"]
>
> | 檢查清單  | 資源 |
> |------------------------------------------------------------------|-----------------------------------------------------------------|
> | **使用群組成員資格來控制叢集的存取權。** 設定 Kubernetes 角色型存取控制（RBAC），以根據使用者身分識別或群組成員資格來限制叢集資源的存取權。 | [使用 RBAC 和 Azure AD 群組來控制對叢集的存取](https://docs.microsoft.com/azure/aks/azure-ad-rbac)    |
> | **建立秘密管理原則。** 使用 Kubernetes 中的秘密管理，安全地部署和管理機密資訊，例如密碼和憑證。 | [瞭解 Kubernetes 中的秘密管理（影片）](https://www.youtube.com/watch?v=KmhM33j5WYk&list=PLLasX02E8BPCrIhFrc_ZiINhbRkYMKdPT&index=10) |
> | **使用網路原則保護 pod 內部網路流量。** 套用最低許可權原則，以控制叢集中 pod 之間的網路流量。 | [使用網路原則保護內部 pod 流量](https://docs.microsoft.com/azure/aks/use-network-policies) |
> | **使用授權的 Ip 來限制對 API 伺服器的存取。** 藉由將 API 伺服器的存取限制為一組有限的 IP 位址範圍，來改善叢集安全性並將受攻擊面降至最低。 | [保護對 API 伺服器的存取](https://docs.microsoft.com/azure/aks/api-server-authorized-ip-ranges) |
> | **限制叢集輸出流量。** 如果您限制叢集的輸出流量，請瞭解要允許哪些埠和位址。 您可以使用 Azure 防火牆或協力廠商防火牆設備來保護您的輸出流量，並定義這些必要的埠和位址。 | [在 AKS 中控制叢集節點的輸出流量](https://docs.microsoft.com/azure/aks/limit-egress-traffic) |
> | **使用 Web 應用程式防火牆（WAF）保護流量。** 使用 Azure 應用程式閘道作為 Kubernetes 叢集的輸入控制器。  | [將 Azure 應用程式閘道設定為輸入控制器](https://docs.microsoft.com/azure/application-gateway/ingress-controller-overview)    |
> | **將安全性和核心更新套用至背景工作節點。** 瞭解 AKS 節點更新體驗。 為了保護您的叢集，安全性更新會自動套用至 AKS 中的 Linux 節點。 這些更新包括 OS 安全性修正或核心更新。 這其中有一些更新需要重新啟動節點，才能完成此程序。 | [使用 kured 自動重新開機節點以套用更新](https://docs.microsoft.com/azure/aks/node-updates-kured) |
> | **設定容器和叢集掃描解決方案。** 掃描已推送至 Azure Container Registry 的容器，並深入瞭解您的叢集節點、雲端流量和安全性控制。 | [Azure Container Registry 與資訊安全中心整合](https://docs.microsoft.com/azure/security-center/azure-container-registry-integration) <br> [Azure Kubernetes Service 與資訊安全中心整合](https://docs.microsoft.com/azure/security-center/azure-kubernetes-service-integration)  |

## <a name="optimize-and-scale"></a>優化和調整

應用程式現在已進入生產階段，如何優化您的工作流程，並準備您的應用程式和小組進行調整？ 使用優化和調整檢查清單來準備。 您應該能夠回答：

> [!div class="checklist"]
>
> - 您可以大規模強制執行治理和叢集原則嗎？

<!-- markdownlint-disable MD033 -->

> [!div class="tdCol2BreakAll"]
>
> | 檢查清單  | 資源 |
> |------------------------------------------------------------------|-----------------------------------------------------------------|
> | **強制執行叢集治理原則。** 以集中、一致的方式，在您的叢集上套用大規模的實行原則和保護。 | [使用 Azure 原則控制部署](https://docs.microsoft.com/azure/governance/policy/concepts/rego-for-aks)    |
> | **定期輪替叢集憑證。** Kubernetes 會使用憑證來驗證其許多元件。 基於安全性或原則的理由，您可能會想要定期輪替那些憑證。 | [在 Azure Kubernetes Service 中輪替憑證（AKS）](https://docs.microsoft.com/azure/aks/certificate-rotation)    |
