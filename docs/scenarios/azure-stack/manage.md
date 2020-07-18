---
title: 管理在 Azure Stack Hub 上執行的工作負載
description: 瞭解如何管理在 Azure Stack Hub 上執行的工作負載。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/19/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 58d3f1eee0ea3514ccf3b9ee99f30fb7bcef978e
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86451843"
---
# <a name="manage-workloads-running-on-azure-stack-hub"></a>管理在 Azure Stack Hub 上執行的工作負載

跨公用和私用雲端平臺的混合式解決方案操作和管理變得複雜，而且可能會對商務營運造成風險。 由於 Azure Stack Hub 是您在資料中心內執行的專屬 Azure 私人實例，因此混合式作業的風險會大幅降低。

如雲端採用架構的[管理方法](../../manage/index.md)中所述，建議的作業管理活動著重于下列的核心責任清單。 同樣的責任也適用于支援 Azure Stack 中樞的營運管理團隊。

- **清查和可見性：** 建立跨多個雲端的資產清查。 開發對於每個資產執行狀態的可見性。
- **作業合規性：** 建立控管規定和流程，確保每個狀態都已完成正確設定，並在妥善管理的環境中執行。
- **保護和復原：** 確保所有受控資產都有受到保護，並可使用基準管理工具加以復原。
- **增強的基準選項：** 評估可能符合商務需求的常見附加基準。
- **平台作業：** 使用妥善定義的服務目錄和集中管理的平台來延伸管理基準。
- **工作負載作業：** 延伸管理基準，以納入對於任務關鍵性工作負載的關注。

## <a name="azure-stack-hub-operations-management-considerations"></a>Azure Stack 中樞作業管理考慮

某些標準作業管理活動需要稍微不同的技術考慮。 下列文章概述這些考慮事項。

- 客戶[自助服務支援](https://azure.microsoft.com/blog/azure-stack-iaas-part-five/)，包括開機診斷、螢幕擷取畫面、序列記錄和計量。
- [來賓管理](https://azure.microsoft.com/blog/azure-stack-iaas-part-one/)，包括 VM 擴充功能、執行自訂程式碼、軟體清查和變更追蹤的能力。
- 透過 VM 備份和嚴重損壞修復選項的[業務持續性](https://azure.microsoft.com/blog/azure-stack-iaas-part-four/)。

## <a name="next-step-integrate-this-strategy-into-your-cloud-adoption-journey"></a>下一步：將此策略整合到您的雲端採用旅程圖

當您的 Azure Stack Hub 遷移達到運作狀態之後，您就可以使用 Azure 公用雲端中的 Azure Stack 中樞或其他遷移案例開始進行下一次的遷移反復作業。

- [規劃 Azure Stack 中樞遷移](./plan.md)
- [環境就緒](./ready.md)
- [評估 Azure Stack 中樞的工作負載](./migrate-assess.md)
- [將工作負載部署到 Azure Stack 中樞](./migrate-deploy.md)
- [管理 Azure Stack 中樞](./govern.md)
- [管理 Azure Stack Hub](./manage.md)
