---
title: 保護和管理遷移至 Azure 之工作負載的最佳做法
description: 使用適用于 Azure 的雲端採用架構，以瞭解操作、管理及保護已遷移工作負載的最佳做法。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 73cf542a7d40e58f7da8ec94303dc4c815d71ad4
ms.sourcegitcommit: bcc73d194c6d00c16ae2e3c7fb2453ac7dbf2526
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/09/2020
ms.locfileid: "86194131"
---
<!-- cSpell:ignore FIPS SIEM majeure NSGs -->

# <a name="best-practices-to-secure-and-manage-workloads-migrated-to-azure"></a>保護和管理遷移至 Azure 之工作負載的最佳做法

在您針對移轉進行規劃及設計的同時，除了思考有關移轉本身的事項之外，您也需要考慮移轉後在 Azure 中的安全性和管理模型。 本文會描述適用於在移轉之後保護您的 Azure 部署，以及適用於使部署以最佳狀態持續執行之工作的規劃與最佳做法。

> [!IMPORTANT]
> 本文所述的最佳做法和意見是以本文撰寫當下可用的 Azure 平台和服務功能作為基礎。 特色與功能會隨著時間改變。

## <a name="secure-migrated-workloads"></a>保護已移轉的工作負載

在移轉之後，最重要的工作便是保護已移轉的工作負載，使其不受來自內部和外部的威脅。 這些最佳做法可以協助您執行此作業：

- [使用 Azure 資訊安全中心](#best-practice-follow-azure-security-center-recommendations)：瞭解如何使用 Azure 資訊安全中心所提供的監視、評量和建議。
- [加密您的資料](#best-practice-encrypt-data)：取得在 Azure 中加密資料的最佳作法。
- [設定反惡意](#best-practice-protect-vms-with-antimalware)代碼：保護您的 vm 免于遭受惡意軟體和惡意的攻擊。
- [保護 web 應用程式](#best-practice-secure-web-apps)：在遷移的 web 應用程式中保護機密資訊的安全。
- [審查](#best-practice-review-subscriptions-and-resource-permissions)訂用帳戶：確認誰可以在遷移後存取您的 Azure 訂用帳戶和資源。
- 使用[記錄](#best-practice-review-audit-and-security-logs)：定期檢查您的 Azure 審核和安全性記錄。
- [查看其他安全性功能](#best-practice-evaluate-other-security-features)：瞭解及評估 Azure 提供的先進安全性功能。

## <a name="best-practice-follow-azure-security-center-recommendations"></a>最佳做法：遵循 Azure 資訊安全中心建議

Microsoft 努力確保 Azure 租用戶系統管理員能擁有必要的資訊，以提供能保護工作負載不受攻擊的安全性功能。 Azure 資訊安全中心能提供統一的安全性管理。 您可以從從資訊安全中心跨工作負載套用安全性原則、限制暴露在威脅下的風險，以及偵測並回應攻擊。 資訊安全中心會跨 Azure 租用戶分析資源及設定，然後提供安全性建議，其中包括：

- **集中式原則管理：** 藉由集中管理所有混合式雲端工作負載的安全性原則，確保符合公司或法規的安全性需求。
- **持續安全性評估：** 監視機器、網路、儲存體和資料服務以及應用程式的安全性狀態，以找出潛在的安全性問題。
- 可**操作的建議：** 補救安全性弱點，攻擊者可以利用已設定優先順序和可操作的安全性建議來加以入侵。
- **排定優先順序的警示和事件：** 先專注于最重要的威脅，再加上優先順序的安全性警示和事件。

除了評量和建議之外，Azure 資訊安全中心也能提供可針對特定資源啟用的其他安全性功能。

- **即時 (JIT) 存取。** 使用 Just-In-Time 來控制針對 Azure VM 上管理連接埠的存取，以減少您網路的受攻擊面。
  - 在網際網路上開啟 VM RDP 連接埠 3389，將會使 VM 持續暴露在不良執行者活動之下。 由於 Azure IP 位址是眾所周知的，因此駭客會持續對它們進行探查，以對開啟的 3389 連接埠發動攻擊。
  - Just-In-Time 會使用能限制指定連接埠開啟時間的網路安全性群組 (NSG) 及傳入規則。
  - 在啟用 Just-In-Time 的情況下，資訊安全中心會檢查使用者是否具有 VM 的角色型存取控制 (RBAC) 寫入存取權限。 此外，還會指定使用者如何能連線到 VM 的規則。 如果權限沒問題，系統便會核准要求，且資訊安全中心會設定 NSG 以在您指定時間內允許針對選取連接埠的傳入流量。 時間到期時，NSG 便會返回其先前的狀態。
- **適應性應用程式控制。** 藉由使用動態允許清單來控制要在其上執行的應用程式，將軟體和惡意程式碼保留在 Vm 上。
  - 彈性應用程式控制可讓您核准應用程式，並防止惡意使用者或系統管理員在您的 Vm 上安裝未經核准或調查的軟體應用程式。
    - 您可以封鎖或警示嘗試執行惡意應用程式、避免不必要或惡意的應用程式，並確保符合您組織的應用程式安全性原則。
- **檔案完整性監視。** 確保在 VM 上執行之檔案的完整性。
  - 您不需要安裝軟體來造成 VM 問題。 變更系統檔案也可能會造成 VM 失敗或效能降低。 檔案完整性監視會檢查系統檔案和登錄設定的變更，並在有內容更新時通知您。
  - 資訊安全中心會建議您應監視的檔案。

**瞭解更多資訊：**

- 深入瞭解[Azure 資訊安全中心](https://docs.microsoft.com/azure/security-center/security-center-intro)。
- 深入瞭解即時[VM 存取](https://docs.microsoft.com/azure/security-center/security-center-just-in-time)。
- 瞭解如何套用彈性[應用](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application)程式控制。
- [開始使用](https://docs.microsoft.com/azure/security-center/security-center-file-integrity-monitoring)檔案完整性監視。

## <a name="best-practice-encrypt-data"></a>最佳做法：加密資料

加密是 Azure 安全性作法相當重要的一部分。 確保加密已在所有層級啟用，可協助防止未經授權的對象取得敏感性資料 (包括傳輸及待用中的資料) 的存取權。

### <a name="encryption-for-iaas"></a>適用於 IaaS 的加密

- **虛擬機器：** 針對 Vm，您可以使用 Azure 磁碟加密來加密您的 Windows 和 Linux IaaS VM 磁片。
  - Azure 磁碟加密使用適用于 Windows 的 BitLocker，以及適用于 Linux 的 dm crypt 來提供 OS 和資料磁片的磁片區加密。
  - 您可以使用由 Azure 所建立的加密金鑰，或是自行提供保護於 Azure Key Vault 中的加密金鑰。
  - 使用 Azure 磁碟加密，IaaS VM 資料會在磁片) 和 VM 開機期間的待用 (受到保護。
    - Azure 資訊安全中心會在您有未加密的 VM 時向您傳送警示。
- **儲存體：** 保護儲存在 Azure 儲存體中的待用資料。
  - Azure 儲存體帳戶中儲存的資料可以使用符合 FIPS 140-2 規範的 Microsoft 產生的 AES 金鑰進行加密，或者您也可以使用自己的金鑰。
  - 已針對所有新的和現有的儲存體帳戶啟用 Azure 儲存體加密，且無法停用。

### <a name="encryption-for-paas"></a>適用於 PaaS 的加密

與您管理自己的 Vm 和基礎結構的 IaaS 不同，PaaS 模型平臺和基礎結構是由提供者所管理，讓您專注于核心應用程式邏輯和功能。 由於 PaaS 服務類型眾多，因此基於安全性目的，我們會對每個服務進行個別評估。 例如，讓我們看看要如何針對 Azure SQL 資料庫啟用加密。

- **Always Encrypted：** 使用 SQL Server Management Studio 中的 Always Encrypted wizard 來保護待用資料。
  - 您會建立 Always Encrypted 金鑰來加密個別的資料行資料。
  - Always Encrypted 金鑰能以加密的形式儲存在資料庫中繼資料中，或是儲存在受信任的金鑰存放區中，例如 Azure Key Vault。
  - 使用此功能時，可能需要進行應用程式變更。
- **透明資料加密 (TDE) ：** 使用資料庫、相關聯的備份和待用的交易記錄檔的即時加密和解密，保護 Azure SQL Database。
  - TDE 允許在不變更應用層的情況下進行加密活動。
  - TDE 可以使用 Microsoft 所提供的加密金鑰，或者您可以攜帶自己的金鑰。

**瞭解更多資訊：**

- 深入瞭解[虛擬機器和虛擬機器擴展集的 Azure 磁碟加密](https://docs.microsoft.com/azure/security/fundamentals/azure-disk-encryption-vms-vmss)。
- 啟用[Windows vm 的 Azure 磁碟加密](https://docs.microsoft.com/azure/virtual-machines/windows/disk-encryption-overview)。
- 瞭解待用[資料的 Azure 儲存體加密](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)。
- 閱讀[Always Encrypted 的總覽](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault)。
- 閱讀[SQL Database 和 Azure synapse 的透明資料加密](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql)。
- 瞭解[使用客戶管理的金鑰進行 AZURE SQL 透明資料加密](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-byok-azure-sql)。

## <a name="best-practice-protect-vms-with-antimalware"></a>最佳做法：使用反惡意程式碼保護 Vm

特別是較舊的 Azure 已移轉 VM，可能沒有安裝適當層級的反惡意程式碼軟體。 Azure 提供免費的端點解決方案，可協助保護 VM 不受病毒、間諜軟體及其他惡意程式碼的威脅。

- 適用於 Azure 的 Microsoft Antimalware 會在已知的惡意或垃圾軟體嘗試自行安裝時產生警示。
- 它是能在無人為介入的情況下於背景中執行的單一代理程式解決方案。
- 在 Azure 資訊安全中心中，您可以輕鬆地識別沒有執行 endpoint protection 的 Vm，並視需要安裝 Microsoft 反惡意程式碼。

  ![Vm 反惡意程式碼 Vm 的反惡意程式碼 ](./media/migrate-best-practices-security-management/antimalware.png)
   _。_

**瞭解更多資訊：**

- 瞭解[適用于 Azure 雲端服務和虛擬機器的 Microsoft Antimalware](https://docs.microsoft.com/azure/security/fundamentals/antimalware)。

## <a name="best-practice-secure-web-apps"></a>最佳做法：保護 web 應用程式

已移轉的 Web 應用程式會面臨幾個問題：

- 大部分的舊版 Web 應用程式，在設定檔中通常都會有機密資訊。 包含這類資訊的檔案可能會在備份應用程式，或將應用程式程式碼簽入或移出原始檔控制時，出現安全性問題。
- 此外，當您遷移位於 VM 中的 web 應用程式時，您可能會將該機器從內部部署網路和受防火牆保護的環境移至面向網際網路的環境。 確定自己已設定能執行與內部部署保護資源相同工作的解決方案。

Azure 能提供數個解決方案：

- **Azure Key Vault：** 現今，web 應用程式開發人員正在採取步驟，以確保機密資訊不會洩漏這些檔案。 保護資訊的其中一個方法，是將它從檔案中擷取出來，並置於 Azure Key Vault 中。
  - 您可以使用 Key Vault 來集中儲存應用程式秘密，並控制其散發。 它可以避免在應用程式檔中儲存安全性資訊的需要。
  - 應用程式可以使用 Uri 安全地存取保存庫中的資訊，而不需要自訂程式碼。
  - Azure Key Vault 可讓您透過 Azure 安全性控制項來鎖定存取，以及順暢地實作「輪替金鑰」。 Microsoft 並無法查看或擷取您的資料。
- **Azure App Service 環境：** 如果您遷移的應用程式需要額外的保護，請考慮新增 App Service 環境和 Web 應用程式防火牆來保護應用程式資源。
  - Azure App Service 環境提供完全隔離且專用的環境，以執行 Windows 和 Linux web 應用程式、docker 容器、行動應用程式及函式應用程式等應用程式。
  - 這適用于非常大規模的應用程式，需要隔離和安全的網路存取，或具有高記憶體使用率。
- **Web 應用程式防火牆：** Azure 應用程式閘道的功能，可提供 web 應用程式的集中式保護。
  - 它能在無需後端程式碼修改的情況下保護 Web 應用程式。
  - 它會在應用程式閘道背後同時保護多個 web 應用程式。
  - 您可以使用 Azure 監視器來監視 Web 應用程式防火牆，並將其整合到 Azure 資訊安全中心。

  ![Azure Key Vault 保護 web 應用程式 ](./media/migrate-best-practices-security-management/web-apps.png)
   _。_

**瞭解更多資訊：**

- 閱讀[Azure Key Vault 的總覽](https://docs.microsoft.com/azure/key-vault/general/overview)。
- 深入瞭解[Web 應用程式防火牆](https://docs.microsoft.com/azure/application-gateway/waf-overview)。
- 閱讀[App Service 環境的簡介](https://docs.microsoft.com/azure/app-service/environment/intro)。
- 瞭解如何[設定 web 應用程式以從 Key Vault 讀取秘密](https://docs.microsoft.com/azure/key-vault/tutorial-web-application-keyvault)。
- 深入瞭解[Web 應用程式防火牆](https://docs.microsoft.com/azure/application-gateway/waf-overview)。

## <a name="best-practice-review-subscriptions-and-resource-permissions"></a>最佳做法：審查訂閱和資源許可權

在您移轉工作負載並在 Azure 中執行它們的同時，具有工作負載存取權的人員也會四處移動。 您的安全性小組應該定期檢閱針對您 Azure 租用戶和資源群組的存取權。 Azure 有一些供應項目可供進行身分識別管理和存取控制安全性，其中包括角色型存取控制 (RBAC) 以針對存取 Azure 資源的權限進行授權。

- RBAC 會針對安全性主體指派存取權限。 安全性主體代表使用者、群組 (一組使用者) 、應用程式和服務所使用的服務主體 (身分識別) 以及受控識別 (Azure Azure Active Directory 自動管理的) 身分識別。
- RBAC 可以將角色指派給安全性主體 (例如擁有者、參與者及讀者)，以及能定義角色可執行之作業的角色定義 (權限的集合)。
- RBAC 也可以設定範圍以設定角色的界線。 範圍可以設定於數個層級上，包括管理群組、訂用帳戶、資源群組或資源。
- 請確定具有 Azure 存取權的系統管理員只能存取您想要允許的資源。 如果 Azure 中預先定義的角色不夠細微，您可以建立自訂角色以區分並限制存取權限。

  ![存取控制 ](./media/migrate-best-practices-security-management/subscription.png)
   _存取控制-IAM。_

**瞭解更多資訊：**

- [關於](https://docs.microsoft.com/azure/role-based-access-control/overview) RBAC。
- [了解](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)如何使用 RBAC 和 Azure 入口網站來管理存取權。
- 深入瞭解[自訂角色](https://docs.microsoft.com/azure/role-based-access-control/custom-roles)。

## <a name="best-practice-review-audit-and-security-logs"></a>最佳做法：審查 audit 和 security 記錄

Azure Active Directory (Azure AD) 提供會在 Azure 監視器中顯示的活動記錄。 記錄會擷取在 Azure 租用戶中執行的作業、其發生的時間，以及執行它們的人員。

- 稽核記錄會顯示租用戶中的工作歷程記錄。 登入活動記錄會顯示執行該工作的人員。
- 安全性報告的存取方式取決於您的 Azure AD 授權。 在 [免費] 和 [基本] 中，您會取得有風險的使用者和登入清單。在 premium 1 和 premium 2 版本中，您會取得基礎事件資訊。
- 您可以將活動記錄路由傳送到各種端點，以進行長期保留並取得資料見解。
- 請養成經常檢閱記錄的習慣，或與您的安全性資訊與事件管理 (SIEM) 工具整合以自動檢閱異常狀況。 如果您不是使用 premium 1 或2，就必須自行進行大量分析或使用 SIEM 系統。 分析包含尋找具風險的登入和事件，以及其他使用者攻擊模式。

  ![使用者和群組 ](./media/migrate-best-practices-security-management/azure-ad.png)
   _Azure AD 使用者和群組。_

**瞭解更多資訊：**

- 深入瞭解[Azure 監視器中的 Azure AD 活動記錄](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-activity-logs-azure-monitor)。
- 瞭解如何[在 Azure AD 入口網站中審核活動報告](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-audit-logs)。

## <a name="best-practice-evaluate-other-security-features"></a>最佳做法：評估其他安全性功能

Azure 提供能提供進階安全性選項的其他安全性功能。 這些最佳做法有一部分需要附加元件授權和進階選項。

- ** (AU) 執行 Azure AD 管理單位。** 使用基本的 Azure 存取控制來委派系統管理工作以支援人員，可能會是一件相當困難的事。 給予支援人員存取權以管理 Azure AD 中的所有群組，對組織的安全性而言可能不是理想的方法。 使用 AU 可讓您以和內部部署組織單位 (OU) 類似的方式，將 Azure 資源隔離在容器內。 若要使用 AU，AU 系統管理員必須擁有進階 Azure AD 授權。 [深入了解](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-administrative-units)。
- **使用多重要素驗證。** 如果您有進階 Azure AD 授權，您可以在系統管理員帳戶上啟用並強制執行多重要素驗證。 網路釣魚是用來入侵帳戶認證的最常見方式。 當不良執行者擁有系統管理員帳戶認證之後，便沒有任何方法可以阻止他們進行會造成嚴重影響的動作 (例如刪除您所有的資源群組)。 您可以用數種方式建立多重要素驗證，包括電子郵件、驗證器應用程式和電話簡訊。 身為系統管理員，您可以選取最不具侵入性的選項。 多重要素驗證會與威脅分析和條件式存取原則整合，以隨機要求多重要素驗證挑戰回應。 深入了解[安全性指引](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication-security-best-practices)，以及[如何設定多重要素驗證](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication-security-best-practices)。
- **實作條件式存取。** 在大部分中小型組織中，Azure 系統管理員和支援小組可能位於單一地理位置。 在此情況下，大部分的登入都會來自相同的區域。 如果這些位置的 IP 位址都相當固定，您應該不會看見系統管理員從這些區域以外的地方進行登入。 即使遠端不良動作專案會危害系統管理員的認證，您也可以執行與多重要素驗證結合的條件式存取等安全性功能，以防止從遠端位置或來自隨機 IP 位址的詐騙位置進行登入。 若要深入瞭解[條件式存取](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)，請參閱 Azure AD 中的條件式存取的[最佳做法](https://docs.microsoft.com/azure/active-directory/conditional-access/best-practices)。
- **審查企業應用程式許可權。** 隨著時間的經過，系統管理員可能會習慣直接選取來自 Microsoft 和協力廠商的連結，而未留意到該動作會對組織帶來什麼影響。 這些連結可能會顯示能將權限指派給 Azure 應用程式的同意畫面，且可能會允許讀取 Azure AD 資料的存取權，甚至是管理整個 Azure 訂用帳戶的完整存取權。 您應該定期檢查您的系統管理員和使用者允許存取 Azure 資源的應用程式。 請確定這些應用程式只有必要的許可權。 此外，每季或半年您可以透過電子郵件將使用者連結至應用程式頁面，讓他們知道他們允許存取其組織資料的應用程式。 [深入瞭解](https://docs.microsoft.com/azure/active-directory/manage-apps/application-types)應用程式類型，以及[如何控制](https://docs.microsoft.com/azure/active-directory/manage-apps/remove-user-or-group-access-portal)Azure AD 中的應用程式指派。

## <a name="managed-migrated-workloads"></a>管理已移轉的工作負載

在本節中，我們將會建議適用於 Azure 管理的一些最佳做法，包括：

- [管理資源](#best-practice-name-resource-groups)： Azure 資源群組和資源的最佳作法，包括智慧型命名、防止意外刪除、管理資源許可權，以及有效的資源標記。
- [使用藍圖](#best-practice-implement-blueprints)：快速瞭解如何使用藍圖來建立和管理您的部署環境。
- [審核架構](#best-practice-review-azure-reference-architectures)：當您建立遷移後部署時，請查看要從中學習的範例 Azure 架構。
- [設定管理群組](#best-practice-manage-resources-with-azure-management-groups)：如果您有多個訂用帳戶，您可以將其收集到管理群組，並將治理設定套用到這些群組。
- [設定存取原則](#best-practice-deploy-azure-policy)：將合規性原則套用至您的 Azure 資源。
- [實行 BCDR 策略](#best-practice-implement-a-bcdr-strategy)：結合商務持續性和嚴重損壞修復 (BCDR) 策略，讓資料保持安全、您的環境彈性，以及在發生中斷時啟動和執行資源。
- [管理 vm](#best-practice-use-managed-disks-and-availability-sets)：將 vm 分組到可用性群組，以提供恢復功能和高可用性。 使用受控磁碟來輕鬆進行 VM 磁碟和儲存體管理。
- [監視資源使用](#best-practice-monitor-resource-usage-and-performance)方式：啟用適用于 Azure 資源的診斷記錄、建立警示和操作手冊以進行主動式疑難排解，以及使用 Azure 儀表板來一致地查看您的部署健康情況和狀態。
- [管理支援和更新](#best-practice-manage-updates)：瞭解您的 Azure 支援方案和如何實行，取得讓 vm 保持在最新狀態的最佳做法，並將處理常式放在進行變更管理。

## <a name="best-practice-name-resource-groups"></a>最佳做法：命名資源群組

透過確保資源群組具備有意義的名稱，讓系統管理員和支援小組能輕鬆辨識及瀏覽，將能大幅改善生產力與效率。

- 我們建議遵循 Azure 命名慣例。
- 如果您會使用 Azure AD Connect 將內部部署 Active Directory 同步至 Azure AD，請考慮使內部部署安全性群組的名稱與 Azure 中資源群組的名稱相符。

  ![命名 ](./media/migrate-best-practices-security-management/naming.png)
   _資源群組命名。_

**瞭解更多資訊：**

- 瞭解[建議的命名慣例](../../ready/azure-best-practices/naming-and-tagging.md)。

## <a name="best-practice-implement-delete-locks-for-resource-groups"></a>最佳做法：執行資源群組的刪除鎖定

您最不想遇到的情況，便是資源群組被意外刪除而消失不見。 我們建議您實作刪除鎖定來避免發生此情況。

  ![刪除鎖定 ](./media/migrate-best-practices-security-management/locks.png) _刪除鎖定。_

**瞭解更多資訊：**

- 瞭解[鎖定資源以防止非預期的變更](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources)。

## <a name="best-practice-understand-resource-access-permissions"></a>最佳做法：瞭解資源存取權限

訂用帳戶擁有者會具有訂用帳戶中所有資源群組和資源的存取權。

- 請謹慎地將這個貴重的權限指派給其他人員。 了解指派這些權限類型的後果，將能大幅協助保持環境的安全及穩定性。
- 請確定您將資源置於適當的資源群組中：
  - 將具有類似生命週期的資源配對在一起。 在理想情況下，您應該不需要在刪除整個資源群組時移動某個資源。
  - 支援某個功能或工作負載的資源應該被放置在一起，以簡化管理工作。

**瞭解更多資訊：**

- 瞭解如何[組織訂閱和資源群組](https://azure.microsoft.com/blog/organizing-subscriptions-and-resource-groups-within-the-enterprise)。

## <a name="best-practice-tag-resources-effectively"></a>最佳做法：有效標記資源

僅使用與資源相關的資源群組名稱，通常沒辦法提供足夠的中繼資料來有效實作各種機制，例如訂用帳戶之內的內部帳單或管理。

- 作為最佳做法，您應該使用 Azure 標記來新增可查詢及報告的有用中繼資料。
- 標記能提供搭配您所定義的屬性，以邏輯方式組織資源的方式。 標記可以直接套用到資源群組或資源。
- 標記可以套用到資源群組或是個別的資源之上。 資源群組標記不會被群組中的資源所繼承。
- 您可以使用 PowerShell 或 Azure 自動化來將標記自動化，您也可以標記個別的群組和資源。 -標記方法或自助服務。 如果您具有要求或變更管理系統，則可以輕鬆地在要求中使用該資訊，以填入公司特定的資源標記。

  ![標記 ](./media/migrate-best-practices-security-management/tagging.png)
   _標記。_

**瞭解更多資訊：**

- 瞭解[標記和標記限制](https://docs.microsoft.com/azure/azure-resource-manager/management/tag-resources)。
- [請參閱 PowerShell 和 CLI 範例來設定標記，並將標記從資源群組套用至其資源](https://docs.microsoft.com/azure/azure-resource-manager/management/tag-resources#powershell)。
- [閱讀](https://www.azurefieldnotes.com/2016/07/18/azure-resource-tagging-best-practices) \(英文\) Azure 標記最佳做法。

## <a name="best-practice-implement-blueprints"></a>最佳做法：執行藍圖

就像藍圖可讓工程師和架構師草擬專案的設計參數，Azure 藍圖讓雲端架構設計人員和中央 IT 小組定義一組可重複使用的 Azure 資源，以符合組織的標準、模式和需求。 透過 Azure 藍圖，開發小組可快速地建置及建立符合組織合規性要求，且包含一組內建元件 (例如網路) 的新環境，以加速開發和交貨。

- 使用藍圖來協調資源群組、Azure Resource Manager 範本，以及原則和角色指派的部署。
- 藍圖會儲存在全域散佈的 Azure Cosmos DB 中。 藍圖物件會複寫至多個 Azure 區域。 複寫可針對藍圖提供低延遲、高可用性且一致的存取，無論藍圖部署資源的區域為何。

**瞭解更多資訊：**

- [閱讀](https://docs.microsoft.com/azure/governance/blueprints/overview)藍圖的相關資訊。
- 回顧[在醫療保健中加速 AI 的範例藍圖](https://azure.microsoft.com/blog/customizing-azure-blueprints-to-accelerate-ai-in-healthcare)。

## <a name="best-practice-review-azure-reference-architectures"></a>最佳做法：查看 Azure 參考架構

在 Azure 中建置安全、可擴充且可管理的工作負載，可能會是一件令人望之卻步的工作。 由於變更會持續發生，因此在顧及各種不同功能之下維持最佳化的環境，可能會相當困難。 在設計及移轉工作負載時，擁有可從中學習的參考可能會很有幫助。 Azure 和 Azure 合作夥伴已針對各種環境類型建置數個範例參考架構。 這些範例是設計來提供各種想法，以供您從中學習並將它作為建置範本。

參考架構會依案例編排。 其中包含有關管理、可用性、擴充性和安全性的最佳做法和建議。 Azure App Service 環境提供完全隔離且專用的環境，以執行 Windows 和 Linux web 應用程式、docker 容器、行動應用程式和功能等應用程式。 App Service 能將 Azure 的功能新增到您的應用程式，其中包括安全性、負載平衡、自動調整和自動化管理。 您也可以利用它的 DevOps 功能，例如來自 Azure DevOps 和 GitHub 的持續部署、套件管理、預備環境、自訂網域和 SSL 憑證。 App Service 適用于需要隔離和安全網路存取的應用程式，以及使用大量記憶體和其他需要調整之資源的應用程式。

**瞭解更多資訊：**

- 瞭解[Azure 參考架構](https://docs.microsoft.com/azure/architecture/reference-architectures)。
- 請參閱[Azure 範例案例](https://docs.microsoft.com/azure/architecture/example-scenario)。

## <a name="best-practice-manage-resources-with-azure-management-groups"></a>最佳做法：使用 Azure 管理群組來管理資源

如果您的組織具有多個訂用帳戶，您便需要為它們管理存取、原則及合規性。 Azure 管理群組提供了訂用帳戶之上的範圍層級。

- 您會將訂用帳戶整理到稱為管理群組的容器中，並將治理條件套用至它們。
- 管理群組中的所有訂用帳戶都會自動繼承管理群組條件。
- 無論具有何種類型的訂用帳戶，管理群組都可提供大規模的企業級管理功能。
- 例如，您可以套用能限制可建立 VM 之區域的管理群組原則。 此原則接著會套用至所有的管理群組、訂用帳戶以及該管理群組下的資源。
- 您可以建置管理群組和訂用帳戶的彈性結構，將資源組織到一個階層中，以便執行統一原則與存取管理。

下圖顯示使用管理群組建立治理階層的範例。

  ![管理群組 ](./media/migrate-best-practices-security-management/management-groups.png) _管理群組。_

**瞭解更多資訊：**

- 深入瞭解將[資源組織成管理群組](https://docs.microsoft.com/azure/governance/management-groups)。

## <a name="best-practice-deploy-azure-policy"></a>最佳做法：部署 Azure 原則

Azure 原則是 Azure 中的一個服務，您可以用來建立、指派和管理原則。

- 原則會對您的資源強制執行不同的規則和效果，讓這些資源保持符合您的公司標準和服務等級協定。
- Azure 原則會評估您的資源，掃描那些不符合您原則規範的資源。
- 例如，您可以建立對環境中的 VM 僅允許特定 SKU 大小的原則。 Azure 原則在建立和更新資源，以及掃描現有資源時，將會評估此設定。
- Azure 會提供一些可供您指派的內建原則，或者您也可以自行建立原則。

  ![Azure 原則 ](./media/migrate-best-practices-security-management/policy.png)
   _Azure 原則。_

**瞭解更多資訊：**

- 閱讀[Azure 原則的總覽](https://docs.microsoft.com/azure/governance/policy/overview)。
- 瞭解如何[建立和管理原則以強制執行合規性](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)。

## <a name="best-practice-implement-a-bcdr-strategy"></a>最佳做法：執行 BCDR 策略

規劃商務持續性和災害復原 (BCDR) 是一件非常重要的工作，您應該在 Azure 移轉規劃過程中完成它。 就法律條款而言，您的合約可包含能因不可抗力 (例如颶風或地震) 而免除義務的不可抗力條款。 但您也有責任確保服務會繼續執行，並在必要時復原，以防嚴重損壞。 您達成此義務的能力，可能會左右您公司的未來發展。

廣義來說，您的 BCDR 策略必須考慮：

- **資料備份：** 如何保護您的資料安全，以便在發生中斷時輕鬆地進行復原。
- 嚴重損壞**修復：** 如何讓您的應用程式在發生中斷時復原並可供使用。

### <a name="set-up-bcdr"></a>設定 BCDR

移轉到 Azure 時，請務必了解雖然 Azure 平台會提供這些內建的復原功能，您仍需自行設計 Azure 部署以利用能提供高可用性、災害復原及備份的 Azure 功能和服務。

- 您的 BCDR 解決方案將取決於您公司的目標，並會被您的 Azure 部署策略所影響。 基礎結構即服務 (IaaS) 和平台即服務 (PaaS) 部署會為 BCDR 帶來不同的挑戰。
- 實作完畢之後，您應該定期測試 BCDR 解決方案，以檢查您的策略是否仍然有效。

### <a name="back-up-an-iaas-deployment"></a>備份 IaaS 部署

在大部分的情況下，內部部署工作負載會在移轉後淘汰，且您必須對用於備份資料的內部部署策略進行擴充或加以取代。 如果您將整個資料中心移轉到 Azure，您必須使用 Azure 技術或協力廠商的整合解決方案來設計並實作完整的備份解決方案。

針對在 Azure IaaS VM 上執行的工作負載，請考慮這些備份解決方案：

- **Azure 備份：** 提供適用于 Azure Windows 和 Linux Vm 的應用程式一致備份。
- **儲存體快照集：** 取得 Blob 儲存體的快照集。

#### <a name="azure-backup"></a>Azure 備份

Azure 備份會建立儲存在 Azure 儲存體中的資料復原點。 Azure 備份可以備份 Azure VM 磁碟，以及 Azure 檔案 (預覽)。 Azure 檔案能提供位於雲端中並可透過 SMB 存取的檔案共用。

您可以使用 Azure 備份以數種方式備份 VM。

- **從 VM 設定直接備份。** 您可以直接從 Azure 入口網站中的 VM 選項，直接搭配 Azure 備份來備份 VM。 您每天可以備份一次 VM，而且您可以視需要還原 VM 磁片。 Azure 備份會 (vss) 取得應用程式感知的資料快照集，且 VM 上不會安裝任何代理程式。
- **復原服務保存庫中的直接備份。** 您可以部署 Azure 備份復原服務保存庫，來備份您的 IaaS VM。 這能提供單一位置以追蹤及管理備份，以及更細微的備份與還原選項。 備份一天最多三次，並於檔案/資料夾層級執行。 它無法感知應用程式，且不支援 Linux。 使用此方法，在每個想要備份的 VM 上安裝 Microsoft Azure 復原服務 (MARS) 代理程式。
- **保護 VM 以 Azure 備份伺服器。** Azure 備份 server 是免費提供的 Azure 備份。 VM 會備份至本機 Azure 備份伺服器儲存體。 然後將 Azure 備份伺服器備份到保存庫中的 Azure。 備份可感知應用程式，並針對備份頻率和保留期提供完整的細微控制。 您可以在應用層級備份，例如備份 SQL Server 或 SharePoint。

基於安全性，Azure 備份會使用 AES-256 來加密進行中的資料，並透過 HTTPS 將它傳送至 Azure。 在 Azure 中備份的待用資料會使用[Azure 儲存體加密](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)進行加密。

![Azure 備份 ](./media/migrate-best-practices-security-management/iaas-backup.png)
 _Azure 備份。_

**瞭解更多資訊：**

- 瞭解[Azure 備份](https://docs.microsoft.com/azure/backup/backup-overview)服務。
- 規劃[Azure vm 的備份基礎結構](https://docs.microsoft.com/azure/backup/backup-azure-vms-introduction)。

#### <a name="storage-snapshots"></a>儲存體快照集

Azure VM 會以分頁 Blob 的形式儲存在 Azure 儲存體中。

- 快照集會擷取特定時間點的 Blob 狀態。
- 作為 Azure VM 磁碟的替代備份方法，您可以擷取儲存體 Blob 的快照集，然後將它們複製到另一個儲存體帳戶。
- 您可以複製整個 Blob，或使用增量快照複製以僅複製差異變更，並減少儲存空間。
- 為求額外的預防措施，您可以啟用 Blob 儲存體帳戶的虛刪除。 啟用此功能時，系統會將已刪除的 Blob 標示為刪除，但不會立即清除它。 可以在過渡期間還原該 Blob。

**瞭解更多資訊：**

- 瞭解[Azure Blob 儲存體](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)。
- 瞭解如何[建立 blob 快照](https://docs.microsoft.com/azure/storage/blobs/storage-blob-snapshots)集。
- 請參閱 Blob 儲存體備份[的範例案例](https://azure.microsoft.com/blog/microsoft-azure-block-blob-storage-backup)。
- [閱讀](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete)虛刪除的相關資訊。
- [Azure 儲存體中的災害復原和強制容錯移轉 (預覽)](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance)

#### <a name="third-party-backup"></a>協力廠商備份

除此之外，您可以使用協力廠商解決方案來將 Azure VM 和儲存體容器備份到本機儲存體或其他雲端服務提供者。 如需詳細資訊，請參閱[Azure Marketplace 中的備份解決方案](https://azuremarketplace.microsoft.com/marketplace/apps?search=backup&page=1)。

### <a name="set-up-disaster-recovery-for-iaas-applications"></a>設定 IaaS 應用程式的嚴重損壞修復

除了保護資料以外，BCDR 規劃也必須考慮如何讓應用程式和工作負載在發生嚴重損壞時保持可用。 針對在 Azure IaaS Vm 和 Azure 儲存體上執行的工作負載，請考慮下列解決方案：

#### <a name="azure-site-recovery"></a>Azure Site Recovery

Azure Site Recovery 是主要的 Azure 服務，可確保 Azure Vm 可以上線，並在發生中斷時提供 VM 應用程式。

Site Recovery 會將 VM 從主要 Azure 區域複寫到次要 Azure 區域。 發生災害時，您會從主要區域將 VM 容錯移轉到次要區域，並如往常一般地繼續存取它們。 當作業返回正常時，您便可以將 VM 容錯回復到主要區域。

  ![Azure Site Recovery ](./media/migrate-best-practices-security-management/site-recovery.png) _Site Recovery。_

**瞭解更多資訊：**

- 查看[Azure vm 的嚴重損壞修復案例](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-disaster-recovery-guidance)。
- 瞭解如何[在遷移後設定 AZURE VM 的](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-replicate-after-migration)嚴重損壞修復。

## <a name="best-practice-use-managed-disks-and-availability-sets"></a>最佳做法：使用受控磁片和可用性設定組

Azure 會使用可用性設定組來以邏輯方式將 VM 分組在一起，以及將設定組中的 VM 與其他資源隔離。 可用性設定組中的 Vm 會分散到多個具有不同子系統的容錯網域，以防止本機失敗。 Vm 也會分散到多個更新網域，以防止同時重新開機集合中的所有 Vm。

Azure 受控磁片會藉由管理與 VM 磁片相關聯的儲存體帳戶，來簡化 Azure 虛擬機器的磁片管理。

- 盡可能使用受控磁片。 您只需要指定您想要使用的儲存體類型和所需的磁片大小，Azure 就會為您建立並管理磁片。
- 您可以將現有的磁片轉換成受控磁片。
- 您應該在可用性設定組中建立 VM，以取得高復原性和可用性。 當發生計畫或非計畫的中斷時，可用性設定組會確保集合中至少有一個 VM 可供使用。

  ![受控磁片 ](./media/migrate-best-practices-security-management/managed-disks.png)
   _受控磁片。_

**瞭解更多資訊：**

- 閱讀[受控磁片總覽](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview)。
- 瞭解如何[將磁片轉換為受控](https://docs.microsoft.com/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks)。
- 瞭解如何[在 Azure 中管理 Windows vm 的可用性](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability)。

## <a name="best-practice-monitor-resource-usage-and-performance"></a>最佳做法：監視資源使用方式和效能

您將工作負載移轉到 Azure 的原因，可能是為了它強大的擴充能力。 但移動您的工作負載並不表示 Azure 會自動執行調整，而不需要您輸入。 例如：

- 如果您的行銷組織推播新的電視廣告，以提高300% 的流量，這可能會導致網站可用性問題。 您剛移轉完畢的工作負載可能會達到指派的限制並當機。
- 另一個範例則是對您已移轉的工作負載發動的分散式阻斷服務 (DDoS) 攻擊。 在此情況下，您可能不會想要進行調整，而是防止攻擊的來源處達您的資源。

這兩個案例有著不同的解決方式，但都需要您透過使用量和效能監控，對正在發生的情況取得見解。

- Azure 監視器可以協助呈現這些計量，並提供警示、自動調整、事件中樞、Logic Apps 等等的回應。
- 除了 Azure 監視之外，您可以整合自己的協力廠商 SIEM 應用程式來監視 Azure 記錄，以進行稽核和效能事件。

  ![Azure 監視器 ](./media/migrate-best-practices-security-management/monitor.png)
   _Azure 監視器。_

**瞭解更多資訊：**

- 了解 [Azure 監視器](https://docs.microsoft.com/azure/azure-monitor/overview)。
- 取得監視和診斷的[最佳做法](https://docs.microsoft.com/azure/architecture/best-practices/monitoring)。
- 深入瞭解[自動調整。](https://docs.microsoft.com/azure/architecture/best-practices/auto-scaling)
- 瞭解如何將[Azure 資料路由傳送至 SIEM 工具](https://docs.microsoft.com/azure/security-center/security-center-export-data-to-siem)。

## <a name="best-practice-enable-diagnostic-logging"></a>最佳做法：啟用診斷記錄

Azure 資源會產生相當多的記錄計量和遙測資料。

- 根據預設，大部分的資源類型都沒有啟用診斷記錄。
- 透過在資源上啟用診斷記錄，您可以查詢記錄資料，並根據它建置警示和劇本。
- 當您啟用診斷記錄時，每個資源都會有特定的類別集合。 您可以選取一或多個記錄類別，以及記錄資料的儲存位置。 記錄可以被傳送至儲存體帳戶、事件中樞或 Azure 監視器記錄。

![診斷記錄 ](./media/migrate-best-practices-security-management/diagnostics.png)
 _診斷記錄。_

**瞭解更多資訊：**

- 瞭解如何[收集和使用記錄資料](https://docs.microsoft.com/azure/azure-monitor/platform/platform-logs-overview)。
- [了解](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-schema)診斷記錄的支援項目。

## <a name="best-practice-set-up-alerts-and-playbooks"></a>最佳做法：設定警示和操作手冊

在針對 Azure 資源啟用診斷記錄的情況下，您可以開始使用記錄資料來建立自訂警示。

- 當您的監視資料中發現條件時，警示會主動通知您。 您接著便可以在系統使用者注意到這些問題之前解決他們。 您可以針對如計量值、記錄搜尋查詢、活動記錄事件、平台健康情況，以及網站可用性等取得警示。
- 觸發警示時，您可以執行邏輯應用程式腳本。 劇本可協助您針對特定警示自動化並協調回應。 劇本是以 Azure Logic Apps 為基礎。 您可以使用邏輯應用程式範本來建立腳本，或建立您自己的腳本。
- 作為一個簡單的範例，您可以建立會在針對網路安全性群組發生連接埠掃描時觸發的警示。 您可以設定能執行並封鎖掃描來源之 IP 位址的劇本。
- 另一個範例是記憶體流失的應用程式。 當記憶體使用量達到某個程度時，範本便可以回收處理程序。

  ![警示 ](./media/migrate-best-practices-security-management/alerts.png)
   _警示。_

**瞭解更多資訊：**

- 瞭解[警示](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)。
- 瞭解[回應資訊安全中心警示的安全性手冊](https://docs.microsoft.com/azure/security-center/security-center-playbooks)。

## <a name="best-practice-use-the-azure-dashboard"></a>最佳做法：使用 Azure 儀表板

Azure 入口網站是網頁型的統一主控台，可讓您建置、管理及監控所有項目，從簡單 Web 應用程式到複雜的雲端應用程式皆包含在內。 它也包含了自訂的儀表板各種協助工具選項。

- 您可以建立多個儀表板，並與可存取您 Azure 訂用帳戶的其他人共用。
- 透過此共用模型，您的團隊便能檢視 Azure 環境，讓他們可以主動管理雲端中的系統。

  ![Azure 儀表板 ](./media/migrate-best-practices-security-management/dashboard.png)
   _azure 儀表板。_

**瞭解更多資訊：**

- 瞭解如何[建立儀表板](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards)。
- 深入瞭解[儀表板結構](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards-structure)。

## <a name="best-practice-understand-support-plans"></a>最佳做法：瞭解支援方案

在某個時間點，您將必須與支援人員或 Microsoft 支援服務人員進行共同作業。 擁有一系列原則和程序以在災害復原等案例期間提供支援，是一件非常重要的事。 此外，您的系統管理員和支援人員都應該接受訓練以實作那些原則。

- 在 Azure 服務問題影響到您工作負載的罕見情況下，系統管理員應該要知道如何以最適當且有效率的方式向 Microsoft 提交支援票證。
- 請熟悉各種針對 Azure 所提供的支援方案。 其範圍從開發人員實例專用的回應時間，到具有不到15分鐘回應時間的頂級支援。

  ![支援方案 ](./media/migrate-best-practices-security-management/support.png)
   _支援方案。_

**瞭解更多資訊：**

- 閱讀[Azure 支援方案的總覽](https://azure.microsoft.com/support/options)。
- 瞭解[ (sla) 的服務層級協定](https://azure.microsoft.com/support/legal/sla)。

## <a name="best-practice-manage-updates"></a>最佳做法：管理更新

搭配最新的作業系統和軟體更新來將 Azure VM 保持在最新狀態，是一件規模龐大的工作。 因此，能同時顯示所有 VM 以了解個別 VM 所需要得更新，並自動推送那些更新的功能，顯得非常可貴。

- 您可以使用 Azure 自動化中的「更新管理」，針對執行部署于 Azure、內部部署及其他雲端提供者之 Windows 和 Linux 電腦的電腦，管理作業系統更新。
- 使用「更新管理」來快速評估所有代理程式電腦上可用更新的狀態，以及管理更新安裝。
- 您可以直接從 Azure 自動化帳戶啟用 Vm 的更新管理。 您也可以在 Azure 入口網站中從 VM 頁面更新單一 VM。
- 此外，Azure VM 可以向 System Center Configuration Manager 註冊。 接著，您可以將 Configuration Manager 工作負載移轉到 Azure，並從單一 Web 介面進行報告和軟體更新。

  ![VM 更新 ](./media/migrate-best-practices-security-management/updates.png)
   _更新。_

**瞭解更多資訊：**

- 瞭解[Azure 中的更新管理](https://docs.microsoft.com/azure/automation/automation-update-management)。
- 瞭解如何[整合 Configuration Manager 與更新管理](https://docs.microsoft.com/azure/automation/oms-solution-updatemgmt-sccmintegration)。
- 關於 Azure 中 Configuration Manager 的[常見問題集](https://docs.microsoft.com/sccm/core/understand/configuration-manager-on-azure)。

## <a name="implement-a-change-management-process"></a>實作變更管理程序

如同任何生產系統，任何類型的變更都可能會對您的環境產生影響。 變更管理程序能讓使用者需要提交要求才能對生產系統進行變更，這對於已移轉的環境來說是一個相當有價值的額外功能。

- 您可以針對變更管理建置最佳做法架構，以協助系統管理員和支援人員熟悉這個程序。
- 您可以使用 Azure 自動化來協助針對已移轉的工作流程進行組態管理及變更追蹤。
- 強制執行變更管理程序時，您可以使用稽核記錄來將 Azure 變更記錄連結至應該是 (也可以不是) 現有的變更要求。 因此，如果您看見沒有搭配相對應變更要求所做的變更，便可以調查程序中發生了什麼問題。

Azure 在 Azure 自動化中具有變更追蹤解決方案：

- 這個解決方案會追蹤對 Windows 與 Linux 軟體和檔案、Windows 登錄機碼、Windows 服務以及 Linux 精靈所做的變更。
- 在被監視伺服器上所做的變更會被傳送至雲端的 Azure 監視器服務進行處理。
- 會將邏輯套用至接收的資料，且雲端服務會記錄資料。
- 在 [變更追蹤] 儀表板上，您可以輕鬆地查看伺服器基礎結構中所做的變更。

  ![變更管理 ](./media/migrate-best-practices-security-management/change.png)
   _變更管理。_

**瞭解更多資訊：**

- 瞭解[變更追蹤](https://docs.microsoft.com/azure/automation/automation-change-tracking)。
- 瞭解[Azure 自動化功能](https://docs.microsoft.com/azure/automation/automation-intro)。

## <a name="next-steps"></a>後續步驟

檢閱其他最佳做法：

- 移轉後的網路功能[最佳做法](./migrate-best-practices-networking.md)。
- 移轉後的成本管理[最佳做法](./migrate-best-practices-costs.md)。
