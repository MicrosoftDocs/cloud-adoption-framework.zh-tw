---
title: 安全性基準專業領域改進
description: 瞭解公司在雲端採用的每個階段開發和成熟安全性基準專業領域時，所執行的潛在工作。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: f240ca121f52d08d1a666c7a5f0ee5f396779c7c
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88569003"
---
# <a name="security-baseline-discipline-improvement"></a>安全性基準專業領域改進

安全性基準專業領域著重于建立原則的方式，以保護網路、資產，以及最重要的資料，也就是位於雲端提供者解決方案上的資料。 在雲端治理的五個專業領域中，安全性基準專業領域包含數位資產和資料的分類。 此外也包含與資料、資產和網路的安全性相關聯的風險、商業容忍度和風險降低策略的文件說明。 從技術觀點來看，這也包括有關 [加密](../../decision-guides/encryption/index.md)、 [網路需求](../../decision-guides/software-defined-network/index.md)、混合式身分 [識別策略](../../decision-guides/identity/index.md)， [以及用來](./compliance-processes.md) 開發雲端安全性基準原則之程式的參與考慮。

本文將概述一些貴公司可參與的潛在工作，以更好的方式來開發安全性基準專業領域並使其臻至成熟。 這些工作可以細分為實作雲端解決方案的規劃、建置、採用及操作階段，接著反覆執行以允許開發[雲端治理的累加方法](../guides/index.md#an-incremental-approach-to-cloud-governance)。

![四個採用階段](../../_images/govern/adoption-phases.png)

_圖1：雲端治理增量方法的採用階段。_

沒有任何一份文件能夠滿足所有企業需求。 因此，本文將針對治理成熟流程的每個階段，概述建議的最小和潛在範例活動。 這些活動的初始目標是協助您建立 [原則 MVP](../guides/index.md#an-incremental-approach-to-cloud-governance) ，並建立用於增量原則改進的架構。 您的雲端治理小組必須決定投資這些活動的數量，以改善您的安全性基準專業領域。

> [!CAUTION]
> 本文所述的最小或潛在活動都不符合特定的公司原則或協力廠商合規性需求。 此指導方針旨在協助促成交談，從而使這兩個需求與雲端治理模型保持一致。

## <a name="planning-and-readiness"></a>規劃和整備

這個治理成熟度階段可消彌業務成果與可操作之策略間的鴻溝。 在此程序中，領導小組會定義特定的計量、將這些計量對應至數位資產，並開始規劃整體移轉工作。

**最小的建議活動：**

- 評估您的[安全性基準工具鏈](./toolchain.md)選項。
- 開發草稿架構指導方針檔，並散發給重要的專案關係人。
- 教育並涵蓋受到開發架構指導方針影響的人員和小組。
- 將已設定優先權的安全性工作新增至您的移轉待辦項目中。

**潛在的活動：**

- 定義資料分類結構描述。
- 進行數位資產規劃程序以清查涉及您商務程序並支持營運的目前 IT 資產。
- 進行[原則檢閱](../../govern/policy-compliance/cloud-policy-review.md)以開始將現有的公司 IT 安全性原則現代化並定義 MVP 原則以因應已知風險。
- 檢閱您雲端平台的安全性指導方針。 針對 Azure，可以在 [Microsoft 服務信任入口網站](https://servicetrust.microsoft.com)中找到。
- 判斷您的安全性基準原則是否包含 [安全性開發生命週期](https://www.microsoft.com/sdl)。
- 根據接下來的一到三個版本評估網路、資料與資產相關商務風險，並判定您組織對那些風險的容忍度。
- 複習 Microsoft [在網路安全性報告中的熱門趨勢](https://www.microsoft.com/security/operations/security-intelligence-report) ，以瞭解目前的安全性環境。
- 請考慮在您的組織中開發 [DevSecOps](https://www.microsoft.com/devsecops) 角色。

## <a name="build-and-predeployment"></a>組建和部署

成功遷移環境需要數個技術性和非技術性的必要條件。 此程序著重於可繼續進行移轉的決策、整備和核心基礎結構。

**最小的建議活動：**

- 在預先部署階段推出，以實行 [安全性基準工具鏈](./toolchain.md) 。
- 更新架構指導方針檔，並散發給重要的專案關係人。
- 在已設定優先權的移轉待辦項目上實作安全性工作。
- 開發教育性資料和文件、認知溝通、獎勵和其他計畫，以協助試用產品的使用者採用。

**潛在的活動：**

- 決定您組織的雲端裝載資料[加密](../../decision-guides/encryption/index.md)策略。
- 評估您雲端部署的[身分識別](../../decision-guides/identity/index.md)策略。 決定您的雲端式身分識別解決方案將與內部部署身分識別提供者共存或整合。
- 決定您[軟體定義網路 (SDN)](../../decision-guides/software-defined-network/index.md) 設計的網路界限原，以確保可以獲得安全的虛擬化網路功能。
- 評估您組織的 [最低許可權存取](/azure/active-directory/users-groups-roles/roles-delegate-by-task) 原則，並使用以工作為基礎的角色來提供特定資源的存取權。
- 將安全性與監視機制套用至所有雲端服務和虛擬機器。
- 在可能的情況下自動化[安全性原則](../../decision-guides/policy-enforcement/index.md)。
- 請檢查您的安全性基準原則，並判斷您是否需要根據最佳做法指導方針來修改方案，例如 [安全性開發生命週期](https://www.microsoft.com/sdl)中所述。

## <a name="adopt-and-migrate"></a>採用和移轉

移轉是一個累加式程序，著重於在現有的數位資產中移動、測試及採用應用程式或工作負載。

**最小的建議活動：**

- 將您的 [安全性基準工具鏈](./toolchain.md) 從預先部署遷移至生產環境。
- 更新架構指導方針檔，並散發給重要的專案關係人。
- 開發教育性資料和文件、認知溝通、獎勵和其他計畫，以協助試用產品的使用者採用。

**潛在的活動：**

- 請參閱最新的安全性基準和威脅資訊，以找出任何新的商務風險。
- 判斷您組織的容忍度以處理可能會發生的新安全性威脅。
- 找出來自原則的偏差，並強制進行修正。
- 調整安全性與存取控制自動化，以確保可以獲得最大的原則合規性。
- 驗證在組建和預先部署階段期間定義的最佳作法是否正確執行。
- 檢查您的最低許可權存取原則，並調整存取控制以將安全性最大化。
- 針對您的工作負載測試您的安全性基準工具鏈，以找出並解決任何弱點。

## <a name="operate-and-post-implementation"></a>操作和實作後

轉換完成之後，治理和操作必須依存於應用程式或工作負載的自然生命週期。 這個治理成熟度階段著重於通常會在實作解決方案且轉換週期開始穩定後隨之而來的活動。

**最小的建議活動：**

- 驗證並精簡您的 [安全性基準工具鏈](./toolchain.md)。
- 自訂通知與報告，以在發生潛在安全性問題時接收通知。
- 精簡架構指導方針，以引導未來的採用流程。
- 定期與受影響的小組溝通並教育他們，以確保會持續遵循架構指導方針。

**潛在的活動：**

- 探索您工作負載的模式與行為，並設定您的監視與報告工具，以偵測任何異常活動、存取或資源使用狀況並通知您。
- 持續更新您的監視與報告原則，以偵測最新的弱點、漏洞與攻擊。
- 備妥適當的程序，以快速停止未經授權存取並停用可能已被攻擊者入侵的資源。
- 定期檢閱最新的安全性最佳做法，並在可能的情況下套用建議到您的安全性原則、自動化與監視功能。

## <a name="next-steps"></a>後續步驟

現在您已了解雲端安全性治理的概念，請繼續深入了解 [Microsoft 為 Azure 提供哪些安全性與最佳做法](./azure-security-guidance.md)。

> [!div class="nextstepaction"]
> [瞭解 Azure](./azure-security-guidance.md) 
>  的安全性指引[Azure 安全性簡介](/azure/security/fundamentals/overview) 
> [瞭解記錄、報告和監視](../../decision-guides/logging-and-reporting/index.md)
