---
title: CISO cloud security 準備指南
description: 準備資訊安全辦公室 (CISO) 進行雲端轉換和累加式治理。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: internal
ms.openlocfilehash: 7a52ba860bfe3de9851d2c7a378d58fe75162b26
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102112376"
---
# <a name="ciso-cloud-readiness-guide"></a>CISO 雲端整備指南

「雲端採用架構」這類 Microsoft 指引並不是為了決定或引導這份檔所支援的數千個企業的獨特安全性限制。 移至雲端時，資訊安全資訊安全人員或資訊安全辦公室 (CISO) 的角色並不會受到雲端技術的會。 相反地，CISO 和 CISO 的辦公室都將擔任起更加根深蒂固且整合的角色。 本指南假設讀者已熟悉 CISO 流程，並正在尋求將這些程式現代化以啟用雲端轉換。

雲端採用能提供通常不會在傳統 IT 環境中考慮使用的服務。 自助或自動化部署通常是由應用程式開發或其他不是傳統上與生產環境部署的 IT 小組所執行。 在某些組織中，其商務組成也同樣具有類似的自助功能。 這可能會產生先前不存在於內部部署環境中的新安全性需求。 集中式安全性更具挑戰性，安全性通常會在企業和 IT 文化上成為共同責任。 本文可以協助 CISO 針對該作法進行準備，並開始著手於漸進式的治理。

## <a name="how-can-a-ciso-prepare-for-the-cloud"></a>CISO 如何為雲端做準備？

就像大部分的原則一樣，組織內的安全性和治理原則通常會成長茁壯。 發生安全性事件時，這些事件會塑造出原則來通知使用者，並降低相同事件重複發生的機會。 此方式雖然自然，卻會產生原則膨脹與技術相依性。 雲端轉換旅程創造出將原則現代化和重設的獨特機會。 在準備任何轉換旅程時，CISO 可透過擔任[原則檢閱](./cloud-policy-review.md)的主要關係人，來建立立即且可測量的值。

在這類審核中，CISO 的角色是在現有原則/合規性的條件約束與雲端提供者的改進安全性狀態之間建立安全的平衡。 測量此進度可能需要許多形式，通常是以安全地卸載至雲端提供者的安全性原則數量來測量。

**傳送安全性風險：** 當服務移至基礎結構即服務 (IaaS) 裝載模型時，企業會假設硬體布建的直接風險較低。 風險不會被移除，而是轉移給雲端廠商。 如果雲端廠商的硬體布建方法提供相同等級的風險降低風險，在安全的可重複程式中，就會從公司 IT 的職責區域中移除硬體布建執行的風險，並轉移至雲端提供者。 這可降低公司 IT 負責管理的整體安全性風險，不過仍應定期追蹤和審核風險本身。

隨著解決方案進一步「向上堆疊」，以納入平臺即服務 (PaaS) 或軟體即服務 (SaaS) 模型，您可以避免或傳輸額外的風險。 當風險被安全地移到雲端提供者時，執行、監視及強制執行安全性原則或其他合規性原則的成本也都能安全地降低。

**成長思維：** 變更可能會對商務和技術實作者有很可怕的變化。 當組織中的 CISO 能夠率先轉換為成長心態時，我們發現那些自然而來的憂慮，都會轉換成對安全性及原則合規性與日俱增的興趣。 利用成長思維來接近 [原則審核](./cloud-policy-review.md)、轉型旅程或簡單的執行審核，讓小組能夠快速地移動，但不會代價是公平且可管理的風險設定檔。

## <a name="resources-for-the-chief-information-security-officer"></a>首席資訊安全長的資源

對於雲端的認識，是抱持成長心態進行[原則檢閱](./cloud-policy-review.md)最重要的關鍵。 下列資源可協助 CISO 更加了解 Microsoft Azure 平台的安全性狀態。

<!-- docutune:casing "Security Response in the Cloud" -->

**安全性平台資源：**

- [安全性開發生命週期，內部審核](https://www.microsoft.com/sdl)
- [必要的安全性訓練、背景檢查](https://downloads.cloudsecurityalliance.org/star/self-assessment/StandardResponsetoRequestforInformationWindowsAzureSecurityPrivacy.docx)
- [滲透測試、入侵偵測、DDoS、稽核和記錄](https://www.microsoft.com/security/business/operations) \(英文\)
- [尖端的資料中心](https://azure.microsoft.com/global-infrastructure/)、實體安全性、[安全網路](/azure/security/fundamentals/network-overview)

**隱私權與控制：**

- [隨時管理您的資料 (英文)](https://www.microsoft.com/trust-center/privacy/data-management)
- [資料位置控制 (英文)](https://www.microsoft.com/trust-center/privacy/data-location)
- [根據您的條款提供資料存取 (英文)](https://www.microsoft.com/trust-center/privacy/data-access)
- [回應執法機關 (英文)](https://www.microsoft.com/trust-center/privacy)
- [嚴格的隱私權標準 (英文)](https://www.microsoft.com/trust-center/privacy)

<!-- docutune:casing "Cloud Services Due Diligence Checklist" -->

**合 規：**

- [Microsoft 信任中心](https://www.microsoft.com/trust-center)
- [通用控制項中樞](https://www.microsoft.com/trust-center/compliance/compliance-overview)
- [雲端服務的審慎檢查檢查清單](https://www.microsoft.com/trust-center/compliance/due-diligence-checklist)
- [區域和國家/地區相容性](https://www.microsoft.com/trust-center/compliance/regional-country-compliance)

**透明度：**

- [Microsoft 如何保護 Azure 服務中的客戶資料 (英文)](https://www.microsoft.com/trust-center)
- [Microsoft 如何管理 Azure 服務中的資料位置](https://azuredatacentermap.azurewebsites.net) \(英文\)
- [Microsoft 內部的哪些人員可根據哪些條款存取您的資料 (英文)](https://www.microsoft.com/trust-center/privacy/data-access)
- [查看 Azure 服務、透明度中樞的認證](https://www.microsoft.com/trust-center/compliance/compliance-overview)

## <a name="next-steps"></a>下一步

在任何治理策略中採取動作的第一個步驟，就是 [原則審核](./cloud-policy-review.md)。 原則[和合規性](./index.md)在原則審核期間可能是實用的指南。

> [!div class="nextstepaction"]
> [針對原則檢閱進行準備](./cloud-policy-review.md)
