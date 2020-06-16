---
title: 開始使用：保護企業環境
description: 開始在您的雲端採用工作和營運期間，整合關鍵要點的安全性。
author: JanetCThomas
ms.author: janet
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.openlocfilehash: 48d4642c7374af5da2684c9fc4f3bd5edfbbc6ab
ms.sourcegitcommit: d88c1cc3597a83ab075606d040ad659ac4b33324
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/15/2020
ms.locfileid: "84786002"
---
<!-- cSpell:ignore CISO passwordless -->

# <a name="get-started-implement-security-across-the-enterprise-environment"></a>開始使用：在企業環境中執行安全性

安全性有助於為企業的機密性、完整性和可用性創造保證。 安全性工作的重點在於保護內部和外部惡意和非預期的行為所造成的作業可能影響。

本入門指南概述將緩和或避免商業風險網路安全性攻擊的主要步驟。 它可協助您在雲端中快速建立基本的安全性作法，並將安全性整合到您的雲端採用程式。

本指南中的步驟適用于所有支援雲端環境和登陸區域安全性保證的角色。 工作包括立即風險緩和優先順序、建立現代化安全性策略的指引、運用方法，以及在該策略上執行。

本指南包含適用于 Azure 的 Microsoft Cloud 採用架構中的元素：

![企業安全性入門](../_images/get-started/security-map.png)

遵循本指南中的步驟，可協助您在程式中的關鍵點整合安全性。 目標是避免雲端採用的障礙，並減少不必要的業務或操作中斷。

Microsoft 已建立功能和資源，可協助您在 Microsoft Azure 上加速執行此安全性指引。 您會在本指南中看到這些資源的參考。 其設計目的是協助您建立、監視和強制執行安全性，而且它們經常更新和審核。

下圖顯示在 Azure 中使用安全性指引和平臺工具來建立安全性可見度和控制雲端資產的整體方法。 我們建議採用這種方法。

![安全性圖表](../_images/security/security-diagram.png)

使用下列步驟來規劃及執行您的策略，以保護您的雲端資產，並使用雲端將安全性作業現代化。

## <a name="step-1-establish-essential-security-practices"></a>步驟1：建立基本的安全性作法

雲端中的安全性從音效實務開始。 無論您是否已在雲端操作，或打算未來採用，都必須快速建立基本的安全性作法。

除了符合任何明確的法規合規性需求之外，我們建議下列步驟，以解決大部分組織移至雲端時所面臨的最大安全性挑戰。

**交付專案和支援指引：**

- **技術：** 藉由啟用系統管理員的無密碼或多重要素驗證，以及啟用雲端資源的威脅防護，來降低主要風險並提高資產的可見度和控制權。
  - [適用于系統管理員的無密碼或多重要素驗證](https://docs.microsoft.com/azure/architecture/framework/security/critical-impact-accounts#passwordless-or-multi-factor-authentication-for-admins)
  - [Azure 資訊安全中心中的](https://docs.microsoft.com/azure/security-center/threat-protection)[安全性作業](https://docs.microsoft.com/azure/architecture/framework/security/security-operations)和威脅防護
- **進程：** 藉由指派安全性角色和責任，以及建立事件回應程式，來啟用快速的安全性決策和持續改進。
  - [清除的責任](https://docs.microsoft.com/azure/architecture/framework/security/governance#clear-lines-of-responsibility)、[指派管理環境的許可權](https://docs.microsoft.com/azure/architecture/framework/security/governance#assign-privileges-for-managing-the-environment)，以及讓安全分數 <!-- TODO: Improve this and add link to AAF article -->
  - 安全性角色和責任 <!-- TODO: add link to bookmark -->
  - [事件回應參考指南](https://aka.ms/irrg)
- **人員：** 為安全性小組提供在轉換至雲端環境時成功部署和操作所需的教育、工具和存取權。
  - **教育所有人**瞭解雲端和雲端安全性如何演變的概念：
    - [威脅環境、角色和數位策略的演進](https://docs.microsoft.com/security/compass/microsoft-security-compass-introduction#evolution-of-threat-environment-roles--digital-strategies-2004)
    - [安全性、策略、工具和威脅的轉換](https://docs.microsoft.com/security/compass/microsoft-security-compass-introduction#transformation-of-security-strategies-tools--threats-1513)
  - 針對他們所使用的平臺，**訓練技術人員**雲端安全性功能的技術詳細資料。 Microsoft 提供廣泛的[Azure 安全性檔案](https://docs.microsoft.com/azure/security)。
- **長期架構決策：** 以適當的決策建立長期基礎。 這在稍後變更時很難且昂貴。
  - [建立企業分割策略，並將技術架構與 it 協調（網路分割、身分識別分割等等）](https://docs.microsoft.com/azure/architecture/framework/security/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)
  - [單一企業目錄](https://docs.microsoft.com/azure/architecture/framework/security/identity#single-enterprise-directory)
  - [服務的驗證策略](https://docs.microsoft.com/azure/architecture/framework/security/applications-services#prefer-identity-authentication-over-keys)
  - [許可權指派策略](https://docs.microsoft.com/azure/architecture/framework/security/critical-impact-accounts#avoid-granular-and-custom-permissions)

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端安全性小組 <br><br><br> | <li> 雲端策略小組 <li> 雲端採用小組 <li> 卓越或中央 IT 的雲端中心 |

在此初始步驟中，治理小組也應開始協調可跨環境監視、管理及強制執行的安全性基準建立。 如需建立此功能的其他指引，請見稍後的步驟4。

> [!NOTE]
> 每個組織都應該定義它自己的最低標準。 風險狀態和後續對該風險的承受度可能會根據產業、文化和其他因素而有很大的差異。 例如，銀行可能無法容忍其對測試系統的輕微攻擊，其信譽的任何潛在損害。 有些組織會在將數位轉型加速到六個月後，接受這項相同的風險。

## <a name="step-2-modernize-the-security-strategy"></a>步驟2：將安全性策略現代化

雲端中的有效安全性需要一個策略來反映目前的威脅環境，以及裝載企業資產的雲端平臺本質。 明確的策略可改善所有小組的工作，以提供安全且可持續的企業雲端環境。 安全性策略必須啟用已定義的商務結果、降低風險，使其成為可接受的層級，並讓員工具有生產力。

雲端安全性策略提供所有團隊的指導方針，可讓您處理這項採用的技術、程式和人員。 策略應該會通知雲端架構和技術功能、引導安全性架構和功能，以及影響小組的訓練和教育。

**項**

策略步驟應該會產生一份檔，輕鬆地傳達給組織內的許多專案關係人。 專案關係人可能會包含組織領導小組的主管。

我們建議您在簡報中捕捉策略，以方便進行討論和更新。 視文化特性和喜好設定而定，檔可支援此簡報。

- **策略簡報：** 您可能會有單一策略簡報，或者您也可以選擇為領導物件建立摘要版本。
  - **完整簡報：** 這應該包含主要簡報或選擇性參考投影片中安全性原則的一組完整元素。
  - **執行摘要：** 與資深主管和麵板成員搭配使用的版本，可能只包含與其角色相關的重要元素，例如風險胃口、最高優先順序或已接受的風險。
- 您也可以在[策略和計畫範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)中記錄動機、結果和業務理由。

**建立安全性策略的最佳作法：**

成功的程式會將這些元素併入其安全性策略流程中：

- **密切配合商務策略：** 安全性的職責是保護商業價值。 請務必讓所有安全性工作符合該目的，並將內部衝突降到最低。
  - **建立對**企業、IT 和安全性需求的共同瞭解。
  - **及早將安全性整合到雲端採用**，以避免最後一分鐘的危機肇因風險。
  - **使用靈活的方法**立即建立最低安全性需求，並持續改善一段時間的安全性保證。
  - 透過刻意的主動式領導行動，**鼓勵安全性文化**特性的改變。

  如需詳細資訊，請參閱[轉換、心態和預期](../strategy/define-security-strategy.md#transformations-mindsets-and-expectations)。

- **現代化安全性策略：** 安全性策略應包括現代化技術環境、目前威脅範圍和安全性團體資源的所有層面考慮。
  - **適應雲端的共同責任模型**。
  - **包含所有雲端類型和多重雲端部署**。
  - **偏好原生雲端控制項**，以避免不必要和有害的摩擦。
  - **整合安全性小組**來跟上攻擊者演進的步調。

**其他內容的相關資源：**

- [威脅環境、角色和數位策略的演進](https://docs.microsoft.com/security/compass/microsoft-security-compass-introduction#evolution-of-threat-environment-roles--digital-strategies-2004)
- [安全性、策略、工具和威脅的轉換](https://docs.microsoft.com/security/compass/microsoft-security-compass-introduction#transformation-of-security-strategies-tools--threats-1513)
- 雲端採用架構的策略考慮：
  - [現代化您的安全性策略](../strategy/define-security-strategy.md#modernize-your-security-strategy)
  - [網路安全性復原](../strategy/define-security-strategy.md#cybersecurity-resilience)
  - [雲端如何改變安全性關係和責任](../strategy/define-security-strategy.md#how-the-cloud-is-changing-security)

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 安全性領導小組（資訊安全長（CISO）或對等專案） | <li> 雲端策略小組 <li> 雲端安全性小組 <li> 雲端採用小組 <li> 卓越或中央 IT 的雲端中心 |

**策略核准：**

在組織內的結果或商務線風險責任的主管和商務領導人，應核准此策略。 此群組可能包含董事會的面板，視組織而定。

## <a name="step-3-develop-a-security-plan"></a>步驟3：開發安全性計畫

規劃會藉由定義結果、里程碑、時程表和工作擁有者，將安全性策略放在動作中。 此方案也會概述小組的角色和責任。

安全性規劃和雲端採用規劃不應在隔離的情況中完成。 請務必及早邀請雲端安全性小組進入規劃週期，以避免工作停止或增加所發現之安全性問題的風險。 安全性規劃最適合用來深入瞭解並感知數位資產和現有的 IT 組合，這是完全整合到雲端規劃程式。

**項**

- **安全性計畫：** 安全性計畫應該是雲端主要規劃檔的一部分。 這可能是使用[策略和計畫範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)、詳細的投影片或專案檔的檔。 或者，它可能是這些格式的組合，視組織的大小、文化特性和標準實務而定。

  安全性計畫應包含所有這些元素：

  - **組織**函式方案，讓小組知道目前的安全性角色和責任如何隨著移至雲端而改變。
  - **安全性技能計畫**，在流覽技術、角色和責任方面的重大變更時，支援小組成員。
  - **技術安全性架構和功能藍圖**，以引導技術團隊。
  Microsoft 提供參考架構和技術功能，可在您建立架構和藍圖時協助您，包括：
    - [Azure 元件和參考模型](https://docs.microsoft.com/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)，可加速規劃和設計 azure 安全性角色。

      ![Azure 系統管理模型](../_images/security/azure-administration-model.png)

      ![Azure RBAC 模型](../_images/security/azure-rbac-model.png)
    - [Microsoft 網路安全性參考架構](https://aka.ms/mcra)，為橫跨內部部署和雲端資源的混合式企業建立網路安全性架構。
    - [安全性作業中心（SOC）參考架構](https://docs.microsoft.com/security/compass/security-operations-videos-and-decks#part-1-introduction---soc-learnings-strategies-and-technical-integration-2430)，以現代化安全性偵測、回應和復原。
    - [零信任的使用者存取參考架構](https://docs.microsoft.com/security/ciso-workshop/ciso-workshop-module-3#part-5-zero-trust-user-access-reference-architecture-842)，以現代化雲端產生的存取控制架構。
    - [Azure 資訊安全中心](https://docs.microsoft.com/azure/security-center)和[Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security)協助保護雲端資產。
  - **安全性意識與教育計畫**，讓所有小組都有基本的重要安全性知識。
  - 區分**資產的區分**，使用與業務影響一致的分類法來指定敏感性資產。 此分類是由商務專案關係人、安全性小組和其他感興趣的合作物件共同建立的。

- **雲端方案的安全性變更：** 更新雲端採用方案的其他區段，以反映安全性計畫所觸發的變更。

**安全性規劃的最佳作法：** 如果您的規劃採用下列方法，您的安全性計畫可能會更成功：

- **假設混合式環境：** 其中包括軟體即服務（SaaS）應用程式和內部部署環境。 它也包含多個雲端基礎結構即服務（IaaS）和平臺即服務（PaaS）提供者（如果適用的話）。
- **採用 agile 安全性：** 先建立最低安全性需求，並將所有非關鍵專案移至後續步驟的優先順序清單。
這不應該是傳統的詳細規劃3-5 年。 雲端和威脅環境的變更速度太快，使該類型的計畫很有用。 您的計畫應著重于開發開始步驟和結束狀態：
  - **很快**就會開始進行，在長期的計畫開始之前，會先提供高影響力。 時間範圍可以是3-12 個月，視組織文化特性、標準實務和其他因素而定。
  - **清除**所需結束狀態的願景，以引導每個小組的規劃程式（這可能需要數年才能達成）。
- **廣泛共用方案：** 提升專案關係人的認知、意見反應，以及進行購買。
- **符合策略性成果：** 請確定您的計畫符合並完成安全性策略中所述的策略結果。
- **設定擁有權、責任和期限：** 請確定已識別每個工作的擁有者，並已認可在特定時間範圍內完成該工作。
- **連接安全性的人：** 在這段轉換和新的預期期間，與人員接洽：
  - **主動支援小組成員轉換**與清楚溝通和指導：
    - 他們需要學習哪些技能。
    - 為何需要學習技能（以及這麼做的優點）。
    - 如何取得這種知識（並提供協助他們瞭解的資源）。
  
    您可以使用[策略和計畫範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx)來記錄計畫。 而且您可以使用[線上 Microsoft 安全性訓練](https://docs.microsoft.com/security/compass/microsoft-security-compass-introduction)來協助您的小組成員教育。
  - **讓安全性意識成為**協助人員真的與組織保持安全的一員互動。
- **查看 Microsoft 學習和指引：** Microsoft 已發佈見解和觀點，協助您的組織規劃其對雲端的轉換，以及現代化的安全性策略。 內容包含記錄的訓練、檔和安全性最佳做法，以及建議的標準。
  如需協助建立方案和架構的技術指導方針，請參閱[Microsoft 安全性檔案](https://docs.microsoft.com/security)。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端安全性小組 | <li> 雲端策略小組 <li> 雲端治理小組 <li> 組織中的任何風險小組 <li> 卓越或中央 IT 的雲端中心 |

**安全性計畫核准：**

安全性領導小組（CISO 或對等）應核准方案。

## <a name="step-4-secure-new-workloads"></a>步驟4：保護新的工作負載

以安全的狀態啟動，比稍後在您的環境中)) 安全性來得容易許多。 我們強烈建議您從安全的設定開始，以確保工作負載會遷移至安全的環境，並在其中進行開發和測試。

在[登陸區域](https://docs.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone)執行期間，許多決策可能會影響安全性和風險設定檔。 雲端安全性小組應檢查登陸區域設定，以確保它符合組織安全性基準中的安全性標準和需求。

**項**

- 請確定新的登陸區域符合組織的合規性和安全性需求。

**支援交付後完成的指引：**

- **Blend 現有需求和雲端建議：** 從建議的指引開始，然後根據您的獨特安全性需求進行調整。 我們發現嘗試強制執行現有內部部署原則和標準的挑戰，因為這通常是指過時的技術或安全性方法。

  Microsoft 已發佈指引以協助您建立安全性基準：
  - [適用于策略和架構的 Azure 安全性標準](https://docs.microsoft.com/security/compass/compass)：用來塑造環境安全性狀態的策略和架構建議。
  - [Azure 安全性基準](https://docs.microsoft.com/azure/security/benchmarks/introduction)檢驗：保護 Azure 環境的特定設定建議。
  - [Azure 安全性基準訓練](https://docs.microsoft.com/learn/modules/create-security-baselines)。
- **提供護欄：** 保護應包括自動化原則的審核和強制執行。 針對這些新環境，小組應該致力於審查和強制執行組織的安全性基準。 這些工作可協助將工作負載的開發期間的安全性意外降到最低，以及持續整合和持續部署（CI/CD）工作負載。

  Microsoft 在 Azure 中提供了數個原生功能來啟用：
  - [安全分數](https://docs.microsoft.com/azure/security-center/secure-score-security-controls)：使用 Azure 安全性狀態的評分評量，來追蹤組織中的安全性工作和專案。
  - [Azure 藍圖](https://docs.microsoft.com/azure/governance/blueprints/overview)：雲端架構設計人員和中央 IT 小組可以定義一組可重複使用的 Azure 資源，以執行並遵循組織的標準、模式和需求。
  - [Azure 原則](https://docs.microsoft.com/azure/governance/policy)：這是其他服務所使用之可見度和控制功能的基礎。 Azure 原則已整合到[Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager)中，因此您可以在 Azure 中的任何資源之前、期間或建立之後，對其進行各種變更並強制執行原則。
- [改善登陸區域作業](../ready/considerations/landing-zone-security.md)：使用最佳作法來改善登陸區域內的安全性。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端安全性小組 | <li> 雲端採用小組 <li> 雲端平臺小組 <li> 雲端策略小組 <li> 雲端治理小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-5-secure-existing-cloud-workloads"></a>步驟5：保護現有的雲端工作負載

許多組織都已將資產部署到企業雲端環境，而不需要套用安全性最佳作法，因而創造出更高的業務風險。

在您確定新的應用程式和登陸區域遵循安全性最佳做法之後，您應該專注于將現有的環境帶入相同的標準。

**項**

- 確保所有現有的雲端環境和登陸區域都符合組織的合規性和安全性需求。
- 使用安全性基準的原則，測試生產部署的作業準備就緒。
- 驗證是否遵循安全性基準的設計指引和安全性需求。

**支援交付後完成的指引：**

- 使用您在[步驟 4](#step-4-secure-new-workloads)中建立的相同安全性基準作為理想的狀態。 您可能必須將某些原則設定調整為只進行審核，而不是強制執行它們。
- 平衡營運和安全性風險。 因為這些環境可能裝載可啟用重要商務程式的生產系統，所以您可能需要以累加方式執行安全性改進，以避免風險的作業停機時間。
- 依據業務重要性來設定探索和補救安全性風險的優先順序。 開始使用具有高商業影響的工作負載（如果遭到入侵，且工作負載的風險很高）。

如需詳細資訊，請參閱[識別和分類商務關鍵性應用程式](https://docs.microsoft.com/azure/architecture/framework/security/applications-services?toc=/security/compass/toc.json&bc=/security/compass/breadcrumb/toc.json#identify-and-classify-business-critical-applications)。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端採用小組 | <li> 雲端採用小組 <li> 雲端策略小組 <li> 雲端安全性小組 <li> 雲端治理小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="step-6-govern-to-manage-and-improve-security-posture"></a>步驟6：管理和改善安全性狀態

就像所有現代化的專業領域一樣，安全性是一種反復的程式，應該著重在持續改進的過程中。 如果組織不會在一段時間內承受焦點，則安全性狀態也可能會衰減。

一致的安全性需求應用來自于音效治理專業領域和自動化解決方案。 在雲端安全性小組定義安全性基準之後，應該進行這些需求的審核，以確保它們會一致地套用至所有雲端環境（並在適用時強制執行）。

**項**

- 確定組織的安全性基準已套用至所有相關的系統。 使用[安全分數](https://docs.microsoft.com/azure/security-center/secure-score-security-controls)或類似的機制來審核異常狀況。
- 記錄安全性[基準專業領域範本](../govern/security-baseline/template.md)中的安全性基準原則、程式和設計指引。

**支援交付後完成的指引：**

- 使用您在[步驟 4](#step-4-secure-new-workloads)中建立的相同安全性基準和審核機制，做為監視基準的技術元件。 以人員和流程式控制制來補充這些基準，以確保一致性。
- 確保所有的工作負載和資源都遵循適當的[命名和標記慣例](../ready/azure-best-practices/naming-and-tagging.md)。 [使用 Azure 原則來強制執行標記慣例](https://docs.microsoft.com/azure/governance/policy/tutorials/govern-tags)，特別強調「資料敏感性」標記。
- 如果您不熟悉雲端治理，請使用管控方法來建立[治理原則、處理常式和專業領域](../govern/index.md)。

<!-- markdownlint-disable MD033 -->
<br>

| 責任小組 | 負責與支援小組 |
| --- | --- |
| <li> 雲端治理小組 | <li> 雲端策略小組 <li> 雲端安全性小組 <li> 卓越或中央 IT 的雲端中心 |

## <a name="next-steps"></a>後續步驟

本指南中的步驟可協助您實行一致管理整個企業安全性風險所需的策略、控制項、程式、技能和文化特性。
當您繼續進入雲端安全性的作業模式時，請考慮下列後續步驟：

- 請參閱[Microsoft 安全性檔案](https://docs.microsoft.com/security)。 它提供技術指導方針，可協助安全性專業人員建立及改善網路安全性策略、架構和排定優先順序的藍圖。
- 請參閱[Azure 服務的內建安全性控制](https://docs.microsoft.com/azure/security/fundamentals/security-controls)中的安全性資訊。
- 查看 azure[上可用的安全性服務和技術](https://docs.microsoft.com/azure/security/azure-security-services-technologies)中的 azure 安全性工具和服務。
- 請參閱[Microsoft 信任中心](https://www.microsoft.com/trustcenter/guidance/risk-assessment)。 其中包含廣泛的指引、報告和相關檔，可協助您在法規遵循流程中執行風險評估。
- 回顧可用的協力廠商工具，以協助滿足您的安全性需求。 請參閱[整合 Azure 資訊安全中心中的安全性解決方案](https://docs.microsoft.com/azure/security-center/security-center-partner-integration)。
