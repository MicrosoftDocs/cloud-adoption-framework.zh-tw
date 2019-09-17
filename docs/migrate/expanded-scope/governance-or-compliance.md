---
title: 治理或合規性策略
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 治理或合規性策略
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 6c1ac4000f5b79d6b177e8703f5e58b6dd9c9e56
ms.sourcegitcommit: 443c28f3afeedfbfe8b9980875a54afdbebd83a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2019
ms.locfileid: "71022740"
---
# <a name="governance-or-compliance-strategy"></a>治理或合規性策略

在整個移轉工作中需要治理或合規性時，就會需要額外的範圍。 下列指引將擴充 [Azure 移轉指南](../azure-migration-guide/index.md)的範圍，以說明用來解決治理或合規性需求的不同方法。

## <a name="general-scope-expansion"></a>一般範圍擴充

當需要治理或合規性時，必要條件活動所受到的影響最大。 在評估、移轉和最佳化期間可能需要進行其他調整。

## <a name="suggested-prerequisites"></a>建議的必要條件

在整合治理或合規性需求時，基本 Azure 環境的設定可能會大幅變更。 若要了解必要條件的變更情形，請務必了解需求的本質。 在開始進行任何需要治理或合規性的移轉之前，應該先在雲端環境中選擇並實作方法。 以下是一些在移轉期間經常會看到的高階方法：

**常見的治理方法：** 對於大部分的組織而言，[雲端採用架構治理模型](../../govern/guides/index.md)方法便已足夠，此方法會包含最簡可行產品 (MVP) 實行，隨後並有治理成熟度的目標反覆項目可供解決採用方案中所識別的有形風險。 此方法會提供建立一致性治理所需的最低限度工具，因此小組能夠了解這些工具。 接著，此方法會詳述這些用來解決常見治理顧慮的工具。

**ISO 27001 合規性藍圖：** 對於必須遵守 ISO 合規性標準的客戶，[ISO 27001 共用服務藍圖範例](https://docs.microsoft.com/azure/governance/blueprints/samples/iso27001-shared/index)可作為更有效的 MVP 以在反覆性程序中及早產生更豐富的治理條件約束。 [ISO 27001 App Service 環境/SQL Database 範例](https://docs.microsoft.com/azure/governance/blueprints/samples/iso27001-ase-sql-workload)會詳述可對應控制項並為應用程式環境部署通用架構的藍圖。 當其他合規性藍圖發行時，我們也會在這裡提供相關參考。

**虛擬資料中心：** 您可能需要更強固的治理起點。 在這類情況下，請考慮 [Azure 虛擬資料中心 (VDC)](../../reference/vdc.md)。 在進行企業規模的採用工作期間，特別是超過 10,000 個資產的工作，我們通常會建議您使用此方法。 此方法也是有下列任何需要的複雜治理案例所存在的既定選擇：廣泛的第三方合規性需求、深度網域專業知識或與成熟 IT 治理原則和合規性保持對應的需求。

### <a name="partnership-option-to-complete-prerequisites"></a>可供完成必要條件的合作關係選項

**Microsoft 服務：** Microsoft 服務會提供可配合雲端採用架構治理模型、合規性藍圖或虛擬資料中心選項的解決方案供應項目，以確保有最適當的治理或合規性模型。 請使用[安全雲端深入解析 (SCI)](https://download.microsoft.com/download/C/7/C/C7CEA89D-7BDB-4E08-B998-737C13107361/Secure_Cloud_Insights_Datasheet_EN_US.pdf) 解決方案供應項目以建立客戶在 Azure 中部署的資料驅動圖，並在識別現有部署架構的最佳化時驗證客戶的 Azure 實作成熟度，以移除治理的安全性和可用性風險。 根據客戶深入解析，您應該從下列方法開始：

- **雲端基礎：** 使用[混合式雲端基礎 (HCF)](https://download.microsoft.com/download/D/8/7/D872DFD0-1C46-4145-95E4-B5EAB2958B96/Hybrid_Cloud_Foundation_Datasheet_EN_US.pdf) 解決方案供應項目來建立客戶的核心 Azure 設計、模式和治理架構。 將客戶的需求對應至最適當的參考架構。 實作包含共用服務和 IaaS 工作負載的最簡可行產品。
- **雲端現代化：** 請使用[雲端現代化](https://download.microsoft.com/download/3/7/3/373F90E3-8568-44F3-B096-CD9C1CD28AB7/Cloud_Modernization_Datasheet_EN_US.pdf)解決方案供應項目作為完整方法，來將應用程式、資料和基礎結構移至符合企業需求的雲端，以及在該雲端中進行一次最佳化和現代化。
- **使用雲端來創新：** 透過一套創新且獨特[的雲端中心卓越（CCoE）](https://download.microsoft.com/download/F/8/B/F8BBE4BD-E5F8-4DFB-82F7-C0A4E17051BB/Cloud_Center_of_Excellence_Datasheet_EN_US.pdf)解決方案方法來與客戶互動，以建立現代化的 IT 組織，同時保有 DevOps 的彈性，同時保有控制能力。 實作敏捷方法來擷取業務需求、重複使用可與安全性、合規性和服務管理原則配合的部署套件，並維護可與操作程序配合的 Azure 平台。

## <a name="assess-process-changes"></a>評估程序變更

評估期間需要額外的決策以配合所需的治理方法。 雲端治理小組應該先為雲端採用小組的所有成員提供原則聲明、架構指引或治理/合規性需求，再評估工作負載。

### <a name="suggested-action-during-the-assess-process"></a>評定程序進行期間的建議動作

治理和合規性評估需求是極度專屬於客戶的，因此無法針對評估期間所採取的實際步驟提供一般性的指引。 不過，建議您在程序中納入工作和時間配置，以「配合合規性/治理需求」。 若要進一步了解這些需求，請參閱下列連結：

若要深入了解治理，請檢閱[雲端治理的五個專業領域](../../govern/governance-disciplines.md)。 雲端採用架構的這個區段也包含範本，範本內會記載這五個區段各自的原則、指引和需求：

- [成本管理](../../govern/cost-management/template.md)
- [安全性基準](../../govern/security-baseline/template.md)
- [資源一致性]。/../govern/resource-consistency/template.md)
- [身分識別基準].。/../govern/identity-baseline/template.md)
- [部署加速](../../govern/deployment-acceleration/template.md)

如需根據雲端採用架構治理模型來開發治理指引的相關指引，請參閱[實作雲端治理策略](../../govern/corporate-policy.md)。

## <a name="optimize-and-promote-process-changes"></a>將程序變更最佳化並升階

在最佳化和升階程序進行期間，建議雲端治理小組花點時間來測試及驗證其策略是否遵循治理和合規性標準。 此外，您也可以在這個步驟插入程序來讓雲端治理小組擷取範本，以便為未來的專案提供額外的[部署加速](../../govern/deployment-acceleration/index.md)機制。

### <a name="suggested-action-during-the-optimize-and-promote-process"></a>最佳化和升階程序期間的建議動作

在此程序進行期間，建議於專案計劃內納入時間配置，以供雲端治理小組對規劃要進行生產升階的每個工作負載執行合規性檢閱。

## <a name="next-steps"></a>後續步驟

作為[擴充的範圍檢查清單](./index.md)的最後一個項目，建議讀者回到檢查清單並重新評估移轉工作的任何其他範圍需求。

> [!div class="nextstepaction"]
> [擴充的範圍檢查清單](./index.md)
