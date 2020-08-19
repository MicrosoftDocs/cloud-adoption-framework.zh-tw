---
title: 治理或合規性策略
description: 治理或合規性策略
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: f38e8de88c38ef27ca6148bd5ff61c1dfdbc1b7e
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88574800"
---
# <a name="governance-or-compliance-strategy"></a>治理或合規性策略

當整個遷移工作需要治理或合規性時，您需要放寬範圍，以考慮這些需求。 下列指導方針將擴充 [Azure 遷移指南](../azure-migration-guide/index.md) 的範圍，以解決管理治理或合規性需求的不同方法。

## <a name="general-scope-expansion"></a>一般範圍擴充

當需要治理或合規性時，必要條件活動所受到的影響最大。 在評量、遷移和優化期間，也可能需要進行其他調整。

## <a name="suggested-prerequisites"></a>建議的必要條件

當您要整合治理或合規性需求時，基底 Azure 環境的設定可能會大幅改變。 若要了解必要條件的變更情形，請務必了解需求的本質。 在開始進行任何需要治理或合規性的遷移之前，您應該選擇並在雲端環境中執行一種方法。 以下是一些在移轉期間經常會看到的高階方法：

**常見的治理方法：** 對於大部分的組織而言， [雲端採用架構治理模型](../../govern/guides/index.md) 是一種足夠的方法。 它是由 (MVP) 實行的最小可行產品所組成，後面接著治理成熟度的目標反復專案，以解決採用計畫中所識別的有形風險。 此方法會提供建立一致性治理所需的最低限度工具，因此小組能夠了解這些工具。 接著，此方法會詳述這些用來解決常見治理顧慮的工具。

**國際標準組織 (ISO) 27001 合規性藍圖：** 如果您的組織需要遵守 ISO 合規性標準， [iso 27001 共用服務藍圖範例](/azure/governance/blueprints/samples/iso27001-shared) 可以成為更有效的 MVP。 藍圖可以產生更豐富的治理條件約束，稍早在反復的流程中。 [ISO 27001 App Service 環境/SQL Database 工作負載藍圖範例](/azure/governance/blueprints/samples/iso27001-ase-sql-workload)會在共用服務藍圖上展開，以對應控制項並部署應用程式環境的通用架構。

**雲端採用架構企業規模登陸區域：** 您可能需要更健全的治理起點。 如果有，請考慮採用 [雲端採用架構的企業規模登陸區域](../../ready/enterprise-scale/index.md)。 雲端採用架構的企業規模登陸區域方法著重于在24個月內擁有長期目標 (的採用小組) 裝載超過1000的資產 (應用程式、基礎結構或資料資產) 在雲端中。 雲端採用架構企業規模登陸區域是針對這些大型雲端採用工作的複雜治理案例*的選擇。*

### <a name="partnership-option-to-complete-prerequisites"></a>可供完成必要條件的合作關係選項

**Microsoft 服務：** Microsoft 服務提供的解決方案供應專案可符合雲端採用架構治理模型、合規性藍圖或雲端採用架構的企業規模登陸區域選項。 此選項可協助您確保使用的是最適當的治理或合規性模型。 使用 [安全的雲端深入](https://download.microsoft.com/download/C/7/C/C7CEA89D-7BDB-4E08-B998-737C13107361/Secure_Cloud_Insights_Datasheet_EN_US.pdf) 解析解決方案，為 Azure 中的客戶部署建立資料驅動的圖片。 此解決方案也會驗證客戶́s Azure 實現成熟度，同時識別現有部署架構的優化。 安全的 Cloud Insights 也可協助您降低與治理安全性和可用性有關的風險。 根據客戶深入解析，您應該從下列方法開始：

- **雲端基礎：** 使用 [混合式雲端基礎](https://download.microsoft.com/download/D/8/7/D872DFD0-1C46-4145-95E4-B5EAB2958B96/Hybrid_Cloud_Foundation_Datasheet_EN_US.pdf) 解決方案，建立客戶的核心 Azure 設計、模式和治理架構。 將客戶的需求對應至最適當的參考架構。 實行包含共用服務和 IaaS 工作負載的最小可行產品。
- **雲端現代化：** 使用 [雲端現代化](https://download.microsoft.com/download/3/7/3/373F90E3-8568-44F3-B096-CD9C1CD28AB7/Cloud_Modernization_Datasheet_EN_US.pdf) 解決方案作為將應用程式、資料和基礎結構移至符合企業需求之雲端的全方位方法。 您也可以在雲端部署之後優化和現代化。
- **使用雲端創新：** 透過 [雲端卓越的雲端中心 (CCoE) ](https://download.microsoft.com/download/F/8/B/F8BBE4BD-E5F8-4DFB-82F7-C0A4E17051BB/Cloud_Center_of_Excellence_Datasheet_EN_US.pdf) 解決方案來與客戶互動。 它會實行敏捷式方法來捕捉商務需求，並重複使用與安全性、合規性和服務管理原則一致的部署套件。 它也會維護 Azure 平臺與操作程式的對齊。

## <a name="assess-process-changes"></a>評定程序變更

在評估期間，您必須進行額外的決策，以配合所需的治理方法。 雲端治理小組會在評估工作負載之前，為雲端採用小組的所有成員提供任何原則聲明、架構指引或治理或合規性需求。

### <a name="suggested-action-during-the-assessment-process"></a>評估過程中的建議動作

治理和合規性評估需求是極度專屬於客戶的，因此無法針對評估期間所採取的實際步驟提供一般性的指引。 此流程應該包含符合合規性和治理需求的工作和時間。

若要深入瞭解治理，請閱讀 [雲端治理專業領域](../../govern/governance-disciplines.md)的總覽。 雲端採用架構的這一節包含範本，可記錄以下各節的原則、指引和需求：

- [成本管理專業領域](../../govern/cost-management/template.md)
- [安全性基準專業領域](../../govern/security-baseline/template.md)
- [資源一致性專業領域](../../govern/resource-consistency/template.md)
- [身分識別基準專業領域](../../govern/identity-baseline/template.md)
- [部署加速專業領域](../../govern/deployment-acceleration/template.md)

如需根據雲端採用架構治理模型開發治理指導方針的詳細資訊，請參閱 [實施雲端治理策略](../../govern/corporate-policy.md)。

## <a name="optimize-and-promote-process-changes"></a>將程序變更最佳化並升階

在優化和升階程式進行期間，雲端治理小組應投入時間來測試及驗證遵循治理和合規性標準。 此外，此步驟是讓雲端治理小組策展範本的好時機，以提供未來專案的其他指導方針，特別是在部署加速專業領域中。

### <a name="suggested-action-during-the-optimize-and-promote-process"></a>最佳化和升階程序期間的建議動作

在此過程中，專案計劃應該包含雲端治理小組針對生產升級規劃的每個工作負載執行合規性審查的時間配置。

## <a name="next-steps"></a>後續步驟

返回檢查清單，以重新評估遷移工作的任何其他範圍需求。

> [!div class="nextstepaction"]
> [遷移最佳做法檢查清單](./index.md)
