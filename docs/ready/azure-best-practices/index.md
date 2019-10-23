---
title: Azure 移轉整備程度最佳做法
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: Azure 移轉整備程度的最佳做法簡介
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 67b65affee2a2ac351ab603a52b0b6202d41458f
ms.sourcegitcommit: 35c162d2d09ec1c4a57d3d57a5db1d56ee883806
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2019
ms.locfileid: "72548999"
---
# <a name="best-practices-for-azure-readiness"></a>Azure 移轉整備程度的最佳做法

雲端移轉整備程度的很大一部分是為員工配備開始進行雲端採用所需的技術技能，並為將遷移到雲端的資產和工作負載準備遷移目標環境。 下列主題提供最佳做法和額外的指引，來協助您的小組建立並準備您的 Azure 環境。

## <a name="azure-fundamentals"></a>Azure 基礎

在 Azure 環境組織和部署您的資產時，請使用下列指引：

- [Azure 基礎概念](../considerations/fundamental-concepts.md)。 了解 Azure 中使用的基礎概念和字詞。 亦請了解這些概念如何彼此相關。
- [建議的命名和標記慣例](../considerations/naming-and-tagging.md)。 參閱命名和標記資源的詳細建議。 這些建議支援企業雲端採用工作。
- [使用多個 Azure 訂用帳戶進行調整](../considerations/scaling-subscriptions.md)。 了解使用多個 Azure 訂用帳戶進行調整的策略。
- [使用 Azure 管理群組來組織資源](https://docs.microsoft.com/azure/governance/management-groups/?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)。 了解 Azure 管理群組如何跨多個訂用帳戶管理資源、角色、原則和部署。
- [建立混合式雲端一致性](../../infrastructure/misc/hybrid-consistency.md)。 建立混合式雲端解決方案，以提供雲端創新的優勢，同時保有內部部署管理的許多便利性。

## <a name="networking"></a>網路功能

請使用下列指引，來準備您的雲端網路基礎結構以支援您的工作負載：

- [網路決策](../considerations/network-decisions.md)。 選擇網路服務、工具和架構，它們將支援您組織的工作負載、治理和連線能力需求。
- [虛擬網路規劃](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)。 了解如何根據您的隔離、連線能力和位置需求規劃虛擬網路。
- [網路安全性的最佳做法](https://docs.microsoft.com/azure/security/azure-security-network-security-best-practices?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)。 了解使用內建 Azure 功能來解決常見網路安全性問題的最佳做法。
- [周邊網路](./perimeter-networks.md)。 周邊網路也稱為非管制區域 (DMZ)，可在雲端網路與內部部署或實體資料中心網路之間啟用安全連線，以及啟用任何進出網際網路的連線。
- [中樞和輪輻網路拓撲](./hub-spoke-network-topology.md)。 中樞和輪輻是一種網路模型，用於有效管理複雜工作負載的常見通訊或安全性需求。 同時還解決潛在的 Azure 訂用帳戶限制。

## <a name="identity-and-access-control"></a>身分識別與存取控制

設計身分識別和存取控制基礎結構，來改善工作負載的安全性和管理效率時，請使用下列指引導：

- [Azure 身分識別管理和存取控制安全性最佳做法](https://docs.microsoft.com/azure/security/azure-security-identity-management-best-practices?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)。 了解使用內建 Azure 功能進行身分識別管理和存取控制的最佳做法。
- [角色型存取控制最佳做法](./roles.md)。 Azure 角色型存取控制 (RBAC) 提供更細緻的群組型存取管理，以根據使用者角色組織資源。
- [在 Azure Active Directory 中保護混合式部署和雲端部署的特殊權限存取](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-admin-roles-secure?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)。 使用 Azure Active Directory，以協助確保組織的系統管理存取和系統管理員帳戶在整個雲端和內部部署環境中安全無虞。

## <a name="storage"></a>儲存體

- [Azure 儲存體指引](../considerations/storage-guidance.md)。 選取正確的 Azure 儲存體解決方案，以支援您的使用案例。
- [Azure 儲存體安全性指南](https://docs.microsoft.com/azure/storage/common/storage-security-guide?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)。 了解 Azure 儲存體中的安全性功能。

## <a name="databases"></a>資料庫

- [在 Azure 中選擇正確的 SQL Server 選項](https://docs.microsoft.com/azure/sql-database/sql-database-paas-vs-sql-server-iaas?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)。 選擇最能支援 SQL Server 工作負載的 PaaS 或 IaaS 解決方案。
- [資料庫安全性最佳做法](https://docs.microsoft.com/azure/security/azure-database-security-best-practices?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)。 了解 Azure 平台上的資料庫安全性最佳做法。
- [選擇正確的資料存放區](https://docs.microsoft.com/azure/architecture/guide/technology-choices/data-store-overview)。 針對您的需求選取正確的資料存放區，是關鍵的設計決策。 實際上有數百個實作可以從 SQL 和 NoSQL 資料庫中選擇。 資料存放區通常是依據它們的結構資料和它們所支援的作業類型來分類。 這篇文章描述一些最常見的儲存體模型。

## <a name="cost-management"></a>成本管理

- [跨營業單位、環境和專案追蹤成本](./track-costs.md)。 了解建立適當成本追蹤機制的最佳做法。
- [如何透過 Azure 成本管理將雲端投資最佳化](https://docs.microsoft.com/azure/cost-management/cost-mgt-best-practices?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)。 實作成本管理的策略，並了解可用於處理成本挑戰的工具。
- [建立及管理預算](https://docs.microsoft.com/azure/cost-management/tutorial-acm-create-budgets?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)。 了解使用 Azure 成本管理來建立和管理預算。
- [匯出成本資料](https://docs.microsoft.com/azure/cost-management/tutorial-export-acm-data?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)。 了解在 Azure 成本管理中建立和管理匯出的資料。
- [根據建議最佳化成本](https://docs.microsoft.com/azure/cost-management/tutorial-acm-opt-recommendations?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)。 了解使用 Azure 成本管理和 Azure Adviso 來識別使用量過低的資源，並採取行動以降低成本。
- [使用成本警示監視使用量和支出](https://docs.microsoft.com/azure/cost-management/cost-mgt-alerts-monitor-usage-spending?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)。 了解使用成本管理警示來監視您的 Azure 使用量和支出。
