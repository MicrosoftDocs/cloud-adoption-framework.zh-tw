---
title: 身分識別基準範例原則聲明
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 身分識別基準範例原則聲明
author: BrianBlanchard
ms.author: brblanch
ms.date: 02/11/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: fae5bb8283487ef7724f872fc293def2c1a80071
ms.sourcegitcommit: 443c28f3afeedfbfe8b9980875a54afdbebd83a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2019
ms.locfileid: "71027305"
---
# <a name="identity-baseline-sample-policy-statements"></a>身分識別基準範例原則聲明

個別雲端原則聲明是解決在風險評估流程中識別出的特定風險方針。 這些聲明應該會提供風險的簡明摘要，以及如何處理這些風險的方案。 每個聲明定義都應該包含下列資訊：

- **技術風險：** 此原則將解決的風險摘要。
- **原則聲明：** 明確地摘要說明原則需求。
- **設計選項：** IT 小組與開發人員可以在實作原則時使用之可操作的建議、規格或其他指引。

下列範例原則聲明會解決常見的身分識別相關商務風險。 這些語句是您在草擬原則聲明以滿足組織需求時可以參考的範例。 這些範例並非禁止使用，而且可能會有數個原則選項來處理每個已識別的風險。 與企業和 IT 小組密切合作，找出您獨特風險的最佳原則。

## <a name="lack-of-access-controls"></a>缺少存取控制

**技術風險：** 不足或特定的存取控制設定可能導致敏感或任務關鍵性資源遭到未經授權的存取。

**原則聲明：** 部署至雲端的所有資產，均應使用經目前的治理原則核准的身分識別與角色來控管。

**潛在的設計選項：** [Azure Active Directory 條件式存取](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)是 Azure 中的預設存取控制機制。

## <a name="overprovisioned-access"></a>過度佈建的存取

**技術風險：** 使用者和群組若對資源有超過其責任範圍的控制權，則可能會發生未經授權的修改情形，進而造成業務中斷或安全性弱點。

**原則聲明：** 您將實作下列原則：

- 最低許可權存取模型將會套用至任何與任務關鍵性應用程式或受保護資料相關的資源。
- 較高的許可權應該是例外狀況，而且任何這類例外狀況都必須與雲端治理小組一起記錄。 應定期稽核例外狀況。

**潛在的設計選項：** 請參閱[Azure 身分識別管理最佳作法](https://docs.microsoft.com/azure/security/azure-security-identity-management-best-practices)，以執行以角色為基礎的存取控制（RBAC）策略，根據[需要知道](https://wikipedia.org/wiki/Need_to_know)和[最低許可權的安全性](https://wikipedia.org/wiki/Principle_of_least_privilege)原則來限制存取。

## <a name="lack-of-shared-management-accounts-between-on-premises-and-the-cloud"></a>缺少內部部署與雲端之間的共用管理帳戶

**技術風險：** 您內部部署 Active Directory 上具有帳戶的 IT 管理或系統管理人員若無足夠的存取權來存取雲端資源，則可能無法有效率地解決操作問題或安全性問題。

**原則聲明：** 內部部署 Active Directory 基礎結構中已提高權限的所有群組，均應對應至經核准的 RBAC 角色。

**潛在的設計選項：** 在您的雲端式 Azure Active Directory 與內部部署 Active Directory 之間執行混合式身分識別解決方案，並將必要的內部部署群組新增至執行其工作所需的 RBAC 角色。

## <a name="weak-authentication-mechanisms"></a>弱式驗證機制

**技術風險：** 身分識別管理系統所用的使用者驗證方法 (例如基本的使用者/密碼組合) 若不夠安全，可能會導致密碼外洩或遭駭客入侵，而最大風險是安全的雲端系統會遭到未經授權的存取。

**原則聲明：** 所有帳戶都必須使用多重要素驗證方法來登入受保護的資源。

**潛在的設計選項：** 針對 Azure Active Directory，請將 [Azure Multi-factor Authentication](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks) 實作為使用者授權程序的一部分。

## <a name="isolated-identity-providers"></a>獨立的身分識別提供者

**技術風險：** 不相容的識別提供者可能會導致無法與客戶或其他業務夥伴共用資源或服務。

**原則聲明：** 部署需要客戶驗證的任何應用程式時，必須使用適用於內部使用者的主要身分識別提供者核准的身分識別提供者。

**潛在的設計選項：** 在內部與客戶識別提供者之間實作[與 Azure Active Directory 同盟](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-fed)。

## <a name="identity-reviews"></a>身分識別檢閱

**技術風險：** 當業務隨著時間發生變化時，您就需要增加新的雲端部署或其他安全性考量，而這可能會增加安全資源遭受未經授權存取的風險。

**原則聲明：** 雲端治理流程必須包括與身分識別管理小組一起進行每季檢閱，以找出雲端資產設定應該避免的惡意執行者或使用模式。

**潛在的設計選項：** 召開每季安全性檢閱會議，其中包括治理小組成員，以及負責管理識別服務的 IT 人員。 請參閱現有的安全性資料和計量，以在目前的身分識別管理原則和工具中建立間距，並更新原則以補救任何新的風險。

## <a name="next-steps"></a>後續步驟

請使用本文中所述的範例，做為開發原則的起點，以解決符合您雲端採用方案的特定商務風險。

若要開始自行開發與身分識別基準相關的自訂原則聲明，請下載[身分識別基準範本](./template.md)。

若要加速採用這個專業領域，請選擇最符合您環境的可採取動作的[治理指南](../guides/index.md)。 然後修改設計，以納入您特定的公司原則決策。

根據風險和承受度建立流程來治理和傳達身分識別基準原則的遵循情況。

> [!div class="nextstepaction"]
> [建立原則合規性流程](./compliance-processes.md)
