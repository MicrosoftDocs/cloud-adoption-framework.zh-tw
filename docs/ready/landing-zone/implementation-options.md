---
title: 登陸區域部署選項
description: 判斷哪一個登陸區域部署選項最適合您的需求。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 93b5f540e8b7a501a7491f406d15296f2245e6ee
ms.sourcegitcommit: 9b183014c7a6faffac0a1b48fdd321d9bbe640be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2020
ms.locfileid: "85076803"
---
# <a name="landing-zone-implementation-options"></a>登陸區域的執行選項

[Azure 登陸區域](./index.md)為雲端採用團隊提供了妥善管理的環境，可用於其工作負載。 遵循[登陸區域設計區域](./design-areas.md)指引來利用這些功能。

下列每個實施選項都是針對一組特定的作業模型相依性所設計，以支援您的非功能性需求。 每個實作為選項都包含不同的自動化方法。 如有提供，則會包含參考架構和參考，以加速您的雲端採用旅程。 雖然每個選項都會對應至不同的作業模型，但它們會共用相同的設計區域。 其差異在於您選擇要如何實行。

## <a name="platform-development-velocity"></a>平臺開發速度

除了建議的設計區域之外，選擇最佳部署選項時，您的平臺開發速度（平臺小組開發所需技能的速度有多快）是一項關鍵因素。 請考慮兩個主要模式：

**開始使用企業規模：** 當商務需求需要豐富的登陸區域初始執行功能時，從一開始就具備完全整合的管理、安全性和作業，Microsoft 建議採用企業級的方法。 使用此方法，您可以使用 Azure 入口網站或基礎結構即程式碼來設定您的環境。 當您的組織準備就緒時，您也可以在入口網站和基礎結構即程式碼之間轉換（建議選項）。 基礎結構即程式碼的方法需要 Azure Resource Manager 範本和 GitHub 中的技能。

**從小規模開始，並展開：** 如果在您深入瞭解雲端時開發這些技能並認可您的決策更為重要，請考慮使用更模組化的方法。 在此方法中，登陸區域只會著重于執行啟動雲端採用所需的基本登陸區域考慮。 隨著採用的擴大，管理和管理方法中的模組將會建置於初始登陸區域的最上層。 任何 Azure 登陸區域的設計原則，會概述將需要在一段時間內重構的特定設計區域。

## <a name="implementation-options"></a>實作選項

登陸區域的一些執行選項以及可能驅動決策的變數如下所示。

<!-- docsTest:ignore "CAF Enterprise-scale" "CAF Terraform" -->

| [執行] 選項 | 描述 | 部署速度 | 更深入的設計原則 | 部署指示 |
|---|---|---|---|---|---|---|---|
| [CAF 移轉登陸區域藍圖](./migrate-landing-zone.md) | 部署用於遷移低風險資產的基本基礎。 | 從小開始 | [設計原則](./migrate-landing-zone.md#design-principles) | [部署](./migrate-landing-zone.md) |
| [CAF 基礎藍圖](./foundation-blueprint.md) | 新增開始開發治理策略所需的最小工具。 | 從小開始 | [設計原則](./foundation-blueprint.md#design-principles) | [部署](./foundation-blueprint.md) |
| [CAF 企業規模登陸區域](./enterprise-scale.md) | 使用所有必要的共用服務來部署企業就緒的平臺基礎，以支援完整的 IT 組合。 | 企業規模 | [設計原則](../enterprise-scale/design-principles.md) | [部署](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/contoso/Readme.md) |
| [CAF Terraform 模組](./terraform-landing-zone.md) | 多重雲端作業模型的協力廠商路徑。 此路徑可以限制 Azure 優先的作業模型。 | 從小開始 | [設計原則](./terraform-landing-zone.md#design-decisions) | [部署](./terraform-landing-zone.md#customize-and-deploy-your-first-landing-zone) |

下表從稍微不同的觀點來探討相同的執行選項，以引導更多技術決策流程。

| [執行] 選項 | 中樞 | 分支 | 部署技術 | 部署指示 |
|---|---|---|---|---|
| [CAF 企業規模登陸區域](./enterprise-scale.md) | 已包括  | 已包括 | Azure Resource Manager 範本、Azure 入口網站、Azure 原則和 GitHub | [部署](../enterprise-scale/implementation-guidelines.md) |
| [CAF 移轉登陸區域藍圖](./migrate-landing-zone.md) | 需要重構 | 已包括 | Azure Resource Manager 範本、Azure 入口網站和 Azure 藍圖 | [部署](./migrate-landing-zone.md) |
| [CAF Terraform 模組](./terraform-landing-zone.md)  | 包含在虛擬資料中心模組中 | 已包括 | Terraform | [部署](./terraform-landing-zone.md#customize-and-deploy-your-first-landing-zone) |

## <a name="next-steps"></a>後續步驟

若要繼續，請選擇上面的其中一個 [執行] 選項。 每個選項都包含部署指示的連結，以及引導執行的特定設計原則。
