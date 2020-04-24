---
title: 將 Azure 伺服器管理服務上架
description: 使用 Azure 虛擬機器和內部部署伺服器的資訊將 Azure 伺服器管理服務上架。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: f83678c1a2b3387155b9fa908c54e81670daf8ae
ms.sourcegitcommit: 7d3fc1e407cd18c4fc7c4964a77885907a9b85c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "80426512"
---
# <a name="phase-2-onboarding-azure-server-management-services"></a>第2階段：上架 Azure 伺服器管理服務

在您熟悉 Azure 管理服務所涉及的[工具](./tools-services.md)和[規劃](./prerequisites.md)之後，您就可以開始進行第二個階段。 第2階段提供將這些服務上線以與您的 Azure 資源搭配使用的逐步指引。 一開始先評估這項上執行緒序，然後在您的環境中廣泛採用。

> [!NOTE]
> 本指南後續章節中所討論的自動化方法，適用于尚未將伺服器部署至雲端的部署。 他們要求您必須擁有訂用帳戶的「擁有者」角色，才能建立所有必要的資源和原則。 如果您已經建立 Log Analytics 工作區和自動化帳戶，建議您在啟動範例自動化腳本時，將這些資源傳入適當的參數。

## <a name="onboarding-processes"></a>上架程式

本指南的這一節涵蓋下列適用于 Azure 虛擬機器和內部部署伺服器的上架程式：

- **使用入口網站，在單一 VM 上啟用管理服務以進行評估**。 使用此程式來熟悉 Azure 伺服器管理服務。
- **使用入口網站設定訂**用帳戶的管理服務。 此程式可協助您設定 Azure 環境，讓布建的任何新 Vm 都會自動使用管理服務。 如果您偏好腳本和命令列的 Azure 入口網站體驗，請使用此方法。
- **使用 Azure 自動化設定訂**用帳戶的管理服務。 此程式已完全自動化。 只要建立訂用帳戶，腳本就會將環境設定為針對任何新布建的 VM 使用管理服務。 如果您熟悉 PowerShell 腳本和 Azure Resource Manager 範本，或想要學習如何使用，請使用此方法。

這兩種方法的程式各不相同。

> [!NOTE]
> 當您使用 Azure 入口網站時，上架步驟的順序會與自動上架步驟不同。 入口網站提供更簡單的上線體驗。

下圖顯示管理服務的建議部署模型：

![建議部署模型的圖表](./media/recommended-deployment.png)

如上圖所示，Log Analytics 代理程式同時具有內部部署伺服器的*自動註冊*和*加入宣告*設定：

- **自動註冊：** 當 Log Analytics 代理程式安裝在伺服器上並設定為連接到工作區時，在該工作區上啟用的解決方案會自動套用至伺服器。
- **加入宣告：** 即使代理程式已安裝且已連線至工作區，也不會套用解決方案，除非它已新增至工作區中的伺服器範圍設定。

## <a name="next-steps"></a>後續步驟

瞭解如何使用入口網站來上架單一 VM，以評估上執行緒序。

> [!div class="nextstepaction"]
> [將單一 Azure VM 上架以進行評估](./onboard-single-vm.md)
