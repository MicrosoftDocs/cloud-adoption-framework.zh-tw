---
title: 複雜的企業治理：改善身分識別基準專業領域
description: 使用適用于 Azure 的雲端採用架構，瞭解如何將身分識別主控項新增至治理的最小可行產品 (MVP) 。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/06/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: internal
ms.openlocfilehash: 0f00a68c43c5761954182af2b2edfa318692e121
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101790760"
---
# <a name="governance-guide-for-complex-enterprises-improve-the-identity-baseline-discipline"></a>適用于複雜企業的治理指南：改善身分識別基準專業領域

本文藉由將身分識別主控項新增至治理 MVP，來提升敘述的進展。

## <a name="advancing-the-narrative"></a>前進敘述

CFO 已核准將兩個資料中心移轉至雲端的商業論證。 在研究技術可行性的期間，其找到幾個障礙：

- 受保護的資料及任務關鍵性應用程式佔了兩個資料中心 25% 的工作負載。 在目前已現代化有關機密個人資料和任務關鍵性應用程式的治理原則之前，都無法消除。
- 在這兩個資料中心內，7% 的資產與雲端不相容。 其會先移至替代的資料中心，再終止資料中心的合約。
- 資料中心內有 15% 的資產 (750 個虛擬機器) 相依於舊式的驗證或第三方多重要素驗證。
- 連結現有資料中心與 Azure 的 VPN 連線未提供足夠的資料傳輸速度或延遲，因而無法在兩年內遷移大量資產，從而淘汰資料中心。

前兩個障礙會以平行方式進行管理。 本文會講述第三個和第四個障礙的解決方案。

### <a name="expand-the-cloud-governance-team"></a>拓展雲端治理小組

雲端治理小組正在進行擴充。 由於需要額外的身分識別管理支援，來自身分識別基準小組的系統管理員現在會參與每週會議，讓現有的團隊成員知道有哪些變更。

### <a name="changes-in-the-current-state"></a>處於目前狀態的變更

IT 小組已核准將 CIO 和 CFO 的計畫推進，以淘汰兩個資料中心。 小組擔心，這些資料中心內的資產 750 (15% ) 必須移至雲端以外的地方。

### <a name="incrementally-improve-the-future-state"></a>以累加方式改進未來的狀態

新的未來狀態計畫需要更健全的身分識別基準解決方案，才能遷移具有舊版驗證需求的750虛擬機器。 除了這兩個資料中心，這項挑戰預期會影響其他資料中心內類似的資產百分比。

未來的狀態現在也需要從雲端提供者到公司的 MPLS/租用線路解決方案之間的連接。

目前和未來狀態的變更會產生新風險，因此需要新的原則聲明。

## <a name="changes-in-tangible-risks"></a>有形風險的變更

**在遷移期間的業務中斷。** 移轉至雲端時，會產生受控而可以管理的具時效性風險。 將老舊硬體移至世界上的其他地方，風險會更高。 為了避免中斷業務運作，其需要緩解策略。

**現有的身分識別相依性。** 對現有驗證和身分識別服務的相依性可能會延遲或阻礙將某些工作負載移轉至雲端的工作。 若未能讓這兩個資料中心準時恢復上線，將會產生好幾百萬美元的資料中心租賃費用。

此業務風險可能會延伸出少數技術風險：

- 舊式驗證可能無法在雲端中使用，而限制了某些應用程式的部署。
- 目前的協力廠商多重要素驗證解決方案可能無法在雲端中使用，而限制了某些應用程式的部署。
- Techops 或移動可能會導致中斷或增加成本。
- VPN 的速度和穩定性可能會妨礙移轉。
- 進入雲端的流量可能會造成全球網路的其他部分發生安全性問題。

## <a name="incremental-improvement-of-the-policy-statements"></a>原則語句的累加式改進

下列原則變更將有助於補救新的風險和指南的實施。

- 所選擇的雲端提供者必須提供可透過舊式方法進行驗證的途徑。
- 選擇的雲端提供者必須提供一種驗證方法，以使用目前的協力廠商多重要素驗證解決方案。
- 在雲端提供者與公司的電信提供者之間建立高速私人連線，將雲端提供者連接到資料中心的全球網路。
- 在建立好足夠的安全性需求之前，不得讓輸入的公用流量存取裝載在雲端的公司資產。 所有連接埠都要封鎖位於全域 WAN 之外的來源。

## <a name="incremental-improvement-of-best-practices"></a>最佳做法的累加式改進

治理 MVP 設計變更為包含新的 Azure 原則，以及在虛擬機器上執行 Active Directory。 這兩個設計變更會共同實現新的公司原則聲明。

以下是新的最佳做法：

- **安全混合式虛擬網路藍圖：** 混合式網路的內部部署端應設定為允許下列解決方案與內部部署 Active Directory 伺服器之間的通訊。 這種最佳做法需要周邊網路，才能跨網路界限啟用 Active Directory 網域服務。
- **Azure Resource Manager 範本：**
    1. 定義 NSG 以封鎖外部流量，並允許內部流量。
    2. 根據黃金映射，在負載平衡的配對中部署兩部 Active Directory 虛擬機器。 在首次開機時，該映像會執行 PowerShell 指令碼，來加入網域並向網域服務註冊。 如需詳細資訊，請參閱[將 Active Directory Domain Services (AD DS) 擴充至 Azure](/azure/architecture/reference-architectures/identity/adds-extend-domain)。
- Azure 原則：將 NSG 套用至所有資源。
- Azure 藍圖：
    1. 建立名為 `active-directory-virtual-machines` 的藍圖。
    2. 將每個 Active Directory 範本和原則新增至藍圖。
    3. 將藍圖發行至任何適用的管理群組中。
    4. 將藍圖套用至任何需要舊版或協力廠商多重要素驗證的訂用帳戶。
    5. 在 Azure 中執行的 Active Directory 實例現在可以做為內部部署 Active Directory 解決方案的延伸模組，讓它能夠與現有的多重要素驗證工具整合，並透過現有的 Active Directory 功能提供宣告式驗證。

## <a name="conclusion"></a>結論

將這些變更新增至治理 MVP 有助於補救本文中的許多風險，讓每個雲端採用小組都能快速地超越此障礙。

## <a name="next-steps"></a>下一步

隨著雲端採用持續提供額外的商業價值，風險和雲端治理需求也會改變。 以下是可能會發生的一些變更。 針對此虛構公司，下一個觸發程式是在雲端採用計畫中納入受保護的資料。 這種變更需要額外的安全性控制。

> [!div class="nextstepaction"]
> [改善安全性基準專業領域](./security-baseline-improvement.md)
