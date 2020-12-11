---
title: 資源一致性原則聲明範例
description: 使用適用于 Azure 的雲端採用架構，取得範例資源一致性原則聲明，以協助您將組織的原則聲明草稿。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: internal
ms.openlocfilehash: cb6938f6d4c9214c7dbdb5fb035abafdd5c0f51f
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97019327"
---
# <a name="resource-consistency-sample-policy-statements"></a>資源一致性原則聲明範例

個別雲端原則聲明是解決在風險評估流程中識別出的特定風險方針。 這些聲明應該會提供風險的簡明摘要，以及如何處理這些風險的方案。 每個聲明定義都應該包含下列資訊：

- **技術風險：** 此原則將解決的風險摘要。
- **原則聲明：** 原則需求的清楚摘要說明。
- **設計選項：** 可操作的建議、規格或其他指引，可供 IT 小組和開發人員在執行原則時使用。

下列範例原則聲明可解決與資源一致性相關的常見商務風險。 這些語句是在草擬原則聲明以解決組織需求時可參考的範例。 這些範例並非禁止使用，而且可能會有數個原則選項可處理每個已識別的風險。 與商務和 IT 小組密切合作，為您的唯一風險組合找出最佳原則。

## <a name="tagging"></a>標記

**技術風險：** 如果沒有適當的元資料標記與已部署的資源相關聯，IT 作業將無法根據所需的 SLA、對商務營運的重要性或營運成本來優先支援或優化資源。 這可能會導致 IT 資源的錯誤配置，以及事件解決方案中的潛在延遲。

**原則聲明：** 將會執行下列原則：

- 已部署的資產應該標記下列值：
  - 成本
  - 重要性
  - SLA
  - 環境
- 治理工具必須確認與成本、重要性、SLA、應用程式及環境相關的標記。 所有值必須符合由治理小組管理的預先定義值。

**可能的設計選項：** 在 Azure 中，大部分的資源類型都支援 [標準名稱/值元資料標記](/azure/azure-resource-manager/management/tag-resources) 。 [Azure 原則](/azure/governance/policy/overview)用於在資源建立期間強制執行特定標記。

## <a name="ungoverned-subscriptions"></a>未治理的訂用帳戶

**技術風險：** 任意建立訂用帳戶和管理群組可能會導致您的雲端資產隔離區段，而這些區段未適當地受制于您的治理原則。

**原則聲明：** 針對任何任務關鍵性應用程式或受保護的資料建立新的訂用帳戶或管理群組，將需要雲端治理小組的評論。 核准的變更會整合到適當的藍圖指派中。

**可能的設計選項：** 鎖定您組織 [Azure 管理群組](/azure/governance/management-groups) 的系統管理存取權，僅限經過核准的治理小組成員，負責控制訂用帳戶的建立和存取控制流程。

## <a name="manage-updates-to-virtual-machines"></a>管理虛擬機器的更新

**技術風險：** 虛擬機器 (Vm) 不是最新的更新和軟體修補程式，很容易發生安全性或效能問題，這可能會導致服務中斷。

**原則聲明：** 治理工具必須強制在所有已部署的 Vm 上啟用自動更新。 必須與作業管理小組一起檢閱違規事件，並根據作業原則來進行修復。 不會自動更新之資產必須包含在 IT 部門所負責的處理程序中。

<!-- docutune:ignore "consistent update management" -->

**可能的設計選項：** 針對 Azure 裝載的 Vm，您可以使用 [Azure 自動化中的更新管理解決方案](/azure/automation/update-management/overview)，以提供一致的更新管理。

## <a name="deployment-compliance"></a>部署合規性

**技術風險：** 雲端治理小組未完全通過的部署腳本和自動化工具，可能會導致違反原則的資源部署。

**原則聲明：** 將會執行下列原則：

- 部署工具必須由雲端治理小組核准，以確保持續治理已部署的資產。
- 部署腳本必須在可供雲端治理小組存取的中央存放庫中進行維護，以進行定期審核和審核。

**可能的設計選項：** 一致使用 [Azure 藍圖](/azure/governance/blueprints) 來管理自動化部署，可讓您一致地部署符合您組織治理標準和原則的 Azure 資源。

## <a name="monitoring"></a>監視

**技術風險：** 未正確執行或不一致的檢測監視可能會導致無法偵測工作負載健康情況問題或其他原則合規性違規。

**原則聲明：** 將會執行下列原則：

- 治理工具必須確認所有資產都包含在監視中，以因應資源耗盡、安全性、合規性及優化。
- 治理工具必須確認正在為所有應用程式和資料收集適當的記錄資料層級。

**可能的設計選項：** [Azure 監視器](/azure/azure-monitor/overview) 是 Azure 中的預設監視服務，並可在部署資源時透過 [Azure 藍圖](/azure/governance/blueprints) 來強制執行一致的監視。

## <a name="disaster-recovery"></a>災害復原

**技術風險：** 資源失敗、刪除或損毀可能會導致任務關鍵性應用程式或服務中斷，以及機密資料遺失。

**原則聲明：** 所有任務關鍵性應用程式和受保護的資料都必須執行備份和復原解決方案，以將中斷或系統失敗的業務影響降到最低。

**可能的設計選項：**[Azure Site Recovery](/azure/site-recovery/site-recovery-overview)服務提供備份、復原和複寫功能，可將商務持續性和嚴重損壞修復的停機時間降到最低， (BCDR) 案例。

## <a name="next-steps"></a>後續步驟

使用本文提及的範例作為起點，以開發與您雲端採用方案保持一致的原則來解決特定業務風險。

若要開始開發您自己的自訂資源一致性原則聲明，請下載 [資源一致性專業版範本](./template.md)。

若要加速採用此專業領域，請選擇最符合您環境的可 [操作治理指南](../guides/index.md) 。 然後修改設計，以納入您特定的公司原則決策。

根據風險和承受度建立流程來治理和傳達資源一致性原則的遵循情況。

> [!div class="nextstepaction"]
> [建立原則合規性流程](./compliance-processes.md)
