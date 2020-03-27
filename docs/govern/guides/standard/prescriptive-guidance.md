---
title: 標準企業治理：最佳做法說明
description: 使用適用于 Azure 的雲端採用架構，針對反映標準企業最佳做法的治理建立最基本的可行產品（MVP）。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/05/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 1d0c6d30e7bba864fb52b14fb82e1e88231e9a3c
ms.sourcegitcommit: ea63be7fa94a75335223bd84d065ad3ea1d54fdb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/27/2020
ms.locfileid: "80357051"
---
# <a name="standard-enterprise-governance-guide-best-practices-explained"></a>標準企業治理指南：說明的最佳做法

治理指南會從一組初始的[公司原則](./initial-corporate-policy.md)開始。 這些原則會用來建立治理 MVP，以反映出[最佳做法](./index.md)。

在本文中，我們會討論建立治理 MVP 所需的策略概要。 治理 MVP 的核心在於[部署加速](../../deployment-acceleration/index.md)專業領域。 在此階段套用的工具和模式，將可讓您在未來擴充治理所需的增量改善。

## <a name="governance-mvp-initial-governance-foundation"></a>治理 MVP （初始治理基礎）

快速採用治理和公司原則是可行的，因為有一些簡單的原則和雲端控管工具。 這些是在任何治理程式中要處理的前三個專業領域。 這篇文章將進一步說明每個專業領域。

為建立起點，本文將針對建立治理 MVP (所有採用的基礎) 所需的身分識別基準、安全性基準和部署加速，討論其背後的策略概要。

![累加式治理 MVP 的範例](../../../_images/govern/governance-mvp.png)

## <a name="implementation-process"></a>實作程序

治理 MVP 的實作相依於身分識別、安全性和網路。 一旦解決了相依性，雲端治理小組就會決定治理的幾個層面。 雲端治理小組和支援小組的決策會透過單一強制資產封裝來執行。

![累加式治理 MVP 的範例](../../../_images/govern/governance-mvp-implementation-flow.png)

這項實作也可以使用簡單的檢查清單來說明：

1. 關於核心相依性的請求決策：身分識別、網路功能、監視和加密。
2. 判斷在強制執行公司原則時要使用的模式。
3. 針對資源一致性、資源標記和記錄和報告專業領域，判斷適當的治理模式。
4. 實作符合所選原則強制執行模式的治理工具，以套用相依性決策和治理決策。

[!INCLUDE [implementation-process](../../../../includes/implementation-process.md)]

## <a name="application-of-governance-defined-patterns"></a>治理定義模式的應用程式

雲端治理小組負責下列決策和實施。 許多需要其他小組的輸入，但雲端治理小組可能會同時擁有決策和執行。 下列各節將概述為此使用案例所做的決策，以及每個決策的詳細資料。

### <a name="subscription-design"></a>訂用帳戶設計

決定要使用的訂用帳戶設計會決定 Azure 訂用帳戶的結構，以及如何使用 Azure 管理群組來有效率地管理這些訂用帳戶的存取、原則和合規性。 在此敘述中，治理小組已建立生產和非生產工作負載[生產和非生產](../../../ready/azure-best-practices/initial-subscriptions.md)訂用帳戶設計模式的訂閱。

- 在目前的焦點下，不太可能需要部門。 部署應該會侷限在單一計費單位內。 在採用階段，甚至可能不會有集中管理計費的企業合約。 這個層級的採用可能會由單一的隨用隨付 Azure 訂用帳戶來管理。
- 無論使用 EA 入口網站或企業合約是否存在，訂用帳戶模型仍應定義並同意，以將管理段無意的費用降至最低。
- 根據上述兩點，應同意將常見的命名慣例列為訂用帳戶設計的一部分。

### <a name="resource-consistency"></a>資源一致性

資源一致性決策會決定在訂用帳戶內一致地部署、設定和管理 Azure 資源所需的工具、程式和工作。 在此敘述中，已選擇 **[部署一致性](../../../decision-guides/resource-consistency/index.md#deployment-consistency)** 做為主要資源一致性模式。

- 系統會為使用生命週期方法的應用程式建立資源群組。 所有建立、維護及淘汰的專案，都應該放在單一資源群組中。 如需資源群組的詳細資訊，請參閱[這裡](../../../decision-guides/resource-consistency/index.md#basic-grouping)。
- Azure 原則應該套用至相關管理群組中的所有訂用帳戶。
- 作為部署程序的一部分，資源群組的 Azure 資源一致性範本應該儲存在原始檔控制中。
- 每個資源群組都會根據上述的生命週期方法，與特定的工作負載或應用程式相關聯。
- Azure 管理群組可在公司原則成熟時更新治理設計。
- Azure 原則的廣泛執行可能會超過小組的時間承諾，而且目前可能不會提供很大的價值。 不過，應建立簡單的預設原則並套用至每個管理群組，以強制執行少量目前的雲端治理原則陳述。 此原則會定義特定治理需求的實作。 然後，這些實作可以套用到所有已部署的資產上。

>[!IMPORTANT]
>每當資源群組中的資源不再共用相同的生命週期時，就應該將它移到另一個資源群組。 範例包括一般資料庫和網路元件。 雖然它們可以服務開發中的應用程式，但它們也可以提供其他用途，因此應該存在於其他資源群組中。

### <a name="resource-tagging"></a>資源標記

資源標記決策會決定如何將中繼資料套用至訂用帳戶內的 Azure 資源，以支援作業、管理和計量的用途。 在此敘述中，已選擇 **[分類](../../../decision-guides/resource-tagging/index.md#resource-tagging-patterns)** 模式做為資源標記的預設模型。

- 已部署的資產應標記為：
  - 資料分類
  - 程度
  - SLA
  - 環境
- 這四個值會促成治理、作業和安全性決策。
- 如果此治理指南是針對大型公司內的業務單位或小組所實行，則標記也應該包含計費單位的中繼資料。

### <a name="logging-and-reporting"></a>記錄與報告

記錄和報告決策會決定您的存放區記錄資料，以及讓 IT 人員知道如何在操作健全狀況上獲得通知的監視和報告工具是結構化的。 在此敘述中，建議使用[雲端原生模式](../../../decision-guides/logging-and-reporting/index.md#cloud-native)* * 來進行記錄和報告。

## <a name="incremental-improvement-of-governance-processes"></a>管理程式的累加式改進

當治理變更時，某些原則聲明不能或不應該由自動化工具來控制。 其他原則會導致 IT 安全性小組和內部部署身分識別管理小組必須不時工作。 為了協助管理發生的新風險，雲端治理小組將會監督下列程式。

**採用加速：** 雲端治理小組已跨多個小組檢查部署腳本。 他們維護了一組能作為部署範本的指令碼。 這些範本可供雲端採用小組和 DevOps 小組使用，進而更快速地定義部署。 其中每個腳本都包含強制執行一組治理原則的必要需求，而不需要雲端採用工程師進行額外的工作。 隨著這些腳本的 curators，雲端治理小組可以更快速地執行原則變更。 由於腳本鑒藏，雲端治理小組會被視為採用加速的來源。 這可以在沒有嚴格的強制遵循要求下，讓部署具有一致性。

**工程師訓練：** 雲端治理小組提供 bimonthly 訓練課程，並為工程師建立兩段影片。 這些資料可協助工程師快速了解治理文化，以及如何在部署期間完成各項工作。 小組正在新增定型資產，以顯示生產與非生產部署之間的差異，讓工程師能夠瞭解新原則會如何影響採用。 這可以在沒有嚴格的強制遵循要求下，讓部署具有一致性。

**部署規劃：** 在部署任何包含受保護資料的資產之前，雲端治理小組將會審查部署腳本，以驗證治理的對齊方式。 若現有小組使用先前已核准的部署，則會使用程式設計工具對其進行稽核。

**每月審查和報告：** 雲端治理小組每個月都會執行所有雲端部署的審查，以驗證原則的持續一致。 如果發現偏差，他們就會記下這些偏差，然後將其分享給雲端採用小組。 如果強制作業沒有造成業務中斷或資料流失的風險，就會自動強制執行原則。 在 audit 結束時，雲端治理小組會為雲端策略小組和每個雲端採用小組編譯報告，以溝通整體遵循原則。 報告也會儲存以便用於稽核和法律用途。

**每季原則審查：** 雲端治理小組和雲端策略小組每季都會回顧審查結果，並建議變更公司原則。 這些建議中有許多都是持續改進與觀察使用量模式的結果。 核准的原則變更會在後續的稽核週期內整合至治理工具中。

## <a name="alternative-patterns"></a>替代模式

如果在此治理指南中選取的任何模式未符合讀者的需求，則會提供每種模式的替代方案：

- [加密模式](../../../decision-guides/encryption/index.md)
- [身分識別模式](../../../decision-guides/identity/index.md)
- [記錄與報告模式](../../../decision-guides/logging-and-reporting/index.md)
- [原則強制執行模式](../../../decision-guides/policy-enforcement/index.md)
- [資源一致性模式](../../../decision-guides/resource-consistency/index.md)
- [資源標記模式](../../../decision-guides/resource-tagging/index.md)
- [軟體定義網路模式](../../../decision-guides/software-defined-network/index.md)
- [訂用帳戶設計模式](../../../decision-guides/subscriptions/index.md)

## <a name="next-steps"></a>後續步驟

在實作本指南後，每個雲端採用小組即可使用可靠的治理基礎繼續進行下去。 同時，雲端治理小組也會持續更新公司原則和治理專業領域。

這兩個小組會使用承受度指標，來識別要繼續支援雲端採用所需的下一組改良功能。 對於本指南中的虛構公司，下一個步驟是改善安全性基準，以支援將受保護的資料移至雲端。

> [!div class="nextstepaction"]
> [改善安全性基準專業領域](./security-baseline-improvement.md)
