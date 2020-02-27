---
title: 進行雲端原則審查
description: 瞭解如何將現有的公司 IT 原則現代化，為雲端式資源提供對等層級的風險管理。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: e4235830eee8e57581214f3eabb46c32ebcf975b
ms.sourcegitcommit: af45c1c027d7246d1a6e4ec248406fb9a8752fb5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2020
ms.locfileid: "77709119"
---
<!-- markdownlint-disable MD026 -->

# <a name="conduct-a-cloud-policy-review"></a>進行雲端原則審查

雲端原則審查是雲端中[治理成熟度](../index.md)的第一個步驟。 此流程的目標是要將現有的公司 IT 原則現代化。 完成後，更新的原則會針對雲端式資源提供對等層級的風險管理。 本文說明雲端原則檢閱的流程及其重要性。

## <a name="why-perform-a-cloud-policy-review"></a>為何執行雲端原則檢閱？

大部分企業會透過執行符合治理原則的流程來管理 IT。 在小型企業，這些原則可能很有趣，且流程定義很寬鬆。 隨著企業成長為大型企業，原則和流程會更清楚地記載且一致執行。

隨著公司的 IT 原則日益成熟，過去對技術決策的相依性有滲入治理原則的傾向。 比方說，常見到在災害復原流程中納入規定異地磁帶備份的原則。 此項納入假設對於某種類型技術 (磁帶備份) 的相依，可能不再是最相關的解決方案。

雲端轉換會建立天然的變化點，以重新考慮過去的舊版原則決策。 雲端中的技術功能和預設流程大幅改變，固有風險也大幅改變。 使用先前的範例，磁帶備份原則會從單一失敗點的風險中詞幹分析，方法是將資料放在一個位置，而業務需要藉由降低此風險來將風險設定檔降至最低。 在雲端部署中，有數個選項可提供相同的風險降低，且復原時間目標 (RTO) 更低。 例如：

- 雲端原生解決方案可以啟用 Azure SQL Database 的異地複寫。
- 混合式解決方案可以使用 Azure Site Recovery 將 IaaS 工作負載複寫至 Azure。

當執行雲端轉換時，原則通常可控管雲端採用小組可用的許多工具、服務和程序。 如果這些原則是以舊技術為基礎，則可能會妨礙小組推動變革的工作。 在最糟的情況下，移轉小組會完全忽略重要原則，以便採取因應措施。 兩者都不是可接受的結果。

## <a name="the-cloud-policy-review-process"></a>雲端原則檢閱流程

雲端原則審查將現有的 IT 治理和 IT 安全性原則與[五個雲端治理專業領域](../index.md)相配合：[成本管理](../cost-management/index.md)、[安全性基準](../security-baseline/index.md)、身分[識別基準](../identity-baseline/index.md)、[資源一致性](../resource-consistency/index.md)和[部署加速](../deployment-acceleration/index.md)。

針對上述每個專業領域，檢閱流程會遵循下列步驟：

1. 檢閱與特定專業領域相關的現有內部部署原則，並尋找兩個重要資料點：舊版相依性和已識別的業務風險。
2. 詢問一個簡單的問題來評估每個商務風險：「企業風險是否仍存在於雲端模型中？」
3. 如果風險仍然存在，請記錄必要的業務緩和，而不是技術解決方案，以重寫原則。
4. 請參閱雲端採用小組的更新原則，以瞭解所需風險降低的潛在技術解決方案。

## <a name="example-of-a-policy-review-for-a-legacy-policy"></a>舊原則的原則檢閱範例

為了提供流程範例，我們會再次使用上一節的磁帶備份原則：

- 公司原則規定所有的生產系統都要有異地磁帶備份。 在此原則中，您可以看到兩個有趣的資料點：
  - 磁帶備份解決方案的舊版相依性
  - 假設的業務風險，該風險與生產設備相同的實體位置中的備份儲存體相關聯。
- 風險仍然存在嗎？ 是。 即使在雲端，對於單一設備的相依性仍會產生一些風險。 此風險影響業務的可能性比在內部部署解決方案中出現還要低，但仍有風險存在。
- 重新撰寫原則。 在全資料中心發生災害的情況下，必須有辦法在中斷的 24 小時內，於不同的資料中心和不同的地理位置還原生產系統。
  - 也請務必考慮，在上述需求中指定的時間軸可能已由不再存在於雲端中的技術條件約束所設定。 請務必先瞭解雲端的技術限制和功能，再直接套用舊版的 RTO/RPO。
- 與雲端採用小組一起檢閱。 根據所實作的解決方案，遵守此資源一致性原則的方法有很多種。

## <a name="next-steps"></a>後續步驟

深入瞭解如何在您的雲端治理策略中納入資料分類。

> [!div class="nextstepaction"]
> [資料分類](./data-classification.md)
