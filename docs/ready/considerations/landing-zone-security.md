---
title: 改善登陸區域安全性
description: 改進登陸區域安全性。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/15/2020
ms.topic: overview
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: 4c3def16dacdcc24459547e5acfe4f4d81cc73a5
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88566861"
---
<!-- cSpell:ignore SIEM -->

# <a name="improve-landing-zone-security"></a>改善登陸區域安全性

當裝載它的工作負載或登陸區域需要存取任何敏感性資料或重要系統時，保護資料和資產是很重要的。 藉由展開或重構登陸區域以考慮提高安全性需求，來改善登陸區域安全性的基礎， [以測試導向的開發方式來登陸區域](./test-driven-development.md) 。

## <a name="landing-zone-security-best-practices"></a>登陸區域安全性最佳作法

下列參考架構和最佳作法清單提供改進登陸安全性的方法範例：

- [Azure 資訊安全中心](/azure/security-center/security-center-get-started?bc=%2fazure%2fcloud-adoption-framework%2f_bread%2ftoc.json&toc=%2fazure%2fcloud-adoption-framework%2ftoc.json)：將訂用帳戶上架到「安全性中心」。
- [Azure Sentinel](/azure/sentinel/quickstart-onboard?bc=%2fazure%2fcloud-adoption-framework%2f_bread%2ftoc.json&toc=%2fazure%2fcloud-adoption-framework%2ftoc.json)：將 Azure Sentinel 上線，以提供 **安全性資訊事件管理 (SIEM) ** 和 **安全性協調流程自動化回應 (SOAR) ** 解決方案。
- [網路界限安全性](../../reference/networking-vdc.md)：用於開發網路的數個參考模式，類似于在資料中心內保護網路界限的方式。
- [安全網路架構](/azure/architecture/reference-architectures/dmz/secure-vnet-dmz?bc=%2fazure%2fcloud-adoption-framework%2f_bread%2ftoc.json&toc=%2fazure%2fcloud-adoption-framework%2ftoc.json)：用於執行周邊網路和安全網路架構的參考架構。
- 身分[識別管理和存取控制](/azure/security/fundamentals/identity-management-best-practices?bc=%2fazure%2fcloud-adoption-framework%2f_bread%2ftoc.json&toc=%2fazure%2fcloud-adoption-framework%2ftoc.json)：在 Azure 中執行身分識別和存取安全登陸區域的最佳作法系列。
- [網路安全性作法](/azure/security/fundamentals/network-best-practices?bc=%2fazure%2fcloud-adoption-framework%2f_bread%2ftoc.json&toc=%2fazure%2fcloud-adoption-framework%2ftoc.json)：提供保護網路的其他最佳做法。
- [營運安全性](/azure/security/fundamentals/operational-best-practices?bc=%2fazure%2fcloud-adoption-framework%2f_bread%2ftoc.json&toc=%2fazure%2fcloud-adoption-framework%2ftoc.json) 可提供在 Azure 中提高營運安全性的最佳作法。
- [安全性基準專業領域](../../govern/guides/complex/security-baseline-improvement.md#incremental-improvement-of-the-best-practices)：開發治理導向安全性基準以強制執行安全性需求的範例。

## <a name="test-driven-development-cycle"></a>測試導向的開發週期

在開始進行任何安全性改進之前，請務必先瞭解「完成定義」和所有「接受準則」。 如需詳細資訊，請參閱在 Azure 中對登陸區域和[測試導向開發](./azure-test-driven-development.md)進行[測試導向開發的相關](./test-driven-development.md)文章。

![雲端登陸區域的測試導向開發流程](../../_images/ready/test-driven-development-process.png)

## <a name="next-steps"></a>後續步驟

瞭解如何 [改善登陸區域作業](./landing-zone-operations.md) 以支援重要的應用程式。

> [!div class="nextstepaction"]
> [改善登陸區域作業](./landing-zone-operations.md)
