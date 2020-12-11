---
title: Azure 登陸區域考量
description: 使用適用于 Azure 的雲端採用架構，以瞭解登陸區域如何提供任何雲端採用環境的基本組建區塊。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/09/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: internal
ms.openlocfilehash: 4743187cbec0e6030642a065bae772248cbe26d7
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97026042"
---
# <a name="landing-zone-considerations"></a>登陸區域考量

登陸區域是任何雲端採用環境的基本建置區塊。 「登陸區域」一詞指的是已佈建並準備好在雲端環境 (例如 Azure) 中裝載工作負載的環境。 完全運作的登陸區域是最終交付項目，由雲端採用架構備妥方法的任何反覆項目組成。

![登陸區域考慮 ](../../_images/ready/landing-zone-considerations.png)
 _圖1：登陸區域考慮。_

此圖顯示用於實作任何登陸區域部署的主要考量。 這些考量可分成三種類別或類型的考量：裝載、Azure 基礎和治理。

## <a name="hosting-considerations"></a>裝載考量

所有登陸區域都會提供裝載選項的結構。 此結構是透過治理控制項明確建立的，或是透過採用登陸區域內的服務有組織地建立的。 下列文章可協助您做出決策，它們將反映在藍圖或其他建立登陸區域的自動化指令碼中：

- [計算決策](./compute-options.md)：若要將操作複雜度降至最低，請將計算選項與登陸區域的目的保持一致。 您可以使用自動化工具鏈（例如 Azure 原則計畫和登陸區域）來強制執行這項決策。
- [儲存體決策](./storage-options.md)：選擇適當的 Azure 儲存體解決方案，以支援您的工作負載需求。
- [網路決策](./networking-options.md)：選擇網路服務、工具和架構，以支援您組織的工作負載、治理和連線能力需求。
- [資料庫決策](./data-options.md)：判斷哪一種資料庫技術最適合您的工作負載需求。

## <a name="azure-fundamentals"></a>Azure 基礎

每個登陸區域都是更廣泛解決方案的一部分，而此解決方案用於組織整個雲端環境中的資源。 Azure 基礎是組織的基本建置區塊。

- [Azure 基本概念](./fundamental-concepts.md)：瞭解用來組織 Azure 資源的基本概念和詞彙，以及這些概念彼此之間的關聯性。
- [資源一致性決策指南](../../decision-guides/resource-consistency/index.md)：當您瞭解每個基礎時，資源組織決策指南可以協助您做出決定登陸區域的決策。

## <a name="governance-considerations"></a>治理考量

雲端採用架構的治理方法會建立一個治理整個環境的程序。 許多使用案例可能會要求您根據每個登陸區域做出治理決策。 在許多案例中，系統會針對每個登陸區域強制執行治理基準，即使基準是全面性地型建立的也一樣。 這適用於組織部署的前幾個登陸區域。

下列文章可協助您對登陸區域做出治理相關決策。 您可以將每個決策納入治理基準。

- **成本需求。** 根據組織採用雲端的動機以及對此環境所做的操作承諾，可能需要針對此登陸區域變更各種成本管理設定。
- **監視決策。** 根據登陸區域的操作需求，可以部署各種監視工具。 監視決策文章可以協助您判定最適合部署的工具。
- **以角色為基礎的存取控制。** Azure [角色型存取控制 (RBAC)](../considerations/roles.md) 提供更細緻的群組型存取管理，以根據使用者角色組織資源。
- **原則決策。** [Azure 藍圖範例](/azure/governance/blueprints/samples)提供預先建立的合規性藍圖，每個藍圖都有預先定義的原則計劃。 原則決策協助根據您的需求和限制告知如何選出最佳藍圖或原則計畫。
- **建立 [混合式雲端一致性](./hybrid-consistency.md)。** 建立混合式雲端解決方案，以提供您組織雲端創新的優勢，同時保有內部部署管理的許多便利性。
