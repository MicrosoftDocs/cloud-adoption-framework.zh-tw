---
title: Azure 中登陸區域的測試導向開發
description: Azure 中登陸區域的測試導向開發。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: internal
ms.openlocfilehash: 8529745bb200361247fbcff04f43e74c861379ee
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97024223"
---
# <a name="test-driven-development-for-landing-zones-in-azure"></a>Azure 中登陸區域的測試導向開發

如同上一篇有關 [測試導向開發 (TDD) 針對登陸區域](./test-driven-development.md)所述，tdd 迴圈開始的測試會驗證傳遞雲端採用計畫所需之特定功能的接受準則。 然後，您可以測試登陸區域的展開或重構，以驗證是否符合接受準則。 本文概述 Azure 中的雲端原生工具鏈，以自動化測試導向的開發週期。

## <a name="azure-tools-to-support-landing-zone-tdd-cycles"></a>支援登陸區域 TDD 週期的 Azure 工具

![Azure 圖1中的測試導向開發工具 ](../../_images/ready/azure-tdd-tools.png)
 _： azure 中的測試驅動開發工具。_

Azure 原生治理產品和服務的工具鏈可以輕鬆地整合到測試導向開發，以建立登陸區域。 這些工具每一個都有特定用途，讓您更容易開發、測試及部署登陸區域，以配合 TDD 週期進行調整。

## <a name="microsoft-provided-test-and-deployment-templates-to-accelerate-tdd"></a>Microsoft 提供的測試和部署範本，以加速 TDD

下列範例是由 Microsoft 提供，用於治理用途。 每個都可用來做為登陸區域的測試導向開發週期中的測試或一系列測試。 下列各節提供有關每個工具的詳細資訊：

- Azure 藍圖提供各種 [藍圖範例](/azure/governance/blueprints/samples)，包括用於測試的原則，以及用於部署的範本。 這些藍圖範例可以加速在 TDD 週期中的開發、部署和測試工作。
- Azure 原則也包含 [內建原則方案](/azure/governance/policy/samples/built-in-initiatives)，可用來測試並強制執行登陸區域完成的完整定義。 Azure 原則包含內 [建原則定義](/azure/governance/policy/samples/built-in-policies) ，這些定義可以符合完成定義內的個別接受準則。
- Azure Graph 包含 advanced [query 範例](/azure/governance/resource-graph/samples/advanced)，可用來瞭解如何在登陸區域內部署工作負載，以進行先進的測試案例。
- [Azure 快速入門範本](https://azure.microsoft.com/resources/templates) 提供原始程式碼範本，可協助加速登陸區域和工作負載部署。

上面所列的範例可作為加速 TDD 週期的工具。 他們會在下列各節中的治理工具上執行，並允許雲端平臺小組建立自己的原始程式碼和測試。

## <a name="azure-governance-tools-that-can-accelerate-tdd-cycles"></a>可加快 TDD 週期的 Azure 治理工具

[Azure 原則](/azure/governance/policy)：部署或嘗試部署偏離治理原則時，Azure 原則可以提供自動化的偵測、保護和解決方式。 但 Azure 原則也提供在完成定義中測試驗收準則的主要機制。 在 TDD 迴圈中，您可以建立原則定義來測試單一接受準則。 同樣地，所有驗收準則都可以新增至指派給整個訂用帳戶的原則方案。 這種方法會在修改登陸區域之前，提供 red 測試的機制。 一旦登陸區域符合完成的定義之後，就可以使用它來強制執行測試準則，以避免在未來的版本中導致測試失敗的程式碼變更。

[Azure 藍圖](/azure/governance/blueprints)： Azure 藍圖會將原則和其他部署工具組合成可重複使用的套件，以指派給多個登陸區域。 當多個採用工作共用完成的一般定義時（您可能會想要隨著時間進行更新），藍圖證明很有用。 它也可以在後續努力擴充和重構登陸區域時協助進行部署。

[Azure Resource Graph](/azure/governance/resource-graph/overview)： Resource Graph 提供查詢語言，以根據登陸區域內部署的資產相關資訊來建立資料驅動的測試。 在後續的採用方案中，此工具也可以根據工作負載資產與基礎雲端環境之間的互動，來定義複雜的測試。

[Azure Resource Manager 範本](/azure/azure-resource-manager/templates/overview)：這些範本可為 Azure 中部署的任何環境提供主要原始程式碼。 某些協力廠商工具（例如 Terraform）會產生自己的 ARM 範本，然後提交給 Azure Resource Manager。

[Azure Resource Manager](/azure/azure-resource-manager/management/overview)： Resource Manager 為組建和部署功能提供一致的平臺。 這個平臺可以根據原始程式碼定義來部署登陸區域。

## <a name="next-steps"></a>後續步驟

若要開始重構您的第一個登陸區域，請評估 [基本登陸區域考慮](./basic-considerations.md)。

> [!div class="nextstepaction"]
> [基本登陸區域考量](./basic-considerations.md)
