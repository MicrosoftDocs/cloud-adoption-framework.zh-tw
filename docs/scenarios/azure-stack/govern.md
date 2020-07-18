---
title: 管理您資料中心內執行的 Azure
description: 瞭解如何在您的資料中心內，管理在 Azure Stack Hub 上執行的 Azure。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/19/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.openlocfilehash: c9f8a2f161571ced20f3a9317469cee14cc35fb1
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86451723"
---
# <a name="govern-azure-running-in-your-datacenter"></a>管理您資料中心內執行的 Azure

管理跨公用和私用雲端平臺的混合式解決方案，會增加複雜度。 因為 Azure Stack Hub 是您在資料中心內執行的專屬 Azure 私人實例，所以該複雜度原本就會降低。

商務程式、專業領域，以及許多在雲端採用架構的治理[方法](../../govern/index.md)中所述的最佳作法，仍然可以運用在 Azure Stack 中樞的混合式管理。 Azure 公用雲端版本中使用的許多雲端原生工具也可以在 Azure Stack Hub 中使用。

## <a name="azure-stack-hub-governance-considerations"></a>Azure Stack 中樞治理考慮

下列一系列的 blog 會示範如何為 Azure Stack Hub 實行雲端治理概念：

- [組織服務](https://azure.microsoft.com/blog/azure-stack-iaas-part-seven/)，例如資源群組、角色型存取控制（RBAC）、變更審核、鎖定和標記。
- [安全性服務](https://azure.microsoft.com/blog/azure-stack-iaas-part-four/)，包括預設防火牆、限制、VM 更新和修補程式管理，以及惡意程式碼狀態。
- [DevOps 選項](https://azure.microsoft.com/blog/azure-stack-iaas-part-seven-2/)，包括基礎結構即程式碼（IaC）、使用 PowerShell 和命令列介面（CLI）的入口網站、Azure 應用程式深入解析，以及與 Azure DevOps 和 Jenkins 的整合。

## <a name="governance-toolchain-for-azure-stack-hub"></a>Azure Stack 中樞的治理工具鏈

如需將雲端原生治理工具套用至 Azure Stack 中樞環境的指引，請參閱下列連結：

- [Azure Resource Manager 範本和 Desired State Configuration](https://docs.microsoft.com/azure-stack/user/azure-stack-arm-templates?view=azs-2002)
- [PowerShell](https://docs.microsoft.com/azure-stack/user/azure-stack-powershell-overview?view=azs-2002)
- [Azure 原則](https://docs.microsoft.com/azure-stack/user/azure-stack-policy-module?view=azs-2002)
- [角色型存取控制 (RBAC)](https://docs.microsoft.com/azure-stack/user/azure-stack-manage-permissions?view=azs-2002)

## <a name="next-step-integrate-this-strategy-into-your-cloud-adoption-journey"></a>下一步：將此策略整合到您的雲端採用旅程圖

下列文章提供在雲端採用旅程圖的特定時間點中找到的指引。

- [管理 Azure Stack Hub](./manage.md)
