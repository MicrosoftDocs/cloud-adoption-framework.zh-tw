---
title: 登陸區域的測試導向開發
description: 登陸區域的測試導向開發。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: internal
ms.openlocfilehash: d738d917f43c2d9e454e30fcb73b8030ba99fd9c
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101788125"
---
# <a name="test-driven-development-tdd-for-landing-zones"></a>登陸區域的測試驅動開發 (TDD)

測試導向開發是一種常見的軟體發展和 DevOps 程式，可改善任何程式碼架構解決方案中新功能和改良功能的品質。 雲端式基礎結構和基礎原始程式碼可以使用此程式來確保登陸區域符合核心需求，而且具有高品質。 當登陸區域在平行開發工作中進行開發和重構時，此程式特別有用。

![雲端登陸區域的測試導向開發流程](../../_images/ready/test-driven-development-process.png)

在雲端中，基礎結構是程式碼執行的輸出。 結構完善、經過測試且經過驗證的程式碼會產生可行的登陸區域。 [登陸區域](../landing-zone/index.md)是用來裝載工作負載的環境，可透過程式碼 preprovisioned。 它包含基本功能，並使用一組已定義的雲端服務和最佳作法，讓您能成功進行設定。 本指南說明使用測試導向開發來滿足該定義最後一部分的方法，同時符合品質、安全性、作業和治理需求。

這種方法可在早期開發期間用來滿足簡單的功能要求。 稍後在雲端採用生命週期中，此程式可以用來符合安全性、作業、治理或合規性需求。

## <a name="definition-of-done"></a>對完成的定義

「設定為成功」是主觀的陳述。 此聲明會在登陸區域開發或重構工作期間，提供雲端平臺小組一些可採取動作的資訊。 這種缺乏清楚的方式可能會導致雲端環境中遺失期望和弱點。 在重構或擴充任何登陸區域之前，雲端平臺小組應針對每個登陸區域的「完成定義」尋找更清楚的資訊。

完成的定義是雲端平臺小組與其他受影響小組之間的簡單合約。 本合約概述預期的值新增功能，這些功能應該包含在任何登陸區域開發工作中。 完成的定義通常是與短期雲端採用方案一致的檢查清單。 在成熟的流程中，檢查清單中的預期功能各有自己的驗收準則，可以更清楚地建立更清楚的程式。 當每個值新增的功能都符合接受準則時，即會充分設定登陸區域，以成功進行目前的浪潮或使用中的採用工作。

當小組採用額外的工作負載和雲端功能時，完成和驗收準則的定義將會變得越來越複雜。

## <a name="test-driven-development-cycle"></a>測試導向的開發週期

讓測試導向開發生效的迴圈通常稱為紅色/綠色測試。 在此方法中，雲端平臺小組會根據完成和定義的驗收準則定義，從失敗的測試 (red test) 。 針對每個功能或驗收準則，雲端平臺小組會完成開發工作，直到測試通過 (環保測試) 為止。 測試導向的開發週期 (或 red/綠測試) 會重複下圖中的基本步驟，並列出下列清單，直到完成完整定義為止。

![雲端登陸區域的測試導向開發流程](../../_images/ready/test-driven-development-process.png)

- **建立測試：** 定義測試以驗證是否符合特定值-新增功能的接受準則。 盡可能將測試自動化。
- **測試登陸區域：** 執行新的測試和任何現有的測試。 如果先前的開發工作尚未符合所需的功能，而且不包含在雲端提供者的供應專案中，則測試應該會失敗。 執行現有的測試將有助於驗證新的測試不會降低現有程式碼所提供之登陸區域功能的可靠性。
- **展開並重構登陸區域：** 新增或修改原始程式碼，以滿足要求的「值新增」功能，並改善程式碼基底的一般品質。 為了符合測試導向開發的最大精神，雲端平臺小組只會新增程式碼來滿足所要求的功能，而不會再新增任何專案。 同時，程式碼品質和維護是共用的工作。 當您完成新的功能要求時，雲端平臺小組應藉由移除重複專案並闡明程式碼來尋找改善程式碼。 強烈建議在新的程式碼建立和原始程式碼重構之間執行測試。
- **部署登陸區域：** 一旦原始程式碼能夠完成功能要求，請將修改過的登陸區域部署到受控制測試或沙箱環境中的雲端提供者。
- **測試登陸區域：** 重新測試登陸區域應該會驗證新的程式碼是否符合所要求功能的接受準則。 一旦所有測試都通過之後，此功能就會被視為已完成，並將驗收準則視為符合。

當所有值新增的功能和驗收準則都通過其相關聯的測試時，登陸區域便已準備好支援雲端採用計畫的下一波。

## <a name="simple-example-of-a-definition-of-done"></a>完成定義的簡單範例

對於初始的遷移工作，完成的定義可能會太簡單。 以下是其中一個過於簡單範例的範例。

- 初始登陸區域將用來裝載10個工作負載，以供初始學習之用。 這些工作負載對企業並不重要，而且無法存取機密資料。 未來可能會將這些工作負載發行至生產環境，但重要性和敏感性不應變更。 為了支援這些工作負載，雲端採用小組需要符合下列準則：

- 網路分割以配合建議的網路設計。
- 存取計算、儲存和網路資源，以裝載與數位資產探索一致的工作負載。
- 命名和標記架構以方便使用。
- 此環境應該視為可存取公用網際網路的周邊網路。
- 在採用工作期間，雲端採用小組會想要暫時存取環境來變更服務設定。
- 僅限感知：在生產環境發行之前，這些工作負載需要與公司身分識別提供者整合，以管理進行中的身分識別和存取，以進行作業管理。 此時應撤銷雲端採用小組的存取權。

上述最後一個點不是功能或接受準則。 但這是一個指出需要額外擴充的指標，且應該提早與其他小組進行探索。

## <a name="additional-examples-of-a-definition-of-done"></a>完成定義的其他範例

雲端採用架構中的控管方法，提供了一段敘述性旅程，讓您瞭解治理小組的自然成熟度。 內嵌在該旅程中的數個範例是「完成定義」和「驗收準則」（以原則聲明的形式）。

- [初始原則聲明](../../govern/guides/complex/initial-corporate-policy.md#policy-statements)：根據早期階段採用需求，管理和初始定義完成的公司原則範例。
- 身分 [識別擴充](../../govern/guides/complex/identity-baseline-improvement.md#incremental-improvement-of-the-policy-statements)：公司原則的範例，負責管理 *完成的定義*，以符合擴充登陸區域身分識別管理的需求。
- [安全性擴充](../../govern/guides/complex/security-baseline-improvement.md#incremental-improvement-of-the-policy-statements)：公司原則的範例，控管 *完成的定義* 符合參考雲端採用方案的安全性需求。
- [作業擴充](../../govern/guides/complex/resource-consistency-improvement.md#incremental-improvement-of-the-policy-statements)：公司原則的範例，控管 *完成的定義* 符合基本作業管理需求。
- [成本擴充](../../govern/guides/complex/cost-management-improvement.md#changes-to-the-policy-statements)：公司原則的範例，控管完成以符合成本管理需求的 *定義* 。

上述範例是基本的範例，可協助開發針對登陸區域 *完成的定義* 。 [雲端治理的五個專業領域](../../govern/governance-disciplines.md)都有其他範例原則可供使用。

## <a name="next-steps"></a>下一步

若要在 Azure 中加速測試導向開發，請參閱 [azure 的測試導向開發功能](./azure-test-driven-development.md)。

> [!div class="nextstepaction"]
> [Azure 中的測試驅動開發](./azure-test-driven-development.md)
