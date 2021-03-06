---
title: 用來執行 Azure 登陸區域的合作夥伴選項
description: 瞭解如何驗證 Azure 登陸區域的合作夥伴實行選項。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/07/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: internal
ms.openlocfilehash: c3999d47b4d85789d332a233cb4c9d8651ff3375
ms.sourcegitcommit: b1217b40301583286a3d05032dbfd7a8e6b83fd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/08/2021
ms.locfileid: "99839007"
---
# <a name="evaluate-a-microsoft-partners-azure-landing-zone"></a>評估 Microsoft 合作夥伴的 Azure 登陸區域

雲端採用架構將雲端採用作為自助活動。 目標是要讓每個小組都能透過標準化的方法來支援採用。 在實務上，您無法假設自助方法對所有採用活動都已足夠。

成功的雲端採用方案通常牽涉到至少一個層級的協力廠商支援。 許多雲端採用工作都需要系統整合者的支援 (SI) 或諮詢合作夥伴，以提供加速雲端採用的服務。 受控服務提供者 (Msp) 透過支援登陸區域和雲端採用來提供議論值，但它們也提供後續採用作業管理支援。 此外，成功的雲端採用工作通常會將一或多個獨立軟體廠商 (ISV) ，其提供可加速雲端採用的軟體型服務。 適用于 SIs、Isv、Msp 及其他 Microsoft 合作夥伴形式的豐富合作夥伴生態系統，已將他們的供應專案對應到雲端採用架構中的特定方法。 當合作夥伴符合此架構的就緒方法時，他們可能會提供自己的 Azure 登陸區域實行選項。

本文提供一組問題，可協助您瞭解合作夥伴的 Azure 登陸區域實行選項的範圍。

> [!IMPORTANT]
> 合作夥伴供應專案和 Azure 登陸區域的執行選項是由合作夥伴所定義，它會根據其廣泛體驗來協助客戶採用雲端。
>
> 合作夥伴可能會選擇在最初的登陸區域實行中省略特定設計區域的實作為。 不過，它們應該可以在每個設計區域的執行時間和方式進行通訊，以及盡可能完成該設計區域的成本範圍。
>
> 其他合作夥伴解決方案可能有足夠的彈性，可支援下列每個問題的多個選項。 您可以使用這些問題，以確保您比較的是合作夥伴供應專案和自助選項。

## <a name="find-a-partner"></a>尋找合作夥伴

如果您需要合作夥伴來執行您的 Azure 登陸區域，請從已核准且符合雲端採用架構的夥伴清單開始。 具體而言，請從已符合 [就緒方法的](https://www.microsoft.com/azure/partners/adopt?filters=ready)合作夥伴著手。

此外，所有 [Azure 專家管理的服務提供者 (msp) ](https://www.microsoft.com/azure/partners/azureexpertmsp?filters=all) 都經過審核，可驗證其傳遞雲端採用架構各方法的能力。 雖然特定的夥伴可能沒有對齊的供應專案，但所有合作夥伴在技術提供期間都已證明一致。

## <a name="validate-a-partner-offer"></a>驗證合作夥伴供應專案

選取合作夥伴之後，請使用本文的其餘部分來引導您驗證合作夥伴供應專案。 每一節都包含要尋找之事項的摘要，以及要詢問合作夥伴的問題清單。 合作夥伴對於這些問題的答案不應視為正確或錯誤。 相反地，問題的設計目的是協助您評估合作夥伴供應專案是否符合您的商務需求。

## <a name="platform-development-velocity"></a>平臺開發速度

如同 [Azure 登陸區域的執行選項](./implementation-options.md)中所述，根據您要如何開發登陸區域，有兩個高階方法可以登陸區域。

**合作夥伴的問題：** 合作夥伴的 Azure 登陸區域解決方案支援下列哪一種方法？

- **從小規模開始，並展開：** 從輕量範本開始。 因為您所需的雲端作業模型變得更清楚，所以登陸區域解決方案會隨著時間而成熟。
- **從企業規模開始：** 從更完整的參考實行著手。 參考架構是以定義完善的雲端作業模型為基礎，需要較少的反復專案才能到達成熟的解決方案。
- **其他：** 合作夥伴有修改過的方法，應該能夠描述方法。

## <a name="design-principles"></a>設計原則

所有 Azure 登陸區域都必須考慮下列一組常見的設計區域。 我們指的是將這些設計區域實作為設計原則的方式。 下列各節將協助驗證合作夥伴的設計原則，以定義 Azure 登陸區域的執行。

### <a name="deployment-options"></a>部署選項

提供 Azure 登陸區域解決方案的合作夥伴可能會支援一或多個選項，以部署 (或修改/展開登陸區域，) 解決方案加入至您的 Azure 租使用者。

**合作夥伴的問題：** 您的 Azure 登陸區域解決方案支援下列哪一項？

- 設定 **自動化：** 解決方案是否會從部署管線或部署工具部署登陸區域？
- **手動設定：** 解決方案是否可讓 IT 小組手動設定登陸區域，而不會將錯誤插入登陸區域原始程式碼中？

**合作夥伴的問題：** 合作夥伴的解決方案支援哪些 Azure 登陸區域的執行選項？ 如需完整的選項清單，請參閱 [Azure 登陸區域的執行選項](./implementation-options.md) 一文。

### <a name="identity"></a>身分識別

身分識別可能是最重要的設計區域，可在合作夥伴解決方案中進行評估。

**合作夥伴的問題：** 合作夥伴解決方案支援下列哪一個身分識別管理選項？

- **Azure AD：** 建議的最佳作法是使用 Azure AD 和 Azure 角色型存取控制來管理 Azure 中的身分識別和存取。
- **Active Directory：** 如有需要，合作夥伴解決方案是否提供將 Active Directory 部署為基礎結構即服務解決方案的選項？
- **協力廠商識別提供者：** 如果您的公司使用協力廠商身分識別解決方案，請判斷合作夥伴的 Azure 登陸區域是否與協力廠商解決方案整合。

### <a name="network-topology-and-connectivity"></a>網路拓樸和連線能力

網路可以說是第二個最重要的設計領域。 有幾個最佳作法可以提供網路拓朴和連線能力。

**合作夥伴的問題：** 合作夥伴的 Azure 登陸區域解決方案包含下列哪一個選項？ 下列任何選項是否與夥伴解決方案不相容？

- **虛擬網路：** 合作夥伴解決方案是否會設定虛擬網路？ 是否可以修改其拓撲以符合您的技術或業務條件約束？
- **虛擬私人網路 (VPN) ：** 合作夥伴的登陸區域設計中是否包含 VPN 設定，可將雲端連線到現有的資料中心或辦公室？
- **高速連線能力：** 高階連線（例如 Azure ExpressRoute）是否包含在登陸區域設計中？
- **虛擬網路對等互連：** 此設計是否包含 Azure 中不同訂用帳戶或虛擬網路之間的連線能力？

### <a name="resource-organization"></a>資源組織

雲端的音效治理和操作管理從最佳作法資源組織開始。

**合作夥伴的問題：** 合作夥伴的登陸區域設計是否包含下列資源組織實務的考慮？

- **命名標準：** 這項供應專案會遵循哪些 [命名標準](../azure-best-practices/naming-and-tagging.md) ，並透過原則自動強制執行標準？
- **標記標準：** 登陸區域設定是否遵循並強制執行 [標記資產的特定標準](../azure-best-practices/resource-tagging.md)？
- **訂用帳戶設計：** 合作夥伴供應專案支援哪些訂用帳戶 [設計策略](../../decision-guides/subscriptions/index.md) ？
- **管理群組設計：** 合作夥伴供應專案是否遵循 [Azure 管理群組](../azure-best-practices/organize-subscriptions.md) 階層的定義模式來組織訂用帳戶？
- **資源群組對齊：** 如何使用資源群組將部署至雲端的資產分組？ 在合作夥伴供應專案中，資源群組是用來將資產群組至工作負載、部署套件或其他組織標準？

**合作夥伴的問題：** 合作夥伴是否提供登入檔來 [追蹤基礎決策](../../get-started/cloud-concepts.md) 和教育人員？ 如需這類檔的範例，請參閱 [初始決策範本](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/references/initial-decisions-checklist.docx) 。

### <a name="governance-disciplines"></a>治理專業領域

您的治理需求可能會嚴重影響任何複雜的登陸區域設計。 許多合作夥伴會提供個別的供應專案，以在部署登陸區域之後完全實行治理專業領域。 下列問題將有助於更清楚地瞭解將內建于任何登陸區域的治理層面。

**合作夥伴的問題：** 合作夥伴解決方案會在登陸區域實行中包含哪些治理工具？

- **原則合規性監視：** 合作夥伴的登陸區域解決方案是否包含已定義的治理原則，以及用來監視合規性的工具和流程？ 這項供應專案是否包含自訂原則以符合您的治理需求？
- **原則強制執行：** 合作夥伴的登陸區域解決方案包含自動化強制工具和流程嗎？
- **雲端平臺治理：** 合作夥伴供應專案是否包含解決方案來維護所有訂用帳戶上的一組通用原則？ 或受限於個別訂用帳戶的範圍嗎？
- **N/A：** 開始-小型方法會刻意延後將治理決策延後，直到團隊將低風險工作負載部署至 Azure 為止。 部署登陸區域解決方案之後，您可以在個別的供應專案中解決此問題。

**合作夥伴的問題：** 合作夥伴供應專案是否超越治理工具，也包含提供下列任何雲端治理專業領域的流程和作法？

- **成本管理：** 合作夥伴是否可提供團隊評估、監視及優化支出，同時與工作負載小組建立成本責任？
- **安全性基準：** 合作夥伴是否提供讓小組在安全性需求變更和成熟的情況之下維持合規性的準備？
- **資源一致性：** 合作夥伴是否可為小組提供準備，以確保雲端中的所有資產都上線到相關的作業管理程式中？
- 身分 **識別基準：** 合作夥伴供應專案是否會在部署初始登陸區域之後，準備小組來維護身分識別、角色定義和指派？

### <a name="operations-baseline"></a>作業基準

在登陸區域的執行期間，您的作業管理需求可能會影響特定 Azure 產品的設定。 許多合作夥伴都提供不同的供應專案，可在雲端採用旅程圖中的之後，但在您的第一個工作負載發行以供生產環境使用之前，完全實行營運基準和先進的作業。 但是，夥伴的登陸區域解決方案可能預設包含數個 operations management 工具的設定。

**合作夥伴的問題：** 合作夥伴解決方案是否包含支援任何雲端作業專業領域的設計選項？

- **清查和可見度：** 登陸區域是否包含工具，以確保會集中監視100% 的資產？
- **營運合規性：** 架構是否包含工具和自動化程式，以強制執行修補或其他作業合規性需求？
- **保護和復原：** 合作夥伴供應專案是否包含工具和設定，以確保已部署100% 資產的最基本標準備份和復原？
- **平臺作業：** 登陸區域供應專案是否包含工具或流程，以優化整個組合的作業？
- **工作負載作業：** 登陸區域供應專案包含用來管理工作負載特定作業需求的工具，並確保每個工作負載都能妥善架構？

## <a name="take-action"></a>採取動作

使用上述問題複習合作夥伴的 Azure 登陸區域供應專案或解決方案之後，您的小組將能更妥善地選擇 Azure 登陸區域最接近您雲端作業模式的合作夥伴。

如果您判斷 Azure 登陸區域部署的自助方法更適合，請參閱或重新流覽 [azure 登陸區域的執行選項](./implementation-options.md) ，以找出最符合您雲端作業模式的樣板化登陸區域方法。

## <a name="next-steps"></a>下一步

瞭解重構登陸區域的流程。

> [!div class="nextstepaction"]
> [重構登陸區域](./refactor.md)
