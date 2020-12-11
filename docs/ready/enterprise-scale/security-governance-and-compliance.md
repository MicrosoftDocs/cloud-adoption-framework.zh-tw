---
title: 企業規模的安全性、治理和合規性
description: 瞭解適用于 Azure 的 Microsoft 雲端採用架構中企業規模的安全性治理和合規性。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: think-tank
ms.openlocfilehash: 4e76a3299db2962b46bb6f68c76b5aa829fef2d4
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97023577"
---
<!-- cSpell:ignore FIPS SIEM majeure NSGs -->
<!-- docutune:casing "FIPS 140-2 Level" "Patch and update management" "SOC2 Trust Service Principles and Criteria" -->

# <a name="enterprise-scale-security-governance-and-compliance"></a>企業規模的安全性、治理和合規性

本文涵蓋定義加密與金鑰管理、規劃治理、定義安全性監視和稽核原則，以及規劃平臺安全性。 在本文結尾，您可以參考描述架構的表格，以評估 Azure 服務的企業安全性就緒程度。

## <a name="define-encryption-and-key-management"></a>定義加密與金鑰管理

加密是在 Microsoft Azure 中確保資料隱私權、合規性及資料落地的重要步驟。 其也是許多企業最重要的安全性考量之一。 本節涵蓋與加密及金鑰管理有關的設計考慮和建議。

### <a name="design-considerations"></a>設計考量

- 適用于 Azure Key Vault 的訂用帳戶和規模限制： Key Vault 具有金鑰和秘密的交易限制。 若要在特定期間內對每個保存庫進行交易節流，請參閱 [Azure 限制](/azure/azure-resource-manager/management/azure-subscription-service-limits)。

- Key Vault 可提供安全性界限，因為金鑰、秘密和憑證的存取權限是在保存庫層級。 Key Vault 存取原則指派會分別授與金鑰、秘密或憑證的許可權。 它們不支援細微的物件層級許可權，例如特定金鑰、秘密或憑證 [金鑰管理](/azure/security/fundamentals/data-encryption-best-practices)。

- 您可以將特定應用程式和工作負載特定的密碼和共用的密碼，以適當的 [控制存取權](/azure/key-vault/general/best-practices)來隔離。

- 您可以優化 Premium Sku，其中需要硬體安全模組保護的金鑰。 基礎硬體安全性模組 (Hsm) 符合 FIPS 140-2 層級2的規範。 考慮支援的案例，以管理 FIPS 140-2 Level 3 合規性的 Azure 專用 HSM。

- 金鑰輪替和密碼到期。

  - 使用 Key Vault 的 [憑證](/azure/key-vault/certificates/about-certificates)來購買憑證和簽署憑證。
  - 警示/通知和自動憑證續約。

- 金鑰、憑證和秘密的嚴重損壞修復需求。

  Key Vault 的服務複寫和容錯移轉功能： [可用性和冗余](/azure/key-vault/general/disaster-recovery-guidance)。

- 監視金鑰、憑證和秘密使用方式。

  使用金鑰保存庫或 Azure 監視器 Log Analytics 工作區偵測未經授權的存取： [監視和警示](/azure/key-vault/general/alert)。

- 委派 Key Vault 具現化和特殊許可權存取： [安全存取](/azure/key-vault/general/secure-your-key-vault)。

- 針對原生加密機制（例如 Azure 儲存體加密）使用客戶管理金鑰的需求：
  - [客戶管理的金鑰](/azure/storage/common/storage-encryption-keys-portal)。
  - 虛擬機器 (Vm) 的完整磁片加密。
  - 傳輸中的資料加密。
  - 待用資料加密。

### <a name="design-recommendations"></a>設計建議

- 使用同盟 Azure Key Vault 模型來避免交易規模限制。

- 布建已啟用虛刪除和清除原則的 Azure Key Vault，以允許已刪除物件的保留保護。

- 藉由限制授權將金鑰、秘密和憑證永久刪除到特殊的自訂 Azure Active Directory (Azure AD) 角色，來遵循最低許可權的模型。

- 使用公開憑證授權單位單位將憑證管理和更新程式自動化，以簡化系統管理。

- 建立金鑰和憑證輪替的自動化進程。

- 啟用保存庫上的防火牆和虛擬網路服務端點，以控制金鑰保存庫的存取權。

- 使用平臺中央 Azure 監視器 Log Analytics 工作區，以在每個 Key Vault 實例內審核金鑰、憑證和秘密的使用方式。

- 委派 Key Vault 具現化和特殊許可權存取，並使用 Azure 原則來強制執行一致的合規性設定。

- 預設為主要加密功能的 Microsoft 管理金鑰，並在必要時使用客戶管理的金鑰。

- 請勿針對應用程式金鑰或秘密使用 Key Vault 的集中式實例。

- 請勿在應用程式之間共用 Key Vault 實例，以避免跨環境共用秘密。

## <a name="plan-for-governance"></a>為控管做規劃

治理提供多項機制和流程，以便維持控制 Azure 中的應用程式與資源。 Azure 原則在企業技術資產中確保安全性與合規性是不可或缺的。 它可以在 Azure 平臺服務中強制執行重要的管理和安全性慣例，並補充角色型存取控制 (RBAC) ，以控制授權使用者可執行檔動作。

**設計考慮：**

- 判斷所需的 Azure 原則。

- 強制執行管理和安全性慣例，例如使用私用端點。

- 使用原則定義來管理和建立原則指派，可以在多個繼承的指派範圍內重複使用。 您可以在管理群組、訂用帳戶和資源群組範圍中，擁有集中式的基準原則指派。

- 確保符合合規性報告和審核的持續性。

- 瞭解 Azure 原則有限制，例如任何特定範圍的定義限制： [原則限制](/azure/azure-resource-manager/management/azure-subscription-service-limits)。

- 瞭解法規合規性原則。 這些可能包括 HIPAA、PCI DSS，以及 SOC2 信任服務準則和準則。

**設計建議：**

- 識別所需的 Azure 標記，並使用附加原則模式來強制使用。

- 將法規和合規性需求對應至 Azure 原則定義和 Azure AD RBAC 指派。

- 在最上層根管理群組上建立 Azure 原則定義，以便在繼承的範圍指派這些定義。

- 如有必要，請在最高層級以最上層的排除專案管理原則指派。

- 使用 Azure 原則來控制訂用帳戶和/或管理群組層級的資源提供者註冊。

- 使用內建原則，盡可能將作業的額外負荷降至最低。

- 在特定範圍指派內建原則參與者角色，以啟用應用層級的治理。

- 限制在根管理群組範圍中進行的 Azure 原則指派數目，以避免在繼承的範圍內透過排除專案進行管理。

## <a name="define-security-monitoring-and-an-audit-policy"></a>定義安全性監視和稽核原則

企業必須可以看到其技術雲端資產中的情況。 Azure 平臺服務的安全性監視和審核記錄是可擴充架構的重要元件。

**設計考慮：**

- Audit data 的資料保留期限。 Azure AD Premium 報表有30天的保留期限。

- 記錄的長期封存，例如 Azure 活動記錄、VM 記錄和平臺即服務 (PaaS) 記錄檔。

- 透過 Azure in guest VM 原則的基準安全性設定。

- 重大弱點的緊急修補。

- 修補離線一段時間的 Vm。

- 即時監視和警示的需求。

- 安全性資訊與事件管理與 Azure 資訊安全中心和 Azure Sentinel 的整合。

- Vm 的弱點評定。

**設計建議：**

- 使用 Azure AD 報告功能來產生存取控制審核報告。

- 將 Azure 活動記錄匯出至適用于長期資料保留的 Azure 監視器記錄。 如有需要，請匯出至 Azure 儲存體的長期儲存期限超過兩年。

- 針對所有訂用帳戶啟用安全中心標準，並使用 Azure 原則以確保合規性。

- 透過 Azure 監視器記錄和 Azure 資訊安全中心，監視基礎作業系統修補漂移。

- 使用 Azure 原則透過 VM 擴充功能自動部署軟體設定，並強制執行符合規範的基準 VM 設定。

- 透過 Azure 原則監視 VM 安全性設定漂移。

- 將預設資源設定連接到集中式 Azure 監視器 Log Analytics 工作區。

- 使用以 Azure 事件方格為基礎的解決方案來進行記錄導向的即時警示。

## <a name="plan-for-platform-security"></a>規劃平臺安全性

當您採用 Azure 時，您必須維持狀況良好的安全性狀況。 除了可見度之外，您還必須能夠控制 Azure 服務發展時的初始設定和變更。 因此，規劃平臺安全性是關鍵的。

**設計考慮：**

- 共同責任。

- 高可用性和嚴重損壞修復。

- 在資料管理和控制平面作業方面，都能以一致的方式在 Azure 服務之間保持安全。

- 主要平臺元件的多租使用者。 這包括 Hyper-v、Hsm 底層 Key Vault 和資料庫引擎。

**設計建議：**

- 在您的基礎需求內容中，進行每個必要服務的聯合檢查。 如果您想要攜帶您自己的金鑰，則可能不會在所有已被視為的服務上受到支援。 實行相關的緩和措施，讓不一致的結果不會妨礙預期的結果。 選擇適當的區域配對和嚴重損壞修復區域，以將延遲降至最低。

- 開發安全性允許清單計畫來評估服務安全性設定、監視、警示，以及如何將這些服務與現有的系統整合。

- 在允許 Azure 服務進入生產環境之前，請先判斷其事件回應計畫。

- 使用 Azure AD 報告功能來產生存取控制審核報告。

- 將您的安全性需求與 Azure 平臺藍圖保持一致，以保持最新發行的安全性控制。

- 在適當的情況下，實以零信任方式存取 Azure 平臺。

<!-- docutune:ignore "and conditional access" -->

## <a name="azure-security-benchmark"></a>Azure 安全性效能評定

Azure 安全性基準測試包含一組高度影響的安全性建議，可用來協助保護您在 Azure 中使用的大部分服務。 您可以將這些建議視為「一般」或「組織」，因為它們適用于大部分的 Azure 服務。 接著會針對每個 Azure 服務自訂 Azure 安全性效能評定建議，而此自訂指導方針包含在服務建議文章中。

Azure 安全性基準測試檔會指定安全性控制和服務建議。

- [安全性控制](/azure/security/benchmarks/overview)： Azure 安全性效能評定建議會依安全性控制進行分類。 安全性控制代表高階廠商中立的安全性需求，例如網路安全性和資料保護。 每個安全性控制都有一組安全性建議和指示，可協助您執行這些建議。
- [服務建議](/azure/security/benchmarks/security-baselines-overview)：如果有的話，適用于 azure 服務的基準測試建議將包含專為該服務量身打造的 Azure 安全性基準測試建議。

## <a name="service-enablement-framework"></a>服務啟用架構

當業務單位要求將工作負載部署至 Azure 時，您需要額外的工作負載可見度，以決定如何達到適當層級的治理、安全性和合規性。 需要新的服務時，您必須允許。 下表提供的架構可讓您評估 Azure 服務的企業安全性就緒程度：

| 評量                    | 類別                                                              | 準則                                                                                                                                     |
|------------------------------|-----------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| 安全性                     | 網路端點                                                      | 服務是否有可從虛擬網路外部存取的公用端點？                                                                |
|                              |                                                                       | 它是否支援虛擬網路服務端點？                                                                                                      |
|                              |                                                                       | Azure 服務可以直接與服務端點互動嗎？                                                                              |
|                              |                                                                       | 它是否支援 Azure Private Link 端點？                                                                                                           |
|                              |                                                                       | 可以在虛擬網路內部署嗎？                                                                                                            |
|                              | 預防資料外洩                                          | PaaS 服務在 Azure ExpressRoute Microsoft 對等互連中是否有個別的邊界閘道通訊協定群體？ ExpressRoute 是否會公開服務的路由篩選？ |
|                              |                                                                       | 服務是否支援端點 Private Link？                                                                                                       |
|                              | 針對管理和資料平面作業強制執行網路流量流程 | 是否可以檢查輸入/離開服務的流量？ 是否可以使用使用者定義的路由來強制 tunnelled 流量？                                    |
|                              |                                                                       | 管理作業會使用 Azure 共用的公用 IP 範圍嗎？                                                                                 |
|                              |                                                                       | 管理流量是透過主機上公開的連結本機端點來導向？                                                                |
|                              | 待用資料加密                                               | 預設會套用加密嗎？                                                                                                            |
|                              |                                                                       | 可以停用加密嗎？                                                                                                                  |
|                              |                                                                       | 使用 Microsoft 管理的金鑰或客戶管理的金鑰來執行加密嗎？                                                   |
|                              | 傳輸中資料加密                                            | 以通訊協定層級加密的服務流量 (安全通訊端層/傳輸層安全性) ？                                                                           |
|                              |                                                                       | 是否有任何 HTTP 端點，是否可以停用？                                                                                        |
|                              |                                                                       | 基礎服務通訊也會加密嗎？                                                                                          |
|                              |                                                                       | 使用 Microsoft 管理的金鑰或客戶管理的金鑰來執行加密嗎？  (支援自備加密嗎？ )                                                                                |
|                              | 軟體部署                                                   | 應用程式軟體或協力廠商產品是否可部署至服務？                                                                 |
|                              |                                                                       | 軟體部署的執行與管理方式為何？                                                                                            |
|                              |                                                                       | 是否可以強制執行原則來控制來源或程式碼完整性？                                                                                   |
|                              |                                                                       | 如果軟體是可部署的，則可使用反惡意程式碼功能、弱點管理和安全性監視工具嗎？                                  |
|                              |                                                                       | 服務是否會以原生方式提供這類功能，例如 Azure Kubernetes Service？                                                                              |
| 身分識別和存取管理 | 驗證和存取控制                                       | 受 Azure AD 管理的所有控制平面作業？ 是否有嵌套的控制項平面，例如 Azure Kubernetes Service？                             |
|                              |                                                                       | 有哪些方法可提供資料平面的存取權？                                                                                      |
|                              |                                                                       | 資料平面是否與 Azure AD 整合？                                                                                                      |
|                              |                                                                       | Azure 對 Azure (服務對服務) 驗證是否使用 MSI/服務主體？                                                         |
|                              |                                                                       | Azure 對 IaaS (透過 Azure AD 的服務對虛擬網路) 驗證嗎？                                                                                   |
|                              |                                                                       | 如何管理任何適用的金鑰或共用存取簽章？                                                                                                     |
|                              |                                                                       | 如何撤銷存取權？                                                                                                                   |
|                              | 責任隔離                                                 | 服務是否會在 Azure AD 內分開控制平面和資料平面作業？                                                                |
|                              | 多重要素驗證和條件式存取                                            | 是否對使用者強制執行多重要素驗證以進行服務互動？                                                                                            |
| 控管                   | 資料匯出和匯入                                                  | 服務是否可讓您安全地匯入和匯出資料？                                                                     |
|                              | 資料隱私權和使用方式                                                  | Microsoft 工程師可以存取資料嗎？                                                                                                     |
|                              |                                                                       | 是否有任何 Microsoft 支援服務與服務的互動進行審核？                                                                               |
|                              | 資料存留處                                                        | 資料是否包含在服務部署區域中？                                                                                          |
| 作業                   | 監視                                                            | 服務是否與 Azure 監視器整合？                                                                                               |
|                              | 備份管理                                                     | 需要備份哪些工作負載資料？                                                                                                       |
|                              |                                                                       | 如何捕獲備份？                                                                                                                    |
|                              |                                                                       | 備份的執行頻率為何？                                                                                                         |
|                              |                                                                       | 備份可以保留多久？                                                                                                        |
|                              |                                                                       | 備份經過加密？                                                                                                                       |
|                              |                                                                       | 使用 Microsoft 管理的金鑰或客戶管理的金鑰來執行備份加密嗎？                                                                                             |
|                              | 災害復原                                                     | 如何使用區域多餘的方式來使用服務？                                                                                 |
|                              |                                                                       | 可達到的復原時間目標和復原點目標為何？                                                                                                          |
|                              | SKU                                                                   | 有哪些可用的 Sku？ 它們有何不同？                                                                                             |
|                              |                                                                       | 是否有任何與 Premium SKU 安全性相關的功能？                                                                                  |
|                              | 容量管理                                                   | 如何監視容量？                                                                                                                   |
|                              |                                                                       | 水準調整的單位為何？                                                                                                        |
|                              | 修補和更新管理                                             | 服務需要進行中的更新，否則會自動進行更新嗎？                                                                        |
|                              |                                                                       | 套用更新的頻率為何？ 它們可以自動進行嗎？                                                                                |
|                              | 稽核                                                                 | 是否已捕獲 (的嵌套控制項平面作業，例如 Azure Kubernetes Service 或 Azure Databricks) ？                                                                      |
|                              |                                                                       | 是否記錄重要資料平面活動？                                                                                                      |
|                              | 設定管理                                              | 它是否支援標記並提供 `put` 所有資源的架構？                                                                             |
| Azure 服務合規性     | 服務證明、認證和外部審核                | 服務 PCI/ISO/SOC 是否相容？                                                                                                        |
|                              | 服務可用性                                                  | 服務是私人預覽、公開預覽還是正式推出？                                                                                            |
|                              |                                                                       | 服務有哪些區域可用？                                                                                                    |
|                              |                                                                       | 服務的部署範圍為何？ 它是區域或全域服務嗎？                                                      |
|                              | 服務等級協定 (Sla)                                               | 服務可用性的 SLA 為何？                                                                                                    |
|                              |                                                                       | 如果適用的話，效能的 SLA 為何？                                                                                              |
