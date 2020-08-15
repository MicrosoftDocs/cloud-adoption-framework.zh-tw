---
title: 管理資料中心內的 Azure 實例
description: 瞭解如何在您的資料中心內管理在 Azure Stack Hub 上執行的 Azure 實例。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/19/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.openlocfilehash: ba4746cb146e2c742a265591ca2c7aa5e4724af6
ms.sourcegitcommit: 76edf563a08ff7dc81c3fc2dc6c8972ab3b4c55b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2020
ms.locfileid: "88237112"
---
# <a name="govern-an-azure-instance-in-your-datacenter"></a>管理資料中心內的 Azure 實例

管理跨公用和私用雲端平臺的混合式解決方案，會增加複雜度。 因為您的 Azure Stack 中樞是您在資料中心內執行的專屬 Azure 私人實例，所以該複雜度原本就會降低。

商務程式、專業領域，以及許多在雲端採用架構的治理 [方法](../../govern/index.md) 中所述的最佳作法，仍然可以運用在 Azure Stack 中樞的混合式管理。 Azure 公用雲端版本中使用的許多雲端原生工具也可以在您的 Azure Stack hub 中使用。

## <a name="azure-stack-hub-governance-considerations"></a>Azure Stack 中樞治理考慮

下列一系列的 blog 會顯示您的組織如何為 Azure Stack Hub 執行雲端治理概念：

- [組織服務](https://azure.microsoft.com/blog/azure-stack-iaas-part-seven/) ，例如資源群組、角色型存取控制 (RBAC) 、變更審核、鎖定和標記。
- [安全性服務](https://azure.microsoft.com/blog/azure-stack-iaas-part-four/)，包括預設防火牆、限制、VM 更新和修補程式管理，以及惡意程式碼狀態。
- [DevOps 選項](https://azure.microsoft.com/blog/azure-stack-iaas-part-seven-2/)，包括基礎結構即程式碼、使用 PowerShell 和命令列介面的入口網站、Azure 應用程式深入解析，以及與 Azure DevOps 和 Jenkins 整合。

## <a name="governance-toolchain-for-azure-stack-hub"></a>Azure Stack 中樞的治理工具鏈

如需將雲端原生治理工具套用至 Azure Stack 中樞環境的指引，請參閱：

- [Azure Resource Manager 範本和 Desired State Configuration](https://docs.microsoft.com/azure-stack/user/azure-stack-arm-templates?view=azs-2002)
- [PowerShell](https://docs.microsoft.com/azure-stack/user/azure-stack-powershell-overview?view=azs-2002)
- [Azure 原則](https://docs.microsoft.com/azure-stack/user/azure-stack-policy-module?view=azs-2002)
- [角色型存取控制](https://docs.microsoft.com/azure-stack/user/azure-stack-manage-permissions?view=azs-2002)

## <a name="next-steps"></a>後續步驟

如需雲端採用旅程的特定元素指引，請參閱：

- [管理 Azure Stack Hub](./manage.md)
