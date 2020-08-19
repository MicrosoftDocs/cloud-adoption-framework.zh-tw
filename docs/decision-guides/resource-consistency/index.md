---
title: 資源一致性決策指南
description: 了解雲端資產資源一致性的重要性，以及推動對資源一致性需求的各種因素。
author: doodlemania2
ms.author: dermar
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: decision-guide
ms.custom: governance
ms.openlocfilehash: edeb9aa24b57741bb81d834384b8b5d3a01083da
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88574086"
---
# <a name="resource-consistency-decision-guide"></a>資源一致性決策指南

Azure [訂用帳戶設計](../subscriptions/index.md)可依您組織的結構、會計實務及工作負載需求，定義雲端資產的組織方式。 除了此結構層級，跨雲端資產處理組織治理原則需求，還需要能在訂用帳戶內，以一致的方式組織、部署及管理資源。

![規劃符合下列快速連結的資源一致性選項 (從最簡單到最複雜)](../../_images/decision-guides/decision-guide-resource-consistency.png)

跳至：[基本群組](#basic-grouping) | [部署一致性](#deployment-consistency) | [原則一致性](#policy-consistency) | [階層式一致性](#hierarchical-consistency) | [自動化的一致性](#automated-consistency)

和雲端資產的資源一致性需求層級相關的決策，主要取決於下列因素：移轉後的數位資產大小、不符合您現有訂用帳戶設計方式的業務或環境需求，或需要在部署資源之後持續強制執行治理。

隨著這些因素的重要性提高，確保雲端資源部署、分組和管理一致性的優點也變得更加重要。 想達到更高階的資源一致性層級以滿足不斷增加的需求，需要付出更多心力強制使用自動化、工具及一致性，也因此要在變更管理和追蹤上花費額外的時間。

## <a name="basic-grouping"></a>基本分組

在 Azure 中，[資源群組](/azure/azure-resource-manager/management/overview#resource-groups)是以邏輯方式將訂用帳戶內的資源分組的核心資源組織機制。

資源群組會作為具有一般生命週期和共用管理條件約束 (例如原則或角色型存取控制 (RBAC) 要求) 的資源容器。 資源群組不能建立巢狀結構，且資源只能屬於一個資源群組。 所有控制平面動作都會對資源群組中的所有資源採取行動。 例如，刪除資源群組也會刪除該群組內的所有資源。 資源群組管理的慣用模式是考慮：

1. 資源群組的內容是否一起開發？
1. 資源群組的內容是否由相同的人員或小組一起管理、更新和監視？
1. 資源群組的內容是否一起淘汰？

如果您對上述任一點回答了「否」，則應該將有問題的資源放在另一個資源群組中的其他位置。

> [!IMPORTANT]
> 資源群組也是區域專屬的項目；不過，資源常常會在相同資源群組內的不同區域中，因為其會以如上所述方式一起接受管理。 如需如何選擇區域的詳細資訊，請參閱[多個區域](../../migrate/azure-best-practices/multiple-regions.md)。

## <a name="deployment-consistency"></a>部署一致性

Azure 平台皆建置在基本的資源分組機制之上，提供的系統可使用範本將資源部署至雲端環境。 您可以在部署工作負載時使用範本建立一致的組織和命名慣例，強制執行您資源部署和管理設計的各個層面。

[Azure Resource Manager 範本](/azure/azure-resource-manager/templates/overview)可讓您使用預先決定的組態和資源群組結構，以一致的狀態重複部署資源。 Resource Manager 範本可協助您定義一組標準作為部署基礎。

例如，您可以使用標準範本部署，將包含兩個虛擬機器的 Web 伺服器工作負載，部署為結合負載平衡器的 Web 伺服器，以分散伺服器之間的流量。 然後只要需要這類型的工作負載，就可以重複使用此範本建立結構完全相同的虛擬機器與負載平衡器組合，僅需變更涉及的部署名稱和 IP 位址。

您也可以用程式設計方式部署這些範本，並將它們與您的 CI/CD 系統整合。

## <a name="policy-consistency"></a>原則一致性

為確保在建立資源時套用治理原則，資源群組設計的一部份會涉及使用一般組態部署資源。

您可以透過結合資源群組和標準化 Resource Manager 範本，針對部署時需要哪些設定，以及要對每個資源群組或資源套用哪些 [Azure 原則](/azure/governance/policy/overview)規則等需求，強制執行適用標準。

例如，您可能需要讓訂用帳戶內部署的所有虛擬機器皆連線到由您的集中式 IT 小組所管理的一般子網路。 您可以建立標準範本，以用於部署工作負載 VM，針對工作負載建立個別資源群組，並在其中部署必要 VM。 此資源群組會有一項原則規則，限制只允許將資源群組內的網路介面加入至共用子網路。

如需有關在雲端部署內強制執行原則決策的更深入討論，請參閱[原則強制執行](../policy-enforcement/index.md)。

## <a name="hierarchical-consistency"></a>階層式一致性

資源群組可讓您在訂用帳戶內，透過在資源群組層級套用 Azure 原則規則和存取控制，以在組織內支援額外的階層層級。 當雲端資產的大小成長時，您可能需要支援比使用 Azure Enterprise 合約的企業/部門/帳戶/訂用帳戶階層時更複雜的跨訂用帳戶治理要求。

[Azure 管理群組](/azure/governance/management-groups)可讓您使用和 Enterprise 合約階層不同的階層來將訂用帳戶分組，將訂用帳戶組織成更複雜的組織結構。 這個替代階層可讓您跨多個訂用帳戶與當中包含的資源，套用存取控制和原則強制執行機制。 使用管理群組階層，可讓您的雲端資產訂用帳戶滿足作業或商務治理需求。 如需詳細資訊，請參閱[訂用帳戶決策指南](../subscriptions/index.md)。

## <a name="automated-consistency"></a>自動化的一致性

對於大型雲端部署而言，全域治理變得更加重要，而且更複雜。 務必要自動套用和部署資源時，強制執行控管需求，以及針對現有部署更新的需求。

[Azure 藍圖](/azure/governance/blueprints/overview)可讓組織支援 Azure 中大型雲端資產的全域治理。 藍圖超越了標準 Azure Resource Manager 範本所提供的功能，可建立完整且能夠部署資源及套用原則規則的部署協調流程。 藍圖支援下列功能：版本控制、更新使用藍圖之所有訂用帳戶的能力，以及鎖定已部署的訂用帳戶，藉此防止未經授權建立及修改資源。

這些部署套件可讓 IT 和開發團隊，快速部署符合變更組織性原則需求的新工作負載和網路資產。 藍圖也可以整合至 CI/CD 管線中，以在更新部署時套用修改過的治理標準。

## <a name="next-steps"></a>後續步驟

資源一致性只是雲端採用程序期間，其中一個需針對架構做出決策的核心基礎結構元件。 請瀏覽架構決策指南概觀，以了解在為其他類型的基礎結構制定設計決策時，使用的替代模式或模型。

> [!div class="nextstepaction"]
> [架構相關決策指南](../index.md)
