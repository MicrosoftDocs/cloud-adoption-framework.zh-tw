---
title: 身分識別基準專業領域概觀
description: 了解開發身分識別基準專業領域作為雲端管理策略一部分的方法。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 92e793e2df89bbb70fe51fd7af9ef8fc1456ffa2
ms.sourcegitcommit: 7660521b631ea092fb805df9c9d28ad3024287ff
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/19/2020
ms.locfileid: "83620665"
---
# <a name="identity-baseline-discipline-overview"></a>身分識別基準專業領域概觀

身分識別基準是[雲端採用架構治理模型](../index.md)中的[五個雲端治理專業領域](../governance-disciplines.md)之一。 身分識別逐漸被視為雲端的主要安全邊界，從傳統著重於網路安全性轉變而來。 身分識別服務提供核心機制，以支援 IT 環境中的存取控制和組織，而身分識別基準專業領域可透過雲端採用工作一致地套用驗證和授權需求，以補充[安全性基準專業領域](../security-baseline/index.md)。

> [!NOTE]
> 身分識別基準專業領域不會取代可讓組織管理及保護身分識別服務的現有 IT 小組、流程和程序。 此專業領域的主要目的是要找出潛在的身分識別相關業務風險，並將風險降低指引提供給負責實作、維護及操作身分識別管理基礎結構的 IT 人員。 當您開發治理原則和流程時，請務必在規劃與檢閱流程中包含相關的 IT 小組。

＜雲端採用架構＞中的本節將概述開發身分識別基準專業領域作為雲端治理略一部分的方法。 本指引之主要適用對象為組織的雲端架構設計師和雲端治理小組的其他成員。 不過，從此專業領域衍生的決策、原則和流程，應涉及與負責實作和管理貴組織身分識別管理解決方案的 IT 小組相關成員進行互動與討論。

如果您的組織沒有身分識別和安全性方面的內部專業知識，應考慮聘請外部顧問投入此專業領域。 此外，考慮納入 [Microsoft Consulting Services](https://www.microsoft.com/industry/services/consulting)、[Microsoft FastTrack](https://azure.microsoft.com/programs/azure-fasttrack) 雲端採用服務，或聘請其他外部雲端採用合作夥伴來討論此專業領域的相關事宜。

## <a name="policy-statements"></a>Policy statements

可操作的原則聲明和所產生的架構需求，均可作為身分識別基準專業領域的基礎。 若要查看原則聲明範例，請參閱關於[身分識別基準原則聲明](./policy-statements.md)的文章。 這些範例可作為貴組織治理原則的起點。

> [!CAUTION]
> 範例原則來自於常見的客戶體驗。 若要進一步使這些原則與特定雲端治理需求保持一致，請執行下列步驟，以建立符合您獨特商務需求的原則聲明。

## <a name="develop-governance-policy-statements"></a>開發治理原則聲明

下列六個步驟提供在開發身分識別基準專業領域時所應考量的範例和可能選項。 使用每個步驟作為起點，在雲端治理小組中討論，並與組織內受影響的業務和 IT 小組討論，藉以建立管理身分識別相關風險所需的原則和程序。

<!-- markdownlint-disable MD033 -->

| | |
|---|---|
| <br> ![範本圖示](../../_images/govern/process-template.png)   | <br> [身分識別基準專業領域範本](./template.md)：下載此範本以便記錄身分識別基準專業領域。 |
| <br> ![風險圖示](../../_images/govern/process-risks.png)         | <br> [商務風險](./business-risks.md)：了解通常與身分識別基準專業領域相關聯的動機與風險。 |
| <br> ![計量圖示](../../_images/govern/process-metrics.png)     | <br> [指標和計量](./metrics-tolerance.md)：了解其是否為投資身分識別基準專業領域之正確時機的指標。 |
| <br> ![遵循圖示](../../_images/govern/process-enforce.png)   | <br> [原則遵循流程](./compliance-processes.md)：用以在身分識別基準專業領域中支援原則合規性的建議流程。 |
| <br> ![成熟度圖示](../../_images/govern/process-maturity.png)   | <br> [成熟度](./discipline-improvement.md)：使雲端管理成熟度與雲端採用階段保持一致。 |
| <br> ![工具鏈圖示](../../_images/govern/process-toolchain.png) | <br> [工具鏈](./toolchain.md)：可實作來支援身分識別基準專業領域的 Azure 服務。 |

<!-- markdownlint-enable MD033 -->

## <a name="next-steps"></a>後續步驟

開始評估特定環境中的[商務風險](./business-risks.md)。

> [!div class="nextstepaction"]
> [了解業務風險](./business-risks.md)
