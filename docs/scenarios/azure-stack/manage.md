---
title: 管理在 Azure Stack Hub 上執行的工作負載
description: 瞭解如何管理在 Azure Stack Hub 上執行的工作負載。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/19/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 487bb83f8b9550253e856a92bbcc7eed6f5832a3
ms.sourcegitcommit: 76edf563a08ff7dc81c3fc2dc6c8972ab3b4c55b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2020
ms.locfileid: "88237027"
---
# <a name="manage-workloads-that-run-on-azure-stack-hub"></a>管理在 Azure Stack Hub 上執行的工作負載

跨公用和私用雲端平臺的混合式解決方案操作和管理變得複雜，而且可能會對商務營運造成風險。 因為 Azure Stack 中樞是您的組織在您的資料中心執行的 Azure 私用實例，所以混合式作業的風險會大幅降低。

如雲端採用架構的 [管理方法](../../manage/index.md) 中所述，建議的作業管理活動著重于下列的核心責任清單。 支援 Azure Stack 中樞的營運管理團隊也成立同樣的責任。

- **清查和可見度**：建立跨多個雲端的資產清查。 開發對於每個資產執行狀態的可見性。
- 作業**合規性**：建立控制項和程式，以確保每個狀態都已正確設定，並在妥善管理的環境中執行。
- **保護與復原**：確保所有受控資產都受到保護，並可使用基準管理工具來復原。
- **增強的基準選項**：評估可能符合商務需求之基準的一般新增專案。
- **平臺作業**：使用妥善定義的服務類別目錄和集中管理的平臺，擴充管理基準。
- **工作負載作業**：擴充管理基準，將焦點放在任務關鍵性工作負載上。

## <a name="considerations-for-azure-stack-hub-operations-management"></a>Azure Stack 中樞作業管理的考慮

某些標準作業管理活動需要稍微不同的技術考慮。 下列文章概述這些考慮。

- 客戶[自助服務支援](https://azure.microsoft.com/blog/azure-stack-iaas-part-five/)，包括開機診斷、螢幕擷取畫面、序列記錄和計量。
- [來賓管理](https://azure.microsoft.com/blog/azure-stack-iaas-part-one/)，包括虛擬機器 (VM) 擴充功能、執行自訂程式碼、軟體清查和變更追蹤的能力。
- 透過 VM 備份和嚴重損壞修復選項的[業務持續性](https://azure.microsoft.com/blog/azure-stack-iaas-part-four/)。

## <a name="next-steps"></a>後續步驟

在 Azure Stack Hub 遷移達到運作狀態之後，您可以在 Azure 公用雲端中使用 Azure Stack 中樞或其他遷移案例，開始進行下一次的遷移反復專案。

- [規劃 Azure Stack 中樞遷移](./plan.md)
- [環境整備](./ready.md)
- [評估 Azure Stack Hub 的工作負載](./migrate-assess.md)
- [將工作負載部署到 Azure Stack Hub](./migrate-deploy.md)
- [治理 Azure Stack Hub](./govern.md)
- [管理 Azure Stack Hub](./manage.md)
