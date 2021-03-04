---
title: 管理反模式
description: 採用雲端時，請瞭解公司責任、雲端提供者責任，以及雲端治理和安全性標準。
author: sarahwendel
ms.author: brblanch
ms.date: 02/18/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: think-tank
ms.openlocfilehash: cafd35c3353e9cb54bd8d57d938e96c3044cb9f0
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102117282"
---
# <a name="govern-antipatterns"></a>管理反模式

客戶通常會在雲端採用的治理階段遇到反模式。 花時間瞭解共用職責可協助您避免這些反模式，因為在現有的架構上建立您的安全性策略，而不是建立自己的架構。

## <a name="antipattern-misunderstand-shared-responsibilities"></a>反模式：誤解共同責任

當您採用雲端時，不一定會清楚您的責任結束，且雲端提供者的責任會開始與不同的服務模型有關。 需要雲端技能和知識，才能在使用服務模型的工作專案周圍建立流程和實務。

### <a name="example-assume-the-cloud-provider-manages-updates"></a>範例：假設雲端提供者管理更新

公司人力資源 (HR) 部門的成員會使用基礎結構即服務 (IaaS) ，在雲端中設定許多 Windows 伺服器。 他們假設雲端提供者會管理更新，因為現場網站通常會處理更新安裝。 HR 部門不會設定更新，因為它們並不知道 Azure 預設不會部署和安裝作業系統更新。 因此，伺服器不符合規範，而且會造成安全性風險。

### <a name="preferred-outcome-create-a-readiness-plan"></a>慣用結果：建立就緒計畫

瞭解雲端的 [共同責任](/azure/security/fundamentals/shared-responsibility) 。 建立並建立 [就緒計畫](../plan/adapt-roles-skills-processes.md)。 準備就緒計畫可為學習和開發專業知識創造持續的動力。

## <a name="antipattern-assume-out-of-the-box-solutions-provide-security"></a>反模式：假設現成的解決方案提供安全性

公司通常會在雲端中察覺到安全性。 雖然這項假設通常是正確的，但大部分的環境也都必須遵守合規性架構需求，這可能與安全性需求不同。 Azure 提供基本安全性。 然後，透過 [azure 資訊安全中心](/azure/security-center/)， [azure 入口網站](https://portal.azure.com) 可協助改善安全性。 但是，當您建立訂用帳戶時，強制執行合規性和安全性標準並不是現成的體驗。

### <a name="example-neglect-cloud-security"></a>範例：忽略雲端安全性

公司在雲端開發新的應用程式。 它會根據許多平臺即服務來選擇架構， (PaaS) 服務，再加上一些用於進行偵錯工具的 IaaS 元件。 在將應用程式發行至生產環境之後，公司發現其中一個跳躍伺服器已遭入侵，並正在將資料解壓縮至未知的 IP 位址。 公司發現問題是跳躍伺服器的公用 IP 位址，以及容易猜到的密碼。 如果該公司已專注于雲端安全性，就可以避免此情況。

### <a name="preferred-outcome-define-a-cloud-security-strategy"></a>慣用結果：定義雲端安全性策略

定義適當的雲端 [安全性策略](../strategy/define-security-strategy.md)。 如需詳細資訊，請參閱 [CISO 雲端就緒指南](../govern/policy-compliance/cloud-security-readiness.md) ，請參閱本指南的資訊安全辦公室 (CISO) 。 它會討論安全性平臺資源、隱私權與控制、合規性和透明度等主題。

深入瞭解 [Azure 安全性基準測試](/azure/security/benchmarks/introduction)中的安全雲端工作負載。 [以網際網路安全性為中心的 (CIS) 控制7.1 版](https://learn.cisecurity.org/cis-controls-download)和[美國國家標準規範的標準與技術 (NIST) SP800-53 架構](https://www.nist.gov/privacy-framework/nist-sp-800-53)，可解決大部分的安全性風險和措施。

使用 Azure 安全性中心來識別風險、調整最佳做法，以及改善公司的安全性狀態。

使用 [Azure 原則](/azure/governance/policy/overview) 和 [azure 藍圖](/azure/governance/blueprints/overview)，來實行或支援公司特定、自動化的合規性和安全性需求。

## <a name="antipattern-use-a-custom-compliance-or-governance-framework"></a>反模式：使用自訂合規性或治理架構

引進的自訂合規性和治理架構不是以業界標準為基礎，因為它可能很難將自訂架構轉譯為雲端設定，所以會大幅增加雲端採用時間。 這種情況可能會增加將自訂量值和需求轉譯成而且實作安全性控制所需的工作。 大部分的公司都必須遵守類似的安全性與合規性需求集合。 如此一來，大部分的自訂合規性和安全性架構都只會與目前的合規性架構稍有不同。 具有額外安全性需求的公司，可以考慮建立新的架構。

### <a name="example-use-a-custom-security-framework"></a>範例：使用自訂安全性架構

公司 CISO 為 IT 安全性員工指派雲端安全性策略與架構的工作。 IT 安全性部門不會根據產業標準建立新的架構，而是以目前的內部部署安全性原則為基礎。 完成雲端安全性原則之後，AppOps 和 DevOps 團隊就無法執行雲端安全性原則。

Azure 提供更完整的安全性與合規性結構，與公司本身的架構不同。 CISO 團隊認為 Azure 控制項與其本身的合規性和安全性規則不相容。 如果它是以標準化控制項的架構為基礎，就不會有這種結論。

### <a name="preferred-outcome-rely-on-existing-frameworks"></a>慣用結果：依賴現有的架構

在您建立或引進自訂公司合規性架構之前，請先使用或建立現有的架構（例如 CIS 控制項版本7.1 或 NIST SP800-53）。 現有的架構可讓轉換至雲端安全性設定變得更容易且更具測量。 在 [Azure 藍圖範例頁面](/azure/governance/blueprints/samples)上尋找更多架構的架構。 適用于 Azure 的通用合規性架構藍圖也可供使用。

## <a name="next-steps"></a>下一步

- [調整雲端的現有角色、技能和流程](../plan/adapt-roles-skills-processes.md)
- [定義安全性策略](../strategy/define-security-strategy.md)
