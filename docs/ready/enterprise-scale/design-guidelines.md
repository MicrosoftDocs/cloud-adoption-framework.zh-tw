---
title: 雲端採用架構企業規模的設計指導方針
description: 瞭解適用于 Azure 的 Microsoft Cloud 採用架構中的企業規模設計指導方針。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: c3a093d92777f41783cd47478f2a72dc3fa0c2ad
ms.sourcegitcommit: d31a9043d1ae9283ed126bf118ca26d1d18d6948
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/10/2020
ms.locfileid: "88040876"
---
# <a name="cloud-adoption-framework-enterprise-scale-design-guidelines"></a>雲端採用架構企業規模的設計指導方針

本文和文章系列會概述企業規模架構如何在每個[Azure 登陸區域設計區域](../landing-zone/design-areas.md)上提供固定的位置。 此系列提供逐步執行的設計指導方針，可讓您遵循企業規模解決方案中所述的設計原則。

企業規模架構的核心包含重要的設計路徑，其中包括了與高度相關且相依之設計決策的基本設計主題。 此存放庫提供這些架構重要技術領域的設計指導方針，以支援定義企業規模架構時必須進行的重要設計決策。 針對每個被視為的網域，請參閱提供的考慮和建議，並在每個區域內使用它們來結構和驅動設計。

例如，您可能會詢問您的資產需要多少訂用帳戶。 您可以審查[訂用帳戶組織和治理](./management-group-and-subscription-organization.md#subscription-organization-and-governance)，並使用概述的建議來驅動訂閱決策。

## <a name="critical-design-areas"></a>重要設計區域

下列八個重要的設計區域可協助您將需求轉譯為 Azure 結構和功能。 這些設計區域可協助您解決內部部署和雲端設計基礎結構之間的不相符，這通常會在企業級定義與 Azure 採用之間建立 dissonance 和摩擦。

在這些重要領域中做出決策的影響將會跨企業規模的架構 reverberate，並影響其他決策。 熟悉這八個領域以進一步瞭解內含決策的後果，這可能會在稍後於相關領域中產生取捨。

- [Enterprise 合約 (EA) 註冊和 Azure Active Directory 租使用者](./enterprise-enrollment-and-azure-ad-tenants.md)
- [身分識別和存取管理](./identity-and-access-management.md)
- [管理群組和訂用帳戶組織](./management-group-and-subscription-organization.md)
- [網路拓樸和連線能力](./network-topology-and-connectivity.md)
- [管理與監視](./management-and-monitoring.md)
- [商務持續性和災害復原](./business-continuity-and-disaster-recovery.md)
- [安全性、治理和合規性](./security-governance-and-compliance.md)
- [平台自動化和 DevOps](./platform-automation-and-devops.md)
