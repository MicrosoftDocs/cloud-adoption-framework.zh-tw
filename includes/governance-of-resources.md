<!-- TEMPLATE FILE - DO NOT ADD METADATA -->
<!-- markdownlint-disable MD002 MD041 -->
> [!NOTE]
> 當商務需求改變時，Azure 管理群組可讓您輕鬆重新組織管理階層與訂用帳戶群組指派。 不過，請記住階層中該群組下的所有訂用帳戶，都會繼承原則與套用至管理群組的角色指派。 如果您打算重新指派訂用帳戶之間的管理群組，請確定您已了解可能會造成任何原則和角色指派變更項目。 如需詳細資訊，請參閱 [Azure 管理群組文件](/azure/governance/management-groups)。

### <a name="governance-of-resources"></a>資源治理

一組全域原則和 RBAC 角色將提供基準層級的加強治理。 為了符合雲端治理小組的原則需求，實作治理 MVP 需要完成下列工作：

1. 識別所需的 Azure 原則定義，以強制執行商務需求。 這可能包含使用內建的定義，以及建立新的自訂定義。 為了跟上新發行內建定義的步調，您可以使用內建原則的所有認可 [Atom 摘要](https://github.com/Azure/azure-policy/commits/master/built-in-policies.atom)，以便用於 RSS 摘要。 或者，您也可以檢查 [AzAdvertizer](https://www.azadvertizer.net/)。 
2. 使用這些內建和自訂原則，以及治理 MVP 所需的角色指派，來建立藍圖定義。
3. 透過將藍圖定義指派給所有訂用帳戶，即可全域套用原則和設定。

#### <a name="identify-policy-definitions"></a>識別原則定義

Azure 提供數個內建原則與角色定義，您可以指派給管理群組、訂用帳戶或資源群組。 您可以使用內建的定義來處理許多常見的治理需求。 不過，您也可能需要建立自訂原則定義，以處理特定需求。

自訂原則定義會儲存於管理群組或訂用帳戶，並透過管理群組階層來繼承。 如果管理群組是原則定義的儲存位置，則可將該原則定義指派給該群組的任何子管理群組或訂用帳戶。

由於支援治理 MVP 所需的原則，需要套用至所有目前的訂用帳戶，因此將使用根管理群組中建立的內建定義和自訂定義組合，來實作下列商務需求：

1. 可用的角色指派清單以雲端治理小組授權的一組內建 Azure 角色為限。 這需要[自訂原則定義](https://github.com/azure/azure-policy/tree/master/samples/Authorization/allowed-role-definitions)。
2. 所有資源上都需要下列標記：部門/計費單位、地理位置、資料分類、重要性、SLA、環境、應用程式原型、應用程式及應用程式擁有者。 這可使用 `Require specified tag` 內建定義來處理。
3. 要求資源的 `Application`標記應符合相關資源群組的名稱。 這可以使用「需要標籤及其值」內建定義來處理。

如需定義自訂原則的詳細資訊，請參閱 [Azure 原則文件](/azure/governance/policy/tutorials/create-custom-policy-definition)。 如需自訂原則的指導方針與範例，請參閱 [Azure 原則範例網站](/azure/governance/policy/samples)和相關聯的 [GitHub 存放庫](https://github.com/azure/azure-policy)。

#### <a name="assign-azure-policy-and-rbac-roles-using-azure-blueprints"></a>使用 Azure 藍圖指派 Azure 原則和 RBAC 角色

Azure 原則可以指派於資源群組、訂用帳戶，以及管理群組層級，而且可以併入 [Azure 藍圖](/azure/governance/blueprints/overview)定義中。 雖然此治理 MVP 中定義的原則需求會套用到所有目前的訂用帳戶，但未來的部署很可能需要例外狀況或替代的原則。 如此一來，使用管理群組指派原則，讓所有子訂用帳戶繼承這些指派，可能不具備足夠的彈性支援這些案例。

Azure 藍圖允許指派一致的原則和角色、Resource Manager 範本的應用程式，以及跨多個訂用帳戶部署資源群組。 如同原則定義，藍圖定義會儲存至管理群組或訂用帳戶。 原則定義可透過繼承到管理群組階層中任何子系的方式來取得。

雲端治理小組已決定透過 Azure 藍圖和相關聯的成品，實作跨訂用帳戶強制執行必要的 Azure 原則和 RBAC 指派：

1. 在根管理群組中，建立名為 `governance-baseline` 的藍圖定義。
2. 在藍圖定義中新增下列藍圖成品：
    1. 在根管理群組定義自訂 Azure 原則定義的原則指派。
    2. 由治理 MVP 建立或控管的訂用帳戶，都需要任何群組的資源群組定義。
    3. 由治理 MVP 建立或控管的訂用帳戶，都需要標準角色指派。
3. 發佈藍圖定義。
4. 將 `governance-baseline` 藍圖定義指派給所有訂用帳戶。

如需有關建立和使用藍圖定義 的詳細資訊，請參閱 [Azure 藍圖文件](/azure/governance/blueprints/overview)。

### <a name="secure-hybrid-vnet"></a>安全混合式 VNet

特定訂用帳戶對於內部部署資源經常需要某種程度的存取權。 這在相依資源位在內部部署資料中心的移轉案例或開發案例，是常有的情況。

在雲端環境中完全建立信任之前，務必要嚴密控制和監視內部部署環境與雲端工作負載之間允許的任何通訊，以及保護內部部署網路，防止可能未經授權的雲端式資源存取。 若要支援這些案例，治理 MVP 可新增下列最佳做法：

1. 建立雲端安全混合式 VNet。
    1. [VPN 參考架構](/azure/architecture/reference-architectures/hybrid-networking/vpn)會建立在 Azure 中建立 VPN 閘道的模式和部署模型。
    2. 驗證內部部署安全性和流量管理機制，會將已連線的雲端網路視為不受信任。 雲端中裝載的資源和服務應該只能存取授權的內部部署服務。
    3. 驗證內部部署資料中心中的本機 Edge 裝置可與 [Azure VPN 閘道需求](/azure/vpn-gateway/vpn-gateway-about-vpn-devices)相容，並設定為存取公用網際網路。
    4. 請注意，除了最簡單的工作負載以外，請勿將 VPN 通道視為任何項目的生產環境就緒線路。 除了少數需要內部部署連線的簡單工作負載以外，任何項目都應該使用 Azure ExpressRoute。
1. 在根管理群組中，建立名為 `secure-hybrid-vnet` 的第二個藍圖定義。
    1. 將 VPN 閘道的 Resource Manager 範本作為成品新增至藍圖定義。
    2. 將虛擬網路的 Resource Manager 範本作為成品新增至藍圖定義。
    3. 發佈藍圖定義。
1. 將 `secure-hybrid-vnet` 藍圖定義指派給任何需要內部部署連線的訂用帳戶。 除了 `governance-baseline` 藍圖定義，還應該指派此定義。

IT 安全性與傳統治理小組所引起的最大問題之一，是早期階段的雲端採用有可能使現有資產受損。 上述方法可讓雲端採用小組建置和移轉混合式解決方案，而降低內部部署資產承受的風險。 隨著雲端環境中的信任逐漸提升，後續的演進可能會移除這個暫時性的解決方案。

> [!NOTE]
> 以上是快速建立基準治理 MVP 的起始點。 這只是治理旅程的開端。 隨著公司持續採用雲端，且承受更多來自下列領域的風險，治理將需要進一步演進修正：
>
> - 任務關鍵性工作負載
> - 受保護的資料
> - 成本管理
> - 多重雲端案例
>
> 此外，此 MVP 的特定詳細資料皆以虛構公司的範例旅程為基礎，相關說明請見後續文章。 強烈建議您在依照此最佳做法實作之前，應先閱讀這一系列的其他文章。
