---
title: 開始使用雲端採用架構的企業規模登陸區域
description: 開始使用適用於 Azure 的 Microsoft 雲端採用架構的企業級登陸區域。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: think-tank
ms.openlocfilehash: 40e0f6b736e2c009ecf173ac105c65c68aadc84d
ms.sourcegitcommit: b1217b40301583286a3d05032dbfd7a8e6b83fd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/08/2021
ms.locfileid: "99976230"
---
# <a name="start-with-cloud-adoption-framework-enterprise-scale-landing-zones"></a>開始使用雲端採用架構的企業規模登陸區域

企業級架構代表您 Azure 環境的策略性設計路徑和目標技術狀態。 其會隨著 Azure 平台持續發展，且由組織為了因應其 Azure 旅程所必須做出的各種設計決策來定義。

並非所有企業都會以相同的方式採用 Azure，因此 Azure 企業規模登陸區域架構的雲端採用架構會因客戶而異。 企業規模架構的技術考量和設計建議，可能會根據您組織的案例而導致不同的取捨。 預計會有一些變化，但您如果遵循核心建議，則所產生的目標架構會將您組織放在可持續調整的路徑上。

## <a name="prescriptive-guidance"></a>規範性指引

企業規模架構提供與 Azure 最佳做法結合的規範性指引。 其會遵循組織的 Azure 環境重要設計區域中的設計原則。

## <a name="qualifiers-should-i-start-with-enterprise-scale"></a>限定詞：我應該從企業規模開始嗎？

企業規模架構是依設計進行模組化。 可讓您從支援應用程式組合的基礎登陸區域著手，而不管應用程式是否正在遷移或是新開發並部署至 Azure。 不論調整點為何，此架構都可以隨著您的商務需求調整。

## <a name="start-with-a-cloud-adoption-framework-enterprise-scale-landing-zone"></a>開始使用雲端採用架構的企業規模登陸區域

用於建構登陸區域的企業級方法包含三組可支援雲端小組的資產：

- [設計指導方針](./design-guidelines.md)：驅動 Azure 企業規模登陸區域的雲端採用架構設計的重要決策指南。
- [架構](./architecture.md)：概念性參考架構，可示範設計區域和最佳做法。
- [實作](./implementation.md)：此架構的 Azure Resource Manager 範本，用於加速採用。

<!-- TODO: Reinstate once template.md is ready.
- [Template](./template.md): A documentation template to quickly capture decisions and any deviation from the suggested architecture or implementation.
-->

## <a name="community"></a>社群

<!-- docutune:ignore "Cloud Solutions Unit" -->

本指南主要是由 Microsoft 架構設計人員和更廣大的 Cloud Solutions Unit 技術社群所開發。 此社群積極地提升本指南，以分享在企業級採用過程中所學到的經驗。

本指南與標準的準備方法共用相同的設計原則。 其會展開這些原則，以便在規劃流程中整合先前治理和安全性等主體。 擴充標準程序是必要的，因為當採用工作需要大規模企業變更時，可以進行一些自然假設。

## <a name="next-steps"></a>後續步驟

[實作雲端採用架構的企業規模登陸區域](./implementation.md)
