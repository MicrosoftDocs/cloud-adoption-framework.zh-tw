---
title: 改善登陸區域安全性
description: 改善登陸區域安全性
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2020
ms.topic: overview
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: be340d1566fce58012cd63c6efc90fd2e0636260
ms.sourcegitcommit: 7d3fc1e407cd18c4fc7c4964a77885907a9b85c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "81121710"
---
<!-- cSpell:ignore SIEM -->

# <a name="improve-landing-zone-security"></a>改善登陸區域安全性

當工作負載或其主控的登陸區域需要存取任何敏感性資料或重要的系統時，請務必保護資料和資產。 藉由展開或重整登陸區域來考慮提升的安全性需求，改善登陸區域安全性是[以測試導向的開發方法為基礎來進行登陸](./test-driven-development.md)區域。

## <a name="landing-zone-security-best-practices"></a>登陸區域安全性最佳作法

下列參考架構和最佳作法清單提供了改善登陸安全性的方式範例：

- [Azure 資訊安全中心](https://docs.microsoft.com/azure/security-center/security-center-get-started?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)：將訂用帳戶上架至資訊安全中心。
- [Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)：上架 Azure Sentinel 以提供**安全性資訊事件管理（SIEM）** 和**安全性協調流程自動化回應（攀升情況）** 解決方案。
- [網路界限安全性](../../reference/networking-vdc.md)：用於開發網路的數個參考模式，類似于資料中心內的網路界限如何保護。
- [安全網路架構](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-dmz?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)：用來執行非軍事區域和安全網路架構的參考架構。
- 身分[識別管理和存取控制](https://docs.microsoft.com/azure/security/fundamentals/identity-management-best-practices?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)：一系列用來執行身分識別和存取權的最佳作法，以保護 Azure 中的登陸區域。
- [網路安全性做法](https://docs.microsoft.com/azure/security/fundamentals/network-best-practices?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)：提供保護網路的其他最佳作法。
- 作業[安全性](https://docs.microsoft.com/azure/security/fundamentals/operational-best-practices?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)提供在 Azure 中增加營運安全性的最佳作法。
- [安全性基準](../../govern/guides/complex/security-baseline-improvement.md#incremental-improvement-of-the-best-practices)：開發治理導向的安全性基準來強制執行安全性需求的範例。

## <a name="test-driven-development-cycle"></a>以測試為導向的開發週期

開始任何安全性改進之前，請務必瞭解「完成定義」和所有「接受條件」。 如需詳細資訊，請參閱在 azure 中進行登陸區域和[測試導向開發](./azure-test-driven-development.md)的[測試導向開發](./test-driven-development.md)文章。

![雲端登陸區域的測試導向開發流程](../../_images/ready/test-driven-development-process.png)

## <a name="next-steps"></a>後續步驟

瞭解如何[改善登陸區域作業](./landing-zone-operations.md)，以支援重要的應用程式。

> [!div class="nextstepaction"]
> [改善登陸區域作業](./landing-zone-operations.md)
