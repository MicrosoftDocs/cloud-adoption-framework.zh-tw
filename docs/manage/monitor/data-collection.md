---
title: 雲端監視指南–收集正確的資料
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 選擇何時使用 Azure 監視器或 System Center Operations Manager Microsoft Azure
author: MGoedtel
ms.author: magoedte
ms.date: 06/26/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: operate
services: azure-monitor
ms.openlocfilehash: 8b0aebe00d987ac49ba965fc1982c1615372ba92
ms.sourcegitcommit: 443c28f3afeedfbfe8b9980875a54afdbebd83a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2019
ms.locfileid: "71027107"
---
# <a name="cloud-monitoring-guide-collecting-the-right-data"></a>雲端監視指南：收集正確的資料

本文說明在雲端應用程式中收集監視資料的一些考慮。

若要觀察雲端解決方案的健全狀況和可用性, 您必須設定監視工具來收集以可預測的失敗狀態為基礎的信號層級。 這些信號是失敗的徵兆, 而不是原因。 監視工具會使用計量, 而在 advanced diagnostics 和根本原因分析的情況下, 也會使用記錄。

仔細規劃監視和遷移。 一開始請先在規劃階段包含監視服務擁有者、作業管理員和其他相關人員, 並繼續在整個開發和發行週期中參與。 其焦點將是根據下列準則來開發監視設定:

- 這項服務的組成為何, 以及現今受監視的相依性是什麼？ 若是如此, 是否有多項工具？ 是否有機會進行合併, 而不會產生風險？
- 服務的 SLA 為何, 以及我要如何測量和報告？
- 當事件引發時, 服務儀表板應該看起來像什麼？ 對於服務擁有者和支援服務的小組而言, 儀表板的外觀為何？
- 需要監視的資源會產生哪些計量？  
- 服務擁有者、支援小組和其他人員將如何搜尋記錄？

如何回答這些問題, 以及警示的準則, 可決定您將如何使用監視平臺。 如果您要從現有的監視平臺或一組監視工具進行遷移, 請使用「遷移」做為重新評估所收集信號的機會。 這特別適用于在遷移或整合雲端式監視平臺 (如 Azure 監視器) 時要考慮的幾個成本因素。 請記住, 監視資料必須是可採取動作的。 您必須已收集優化的資料, 為您提供服務整體健全狀況的「10000腳」觀點。 定義來識別真實事件的檢測應該盡可能簡單、可預測且可靠。

## <a name="develop-a-monitoring-configuration"></a>開發監視設定

監視服務擁有者和小組通常會遵循一組共同的活動來開發監視設定。 這些活動會從初始規劃階段開始，在非生產環境中繼續進行測試和驗證，並延伸至部署到生產環境。 監視設定衍生自已知的失敗模式、模擬失敗的測試結果, 以及組織中數人的經驗 (服務台、營運、工程師和開發人員)。 這類設定假設服務已存在、正在遷移至雲端, 而且尚未重新架構。

在開發過程中, 儘早監視這些服務的健全狀況和可用性, 以取得服務層級的品質結果。 如果您事後監視該服務或應用程式的設計, 您的結果將不會成功。

若要提高事件的解決速度, 請考慮下列建議:

- 為每個服務元件定義儀表板。
- 使用計量來協助進一步診斷, 以在無法發現根本原因時, 找出問題的解決方法或因應措施。
- 使用 [儀表板] 向下切入功能, 或支援自訂視圖來精簡。
- 如果您需要詳細資訊記錄, 計量應已協助將目標設為搜尋準則。 如果不是, 請改善下一個事件的計量。

採用這組指導原則可提供您近乎即時的深入解析, 並更妥善管理您的服務。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [警示策略](./alerting.md)
