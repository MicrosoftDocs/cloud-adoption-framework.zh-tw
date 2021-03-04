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
ms.openlocfilehash: 571097cc84f7380e25f8a5393afb920c5a4ac9a1
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102112818"
---
# <a name="manage-your-portfolio-across-hybrid-and-multicloud-operations"></a>跨混合式和多重雲端作業管理您的組合

混合式和多重雲端會導致您在操作雲端時的自然轉變。 適用于 Azure 的 Microsoft 雲端採用架構中的「 [管理」方法](../../manage/index.md) 會概述用來執行作業基準的路徑，並在整個雲端採用生命週期中成熟該基準。 擴充您的策略以納入混合式、多重雲端和邊緣部署，將需要您如何進行適當的作業管理。 [整合作業](./unified-operations.md) 是解決這些移位需求的最佳作法概念。

下一節將概述如何套用整合作業的概念，以及如何實行最佳作法以確保有效的混合式、多重雲端和邊緣作業。

## <a name="extending-your-operations-baseline"></a>擴充您的作業基準

[Azure Arc](/azure/azure-arc/overview) 可降低擴充營運基準的複雜度和成本。 在您的資料中心、混合式雲端和多重雲端環境中部署 Azure Arc，將會擴充 azure Resource Manager 中包含的 Azure 原生功能。

若要開始使用橫跨內部部署和多個雲端提供者的作業基準，請完成簡單的清查和標記練習。 這個簡單的練習將開始以幾個簡單的步驟擴充您的作業基準：

- 將的標記新增 `hosting platform` 至所有混合式、多重雲端和邊緣資產。
- 從 AWS、GCP 等標記資源。
- 查詢您的資源以探索其託管位置。

若要開始使用，請 [清查並標記您的混合式和多重雲端資源](../../manage/hybrid/server/best-practices/arc-inventory-tagging.md)

<!-- docutune:casing "update management guide" -->

完成基本的清查和標記練習之後，您就可以開始操作您的混合式和多重雲端環境。 在跨雲端擴充作業時，大部分客戶所採取的第一個步驟是為 **修補和更新管理建立一致的計畫**。 遵循 [混合式和多重雲端更新管理指南](../../manage/hybrid/server/best-practices/arc-update-management.md) ，部署可跨雲端提供者控制修補的工具。

## <a name="enhanced-baseline"></a>增強的基準

將持續更廣泛的資產和雲端提供者上線，以增強您的營運基準。 下列清單提供一些範例，說明您可以新增至展開的作業基準的資產類型。

- 將 Azure 資產上架： [Linux vm](../../manage/hybrid/server/best-practices/arm-template-linux.md) 和 [Windows vm](../../manage/hybrid/server/best-practices/arm-template-windows.md)
- 在您的本機資料中心內架資產： [Linux vm](../../manage/hybrid/server/best-practices/onboard-server-linux.md) 和 [Windows vm](../../manage/hybrid/server/best-practices/onboard-server-windows.md)
- 將 VMware 資產上架： [Linux vm](../../manage/hybrid/server/best-practices/vmware-scaled-powercli-linux.md) 和 [Windows vm](../../manage/hybrid/server/best-practices/vmware-scaled-powercli-windows.md)
- 將 AWS 資產上線： [Linux vm 搭配 Terraform](../../manage/hybrid/server/best-practices/aws-terraform-al2.md) 和 [AWS Ubuntu with Terraform](../../manage/hybrid/server/best-practices/aws-terraform-ubuntu.md)
- 將 GCP 資產上架： [Ubuntu vm](../../manage/hybrid/server/best-practices/gcp-terraform-ubuntu.md) 和 [Windows vm](../../manage/hybrid/server/best-practices/gcp-terraform-windows.md)

## <a name="operations-management-disciplines"></a>營運管理專業領域

除了標記和上架資產之外，您也可以使用混合式和多重雲端工具，提供許多營運管理專業領域。

使用 Microsoft Monitoring Agent (MMA) 管理軟體安裝、防毒軟體防護或其他設定管理功能的其中一個範例是成熟的作業管理專業領域。 下列文章示範如何在混合式和多重雲端環境中設定 MMA。

- [使用 MMA 管理 Vm](../../manage/hybrid/server/best-practices/arc-vm-extension-mma.md)
- [MMA 的調整規模設定](../../manage/hybrid/server/best-practices/arc-vm-extension-custom-script.md)

## <a name="next-step-your-next-migration-iteration"></a>下一步：您的下一個遷移反復專案

完成整合作業遷移之後，雲端採用小組就可以開始下一個案例特定的遷移。 或者，如果有其他要遷移的平臺，您可以使用下列文章來協助引導您下一個整合作業的遷移或部署。

- [整合作業的策略](./strategy.md)
- [規劃整合作業](./plan.md)
- [檢查您的環境或 Azure 登陸區域](./ready.md)
- [混合式和多重雲端遷移](./migrate.md)
- [管理混合式和多重雲端環境](./govern.md)
