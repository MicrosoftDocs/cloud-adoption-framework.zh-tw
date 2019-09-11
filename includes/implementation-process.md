<!-- TEMPLATE FILE - DO NOT ADD METADATA -->
<!-- markdownlint-disable MD002 MD041 -->

## <a name="dependent-decisions"></a>相依決策

下列決策來自雲端治理小組以外的團隊。 每項決策的實作都將來自相同的小組。 不過，雲端治理小組會負責執行解決方案，以驗證是否一致地套用這些部署。

### <a name="identity-baseline"></a>身分識別基準

身分識別基準是所有治理工作的基本起始點。 嘗試施行治理之前，必須先建立身分識別。 已建立的身分識別策略隨後將會由治理解決方案強制執行。
在此治理指南中，身分識別管理小組會實行 **[目錄同步](/azure/architecture/cloud-adoption/decision-guides/identity/overview#directory-synchronization)** 處理模式：

- Azure Active Directory (Azure AD) 會使用在公司移轉至 Office 365 期間實作的目錄同步處理或「相同登入」提供 RBAC。 如需實作指引，請參閱 [Azure AD 整合的參考架構](/azure/architecture/reference-architectures/identity/azure-ad)。
- Azure AD 租用戶也會對部署至 Azure 的資產進行驗證和存取的控管。

在治理 MVP 中，治理小組會透過訂用帳戶治理工具強制執行已複寫租使用者的應用程式，本文稍後將討論此功能。 在未來的反復專案中，治理小組也可以在 Azure AD 中強制執行豐富的工具，以擴充這項功能。

### <a name="security-baseline-networking"></a>安全性基準：網路功能

軟體定義網路是安全性基準的重要初始層面。 要建立治理 MVP，有賴安全性管理小組擬定早期決策，以定義安全設定網路的方式。

由於缺乏需求，IT 安全性會安全地扮演它，而且需要 **[雲端 DMZ](/azure/architecture/cloud-adoption/decision-guides/software-defined-network/cloud-dmz)** 模式。 這表示 Azure 部署本身的治理只會略為介入。

- Azure 訂用帳戶可以透過 VPN 連線到現有的資料中心，但必須遵循所有現有的內部部署 IT 治理原則，與不受保護的資源連線相關。 如需關於 VPN 連線的實作指引，請參閱 [VPN 參考架構](/azure/architecture/reference-architectures/hybrid-networking/vpn)。
- 有關於子網路、防火牆和路由的決策，目前均留待每個應用程式/工作負載的潛在客戶決定。
- 發行任何受保護的資料或任務關鍵性工作負載之前，需執行額外的分析。

在此模式中，雲端網路只能透過與 Azure 相容的現有 VPN 連接到內部部署資源。 透過該連線傳送的流量，將視同任何來自非管制區域的流量。 對於內部部署 Edge 裝置可能需要額外的考量，以便能安全地處理來自 Azure 的流量。

雲端治理小組已主動邀請網路和 IT 安全性小組的成員進行定期會議，以持續滿足網路需求和風險。

### <a name="security-baseline-encryption"></a>安全性基準：加密

加密是安全性基準專業領域中的另一項基本決策。 由於公司目前尚未儲存任何受保護的資料在雲端中，安全性小組已決定採用主動性較低的加密模式。
此時，建議使用 **[雲端原生模式進行加密](/azure/architecture/cloud-adoption/decision-guides/encryption/overview#key-management)** ，但不需要任何開發小組。

- 目前並未設定關於使用加密的治理需求，因為現行的公司原則並不允許將任務關鍵性或受保護的資料放在雲端中。
- 發行任何受保護的資料或任務關鍵性工作負載之前，需要進行額外的分析。

## <a name="policy-enforcement"></a>原則強制執行

在「部署加速」方面的首要決策，是強制執行的模式。 在此敘述中，治理小組決定要執行 **[自動化強制](/azure/architecture/cloud-adoption/decision-guides/policy-enforcement/overview#automated-enforcement)** 模式。

- Azure 資訊安全中心將可供安全性和身分識別小組用來監視安全性風險。 這兩個小組也可能使用資訊安全中心來識別新的風險，並改善公司原則。
- 所有訂用帳戶中都需要 RBAC，以控管驗證的強制執行。
- Azure 原則將發佈給每個管理群組，並套用至所有訂用帳戶。 但在這個初始治理 MVP 中，強制執行原則的層級將會非常有限。
- 雖然使用了 Azure 管理群組，但階層應該還是相當簡單。
- Azure 藍圖將用來對各個管理群組套用 RBAC 需求、Resource Manager 範本和 Azure 原則，以部署及更新訂用帳戶。

## <a name="applying-the-dependent-patterns"></a>套用相依模式

下列決策代表要透過前述原則強制執行策略來強制執行的模式：

**身分識別基準。** Azure 藍圖會在訂用帳戶層級設定 RBAC 需求，以確定可為所有訂用帳戶設定一致的身分識別。

**安全性基準：網路。** 雲端治理小組會維護 Resource Manager 範本，以便在 Azure 與內部部署 VPN 裝置之間建立 VPN 閘道。 當應用程式小組需要 VPN 連線時，雲端治理小組會透過 Azure 藍圖將閘道 Resource Manager 範本。

**安全性基準：加密.** 此時，此區域不需要強制執行任何原則。 這會在稍後的反復專案中進行。
