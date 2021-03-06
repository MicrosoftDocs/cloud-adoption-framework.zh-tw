---
title: Azure 企業 Scaffold
description: Azure enterprise scaffold 現在是適用于 Azure 的 Microsoft 雲端採用架構。 瞭解如何滿足治理的需求，並將其與敏捷的需求進行平衡。
author: rdendtler
ms.author: brblanch
ms.date: 09/22/2018
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: reference
ms.custom: think-tank
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: a0d88b3f6c308c531fd9128e4861babd0769131b
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101786204"
---
<!-- docutune:disable -->
<!-- cSpell:ignore subscope ITSM Hashi -->

# <a name="azure-enterprise-scaffold-prescriptive-subscription-governance"></a>Azure 企業 Scaffold：規定的訂用帳戶治理

> [!NOTE]
> Azure 企業版的架構已整合至 Microsoft 雲端採用架構。 本文中的內容現在以架構的 [就緒方法](../ready/index.md) 表示。 本文將在2020年初淘汰。 若要開始使用新的程式，請參閱備妥的 [方法總覽](../ready/index.md)、 [Azure 登陸區域](../ready/landing-zone/index.md)和 [登陸區域考慮](../ready/considerations/index.md)。

企業日漸採用公用雲端，以獲取其靈活度和彈性。 他們依賴雲端的優勢來產生收益，以及將企業的資源使用優化。 Microsoft Azure 提供許多服務和功能，企業可以像堆積木一樣組合這些服務，以處理各式各樣的工作負載和應用程式。

決定使用 Microsoft Azure 只是獲得雲端優勢的第一步。 第二步是了解企業可如何有效地使用 Azure，並找出需要先準備的基本功能，以處理類似下列的問題：

- 「我擔心資料主權的問題；該如何確保我的資料和系統符合法規需求？」
- 「如何知道每個資源支援什麼，才能精準地斟酌考量並回收成本？」
- 「我想要確保在公用雲端中部署或執行的一切事項都以安全第一的思維作為前提，我該如何推動這件事？」

空白訂用帳戶不具護欄的潛在客戶會很困難。 這個空白空間可能會阻礙您移至 Azure。

本文是技術專業人員處理治理需求並以靈活度需求加以平衡的起點。 文中介紹企業 Scaffold 的概念，引導組織以安全的方式來實作和管理其 Azure 環境。 其中提供的架構可用來開發有效且高效率的控制項。

## <a name="need-for-governance"></a>治理需求

移至 Azure 時，您必須處理早期治理主題，以確保在企業內成功使用雲端。 不幸的是，建立全方位治理系統的時間和體系表示某些事業群會直接接洽提供者，而不需企業 IT 部門參與。 如果沒有妥善管理資源，這種方法可能會讓企業遭受攻擊。 公用雲端 &mdash; 彈性、彈性及耗用量型定價的特性， &mdash; 對於需要快速滿足內部和外部) 客戶 (需求的業務群組而言很重要。 但是企業 IT 必須確保資料和系統受到有效的保護。

建立建築物時，我們使用 Scaffold (鷹架) 來建立結構的基礎。 Scaffold 可引導出大致輪廓，並可為即將裝載的永久系統提供更多錨點。 企業 Scaffold 也是如此︰一組有彈性的控制項和可提供環境結構的 Azure 功能，以及公用雲端上所建置服務的錨點。 這為有交期壓力的建置者 (IT 和事業群) 提供建立和附加新服務的基礎。

Scaffold 是以我們經由與各種規模的用戶端合作而蒐集到的實務作法為基礎。 這些用戶端的範圍從將雲端中的解決方案開發到大型跨國企業，以及遷移工作負載和開發雲端原生解決方案的獨立軟體廠商的小型組織。 企業 scaffold 是「專門打造」的彈性，可支援傳統的 IT 工作負載和敏捷式工作負載，例如建立軟體即服務的開發人員 (SaaS) 以 Azure 平臺功能為基礎的應用程式。

Enterprise scaffold 可以作為 Azure 中每個新訂用帳戶的基礎。 它可讓系統管理員確保工作負載符合組織的最低治理需求，而不會阻礙事業群和開發人員快速達成他們自己的目標。 我們的經驗顯示，這會大幅加速，而不是妨礙公共雲端的成長。

> [!NOTE]
> Microsoft 已發行稱為[Azure 藍圖](/azure/governance/blueprints/overview)的新功能預覽，此功能可讓您封裝、管理及部署訂用帳戶和管理群組上常用的映像、範本、原則和指令碼。 這項功能像是座橋梁，以 Scaffold 的目的作為參考模型，然後將該模型部署到您的組織。

下圖顯示 Scaffold 的元件。 其基礎依賴於管理階層和訂用帳戶的可靠計畫。 其要件是由 Resource Manager 原則和強大的命名標準所組成。 Scaffold 的其餘部分是 Azure 核心功能，而這些功能可打造出安全且易於管理的環境，並與之連線。

![企業 scaffold](../_images/reference/scaffold-v2.png)

## <a name="define-your-hierarchy"></a>定義您的階層

Scaffold 的基礎是 Enterprise 合約 (EA) 註冊到訂用帳戶和資源群組的階層和關聯性。 註冊會從契約的觀點來定義公司內部的 Azure 服務的形狀和使用。 在 Enterprise 合約中，您可以進一步將環境細分成部門、帳戶、訂用帳戶和資源群組，以符合您組織的結構。

![階層](../_images/reference/agreement.png)

Azure 訂用帳戶是內含所有資源的基本單位。 它也可在 Azure 中定義數個限制，例如核心、虛擬網路和其他資源的數目。 資源群組可用來進一步精簡訂用帳戶模型，並啟用更自然的資源群組。

每個企業都不同，上圖中的階層對於在公司內組織 Azure 的方式容許極大的彈性。 將階層模型化以反映貴公司的計費、資源管理和資源存取需求，是您在公用雲端中開始時的第一個和最重要的決策。

### <a name="departments-and-accounts"></a>部門和帳戶

EA 註冊的三種常見模式如下：

- **功能性** 模式：

  ![功能模式](../_images/reference/functional.png)

- **業務單位** 模式：

  ![業務單位模式](../_images/reference/business.png)

- **地理** 模式：

  ![地理模式](../_images/reference/geographic.png)

雖然這些模式中的每一個都有其作用，但愈來愈多人採用 **業務單位** 模式，因為在模型化組織成本模型及反映控制範圍上，此模式具有高度彈性。 Microsoft 核心工程和作業群組已建立以 **聯邦**、**州** 和 **地方** 建立模型的有效 **商務單位** 模式子集。 如需詳細資訊，請參閱組織您的訂用帳戶 [和資源群組](../ready/azure-best-practices/organize-subscriptions.md)。

### <a name="azure-management-groups"></a>Azure 管理群組

Microsoft 現在提供另一種模型來建立階層模型： [Azure 管理群組](/azure/azure-resource-manager/management-groups-overview)。 管理群組比部門和帳戶更有彈性，而且可以嵌套最多六個層級。 管理群組可讓您建立與您的計費階層不同的階層，而這只是為了有效率地管理資源。 管理群組可以反映您的計費階層，而且企業通常會以此方式作為起點。 不過，管理群組的強大功能是在您使用它們來建立組織模型、將相關的訂用帳戶群組在一起 (無論其在計費階層中的位置為何) 並指派一般角色、原則和方案。 部分範例包括：

- **生產與非生產。** 有些企業會建立管理群組來識別其生產和非生產訂閱。 管理群組可讓這些客戶更容易管理角色和原則。 例如，非生產訂用帳戶可能會允許開發人員「參與者」存取，但是在生產環境中，他們只會有「讀者」存取權。
- **內部服務與外部服務的比較。** 企業通常會有不同的需求、原則和內部服務的角色，與客戶面向的服務。

設計完善的管理群組，以及 Azure 原則和方案，都是有效治理 Azure 的骨幹。

### <a name="subscriptions"></a>訂用帳戶

在決定您的部門及帳戶 (或管理群組) 時，您會優先探討如何分配 Azure 環境以符合您的組織。 不過，訂用帳戶是實際工作的發生位置，而此處的決策會影響安全性、擴充性和計費。 許多組織會查看下列模式來作為他們的指引：

- **應用程式/服務：** 訂用帳戶代表應用程式或服務 (應用程式組合) 
- **生命週期：** 訂用帳戶代表服務的生命週期，例如生產或開發。
- **部門：** 訂用帳戶代表組織中的部門。

前兩個模式最常使用，而且這兩者都十分建議使用。 生命週期方法適用於大部分的組織。 在此情況下，一般建議是使用兩個基底訂用帳戶，然後 `Production` `Nonproduction` 使用資源群組來進一步分隔環境。

### <a name="resource-groups"></a>資源群組

Azure Resource Manager 可讓您將資源組織成有意義的群組，以便管理、計費或自然親和性。 資源群組是資源的容器，其中的資源具有共同的生命週期或共用「所有 SQL 伺服器」或「應用程式 A」等屬性。

資源群組不能建立巢狀結構，且資源只能屬於一個資源群組。 某些動作可套用於資源群組中的所有資源。 例如，刪除資源群組即可移除資源群組內的所有資源。 如同訂用帳戶，建立資源群組時有很多常見模式可選，而且在「傳統 IT」工作負載和「敏捷式 IT」工作負載上會有所不同：

- 「傳統 IT」工作負載通常會依照相同生命週期內的項目分組，例如應用程式。 依照應用程式分組，即可進行個別應用程式管理。
- 「敏捷式 IT」工作負載傾向著重於外部客戶面向的雲端應用程式。 資源群組通常會反映出部署 (例如 Web 層或應用程式層) 和管理的層次。

> [!NOTE]
> 了解您的工作負載可協助您開發資源群組策略。 這些模式可以混合搭配及配對。 例如，共用服務資源群組與「敏捷」資源群組位於相同的訂用帳戶中。

## <a name="naming-standards"></a>命名標準

Scaffold 的第一要件是一致的命名標準。 設計良好的命名標準可讓您識別入口網站中、帳單上和指令碼內的資源。 您可能已經有內部部署基礎結構的現有命名標準。 將 Azure 新增至您的環境時，您應該將這些命名標準延伸至 Azure 資源。

> [!TIP]
> 針對命名慣例：
>
> - 盡可能複習並採用 [雲端採用架構命名和標記指引](../ready/azure-best-practices/naming-and-tagging.md) 。 本指南可協助您決定有意義的命名標準，並提供廣泛的範例。
> - 使用 Resource Manager 原則來協助強制執行命名標準。
>
> 請記住，之後若要變更名稱會相當困難，因此現在多花幾分鐘，可省去您之後的麻煩。

您的命名標準應著重於較常使用和搜尋的資源。 例如，為求簡單明瞭，所有資源群組應該遵循強式標準。

### <a name="resource-tags"></a>資源標記

資源標記會密切地配合命名標準。 當有資源新增至訂用帳戶時，資源標記的重要性就會提高，因為它要以邏輯方式將這些資源分類為計費、管理和操作用途。 如需詳細資訊，請參閱 [使用標記組織您的 Azure 資源](/azure/azure-resource-manager/management/tag-resources)。

> [!IMPORTANT]
> 標記可包含個人資訊，因此可能會落在 GDPR 的規範下。 請仔細規劃您的標記管理。 如果您要尋找 GDPR 的一般資訊，請參閱 [服務信任入口網站](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)的 GDPR 一節。

除了計費和管理外，標記還會用在許多方面。 它們通常用來作為自動化的一部分 (請參閱後面章節)。 如果沒有事先考量，這可能會導致衝突。 最佳做法是在企業層級 (（例如 ApplicationOwner 和 CostCenter) ）找出所有的通用標籤，並在使用自動化部署資源時一致地套用這些標記。

## <a name="azure-policy-and-initiatives"></a>Azure 原則與計劃

Scaffold 的第二個要件牽涉到使用 [Azure 原則和計畫](/azure/azure-policy/azure-policy-introduction) 來管理風險，方法是在訂用帳戶中的資源和服務上強制執行規則 (效果) 。 Azure 計畫是用來達成單一目標的原則集合。 原則和計畫會接著指派給資源範圍，以開始強制執行這些原則。

使用稍早所述的管理群組時，原則和方案更能提供更強大的功能。 管理群組讓計劃或原則可指派到一整組訂用帳戶。

### <a name="common-uses-of-resource-manager-policies"></a>Resource Manager 原則的常見用途

原則和計畫是 Azure 工具組中的強大工具。 原則可讓公司提供「傳統 IT」工作負載的控制項，以啟用企業營運應用程式所需的穩定性，同時也允許「敏捷式」工作負載 &mdash; （例如，開發客戶應用程式，而不會讓企業面臨額外的風險）。 最常見的原則模式如下：

- **地區相容性與資料主權。** Azure 有遍布世界各地且持續增加的區域清單。 企業通常需要確保特定範圍中的資源會保留在地理區域中，以滿足法規需求。
- **避免公開公開的伺服器。** Azure 原則可以禁止部署特定的資源類型。 建立原則來拒絕在特定範圍內建立公用 IP，以避免非預期的伺服器暴露在網際網路上，是很常見的情況。
- **成本管理和中繼資料。** 資源標記通常用來將重要的帳單資料新增到資源和資源群組，例如 CostCenter、擁有者等。 對於計費和資源管理的精確度而言，這些標記非常重要。 原則可以將有資源標記的應用程式強制送往所有已部署的資源，讓資源更容易管理。

### <a name="common-uses-of-initiatives"></a>計畫的常見用法

方案可讓企業將邏輯原則分組，並以單一實體的形式來追蹤。 方案可協助企業滿足 agile 和傳統工作負載的需求。 常見的方案用途包括：

- **啟用 Azure 安全性中心內的監視功能。** 這是 Azure 原則中的預設計畫，也是什麼是計畫的絕佳範例。 它可讓您識別未加密的 SQL 資料庫、虛擬機器 (VM) 弱點，以及更常見的安全性相關需求的原則。
- **法規特定的方案。** 企業通常會將常用於法規需求 (例如 HIPAA) 的原則群組在一起，以便更有效率地追蹤控制項和這些控制項的合規性。
- **資源類型和 Sku。** 建立的方案會限制可部署的資源類型以及可部署的 Sku，有助於控制成本，並確保您的組織只會部署您的小組擁有技能集和支援程式的資源。

> [!TIP]
> 我們建議您一律使用計畫定義，而不是原則定義。 將計畫指派給某個範圍 (例如訂用帳戶或管理群組) 之後，您可以輕鬆地將另一個原則新增至計畫，而不必變更任何指派。 這可讓您更容易了解套用內容及追蹤合規性。

### <a name="policy-and-initiative-assignments"></a>原則和計畫指派

建立原則並將這些原則分組為邏輯計畫之後，您必須將原則指派給某個範圍，例如管理群組、訂用帳戶或甚至是資源群組。 指派也可讓您從原則指派中排除子範圍。 比方說，如果您拒絕在某個訂用帳戶內建立公用 IP，您可以建立有排除項目的指派，讓資源群組連線到您受保護的 DMZ。

在此 [GitHub](https://github.com/azure/azure-policy) 存放庫上，您可以找到多個示範原則和計畫如何套用至在 Azure 中各種資源的範例。

## <a name="identity-and-access-management"></a>身分識別和存取管理

開始使用公用雲端時，您會問自己的首要問題之一 (也是最重要的) 就是「誰應該有資源的存取權？」 以及「如何控制此存取權？」 控制 Azure 入口網站和入口網站中資源的存取權，對於雲端中的資產長期安全而言是不可或缺的。

若要保護對資源的存取，您必須先設定您的身分識別提供者，然後設定角色和存取權。 連線至您內部部署 Active Directory 的 Azure Active Directory (Azure AD) 是 Azure 身分識別的基礎。 不過，Azure **ad 與內部** 部署 Active Directory 不同，因此請務必瞭解 azure ad 租使用者是什麼，以及它與您的註冊有何關聯。 查看 [azure 中的資源存取管理](../govern/resource-consistency/resource-access-management.md) ，以充分瞭解 azure AD 和內部部署 Active Directory。 若要將內部部署目錄連線並同步處理至 Azure AD，請在內部部署環境中安裝並設定 [AZURE Ad connect 工具](/azure/active-directory/connect/active-directory-aadconnect) 。

![Active Directory 架構的圖表](../_images/reference/ad-architecture.png)

Azure 最初發行時，訂用帳戶的存取控制是基本的︰系統管理員或共同管理員。 存取傳統模型中的訂用帳戶，意味著存取入口網站中的所有資源。 這缺乏細微的控制會導致訂用帳戶激增，以提供適當的註冊存取控制層級。 不再需要此種訂用帳戶激增情況。 使用 Azure 角色型存取控制 (Azure RBAC) ，您可以將使用者指派給提供常見存取權的標準角色，例如「擁有者」、「參與者」或「讀者」，甚至建立您自己的角色。

在執行 Azure 角色型存取控制時，強烈建議您遵循下列步驟：

- 控制訂用帳戶的系統管理員/共同管理員，因為這些角色具有廣泛的權限。 如果「訂用帳戶擁有者」需要管理 Azure 傳統部署，只需將它們新增為共同管理員即可。
- 使用管理群組來跨多個訂用帳戶指派[角色](/azure/azure-resource-manager/management-groups-overview#management-group-access)，並降低在訂用帳戶層級上管理這些角色的負擔。
- 將 Azure 使用者新增至 Active Directory 中的群組 (例如，應用程式 X 擁有者)。 使用已同步處理的群組，提供群組成員管理資源群組 (包含應用程式) 的適當權限。
- 依照授與執行預期工作所需 **最低權限** 的原則。

> [!IMPORTANT]
> 請考慮使用 [Azure AD Privileged Identity Management](/azure/active-directory/privileged-identity-management/pim-configure)、Azure [Multi-factor Authentication](/azure/active-directory/authentication/howto-mfa-getstarted)和[條件式存取](/azure/active-directory/conditional-access/overview)功能，為您 Azure 訂用帳戶上的系統管理動作提供更佳的安全性和更高的可見性。 這些功能來自有效的 Azure AD Premium 授權 (視功能而訂)，可進一步保護及管理您的身分識別。 Azure AD PIM 可啟用 "Just-in-Time" 管理存取及核准工作流程，以及完整的系統管理員啟用和活動稽核。 Azure 多重要素驗證是另一項重要功能，可讓您進行雙步驟驗證以登入 Azure 入口網站。 如果結合條件式存取控制，您可以有效地管理損害風險。

規劃和準備您的身分識別和存取控制，以及遵循 [Azure 身分識別管理最佳作法](/azure/security/fundamentals/identity-management-best-practices) ，是您可以採用的最佳風險降低策略，而且應該視為每個部署的必要項。

## <a name="security"></a>安全性

過度考量安全性一直是雲端採用的其中一個最大阻礙。 IT 風險管理員和資訊安全部門必須確保 Azure 中的資源會預先受到保護。 Azure 提供您可用來保護資源的功能，同時偵測和消除對這些資源的威脅。

### <a name="azure-security-center"></a>Azure 資訊安全中心

[Azure 資訊安全中心](/azure/security-center/security-center-intro)可讓您統一檢視環境中所有資源的安全性狀態，並提供進階的威脅防護。 Azure 資訊安全中心是一個開放平台，可讓 Microsoft 合作夥伴建立可插入其中的軟體並增強其功能。 Azure 安全性中心的基準功能 (免費層) 提供可加強安全性狀態的評量和建議。 它的付費層可提供額外且有價值的功能，例如即時系統管理員存取權和自我調整應用程式控制 (允許清單) 。
> [!TIP]
> Azure 安全性中心是一項功能強大的工具，可讓您用來偵測威脅和保護企業的新功能，定期改進。 強烈建議您一律啟用 Azure 安全性中心。

### <a name="locks-for-azure-resources"></a>Azure 資源的鎖定

當您的組織將核心服務新增至訂用帳戶時，將會變得越來越重要，以避免業務中斷。 當 Azure 訂用帳戶中執行的腳本或工具不慎刪除了資源時，就會發生一個常見的中斷情形。 [鎖定](/azure/azure-resource-manager/resource-group-lock-resources) 會限制高價值資源的作業，其中修改或刪除這些資源會造成顯著的影響。 您可以將鎖定套用至訂用帳戶、資源群組或個別資源。 將鎖定套用至基礎資源，例如虛擬網路、閘道、網路安全性群組和金鑰儲存體帳戶。

### <a name="secure-devops-kit-for-azure"></a>適用于 Azure 的安全 DevOps 套件

適用于 Azure (AzSK) 的安全 DevOps 套件是一系列的腳本、工具、延伸和自動化功能，最初是由 Microsoft 本身的 IT 團隊所建立，並透過 [GitHub 發行為開放原始](https://github.com/azsk/devopskit-docs)碼。 AzSK 已經考慮至端對端 Azure 訂用帳戶和資源安全性需求，適用于使用廣泛自動化的團隊，並順暢地將安全性整合到原生 DevOps 工作流程，以協助完成具有以下六個焦點區域的安全 DevOps：

- 保護訂用帳戶
- 啟用安全開發
- 將安全性整合到 CI/CD
- 持續保證
- 警示和監視
- 雲端風險治理

![適用于 Azure 的安全 DevOps 套件的總覽圖](../_images/reference/secure-devops-kit.png)

AzSK 是一組豐富的工具、腳本和資訊，這是完整 Azure 治理計畫很重要的一部分，並將其併入您的 scaffold，對於支援組織風險管理目標很重要。

### <a name="azure-update-management"></a>Azure 更新管理

保護您環境安全的主要工作之一是確保伺服器已套用最新的更新。 雖然有許多工具可達成此目的，但 Azure 提供的 [Azure 更新管理](/azure/automation/update-management/overview)解決方案會對重大 OS 修補程式的識別和推出有所因應。 並且會運用 Azure 自動化 (稍後會在本指南的[自動化](#automate)區段中加以說明)。

## <a name="monitor-and-alerts"></a>監視和警示

針對您在 Azure 訂用帳戶中所使用服務的活動、效能計量、健全狀況和可用性，收集和分析遙測資料，是主動管理您的應用程式和基礎結構的關鍵，而且是每個 Azure 訂用帳戶的基本需求。 每個 Azure 服務都會以活動記錄、計量和診斷記錄的形式發出遙測。

- **活動記錄** 會描述對訂用帳戶中的資源執行的所有作業。
- **計量** 是由資源發出的數值資訊，可描述資源的效能和健康情況。
- **診斷記錄** 是由 Azure 服務發出，提供有關該服務作業的豐富、經常性資料。

這項資訊可以在多個層級上查看和處理，並持續改善。 Azure 透過下圖中所述的服務，提供 Azure 資源的 **共用**、 **核心** 和 **深層** 監視功能。

![描述深層應用程式監視、深層基礎結構監視、核心監視和共用功能的圖表。](../_images/reference/monitoring.png)

### <a name="shared-capabilities"></a>共用功能

- **警示：** 您可以從 Azure 資源收集每個記錄、事件和計量，但不能收到重大條件和動作的通知，這項資料僅適用于歷史用途和辯論。 Azure 警示會針對您在所有應用程式和基礎結構上定義的條件，主動發出通知。 您可以在使用動作群組的記錄、事件和計量之間建立警示規則，以通知收件者集合。 動作群組也可讓您使用外部動作 (例如 Webhook) 來執行 Azure 自動化 Runbook 和 Azure Functions，以自動化補救作業。

- **儀表板：** 儀表板可讓您匯總監視視圖，並將資料合併到資源與訂用帳戶，讓您可以整個企業查看 Azure 資源的遙測。 您可以建立並設定您自己的檢視，然後與他人共用。 例如，您可以建立包含各種不同磚的儀表板，讓資料庫管理員在所有 Azure 資料庫服務中提供資訊，包括 Azure SQL Database、適用于于 postgresql 的 azure DB 和適用于 MySQL 的 Azure DB。

- **計量瀏覽器：** 計量是 Azure 資源所產生的數值 (例如% CPU 或磁片 i/o) ，可讓您深入瞭解資源的作業與效能。 使用計量瀏覽器，您可以定義和傳送您對 Log Analytics 感興趣的計量，以進行匯總和分析。

### <a name="core-monitoring"></a>核心監視

- **Azure 監視器：** Azure 監視器是核心平臺服務，提供監視 Azure 資源的單一來源。 Azure 監視器的 Azure 入口網站介面可針對 Azure 中的所有監視功能提供集中式的跳躍點，包括 Application Insights、Log Analytics、網路監視、管理解決方案和服務對應的深層監視功能。 您可以使用 Azure 監視器來視覺化、查詢、路由、封存，以及針對來自整個雲端資產的 Azure 資源所提供的計量和記錄採取行動。 除了入口網站之外，您還可以透過監視器 PowerShell Cmdlet、跨平臺 CLI 或 Azure 監視器 REST Api 來取得資料。

- **Azure Advisor：** Azure Advisor 會持續監視您訂用帳戶和環境內的遙測。 此外，它也會建議對 Azure 資源進行成本優化的最佳作法，並改善應用程式資源的效能、安全性和可用性。

- **Azure 服務健康狀態：** Azure 服務健康狀態可識別任何可能影響您應用程式的 Azure 服務問題，並協助您規劃排程維護時段。

- **活動記錄：** 活動記錄會描述訂用帳戶中資源上的所有作業。 它會提供一個審核 _記錄，以_ 判斷任何在資源上建立、更新、刪除 _作業的__內容_。 活動記錄檔事件會儲存在平臺中，而且可供查詢90天。 您可以將活動記錄內嵌至 Log Analytics，以獲得較長的保留期間，以及跨多個資源更深入的查詢和分析。

### <a name="deep-application-monitoring"></a>深層應用程式監視

- **Application Insights：** Application Insights 可讓您收集應用程式專屬的遙測，以及監視雲端或內部部署中應用程式的效能、可用性和使用情形。 藉由使用適用于多種語言的支援 Sdk 來檢測您的應用程式，包括 .NET、JavaScript、JAVA、Node.js、Ruby 和 Python。 Application Insights 事件會內嵌至支援基礎結構和安全性監視的相同 Log Analytics 資料存放區，讓您可以透過豐富的查詢語言，讓一段時間中的事件相互關聯並加以彙總。

### <a name="deep-infrastructure-monitoring"></a>深層基礎結構監視

- **Log Analytics：** Log Analytics 會從各種來源收集遙測和其他資料，並提供查詢語言和分析引擎，讓您深入瞭解應用程式和資源的作業，藉此在 Azure 監視中扮演著重要的角色。 您可以透過快速的記錄搜尋和流覽來直接與 Log Analytics 資料互動，也可以在其他 Azure 服務中流量分析工具，將其資料儲存在 Log Analytics 中，例如 Application Insights 或 Azure 資訊安全中心。

- **網路監視：** Azure 的網路監視服務可讓您深入瞭解網路流量、效能、安全性、連線能力和瓶頸。 規劃良好的網路設計應包括設定 Azure 網路監視服務，例如網路監看員和 ExpressRoute 監視器。

- **管理解決方案：** 管理解決方案是一組封裝的邏輯、深入解析和預先定義的 Log Analytics 查詢，適用于應用程式或服務。 這些解決方案以 Log Analytics 作為基礎來儲存和分析事件資料。 管理解決方案範例包含監視容器和 Azure SQL Database 分析。

- **服務對應：** 服務對應可讓您以圖形方式查看基礎結構元件、其進程，以及其他電腦和外部進程的相依性。 它會整合 Log Analytics 中的事件、效能資料和管理解決方案。

> [!TIP]
> 建立個別警示之前，請先建立並維護一組可在 Azure 警示之間共用的動作群組。 這可讓您集中維護收件者清單的生命週期、通知傳遞方法 (電子郵件、SMS 電話號碼) ，以及 webhook 至外部動作 (Azure 自動化 runbook、Azure 函式和邏輯應用程式、ITSM) 。

## <a name="cost-management"></a>成本管理

從內部部署雲端移到公用雲端時，您會面臨的其中一個重大變更就是從資本支出 (購買硬體) 切換到營運支出 (支付所使用的服務)。 此參數也需要更仔細地管理您的成本。 雲端的好處是，您只需要在不需要時關閉或調整其大小，就可以徹底影響您所使用服務的成本。 刻意管理您在雲端中的成本是一個最佳作法，也就是成熟客戶每天進行的工作。

Microsoft 提供數個工具，可協助您視覺化、追蹤和管理您的成本。 我們也提供一組完整的 API，可讓您自訂成本管理，並將其整合至您自己的工具和儀表板中。 這些工具會鬆散分組為 Azure 入口網站功能和外部功能。

### <a name="azure-portal-capabilities"></a>Azure 入口網站功能

這些工具可為您提供成本的即時資訊，以及採取動作的能力。

- **訂用帳戶資源成本：** 在入口網站中， [Azure 成本管理](/azure/cost-management-billing/cost-management-billing-overview) 視圖可讓您快速查看資源或資源群組每日支出的成本和資訊。
- **Azure 成本管理：** 這可讓您管理及分析您的 Azure 費用，以及您在其他公用雲端提供者上的支出。 免費和付費層都有絕佳的功能。
- **Azure 預算和動作群組：** 先瞭解什麼是成本，並對其執行一些工作，直到最近有更多手動練習為止。 隨著 Azure 預算和其 Api 的推出，現在您可以建立在成本達到閾值時執行的 [動作](https://channel9.msdn.com/Shows/Azure-Friday/Managing-costs-with-the-Azure-Budgets-API-and-Action-Groups) 。 例如，您可以在達到其預算的100% 時關閉「測試」資源群組。
- **Azure Advisor：** 知道成本只占一半的部分，另外一半也知道要如何處理這項資訊。 [Azure Advisor](/azure/advisor/advisor-overview) 會提供節省成本、改善可靠性或甚至是提高安全性所應採取的動作建議。

### <a name="external-cost-management-tools"></a>外部成本管理工具

<!-- TODO: Content packs are deprecated. -->

- **POWER BI Azure 使用量見解：** 您是否要為您的組織建立自己的視覺效果？ 若是如此，則適用于 Power BI 的 Azure 使用量見解內容套件是您選擇的工具。 您可以使用此內容套件和 Power BI 來建立自訂視覺效果，以代表您的組織、對成本進行更深入的分析，並新增其他資料來源，以供進一步擴充。

- **Azure 使用量 api：**[使用量 api](/rest/api/consumption)可讓您以程式設計方式存取成本和使用量資料，以及預算、保留實例和 marketplace 費用的資訊。 這些 Api 只能供 EA 註冊和某些 Web Direct 訂用帳戶存取，不過它們可讓您將成本資料整合到您自己的工具和資料倉儲中。 您也可以透過 [AZURE CLI 存取這些 api](/cli/azure/consumption)。

身為長期和成熟雲端使用者的客戶，會遵循特定的最佳作法：

- **主動監視成本。** 身為成熟 Azure 使用者的組織會時常監視成本，並在有需要時採取動作。 某些組織甚至讓專員執行使用量的分析和建議變更，當這些人第一次找到執行多個月但未使用的 HDInsight 叢集時，組織就可以回本了。
- **使用 Azure 保留的 VM 實例。** 管理雲端成本的另一個重要原則是使用適用於作業的工具。 如果您有必須全天候維持的 IaaS VM，則使用保留的實例將可為您省下可觀的費用。 在自動關閉 Vm 和使用保留實例之間尋找適當的平衡，將會取得經驗與分析。
- **有效使用自動化。** 許多工作負載不需要每天執行。 每天關閉一段四小時的 VM，即可節省15% 的成本。 自動化的回本速度相當快。
- **使用資源標記以取得可見度。** 如同本文中其他地方所述，使用資源標記會提供更好的成本分析。

成本管理是專業領域，目的是要有效且高效率地執行公用雲端。 達成成功的企業可以控制其成本，並使其符合實際需求，而不是過度購買和希望的需求。

## <a name="automate"></a>自動化

造成組織 (使用雲端提供者) 成熟度差異的功能有很多，其中一個是已併入的自動化程度。 自動化是永無止盡的程序，而且隨著您的組織移到雲端，任何區域的建置都需要您投入資源和時間。 自動化有許多用途，包括一致的資源推出 (直接系結至另一個核心 scaffold 概念、範本和 DevOps) 至補救問題。 自動化是 Azure Scaffold 的「連結組織」，用來將每個區域連結在一起。

有幾個工具可協助您從第一方工具（例如 Azure 自動化、事件方格和 Azure CLI）建立這項功能，到大量的協力廠商工具（例如 Terraform、Jenkins、Chef 和 Puppet）。 核心自動化工具組括 Azure 自動化、事件方格和 Azure Cloud Shell。

- **Azure 自動化** 是一種雲端式功能，可讓您以 PowerShell 或 Python) 撰寫 runbook (，並可讓您自動化程式、設定資源，甚至是套用修補程式。 [Azure 自動化](/azure/automation/automation-intro)具有一組廣泛的跨平台功能，並且可整合至您的部署，但因為範圍太廣泛，無法在此深入說明。
- **事件方格** 是完全受控的事件路由系統，可讓您對 Azure 環境內的事件做出回應。 如同 Azure 自動化是成熟雲端組織的連線組織， [事件方格](/azure/event-grid) 是良好自動化的連線組織。 您可以使用事件方格建立簡單的無伺服器動作，以在每次建立新資源時傳送電子郵件給系統管理員，並將該資源記錄至資料庫。 同樣的事件方格可以在刪除資源和從資料庫中移除項目時，發出通知。
- **Azure Cloud Shell** 是以瀏覽器為基礎的互動式 [shell](/azure/cloud-shell/overview) ，可讓您在 Azure 中管理資源。 其提供可視需要啟動 PowerShell 或 Bash 的完整環境 (還可以為您進行維護)，讓您擁有統一的環境可執行指令碼。 Azure Cloud Shell 可讓您存取已安裝的其他重要工具--用以自動化您的環境，包括 [AZURE CLI](/cli/azure/get-started-with-azure-cli)、 [Terraform](/azure/virtual-machines/linux/terraform-install-configure) ，以及用來管理容器、資料庫 (sqlcmd) 等其他 [工具](https://azure.microsoft.com/updates/cloud-shell-new-cli-tools-and-font-size-selection) 的不斷成長清單。

自動化是一項全職的工作，而且很快就會成為雲端小組內最重要的作業工作之一。 採用「自動化優先」方法的組織在使用 Azure 上有更高的成就：

- **管理成本：** 積極地尋求機會和建立自動化來調整資源大小、擴大或縮小規模，以及關閉未使用的資源。
- **操作彈性：** 透過自動化 (以及範本和 DevOps) ，您可以獲得提升可用性、提高安全性，並讓您的小組專注于解決商務問題的重複性層級。

## <a name="templates-and-devops"></a>範本和 DevOps

如同自動化區段中所強調的，您的目標應該是讓組織透過由原始程式碼控制的範本和指令碼來佈建資源，並將環境的互動式設定降到最低。 使用此「基礎結構即程式碼」方法及用於持續部署的嚴謹 DevOps 程序，即可確保環境中的一致性並減少漂移。 幾乎每個 Azure 資源都可透過 [Azure Resource MANAGER JSON 範本](/azure/azure-resource-manager/resource-group-template-deploy) 來部署，搭配 PowerShell 或 azure 跨平臺 CLI 以及像是 Terraform by HashiCorp 的工具，其具有一流的支援並與 Azure Cloud Shell) 整合。

[使用 Azure Resource manager 範本的最佳作法（例如使用 Azure Resource manager 範本的最佳作法](/archive/blogs/mvpawardprogram/azure-resource-manager)）可提供最佳作法的絕佳討論，以及使用[azure DevOps](/azure/devops/user-guide)工具鏈將 DevOps 方法套用至 azure Resource manager 範本所學習到的經驗。 請花時間和精力開發一組您組織需求專屬的核心範本，並使用 DevOps 工具鏈 (（例如 Azure DevOps、Jenkins、Bamboo、TeamCity 和 Concourse) ）來開發持續傳遞管線，特別是針對您的生產環境和 QA 環境。 GitHub 上有一個大型的 [Azure 快速入門範本](https://github.com/azure/azure-quickstart-templates) 程式庫，您可以使用這些範本作為範本的起點，也可以使用 Azure DevOps 快速建立雲端式傳遞管線。

作為生產訂用帳戶或資源群組的最佳做法，您的目標應該使用 Azure RBAC 安全性，根據預設不允許互動式使用者，並根據服務主體使用自動化的持續傳遞管線來布建所有資源，並傳遞所有的應用程式程式碼。 系統管理員或開發人員應接觸 Azure 入口網站，以互動方式設定資源。 此層級的 DevOps 會採取一致的努力，並使用 Azure scaffold 的所有概念，提供一致且更安全的環境，以符合您組織的規模需求。

> [!TIP]
> 當設計和開發複雜的 Azure Resource Manager 範本時，您可使用[連結範本](/azure/azure-resource-manager/resource-group-linked-templates)來組織及重構整合型 JSON 檔案中的複雜資源關聯性。 這可讓您個別管理資源，並讓您的範本更容易讀取、可供測試及重複使用。

Azure 是超大規模雲端提供者。 當您將組織從內部部署伺服器移至雲端時，依賴雲端提供者和 SaaS 應用程式所使用的相同概念，將可協助組織更有效率地回應業務的需求。

## <a name="core-network"></a>核心網路

Azure Scaffold 參考模型的最後一個元件是，您的組織要如何以安全的方式存取 Azure。 資源的存取可以是內部的 (在公司網路內) 或外部的 (透過網際網路)。 您組織中的使用者很容易不小心將資源放在錯誤的位置，並可能加以開啟而遭到惡意存取。 針對內部部署裝置，企業必須加上適度的控制，以確保 Azure 使用者能做出適當的決策。 針對訂用帳戶治理，我們會找出可提供基本存取控制的核心資源。 核心資源是由下列各項所組成︰

- **虛擬網路** 是子網路的容器物件。 雖然不是絕對必要，但通常使用於將應用程式連接到內部公司資源時。
- **使用者定義的路由** 可讓您操作子網內的路由表，讓您可以透過網路虛擬裝置或對等互連虛擬網路上的遠端閘道，來傳送流量。
- **虛擬網路對等互連** 可讓您在 Azure 中順暢地連接兩個或多個虛擬網路，以建立更複雜的中樞和輪輻設計或共用服務網路。
- **服務端點。** 在過去，PaaS 服務會仰賴不同方法來保護您虛擬網路中這些資源的存取。 服務端點可讓您安全地存取已啟用的 PaaS 服務， **只** 來自已連線的端點，進而提高整體安全性。
- **安全性群組** 是一組廣泛的規則，可讓您允許或拒絕進出 Azure 資源的輸入和輸出流量。 [安全性群組](/azure/virtual-network/security-overview) 是由可透過 **服務** 標籤增強的安全性規則所組成 (定義常見的 Azure 服務（例如 Azure KEY Vault 或 azure SQL Database）) 和 **應用程式安全性群組** (定義和應用程式結構，例如 web 伺服器或應用程式伺服器) 。

> [!TIP]
> 使用網路安全性群組中的服務標記和應用程式安全性群組來：
>
> - 增強您規則的可讀性，這對了解影響至關重要。
> - 在較大的子網中啟用有效的微分割，減少蔓延和增加彈性。

<!-- TODO: Refactor VDC content below. -->
<!-- docutune:ignore "Azure Virtual Datacenter" -->

### <a name="azure-virtual-datacenter"></a>Azure 虛擬資料中心

Azure 提供來自廣泛合作夥伴網路的內部和協力廠商功能，提供您有效的安全性立場。 更重要的是，Microsoft 會以 [Azure 虛擬資料中心 (VDC) ](./networking-vdc.md)的形式提供最佳作法和指引。 當您從單一工作負載移至使用混合式功能的多個工作負載時，VDC 指導方針將提供您「食譜」來啟用彈性的網路，隨著您在 Azure 中的工作負載成長，將會隨之成長。

## <a name="next-steps"></a>下一步

治理是 Azure 成功的重要關鍵。 本文是以企業 Scaffold 的技術實作為目標，但只論及更廣泛的處理程序和元件之間的關聯性。 原則治理會從上而下流動，並由需要達成的業務來決定。 當然，Azure 治理模型的建立納入 IT 部門代表，但更重要的是應該具有來自事業群領導者及安全性和風險管理階層的強大代表性。 最後，企業 Scaffold 與降低業務風險有關，以達成組織的任務與目標。

既然您已了解訂用帳戶治理，現在就可以參閱這些實務建議。 如需詳細資訊，請參閱 [Azure 就緒的最佳作法](../ready/azure-best-practices/index.md)。
