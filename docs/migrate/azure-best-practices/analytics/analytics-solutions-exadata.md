---
title: Oracle 資料倉儲的 Azure Synapse Analytics 遷移
description: 使用適用于 Azure 的雲端採用架構，瞭解如何將 Oracle 資料倉儲架構遷移至 Azure Synapse Analytics。
author: v-hanki
ms.author: brblanch
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 6498aa455b196a8648902f42c22131c146ef48d6
ms.sourcegitcommit: c1d6c1c777475f92a3f8be6def84f1779648a55c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/21/2020
ms.locfileid: "92334845"
---
<!-- cSpell:ignore Exadata SSMA -->

# <a name="azure-synapse-analytics-solutions-and-migration-for-an-oracle-data-warehouse"></a>Azure Synapse Analytics Oracle 資料倉儲的解決方案和遷移

Oracle 資料倉儲架構不同于 Azure Synapse Analytics 的方式有好幾種。 這些差異包括資料庫、資料類型，以及 Azure Synapse 中不支援的 Oracle Database 物件類型範圍。

就像其他資料庫管理系統一樣，當您將 Oracle 資料倉儲遷移至 Azure Synapse 時，您會發現 Oracle 具有多個個別的資料庫，而 Azure Synapse 只有一個資料庫。 您可能需要使用新的命名慣例（例如，串連 Oracle 架構和資料表名稱），將 Oracle 資料倉儲臨時資料庫、生產資料庫和資料超市資料庫中的資料表和觀點移至 Azure Synapse。

Azure Synapse 不支援數個 Oracle Database 物件。 Azure Synapse 中不支援的資料庫物件包括 Oracle 位對應索引、以函式為基礎的索引、定義域索引、Oracle 叢集資料表、資料列層級觸發程式、使用者定義資料類型，以及 PL/SQL 預存程式。 您可以藉由查詢各種 Oracle 系統目錄資料表和 views 來識別這些物件。 在某些情況下，您可以使用因應措施。 例如，您可以使用 Azure Synapse 中的資料分割或其他索引類型，來解決 Oracle 中不支援的索引類型。 您可以使用具體化視圖（而不是 Oracle 叢集資料表），而 Oracle 的 SQL Server 移轉小幫手 (SSMA) 等遷移工具，至少可以轉譯一些 PL/SQL。

當您遷移 Oracle 資料倉儲架構時，您也必須考慮資料行的資料類型差異。 若要在您的 Oracle 資料倉儲和資料超市架構中尋找資料類型不會對應至 Azure Synapse 中資料類型的資料行，請查詢 Oracle 目錄。 您可以針對這些實例的數個使用因應措施。

若要在遷移後維護或改進架構的效能，請考慮您目前已有的效能機制，例如 Oracle 索引。 例如，Oracle 查詢經常使用的位對應索引，可能表示在 Azure Synapse 上遷移的架構中建立非叢集索引會很有利。

Azure Synapse 中的一個良好做法，包括使用資料散發來共置要聯結至相同處理節點的資料。 Azure Synapse 中的另一個良好做法，是確保要聯結的資料行資料類型相同。 使用相同的聯結資料行，可減少轉換資料以進行比對的需求，以優化聯結處理。 在 Azure Synapse 中，通常不需要遷移每個 Oracle 索引，因為其他功能可提供高效能。 您可以改為使用平行查詢處理、記憶體內部資料，以及可減少 i/o 的結果集快取和資料分佈選項。

適用于 Oracle 的 SSMA 可協助您將 Oracle 資料倉儲或資料超市遷移至 Azure Synapse。 SSMA 的設計目的是要將從現有 Oracle 環境遷移資料表、視圖及資料的流程自動化。 除了其他功能之外，SSMA 還建議目標 Azure Synapse 資料表的索引類型和資料散發，並在遷移期間套用資料類型對應。 雖然對於非常大量的資料而言，SSMA 不是最有效率的方法，但對於較小的資料表很有用。
