---
title: 追蹤營業單位、環境或專案之間的成本
description: 使用適用于 Azure 的雲端採用架構，瞭解建立追蹤機制的決策和實行方法。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/05/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 8b4ed49f9a6eea93a8f9b6095d315f60b3b7ef70
ms.sourcegitcommit: 57b757759b676a22f13311640b8856557df36581
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2020
ms.locfileid: "94997347"
---
# <a name="track-costs-across-business-units-environments-or-projects"></a>追蹤營業單位、環境或專案之間的成本

[建立符合成本效益的組織](../../organize/cost-conscious-organization.md)需要可見度和適當定義的存取權 (或範圍)，以符合成本相關的資料。 這篇最佳做法文章概述建立追蹤機制的決策和實作方法。

![符合成本效益的程式 ](../../_images/ready/cost-optimization-process.png)
 _圖1：符合成本的流程大綱。_

## <a name="establish-a-well-managed-environment-hierarchy"></a>建立妥善管理的環境階層

成本控制 (如治理和其他管理建構) 取決於妥善管理的環境。 在所有資產的分類和組織中建立這類環境 (尤其是一個複雜的環境) 需要一致的進程。

資產 (也稱為資源) 包括所有虛擬機器、資料來源，以及部署至雲端的應用程式。 Azure 提供數種機制來分類和組織資產。 [組織及管理您的 Azure](../azure-best-practices/organize-subscriptions.md) 訂用帳戶詳細資料選項，以根據多個準則來組織資源，以建立妥善管理的環境。 本文著重於 Azure 基本概念的應用，以提供雲端成本可見度。

### <a name="classification"></a>分類

「標記」是將資產分類的簡單方式。 標記會使中繼資料與資產產生關聯。 該中繼資料可以用來根據各種資料點以分類資產。 當使用標記來分類資產作為成本管理工作的一部分時，公司通常需要下列標籤：營業單位、部門、收費代碼、地理位置、環境、專案、工作負載或「應用程式分類」。 Azure 成本管理 + 計費可以使用這些標記來建立不同的成本資料檢視。

標記是了解任何成本報告中資料的主要方式。 它是任何妥善管理環境的基本部分。 這也是建立任何環境適當治理的第一步。

在營業單位、環境和專案之間準確追蹤成本資訊的第一個步驟，是定義標記標準。 第二個步驟是確定已一致地套用標記標準。 下列文章可協助您完成： [開發命名和標記標準](../azure-best-practices/naming-and-tagging.md) ，以及 [建立治理 MVP 以強制執行標記標準](../../govern/guides/complex/index.md)

### <a name="resource-organization"></a>資源組織

有數種方法可以組織資產。 本節概述的最佳做法，是以大型企業的需求為基礎，並將成本結構散佈在不同的營業單位、地理位置和 IT 組織。 [標準企業治理指南](../../govern/guides/standard/index.md)中提供較小、較不復雜組織的類似最佳作法。

針對大型企業，下列管理群組、訂用帳戶和資源群組的模型會建立一個階層，讓每個小組都能擁有正確的可見度層級來執行其職責。 當企業需要成本控制來避免預算溢出時，它可以將 Azure 藍圖或 Azure 原則這類治理工具套用到此結構內的訂用帳戶，以快速封鎖未來的成本錯誤。

![大型企業的資源組織圖 ](../../_images/govern/large-enterprise-resource-organization.png)
 _2：大型企業的資源組織。_

在上圖中，管理群組階層的根會包含每個營業單位的節點。 在此範例中，跨國公司需要了解區域營業單位，因此它會針對階層中每個營業單位下的地理位置建立一個節點。

在每個地理位置中，有一個適用於生產和非生產環境的個別節點，用以隔離成本、存取和治理控制項。 若要允許更有效率的作業和最睿智做法作業投資，公司會使用訂用帳戶來進一步隔離生產環境，以及不同程度的營運效能承諾。 最後，公司會使用資源群組來擷取函式的可部署單位，稱為應用程式。

此圖顯示最佳做法，但不包含下列選項：

- 許多公司會將作業限制為單一地緣政治區域。 這種方法可減少多樣化治理專業領域的需求，或根據當地資料主權的需求來計算成本資料。 在這些情況下，地理節點是不必要的。
- 有些公司想要進一步將開發、測試和品質控制環境隔離成不同的訂用帳戶。
- 當公司整合卓越雲端中心 (CCoE) 團隊時，每個地理節點中的共用服務訂用帳戶可以減少重複的資產。
- 較小的採用工作可能會有更小的管理階層。 通常會看到公司 IT 的單一根節點，並在不同環境的階層中具有單一層級的從屬節點。 這不會違反妥善管理環境的最佳做法。 但是，它會讓您更難以提供成本控制和其他重要功能的最低許可權存取模型。

本文的其餘部分假設您使用上圖中的最佳做法方法。 但是，下列文章可協助您將方法套用至最適合您公司的資源組織：

- [使用多個訂用帳戶調整您的 Azure 環境](../azure-best-practices/scale-subscriptions.md)
- [組織和管理您的 Azure 訂用帳戶](../azure-best-practices/organize-subscriptions.md)
- [部署治理 MVP 以管理妥善管理的環境標準](../../govern/guides/complex/index.md)

## <a name="provide-the-right-level-of-cost-access"></a>提供正確的成本存取層級

管理成本是小組活動。 雲端採用架構的 [組織整備程度] 區段會定義少數核心小組，並概述這些小組如何支援雲端採用工作。 本文會展開小組定義，以定義要指派給每個小組成員的範圍和角色，以取得成本管理資料的適當可見度層級。

「角色」會定義使用者可對各種資產執行哪些動作。 **範圍** 定義了哪些資產 (使用者、群組、服務主體或受控識別) 使用者可以對這些專案進行這些動作。

在一般的最佳做法中，我們會建議將人員指派給各種角色和範圍的最低許可權模型。

### <a name="roles"></a>角色

<!-- docutune:casing Owner Contributor Reader -->

Azure 成本管理 + 計費支援每個範圍的下列內建角色：

- [擁有](/azure/role-based-access-control/built-in-roles#owner)者：可以查看成本及管理所有專案，包括成本配置。
- [參與者](/azure/role-based-access-control/built-in-roles#contributor)：可以查看成本及管理所有專案，包括成本設定，但不包括存取控制。
- [讀者](/azure/role-based-access-control/built-in-roles#reader)：可以查看所有內容，包括成本資料和設定，但無法進行變更。
- [成本管理參與者](/azure/role-based-access-control/built-in-roles#cost-management-contributor)：可以查看成本及管理成本設定。
- [成本管理讀者](/azure/role-based-access-control/built-in-roles#cost-management-reader)：可以查看成本資料和設定。

一般的最佳做法是，所有小組成員都應該獲指派成本管理參與者的角色。 此角色會授與建立與管理預算和匯出的存取權，以更有效率地監視和報告成本。 但 [雲端策略小組](../../organize/cloud-strategy.md) 的成員只應設定為成本管理讀者。 這是因為它們並不涉及在 Azure 成本管理 + 計費工具內設定預算。

### <a name="scope"></a>影響範圍

下列範圍和角色設定會建立成本管理所需的可見度。 這種最佳做法可能需要較小的變更，以符合資產組織決策。

- [雲端採用小組](../../organize/cloud-adoption.md)。 進行中最佳化變更的責任需要資源群組層級的成本管理參與者存取權。

  - **工作環境。** 雲端採用小組至少應該已有所有受影響資源群組的[參與者](/azure/role-based-access-control/built-in-roles#contributor)存取權，或至少與開發/測試或進行中部署活動相關群組的存取權。 不需要其他範圍設定。
  - **生產環境。** 建立適當的責任分隔時，雲端採用小組可能不會繼續具備其專案相關資源群組的存取權。 支援工作負載生產執行個體的資源群組需要額外的範圍，讓此小組能夠看到其決策的生產成本影響。 為此小組的生產資源群組設定[成本管理參與者](/azure/role-based-access-control/built-in-roles#cost-management-contributor)範圍，可讓小組監視成本，並根據所支援工作負載的使用量和持續投資來設定預算。

- [雲端策略小組](../../organize/cloud-strategy.md)。 若要追蹤多個專案和營業單位的成本，必須在管理群組階層的根層級取得成本管理讀者存取權。

  - 在管理群組中，將[成本管理讀者](/azure/role-based-access-control/built-in-roles#cost-management-reader)存取權指派給此小組。 這可確保所有與該管理群組階層所治理訂用帳戶相關聯的部署持續可見。

- [雲端治理小組](../../organize/cloud-governance.md)。 負責管理成本、預算對齊，以及跨所有採用工作進行報告，需要管理群組階層根層級的成本管理參與者存取權。

  - 在妥善管理的環境中，雲端治理小組可能已經有更高程度的存取權，因此不需要為[成本管理參與者](/azure/role-based-access-control/built-in-roles#cost-management-contributor)進行額外的範圍指派。

<!-- cSpell:ignore automations -->

- [卓越的雲端中心](../../organize/cloud-center-of-excellence.md)。 負責管理與共用服務相關的成本，需要訂用帳戶層級的成本管理參與者存取權。 此外，此小組可能需要資源群組或訂用帳戶 (包含 CCoE 自動化所部署的資產) 的成本管理參與者存取權，以了解這些自動化如何影響成本。

  - **共用服務。** 當卓越的雲端中心參與時，最佳做法會建議中樞和輪輻模型內的集中式共用服務訂用帳戶支援 CCoE 所管理的資產。 在此案例中，CCoE 可能具有該訂用帳戶的參與者或擁有者存取權，因此不需要為 [成本管理參與者](/azure/role-based-access-control/built-in-roles#cost-management-contributor) 進行額外的範圍指派。
  - **CCoE 自動化/控制項。** CCoE 通常會為雲端採用小組提供控制項和自動化部署指令碼。 CCoE 有責任了解這些加速器如何影響成本。 若要取得該可見度，小組需要執行這些加速器的任何資源群組或訂用帳戶[成本管理參與者](/azure/role-based-access-control/built-in-roles#cost-management-contributor)存取權。

- **雲端營運團隊。** 負責管理生產環境的持續成本，需要所有生產訂用帳戶的成本管理參與者存取權。

  - 一般建議將生產和非生產資產放在個別的訂用帳戶中，這些訂用帳戶是由與生產環境相關聯的管理群組階層節點所控制。 在妥善管理的環境中，營運小組的成員可能已經有生產訂用帳戶的擁有者或參與者存取權，因此不需要 [成本管理參與者](/azure/role-based-access-control/built-in-roles#cost-management-contributor) 角色。

## <a name="additional-cost-management-resources"></a>額外的成本管理資源

Azure 成本管理 + 計費是妥善記載的工具，可用於設定預算，並取得 Azure 或 AWS 的雲端成本可見度。 在您建立妥善管理環境階層的存取權之後，下列文章可協助您使用該工具來監視和控制成本。

### <a name="get-started-with-azure-cost-management--billing"></a>開始使用 Azure 成本管理 + 計費

若要開始使用 Azure 成本管理 + 計費，請參閱 [如何使用 Azure 成本管理 + 計費來優化您的雲端投資](/azure/cost-management-billing/costs/cost-mgt-best-practices?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。

### <a name="use-azure-cost-management--billing"></a>使用 Azure 成本管理 + 計費

- [建立及管理預算](/azure/cost-management-billing/costs/tutorial-acm-create-budgets)
- [匯出成本資料](/azure/cost-management-billing/costs/tutorial-export-acm-data)
- [根據建議將成本優化](/azure/cost-management-billing/costs/tutorial-acm-opt-recommendations)
- [使用成本警示監視使用量和支出](/azure/cost-management-billing/costs/cost-mgt-alerts-monitor-usage-spending)

<!-- docutune: "AWS Cost and Usage" -->

### <a name="use-azure-cost-management--billing-to-govern-aws-costs"></a>使用 Azure 成本管理 + 計費來管理 AWS 成本

- [設定 AWS 成本和使用量報告整合](/azure/cost-management-billing/costs/aws-integration-set-up-configure)
- [管理 AWS 成本](/azure/cost-management/aws-integration-manage)

### <a name="establish-access-roles-and-scope"></a>建立存取權、角色和範圍

- [了解成本管理範圍](/azure/cost-management/understand-work-scopes)
- [設定資源群組的範圍](/azure/role-based-access-control/quickstart-assign-role-user-portal)
