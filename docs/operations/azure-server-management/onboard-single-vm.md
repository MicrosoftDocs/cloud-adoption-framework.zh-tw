---
title: 在單一 VM 上啟用管理服務以進行評估
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 在單一 VM 上啟用管理服務以進行評估
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 4ebf0bf17fafcd495414d32fe04882c36634d4e2
ms.sourcegitcommit: a26c27ed72ac89198231ec4b11917a20d03bd222
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70825233"
---
# <a name="enable-management-services-on-a-single-vm-for-evaluation"></a>在單一 VM 上啟用管理服務以進行評估

瞭解如何在單一 VM 上啟用管理服務以進行評估。

> [!NOTE]
> 將虛擬機器上架至 Azure 管理服務之前, 請先建立必要的[Log Analytics 工作區和 Azure 自動化帳戶](./prerequisites.md#create-a-workspace-and-automation-account)。

在 Azure 入口網站中, 將個別虛擬機器 (Vm) 上架至 Azure 伺服器管理服務是很簡單的。 入口網站可讓您先熟悉這些服務, 再將其上線至您的 Vm。 當您選取 VM 實例時,[管理工具和服務](./tools-services.md)清單中所討論的所有解決方案都會出現在 [**作業**] 功能表或 [**監視**] 功能表底下。 您可以選取每個解決方案, 然後依照嚮導的指示來上架。

![Azure 入口網站中虛擬機器設定的螢幕擷取畫面](./media/onboarding-single-vm.png)

## <a name="related-resources"></a>相關資源

如需將個別 Vm 上架到每個解決方案的詳細資訊, 請參閱:

- [從 Azure 虛擬機器上架更新管理、變更追蹤和清查解決方案](/azure/automation/automation-onboard-solutions-from-vm)
- [將適用于 VM 的 Azure 監視器上線](/azure/azure-monitor/insights/vminsights-enable-single-vm)

## <a name="next-steps"></a>後續步驟

瞭解如何使用 Azure 原則來大規模上架 Azure Vm。

> [!div class="nextstepaction"]
> [設定訂用帳戶的 Azure 管理服務](./onboard-at-scale.md)
