---
title: 雲端 SOC 功能
description: 瞭解雲端安全性作業中心 (SOC) 的功能。
author: JanetCThomas
ms.author: janet
ms.service: cloud-adoption-framework
ms.subservice: organize
ms.topic: conceptual
ms.date: 05/15/2020
ms.openlocfilehash: ffb7f04de4a9d4b8e6b3d379269dc60ca161113b
ms.sourcegitcommit: bcc73d194c6d00c16ae2e3c7fb2453ac7dbf2526
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/09/2020
ms.locfileid: "86194284"
---
<!-- docsTest:ignore "Cyber Defense Operations Center" -->
<!-- cSpell:ignore CISO MTTA MTTR SIEM NIST SOCs CDOC -->

# <a name="cloud-soc-functions"></a>雲端 SOC 功能

雲端安全性作業中心 (SOC) 的主要目標，是要偵測、回應及復原企業資產上的主動攻擊。

隨著 SOC 成熟，安全性作業應該：

- 被動回應工具偵測到的攻擊
- 主動尋找落後過被動偵測的攻擊

## <a name="modernization"></a>現代化

偵測和回應威脅目前正在進行所有層級的重大現代化。

- 提高**業務風險管理的許可權：** SOC 正成長為管理組織業務風險的重要元件
- **計量和目標：** 追蹤 SOC 的有效性會從「偵測時間」演變為下列關鍵指標：
  - 透過平均認可 (MTTA) 的_回應_時間。
  - 透過平均補救時間 (MTTR) 的_補救速度_。
- **技術演進：** SOC 技術從獨佔使用 SIEM 中的記錄檔，到新增使用特殊工具和複雜的分析技術。 這可提供更深入的資產深入解析，提供高品質的警示和調查體驗，以補充 SIEM 的廣度。 這兩種工具都使用 AI 和機器學習服務、行為分析和整合式威脅情報來增加，以協助找出可能是惡意攻擊者的異常動作並設定其優先順序。
- **威脅搜尋：** Soc 正新增假設導向的威脅搜尋，以主動識別先進的攻擊者，並將雜訊警示從第一線分析師佇列移出。
- **事件管理：** 專業領域已正規化，可協調事件的非技術性元素與法律、通訊和其他小組。
**內部內容的整合：** 協助設定 SOC 活動的優先順序，例如使用者帳戶和裝置的相對風險分數、資料和應用程式的敏感性，以及要緊密防禦的關鍵安全性隔離界限。

 如需詳細資訊，請參閱：

- [策略和架構標準 &mdash; 安全性作業](https://docs.microsoft.com/security/compass/security-operations-videos-and-decks)
- [CISO 研討會模組4b：威脅防護策略](https://docs.microsoft.com/security/ciso-workshop/ciso-workshop-module-4b)
- 網路防禦操作中心 (CDOC) blog 系列[第1部分](https://www.microsoft.com/security/blog/2019/02/21/lessons-learned-from-the-microsoft-soc-part-1-organization)，[第 2a](https://www.microsoft.com/security/blog/2019/04/23/lessons-learned-microsoft-soc-part-2-organizing-people)部，[第 2b](https://www.microsoft.com/security/blog/2019/06/06/lessons-learned-from-the-microsoft-soc-part-2b-career-paths-and-readiness)部，part [3a](https://www.microsoft.com/security/blog/2019/10/07/ciso-series-lessons-learned-from-the-microsoft-soc-part-3a-choosing-soc-tools)，[第 3b](https://www.microsoft.com/security/blog/2019/12/23/ciso-series-lessons-learned-from-the-microsoft-soc-part-3b-a-day-in-the-life)部
- [NIST 電腦安全性性事件處理指南](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)
- [網路安全性事件修復的 NIST 指南](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-184.pdf)

## <a name="team-composition-and-key-relationships"></a>小組撰寫和按鍵關聯性

雲端安全性作業中心通常由下列角色類型組成。

- IT 營運 (關閉一般連絡人) 
- 威脅情報
- 安全性架構
- Insider 風險計畫
- 法律和人力資源
- 通訊小組
- 風險組織 (（如果有的話）) 
- 產業特定的關聯、社區和廠商 (發生事件之前) 

## <a name="next-steps"></a>後續步驟

檢查[安全性架構](./cloud-security-architecture.md)的功能。
