---
title: 定義您的標記策略
description: 查看 Azure 資源和資產的標記建議。
author: BrianBlanchard
ms.author: brblanch
ms.date: 12/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: internal, readiness, fasttrack-edit
ms.openlocfilehash: 9e8bad823dde6414fe547a7537dd290362637c18
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102115708"
---
# <a name="define-your-tagging-strategy"></a>定義您的標記策略

將中繼資料標記套用到雲端資源時，您可以包含資源名稱中無法包含的資產相關資訊。 您可以使用該資訊來對資源執行更複雜的篩選和報告。 建議您讓這些標記包含資源相關的工作負載或應用程式、營運需求和擁有權資訊等內容。 IT 或業務小組可以使用這些資訊來尋找資源，或產生資源使用量和計費的相關報告。

您要套用至資源的標記，以及必要標記或選擇性標記的判定，會因為組織不同而有所差異。 下列清單提供可捕捉資源重要內容和相關資訊的常見標記範例。 請使用此清單作為建立您自有標記慣例的起點。

## <a name="minimum-suggested-tags"></a>建議標記的最小值

下列標記將引導所有後續的雲端採用架構方法中的執行和流程。 這些方法中的許多最佳作法會根據下列標記來示範雲端作業和治理的自動化。

| 標記名稱 | 描述 | 索引鍵和範例值 |
|--|--|--|
| **工作負載名稱** | 資源支援的工作負載名稱。 | *WorkloadName* <br><br> <li> `ControlCharts` |
| **資料分類** | 此資源所裝載資料的敏感度。 | *DataClassification* <br><br> <li> `Non-business` <li> `Public` <li> `General` <li> `Confidential` <li> `Highly confidential` |
| **業務關鍵性** | 資源或支援的工作負載的業務影響。 | *重要性* <br><br> <li> `Low` <li> `Medium` <li> `High` <li> `Business unit-critical` <li> `Mission-critical` |
| **業務單位** | 您公司的最上層部門，擁有資源所屬的訂用帳戶或工作負載。 在小型組織中，此標記可能是單一公司或共用的頂層組織元素。 | *BusinessUnit* <br><br> <li> `Finance` <li> `Marketing` <li> `Product XYZ` <li> `Corp` <li> `Shared` |
| **作業承諾** | 為此工作負載或資源提供的作業支援層級。 | *OpsCommitment* <br><br> <li> `Baseline only` <li> `Enhanced baseline` <li> `Platform operations` <li> `Workload operations` |
| **營運小組** | 小組負責每日作業。 | *OpsTeam* <br><br> <li> `Central IT` <li> `Cloud operations` <li> `ControlCharts team` <li> `MSP-{Managed Service Provider name}` |

## <a name="additional-common-tagging-examples"></a>其他常見的標記範例

以下是在 Azure 中經常使用的一些標籤，以提升 Azure 資源使用量的可見度。

| 標記名稱 | 描述 | 索引鍵和範例值 |
|--|--|--|
| **應用程式名稱** | 如果工作負載是在多個應用程式或服務之間進行細分，則會增加資料細微性。 | *ApplicationName* <br><br> <li> `IssueTrackingSystem` |
| **核准者名稱** | 負責核准此資源相關成本的人員。 | *核准者* <br><br> <li> `chris@contoso.com` |
| **需要的/核准的預算** | 配置給此應用程式、服務或工作負載的金額。 | *BudgetAmount* <br><br> <li> `$200,000` |
| **成本中心** | 與此資源相關聯的會計成本中心。 | *CostCenter* <br><br> <li> `55332` |
| **災害復原** | 應用程式、工作負載或服務的業務關鍵性。 | *DR* <br><br> <li> `Mission-critical` <li> `Critical` <li> `Essential` |
| **專案的結束日期** | 排定淘汰應用程式、工作負載或服務的日期。 | *日期* <br><br> <li> `2023-10-15` |
| **環境** | 應用程式、工作負載或服務的部署環境。 | *Env* <br><br> <li> `Prod` <li> `Dev` <li> `QA` <li> `Stage` <li> `Test` |
| **擁有者名稱** | 應用程式、工作負載或服務的擁有者。 | *擁有者* <br><br> <li> `jane@contoso.com` |
| **要求者名稱** | 要求建立此應用程式的使用者。 | *要求者* <br><br> <li> `john@contoso.com` |
| **服務類別** | 應用程式、工作負載或服務的服務等級協定層級。 | *ServiceClass* <br><br> <li> `Dev` <li> `Bronze` <li> `Silver` <li> `Gold` |
| **專案的開始日期** | 初次部署應用程式、工作負載或服務的日期。 | *起始* <br><br> <li> `2020-10-15` |

## <a name="take-action"></a>採取動作

請參閱 [資源命名和標記決策指南](../../decision-guides/resource-tagging/index.md)。

## <a name="next-steps"></a>下一步

瞭解如何在 Azure 中的訂用帳戶之間移動資源群組和資產。

> [!div class="nextstepaction"]
> [在訂用帳戶之間移動資源群組和資產](/azure/azure-resource-manager/management/move-resource-group-and-subscription)
