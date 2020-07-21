---
title: 在資料中心內執行 Azure 的策略選項
description: 使用 Azure Stack Hub 在資料中心內部署 Azure。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/19/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: strategy
ms.openlocfilehash: 5b0f1e25f542a2e13403707ea5604ecc29159d66
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86451463"
---
# <a name="strategic-option-to-run-azure-in-your-datacenter"></a>在資料中心內執行 Azure 的策略選項

Microsoft 採用雲端優先方法。 其首要目標是將應用程式和資料移到一或多個超大規模資料庫雲端上，包括全域 Azure 選項或是地區專屬的主權雲端，例如 Azure 德國或 Azure Government。 Azure Stack Hub 會作為主權雲端的另一個執行個體，由客戶在其資料中心內運作，或透過雲端服務提供者來取用。 不過，Azure Stack Hub 不是超大規模資料庫雲端，而且 Microsoft 也不會發佈或支援適用於 Azure Stack Hub 的任何服務等級協定 (SLA)。

## <a name="understand-your-cloud-journey"></a>了解您的雲端旅程圖

每個組織根據其歷史、業務細節、文化以及 (可能是最重要的一點) 其起點，各自會有獨特的雲端旅程圖。 雲端旅程圖提供許多了選項、特性和功能，並讓其有機會改善現有的治理、運作、實作新方式，甚至還能重新設計應用程式以利用雲端架構。

您的旅程圖可能可以識別在 IT 和商務策略中使用雲端的清楚優勢。 但您的旅程圖也可以識別同樣強烈的動機，要將雲端保留在資料中心內，_至少目前是這樣_。 如果您遇到這兩個彼此競爭的驅動因素，您不必做選擇。 Azure Stack Hub 基本上是[基礎結構即服務 (IaaS)](https://azure.microsoft.com/blog/azure-stack-iaas-part-one)，同時也提供平台即服務 (PaaS) 服務，讓您可以在自己的資料中心內執行 Azure 服務子集。

## <a name="azure-stack-hub-in-your-strategy"></a>您策略中的 Azure Stack Hub

Azure Stack Hub 移轉提供了替代方法，讓您可以在實體伺服器或現有虛擬化平台上，遷移執行的現有應用程式。 藉由將這些工作負載移到 Azure Stack Hub IaaS 環境，小組可以從更順暢的作業、自助式部署、標準化的硬體設定和 Azure 一致性中獲益。 使用 Azure Stack Hub 來提供現代化或創新支援，可讓您的小組針對應用程式和工作負載做好準備，以充分利用雲端。

藉由跨 Azure 和 Azure Stack Hub 遵循一致的雲端採用做法，您可以將相同的治理和作業模型套用至公用雲端或自有資料中心內的資產。 Azure Stack Hub 使用的 Azure Resource Manager 模型與 Azure 相同，因此您可以在單一位置檢視您所有的解決方案。

## <a name="understand-the-differences"></a>了解差異

Azure 與 Azure Stack Hub 之間有一些差異。 有些很明顯，有些則要等到實作週期的後期才會看到。 以下是一些要注意的差異：

- Azure 提供近乎無限的容量。 Azure Stack Hub 則建置在您資料中心內的實體硬體之上，因此會導致容量有限。
- Azure 與 Azure Stack Hub 之間的 API 版本和驗證機制可能會稍有不同。
- Azure Stack Hub 在雲端的_操作者_這一點有所不同，因此會影響工作負載作業的層級。
- 您必須考慮 Azure Stack Hub 操作員執行的是 Azure Stack Hub 服務的哪個部分，因為這會決定終端客戶要呼叫 PaaS 還是 SaaS 服務。

其他差異將會在雲端採用生命週期的不同時間點，於其他 Azure Stack Hub 文章中加以說明。

## <a name="next-step-integrate-this-strategy-into-your-cloud-adoption-journey"></a>下一步：將此策略整合到雲端採用旅程圖

下列文章清單會帶您前往在雲端採用旅程圖的特定時間點所找到的指引。

- [規劃 Azure Stack Hub 移轉](./plan.md)
- [環境整備](./ready.md)
- [評估 Azure Stack Hub 的工作負載](./migrate-assess.md)
- [將工作負載部署到 Azure Stack Hub](./migrate-deploy.md)
- [治理 Azure Stack Hub](./govern.md)
- [管理 Azure Stack Hub](./manage.md)
