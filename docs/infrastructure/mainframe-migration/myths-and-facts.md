---
title: 大型主機遷移：誤解和事實
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 將應用程式從大型主機環境遷移至 Azure，這是經過實證、高度可用且可調整的基礎結構，適用於目前在大型主機上執行的系統。
author: njray
ms.author: v-nanra
ms.date: 12/27/2018
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 4adf229b1ffca1d1360d197ab09a04f0d9584ef8
ms.sourcegitcommit: 35c162d2d09ec1c4a57d3d57a5db1d56ee883806
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2019
ms.locfileid: "72547912"
---
# <a name="mainframe-myths-and-facts"></a>大型主機迷思與事實

大型主機在計算歷史中特別突出且重要，而且對非常特定的工作負載持續保持可行。 多數人同意大型主機是經過實證的平台，具有悠久的作業程序，造就其可靠且穩固的環境。 軟體會根據使用量來執行，以每秒百萬個指令（MIPS）來測量，而廣泛的使用量報表則可供退款。

大型主機的可靠性、可用性和處理能力佔據迷思大部分的比例。 為了評估最適合 Azure 的大型主機工作負載，您會想要先區別迷思與現實。

## <a name="myth-mainframes-never-go-down-and-have-a-minimum-of-five-9s-of-availability"></a>迷思：大型主機永遠不會停機，而且至少有五個9的可用性

大型主機硬體和作業系統被視為可靠又穩定。 但事實上為了維護及重新開機 (稱為初始程式載入或 IPL)，還是必須排程停機時間。 當考慮到這些工作時，大型主機解決方案的可用性通常更貼近兩個或三個 9，這等同於高階的 Intel 型伺服器。

大型主機也與任何其他伺服器一樣易受災害損害，因此需要不斷電供應系統 (UPS) 來處理這些類故障。

## <a name="myth-mainframes-have-limitless-scalability"></a>迷思：大型主機具有無限的擴充性

大型主機的擴充性取決於其系統軟體的容量，例如客戶資訊控制系統（CICS），以及大型主機引擎和儲存體的新實例容量。 某些使用大型主機的大型公司已針對效能自訂其 CICS，並在其他方面已超越最大型可取得主機的功能。

## <a name="myth-intel-based-servers-are-not-as-powerful-as-mainframes"></a>迷思： Intel 伺服器的功能不如大型主機

新的核心密集 Intel 系統的計算容量與大型主機一樣。

## <a name="myth-the-cloud-cant-accommodate-mission-critical-applications-for-large-companies-such-as-financial-institutions"></a>迷思：雲端無法容納金融機構等大型公司的任務關鍵性應用程式

雖然可能有一些隔離的實例，其中的雲端解決方案很短，但通常是因為無法散發應用程式演算法。 這些範例是例外狀況，而不是規則。

## <a name="summary"></a>總結

相較之下，Azure 提供的替代平臺能夠傳遞對等的大型主機功能和功能，而且成本更低。 此外，雲端以訂閱為基礎之訂用帳戶的擁有權總成本（TCO）與大型主機的成本模型相比，價格遠低於主機電腦。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [將從大型主機切換至 Azure](./migration-strategies.md)
