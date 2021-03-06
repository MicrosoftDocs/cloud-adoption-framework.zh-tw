---
title: 在移轉前修復資產
description: 瞭解如何在開始遷移之前，補救您判斷與所選雲端提供者不相容的任何資產。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: internal
ms.openlocfilehash: 02a2b5fe45daf2fa73957d44c0033ed0424eb327
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101786697"
---
# <a name="remediate-assets-prior-to-migration"></a>在移轉前修復資產

在移轉的評估程序進行期間，小組會設法找出任何會使資產與所選雲端提供者不相容的設定。 「修復」是移轉程序中的檢查點，可確保這些不相容的問題能夠獲得解決。 本文會討論一些常見的修復工作以供您參考。 文中也會建立可用來決定修復是否值得的基本架構程序。

## <a name="common-remediation-tasks"></a>常見的修復工作

任何公司環境都有技術債務。 有些債務是健康的且符合預期。 適用於內部部署環境的架構決策不一定就完全適用於雲端平台。 不論是哪一種情況，您可能都需要進行常見的修復工作才能讓資產準備好進行移轉。 以下是一些範例：

- **稍微升級主機。** 有時候必須先將已過期的主機升級才能進行複寫。
- **稍微升級客體作業系統。** 作業系統在進行複寫之前很可能需要先修補或升級。
- **修改 SLA。** 雲端平台中的備份和復原功能有大幅變更。 資產可能需要稍微修改其備份程序以確保能在雲端中繼續運作。
- **移轉 PaaS。** 在某些情況下，可能需要對資料結構或應用程式進行 PaaS 部署才能加快部署速度。 您可能需要稍微修改 PaaS 部署才能讓其解決方案做好準備。
- **變更 PaaS 程式碼。** 自訂應用程式需要稍微修改程式碼才能準備好用於 PaaS 的情況並不少見。 範例可能包括會寫入至本機磁碟或使用記憶體中工作階段狀態等等的方法。
- **變更應用程式設定。** 遷移後的應用程式可能需要變更變數 (例如相依資產的網路路徑)、變更服務帳戶或更新相依的 IP 位址。
- **稍微變更網路路徑。** 您可能需要修改路由模式才能正確地將使用者流量路由傳送至新的資產。
    > [!NOTE]
    > 這並非生產環境路由傳送至新的資產，而是可讓您在一般情況下適當地路由傳送至資產的設定。

## <a name="large-scale-remediation-tasks"></a>大規模修復工作

如果資料中心有正確地維護、修補和更新，就可能不太需要修復。 需要大幅修復的環境往往存在於大型企業、經歷過 IT 部門大幅裁員的組織、一些舊版的受控服務環境，以及進行過大量收購的環境。 上述幾類環境的修復可能會佔據移轉工作的一大部分內容。 當下列修復工作經常出現並且會對移轉速度或一致性造成負面影響時，最好將修復工作細分為平行的工作和小組 (類似於雲端採用和雲端治理平行執行的方式)。

- **經常升級主機。** 當有大量主機必須升級才能完成工作負載移轉時，移轉小組可能會遭遇延遲困擾。 在任何已規劃的版本中包含受影響的應用程式之前，最好先將受影響的應用程式細分，並解決補救步驟。
- **經常升級客體作業系統。** 大型企業一般會有在過期的 Linux 或 Windows 版本上執行的伺服器。 操作過期的作業系統除了明顯會有安全性風險外，也會有不相容的問題而會導致無法遷移受影響的工作負載。 當有大量的 VM 需要修復作業系統時，最好將這些工作細分為平行的反覆項目。
- **大幅變更程式碼。** 較舊的自訂應用程式可能需要更大幅度的修改才能讓其做好進行 PaaS 部署的準備。 有這種情況時，最好是將這些應用程式從移轉待辦項目完整移除，並在完全分開的程式中進行管理。

## <a name="decision-framework"></a>決策架構

因為較小的工作負載補救可能很簡單，所以您應該為初始遷移選擇較小的工作負載。 不過，隨著移轉工作的熟練，當您開始處理較大型的工作負載時，修復程序將會相當耗時且成本高昂。 例如，在涉及資產有 5000 個以上 VM 的集區中，其 Windows Server 2003 移轉的修復工作可能會導致移轉延遲數個月。 如果需要這類大規模修復，下列問題可協助您引導決策的進行：

- 是否已在移轉待辦項目中識別出並註明修復所影響到的所有工作負載？
- 對於未受影響的工作負載，移轉是否會產生類似的投資報酬率 (ROI)？
- 受影響的資產是否可以配合原始的移轉時間表來進行修復？ 時間表變更會對 ROI 造成什麼影響？
- 讓修復資產與移轉工作平行進行在經濟上是否可行？
- 員工是否有足夠的頻寬可進行修復和遷移？ 是否應聯繫合作夥伴來執行其中一項工作或兩項都執行？

如果這些問題沒有得到有利的答案，或許還有幾種超越基本 IaaS 重新裝載策略的替代方法值得考慮：

- **集裝箱。** 某些資產可以裝載於容器化環境中，而不需要修復。 這可能會產生較不理想的效能，且不會解決安全性或合規性問題。
- **自動化。** 視工作負載和修復需求而定，使用 DevOps 方法編寫部署至新資產的指令碼可能會更有利。
- **重建。** 當修復成本非常高且商業價值同樣很高時，工作負載可能就相當適合作為重建或重新架構的候選項目。

## <a name="next-steps"></a>下一步

修復完成後，[複寫活動](./replicate.md)就準備就緒。

> [!div class="nextstepaction"]
> [複寫資產](./replicate.md)
