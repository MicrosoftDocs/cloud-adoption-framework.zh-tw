---
title: 部署移轉基礎結構
description: 了解 Contoso 如何設定移轉至 Azure 的 Azure 基礎結構。
author: deltadan
ms.author: abuck
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: b044c86618ccafbfe69106a5f3c7e7d53db68fce
ms.sourcegitcommit: 8b82889dca0091f3cc64116f998a3a878943c6a1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/09/2020
ms.locfileid: "89605237"
---
<!-- cSpell:ignore untrust CIDR RRAS CONTOSODC sysvol ITIL NSGs ASGs -->

# <a name="deploy-a-migration-infrastructure"></a>部署移轉基礎結構

本文示範虛構公司 Contoso 如何準備其內部部署基礎結構以便進行移轉、設定 Azure 基礎結構以準備進行移轉，以及在混合式環境中執行業務。

當您使用此範例來協助規劃您自己的基礎結構遷移工作時，請記住，所提供的範例架構是 Contoso 專用的。 在對訂用帳戶設計或網路架構做出重要的基礎結構決策時，請檢查您組織的商務需求、結構和技術需求。

您是否需要本文所說明的所有元素，取決於您的移轉策略。 例如，如果您只在 Azure 中建立雲端原生應用程式，則可能需要較不復雜的網路結構。

## <a name="overview"></a>概觀

Contoso 必須先將 Azure 基礎結構準備就緒，才能遷移至 Azure。 通常，Contoso 需要考慮六個區域：

> [!div class="checklist"]
>
> - **步驟1： Azure 訂用帳戶。** 它會如何購買 Azure，並與 Azure 平臺和服務互動？
> - **步驟2：混合式身分識別。** 在移轉之後，其會如何管理及控制對內部部署和 Azure 資源的存取？ 它如何將身分識別管理擴充或移至雲端？
> - **步驟3：嚴重損壞修復和恢復能力。** 它會如何確保其應用程式和基礎結構在中斷和嚴重損壞發生時具有復原能力？
> - **步驟4：網路。** 如何設計網路基礎結構，並建立其內部部署資料中心與 Azure 之間的連線能力？
> - **步驟5：安全性。** 它會如何保護混合式部署的安全？
> - **步驟6：管理。** 它會如何讓部署符合安全性和治理需求？

## <a name="before-you-start"></a>開始之前

開始查看基礎結構之前，請考慮閱讀相關 Azure 功能的一些背景資訊：

- 有幾個選項可用來購買 Azure 存取權，包括隨用隨付訂用帳戶、Microsoft Enterprise 合約 (EA) 、Microsoft 轉銷商的 Open 授權，或雲端解決方案提供者 (CSP) 方案中的 Microsoft 合作夥伴購買。 請了解[購買選項](https://azure.microsoft.com/pricing/purchase-options)，並閱讀如何[組織 Azure 訂用帳戶](https://azure.microsoft.com/blog/organizing-subscriptions-and-resource-groups-within-the-enterprise)的相關資訊。
- 深入瞭解 Azure 身分 [識別與存取管理 (IAM) ](https://www.microsoft.com/security/business/identity)。 瞭解 [Azure Active Directory (Azure AD) ，以及將內部部署 Active Directory 延伸至雲端](/azure/active-directory/fundamentals/active-directory-whatis)。
- Azure 提供穩固的網路基礎結構，並提供混合式連線的選項。 請取得[網路功能和網路存取控制](/azure/security/security-network-overview)的概觀。
- 閱讀 [Azure 安全性的簡介](/azure/security/fundamentals/overview) ，並瞭解如何建立 [Azure 治理](/azure/governance)的方案。

## <a name="on-premises-architecture"></a>內部部署架構

以下圖表顯示目前的 Contoso 內部部署基礎結構。

![Contoso 架構的圖表。](./media/contoso-migration-infrastructure/contoso-architecture.png)

_圖1： Contoso 內部部署架構。_

- Contoso 在美國東部有一個位於紐約城市的主要資料中心。
- 在全美另有三家地區性分公司。
- 主要資料中心透過光纖 Metro Ethernet 連線連接到網際網路， (500 Mbps) 。
- 每個分支都會透過商務級連線，在本機連線到網際網路，並透過 IPsec VPN 通道回主要資料中心。 這種方法可讓整個網路永久連線，並將網際網路連線優化。
- 主要資料中心已透過 VMware 完全虛擬化。 Contoso 有兩部 ESXi 6.5 虛擬化主機受 vCenter Server 6.5 管理。
- Contoso 會使用 Active Directory 來進行身分識別管理和網域名稱系統 (內部網路上的 DNS) 伺服器。
- 資料中心內的網域控制站會在 VMware 虛擬機器上執行， (Vm) 。 地區分公司的網域控制站會在實體伺服器上執行。

## <a name="step-1-buy-and-subscribe-to-azure"></a>步驟 1：購買和訂閱 Azure

Contoso 必須瞭解如何購買 Azure、如何管理訂用帳戶，以及如何授權服務和資源。

### <a name="buy-azure"></a>購買 Azure

Contoso 正在 [Enterprise 合約](https://azure.microsoft.com/pricing/enterprise-agreement)中註冊。 本合約需要預先預付 Azure 的承諾用量。 Contoso 讓 Contoso 獲得權益，例如彈性的計費選項和優化的定價。

詳細資料如下：

- Contoso 估算將須支付多少 Azure 年費。 Contoso 在簽署合約時，即已支付第一年的所有費用。
- Contoso 必須使用所有承諾，年份才會超過或失去這些金額的價值。
- 如果基於某些原因，Contoso 超過承諾用量並花費更多費用，Microsoft 將會針對差異進行發票。
- 因前述承諾用量而產生的任何費用，都會採用 Contoso 合約中所載的相同費率。 超量不會有任何罰款。

### <a name="manage-subscriptions"></a>管理訂用帳戶

向 Azure 支付費用後，Contoso 必須了解如何管理 Azure 訂用帳戶。 由於 Contoso 有 EA，因此它可以建立的 Azure 訂用帳戶數目沒有任何限制。 Azure Enterprise 合約註冊會定義公司圖形和使用 Azure 服務的方式，並定義核心治理結構。

在第一個步驟中，Contoso 定義了一種結構，稱為 *企業 scaffold* 進行註冊。 Contoso 使用 [Azure enterprise scaffold 指引](/azure/azure-resource-manager/resource-manager-subscription-governance) 來協助您瞭解及設計 scaffold。

現在，Contoso 已決定使用功能性方法來管理訂用帳戶：

- 在企業內，它會使用單一 IT 部門來控制 Azure 預算。 這將是唯一具有訂用帳戶的群組。
- Contoso 會在未來擴充此模型，讓其他公司群組可以加入註冊階層中的部門。
- 在 IT 部門內部，Contoso 具有結構化兩個訂用帳戶 `Production` 和 `Development` 。
- 如果 Contoso 在未來需要更多訂用帳戶，則也需要管理這些訂用帳戶的存取、原則和合規性。 Contoso 會藉由將 [Azure 管理群組](/azure/azure-resource-manager/management-groups-overview) 引進為訂用帳戶的額外層級，來達成此目的。

![企業階層架構的圖表。](./media/contoso-migration-infrastructure/enterprise-structure.png)
  
_圖2：企業階層。_

### <a name="examine-licensing"></a>檢查授權

在設定訂用帳戶後，Contoso 可以查看 Microsoft 授權。 授權策略會取決於 Contoso 要遷移至 Azure 的資源，以及如何在 Azure 中選取和部署 Vm 和服務。

#### <a name="azure-hybrid-benefit"></a>Azure Hybrid Benefit

若要在 Azure 中部署 Vm，標準映射包含的授權將收取 Contoso 每分鐘的費用，以供所使用的軟體使用。 不過，Contoso 一直是 Microsoft 的長期客戶，並透過軟體保證維護 EAs 和 open 授權。

Azure Hybrid Benefit 可提供符合成本效益的方法來進行遷移。 它可讓 Contoso 藉由轉換或重複使用 Windows Server Datacenter 和軟體保證所涵蓋的標準版授權，來節省 Azure Vm 和 SQL Server 工作負載。 這可讓 Contoso 針對 Vm 和 SQL Server 支付較低的基礎計算費率。 如需詳細資訊，請參閱 [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit)。

#### <a name="license-mobility"></a>授權流動性

藉軟體保證而實現的授權行動性讓 Microsoft 大量授權客戶（例如 Contoso）有彈性可在 Azure 上使用主動式軟體保證部署合格的伺服器應用程式。 這樣就不需要購買新的授權。 現有的授權不會產生相關行動費用，可輕易部署在 Azure 中。 如需詳細資訊，請參閱 [Azure 上透過軟體保證的授權機動性](https://azure.microsoft.com/pricing/license-mobility)。

#### <a name="reserved-instances-for-predictable-workloads"></a>可預測工作負載的保留實例

執行的 Vm 一律需要可預測的工作負載，例如 SAP ERP 系統等企業營運應用程式。 無法預測的工作負載是變數，例如在高需求期間開啟的 Vm，以及當需求很低時關閉的 Vm。

![Azure 保留的虛擬機器執行個體的圖表。](./media/contoso-migration-infrastructure/reserved-instance.png)

_圖3： Azure 保留的虛擬機器執行個體。_

在 exchange 中，針對必須長期維護的特定 VM 實例使用保留實例時，Contoso 可以同時取得折扣和優先的容量。 使用 [Azure 保留的虛擬機器執行個體](https://azure.microsoft.com/pricing/reserved-vm-instances) 搭配 Azure Hybrid Benefit 可將 Contoso 省下最多82% 的標準隨用隨付定價 (自2018年4月的) 。

## <a name="step-2-manage-hybrid-identity"></a>步驟 2：管理混合式身分識別

使用身分識別和存取管理來提供和控制使用者對 Azure 資源的存取，是將 Azure 基礎結構提取在一起的重要步驟。

Contoso 公司決定將其內部部署 Active Directory 擴充至雲端，而不是在 Azure 中建置新的個別系統。 因為 Contoso 尚未使用 Microsoft 365，所以需要布建 Azure AD 實例。 如果 Contoso 使用 Microsoft 365，則它已經有現有的 Azure AD 租使用者和目錄，可以用來做為主要的 Azure AD 實例。

深入瞭解 [Microsoft 365 身分識別模型和 Azure Active Directory](/office365/enterprise/about-office-365-identity)。 您也可以瞭解如何 [將 Azure 訂用帳戶關聯或新增至您的 Azure Active Directory 租使用者](/azure/active-directory/fundamentals/active-directory-how-subscriptions-associated-directory)。

### <a name="create-an-azure-ad-directory"></a>建立 Azure AD 目錄

Contoso 會使用 Azure 訂用帳戶隨附的 Azure AD Free 版本。 Contoso 管理員會建立 Azure AD 目錄：

1. 在[Azure 入口網站](https://portal.azure.com)中，他們會移至**建立資源**身分  >  **識別**  >  **Azure Active Directory**。

1. 在 [ **建立目錄**] 中，他們會指定目錄的名稱、初始功能變數名稱，以及應建立目錄的區域。

   ![建立 Azure AD 目錄之選取專案的螢幕擷取畫面。](./media/contoso-migration-infrastructure/azure-ad-create.png)

   _圖4：建立 Azure AD 目錄。_

> [!NOTE]
> 所建立的目錄在表單中具有初始功能變數名稱 `domain-name.onmicrosoft.com` 。 此名稱無法變更或刪除。 系統管理員必須將其已註冊的功能變數名稱新增至 Azure AD。

### <a name="add-the-domain-name"></a>新增網域名稱

若要使用標準功能變數名稱，Contoso 管理員必須將它新增為 Azure AD 的自訂功能變數名稱。 此選項可讓他們指派熟悉的使用者名稱。 例如，使用者可以使用電子郵件地址來登入， `billg@contoso.com` 而不是登入 `billg@contosomigration.microsoft.com` 。

若要設定自訂功能變數名稱，系統管理員會將它新增至目錄、新增 DNS 專案，然後在 Azure AD 中確認名稱。

1. 在**自訂功能變數名稱**  >  **新增自訂**網域時，他們會新增網域。
2. 若要在 Azure 中使用 DNS 專案，他們必須向其網域註冊機構註冊：

    - 在 [自訂網域名稱]**** 清單中，他們記下名稱的 DNS 資訊。 它使用 MX 記錄。
    - 他們需要存取名稱伺服器。 他們會登入 `contoso.com` 網域，並使用記下的詳細資料，為 Azure AD 所提供的 DNS 專案建立新的 MX 記錄。

3. 在 DNS 記錄傳播之後，他們會選取 [ **驗證** ] 來檢查網域詳細資料中的自訂功能變數名稱。

    ![顯示 Azure Active Directory D N S 之選取範圍的螢幕擷取畫面。](./media/contoso-migration-infrastructure/azure-ad-dns.png)

    _圖5：檢查功能變數名稱。_

### <a name="set-up-on-premises-and-azure-groups-and-users"></a>設定內部部署和 Azure 群組與使用者

既然已建立 Azure AD 目錄，Contoso 管理員必須將員工新增至將同步處理至 Azure AD 的內部部署 Active Directory 群組。 他們應該使用與 Azure 中的資源群組名稱相符的內部部署群組名稱。 這可讓您更輕鬆地找出同步用途的相符項。

#### <a name="create-resource-groups-in-azure"></a>在 Azure 中建立資源群組

Azure 資源群組會將 Azure 資源收集在一起。 使用資源群組識別碼可讓 Azure 對群組內的資源執行作業。

Azure 訂用帳戶可以有多個資源群組。
資源群組存在於單一訂用帳戶中。 此外，單一資源群組可以有多個資源。 資源屬於單一資源群組。

Contoso 管理員會設定 Azure 資源群組，如下表所示。

| 資源群組 | 詳細資料 |
| --- | --- |
| `ContosoCobRG` | 此群組包含與商務持續性相關的所有資源。 它包含 Contoso 將用於 Azure Site Recovery 服務和 Azure 備份服務的保存庫。 <br><br> 它也包含用於遷移的資源，包括 Azure Migrate 和 Azure 資料庫移轉服務。 |
| `ContosoDevRG` | 此群組包含開發/測試資源。 |
| `ContosoFailoverRG` | 此群組可做為容錯移轉資源的登陸區域。 |
| `ContosoNetworkingRG` | 此群組包含所有網路資源。 |
| `ContosoRG` | 此群組包含與生產應用程式和資料庫相關的資源。 |

他們依照下列方式建立資源群組：

1. 在 Azure 入口網站 > [資源群組]**** 中，他們新增群組。
2. 針對每個群組，他們會指定名稱、群組所屬的訂用帳戶，以及區域。
3. 資源群組會出現在 [資源群組]**** 清單中。

   ![顯示資源群組清單的螢幕擷取畫面](./media/contoso-migration-infrastructure/resource-groups.png)

   _圖6：資源群組。_

##### <a name="scale-resource-groups"></a>調整資源群組

日後，Contoso 會根據需求來新增其他資源群組。 例如，它可能會定義每個應用程式或服務的資源群組，讓每個應用程式或服務都可以獨立管理和保護。

#### <a name="create-matching-security-groups-on-premises"></a>在內部建立相符的安全性群組

在內部部署 Active Directory 實例中，Contoso 管理員會設定安全性群組，其名稱會與 Azure 資源群組的名稱相符。

![顯示內部部署 Active Directory 安全性群組的螢幕擷取畫面。](./media/contoso-migration-infrastructure/on-premises-ad.png)

_圖7：內部部署 Active Directory 安全性群組。_

為了方便管理，他們額外建立了將會新增至所有其他群組的群組。 此群組將有權使用 Azure 中的所有資源群組。 系統會將有限數目的全域管理員新增至此群組。

### <a name="synchronize-active-directory"></a>同步處理 Active Directory

Contoso 想要提供一般身分識別，用來存取內部部署和雲端中的資源。 若要這樣做，它會將內部部署 Active Directory 實例與 Azure AD 整合。 使用此模型，使用者和組織可以利用單一身分識別來存取內部部署應用程式和雲端服務，例如 Microsoft 365 或網際網路上上千個其他網站。 系統管理員可以使用 Active Directory 中的群組，在 Azure 中執行以 [角色為基礎的存取控制 (RBAC) ](/azure/role-based-access-control/role-assignments-portal) 。

為了加速整合，Contoso 使用 [Azure AD Connect 工具](/azure/active-directory/connect/active-directory-aadconnect)。 當您在網域控制站上安裝並設定此工具時，它會同步處理內部部署 Active Directory 身分識別，以 Azure AD。

### <a name="download-the-tool"></a>下載工具

1. 在 Azure 入口網站中，Contoso 管理員會移至**Azure Active Directory**  >  **Azure AD Connect** ，並將最新版本的工具下載至他們用來進行同步處理的伺服器。

    ![顯示下載 Azure A D Connect 之連結的螢幕擷取畫面。](./media/contoso-migration-infrastructure/download-ad-connect.png)

    _圖8：下載 Azure AD Connect。_

2. 他們使用 [ `AzureADConnect.msi` **快速設定**] 開始安裝。 這是最常見的安裝，可用於具有密碼雜湊同步處理的單一樹系拓撲以進行驗證。

    ![顯示 Azure AD Connect Wizard 的螢幕擷取畫面。](./media/contoso-migration-infrastructure/ad-connect-wiz1.png)

    _圖9： Azure AD Connect Wizard。_

3. 在 **[連接到 Azure AD]** 中，他們會指定以表單或) 連接至 Azure AD (的認證 `admin@contoso.com` `admin@contoso.onmicrosoft.com` 。

    ![螢幕擷取畫面：顯示 Azure A D Connect Wizard 的 [連線到 Azure] D 頁面。](./media/contoso-migration-infrastructure/ad-connect-wiz2.png)

    _圖10： Azure AD Connect Wizard：連接到 Azure AD。_

4. 在 **[連接到 AD DS]** 中，他們會以表單或) 指定內部部署目錄 (的認證 `CONTOSO\admin` `contoso.com\admin` 。

    ![螢幕擷取畫面：顯示 Azure A D Connect Wizard 的 [連接到 D S] 頁面。](./media/contoso-migration-infrastructure/ad-connect-wiz3.png)

    _圖11： Azure AD Connect Wizard：連接到 AD DS。_

5. 在 [準備好設定]**** 頁面中，他們會選取 [在設定完成時開始同步處理程序]**** 以立即啟動同步。 然後，他們進行安裝。

    注意下列事項：

    - Contoso 直接連線至 Azure。 如果您的內部部署 Active Directory 實例位於 proxy 後方，請參閱 [Azure AD 連線能力的疑難排解](/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-connectivity)。

    - 第一次同步處理之後，內部部署 Active Directory 物件會顯示在 Azure AD 目錄中。

      ![顯示 Azure Active Directory 中可見的內部部署 Active Directory 物件的螢幕擷取畫面。](./media/contoso-migration-infrastructure/on-premises-ad-groups.png)

      _圖12： Azure AD 中可見的內部部署 Active Directory 物件。_

    - Contoso IT 小組會在每個群組中表示，並根據其角色。

      ![顯示群組成員資格的螢幕擷取畫面。](./media/contoso-migration-infrastructure/on-premises-ad-group-members.png)

      _圖13：群組成員資格。_

### <a name="set-up-rbac"></a>設定 RBAC

Azure [RBAC](/azure/role-based-access-control/role-assignments-portal) 可讓您對 azure 進行更細緻的存取管理。 藉由使用 RBAC，您可以只授與使用者執行工作所需的存取權數量。 您可以在特定範圍層級，將適當的 RBAC 角色指派給使用者、群組及應用程式。 角色指派的範圍可以是訂用帳戶、資源群組或單一資源。

然後，Contoso 管理員會將角色指派給他們從內部部署同步處理的 Active Directory 群組。

1. 在 `ControlCobRG` 資源群組中，他們會選取 [**存取控制] (IAM) **  >  **新增角色指派**]。
2. 在「**新增角色指派**  >  **角色**  >  **參與者**」中，他們會 `ContosoCobRG` 從清單中選取安全性群組。 接著，群組就會出現在 [選取的成員]**** 清單中。
3. 他們以相同的許可權對其他資源群組重複此步驟 (但 `ContosoAzureAdmins`) ，方法是將 **參與者** 許可權新增至符合資源群組的安全性群組。
4. 針對 `ContosoAzureAdmins` 安全性群組，他們會指派「 **擁有** 者」角色。

    ![顯示內部部署 Azure Active Directory 群組的螢幕擷取畫面。](./media/contoso-migration-infrastructure/on-premises-ad-groups.png)

    _圖14：將角色指派給安全性群組。_

## <a name="step-3-design-for-resiliency"></a>步驟3：針對復原設計

### <a name="set-up-regions"></a>設定區域

Azure 資源會部署在區域內。 區域會組織成地理位置。 資料落地、主權、合規性及復原需求都可在地理界限內接受。

區域是由一組資料中心所組成。 這些資料中心部署在定義有延遲的邊緣網路，並透過區域低延遲網路進行連線。

每個 Azure 區域都會與另一個區域配對，以提供復原能力。 閱讀 [Azure 區域](https://azure.microsoft.com/global-infrastructure/regions)的相關資訊，並了解[區域的配對方式](/azure/best-practices-availability-paired-regions)。

Contoso 已決定使用 `East US 2` 位於佛吉尼亞) 的 (作為主要區域，並將 `Central US` (位於愛荷華州) 作為次要區域，原因如下：

- Contoso 的資料中心位於紐約，Contoso 已考量過最近資料中心的延遲。
- `East US 2` 具有 Contoso 需要的所有服務和產品。 並非所有 Azure 區域都有相同的產品和服務可供使用。 如需詳細資訊，請參閱 [依區域的 Azure 產品](https://azure.microsoft.com/global-infrastructure/services)。
- `Central US` 是適用于的 Azure 配對區域 `East US 2` 。

Contoso 在考量混合式環境時，必須考量如何將復原能力和災害復原策略建置到區域設計中。 最簡單的策略是單一區域部署，依賴 Azure 平臺功能（例如容錯網域和區域配對）來進行復原。 最複雜的是完整的主動-主動模型，在此模型中會部署雲端服務和資料庫，並提供兩個區域的使用者服務。

Contoso 決定採取折衷方式。 它會在主要區域中部署應用程式與資源，並在次要區域中保留基礎結構的完整複本。 有了這項策略之後，如果發生完整的應用程式嚴重損壞或區域性失敗，複本就可以作為完整備份。

### <a name="set-up-availability"></a>設定可用性

#### <a name="availability-sets"></a>可用性設定組

可用性設定組可協助保護應用程式和資料免于本機硬體和資料中心內的網路中斷。 可用性設定組會將 Azure Vm 分散到資料中心內的實體硬體。

容錯網域代表資料中心內具有通用電源和網路開關的基礎硬體。 可用性設定組中的 Vm 會分散到多個容錯網域，以將單一硬體或網路故障所造成的中斷情況降到最低。

更新網域代表可以同時進行維護或重新啟動的基礎硬體。 可用性設定組也會將 Vm 分散到多個更新網域，以確保至少有一個實例會在任何時間執行。

每當 VM 工作負載需要高可用性時，Contoso 就會實作可用性設定組。 如需詳細資訊，請參閱 [在 Azure 中管理 Windows vm 的可用性](/azure/virtual-machines/windows/manage-availability)。

#### <a name="availability-zones"></a>可用性區域

可用性區域可協助保護應用程式和資料免于影響區域內整個資料中心的失敗。

每個可用性區域都代表一個 Azure 區域內的唯一實體位置。 每個區域皆由一或多個配備獨立電力、冷卻系統及網路的資料中心所組成。

所有已啟用的 Azure 區域中都至少有三個不同的可用性區域。 Azure 區域內可用性區域的實體區隔可保護應用程式和資料不受資料中心故障影響。

只要應用程式需要更高的擴充性、可用性和復原能力，Contoso 就會使用可用性區域。 如需詳細資訊，請參閱 [Azure 中的區域和可用性區域](/azure/availability-zones/az-overview)。

### <a name="configure-backup"></a>設定備份

#### <a name="azure-backup"></a>Azure 備份

您可以使用 Azure 備份來備份和還原 Azure VM 磁片。

Azure 備份允許自動備份儲存在 Azure 儲存體中的 VM 磁片映射。 備份的應用程式一致，可確保備份的資料在交易上是一致的，而且應用程式會在還原後啟動。

Azure 備份支援本機冗余儲存體 (LRS) 在發生本機硬體故障時，複寫資料中心內的多個備份資料複本。 如果發生區域性中斷，Azure 備份也支援異地冗余儲存體 (GRS) ，以將備份資料複寫至次要配對區域。

Azure 備份使用 AES-256 來加密傳輸中的資料。 待用備份的資料會透過 [Azure 儲存體加密](/azure/storage/common/storage-service-encryption)來加密。

Contoso 會在所有生產 Vm 上使用 Azure 備份搭配 GRS，以確保工作負載資料會進行備份，並可在發生中斷時快速還原。 如需詳細資訊，請參閱 [AZURE VM 備份的總覽](/azure/backup/backup-azure-vms-introduction)。

### <a name="set-up-disaster-recovery"></a>設定災害復原

#### <a name="azure-site-recovery"></a>Azure Site Recovery

Azure Site Recovery 可讓商務應用程式和工作負載在區域性中斷期間執行，以協助確保商務持續性。

Azure Site Recovery 會持續將 Azure Vm 從主要區域複寫到次要區域，以確保兩個位置的功能性複本。 當主要區域發生中斷時，應用程式或服務會容錯移轉，以使用次要區域中複寫的 VM 實例。 此容錯移轉可將潛在的中斷降至最低。 當作業恢復正常時，應用程式或服務可以容錯回復到主要區域中的 Vm。

Contoso 會針對任務關鍵性工作負載中使用的所有生產環境 Vm 實行 [Azure Site Recovery](/azure/site-recovery/site-recovery-overview) ，以確保主要區域中斷期間的中斷情況降到最少。

## <a name="step-4-design-a-network-infrastructure"></a>步驟 4：設計網路基礎結構

有了區域設計之後，Contoso 就可以開始考慮網路原則。 它需要思考內部部署資料中心與 Azure 如何彼此進行連線和通訊，以及如何在 Azure 中設計網路基礎結構。 具體而言，Contoso 必須：

- **規劃混合式網路連線能力。** 了解其會如何連接內部部署和 Azure 之間的網路。
- **設計 Azure 網路基礎結構。** 決定其會如何跨區域部署網路。 網路會如何在相同區域內和跨區域進行通訊？
- **設計及設定 Azure 網路。** 設定 Azure 網路和子網路，並決定哪些項目位於其中。

### <a name="plan-hybrid-network-connectivity"></a>規劃混合式網路連線

Contoso 針對 Azure 與內部部署資料中心之間的混合式網路考量了[多種架構](/azure/architecture/reference-architectures/hybrid-networking)。 如需詳細資訊，請參閱[選擇將內部部署網路連線到 Azure 的解決方案](/azure/architecture/reference-architectures/hybrid-networking/considerations)。

提醒您，Contoso 內部部署網路基礎結構目前包含紐約的資料中心，以及美國東部半地區的當地分支。 所有位置都有與網際網路的商務級連線。 接著，每個分支會透過網際網路透過 IPsec VPN 通道連線至資料中心。

![Contoso 網路的圖表。](./media/contoso-migration-infrastructure/contoso-networking.png)

_圖15： Contoso 網路。_

以下是 Contoso 決定實作混合式連線的方式：

1. 在紐約與兩個 Azure 區域之間的 Contoso 資料中心之間設定新的站對站 VPN 連線， `East US 2` 以及 `Central US` 。
2. 針對 Azure 中的虛擬網路所系結的分公司流量，會路由傳送到主要的 Contoso 資料中心。
3. 當 Contoso 相應增加 Azure 部署時，它會在資料中心與 Azure 區域之間建立 Azure ExpressRoute 連線。 在這種情況下，Contoso 會基於容錯移轉這個唯一的原因，保留 VPN 站對站連線。
    - 深入瞭解如何 [在 VPN 和 ExpressRoute 混合式解決方案之間進行選擇](/azure/architecture/reference-architectures/hybrid-networking/considerations)。
    - 確認 [ExpressRoute 位置和支援](/azure/expressroute/expressroute-locations-providers)。

**僅限 VPN：**

![顯示 Contoso VPN 的螢幕擷取畫面。](./media/contoso-migration-infrastructure/hybrid-vpn.png)

_圖16： Contoso VPN。_

**VPN 和 ExpressRoute：**

![顯示 Contoso VPN 和 ExpressRoute 的螢幕擷取畫面。](./media/contoso-migration-infrastructure/hybrid-vpn-expressroute.png)

_圖17： Contoso VPN 和 ExpressRoute。_

### <a name="design-the-azure-network-infrastructure"></a>設計 Azure 網路基礎結構

Contoso 的網路設定必須讓混合式部署成為安全且可擴充的。 Contoso 正在採用長期的方法，設計虛擬網路以提供復原能力和企業就緒。 如需詳細資訊，請參閱 [規劃虛擬網路](/azure/virtual-network/virtual-network-vnet-plan-design-arm)。

為了連接兩個區域，Contoso 會執行中樞對中樞網路模型。 在每個區域中，Contoso 都將使用中樞和輪輻模型。 為了連接網路和中樞，Contoso 將使用 Azure 網路對等互連。

#### <a name="network-peering"></a>網路對等互連

Azure 中的[虛擬網路對等互連](/azure/virtual-network/virtual-network-peering-overview)會連接虛擬網路和中樞。 全域對等互連允許不同區域中的虛擬網路或中樞之間的連線。 本機對等互連會連接相同區域中的虛擬網路。

虛擬網路對等互連提供幾項優點：

- 對等互連虛擬網路之間的網路流量為私用。
- 虛擬網路之間的流量會保留在 Microsoft 骨幹網路上。 虛擬網路之間的通訊不需要公用網際網路、閘道或加密。
- 對等互連可在不同虛擬網路中的資源之間提供預設、低延遲、高頻寬的連線。

#### <a name="hub-to-hub-model-across-regions"></a>跨區域的中樞對中樞模型

Contoso 會在每個區域中部署中樞。 中樞是 Azure 中的虛擬網路，可作為內部部署網路的連線中心點。 中樞虛擬網路會透過全域虛擬網路對等互連彼此連線，這會跨 Azure 區域連接虛擬網路。 每個區域中的中樞會分別與其他區域中的夥伴中樞配對。 中樞會對等互連至其區域中的每個網路，而且可以連線到所有網路資源。

![全域對等互連的圖表。](./media/contoso-migration-infrastructure/global-peering.png)

_圖18：全域對等互連。_

#### <a name="hub-and-spoke-model-within-a-region"></a>區域內的中樞和輪輻模型

在每個區域中，Contoso 會將不同用途的虛擬網路部署為來自區域中樞的輪輻網路。 區域內的虛擬網路會使用對等互連來連線到其中樞，以及彼此之間的連接。

#### <a name="design-the-hub-network"></a>設計中樞網路

在中樞和輪輻模型內，Contoso 需要考慮來自內部部署資料中心和來自網際網路的流量將如何路由傳送。 以下是 Contoso 決定如何處理 `East US 2` 和中樞的路由 `Central US` ：

- Contoso 正在設計網路，以允許來自網際網路的流量，以及從公司網路使用 Azure 的 VPN。
- 此網路架構有兩個界限，即不受信任的前端周邊區域和後端受信任的區域。
- 防火牆在每個區域中都會有網路介面卡，用以控制對受信任區域的存取。
- 從網際網路：
  - 網際網路流量會送到周邊網路上的負載平衡公用 IP 位址。
  - 此流量會透過防火牆路由傳送，並受限於防火牆規則。
  - 網路存取控制實作之後，流量將會轉送至受信任區域中的適當位置。
  - 來自虛擬網路的輸出流量會透過使用者定義的路由路由傳送至網際網路。 流量會透過防火牆強制執行，並與 Contoso 原則一起檢查。
- 從 Contoso 資料中心：
  - 透過站對站 VPN 或 ExpressRoute 的連入流量會達到 Azure VPN 閘道的公用 IP 位址。
  - 流量會透過防火牆根據防火牆規則進行路由。
  - 在應用程式的防火牆規則之後，流量會轉送至受信任內部區域子網上 (標準 SKU) 的內部負載平衡器。
  - 透過 VPN 將來自信任子網的輸出流量傳送至內部部署資料中心，並透過防火牆路由傳送。 規則會在流量通過 VPN 站對站連線之前套用。

### <a name="design-and-set-up-azure-networks"></a>設計和設定 Azure 網路

使用網路和路由拓撲時，Contoso 就可以開始設定 Azure 網路和子網：

<!-- docsTest:casing "class-A" "class-B" -->

- Contoso 會在 Azure () 中執行類別為專用的網路 `10.0.0.0/8` 。 這適用于內部部署;它目前有 () 的類別 B 私人位址空間 `172.160.0.0/16` 。 Contoso 可以確定位址範圍之間不會有任何重迭。
- Contoso 會在主要和次要區域中部署虛擬網路。
- Contoso 會使用包含前置 `VNET` 詞和區域縮寫或的命名慣例 `EUS2` `CUS` 。 使用此標準，中樞網路將會命名 `VNET-HUB-EUS2` 在 `East US 2` 區域和 `VNET-HUB-CUS` `Central US` 區域中。

#### <a name="virtual-networks-in-east-us-2"></a>中的虛擬網路 `East US 2`

`East US 2` 是 Contoso 將用來部署資源和服務的主要區域。 以下是 Contoso 將在該區域中設計網路的方式：

- **中樞：** 中的中樞虛擬網路 `East US 2` 會被視為 Contoso 對內部部署資料中心的主要連線能力。
- **虛擬網路：** 如有必要，您可以使用中的輪輻虛擬網路 `East US 2` 來隔離工作負載。 除了中樞虛擬網路，Contoso 會在中有兩個輪輻虛擬網路 `East US 2` ：
  - `VNET-DEV-EUS2`. 此虛擬網路會為開發/測試團隊提供適用于開發專案的全功能網路。 它會作為生產試驗區域，且其運作將依賴產生基礎結構。
  - `VNET-PROD-EUS2`. Azure IaaS 生產元件將位於此網路中。
  
  每個虛擬網路都有自己的唯一位址空間，而不會重迭。 Contoso 打算在不需要 (NAT) 的網路位址轉譯的情況下設定路由。
- **子網：** 每個網路中將會有每個應用層的子網。 生產網路中的每個子網在開發虛擬網路中都會有相符的子網。 生產網路具有適用于網域控制站的子網。

下表摘要說明中的虛擬網路 `East US 2` 。

| 虛擬網路 | 範圍 | 對等 |
| --- | --- | --- |
| `VNET-HUB-EUS2` | `10.240.0.0/20` | `VNET-HUB-CUS2`, `VNET-DEV-EUS2`, `VNET-PROD-EUS2` |
| `VNET-DEV-EUS2` | `10.245.16.0/20` | `VNET-HUB-EUS2` |
| `VNET-PROD-EUS2` | `10.245.32.0/20` | `VNET-HUB-EUS2`, `VNET-PROD-CUS` |

![主要區域中的中樞和輪輻模型的圖表。](./media/contoso-migration-infrastructure/primary-hub-peer.png)

_圖19：中樞和輪輻模型。_

#### <a name="subnets-in-the-east-us-2-hub-network-vnet-hub-eus2"></a>網路中的子網 `East US 2 Hub` (`VNET-HUB-EUS2`) 

| 子網路/區域 | CIDR | 可用的 IP 位址 |
| --- | --- | --- |
| `IB-UntrustZone` | `10.240.0.0/24`  | 251 |
| `IB-TrustZone`   | `10.240.1.0/24`  | 251 |
| `OB-UntrustZone` | `10.240.2.0/24`  | 251 |
| `OB-TrustZone`   | `10.240.3.0/24`  | 251 |
| `GatewaySubnet`  | `10.240.10.0/24` | 251 |

#### <a name="subnets-in-the-east-us-2-development-network-vnet-dev-eus2"></a>開發網路中的子網 `East US 2` (`VNET-DEV-EUS2`) 

開發小組使用開發虛擬網路作為生產試驗區域。 它有三子網路。

| 子網路 | CIDR | 位址 | 在子網路中 |
| --- | --- | --- | --- |
| `DEV-FE-EUS2` | `10.245.16.0/22` | 1019 | 前端/網路層 Vm |
| `DEV-APP-EUS2` | `10.245.20.0/22` | 1019 | 應用程式層 VM |
| `DEV-DB-EUS2` | `10.245.24.0/23` | 507 | 資料庫 VM |

#### <a name="subnets-in-the-east-us-2-production-network-vnet-prod-eus2"></a>生產網路中的子網 `East US 2` (`VNET-PROD-EUS2`) 

Azure IaaS 元件位於生產網路中。 每個應用層都有自己的子網。 子網會與開發網路中的子網相符，並新增網域控制站的子網。

| 子網路 | CIDR | 位址 | 在子網路中 |
| --- | --- | --- | --- |
| `PROD-FE-EUS2` | `10.245.32.0/22` | 1019 | 前端/網路層 Vm |
| `PROD-APP-EUS2` | `10.245.36.0/22` | 1019 | 應用程式層 VM |
| `PROD-DB-EUS2` | `10.245.40.0/23` | 507 | 資料庫 VM |
| `PROD-DC-EUS2` | `10.245.42.0/24` | 251 | 網域控制站 VM |

![中樞網路架構的圖表。](./media/contoso-migration-infrastructure/azure-networks-eus2.png)

_圖20：中樞網路架構。_

#### <a name="virtual-networks-in-central-us-secondary-region"></a> (次要區域中的虛擬網路 `Central US`) 

`Central US` 是 Contoso 的次要地區。 Contoso 會透過下列方式建構其中的網路：

- **中樞：** 中的中樞虛擬網路 `Central US` 會被視為內部部署資料中心連線的次要點。 `Central US`如有必要，您可以使用中的輪輻虛擬網路來隔離工作負載，與其他輪輻分開管理。
- **虛擬網路：** Contoso 會在下列兩個虛擬網路中 `Central US` ：
  - `VNET-PROD-CUS`：這是生產網路，可視為次要中樞。
  - `VNET-ASR-CUS`：此虛擬網路將作為從內部部署容錯移轉之後建立 Vm 的位置，或做為 Azure Vm 從主要區域容錯移轉至次要區域的位置。 此網路類似于生產網路，但沒有任何網域控制站。
  
  區域中的每個虛擬網路都有自己的位址空間，而不會重迭。 Contoso 會設定不需要 NAT 的路由。
- **子網：** 子網將會以類似的方式設計 `East US 2` 。

下表摘要說明中的虛擬網路 `Central US` 。

| 虛擬網路 | 範圍 | 對等 |
| --- | --- | --- |
| `VNET-HUB-CUS` | `10.250.0.0/20` | `VNET-HUB-EUS2`, `VNET-ASR-CUS`, `VNET-PROD-CUS` |
| `VNET-ASR-CUS` | `10.255.16.0/20` | `VNET-HUB-CUS`, `VNET-PROD-CUS` |
| `VNET-PROD-CUS` | `10.255.32.0/20` | `VNET-HUB-CUS`, `VNET-ASR-CUS`, `VNET-PROD-EUS2` |

![配對區域中的中樞和輪輻模型的圖表。](./media/contoso-migration-infrastructure/paired-hub-peer.png)

_圖21：配對區域中的中樞和輪輻模型。_

#### <a name="subnets-in-the-central-us-hub-network-vnet-hub-cus"></a>中樞網路中的子網 `Central US` (`VNET-HUB-CUS`) 

| 子網路 | CIDR | 可用的 IP 位址 |
| --- | --- | --- |
| `IB-UntrustZone` | `10.250.0.0/24` | 251 |
| `IB-TrustZone` | `10.250.1.0/24` | 251 |
| `OB-UntrustZone` | `10.250.2.0/24` | 251 |
| `OB-TrustZone` | `10.250.3.0/24` | 251 |
| `GatewaySubnet` | `10.250.2.0/24` | 251 |

#### <a name="subnets-in-the-central-us-production-network-vnet-prod-cus"></a>生產網路中的子網 `Central US` (`VNET-PROD-CUS`) 

與主要區域中的生產網路平行 (`East US 2`) ，次要區域中的生產網路 (`Central US`) 。

| 子網路 | CIDR | 位址 | 在子網路中 |
| --- | --- | --- | --- |
| `PROD-FE-CUS` | `10.255.32.0/22` | 1019 | 前端/網路層 Vm |
| `PROD-APP-CUS` | `10.255.36.0/22` | 1019 | 應用程式層 VM |
| `PROD-DB-CUS` | `10.255.40.0/23` | 507 | 資料庫 VM |
| `PROD-DC-CUS` | `10.255.42.0/24` | 251 | 網域控制站 VM |

#### <a name="subnets-in-the-central-us-failoverrecovery-network-vnet-asr-cus"></a>容錯移轉/復原網路中的子網 `Central US` (`VNET-ASR-CUS`) 

`VNET-ASR-CUS`網路用於區域間的容錯移轉。 Site Recovery 將用來在區域之間複寫和容錯移轉 Azure VM。 它也可做為 Azure 網路的 Contoso 資料中心，以用於保留內部部署但容錯移轉至 Azure 以進行嚴重損壞修復的受保護工作負載。

`VNET-ASR-CUS` 是與美國東部2的生產虛擬網路相同的基本子網，但不需要網域控制站子網。

| 子網路 | CIDR | 位址 | 在子網路中 |
| --- | --- | --- | --- |
| `ASR-FE-CUS` | `10.255.16.0/22` | 1019 | 前端/網路層 Vm |
| `ASR-APP-CUS` | `10.255.20.0/22` | 1019 | 應用程式層 VM |
| `ASR-DB-CUS` | `10.255.24.0/23` | 507 | 資料庫 VM |

![中樞網路架構的圖表。](./media/contoso-migration-infrastructure/azure-networks-cus.png)

_圖22：中樞網路架構。_

#### <a name="configure-peered-connections"></a>設定對等互連連線

每個區域中的中樞將會對等互連至另一個區域中的中樞，以及中樞區域內的所有虛擬網路。 此設定可讓中樞進行通訊，以及查看區域內的所有虛擬網路。 請注意，對等互連會建立雙側連接。 其中一個是來自第一個虛擬網路上的起始對等，另一個則是在第二個虛擬網路上。

在混合式部署中，在對等節點之間傳送的流量必須可從內部部署資料中心與 Azure 之間的 VPN 連線檢視。 若要啟用此設定，Contoso 必須使用對等互連連接上的特定設定。 針對從輪輻虛擬網路到內部部署資料中心的任何連線，Contoso 必須允許轉送流量，並透過 VPN 閘道。

##### <a name="domain-controller"></a>網域控制站

針對網路中的網域控制站 `VNET-PROD-EUS2` ，Contoso 想要在 `EUS2` 中樞/生產網路之間，以及透過 VPN 連線，在內部部署之間流動流量。 若要這樣做，Contoso 管理員必須允許下列各項：

1. 在對等連線上**允許轉送的流量**和**允許閘道傳輸組態**。 在我們的範例中，這會是從 `VNET-HUB-EUS2` 到的連接 `VNET-PROD-EUS2` 。

    ![顯示選取核取方塊的螢幕擷取畫面，可允許轉送的流量並允許閘道傳輸。](./media/contoso-migration-infrastructure/peering1.png)

    _圖23：對等互連連接。_

2. **允許轉送的流量** ，並在與之間的連接上，使用對等互連另一端的 **遠端閘道** `VNET-PROD-EUS2` `VNET-HUB-EUS2` 。

    ![顯示所選核取方塊的螢幕擷取畫面，可允許轉送的流量和使用遠端閘道。](./media/contoso-migration-infrastructure/peering2.png)

    _圖24：對等互連連接。_

3. 在內部部署中，他們會設定靜態路由，以將本機流量導向跨 VPN 通道路由傳送至虛擬網路。 此設定會在閘道上完成，提供從 Contoso 到 Azure 的 VPN 通道。 它們會針對靜態路由使用 (RRAS) 的路由及遠端存取服務。

    ![顯示靜態路由選取範圍的螢幕擷取畫面。](./media/contoso-migration-infrastructure/peering3.png)

    _圖25：對等互連連接。_

##### <a name="production-networks"></a>生產網路

輪輻對等網路無法透過中樞看到另一個區域中的輪輻對等網路。 針對這兩個區域中的 Contoso 的生產網路，Contoso 管理員必須為和建立直接對等互連連接 `VNET-PROD-EUS2` `VENT-PROD-CUS` 。

![建立 direct 對等互連連接的圖表。](./media/contoso-migration-infrastructure/peering4.png)

_圖26：建立直接對等互連連接。_

### <a name="set-up-dns"></a>設定 DNS

當您在虛擬網路中部署資源時，您會有數種網域名稱解析可供選擇。 您可以使用 Azure 所提供的名稱解析，或提供 DNS 伺服器以進行解析。 您使用的名稱解析類型取決於資源需要如何彼此通訊。 取得關於 Azure DNS 服務的[詳細資訊](/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#azure-provided-name-resolution)。

Contoso 管理員認為 Azure DNS 服務在混合式環境中並非理想的選擇。 相反地，他們會使用內部部署 DNS 伺服器。 詳細資料如下：

- 因為這是混合式網路，所以內部部署和 Azure 中的所有 Vm 都必須能夠解析名稱，才能正常運作。 這表示自訂 DNS 設定必須套用至所有虛擬網路。
- Contoso 目前已在 Contoso 資料中心和分公司部署網域控制站 (Dc) 。 主要 DNS 伺服器 `contosodc1` (`172.16.0.10`) 和 `contosodc2` (`172.16.0.1`) 。
- 部署虛擬網路之後，內部部署網域控制站會設定為網路中的 DNS 伺服器。
- 如果為虛擬網路指定了選擇性的自訂 DNS，則 `168.63.129.16` 必須將 Azure 中遞迴解析程式的虛擬 IP 位址新增至清單。 若要這樣做，Contoso 會在每個虛擬網路上設定 DNS 伺服器設定。 例如，網路的自訂 DNS 設定如下所示 `VNET-HUB-EUS2` ：

    ![顯示自訂 DNS 設定的螢幕擷取畫面。](./media/contoso-migration-infrastructure/custom-dns.png)

    _圖27：自訂 DNS。_

除了內部部署網域控制站以外，Contoso 還會執行四個網域控制站，以支援每個區域的 Azure 網路 (兩個) ：

| Region | DC | 虛擬網路 | 子網路 | IP 位址 |
| --- | --- | --- | --- | --- |
| `East US 2` | `contosodc3` | `VNET-PROD-EUS2` | `PROD-DC-EUS2` | `10.245.42.4` |
| `East US 2` | `contosodc4` | `VNET-PROD-EUS2` | `PROD-DC-EUS2` | `10.245.42.5` |
| `Central US` | `contosodc5` | `VNET-PROD-CUS` | `PROD-DC-CUS` | `10.255.42.4` |
| `Central US` | `contosodc6` | `VNET-PROD-CUS` | `PROD-DC-CUS` | `10.255.42.4` |

在部署內部部署網域控制站之後，Contoso 必須在其中一個區域的網路上更新 DNS 設定，以將新的網域控制站納入 DNS 伺服器清單中。

#### <a name="set-up-domain-controllers-in-azure"></a>在 Azure 中設定網域控制站

在更新網路設定之後，Contoso 管理員即可在 Azure 中建置網域控制站。

1. 在 Azure 入口網站中，他們會將新的 Windows Server VM 部署到適當的虛擬網路。
2. 他們會在 VM 的每個位置中 [建立可用性設定組](/azure/virtual-machines/windows/tutorial-availability-sets) 。 可用性設定組可確保 Azure 網狀架構會將 Vm 區分為 Azure 區域中的不同基礎結構。 可用性設定組也可讓 Contoso 符合 Azure 中 Vm 的99.95% 服務等級協定 (SLA) 資格。

    ![顯示建立可用性設定組的螢幕擷取畫面。](./media/contoso-migration-infrastructure/availability-group.png)

    _圖28：可用性設定組。_

3. 部署 VM 之後，他們會對 VM 開放網路介面。 他們將私人 IP 位址設定為靜態，並指定有效的位址。

    ![顯示 VM 網路介面連線的螢幕擷取畫面。](./media/contoso-migration-infrastructure/vm-nic.png)

    _圖29： VM NIC。_

4. 他們會將新的資料磁片連結至 VM。 此磁片包含 Active Directory 資料庫和 SYSVOL 共用。

   磁碟的大小將會決定它所支援的 IOPS 數目。 經過一段時間後，磁片大小可能需要隨著環境成長而增加。

   > [!NOTE]
   > 磁片不應該設定為主機快取的讀取/寫入。 Active Directory 資料庫不支援此功能。

   ![顯示 Active Directory 磁片的螢幕擷取畫面。](./media/contoso-migration-infrastructure/ad-disk.png)

   _圖30： Active Directory 磁片。_

5. 新增磁片之後，它們會透過遠端桌面服務連接到 VM，並開啟伺服器管理員。

6. 在 [檔案 **和存放服務**] 中，他們會執行新的磁片區 Wizard。 它們可確保在本機 VM 上將磁碟機號指派為 F 或以上。

    ![顯示新的磁片區嚮導的螢幕擷取畫面。](./media/contoso-migration-infrastructure/volume-wizard.png)

    _圖31：新的磁片區 Wizard。_

7. 在伺服器管理員中，他們新增 [Active Directory Domain Services]**** 角色。 接著，他們將 VM 設定為網域控制站。

    ![顯示選取伺服器角色的螢幕擷取畫面。](./media/contoso-migration-infrastructure/server-role.png)

    _圖32：新增伺服器角色。_

8. 將 VM 設定為 DC 並重新啟動之後，他們會開啟 [DNS 管理員]，並將 Azure DNS 解析程式設定為轉寄站。 這可讓 DC 轉送它無法在 Azure DNS 中解析的 DNS 查詢。

    ![顯示將 DNS 解析程式設定為轉寄站的螢幕擷取畫面。](./media/contoso-migration-infrastructure/dns-forwarder.png)

    _圖33：設定 Azure DNS 解析程式。_

9. 他們會使用虛擬網路區域的適當網域控制站，更新每個虛擬網路的自訂 DNS 設定。 他們將內部部署 DC 納入清單中。

### <a name="set-up-active-directory"></a>設定 Active Directory

Active Directory 是網路的重要服務，必須正確設定。 Contoso 管理員會為 Contoso 資料中心和和區域建立 Active Directory 網站 `East US 2` `Central US` 。

1. 他們建立兩個新的網站 (`AZURE-EUS2` ，並 `AZURE-CUS`)  () 的資料中心網站 `contoso-datacenter` 。
2. 建立網站之後，他們會在網站中建立子網，以符合虛擬網路和資料中心。

    ![顯示資料中心子網建立的螢幕擷取畫面。](./media/contoso-migration-infrastructure/dc-subnets.png)

    _圖34：資料中心子網。_

3. 他們建立兩個站台連結來連接所有專案。 網域控制站隨後應移至其位置。

    ![顯示資料中心連結建立的螢幕擷取畫面。](./media/contoso-migration-infrastructure/dc-links.png)

    _圖35：資料中心連結。_

4. 他們確認 Active Directory 複寫拓撲已就緒。

    ![顯示資料中心複寫拓撲的螢幕擷取畫面。](./media/contoso-migration-infrastructure/ad-resolution.png)

    _圖36：資料中心複寫。_

一切完成後，就會在內部部署 Active Directory 管理中心中顯示網域控制站和網站的清單。

![顯示 Active Directory 管理中心的螢幕擷取畫面。](./media/contoso-migration-infrastructure/ad-center.png)

_圖37： Active Directory 管理中心。_

## <a name="step-5-plan-for-governance"></a>步驟 5︰為控管做規劃

Azure 提供多種跨服務和 Azure 平台的控管功能。 如需詳細資訊，請參閱 [Azure 治理選項](/azure/security/governance-in-azure)。

Contoso 在設定身分識別和存取控制時，已經開始將治理和安全性的某些層面放在哪裡。 大致上，它需要考慮三個領域：

- **原則：** Azure 原則對您的資源套用並強制執行規則和效果，讓資源符合公司的需求和 Sla。
- **鎖定：** Azure 可讓您鎖定訂用帳戶、資源群組和其他資源，使其只能由具有許可權的人員進行修改。
- **標記：** 您可以使用標記來控制、審核及管理資源。 標記會將中繼資料附加至資源，而提供資源或擁有者的相關資訊。

### <a name="set-up-policies"></a>設定原則

Azure 原則服務會藉由掃描不符合原則定義的資源來評估您的資源。 例如，您的原則可能只允許特定類型的 Vm，或要求資源必須有特定的標記。

原則會指定原則定義，而原則指派則會指定應套用原則的範圍。 從管理群組到資源群組皆可涵蓋於此範圍中。 [了解](/azure/governance/policy/tutorials/create-and-manage)如何建立及管理原則。

Contoso 想要開始兩個原則。 它想要有一個原則，以確保資源只能部署在 `East US 2` 和 `Central US` 區域中。 它也需要將 VM Sku 限制為僅限核准 Sku 的原則。 其目的是要確保不會使用昂貴的 VM SKU。

#### <a name="limit-resources-to-regions"></a>將資源限定於區域

Contoso 使用內建的原則定義 [允許的位置]**** 來限制資源區域。

1. 在 Azure 入口網站中，選取 [所有服務]****，然後搜尋 [原則]****。
2. 選取**Assignments**  >  **指派指派原則**。
3. 在原則清單中，選取 [允許的位置]****。
4. 將 [範圍]**** 設定為 Azure 訂用帳戶的名稱，然後在允許清單中選取兩個區域。

    ![顯示透過原則定義之允許位置的螢幕擷取畫面。](./media/contoso-migration-infrastructure/policy-region.png)

    _圖38：允許透過原則定義的位置。_

5. 根據預設，原則會設定為 [ **拒絕**]。 這項設定表示，如果有人在不屬於或區域的訂用帳戶中啟動部署 `East US 2` `Central US` ，部署將會失敗。 如果 Contoso 訂用帳戶中有人嘗試在中設定部署，就會發生這種情況 `West US` 。

    ![顯示失敗原則錯誤的螢幕擷取畫面。](./media/contoso-migration-infrastructure/policy-failed.png)

    _圖39：失敗的原則。_

#### <a name="allow-specific-vm-skus"></a>允許特定 VM SKU

Contoso 會使用內建的原則定義 `Allow virtual machine SKUs` 來限制可在訂用帳戶中建立的 vm 類型。

![顯示 SKU 選取專案的螢幕擷取畫面。](./media/contoso-migration-infrastructure/policy-sku.png)

_圖40：原則 SKU。_

#### <a name="check-policy-compliance"></a>檢查原則合規性

原則會立即生效，而 Contoso 可以檢查資源的合規性。 在 Azure 入口網站中，選取 [合規性]**** 連結。 合規性儀表板隨即出現。 您可以向下切入以取得詳細資料。

![顯示合規性儀表板的螢幕擷取畫面。](./media/contoso-migration-infrastructure/policy-compliance.png)
  
_圖41：原則合規性。_

### <a name="set-up-locks"></a>設定鎖定

Contoso 一直都使用 ITIL 架構來管理其系統。 變更控制是此架構最重要的環節之一，而 Contoso 想要確定 Azure 部署中已實作變更控制。

Contoso 會 [鎖定資源](/azure/azure-resource-manager/resource-group-lock-resources)。 任何生產或容錯移轉元件都必須在具有唯讀鎖定的資源群組中。 這表示，若要修改或刪除生產專案，授權的使用者必須移除鎖定。 非生產資源群組將會有 `CanNotDelete` 鎖定。 這表示授權的使用者可以讀取或修改資源，但無法將它刪除。

### <a name="set-up-tagging"></a>設定標籤

為了追蹤日漸增加的資源，Contoso 勢必要為資源與適當的部門、客戶和環境建立關聯，因為其重要性也是與日俱增。 除了提供資源和擁有者的相關資訊之外，標記還可讓 Contoso 匯總資源並將其分組，並將該資料用於退款用途。

Contoso 必須以對企業有意義的方式將其 Azure 資產視覺化，例如角色或部門。 請注意，資源不需要位於相同的資源群組即可共用標記。 Contoso 會建立標記分類，讓每個人都使用相同的標記。

| 標籤名稱 | 值 |
| --- | --- |
| `CostCenter` | 12345：它必須是來自 SAP 的有效成本中心。 |
| `BusinessUnit` | 從 SAP)  (的業務單位名稱。 符合 `CostCenter` 。 |
| `ApplicationTeam` | 擁有應用程式支援之小組的電子郵件別名。 |
| `CatalogName` | 應用程式的名稱 `SharedServices` ，或根據資源支援的服務類別目錄。 |
| `ServiceManager` | 資源之 ITIL 服務管理員的電子郵件別名。 |
| `COBPriority` | BCDR 的企業所設定的優先順序。 1-5 的值。 |
| `ENV` | `DEV`、 `STG` 和 `PROD` 是允許的值，代表開發、預備和生產環境。 |

例如：

![顯示 Azure 標記的螢幕擷取畫面。](./media/contoso-migration-infrastructure/azure-tag.png)

_圖42： Azure 標記。_

建立標記之後，Contoso 會返回並建立新的原則定義和指派，以強制在整個組織使用必要的標記。

## <a name="step-6-consider-security"></a>步驟 6：考量安全性

安全性在雲端中至關重要，而 Azure 提供了豐富的安全性工具和功能。 這些可協助您在安全的 Azure 平臺上建立安全的解決方案。 若要深入瞭解 Azure 安全性，請參閱 [信任您的雲端](https://azure.microsoft.com/overview/trusted-cloud) 。

Contoso 有幾個需要考量的層面：

- [Azure 資訊安全中心](/azure/security-center/security-center-intro) 可跨混合式雲端工作負載提供統一的安全性管理和 Azure 進階威脅防護。 您可以使用它來跨工作負載套用安全性原則、限制暴露于威脅的程度，以及偵測和回應攻擊。
- [網路安全性群組 (NSG) ](/azure/virtual-network/security-overview)會根據安全性規則清單來篩選網路流量，以允許或拒絕連線至 Azure 虛擬網路之資源的網路流量。
- [Azure 磁碟加密](/azure/security/fundamentals/encryption-atrest) 是一項功能，可協助您加密 Windows 和 LINUX IaaS VM 磁片。

### <a name="work-with-the-azure-security-center"></a>使用 Azure 資訊安全中心

Contoso 正在尋找其新混合式雲端（尤其是其 Azure 工作負載）安全性狀態的快速觀點。 因此，Contoso 決定從下列功能開始實作 Azure 資訊安全中心：

- 集中式管理原則
- 持續評估
- 可操作的建議

#### <a name="centralize-policy-management"></a>集中管理原則

透過集中式原則管理，Contoso 將可集中管理整個環境的安全性原則，以確實符合安全性需求。 它可以簡單快速地實行套用至其所有 Azure 資源的原則。

![顯示安全性原則選取專案的螢幕擷取畫面。](./media/contoso-migration-infrastructure/security-policy.png)

_圖43：安全性原則。_

#### <a name="assess-security"></a>評估安全性

Contoso 將利用持續的安全性評估，來監視機器、網路、儲存體、資料和應用程式的安全性，以找出潛在的安全性問題。

「安全性中心」會分析 Contoso 計算、基礎結構和資料資源的安全性狀態。 它也會分析 Azure 應用程式和服務的安全性狀態。 持續評估可協助 Contoso 作業小組找出潛在的安全性問題，例如遺漏安全性更新或已公開網路連接埠的系統。

Contoso 想要確保所有的 Vm 都受到保護。 安全性中心可協助您進行這種處理。 它會驗證 VM 的健康情況，並在惡意探索安全性弱點之前，先進行優先的可採取動作建議。

![顯示虛擬機器監視的螢幕擷取畫面。](./media/contoso-migration-infrastructure/monitoring.png)

_圖44：監視。_

### <a name="work-with-nsgs"></a>使用 NSG

Contoso 可以使用網路安全性群組，限制對虛擬網路中資源的網路流量。

網路安全性群組包含一些安全性規則，可根據來源或目的地 IP 位址、連接埠和通訊協定允許或拒絕輸入或輸出網路流量。 套用至子網路時，規則將會套用至子網路中的所有資源。 除了網路介面以外，這包括在子網路中部署的 Azure 服務執行個體。

應用程式安全性群組 (Asg) 可讓您將網路安全性設定為應用程式結構的自然延伸。 然後，您可以將 Vm 分組，並定義以這些群組為基礎的網路安全性原則。

Contoso 可以使用 Asg 來大規模重複使用安全性原則，而不需進行明確 IP 位址的手動維護。 平臺會處理明確 IP 位址和多個規則集的複雜性，讓組織可以專注于商務邏輯。 Contoso 可以將 ASG 指定為安全性規則中的來源和目的地。 定義安全性原則之後，Contoso 可以建立 Vm，並將 VM Nic 指派給群組。

Contoso 會混合實作 NSG 與 ASG。 Contoso 對 NSG 管理有所顧慮。 此外，它也會擔心 Nsg 的過度利用以及增加的作業人員複雜度。 以下是 Contoso 將採取的做法：

- 除了中樞網路中的閘道子網以外，所有連入和傳出所有子網的流量 (北/南部) 都會受到 NSG 規則的規範。
- 任何防火牆或網域控制站都會受到子網 Nsg 和 NIC Nsg 的保護。
- 所有生產應用程式都將套用 ASG。

Contoso 已經建立了此安全性設定將如何尋找其應用程式的模型。

![Contoso 安全性模型的圖表。](./media/contoso-migration-infrastructure/asg.png)

_圖45：安全性模型。_

與 ASG 相關聯的 NSG 將會以最低權限設定，以確保只有允許的封包可從網路的某個部分流向其目的地。

| 動作 | 名稱 | 來源 | 目標 | Port |
| --- | --- | --- | --- | --- |
| `Allow` | `AllowInternetToFE` | `VNET-HUB-EUS1`/`IB-TrustZone` | `APP1-FE` | 80、443 |
| `Allow` | `AllowWebToApp` | `APP1-FE` | `APP1-APP` | 80、443 |
| `Allow` | `AllowAppToDB` | `APP1-APP` | `APP1-DB` | 1433 |
| `Deny`  | `DenyAllInbound` | 任意 | 任意 | 任意 |

### <a name="encrypt-data"></a>加密資料

Azure 磁碟加密與 Azure Key Vault 整合，以協助控制和管理訂用帳戶的磁片加密金鑰和秘密。 它可確保 VM 磁片上的所有資料都會在 Azure 儲存體的靜止時加密。

Contoso 確定特定 VM 需要加密。 Contoso 會將加密套用至具有客戶、機密或個人資料的 Vm。

## <a name="conclusion"></a>結論

在本文中，Contoso 會為 Azure 訂用帳戶、混合式身分識別、損毀修復、網路、治理和安全性設定 Azure 基礎結構和原則。

雲端遷移並不需要在此處採取的每個步驟。 在此案例中，Contoso 規劃了可處理所有類型的遷移，同時又安全、具彈性且可擴充的網路基礎結構。

## <a name="next-steps"></a>後續步驟

設定好其 Azure 基礎結構之後，Contoso 就可以開始將工作負載遷移至雲端。 如需使用此範例基礎結構作為遷移目標的案例，請參閱 [移轉模式和範例總覽](./contoso-migration-overview.md#windows-server-workloads) 。
