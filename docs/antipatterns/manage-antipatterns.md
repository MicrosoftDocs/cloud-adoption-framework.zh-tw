---
title: 雲端操作和管理反模式
description: 引進或現代化 IT 工具不一定能保證更快交貨或更佳的業務成果。
author: lpassig
ms.author: brblanch
ms.date: 02/19/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: think-tank
ms.openlocfilehash: 629f4d0e9c1d675941e91a4fe75c3ae511effb91
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102117281"
---
# <a name="cloud-operation-and-management-antipatterns"></a>雲端操作和管理反模式

客戶通常會在雲端採用的作業或管理階段遇到反模式。 藉由謹慎引進新的或現代化工具鏈，您通常可以避免這些反模式。

## <a name="antipattern-focus-on-tooling-not-business-outcomes"></a>反模式：專注于工具，而不是商務成果

現代化 IT 工具可以藉由免除繁瑣工作的員工，來改善工作環境。 重要的是，您必須測量新的 IT 工具，以找出它是否能改善業務成果。 新的或現代化的工具鏈不會自動提供更快速的傳遞或更好的商務成果。

### <a name="example-introduce-a-platform-that-doesnt-improve-performance"></a>範例：引進無法改善效能的平臺

公司在 (CI/CD) 平臺的持續整合和持續傳遞中，引進了改良的全新版本。 此工具可讓您更輕鬆地定義傳遞和部署管線，讓您可以更快速地部署功能。 IT 部門的熱心是要提供可加速管線設定的平臺。 當業務單位使用此工具時，它會發現上市時間並不明顯，相較于舊的平臺。 最終的核准和發行程式未變更或改進。

### <a name="preferred-outcome-measure-success-with-business-outcomes"></a>慣用結果：使用商務成果測量成功

若要讓您的技術與商務目標保持一致，請讓兩個區域的領導者共同定義所需的結果。 請確定這些結果和目標都是特定、可測量、可達成、合理且有時間限制的 (智慧型) 。 確定結果和目標會影響技術與企業。 適用于 Azure 的 Microsoft 雲端採用架構有助於 [判斷企業內適當的承諾](/azure/cloud-adoption-framework/manage/considerations/commitment)用量。

請勿使用簡單的技術輸出 (例如，更快速的部署和管線設定) 來測量成功。 相反地，請使用技術和業務成果。 如需這項工作的協助，請參閱 [開發人員速度](https://azure.microsoft.com/overview/developer-velocity/)。

## <a name="next-step"></a>後續步驟

- [雲端管理的業務承諾](/azure/cloud-adoption-framework/manage/considerations/commitment)