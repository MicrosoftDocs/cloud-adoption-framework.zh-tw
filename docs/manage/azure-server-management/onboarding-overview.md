---
title: 上架 Azure 伺服器管理服務
description: 將 Azure 伺服器管理服務與 Azure 和內部部署伺服器中的虛擬機器相關資訊上架。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: internal
ms.openlocfilehash: 1d4b157ec25d6324d58a0ca2e725ece84cf0bab0
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97016947"
---
# <a name="phase-2-onboarding-azure-server-management-services"></a>第2階段：上架 Azure 伺服器管理服務

在您熟悉 Azure 管理服務的相關 [工具](./tools-services.md) 和 [規劃](./prerequisites.md) 之後，您就可以開始進行第二階段。 第2階段提供逐步指導方針，讓您將這些服務上線以搭配您的 Azure 資源使用。 先評估此上執行緒序，再將它廣泛地在您的環境中採用。

> [!NOTE]
> 本指南的後續章節中所討論的自動化方法，適用于尚未將伺服器部署至雲端的部署。 他們要求您必須擁有訂用帳戶的「擁有者」角色，才能建立所有必要的資源和原則。 如果您已經建立 Log Analytics 工作區和自動化帳戶，我們建議您在啟動範例自動化腳本時，在適當的參數中傳遞這些資源。

## <a name="onboarding-processes"></a>上架程式

本指南的這一節涵蓋了 Azure 和內部部署伺服器中兩部虛擬機器的下列上架程式：

- **使用入口網站在單一 VM 上啟用管理服務以進行評估。** 您可以使用此程式來熟悉 Azure 伺服器管理服務。
- **使用入口網站設定訂用帳戶的管理服務。** 此程式可協助您設定 Azure 環境，以便任何布建的新 Vm 都會自動使用管理服務。 如果您想要 Azure 入口網站的腳本和命令列體驗，請使用此方法。
- **使用 Azure 自動化設定訂用帳戶的管理服務。** 此程式完全自動化。 只要建立訂用帳戶，腳本就會將環境設定為針對任何新布建的 VM 使用管理服務。 如果您熟悉 PowerShell 腳本和 Azure Resource Manager 範本，或者您想要瞭解如何使用這些範本，請使用此方法。

這兩種方法的程式不同。

> [!NOTE]
> 當您使用 Azure 入口網站時，上線步驟的順序與自動上線步驟不同。 入口網站提供更簡單的上線體驗。

下圖顯示管理服務的建議部署模型：

![建議部署模型的圖表](./media/recommended-deployment.png)

如上圖所示，Log Analytics 代理程式有兩個適用于內部部署伺服器的設定：

- **自動註冊：** 在伺服器上安裝 Log Analytics 代理程式，並設定為連線至工作區時，會自動將該工作區上所啟用的解決方案套用至伺服器。
- **加入宣告：** 即使已安裝代理程式並聯機至工作區，仍不會套用解決方案，除非將其新增至工作區中的伺服器範圍設定。

## <a name="next-steps"></a>後續步驟

瞭解如何使用入口網站來上架單一 VM，以評估上執行緒序。

> [!div class="nextstepaction"]
> [將單一 Azure VM 上架以進行評估](./onboard-single-vm.md)
