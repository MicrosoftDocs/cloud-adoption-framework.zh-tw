---
title: 標準 enterprise 指南：改善成本管理專業領域
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 標準 enterprise 指南：改善成本管理專業領域
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/05/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: b4ff2db2b8d7009eb9d5a50dee630c1a8a60723c
ms.sourcegitcommit: 945198179ec215fb264e6270369d561cb146d548
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2019
ms.locfileid: "71967582"
---
# <a name="standard-enterprise-guide-improve-the-cost-management-discipline"></a>標準 enterprise 指南：改善成本管理專業領域

本文藉由將成本控制新增至治理 MVP 來推進敘述。

## <a name="advancing-the-narrative"></a>推進敘述

採用已成長超越治理 MVP 中定義的成本承受度指標。 這樣很好，因為它與 "DR" 資料中心的移轉相對應。 消費的增加現在證明了雲端治理小組的時間投資。

### <a name="changes-in-the-current-state"></a>目前狀態的變更

在此敘述的先前階段中，IT 已淘汰 100% 的 DR 資料中心。 應用程式開發和 BI 小組已準備好使用生產環境流量。

從那時起，某些事項已經改變，將會影響治理：

- 移轉小組已開始將 VM 移轉出生產環境資料中心外。
- 應用程式開發小組會積極地透過 CI/CD 管線將生產環境應用程式推送至雲端。 這些應用程式可以因應使用者需求被動地進行調整。
- IT 內的商業智慧小組在雲端中提供了數個預測性分析工具。 雲端中匯總的資料量會持續成長。
- 此成長支援認可業務成果。 不過，成本已開始迅速成長。 預計預算的成長速度比預期更快。 CFO 需要改進管理成本的方法。

### <a name="incrementally-improve-the-future-state"></a>以累加方式改善未來的狀態

成本監視和報告要新增至雲端解決方案。 IT 仍然作為成本結算所。 這表示 IT 採購會持續有雲端服務的付款。 不過，報告應該將直接操作費用系結至耗用雲端成本的功能。 此模型稱為「顯示回」雲端帳戶處理模型。

目前和未來狀態的變更會產生新風險，因此需要新的原則聲明。

## <a name="changes-in-tangible-risks"></a>有形風險的變更

**預算控制：** 有一個固有風險是，自助功能將在新平台上導致超量且非預期的成本。 監視成本及降低持續成本風險的治理流程必須就緒，才能確保會持續與規劃的預算保持一致。

此業務風險會延伸成少數技術風險：

- 實際成本可能會超過計劃。
- 業務狀況變更。 發生變更時，業務功能有可能必須耗用比預期還多的雲端服務，造成異常支出。 會有此額外支出被視為超額 (相對於計劃的必要調整) 的風險。
- 系統可能會過度佈建，導致過多支出。

## <a name="incremental-improvement-of-the-policy-statements"></a>原則聲明的累加式改進

下列原則變更將有助於補救新的風險和指南的執行。

- 治理小組應該針對計劃每週監視所有雲端成本。 雲端成本與計劃之間偏差的報告要每月與 IT 主管和財務部門分享。 所有雲端成本和計劃更新應該要每月與 IT 主管和財務部門一起檢閱。
- 所有成本必須針對權責目的配置給業務功能。
- 應該針對最佳化商機持續監視雲端資產。
- 雲端治理工具必須將資產調整大小選項限制為已核准的設定清單。 此工具必須確保所有資產都可探索且可透過成本監視解決方案來追蹤。
- 在部署規劃期間，應該記錄與生產環境工作負載裝載相關聯的任何必要雲端資源。 本文件可協助精簡預算及準備其他自動化，以避免使用較昂貴的選項。 在此流程期間，應該考量雲端提供者提供的不同折扣工具，例如保留執行個體或授權成本降低。
- 所有應用程式擁有者都必須參加將工作負載最佳化的實務訓練，以更好的方式來控制雲端成本。

## <a name="incremental-improvement-of-the-best-practices"></a>改善最佳做法的增量

本文的這一節將會變更治理 MVP 設計，以包含新的 Azure 原則和 Azure 成本管理的執行。 這兩個設計變更將共同實現新的公司原則聲明。

1. 實作 Azure 成本管理。
    1. 建立存取的正確範圍，與訂用帳戶模式和資源一致性專業領域保持一致。 假設與先前文章中定義的治理 MVP 一致，這需要在高階報告上執行之雲端治理小組的**註冊帳戶範圍**存取權。 治理之外的其他小組可能需要**資源群組範圍**存取權。
    1. 在 Azure 成本管理中建立預算。
    1. 檢閱初始建議並且採取動作。 進行週期性流程以支援報告。
    1. 設定及執行初始和週期性 Azure 成本管理報告。
2. 更新 Azure 原則
    1. 稽核標記、管理群組、訂用帳戶及資源群組值，以識別任何偏差。
    1. 建立 SKU 大小選項以限制對於部署規劃文件中列出之 SKU 的部署。

## <a name="conclusion"></a>結論

將這些流程和變更新增至治理 MVP 有助於補救與成本治理相關聯的許多風險。 它們會建立控制成本所需的可見度、責任歸屬和最佳化。

## <a name="next-steps"></a>後續步驟

隨著雲端採用持續並提供額外的商業價值，風險和雲端治理需求也會隨之改變。 對於本指南中的虛構公司，下一個步驟是使用此治理投資來管理多個雲端。

> [!div class="nextstepaction"]
> [多重雲端演進](./multicloud-improvement.md)
