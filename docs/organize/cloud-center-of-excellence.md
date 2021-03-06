---
title: 瞭解雲端 (CCoE) 功能的卓越雲端中心
description: 瞭解雲端中心卓越 (CCoE) 的功能，包括來源、範圍和交付專案。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: organize
ms.custom: internal
ms.openlocfilehash: ca61c0efb127190d263693112d55acd2e2fb959c
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102116337"
---
# <a name="cloud-center-of-excellence-ccoe-functions"></a>卓越的雲端中心 (CCoE) 功能

商務和技術靈活性是大部分 IT 組織的核心目標。 雲端卓越 (CCoE) 是一種可在速度和穩定性之間建立平衡的函式。

## <a name="function-structure"></a>函數結構

CCoE 模型需要下列各項之間的共同作業：

- 雲端採用 (特別是解決方案架構設計人員) 
- 雲端策略 (特別是計畫和專案經理) 
- 雲端治理
- 雲端平台
- 雲端自動化

## <a name="impact-and-cultural-change"></a>影響與文化變更

當此函式已正確結構化且受支援時，參與者可以加速創新和遷移工作，同時降低變更的整體成本並提高企業靈活性。 成功完成時，此函式可能會在上市時產生顯著的縮減。 隨著團隊實務的成熟，品質指標將會改善，包括可靠性、效能效率、安全性、可維護性和客戶滿意度。 如果公司打算實行大規模雲端遷移工作，或想要使用雲端來推動與市場差異相關聯的創新，則這些提升效率、靈活性和品質就特別重要。

成功時，CCoE 模型會在其中建立大量的文化轉變。 CCoE 方法的基本前提是它可作為企業的代理人、合作夥伴或代表。 此模型是在企業與 IT 資產之間，從傳統的觀點來看，成為營運單位或抽象層。

下圖提供此文化變更的比喻。 如果沒有 CCoE 方法，它可能會專注于提供控制和集中責任，就像是在交集的 stoplights。 當 CCoE 成功時，焦點會放在自由和委派的責任上，這比較像是在交集的這繞遠路。

![CCoE 典範轉變的類比](../_images/ready/ccoe-paradigm-shift.png)

上述類似圖中所示的兩種方法都不正確或錯誤，它們只是責任與管理的替代觀點。 如果想要建立自助模型，讓業務單位做出自己的決策，同時遵守一組指導方針和建立的可重複控制項，則 CCoE 模型可以納入技術策略中。

## <a name="key-responsibilities"></a>重要責任

CCoE 團隊的主要職責是透過雲端原生或混合式解決方案，加速雲端採用。

CCoE 的目標是：

- 透過敏捷式方法來協助打造現代化的 IT 組織，以實現商務需求。
- 使用與安全性、合規性和服務管理原則一致的可重複使用部署套件。
- 維護可運作的 Azure 平臺與操作程式的一致性。
- 複習並核准使用雲端原生工具。
- 經過一段時間，將通常需要的平臺元件和解決方案標準化並自動化。

## <a name="meeting-cadence"></a>會議步調

CCoE 是由四個高需求小組所組成的函式。 您必須允許進行有機共同作業，並透過常見的儲存機制/解決方案目錄來追蹤成長。 最大化自然互動，但是將會議最小化。 當此函式成熟時，小組應該嘗試限制專用會議。 週期性會議（例如雲端採用小組所裝載的發行會議）的出席將提供資料輸入。 以平行方式，每個發行計畫之後的會議都可以為此小組提供最小的觸控點。

## <a name="solutions-and-controls"></a>解決方案和控制項

CCoE 的每個成員都是負責瞭解所需的條件約束、風險和防護，而這些條件約束會導致目前的 IT 控制群組合。 CCoE 的集體努力應該將這些理解轉換成雲端原生 (或混合式) 解決方案或控制項，以實現所需的自助業務成果。 當建立方案時，它們會以控制項或自動化流程的形式與各種小組共用，以做為各種工作的護欄。 這些護欄可協助您路由傳送各種團隊的自由流動活動，同時將責任委派給參與者，以進行各種不同的遷移或創新工作。

這項轉換的範例：

| 狀況 | 預先 CCoE 的解決方案 | CCoE 後解決方案 |
|---------|---------|---------|
| 布建生產 SQL Server | 網路、IT 和資料平臺小組會在幾天或甚至數周的時間內布建各種元件。 | 需要伺服器的小組會部署 Azure SQL Database 的 PaaS 實例。 或者，preapproved 範本可以用來將所有 IaaS 資產部署到雲端（以小時為單位）。 |
| 布建開發環境 | 網路、IT、開發和 DevOps 團隊同意規格和部署環境。 | 開發小組會定義自己的規格，並根據配置的預算部署環境。 |
| 更新安全性需求以改善資料保護 | 網路、IT 和安全性小組會在多個環境中更新各種網路裝置和 Vm，以新增保護。 | 雲端治理工具可用來更新可以立即套用至所有雲端環境中所有資產的原則。 |

## <a name="negotiations"></a>談判

任何 CCoE 工作的根目錄都是持續進行的協商流程。 CCoE 團隊會協調現有的 IT 功能，以減少集中控制。 這項談判的業務取捨是自由、靈活且速度。 現有 IT 團隊的取捨價值是以新解決方案的形式提供。 新的解決方案可為現有的 IT 團隊提供下列一或多項優點：

- 能夠將常見的問題自動化。
-  (減少日常挫折) 的一致性改善。
- 學習和部署新技術解決方案的機會。
- 減少高嚴重性事件 (較少的快速修正或晚于待命的傳呼回應) 。
- 能夠擴大其技術範圍，以解決更廣泛的主題。
- 參與更高層級的商務解決方案，以解決技術的影響。
- 減少起來更加無趣維護工作。
- 提高技術策略和自動化。

在 exchange 中有這些優點時，現有的 IT 功能可能會以下列值（不論是真實或認知）來表示交易：

- 從手動核准程式控制的意義。
- 變更控制的穩定性。
- 從完成必要但重複的工作，以瞭解作業安全性。
- 遵循現有 IT 解決方案廠商的一致性。

在良好的雲端轉送公司中，此協調程式是對等互連與合作 IT 團隊之間的動態交談。 技術詳細資料可能很複雜，但在瞭解目標並可協助 CCoE 工作時，可加以管理。 當它小於支援時，下一節的啟用 CCoE 成功有助於克服文化障礙。

## <a name="enable-ccoe-success"></a>啟用 CCoE 成功

繼續進行此模型之前，請務必考慮公司對於成長思維的承受度，並以釋出中央職責來緩和等級。 如先前所述，CCoE 的目的是要交換靈活性和速度的控制權。

這種類型的變更需要時間、實驗和協商。 在此成熟過程中，將會有一些凹凸和設定的支援。 但是，如果小組保持用心，而不是實驗的建議，則提高靈活性、速度和可靠性的機率就很高。 CCoE 成功或失敗的其中一個最大因素就是支援領導和重要的專案關係人。

### <a name="key-stakeholders"></a>重要的專案關係人

IT 領導力是第一個也是最明顯的專案關係人。 IT 管理員會扮演重要的部分。 但是在此程式期間，需要 CIO 和其他主管層級 IT 領導人的支援。

較不明顯的是商務專案關係人的需求。 商業彈性和上市時間是 CCoE 構成的關鍵動機。 因此，重要的專案關係人在這些領域中應該會有有意的興趣。 商務專案關係人的範例包括企業營運領導人、財務主管、營運主管和商務產品擁有者。

### <a name="business-stakeholder-support"></a>商務專案關係人支援

CCoE 工作可透過商務專案關係人的支援來加速。 CCoE 工作的大部分焦點都集中在針對企業靈活性和速度進行長期改進。 定義目前作業模型的影響以及改善的價值，對於 CCoE 的指南和協調工具很有價值。 針對 CCoE 支援，建議您記錄下列專案：

- 建立一組預期的業務成果和目標，作為企業靈活性和速度的結果。
- 明確定義目前 IT 程式所建立的難題， (例如速度、靈活性、穩定性和成本挑戰) 。
- 清楚地定義這些痛點的歷程記錄影響 (例如失去市場佔有率、功能和功能的競爭對手增益、不佳的客戶體驗，以及預算增加) 。
- 定義目前的痛點和營運模型所封鎖的業務改進商機。
- 建立與這些商機相關的時程表和計量。

這些資料點並不是攻擊。 相反地，它們可協助 CCoE 從過去學習，並建立實際的待處理專案並規劃改進。

**持續支援和參與：** CCoE 團隊可以示範一些區域中的快速回復。 但較高層級的目標（例如商務彈性和上市時間）可能需要更長的時間。 在成熟期間，可能會有很大的風險，也就是不建議 CCoE，也不會被拉出以專注于其他 IT 工作。

在 CCoE 工作的前六到九個月中，我們建議商務專案關係人配置時間，以符合 IT 領導和 CCoE 的每月。 這些會議不需要正式的繁瑣。 只要提醒 CCoE 成員及其對此計畫重要性的領導力，就可以推動 CCoE 的成功。

此外，我們建議商務專案關係人隨時掌握 CCoE 團隊所遭遇的進度和阻礙。 他們的許多努力看起來就像技術 minutiae。 但是，商務專案關係人必須瞭解計畫的進度，讓他們可以在團隊失去流時或因其他優先順序而產生注意力時進行互動。

### <a name="it-stakeholder-support"></a>IT 專案關係人支援

**支援願景：** 成功的 CCoE 工作需要與現有的 IT 小組成員進行大量的協商。 當工作順利完成時，它會對方案造成影響，並熟悉這項變更。 如果不是這種情況，現有 IT 小組的某些成員可能基於各種原因而想要保有控制機制。 IT 專案關係人的支援在發生這些情況時，對 CCoE 的成功至關重要。 鼓勵和增強式 CCoE 的整體目標對於將封鎖器解析為適當的協商而言很重要。 在罕見的情況下，IT 專案關係人甚至可能需要逐步執行並中斷鎖死或聯繫投票，以保持 CCoE 進度。

**維持焦點：** CCoE 對任何資源受限的 IT 團隊來說，都是相當重要的承諾。 從短期專案中移除強大的架構設計人員以專注于長期增益，對於不屬於 CCoE 的團隊成員而言可能會造成困難。 IT 領導階層和 IT 專案關係人必須專注于 CCoE 的目標。 IT 領導人和 IT 專案關係人的支援需要推遲日常作業的中斷，以利 CCoE 的職責。

**建立緩衝區：** CCoE 團隊將會試驗新的方法。 其中有些方法無法妥善配合現有的作業或技術限制。 當實驗失敗時，CCoE 發生壓力或要是時，會有一個真正的風險。 從「快速失敗」學習機會的結果中，鼓勵和緩衝處理小組是很重要的。 小組必須負責成長思維，以確保他們從這些實驗中學習，並找出更好的解決方案，也同樣重要。

## <a name="next-steps"></a>下一步

CCoE 模型需要雲端平臺功能和雲端自動化功能。 下一步是要調整雲端平臺功能。

深入了解：

- [雲端平台功能](./cloud-platform.md)
- [雲端自動化功能](./cloud-automation.md)
