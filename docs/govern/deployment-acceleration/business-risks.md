---
title: 部署加速專業領域中的動機和業務風險
description: 使用適用于 Azure 的雲端採用架構，瞭解部署加速專業領域的商務風險，可用於治理策略。
author: alexbuckgit
ms.author: abuck
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: internal
ms.openlocfilehash: 76707cd3f67fe5dc3f6a78002b8fcecf07285b27
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97021656"
---
# <a name="motivations-and-business-risks-in-the-deployment-acceleration-discipline"></a>部署加速專業領域中的動機和業務風險

本文討論客戶通常會在雲端治理策略中採用部署加速專業領域的原因。 它也提供數個衍生原則聲明的業務風險範例。

## <a name="relevance"></a>相關性

內部部署系統通常會使用基準映像或安裝指令碼來部署。 通常需要額外設定，這可能會牽涉到多個步驟或人為介入。 這些手動流程容易出錯，經常會導致「設定漂移」，需要耗時進行疑難排解和補救工作。

大部分 Azure 資源可以透過 Azure 入口網站以手動方式部署及設定。 當您只有一些資源要管理的時候，這種方法對於您的需求可能就已足夠。 當您的雲端資產成長時，您的組織應開始將自動化整合到您的部署程式，以確保您的雲端資源可以避免設定漂移或手動程式所引進的其他問題。 採用 DevOps 或 [DevSecOps](https://www.microsoft.com/devsecops) 方法通常是在雲端採用工作成熟時管理部署的最佳方式。

健全的部署加速方案可確保您的雲端資源已正確且一致地進行部署、更新及設定，而且仍維持如此。 您的部署加速策略的成熟度，也會是您[成本管理策略](../cost-management/index.md)中的重要因素。 自動化佈建及設定您的雲端資源，可讓您在需求較低或有時間限制時相應減少或解除配置資源，讓您僅需支付所需資源的費用。

## <a name="business-risk"></a>業務風險

部署加速專業領域會嘗試解決下列業務風險。 在雲端採用期間，監視下列各項的相關性：

- **服務中斷：** 缺乏可預測的可重複部署程式或系統設定的非受控變更可能會中斷正常作業，而且可能會導致生產力遺失或企業遺失。
- **成本超支：** 設定系統資源時，發生非預期的變更，可讓找出問題的根本原因變得更困難，進而提高開發、營運和維護的成本。
- **組織效率** 不佳：開發、營運和安全性小組之間的障礙可能會導致許多挑戰，以有效採用雲端技術和開發統一的雲端治理模型。

## <a name="next-steps"></a>後續步驟

使用「 [部署加速專業領域」範本](./template.md) ，記錄可能由目前雲端採用方案引進的商務風險。

一旦建立對於實際商務風險的了解，下一步是記錄風險的業務承受度與用來監視承受度的指標和關鍵計量。

> [!div class="nextstepaction"]
> [計量、指標及風險承受度](./metrics-tolerance.md)
