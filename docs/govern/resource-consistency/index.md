---
title: 資源一致性專業領域的概觀
description: 了解開發資源一致性專業領域作為雲端管理策略一部分的方法。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: internal
ms.openlocfilehash: ca7e8adc580f1ec751e00db0bcc34d2bf81bf2ba
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97019582"
---
# <a name="resource-consistency-discipline-overview"></a>資源一致性專業領域的概觀

資源一致性是[雲端採用架構治理模型](../index.md)中的[五個雲端治理專業領域](../governance-disciplines.md)之一。 這個專業領域著重於訂定與環境、應用程式或工作負載之作業管理相關的原則。 IT 作業小組通常會監視應用程式、工作負載和資產效能。 他們也經常執行必要的工作，以符合擴展需求、補救效能服務等級協定 (SLA) 違規，和藉由自動補救的方式主動避免效能 SLA 違規。 在雲端治理的五個專業領域中，資源一致性專業領域可確保資源以一致的方式設定，以便 IT 作業能夠找到這些資源；此外，這些資源包含在復原解決方案中，且可納入可重複的作業程序中。

> [!NOTE]
> 資源一致性專業領域不會取代可讓組織有效管理雲端式資源所用的現有 IT 小組、流程和程序。 此專業領域的主要目的是識別潛在商務風險，且為負責管理雲端資源的 IT 人員提供降低風險指引。 當您開發治理原則和流程時，請務必在規劃與檢閱流程中包含相關的 IT 小組。

雲端採用架構的這個部分概述如何在雲端治理策略中開展資源一致性專業領域。 本指引之主要適用對象為組織的雲端架構設計師和雲端治理小組的其他成員。 從此專業領域衍生的決策、原則和流程，應包含與負責實作和管理組織資源一致性解決方案的 IT 小組相關成員進行互動與討論。

如果您的組織沒有資源一致性策略的內部專業知識，應考慮聘請外部顧問投入此專業領域。 此外，考慮納入 [Microsoft Consulting Services](https://www.microsoft.com/industry/services/consulting)、[Microsoft FastTrack](https://azure.microsoft.com/programs/azure-fasttrack) 雲端採用服務，或聘請其他外部雲端採用專家來討論如何最充分組織、追蹤和最佳化雲端式資產。

## <a name="policy-statements"></a>Policy statements

可採取動作的原則聲明和所產生架構需求，均可作為資源一致性專業領域的基礎。 使用[範例原則聲明](./policy-statements.md)。 這些範例可作為定義資源一致性原則的起點。

> [!CAUTION]
> 範例原則來自於常見的客戶體驗。 若要進一步使這些原則與特定雲端治理需求保持一致，請執行下列步驟，以建立符合您獨特商務需求的原則聲明。

## <a name="develop-governance-policy-statements"></a>開發治理原則聲明

下列步驟提供在開發資源一致性專業領域時所應考量的範例和可能選項。 請使用每個步驟作為起點，在雲端治理小組中討論，並與組織內受影響的業務和 IT 小組討論，藉以建立管理資源一致性專業領域風險所需的原則和程序。

| <span title="圖示">&nbsp;</span> | <span title="描述">&nbsp;</span> |
|--|--|
| <br> ![範本圖示](../../_images/govern/process-template.png) | <br> [資源一致性專業領域範本](./template.md)：下載此範本以記錄資源一致性專業領域。 |
| <br> ![風險圖示](../../_images/govern/process-risks.png) | <br> [商務風險](./business-risks.md)：了解通常與資源一致性專業領域建立關聯的動機與風險。 |
| <br> ![計量圖示](../../_images/govern/process-metrics.png) | <br> [指標和計量](./metrics-tolerance.md)：了解其是否為投資資源一致性專業領域正確時機的指標。 |
| <br> ![遵循圖示](../../_images/govern/process-enforce.png) | <br> [原則遵循流程](./compliance-processes.md)：用以在資源一致性專業領域中支援原則合規性的建議流程。 |
| <br> ![成熟度圖示](../../_images/govern/process-maturity.png) | <br> [成熟度](./discipline-improvement.md)：使雲端管理成熟度與雲端採用階段保持一致。  |
| <br> ![工具鏈圖示](../../_images/govern/process-toolchain.png) | <br> [工具鏈](./toolchain.md)：可實作來支援資源一致性專業領域的 Azure 服務。 |

## <a name="next-steps"></a>後續步驟

開始評估特定環境中的[商務風險](./business-risks.md)。

> [!div class="nextstepaction"]
> [了解業務風險](./business-risks.md)
