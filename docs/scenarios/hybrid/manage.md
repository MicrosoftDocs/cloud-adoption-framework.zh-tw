---
title: 跨混合式和多重雲端作業管理您的組合
description: 利用 Azure 的企業控制平面，執行有效的控制項，以提供跨混合式和多重雲端部署的作業管理。
author: brianblanchard
ms.author: brblanch
ms.date: 02/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: e2e-hybrid
ms.openlocfilehash: bc9912d7226b204e34c6944585ab3b2a73c4ff12
ms.sourcegitcommit: c167c45b66cc7324b60c88b8b7aac439f956b65d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2021
ms.locfileid: "102208518"
---
# <a name="manage-your-portfolio-across-hybrid-and-multicloud-operations"></a>跨混合式和多重雲端作業管理您的組合

使用混合式和多重雲端環境，可在雲端中操作的方式中產生自然的變化。 適用于 Azure 的雲端採用架構中的 [管理方法](../../manage/index.md) 會概述在整個雲端採用生命週期中執行營運基準和開發該基準的途徑。 擴充您的策略以納入混合式、多重雲端和邊緣部署，需要在您執行適當作業管理的方式中轉移。 [整合作業](./unified-operations.md) 是解決這些移位需求的最佳概念。

下一節將概述如何套用整合作業的概念，並實施最佳做法，以確保能有效地進行混合、多重雲端和邊緣作業。

## <a name="extend-your-operations-baseline"></a>擴充您的作業基準

[Azure Arc](/azure/azure-arc/overview) 可降低擴充營運基準的複雜度和成本。 在您的資料中心、混合式雲端和多重雲端環境中部署 Azure Arc，可延伸 Azure Resource Manager 中包含的 Azure 原生功能。

若要開始使用橫跨內部部署和多個雲端提供者的作業基準，請完成清查和標記練習。 此練習將開始在幾個步驟中擴充您的作業基準：

- 將的標記新增 `hosting platform` 至所有混合式、多重雲端和邊緣資產。
- 從 AWS、GCP 等標記資源。
- 查詢您的資源以探索其託管位置。

若要開始使用，請 [清查並標記您的混合式和多重雲端資源](../../manage/hybrid/server/best-practices/arc-inventory-tagging.md)。

<!-- docutune:casing "update management guide" -->

完成練習之後，您就可以開始操作混合式和多重雲端環境。 一般來說，您在跨雲端擴充作業時所採取的第一步，就是為 *修補和更新管理建立一致的計畫*。 若要部署可跨雲端提供者控制修補的工具，請依照「 [混合式和多重雲端更新管理指南](../../manage/hybrid/server/best-practices/arc-update-management.md)」中的步驟進行。

## <a name="enhanced-baseline"></a>增強的基準

藉由提供持續更廣泛的資產和雲端提供者，加強您的營運基準。 下列清單提供一些範例，說明您可以新增至展開的作業基準的資產類型：

- **Azure 資產**： [Linux Vm](../../manage/hybrid/server/best-practices/arm-template-linux.md) 和 [Windows vm](../../manage/hybrid/server/best-practices/arm-template-windows.md)
- **您本機資料中心的資產**： [Linux Vm](../../manage/hybrid/server/best-practices/onboard-server-linux.md) 和 [Windows vm](../../manage/hybrid/server/best-practices/onboard-server-windows.md)
- **VMware 資產**： [Linux Vm](../../manage/hybrid/server/best-practices/vmware-scaled-powercli-linux.md) 和 [Windows vm](../../manage/hybrid/server/best-practices/vmware-scaled-powercli-windows.md)
- **AWS 資產**：搭配使用 Terraform 和 [AWS Ubuntu 和 Terraform 的](../../manage/hybrid/server/best-practices/aws-terraform-ubuntu.md) [Linux vm](../../manage/hybrid/server/best-practices/aws-terraform-al2.md)
- **GCP 資產**： [Ubuntu Vm](../../manage/hybrid/server/best-practices/gcp-terraform-ubuntu.md) 和 [Windows vm](../../manage/hybrid/server/best-practices/gcp-terraform-windows.md)

## <a name="operations-management-disciplines"></a>營運管理專業領域

除了標記和帶入資產之外，您也可以使用混合式和多重雲端工具來提供許多營運管理專業領域。

使用 Microsoft Monitoring Agent 管理軟體安裝、防毒軟體防護或其他設定管理功能的其中一個範例是成熟的作業管理專業領域。 下列文章示範如何在混合式和多重雲端環境中設定監視代理程式：

- [使用監視代理程式管理 Vm](../../manage/hybrid/server/best-practices/arc-vm-extension-mma.md)
- [監視代理程式的調整規模設定](../../manage/hybrid/server/best-practices/arc-vm-extension-custom-script.md)

## <a name="next-steps"></a>下一步

完成整合作業遷移之後，雲端採用小組就可以開始下一個案例特定的遷移。 如果有更多平臺要遷移，請使用下列文章來協助引導您下一個整合作業的遷移或部署：

- [整合作業的策略](./strategy.md)
- [規劃整合作業](./plan.md)
- [檢查您的環境或 Azure 登陸區域](./ready.md)
- [混合式和多重雲端遷移](./migrate.md)
- [管理混合式和多重雲端環境](./govern.md)
