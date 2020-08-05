---
title: 在 Azure 中執行 CAF 企業級登陸區域
description: 請參閱可執行 CAF 企業級架構的選項。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 164a52dc11673e454ff7ad63fc7c4ddc4617395d
ms.sourcegitcommit: 9662234674e663bc7d4bc134d303520cb146bd95
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/04/2020
ms.locfileid: "87560435"
---
# <a name="implement-caf-enterprise-scale-landing-zones-in-azure"></a>在 Azure 中執行 CAF 企業級登陸區域

當商務需求需要從一開始就具備完全整合的管理、安全性和作業來進行登陸區域的豐富初始執行時，請使用此處所列的企業規模範例選項。 使用此方法，您可以使用 Microsoft Azure 入口網站或基礎結構即程式碼來設定您的環境。 當您的組織準備就緒時，您也可以在入口網站和基礎結構之間轉換為程式碼 (建議) 。

## <a name="example-implementation"></a>範例執行

下表列出範例模組化的執行。

| 部署範例  | 描述  | GitHub 存放庫 | 部署至 Azure |
|---------|---------|---------|---------|---------|---------|---------|---------|
| 企業規模基礎 | 這是適用于企業規模採用的建議基礎。 | [GitHub 中的範例](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/wingtip/README.md) | [將範例部署至 Azure](https://ms.portal.azure.com/?feature.customportal=false#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FEnterprise-Scale%2Fmain%2Fdocs%2Freference%2Fwingtip%2FarmTemplates%2Fes-foundation.json/createUIDefinitionUri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FEnterprise-Scale%2Fmain%2Fdocs%2Freference%2Fwingtip%2FarmTemplates%2Fportal-es-foundation.json) |
| 企業規模的虛擬 WAN | 將虛擬 WAN 網路模組新增至企業規模的基礎。 | [GitHub 中的範例](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/contoso/Readme.md) | [將範例部署至 Azure](https://ms.portal.azure.com/?feature.customportal=false#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FEnterprise-Scale%2Fmain%2Fdocs%2Freference%2Fcontoso%2FarmTemplates%2Fes-vwan.json/createUIDefinitionUri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FEnterprise-Scale%2Fmain%2Fdocs%2Freference%2Fcontoso%2FarmTemplates%2Fportal-es-vwan.json) |
| 企業規模中樞和輪輻 | 將中樞和輪輻網路模組新增至企業規模的基礎。 | [GitHub 中的範例](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/adventureworks/README.md) | <!-- [Deploy example to Azure](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fkrnese%2FAzureDeploy%2Fmaint%2FARM%2Fdeployments%2Fe2e.json) --> 即將推出 |

## <a name="next-steps"></a>後續步驟

這些範例提供簡單的部署選項，以支援持續學習企業規模的方法。 在企業規模的生產版本中使用這些範例之前，您應該先複習[企業規模的架構](./architecture.md)。
