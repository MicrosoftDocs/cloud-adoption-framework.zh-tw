---
title: CISO 雲端安全性就緒指南
description: 瞭解如何準備資訊安全機構（CISO），以進行雲端轉換和增量治理。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.openlocfilehash: fcf85d3da5821a87b7dd18cc421ecdbda4c3c39d
ms.sourcegitcommit: 60d8b863d431b5d7c005f2f14488620b6c4c49be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83224250"
---
<!-- cSpell:ignore CISO -->

# <a name="ciso-cloud-readiness-guide"></a>CISO 雲端整備指南

Microsoft 對於雲端採用架構的指導方針並不是為了判斷或引導這份檔所支援的數以千計企業特有的安全性限制。 移往雲端時，資訊安全人員或資訊安全辦公室（CISO）的角色並不是由雲端技術所會。 相反地，CISO 和 CISO 的辦公室都將擔任起更加根深蒂固且整合的角色。 本指南假設讀者熟悉 CISO 流程，並想要將這些程式現代化以實現雲端轉換。

雲端採用能提供通常不會在傳統 IT 環境中考慮使用的服務。 自助或自動化部署通常是由應用程式開發或其他 IT 小組執行，傳統上並不會與生產環境部署一致。 在某些組織中，其商務組成也同樣具有類似的自助功能。 這可能會產生先前不存在於內部部署環境中的新安全性需求。 集中式安全性更具挑戰性，安全性通常會成為企業和 IT 文化的共同責任。 本文可以協助 CISO 針對該作法進行準備，並開始著手於漸進式的治理。

<!-- markdownlint-disable MD026 -->

## <a name="how-can-a-ciso-prepare-for-the-cloud"></a>CISO 如何為雲端做準備？

就像大多數的原則一樣，組織內的安全性和治理原則通常會增加成長。 發生安全性事件時，這些事件會塑造出原則來通知使用者，並降低相同事件重複發生的機會。 此方式雖然自然，卻會產生原則膨脹與技術相依性。 雲端轉型旅程建立了將原則現代化和重設的獨特機會。 在準備任何轉換旅程時，CISO 可透過擔任[原則檢閱](./cloud-policy-review.md)的主要關係人，來建立立即且可測量的值。

在這類審查中，CISO 的角色是在現有原則/合規性的限制與雲端提供者的改良安全性狀態之間建立安全的平衡。 此程序的測量可以透過許多形式達成，並通常會以可安全地卸載到雲端提供者的安全性原則數目來測量。

**傳輸安全性風險：** 當服務移至基礎結構即服務（IaaS）裝載模型時，企業會針對硬體布建承擔較不直接的風險。 但此風險並沒有就此消失，而是被轉換到雲端廠商身上。 如果雲端廠商的硬體布建方法提供相同層級的風險降低，在安全的可重複程式中，硬體布建執行的風險就會從公司 IT 的責任領域移除，並轉移給雲端提供者。 這可減少公司 IT 負責管理的整體安全性風險，雖然風險本身仍應定期追蹤和審核。

當解決方案進一步「向上堆疊」以併入平臺即服務（PaaS）或軟體即服務（SaaS）模型時，可以避免或轉移額外的風險。 當風險被安全地移到雲端提供者時，執行、監視及強制執行安全性原則或其他合規性原則的成本也都能安全地降低。

**成長思維：** 變更可能會對商務和技術實作者很可怕。 當組織中的 CISO 能夠率先轉換為成長心態時，我們發現那些自然而來的憂慮，都會轉換成對安全性及原則合規性與日俱增的興趣。 接近[原則審查](./cloud-policy-review.md)、轉換旅程或具有成長思維的簡單執行審查，可讓小組快速移動，而不是以公平且可管理的風險設定檔為代價。

## <a name="resources-for-the-chief-information-security-officer"></a>資訊安全主管的資源

對於雲端的認識，是抱持成長心態進行[原則檢閱](./cloud-policy-review.md)最重要的關鍵。 下列資源可協助 CISO 更加了解 Microsoft Azure 平台的安全性狀態。

<!-- docsTest:ignore "Security Response in the Cloud" -->

**安全性平台資源：**

- [安全性開發週期，內部審核](https://www.microsoft.com/sdl)
- [必要的安全性訓練、背景檢查](https://downloads.cloudsecurityalliance.org/star/self-assessment/StandardResponsetoRequestforInformationWindowsAzureSecurityPrivacy.docx)
- [滲透測試、入侵偵測、DDoS、稽核和記錄](https://www.microsoft.com/security/business/operations) \(英文\)
- [尖端的資料中心](https://www.microsoft.com/cloud-platform/global-datacenters)、實體安全性、[安全網路](https://docs.microsoft.com/azure/security/security-network-overview)
- [雲端的 Microsoft Azure 安全性回應](https://aka.ms/securityresponsepaper) \(英文\)

**隱私權與控制：**

- [隨時管理您的資料 (英文)](https://www.microsoft.com/trust-center/privacy/data-management)
- [資料位置控制 (英文)](https://www.microsoft.com/trust-center/privacy/data-location)
- [根據您的條款提供資料存取 (英文)](https://www.microsoft.com/trust-center/privacy/data-access)
- [回應執法機關 (英文)](https://www.microsoft.com/trust-center/privacy)
- [嚴格的隱私權標準 (英文)](https://www.microsoft.com/trust-center/privacy)

<!-- docsTest:ignore "Cloud Services Due Diligence Checklist" -->

**實現**

- [Microsoft 信任中心](https://www.microsoft.com/trust-center)
- [通用控制項中樞](https://www.microsoft.com/trust-center/compliance/compliance-overview)
- [雲端服務的因應的審慎檢查清單](https://www.microsoft.com/trust-center/compliance/due-diligence-checklist)
- [地區和國家/地區合規性](https://www.microsoft.com/trust-center/compliance/regional-country-compliance)

**無關**

- [Microsoft 如何保護 Azure 服務中的客戶資料 (英文)](https://www.microsoft.com/trust-center)
- [Microsoft 如何管理 Azure 服務中的資料位置](https://azuredatacentermap.azurewebsites.net) \(英文\)
- [Microsoft 內部的哪些人員可根據哪些條款存取您的資料 (英文)](https://www.microsoft.com/trust-center/privacy/data-access)
- [審查 Azure 服務、透明度中樞的認證](https://www.microsoft.com/trust-center/compliance/compliance-overview)

## <a name="next-steps"></a>後續步驟

在任何治理策略中採取動作的第一個步驟是[原則審查](./cloud-policy-review.md)。 [原則與合規性](./index.md)在原則審查期間可能是有用的指南。

> [!div class="nextstepaction"]
> [針對原則檢閱進行準備](./cloud-policy-review.md)
