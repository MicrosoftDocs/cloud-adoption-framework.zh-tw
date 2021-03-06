---
title: 標準企業治理：改善資源一致性專業領域
description: 使用適用于 Azure 的雲端採用架構，瞭解如何改善治理基準，以及新增復原、調整大小和監視控制項來補救風險。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/05/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: internal
ms.openlocfilehash: c279f43f0f2efc134cf3cdadc8551e3b9b828e5d
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97020602"
---
# <a name="standard-enterprise-governance-guide-improve-the-resource-consistency-discipline"></a>標準企業治理指南：改善資源一致性專業領域

本文將新增資源一致性控制項以支援任務關鍵性應用程式，以提升敘述的進展。

## <a name="advancing-the-narrative"></a>前進敘述

新的客戶體驗、新的預測工具和已移轉的基礎結構會繼續進行。 企業現在已準備開始在產能上使用這些資產。

### <a name="changes-in-the-current-state"></a>處於目前狀態的變更

在此敘述的上一個階段中，應用程式開發和 BI 小組幾乎已準備好將客戶和財務資料整合到生產工作負載。 IT 小組已經開始淘汰 DR 資料中心。

從那時起，某些將會影響治理的事項已經改變：

- IT 已 100% 淘汰 DR 資料中心，且進度超前。 在此程式中，生產資料中心內的一組資產被視為雲端遷移候選。
- 應用程式開發小組現在已準備好使用生產環境流量。
- BI 小組已準備好將預測和深入解析送回生產資料中心內的作業系統。

### <a name="incrementally-improve-the-future-state"></a>以累加方式改進未來的狀態

在生產業務程序中使用 Azure 部署之前，必須有成熟的雲端作業。 此外，還需要進行其他治理變更，以確保資產可以正常運作。

目前和未來狀態的變更會產生新風險，因此需要新的原則聲明。

## <a name="changes-in-tangible-risks"></a>有形風險的變更

**商務中斷：** 任何新平臺都會造成關鍵性商務程式中斷的固有風險。 IT 營運小組和在不同雲端採用上執行的團隊，對雲端營運來說相當缺乏經驗。 這會增加中斷的風險，且必須加以補救及控管。

此商務風險可能會延伸出一些技術風險：

1. 外部入侵或拒絕服務攻擊可能會導致業務中斷。
1. 可能無法正確地探索任務關鍵性資產，因此可能無法正常運作。
1. 現有的作業管理程序可能不支援未探索到或標記錯誤的資產。
1. 已部署資產的設定可能不符合效能預期。
1. 記錄可能無法正確記載並集中，因此無法補救效能問題。
1. 復原原則可能會失敗，或是執行時間超出預期。
1. 不一致的部署流程可能導致安全性落差，因而造成資料外洩或中斷。
1. 設定漂移或遺失修補程式可能導致非預期的安全性落差，因而造成資料外洩或中斷。
1. 設定可能不會強制執行所定義的 SLA 需求或已認可的復原需求。
1. 部署的作業系統或應用程式可能無法符合強化需求。
1. 有很多小組在雲端中工作時，會有不一致的風險。

## <a name="incremental-improvement-of-the-policy-statements"></a>原則語句的累加式改進

下列原則變更將有助於補救新的風險和指南的實施。 清單看起來很長，但採用這些原則可能比看起來的簡單。

1. 所有已部署的資產都必須依據嚴重性和資料分類來分類。 分類會由雲端治理小組和應用程式擁有者審查，再部署至雲端。
1. 包含任務關鍵性應用程式的子網路必須受到防火牆解決方案的保護，以偵測入侵及回應攻擊。
1. 治理工具必須稽核並強制執行安全性管理小組所定義的網路設定需求。
1. 治理工具必須確認所有與任務關鍵性應用程式或受保護資料相關的資產都會受到監視，以了解資源損耗與最佳化的情形。
1. 治理工具必須確認會針對所有任務關鍵性應用程式或受保護的資料，收集適當層級的記錄資料。
1. 治理程序必須確認任務關鍵性應用程式和受保護資料的備份、復原和 SLA 遵循皆正確實作。
1. 治理工具必須限制只對已核准的映像進行虛擬機器部署。
1. 治理工具必須強制避免在支援任務關鍵性應用程式的所有已部署資產上進行自動更新。 必須與作業管理小組一起檢閱違規事件，並根據作業原則來進行修復。 不會自動更新之資產必須包含在 IT 部門所負責的處理程序中。
1. 治理工具必須確認與成本、重要性、SLA、應用程式及資料類別相關的標記。 所有值必須符合由治理小組管理的預先定義值。
1. 治理流程必須包含部署期間和一般週期的稽核，以確保所有資產之間的一致性。
1. 安全性小組應定期檢閱可能影響雲端部署的趨勢與攻擊，以更新雲端中使用的安全性管理工具。
1. 在發行至生產環境之前，必須將所有任務關鍵性應用程式和受保護的資料新增至指定的作業監視解決方案。 如果所選取的 IT 作業工具找不到某些資產，則這些資產就無法發行為生產用的資產。 為了讓資產變為可搜尋所做的變更，也必須對相關部署程序執行，如此才能確保在未來的部署中能探索到該資產。
1. 當探索到時，營運管理小組會調整資產大小，以確保資產符合效能需求。
1. 部署工具必須由雲端治理小組核准，以確保持續治理已部署的資產。
1. 部署腳本必須在雲端治理小組可存取的中央存放庫中進行維護，以進行定期審核和審核。
1. 治理檢閱程序必須確認部署的資產已根據 SLA 及復原需求進行正確設定。

## <a name="incremental-improvement-of-governance-practices"></a>治理做法的累加式改進

本文的這一節將會變更治理 MVP 設計，以包含新的 Azure 原則以及 Azure 成本管理 + 計費的實施。 這兩個設計變更將共同實現新的公司原則聲明。

1. 雲端營運團隊將會定義營運監視工具和自動化補救工具。 雲端治理小組將會支援這些探索流程。 在此使用案例中，雲端作業小組選擇 Azure 監視器作為監視要徑任務應用程式的主要工具。
2. 在 Azure DevOps 中建立存放庫來存放所有相關的 Resource Manager 範本和指令碼式的組態，並為這些項目設定版本。
3. Azure 復原服務保存庫的執行：
    1. 定義及部署用於備份和復原程式的 Azure 復原服務保存庫。
    2. 建立 Resource Manager 範本以在每個訂用帳戶中建立保存庫。
4. 更新所有訂用帳戶的 Azure 原則：
    1. 在所有訂用帳戶上稽核並強制執行重要性和資料分類，以識別任何具有任務關鍵性資產的訂用帳戶。
    2. 僅稽核並強制使用已核准的映像。
5. Azure 監視器實作：
    1. 一旦識別出任務關鍵性的工作負載，請建立 Azure 監視器 Log Analytics 工作區。
    2. 在部署測試期間，雲端作業小組會部署必要的代理程式和測試探索。
6. 針對包含任務關鍵性應用程式的所有訂用帳戶更新 Azure 原則。
    1. 稽核並強制執行使用 NSG 連到所有 NIC 和子網路的應用程式。 網路和 IT 安全性定義了 NSG。
    2. 針對每個網路介面，審核並強制使用已核准的網路子網和虛擬網路。
    3. 稽核並強制執行使用者定義的路由表限制。
    4. 稽核並強制對所有虛擬機器部署 Azure 監視器代理程式。
    5. 審核並強制執行訂用帳戶中的 Azure 復原服務保存庫。
7. 防火牆組態：
    1. 識別符合安全性需求的 Azure 防火牆設定。 或者，識別與 Azure 相容的第三方應用裝置。
    1. 建立 Resource Manager 範本來部署具有必要設定的防火牆。
8. Azure 藍圖：
    1. 建立名為 `protected-data` 的新 Azure 藍圖。
    2. 將防火牆和 Azure 復原服務保存庫範本新增至藍圖。
    3. 為受保護資料的訂用帳戶新增原則。
    4. 將藍圖發佈到任何將裝載任務關鍵性應用程式的管理群組。
    5. 對每個受影響的訂用帳戶及現有藍圖套用新的藍圖。

## <a name="conclusion"></a>結論

這些額外的流程和治理 MVP 的變更，有助於補救與資源治理相關聯的許多風險。 同時還會新增可加強雲端感知作業的復原、大小調整及監視控制項。

## <a name="next-steps"></a>後續步驟

隨著雲端採用持續提供額外的商業價值，風險和雲端治理需求也會改變。 針對本指南中的虛構公司，下一個觸發程式是當部署規模超過100個資產到雲端，或每月支出超過每月 $1000。 此時，雲端治理小組會新增成本管理控制項。

> [!div class="nextstepaction"]
> [改善成本管理專業領域](./cost-management-improvement.md)
