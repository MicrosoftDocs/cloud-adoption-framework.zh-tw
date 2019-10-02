---
title: 在 Azure 中部署移轉登陸區域
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 了解如何在 Azure 中部署移轉登陸區域。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/27/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: fasttrack-edit
ms.openlocfilehash: 329274859f50aa83ebb90e79597fa1ffe0973ab8
ms.sourcegitcommit: 1dccf1aed8e98aa0f58c4f86d90c65f5fa5ac84d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/02/2019
ms.locfileid: "71811113"
---
# <a name="deploy-a-migration-landing-zone"></a>部署移轉登陸區域

「移轉登陸區域」一詞用來描述一個環境，該環境已佈建並準備好裝載從內部部署環境遷移至 Azure 的工作負載。 移轉登陸區域是 Azure 移轉整備程度指南中最後一項交付成果。 本文結合本指南討論的所有整備主題，並會將所做決策套用至您第一個移轉登陸區域的部署。

下列各節將概述常用來建立移轉期間適用環境的登陸區域。 本文中所述的環境或登陸區域也可在 Azure 藍圖中取得。 您可以使用「雲端採用架構」移轉登陸區域藍圖，只需按一下即可部署已定義的環境。

## <a name="purpose-of-the-blueprint"></a>藍圖的用途

「雲端採用架構」移轉登陸區域藍圖會建立登陸區域。 該登陸區域會刻意地加上限制。 其設計目的是要建立一致的起點，以提供空間來學習基礎結構即程式碼。 針對某些移轉工作，此登陸區域可能足以滿足您的需求。 您也可能需要變更藍圖中的某些項目，以符合您特有的條件限制。

## <a name="blueprint-alignment"></a>藍圖定位

下圖顯示「雲端採用架構」移轉登陸區域藍圖與架構複雜性和合規性需求的關係。

![藍圖定位](../../_images/ready/blueprint-overview.png)

- 字母 A 位在標示為此藍圖範圍的曲線內。 該範圍主要在傳達此藍圖涵蓋了有限的架構複雜性，但是以相對中等的合規性需求為基礎。
- 具有高度複雜性和嚴格合規性需求的客戶，可能較適合使用合作夥伴的擴充藍圖，或其中一個[以標準為基礎的藍圖範例](https://docs.microsoft.com/azure/governance/blueprints/samples)。
- 大部分客戶的需求會落在這兩個極端之間的區域。 字母 B 代表[登陸區域考量](../considerations/index.md)文章中所述的程序。 對於此空間中的客戶，您可以使用在這些文章中找到的決策指南，識別要新增至「雲端採用架構」移轉登陸區域藍圖的節點。 此方法可讓您自訂藍圖以符合您的需求。

## <a name="use-this-blueprint"></a>使用此藍圖

使用「雲端採用架構」移轉登陸區域藍圖之前，請先參閱下列假設、決策和實作指引。

## <a name="assumptions"></a>假設

我們定義此初始登陸區域時，已使用下列假設或條件約束。 如果這些假設符合您的條件約束，您可以使用藍圖來建立您的第一個登陸區域。 藍圖也可以加以擴充，以建立符合您特有條件限制的登陸區域藍圖。

- **訂用帳戶限制：** 此採用工作不應超過[訂用帳戶限制](https://docs.microsoft.com/azure/azure-subscription-service-limits)。 兩個常見指標為超過 25,000 部 VM 或 10,000 個 vCPU。
- **合規性：** 此登陸區域不需要第三方合規性需求。
- **架構複雜性：** 架構複雜性不需要額外的生產訂用帳戶。
- **共用服務：** Azure 中沒有任何現有共用服務需要將此訂用帳戶視為中樞和輪輻架構中的輪輻。

如果這些假設與您目前的環境一致，則此藍圖可能是開始建立您登陸區域的絕佳位置。

## <a name="decisions"></a>決策

下列決策會在登陸區域藍圖中表示。

| 元件 | 決策 | 替代方法 |
|---------|---------|---------|
|移轉工具|將會部署 Azure Site Recovery，並建立 Azure Migrate 專案。|[移轉工具決策指南](../../decision-guides/migrate-decision-guide/index.md)|
|記錄和監視|將會布建 Operational Insights 工作區和診斷儲存體帳戶。|         |
|網路|將會建立包含子網路的虛擬網路，以用於閘道、防火牆、jumpbox 和登陸區域。|[網路決策](../considerations/network-decisions.md)|
|身分識別|假設訂用帳戶已經與 Azure Active Directory 執行個體相關聯。|[身分識別管理最佳做法](https://docs.microsoft.com/azure/security/azure-security-identity-management-best-practices?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/bread/toc.json)         |
|原則|此藍圖目前會假設未套用任何 Azure 原則。|         |
|訂用帳戶設計|N/A - 專為單一生產訂用帳戶所設計。|[調整訂用帳戶](../considerations/scaling-subscriptions.md)|
|管理群組|N/A - 專為單一生產訂用帳戶所設計。|[調整訂用帳戶](../considerations/scaling-subscriptions.md)         |
|資源群組|N/A - 專為單一生產訂用帳戶所設計。|[調整訂用帳戶](../considerations/scaling-subscriptions.md)         |
|Data|N/A|在 Azure 和[Azure 資料存放區](https://docs.microsoft.com/azure/architecture/guide/technology-choices/data-store-overview)[中選擇正確的 SQL Server 選項](https://docs.microsoft.com/azure/sql-database/sql-database-paas-vs-sql-server-iaas?toc=https://docs.microsoft.com/azure/architecture/toc.json&bc=https://docs.microsoft.com/azure/architecture/bread/toc.json) |
|儲存體|N/A|[Azure 儲存體指引](../considerations/storage-guidance.md)         |
|命名和標記標準|N/A|[命名和標記最佳做法](../considerations/naming-and-tagging.md)         |
|成本管理|N/A|[追蹤成本](../azure-best-practices/track-costs.md)|
|計算|N/A|[計算選項](../considerations/compute-decisions.md)|

## <a name="customize-or-deploy-a-landing-zone-from-this-blueprint"></a>從此藍圖自訂或部署登陸區域

深入瞭解並下載雲端採用架構的參考範例遷移登陸區域藍圖，以從[Azure 藍圖範例](https://docs.microsoft.com/azure/governance/blueprints/samples)進行部署或自訂。

您也可以在入口網站中取得藍圖範例。 如需如何建立藍圖的詳細資訊，請參閱[Azure 藍圖](./govern-org-compliance.md?tabs=azureblueprints#create-a-blueprint)。

如需在此藍圖或所產生的登陸區域上應進行的自訂指引，請參閱[登陸區域考量](../considerations/index.md)文章。

## <a name="next-steps"></a>後續步驟

部署移轉登陸區域之後，您就可以開始將工作負載遷移至 Azure。
如需遷移第一個工作負載所需的工具和程序指引，請參閱 [Azure 移轉指南](../../migrate/azure-migration-guide/index.md)。

> [!div class="nextstepaction"]
> [使用 Azure 移轉指南遷移您的第一個工作負載](../../migrate/azure-migration-guide/index.md)
