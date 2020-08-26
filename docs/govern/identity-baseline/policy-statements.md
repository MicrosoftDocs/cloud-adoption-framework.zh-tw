---
title: 身分識別基準範例原則聲明
description: 使用適用于 Azure 的雲端採用架構，取得可協助您草擬原則聲明的範例身分識別基準原則聲明。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: dc239f34d54e49310620cd7245abfdaec4019356
ms.sourcegitcommit: 07d56209d56ee199dd148dbac59671cbb57880c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88880130"
---
# <a name="identity-baseline-sample-policy-statements"></a>身分識別基準範例原則聲明

個別雲端原則聲明是解決在風險評估流程中識別出的特定風險方針。 這些聲明應該會提供風險的簡明摘要，以及如何處理這些風險的方案。 每個聲明定義都應該包含下列資訊：

- **技術風險：** 此原則將解決的風險摘要。
- **原則聲明：** 原則需求的清楚摘要說明。
- **設計選項：** 可操作的建議、規格或其他指引，可供 IT 小組和開發人員在執行原則時使用。

下列範例原則聲明會解決常見的身分識別相關商務風險。 這些語句是在草擬原則聲明以解決組織需求時可參考的範例。 這些範例並非禁止使用，而且可能會有數個原則選項可處理每個已識別的風險。 與商務和 IT 小組密切合作，為您的唯一風險組合找出最佳原則。

## <a name="lack-of-access-controls"></a>缺少存取控制

**技術風險：** 沒有足夠或臨機操作存取控制設定可能會導致未經授權存取敏感性或任務關鍵性資源的風險。

**原則聲明：** 部署至雲端的所有資產都應使用由目前治理原則核准的身分識別和角色來控制。

**可能的設計選項：** [Azure Active Directory 條件式存取](/azure/active-directory/conditional-access/overview) 是 Azure 中的預設存取控制機制。

## <a name="overprovisioned-access"></a>過度佈建的存取

**技術風險：** 除了責任範圍之外，可控制資源的使用者和群組可能會導致未經授權的修改導致中斷或安全性弱點。

**原則聲明：** 將會執行下列原則：

- 最小許可權的存取模型將會套用至任何與任務關鍵性應用程式或受保護資料相關的資源。
- 更高的許可權應該是例外狀況，而任何這類例外狀況都必須與雲端治理小組一起記錄。 應定期稽核例外狀況。

**可能的設計選項：** 請參閱 [Azure 身分識別管理最佳作法](/azure/security/fundamentals/identity-management-best-practices) ，以 (RBAC) 策略來限制存取，根據 [需要知道](https://wikipedia.org/wiki/Need_to_know) 和 [最低許可權安全性原則](https://wikipedia.org/wiki/Principle_of_least_privilege) 來限制存取。

## <a name="lack-of-shared-management-accounts-between-on-premises-and-the-cloud"></a>缺少內部部署與雲端之間的共用管理帳戶

**技術風險：** 在內部部署 Active Directory 有帳戶的 IT 管理或系統管理人員可能沒有足夠的雲端資源存取權，可能無法有效率地解決操作或安全性問題。

**原則聲明：** 內部部署 Active Directory 基礎結構中具有更高許可權的所有群組，都應該對應至核准的 RBAC 角色。

**可能的設計選項：** 在雲端式 Azure Active Directory 與您的內部部署 Active Directory 之間執行混合式身分識別解決方案，並將必要的內部部署群組新增至執行其工作所需的 RBAC 角色。

## <a name="weak-authentication-mechanisms"></a>弱式驗證機制

**技術風險：** 具有能力不佳安全使用者驗證方法（例如基本使用者/密碼組合）的身分識別管理系統可能會導致遭盜用或遭到駭客破解的密碼，以提供在未經授權的情況下存取安全雲端系統的重大風險。

**原則聲明：** 所有帳戶都必須使用多重要素驗證方法來登入受保護的資源。

**可能的設計選項：** 針對 Azure Active Directory，請在您的使用者授權程式中執行 [Azure Multi-Factor Authentication](/azure/active-directory/authentication/concept-mfa-howitworks) 。

## <a name="isolated-identity-providers"></a>獨立的身分識別提供者

**技術風險：** 不相容的身分識別提供者可能會導致無法與客戶或其他商務夥伴共用資源或服務。

**原則聲明：** 部署任何需要客戶驗證的應用程式，都必須使用與內部使用者的主要身分識別提供者相容的核准身分識別提供者。

**可能的設計選項：** 使用內部和客戶識別提供者之間 [的 Azure Active Directory 來執行同盟，](/azure/active-directory/hybrid/whatis-fed) 或使用 [Azure Active Directory B2B](/azure/active-directory/b2b/what-is-b2b)

## <a name="identity-reviews"></a>身分識別檢閱

**技術風險：** 隨著時間變化，加入新的雲端部署或其他安全性考慮，可能會增加未經授權存取安全資源的風險。

**原則聲明：** 雲端治理程式必須包含身分識別管理小組的每季評論，以找出雲端資產設定應該避免的惡意執行者或使用模式。

**可能的設計選項：** 建立每季安全性審核會議，其中包括治理小組成員，以及負責管理身分識別服務的 IT 人員。 請參閱現有的安全性資料和計量，以在目前的身分識別管理原則和工具中建立差距，並更新原則來補救任何新風險。

## <a name="next-steps"></a>後續步驟

使用本文中所述的範例做為開發原則的起點，以解決符合您雲端採用方案的特定商務風險。

若要開始開發您自己的自訂身分識別基準原則聲明，請下載身分 [識別基準專業版範本](./template.md)。

若要加速採用此專業領域，請選擇最符合您環境的可 [操作治理指南](../guides/index.md) 。 然後修改設計，以納入您特定的公司原則決策。

根據風險和承受度建立流程來治理和傳達身分識別基準原則的遵循情況。

> [!div class="nextstepaction"]
> [建立原則合規性流程](./compliance-processes.md)
