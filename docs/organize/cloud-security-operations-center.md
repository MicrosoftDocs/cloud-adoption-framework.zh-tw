---
title: 雲端 SOC 函數
description: 瞭解雲端安全性作業中心 (SOC) 功能。
author: JanetCThomas
ms.author: janet
ms.service: cloud-adoption-framework
ms.subservice: organize
ms.topic: conceptual
ms.date: 05/15/2020
ms.openlocfilehash: 373e9b9d6a64e43beb44860507db4b877e165651
ms.sourcegitcommit: 4e12d2417f646c72abf9fa7959faebc3abee99d8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2020
ms.locfileid: "90776358"
---
<!-- docutune:casing "Cyber Defense Operations Center" -->
<!-- cSpell:ignore CISO MTTA MTTR SIEM NIST SOCs CDOC -->

# <a name="cloud-soc-functions"></a>雲端 SOC 函數

雲端安全性作業中心 (SOC) 的主要目標是偵測、回應及復原企業資產上的主動式攻擊。

隨著 SOC 成熟，安全性作業應：

- 被動回應工具偵測到的攻擊
- 主動搜尋過去回應偵測的攻擊

## <a name="modernization"></a>現代化

偵測和回應威脅目前正在進行所有層級的大量現代化。

- **提升企業風險管理：** SOC 不斷成長成為管理組織業務風險的重要元件
- **計量和目標：** 追蹤 SOC 效能從「偵測時間」到這些關鍵指標的演變：
  - 透過平均時間認可 (MTTA) 的_回應_性。
  - 透過平均時間來補救 (MTTR) 的_補救速度_。
- **技術演進：** SOC 技術正從 SIEM 中的靜態記錄檔的靜態分析發展而來，以新增使用特製化工具和複雜的分析技術。 這可提供更深入的資產深入解析，以提供高品質的警示和調查體驗，以補充 SIEM 的廣泛觀點。 這兩種工具都使用 AI 和機器學習、行為分析和整合威脅情報，來協助找出可能成為惡意攻擊者的異常動作並設定其優先順序。
- **威脅搜尋：** SOCs 新增了假設導向的威脅搜尋，可主動找出 frontline 分析師佇列的 advanced 攻擊者和轉移雜訊警示。
- **事件管理：** 專業領域已成為正規化，可協調事件的非技術性元素與法律、通訊和其他小組。
**整合內部內容：** 協助設定 SOC 活動的優先順序，例如使用者帳戶和裝置的相對風險分數、資料和應用程式的機密性，以及重要的安全隔離界限，以利緊密防禦。

 如需詳細資訊，請參閱

- [策略和架構標準 &mdash; 安全性作業](/security/compass/security-operations-videos-and-decks)
- [CISO 研討會模組4b：威脅防護策略](/security/ciso-workshop/ciso-workshop-module-4b)
- 網路防禦作業中心 (CDOC) 的 blog 系列[第 1](https://www.microsoft.com/security/blog/2019/02/21/lessons-learned-from-the-microsoft-soc-part-1-organization)部，第[2a](https://www.microsoft.com/security/blog/2019/04/23/lessons-learned-microsoft-soc-part-2-organizing-people)部，第[2b](https://www.microsoft.com/security/blog/2019/06/06/lessons-learned-from-the-microsoft-soc-part-2b-career-paths-and-readiness)部，[第 3a](https://www.microsoft.com/security/blog/2019/10/07/ciso-series-lessons-learned-from-the-microsoft-soc-part-3a-choosing-soc-tools)部，第[3b](https://www.microsoft.com/security/blog/2019/12/23/ciso-series-lessons-learned-from-the-microsoft-soc-part-3b-a-day-in-the-life)部
- [NIST 電腦安全性性事件處理指南](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)
- [網路安全性事件修復的 NIST 指南](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-184.pdf)

## <a name="team-composition-and-key-relationships"></a>小組撰寫和索引鍵關聯性

雲端安全性作業中心通常是由下列類型的角色所組成。

- IT 作業 (關閉一般連絡人) 
- 威脅情報
- 安全性架構
- Insider 風險方案
- 法律和人力資源
- 通訊小組
- 風險組織 (如果存在) 
- 在發生事件之前 (產業專屬的關聯、社區和廠商) 

## <a name="next-steps"></a>後續步驟

複習 [安全性架構](./cloud-security-architecture.md)的功能。
