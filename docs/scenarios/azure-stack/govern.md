---
title: 在您的資料中心管理 Azure 實例
description: 瞭解如何在您的資料中心內管理 Azure Stack Hub 上執行的 Azure 實例。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/19/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: internal
ms.openlocfilehash: 885278cb4ebf0faaf5686a16687bef7ccd7fe684
ms.sourcegitcommit: a0ddde4afcc7d8c21559e79d406dc439ee4f38d2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/21/2020
ms.locfileid: "97713805"
---
# <a name="govern-an-azure-instance-in-your-datacenter"></a>在您的資料中心管理 Azure 實例

跨公用和私用雲端平臺管理混合式解決方案，會增加複雜性。 因為您的 Azure Stack Hub 部署是您自己的資料中心內執行的 Azure 私人實例，所以此複雜性原本就會降低。

在雲端採用架構的控管 [方法](../../govern/index.md) 中所述的商務程式、專業領域和許多最佳作法，仍然可以套用至 Azure Stack Hub 的混合式治理。 適用于 Azure 公用雲端版本的許多雲端原生工具也可以在您的 Azure Stack Hub 部署中使用。

## <a name="azure-stack-hub-governance-considerations"></a>Azure Stack Hub 治理考慮

下列一系列的 blog 將說明您的組織如何針對 Azure Stack Hub 執行雲端治理概念：

- [組織服務](https://azure.microsoft.com/blog/azure-stack-iaas-part-seven/) （例如資源群組、azure 角色型存取控制 (azure RBAC) 、變更審核、鎖定和標記）。
- [安全性服務](https://azure.microsoft.com/blog/azure-stack-iaas-part-four/)，包括預設防火牆、限制、VM 更新和修補程式管理，以及惡意程式碼狀態。
- [DevOps 選項](https://azure.microsoft.com/blog/azure-stack-iaas-part-seven-2/)，包括基礎結構即程式碼、具有 PowerShell 和命令列介面的入口網站、Azure 應用程式見解，以及與 Azure DevOps 和 Jenkins 的整合。

## <a name="governance-toolchain-for-azure-stack-hub"></a>Azure Stack Hub 的治理工具鏈

如需將雲端原生治理工具套用至 Azure Stack Hub 環境的指引，請參閱：

- [Azure Resource Manager 範本和 Desired State Configuration](/azure-stack/user/azure-stack-arm-templates?view=azs-2002)
- [PowerShell](/azure-stack/user/azure-stack-powershell-overview?view=azs-2002)
- [Azure 原則](/azure-stack/user/azure-stack-policy-module?view=azs-2002)
- [Azure 角色型存取控制](/azure-stack/user/azure-stack-manage-permissions?view=azs-2002) \(部分機器翻譯\)

## <a name="next-steps"></a>後續步驟

如需雲端採用旅程圖的特定元素指引，請參閱：

- [管理 Azure Stack Hub](./manage.md)
