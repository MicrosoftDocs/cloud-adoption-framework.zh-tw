---
title: 部署加速的計量、指標及風險承受度
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 部署加速的計量、指標及風險承受度
author: alexbuckgit
ms.author: abuck
ms.date: 02/11/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 5049b41abc03c5f59d0d750373b48a39b0638084
ms.sourcegitcommit: 443c28f3afeedfbfe8b9980875a54afdbebd83a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2019
ms.locfileid: "71029771"
---
# <a name="deployment-acceleration-metrics-indicators-and-risk-tolerance"></a>部署加速的計量、指標及風險承受度

本文旨在協助您量化商務風險承受度，因為這與部署加速息息相關。 定義計量和指標可協助您建立商務案例，藉此讓部署加速專業領域變得更完善。

## <a name="metrics"></a>計量

部署加速專業領域著重于如何設定、部署、更新和維護雲端資源的相關風險。 在採用此雲端治理專業領域時，下列資訊會很有用：

- **部署失敗：** 失敗或導致資源設定錯誤的部署百分比。
- **部署時間：** 將更新部署至現有系統所需的時間量。
- **資產不符合規範：** 與所定義原則不符的資源數量或百分比。

## <a name="risk-tolerance-indicators"></a>風險承受度指標

與部署加速相關的風險，大多與企業所部署雲端式系統的數量和複雜度相關。 隨著雲端資產的增加，所部署的系統數量和雲端資源的更新頻率也會增加。 由於資源彼此相依，確保資源設定正確以及為系統設計復原能力以在一或多個資源發生非預期停機時可以恢復運作，就變得更加重要。

<!-- "en-us" location is required for the URL below. -->

在雲端採用旅程的初期，就請考慮採用 DevOps 或 [DevSecOps](https://www.microsoft.com/en-us/securityengineering/devsecops) 組織文化。 傳統的公司 IT 組織通常會有孤立的作業、安全性和開發團隊，這些小組通常無法正常合作，或甚至彼此形成對立或惡意。 及早認清這些挑戰並整合來自各個小組的重要關係人，將有助於企業確保在採用雲端時的靈活度，而又保有安全且治理完善的環境。

請與 DevSecOps 小組和業務關係人一同找出與設定相關的[業務風險](./business-risks.md)，然後判斷在設定風險承受度時可接受的基準。 雲端採用架構指引的這一節提供範例，但您公司或部署的詳細風險和基準可能會有所不同。

擁有基準之後，請建立最低基準，來指出所識別的風險增加到什麼程度會讓您無法接受。 當您需要採取動作來補救這些風險時，這些基準測試會作為觸發程式。 以下幾個例子會說明，與設定相關的計量 (例如，前面討論過的那些) 如何證明您應該提高對於部署加速專業領域的投資。

- **設定漂移觸發程式：** 公司若遭遇重要系統元件的設定發生非預期的變更，或未能部署或更新其系統，就應該投資部署加速專業領域，以找出根本原因和補救步驟。
- **不符合規範的觸發程式：** 如果不符合規範的資源數量超過所定義的臨界值 (無論是資源總數還是佔總資源的百分比)，公司就應該投資改善部署加速專業領域，以確保每個資源的設定在該資源的生命週期內皆會保持符合規範。
- **專案排程觸發程式：** 如果部署公司資源和應用程式的時間經常超過所定義的臨界值，公司就應該投資其部署加速流程，以導入或改善自動化部署來獲得一致性和可預測性。 部署時間若以幾天甚至是幾週為單位來計算，通常表示這不是最佳的部署加速策略。

## <a name="next-steps"></a>後續步驟

使用[雲端管理範本](./template.md)，記錄與目前的雲端採用方案一致的計量和承受度指標。

請參閱範例部署加速原則作為起點，以開發可解決符合您雲端採用方案之特定商務風險的原則。

> [!div class="nextstepaction"]
> [審查範例原則](./policy-statements.md)
