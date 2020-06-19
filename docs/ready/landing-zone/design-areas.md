---
title: 設計完善之登陸區域的設計區域
description: 設計完善之登陸區域的設計區域。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: ff839cb704ce79910fc29d6954712a0337a53e1e
ms.sourcegitcommit: 9b183014c7a6faffac0a1b48fdd321d9bbe640be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2020
ms.locfileid: "85077009"
---
# <a name="modular-design-areas-considered-by-all-azure-landing-zones"></a>所有 Azure 登陸區域所考慮的模組化設計區域

每個 Azure 登陸區域的 [執行] 選項都會提供部署方法和定義的設計原則，以協助您執行下列設計區域。 選擇 [執行] 選項之前，請使用本文來瞭解這些設計區域。

> [!NOTE]
> 下列設計區域概述部署登陸區域之前應考慮的事項。 本文特意不是深度或可採取動作，而是做為簡單的參考。 [登陸區域執行選項](./implementation-options.md)一文的下一個步驟將提供固定的設計原則，以及部署的可採取動作步驟。  

不論部署選項為何，都應該考慮下列各項，並針對每個設計區域進行決策。 這些決策會影響每個登陸區域所依賴的平臺基礎。

| 設計區域  | 目標  | 相關的方法 |
|---|---|---|
| 企業註冊 | 針對具有 Azure 承諾用量的企業客戶，適當的租使用者建立和註冊是很重要的初期步驟。 | 就緒 |
| 身分識別 | 身分識別與存取管理（IAM）是公用雲端中的主要安全性界限。 它是任何安全且完全相容之架構的基礎。 | 就緒 |
| 網路拓樸和連線能力 | 網路和連線能力決策是任何雲端架構中同樣重要的基本層面。 | 就緒 |
| 資源組織 | 隨著雲端採用規模調整，訂用帳戶設計和管理群組階層的考慮將會影響治理、作業管理和採用模式。 | 治理 |
| 治理專業領域 | 自動化安全性、治理和合規性原則的審核和強制執行。 | 治理 |
| 作業基準 | 針對雲端中的穩定、進行中的作業，需要有作業基準，才能提供可見度、作業合規性及保護和復原功能。 | 管理 |
| 業務持續性和災害復原 (BCDR) | BCDR 提供可靠性和快速復原的基礎。 | 管理 |
| 部署選項 | 調整最佳工具和範本，以部署登陸區域和支援資源。 | 就緒 |

## <a name="next-steps"></a>後續步驟

您會在下一篇文章中看到，這些設計區域可以在一段時間內實行，讓您能夠成長到雲端作業模型。 或者，也有豐富的固定執行選項，其開頭為每個設計區域上定義的位置。

瞭解模組化設計區域之後，下一步是選擇最符合您雲端採用方案和需求的 [[登陸區域執行] 選項](./implementation-options.md)。

> [!div class="nextstepaction"]
> [選擇 [執行] 選項](./implementation-options.md)
