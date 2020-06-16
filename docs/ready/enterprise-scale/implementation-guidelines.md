---
title: 實施指導方針
description: 實施指導方針
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: ae3dd2779de25182065f78cd46be74df526a3ad3
ms.sourcegitcommit: d88c1cc3597a83ab075606d040ad659ac4b33324
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/15/2020
ms.locfileid: "84792502"
---
<!-- cSpell:ignore interdomain VMSS -->

# <a name="implementation-guidelines"></a>實施指導方針

本節涵蓋如何開始使用企業級的平臺原生參考，並概述設計目標、目前的設計、常見問題和已知問題。

必須進行兩組活動，才能執行企業級架構。 第一組是企業規模的「必須」為 true。 它包含必須由 Azure Active Directory 系統管理員執行以建立初始設定的活動。 這些是依本質順序排列，而且主要是一次性活動。 第二個集合**包含 [** 檔案] [新增區域] 和 [檔案]  >  **New**  >  **Region** **File**  >  **新**  >  **登陸區域**。 這些是具現化登陸區域所需的重複活動，而且需要使用者輸入來 kickstart 工作流程，以協調 Azure AD 內的資源建立。 若要大規模進行讓，這些活動的關鍵是遵循_基礎結構即程式碼（IaC）_ 的原則，以及部署管線的自動化。

下列部分資料表包含空白資料行。 使用這些資料行來捕捉您環境特有的值和附注。

## <a name="what-must-be-true-for-an-enterprise-scale-landing-zone"></a>企業規模登陸區域必須符合的條件

### <a name="enterprise-agreement-ea-enrollment-and-azure-ad-tenants"></a>Enterprise 合約（EA）註冊和 Azure AD 租使用者

| 活動 | 需要參數 | 企業規模範例設定 |
|---|---|---|
| 1. 設定 EA 系統管理員和通知帳戶。 |                     |                                  |
| 2. 建立部門/商務網域/地理型/組織階層。 |                     |                                  |
| 3. 建立 EA 帳戶並指派預算。 |                                  |
| 4. 如果要從內部部署同步處理身分識別，請為每個 Azure AD 租使用者設定 Azure AD Connect。 |                     |                                  |
| 5. 建立零對 Azure 資源的持續存取權，並透過 Azure AD Privileged Identity Management （PIM）即時存取。    |                     |                                  |

### <a name="management-group-and-subscription"></a>管理群組和訂用帳戶

| 活動                                                                                                                            | 需要參數 | 企業規模範例設定   |
|---------------------------------------------------------------------------------------------------------------------------------------|---------------------|----------------------------------|
| 1. 建立管理群組階層（理想的情況是不超過三或四個層級）。                      |                                  |
| 2. 建立最上層的沙箱管理群組，讓使用者進行 Azure 實驗。                                              |                     |                                  |
| 3. 發佈訂用帳戶布建準則，以及訂用帳戶擁有者的責任（可能是 wiki）。 |                     |                                  |
| 4. 建立平臺管理和全域網路和連線能力資源的管理和連線能力訂閱。  |                     |                                  |
| 5. 設定要搭配平臺持續整合/持續部署管線使用的 Git 存放庫和服務主體。                                            |                     |                                  |
| 6. 使用訂用帳戶和管理群組範圍的 Azure AD PIM，建立自訂角色定義和管理權利。              |                     |                                  |

### <a name="global-networking-and-connectivity"></a>全域網路和連線能力

<!-- markdownlint-disable MD033 -->
<!-- cSpell:ignore vwan vhub -->
<!-- docsTest:ignore "Standard virtual WAN" -->

| 活動 | 需要參數 | 企業規模範例設定 |
|---|---|---|
| 1. 為每個 Azure 區域配置適當的虛擬網路無類別網域間路由（CIDR）範圍，其中將會部署 Azure 虛擬 WAN 虛擬中樞和虛擬網路。 | 每個區域一個 CIDR 範圍 | 北歐：`10.0.0.0/16` <br> 西歐：`10.1.0.0/16` <br> 美國東部：`10.2.0.0/16` |
| 2. 在連接訂用帳戶內建立標準虛擬 WAN。 | 虛擬 WAN 名稱 <br> Azure 區域 | 虛擬 WAN 名稱：`contoso-vwan` <br> Azure 區域：`North Europe` |
| 3. 為每個區域建立虛擬中樞。 請確定每個虛擬中樞至少已部署一個閘道（ExpressRoute 或 VPN）。 | 虛擬 WAN 名稱 <br> 虛擬中樞名稱 <br> 虛擬中樞區域 <br> 虛擬中樞位址空間 <br> ExpressRoute 閘道 <br> VPN 閘道 | 虛擬 WAN：`contoso-vwan` <br> 虛擬中樞區域：`North Europe` <br> 虛擬中樞名稱：`vhub-neu` <br> 虛擬中樞位址空間：`10.0.0.0/16` <br> ExpressRoute 閘道：是（1個縮放單位） <br> VPN 閘道：否 |
| 4. 使用 Azure 防火牆管理員，藉由在每個虛擬中樞內部署 Azure 防火牆來保護虛擬中樞。                                                        | 虛擬中樞名稱                           | 虛擬中樞名稱：`vhub-neu`                       |
| 5. 在連線訂用帳戶內建立必要的防火牆原則，並將其指派給安全的虛擬中樞。                                                 | Azure 防火牆原則名稱 <br> 防火牆原則輸入/輸出規則 | 防火牆原則名稱：`contoso-global-fw-policy` <br> 允許輸出規則`*.microsoft.com`  |
| 6. 使用 Azure 防火牆管理員，確保所有連線的虛擬網路與安全的虛擬中樞都受到 Azure 防火牆的保護。                                                | 虛擬中樞名稱 <br> 網際網路流量-來自虛擬網路的流量 | 虛擬中樞名稱：`vhub-neu` <br> 網際網路流量-來自虛擬網路的流量-透過 Azure 防火牆傳送 |
| 7. 在全域連線訂用帳戶內部署和設定 Azure 私人 DNS 區域。                                                             | 私人 DNS 區功能變數名稱稱               | 私人 DNS 區功能變數名稱稱：`azure.contoso.com` |
| 8. 布建具有私人對等互連的 ExpressRoute 線路。                                                                                                 | [遵循每篇文章的指示](https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager#private) | [遵循每篇文章的指示](https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager#private) |
| 9. 透過 ExpressRoute 線路將內部部署 HQs/Dc 連接到 Azure 虛擬中樞。                                                                                | 授權金鑰 <br> 虛擬中樞名稱 | 授權金鑰：`xxxxxxxx` <br> 虛擬中樞：`vhub-neu` |
| 10. （選擇性）：透過 ExpressRoute 私用對等互連設定加密。                                                                                         | [遵循每篇文章的指示](https://docs.microsoft.com/azure/virtual-wan/vpn-over-expressroute) | [遵循每篇文章的指示](https://docs.microsoft.com/azure/virtual-wan/vpn-over-expressroute) |
| 11. （選擇性）：透過 VPN 將分支連線至虛擬中樞。                                                                                                | [遵循每篇文章的指示](https://docs.microsoft.com/azure/virtual-wan/virtual-wan-site-to-site-portal) | [遵循每篇文章的指示](https://docs.microsoft.com/azure/virtual-wan/virtual-wan-site-to-site-portal) |
| 12. 使用 Nsg 保護虛擬中樞之間的虛擬網路流量。                                                                                                             | 輸入規則 <br> 輸出規則 | 輸入規則 <br> 輸出規則 |
| 13. 當有多個內部部署位置透過 ExpressRoute 連線到 Azure 時，請設定 ExpressRoute 全域延伸來連接內部部署 HQs/Dc。 | [遵循每篇文章的指示](https://docs.microsoft.com/azure/expressroute/expressroute-howto-set-global-reach) | [遵循每篇文章的指示](https://docs.microsoft.com/azure/expressroute/expressroute-howto-set-global-reach) |

<!-- markdownlint-enable MD033 -->

### <a name="security-governance-and-compliance"></a>安全性、治理和合規性

| 活動                                                                                                                                              | 需要參數 | 企業規模範例設定   |
|---------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------|----------------------------------|
| 1. 定義和套用服務允許清單架構，以確保 Azure 服務符合企業安全性和治理需求（請參閱附錄）。 |                     |                                  |
| 2. 建立自訂的角色型存取控制（RBAC）角色定義。                                                                                                              |                     |                                  |
| 3. 啟用 PIM 並探索 Azure 資源，以加速 PIM。                                                             |                     |                                  |
| 4. 使用 PIM 建立僅限 Azure AD 群組來管理資源的 Azure 控制平面。                                                             |                     |                                  |
| 5. 套用 Azure 原則以確保 Azure 服務符合企業需求。                                                             |                     |                                  |
| 6. 定義命名慣例，並透過 Azure 原則加以強制執行。                                                                                       |                     |                                  |
| 7. 在所有範圍建立原則矩陣（例如，針對所有 Azure 服務啟用監視）。                                                             |                     |                                  |
| 8. 套用與網路功能、安全性和監視相關的 Azure 原則（請參閱下面提供的範例原則清單）。                |                     |                                  |

### <a name="platform-management-and-monitoring"></a>平臺管理和監視

| 活動                                                                                                       | 需要參數 | 企業規模範例設定   |
|------------------------------------------------------------------------------------------------------------------|---------------------|----------------------------------|
| 1. 為組織和以資源為中心的視圖建立原則合規性與安全性儀表板。            |                     |                                  |
| 2. 建立平臺密碼（服務主體和自動化帳戶）和金鑰變換的工作流程。     |                     |                                  |
| 3. 選擇性：設定整個組織的虛擬機器（VM）資源庫映射。                                                  |                     |                                  |
| 4. 針對 Azure 監視器記錄檔中的記錄設定長期封存和保留。                                    |                     |                                  |
| 5. 為用來儲存平臺秘密的金鑰保存庫，設定商務持續性和嚴重損壞修復。                                                  |                     |                                  |
| 6. 使用 Azure 防火牆管理員，確保所有連線的虛擬網路對安全的虛擬中樞都受到 Azure 防火牆的保護。 |                     |                                  |

下表提供在管理群組範圍強制執行一般網路功能、安全性和監視控制項的範例 Azure 原則清單。

| 類別   | 原則                                                                                           |
|------------|--------------------------------------------------------------------------------------------------|
| 網路    | 1. （預覽）： Container Registry 應使用虛擬網路服務端點。                   |
|            | 2. 自訂 IP 安全性/網際網路金鑰交換原則必須套用至所有 Azure 虛擬網路閘道連線。    |
|            | 3. App Service 應該使用虛擬網路服務端點（僅限內部應用程式）。                |
|            | 4. Azure VPN 閘道不應使用基本的庫存單位（SKU）。                                                 |
|            | 5. Azure Cosmos DB 應該使用虛擬網路服務端點。                                       |
|            | 6. 在建立虛擬網路時部署網路監看員。                                      |
|            | 7. 事件中樞應使用虛擬網路服務端點。                                       |
|            | 8. 不應使用網路安全性群組來設定閘道子網。                        |
|            | 9. Key Vault 應該使用虛擬網路服務端點。                                       |
|            | 10. 網路介面應停用 IP 轉送。                                              |
|            | 11. 服務匯流排應該使用虛擬網路服務端點。                                    |
|            | 12. SQL Server 應該使用虛擬網路服務端點。                                     |
|            | 13. 儲存體帳戶應使用虛擬網路服務端點。                               |
|            | 14. 子網應該與網路安全性群組相關聯。                                   |
|            | 15. 在 Azure 資訊安全中心中監視未受保護的網路端點。                          |
| 安全性   | 1. [預覽]： Audit Microsoft Monitoring Agent 部署-未列出的 VM 映射（OS）。                         |
|            | 2. [預覽]：在 VMSS 中審核 Microsoft Monitoring Agent 部署-未列出 VM 映射（OS）。                 |
|            | 3. [預覽]： Audit Azure 監視器 Log Analytics 代理程式部署-未列出的 VM 映射（OS）。                      |
|            | 4. [預覽]：在 VMSS 中 Audit Azure 監視器 Log Analytics 代理程式部署-未列出 VM 映射（OS）。              |
|            | 5. [預覽]： Azure 監視器 Log Analytics 工作區用於 VM-報告不相符。                             |
|            | 6. [預覽]：部署 Linux Vm 的 Microsoft Dependency Agent。                                              |
|            | 7. [預覽]：部署 Windows Vm 的 Microsoft Dependency Agent。                                            |
|            | 8. [預覽]：部署 Azure 監視器適用于 Linux Vm 的 Log Analytics 代理程式。                                           |
|            | 9. [預覽]：為 Windows Vm 部署 Azure 監視器 Log Analytics 代理程式。                                         |
|            | 10. 活動記錄至少應保留一年。                                        |
|            | 11. Audit 診斷設定。                                                                     |
|            | 12. Azure 監視器記錄檔設定檔應該收集類別 `write` 、和的記錄檔 `delete` `action` 。 |
|            | 13. Azure 監視器應該從所有區域收集活動記錄。                                  |
|            | 14. 必須部署 Azure 監視器解決方案的「安全性和 audit」。                                 |
|            | 15. Azure 訂用帳戶應具有活動記錄的記錄設定檔。                               |
|            | 16. 將 batch 帳戶的診斷設定部署至事件中樞。                                    |
|            | 17. 將 batch 帳戶的診斷設定部署到 Azure 監視器 Log Analytics 工作區。                      |
|            | 18. 將 Data Lake Analytics 的診斷設定部署至事件中樞。                              |
|            | 19. 將 Data Lake Analytics 的診斷設定部署 Azure 監視器 Log Analytics 工作區。                |
|            | 20. 將 Data Lake Storage gen1 的診斷設定部署至事件中樞。                           |
|            | 21. 將 Data Lake Storage gen1 的診斷設定部署至 Azure 監視器 Log Analytics 工作區。             |
|            | 22. 將事件中樞的診斷設定部署至事件中樞。                                        |
|            | 23. 將事件中樞的診斷設定部署到 Azure 監視器 Log Analytics 工作區。                          |
|            | 24. 將 Key Vault 的診斷設定部署 Azure 監視器 Log Analytics 工作區。                          |
|            | 25. 將 Logic Apps 的診斷設定部署至事件中樞。                                       |
|            | 26. 將 Logic Apps 的診斷設定部署至 Azure 監視器 Log Analytics 工作區。                         |
|            | 27. 部署網路安全性群組的診斷設定。                                       |
|            | 28. 將搜尋服務的診斷設定部署至事件中樞。                                  |
|            | 29. 將搜尋服務的診斷設定部署到 Azure 監視器 Log Analytics 工作區。                    |
|            | 30. 將服務匯流排的診斷設定部署至事件中樞。                                      |
|            | 31. 將服務匯流排的診斷設定部署 Azure 監視器 Log Analytics 工作區。                        |
|            | 32. 將串流分析的診斷設定部署至事件中樞。                                 |
|            | 33. 將串流分析的診斷設定部署到 Azure 監視器 Log Analytics 工作區。                   |
|            | 34。 Azure 監視器 Log Analytics 代理程式應該安裝在 VM 擴展集上。                    |
|            | 35。 Azure 監視器 Log Analytics 代理程式應該安裝在 Vm 上。                              |
| 監視 | 36. [預覽]： Audit Microsoft Dependency Agent 部署-未列出的 VM 映射（OS）。                        |
|            | 37. [預覽]：在 VMSS 中審核 Microsoft Dependency Agent 部署-未列出 VM 映射（OS）。                |
|            | 38. [預覽]： Audit Azure 監視器 Log Analytics 代理程式部署-未列出的 VM 映射（OS）。                     |
|            | 39. [預覽]：在 VMSS 中 Audit Azure 監視器 Log Analytics 代理程式部署-未列出 VM 映射（OS）。             |
|            | 40. [預覽]： Audit Azure 監視器 VM 的 Log Analytics 工作區-報告不相符。                            |
|            | 41. [預覽]：部署 Linux Vm 的 Microsoft Dependency Agent。                                             |
|            | 42. [預覽]：部署 Windows Vm 的 Microsoft Dependency Agent。                                           |
|            | 43. [預覽]：部署 Azure 監視器適用于 Linux Vm 的 Log Analytics 代理程式。                                          |
|            | 44. [預覽]：為 Windows Vm 部署 Azure 監視器 Log Analytics 代理程式。                                        |
|            | 45。活動記錄至少應保留一年。                                        |
|            | 46. Audit 診斷設定。                                                                     |
|            | 47。 Azure 監視器記錄檔設定檔應收集類別 `write` 、和的記錄檔 `delete` `action` 。 |
|            | 48。 Azure 監視器應從所有區域收集活動記錄。                                  |
|            | 49。 `security and audit` 必須部署 Azure 監視器解決方案。                                 |
|            | 50。 Azure 訂用帳戶應具有活動記錄的記錄設定檔。                               |
|            | 51. 將 batch 帳戶的診斷設定部署至事件中樞。                                    |
|            | 52. 將 batch 帳戶的診斷設定部署到 Azure 監視器 Log Analytics 工作區。                      |
|            | 53. 將 Data Lake Analytics 的診斷設定部署至事件中樞。                              |
|            | 54. 將 Data Lake Analytics 的診斷設定部署 Azure 監視器 Log Analytics 工作區。                |
|            | 55. 將 Data Lake Storage gen1 的診斷設定部署至事件中樞。                           |
|            | 56. 將 Data Lake Storage gen1 的診斷設定部署至 Azure 監視器 Log Analytics 工作區。             |
|            | 57. 將事件中樞的診斷設定部署至事件中樞。                                        |
|            | 58. 將事件中樞的診斷設定部署到 Azure 監視器 Log Analytics 工作區。                          |
|            | 59. 將 Key Vault 的診斷設定部署 Azure 監視器 Log Analytics 工作區。                          |
|            | 60. 將 Logic Apps 的診斷設定部署至事件中樞。                                       |
|            | 61. 將 Logic Apps 的診斷設定部署 Azure 監視器 Log Analytics 工作區。                         |
|            | 62. 部署網路安全性群組的診斷設定。                                       |
|            | 63. 將搜尋服務的診斷設定部署至事件中樞。                                  |
|            | 64. 將搜尋服務的診斷設定部署到 Azure 監視器 Log Analytics 工作區。                    |
|            | 65. 將服務匯流排的診斷設定部署至事件中樞。                                      |
|            | 66. 將服務匯流排的診斷設定部署 Azure 監視器 Log Analytics 工作區。                        |
|            | 67. 將串流分析的診斷設定部署至事件中樞。                                 |
|            | 68. 將串流分析的診斷設定部署到 Azure 監視器 Log Analytics 工作區。                   |
|            | 69。 Azure 監視器 Log Analytics 代理程式應該安裝在 VM 擴展集上。                    |
|            | 70。 Azure 監視器 Log Analytics 代理程式應該安裝在 Vm 上。                              |
| Key Vault  | 71. [預覽]：管理允許的憑證金鑰類型。                                              |
|            | 72. [預覽]：管理橢圓曲線密碼編譯憑證的允許曲線名稱。           |
|            | 73. [預覽]：管理憑證存留期動作觸發程式。                                       |
|            | 74. [預覽]：管理憑證有效期間。                                                |
|            | 75. [預覽]：管理 nonintegrated 憑證授權單位單位所發出的憑證。                                 |
|            | 76. [預覽]：管理整合式憑證授權單位單位所發行的憑證。                                    |
|            | 77. [預覽]：管理在指定的到期天數內的憑證。      |
|            | 78. [預覽]：管理 RSA 憑證的最小金鑰大小。                                      |
|            | 79. 將 Key Vault 的診斷設定部署至事件中樞。                                        |
|            | 80。應該啟用 Key Vault 中的診斷記錄。                                               |
|            | 81. 啟用 Key Vault 的虛刪除。                                                             |

## <a name="file--gt-new--gt-region-and-file--gt-new--gt-landing-zone"></a>檔案 **- &gt; 新增- &gt; 區域**和檔案 **- &gt; 新增- &gt; 登陸區域**

### <a name="file--gt-new--gt-region"></a>檔案- &gt; 新增- &gt; 區域

1. 在連線訂用帳戶內，于現有的 Azure 虛擬 WAN 中建立新的虛擬中樞。

2. 藉由在虛擬中樞內部署 Azure 防火牆，並將現有或新的防火牆原則連結至 Azure 防火牆，以 Azure 防火牆管理員保護虛擬中樞。

3. 使用 Azure 防火牆管理員，確保所有連線的虛擬網路對安全的虛擬中樞都受到 Azure 防火牆的保護。

4. 使用 ExpressRoute 或透過 VPN，將虛擬中樞連線到內部部署。

5. （選擇性）透過 ExpressRoute 私用對等互連設定加密。

6. 使用網路安全性群組來保護跨虛擬中樞的虛擬網路流量。

### <a name="file--gt-new--gt-landing-zone-for-applications-and-workloads"></a>檔案- &gt; 新增- &gt; 應用程式和工作負載的登陸區域

1. 建立訂用帳戶，並將要求者指派為訂用帳戶擁有者。

2. 指派管理群組階層中的訂用帳戶。

3. 設定預算和警示通知。

4. 設定安全性連絡人的電子郵件地址和電話號碼。

5. 建立訂用帳戶的 Azure AD 群組（n）擁有者、讀者、參與者等等。

6. 建立 Azure AD 群組的 Azure AD PIM 權利。

7. 如果需要虛擬網路連線能力，請布建虛擬網路 CIDR 範圍。

8. 使用區域虛擬中樞對等互連虛擬網路。

9. 布建必要的共用服務（例如網域控制站）。

10. 為訂用帳戶範圍指派必要的原則，這是由組織所定義的原則矩陣所規定。

\*沙箱訂閱的選擇性。

下表提供在訂用帳戶範圍強制執行一般企業控制項的範例 Azure 原則清單。

| 類別          | 原則                                                                                                 |
|-------------------|--------------------------------------------------------------------------------------------------------|
| 資源和標記 | 1. 允許的位置                                                                                   |
|                   | 2. 允許的資源群組位置                                                               |
|                   | 3. 允許的資源類型                                                                              |
|                   | 4. 審核自訂 RBAC 規則的使用方式                                                                    |
|                   | 5. 自訂訂用帳戶擁有者角色不應存在                                                    |
|                   | 6. 不允許的資源類型                                                                          |
|                   | 7. 將標記新增至資源群組                                                                        |
|                   | 8. 將標記新增至資源                                                                              |
|                   | 9. 從資源群組繼承標記（如果遺失）                                                    |
|                   | 10. 需要 tag 和其值                                                                          |
|                   | 11. 在資源群組上需要標籤及其值                                                       |
| 計算           | 12. 允許的 VM Sku                                                                       |
|                   | 13. 未設定嚴重損壞修復的情況下，請審核 Vm                                        |
|                   | 14. 審查不使用受控磁片的 Vm                                                            |
|                   | 15. 部署 Windows Server 的預設 Microsoft 基礎結構即服務（IaaS）反惡意程式碼擴充功能                              |
|                   | 16. 應啟用 VM 擴展集中的診斷記錄                                    |
|                   | 17. 應將 Azure Microsoft Antimalware 設定為自動更新保護簽章 |
|                   | 18. 適用于 Windows 的 Microsoft Antimalware 擴充功能應該部署在 Windows 伺服器上                          |
|                   | 19. 只應安裝已核准的 VM 擴充功能                                                    |
|                   | 20. 在 VM 擴展集上需要自動 OS 映射修補                                  |
|                   | 21. 應將未附加的磁片加密                                                               |
|                   | 22. 網路介面不應具有公用 Ip                                                      |
|                   | 23. Vm 應連線到已核准的虛擬網路                                |
|                   | 24. 虛擬網路應使用指定的虛擬網路閘道                                      |
