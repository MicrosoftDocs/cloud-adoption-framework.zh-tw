---
title: Enterprise-Scale 混合式和多重雲端的支援
description: 描述企業規模如何能夠加速採用混合式或多重雲端架構。
author: DominicAllen
ms.author: doalle
ms.date: 03/01/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 698fa8ed9569755df0349bbd8c7c77021d295000
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102116354"
---
# <a name="enterprise-scale-support-for-hybrid-and-multicloud"></a>混合式和多重雲端的企業級支援
  
企業規模的登陸區域提供特定的架構方法、參考架構和參考，以針對任務關鍵性技術平臺和任何支援的工作負載準備登陸區域。

企業規模的設計是以混合式和多重雲端為考慮。 支援混合式和多重雲端需要三個簡單的參考架構新增：

- **混合式：** 新增混合式網路連線能力
- **多重雲端：** 新增多重雲端網路連線能力
- **整合作業：** 新增 Azure Arc 以擴充治理和營運支援

## <a name="why-hybrid"></a>為何要混合？

當組織想要採用新式雲端服務及其帶來的優點時，與舊版的內部部署基礎結構之間會有不可避免的並存執行。

此外，隨著雲端服務的評估或業務需求的考慮，組織也可以選擇在多個公用雲端服務中執行服務。

分散式的異類資產需要經過簡化的合併管理和治理，以降低操作的影響。

作為雲端採用架構指引一部分引進的登陸區域概念可用來建立用於建立混合式架構的模式，並引進連線能力、治理和監視的標準。

如果策略性的目的是要簡化併合並遷移專案的基礎結構和服務，以做為管理程式和工具的設定標準，這會很有説明，這表示在移至 Azure 之後，不需要翻新工作負載。

## <a name="prerequisite"></a>必要條件

本文假設企業規模登陸區域已成功執行。 如需有關此必要條件的詳細資訊，請參閱企業規模的 [總覽](../../ready/enterprise-scale/index.md) 和 [實施指引](../../ready/enterprise-scale/implementation.md)。

## <a name="design-guidelines"></a>設計指導方針

推動 Azure 企業規模登陸區域的雲端採用架構設計的關鍵決策指南。 有六個重要的設計區域，可用來檢查和修改企業規模登陸區域或任何其他 Azure 登陸區域的執行：

- [身分識別和存取管理](../../ready/enterprise-scale/identity-and-access-management.md)
- [網路拓樸和連線能力](../../ready/enterprise-scale/network-topology-and-connectivity.md)
- [管理與監視](../../ready/enterprise-scale/management-and-monitoring.md)
- [商務持續性和災害復原](../../ready/enterprise-scale/business-continuity-and-disaster-recovery.md)
- [安全性、治理和合規性](../../ready/enterprise-scale/security-governance-and-compliance.md)
- [平台自動化和 DevOps](../../ready/enterprise-scale/platform-automation-and-devops.md)

## <a name="implementation-additions"></a>執行新增

若要擴充企業規模以因應混合和多重雲端的需求，請考慮下列各項作為實作為的一部分：

## <a name="landing-zone"></a>登陸區域

登陸區域是一組參考架構，可協助組織快速建立雲端環境並加速採用雲端技術。

雲端採用架構包含在 Azure 中開發架構以連接到外部環境的特定指引。 [中樞和輪輻等模式可用來建立登陸區域，以便從外部位置進行輸入和輸出。](../../ready/enterprise-scale/implementation.md)

例如，您 [可以在這裡找到指導](../../ready/azure-best-practices/connectivity-to-other-cloud-providers.md) 方針，以協助您特別針對與其他雲端環境的連線，建立登陸區域架構。

## <a name="network"></a>網路

在 Azure 和外部環境之間進行整合時，網路拓撲是重要的考慮因素，應納入任何規劃練習和設計工作中，以確保能順利執行。
以下提供特定主題的一些特定且可採取動作的指導方針：

- [Azure 的連線能力](../../ready/azure-best-practices/connectivity-to-azure.md)
- [IP 位址](../../ready/azure-best-practices/plan-for-ip-addressing.md)
- [中樞和輪輻網路拓撲](../../ready/azure-best-practices/hub-spoke-network-topology.md)
- [PrivateLink 和 DNS 整合](../../ready/azure-best-practices/private-link-and-dns-integration-at-scale.md)
- [登陸區域網路分割](../../ready/azure-best-practices/plan-for-landing-zone-network-segmentation.md)

## <a name="identity"></a>身分識別

當組織在現有的內部部署資料中心內建立雲端環境時，身分識別會成為考慮的領域，以確保不會引進問題。
當應用程式在內部部署與雲端之間散佈時，對使用者的存取管理可能會成為一項挑戰。
Azure 提供的技術可協助組織在內部部署和雲端環境間管理身分識別，以簡化使用者的體驗。

為了達成混合式身分識別，組織應考慮下列三種 Azure Active Directory 方法：

- [密碼雜湊同步處理 (PHS)](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-phs)
- [傳遞驗證 (PTA)](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta)
- [同盟 (AD FS)](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-fed)

您[可以在這裡找到更多有關](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity)在混合式環境中規劃身分識別模型的資訊。

## <a name="governance"></a>控管

將基礎結構和應用程式擴展到多個位置，可能會增加維護治理標準的複雜度。
在規劃混合式、集中式治理工具和流程的過程中，您應該執行這些工作負載，以便在工作負載向外延展時建立良好的模式。

[雲端採用架構的企業規模登陸區域架構](../../ready/enterprise-scale/architecture.md)包含模式，可透過結構化使用管理群組將資源分割成邏輯群組，以標準化[Azure 原則](https://docs.microsoft.com/azure/governance/policy/overview)和[角色型存取控制的部署， (RBAC) ](../../ready/azure-setup-guide/manage-access.md) 。
您可以使用 Azure Arc 之類的技術來擴充這些模式，如下所述。

## <a name="management"></a>管理性

類似于在混合式環境中治理所需的考慮，大規模管理分散式工作負載需要規劃，以確保不會在部署規模範圍內引進問題。
使用 [Log Analytics](/azure/azure-monitor/log-query/log-analytics-overview)、 [Application Insights](/azure/azure-monitor/app/app-insights-overview)、 [Azure 監視器](https://azure.microsoft.com/services/monitor/#features)和 [azure 資訊安全中心](/azure/security-center/) 之類的技術來 [匯總遙測資料，並從「單一窗格](../../manage/azure-management-guide/inventory.md?tabs=AzureServiceHealth%2CLog-Analytics%2CAzure-Monitor%2CConfigure-solutions) 」進行工作，可讓基礎結構和應用程式小組依例外狀況進行管理，並將焦點放在從合併視圖修正已識別的問題。  

除了治理技巧，上述的管理技術也可以延伸到其他環境，例如在特定使用案例中的內部部署和其他雲端平臺。

## <a name="azure-arc"></a>Azure Arc

如本文中所述，Azure 為組織提供各種管理工具，可讓基礎結構和應用程式大規模進行監視和控管。
在實行混合式登陸區域時，應擴充這些 Azure 工具，以控制在 Azure 外部執行的基礎結構和應用程式。

如此一來，就能在整個混合式資產上提供單一管理平面和單一觀點，讓大規模監視和管理變得更簡單。

[Azure Arc](/azure/azure-arc/) 提供一致的多雲端和內部部署管理平臺，以簡化治理和管理。
Azure Arc 可讓您藉由將現有資源投射到 [Azure Resource Manager](/azure/azure-resource-manager/management/overview#:~:text=Azure%20Resource%20Manager%20is%20the%20deployment%20and%20management,Manager%20templates%20(ARM%20templates),%20see%20the%20template)，來管理整個環境。

您現在可以管理虛擬機器、Kubernetes 叢集和資料庫，就像這些服務都在 Azure 中執行一樣。 無論其位於何處，您都可以使用熟悉的 Azure 服務和管理功能。 Azure Arc 可讓您繼續使用傳統 ITOps，同時引進 DevOps 實務來支援您環境中的新雲端原生模式。
