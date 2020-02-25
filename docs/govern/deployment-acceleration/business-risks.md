---
title: 部署加速業務風險
description: 瞭解部署加速專業領域的業務風險，其可用於 Azure 的 Microsoft Cloud 採用架構中的治理策略。
author: alexbuckgit
ms.author: abuck
ms.date: 09/17/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: b7f56bc9181226b0f0fe03fbcf08a061af33099f
ms.sourcegitcommit: 1de39a4c3954512892f11e3d1330a04e95ce187d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/24/2020
ms.locfileid: "77567685"
---
# <a name="deployment-acceleration-motivations-and-business-risks"></a>部署加速動機和業務風險

本文討論客戶通常會在雲端治理策略中採用部署加速專業領域的原因。 它也提供數個衍生原則聲明的業務風險範例。

<!-- markdownlint-disable MD026 -->

## <a name="deployment-acceleration-relevancy"></a>部署加速相關性

內部部署系統通常會使用基準映像或安裝指令碼來部署。 通常需要額外設定，這可能會牽涉到多個步驟或人為介入。 這些手動流程容易出錯，經常會導致「設定漂移」，需要耗時進行疑難排解和補救工作。

大部分 Azure 資源可以透過 Azure 入口網站以手動方式部署及設定。 當您只有一些資源要管理的時候，這種方法對於您的需求可能就已足夠。 不過，當您的雲端資產成長時，您的組織應該開始將自動化整合到您的部署程式，以確保您的雲端資源可以避免設定漂移或手動處理常式所引進的其他問題。 採用 DevOps 或[DevSecOps](https://www.microsoft.com/en-us/securityengineering/devsecops)方法，通常是在雲端採用工作成熟時管理部署的最佳方式。

<!-- "en-us" location is required for the URL above. -->

強固的部署加速計劃可確保您的雲端資源正確且一致地部署、更新及設定，並且保持如此。 您的部署加速策略的成熟度，也會是您[成本管理策略](../cost-management/index.md)中的重要因素。 自動化佈建及設定您的雲端資源，可讓您在需求較低或有時間限制時相應減少或解除配置資源，讓您僅需支付所需資源的費用。

## <a name="business-risk"></a>業務風險

部署加速專業領域會嘗試解決下列業務風險。 在雲端採用期間，監視下列各項的相關性：

- **服務中斷：** 缺少可預測的重複部署程式或系統設定的非受控變更，可能會中斷正常作業，而且可能會導致生產力遺失或失去業務。
- **成本超支：** 系統資源設定中的非預期變更可讓識別問題的根本原因更棘手，進而提高開發、作業和維護的成本。
- **組織效率**不佳：開發、作業和安全性小組之間的阻礙可能會造成許多挑戰，以有效採用雲端技術和開發統一的雲端治理模型。

## <a name="next-steps"></a>後續步驟

使用[雲端管理範本](./template.md)，記錄了目前的雲端採用方案可能引進的業務風險。

一旦建立對於實際商務風險的了解，下一步是記錄風險的業務承受度與用來監視承受度的指標和關鍵計量。

> [!div class="nextstepaction"]
> [計量、指標及風險承受度](./metrics-tolerance.md)
