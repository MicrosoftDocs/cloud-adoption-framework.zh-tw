---
title: 登陸區域的執行選項
description: 判斷哪一個登陸區域的執行選項最適合您的需求。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: fada3a1485c470e04562365fe4f0a038c121708c
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88569649"
---
# <a name="landing-zone-implementation-options"></a>登陸區域的執行選項

[Azure 登陸區域](./index.md) 為雲端採用小組提供妥善管理的環境來處理其工作負載。 遵循 [登陸區域設計區域](./design-areas.md) 指引，以利用這些功能。

下列每個執行選項都是針對一組特定的作業模型相依性所設計，以支援您的非功能性需求。 每個實作為選項都包含不同的自動化方法。 如果有，則會包含參考架構和參考，以加速您的雲端採用旅程。 雖然每個選項都對應至不同的作業模型，但它們共用相同的設計區域。 不同之處在于您選擇如何執行這些專案。

## <a name="platform-development-velocity"></a>平臺開發速度

除了建議的設計領域之外，您的平臺開發速度 (您的平臺小組開發所需技能的速度) 是選擇最佳部署選項時的關鍵因素。 請考慮兩個主要模式：

**從企業規模開始：** 如果您的業務需求需要從一開始就完全整合的治理、安全性和作業，進行登陸區域的豐富初始實行，請使用此模式。 使用此方法時，您可以使用 Azure 入口網站或基礎結構即程式碼來設定和設定您的環境。 您也可以在您的組織準備就緒時，于入口網站和基礎結構即程式碼 (建議的) 之間進行轉換。 基礎結構即程式碼的方法需要 Azure Resource Manager 範本和 GitHub 的技能。

**從小規模開始，並展開：** 如果在您深入瞭解雲端時，更重要的是開發這些技能並認可您的決策，請使用此模式。 在此方法中，登陸區域只著重在實行開始採用雲端所需的基本登陸區域考慮。 隨著您的採用擴充，管理和管理方法中的課程模組將會建立在初始登陸區域之上。 任何 Azure 登陸區域的設計原則都會概述將需要重構一段時間的特定設計領域。

## <a name="implementation-options"></a>實作選項

下表說明登陸區域的一些實作為選項，以及可能會推動決策的變數。

| 實選項 | 描述 | 部署速度 | 更深入的設計原則 | 部署指示 |
|---|---|---|---|---|
| [雲端採用架構遷移登陸區域藍圖](./migrate-landing-zone.md) | 部署用於遷移低風險資產的基本基礎。 | 從小規模開始 | [設計原則](./migrate-landing-zone.md#design-principles) | [部署](./migrate-landing-zone.md) |
| [雲端採用架構基礎藍圖](./foundation-blueprint.md) | 新增開始開發治理策略所需的基本工具。 | 從小規模開始 | [設計原則](./foundation-blueprint.md#design-principles) | [部署](./foundation-blueprint.md) |
| [雲端採用架構企業規模登陸區域](../enterprise-scale/index.md) | 使用所有必要的共用服務來部署符合企業需求的平臺基礎，以支援完整的 IT 組合。 | 企業規模 | [設計原則](../enterprise-scale/design-principles.md) | [部署](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/contoso/Readme.md) |
| [雲端採用架構 Terraform 課程模組](./terraform-landing-zone.md) | 多重雲端作業模型的協力廠商路徑。 此路徑可以限制 Azure 優先的作業模型。 | 從小規模開始 | [設計原則](./terraform-landing-zone.md#design-decisions) | [部署](./terraform-landing-zone.md#customize-and-deploy-your-first-landing-zone) |

下表以稍微不同的觀點來查看相同的實選項，以引導更多技術決策流程。

| 實選項 | 集線器 | 說話 | 部署技術 | 部署指示 |
|---|---|---|---|---|
| [雲端採用架構企業規模登陸區域](../enterprise-scale/index.md) | 已包括  | 已包括 | Azure Resource Manager 範本、Azure 入口網站、Azure 原則和 GitHub | [部署](../enterprise-scale/implementation-guidelines.md) |
| [雲端採用架構遷移登陸區域藍圖](./migrate-landing-zone.md) | 需要重構 | 已包括 | Azure Resource Manager 範本、Azure 入口網站和 Azure 藍圖 | [部署](./migrate-landing-zone.md) |
| [雲端採用架構 Terraform 課程模組](./terraform-landing-zone.md)  | 包含在虛擬資料中心模組中 | 已包括 | Terraform | [部署](./terraform-landing-zone.md#customize-and-deploy-your-first-landing-zone) |

## <a name="next-steps"></a>後續步驟

若要繼續，請選擇上表所示的其中一個 [執行] 選項。 每個選項都包含部署指示的連結，以及引導執行的特定設計原則。
