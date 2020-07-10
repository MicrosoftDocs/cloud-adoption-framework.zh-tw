---
title: CAF 企業規模的設計指導方針
description: 瞭解適用于 Azure 的 Microsoft Cloud 採用架構中的企業規模設計指導方針。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 1764c949d48ee2a68a13e2cff251a6949f9933ef
ms.sourcegitcommit: bcc73d194c6d00c16ae2e3c7fb2453ac7dbf2526
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/09/2020
ms.locfileid: "86194743"
---
# <a name="caf-enterprise-scale-design-guidelines"></a>CAF 企業規模的設計指導方針

本文和文章系列將概述企業規模架構如何在每個 Microsoft [Azure 登陸區域設計區域](../landing-zone/design-areas.md)上提供固定的位置。 這會提供一組逐步執行的設計方針，讓您可以遵循這些原則來實行企業級解決方案中所述的設計原則。

企業規模架構的核心包含重要的設計路徑，其中包括了與高度相關且相依之設計決策的基本設計主題。 此存放庫提供這些架構重要技術領域的設計指導方針，以支援定義企業規模架構時必須進行的重要設計決策。 針對每個被視為的網域，您應該檢查提供的考慮和建議，並在每個區域內使用它們來結構和驅動設計。

例如，您可能會詢問您的資產需要多少訂用帳戶。 在此情況下，您應該審查訂用帳戶[組織和治理](./management-group-and-subscription-organization.md#subscription-organization-and-governance)，並使用概述的建議來驅動訂閱決策。

## <a name="critical-design-areas"></a>重要設計區域

下列八個重要的設計區域可協助您將需求轉譯成 Microsoft Azure 的結構和功能。 它可協助您解決內部部署和雲端設計基礎結構之間的不相符，這通常會在企業級定義與 Azure 採用之間建立 dissonance 和摩擦。

在這些重要領域中做出決策的影響將會跨企業規模的架構 reverberate，並影響其他決策。 您應該熟悉下列八個區域，以進一步瞭解內含決策的後果，這可能會在稍後於相關領域中產生取捨。

1. [Enterprise 合約 (EA) 註冊和 Azure Active Directory 租使用者](./enterprise-enrollment-and-azure-ad-tenants.md)
2. [身分識別和存取管理](./identity-and-access-management.md)
3. [管理群組和訂用帳戶組織](./management-group-and-subscription-organization.md)
4. [網路拓樸和連線能力](./network-topology-and-connectivity.md)
5. [管理與監視](./management-and-monitoring.md)
6. [商務持續性和災害復原](./business-continuity-and-disaster-recovery.md)
7. [安全性、治理和合規性](./security-governance-and-compliance.md)
8. [平台自動化和 DevOps](./platform-automation-and-devops.md)
