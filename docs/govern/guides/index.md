---
title: 雲端治理指南
description: 檢閱雲端治理指南，這些指南說明了針對任何治理案例累加式方法的最佳做法。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/05/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: f8f45a16e3699977dce152a06c563844d83b4b1b
ms.sourcegitcommit: 9b183014c7a6faffac0a1b48fdd321d9bbe640be
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2020
ms.locfileid: "85075518"
---
# <a name="cloud-governance-guides"></a>雲端治理指南

本節中可採取動作的治理指南，根據先前描述的[治理方法](../methodology.md)，說明了雲端採用架構治理模型的漸進式方法。 您可以建立一個敏捷式雲端治理方法，以滿足任何雲端治理案例的需求。

## <a name="review-and-adopt-cloud-governance-best-practices"></a>檢閱並採用雲端治理最佳作法

若要開始進行雲端採用旅程，請選擇下列其中一個治理指南。 每個指南分別根據一組的虛構客戶體驗，概述了一系列最佳做法。 讀者若不熟悉雲端採用架構治理模型的漸進式方法，請先檢閱以下高階治理理論簡介，然後再採用其中一組最佳做法。

<!-- markdownlint-disable MD033 -->

- [標準治理指南](./standard/index.md)：此指南適用於使用建議的兩個訂用帳戶模型的大部分組織，設計訴求為在多個區域中部署，但不橫跨公用和主權/政府雲端。

> [!div class="nextstepaction"]
> [標準治理指南](./standard/index.md)

- [適用於複雜企業的治理指南](./complex/index.md)：適用於由多個獨立 IT 業務單位或跨公用和主權/政府雲端管理的企業指南。

> [!div class="nextstepaction"]
> [適用於複雜企業的治理指南](./complex/index.md)

<!-- markdownlint-enable MD033 -->

## <a name="an-incremental-approach-to-cloud-governance"></a>雲端治理的漸進式方法

## <a name="choose-a-governance-guide"></a>選擇治理指南

此指南示範如何實作治理 MVP。 從該處開始，每個指南都顯示了雲端治理小組如何與雲端採用小組合作，為其完成前置作業，以加速進行採用工作。 從基礎到後續改良和演進，雲端採用架構治理模型都會引導治理的應用。

若要開始治理旅程，請選擇下列兩個選項其中之一。 這些選項是依據綜合的客戶體驗。 標題會依據企業複雜度制定，以便於瀏覽。 您的決策可能更為複雜。 下表概述了這兩個選項之間的差異。

<!-- TODO: Refactor VDC content below. -->
<!-- docsTest:ignore "Azure Virtual Datacenter" -->

> [!WARNING]
> 您可能需要更強固的治理起點。 在這類情況下，請考慮使用 [CAF 企業級登陸區域](../../ready/enterprise-scale/index.md)。 CAF 企業級登陸區域方法著重於具有以下中期目標 (24 個月內) 的採用小組：在雲端裝載 1000 個以上的資產 (應用程式、基礎結構或資料資產)。 對於這類較大型的雲端採用工作而言，CAF 企業級登陸區域是複雜治理案例的實質選擇。

<!-- markdownlint-disable MD028 -->

> [!NOTE]
> 每個指南都不太可能完全符合您的狀況。 請選擇最接近您的狀況的指南，並用它當作起點。 在整個指南中，會提供額外資訊來協助您自訂決策，以符合特定準則。

### <a name="business-characteristics"></a>商務特性

| 特性 | 標準組織 | 複雜企業 |
|---|---|---|
| 地理位置 (國家或地緣政治區域) | 客戶或員工主要位於一個地理位置 | 客戶或員工位於多個地理位置或需要主權雲端。 |
| 受影響的營業單位 | 共用一般 IT 基礎結構的業務單位 | 未共用一般 IT 基礎結構的業務單位 |
| IT 預算 | 單一 IT 預算 | 以不同的貨幣在多個營業單位間分配預算 |
| IT 投資 | 資本支出導向的投資是每年計劃一次，而且通常僅涵蓋基本維護。 | 資本支出導向的投資是每年計劃一次，而且通常包含三到五年的維護和更新週期。 |

### <a name="current-state-before-adopting-cloud-governance"></a>採用雲端治理之前的目前狀態

| State | 標準企業 | 複雜企業 |
|---|---|---|
| 資料中心或協力廠商主機服務提供者 | 少於五個資料中心 | 超過五個資料中心 |
| 網路功能 | 無 WAN，或是 1 &ndash; 2 個 WAN 提供者 | 複雜網路或全域 WAN |
| 身分識別 | 單一樹系、單一網域。 | 複雜、多個樹系、多個網域。 |

### <a name="desired-future-state-after-incremental-improvement-of-cloud-governance"></a>雲端治理累加式改進後所需的未來狀態

| State | 標準組織 | 複雜企業 |
|---|---|---|
| 成本管理 &mdash; 雲端帳戶處理 | 回報模型。 透過 IT 集中計費。 | 退款模型。 可透過 IT 採購來散發計費。 |
| 安全性基準 &mdash; 受保護的資料 | 公司財務資料和 IP。 有限的客戶資料。 沒有協力廠商合規性需求。 | 客戶的財務和個人資料有多個集合。 可能必須考慮協力廠商合規性。 |

## <a name="caf-enterprise-scale-landing-zone"></a>CAF 企業級登陸區域

[CAF 企業級登陸區域](../../ready/enterprise-scale/index.md)能充分發揮 Azure 雲端平台的功能，同時還能遵循企業的安全性與治理需求。

相較於傳統內部部署環境，Azure 可讓工作負載開發團隊和其業務贊助者善用雲端平台所提供的更佳部署靈活度。 當雲端採用工作擴大而要納入關鍵任務資料和工作負載時，此靈活度可能會與 IT 小組所建立的公司安全性和原則合規性需求衝突。 已有複雜治理和法規需求的大型企業尤其會如此。

CAF 企業級登陸區域架構的目的在於經由架構、實作和指引來處理採用生命週期早期的這些疑慮，協助在企業雲端採用期間達到雲端採用小組與中央 IT 小組需求之間的平衡。 這種方法的核心是共用服務架構和妥善管理登陸區域的概念。

CAF 企業級登陸區域會在 Azure 平台內部署您自己的「孤立雲端」，整合管理程序、法規需求和治理原則所需的安全性程序。 在此虛擬邊界內，CAF 企業級登陸區域會在確保一致合規性的同時，提供用於部署工作負載的模型範例，並提供基本指引來讓您了解如何在雲端中為組織實作角色和職責的隔離。

### <a name="caf-enterprise-scale-landing-zone-qualifications"></a>CAF 企業級登陸區域規格

雖然較小型的小組可受益於 CAF 企業級登陸區域所提供的架構和建議。 但我們的目標是要持續簡化 CAF 企業級登陸區域實作，使其更方便較小型的小組進行實作。 此方法目前的設計訴求是要引導中央 IT 小組管理大型雲端環境。

[CAF 企業級登陸區域](../../ready/enterprise-scale/index.md)方法著重於具有以下中期目標 (24 個月內) 的採用小組：**在雲端裝載 1000 個以上的資產 (應用程式、基礎結構或資料資產)** 。

對於符合下列準則的組織，您也可以開始使用 [CAF 企業級登陸區域](../../ready/enterprise-scale/index.md)：

- 貴企業受到法規合規性需求規範，而需要集中的監視和稽核功能。
- 您需要維護一般原則和治理合規性，以及對核心服務的集中式 IT 控制。
- 您的行業依賴復雜的平台，需要復雜的控制項和深入網域的專業知識來管理平台。 這在金融、製造業、石油和天然氣的大型企業中最為常見。
- 現有的 IT 治理原則需要與現有功能更加緊密地保持對應，即使在早期階段採用期間也是如此。

## <a name="next-steps"></a>後續步驟

請選擇其中一個指南：

> [!div class="nextstepaction"]
> [標準企業治理指南](./standard/index.md)
>
> [適用於複雜企業的治理指南](./complex/index.md)
