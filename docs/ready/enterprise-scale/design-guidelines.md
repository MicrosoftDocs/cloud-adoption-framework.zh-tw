---
title: 雲端採用架構企業規模的設計指導方針
description: 瞭解適用于 Azure 的 Microsoft 雲端採用架構中的企業規模設計指導方針。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: think-tank
ms.openlocfilehash: e6703dd1eca75a0e08c8c464bfb09bfb659e3640
ms.sourcegitcommit: d957bfc1fa8dc81168ce9c7d801a8dca6254c6eb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/23/2020
ms.locfileid: "95447365"
---
# <a name="cloud-adoption-framework-enterprise-scale-design-guidelines"></a>雲端採用架構企業規模的設計指導方針

本文和文章系列將概述企業規模架構如何在每個 [Azure 登陸區域設計區域](../landing-zone/design-areas.md)上提供固定的位置。 此系列提供一組逐步執行的設計指導方針，可讓您遵循企業規模解決方案中所述的設計原則。

企業規模架構的核心包含重要的設計路徑，其中包含了高度相關且相依設計決策的基本設計主題。 此存放庫提供這些跨架構重要技術領域的設計指引，以支援必須發生才能定義企業規模架構的重要設計決策。 針對每個被視為的網域，請參閱所提供的考慮和建議，並在每個區域內使用它們來結構設計及推動設計。

例如，您可能會詢問您的資產需要多少訂閱。 您可以審視 [訂用帳戶組織和治理](./management-group-and-subscription-organization.md#subscription-organization-and-governance) ，並使用概述的建議來推動訂用帳戶的決策。

## <a name="critical-design-areas"></a>重要設計區域

下列八個重要的設計區域可協助您將需求轉譯為 Azure 結構和功能。 這些設計區域可協助您解決內部部署與雲端設計基礎結構之間的不相符，這通常會在企業規模定義和 Azure 採用之間建立 dissonance 和摩擦。

在這些重要區域內所做的決策影響，將會跨企業規模架構 reverberate，並影響其他決策。 請熟悉這八個領域，以深入瞭解包含的決策結果，這會在稍後於相關領域中產生取捨。

- [Enterprise 合約 (EA) 註冊和 Azure Active Directory 租使用者](./enterprise-enrollment-and-azure-ad-tenants.md)
- [身分識別和存取管理](./identity-and-access-management.md)
- [管理群組和訂用帳戶組織](./management-group-and-subscription-organization.md)
- [網路拓樸和連線能力](./network-topology-and-connectivity.md)
- [管理與監視](./management-and-monitoring.md)
- [商務持續性和災害復原](./business-continuity-and-disaster-recovery.md)
- [安全性、治理和合規性](./security-governance-and-compliance.md)
- [平台自動化和 DevOps](./platform-automation-and-devops.md)
