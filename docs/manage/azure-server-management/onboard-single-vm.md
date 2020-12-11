---
title: 啟用 VM 上的伺服器管理服務
description: 使用適用于 Azure 的雲端採用架構，瞭解如何在單一 VM 上啟用 Azure 伺服器管理服務。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: internal
ms.openlocfilehash: 7b39a03b54c2ebd99db4fc6dd508e914620f4257
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97016964"
---
# <a name="enable-server-management-services-on-a-single-vm-for-evaluation"></a>在單一 VM 上啟用伺服器管理服務以進行評估

瞭解如何在單一 VM 上啟用伺服器管理服務以進行評估。

> [!NOTE]
> 在 VM 上執行 Azure 管理服務之前，請先建立必要的 [Log Analytics 工作區和 Azure 自動化帳戶](./prerequisites.md#create-a-workspace-and-automation-account) 。

您可以輕鬆地將 Azure 伺服器管理服務上架到 Azure 入口網站中的個別虛擬機器。 您可以先熟悉這些服務，再將它們上架。 當您選取 VM 實例時， [管理工具和服務](./tools-services.md) 清單上的所有解決方案都會出現在 [ **操作** ] 或 [ **監視** ] 功能表上。 您可以選取方案，並依照嚮導將其上架。

![Azure 入口網站中虛擬機器設定的螢幕擷取畫面](./media/onboarding-single-vm.png)

## <a name="related-resources"></a>相關資源

如需有關如何將這些解決方案上架到個別 Vm 的詳細資訊，請參閱：

- [為 Azure 中的 VM 上架更新管理解決方案和變更追蹤和清查解決方案](/azure/automation/change-tracking/manage-inventory-vms)
- [將適用於 VM 的 Azure 監視器上線](/azure/azure-monitor/insights/vminsights-enable-single-vm)

## <a name="next-steps"></a>後續步驟

瞭解如何使用 Azure 原則來大規模上架 Azure Vm。

> [!div class="nextstepaction"]
> [設定訂用帳戶的 Azure 管理服務](./onboard-at-scale.md)
