---
title: 瞭解應用程式安全性和 DevSecOps 函式
description: 瞭解應用程式安全性和 DevSecOps 功能。
author: JanetCThomas
ms.author: janet
ms.service: cloud-adoption-framework
ms.subservice: organize
ms.topic: conceptual
ms.date: 05/15/2020
ms.openlocfilehash: ecbe91a3c5a1ed8f06f49bd82214c5b734e7b807
ms.sourcegitcommit: d88c1cc3597a83ab075606d040ad659ac4b33324
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/15/2020
ms.locfileid: "84787736"
---
# <a name="application-security-and-devsecops-functions"></a>應用程式安全性和 DevSecOps 函式

應用程式安全性和 DevSecOps 的目標是要將安全性保證整合到開發流程和自訂企業營運（LOB）應用程式。

## <a name="modernization"></a>現代化

應用程式開發會在多個層面中快速重做，包括 DevOps 小組模型、DevOps 快速發行步調，以及透過雲端服務和 Api 的應用程式技術組合。 如需這些變更的詳細資訊，請參閱雲端如何變更安全性關聯性和責任。

這項 antiquated 開發模型的現代化呈現了機會，以及將應用程式和開發流程的安全性現代化的需求。 將安全性融合到 DevOps 程式中通常稱為 DevSecOps 和磁片磁碟機變更，包括：

<!-- TODO: Link needed below? -->
- **安全性已整合，而不是在核准外部：** 應用程式開發的快速變更速度使傳統臂長度的「掃描和報告」方法過時。 這些舊版方法無法趕上發行，而不需要再進行終止開發，也不會建立上市時間、開發人員使用量過低，以及問題待處理專案的成長。
  - 在稍早的應用程式開發過程中，向**左移動**安全性，因為修正問題較便宜、更快速且更有效率。 如果您等到蛋糕內建之後，就難以變更圖形。
  - **原生整合：** 安全性作法必須緊密整合，以避免在開發工作流程和持續整合/持續部署（CI/CD）流程中發生狀況不佳的摩擦。 若要瞭解 GitHub 如何達到此，請參閱[保護軟體的安全](https://github.blog/2019-09-18-securing-software-together)。
  - **高品質的安全性：** 安全性必須提供高品質的發現和指導方針，讓開發人員能夠快速修正問題，而不會浪費開發人員時間的誤報。
  - 交集**文化特性：** 安全性、開發和作業角色應該將重要元素提供給共用的文化、共用值，以及共用的目標和標準責任。
- **Agile 安全性：** 將安全性從「必須是完美的出貨」方法轉移到靈活的方法，其開頭為應用程式的最小可行安全性（以及開發它們的進程），並以累加方式持續改善。
- 採用**雲端原生**基礎結構和安全性功能，以簡化開發流程，同時整合安全性。
- **供應鏈風險管理：** 採用零信任的方法來開啟來源軟體（OSS）和協力廠商元件，以驗證其完整性並確保將錯誤修正和更新套用到這些元件。
- **持續學習：** 開發人員服務的快速發行步調，有時稱為平臺即服務（PaaS）服務，而變更應用程式的組合，表示 dev、ops 和安全性小組成員將持續學習新技術。
- 應用程式安全性的程式**設計方式**，以確保能持續改進 agile 方法。

如需其他內容，請參閱[Microsoft 安全開發生命週期](https://www.microsoft.com/sdl)。

## <a name="team-composition-and-key-relationships"></a>小組撰寫和按鍵關聯性

應用程式安全性和 DevSecOps 函式最適合由安全性感知開發人員和營運團隊（支援安全性主題專家）來執行。

此函式通常與其他函數和專家互動，包括：。

- 安全性架構和作業
- 基礎結構安全性
- 通訊（訓練和工具）
- 人員安全性
- 身分識別和金鑰
- 合規性/風險管理小組
- 主要業務領導人或其代表

## <a name="next-steps"></a>後續步驟

檢查[資料安全性](./cloud-security-data-security.md)的功能。
