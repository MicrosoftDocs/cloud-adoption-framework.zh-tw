---
title: 叢集和應用程式安全性
description: 瞭解適用于叢集和應用程式安全性的雲端採用架構中的 Kubernetes。
author: sabbour
ms.author: brblanch
ms.date: 03/20/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.custom: think-tank
ms.openlocfilehash: b0dd32f4696676be08dda5ae6c06779f9970d74f
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102112954"
---
<!-- cSpell:ignore kured -->

# <a name="cluster-and-application-security"></a>叢集和應用程式安全性

熟悉 Kubernetes security essentials，並查看叢集和應用程式安全性指導的安全設定。

## <a name="plan-train-and-proof"></a>規劃、定型和證明

當您開始使用時，下列檢查清單和資源將協助您規劃叢集作業和安全性。 您應該可以回答下列問題：

> [!div class="checklist"]
>
> - 您是否已複習 Kubernetes 叢集的安全性與威脅模型？
> - 是否已啟用您的叢集來 Kubernetes 角色型存取控制？
> [!div class="tdCol2BreakAll"]
>
> | 檢查清單 | 資源 |
> |---|---|
> | **熟悉安全性基本知識白皮書。** 安全 Kubernetes 環境的主要目標是確保它所執行的應用程式受到保護、可以快速識別並解決安全性問題，而且將會防止未來發生類似的問題。 | [保障 Kubernetes 安全的最終指南 (白皮書) ](https://clouddamcdnprodep.azureedge.net/gdc/gdc8LXmoZ/original)     |
> | **檢查叢集節點的安全性強化設定。** 安全性強化的主機 OS 可減少攻擊的介面區，並允許安全地部署容器。 | [AKS 虛擬機器主機中的安全性強化](/azure/aks/security-hardened-vm-host-image)     |
> | **設定叢集 Kubernetes 以角色為基礎的存取控制 (Kubernetes RBAC) 。** 此控制機制可讓您指派權限給使用者或使用者群組，以執行像是建立或修改資源，或檢視執行中應用程式工作負載的記錄等動作。 | [瞭解 Kubernetes 以角色為基礎的存取控制 (Kubernetes RBAC)  (影片) ](https://www.youtube.com/watch?list=PLLasX02E8BPCrIhFrc_ZiINhbRkYMKdPT&v=G3R24JSlGjY&index=12) <br> [整合 Azure AD 與 Azure Kubernetes Service](/azure/aks/azure-ad-integration-cli) <br> [叢集組態檔的限制存取](/azure/aks/control-kubeconfig-access)   |

## <a name="deploy-to-production-and-apply-best-practices"></a>部署至生產環境並套用最佳作法

當您準備應用程式以進行生產時，您應該執行一組最小的最佳作法。 在這個階段使用下列檢查清單。 您應該能夠回答下列問題：

> [!div class="checklist"]
>
> - 您是否已針對輸入、輸出和 pod 內通訊設定網路安全性規則？
> - 您的叢集是否已設定為自動套用節點安全性更新？
> - 您是否正在為您的叢集和容器工作負載執行安全性掃描解決方案？
> [!div class="tdCol2BreakAll"]
>
> | 檢查清單 | 資源 |
> |---|---|
> | **使用群組成員資格來控制對叢集的存取。** 設定 Kubernetes 角色型存取控制 (Kubernetes RBAC) ，根據使用者身分識別或群組成員資格來限制對叢集資源的存取。 | [使用 Kubernetes RBAC 和 Azure AD 身分識別來控制對叢集資源的存取](/azure/aks/azure-ad-rbac)    |
> | **建立秘密管理原則。** 使用 Kubernetes 中的秘密管理來安全地部署及管理機密資訊（例如密碼和憑證）。 | [瞭解 Kubernetes 中的秘密管理 (影片) ](https://www.youtube.com/watch?list=PLLasX02E8BPCrIhFrc_ZiINhbRkYMKdPT&v=KmhM33j5WYk&index=10) |
> | **使用網路原則來保護 pod 內的網路流量。** 套用最低許可權原則來控制叢集中 pod 之間的網路流量。 | [使用網路原則保護 pod 內流量](/azure/aks/use-network-policies) |
> | **使用授權 Ip 來限制對 API 伺服器的存取。** 將對 API 伺服器的存取限制為一組有限的 IP 位址範圍，以改善叢集安全性並將攻擊面降至最低。 | [保護對 API 伺服器的存取](/azure/aks/api-server-authorized-ip-ranges) |
> | **限制叢集輸出流量。** 瞭解當您限制叢集的輸出流量時，所允許的埠和位址。 您可以使用 Azure 防火牆或協力廠商防火牆設備來保護輸出流量，並定義這些必要的埠和位址。 | [在 AKS 中控制叢集節點的輸出流量](/azure/aks/limit-egress-traffic) |
> | **使用 Web 應用程式防火牆保護流量 (WAF) 。** 使用 Azure 應用程式閘道作為 Kubernetes 叢集的輸入控制器。 | [將 Azure 應用程式閘道設定為輸入控制器](/azure/application-gateway/ingress-controller-overview) |
> | **將安全性和核心更新套用至背景工作節點。** 瞭解 AKS 節點更新體驗。 為了保護您的叢集，系統會自動將安全性更新套用至 AKS 中的 Linux 節點。 這些更新包括 OS 安全性修正或核心更新。 這其中有一些更新需要重新啟動節點，才能完成此程序。 | [使用 kured 自動重新開機節點以套用更新](/azure/aks/node-updates-kured) |
> | **設定容器和叢集掃描解決方案。** 掃描推送到 Azure Container Registry 的容器，並深入瞭解您的叢集節點、雲端流量和安全性控制措施。 | [Azure Container Registry 與 Security Center 的整合](/azure/security-center/defender-for-container-registries-introduction) <br> [Azure Kubernetes Service 與 Security Center 整合](/azure/security-center/defender-for-kubernetes-introduction)  |

## <a name="optimize-and-scale"></a>優化和調整

應用程式現在已在生產環境中，您要如何優化您的工作流程，並準備您的應用程式和小組進行調整？ 使用「優化」和「調整」檢查清單來準備。 您應該可以回答：

> [!div class="checklist"]
>
> - 您可以大規模強制執行治理和叢集原則嗎？
> [!div class="tdCol2BreakAll"]
>
> | 檢查清單 | 資源 |
> |---|---|
> | **強制執行叢集治理原則。** 以集中、一致的方式，在您的叢集上套用大規模大規模地規範和保護。 | [使用 Azure 原則控制部署](/azure/governance/policy/concepts/policy-for-kubernetes) |
> | **定期輪替叢集憑證。** Kubernetes 會使用憑證來驗證它的許多元件。 基於安全性或原則的考慮，您可能會想要定期輪替這些憑證。 | [在 Azure Kubernetes Service 中輪替憑證 (AKS) ](/azure/aks/certificate-rotation) |
