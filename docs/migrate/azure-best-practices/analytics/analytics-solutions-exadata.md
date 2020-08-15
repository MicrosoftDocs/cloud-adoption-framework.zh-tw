---
title: 適用于 Oracle 資料倉儲的 Azure Synapse 分析遷移
description: 使用適用于 Azure 的雲端採用架構，以瞭解如何將 Oracle 資料倉儲架構遷移至 Azure Synapse 分析。
author: v-hanki
ms.author: brblanch
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: fe56ceb769c7177022db223136bce032bc2135f8
ms.sourcegitcommit: 76edf563a08ff7dc81c3fc2dc6c8972ab3b4c55b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2020
ms.locfileid: "88237078"
---
<!-- cSpell:ignore Exadata SSMA -->

# <a name="azure-synapse-analytics-solutions-and-migration-for-an-oracle-data-warehouse"></a>Oracle 資料倉儲的 Azure Synapse 分析解決方案和遷移

Oracle 資料倉儲架構與 Azure Synapse 分析有幾種不同。 其中的差異包括資料庫、資料類型，以及 Azure Synapse 中不支援的 Oracle 資料庫物件類型範圍。

就像其他資料庫管理系統一樣，當您將 Oracle 資料倉儲遷移至 Azure Synapse 時，您會發現 Oracle 有多個不同的資料庫，而 Azure Synapse 只有一個資料庫。 您可能需要使用新的命名慣例（例如，串連 Oracle 架構和資料表名稱），將 Oracle 資料倉儲臨時資料庫、生產資料庫和資料超市資料庫中的資料表和 views 移至 Azure Synapse。

Azure Synapse 不支援數個 Oracle 資料庫物件。 Azure Synapse 中不支援的資料庫物件包括 Oracle 位對應索引、以函式為基礎的索引、網域索引、Oracle 叢集資料表、資料列層級觸發程式、使用者定義資料類型，以及 PL/SQL 預存程式。 您可以藉由查詢各種 Oracle 系統目錄資料表和 views 來識別這些物件。 在某些情況下，您可以使用因應措施。 例如，您可以使用 Azure Synapse 中的資料分割或其他索引類型來解決 Oracle 中不支援的索引類型。 您可以使用具體化視圖，而不是 Oracle 叢集資料表，而 SQL Server 移轉小幫手 (SSMA) 之類的遷移工具可以至少轉譯一些 PL/SQL。

當您遷移 Oracle 資料倉儲架構時，您也必須考慮資料行的資料類型差異。 若要在您的 Oracle 資料倉儲和資料超市架構中尋找資料類型未對應至 Azure Synapse 中資料類型的資料行，請查詢 Oracle 目錄。 您可以使用其中幾個實例的因應措施。

若要在遷移之後維護或改善架構的效能，請考慮您目前已準備好的效能機制（例如 Oracle 索引）。 例如，Oracle 查詢經常使用的位對應索引，可能表示在 Azure Synapse 上遷移的架構中建立非叢集索引是有利的。 

Azure Synapse 中的一個良好作法包括使用資料散發來共置要聯結至相同處理節點的資料。 Azure Synapse 中另一個不錯的作法，是確保要聯結的資料行類型都相同。 使用相同聯結的資料行可讓您藉由減少轉換資料以進行比對的需求，來優化聯結處理。 在 Azure Synapse 中，您通常不需要遷移每個 Oracle 索引，因為其他功能會提供高效能。 您可以改用平行查詢處理、記憶體內部資料和結果集快取，以及降低 i/o 的資料散發選項。

SSMA for Oracle 可以協助您將 Oracle 資料倉儲或資料超市遷移至 Azure Synapse。 SSMA 的設計目的是要自動化從現有 Oracle 環境中遷移資料表、視圖和資料的程式。 除了其他功能之外，SSMA 也建議目標 Azure Synapse 資料表的索引類型和資料散發，並在遷移期間套用資料類型對應。 雖然 SSMA 對於非常大量的資料並不是最有效率的方法，但對於較小的資料表非常有用。

