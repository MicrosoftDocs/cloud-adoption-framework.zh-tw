---
title: Azure 安全性最佳做法
description: 瞭解 Microsoft 為最重要的 Azure 安全性最佳作法所建議的內容。
author: JanetCThomas
ms.author: mas
ms.date: 09/18/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: reference
ms.openlocfilehash: 1977d66b5bfc387e94b33106087ddf1df0c2ee20
ms.sourcegitcommit: a7eb2f6c4465527cca2d479edbfc9d93d1e44bf1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94712862"
---
# <a name="azure-security-best-practices"></a>Azure 安全性最佳做法

這些是 Microsoft 建議的最重要 Azure 安全性最佳作法，根據在客戶和我們自己的環境中所學到的經驗。

您可以在 [Microsoft 技術小組](https://techcommunity.microsoft.com/t5/video-hub/top-10-best-practices-for-azure-security/m-p/1698837)中觀看這些最佳作法的影片展示。

## <a name="1-people-educate-teams-about-the-cloud-security-journey"></a>1. 人員：教育團隊關於雲端安全性旅程

*小組必須瞭解他們所在的旅程。*

事項：教育您的安全性和 IT 小組 **瞭解** 雲端安全性旅程，以及他們將流覽的變更，包括：

- 雲端威脅的變更
- 共同責任模型及其對安全性的影響
- 雲端採用通常伴隨的文化和角色/責任變更

**原因**：移至雲端是很重要的變更，需要針對安全性進行轉換的思維和方法。 雖然安全性提供給組織的結果不會改變，但在雲端中達成此目標的最佳方式通常會變更，有時也會大幅改變。

在許多方面，移至雲端的方式類似于從獨立公司移至高度上升的豪華公寓大樓。 您仍有基本的基礎結構 (配管、電力等 ) 並執行類似的活動 (socializing、烹飪、電視和網際網路等 ) 但建築物 (gym、餐廳等的各項功能通常有很大的差異。

**誰**：安全性和 IT 組織中具有任何安全性責任的每個人都應該熟悉此內容，並將 (從 CIO/CISO 變更為技術專業人員) 。

**如何**：為小組提供在轉換至雲端環境期間成功部署和運作所需的內容。

Microsoft 已在旅程至雲端的客戶和我們自己的 IT 組織所學到的經驗發表經驗：

- 安全性組織如何發展[安全性角色和責任](../organize/cloud-security.md)
- [威脅環境、角色和數位策略的演進](/security/compass/microsoft-security-compass-introduction#evolution-of-threat-environment-roles--digital-strategies-2004)
- [安全性、策略、工具和威脅的轉換](/security/compass/microsoft-security-compass-introduction#transformation-of-security-strategies-tools--threats-1513)
- [從 Microsoft 體驗保護超大規模雲端環境的學習](/security/compass/microsoft-security-compass-introduction#microsoft-security-practices-1349) ，可協助您進行旅程

另請參閱 Azure 安全性基準測試 [GS-3：讓組織角色、職責和責任保持一致](/azure/security/benchmarks/security-controls-v2-governance-strategy#gs-3-align-organization-roles-responsibilities-and-accountabilities)。

## <a name="2-people-educate-teams-on-cloud-security-technology"></a>2. 人員：教育小組雲端安全性技術

*人們必須瞭解他們的進展。*

**什麼是**：確保您的小組有時間在保護雲端資源的技術教育上進行設定，包括：

- 雲端技術與雲端安全性技術
- 建議的設定和最佳作法
- 您可以視需要深入瞭解技術詳細資料

**原因**：技術小組需要存取技術資訊，以做出明智的安全性決策。 技術小組很適合用來學習工作上的新技術，但雲端中的詳細資料量通常會 overwhelms 其符合每日例行工作的能力。

打造技術學習的專用時間，可協助確保人員有時間能夠評估雲端安全性，並思考如何調整現有的技能和流程。 即使是軍事中最聰明的特殊營運團隊，也需要訓練和智慧才能以最佳方式執行。

**誰**：直接與雲端技術互動的所有角色 (在安全性和 IT 部門) 應該將時間提供給雲端平臺上的技術學習，以及如何保護這些角色。

此外，安全性和 IT 技術管理員 (，且通常是專案經理) 應該熟悉一些可保護雲端 (資源的技術詳細資料，因為這可協助他們更有效地領導和協調雲端創新的) 。

作法 **：確定** 安全性的技術專業人員有時間設定有關如何保護雲端資產的自學訓練。 雖然這不一定是可行的，但在理想的情況下，您可以透過經驗豐富的講師和實習實驗室來存取正式的訓練。

> [!IMPORTANT]
> 身分識別通訊協定對於雲端中的存取控制很重要，但通常不會優先使用內部部署安全性，因此安全性小組應該確保專注于熟悉這些通訊協定和記錄。

Microsoft 提供廣泛的資源，協助技術專業人員提升 Azure 資源的安全性和報告合規性：

- Azure 安全性
  - AZ-500 [學習路徑](/learn/certifications/exams/az-500?tab=tab-learning-paths) (和認證) 
  - [Azure 安全性基準測試 (ASB) ](/azure/security/benchmarks/) –適用于 azure 安全性的規範最佳作法和控制項
    - [適用于 azure 的安全性基準](/azure/security/benchmarks/security-baselines-overview) -ASB 至個別 Azure 服務的應用程式
  - [Microsoft 安全性最佳作法](/security/compass/microsoft-security-compass-introduction) -影片和檔
- Azure 法規遵循
  - Azure 資訊安全中心的[法規合規性](/azure/security-center/security-center-compliance-dashboard)評估
- 身分識別通訊協定和安全性
  - [Azure 安全性檔案網站](/azure/security/)
  - Azure AD 驗證 [YouTube 系列](https://www.youtube.com/playlist?list=PLLasX02E8BPD5vC2XHS_oHaMVmaeHHPLy)
  - [使用 Azure active directory 保護 Azure 環境](https://azure.microsoft.com/resources/securing-azure-environments-with-azure-active-directory/)

另請參閱 Azure 安全性基準測試 [GS-3：讓組織角色、職責和責任保持一致](/azure/security/benchmarks/security-controls-v2-governance-strategy#gs-3-align-organization-roles-responsibilities-and-accountabilities)

## <a name="3-process-assign-accountability-for-cloud-security-decisions"></a>3. 進程：指派雲端安全性決策的責任

*如果沒有人負責制定安全性決策，就不會進行。*

**事項：指定** 負責為企業 Azure 環境進行各類安全性決策的人員。

**原因**：清除安全性決策的擁有權可加速雲端採用 *並* 提高安全性。 缺乏的情況通常會造成不相關的問題，因為沒有人有能力進行決策、沒有人知道誰要要求決策，而且沒有人有主動性研究明智的決策。 這項摩擦通常會妨礙商務目標、開發人員時程表、IT 目標和安全性保證，進而導致：

- 停止等待安全性核准的專案
- 無法等待安全性核准的不安全部署

**誰**：安全性領導力會指定哪些小組或個人負責制定有關雲端的安全性決策。

**如何**：指定將負責進行重要安全性決策的群組 (或個人) 。

記錄這些擁有者、他們的連絡人資訊，並在安全性、IT 和雲端團隊內交際這項資訊，以確保所有角色都能輕鬆地與他們聯絡。

這些是需要安全性決策的一般區域、描述，以及哪些小組通常會進行決策。

| 決策         | 描述           | 一般小組  |
| ------------- |-------------| -----|
| 網路安全性 | Azure 防火牆的設定和維護、網路虛擬裝置 (以及相關聯的路由) 、Waf、Nsg、Asg 等。 |    *[基礎結構和端點安全性](../organize/cloud-security-infrastructure-endpoint.md)小組通常著重于網路安全性*  |
| 網路管理 | 企業級的虛擬網路和子網配置  | *一般來說 [，在中央 IT 作業](../organize/central-it.md)中，現有的網路作業小組* |
| 伺服器端點安全性 | 監視及修復伺服器安全性 (修補、設定、端點安全性等 )   | *通常 [中央 IT 營運](../organize/central-it.md) 和 [基礎結構和端點安全性](../organize/cloud-security-infrastructure-endpoint.md) 小組共同合作* |
| 事件監視和回應 | 調查和補救 SIEM 或來源主控台中的安全性事件 (Azure 資訊安全中心、Azure AD Identity Protection 等 )  | *一般 [安全性作業](../organize/cloud-security-operations-center.md) 小組* |
| 原則管理 | 設定使用角色型存取控制的方向 (RBAC) 、Azure 資訊安全中心、系統管理員保護原則，以及用來管理 Azure 資源的 Azure 原則 | *通常是 [原則和標準](../organize/cloud-security-policy-standards.md)  +  [安全性架構](../organize/cloud-security-architecture.md)小組共同* |
| 身分識別安全性與標準 | Azure AD 目錄、PIM/PAM 使用量、MFA、密碼/同步處理設定、應用程式識別標準的設定方向 | *通常是身分 [識別與金鑰管理](../organize/cloud-security-identity-keys.md)  +  [原則以及標準](../organize/cloud-security-policy-standards.md)  +  [安全性架構](../organize/cloud-security-architecture.md)小組共同*  |

> [!NOTE]
>
> - 請確定決策者在其雲端區域中具有適當的教育，以伴隨此責任。
> - 請確定在原則和標準中記載決策，以提供記錄，並在長期引導組織。

另請參閱 Azure 安全性基準測試 [GS-3：讓組織角色、職責和責任保持一致](/azure/security/benchmarks/security-controls-v2-governance-strategy#gs-3-align-organization-roles-responsibilities-and-accountabilities)

## <a name="4-process-update-incident-response-ir-processes-for-cloud"></a>4. 進程：更新雲端 (IR) 進程的事件回應

*您沒有時間規劃危機期間的危機。*

**什麼**：更新處理常式並準備分析師，以回應您 Azure 雲端平臺上的安全性事件 (包括您已採用) 的任何 [原生威脅偵測工具](../get-started/security.md#step-1-establish-essential-security-practices) 。 更新程式、準備您的小組，以及實務模擬的攻擊，讓他們在事件調查、補救和威脅搜尋期間都能以最佳方式執行。

**原因**：主動式攻擊者對組織帶來立即的風險，可能很快就會變得很難控制，因此您必須快速有效地回應攻擊。 此事件回應 (IR) 進程必須適用于您的整個資產，包括裝載企業資料、系統和帳戶的所有雲端平臺。

和許多方面的相似之處在于，雲端平臺與內部部署系統之間有重要的技術差異，可能會破壞現有的程式，通常是因為資訊以不同的形式提供。 安全性分析師可能會迅速回應不熟悉的環境，因為它們只會在傳統內部部署架構和網路/磁片的辯論) 方法上進行定型，所以會減緩 (的問題。

**誰**：現代化 IR 程式通常是由 [安全性作業](../organize/cloud-security-operations-center.md) 所導致，並支援其他群組的知識與專業知識。

- *贊助* -此程式現代化通常是由安全性作業總監或對等專案贊助。
- *執行* -將現有的程式 (或第一次撰寫) 是一項共同作業的工作，其中包括：

  - **[安全性作業](../organize/cloud-security-operations-center.md)** 事件管理小組或領導階層–會產生更新來處理和整合重要的外部專案關係人，包括法律和溝通/公共關係小組
  - **[安全性作業](../organize/cloud-security-operations-center.md)** 安全性分析師-提供技術事件調查和分級的專業知識
  - **[中央 IT 營運](../organize/central-it.md)** -透過雲端平臺的卓越 (，或透過外部顧問) ，提供雲端平臺的專業知識。

**如何**：更新處理常式並準備您的小組，讓他們知道當他們找到作用中的攻擊者時該怎麼辦。

- **流程和操作手冊：** 調整現有的調查、補救和威脅搜尋程式，以瞭解雲端平臺的運作方式 (新的/不同的工具、資料來源、身分識別通訊協定等 ) 。

- **教育：** 教育分析師的整體雲端轉型、平臺運作方式的技術詳細資料，以及新的/更新的程式，讓他們知道有何不同，以及在何處可以找到所需的內容。

**重要焦點區域**：雖然資源連結中說明了許多詳細資料，但這些都是專注于教育和規劃工作的重要區域：

- **共用的責任模型和雲端架構：** 對安全性分析師而言，Azure 是一種軟體定義的資料中心，可提供許多服務，包括 (熟悉) 的 Vm，以及與內部部署非常不同的 Vm （例如 Azure SQL Azure Functions 等），其中最適合的資料位於服務記錄或特製化的威脅偵測服務中，而不是由) Microsoft 所營運的基礎 OS/ (Vm 的記錄檔或專門的威脅 分析師需要瞭解此內容，並將其整合到其每日工作流程中，以便他們知道預期的資料、取得的位置，以及其所要的格式。
- **端點資料來源：** 使用原生雲端偵測工具（例如 Azure 資訊安全中心和 EDR 系統）（而不是傳統的直接磁片存取方法），在雲端裝載的伺服器上取得攻擊和惡意程式碼的見解和資料通常更快、更容易且更準確。 雖然直接磁片剖析適用于法律訴訟 (電腦取證的情況，但 [在 Azure) 中](/azure/architecture/example-scenario/forensics/) ，這通常是偵測和調查攻擊的最沒有效率的方法。
- **網路和身分識別資料來源：** 雲端平臺的許多功能主要是使用身分識別來進行存取控制，例如存取 Azure 入口網站 (但是網路存取控制也廣泛使用) 。 這需要分析師開發人員瞭解雲端身分識別通訊協定，以取得完整、豐富的攻擊者活動 (和合法的使用者活動) 來支援事件調查和補救。 身分識別目錄和通訊協定也會與內部部署不同，因為它們通常是以 SAML、OAuth、OIDC 和雲端目錄為基礎，而不是 LDAP、Kerberos、NTLM 和通常在內部部署環境中找到的 Active Directory。
- **練習練習：** 模擬的攻擊和回應有助於為您的安全性分析師、威脅獵人及、事件管理員和組織中的其他專案關係人建立組織肌肉記憶體和技術準備。 學習作業並進行調整是事件回應的一個自然部分，但您應該努力將您必須在危機中學習的數量降到最低。

**重要資源：**

- [事件回應參考指南](https://aka.ms/IRRG) (IRRG) 
- [建立您自己的安全性事件回應流程](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)的指引
- [Azure 記錄和警示](/azure/security/fundamentals/log-audit)
- Microsoft 安全性最佳做法
  - [安全性、策略、工具 & 威脅的轉換](/security/compass/microsoft-security-compass-introduction#transformation-of-security-strategies-tools--threats-1513)
  - [安全性作業](/security/compass/security-operations-videos-and-decks)
- 從網路防禦作業中心 (CDOC) 的 Microsoft 學習
  - [學習的整體經驗](https://www.microsoft.com/security/blog/2019/02/21/lessons-learned-from-the-microsoft-soc-part-1-organization/)
  - [事件調查](https://www.microsoft.com/security/blog/2019/12/23/ciso-series-lessons-learned-from-the-microsoft-soc-part-3b-a-day-in-the-life/)
  - [事件補救](https://www.microsoft.com/security/blog/2020/05/04/lessons-learned-microsoft-soc-part-3c/)

另請參閱 Azure 安全性基準測試 [IR-1：準備–更新 azure 的事件回應](/azure/security/benchmarks/security-controls-v2-incident-response#ir-1-preparation--update-incident-response-process-for-azure)程式。

## <a name="5-process-establish-security-posture-management"></a>5. 進程：建立安全性狀態管理

*首先，請先知道 thyself。*

**什麼**：確定您主動管理 Azure 環境的安全性狀態：

- 指派明確的責任擁有權給
  - 監視安全性狀態
  - 降低資產的風險
- 自動化和簡化這些工作

**原因**：迅速找出並修復常見的安全防護風險，可大幅降低組織的風險。

雲端資料中心的軟體定義本質可利用廣泛的資產檢測，持續監視安全性風險 (軟體弱點、安全性錯誤等 ) 。 開發人員和 IT 小組可以用來部署 Vm、資料庫和其他資源的速度也需要確保資源安全且受到主動監視。

這些新功能可提供新的可能性，但從這些新功能實現價值需要指派責任給他們。 在快速發展的雲端作業上一致地執行，也需要盡可能讓人力流程保持簡單並自動化。 請參閱「磁片磁碟機簡單性」 [安全性原則](/azure/architecture/framework/security/security-principles)。

> [!NOTE]
 > 簡化和自動化的目標並不是要擺脫工作，而是要將重複性工作的負擔從人們中移除，讓他們可以專注于更高價值的人類活動，像是參與和教育 IT 和 DevOps 團隊。

**誰**：這通常分為兩組責任：

- **[安全性狀態管理](../organize/cloud-security-posture-management.md)** –這個較新的功能通常是現有弱點管理或治理功能的演進。 這包括使用 Azure 資訊安全中心的安全分數和其他資料來源來監視整體安全性狀態、主動與資源擁有者合作以降低風險，並向安全性領導階層報告風險。
- **安全性補救：** 為負責管理這些資源的小組指派責任，以解決這些風險。 這應該是 DevOps 團隊管理自己的應用程式資源，或是在 **[中央 IT 營運](../organize/central-it.md)** 中管理技術專屬的團隊：

  - **計算和應用程式資源：**
    - **應用程式服務** -應用程式開發/安全性小組 (s) 
    - **容器** -應用程式開發和/或基礎結構/IT 作業
    - **Vm/擴展集/計算** -IT/基礎結構作業
  - **資料 & 儲存體資源：**
    - **SQL/Redis/Data Lake Analytics/Data Lake Store** -資料庫小組
    - **儲存體帳戶** -儲存體/基礎結構小組
  - **身分識別和存取資源：**
    - 訂用 **帳戶-身分** 識別小組 (s) 
    - **Key Vault** –身分識別或資訊/資料安全性小組
  - 網路 **資源**-網路安全性小組
  - **Iot 安全性** -Iot 營運小組

**如何**：安全性是每個人的工作，但不是每個人都知道它的重要性、要做什麼，以及如何執行。

- 保有資源擁有者對安全性風險負的責任，因為它們是針對可用性、效能、成本和其他成功因素所負的責任。
- 支援資源擁有者清楚瞭解安全性風險對其資產的重要性、他們應該如何減輕風險，以及如何以最低的生產力損失來實現。

> [!IMPORTANT]
> 在不同的資源類型和應用程式之間，如何保護資源的原因、原因和方式的說明通常很類似，但請務必將這些資源與每個小組已經知道並在意的專案建立關聯。 安全性小組應該與其 IT 和 DevOps 的合作關係，以信任的顧問和合作夥伴為焦點，讓這些團隊能成功進行。

**工具**： Azure 資訊安全中心的 [安全分數](/azure/security-center/security-center-secure-score) 可針對各種不同的資產，評估 Azure 中最重要的安全性資訊。 這應該是您的狀態管理起點，而且可以視需要使用自訂的 Azure 原則和其他機制來補充。

**頻率**：設定週期性步調 (通常是每月) ，以使用特定的改進目標來審查 Azure 安全分數和方案計畫。 您可以視需要增加頻率。

> [!TIP]
 > 盡可能 Gamify 活動以提高參與度，例如為 DevOps 小組建立有趣的競賽和獎品，以改善分數。

另請參閱 Azure 安全性基準測試 [GS-2：定義安全性狀態管理原則](/azure/security/benchmarks/security-controls-v2-governance-strategy#gs-2-define-security-posture-management-strategy)。

## <a name="6-technology-require-passwordless-or-multi-factor-authentication-mfa"></a>6. 技術：需要無密碼或 Multi-Factor Authentication (MFA) 

*您是否願意在企業安全性攻擊者無法猜出或竊取您系統管理員密碼的安全性？*

**原因**：要求所有重大影響管理員使用無密碼或多重要素驗證 (MFA) 。

**原因**：就像較新的基本架構金鑰不會保護房子以進行現代化的竊賊，密碼無法保護帳戶以抵禦我們今天看到的常見攻擊。 [您的 Pa $ $word](https://techcommunity.microsoft.com/t5/azure-active-directory-identity/your-pa-word-doesn-t-matter/ba-p/731984)中描述的技術詳細資料並不重要。

雖然 MFA 是一項繁瑣的額外步驟，但無密碼方法目前會使用生物特徵辨識方法（例如 Windows Hello 和行動 (裝置中的臉部辨識）來改善登入體驗，而不需要記住或輸入密碼) 。 此外，零信任方法會記住受信任的裝置，這會減少提示您有討厭的頻外 MFA 動作 (查看 [使用者登入頻率](/azure/active-directory/conditional-access/howto-conditional-access-session-lifetime#user-sign-in-frequency)) 。

**誰**：密碼和多重要素方案通常是由身分 [識別和金鑰管理](../organize/cloud-security-identity-keys.md) 及/或 [安全性架構](../organize/cloud-security-architecture.md)所導致。

- *贊助* -這通常是由 CISO、CIO 或身分識別總監贊助
- *執行* -這是一項共同作業，牽涉到
  - [原則和標準](../organize/cloud-security-policy-standards.md) 小組建立明確的需求
  - 執行原則的身分[識別和金鑰管理](../organize/cloud-security-identity-keys.md)或[中央 IT 作業](../organize/central-it.md)
  - [安全性合規性管理](../organize/cloud-security-compliance-management.md) 監視以確保合規性

**如何**：執行無密碼或 MFA 驗證、將系統管理員訓練 (視需要) ，並要求系統管理員遵循已寫入的原則。 這可以透過下列一或多項技術來達成：

- [無密碼 (Windows Hello) ](/windows/security/identity-protection/hello-for-business/hello-identity-verification)
- [無密碼 (驗證器應用程式) ](/azure/active-directory/authentication/howto-authentication-phone-sign-in)
- [Azure 多重要素驗證](/azure/active-directory/authentication/howto-mfa-userstates)
- 協力廠商 MFA 解決方案

> [!NOTE]
> 以文字訊息為基礎的 MFA 現在相對較便宜，讓攻擊者無法略過，因此專注于無密碼 & 更強的 MFA。

另請參閱 Azure 安全性基準測試 [識別碼-4：針對所有以 Azure Active Directory 為基礎的存取，使用強式驗證控制項](/azure/security/benchmarks/security-controls-v2-identity-management#id-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access)。

## <a name="7-technology-integrate-native-firewall-and-network-security"></a>7. 技術：整合原生防火牆和網路安全性

*簡化系統和資料對網路攻擊的保護。*

**什麼**：整合 azure 防火牆、Azure Web 應用程式防火牆 (WAF) ，以及分散式阻絕服務 (DDoS) 緩和措施，以簡化您的網路安全性策略與維護。

**原因**：簡化是安全性的關鍵，因為它可降低風險的風險，而不會造成混淆、錯誤和其他人為錯誤。 請參閱「磁片磁碟機簡單性」 [安全性原則](/azure/architecture/framework/security/security-principles)。

防火牆和 Waf 是保護應用程式免于惡意流量的重要基本安全性控制，但其設定和維護可能很複雜，而且會耗用大量的安全性小組時間和注意 (類似于將自訂 aftermarket 元件新增至汽車) 。 Azure 的原生功能可以簡化防火牆、Web 應用程式防火牆、分散式阻絕服務 (DDoS) 緩和措施等的執行和操作。

這可以讓您的小組在評估 Azure 服務的安全性、自動化安全性作業，以及將安全性與應用程式和 IT 解決方案整合時，釋出更高價值的安全性工作。

**誰**：

- *贊助*：這項網路安全性策略更新通常是由安全性領導力和/或 IT 領導階層贊助
- *執行*：將這些整合到您的雲端網路安全性策略，是一項共同作業的工作，包括：

  - **[安全性架構](../organize/cloud-security-architecture.md)** -使用雲端網路與雲端網路安全性潛在客戶建立雲端網路安全性架構。
  - **雲端網路領導** ([中央 IT 營運](../organize/central-it.md)) + **雲端網路安全性的潛在客戶** ([基礎結構安全性小組](../organize/cloud-security-infrastructure-endpoint.md)) 
    - 使用安全性架構設計人員建立雲端網路安全性架構
    - 設定防火牆、NSG 和 WAF 功能，並使用 WAF 規則上的應用程式架構設計人員
  - **應用程式架構設計人員：** 使用網路安全性來建立並精簡 WAF 規則集和 DDoS 設定，以保護應用程式，而不會中斷可用性

**如何**：希望簡化作業的組織有兩個選項：

- **擴充現有的功能和架構：** 許多組織通常會選擇擴充現有防火牆功能的使用方式，以便將現有的投資運用在技能和流程整合中，特別是在第一次採用雲端時。
- 採用 **原生安全性控制項：** 越來越多的組織開始偏好使用原生控制項，以避免整合協力廠商功能的複雜度。 這些組織通常會想要避免在負載平衡、使用者定義的路由、防火牆/WAF 本身，以及不同技術小組之間遞交延遲的風險。 此選項特別吸引採用基礎結構即程式碼方法的組織，因為它們可以更輕鬆地自動化和檢測內建功能，而不是協力廠商功能。

您可以在下列位置找到 Azure 原生網路安全性功能的相關檔：

- [Azure 防火牆](/azure/firewall/overview)
- [Azure Web 應用程式防火牆 (WAF)](/azure/web-application-firewall/)
- [Azure DDoS 保護](/azure/virtual-network/ddos-protection-overview)

[Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps?page=1&search=firewall) 包含許多協力廠商防火牆提供者。

另請參閱 Azure 安全性基準測試 [NS-4：保護應用程式和服務免于外部網路攻擊](/azure/security/benchmarks/security-controls-v2-network-security#ns-4-protect-applications-and-services-from-external-network-attacks)。

## <a name="8-technology-integrate-native-threat-detection"></a>8. 技術：整合原生威脅偵測

*針對 Azure 系統和資料簡化攻擊的偵測和回應。*

**功能**：將原生威脅偵測功能併入您的安全性作業和 SIEM 中，藉此簡化您的威脅偵測和回應策略。

**原因**：安全性作業的目的是要降低取得環境存取權的主動攻擊者對環境的影響，以平均時間認可 (MTTA) 並補救 (MTTR) 事件。 這在事件回應的所有元素中都需要精確度和速度，因此工具品質和程式執行效率都是最重要的。

使用現有的工具和方法來偵測內部部署威脅偵測是很困難的，因為雲端技術的差異與其變更的速度很快。 原生整合的偵測可提供雲端提供者所維護的產業規模解決方案，以隨時掌握目前的威脅和雲端平臺變更。

這些原生解決方案也可讓安全性作業小組專注于事件調查和補救，而不必浪費時間嘗試從不熟悉的記錄資料建立警示、整合工具和維護工作。

**誰**：這通常是由 [安全性作業](../organize/cloud-security-operations-center.md) 小組驅動。

- **贊助：** 這通常是由安全性作業 Director (或對等) 來贊助。
- **執行：** 整合原生威脅偵測是一項共同作業，其中包括：
  - **[安全性作業](../organize/cloud-security-operations-center.md)：** 將警示整合至 SIEM 和事件調查程式、教育分析師的雲端警示和其意義，以及如何使用原生雲端工具。
  - **[事件準備](../organize/cloud-security-incident-preparation.md)：** 將雲端事件整合至實務練習，並確保執行練習以推動團隊的就緒。
  - **[威脅情報](../organize/cloud-security-threat-intelligence.md)：** 研究及整合雲端攻擊的資訊，以通知小組內容和智慧。
  - **[安全性架構](../organize/cloud-security-architecture.md)：** 將原生工具整合至安全性架構檔。
  - **[原則和標準](../organize/cloud-security-policy-standards.md)：** 設定可在整個組織中啟用原生工具的標準和原則。 監視合規性。
  - **[基礎結構和端點](../organize/cloud-security-infrastructure-endpoint.md)**  / **[中央 IT 營運](../organize/central-it.md)：** 設定及啟用偵測，並整合到自動化和基礎結構即程式碼解決方案。

**如何**： [在 Azure 安全性中心](/azure/security-center/threat-protection) 針對您所使用的所有資源啟用威脅偵測，並讓每個小組將這些資源整合到上述程式中。

另請參閱 Azure 安全性基準測試 [LT-1：啟用 azure 資源的威脅偵測](/azure/security/benchmarks/security-controls-v2-logging-threat-detection#lt-1-enable-threat-detection-for-azure-resources)。

## <a name="9-architecture-standardize-on-a-single-directory-and-identity"></a>9. 架構：標準化單一目錄和身分識別

*沒有人想要處理多個身分識別和目錄。*

**事項：在** 單一 Azure AD 目錄上標準化，並為 Azure (中的每個應用程式和使用者提供單一身分識別，以) 所有企業身分識別功能。

> [!NOTE]
> 這種最佳做法是專門針對企業資源。 針對合作夥伴帳戶，請使用 [AZURE AD B2B](/azure/active-directory/external-identities/what-is-b2b) ，如此就不需要在您的目錄中建立和維護帳戶。 若為客戶/公民帳戶，請使用 [Azure AD B2C](/azure/active-directory-b2c/) 管理它們。

**原因**：多個帳戶和身分識別目錄會針對生產力使用者、開發人員、IT 和身分識別管理員、安全性分析師和其他角色，在日常工作流程中建立不必要的分歧和混淆。

管理多個帳戶和目錄也會為不佳的安全性作法帶來獎勵，例如跨帳戶重複使用相同的密碼，並提高攻擊者可鎖定的過時/放棄帳戶的可能性。

雖然有時可以更輕鬆地根據 LDAP 來 (以 LDAP 為基礎的自訂目錄，以及針對特定應用程式或工作負載 ) ，這樣做會產生更多的整合和維護工作來進行設定和管理。 這在許多方面都很類似，可決定如何設定額外的 Azure 租使用者或其他內部部署 Active Directory 樹系，以及使用現有的企業帳戶。 另請參閱「磁片磁碟機簡單性」 [安全性原則](/azure/architecture/framework/security/security-principles)。

**誰**：這通常是由 [安全性架構](../organize/cloud-security-architecture.md) 或身分 [識別和金鑰管理](../organize/cloud-security-identity-keys.md) 小組所推動的跨小組工作。

- *贊助* -這通常是由身分 [識別和金鑰管理](../organize/cloud-security-identity-keys.md) 和 [安全性架構](../organize/cloud-security-architecture.md) 所贊助 (但某些組織可能需要 CISO 或 CIO 的贊助) 
- *執行* –這是一項共同作業的工作，其中包括：
  - **[安全性架構](../organize/cloud-security-architecture.md)：** 納入安全性和 IT 架構檔和圖表
  - **[原則和標準](../organize/cloud-security-policy-standards.md)：** 符合規範的檔原則和監視
  - 身分 **[識別和金鑰管理](../organize/cloud-security-identity-keys.md)** 或 **[集中 IT 作業](../organize/central-it.md)**，藉由啟用功能並支援帳戶、教育等的開發人員來執行此原則。
  - **應用程式開發人員** 和/或 **[中央 IT 作業](../organize/central-it.md)：** 在應用程式中使用身分識別和 Azure 服務設定 (責任會根據 DevOps 採用層級而有所不同) 

**如何**：採用以新的 _greenfield_ 功能開始的實用方法 (目前) ，然後透過 _棕色地帶_ 現有的應用程式和服務來清除挑戰，作為後續練習：

- **Greenfield：** 建立並執行明確的原則，讓所有企業身分識別都應該使用單一 Azure AD 目錄，並為每個使用者使用單一帳戶。

- **棕色地帶：** 許多組織通常會有多個舊版目錄和身分識別系統。 當進行中的管理衝突的成本超過將其清除的投資時，請解決這些衝突。 雖然身分識別管理和同步處理解決方案可以減輕其中一些問題，但它們缺乏安全性和生產力功能的緊密整合，可讓使用者、系統管理員和開發人員順暢地體驗。

在應用程式開發週期中，合併使用身分識別的理想時機是：

- 將雲端的應用程式現代化
- 使用 DevOps 流程來更新雲端應用程式

雖然在非常獨立的業務單位或法規需求的情況下，不同的目錄有有效的原因，但在所有其他情況下都應避免使用多個目錄。

另請參閱 Azure 安全性基準測試 [識別碼-1：將 Azure Active Directory 標準化為中央身分識別和驗證系統](/azure/security/benchmarks/security-controls-v2-identity-management#id-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system)。

> [!IMPORTANT]
> 單一帳戶規則唯一的例外是具有特殊許可權的使用者 (包括 IT 系統管理員和安全性分析師) 應該針對標準使用者工作與系統管理工作擁有不同的帳戶。
>
> 如需詳細資訊，請參閱 Azure 安全性基準測試特殊 [許可權存取](/azure/security/benchmarks/security-controls-v2-privileged-access)。

## <a name="10-architecture-use-identity-based-access-control-instead-of-keys"></a>10. 架構：使用以身分識別為基礎的存取控制 (而非金鑰) 

**原因**：在任何可能的情況下，使用 Azure AD 身分識別，而不是金鑰型驗證 (Azure 服務、應用程式、api 等 ) 。

**原因**：金鑰型驗證可以用來對雲端服務和 api 進行驗證，但需要安全地管理金鑰，這是執行妥善 (的挑戰，特別是大規模) 。 安全金鑰管理對於開發人員和基礎結構專業人員等非安全性專業人員而言很困難，而且通常無法安全地進行，通常會為組織帶來重大的安全性風險。

以身分識別為基礎的驗證克服了許多挑戰，並提供秘密輪替、生命週期管理、系統管理委派等的成熟功能。

**誰**：這通常是由 [安全性架構](../organize/cloud-security-architecture.md) 或身分 [識別和金鑰管理](../organize/cloud-security-identity-keys.md) 小組所推動的跨小組工作。

- *贊助* -這通常是由 [安全性架構](../organize/cloud-security-architecture.md) 或身分 [識別和金鑰管理](../organize/cloud-security-identity-keys.md)  所贊助 (但某些組織可能需要 CISO 或 CIO) 的贊助。
- *執行* –這是一項共同作業的工作，牽涉到
  - **[安全性架構](../organize/cloud-security-architecture.md)：** 結合安全性和 IT 架構圖表與檔。
  - **[原則和標準](../organize/cloud-security-policy-standards.md)：** 符合規範的檔原則和監視。
  - 身分 **[識別和金鑰管理](../organize/cloud-security-identity-keys.md)** 或 **[集中 IT 作業](../organize/central-it.md)**，藉由啟用功能並支援帳戶、教育版等開發人員來執行此原則。
  - **應用程式開發人員** 和/或 **[中央 IT 作業](../organize/central-it.md)：** 在應用程式中使用身分識別和 Azure 服務設定 (責任會根據 DevOps 採用) 層級而有所不同。

**如何**：設定組織喜好設定和習慣使用以身分識別為基礎的驗證需要遵循處理常式並啟用技術。

**程序：**

1. **建立** 明確概述預設身分識別型驗證以及可接受例外狀況的原則和標準。
2. **教育** 開發人員 & 基礎結構小組，瞭解為何要使用新方法、他們需要做什麼，以及如何執行。
3. 以實用的方式 **執行** 變更–從現在採用的新 greenfield 功能開始，以及未來的 (新的 Azure 服務、新的應用程式) ，然後再清除現有的棕色地帶設定。
4. **監視** 合規性，並追蹤開發人員和基礎結構小組以進行補救。

**技術：** 針對非人力帳戶（例如服務或自動化），請使用 [受控](/azure/active-directory/managed-identities-azure-resources/overview)識別。 Azure 受控識別可以向支援 Azure AD authentication 的 Azure 服務和資源進行驗證。 透過預先定義的存取授與規則來啟用驗證，以避免在原始程式碼或設定檔中使用硬式編碼的認證。

針對不支援受控識別的服務，請使用 Azure AD，改為在資源層級上建立具有限制許可權的 [服務主體](/azure/active-directory/develop/app-objects-and-service-principals) 。 我們建議您設定具有憑證認證的服務主體，並切換回用戶端密碼。 在這兩種情況下， [Azure Key Vault](/azure/key-vault/general/overview) 可以搭配使用 azure 受控識別，讓執行時間環境 (例如 azure function) 可以從金鑰保存庫取得認證。

另請參閱 Azure 安全性基準測試 [識別碼-2：安全且自動地管理應用程式識別](/azure/security/benchmarks/security-controls-v2-identity-management#id-2-manage-application-identities-securely-and-automatically)。

## <a name="11-architecture-establish-a-single-unified-security-strategy"></a>11. 架構：建立單一整合的安全性策略

*每個人都必須以相同方向的資料列，以使船向前移動。*

**什麼**：確保所有團隊都符合可啟用和保護企業系統和資料的單一策略。

**原因**：當小組在隔離的情況下進行隔離，而不是與共同的策略一致時，他們的個別動作可能會不慎破壞彼此的工作，並建立不必要的衝突，讓每個人的目標進度變慢。

在許多組織中一致地播放的其中一個範例是資產的分割：

- _網路安全性小組_ 開發了一套將一般 _網路_ 分割的策略，以根據實體網站、指派的 IP 位址位址/範圍或類似的) 來提高安全性 (
- 身分 *識別小組* 會個別根據其對組織的瞭解和知識，為群組和 Active Directory 組織單位開發策略， (ou) 。
- *應用程式小組* 通常會發現很難使用這些系統，因為它們是以有限的輸入和瞭解商務營運、目標和風險來設計的。

在發生這種情況的組織中，小組經常會經歷與防火牆例外的衝突，而這會對安全性 (例外狀況造成負面影響，通常是核准的) 和產能 (部署對於商務需求) 的應用程式功能會變慢

雖然安全性可以藉由強制執行重要的思考來建立狀況良好的衝突，但這項衝突只會產生妨礙目標的不良衝突。 如需詳細資訊，請參閱 [安全性策略指引](../strategy/define-security-strategy.md#modernize-your-security-strategy)中的 *適當安全性分歧層級*。

**誰**：

- *贊助* -通常由 CIO、CISO 和 (CTO 共同贊助的整合策略，通常會有一些高階元素的商務領導力支援) ，以及每個小組的代表探討。
- *執行* -每個人都必須實行安全性策略，因此它應該整合各小組的輸入，以提升擁有權、購買和成功的可能性。
  - **[安全性架構](../organize/cloud-security-architecture.md)：** 帶領您致力於建立安全性策略和產生的架構、主動收集小組的意見反應，並將其記錄在各種觀眾的簡報、檔和圖表中。
  - **[原則和標準](../organize/cloud-security-policy-standards.md)：** 會將適當的元素捕獲到標準和原則，然後監視合規性。
  - **所有技術 IT 和安全性小組：** 提供輸入需求，然後配合並實行企業策略。
  - **應用程式擁有者和開發人員：** 閱讀並瞭解適用于這些應用程式的策略檔 (在理想的情況下，針對其角色) 量身打造的指引。

**How** 作法：

打造及實施雲端的安全性策略，其中包含所有小組的輸入和積極參與。 雖然流程檔案格式會有所不同，但這應該一律包含：

- **小組的主動式輸入：** 如果組織中的人員未購買，策略通常會失敗。 在理想的情況下，取得相同房間內的所有小組，共同建立策略。 在我們與客戶合作的研討會中，我們通常會發現組織已在實際的定址接收器中運作，而這些會議通常會讓人們第一次達成對方。 我們也發現包容性是一項需求。 如果某些小組未受邀，此會議通常必須重複，直到所有參與者將其加入 (，否則專案不會向前) 。
- **記載並清楚傳達：** 所有小組都應該知道安全性策略， (最好是整體技術策略的安全性元件) 包括為何要整合安全性、安全性的重要部分，以及安全性成功的外觀。 這應該包含應用程式和開發小組的特定指導方針，讓他們可以取得清楚優先的指導方針，而不需要閱讀本指南的非相關部分。
- **穩定但有彈性：** 策略應維持相對一致且穩定，但架構和檔可能需要變更，以增加清楚，並配合雲端的動態本質。 例如，即使您從使用協力廠商新一代防火牆轉移至 Azure 防火牆，以及調整圖表/如何執行的指引，篩選出惡意的外部流量仍會保持一致。
- **從分割開始：** 在雲端採用的過程中，您的小組將解決許多非常龐大的策略主題，但是您必須從某處開始。 我們建議您以企業資產分割來開始安全性策略，因為這是一項基本決策，在稍後需要進行變更，且需要商務輸入和許多技術團隊。

Microsoft 已發佈在 [這段影片](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151) 中將分割策略套用至 Azure 的指引，以及將 [企業分割](/security/compass/governance#enterprise-segmentation-strategy) 的相關檔，並讓 [網路安全性](/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)一致。

雲端採用架構包含指引來協助您的小組：

- **[建立雲端策略小組](../get-started/team/cloud-strategy.md)：** 在理想的情況下，安全性應該整合至現有的雲端策略。
- **[打造或現代化安全性策略](../strategy/define-security-strategy.md)：** 符合目前雲端服務和新式威脅的商務和安全性目標。

另請參閱 Azure 安全性基準測試 [治理和策略](/azure/security/benchmarks/security-controls-v2-governance-strategy)。
