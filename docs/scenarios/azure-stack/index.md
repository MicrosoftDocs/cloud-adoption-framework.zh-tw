---
title: 在您的資料中心內執行 Azure 的策略性選項 - Azure Stack
description: 透過使用 Azure Stack Hub 在您的資料中心內部署 Azure。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/19/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: strategy
ms.custom: internal
ms.openlocfilehash: 1c188c57de5881829b57d606e78692d456e4272d
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97025821"
---
# <a name="azure-stack-a-strategic-option-for-running-azure-in-your-datacenter"></a>Azure Stack：在您的資料中心內執行 Azure 的策略選項

Microsoft 會針對應用程式和資料儲存採用雲端優先的方法。 其首要目標是將應用程式和資料移到一或多個超大規模資料庫雲端上，包括全域 Azure 選項或是地區專屬的主權雲端，例如 Azure 德國或 Azure Government。

Azure Stack Hub 會作為主權雲端的另一個執行個體，由客戶在其資料中心內運作，或透過雲端服務提供者來取用。 不過，Azure Stack Hub 不是超大規模資料庫雲端，而且 Microsoft 不會發佈或支援適用於 Azure Stack Hub 的任何服務等級協定。

## <a name="understand-your-cloud-journey"></a>了解您的雲端旅程圖

每個組織會根據其歷史、業務細節、文化以及 (可能是最重要的一點) 其起點，擁有獨特的雲端旅程圖。 其旅程提供許多選項、功能和功能。 其也會提供機會來改善其現有的治理和營運、執行新的管理作業，甚至重新設計其應用程式，以充分利用雲端架構。

您組織的旅程圖可能可以識別在 IT 和商務策略中使用雲端的清楚優勢。 但您的旅程圖也可以識別同樣強烈的動機，要將雲端保留在資料中心內，至少目前是這樣。 如果您遇到這兩個彼此競爭的驅動因素，您不必做選擇。 就核心而言，Azure Stack Hub 是[基礎結構即服務 (IaaS)](https://azure.microsoft.com/blog/azure-stack-iaas-part-one)。 其提供平台即服務 (PaaS) 服務，讓您可以在自己的資料中心內執行 Azure 服務子集。

## <a name="azure-stack-hub-in-your-strategy"></a>您策略中的 Azure Stack Hub

Azure Stack Hub 提供了替代方法，讓您可以在實體伺服器或現有虛擬化平台上，遷移執行的現有應用程式。 藉由將這些工作負載移到 Azure Stack Hub 部署 IaaS 環境，小組可以從更順暢的作業、自助式部署、標準化的硬體設定和 Azure 一致性中獲益。 使用 Azure Stack Hub 來提供現代化或創新支援，可讓您的小組針對應用程式和工作負載做好準備，以充分利用雲端。

藉由跨 Azure 和 Azure Stack Hub 遵循一致的雲端採用做法，您可以將相同的治理和作業模型套用至公用雲端或自有資料中心內的資產。 Azure Stack Hub 使用的 Azure Resource Manager 模型與 Azure 相同，因此您可以在單一位置檢視您所有的解決方案。

## <a name="compare-azure-with-azure-stack-hub"></a>比較 Azure 與 Azure Stack Hub

Azure 與 Azure Stack Hub 之間有一些差異。 有些很明顯，其他則要等到實作週期的後期才會看到。 請注意以下差異：

- Azure 提供近乎無限的容量。 Azure Stack Hub 則建置在您資料中心內的實體硬體之上，因此會導致容量有限。
- Azure 與 Azure Stack Hub 之間的 API 版本和驗證機制可能會稍有不同。
- Azure Stack Hub 在雲端的 _操作者_ 這一點有所不同，因此會影響工作負載作業的層級。
- 必須考慮 Azure Stack Hub 操作員執行的是 Azure Stack Hub 服務的哪個部分，因為這會決定客戶要呼叫服務 PaaS 還是軟體即服務 (SaaS)。

其他差異將會在雲端採用生命週期的不同時間點，於其他 Azure Stack Hub 文章中加以說明。

## <a name="next-steps"></a>後續步驟

如需雲端採用旅程圖的特定元素指引，請參閱：

- [規劃 Azure Stack Hub 移轉](./plan.md)
- [環境整備](./ready.md)
- [評估 Azure Stack Hub 的工作負載](./migrate-assess.md)
- [將工作負載部署到 Azure Stack Hub](./migrate-deploy.md)
- [治理 Azure Stack Hub](./govern.md)
- [管理 Azure Stack Hub](./manage.md)
