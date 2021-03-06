---
title: 大型主機遷移誤解和事實
description: 瞭解如何區別關於大型主機的相關誤解，並評估最適合 Azure 的大型主機工作負載。
author: njray
ms.author: brblanch
ms.date: 12/27/2018
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: think-tank
ms.openlocfilehash: 5c0a226143f62a08046382d59a58d4b34ca8282c
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102115028"
---
<!-- cSpell:ignore chargebacks IPLs -->

# <a name="mainframe-myths-and-facts"></a>大型主機迷思與事實

大型主機在計算歷史中特別突出且重要，而且對非常特定的工作負載持續保持可行。 多數人同意大型主機是經過實證的平台，具有悠久的作業程序，造就其可靠且穩固的環境。 軟體會根據使用量來執行，並以每秒百萬個指令 (MIPS) ，而廣泛的使用報告可供退款。

大型主機的可靠性、可用性和處理能力佔據迷思大部分的比例。 為了評估最適合 Azure 的大型主機工作負載，您會想要先區別迷思與現實。

## <a name="myth-mainframes-never-go-down-and-have-a-minimum-of-five-9s-of-availability"></a>迷思：大型主機永遠不會中斷，而且至少有五個9的可用性

大型主機硬體和作業系統被視為可靠又穩定。 但實際的情況是，停機時間必須排程進行維護和重新開機，這稱為初始程式負載 (Ipl) 。 當考慮到這些工作時，大型主機解決方案的可用性通常更貼近兩個或三個 9，這等同於高階的 Intel 型伺服器。

大型主機也與任何其他伺服器一樣易受災害損害，因此需要不斷電供應系統 (UPS) 來處理這些類故障。

## <a name="myth-mainframes-have-limitless-scalability"></a>迷思：大型主機沒有無限的擴充性

大型主機的擴充性取決於其系統軟體的容量，例如客戶資訊控制系統 (CICS) ，以及大型主機引擎和儲存體的新實例的容量。 某些使用大型主機的大型公司已針對效能自訂其 CICS，並在其他方面已超越最大型可取得主機的功能。

## <a name="myth-intel-based-servers-are-not-as-powerful-as-mainframes"></a>迷思： Intel 伺服器與大型主機沒有強大的功能

新的核心密集 Intel 系統的計算容量與大型主機一樣。

## <a name="myth-the-cloud-cant-accommodate-mission-critical-applications-for-large-companies-such-as-financial-institutions"></a>迷思：雲端無法容納大型公司（例如金融機構）的任務關鍵性應用程式

雖然可能有一些雲端解決方案短暫的隔離實例，但通常是因為無法散發應用程式演算法。 這些範例是例外狀況，而不是規則。

## <a name="summary"></a>摘要

相較之下，Azure 提供的替代平臺可以傳遞相等的大型主機功能和功能，且成本較低。 此外，在雲端以訂用帳戶為基礎的訂用帳戶型成本模型中，擁有權 (TCO) 的擁有權總成本，遠低於大型主機電腦的成本。

## <a name="next-steps"></a>下一步

> [!div class="nextstepaction"]
> [將從大型主機切換至 Azure](./migration-strategies.md)
