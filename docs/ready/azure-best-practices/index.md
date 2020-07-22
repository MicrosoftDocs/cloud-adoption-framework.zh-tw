---
title: Azure 移轉整備程度的最佳做法
description: 了解最佳做法和額外的指引，協助您的小組建立並準備您的 Azure 環境。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 107dd9b693a50d56816115ee641f4d78c18c7fa8
ms.sourcegitcommit: 71a4f33546443d8c875265ac8fbaf3ab24ae8ab4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2020
ms.locfileid: "86479903"
---
# <a name="best-practices-for-azure-readiness"></a>Azure 移轉整備程度的最佳做法

雲端移轉整備程度要求為員工配備開始進行雲端採用所需的技術技能，並為將遷移到雲端的資產和工作負載準備遷移目標環境。 請閱讀這些最佳做法和其他指引，以協助您的小組準備 Azure 環境。

## <a name="azure-fundamentals"></a>Azure 基礎

在 Azure 環境中組織及部署您的資產。

- [Azure 基礎概念](../considerations/fundamental-concepts.md)。 了解重要的 Azure 概念和詞彙，以及這些概念彼此之間的關聯。
- [建立您的初始訂用帳戶](./initial-subscriptions.md)。 建立一組初始的 Azure 訂用帳戶，以開始進行雲端採用。
- [使用多個訂用帳戶進行調整](../azure-best-practices/scale-subscriptions.md)。 了解建立其他訂用帳戶以調整 Azure 環境的原因和策略。
- [使用 Azure 管理群組來組織資源](../azure-best-practices/organize-subscriptions.md)。 了解 Azure 管理群組如何跨多個訂用帳戶管理資源、角色、原則和部署。
- [請遵循建議的命名和標記慣例](../azure-best-practices/naming-and-tagging.md)。 參閱命名和標記資源的詳細建議。 這些建議支援企業雲端採用工作。
- [建立混合式雲端一致性](../considerations/hybrid-consistency.md)。 建立混合式雲端解決方案，以提供雲端創新的優勢，同時保有內部部署管理的許多便利性。

## <a name="networking"></a>網路功能

準備雲端網路基礎結構以支援工作負載。

- [網路決策](../considerations/networking-options.md)。 選擇網路服務、工具和架構，它們將支援您組織的工作負載、治理和連線能力需求。
- [虛擬網路規劃](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。 根據隔離、連線能力和位置等需求來規劃虛擬網路。
- [網路安全性的最佳做法](https://docs.microsoft.com/azure/security/fundamentals/network-best-practices?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。 了解使用內建 Azure 功能來解決常見網路安全性問題的最佳做法。
- [周邊網路](./perimeter-networks.md)。 讓雲端網路與內部部署或實體資料中心網路之間能夠安全地連線，並能夠與網際網路雙向連線。
- [中樞和輪輻網路拓撲](./hub-spoke-network-topology.md)。 有效率地管理常見的通訊或安全性需求以適應複雜的工作負載，並解決潛在的 Azure 訂用帳戶限制。

## <a name="identity-and-access-control"></a>身分識別與存取控制

設計身分識別和存取控制基礎結構，以改善工作負載的安全性和管理效率。

- [Azure 身分識別管理和存取控制安全性最佳做法](https://docs.microsoft.com/azure/security/fundamentals/identity-management-best-practices?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。 了解使用內建 Azure 功能進行身分識別管理和存取控制的最佳做法。
- [角色型存取控制最佳做法](../considerations/roles.md)。 能夠針對為使用者角色所組織的資源，進行更細緻和以群組為基礎的存取管理工作。
- [在 Azure Active Directory 中保護混合式部署和雲端部署的特殊權限存取](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-admin-roles-secure?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。 確保組織的系統管理存取，和特殊權限帳戶在整個雲端和內部部署環境中，都是安全無虞的。

## <a name="storage"></a>儲存體

- [Azure 儲存體指引](../considerations/storage-options.md)。 選取正確的 Azure 儲存體解決方案，以支援您的使用案例。
- [Azure 儲存體安全性指南](https://docs.microsoft.com/azure/storage/blobs/security-recommendations?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。 了解 Azure 儲存體中的安全性功能。

## <a name="databases"></a>資料庫

- [在 Azure 中選擇正確的 SQL Server 選項](https://docs.microsoft.com/azure/sql-database/sql-database-paas-vs-sql-server-iaas?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。 選擇最能支援 SQL Server 工作負載的 PaaS 或 IaaS 解決方案。
- [資料庫安全性最佳做法](https://docs.microsoft.com/azure/security/azure-database-security-best-practices?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。 了解 Azure 平台上的資料庫安全性最佳做法。
- [選擇正確的資料存放區](https://docs.microsoft.com/azure/architecture/guide/technology-choices/data-store-overview)。 選取正確的資料存放區以符合您的需求。 SQL 和 NoSQL 資料庫中有數百個實作選項可供選擇。 資料存放區通常是依據它們的結構資料和它們所支援的作業類型來分類。 這篇文章會描述幾個常見的儲存體模型。

## <a name="cost-management"></a>成本管理

- [跨營業單位、環境和專案追蹤成本](./track-costs.md)。 了解建立適當成本追蹤機制的最佳做法。
- [如何透過 Azure 成本管理和計費將雲端投資最佳化](https://docs.microsoft.com/azure/cost-management-billing/costs/cost-mgt-best-practices?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。 實作成本管理的策略，並了解可用於處理成本挑戰的工具。
- [建立及管理預算](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-acm-create-budgets?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。 了解如何使用 Azure 成本管理和計費來建立和管理預算。
- [匯出成本資料](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-export-acm-data?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。 了解如何使用 Azure 成本管理和計費來匯出成本資料。
- [根據建議最佳化成本](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-acm-opt-recommendations?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。 了解如何使用 Azure 成本管理和計費和 Azure Advisor 來識別未充分利用的資源，並降低成本。
- [使用成本警示監視使用量和支出](https://docs.microsoft.com/azure/cost-management-billing/costs/cost-mgt-alerts-monitor-usage-spending?toc=/azure/cloud-adoption-framework/toc.json&bc=/azure/cloud-adoption-framework/_bread/toc.json)。 了解使用 Azure 成本管理和計費警示來監視您的 Azure 使用量和支出。
