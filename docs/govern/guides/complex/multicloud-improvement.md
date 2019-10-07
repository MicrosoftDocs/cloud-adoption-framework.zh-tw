---
title: 複雜企業的治理指南：多重雲端改進
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 複雜企業的治理指南：多重雲端改進
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: aaafd0d4fa3c94d1ccf0b5bc3ee3f30377a2b08e
ms.sourcegitcommit: 945198179ec215fb264e6270369d561cb146d548
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2019
ms.locfileid: "71967659"
---
# <a name="governance-guide-for-complex-enterprises-multicloud-improvement"></a>複雜企業的治理指南：多重雲端改進

## <a name="advancing-the-narrative"></a>推進敘述

Microsoft 認為客戶可能會基於特定用途來採用多個雲端。 本指南中的虛構公司不會發生任何例外狀況。 透過其 Azure 採用旅程，企業成功導致小型但互補的企業。 該企業正在不同的雲端提供者上執行其所有 IT 作業。

本文將說明整合新組織時所產生的變化。 基於敘述的目的，我們假設此公司已完成此治理指南中所述的每項治理反覆運算。

### <a name="changes-in-the-current-state"></a>目前狀態的變更

在此敘述的上一個階段中，公司已開始實行成本控制和成本監視，因為雲端支出會成為公司一般營運費用的一部分。

從那時起，某些將會影響治理的事項已經改變：

- 身分識別會由 Active Directory 的內部部署執行個體來控制。 透過複寫到 Azure Active Directory 來促成混合式身分識別。
- IT 作業或雲端作業主要是由 Azure 監視器和相關的自動化功能來管理。
- 嚴重損壞修復和業務持續性（DRBC）是由 Azure 保存庫實例所控制。
- Azure 資訊安全中心可用來監視安全性違規和攻擊。
- Azure 資訊安全中心和 Azure 監視器可同時用來監視雲端治理。
- Azure 藍圖、Azure 原則和管理群組可用來對原則進行合規性自動化。

### <a name="incrementally-improve-the-future-state"></a>以累加方式改善未來的狀態

目標是盡可能將收購公司整合至現有的作業。

## <a name="changes-in-tangible-risks"></a>有形風險的變更

**商務取得成本：** 取得新業務的預估將在五年內獲利。 由於報酬率偏低，因此，董事會希望盡可能地控制收購成本。 這會產生使成本控制和技術整合彼此衝突的風險。

此業務風險可能會延伸出少數技術風險：

- 雲端遷移的風險會產生額外的購置成本。
- 此外，若新環境未正確治理或導致違反原則，則會產生另一個風險。

## <a name="incremental-improvement-of-the-policy-statements"></a>原則聲明的累加式改進

下列原則變更將有助於補救新的風險和指南的執行。

- 次要雲端中的所有資產都必須透過現有的操作管理和安全性監視工具來監視。
- 所有組織單位都必須整合到現有的識別提供者。
- 主要的識別提供者應該治理對次要雲端中資產的驗證。

## <a name="incremental-improvement-of-the-best-practices"></a>改善最佳做法的增量

本文的這一節會改善治理 MVP 設計，以包含新的 Azure 原則和 Azure 成本管理的執行。 這兩個設計變更將共同實現新的公司原則聲明。

1. 網路連線。 由治理支援的網路和 IT 安全性所執行。
    1. 將來自 MPLS 或租用行提供者的連接新增至新的雲端，將會整合網路。 新增路由表和防火牆設定，將控制環境之間的存取與流量。
2. 合併識別提供者。 根據次要雲端中所裝載的工作負載而定，有各種不同選項可用於合併識別提供者。 以下是一些範例：
    1. 對於使用 OAuth 2 進行驗證的應用程式，可能只會將次要雲端上 Active Directory 中的使用者複製寫到現有的 Azure AD 租用戶。
    2. 另一方面，這兩個內部部署識別提供者之間的同盟，可讓使用者從新的 Active Directory 網域複寫到 Azure。
3. 將資產新增至 Azure Site Recovery。
    1. Azure Site Recovery 是從一開始就建立成混合式和多重雲端工具。
    2. 次要雲端中的虛擬機器可能會受到用來保護內部部署資產的相同 Azure Site Recovery 流程所保護。
4. 將資產新增至 Azure 成本管理。
    1. Azure 成本管理從一開始就建立為多重雲端工具。
    2. 次要雲端中的虛擬機器可能會與某些雲端提供者的 Azure 成本管理相容。 可能需要額外成本。
5. 將資產新增至 Azure 監視器。
    1. 一開始已從混合式雲端工具建置 Azure 監視器。
    2. 次要雲端中的虛擬機器可能會與 Azure 監視器代理程式相容，能夠將它們包含於 Azure 監視器以進行操作監控。
6. 治理強制工具。
    1. 加強治理為雲端特有。
    2. 治理指南中所建立的公司原則不是雲端特有的。 儘管實作可能因雲端而異，但原則聲明還是能夠套用到次要提供者。

根據技術需求或特定商務需求，應將多雲端採用包含在需要的位置。 隨著多重雲端採用的成長，將會造成複雜性和安全性風險。

## <a name="next-steps"></a>後續步驟

在許多大型企業中，雲端治理的五個專業領域可能會成為採用的障礙。 下一篇文章有一些關於讓治理成為小組運動的其他想法，以協助確保雲端中的長期成功。

> [!div class="nextstepaction"]
> [多層治理](./multiple-layers-of-governance.md)
