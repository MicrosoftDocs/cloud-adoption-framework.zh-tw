---
title: 將現有的 Azure 環境轉換成企業規模
description: 將現有環境上線至企業規模架構
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: think-tank, csu
ms.openlocfilehash: 5adbe16e5333c1a2e2b3205a19f739e26796133b
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97026670"
---
<!-- docutune:casing resourceType resourceTypes resourceId resourceIds -->

# <a name="transition-existing-azure-environments-to-enterprise-scale"></a>將現有的 Azure 環境轉換成企業規模

我們認為大部分的組織在 Azure 中可能會有現有的使用量、一或多個訂用帳戶，而且可能是其管理群組的現有結構。 視其最初的商務需求和案例而定，混合式連線 (（例如使用站對站 VPN 和/或 ExpressRoute) ）可能已部署。

本文可協助組織根據現有的 Azure 環境，以轉換成企業規模的形式流覽正確的路徑。 本文也會說明在 Azure 中移動資源的考慮 (例如，將訂用帳戶從一個現有的管理群組移至另一個管理群組) ，可協助您評估及規劃將現有的 Azure 環境轉換成企業規模的登陸區域。

## <a name="moving-resources-in-azure"></a>在 Azure 中移動資源

您可以在建立後移動 Azure 中的某些資源，而且有不同的方法可讓組織依使用者的 RBAC 許可權（以及跨範圍）。 下表概述哪些資源可以移動、哪些範圍，以及與每個資源相關聯的優缺點。

| 影響範圍 | Destination | 優點 | 缺點 |
|--|--|--|--|
| 資源群組中的資源 | 可移至相同或不同訂用帳戶中的新資源群組  | 可讓您在部署之後修改資源群組中的資源組合 | -所有 resourceTypes 都不支援 <br> -某些 resourceTypes 有特定的限制或需求 <br> -Resourceid 會更新並影響現有的監視、警示和控制平面作業 <br> -在移動期間鎖定資源群組 <br> -需要評估原則和 RBAC 的前置和移動後作業 |
| 租使用者中的訂用帳戶  | 可移至不同的管理群組和不同的租使用者 | 因為不會變更 resourceId 值，所以不會影響訂用帳戶內的現有資源 | 需要評估原則和 RBAC 的前置和移動後作業 |

為了瞭解您應使用的移動策略，我們將逐步解說兩者的範例：

## <a name="subscription-move"></a>訂用帳戶移動

移動訂用帳戶的常見使用案例是將訂用帳戶組織成管理群組，或將訂用帳戶傳送到新的 Azure Active Directory 租使用者。 企業規模的訂用帳戶移動著重于將訂用帳戶移至管理群組。 將訂用帳戶移至新的租使用者主要是為了 [轉移帳單擁有權](/azure/cost-management-billing/manage/billing-subscription-transfer)。

### <a name="rbac-requirements"></a>RBAC 需求

若要在移動之前評估訂用帳戶，使用者必須具有適當的 RBAC，例如訂用帳戶上的擁有者 (直接角色指派) ，而且具有目標管理群組的寫入權限 (支援這項作業的內建角色是擁有者角色、參與者角色，以及) 的管理群組參與者角色。

如果使用者對現有管理群組的訂用帳戶具有繼承的擁有者角色許可權，則訂用帳戶只能移至已獲指派擁有者角色之使用者的管理群組。

### <a name="policy"></a>原則

現有的訂用帳戶可能會受限於直接指派的 Azure 原則，或位於其目前所在的管理群組。 評估目前的原則，以及可能存在於新管理群組/管理群組階層中的原則，是很重要的。

Azure Resource Graph 可以用來執行現有資源的清查，並將其設定與目的地現有的原則進行比較。

一旦將訂用帳戶移至已有現有 RBAC 和原則的管理群組之後，請考慮下列選項：

- 任何繼承至已移動訂用帳戶的 RBAC 最多可能需要30分鐘的時間，管理群組快取中的使用者權杖才會重新整理。 若要加速此程式，您可以藉由登出和或要求新的權杖來重新整理權杖。
- 指派範圍包含已移動之訂用帳戶的任何原則，都只會對現有的資源執行審核作業。 具體而言：
  - 訂用帳戶中 **deployIfNotExists** 原則效果的任何現有資源會顯示為不符合規範，且不會自動補救，但需要使用者互動才能手動執行補救。
  - 訂用帳戶中的任何現有資源（受 **拒絕** 原則影響）會顯示為不符合規範，且不會被拒絕。 使用者必須視需要手動減輕此結果。
  - 訂用帳戶中任何符合 **附加** 和 **修改** 原則效果的現有資源將會顯示為不符合規範，且需要使用者互動才能減輕問題。
  - 訂用帳戶中任何現有的資源會受到 **audit** 和 **auditIfNotExist** 的規範，且需要使用者互動才能緩和。
- 針對已移動訂用帳戶中資源的所有新寫入，將會以正常的方式即時受指派的原則。

## <a name="resource-move"></a>資源移動

執行資源移動的主要使用案例是當您想要將資源合併到相同的資源群組時，如果它們共用相同的生命週期，或將資源移至不同的訂用帳戶，因為成本、擁有權或 RBAC 需求。

執行資源移動時，會鎖定來源資源群組和目標資源群組 (此鎖定不會影響資源群組中的任何資源) 在移動作業期間，這表示您無法新增、更新或刪除資源群組中的資源。 資源移動作業不會變更資源的位置。

### <a name="before-you-move-resources"></a>移動資源之前

在移動作業之前，您必須確認 [範圍中的資源受到支援](/azure/azure-resource-manager/management/move-support-resources) ，以及評估其需求和相依性。 例如，移動對等互連虛擬網路需要先停用虛擬網路對等互連，並在移動作業完成後重新啟用對等互連。 這項停用/重新啟用相依性需要預先規劃，以瞭解任何可能連線到您虛擬網路的現有工作負載所造成的影響。

### <a name="post-move-operation"></a>移動後操作

當資源移到相同訂用帳戶中的新資源群組時，仍會套用來自管理群組或/和訂用帳戶範圍的任何繼承 RBAC 和原則。 如果您移至新訂用帳戶中的資源群組，其中訂用帳戶可能會受限於其他 RBAC 和原則指派，則相同的指導方針適用于移動訂用帳戶案例，以驗證資源合規性和存取控制。
