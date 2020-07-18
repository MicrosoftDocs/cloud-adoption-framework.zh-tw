---
title: 適用于 Exadata 的分析解決方案
description: 使用適用于 Azure 的雲端採用架構，以瞭解使用 Exadata 的分析解決方案。
author: v-hanki
ms.author: brblanch
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 9f2812dcdc09e0ff0a5403003384faa997b185ca
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86451743"
---
<!-- cSpell:ignore Exadata SSMA -->

# <a name="analytics-solutions-for-exadata"></a>適用于 Exadata 的分析解決方案

## <a name="migrating-an-oracle-data-warehouse-schema-to-azure-synapse-analytics"></a>將 Oracle 資料倉儲架構遷移至 Azure Synapse 分析

就架構而言，Oracle 資料倉儲與 Azure Synapse 分析有幾種不同。 這包括資料庫、資料類型，以及 Azure Synapse Analytics 中不支援的各種 Oracle 資料庫物件類型。

就像其他資料庫管理系統一樣，將 Oracle 資料倉儲遷移至 Azure Synapse 時，您會發現 Oracle 中有多個不同的資料庫，而且在 Azure Synapse 中只有一個資料庫。 因此，可能需要使用新的命名慣例（例如，串連 Oracle 架構和資料表名稱），將 Oracle 資料倉儲臨時資料庫、生產資料庫和資料超市資料庫中的資料表和 views 移至 Azure Synapse。

此外，也有數個不支援的 Oracle 資料庫物件。 例如，不支援 Oracle 位對應索引、以函數為基礎的索引和網域索引。 Oracle 叢集資料表、資料列層級觸發程式、使用者定義資料類型和 PL/SQL 預存程式也是如此。 您可以藉由查詢各種 Oracle 系統目錄資料表和 views 來識別這些物件。 在幾種情況下，可能會發生因應措施。 例如，您可以在 Azure Synapse 中使用其他索引類型或資料分割來解決 Oracle 中不支援的索引類型。 您也可以使用具體化視圖，而不是 Oracle 叢集資料表，而 SQL Server 移轉小幫手 for Oracle 之類的遷移工具可轉譯至少一些 PL/SQL。

在遷移架構時，您必須考慮的資料行也會有資料類型差異。 您可以藉由查詢 Oracle 目錄，在您的 Oracle 資料倉儲和資料超市架構中尋找資料類型與 Azure Synapse 中資料類型沒有對應的資料行。 其中有數個實例的因應措施。

就維護或改善您的架構在遷移之後的效能，您應該記下目前已準備好的效能機制，例如 Oracle 索引。 例如，Oracle 中查詢經常使用的位對應索引，可能表示應該在 Azure Synapse 上已遷移的架構內建立非叢集索引。 Azure Synapse 上的良好實務包括使用資料散發來共置要聯結至相同處理節點的資料，以及確保聯結資料行的資料類型都相同，以便藉由減少轉換資料以進行比對的需求，來優化聯結處理。 使用 Azure Synapse 時，通常不需要遷移每個 Oracle 索引，因為其他功能是用來提供高效能。 可以改用平行查詢處理、記憶體內部資料和結果集快取，以及減少 i/o 的資料散發選項。

有數個工具可協助您將 Oracle 資料倉儲或資料超市遷移至 Azure Synapse。 這些工具組括適用于 Oracle 的 SQL Server 移轉小幫手（SSMA），其設計目的是為了自動從現有的 Oracle 環境中進行資料表、視圖和資料的遷移。 除了其他功能之外，SSMA 也建議目標 Azure Synapse 資料表的索引類型和資料散發，並在遷移期間套用資料類型對應。 雖然 SSMA 不是高容量資料的最有效率方法，但對於較小的資料表非常有用。

## <a name="next-steps"></a>後續步驟

<!-- TODO: Add actionable next step -->
