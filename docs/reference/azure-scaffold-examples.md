---
title: 訂用帳戶治理的案例和範例
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 提供如何在常見案例中實作 Azure 訂用帳戶治理的範例。
author: rdendtler
ms.author: rodend
ms.date: 01/03/2017
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: reference
ms.openlocfilehash: ffda6a8f11954895e934f310c1a53c95fb2e1351
ms.sourcegitcommit: b30952f08155513480c6b2c47a40271c2b2357cf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72378051"
---
# <a name="examples-of-implementing-azure-enterprise-scaffold"></a>實作 Azure 企業 Scaffold 的範例

> [!NOTE]
> Azure 企業版樣板已整合到 Microsoft Cloud 採用架構中。 這篇文章中的內容現在是以新架構的 [[就緒](../ready/index.md)] 區段表示。 這篇文章將于2020年初淘汰。 若要開始使用新的程式，請參閱[準備好的總覽](../ready/index.md)、[建立您的第一個登陸區域](../ready/azure-setup-guide/migration-landing-zone.md)，以及（或）[登陸區域的考慮](../ready/considerations/index.md)。

本文提供企業如何實作 [Azure 企業 Scaffold](./azure-scaffold.md) 建議的範例。 它會使用名為 Contoso 的虛構公司來說明常見案例的最佳作法。

## <a name="background"></a>背景

Contoso 是一家全球性公司，為客戶提供供應鏈解決方案。 從「軟體即服務」模型到內部部署的封裝模型，都在其所提供的解決方案範圍內。 它們在全球各地開發軟體，並在印度、美國和加拿大設立重要的開發中心。

該公司的 ISV 部份分為數個獨立的業務單位，負責管理重要業務的產品。 每個業務單位都有自己的開發人員、產品經理和架構設計人員。

企業技術服務（ETS）業務單位提供集中化的 IT 功能，並管理數個資料中心，讓業務單位裝載其應用程式。 除了管理資料中心，ETS 組織也提供和管理集中式共同作業（例如電子郵件和網站）和網路/電話語音服務。 它們也會為沒有操作人員的較小業務單位管理客戶面向工作負載。

本文會使用下列人物︰

- Dave 是 ETS Azure 系統管理員。
- Alice 是 Contoso 供應鏈業務單位的開發主管。

Contoso 需要建置企業營運應用程式和客戶面向應用程式。 它已決定要在 Azure 上執行應用程式。 Dave 會閱讀[規定的訂](./azure-scaffold.md)用帳戶治理一文，並準備好執行建議。

## <a name="scenario-1-line-of-business-application"></a>案例 1︰企業營運應用程式

Contoso 正在建置原始程式碼管理系統 (BitBucket)，以供世界各地的開發人員使用。 應用程式會使用「基礎結構即服務」（IaaS）來裝載，並由 web 伺服器和資料庫伺服器組成。 開發人員會存取其開發環境中的伺服器，但不需要存取 Azure 中的伺服器。 Contoso ETS 想要讓應用程式擁有者和小組都能管理應用程式。 此應用程式只能在 Contoso 的公司網路上使用。 Dave 必須為此應用程式設定訂用帳戶。 訂用帳戶在未來也會裝載其他開發人員相關的軟體。

### <a name="naming-standards-and-resource-groups"></a>命名標準與資源群組

Dave 會建立一個訂用帳戶，以支援所有業務單位常見的開發人員工具。 Dave 需要針對訂用帳戶和資源群組建立有意義的名稱 (適用於應用程式和網路)。 他會建立下列訂用帳戶和資源群組︰

| Item | Name | 描述 |
| --- | --- | --- |
| Subscription |Contoso ETS DeveloperTools Production |支援一般開發人員工具 |
| Resource group |bitbucket-prod-rg |包含應用程式 Web 伺服器和資料庫伺服器 |
| Resource group |corenetworks-prod-rg |包含虛擬網路和站對站閘道連線 |

### <a name="role-based-access-control"></a>角色型存取控制

建立自己的訂用帳戶之後，Dave 想要確保適當的小組和應用程式擁有者可以存取其資源。 Dave 認為每個小組有不同的需求。 他會使用已從 Contoso 的內部部署 Active Directory 同步處理到 Azure Active Directory 的群組，並提供對小組的正確存取層級。

Dave 為訂用帳戶指派下列角色︰

| 角色 | 指派對象 | 描述 |
| --- | --- | --- |
| [擁有者](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) |來自 Contoso 內部部署 Active Directory 的受控識別碼 |此識別碼會透過 Contoso 的身分識別管理工具，以即時（JIT）存取來控制，並確保訂用帳戶擁有者存取權受到完整的審核 |
| [安全性讀取者](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#security-reader) |安全性和風險管理部門 |此角色可讓使用者查看 Azure 資訊安全中心和資源的狀態 |
| [網路參與者](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#network-contributor) |網路小組 |此角色可讓 Contoso 的網路小組管理站對站 VPN 和虛擬網路 |
| *自訂角色* |應用程式擁有者 |Dave 會建立一個角色，以授與修改資源群組內資源的能力。 如需詳細資訊，請參閱[AZURE RBAC 中的自訂角色](https://docs.microsoft.com/azure/role-based-access-control/custom-roles) |

### <a name="policies"></a>原則

Dave 管理訂用帳戶中資源的需求如下︰

- 因為開發工具可支援世界各地的開發人員，所以他不想要封鎖使用者在任何區域中建立資源。 不過，他需要知道這些資源建立於何處。
- 他很關心成本問題。 因此，他想要防止應用程式擁有者建立貴得離譜的虛擬機器。
- 因為此應用程式可為許多業務單位的開發人員提供服務，所以他想要在每個資源上標記業務單位和應用程式擁有者。 藉由使用這些標記，ETS 即可向適當的小組收費。

他會透過[Azure 原則](https://docs.microsoft.com/azure/azure-policy/azure-policy-introduction)來建立下列原則：

| 欄位 | 影響 | 描述 |
| --- | --- | --- |
| location |稽核 |稽核在任何區域中的資源建立 |
| 類型 |deny |拒絕建立 G 系列虛擬機器 |
| tags |deny |需要應用程式擁有者標籤 |
| tags |deny |需要成本中心標籤 |
| tags |附加 |將標籤名稱 **BusinessUnit** 和標籤值 **ETS** 附加至所有資源 |

### <a name="resource-tags"></a>資源標籤

Dave 了解他需要有帳單上的特定資訊，才能識別 BitBucket 實作的成本中心。 此外，Dave 想要知道 ETS 擁有的所有資源。

他將下列[標籤](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)新增至資源群組和資源。

| 標籤名稱 | 標籤值 |
| --- | --- |
| ApplicationOwner |管理此應用程式的人員名稱 |
| CostCenter |負責支付 Azure 耗用量之群組的成本中心 |
| BusinessUnit |**ETS** (與訂用帳戶相關聯的業務單位) |

### <a name="core-network"></a>核心網路

Contoso ETS 資訊安全性和風險管理小組會審查 Dave 提議將應用程式移至 Azure 的方案。 他們想要確保應用程式不會在網際網路上公開。 Dave 也有未來將會移至 Azure 的開發人員應用程式。 這些應用程式都需要公用介面。 為了符合這些需求，他會提供內部和外部虛擬網路，並提供網路安全性群組來限制存取。

他會建立下列資源︰

| 資源類型 | Name | 描述 |
| --- | --- | --- |
| 虛擬網路 |內部 vnet |搭配 BitBucket 應用程式使用，而且透過 ExpressRoute 連接到 Contoso 的公司網路。 子網路 (`bitbucket`) 會提供特定 IP 位址空間給應用程式 |
| 虛擬網路 |外部 vnet |適用於未來需要公開端點的應用程式 |
| 網路安全性群組 |bitbucket-nsg |只允許在應用程式所在子網路 (`bitbucket`) 的連接埠 443 上進行連線，確保此工作負載的受攻擊面最小化 |

### <a name="resource-locks"></a>資源鎖定

Dave 認為從 Contoso 的公司網路至內部虛擬網路的連線必須受到保護，以免遭遇任何不受控的指令碼或意外刪除。

他會建立下列[資源鎖定](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources)：

| 鎖定類型 | 資源 | 描述 |
| --- | --- | --- |
| **CanNotDelete** |內部 vnet |防止使用者刪除虛擬網路或子網路，但不會防止新增子網路 |

### <a name="azure-automation"></a>Azure 自動化

Dave 無法自動執行此應用程式。 雖然他建立了 Azure 自動化帳戶，但他一開始不會使用它。

### <a name="azure-security-center"></a>Azure 資訊安全中心

Contoso IT 服務管理部門需要快速識別及處理威脅。 他們也想要了解可能存在的問題。

為了滿足這些需求，Dave 會啟用[Azure 資訊安全中心](https://docs.microsoft.com/azure/security-center/security-center-intro)並提供「安全性讀取者」角色的存取權。

## <a name="scenario-2-customer-facing-app"></a>案例 2︰客戶面向應用程式

供應鏈業務單位的業務領導者已找出利用忠誠卡來增加 Contoso 客戶參與的各種機會。 Alice 的小組必須建立此應用程式，並決定 Azure 可提高其滿足商務需求的能力。 Alice 與來自 ETS 組織的 Dave 一起設定兩個可供開發和操作此應用程式的訂用帳戶。

### <a name="azure-subscriptions"></a>Azure 訂用帳戶

Dave 登入 Azure 企業版入口網站，並看到供應鏈部門已經存在。 不過，因為此專案是 Azure 中供應鏈小組的第一個開發專案，所以 Dave 會針對 Alice 的開發小組，辨識出新帳戶的需求。 他替 Alice 的小組建立「研發」帳戶並將存取權指派給她。 Alice 透過 Azure 入口網站登入並建立兩個訂用帳戶︰一個用來保存開發伺服器，一個用來保存生產伺服器。 建立下列訂用帳戶時，她會依循先前建立的命名標準︰

| 訂用帳戶用途 | Name |
| --- | --- |
| 開發 |Contoso SupplyChain ResearchDevelopment LoyaltyCard Development |
| 生產環境 |Contoso SupplyChain Operations LoyaltyCard Production |

### <a name="policies"></a>原則

Dave 和 Alice 一起討論應用程式並認定此應用程式只為北美地區的客戶提供服務。 Alice 和她的小組計劃使用 Azure 的應用程式服務環境和 Azure SQL 來建立應用程式。 它們可能需要在開發期間建立虛擬機器。 Alice 想要確保她的開發人員具有探索和檢查問題所需的資源，而不需透過 ETS 協助。

他們針對**開發訂用帳戶**訂定下列原則︰

| 欄位 | 影響 | 描述 |
| --- | --- | --- |
| location |稽核 |稽核在任何區域中的資源建立 |

它們不會限制使用者可以在開發中建立的 SKU 類型，而且不需要任何資源群組或資源的標籤。

他們針對**生產訂用帳戶**訂定下列原則︰

| 欄位 | 影響 | 描述 |
| --- | --- | --- |
| location |deny |拒絕在美國資料中心以外建立任何資源 |
| tags |deny |需要應用程式擁有者標籤 |
| tags |deny |需要部門標籤 |
| tags |附加 |將標籤附加至每個表示生產環境的資源群組 |

它們不會限制使用者可以在生產環境中建立的 SKU 類型。

### <a name="resource-tags"></a>資源標籤

Dave 了解他需要有帳單上的特定資訊，才能針對收費和擁有權識別正確的業務群組。 他會針對資源群組和資源定義資源標籤。

| 標籤名稱 | 標籤值 |
| --- | --- |
| ApplicationOwner |管理此應用程式的人員名稱 |
| 部門 |負責支付 Azure 耗用量之群組的成本中心 |
| EnvironmentType |**生產** (即使訂用帳戶的名稱包含 **Production**，但包含這個標籤有助於在查看入口網站中或帳單上的資源時輕鬆識別) |

### <a name="core-networks"></a>核心網路

Contoso ETS 資訊安全性和風險管理小組會審查 Dave 提議將應用程式移至 Azure 的方案。 他們想要確保在 DMZ 網路中適度隔離和保護忠誠卡應用程式。 為了滿足此需求，Dave 和 Alice 建立外部虛擬網路和網路安全性群組，以便隔離忠誠卡應用程式與 Contoso 公司網路。

針對**開發訂用帳戶**，他們可建立︰

| 資源類型 | Name | 描述 |
| --- | --- | --- |
| 虛擬網路 |內部 vnet |為 Contoso 忠誠卡開發環境提供服務，而且透過 ExpressRoute 連接到 Contoso 的公司網路 |

對於**生產訂用帳戶**，他們可建立︰

| 資源類型 | Name | 描述 |
| --- | --- | --- |
| 虛擬網路 |外部 vnet |裝載忠誠卡應用程式，但不會直接連接到 Contoso 的 ExpressRoute。 程式碼會透過其原始程式碼系統直接推送至 PaaS 服務。 |
| 網路安全性群組 |loyaltycard-nsg |只允許 TCP 443 的輸入通訊，以確保此工作負載的受攻擊面最小化。 Contoso 也會使用 web 應用程式防火牆進行調查，以提供額外的保護。 |

### <a name="resource-locks"></a>資源鎖定

Dave 和 Alice 協商並決定在環境中的一些重要資源上新增資源鎖定，以防止在錯誤程式碼推播期間遭到意外刪除。

他們可建立下列鎖定︰

| 鎖定類型 | 資源 | 描述 |
| --- | --- | --- |
| **CanNotDelete** |外部 vnet |為了防止人們刪除虛擬網路或子網路。 此鎖定無法防止新增子網路 |

### <a name="azure-automation"></a>Azure 自動化

Alice 和她的開發小組有大量 Runbook 可管理此應用程式的環境。 Runbook 允許新增/刪除應用程式的節點以及其他 DevOps 工作。

為了使用這些 Runbook，他們會啟用[自動化](https://docs.microsoft.com/azure/automation/automation-intro)。

### <a name="azure-security-center"></a>Azure 資訊安全中心

Contoso IT 服務管理部門需要快速識別及處理威脅。 他們也想要了解可能存在的問題。

為了滿足這些需求，Dave 啟用了 Azure 資訊安全中心。 他可以確保 Azure 資訊安全中心正在監視資源，並提供 DevOps 和安全性小組的存取權。

## <a name="next-steps"></a>後續步驟

- 若要了解如何建立 Resource Manager 範本，請參閱[建立 Azure Resource Manager 範本的最佳做法](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices)。
