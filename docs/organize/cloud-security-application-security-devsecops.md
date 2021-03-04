---
title: 瞭解應用程式安全性和 DevSecOps 功能
description: 瞭解應用程式安全性和 DevSecOps 功能。
author: JanetCThomas
ms.author: janet
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: organize
ms.custom: internal
ms.openlocfilehash: 26648b7f2bfcc7a033b83ed0a07ed533c8c40669
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101789791"
---
# <a name="application-security-and-devsecops-functions"></a>應用程式安全性和 DevSecOps 功能

應用程式安全性和 DevSecOps 的目標是要將安全性保證整合至開發程式，以及自訂的企業營運 (LOB) 應用程式。

## <a name="modernization"></a>現代化

應用程式開發會在多個層面中快速改變，包括 DevOps 團隊模型、DevOps 快速發行步調，以及透過雲端服務和 Api 的應用程式技術組合。 瞭解雲端如何改變安全性關聯性和責任，以瞭解這些變更。

這種這種太開發模型的現代化帶來了商機，以及將應用程式和開發程式的安全性現代化的需求。 將安全性融合至 DevOps 程式通常稱為 DevSecOps 和磁片磁碟機變更，包括：

<!-- TODO: Link needed below? -->

- **安全性是整合式，不是外部核准：** 在應用程式開發中進行變更的快速步調，讓傳統的 arm 長度「掃描和報表」方法過時。 這些舊版方法無法趕上發行的進展，而不需要在終止開發的情況下，建立上市時間、開發人員使用量過高，以及問題待處理專案的成長。
  - 在稍早的應用程式開發過程中，向 **左移** 以參與安全性，以解決先前的問題，更便宜、更快速且更有效率。 如果您在蛋糕內建之後等候，則難以變更圖形。
  - **原生整合：** 安全性作法必須緊密整合，以避免開發工作流程中的狀況不良，以及持續整合/持續部署 (CI/CD) 流程。 如需 GitHub 方法的詳細資訊，請參閱 [保護軟體](https://github.blog/2019-09-18-securing-software-together/)。
  - **高品質安全性：** 安全性必須提供高品質的結果和指引，讓開發人員能夠快速地修正問題，而不會讓開發人員的時間變得不到誤報。
  - **聚合式文化特性：** 安全性、開發和作業角色應將重要元素貢獻到共用的文化特性、共用值，以及共用的目標和責任。
- **Agile 安全性：** 從「最適合出貨」的方法，將安全性轉移到一種敏捷式方法，從應用程式的最小可行安全性著手 (以及讓程式開發它們) ，以累加方式持續改善。
- 採用 **雲端原生** 基礎結構和安全性功能，以簡化開發流程，同時整合安全性。
- **供應鏈風險管理：** 針對開放原始碼軟體 (OSS) 和協力廠商元件（可驗證其完整性，並確保將 bug 修正和更新套用至這些元件）採用零信任方法。
- **持續學習：** 開發人員服務（有時稱為「平臺即服務」）的快速發行步調（有時稱為「平臺即服務」 (PaaS) 服務），以及變更應用程式的組合，表示開發人員、營運人員和安全性團隊成員將不斷學習新的技術。
- 以程式 **設計方式** 處理應用程式安全性，以確保能持續改進 agile 方法。

如需其他內容，請參閱 [Microsoft 安全開發生命週期](https://www.microsoft.com/sdl)。

## <a name="team-composition-and-key-relationships"></a>小組撰寫和索引鍵關聯性

安全性感知開發人員和營運團隊 (的應用程式安全性與 DevSecOps 功能，並支援安全性主題專家) 。

此功能通常會與其他功能和專家互動，包括：

- 安全性架構和作業
- 基礎結構安全性
- 通訊 (訓練和工具) 
- 人員安全性
- 身分識別和金鑰
- 合規性/風險管理小組
- 關鍵業務領導人或其代表

## <a name="next-steps"></a>下一步

檢查 [資料安全性](./cloud-security-data-security.md)的功能。
