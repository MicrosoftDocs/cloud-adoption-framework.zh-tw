---
title: 在 Azure 中執行企業級登陸區域
description: 審查用來執行企業級架構的選項
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 83c95275b124e6a2fe5787ae3f33d6566c0303e9
ms.sourcegitcommit: 568037e0d2996e4644c11eb61f96362a402759ec
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/16/2020
ms.locfileid: "84800056"
---
# <a name="implement-enterprise-scale-landing-zones-in-azure"></a>在 Azure 中執行企業級登陸區域

當商務需求需要從一開始就具備完全整合的管理、安全性和作業來進行登陸區域的豐富初始執行時，Microsoft 建議使用此頁面上的企業規模範例選項。 這種方法可以使用 Microsoft Azure 入口網站或基礎結構即程式碼，來設定您的環境。 當您的組織準備就緒時，您也可以在入口網站和基礎結構即程式碼之間轉換（建議使用）。 如同任何其他 Microsoft Azure 基礎結構即程式碼的方法，您將需要 Azure Resource Manager 範本和 GitHub 技能。

## <a name="example-implementation"></a>範例執行

下表列出範例模組化的執行。

| 部署範例  | Description  | GitHub 存放庫 | 部署至 Azure |
|---------|---------|---------|---------|---------|---------|---------|---------|
| 企業規模基礎 | 這是適用于企業規模採用的建議基礎。 | [GitHub 中的範例](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/wingtip/README.md) | [將範例部署至 Azure](https://ms.portal.azure.com/?feature.customportal=false#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAzOps%2Fmain%2Ftemplate%2Fux-foundation.json) |
| 企業規模的虛擬 WAN | 將虛擬 WAN 網路模組新增至企業規模的基礎。 | [GitHub 中的範例](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/contoso/Readme.md) | [將範例部署至 Azure](https://ms.portal.azure.com/?feature.customportal=false#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAzOps%2Fmain%2Ftemplate%2Fux-vwan.json) |
| 企業規模中樞和輪輻 | 將中樞和輪輻網路模組新增至企業規模的基礎。 | [GitHub 中的範例](https://github.com/Azure/Enterprise-Scale/blob/main/docs/reference/adventureworks/README.md) | <!-- [Deploy example to Azure](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fkrnese%2FAzureDeploy%2Fmaint%2FARM%2Fdeployments%2Fe2e.json) --> 敬請期待 |

## <a name="next-steps"></a>後續步驟

上述範例提供簡單的部署選項，以支援持續學習企業規模的方法。 在企業規模的生產版本中使用這些範例之前，您應該先複習[企業規模的架構](./architecture.md)。
