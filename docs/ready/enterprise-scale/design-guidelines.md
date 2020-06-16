---
title: 設計指導方針
description: 設計指導方針
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: a1f3a23a57071dee637ecd0b7721ce8db4f86194
ms.sourcegitcommit: d88c1cc3597a83ab075606d040ad659ac4b33324
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/15/2020
ms.locfileid: "84792645"
---
# <a name="design-guidelines"></a>設計指導方針

本文和文章系列將概述企業規模架構如何在每個 Microsoft [Azure 登陸區域設計區域](../landing-zone/design-areas.md)上提供固定的位置。 這篇文章系列會建立一組逐步執行的設計方針，讓您可以遵循這些原則來實行企業規模解決方案中所述的設計原則。

企業規模架構的核心包含重要的設計路徑，其中包括了與高度相關且相依之設計決策的基本設計主題。 此存放庫提供這些架構重要技術領域的設計指導方針，以支援定義企業規模架構時必須進行的重要設計決策。 針對每個被視為的網域，讀者應該檢查提供的考慮和建議，並在每個區域內使用它們來結構和驅動設計。

例如，客戶可能會詢問他們的資產需要多少訂用帳戶。 在此情況下，讀者應審查[訂用帳戶組織和治理](./management-group-and-subscription-organization.md#subscription-organization-and-governance)，並使用概述的建議來驅動訂閱決策。

## <a name="critical-design-areas"></a>重要設計區域

下列八個重要設計區域有助於將客戶需求轉譯為 Microsoft Azure 的結構和功能，並解決內部部署和雲端設計基礎結構之間的不相符問題，這通常會在企業級定義與 Azure 採用之間建立 dissonance 和摩擦。

在這些重要領域中做出決策的影響將會跨企業規模的架構 reverberate，並影響其他決策。 讀者應熟悉下列八個區域，以進一步瞭解內含決策的後果，這可能會在稍後於相關領域中產生取捨。

1. [企業註冊和 Azure Active Directory 租使用者](./enterprise-enrollment-and-azure-ad-tenants.md)
2. [身分識別和存取管理](./identity-and-access-management.md)
3. [管理群組和訂用帳戶組織](./management-group-and-subscription-organization.md)
4. [網路拓撲和連線能力](./network-topology-and-connectivity.md)
5. [管理與監視](./management-and-monitoring.md)
6. [商務持續性和災害復原](./business-continuity-and-disaster-recovery.md)
7. [安全性、治理和合規性](./security-governance-and-compliance.md)
8. [平臺自動化和 DevOps](./platform-automation-and-devops.md)
