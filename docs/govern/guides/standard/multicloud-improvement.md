---
title: 標準企業治理：多重雲端改進
description: 使用適用于 Azure 的雲端採用架構來瞭解多個雲端，以及如何將多個雲端整合至現有的作業。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/05/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 27ee8420a1d610705b76c80590229ee1a7c454da
ms.sourcegitcommit: 7d3fc1e407cd18c4fc7c4964a77885907a9b85c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "80434265"
---
<!-- cSpell:ignore MPLS -->

# <a name="standard-enterprise-governance-guide-multicloud-improvement"></a>標準企業治理指南：多重雲端改進

本文會藉由新增多重雲端採用的控制項來推進敘述。

## <a name="advancing-the-narrative"></a>推進敘述

Microsoft 認為客戶可能會基於特定用途來採用多個雲端。 本指南中的虛構客戶不會發生例外狀況。 透過其 Azure 採用旅程，企業成功導致小型但互補的企業。 該企業正在不同的雲端提供者上執行其所有 IT 作業。

本文將說明整合新組織時所產生的變化。 基於敘述的目的，我們假設此公司已完成此治理指南中所述的每項治理反覆運算。

### <a name="changes-in-the-current-state"></a>目前狀態的變更

在此敘述的上一個階段中，公司已經開始透過 CI/CD 管線主動將生產應用程式推送至雲端。

從那時起，某些將會影響治理的事項已經改變：

- 身分識別會由 Active Directory 的內部部署執行個體來控制。 透過複寫到 Azure Active Directory 來促成混合式身分識別。
- IT 作業或雲端作業主要是由 Azure 監視器和相關的自動化程式來管理。
- 嚴重損壞修復和商務持續性是由 Azure 保存庫實例所控制。
- Azure 資訊安全中心可用來監視安全性違規和攻擊。
- Azure 資訊安全中心和 Azure 監視器可同時用來監視雲端治理。
- Azure 藍圖、Azure 原則和 Azure 管理群組可用來對原則進行合規性自動化。

### <a name="incrementally-improve-the-future-state"></a>以累加方式改善未來的狀態

目標是盡可能將收購公司整合至現有的作業。

## <a name="changes-in-tangible-risks"></a>有形風險的變更

**商務取得成本：** 取得新業務的預估將在五年內獲利。 由於報酬率偏低，因此，董事會希望盡可能地控制收購成本。 這會產生使成本控制和技術整合彼此衝突的風險。

此業務風險可能會延伸出少數技術風險：

- 雲端遷移可能會產生額外的購置成本。
- 新環境可能不會受到適當的控管，而可能導致原則違規。

## <a name="incremental-improvement-of-the-policy-statements"></a>原則聲明的累加式改進

下列原則變更將有助於補救新的風險和指南的執行：

- 次要雲端中的所有資產都必須透過現有的操作管理和安全性監視工具來監視。
- 所有組織單位都必須整合到現有的識別提供者。
- 主要的識別提供者應該治理對次要雲端中資產的驗證。

## <a name="incremental-improvement-of-governance-practices"></a>治理做法的累加式改進

本文的這一節將會變更治理 MVP 設計，以包含新的 Azure 原則和 Azure 成本管理的執行。 這些設計變更會共同滿足新的公司原則聲明。

1. 網路連線。 此步驟是由網路和 IT 安全性小組執行，並由雲端治理小組所支援。 新增從 MPLS/租用線路提供者到新雲端的連線，將會整合網路。 新增路由表和防火牆設定，將控制環境之間的存取與流量。
2. 合併識別提供者。 根據次要雲端中所裝載的工作負載而定，有各種不同選項可用於合併識別提供者。 以下是一些範例：
    1. 對於使用 OAuth 2 進行驗證的應用程式，次要雲端上 Active Directory 中的使用者可能只會複製寫到現有的 Azure AD 租用戶。 這可確保所有使用者在租用戶中進行驗證。
    2. 另一個極端情況是，同盟會讓 OU 進入 Active Directory 內部部署環境，然後再到 Azure AD 執行個體。
3. 將資產新增至 Azure Site Recovery。
    1. Azure Site Recovery 是以混合式或多重雲端工具的形式從頭開始設計。
    2. 次要雲端中的 VM 可能會受到用來保護內部部署資產的相同 Azure Site Recovery 流程所保護。
4. 將資產新增至 Azure 成本管理。
    1. Azure 成本管理的設計是從一開始就是多重雲端工具。
    2. 次要雲端中的虛擬機器可能會與某些雲端提供者的 Azure 成本管理相容。 可能需要額外成本。
5. 將資產新增至 Azure 監視器。
    1. Azure 監視器是以混合式雲端工具的形式從頭開始設計。
    2. 次要雲端中的虛擬機器可能會與 Azure 監視器代理程式相容，好讓虛擬機器包含於 Azure 監視器以進行作業監視。
6. 採用治理強制工具。
    1. 加強治理為雲端特有。
    2. 治理指南中所建立的公司原則不是雲端特有的。 儘管實作可能因雲端而異，但原則還是能夠套用到次要提供者。

根據技術需求或特定商務需求，應將多重雲端採用包含在需要的位置。 隨著多重雲端的採用成長，將會造成複雜性和安全性風險。

## <a name="conclusion"></a>結論

這一系列的文章說明了治理最佳作法的累加式開發，與此虛構公司的體驗一致。 藉由從小規模開始 (但使用正確的基礎)，公司可以快速移動，而且還可以在對的時間套用對的治理方法。 MVP 本身不會保護客戶。 相反地，它會建立管理風險和新增保護的基礎。 從該處，已套用治理層來補救有形風險。 此處提供的確切旅程不會 100% 符合讀者的體驗。 但是，可以用此模式來累加治理方式。 您應塑造這些最佳作法，以符合您自己獨特的條件約束和治理需求。
