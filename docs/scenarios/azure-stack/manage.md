---
title: 管理在 Azure Stack Hub 上執行的工作負載
description: 瞭解如何管理在 Azure Stack Hub 上執行的工作負載。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/19/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: internal
ms.openlocfilehash: 63cd498b92141a6c9e8ee0dbefae4842ec223145
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97025804"
---
# <a name="manage-workloads-that-run-on-azure-stack-hub"></a>管理在 Azure Stack Hub 上執行的工作負載

跨公用和私用雲端平臺的混合式解決方案的作業和管理很複雜，而且可能會對商務營運產生風險。 由於 Azure Stack Hub 是您的組織在資料中心內執行的私人 Azure 實例，混合作業的風險會大幅降低。

如同雲端採用架構的 [管理方法](../../manage/index.md) 中所述，建議的作業管理活動著重于下列核心職責清單。 針對支援 Azure Stack Hub 的營運管理小組，相同的職責也成立。

- **清查和可見性：** 建立跨多個雲端的資產清查。 開發對於每個資產執行狀態的可見性。
- **營運合規性：** 建立控制項和程式，以確保每個狀態都已正確設定，且在妥善管理的環境中執行。
- **保護和復原：** 確定所有受管理的資產都受到保護，而且可以使用基準管理工具來復原。
- **增強的基準選項：** 評估可能符合商務需求的常見附加基準。
- **平台作業：** 使用妥善定義的服務目錄和集中管理的平台來延伸管理基準。
- **工作負載作業：** 延伸管理基準，以納入對於任務關鍵性工作負載的關注。

## <a name="considerations-for-azure-stack-hub-operations-management"></a>Azure Stack Hub 作業管理的考慮

某些標準作業管理活動需要稍微不同的技術考慮。 下列文章概述這些考慮事項。

- 客戶的[自助支援](https://azure.microsoft.com/blog/azure-stack-iaas-part-five/)，包括開機診斷、螢幕擷取畫面、序列記錄和計量。
- [來賓管理](https://azure.microsoft.com/blog/azure-stack-iaas-part-one/)，包括虛擬機器 (VM) 擴充功能、執行自訂程式碼的能力、軟體清查和變更追蹤。
- 透過 VM 備份和嚴重損壞修復選項的[商務持續性](https://azure.microsoft.com/blog/azure-stack-iaas-part-four/)。

## <a name="next-steps"></a>後續步驟

當您的 Azure Stack Hub 遷移達到操作狀態之後，您就可以使用 Azure 公用雲端中的 Azure Stack Hub 或其他遷移案例，開始下一次的遷移反復專案。

- [規劃 Azure Stack Hub 遷移](./plan.md)
- [環境整備](./ready.md)
- [評估 Azure Stack Hub 的工作負載](./migrate-assess.md)
- [將工作負載部署到 Azure Stack Hub](./migrate-deploy.md)
- [治理 Azure Stack Hub](./govern.md)
- [管理 Azure Stack Hub](./manage.md)
