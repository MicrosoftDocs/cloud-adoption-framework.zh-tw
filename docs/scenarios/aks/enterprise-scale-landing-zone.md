---
title: 適用于 AKS 企業規模案例的建築集
description: 使用 Azure Kubernetes Service (AKS) 建築集，協助您建立支援 AKS 的企業級登陸區域。
author: BrianBlanchard
ms.author: brblanch
ms.date: 03/01/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 344aba49d1689d30cf6c31ef931fbefb852e329e
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794856"
---
# <a name="construction-set-for-aks-enterprise-scale-scenario"></a>適用于 AKS 企業規模案例的建築集

AKS 建築集是 Terraform 範本的開放原始碼集合，可讓您用來自動部署能夠裝載 Azure Kubernetes Service (AKS) 的環境。 它可以提供特定的架構方法和參考，以準備企業規模的登陸區域。 登陸區域會遵守雲端採用架構的架構和最佳作法。

客戶採用 AKS 的各種方式。 您可以調整此結構，以產生符合 *您* 的方式的架構，並將您的組織放在持續調整的路徑上。

## <a name="to-begin-implement-an-enterprise-scale-landing-zone"></a>若要開始，請執行企業規模的登陸區域

AKS 建築集會假設企業規模登陸區域已成功實施。 如需這項必要條件的詳細資訊，請參閱下列文章：

- [開始使用雲端採用架構的企業規模登陸區域](../../ready/enterprise-scale/index.md)
- [在 Azure 中實施雲端採用架構企業級登陸區域](../../ready/enterprise-scale/implementation.md)

## <a name="what-the-aks-construction-set-provides"></a>AKS 結構集所提供的功能

登陸區域的結構集方法會提供這些資產來支援您的專案：

- 模組化方法，可讓您自訂環境變數
- 協助評估重要決策的設計指導方針
- 登陸區域架構
- 包含下列內容的執行：
  - 能夠為您的 AKS 部署建立環境的可部署參考
  - 經 Microsoft 核准的 AKS 參考，可測試已部署的環境

## <a name="design-guidelines"></a>設計指導方針

這些文章提供建立登陸區域的指導方針：

- [Azure Kubernetes Service (AKS) 企業規模案例](./eslz-identity-and-access-management.md)
- [適用于 AKS 企業規模案例的網路拓撲和連線能力](./eslz-network-topology-and-connectivity.md)
- [適用于 AKS 企業規模案例的管理和監視](./eslz-management-and-monitoring.md)
- [AKS 企業規模案例的商務持續性和嚴重損壞修復](./eslz-business-continuity-and-disaster-recovery.md)
- [AKS 企業規模案例的安全性、治理和合規性](./eslz-security-governance-and-compliance.md)
- [適用于 AKS 企業規模案例的平臺自動化和 DevOps](./eslz-platform-automation-and-devops.md)

## <a name="example-conceptual-reference-architecture"></a>概念參考架構範例

下列概念參考架構是顯示設計區域和最佳作法的範例。

![責任區](./media/aks-enterprise-scale-landing-zone.png)

## <a name="obtain-the-aks-construction-set"></a>取得 AKS 建築集

AKS 建築集是 Terraform 範本的開放原始碼集合，可在 [此 GitHub](https://github.com/Azure/caf-terraform-landingzones-starter/tree/starter/enterprise_scale/construction_sets/aks/online/aks_secure_baseline)存放庫中取得。

範本有兩種類型：

- Terraform 可將基礎結構元件（例如虛擬機器、網路或儲存體）部署至 Azure 的模組。
- Ansible 可在已部署的基礎結構上執行不同角色來設定虛擬機器，以及安裝 AKS HANA 和必要應用程式的操作手冊。

## <a name="next-steps"></a>下一步

- 複習 AKS 建築集合的重要設計領域，以針對您的 AKS 建築集架構做出完整的考慮和建議。 請參閱 [Azure Kubernetes Service (AKS) 企業規模的案例](./eslz-identity-and-access-management.md)。
