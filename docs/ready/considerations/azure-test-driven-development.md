---
title: Azure 中登陸區域的測試導向開發
description: Azure 中登陸區域的測試導向開發。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/15/2020
ms.topic: overview
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 8d44734998d3a59391d9fcceb8f39704b845bafc
ms.sourcegitcommit: 71a4f33546443d8c875265ac8fbaf3ab24ae8ab4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2020
ms.locfileid: "86479784"
---
# <a name="test-driven-development-for-landing-zones-in-azure"></a>Azure 中登陸區域的測試導向開發

如先前[針對登陸區域的測試導向開發（TDD）](./test-driven-development.md)一文所述，TDD 迴圈一開始會先進行測試，以驗證傳遞雲端採用方案所需的特定功能驗收準則。 接著，您可以進行擴充或重構登陸區域，以驗證是否符合驗收準則。 本文概述 Azure 中的雲端原生工具鏈，以自動化測試導向的開發週期。

## <a name="azure-tools-to-support-landing-zone-tdd-cycles"></a>支援登陸區域 TDD 週期的 Azure 工具

![Azure 中的測試驅動開發工具 ](../../_images/ready/azure-tdd-tools.png)
 _圖1： azure 中的測試驅動開發工具。_

Azure 原生治理產品和服務的工具鏈，可以輕鬆整合到以測試為導向的開發，以建立登陸區域。 這些工具都能提供特定用途，讓您更輕鬆地開發、測試和部署登陸區域，以配合 TDD 週期。

## <a name="microsoft-provided-test-and-deployment-templates-to-accelerate-tdd"></a>Microsoft 提供的測試和部署範本，以加速 TDD

下列範例是由 Microsoft 提供，供治理之用。 每個都可以在登陸區域的測試導向開發週期中，用來做為測試或一系列測試。 下列各節提供有關每個工具的詳細資訊：

- Azure 藍圖提供各種[藍圖範例](https://docs.microsoft.com/azure/governance/blueprints/samples)，包括用於測試的原則和部署的範本。 這些藍圖範例可以加速在 TDD 週期中進行開發、部署和測試工作。
- Azure 原則也包含[內建的原則計畫](https://docs.microsoft.com/azure/governance/policy/samples/built-in-initiatives)，可用來測試並強制執行登陸區域的完整定義。 Azure 原則包含內[建的原則定義](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies)，可在完成的定義中符合個別接受準則。
- Azure Graph 包含先進的[查詢範例](https://docs.microsoft.com/azure/governance/resource-graph/samples/advanced)，可以用來瞭解如何在登陸區域內部署工作負載，以進行先進的測試案例。
- [Azure 快速入門範本](https://azure.microsoft.com/resources/templates)提供原始程式碼範本，協助加速登陸區域和工作負載部署。

上列範例可用來做為加速 TDD 週期的工具。 它們會在下列各節的治理工具上執行，並允許雲端平臺小組建立自己的原始程式碼和測試。

## <a name="azure-governance-tools-that-can-accelerate-tdd-cycles"></a>可加速 TDD 週期的 Azure 治理工具

[Azure 原則](https://docs.microsoft.com/azure/governance/policy)：當部署或嘗試部署偏離治理原則時，Azure 原則可以提供自動化的偵測、保護和解決方式。 但是 Azure 原則也會提供在您的完成定義中測試接受準則的主要機制。 在 TDD 迴圈中，可以建立原則定義來測試單一接受準則。 同樣地，所有驗收準則都可以新增至指派給整個訂用帳戶的原則計畫。 這種方法會在修改登陸區域之前，提供紅色測試的機制。 一旦登陸區域符合「完成」的定義，就可以使用它來強制測試準則，以避免程式碼變更，而導致測試在未來的版本中失敗。

[Azure 藍圖](https://docs.microsoft.com/azure/governance/blueprints)： Azure 藍圖會將原則和其他部署工具分組到可重複使用的套件中，而此封裝可以指派給多個登陸區域。 當多個採用工作共用完成的一般定義（您可能想要隨著時間更新）時，藍圖會非常有用。 它也可以在後續進行擴充和重構登陸區域的作業時協助進行部署。

[Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview)： Resource Graph 提供查詢語言，以根據登陸區域內所部署資產的相關資訊來建立資料驅動型測試。 稍後在採用計畫中，此工具也可以根據工作負載資產與基礎雲端環境之間的互動，定義複雜的測試。

[Azure Resource Manager 範本](https://docs.microsoft.com/azure/azure-resource-manager/templates/overview)：這些範本會為 Azure 中部署的任何環境提供主要原始程式碼。 有些協力廠商工具（例如 Terraform）會產生自己的 ARM 範本，然後提交給 Azure Resource Manager。

[Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/management/overview)： Resource Manager 為組建和部署功能提供一致的平臺。 此平臺可以根據原始程式碼定義來部署登陸區域。

## <a name="next-steps"></a>後續步驟

若要開始重構您的第一個登陸區域，請評估[基本登陸區域考慮](./basic-considerations.md)。

> [!div class="nextstepaction"]
> [基本登陸區域考量](./basic-considerations.md)
