---
title: 在 Azure 中實施雲端採用架構企業級登陸區域
description: 查看為 Azure 企業規模架構實施雲端採用架構的選項。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: think-tank
ms.openlocfilehash: ea900edf52de19b8340f43db1a48218780969652
ms.sourcegitcommit: d957bfc1fa8dc81168ce9c7d801a8dca6254c6eb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/23/2020
ms.locfileid: "95447280"
---
# <a name="implement-cloud-adoption-framework-enterprise-scale-landing-zones-in-azure"></a>在 Azure 中實施雲端採用架構企業級登陸區域

當商務需求需要從一開始就完全整合的治理、安全性及營運，進行登陸區域的豐富初始實行時，請使用此處所列的企業規模範例選項。 使用這個方法時，您可以使用 Azure 入口網站或基礎結構即程式碼來設定和設定您的環境。 您也可以在您的組織準備好時，在入口網站與基礎結構即程式碼 (建議的) 之間進行轉換。

## <a name="example-implementation"></a>範例執行

下表列出範例模組化執行。

| 部署範例  | 描述  | GitHub 存放庫 | 部署至 Azure |
|---------|---------|---------|---------|---------|---------|---------|---------|
| 企業規模的基礎 | 這是適用于企業規模採用的建議基礎。 | [GitHub 中的範例](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/wingtip/README.md) | [將範例部署至 Azure](https://portal.azure.com/#blade/Microsoft_Azure_CreateUIDef/CustomDeploymentBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FEnterprise-Scale%2Fmain%2Fdocs%2Freference%2Fwingtip%2FarmTemplates%2Fes-foundation.json/createUIDefinitionUri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FEnterprise-Scale%2Fmain%2Fdocs%2Freference%2Fwingtip%2FarmTemplates%2Fportal-es-foundation.json) |
| 企業規模的虛擬 WAN | 將虛擬 WAN 網路模組新增至企業規模的基礎。 | [GitHub 中的範例](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/contoso/Readme.md) | [將範例部署至 Azure](https://portal.azure.com/#blade/Microsoft_Azure_CreateUIDef/CustomDeploymentBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FEnterprise-Scale%2Fmain%2Fdocs%2Freference%2Fcontoso%2FarmTemplates%2Fes-vwan.json/createUIDefinitionUri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FEnterprise-Scale%2Fmain%2Fdocs%2Freference%2Fcontoso%2FarmTemplates%2Fportal-es-vwan.json) |
| 企業規模的中樞和輪輻 | 將中樞和輪輻網路模組新增至企業規模的基礎。 | [GitHub 中的範例](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/adventureworks/README.md) |[將範例部署至 Azure](https://portal.azure.com/#blade/Microsoft_Azure_CreateUIDef/CustomDeploymentBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FEnterprise-Scale%2Fmain%2Fdocs%2Freference%2Fadventureworks%2FarmTemplates%2Fes-hubspoke.json/createUIDefinitionUri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FEnterprise-Scale%2Fmain%2Fdocs%2Freference%2Fadventureworks%2FarmTemplates%2Fportal-es-hubspoke.json) |

## <a name="next-steps"></a>後續步驟

這些範例提供簡單的部署選項，以支援持續學習企業規模的方法。 在企業規模的生產版本中使用這些範例之前，請先審視 [企業規模的架構](./architecture.md)。
