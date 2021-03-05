---
title: 保護和管理遷移至 Azure 之工作負載的最佳作法
description: 使用適用于 Azure 的雲端採用架構，瞭解操作、管理及保護已遷移工作負載的最佳做法。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: internal
ms.openlocfilehash: a4a05791b220fea280a55b9183011fee57ff3df5
ms.sourcegitcommit: c167c45b66cc7324b60c88b8b7aac439f956b65d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2021
ms.locfileid: "102209147"
---
<!-- docutune:casing "Update Management" -->
<!-- cSpell:ignore FIPS SIEM majeure NSGs -->

# <a name="best-practices-to-secure-and-manage-workloads-migrated-to-azure"></a>保護和管理遷移至 Azure 之工作負載的最佳作法

在您針對移轉進行規劃及設計的同時，除了思考有關移轉本身的事項之外，您也需要考慮移轉後在 Azure 中的安全性和管理模型。 本文說明在遷移後保護 Azure 部署的規劃和最佳作法。 它也涵蓋進行中的工作，讓您的部署以最佳層級執行。

> [!IMPORTANT]
> 本文所述的最佳做法和意見是以本文撰寫當下可用的 Azure 平台和服務功能作為基礎。 特色與功能會隨著時間改變。

## <a name="secure-migrated-workloads"></a>保護已移轉的工作負載

在移轉之後，最重要的工作便是保護已移轉的工作負載，使其不受來自內部和外部的威脅。 這些最佳做法可以協助您執行此作業：

- 了解如何運用由 Azure 資訊安全中心所提供的監視、評量及建議。
- 取得對您在 Azure 中的資料進行加密的最佳做法。
- 保護您的 VM 不受惡意程式碼和惡意攻擊的危害。
- 保護已移轉之 Web 應用程式中的機密資訊。
- 確認在移轉後可以存取您 Azure 訂用帳戶和資源的人員。
- 定期檢閱您的 Azure 稽核和安全性記錄。
- 了解及評估 Azure 提供的進階安全性功能。

下列各節會更詳細地說明這些最佳做法。

## <a name="best-practice-follow-azure-security-center-recommendations"></a>最佳做法：遵循 Azure 安全性中心建議

Azure 租使用者系統管理員必須啟用安全性功能，以保護工作負載免于遭受攻擊。 安全性中心提供統一的安全性管理。 從安全性中心，您可以在工作負載之間套用安全性原則、限制暴露于威脅的風險，以及偵測和回應攻擊。 安全性中心會分析 Azure 租使用者之間的資源和設定，並提出安全性建議，包括：

- **集中式原則管理：** 集中管理所有混合式雲端工作負載的安全性原則，以確保符合公司或法規的安全性需求。
- **持續安全性評估：** 監視機器、網路、儲存體和資料服務以及應用程式的安全性狀態，以找出潛在的安全性問題。
- 可 **操作的建議：** 使用優先順序且可採取動作的安全性建議，在攻擊者遭到攻擊之前補救安全性弱點。
- **排定優先順序的警示和事件：** 使用優先的安全性警示和事件，先將焦點放在最嚴重的威脅。

除了評量和建議之外，「安全性中心」還提供您可以針對特定資源啟用的其他安全性功能。

- **及時 (JIT) 存取。** 利用 JIT、受控存取 Azure Vm 上的管理埠，減少網路攻擊面。
  - 在網際網路上開啟 VM RDP 埠3389，會將 Vm 公開給不良執行者的持續活動。 由於 Azure IP 位址是眾所周知的，因此駭客會持續對它們進行探查，以對開啟的 3389 連接埠發動攻擊。
  - JIT 會使用網路安全性群組 (Nsg) 和內送規則，以限制特定埠的開啟時間量。
  - 啟用 JIT 存取後，安全性中心會檢查使用者是否有 Azure 角色型存取控制 (Azure RBAC) VM 的寫入存取權限。 此外，您可以指定使用者如何連線至 Vm 的規則。 如果許可權沒問題，就會核准存取要求，而「安全性中心」會將 Nsg 設定為在您指定的時間量內允許輸入流量進入選取的埠。 當時間過期時，Nsg 會回到其先前的狀態。
- **適應性應用程式控制。** 使用動態允許清單來控制 Vm 上執行的應用程式，以將軟體和惡意程式碼保留在 Vm 上。
  - 適應性應用程式控制可讓您核准應用程式，並防止惡意使用者或系統管理員在您的 Vm 上安裝未經核准或調查的軟體應用程式。
    - 您可以封鎖或警示執行惡意應用程式的嘗試、避免不想要或惡意的應用程式，並確保符合您組織的應用程式安全性原則。
- **檔案完整性監視。** 確保在 VM 上執行之檔案的完整性。
  - 您不需要安裝軟體就會導致 VM 問題。 變更系統檔案也可能會造成 VM 失敗或效能降低。 檔案完整性監視會檢查系統檔案和登錄設定的變更，並在更新內容時通知您。
  - 資訊安全中心會建議您應監視的檔案。

**瞭解更多資訊：**

- 深入瞭解 [Azure 資訊安全中心](/azure/security-center/security-center-introduction)。
- 深入瞭解 [即時 VM 存取](/azure/security-center/security-center-just-in-time)。
- 瞭解如何套用 [適應性應用](/azure/security-center/security-center-adaptive-application)程式控制。
- [開始使用](/azure/security-center/security-center-file-integrity-monitoring)檔案完整性監視。

## <a name="best-practice-encrypt-data"></a>最佳做法：加密資料

加密是 Azure 安全性作法相當重要的一部分。 確保加密已在所有層級啟用，可協助防止未經授權的對象取得敏感性資料 (包括傳輸及待用中的資料) 的存取權。

### <a name="encryption-for-infrastructure-as-a-service"></a>基礎結構即服務的加密

- **虛擬機器：** 針對 Vm，您可以使用 Azure 磁片加密來加密您的 Windows 和 Linux 基礎結構即服務 (IaaS) VM 磁片。
  - Azure 磁片加密會使用適用于 Windows 的 BitLocker，以及適用于 Linux 的 dm crypt，為作業系統和資料磁片提供磁片區加密。
  - 您可以使用由 Azure 所建立的加密金鑰，或是自行提供保護於 Azure Key Vault 中的加密金鑰。
  - 使用 Azure 磁片加密時，IaaS VM 資料會在磁片) 和 VM 開機期間受到待用 (保護。
    - 如果您有未加密的 Vm，則安全性中心會發出警示。
- **儲存體：** 保護儲存在 Azure 儲存體中的待用資料。
  - 儲存在 Azure 儲存體帳戶中的資料可以使用符合 FIPS 140-2 規範的 Microsoft 產生的 AES 金鑰進行加密，或者您也可以使用自己的金鑰。
  - 已針對所有新的和現有的儲存體帳戶啟用 Azure 儲存體加密，且無法停用。

### <a name="encryption-for-platform-as-a-service"></a>平臺即服務的加密

不同于 IaaS，在您管理自己的 Vm 和基礎結構的平臺即服務 (PaaS) 模型平臺和基礎結構是由提供者所管理。 您可以專注于核心應用程式邏輯和功能。 由於 PaaS 服務類型眾多，因此基於安全性目的，我們會對每個服務進行個別評估。 例如，讓我們看看您可能如何啟用 Azure SQL Database 的加密。

- **Always Encrypted：** 使用 SQL Server Management Studio 中的 [永遠加密] 嚮導來保護待用資料。
  - 您可以建立永遠加密金鑰來加密個別的資料行資料。
  - Always Encrypted 金鑰能以加密的形式儲存在資料庫中繼資料中，或是儲存在受信任的金鑰存放區中，例如 Azure Key Vault。
  - 最有可能的情況是，您必須進行應用程式變更，才能使用此功能。
- **透明資料加密 (TDE) ：** 使用靜止的資料庫、相關聯的備份和交易記錄檔的即時加密和解密，保護 Azure SQL Database。
  - TDE 可讓加密活動在應用層沒有變更的情況下進行。
  - TDE 可以使用 Microsoft 所提供的加密金鑰，或者您可以攜帶自己的金鑰。

**瞭解更多資訊：**

- 瞭解 [適用于虛擬機器和虛擬機器擴展集的 Azure 磁片加密](/azure/security/fundamentals/azure-disk-encryption-vms-vmss)。
- 啟用 [適用于 Windows vm 的 Azure 磁片加密](/azure/virtual-machines/windows/disk-encryption-overview)。
- 瞭解待用 [資料的 Azure 儲存體加密](/azure/storage/common/storage-service-encryption)。
- 閱讀 [Always Encrypted 總覽](/azure/azure-sql/database/always-encrypted-azure-key-vault-configure)。
- 閱讀有關 [SQL Database 和 Azure Synapse 的 TDE](/azure/azure-sql/database/transparent-data-encryption-tde-overview)。
- 瞭解 [AZURE SQL DATABASE TDE 與客戶管理的金鑰](/azure/azure-sql/database/transparent-data-encryption-byok-overview)。

## <a name="best-practice-protect-vms-with-antimalware"></a>最佳做法：使用反惡意程式碼保護 Vm

尤其是，較舊的 Azure 遷移的 Vm 可能不會安裝適當的反惡意程式碼層級。 Azure 提供免費的端點解決方案，可協助保護 VM 不受病毒、間諜軟體及其他惡意程式碼的威脅。

- 當已知惡意或垃圾軟體嘗試自行安裝時，適用于 Azure 雲端服務和虛擬機器的 Microsoft 反惡意程式碼會產生警示。
- 它是能在無人為介入的情況下於背景中執行的單一代理程式解決方案。
- 在 [安全性中心] 中，您可以識別未執行 endpoint protection 的 Vm，並視需要安裝 Microsoft 反惡意程式碼。

  ![Vm 的反惡意程式碼螢幕擷取畫面。 ](./media/migrate-best-practices-security-management/antimalware.png)
  *圖1：虛擬機器的反惡意程式碼。*

**瞭解更多資訊：**

- 瞭解 [適用于 Azure 的 Microsoft 反惡意程式碼軟體](/azure/security/fundamentals/antimalware)。

## <a name="best-practice-secure-web-apps"></a>最佳做法：保護 web 應用程式的安全

已移轉的 Web 應用程式會面臨幾個問題：

- 大部分的舊版 Web 應用程式，在設定檔中通常都會有機密資訊。 包含這類資訊的檔案可能會在備份應用程式時，或在應用程式程式碼簽入或移出原始檔控制時出現安全性問題。
- 當您遷移位於 VM 中的 web 應用程式時，您可能會將該機器從內部部署網路和受防火牆保護的環境移至面向網際網路的環境。 確定自己已設定能執行與內部部署保護資源相同工作的解決方案。

Azure 提供下列解決方案：

- **Azure Key Vault：** 現今，web 應用程式開發人員會採取步驟來確保這些檔案不會洩漏機密資訊。 保護資訊的其中一個方法，是將它從檔案中擷取出來，並置於 Azure Key Vault 中。
  - 您可以使用 Key Vault 來集中儲存應用程式秘密，以及控制其散發。 它可避免在應用程式檔中儲存安全性資訊的需要。
  - 應用程式可以使用 Uri 安全地存取保存庫中的資訊，而不需要自訂程式碼。
  - Azure Key Vault 可讓您透過 Azure 安全性控制來鎖定存取，以及順暢地執行輪流金鑰。 Microsoft 看不到或不會將您的資料解壓縮。
- **適用于 Power Apps 的 App Service 環境：** 如果您遷移的應用程式需要額外的保護，請考慮新增 App Service 環境和 Web 應用程式防火牆來保護應用程式資源。
  - App Service 環境可提供完全隔離且專用的環境來執行應用程式，例如 Windows 和 Linux web 應用程式、Docker 容器、行動應用程式和函式應用程式。
  - 它適用于非常高的應用程式、需要隔離和安全的網路存取，或是具有高記憶體使用量的應用程式。
- **Web 應用程式防火牆：** 這是 Azure 應用程式閘道的一項功能，可為 web 應用程式提供集中式保護。
  - 它能在無需後端程式碼修改的情況下保護 Web 應用程式。
  - 它會在應用程式閘道背後同時保護多個 web 應用程式。
  - 您可以使用 Azure 監視器來監視 Web 應用程式防火牆。 Web 應用程式防火牆已整合至「安全性中心」。

  ![Azure Key Vault 與安全 web 應用程式的圖表。 ](./media/migrate-best-practices-security-management/web-apps.png)
  *圖2： Azure Key Vault。*

**瞭解更多資訊：**

- 閱讀 [Azure 金鑰保存庫總覽](/azure/key-vault/general/overview)。
- 深入瞭解 [Web 應用程式防火牆](/azure/web-application-firewall/ag/ag-overview)。
- 閱讀 [App Service 環境的簡介](/azure/app-service/environment/intro)。
- 瞭解如何 [設定 web 應用程式，以從 Key Vault 讀取秘密](/azure/key-vault/general/tutorial-net-create-vault-azure-web-app)。

## <a name="best-practice-review-subscriptions-and-resource-permissions"></a>最佳做法：審查訂用帳戶和資源許可權

在您移轉工作負載並在 Azure 中執行它們的同時，具有工作負載存取權的人員也會四處移動。 您的安全性小組應該定期檢閱針對您 Azure 租用戶和資源群組的存取權。 Azure 提供身分識別管理和存取控制安全性的供應專案，包括 Azure 角色型存取控制 (Azure RBAC) ，以授與存取 Azure 資源的許可權。

- Azure RBAC 會為安全性主體指派存取權限。 安全性主體代表使用者、群組 (一組使用者) 、服務主體 (應用程式和服務所使用的身分識別) ，以及受控識別 (Azure) 自動管理的 Azure Active Directory 身分識別。
- Azure RBAC 可將角色指派給安全性主體 (例如擁有者、參與者和讀取者) 和角色定義 () 定義角色可執行之作業的許可權集合。
- Azure RBAC 也可以設定設定角色界限的範圍。 範圍可以設定于數個層級上，包括管理群組、訂用帳戶、資源群組或資源。
- 確定具有 Azure 存取權的系統管理員只能存取您想要允許的資源。 如果 Azure 中預先定義的角色不夠細微，您可以建立自訂角色以區分並限制存取權限。

確定具有 Azure 存取權的系統管理員只能存取您想要允許的資源。 如果 Azure 中預先定義的角色不夠細微，您可以建立自訂角色以區分並限制存取權限。

  ![存取控制的螢幕擷取畫面。](./media/migrate-best-practices-security-management/subscription.png)
  *圖3：存取控制。*

**瞭解更多資訊：**

- 瞭解 [AZURE RBAC](/azure/role-based-access-control/overview)。
- 瞭解如何透過 [AZURE RBAC 和 azure 入口網站](/azure/role-based-access-control/role-assignments-portal)來管理存取權。
- 瞭解 [自訂角色](/azure/role-based-access-control/custom-roles)。

## <a name="best-practice-review-audit-and-security-logs"></a>最佳做法：審核審核和安全性記錄

Azure AD 提供出現在 Azure 監視器中的活動記錄。 記錄會擷取在 Azure 租用戶中執行的作業、其發生的時間，以及執行它們的人員。

- 稽核記錄會顯示租用戶中的工作歷程記錄。 登入活動記錄會顯示執行該工作的人員。
- 安全性報告的存取方式取決於您的 Azure AD 授權。 您可以使用免費和基本授權，取得有風險的使用者和登入清單。利用 premium 授權，您可以取得基礎事件資訊。
- 您可以將活動記錄路由傳送到各種端點，以進行長期保留並取得資料見解。
- 請檢查記錄，或整合您的安全性資訊與事件管理 (SIEM) 工具來自動審核異常狀況，這是常見的作法。 如果您不是使用 premium 授權，您必須自行進行大量分析，或使用您的 SIEM 系統。 分析包含尋找具風險的登入和事件，以及其他使用者攻擊模式。

  ![Azure AD 使用者和群組的螢幕擷取畫面。 ](./media/migrate-best-practices-security-management/azure-ad.png)
  *圖4： AZURE AD 使用者和群組。*

**瞭解更多資訊：**

- 瞭解 [Azure 監視器中的 AZURE AD 活動記錄](/azure/active-directory/reports-monitoring/concept-activity-logs-azure-monitor)。
- 瞭解如何 [在 AZURE AD 入口網站中審核活動報告](/azure/active-directory/reports-monitoring/concept-audit-logs)。

## <a name="best-practice-evaluate-other-security-features"></a>最佳做法：評估其他安全性功能

Azure 提供能提供進階安全性選項的其他安全性功能。 請注意，下列的一些最佳作法需要附加元件授權和 premium 選項。

- **(AU) 中執行 Azure AD 管理單位。** 使用基本的 Azure 存取控制來委派系統管理工作以支援人員，可能會是一件相當困難的事。 給予支援人員存取權以管理 Azure AD 中的所有群組，對組織的安全性而言可能不是理想的方法。 使用 AU 可讓您以類似的方式，將 Azure 資源隔離到容器中， (Ou) 的內部部署組織單位。 若要使用 AU，AU 系統管理員必須擁有 premium Azure AD 授權。 如需詳細資訊，請參閱 [AZURE AD 中的管理單位管理](/azure/active-directory/users-groups-roles/directory-administrative-units)。
- **使用多重要素驗證。** 如果您有進階 Azure AD 授權，您可以在系統管理員帳戶上啟用並強制執行多重要素驗證。 網路釣魚是用來入侵帳戶認證的最常見方式。 當不良執行者有系統管理員帳號憑證時，不會將其從最接近的動作中停止，例如刪除您所有的資源群組。 您可以用數種方式建立多重要素驗證，包括電子郵件、驗證器應用程式和電話簡訊。 身為系統管理員，您可以選取最不具侵入性的選項。 多重要素驗證會與威脅分析和條件式存取原則整合，以隨機要求多重要素驗證挑戰回應。 深入了解[安全性指引](/azure/active-directory/authentication/multi-factor-authentication-security-best-practices)，以及[如何設定多重要素驗證](/azure/active-directory/authentication/multi-factor-authentication-security-best-practices)。
- **實作條件式存取。** 在大多數中小型組織中，Azure 系統管理員和支援小組可能會位於單一地理位置。 在此情況下，大部分的登入都是來自相同的區域。 如果這些位置的 IP 位址是相當靜態的，您應該不會在這些區域以外看到系統管理員登入。 即使遠端不良執行者危及系統管理員的認證，您也可以執行與多重要素驗證結合的條件式存取等安全性功能，以防止從遠端位置登入。 這也可以防止來自隨機 IP 位址的欺騙位置。 深入瞭解 [條件式存取](/azure/active-directory/conditional-access/overview) ，並查看 Azure AD 中條件式存取的 [最佳做法](/azure/active-directory/conditional-access/best-practices) 。
- **審查企業應用程式許可權。** 經過一段時間後，系統管理員會選擇 Microsoft 和協力廠商連結，而不知道其對組織的影響。 連結可以呈現將許可權指派給 Azure 應用程式的同意畫面。 這可能會允許存取讀取 Azure AD 資料，甚至是完整存取權來管理整個 Azure 訂用帳戶。 您應定期檢查您的系統管理員和使用者允許存取 Azure 資源的應用程式。 確定這些應用程式只具有必要的許可權。 此外，您也可以使用應用程式頁面的連結傳送電子郵件給使用者，讓他們知道他們允許存取其組織資料的應用程式。 如需詳細資訊，請參閱 [我的應用程式清單中的未預期應用程式](/azure/active-directory/manage-apps/application-types)，以及 [如何控制](/azure/active-directory/manage-apps/remove-user-or-group-access-portal) Azure AD 中的應用程式指派。

## <a name="managed-migrated-workloads"></a>管理已移轉的工作負載

在下列各節中，我們將建議 Azure 管理的一些最佳做法，包括：

- 適用於 Azure 資源群組和資源的最佳做法，包括智慧型命名、防止意外刪除、管理資源權限，以及有效的資源標記。
- 取得使用藍圖來建置及管理部署環境的快速概觀。
- 在建置移轉後部署期間檢閱範例 Azure 架構來取得了解。
- 如果您有多個訂用帳戶，您可以將它們收集成管理群組，並將治理設定套用至那些群組。
- 將合規性原則套用到您的 Azure 資源。
- 規劃商務持續性和災害復原 (BCDR) 策略來保護資料、確保環境的可復原性，並使資源能在發生中斷時持續運作。
- 將 VM 分組為可用性群組，以確保復原性和高可用性。 使用受控磁碟來輕鬆進行 VM 磁碟和儲存體管理。
- 針對 Azure 資源啟用診斷記錄、建置警示和劇本以主動進行疑難排解，以及使用 Azure 儀表板來取得部署健康情況和狀態的統一檢視。
- 瞭解您的 Azure 支援方案以及如何實行，取得讓 Vm 保持最新狀態的最佳做法，並為變更管理提供適當的處理常式。

## <a name="best-practice-name-resource-groups"></a>最佳做法：命名資源群組

確定您的資源群組具有有意義的名稱，可讓系統管理員和支援小組成員輕鬆地辨識和掃描。 這可大幅提升生產力和效率。

如果您要使用 Azure AD Connect 將內部部署 Active Directory 同步處理至 Azure AD，請考慮將內部部署安全性群組的名稱與 Azure 中的資源群組名稱進行比對。

  ![資源群組命名的螢幕擷取畫面。](./media/migrate-best-practices-security-management/naming.png)
  *圖5：資源群組命名。*

**瞭解更多資訊：**

- 瞭解 [建議的命名慣例](../../ready/azure-best-practices/naming-and-tagging.md)。

## <a name="best-practice-implement-delete-locks-for-resource-groups"></a>最佳做法：針對資源群組執行刪除鎖定

您最不想遇到的情況，便是資源群組被意外刪除而消失不見。 建議您執行刪除鎖定，如此一來，就不會發生這種情況。

  ![刪除鎖定的螢幕擷取畫面。](./media/migrate-best-practices-security-management/locks.png)
  *圖6：刪除鎖定。*

**瞭解更多資訊：**

- 瞭解 [鎖定資源以防止非預期的變更](/azure/azure-resource-manager/management/lock-resources)。

## <a name="best-practice-understand-resource-access-permissions"></a>最佳做法：瞭解資源存取權限

訂用帳戶擁有者會具有訂用帳戶中所有資源群組和資源的存取權。

- 請謹慎地將這個貴重的權限指派給其他人員。 了解指派這些權限類型的後果，將能大幅協助保持環境的安全及穩定性。
- 請確定您將資源放在適當的資源群組中：
  - 將具有類似生命週期的資源配對在一起。 在理想情況下，您應該不需要在刪除整個資源群組時移動某個資源。
  - 支援某個功能或工作負載的資源應該被放置在一起，以簡化管理工作。

**瞭解更多資訊：**

- 瞭解如何組織訂用帳戶 [和資源群組](https://azure.microsoft.com/blog/organizing-subscriptions-and-resource-groups-within-the-enterprise/)。

## <a name="best-practice-tag-resources-effectively"></a>最佳做法：有效標記資源

通常，僅使用與資源相關的資源組名，並不會提供足夠的中繼資料來有效實行機制，例如在訂用帳戶內進行內部計費或管理。

- 最佳做法是使用 Azure 標記來新增可查詢和報告的實用中繼資料。
- 標記能提供搭配您所定義的屬性，以邏輯方式組織資源的方式。 標記可以直接套用到資源群組或資源。
- 標記可以套用到資源群組或是個別的資源之上。 資源群組標記不會被群組中的資源所繼承。
- 您可以使用 PowerShell 或 Azure 自動化來將標記自動化，或標記個別的群組和資源。
- 如果您具有要求或變更管理系統，則可以輕鬆地在要求中使用該資訊，以填入公司特定的資源標記。

  ![標記的螢幕擷取畫面。 ](./media/migrate-best-practices-security-management/tagging.png)
  *圖7：標記。*

**瞭解更多資訊：**

- 瞭解 [標記和標記限制](/azure/azure-resource-manager/management/tag-resources)。
- [請參閱 PowerShell 和 CLI 範例，以設定標記，以及將標籤從資源群組套用至其資源](/azure/azure-resource-manager/management/tag-resources#powershell)。
- [閱讀](https://www.azurefieldnotes.com/2016/07/18/azure-resource-tagging-best-practices) \(英文\) Azure 標記最佳做法。

## <a name="best-practice-implement-blueprints"></a>最佳做法：實行藍圖

如同藍圖可讓工程師和架構設計人員草擬專案的設計參數，Azure 藍圖服務可讓雲端架構設計人員和中央 IT 群組定義一組可重複使用的 Azure 資源。 這可協助他們實行和遵循組織的標準、模式和需求。 使用 Azure 藍圖，開發小組可以快速建立及建立符合組織合規性需求的新環境。 這些新環境具有一組內建元件（例如網路），可加速開發和傳遞。

- 使用藍圖來協調資源群組、Azure Resource Manager 範本，以及原則和角色指派的部署。
- 將藍圖儲存在全域散發的服務 Azure Cosmos DB 中。 藍圖物件會複寫至多個 Azure 區域。 複寫可提供低延遲、高可用性和一致的藍圖存取，不論藍圖部署資源的區域為何。

**瞭解更多資訊：**

- [閱讀](/azure/governance/blueprints/overview)藍圖的相關資訊。
- 複習可 [在醫療保健中加速 AI 的範例藍圖](https://azure.microsoft.com/blog/customizing-azure-blueprints-to-accelerate-ai-in-healthcare/)。

## <a name="best-practice-review-azure-reference-architectures"></a>最佳做法：審查 Azure 參考架構

在 Azure 中建置安全、可擴充且可管理的工作負載，可能會是一件令人望之卻步的工作。 由於變更會持續發生，因此在顧及各種不同功能之下維持最佳化的環境，可能會相當困難。 在設計及移轉工作負載時，擁有可從中學習的參考可能會很有幫助。 Azure 和 Azure 合作夥伴已針對各種環境類型建置數個範例參考架構。 這些範例是設計來提供各種想法，以供您從中學習並將它作為建置範本。

參考架構會依案例編排。 其中包含有關管理、可用性、擴充性和安全性的最佳作法和建議。 App Service 環境可提供完全隔離且專用的環境來執行應用程式，例如 Windows 和 Linux web 應用程式、Docker 容器、行動應用程式和功能。 App Service 能將 Azure 的功能新增到您的應用程式，其中包括安全性、負載平衡、自動調整和自動化管理。 您也可以利用它的 DevOps 功能，例如來自 Azure DevOps 和 GitHub 的持續部署、套件管理、預備環境、自訂網域和 SSL 憑證。 App Service 適用于需要隔離和安全網路存取的應用程式，以及使用大量記憶體和其他需要調整之資源的應用程式。

**瞭解更多資訊：**

- 深入瞭解 [Azure 參考架構](/azure/architecture/browse/)。
- 查看 [Azure 範例案例](/azure/architecture/browse/)。

## <a name="best-practice-manage-resources-with-azure-management-groups"></a>最佳做法：使用 Azure 管理群組來管理資源

如果您的組織具有多個訂用帳戶，您便需要為它們管理存取、原則及合規性。 Azure 管理群組提供了訂用帳戶之上的範圍層級。 以下是一些秘訣：

- 您會將訂用帳戶整理到稱為管理群組的容器中，並將治理條件套用至它們。
- 管理群組中的所有訂用帳戶都會自動繼承管理群組條件。
- 無論您擁有何種類型的訂用帳戶，管理群組都能提供大規模的企業級管理。
- 例如，您可以套用能限制可建立 VM 之區域的管理群組原則。 此原則接著會套用至所有的管理群組、訂用帳戶以及該管理群組下的資源。
- 您可以建置管理群組和訂用帳戶的彈性結構，將資源組織到一個階層中，以便執行統一原則與存取管理。

下圖顯示使用管理群組建立治理階層的範例。

  ![管理群組的圖表。](./media/migrate-best-practices-security-management/management-groups.png)
  *圖8：管理群組。*

**瞭解更多資訊：**

- 深入瞭解如何將 [資源組織成管理群組](/azure/governance/management-groups/)。

## <a name="best-practice-deploy-azure-policy"></a>最佳做法：部署 Azure 原則

Azure 原則是一項服務，您可用來建立、指派和管理原則。 原則會對您的資源強制執行不同的規則和效果，讓這些資源能符合您公司標準和服務等級協定的規範。

Azure 原則會評估您的資源，掃描那些不符合您原則規範的資源。 例如，您可以建立原則，只允許您環境中 Vm 的特定 SKU 大小。 當您建立和更新資源，以及掃描現有資源時，Azure 原則會評估此設定。 請注意，Azure 提供一些可供您指派的內建原則，您也可以建立自己的原則。

  ![Azure 原則的螢幕擷取畫面。](./media/migrate-best-practices-security-management/policy.png)
  *圖9： Azure 原則。*

**瞭解更多資訊：**

- 閱讀 [Azure 原則總覽](/azure/governance/policy/overview)。
- 瞭解 [如何建立和管理原則以強制執行合規性](/azure/governance/policy/tutorials/create-and-manage)。

## <a name="best-practice-implement-a-bcdr-strategy"></a>最佳做法：實行 BCDR 策略

規劃 BCDR 是您應在 Azure 遷移規劃過程中完成的重要練習。 就法律規定而言，您的合約可能包含 *強制不可抗力* 子句，因為有更大的強制，例如颶風或地震。 但是您必須確保在發生嚴重損壞時，必要的服務仍繼續執行和復原。 您達成此義務的能力，可能會左右您公司的未來發展。

廣義來說，您的 BCDR 策略必須考慮：

- **資料備份：** 如何保護您的資料安全，讓您可以在發生中斷時輕鬆復原。
- 嚴重損壞 **修復：** 如何讓您的應用程式在發生中斷時保持復原並可供使用。

### <a name="set-up-bcdr"></a>設定 BCDR

當您遷移至 Azure 時，請瞭解雖然 Azure 平臺提供一些內建的復原功能，但您需要設計您的 Azure 部署以充分利用這些功能。

- 您的 BCDR 解決方案將取決於您的公司目標，且會受到您的 Azure 部署策略所影響。 基礎結構即服務 (IaaS) 和平台即服務 (PaaS) 部署會為 BCDR 帶來不同的挑戰。
- 完成之後，應定期測試您的 BCDR 解決方案，以檢查您的策略是否維持可行。

### <a name="back-up-an-iaas-deployment"></a>備份 IaaS 部署

在大部分的情況下，內部部署工作負載會在移轉後淘汰，且您必須對用於備份資料的內部部署策略進行擴充或加以取代。 如果您將整個資料中心遷移到 Azure，您必須使用 Azure 技術或協力廠商整合式解決方案，來設計和執行完整的備份解決方案。

針對在 Azure IaaS VM 上執行的工作負載，請考慮這些備份解決方案：

- **Azure 備份：** 提供適用于 Azure Windows 和 Linux Vm 的應用程式一致備份。
- **儲存體快照集：** 取得 Blob 儲存體的快照集。

#### <a name="azure-backup"></a>Azure 備份

Azure 備份會建立儲存在 Azure 儲存體中的資料復原點。 Azure 備份可以備份 Azure VM 磁碟，以及 Azure 檔案 (預覽)。 Azure 檔案提供雲端中的檔案共用，可透過伺服器訊息區 (SMB) 來存取。

您可以透過下列方式使用 Azure 備份來備份 Vm：

- **從 VM 設定直接備份。** 您可以直接從 Azure 入口網站中的 VM 選項，直接搭配 Azure 備份來備份 VM。 您每天可以備份一次 VM，而且您可以視需要還原 VM 磁片。 Azure 備份會採用可感知應用程式的資料快照集，且 VM 上不會安裝任何代理程式。
- **在復原服務保存庫中直接備份。** 您可以部署 Azure 備份復原服務保存庫，來備份您的 IaaS VM。 這會提供單一位置來追蹤和管理備份，以及細微的備份和還原選項。 在檔案和資料夾層級，備份一天最多三次。 它無法感知應用程式，且不支援 Linux。 使用這個方法，在您要備份的每個 VM 上安裝 Microsoft Azure 復原服務 (MARS) 代理程式。
- **將 VM 保護至 Azure 備份伺服器。** Azure 備份伺服器是免費提供的 Azure 備份。 VM 會備份到本機 Azure 備份伺服器儲存體。 然後，您可以將 Azure 備份伺服器備份至保存庫中的 Azure。 備份可感知應用程式，並具有備份頻率和保留的完整細微性。 您可以在應用層級進行備份，例如備份 SQL Server 或 SharePoint。

針對安全性，Azure 備份會使用 AES-256 來加密進行中的資料。 它會透過 HTTPS 將它傳送至 Azure。 Azure 中已備份的待用資料會使用 [Azure 儲存體加密](/azure/storage/common/storage-service-encryption)進行加密。

![Azure 備份的螢幕擷取畫面。 ](./media/migrate-best-practices-security-management/iaas-backup.png)
*圖10： Azure 備份。*

**瞭解更多資訊：**

- 瞭解 [Azure 備份](/azure/backup/backup-overview)。
- 規劃 [Azure vm 的備份基礎結構](/azure/backup/backup-azure-vms-introduction)。

#### <a name="storage-snapshots"></a>儲存體快照集

Azure VM 會以分頁 Blob 的形式儲存在 Azure 儲存體中。 快照集會擷取特定時間點的 Blob 狀態。 作為 Azure VM 磁碟的替代備份方法，您可以擷取儲存體 Blob 的快照集，然後將它們複製到另一個儲存體帳戶。

您可以複製整個 Blob，或使用增量快照複製以僅複製差異變更，並減少儲存空間。 為了額外的預防措施，您可以啟用 Blob 儲存體帳戶的虛刪除。 啟用此功能時，已刪除的 blob 會標示為刪除，但不會立即清除。 在過渡期間，您可以還原 blob。

**瞭解更多資訊：**

- 深入瞭解 [Azure Blob 儲存體](/azure/storage/blobs/storage-blobs-introduction)。
- 瞭解如何 [建立 blob 快照](/azure/storage/blobs/snapshots-overview)集。
- 檢查 Blob 儲存體備份的[範例案例](https://azure.microsoft.com/blog/microsoft-azure-block-blob-storage-backup/)。
- 深入瞭解 [blob 的虛刪除](/azure/storage/blobs/soft-delete-blob-overview)。
- [Azure 儲存體中的災害復原和強制容錯移轉 (預覽)](/azure/storage/common/storage-disaster-recovery-guidance)

#### <a name="third-party-backup"></a>協力廠商備份

除此之外，您可以使用協力廠商解決方案來將 Azure VM 和儲存體容器備份到本機儲存體或其他雲端服務提供者。 如需詳細資訊，請參閱 [Azure Marketplace 中的備份解決方案](https://azuremarketplace.microsoft.com/marketplace/apps?search=backup&page=1)。

### <a name="set-up-disaster-recovery-for-iaas-applications"></a>設定 IaaS 應用程式的嚴重損壞修復

除了保護資料之外，BCDR 規劃也必須考慮在發生嚴重損壞時，如何讓應用程式和工作負載保持可用。 針對在 Azure IaaS Vm 和 Azure 儲存體上執行的工作負載，請考慮以下各節中的解決方案。

#### <a name="azure-site-recovery"></a>Azure Site Recovery

Azure Site Recovery 是主要的 Azure 服務，可確保在發生中斷時，可以讓 Azure Vm 上線，並讓 VM 應用程式可供使用。

Site Recovery 會將 Vm 從主要區域複寫到次要 Azure 區域。 如果發生嚴重損壞狀況，您可以從主要區域將 Vm 容錯移轉，並在次要區域中繼續以正常方式存取它們。 當作業返回正常時，您便可以將 VM 容錯回復到主要區域。

  ![Azure Site Recovery 的圖表。](./media/migrate-best-practices-security-management/site-recovery.png)
  *圖11： Site Recovery。*

**瞭解更多資訊：**

- 查看 [Azure vm 的嚴重損壞修復案例](/azure/virtual-machines/virtual-machines-disaster-recovery-guidance)。
- 瞭解如何 [在遷移之後設定 AZURE VM 的](/azure/site-recovery/azure-to-azure-replicate-after-migration)嚴重損壞修復。

## <a name="best-practice-use-managed-disks-and-availability-sets"></a>最佳做法：使用受控磁片和可用性設定組

Azure 會使用可用性設定組來以邏輯方式將 VM 分組在一起，以及將設定組中的 VM 與其他資源隔離。 可用性設定組中的 Vm 會散佈在多個容錯網域中，並具有個別的子系統，以防止本機失敗。 Vm 也會分散到多個更新網域，以防止同時重新開機集合中的所有 Vm。

Azure 受控磁片會管理與 VM 磁片相關聯的儲存體帳戶，以簡化 Azure 虛擬機器的磁片管理。

- 盡可能使用受控磁片。 您只需要指定您想要使用的儲存體類型和所需的磁片大小，Azure 就會為您建立及管理磁片。
- 您可以將現有的磁片轉換成受控磁片。
- 您應該在可用性設定組中建立 VM，以取得高復原性和可用性。 當發生計畫或未規劃的中斷時，可用性設定組可確保集合中至少有一部 VM 保持可用。

  ![受控磁片的圖表。 ](./media/migrate-best-practices-security-management/managed-disks.png)
  *圖12：受控磁片。*

**瞭解更多資訊：**

- 閱讀 [受控磁片總覽](/azure/virtual-machines/managed-disks-overview)。
- 瞭解 [如何將磁片轉換成受控磁片](/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks)。
- 瞭解如何 [在 Azure 中管理 Windows vm 的可用性](/azure/virtual-machines/manage-availability)。

## <a name="best-practice-monitor-resource-usage-and-performance"></a>最佳做法：監視資源使用量和效能

您將工作負載移轉到 Azure 的原因，可能是為了它強大的擴充能力。 但移動您的工作負載並不表示 Azure 會自動執行調整，而不需要您的輸入。 以下是兩個範例：

- 如果您的行銷組織推播新的電視公告，以促進300% 以上的流量，這可能會導致網站可用性問題。 您剛遷移的工作負載可能會達到指派的限制，而且會損毀。
- 如果有分散式阻絕服務 (DDoS) 攻擊已遷移的工作負載，在此情況下，您不會想要調整規模。 您想要防止攻擊來源到達您的資源。

這兩種案例具有不同的解決方式，但在這兩種情況下，您需要深入瞭解使用狀況和效能監視的情況。

- Azure 監視器可協助呈現這些計量，並提供警示、自動調整、事件中樞和邏輯應用程式的回應。
- 您也可以整合協力廠商 SIEM 應用程式，以監視 Azure 記錄檔，以取得審核和效能事件。

  ![Azure 監視器的螢幕擷取畫面。 ](./media/migrate-best-practices-security-management/monitor.png)
  *圖13： Azure 監視器。*

**瞭解更多資訊：**

- 了解 [Azure 監視器](/azure/azure-monitor/overview)。
- 取得監視和診斷的[最佳做法](/azure/architecture/best-practices/monitoring)。
- 深入瞭解[自動調整。](/azure/architecture/best-practices/auto-scaling)
- 瞭解如何將 [Azure 資料路由傳送至 SIEM 工具](/azure/security-center/security-center-partner-integration)。

## <a name="best-practice-enable-diagnostic-logging"></a>最佳做法：啟用診斷記錄

Azure 資源會產生相當多的記錄計量和遙測資料。 根據預設，大部分的資源類型都沒有啟用診斷記錄。 透過在資源上啟用診斷記錄，您可以查詢記錄資料，並根據它建置警示和劇本。

當您啟用診斷記錄時，每個資源都會有特定的類別集合。 您可以選取一或多個記錄類別，以及記錄資料的儲存位置。 記錄可以傳送至儲存體帳戶、事件中樞或 Azure 監視器記錄。

![診斷記錄的螢幕擷取畫面。 ](./media/migrate-best-practices-security-management/diagnostics.png)
*圖14：診斷記錄。*

**瞭解更多資訊：**

- 瞭解如何 [收集和使用記錄資料](/azure/azure-monitor/essentials/platform-logs-overview)。
- 瞭解支援 [診斷記錄](/azure/azure-monitor/essentials/resource-logs-schema)的功能。

## <a name="best-practice-set-up-alerts-and-playbooks"></a>最佳做法：設定警示和操作手冊

在針對 Azure 資源啟用診斷記錄的情況下，您可以開始使用記錄資料來建立自訂警示。

- 當您的監視資料中發現條件時，警示會主動通知您。 您接著便可以在系統使用者注意到這些問題之前解決他們。 您可以針對計量值、記錄搜尋查詢、活動記錄事件、平臺健康情況和網站可用性發出警示。
- 觸發警示時，您可以執行邏輯應用程式腳本。 劇本可協助您針對特定警示自動化並協調回應。 劇本是以 Azure Logic Apps 為基礎。 您可以使用邏輯應用程式範本來建立腳本，或建立您自己的工作手冊。
- 簡單的範例，您可以建立警示，以在 NSG 的埠掃描發生時觸發。 您可以設定能執行並封鎖掃描來源之 IP 位址的劇本。
- 另一個範例是具有記憶體流失的應用程式。 當記憶體使用量達到某個程度時，範本便可以回收處理程序。

  ![警示的螢幕擷取畫面。 ](./media/migrate-best-practices-security-management/alerts.png)
  *圖15：警示。*

**瞭解更多資訊：**

- 瞭解 [警示](/azure/azure-monitor/alerts/alerts-overview)。
- 瞭解 [回應資訊安全中心警示的安全性手冊](/azure/security-center/workflow-automation)。

## <a name="best-practice-use-the-azure-dashboard"></a>最佳做法：使用 Azure 儀表板

Azure 入口網站是網頁型的統一主控台，可讓您建置、管理及監控所有項目，從簡單 Web 應用程式到複雜的雲端應用程式皆包含在內。 它也包含了自訂的儀表板各種協助工具選項。

- 您可以建立多個儀表板，並與可存取您 Azure 訂用帳戶的其他人共用。
- 透過此共用模型，您的小組可以看到 Azure 環境，以協助他們在管理雲端中的系統時主動主動。

  ![Azure 儀表板的螢幕擷取畫面。](./media/migrate-best-practices-security-management/dashboard.png)

  ![Azure 儀表板的螢幕擷取畫面。 ](./media/migrate-best-practices-security-management/dashboard.png)
  *圖16： Azure 儀表板。*

**瞭解更多資訊：**

- 瞭解如何 [建立儀表板](/azure/azure-portal/azure-portal-dashboards)。
- 深入瞭解 [儀表板結構](/azure/azure-portal/azure-portal-dashboards-structure)。

## <a name="best-practice-understand-support-plans"></a>最佳做法：瞭解支援方案

在某個時間點，您將必須與支援人員或 Microsoft 支援服務人員進行共同作業。 擁有一系列原則和程序以在災害復原等案例期間提供支援，是一件非常重要的事。 此外，您的系統管理員和支援人員都應該接受訓練以實作那些原則。

- 在 Azure 服務問題影響到您工作負載的罕見情況下，系統管理員應該要知道如何以最適當且有效率的方式向 Microsoft 提交支援票證。
- 請熟悉各種針對 Azure 所提供的支援方案。 其範圍從開發人員實例專用的回應時間，到回應時間少於15分鐘的頂級支援。

  ![支援方案的螢幕擷取畫面。 ](./media/migrate-best-practices-security-management/support.png)
  *圖17：支援方案。*

**瞭解更多資訊：**

- 閱讀 [Azure 支援方案的總覽](https://azure.microsoft.com/support/options/)。
- 瞭解 [ (sla) 的服務等級協定 ](https://azure.microsoft.com/support/legal/sla/)。

## <a name="best-practice-manage-updates"></a>最佳做法：管理更新

搭配最新的作業系統和軟體更新來將 Azure VM 保持在最新狀態，是一件規模龐大的工作。 能夠呈現所有 Vm、找出所需的更新，並自動推送這些更新非常重要。

- 您可以使用 Azure 自動化中的更新管理來管理作業系統更新。 這適用于執行部署在 Azure、內部部署和其他雲端提供者中的 Windows 和 Linux 電腦的電腦。
- 使用更新管理以快速評估所有代理程式電腦上可用更新的狀態，並管理更新安裝。
- 您可以從 Azure 自動化帳戶直接為 VM 啟用更新管理。 您也可以在 Azure 入口網站中從 VM 頁面更新單一 VM。
- 此外，您可以向 System Center Configuration Manager 註冊 Azure Vm。 然後，您可以將 Configuration Manager 工作負載遷移至 Azure，並從單一 web 介面進行報告和軟體更新。

  ![VM 更新的圖表。 ](./media/migrate-best-practices-security-management/updates.png)
  *圖18： VM 更新。*

**瞭解更多資訊：**

- 瞭解 [Azure 中的更新管理](/azure/automation/update-management/overview)。
- 瞭解如何 [整合 Configuration Manager 與更新管理](/azure/automation/update-management/mecmintegration)。
- 關於 Azure 中 Configuration Manager 的[常見問題集](/mem/configmgr/core/understand/configuration-manager-on-azure)。

## <a name="implement-a-change-management-process"></a>實作變更管理程序

如同任何生產系統，任何類型的變更都可能會對您的環境產生影響。 變更管理程序能讓使用者需要提交要求才能對生產系統進行變更，這對於已移轉的環境來說是一個相當有價值的額外功能。

- 您可以針對變更管理建置最佳做法架構，以協助系統管理員和支援人員熟悉這個程序。
- 您可以使用 Azure 自動化來協助針對已移轉的工作流程進行組態管理及變更追蹤。
- 強制執行變更管理程式時，您可以使用 audit 記錄，將 Azure 變更記錄連結至現有的變更要求。 然後，如果您在沒有對應變更要求的情況下看到變更，您可以調查程式中發生的問題。

Azure 在 Azure 自動化中具有變更追蹤解決方案：

- 這個解決方案會追蹤對 Windows 與 Linux 軟體和檔案、Windows 登錄機碼、Windows 服務以及 Linux 精靈所做的變更。
- 受監視伺服器上的變更會傳送至 Azure 監視器進行處理。
- 邏輯會套用至接收的資料，而雲端服務會記錄資料。
- 在 [變更追蹤] 儀表板上，您可以輕鬆地查看在伺服器基礎結構中所做的變更。

  ![變更管理圖表的螢幕擷取畫面。](./media/migrate-best-practices-security-management/change.png)

  _圖19：變更管理圖表。_

**瞭解更多資訊：**

- 瞭解 [變更追蹤](/azure/automation/change-tracking/overview)。
- 瞭解 [Azure 自動化功能](/azure/automation/automation-intro)。

## <a name="next-steps"></a>下一步

檢閱其他最佳做法：

- 移轉後的網路功能[最佳做法](./migrate-best-practices-networking.md)。
- 移轉後的成本管理[最佳做法](./migrate-best-practices-costs.md)。
