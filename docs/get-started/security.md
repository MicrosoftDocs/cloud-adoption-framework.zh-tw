---
title: 開始使用：保護企業環境
description: 開始在您的雲端採用工作和作業期間，在關鍵點整合安全性。
author: JanetCThomas
ms.author: janet
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.openlocfilehash: 4e5521c2f3b699584d7785a80e3c92b3db24e347
ms.sourcegitcommit: 899fcd5314ce2748e98c69e27c7f2e318ab27ac5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2020
ms.locfileid: "91206206"
---
# <a name="get-started-implement-security-across-the-enterprise-environment"></a>開始使用：跨企業環境執行安全性

安全性可協助建立商務的機密性、完整性和可用性保證。 安全性工作的重點在於保護內部和外部惡意和無意的行為所造成的作業可能會造成的影響。

本快速入門手冊會概述可減輕或避免網路安全性攻擊的業務風險的主要步驟。 它可協助您在雲端中快速建立基本的安全性作法，並將安全性整合到您的雲端採用流程。

本指南中的步驟適用于所有支援雲端環境和登陸區域安全性保證的角色。 工作包括立即風險降低優先順序、建立新式安全性策略的指引、運用方法，以及在該策略執行。

本指南包含適用于 Azure 的 Microsoft 雲端採用架構中的元素：

![企業安全性入門](../_images/get-started/security-map.png)

遵循本指南中的步驟可協助您在程式中的關鍵點整合安全性。 目標是避免雲端採用的障礙，並減少不必要的業務或營運中斷。

Microsoft 已建立功能和資源，可協助您加速在 Microsoft Azure 上執行這項安全性指引。 您將在本指南中看到這些資源參考。 它們的設計目的是協助您建立、監視及強制執行安全性，而且它們會經常更新和審核。

下圖顯示使用安全性指引和平臺工具，在 Azure 中建立雲端資產的安全性可見度和控制權的整體方法。 我們建議採用此方法。

![安全性圖表](../_images/security/security-diagram.png)

您可以使用這些步驟來規劃和執行策略，以保護您的雲端資產，並使用雲端將安全性作業現代化。

## <a name="step-1-establish-essential-security-practices"></a>步驟1：建立基本的安全性作法

雲端中的安全性一開始會將最重要的安全性作法套用至系統的人員、程式和技術元素。 此外，某些架構決策是基本的，而且稍後很難變更，因此應謹慎套用。 

無論您是在雲端中運作，或是正在規劃未來的採用，我們都建議您遵循這11個基本的安全性作法 (，並符合) 的任何明確法規合規性需求。

**人：**

1. [教育團隊瞭解雲端安全性旅程](../security/security-top-10.md#1-people-educate-teams-about-the-cloud-security-journey)
2. [教育小組雲端安全性技術](../security/security-top-10.md#2-people-educate-teams-on-cloud-security-technology) 

**過程：**

3. [指派雲端安全性決策的責任](../security/security-top-10.md#3-process-assign-accountability-for-cloud-security-decisions)
4. [更新雲端的事件回應 (IR) 進程](../security/security-top-10.md#4-process-update-incident-response-ir-processes-for-cloud)
5. [建立安全性狀態管理](../security/security-top-10.md#5-process-establish-security-posture-management)

**技術：**

6. [需要無密碼或 Multi-Factor Authentication (MFA) ](../security/security-top-10.md#6-technology-require-passwordless-or-multi-factor-authentication-mfa)
7. [整合原生防火牆和網路安全性](../security/security-top-10.md#7-technology-integrate-native-firewall-and-network-security)
8. [整合原生威脅偵測](../security/security-top-10.md#8-technology-integrate-native-threat-detection)

**基礎架構決策：**

9. [標準化單一目錄和身分識別](../security/security-top-10.md#9-architecture-standardize-on-a-single-directory-and-identity)
10. [使用以身分識別為基礎的存取控制 (而非金鑰) ](../security/security-top-10.md#10-architecture-use-identity-based-access-control-instead-of-keys)
11. [建立單一整合的安全性策略](../security/security-top-10.md#11-architecture-establish-a-single-unified-security-strategy)


> [!NOTE]
> 每個組織都應該定義它自己的最低標準。 風險狀態和後續的風險容錯可能會根據產業、文化特性和其他因素而有很大的差異。 例如，銀行可能無法容忍對測試系統進行輕微攻擊的任何可能損害。 某些組織會在將數位轉型加速三到六個月時，接受這項相同的風險。

## <a name="step-2-modernize-the-security-strategy"></a>步驟2：將安全性策略現代化

雲端中的有效安全性需要一個策略，以反映目前的威脅環境，以及裝載企業資產的雲端平臺本質。 明確的策略可改善所有小組的工作，以提供安全且可持續的企業雲端環境。 安全性策略必須啟用定義的業務成果、降低風險可接受的層級，並讓員工有生產力。

雲端安全性策略提供指導方針，讓所有小組都能針對這項採用的技術、流程和人員做好準備。 此策略應該會通知雲端架構和技術功能、引導安全性架構和功能，並影響小組的訓練和教育。

**交付：**

策略步驟應該會產生可輕易傳達給組織內許多專案關係人的檔。 專案關係人可能包含組織領導團隊的主管。

建議您在簡報中捕捉策略，以簡化討論和更新。 檔可支援此簡報，視文化特性和喜好設定而定。

- **策略簡報：** 您可能會有單一策略展示，也可以選擇建立領導物件的摘要版本。

  - **完整簡報：** 這應該會在主要簡報或選擇性參考投影片中包含一組完整的安全性策略元素。
  - **執行摘要：** 與資深主管和董事會成員搭配使用的版本，可能只包含與其角色相關的重要元素，例如風險胃口、最高優先順序或接受的風險。

- 您也可以在 [策略和方案範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx)中記錄動機、結果和業務理由。

**建立安全性策略的最佳作法：**

成功的程式會將這些元素併入其安全性策略流程中：

- **密切配合商務策略：** 安全性的職責是保護商業價值。 將所有安全性工作與該目的保持一致，並將內部衝突降至最低是很重要的。

  - 打造商務、IT 和安全性需求的**共用理解**。
  - **及早將安全性整合到雲端採用** ，以避免從肇因風險中危機的最後一分鐘。
  - **使用敏捷式方法** 來立即建立最小的安全性需求，並持續改善一段時間的安全性保證。
  - 透過刻意主動領導力的行動，**鼓勵安全性文化變革**。

  如需詳細資訊，請參閱 [轉換、心態和期望](../strategy/define-security-strategy.md#transformations-mindsets-and-expectations)。

- **現代化安全性策略：** 安全性策略應該包含新式技術環境、目前威脅範圍和安全性社區資源的所有層面考慮。

  - 適應雲端的共同責任模型。
  - 包含所有雲端類型和多重雲端部署。
  - 偏好原生雲端控制項，以避免不必要和有害的衝突。
  - 整合安全性群組，跟上攻擊者演進的步調。

**其他內容的相關資源：**

- [威脅環境、角色和數位策略的演進](/security/compass/microsoft-security-compass-introduction#evolution-of-threat-environment-roles--digital-strategies-2004)

- [安全性、策略、工具和威脅的轉換](/security/compass/microsoft-security-compass-introduction#transformation-of-security-strategies-tools--threats-1513)

- 雲端採用架構的策略考慮：

  - [將您的安全性策略現代化](../strategy/define-security-strategy.md#modernize-your-security-strategy)
  - [網路安全性復原](../strategy/define-security-strategy.md#cybersecurity-resilience)
  - [雲端如何改變安全性關聯性和責任](../strategy/define-security-strategy.md#how-the-cloud-is-changing-security)

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 安全性領導團隊 (首席資訊安全長 (CISO) 或對等)  | <li> 雲端策略小組 <li> 雲端安全性小組 <li> 雲端採用小組 <li> 卓越或中央 IT 團隊的雲端中心 |

**策略核准：**

針對組織內的營業單位結果或風險責任負責任的主管和業務領導人應核准此策略。 此群組可能包含董事會的板，視組織而定。

## <a name="step-3-develop-a-security-plan"></a>步驟3：開發安全性計畫

規劃藉由定義結果、里程碑、時程表和工作擁有者，讓安全性策略成為動作。 此計畫也概述小組的角色和責任。

安全性規劃和雲端採用規劃不應以隔離方式完成。 請務必及早將雲端安全性小組邀請到規劃週期，以避免因工作中斷或安全性問題而增加的風險延遲。 安全性規劃最適合與數位資產的深入知識和認知，以及完全整合到雲端規劃程式的現有 IT 組合。

**交付：**

- **安全性方案：** 安全性計畫應該是雲端主要規劃檔的一部分。 它可能是使用 [策略和方案範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx)、詳細的投影片組或專案檔的檔。 或者，它可能是這些格式的組合，視組織的大小、文化特性和標準實務而定。

  安全性計畫應包含所有這些元素：

  - **組織**函式方案，讓小組知道目前的安全性角色和責任如何隨著移至雲端而改變。

  - **安全性技能計畫** 在流覽技術、角色和責任的重大變更時，支援小組成員。

  - 引導技術團隊的**技術安全性架構和功能藍圖**。

    Microsoft 提供參考架構和技術功能，可在您建立架構和藍圖時協助您，包括：

    - [Azure 元件和參考模型](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151) ，可加速規劃和設計 azure 安全性角色。

      ![Azure 管理模型](../_images/security/azure-administration-model.png)

      ![Azure RBAC 模型](../_images/security/azure-rbac-model.png)

    - [Microsoft 網路安全性參考架構](https://aka.ms/mcra) ，可針對橫跨內部部署和雲端資源的混合式企業建立網路安全性架構。

    - [安全性作業中心 (SOC) 參考架構](/security/compass/security-operations-videos-and-decks#part-1-introduction---soc-learnings-strategies-and-technical-integration-2430) ，以現代化安全性偵測、回應和復原。

    - [零信任的使用者存取參考架構](/security/ciso-workshop/ciso-workshop-module-3#part-5-zero-trust-user-access-reference-architecture-842) ，以現代化雲端產生的存取控制架構。

    - [Azure 資訊安全中心](/azure/security-center) 與 [Microsoft cloud 應用程式安全性](/cloud-app-security) ，協助保護雲端資產的安全。

  - **安全性意識和教育計畫**，讓所有小組都有基本的重要安全性知識。

  - 使用與業務影響一致的分類法，標示要指定敏感性資產的**資產敏感性**。 分類法是由商務專案關係人、安全性小組和其他感興趣的合作物件共同建立。

  - **雲端方案的安全性變更：** 更新雲端採用方案的其他區段，以反映安全性計畫所觸發的變更。

**安全性規劃的最佳作法：**

如果您的規劃採用下列方法，您的安全性方案可能會更成功：

- **採用混合式環境：** 這包括軟體即服務 (SaaS) 應用程式和內部部署環境。 它還包含多個雲端基礎結構即服務 (IaaS) 和平臺即服務 (PaaS) 提供者（如果適用）。

- **採用 agile 安全性：** 先建立最基本的安全性需求，並將所有非關鍵的專案移至後續步驟的優先順序清單。 這應該不是傳統的詳細計畫3-5 年。 雲端和威脅環境的變更速度太快，讓這種類型的計畫很有用。 您的計畫應著重于開發開始步驟和結束狀態：

  - 立即開始**使用，這**會在長期的計畫開始之前，提供高影響力。 時間範圍可能是3-12 個月，視組織文化、標準實務和其他因素而定。
  - **清楚知道** 所需的結束狀態，以引導每個小組的規劃流程 (可能需要幾年的時間才能達到) 。

- **廣泛共用方案：** 提高專案關係人的認知、意見反應及購買。

- **滿足策略性結果：** 確定您的計畫符合並完成安全性策略中所述的策略性結果。

- **設定擁有權、責任和期限：** 確定每項工作的擁有者都已識別，並且已認可在特定的時間範圍內完成該工作。

- **與安全性方面的安全性聯繫：** 在這段轉換和新的預期期間，與人們互動：

  - **主動支援小組成員轉** 型與清楚的溝通和指導：
    - 他們需要學習哪些技能。
    - 為什麼他們需要學習技能 (以及這樣做) 的優點。
    - 如何 (取得此知識，並提供資源以協助他們瞭解) 。

    您可以使用 [策略和方案範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx)來記錄計畫。 您也可以使用 [線上 Microsoft 安全性訓練](/security/compass/microsoft-security-compass-introduction) ，協助您的團隊成員教育。

  - 保障**安全性認知**，以協助人員真的與保護組織安全的部分。

- **複習 Microsoft 學習和指導方針：** Microsoft 已發佈見解和觀點，可協助您的組織規劃雲端的轉型和新式的安全性策略。 本檔包含記錄的訓練、檔和安全性最佳作法，以及建議的標準。

  如需協助建立方案和架構的技術指引，請參閱 [Microsoft 安全性檔案](/security)。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端安全性小組 | <li> 雲端策略小組 <li> 雲端治理小組 <li> 您組織中的任何風險小組 <li> 卓越或中央 IT 團隊的雲端中心 |

**安全性計畫核准：**

安全性領導團隊 (CISO 或同等的) 應該核准方案。

## <a name="step-4-secure-new-workloads"></a>步驟4：保護新的工作負載

從安全的狀態開始，比稍後在環境中改建安全性更容易。 我們強烈建議從安全的設定開始，以確保工作負載會在安全的環境中遷移至及開發和測試。

[登陸區域](../ready/landing-zone/index.md)執行期間，許多決策可能會影響安全性和風險設定檔。 雲端安全性小組應查看登陸區域設定，以確保其符合您組織安全性基準中的安全性標準和需求。

**交付：**

- 請確定新的登陸區域符合組織的合規性和安全性需求。

**支援交付完成的指導方針：**

- **Blend 現有需求和雲端建議：** 從建議的指導方針開始，然後根據您的獨特安全性需求進行調整。 我們發現嘗試強制執行現有的內部部署原則和標準的挑戰，因為它們通常是指過時的技術或安全性方法。

  Microsoft 已發佈指導方針，可協助您建立安全性基準：
  - [適用于策略和架構的 Azure 安全性標準](/security/compass/compass)：用來塑造環境安全性狀態的策略和架構建議。
  - [Azure 安全性基準](/azure/security/benchmarks/introduction)檢驗：保護 Azure 環境的特定設定建議。
  - [Azure 安全性基準訓練](/learn/modules/create-security-baselines)。

- **提供護欄：** 保護應包含自動化的原則審核和強制執行。 針對這些新環境，團隊應致力於審核和強制執行組織的安全性基準。 這些工作可協助將工作負載開發期間的安全性意外降至最低，以及持續整合和持續部署 (的 CI/CD) 工作負載。

  Microsoft 在 Azure 中提供數個原生功能來啟用此功能：
  - [安全分數](/azure/security-center/secure-score-security-controls)：使用 Azure 安全性狀態的評分評量來追蹤組織中的安全性工作和專案。
  - [Azure 藍圖](/azure/governance/blueprints/overview)：雲端架構設計人員和集中式 IT 群組可以定義一組可重複使用的 Azure 資源，以符合組織的標準、模式和需求。
  - [Azure 原則](/azure/governance/policy)：這是其他服務所使用之可見度和控制項功能的基礎。 Azure 原則已整合到 [Azure Resource Manager](/azure/azure-resource-manager)中，因此您可以在建立之前、期間或之後，在 Azure 中的任何資源上審核變更和強制執行原則。
  - [改進登陸區域作業](../ready/considerations/landing-zone-security.md)：使用最佳做法來提升登陸區域內的安全性。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端安全性小組 | <li> 雲端採用小組 <li> 雲端平臺小組 <li> 雲端策略小組 <li> 雲端治理小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-5-secure-existing-cloud-workloads"></a>步驟5：保護現有的雲端工作負載

許多組織已在不套用安全性最佳作法的情況下，將資產部署到企業雲端環境，從而提高業務風險。

在您確定新的應用程式和登陸區域遵循安全性最佳作法之後，您應該將焦點放在讓現有的環境達到相同標準。

**交付：**

- 確定所有現有的雲端環境和登陸區域都符合組織的合規性和安全性需求。
- 使用安全性基準的原則，測試生產部署的作業就緒程度。
- 驗證遵循安全性基準的設計指引和安全性需求。

**支援交付完成的指導方針：**

- 使用您在 [步驟 4](#step-4-secure-new-workloads) 中建立的相同安全性基準作為理想的狀態。 您可能必須將某些原則設定調整為只進行審核，而不是強制執行。
- 平衡營運和安全性風險。 因為這些環境可能會裝載啟用重要商務程式的生產系統，所以您可能需要以累加方式執行安全性改進，以避免風險的作業停機時間。
- 依商務重要性排列安全性風險探索和補救的優先順序。 如果遭入侵的工作負載和有高風險風險的工作負載，請從具有高業務影響的工作負載開始著手。

如需詳細資訊，請參閱 [識別和分類商務關鍵應用程式](/azure/architecture/framework/security/applications-services#identify-and-classify-business-critical-applications)。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端採用小組 <li> 雲端策略小組 <li> 雲端安全性小組 <li> 雲端治理小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="step-6-govern-to-manage-and-improve-security-posture"></a>步驟6：管理及改善安全性狀態

就像所有新式專業領域一樣，安全性是一種反復進行的程式，應著重于持續改進。 如果組織不會在一段時間內維持焦點，則安全性狀態也會衰減。

一致的安全性需求應用來自于音效治理專業領域和自動化解決方案。 在雲端安全性小組定義安全性基準之後，應進行這些需求的審核，以確保它們一致地套用至所有雲端環境 (，並在適用的) 時強制執行。

**交付：**

- 確定組織的安全性基準已套用至所有相關系統。 使用 [安全分數](/azure/security-center/secure-score-security-controls) 或類似的機制來審核異常。
- 記錄 [安全性基準專業範本](../govern/security-baseline/template.md)中的安全性基準原則、程式和設計指引。

**支援交付完成的指導方針：**

- 使用您在 [步驟 4](#step-4-secure-new-workloads) 中建立的相同安全性基準和審核機制，作為監視基準的技術元件。 使用人員和流程式控制件來補充這些基準，以確保一致性。
- 確定所有工作負載和資源都遵循適當的 [命名和標記慣例](../ready/azure-best-practices/naming-and-tagging.md)。 [使用 Azure 原則來強制執行標記慣例](/azure/governance/policy/tutorials/govern-tags)，特別強調「資料敏感度」標記。
- 如果您不熟悉雲端治理，請使用管理方法來建立 [治理原則、程式和專業領域](../govern/index.md) 。

<br>

| 責任小組 | 負責和支援的團隊 |
| --- | --- |
| <li> 雲端治理小組 | <li> 雲端策略小組 <li> 雲端安全性小組 <li> 卓越或中央 IT 團隊的雲端中心 |

## <a name="next-steps"></a>後續步驟

本指南中的步驟可協助您實行一致管理整個企業安全性風險所需的策略、控制項、流程、技能和文化特性。

當您繼續進入雲端安全性的作業模式時，請考慮下列後續步驟：

- 請參閱 [Microsoft 安全性檔案](/security)。 它提供技術指引，協助安全性專業人員建立及改進網路安全性策略、架構和優先順序的藍圖。
- 查看 [Azure 服務內建安全性控制中的](/azure/security/fundamentals/security-controls)安全性資訊。
- 複習 azure 安全性工具和服務，以瞭解 [azure 上可用的安全性服務和技術](/azure/security/azure-security-services-technologies)。
- 請參閱 [Microsoft 信任中心](https://www.microsoft.com/trustcenter/guidance/risk-assessment)。 其中包含廣泛的指導方針、報告和相關檔，可協助您在法規遵循流程中執行風險評估。
- 複習可用的協力廠商工具，以協助符合您的安全性需求。 如需詳細資訊，請參閱 [Azure 資訊安全中心中的整合安全性解決方案](/azure/security-center/security-center-partner-integration)。
