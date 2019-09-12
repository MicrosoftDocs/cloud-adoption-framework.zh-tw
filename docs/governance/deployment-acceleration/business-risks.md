---
title: 驅使部署加速的動機和業務風險
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 深入了解雲端治理策略一部分的部署加速專業領域。
author: alexbuckgit
ms.author: abuck
ms.date: 02/11/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: feaf5a7f0f2622c2b2289fe81315ea9ccf2ada4e
ms.sourcegitcommit: a26c27ed72ac89198231ec4b11917a20d03bd222
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70828067"
---
# <a name="deployment-acceleration-motivations-and-business-risks"></a>部署加速動機和業務風險

本文討論客戶通常會在雲端治理策略中採用部署加速專業領域的原因。 它也提供幾個驅使原則聲明之業務風險的範例。

<!-- markdownlint-disable MD026 -->

## <a name="is-deployment-acceleration-relevant"></a>部署加速是否相關？

內部部署系統通常會使用基準映像或安裝指令碼來部署。 通常需要額外設定，這可能會牽涉到多個步驟或人為介入。 這些手動流程容易出錯，經常會導致「設定漂移」，需要耗時進行疑難排解和補救工作。

大部分 Azure 資源可以透過 Azure 入口網站以手動方式部署及設定。 當您只有一些資源要管理的時候，這種方法對於您的需求可能就已足夠。 不過，當您的雲端資產成長時，您的組織應該開始將自動化整合到您的部署程式，以確保您的雲端資源可以避免設定漂移或手動處理常式所引進的其他問題。 採用 DevOps 或[DevSecOps](https://www.microsoft.com/en-us/securityengineering/devsecops)方法，通常是在雲端採用工作成熟時管理部署的最佳方式。

<!-- "en-us" location is required for the URL above. -->

強固的部署加速計劃可確保您的雲端資源正確且一致地部署、更新及設定，並且保持如此。 您的部署加速策略的成熟度，也會是您[成本管理策略](../cost-management/index.md)中的重要因素。 自動化佈建及設定您的雲端資源，可讓您在需求較低或有時間限制時相應減少或解除配置資源，讓您僅需支付所需資源的費用。

## <a name="business-risk"></a>業務風險

部署加速專業領域會嘗試解決下列業務風險。 在雲端採用期間，監視下列各項的相關性：

- **服務中斷：** 缺乏可預測的可重複部署流程或未受管理的系統設定變更，會中斷正常作業以及導致遺失產能或遺失商務。
- **成本超支：** 系統資源設定中非預期的變更會讓識別問題的根本原因更困難，提高開發、作業和維護的成本。
- **組織效率不佳：** 開發、作業及安全性小組之間的屏障，會導致雲端技術的採用和整合雲端治理模型的開發效率出現無數的挑戰。

## <a name="next-steps"></a>後續步驟

使用[雲端管理範本](./template.md)，記錄了目前的雲端採用方案可能引進的業務風險。

一旦建立對於實際商務風險的了解，下一步是記錄風險的業務承受度與用來監視承受度的指標和關鍵計量。

> [!div class="nextstepaction"]
> [計量、指標及風險承受度](./metrics-tolerance.md)
