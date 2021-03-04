---
title: 企業規模的 AKS 身分識別和存取管理
description: 描述此企業規模案例如何改善 Azure Kubernetes 服務的身分識別和存取管理。
author: TomGeske
ms.author: thomasge
ms.date: 03/01/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 12ca9ad27af872e64d4cef1a90a99c04f5c21c87
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794945"
---
# <a name="identity-and-access-management-for-azure-kubernetes-service-aks-enterprise-scale-scenario"></a>適用于 Azure Kubernetes Service 的身分識別與存取管理 (AKS) 企業規模案例

您的組織或企業必須設計適當的安全性設定，以符合其需求。 身分識別和存取管理涵蓋了多個層面，例如叢集身分識別、工作負載身分識別和操作員存取。

## <a name="design-considerations"></a>設計考量

- 決定 (受控識別或服務主體) 使用哪個叢集身分識別。
- 決定如何驗證叢集存取 (以憑證為基礎或 Azure Active Directory) 。
- 決定多租使用者叢集，以及如何在 Kubernetes 中設定以角色為基礎的存取控制 (RBAC) 。
  - 決定隔離 (命名空間、網路原則、計算 (節點集區) 或叢集) 的方法。
  - 決定是否要為每個應用程式小組 Kubernetes RBAC 角色和計算配置以進行隔離。
  - 決定應用程式小組是否可以讀取其叢集中或其他叢集中的其他工作負載。
- 決定 [AKS 登陸區域](../../ready/enterprise-scale/identity-and-access-management.md)的自訂 Azure RBAC 角色。
  - 決定網站可靠性工程需要哪些許可權 (SRE) 角色來管理/疑難排解整個叢集。
  - 決定 SecOps 需要哪些許可權。
  - 決定登陸區域擁有者所需的許可權。
  - 決定應用程式小組部署到叢集中所需的許可權。
- 決定您是否需要 (Azure AD pod 身分識別) 的工作負載身分識別。 Azure 服務（例如 Azure Key Vault 整合、Azure Cosmos DB 及其他服務）可能需要它們。

## <a name="design-recommendations"></a>設計建議

- **叢集身分識別**
  - 針對您的 AKS 叢集使用您自己的 [受控識別](https://aka.ms/aks/mi) 。
  - 為您的 AKS 登陸區域定義自訂 Azure RBAC 角色，以簡化叢集受控身分識別的必要版權管理。
- **叢集存取**
  - 搭配 Azure AD 使用 Kubernetes RBAC 來 [限制許可權](/azure/aks/azure-ad-rbac) ，並將授與系統管理員許可權的最小化，以保護設定和密碼存取。
  - 使用 [AKS 管理的 AZURE ad 整合](https://aka.ms/aks/managed-aad) ，以使用 azure ad 進行驗證和操作員和開發人員存取。
- 在 Kubernetes 中定義必要的 RBAC 角色和角色系結。
  - 使用 [Kubernetes 角色和角色](/azure/aks/concepts-identity#kubernetes-role-based-access-control-kubernetes-rbac) 系結至 Azure AD 群組以進行網站可靠性工程， (SRE) 、SecOps 和開發人員存取。
  - 您應視需要及時授與 SRE 完整存取權。 在 [Azure AD 中使用具特殊許可權的身分識別管理](/azure/active-directory/privileged-identity-management/pim-configure) ，以及 [企業規模](../../ready/enterprise-scale/identity-and-access-management.md)的身分識別與存取管理。
