---
title: 登陸區域部署選項
description: 哪一個登陸區域部署選項最適合您的需求
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 274f13ba5189878782788445e8893a86f4de4030
ms.sourcegitcommit: e5c4db8f660fa4c58d1441f0feb4cce415491dfd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2020
ms.locfileid: "84942925"
---
# <a name="landing-zone-implementation-options"></a>登陸區域的執行選項

[Azure 登陸區域](./index.md)的目標是為雲端採用小組提供管理完善的環境，以供其工作負載使用。 為了達到該目標，Microsoft 建議遵循先前文章中所討論的[登陸區域設計區域](./design-areas.md)。

下列每個執行選項都是針對一組特定的作業模型相依性所建立，以支援您特定的非功能性需求。 每個實作為選項都包含不同的自動化方法。 如有提供，也會包含參考架構和參考，以加速您的雲端採用旅程。 雖然每個都會對應至不同的作業模型，但它們全都共用相同的設計區域。 其差異在於您選擇要如何實行。

## <a name="platform-development-velocity"></a>平臺開發速度

除了建議的設計區域之外，平臺開發速度（您的平臺小組可以開發所需技能的速度）是選擇最佳部署選項的關鍵因素。 有兩個主要模式需要考慮：

**開始使用企業規模：** 當商務需求需要豐富的登陸區域初始執行功能時，從一開始就具備完全整合的管理、安全性和作業，Microsoft 建議使用企業級的方法。 這種方法可以使用 Azure 入口網站或基礎結構即程式碼，來設定您的環境。 當您的組織準備就緒時，您也可以在入口網站和基礎結構即程式碼之間轉換（建議使用）。 如同任何其他 Azure 基礎結構即程式碼的方法，您將需要 Azure Resource Manager 範本和 GitHub 的技能。

**從小規模開始並調整規模：** 如果在您深入瞭解雲端時開發這些技能並認可您自己的決策，則更重要的是，更具模組化的方法可能會有説明。 在此方法中，登陸區域只會著重于執行啟動雲端採用所需的基本登陸區域考慮。 隨著採用規模的規範，管理和管理方法中的模組將會建置於初始登陸區域之上。 任何 Azure 登陸區域的設計原則，將會概述需要在一段時間內重構的特定設計區域。

## <a name="implementation-options"></a>實作選項

下列方格會捕捉一些登陸區域的執行選項和變數，這些是可推動決策的因素。

<!-- docsTest:ignore "CAF Migration" "CAF Foundation" "CAF Enterprise-scale" "CAF Terraform" -->

| [執行] 選項 | 描述 | 部署速度 | 更深入的設計原則 | 部署指示 |
|---------|---------|---------|---------|---------|---------|---------|---------|
| [CAF 遷移](./migrate-landing-zone.md) | 部署用於遷移低風險資產的基本基礎 | 從小開始 | [設計原則](./migrate-landing-zone.md#design-principles) | [部署](./migrate-landing-zone.md) |
| [CAF 基礎](./foundation-blueprint.md) | 新增開始開發治理策略所需的最少工具 | 從小開始 | [設計原則](./foundation-blueprint.md#design-principles) | [部署](./foundation-blueprint.md) |
| [CAF 企業規模](./enterprise-scale.md) | 使用所有必要的共用服務來部署企業就緒的平臺基礎，以支援完整的 IT 組合。 | 企業規模 | [設計原則](../enterprise-scale/design-principles.md) | [部署](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/contoso/Readme.md) |
| [CAF Terraform](./terraform-landing-zone.md) | 多重雲端作業模型的協力廠商路徑。 此路徑可以限制 Azure 優先的作業模型。 | 從小開始 | [設計原則](./terraform-landing-zone.md#design-decisions) | [部署](./terraform-landing-zone.md#customize-and-deploy-your-first-landing-zone) |

下表從稍微不同的觀點來探討相同的執行選項，以引導更多技術決策流程。

| [執行] 選項 | 中樞 | 分支 | 部署技術 | 部署指示 |
|---|---|---|---|---|
| [CAF 企業規模](./enterprise-scale.md) | 已包括  | 已包括 | Azure Resource Manager 範本、Azure 入口網站、Azure 原則和 GitHub | [部署](../enterprise-scale/implementation-guidelines.md) |
| [CAF 遷移](./migrate-landing-zone.md) | 需要重構 | 已包括 | Azure Resource Manager 範本、Azure 入口網站和 Azure 藍圖 | [部署](./migrate-landing-zone.md) |
| [CAF Terraform](./terraform-landing-zone.md)  | 包含在虛擬資料中心模組中 | 已包括 | Terraform | [部署](./terraform-landing-zone.md#customize-and-deploy-your-first-landing-zone) |

## <a name="next-steps"></a>後續步驟

若要繼續，請選擇上面的其中一個 [執行] 選項。 每個選項都包含部署指示的連結，以及將引導執行的特定設計原則。
