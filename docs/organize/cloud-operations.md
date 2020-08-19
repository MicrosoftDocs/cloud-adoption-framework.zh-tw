---
title: 瞭解雲端操作功能
description: 瞭解雲端作業功能的構成，並適當地為您的小組工作。
author: JanetCThomas
ms.author: janet
ms.service: cloud-adoption-framework
ms.subservice: organize
ms.topic: conceptual
ms.date: 05/15/2020
ms.openlocfilehash: 5762688ddd367e6f01e7276d9d9079aaf27f56da
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88569972"
---
# <a name="cloud-operations-functions"></a>雲端作業功能

營運小組著重于監視、修復和解決與傳統 IT 營運和資產相關的問題。 在雲端中，許多資本成本和營運活動都會轉移給雲端提供者，讓 IT 營運人員有機會改善並提供更高的價值。

提供雲端作業功能所需的技能可透過下列方式提供：

- IT 作業
- 外包 IT 營運廠商
- 雲端服務提供者
- 雲端管理的服務提供者
- 應用程式特定的作業小組
- 商務應用程式營運小組
- DevOps 團隊

> [!IMPORTANT]
> 負責雲端營運的個人或團隊通常負責在修復期間對設定進行被動變更。 它們也可能會負責主動式設定變更，以將操作中斷降至最低。 根據組織的雲端作業模式，這些變更可透過基礎結構即程式碼、Azure Pipelines 或入口網站中的直接設定來傳遞。 因為作業小組可能會有較高的許可權，所以填妥此角色的人員必須遵循身分 [識別和存取控制最佳做法](/azure/security/benchmarks/security-control-identity-access-control) ，以將非預期的存取或生產變更降至最低。

## <a name="preparation"></a>準備

- [管理 Azure 中的資源](/learn/paths/manage-resources-in-azure/)：瞭解如何透過 Azure CLI 和入口網站來建立、管理及控制雲端式資源。
- [Azure 網路服務](/learn/modules/intro-to-azure-networking/)：瞭解 azure 網路功能的基本概念，以及如何提升復原能力並減少延遲。

請檢閱下列各項：

- [業務成果](../strategy/business-outcomes/index.md)
- [財務模型](../strategy/financial-models.md)
- [雲端採用動機](../strategy/motivations.md)
- [業務風險](../govern/policy-compliance/risk-tolerance.md)
- [數位資產的合理化](../digital-estate/index.md)

## <a name="minimum-scope"></a>最小範圍

雲端營運團隊的人員所負責的工作，就是在經過同意的作業預算內，提供最大的工作負載效能和最少的業務中斷。

- 判斷工作負載的重要性、中斷的影響或效能降低。
- 建立業務核准的成本和效能承諾。
- 監視和操作雲端工作負載。

## <a name="deliverables"></a>交付項目

- 維護資產和工作負載清查
- 監視工作負載的效能
- 維護營運合規性
- 保護工作負載和相關聯的資產
- 當效能降低或業務中斷時復原資產
- 核心平臺的成熟功能
- 持續改善工作負載效能
- 改善工作負載的預算和設計需求，以配合對企業的承諾

### <a name="meeting-cadence"></a>會議步調

雲端營運團隊應牽涉到發行規劃和卓越的雲端中心規劃，以提供意見反應，並準備操作需求。

## <a name="out-of-scope"></a>超出範圍

針對低層級技術資產維護目前狀態作業的傳統 IT 作業，不在雲端營運團隊的範圍內。 儲存體、CPU、記憶體、網路設備、伺服器和虛擬機器主機等專案都需要持續維護、監視、修復和補救問題，以維護尖峰作業。 在雲端中，其中許多資本成本和營運活動都會轉移給雲端提供者。

## <a name="next-steps"></a>後續步驟

隨著採用和作業規模的考慮，請務必定義和自動化治理的最佳做法，以擴充現有的 IT 需求。 形成雲端卓越中心是調整雲端採用、雲端營運和雲端治理工作的重要步驟。

深入了解：

- [卓越功能的雲端中心](../organize/cloud-center-of-excellence.md) 。
- [組織反模式：定址接收器和領域](../organize/fiefdoms-silos.md)。

瞭解如何藉由開發跨小組的矩陣，識別負責任、負責任、諮詢及告知 (RACI) 合作物件，以協調各小組的責任。 下載並修改 [RACI 範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/organize/raci-template.xlsx)。
