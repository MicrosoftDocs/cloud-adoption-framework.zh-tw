---
title: 治理或合規性策略
description: 治理或合規性策略
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 101c4a962583f522bedc8e6fa5d99587141871c1
ms.sourcegitcommit: d88c1cc3597a83ab075606d040ad659ac4b33324
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/15/2020
ms.locfileid: "84784999"
---
# <a name="governance-or-compliance-strategy"></a>治理或合規性策略

在整個移轉工作中需要治理或合規性時，就會需要額外的範圍。 下列指引將擴充 [Azure 移轉指南](../azure-migration-guide/index.md)的範圍，以說明用來解決治理或合規性需求的不同方法。

## <a name="general-scope-expansion"></a>一般範圍擴充

當需要治理或合規性時，必要條件活動所受到的影響最大。 在評估、移轉和最佳化期間可能需要進行其他調整。

## <a name="suggested-prerequisites"></a>建議的必要條件

在整合治理或合規性需求時，基本 Azure 環境的設定可能會大幅變更。 若要了解必要條件的變更情形，請務必了解需求的本質。 在開始進行任何需要治理或合規性的移轉之前，應該先在雲端環境中選擇並實作方法。 以下是一些在移轉期間經常會看到的高階方法：

**常見的治理方法：** 對於大部分的組織而言，[雲端採用架構治理模型](../../govern/guides/index.md)是一種充分的方法，其中包含最基本的可行產品（MVP）實行，並遵循治理成熟度的目標反復專案來解決採用計畫中所識別的有形風險。 此方法會提供建立一致性治理所需的最低限度工具，因此小組能夠了解這些工具。 接著，此方法會詳述這些用來解決常見治理顧慮的工具。

**ISO 27001 合規性藍圖：** 對於必須遵守 ISO 合規性標準的客戶， [iso 27001 共用服務藍圖範例](https://docs.microsoft.com/azure/governance/blueprints/samples/iso27001-shared)可做為更有效的 MVP，以在稍早的反復程式中產生更豐富的治理條件約束。 [ISO 27001 App Service 環境/SQL Database 範例](https://docs.microsoft.com/azure/governance/blueprints/samples/iso27001-ase-sql-workload)會詳述可對應控制項並為應用程式環境部署通用架構的藍圖。 當其他合規性藍圖發行時，我們也會在這裡提供相關參考。

**CAF 企業規模登陸區域：** 可能需要更健全的治理起點。 在這種情況下，請考慮[CAF 的企業規模登陸區域](../../ready/enterprise-scale/index.md)。 CAF 企業級的登陸區域方法著重于擁有長期目標的採用小組（在24個月內），以在雲端裝載1000以上的資產（應用程式、基礎或資料資產）。 CAF 企業級登陸區域是針對這些較大型雲端採用工作的複雜治理案例所做的選擇。

### <a name="partnership-option-to-complete-prerequisites"></a>可供完成必要條件的合作關係選項

**Microsoft 服務：** Microsoft 服務提供的解決方案供應專案可與雲端採用架構治理模型、合規性藍圖或 CAF 企業級登陸區域選項保持一致，以確保最適當的治理或合規性模型。 請使用[安全雲端深入解析 (SCI)](https://download.microsoft.com/download/C/7/C/C7CEA89D-7BDB-4E08-B998-737C13107361/Secure_Cloud_Insights_Datasheet_EN_US.pdf) 解決方案供應項目以建立客戶在 Azure 中部署的資料驅動圖，並在識別現有部署架構的最佳化時驗證客戶的 Azure 實作成熟度，以移除治理的安全性和可用性風險。 根據客戶深入解析，您應該從下列方法開始：

- **雲端基礎：** 使用[混合式雲端基礎（HCF）](https://download.microsoft.com/download/D/8/7/D872DFD0-1C46-4145-95E4-B5EAB2958B96/Hybrid_Cloud_Foundation_Datasheet_EN_US.pdf)解決方案供應專案，建立客戶的核心 Azure 設計、模式和治理架構。 將客戶的需求對應至最適當的參考架構。 實作包含共用服務和 IaaS 工作負載的最簡可行產品。
- **雲端現代化：** 使用[雲端現代化](https://download.microsoft.com/download/3/7/3/373F90E3-8568-44F3-B096-CD9C1CD28AB7/Cloud_Modernization_Datasheet_EN_US.pdf)解決方案供應專案做為將應用程式、資料和基礎結構移至企業級雲端的完整方法，以及在雲端部署之後優化和現代化。
- **使用雲端創新：** 透過一套創新且獨特[的雲端中心卓越（CCoE）](https://download.microsoft.com/download/F/8/B/F8BBE4BD-E5F8-4DFB-82F7-C0A4E17051BB/Cloud_Center_of_Excellence_Datasheet_EN_US.pdf)解決方案方法來與客戶互動，以建立現代化的 IT 組織，同時保有 DevOps 的彈性，同時保有控制能力。 實作敏捷方法來擷取業務需求、重複使用可與安全性、合規性和服務管理原則配合的部署套件，並維護可與操作程序配合的 Azure 平台。

## <a name="assess-process-changes"></a>評定程序變更

評估期間需要額外的決策以配合所需的治理方法。 雲端治理小組應該先為雲端採用小組的所有成員提供原則聲明、架構指引或治理/合規性需求，再評估工作負載。

### <a name="suggested-action-during-the-assess-process"></a>評定程序進行期間的建議動作

治理和合規性評估需求是極度專屬於客戶的，因此無法針對評估期間所採取的實際步驟提供一般性的指引。 不過，此程式應該包含「符合規範/治理需求的一致」的工作和時間配置。 若要進一步了解這些需求，請參閱下列連結：

若要深入瞭解治理，請閱讀[雲端治理的五個專業領域](../../govern/governance-disciplines.md)的總覽。 雲端採用架構的這一節包含的範本，可記錄五個區段的原則、指引和需求：

- [成本管理專業領域](../../govern/cost-management/template.md)
- [安全性基準專業領域](../../govern/security-baseline/template.md)
- [資源一致性專業領域](../../govern/resource-consistency/template.md)
- [身分識別基準專業領域](../../govern/identity-baseline/template.md)
- [部署加速專業領域](../../govern/deployment-acceleration/template.md)

如需根據雲端採用架構治理模型來開發治理指引的詳細資訊，請參閱[實施雲端治理策略](../../govern/corporate-policy.md)。

## <a name="optimize-and-promote-process-changes"></a>將程序變更最佳化並升階

在優化和升級過程中，雲端治理小組應該投入時間來測試及驗證遵循治理和合規性標準。 此外，此步驟是插入雲端治理小組的處理常式以策展範本的好時機，可以為未來的專案提供額外的[部署加速專業領域](../../govern/deployment-acceleration/index.md)。

### <a name="suggested-action-during-the-optimize-and-promote-process"></a>最佳化和升階程序期間的建議動作

在此過程中，專案計劃應該包含雲端治理小組的時間配置，以針對規劃生產升級的每個工作負載執行合規性檢查。

## <a name="next-steps"></a>後續步驟

作為[遷移最佳做法檢查清單](./index.md)的最後一個專案，請回到檢查清單，然後重新評估遷移工作的任何其他範圍需求。

> [!div class="nextstepaction"]
> [遷移最佳做法檢查清單](./index.md)
