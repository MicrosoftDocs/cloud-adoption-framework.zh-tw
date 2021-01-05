---
title: 虛擬資料中心：網路觀點
description: 使用適用于 Azure 的雲端採用架構，瞭解如何使用 Azure 順暢地將基礎結構延伸至雲端，並建立多層式架構。
author: tracsman
manager: rossort
ms.author: brblanch
ms.date: 02/25/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: reference
ms.custom: think-tank, virtual-network
ms.openlocfilehash: efe7a4e957bd2287c4cfe846562788b0c165c3ee
ms.sourcegitcommit: a0ddde4afcc7d8c21559e79d406dc439ee4f38d2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/21/2020
ms.locfileid: "97713076"
---
<!-- docutune:disable TODO -->
<!-- cSpell:ignore iptables DDOS ITSM LLAP anycast vwan -->

# <a name="the-virtual-datacenter-a-network-perspective"></a>虛擬資料中心：網路觀點

從內部部署遷移的應用程式將受益于 Azure 的安全成本效益基礎結構，即使是最基本的應用程式變更也是如此。 即使是如此，企業也應調整其架構以改善靈活性，並利用 Azure 的功能。

Microsoft Azure 利用企業級功能和可靠性來提供超大規模的服務和基礎結構。 這些服務和基礎結構提供許多混合式連線的選擇，因此客戶可以選擇透過網際網路或私人網路連線來存取這些服務和基礎結構。 Microsoft 合作夥伴也可以提供增強的功能，方法是提供最適合在 Azure 中執行的安全性服務和虛擬裝置。

客戶可以使用 Azure 將其基礎結構順暢地延伸至雲端，並建立多層式架構。

## <a name="what-is-a-virtual-datacenter"></a>什麼是虛擬資料中心？

雲端以平臺的形式開始裝載公開的應用程式。 企業已辨識出雲端的價值，並開始遷移內部企業營運應用程式。 這些應用程式帶來額外的安全性、可靠性、效能和成本考慮，在提供雲端服務時需要額外的彈性。 新的基礎結構和網路服務的設計目的是要提供這種彈性，以及針對彈性規模、嚴重損壞修復和其他考慮提供的新功能。

雲端解決方案一開始是設計來裝載公用頻譜中單一、相對隔離的應用程式。 這種方法已良好運作多年。 由於雲端解決方案的優點變得更清楚，因此雲端上裝載了多個大規模的工作負載。 在雲端服務的整個生命週期中，處理一或多個區域中部署的安全性、可靠性、效能和成本考慮，變得很重要。

在下方的範例雲端部署圖中，紅色方塊反白顯示安全性缺口。 黃色方塊顯示跨工作負載優化網路虛擬裝置的機會。

![0][0]

虛擬資料中心有助於達成企業工作負載所需的規模。 此規模必須處理在公用雲端中執行大規模應用程式時所遇到的挑戰。

虛擬資料中心 (VDC) 執行包含的應用程式工作負載超過雲端。 它也提供網路、安全性、管理，以及其他基礎結構，例如 DNS 和 Active Directory 服務。 當企業將額外的工作負載遷移至 Azure 時，請考慮支援這些工作負載的基礎結構和物件。 仔細地結構化您的資源有助於避免數百個個別管理的「工作負載孤島」有獨立的資料流程、安全性模型和合規性挑戰。

虛擬資料中心概念提供建議和高層級的設計，以執行個別但相關實體的集合。 這些實體通常具有一般支援功能、功能和基礎結構。 將您的工作負載視為虛擬資料中心，有助於從規模經濟降低成本、透過元件和資料流程集中化的最佳安全性，以及更容易操作、管理和合規性審核。

> [!NOTE]
> 虛擬資料中心 **不** 是特定的 Azure 服務。 相反地，會結合各種 Azure 特性和功能來滿足您的需求。 虛擬資料中心是一種考慮工作負載和 Azure 使用量的方式，可將雲端中的資源和功能優化。 它提供模組化的方法，在 Azure 中提供 IT 服務，同時尊重企業的組織角色和責任。

虛擬資料中心可協助企業在下列情況下，在 Azure 中部署工作負載和應用程式：

- 裝載多個相關工作負載。
- 將工作負載從內部部署環境移轉至 Azure。
- 實作工作負載的共用或集中式安全性和存取需求。
- 針對大型企業適當地混合 DevOps 和集中式 IT。

## <a name="who-should-implement-a-virtual-datacenter"></a>哪些人應實作虛擬資料中心？

任何已決定採用 Azure 的客戶都可以受益于設定一組資源以供所有應用程式共同使用的效率。 視大小而定，即使是單一應用程式，也可以利用用來建立 VDC 執行的模式和元件來獲益。

某些組織具有 IT、網路、安全性或合規性的集中式小組或部門。 執行 VDC 有助於強制執行原則點、區分職責，並確保基礎一般元件的一致性。 應用程式小組可以保留適合其需求的自由和控制。

具有 DevOps 方法的組織也可以使用 VDC 概念來提供 Azure 資源的授權隨身。 此方法可確保 DevOps 群組具有該群組內的整體控制（位於訂用帳戶層級或一般訂用帳戶的資源群組內）。 同時，網路和安全性界限會保持相容，如中樞網路和集中管理的資源群組中的集中式原則所定義。

## <a name="considerations-for-implementing-a-virtual-datacenter"></a>執行虛擬資料中心的考慮

設計虛擬資料中心時，請考慮下列 pivotal 問題：

### <a name="identity-and-directory-service"></a>身分識別和目錄服務

身分識別和目錄服務是內部部署和雲端資料中心的主要功能。 身分識別涵蓋 VDC 實行內服務存取和授權的所有層面。 為了確保只有經過授權的使用者和程式可以存取您的 Azure 資源，Azure 會使用數種類型的認證進行驗證，包括帳戶密碼、密碼編譯金鑰、數位簽章和憑證。 [Azure Multi-Factor Authentication][multi-factor-authentication] 為使用強式驗證來存取 Azure 服務，提供一層額外的安全性， (電話、文字訊息或行動裝置代理程式更新) ，讓客戶選擇他們偏好的方法。

任何大型企業都需要定義身分識別管理程式，以描述如何管理其 VDC 內或之間的個別身分識別、其驗證、授權、角色和許可權。 此程式的目標應該是提高安全性與生產力，同時降低成本、停機時間和重複性的手動工作。

企業組織可能需要針對不同的業務需求混合使用服務，而員工在與不同的專案相關時，通常會有不同的角色。 使用 VDC 時，各具有特定角色定義的不同小組之間必須密切合作，使系統在妥善的控管下執行。 責任、存取權和權限的矩陣可能會很複雜。 VDC 中的身分識別管理是透過 [Azure Active Directory (Azure AD) ][azure-ad] 和 azure 角色型存取控制 (azure RBAC) 來實行。

目錄服務是一種共用資訊基礎結構，用以尋找、管理和組織日常項目和網路資源。 這些資源可能包含磁碟區、資料夾、檔案、印表機、使用者、群組、裝置和其他物件。 目錄伺服器會將網路上的每個資源都視為物件。 資源的相關資訊會儲存為與該資源或物件建立關聯的屬性集合。

所有 Microsoft online 商務服務都依賴 Azure Active Directory (Azure AD) 進行登入和其他身分識別需求。 Azure Active Directory 是全方位、高可用性的身分識別和存取管理的雲端解決方案，它結合了核心目錄服務、進階身分識別管制及應用程式存取管理。 Azure AD 可以與內部部署 Active Directory 整合，以啟用所有雲端式和本機裝載 (內部部署) 應用程式的單一登入。 內部部署 Active Directory 的使用者屬性可以自動同步至 Azure AD。

VDC 實作中的所有權限不一定要由單一全域管理員指派。 相反地，每個特定部門、使用者群組或目錄服務中的服務都可以有管理其在 VDC 實作內專屬資源所需的權限。 建構權限需要平衡。 權限太多可能會阻礙效能效率，而權限太少或鬆散可能會增加安全性風險。 Azure 角色型存取控制 (Azure RBAC) 協助您解決此問題，方法是在 VDC 執行的資源提供更細緻的存取管理。

### <a name="security-infrastructure"></a>安全性基礎結構

安全性基礎結構是指在 VDC 實作的特定虛擬網路區段中區隔流量。 此基礎結構會指定如何在 VDC 的執行中控制輸入和輸出。 Azure 是以多租使用者架構為基礎，可使用虛擬網路隔離、存取控制清單、負載平衡器、IP 篩選器和流量原則，防止部署之間的未經授權和意外流量。 網路位址轉譯 (NAT) 會區隔內部網路流量與外部流量。

Azure 網狀架構會將基礎結構資源分配給租用戶工作負載，並管理與虛擬機器 (VM) 之間的通訊。 Azure Hypervisor 會強制執行 VM 之間的記憶體和處理序區隔，並安全地將網路流量路由傳送至客體 OS 租用戶。

### <a name="connectivity-to-the-cloud"></a>雲端的連線

虛擬資料中心需要連接到外部網路，以提供服務給客戶、合作夥伴或內部使用者。 這不僅需要連線至網際網路，也需連線至內部部署網路和資料中心。

客戶可控制哪些服務可以存取並從公用網際網路存取。 這項存取是使用 [Azure 防火牆][AzFW] 或其他類型的虛擬網路設備來控制 (nva) 、使用 [使用者定義的路由][UDR]的自訂路由原則，以及使用 [網路安全性群組][NSG]進行網路篩選。 我們建議，所有面向網際網路的資源也都應受到 [Azure DDoS 保護標準][DDoS]的保護。

企業可能需要將其虛擬資料中心連線至內部部署資料中心或其他資源。 設計有效架構時，Azure 與內部部署網路之間的此連線是重要層面。 企業有兩種不同的方式可建立這種互連：透過網際網路或透過私人直接連線的傳輸。

[Azure 站對站 VPN][VPN]會將內部部署網路連接到 azure 中的虛擬資料中心。 此連結是透過 (IPsec 通道) 的安全加密連線來建立。 Azure 站對站 VPN 連線具有彈性、可快速建立，且通常不需要任何額外的硬體採購。 根據業界標準的通訊協定，最新的網路裝置可透過網際網路或現有的連線路徑來建立與 Azure 的 VPN 連線。

[ExpressRoute][ExR] 可讓您的虛擬資料中心與任何內部部署網路之間進行私人連線。 ExpressRoute 連線不會經過公用網際網路，並提供更高的安全性、可靠性和更高的速度 (高達 100 Gbps) ，以及一致的延遲。 ExpressRoute 提供與私人連線相關聯之相容性規則的優點。 有了 [ExpressRoute Direct][ExRD]，您就可以直接連接至 Microsoft 路由器 10 gbps 或 100 Gbps。

部署 ExpressRoute 連線通常牽涉到與 ExpressRoute 服務提供者互動 (ExpressRoute Direct 是) 例外狀況。 針對需要快速啟動的客戶，一開始通常會使用站對站 VPN 來建立虛擬資料中心與內部部署資源之間的連線能力。 一旦與服務提供者的實體互相連線完成後，便可透過 ExpressRoute 連線來遷移連線能力。

針對大量的 VPN 或 ExpressRoute 連線， [Azure 虛擬 WAN][virtual-wan] 是一種網路服務，透過 Azure 提供最佳且自動化的分支對分支連線能力。 虛擬 WAN 可讓您連線及設定分支裝置，以與 Azure 進行通訊。 您可以手動方式或透過虛擬 WAN 合作夥伴使用慣用的提供者裝置來進行連線和設定。 使用慣用的提供者裝置不僅方便使用，還可簡化連線和管理組態。 Azure WAN 內建儀表板提供即時的疑難排解深入解析，可協助您節省時間，並可讓您輕鬆地查看大規模的站對站連線能力。 虛擬 WAN 也會在您的虛擬 WAN 中樞中，使用選擇性的 Azure 防火牆和防火牆管理員來提供安全性服務。

### <a name="connectivity-within-the-cloud"></a>雲端內的連線

[Azure 虛擬網路][virtual-network] 和 [虛擬網路對等互連][virtual-network-peering] 是虛擬資料中心內的基本網路元件。 虛擬網路可保證虛擬資料中心資源的隔離界限。 對等互連可讓相同 Azure 區域內的不同虛擬網路、跨區域，甚至是不同訂用帳戶中的網路之間的 vnet 互相通訊。 在虛擬網路內部和兩者之間，流量流程可以由針對 [網路安全性群組][NSG]、防火牆原則 ([Azure 防火牆][AzFW] 或 [網路虛擬裝置][NVA]) 和自訂 [使用者定義路由][UDR]所指定的一組安全性規則來控制。

虛擬網路也是將平臺即服務整合 (PaaS) Azure 產品（例如 [Azure 儲存體][Storage]、 [azure SQL][SQL]和其他具有公用端點的整合式公用服務）的錨點。 您可以使用 [服務端點][ServiceEndpoints] 和 [Azure Private Link][PrivateLink]，將公用服務與私人網路整合。 您甚至可以將公用服務私用，但仍可享受 Azure 管理的 PaaS 服務的優點。

## <a name="virtual-datacenter-overview"></a>虛擬資料中心概觀

### <a name="topologies"></a>拓撲

您可以根據需求和規模需求，使用下列其中一種高階拓撲來建立虛擬資料中心：

在 _平面拓撲_ 中，所有資源都會部署在單一虛擬網路中。 子網允許流量控制和隔離。

![11][11]

在 _網狀拓撲_ 中，虛擬網路對等互連會將所有虛擬網路直接連線到彼此。

![12][12]

對 _等互連中樞和輪輻拓撲_ 非常適合具有委派責任的分散式應用程式和小組。

![13][13]

_Azure 虛擬 WAN 拓撲_ 可支援大規模的分公司案例和全球 WAN 服務。

![14][14]

對等互連中樞與輪輻拓撲，以及 Azure 虛擬 WAN 拓撲都使用中樞和輪輻設計，這對通訊、共用資源和集中式安全性原則是最佳的。 中樞的建立方式是使用虛擬網路對等互連中樞 (標示為 `Hub Virtual Network` 圖表) 或虛擬 WAN 中樞 (標示為 `Azure Virtual WAN` 圖表) 中的。 Azure 虛擬 WAN 是針對大規模分支對分支和分支對 Azure 通訊所設計，或為了避免在虛擬網路對等互連中樞中個別建立所有元件的複雜性。 在某些情況下，您的需求可能會強制使用虛擬網路對等互連中樞設計，例如中樞內的網路虛擬裝置需求。

在這兩個中樞和輪輻拓撲中，中樞都是中央網路區域，可控制和檢查不同區域之間的所有流量：網際網路、內部部署和輪輻。 中樞和輪輻拓撲可協助 IT 部門集中強制執行安全性原則。 此外也可降低設定錯誤和暴露的可能性。

中樞通常包含輪輻所耗用的一般服務元件。 以下是常用中央服務的範例：

- 從不受信任的網路存取的第三方在能夠存取輪輻中的工作負載之前進行使用者驗證所需的 Windows Active Directory 基礎結構。 其中包括相關的 Active Directory 同盟服務 (AD FS)。
- 分散式名稱系統 (DNS) 服務來解析輪輻中工作負載的命名，以存取內部部署和網際網路上的資源（如果未使用 [Azure DNS][DNS] ）。
- 公開金鑰基礎結構 (PKI)，用以實作工作負載的單一登入。
- 輪輻網路區域與網際網路之間的 TCP 和 UDP 流量的流量控制。
- 輪輻與內部部署之間的流量控制。
- 兩個輪輻之間的流量控制 (如有需要)。

虛擬資料中心會使用多個輪輻之間的共用中樞基礎結構，以降低整體成本。

每個支點的角色都可以裝載不同類型的工作負載。 輪輻也提供適用的模組化方法，供相同的工作負載進行可重複的部署。 範例包括開發/測試、使用者接受度測試、生產前和生產環境。 輪輻也可區隔和啟用組織內的不同群組。 例如，DevOps 群組。 在輪輻內部，可以使用各層之間的流量控制來部署基本工作負載或複雜的多層式工作負載。

### <a name="subscription-limits-and-multiple-hubs"></a>訂用帳戶限制和多個中樞

> [!IMPORTANT]
> 根據您的 Azure 部署大小，可能需要多個中樞策略。 當設計您的中樞和輪輻策略時，請詢問「此設計規模是否可在此區域中使用另一個中樞虛擬網路」，也就是「此設計規模是否可容納多個區域？」 最好是規劃能調整規模而不需要的設計，而不是無法規劃和需要。
>
> 當您調整至次要 (或更多) 中樞將取決於各種因素，通常是根據規模的固有限制而定。 針對調整進行設計時，請務必檢查訂用帳戶、虛擬網路和虛擬機器的 [限制][limits] 。

在 Azure 中，每個任何類型的元件都會部署在 Azure 訂用帳戶中。 不同 Azure 訂用帳戶中的 Azure 元件隔離可以滿足不同企業營運的需求，例如設定不同層級的存取和授權。

單一 VDC 的執行可以擴大為大量輪輻，雖然每個 IT 系統都有平臺限制。 中樞部署會系結至特定的 Azure 訂用帳戶，此訂用帳戶的限制和限制 (例如，虛擬網路對等互連的最大數目。 如需詳細資訊，請參閱 [Azure 訂用帳戶和服務限制、配額和條件約束][limits]) 。 如果限制可能會產生問題，則將模型從單一中樞支點延伸到中樞和支點叢集，即可進一步向上延展架構。 一或多個 Azure 區域中的多個中樞可以使用虛擬網路對等互連、ExpressRoute、虛擬 WAN 或站對站 VPN 進行連線。

![2][2]

導入多個中樞會增加系統的成本和管理工作。 這只是因為擴充性、系統限制、冗余、區域複寫，以提供使用者效能或嚴重損壞修復的理由。 在需要多個中樞的案例中，所有中樞都應該致力於提供一組相同的易操作服務。

### <a name="interconnection-between-spokes"></a>支點之間的互相連線

在單一輪輻或一般網路設計中，可以執行複雜的多層式工作負載。 多層式設定可以使用子網來執行，也就是在相同虛擬網路中的每個層或應用程式各一。 使用網路安全性群組和使用者定義的路由來完成流量控制和篩選。

架構設計人員可能會想要跨多個虛擬網路部署一個多層式工作負載。 透過虛擬網路對等互連，輪輻可以連線至相同中樞或不同中樞內的其他輪輻。 此案例的常見範例是，應用程式處理伺服器位於一個輪輻 (或虛擬網路) 中。 資料庫則部署在多個輪輻 (或虛擬網路) 中。 在此情況下，您可以輕鬆地使用虛擬網路對等互連將輪輻互連，並藉由這樣做，避免透過中樞傳輸。 您應進行仔細的架構和安全性檢查，以確保略過中樞並不會略過可能僅存在於中樞的重要安全性或審核點。

![3][3]

支點也可以與作為中樞的支點互相連接。 這種方法會建立雙層階層：較高層級的輪輻 (層級 0) 會變成階層中較低輪輻 (層級 1) 的中樞。 需要 VDC 的輪輻才能將流量轉送到中央中樞，讓流量可以傳輸至內部部署網路或公用網際網路的目的地。 具有兩層中樞的架構引進複雜路由，以移除簡單中樞輪輻關聯性的優點。

雖然 Azure 允許複雜拓撲，但是 VDC 概念的其中一個核心準則是重複性和簡單性。 若要將管理投入時間降到最低，我們建議採用 VDC 參考架構這種簡單的中樞輪輻設計。

### <a name="components"></a>單元

虛擬資料中心是由四種基本元件類型所組成： **基礎結構**、 **周邊網路**、 **工作負載** 和 **監視**。

每種元件類型都是由各種 Azure 功能和資源所組成。 VDC 實作是由多種元件類型以及相同元件類型之多個變化的執行個體所構成。 例如，您可能會有許多不同的邏輯分隔工作負載實例，代表不同的應用程式。 您可以使用這些不同的元件類型和執行個體來最後建置 VDC。

![4][4]

VDC 的上述高階概念架構顯示不同中樞輪輻拓撲區域中所使用的不同元件類型。 此圖顯示架構各種組件中的基礎結構元件。

一般較好的做法是，存取權利和權限應該以群組為基礎。 處理群組而不是個別使用者，藉由提供一致的方式來管理跨小組的存取原則，並協助將設定錯誤降至最低，以簡化存取原則的維護。 在適當群組中指派和移除使用者，有助於將特定使用者的許可權保持在最新狀態。

每個角色群組的名稱均應有唯一的前置詞。 此前置詞有助於輕易識別哪個群組與哪個工作負載相關聯。 例如，裝載驗證服務的工作負載可能會名為 **AuthServiceNetOps**、**AuthServiceSecOps**、**AuthServiceDevOps** 和 **AuthServiceInfraOps**。 集中式角色或與特定服務無關的角色，可能會以 **Corp** 開頭。例如， **>corpnetops**。

許多組織都會使用下列群組的一種變化，以提供角色的主要分析：

- 名為 **Corp** 的中央 IT 團隊具有控制基礎結構元件的擁有權許可權。 例如網路和安全性。 群組必須具有訂用帳戶的參與者角色、中樞的控制權，以及輪輻中的網路參與者權限。 大型組織常會將這些管理職責劃分到多個小組間。 例如，網路作業 **CorpNetOps** 群組 (具有網路的獨佔焦點)，和安全性作業 **CorpSecOps** 群組 (負責防火牆和安全性原則)。 在這種特定情況下，需要建立兩個不同的群組，才能指派這些自訂角色。
- 名為 **AppDevOps** 的開發/測試群組必須負責部署應用程式或服務工作負載。 此群組會扮演虛擬機器參與者的角色，以進行 IaaS 部署或一或多個 PaaS 參與者的角色。 如需詳細資訊，請參閱 [Azure 內建角色][Roles]。 （選擇性）開發/測試小組可能需要 (網路安全性群組) 和路由原則的資訊安全性原則， (中樞內) 的使用者定義路由或特定輪輻。 除了工作負載參與者角色之外，此群組也需要網路讀取者角色。
- 稱為 **CorpInfraOps** 或 **AppInfraOps** 的作業和維護群組，負責管理生產環境中的工作負載。 此群組必須是任何生產訂用帳戶中工作負載的訂用帳戶參與者。 某些組織可能也會評估他們是否需要在生產環境和中央中樞訂用帳戶中具有訂用帳戶參與者角色的額外擴大支援小組群組。 額外的群組可修正生產環境中潛在的設定問題。

VDC 的設計目的是為了讓中央 IT 小組所建立的群組，在工作負載層級擁有對應的群組。 除了管理中樞資源之外，中央 IT 團隊還可以控制訂用帳戶的外部存取和最上層許可權。 工作負載群組也可以從中央 IT 小組獨立控制其虛擬網路的資源和許可權。

虛擬資料中心會進行資料分割，以安全地跨不同的企業營運裝載多個專案。 所有專案都需要不同的隔離環境 (開發、UAT 和生產) 。 這些環境的個別 Azure 訂用帳戶可提供自然隔離。

![5][5]

上圖顯示組織的專案、使用者、群組與 Azure 元件部署所在環境之間的關聯性。

通常，在 IT 中，環境 (或層) 是部署和執行多個應用程式的系統。 大型企業使用的是開發環境 (在其中進行變更並測試) 和生產環境 (使用者) 使用的環境。 這些環境之間通常都會分隔成數個預備環境，以允許分階段部署 (推出)、測試以及在發生問題時復原。 部署架構極大，但通常仍會遵循開始開發環境 (DEV) 和結束生產環境 (PROD) 的基本程序。

這類多層式環境的常見架構包含用於開發和測試的 DevOps、適用于預備環境的 UAT，以及生產環境。 組織可以使用單一或多個 Azure AD 租使用者，來定義這些環境的存取權和許可權。 上圖顯示使用兩個不同 Azure AD 租用戶的情況：一個適用於 DevOps 和 UAT，另一個則專用於生產環境。

具有不同的 Azure AD 租用戶會強制執行環境之間的區隔。 相同的使用者群組（例如中央 IT 小組）必須使用不同的 URI 來進行驗證，以存取不同的 Azure AD 租使用者，以修改專案的 DevOps 或生產環境的角色或許可權。 具有存取不同環境的不同使用者驗證可降低可能的中斷以及人為錯誤所導致的其他問題。

#### <a name="component-type-infrastructure"></a>元件類型：基礎結構

此元件類型是大部分支援基礎結構所在的位置。 它也是您的集中式 IT、安全性和合規小組花費最多時間的位置。

![6][6]

基礎結構元件提供不同 VDC 實作元件的互相連線，並且存在於中樞和輪輻中。 管理和維護基礎結構元件的責任通常會指派給中央 IT 小組或安全性小組。

IT 基礎結構小組的其中一個主要工作是確保整個企業的 IP 位址結構描述一致性。 指派給 VDC 實作的私人 IP 位址空間需要一致，而且不會與內部部署網路上指派的私人 IP 位址重疊。

雖然內部部署邊際路由器或 Azure 環境中的 NAT 可以避免 IP 位址衝突，但是會增加基礎結構元件的複雜性。 管理簡化是 VDC 的其中一個關鍵目標，因此使用 NAT 處理 IP 問題，而有效的解決方案則不是建議的解決方案。

基礎結構元件具有下列功能：

- 身分[識別和目錄服務][azure-ad]。 Azure 中每種資源類型的存取權都是受控於目錄服務中所儲存的身分識別。 目錄服務不只會儲存使用者清單，也會儲存對特定 Azure 訂用帳戶中資源的存取權。 這些服務只能存在於雲端，或與 Active Directory 中所儲存的內部部署身分識別同步。
- [虛擬網路][virtual-network]。 虛擬網路是 VDC 的其中一個主要元件，可讓您在 Azure 平臺上建立流量隔離界限。 虛擬網路是由單一或多個虛擬網路區段所組成，每個區段都有特定的 IP 網路首碼 (子網，也就是 IPv4 或雙重堆疊 IPv4/IPv6) 。 虛擬網路定義 IaaS 虛擬機器和 PaaS 服務可建立私人通訊的內部周邊區域。 在一個虛擬網路中) 的 Vm (和 PaaS 服務，無法直接與不同虛擬網路中) 的 (Vm 通訊，即使兩個虛擬網路都是由相同的客戶建立，也可以在相同的訂用帳戶下進行。 隔離是很重要的屬性，可確保客戶 VM 和通訊仍然隱蔽於虛擬網路內。 在需要跨網路連線的情況下，下列功能會說明如何完成這項作業。
- [虛擬網路對等互連][virtual-network-peering]。 用來建立 VDC 基礎結構的基本功能為虛擬網路對等互連，它會透過 Azure 資料中心網路或使用跨區域的 Azure 全球骨幹，連接相同區域中的兩個虛擬網路。
- [虛擬網路服務端點][ServiceEndpoints]。 服務端點會擴充您的虛擬網路私人位址空間，以包含您的 PaaS 空間。 端點也會透過直接連線，將您虛擬網路的身分識別延伸至 Azure 服務。 端點可讓您將重要的 Azure 服務資源只放到您的虛擬網路保護。
- [Private Link][PrivateLink]。 Azure Private Link 可讓您透過虛擬網路中的私人端點，存取 Azure PaaS 服務 (例如 [Azure 儲存體][Storage]、 [Azure Cosmos DB][cosmos-db]和 [Azure SQL Database][SQL]) ，以及 azure 裝載的客戶/合作夥伴服務。 虛擬網路和服務間的流量會在通過 Microsoft 骨幹網路時隨之減少，降低資料在網際網路中公開的風險。 您也可以在虛擬網路中建立自己的 [Private Link 服務][PrivateLinkSvc] ，並將其私下提供給您的客戶。 使用 Azure Private Link 的設定和取用體驗在 Azure PaaS、客戶自有服務和共用合作夥伴服務之間是一致的。
- [使用者定義的路由][UDR]。 依預設會根據系統路由表來路由虛擬網路中的流量。 使用者定義路由是網路系統管理員可與一或多個子網產生關聯的自訂路由表，可覆寫系統路由表的行為，以及定義虛擬網路內的通訊路徑。 如果有使用者定義的路由，則會確保來自輪輻傳輸的輸出流量會透過特定自訂 Vm 或網路虛擬裝置和負載平衡器存在於中樞和輪輻中。
- [網路安全性群組][NSG]。 網路安全性群組是安全性規則清單，可作為 IP 來源、IP 目的地、通訊協定、IP 來源埠和 IP 目的地埠的流量篩選， (也稱為「層級 4 5-元組) 。 網路安全性群組可以套用至子網、與 Azure VM 相關聯的虛擬 NIC，或兩者皆適用。 網路安全性群組在中樞和輪輻中執行正確的流程式控制制時，是不可或缺的。 網路安全性群組所提供的安全性層級是您開啟的埠，以及用途的功能。 客戶應該使用主機型防火牆（例如 iptables 或 Windows 防火牆）來套用額外的每個 VM 篩選。
- [DNS][DNS]。 DNS 可為虛擬資料中心內的資源提供名稱解析。 Azure 可提供 DNS 服務，以進行[公用][DNS]和[私人][PrivateDNS]名稱解析。 私人區域可在虛擬網路內及虛擬網路之間提供名稱解析。 私人區域不僅可以橫跨相同區域內的虛擬網路，也可以橫跨區域和訂用帳戶。 至於公用解析，Azure DNS 可提供 DNS 網域的主機服務，使用 Microsoft Azure 基礎結構提供名稱解析。 只要將您的網域裝載於 Azure，就可以像管理其他 Azure 服務一樣，使用相同的認證、API、工具和計費方式來管理 DNS 記錄。
- [管理群組][MgmtGrp]、 [訂](../ready/azure-best-practices/scale-subscriptions.md)用帳戶和 [資源群組][RGMgmt] 管理。 訂用帳戶定義自然界限，以在 Azure 中建立多個資源群組。 這種分隔可用於函式、角色隔離或計費。 訂用帳戶中的資源會組合在稱為資源群組的邏輯容器中。 資源群組代表邏輯群組，用來組織虛擬資料中心內的資源。 如果貴組織有多個訂用帳戶，您可能需要一個方法來有效率地管理這些訂用帳戶的存取、原則和相容性。 Azure 管理群組可以在訂用帳戶之上提供範圍層級。 您將訂用帳戶整理到稱為管理群組的容器，並將治理條件套用至管理群組。 管理群組內的所有訂用帳戶都會自動繼承套用到管理群組的條件。 若要查看階層視圖中的這三個功能，請參閱在雲端採用架構中 [組織您的資源](../ready/azure-setup-guide/organize-resources.md) 。
- [Azure 角色型存取控制 (AZURE RBAC) ][RBAC]。 Azure RBAC 可以對應組織角色和許可權，以存取特定 Azure 資源，讓您限制使用者只能使用特定的動作子集。 如果您要同步處理 Azure Active Directory 與內部部署 Active Directory，您可以在 Azure 中使用您在內部部署使用的相同 Active Directory 群組。 使用 Azure RBAC，您可以將適當的角色指派給相關範圍內的使用者、群組和應用程式，以授與存取權。 角色指派的範圍可以是 Azure 訂用帳戶、資源群組或單一資源。 Azure RBAC 允許繼承許可權。 在父範圍指派的角色也會授與其內含子系的存取權。 使用 Azure RBAC，您可以將職責區隔，並只授與使用者執行其作業所需的存取權數量。 例如，一個員工可以管理訂用帳戶中的虛擬機器，另一個則可以管理相同訂用帳戶中的 SQL Server 資料庫。

#### <a name="component-type-perimeter-networks"></a>元件類型：周邊網路

周邊網路的元件 (有時稱為 DMZ 網路) 連接您的內部部署或實體資料中心網路，以及任何網際網路連線能力。 周邊通常需要從您的網路和安全性小組進行大量的時間投資。

傳入的封包應該會流經中樞內的安全性設備，然後才能到達輪輻中的後端伺服器和服務。 範例包括防火牆、識別碼和 IP。 來自工作負載的網際網路繫結封包應該也會先流經周邊網路中的安全性設備，才會離開網路。 此流程可進行原則強制執行、檢查和審核。

周邊網路元件包括：

- [虛擬網路][virtual-network]、[使用者定義的路由][UDR]和[網路安全性群組][NSG]
- [網路虛擬設備][NVA]
- [Azure Load Balancer][ALB]
- [Azure 應用程式閘道][AppGW] 搭配 [web 應用程式防火牆 (WAF) ][AppGWWAF]
- [公用 IP][PIP]
- [Azure Front Door][azure-front-door] 與 [web 應用程式防火牆 (WAF) ][AFDWAF]
- [Azure 防火牆][AzFW] 和 [Azure 防火牆管理員][AzFWMgr]
- [標準 DDoS 保護][DDoS]

通常，中央 IT 小組和安全性小組必須負責周邊網路的需求定義和操作。

![7][7]

上圖顯示兩個周邊的強制存取網際網路和內部部署網路（兩者都位於 DMZ 中樞）。 在 DMZ 中樞內，網際網路的周邊網路可以使用多個 Web 應用程式防火牆伺服器陣列 (Waf) 或 Azure 防火牆來擴大，以支援許多企業營運。 中樞也可讓您視需要透過 VPN 或 ExpressRoute 進行內部部署連線。

> [!NOTE]
> 在上圖中，在「DMZ 中樞」中，可以將下列許多功能組合在 Azure 虛擬 WAN 中樞 (例如虛擬網路、使用者定義的路由、網路安全性群組、VPN 閘道、ExpressRoute 閘道、Azure 負載平衡器、Azure 防火牆、防火牆管理員和 DDOS) 。 使用 Azure 虛擬 WAN 中樞可以建立中樞虛擬網路，因此 VDC 更簡單，因為當您部署 Azure 虛擬 WAN 中樞時，Azure 會為您處理大部分的工程複雜度。

[虛擬網路][virtual-network]。 中樞通常建置於具有多個子網的虛擬網路上，以裝載不同類型的服務，以透過 Azure 防火牆、Nva、WAF 和 Azure 應用程式閘道實例來篩選和檢查進出網際網路的流量。

[使用者定義的路由][UDR]。 使用使用者定義的路由，客戶可以部署防火牆、IDS/IPS 和其他虛擬裝置，並透過這些安全性設備來路由傳送網路流量，以進行安全性界限原則強制執行、審核和檢查。 您可以在中樞和輪輻中建立使用者定義的路由，以確保流量會傳輸到 VDC 執行所使用的特定自訂 Vm、網路虛擬裝置和負載平衡器。 為了保證從輪輻傳輸中的虛擬機器產生的流量，到正確的虛擬裝置，您必須將內部負載平衡器的前端 IP 位址設定為下一個躍點，以在輪輻的子網中設定使用者定義的路由。 內部負載平衡器會將內部流量分散到虛擬設備 (負載平衡器後端集區)。

[Azure 防火牆][AzFW] 是一種受管理的網路安全性服務，可保護您的 Azure 虛擬網路資源。 它是具狀態的受控防火牆，具有高可用性和雲端擴充性。 您可以橫跨訂用帳戶和虛擬網路集中建立、強制執行以及記錄應用程式和網路連線原則。 Azure 防火牆會為您的虛擬網路資源提供靜態公用 IP 位址。 這可讓外部防火牆識別來自您虛擬網路的流量。 此服務與 Azure 監視器會完全整合，以進行記錄和分析。

如果您使用 Azure 虛擬 WAN 拓撲， [Azure 防火牆管理員][AzFWMgr] 是一種安全性管理服務，可為雲端式安全性周邊提供集中的安全性原則及路由管理。 它適用于 Azure 虛擬 WAN 中樞，這是 Microsoft 管理的資源，可讓您輕鬆地建立中樞和輪輻架構。 當安全性和路由原則與這類中樞相關聯時，即稱為安全的虛擬中樞。

[網路虛擬裝置][NVA]。 在中樞內，具有網際網路存取的周邊網路通常會透過 Azure 防火牆執行個體或是防火牆伺服器陣列或 Web 應用程式防火牆 (WAF) 的伺服器陣列進行管理。

不同的企業行通常會使用許多 web 應用程式，這些應用程式通常會受到各種弱點和潛在攻擊的影響。 Web 應用程式防火牆是一種特殊類型的產品，用來偵測對 Web 應用程式的攻擊 (HTTP/HTTPS)，而其深入程度高於一般防火牆。 相較於傳統防火牆技術，WAF 有一組特定功能可保護內部網頁伺服器不受威脅。

Azure 防火牆或 NVA 防火牆都會使用共同管理平面，其中有一組安全性規則可保護輪輻中所裝載的工作負載，以及控制對內部部署網路的存取。 Azure 防火牆具有內建延展性，然而 NVA 防火牆可以在負載平衡器後方手動擴充。 一般而言，防火牆伺服器陣列具有比 WAF 還不特殊的軟體，但具有廣泛應用程式範圍可篩選和檢查任何類型的輸出和輸入流量。 如果使用 NVA 方法，您可以從 Azure Marketplace 找到並部署這些方法。

建議您針對來自網際網路的流量使用一組 Azure 防火牆執行個體。 對於來自內部部署的流量則使用另一組執行個體。 對兩者僅使用一組防火牆會造成安全性風險，因為這並未提供兩組網路流量之間的安全性周邊。 使用個別的防火牆層級可降低檢查安全性規則的複雜度，並明確指出哪些規則對應到哪個傳入網路要求。

[Azure Load Balancer][ALB] 提供高可用性層級 4 (TCP/UDP) 服務，可將連入流量分散到負載平衡組中所定義的服務實例之間。 從前端端點傳送至負載平衡器的流量 (公用 IP 端點或私人 IP 端點) 可以在一組後端 IP 位址池 (（例如網路虛擬裝置或虛擬機器) ）中，轉散發或不使用位址轉譯。

Azure Load Balancer 也可以探查各種伺服器實例的健全狀況，而且當實例無法回應探查時，負載平衡器會停止將流量傳送至狀況不良的實例。 在虛擬資料中心內，會將外部負載平衡器部署到中樞和輪輻。 在中樞中，負載平衡器可用來有效率地將流量路由傳送到防火牆實例，而在輪輻中，負載平衡器可用來管理應用程式流量。

[Azure Front Door][azure-front-door] (AFD) 是 Microsoft 兼具高可用性和高擴充性的 Web 應用程式加速平台、全域 HTTP 負載平衡器、應用程式保護和內容傳遞網路。 AFD 在 Microsoft 全球網路邊緣的100個以上位置執行，可讓您建立、操作及擴充您的動態 web 應用程式和靜態內容。 AFD 為您的應用程式提供世界級使用者效能、統一地區/戳記維護自動化、BCDR 自動化、統一用戶端/使用者資訊、快取和服務見解。 平臺提供：

- 效能、可靠性和支援服務等級協定 (Sla) 。
- 合規性認證。
- Azure 開發、操作和原生支援的可審核安全性作法。

Azure Front Door 也提供 web 應用程式防火牆 (WAF) ，以保護 web 應用程式免于遭受常見的弱點和入侵。

[Azure 應用程式閘道][AppGW] 是一種專用的虛擬裝置，可提供受控應用程式傳遞控制器。 它為您的應用程式提供各種第7層負載平衡功能。 它可讓您將 CPU 密集 SSL 終止卸載至應用程式閘道，以優化 web 伺服陣列的效能。 它也提供其他第7層路由功能，例如迴圈配置傳入流量、以 cookie 為基礎的會話親和性、以 URL 路徑為基礎的路由，以及在單一應用程式閘道上裝載多個網站的能力。 Web 應用程式防火牆 (WAF) 也是提供為應用程式閘道 WAF SKU 的一部分。 此 SKU 會保護 Web 應用程式免於遭遇常見的 Web 弱點和攻擊。 應用程式閘道可以設定為面向網際網路的閘道、內部私人網路關或兩者的組合。

[公用 ip][PIP]。 使用某些 Azure 功能時，您可以將服務端點與公用 IP 位址建立關聯，以便從網際網路存取您的資源。 此端點會使用 NAT 將流量路由傳送至 Azure 虛擬網路上的內部位址和埠。 這個路徑是外部流量進入虛擬網路的主要方式。 您可以設定公用 IP 位址以決定要傳入的流量，以及該流量在虛擬網路上的轉譯方式和目的地。

[Azure DDoS 保護 Standard][DDoS] 可針對 [基本服務][DDoS] 層級提供額外的緩和功能，而這些服務層級特別針對 Azure 虛擬網路資源進行調整。 DDoS 保護標準很容易啟用，而且不需要變更應用程式。 保護原則會透過專用的流量監視和機器學習演算法進行調整。 原則會套用至與虛擬網路中部署的資源相關聯的公用 IP 位址。 範例包括 Azure Load Balancer、Azure 應用程式閘道和 Azure Service Fabric 實例。 近乎即時的系統產生記錄檔可透過 Azure 監視器的觀點，在攻擊期間和歷程記錄中取得。 應用程式層保護可透過 Azure 應用程式閘道 Web 應用程式防火牆來新增。 針對 IPv4 和 IPv6 Azure 公用 IP 位址提供保護。

中樞和輪輻拓撲使用虛擬網路對等互連和使用者定義路由來適當地路由傳送流量。

![8][8]

在此圖中，使用者定義的路由可確保流量會從輪輻流向防火牆，然後再透過 ExpressRoute 閘道傳遞給內部部署 (如果防火牆原則允許該流程) 。

#### <a name="component-type-monitoring"></a>元件類型：監視

[監視元件][MonitorOverview] 可提供所有其他元件類型的可見度和警示。 所有小組應該都可以存取他們可存取之元件和服務的監視。 如果您有集中式技術支援中心或作業小組，則他們需要具有這些元件所提供資料的整合式存取權。

Azure 提供不同類型的記錄和監視服務，以追蹤 Azure 裝載資源的行為。 Azure 中的工作負載控管和控制不僅以記錄資料的收集為基礎，也需仰賴依特定報告事件觸發動作的能力。

[Azure 監視器][AzureMonitor]。 Azure 包含多項服務，能在監視空間內個別執行特定的角色或工作。 這些服務共同提供一套完整的解決方案，可從您的應用程式和支援這些服務的 Azure 資源收集、分析及處理系統產生的記錄。 它們也可以用來監視重要的內部部署資源，以提供混合式監視環境。 瞭解可用的工具和資料是開發應用程式之完整監視策略的第一步。

Azure 監視器有兩種基本類型的記錄：

- [計量][Metrics]為數值，可描述系統在特定時間點的某個方面。 它們屬於輕量型，而且能夠支援近乎即時的案例。 對於許多 Azure 資源，您會在 Azure 入口網站的 [概觀] 頁面當中看到 Azure 監視器所收集的資料。 例如，查看任何虛擬機器，您會看到數個圖表顯示效能度量。 選取任何圖表，以在 Azure 入口網站中開啟計量瀏覽器中的資料，這可讓您將一段時間的多個計量值繪製成圖表。 您可以互動方式檢視圖表，或將其釘選到儀表板，利用其他視覺效果進行檢視。

- [記錄][Logs]包含不同類型的資料，而資料會針對每個類型組織成不同的屬性集。 事件和追蹤會儲存為記錄以及效能資料，而這些資料可以合併以進行分析。 可以使用查詢分析 Azure 監視器收集的記錄資料，以快速擷取、彙總和分析收集的資料。 記錄是從 [Log Analytics][LogAnalytics]儲存和查詢。 您可以在 Azure 入口網站中使用 Log Analytics 來建立和測試查詢，然後使用這些工具直接分析資料，或儲存查詢以便搭配視覺效果或警示規則使用。

![9][9]

「Azure 監視器」可以從各種來源收集資料。 您可以將應用程式的監視資料（範圍從您的應用程式、任何作業系統，以及它所依賴的服務）細分為 Azure 平臺本身。 Azure 監視器會從下列各層收集資料：

- **應用程式監視資料：** 您所撰寫之程式碼的效能和功能相關資料（不論其平臺為何）。
- **來賓 OS 監視資料：** 您的應用程式執行所在作業系統的相關資料。 此 OS 可能正在 Azure、另一個雲端或內部部署中執行。
- **Azure 資源監視資料：** 有關 Azure 資源操作的資料。
- **Azure 訂用帳戶監視資料：** 有關 Azure 訂用帳戶作業和管理的資料，以及有關 Azure 本身健康情況和作業的資料。
- **Azure 租使用者監視資料：** 租使用者層級 Azure 服務的作業相關資料，例如 Azure Active Directory。
- **自訂來源：** 也可以包含從內部部署來源傳送的記錄。 範例包括內部部署伺服器事件或網路裝置 syslog 輸出。

如果監視資料可以提高您對於運算環境作業的可見性，監視資料才有用處。 Azure 監視器包含數個功能和工具，可對您的應用程式及其相依的其他資源提供寶貴的深入解析。 監視解決方案和功能 (例如 Application Insights 和適用於容器的 Azure 監視器) 可針對您應用程式和特定 Azure 服務的不同層面提供深入解析。

Azure 監視器中的監視解決方案是幾組封裝的邏輯，可提供特定應用程式或服務的深入解析。 其中包括了用來為應用程式或服務收集監視資料的查詢，以及可用於視覺化的檢視 您可以從 Microsoft 及合作夥伴取得監視解決方案，為各種 Azure 服務和其他應用程式提供監視功能。

在收集所有這些豐富的資料之後，請務必對您環境中發生的事件採取主動式動作，因為手動查詢本身並不足夠。 Azure 監視器中的警示會主動通知您重大情況，並可能嘗試採取矯正措施。 以計量為基礎的警示規則會根據數值提供近乎即時的警示，而以記錄為基礎的規則則允許跨多個來源的資料進行複雜邏輯。 Azure 監視器中的警示規則會使用動作群組，其中包含幾組獨特的收件人以及多個規則可共用的動作。 根據您的需求，動作群組可以執行這類動作，例如使用會導致警示啟動外部動作或與您的 ITSM 工具整合的 webhook。

Azure 監視器也可讓您建立自訂儀表板。 Azure 儀表板可讓您將不同種類的資料 (包括計量和記錄) 結合至 Azure 入口網站的單一窗格中。 您可以選擇性地與其他 Azure 使用者共用儀表板。 您可以將 Azure 監視器的元素新增至 Azure 儀表板 (任何記錄查詢或計量圖表的輸出除外)。 例如，您可以建立一個儀表板，將顯示計量圖表、活動記錄表、來自 Application Insights 的使用情況圖表，以及記錄查詢輸出的圖格結合在一起。

最後，Azure 監視器資料是 Power BI 的原生來源。 Power BI 是一項商務分析服務，可提供橫跨各種資料來源的互動式視覺效果，而且是將資料提供給貴組織內外的其他人使用的有效方法。 您可以將 Power BI 設定為從 Azure 監視器自動匯入記錄資料，以便利用這些額外的視覺效果。

[Azure 網路][NetWatch] 監看員提供工具來監視、診斷和查看計量，以及啟用或停用 Azure 中虛擬網路內資源的記錄。 這是一項多元的服務，可提供下列功能和更多效用：

- 監視虛擬機器與端點之間的通訊。
- 檢視虛擬網路中的資源及其關聯性。
- 診斷進出於 VM 的網路流量篩選問題。
- 診斷來自 VM 的網路路由問題。
- 診斷來自 VM 的輸出連線。
- 擷取進出於 VM 的封包。
- 診斷虛擬網路閘道和連線的問題。
- 判斷 Azure 區域和網際網路服務提供者之間的相對延遲。
- 檢視網路介面的安全性規則。
- 檢視網路計量。
- 分析網路安全性群組的輸入或輸出流量。
- 檢視網路資源的診斷記錄。

#### <a name="component-type-workloads"></a>元件類型：工作負載

工作負載元件是實際應用程式和服務所在的位置。 這是您的應用程式開發小組花費最多時間的地方。

工作負載有各式各樣的可能性。 以下只是一些可能的工作負載類型：

**內部應用程式：** 企業營運應用程式對於企業營運而言是不可或缺的。 這些應用程式有一些常見的特性：

- **互動式：** 輸入資料，並傳回結果或報表。
- **資料驅動：** 經常存取資料庫或其他儲存體的大量資料。
- **整合式：** 提供與組織內部或外部的其他系統整合。

面向 **客戶的網站 (網際網路面向或內部面向的) ：** 大部分的網際網路應用程式都是網站。 Azure 可透過 IaaS 虛擬機器或 [azure Web Apps][WebApps] 網站 (PaaS) 來執行網站。 Azure Web Apps 會與虛擬網路整合，以將 Web 應用程式部署在輪輻網路區域中。 內部的網站不需要公開公用網際網路端點，因為這些資源可以透過私人虛擬網路的私人非網際網路可路由位址來存取。

**Big data analytics：** 當資料需要擴大至較大的磁片區時，關係資料庫可能無法在資料的極端負載或非結構化特性下執行。 [Azure HDInsight][HDInsight] 是雲端中適用于企業的受控、全方位的開放原始碼分析服務。 您可以使用開放原始碼架構，例如 Hadoop、Apache Spark、Apache Hive、LLAP、Apache Kafka、Apache Storm 和 R。 HDInsight 支援部署到以位置為基礎的虛擬網路，可部署到虛擬資料中心輪輻中的叢集。

**事件和訊息：** [Azure 事件中樞][EventHubs] 是大型的資料串流平臺和事件內嵌服務。 它每秒可接收和處理數百萬個事件。 它提供低延遲和可設定的時間保留，可讓您將大量資料內嵌至 Azure，並從多個應用程式讀取資料。 單一資料流程可以同時支援即時和批次型管線。

您可以透過 [Azure 服務匯流排][ServiceBus]在應用程式與服務之間實作高度可靠的雲端傳訊服務。 它提供用戶端與伺服器之間的非同步代理訊息、結構化的先進先出 (FIFO) 訊息，以及發佈和訂閱功能。

![10][10]

這些範例幾乎都是您可以在 Azure 中建立的工作負載類型的表面 &mdash; ，從基本 Web 和 SQL 應用程式到最新的 IoT、big data、機器學習服務、AI 等等。

### <a name="highly-availability-multiple-virtual-datacenters"></a>高可用性：多個虛擬資料中心

到目前為止，這篇文章的重點在於單一 VDC 的設計，描述有助於復原的基本元件和架構。 Azure 負載平衡器、Nva、可用性區域、可用性設定組、擴展集和其他功能等 azure 功能，可協助您將穩固的 SLA 層級納入生產服務中。

不過，由於虛擬資料中心通常會在單一區域內執行，因此可能會受到影響整個區域的中斷。 需要高可用性的客戶必須透過部署至不同區域的兩個或多個 VDC 部署中的相同專案來保護服務。

除了 SLA 考慮，有數個常見的案例可受益于執行多個虛擬資料中心：

- 您終端使用者或合作夥伴的區域或全球存在。
- 嚴重損壞修復需求。
- 一種機制，可在資料中心之間轉移流量以進行負載或效能。

#### <a name="regionalglobal-presence"></a>地區/全球支援

Azure 資料中心存在於全球許多區域。 選取多個 Azure 資料中心時，請考慮兩個相關因素：地理距離和延遲。 若要將使用者體驗優化，請評估每個虛擬資料中心之間的距離，以及每個虛擬資料中心與終端使用者之間的距離。

裝載虛擬資料中心的 Azure 區域必須符合您組織運作所在之任何法律管轄區的法規需求。

#### <a name="disaster-recovery"></a>災害復原

嚴重損壞修復計畫的設計取決於工作負載類型，以及在不同 VDC 執行之間同步處理這些工作負載狀態的能力。 在理想情況下，大部分的客戶都需要快速的容錯移轉機制，而這項需求可能需要在多個 VDC 的部署中執行的部署之間的應用程式資料同步處理。 不過，在設計災害復原方案時，務必考慮到延遲對大部分的應用程式而言可能都很敏感，而此資料同步可能會造成延遲。

不同 VDC 實作中的應用程式同步和活動訊號監視，都需要 VDC 實作透過網路進行通訊。 您可以透過下列方式連接不同區域中的多個 VDC：

- 跨相同虛擬 WAN 區域內的 Azure 虛擬 WAN 中樞內建的中樞對中樞通訊。
- 跨區域連接中樞的虛擬網路對等互連。
- ExpressRoute 私用對等互連，當每個 VDC 中的中樞都連線至相同的 ExpressRoute 線路時。
- 透過您的公司骨幹連接的多個 ExpressRoute 線路，以及連線到 ExpressRoute 線路的多個 VDC 實作為連接。
- 每個 Azure 區域中 VDC 的中樞區域之間的站對站 VPN 連線。

通常，虛擬 WAN 中樞、虛擬網路對等互連或 ExpressRoute 連線是網路連線的慣用功能，因為透過 Microsoft 骨幹傳遞的頻寬和一致延遲等級較高。

執行網路合格測試以驗證這些連線的延遲和頻寬，並根據結果決定同步或非同步資料複寫是否適當。 務必也要以最佳復原時間目標 (RTO) 的角度來衡量這些結果。

#### <a name="disaster-recovery-diverting-traffic-from-one-region-to-another"></a>災害復原：將流量從一個區域轉移到另一個區域

[Azure 流量管理員][azure-traffic-manager]和[Azure Front Door][azure-front-door]都會定期檢查不同 vdc 實作為接聽端點的服務健康狀態，如果這些端點失敗，則會自動路由至下一個最接近的 vdc。 流量管理員會使用即時使用者度量和 DNS，將使用者路由傳送至最接近的 (，或在失敗期間最接近最接近的時間) 。 Azure Front Door 是 100 Microsoft 骨幹 edge 網站上的反向 proxy，使用任意傳播功能將使用者路由傳送到最接近的接聽端點。

### <a name="summary"></a>摘要

資料中心遷移的虛擬資料中心方法會建立可調整的架構，以優化 Azure 資源的使用、降低成本，並簡化系統管理。 虛擬資料中心通常是以使用虛擬網路對等互連或虛擬 WAN 中樞)  (的中樞和輪輻網路拓撲為基礎。 中樞內提供的一般共用服務，以及特定的應用程式和工作負載會部署在輪輻中。 虛擬資料中心也會符合公司角色的結構，其中的不同部門（例如中央 IT、DevOps、作業和維護）會一起運作，同時執行其特定角色。 虛擬資料中心支援將現有的內部部署工作負載遷移至 Azure，但也提供雲端原生部署的許多優點。

## <a name="references"></a>參考資料

深入瞭解本檔中討論的 Azure 功能。

:::row:::
    :::column:::
        **網路功能** <br>
        [Azure 虛擬網路][virtual-network] <br>
        [網路安全性群組][NSG] <br>
        [服務端點][ServiceEndpoints] <br>
        [私人連結][PrivateLink] <br>
        [使用者定義的路由][UDR] <br>
        [網路虛擬設備][NVA] <br>
        [公用 IP 位址][PIP] <br>
        [Azure DNS][DNS]
    :::column-end:::
    :::column:::
        **負載平衡** <br>
        [Azure Front Door][azure-front-door] <br>
        [Azure Load Balancer (L4) ][ALB] <br>
        [應用程式閘道 (L7) ][AppGW] <br>
        [Azure 流量管理員][azure-traffic-manager]
    :::column-end:::
    :::column:::
        **連線能力** <br>
        [虛擬網路對等互連][virtual-network-peering] <br>
        [虛擬私人網路][VPN] <br>
        [虛擬 WAN][virtual-wan] <br>
        [ExpressRoute][ExR] <br>
        [ExpressRoute Direct][ExRD]
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **身分識別** <br>
        [Azure Active Directory][azure-ad] <br>
        [Multi-Factor Authentication][multi-factor-authentication] <br>
        [Azure 角色型存取控制][RBAC] \(部分機器翻譯\) <br>
        [Azure 內建角色][Roles]
    :::column-end:::
    :::column:::
        **監視** <br>
        [網路監看員][NetWatch] <br>
        [Azure 監視器][MonitorOverview] <br>
        [Log Analytics][LogAnalytics]
    :::column-end:::
    :::column:::
        **最佳作法** <br>
        [管理群組][MgmtGrp] <br>
        [訂用帳戶管理](../ready/azure-best-practices/scale-subscriptions.md) <br>
        [資源群組管理][RGMgmt] <br>
        [Azure 訂用帳戶限制][limits]
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **安全性** <br>
        [Azure 防火牆][AzFW] <br>
        [防火牆管理員][AzFWMgr] <br>
        [應用程式閘道 WAF][AppGWWAF] <br>
        [Front Door WAF][AFDWAF] <br>
        [Azure DDoS][DDoS]
    :::column-end:::
    :::column:::
        **其他 Azure 服務** <br>
        [Azure 儲存體][Storage] <br>
        [Azure SQL][SQL] <br>
        [Azure Web Apps][WebApps] <br>
        [Azure Cosmos DB][cosmos-db] <br>
        [HDInsight][HDInsight] <br>
        [事件中樞][EventHubs] <br>
        [服務匯流排][ServiceBus] <br>
        [Azure IoT][IoT] <br>
        [Azure Machine Learning][machine-learning]
    :::column-end:::
:::row-end:::

## <a name="next-steps"></a>後續步驟

- 深入瞭解 [虛擬網路對等互連][virtual-network-peering]，也就是中樞和輪輻拓撲的核心技術。
- 執行 [Azure Active Directory][azure-ad] 以使用 [Azure 角色型存取控制][RBAC]。
- 使用 Azure 角色型存取控制來開發訂用帳戶和資源管理模型，以符合組織的結構、需求和原則。 正在規劃最重要的活動。 分析重組、合併、新的產品線和其他考慮將如何影響您的初始模型，以確保您可以調整規模以符合未來的需求和成長。

<!-- images -->

[0]: ../_images/vdc/networking-vdc-redundant.png "元件重疊範例"
<!-- _1_ >: ../_images/vdc/networking-vdc-high-level.png "Example of hub and spoke VDC" -->
[2]: ../_images/vdc/networking-vdc-cluster.png "中樞和輪輻叢集"
[3]: ../_images/vdc/networking-vdc-spoke-to-spoke.png "支點對支點"
[4]: ../_images/vdc/networking-vdc-block-level-diagram.png "VDC 的區塊層級圖表"
[5]: ../_images/vdc/networking-vdc-users-groups-subscriptions.png "使用者、群組、訂用帳戶和專案"
[6]: ../_images/vdc/networking-vdc-infrastructure.png "基礎結構圖表"
[7]: ../_images/vdc/networking-vdc-perimeter.png "基礎結構圖表"
[8]: ../_images/vdc/networking-vdc-perimeter-peering.png "虛擬網路對等互連和周邊網路"
[9]: ../_images/vdc/networking-vdc-monitoring.png "Azure 監視器的圖表"
[10]: ../_images/vdc/networking-vdc-workloads.png "工作負載的圖表"
[11]: ../_images/vdc/networking-vdc-brief-flat.png "一般網路範例"
[12]: ../_images/vdc/networking-vdc-brief-mesh.png "網狀網狀網路範例"
[13]: ../_images/vdc/networking-vdc-brief-hub.png "中樞和輪輻 VDC 的範例"
[14]: ../_images/vdc/networking-vdc-brief-vwan.png "虛擬 WAN VDC 的範例"

<!-- links -->

[limits]: /azure/azure-resource-manager/management/azure-subscription-service-limits
[Roles]: /azure/role-based-access-control/built-in-roles
[virtual-network]: /azure/virtual-network/virtual-networks-overview
[NSG]: /azure/virtual-network/security-overview
[PrivateLink]: /azure/private-link/private-link-overview
[PrivateLinkSvc]: /azure/private-link/private-link-service-overview
[ServiceEndpoints]: /azure/virtual-network/virtual-network-service-endpoints-overview
[DNS]: /azure/dns/dns-overview
[PrivateDNS]: /azure/dns/private-dns-overview
[virtual-network-peering]: /azure/virtual-network/virtual-network-peering-overview
[UDR]: /azure/virtual-network/virtual-networks-udr-overview
[RBAC]: /azure/role-based-access-control/overview
[multi-factor-authentication]: /azure/active-directory/authentication/concept-mfa-howitworks
[azure-ad]: /azure/active-directory/fundamentals/active-directory-whatis
[VPN]: /azure/vpn-gateway/vpn-gateway-about-vpngateways
[ExR]: /azure/expressroute/expressroute-introduction
[ExRD]: /azure/expressroute/expressroute-erdirect-about
[virtual-wan]: /azure/virtual-wan/virtual-wan-about
[NVA]: /azure/architecture/reference-architectures/dmz/nva-ha
[AzFW]: /azure/firewall/overview
[AzFWMgr]: /azure/firewall-manager/overview
[MgmtGrp]: /azure/governance/management-groups/overview
[RGMgmt]: /azure/azure-resource-manager/management/manage-resource-groups-portal#what-is-a-resource-group
[ALB]: /azure/load-balancer/load-balancer-overview
[DDoS]: /azure/virtual-network/ddos-protection-overview
[PIP]: /azure/virtual-network/virtual-network-public-ip-address
[azure-front-door]: /azure/frontdoor/front-door-overview
[AFDWAF]: /azure/web-application-firewall/afds/afds-overview
[AppGW]: /azure/application-gateway/overview
[AppGWWAF]: /azure/web-application-firewall/ag/ag-overview
[MonitorOverview]: /azure/networking/networking-overview#monitor
[AzureMonitor]: /azure/azure-monitor/overview
[Metrics]: /azure/azure-monitor/platform/data-platform-metrics
[Logs]: /azure/azure-monitor/platform/data-platform-logs
[LogAnalytics]: /azure/azure-monitor/log-query/get-started-portal
[NetWatch]: /azure/network-watcher/network-watcher-monitoring-overview
[WebApps]: /azure/app-service/
[HDInsight]: /azure/hdinsight/hdinsight-overview
[EventHubs]: /azure/event-hubs/event-hubs-about
[ServiceBus]: /azure/service-bus-messaging/service-bus-messaging-overview
[azure-traffic-manager]: /azure/traffic-manager/traffic-manager-overview
[Storage]: /azure/storage/common/storage-introduction
[SQL]: /azure/sql-database/sql-database-technical-overview
[cosmos-db]: /azure/cosmos-db/introduction
[IoT]: /azure/iot-fundamentals/iot-introduction
[machine-learning]: /azure/machine-learning/overview-what-is-azure-ml
