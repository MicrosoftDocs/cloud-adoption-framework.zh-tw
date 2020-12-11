---
title: 複雜的企業治理：多重雲端改進
description: 使用適用于 Azure 的雲端採用架構，瞭解多個雲端，以及如何整合多重雲端組織以進行複雜的企業。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: internal
ms.openlocfilehash: 676732093b1d26c3b2221999d67ae2f010f6ac0a
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97021129"
---
# <a name="governance-guide-for-complex-enterprises-multicloud-improvement"></a>適用于複雜企業的治理指南：多重雲端改進

## <a name="advancing-the-narrative"></a>前進敘述

Microsoft 認為客戶可以針對特定用途採用多個雲端。 本指南中的虛構公司沒有例外。 透過其 Azure 採用旅程，企業成就可獲得小型但互補的企業。 該企業正在不同的雲端提供者上執行其所有 IT 作業。

本文將說明整合新組織時所產生的變化。 基於敘述的目的，我們假設此公司已完成本治理指南中所述的每個治理反復專案。

### <a name="changes-in-the-current-state"></a>處於目前狀態的變更

在此敘述的上一個階段中，公司已開始實行成本控制項和成本監視，因為雲端支出成為公司的一般營運費用的一部分。

從那時起，某些將會影響治理的事項已經改變：

- 身分識別會由 Active Directory 的內部部署執行個體來控制。 透過複寫到 Azure Active Directory 來促成混合式身分識別。
- IT 作業或雲端作業主要由 Azure 監視器和相關的自動化功能來管理。
- 商務持續性和嚴重損壞修復 (BCDR) 由 Azure 復原服務保存庫控制。
- Azure 資訊安全中心可用來監視安全性違規和攻擊。
- Azure 資訊安全中心和 Azure 監視器可同時用來監視雲端治理。
- Azure 藍圖、Azure 原則和管理群組可用來對原則進行合規性自動化。

### <a name="incrementally-improve-the-future-state"></a>以累加方式改進未來的狀態

目標是盡可能將收購公司整合至現有的作業。

## <a name="changes-in-tangible-risks"></a>有形風險的變更

**商務購置成本：** 取得新的業務預估大約五年的利潤。 由於報酬率偏低，因此，董事會希望盡可能地控制收購成本。 這會產生使成本控制和技術整合彼此衝突的風險。

此業務風險可能會延伸出少數技術風險：

- 雲端遷移的風險會產生額外的收購成本。
- 此外，若新環境未正確治理或導致違反原則，則會產生另一個風險。

## <a name="incremental-improvement-of-the-policy-statements"></a>原則語句的累加式改進

下列原則變更將有助於補救新的風險和指南的實施。

- 次要雲端中的所有資產都必須透過現有的操作管理和安全性監視工具來監視。
- 所有組織單位都必須整合到現有的識別提供者。
- 主要的識別提供者應該治理對次要雲端中資產的驗證。

## <a name="incremental-improvement-of-best-practices"></a>最佳做法的累加式改進

本文的這一節會改善治理 MVP 設計，以包含新的 Azure 原則以及 Azure 成本管理 + 計費的實施。 這兩個設計變更將共同實現新的公司原則聲明。

1. 網路連線。 由治理所支援的網路和 IT 安全性所執行。
    1. 新增從 MPLS 或租用行提供者到新雲端的連線，將會整合網路。 新增路由表和防火牆設定，將控制環境之間的存取與流量。
2. 合併識別提供者。 根據次要雲端中所裝載的工作負載而定，有各種不同選項可用於合併識別提供者。 以下是一些範例：
    1. 對於使用 OAuth 2 進行驗證的應用程式，可能只會將次要雲端上 Active Directory 中的使用者複製寫到現有的 Azure AD 租用戶。
    2. 另一方面，這兩個內部部署識別提供者之間的同盟，可讓使用者從新的 Active Directory 網域複寫到 Azure。
3. 將資產新增至 Azure Site Recovery。
    1. Azure Site Recovery 從一開始就建立為混合式和多重雲端工具。
    2. 次要雲端中的虛擬機器可能會受到用來保護內部部署資產的相同 Azure Site Recovery 流程所保護。
4. 將資產新增至 Azure 成本管理 + 計費。
    1. Azure 成本管理 + 計費從頭開始建立為多重雲端工具。
    2. 次要雲端中的虛擬機器可能會與某些雲端提供者的 Azure 成本管理 + 計費相容。 可能需要額外成本。
5. 將資產新增至 Azure 監視器。
    1. 一開始已從混合式雲端工具建置 Azure 監視器。
    2. 次要雲端中的虛擬機器可能會與 Azure 監視器代理程式相容，能夠將它們包含於 Azure 監視器以進行操作監控。
6. 治理強制工具。
    1. 加強治理為雲端特有。
    2. 治理指南中所建立的公司原則不是雲端特定的。 儘管實作可能因雲端而異，但原則聲明還是能夠套用到次要提供者。

根據技術需求或特定商務需求，多重雲端採用應包含在需要的位置。 隨著多重雲端採用的成長，複雜性和安全性風險也隨之增加。

## <a name="next-steps"></a>後續步驟

在許多大型企業中，雲端治理的五個專業領域可能會阻礙採用。 下一篇文章有一些有關讓治理成為小組運動的其他想法，以協助確保雲端的長期成功。

> [!div class="nextstepaction"]
> [多層控管](./multiple-layers-of-governance.md)
