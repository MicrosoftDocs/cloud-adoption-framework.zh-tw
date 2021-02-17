---
title: Azure 登陸區域設計區域
description: 評估用來定義所有 Azure 登陸區域的一組標準考慮。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: internal
ms.openlocfilehash: 88eb52e410c9f7a1caf2c3c8f4bfc45bbb743ce0
ms.sourcegitcommit: 9d76f709e39ff5180404eacd2bd98eb502e006e0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/17/2021
ms.locfileid: "100632523"
---
<!-- TODO: Refactor terms: "design area", "well-architected" -->

# <a name="azure-landing-zone-design-areas"></a>Azure 登陸區域設計區域

每個 Azure 登陸區域的執行選項都會提供部署方法和定義的設計原則。 選擇 [執行] 選項之前，請使用本文來瞭解下表所列的設計領域。

> [!NOTE]
> 這些設計區域會說明部署登陸區域之前應該考慮的事項。 請使用它作為簡單參考。 如需部署的設計原則和可採取動作的步驟，請參閱 [登陸區域的執行選項](./implementation-options.md) 。

## <a name="design-areas"></a>設計區域

無論部署選項為何，您都應該仔細考慮每個設計區域。 您的決策會影響每個登陸區域相依的平臺基礎。

| 設計區域 | 目標  | 相關方法 |
|---|---|---|
| 企業註冊 | 針對具有 Azure 承諾用量的企業客戶，適當的租使用者建立和註冊是一個重要的初期步驟。 | 就緒 |
| 身分識別 | 身分識別和存取管理是公用雲端中的主要安全性界限。 它是任何安全且完全符合規範之架構的基礎。 | 就緒 |
| 網路拓樸和連線能力 | 網路和連線能力決策是任何雲端架構的重要基礎層面。 | 就緒 |
| 資源組織 | 隨著雲端採用規模的考慮，訂用帳戶設計和管理群組階層的考慮會影響治理、作業管理和採用模式。 | 治理 |
| 治理專業領域 | 自動化安全性、治理和合規性政策的審核和強制執行。 | 治理 |
| 作業基準 | 若要在雲端中進行穩定的進行中作業，需要有作業基準才能提供可見度、作業合規性，以及保護和復原功能。 | 管理 |
| 業務持續性和災害復原 (BCDR) | 復原是讓應用程式順利運作的關鍵。 BCDR 是復原的重要元件。 BCDR 牽涉到透過備份來保護資料，以及保護應用程式免于透過災難復原中斷。 | 管理 |
| 部署選項 | 調整最適合的工具和範本，以部署登陸區域和支援資源。 | 就緒 |

## <a name="next-steps"></a>下一步

您可以在一段時間內實行這些設計區域，讓您可以成長到雲端作業模型。 或者，也有豐富的固定實選項，其開頭為每個設計區域上定義的位置。

在瞭解模組化設計區域後，下一步是選擇最符合您雲端採用方案和需求的 [登陸區域執行選項](./implementation-options.md) 。

> [!div class="nextstepaction"]
> [選擇實行選項](./implementation-options.md)
