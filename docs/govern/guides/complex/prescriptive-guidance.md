---
title: 複雜的企業治理：說明最佳作法
description: 使用適用于 Azure 的雲端採用架構來建立最小的可行產品 (MVP) ，以反映複雜企業的最佳作法。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/05/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: internal
ms.openlocfilehash: bc4f7dec5c3de109b36ad2f62aa1ba2cb644a27f
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97021044"
---
# <a name="governance-guide-for-complex-enterprises-best-practices-explained"></a>適用于複雜企業的治理指南：說明的最佳作法

治理指南會從一組初始的 [公司原則](./initial-corporate-policy.md)開始。 這些原則會用來建立治理的最小可行產品 (MVP)，以反映出[最佳做法](./index.md)。

在本文中，我們會討論建立治理 MVP 所需的策略概要。 治理 MVP 的核心是 [部署加速專業領域](../../deployment-acceleration/index.md)。 此階段所套用的工具和模式，可讓您在未來擴充治理所需的增量改進。

## <a name="governance-mvp-initial-governance-foundation"></a>治理 MVP (初始治理基礎) 

快速採用治理和公司原則是可行的，因為有一些簡單的原則和雲端控管工具。 在任何治理程序中都有要達成的三個治理專業領域，而這些是第一個。 本文將進一步說明每個專業領域。

為了建立起點，本文將討論在安全性基準、身分識別基準，以及建立治理 MVP 所需的部署加速專業領域背後的高層級策略。 MVP 作為所有雲端採用的基礎。

![此圖顯示累加式治理 MVP 的範例。](../../../_images/govern/governance-mvp.png)

## <a name="implementation-process"></a>實作程序

治理 MVP 的實施具有身分識別、安全性和網路功能的相依性。 解決相依性後，雲端治理小組就會決定治理的幾個層面。 雲端治理小組和來自支援小組的決策，將透過單一的強制資產套件來實行。

![顯示治理 MVP 之實行流程的圖表。](../../../_images/govern/governance-mvp-implementation-flow.png)

這項實作也可以使用簡單的檢查清單來說明：

1. 要求有關核心相依性的決策：身分識別、網路和加密。
1. 判斷在強制執行公司原則時要使用的模式。
1. 判斷資源一致性、資源標記以及記錄和報告的適當治理模式。
1. 實作符合所選原則強制執行模式的治理工具，以套用相依性決策和治理決策。

[!INCLUDE [implementation-process](../../../../includes/implementation-process.md)]

## <a name="application-of-governance-defined-patterns"></a>治理定義模式的應用程式

雲端治理小組會負責下列決策和實施。 許多人都需要來自其他小組的輸入，但雲端治理小組可能會擁有決策和實行。 下列各節將概述為此使用案例所做的決策，以及每個決策的詳細資料。

### <a name="subscription-design"></a>訂用帳戶設計

決定要使用何種訂用帳戶設計，決定如何將 Azure 訂用帳戶結構化，以及如何使用 Azure 管理群組來有效率地管理這些訂用帳戶的存取、原則和合規性。 在此敘述中，治理小組已選擇 [混合的訂](../../../decision-guides/subscriptions/index.md#mix-subscription-strategies)用帳戶原則。

- 當 Azure 資源有新的要求時，就應該為每個營運地理位置中的每個主要業務單位建立 *部門* 。 在每個部門 *內，都* 應該為每個應用程式原型建立訂用帳戶。
- 應用程式原型是以類似需求將應用程式分組的方式。 常見範例包括：
  - 具有受保護資料的應用程式，受管理的應用程式 (例如 HIPAA 或 FedRAMP) 。
  - 低風險的應用程式。
  - 具有內部部署相依性的應用程式。
  - Azure 中的 SAP 或其他大型主機應用程式。
  - 擴充內部部署 SAP 或大型主機應用程式的應用程式。

  每個組織在資料分類和支援業務的應用程式類型上都有獨特需求。 數位資產的相依性對應可協助定義組織中的應用程式原型。
- 您應該根據上述方式，將一般的命名慣例作為訂用帳戶設計的一部分來採用。

### <a name="resource-consistency"></a>資源一致性

資源一致性決策可決定在訂用帳戶內一致地部署、設定及管理 Azure 資源所需的工具、程式和工作。 在此敘述中，已選擇 [部署一致性](../../../decision-guides/resource-consistency/index.md#deployment-consistency) 作為主要資源一致性模式。

- 使用生命週期方法來建立應用程式的資源群組。 所有建立、維護及淘汰的專案，都應該放在單一資源群組中。 如需詳細資訊，請參閱 [資源一致性決策指南](../../../decision-guides/resource-consistency/index.md#basic-grouping)。
- Azure 原則應該套用至相關管理群組中的所有訂用帳戶。
- 作為部署程式的一部分，資源群組的 Azure 資源一致性範本應該儲存在原始檔控制中。
- 每個資源群組都是根據上述生命週期方法，與特定的工作負載或應用程式相關聯。
- Azure 管理群組可在公司原則成熟時更新治理設計。
- Azure 原則的廣泛實施可能會超出小組的時間承諾，並可能無法在此時提供大量的價值。 您應建立簡單的預設原則並套用至每個管理群組，以強制執行少量的目前雲端治理原則聲明。 此原則會定義特定治理需求的實作。 然後，這些實作可以套用到所有已部署的資產上。

>[!IMPORTANT]
> 一旦資源群組中的資源不再共用相同的生命週期，則應該將它移至另一個資源群組。 範例包括通用資料庫和網路元件。 雖然它們可能會為開發的應用程式提供服務，但它們也可以提供其他用途，因此應該存在於其他資源群組中。

### <a name="resource-tagging"></a>資源標記

資源標記決策可決定如何將中繼資料套用至訂用帳戶內的 Azure 資源，以支援作業、管理和帳戶處理用途。 在此敘述中，已選擇 [會計](../../../decision-guides/resource-tagging/index.md#resource-tagging-patterns) 模式作為資源標記的預設模型。

- 已部署的資產應該以下列值標記：
  - 部門或帳單單位
  - [地理位置]
  - 資料分類
  - 重要性
  - SLA
  - 環境
  - 應用程式原型
  - 應用程式
  - 應用程式擁有者
- 這些值以及與已部署資產相關聯的 Azure 管理群組和訂用帳戶，將會推動治理、營運和安全性決策。

### <a name="logging-and-reporting"></a>記錄與報告

記錄和報告決策可決定您儲存記錄資料的方式，以及如何將讓 IT 人員在營運健康狀態中保持相關的監視和報告工具，結構化。 在此敘述中，建議使用記錄和報告的 [混合式監視](../../../decision-guides/logging-and-reporting/index.md) 模式，但目前並不需要任何開發團隊。

- 在為了進行記錄或報告而收集的特定資料點上，目前並未設定治理需求。 此功能是此虛構敘述特有的，而且應將其視為反模式。 應儘速決定和強制執行記錄標準。
- 發行任何受保護的資料或任務關鍵性工作負載之前，需執行額外的分析。
- 在支援受保護的資料或任務關鍵性工作負載之前，現有的內部部署作業監視解決方案必須獲得用於記錄的工作區存取權。 如果應用程式要由定義的 SLA 來支援，則應用程式必須符合使用該租用戶時的相關安全性和記錄需求。

## <a name="incremental-of-governance-processes"></a>治理流程的累加式

某些原則聲明無法或不應該由自動化工具來控制。 其他原則則需要 IT 安全性和內部部署身分識別基準小組的定期工作。 雲端治理小組必須監督下列程式，才能執行最後八個原則聲明：

**公司原則變更：** 雲端治理小組會對治理 MVP 設計進行變更，以採用新的原則。 治理 MVP 的值可用於自動強制執行新原則。

**採用加速：** 雲端治理小組已跨多個小組審視部署腳本。 他們已維護一組能作為部署範本的指令碼。 這些範本可供雲端採用小組和 DevOps 小組使用，進而更快速地定義部署。 每個指令碼都包含強制執行治理原則的需求，而且不需要雲端採用工程師執行額外的工作。 作為這些指令碼的規劃者，他們能更快速地實作原則變更。 此外，也會將它們視為採用的加速器。 這可以在沒有嚴格的強制遵循要求下，確保部署的一致性。

**工程訓練：** 雲端治理小組提供 bimonthly 的訓練課程，並為工程師建立了兩段影片。 這兩種資源可協助工程師快速了解治理特性，以及如何執行部署。 小組正在新增訓練資產，以示範生產與非生產部署之間的差異，以協助工程師瞭解新原則如何影響採用。 這可以在沒有嚴格的強制遵循要求下，確保部署的一致性。

**部署規劃：** 在部署任何包含受保護資料的資產之前，雲端治理小組會負責檢查部署腳本，以驗證治理的一致性。 若現有小組使用先前已核准的部署，則會使用程式設計工具對其進行稽核。

**每月審核和報告：** 雲端治理小組每個月都會執行所有雲端部署的審核，以驗證原則的持續對齊。 當發現偏差時，它們會記載並與雲端採用小組分享。 如果強制作業沒有造成業務中斷或資料流失的風險，就會自動強制執行原則。 在審核結束時，雲端治理小組會為雲端策略小組和每個雲端採用小組編譯報告，以傳達整體遵循原則。 報告也會儲存以便用於稽核和法律用途。

**每季原則審核：** 每季，雲端治理小組和雲端策略小組，以審視審核結果並建議公司原則的變更。 這些建議中有許多都是持續改進與觀察使用量模式的結果。 核准的原則變更會在後續的稽核週期內整合至治理工具中。

## <a name="alternative-patterns"></a>替代模式

如果本治理指南中選擇的任何模式不符合讀者的需求，則可以使用每種模式的替代方案：

- [加密模式](../../../decision-guides/encryption/index.md)
- [身分識別模式](../../../decision-guides/identity/index.md)
- [記錄和報告模式](../../../decision-guides/logging-and-reporting/index.md)
- [原則強制執行模式](../../../decision-guides/policy-enforcement/index.md)
- [資源一致性模式](../../../decision-guides/resource-consistency/index.md)
- [資源標記模式](../../../decision-guides/resource-tagging/index.md)
- [軟體定義網路模式](../../../decision-guides/software-defined-network/index.md)
- [訂用帳戶設計模式](../../../decision-guides/subscriptions/index.md)

## <a name="next-steps"></a>後續步驟

在實作本指南後，每個雲端採用小組即可使用可靠的治理基礎繼續進行下去。 同時，雲端治理小組也會努力持續更新公司原則和治理專業領域。

這兩個小組都會使用容錯指標來識別繼續支援雲端採用所需的下一組改良功能。 此公司的下一步是其治理基準的累加式改進，以支援具有舊版或協力廠商多重要素驗證需求的應用程式。

> [!div class="nextstepaction"]
> [改善身分識別基準專業領域](./identity-baseline-improvement.md)
