---
title: 安全性基準專業領域概觀
description: 了解開發安全性基準專業領域作為雲端管理策略一部分的方法。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 17b40a610bae3cd8af6b35dd8f95797c720b473f
ms.sourcegitcommit: 07d56209d56ee199dd148dbac59671cbb57880c0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88879093"
---
# <a name="security-baseline-discipline-overview"></a>安全性基準專業領域概觀

安全性基準是[雲端採用架構治理模型](../index.md)中的[五個雲端治理專業領域](../governance-disciplines.md)之一。 安全性是任何 IT 部署的元件，而雲端本身有獨特的安全性問題。 許多企業都必須遵循法規需求，這些需求使得考量雲端轉換時保護敏感性資料成為組織的當務之急。 識別雲端環境潛在安全威脅並建立解決這些威脅的流程和程序，應該是任何 IT 安全性或網路安全性小組的首要任務。 隨著這些需求逐漸具體明朗，安全性基準專業領域可確保技術需求和安全性限制一致套用至雲端環境。

> [!NOTE]
> 安全性基準專業領域不會取代可讓組織保護雲端部署的資源所用的現有 IT 小組、流程和程序。 此專業領域的主要目的是識別與安全性相關的商務風險，且為負責安全性基礎結構的 IT 人員提供降低風險指引。 當您開發治理原則和流程時，請務必在規劃與檢閱流程中包含相關的 IT 小組。

本文概述開發安全性基準專業領域作為雲端治理策略一部分的方法。 本指引之主要適用對象為組織的雲端架構設計師和雲端治理小組的其他成員。 從此專業領域衍生的決策、原則和流程，應會涉及與您的 IT 和安全性小組相關成員進行互動與討論，特別是負責實作網路、加密和身分識別服務的技術主管。

對於雲端部署成功和更成功的業務推展而言，制定正確的安全性決策極為重要。 如果貴組織沒有網路安全性方面的內部專業知識，請考慮聘請外部安全顧問成為此專業領域的一部分。 此外，考慮納入 [Microsoft Consulting Services](https://www.microsoft.com/industry/services/consulting)、[Microsoft FastTrack](https://azure.microsoft.com/programs/azure-fasttrack) 雲端採用服務，或聘請其他外部雲端採用專家來討論此專業領域的相關事宜。

## <a name="policy-statements"></a>Policy statements

可採取動作的原則聲明和所產生架構需求，均可作為安全性基準專業領域的基礎。 使用[範例原則聲明](./policy-statements.md)作為定義安全性基準原則的起點。

> [!CAUTION]
> 範例原則來自於常見的客戶體驗。 若要進一步使這些原則與特定雲端治理需求保持一致，請執行下列步驟，以建立符合您獨特商務需求的原則聲明。

## <a name="develop-governance-policy-statements"></a>開發治理原則聲明

下列步驟提供在開發安全性基準專業領域時所應考量的範例和可能選項。 使用每個步驟作為起點，在雲端治理小組中討論以及與組織內受影響的業務和 IT 和安全性小組討論，藉以建立管理安全性相關風險所需的原則和程序。

|  |  |
|--|--|
| <br> ![範本圖示](../../_images/govern/process-template.png) | <br> [安全性基準專業領域範本](./template.md)：下載此範本以記錄安全性基準專業領域。 |
| <br> ![風險圖示](../../_images/govern/process-risks.png) | <br> [商務風險](./business-risks.md)：了解通常與安全性基準專業領域建立關聯的動機與風險。 |
| <br> ![計量圖示](../../_images/govern/process-metrics.png) | <br> [指標和計量](./metrics-tolerance.md)：用於了解其是否為投資安全性基準專業領域正確時機的指標。 |
| <br> ![遵循圖示](../../_images/govern/process-enforce.png) | <br> [原則遵循流程](./compliance-processes.md)：用以在安全性基準專業領域中支援原則合規性的建議流程。 |
| <br> ![成熟度圖示](../../_images/govern/process-maturity.png) | <br> [成熟度](./discipline-improvement.md)：使雲端管理成熟度與雲端採用階段保持一致。 |
| <br> ![工具鏈圖示](../../_images/govern/process-toolchain.png) | <br> [工具鏈](./toolchain.md)：可實作用來支援安全性基準專業領域的 Azure 服務。 |

## <a name="next-steps"></a>後續步驟

開始評估特定環境中的商務風險。

> [!div class="nextstepaction"]
> [了解業務風險](./business-risks.md)
