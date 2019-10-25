---
title: 雲端創新：將大眾化資料
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 雲端創新簡介-將大眾化資料
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: 80cd8c74f283436d9f32ce647c27bb7c67d3c92c
ms.sourcegitcommit: 15898374495761bfb76cee719e0f9189856884e6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2019
ms.locfileid: "72888899"
---
# <a name="democratize-data"></a>將大眾化資料

煤力、石油和人力可能是產業革命中三個最具衍生性的資產。 這些資產內建的公司、移動的市場，最後改變了國家/地區。 在數位經濟中，有三個同樣重要的資產：資料、裝置和人為潛能。 其中每個資產都有絕佳的創新潛力。 在任何創新的工作中，資料是新的石油。

現今的每一家公司都有隨身的資料，可讓您更快速地尋找及滿足客戶需求。 可惜的是，將該資料進行挖掘以推動創新的程式，既耗費昂貴又耗時。 許多最有價值的解決方案可滿足客戶需求，因為適當的人員無法存取所需的資料。

資料的 Democratization 是將這項資料帶入適當手中以推動創新的過程。 此程式可能需要數個圖形，但通常包含內嵌或整合式原始資料的解決方案、資料的集中化、共用資料，以及保護資料安全。 成功時，公司周圍的專家就可以利用資料來測試假設。 在許多情況下，雲端採用小組可以只使用資料來[建立客戶的理解](./build.md)，進而對現有的客戶需求產生快速的影響。

## <a name="process-of-democratizing-data"></a>Democratizing 資料的處理常式

下列階段將引導您採用促進大眾化資料的解決方案所需的決策和方法。 每個階段都可能不需要建立特定的解決方案。 不過，針對客戶假設建立解決方案時，應該評估每個。 每個都提供獨特的方法來建立創新的解決方案。

![Democratizing 資料的處理常式](../../_images/innovate/democratize-data.png)

### <a name="share-data"></a>共用資料

[隨著客戶的理解](./build.md)，所有流程都專注于客戶需要的技術解決方案。 Democratizing 資料不例外，所以我們從共用資料開始。 若要將大眾化資料，它必須包含與資料取用者共用資料的解決方案。 資料的取用者可以是直接客戶，或為客戶做出決策的 proxy。 已核准的資料取用者能夠分析、查閱和報告集中式資料，而不需要 IT 人員的支援。

許多成功的創新都是以 Mvp 的形式啟動，這代表客戶提供手動、資料驅動的流程。 在此指引模型中，員工是資料取用者。 該員工會使用資料來協助客戶。 每次客戶參與手動支援時，都可以測試並驗證假設。 這種方法通常是一種符合成本效益的方法，可在大量投資整合式解決方案之前，先測試以客戶為焦點的假設。

直接與資料取用者共用資料的主要工具組括自助式報告，或內嵌在其他體驗中的資料，使用[Power BI](https://docs.microsoft.com/power-bi)之類的工具。

> [!NOTE]
> 共用資料之前，請務必留意下列各節。 共用資料可能需要治理以提供共用資料的保護。 此外，資料可能會分散到多個雲端，而且可能需要集中化。 大部分的資料甚至可能位於應用程式內，這需要在共用資料之前進行收集。

### <a name="govern-data"></a>管理資料

共用資料可以快速產生可用於客戶交談的 MVP。 不過，資料本身只是資料。 若要將共用資料轉換成可操作的知識，通常需要更多。 一旦假設通過資料共用驗證後，下一個階段的開發通常是資料管理。

資料控管是很廣泛的主題，可能需要它自己的專屬架構。 這不在[雲端採用架構](../../index.md)的範圍內。 不過，有幾個層面的資料控管，只要一開始就會驗證客戶假設。 以下是這些問題的幾個範例：

- **共用資料是否區分大小寫？** [資料應該在](../../govern/policy-compliance/data-classification.md)任何公用共用之前分類，以保護客戶和公司的興趣。
- **如果資料很敏感，是否受到保護？** 機密資料的保護應為任何大眾化資料的需求。 著重于[保護資料解決方案](https://docs.microsoft.com/azure/architecture/data-guide/scenarios/securing-data-solutions)的範例工作負載會提供幾個用於保護資料的參考。
- **資料是否已編目？** 若要深入瞭解所共用的資料，將有助於進行長期的資料管理。 記錄資料的工具（例如 Azure 資料目錄）可以讓此程式在雲端中更容易進行。 有關資料和[資料來源之](https://docs.microsoft.com/azure/data-catalog/data-catalog-how-to-documentation)[批註](https://docs.microsoft.com/azure/data-catalog/data-catalog-how-to-annotate)的指引可協助加速此流程。

當 democratization 資料對客戶為重點的假設很重要時，共用資料的治理應位於發行計畫的某處，以保護客戶、資料取用者和公司。

### <a name="centralize-data"></a>集中資料

當資料在 IT 環境中中斷時，創新的機會會非常受限、昂貴且耗時。 雲端提供新的機會，將資料集中在各種不同的資料定址接收器。 當需要集中多個資料來源時，以[客戶的方式來打造](./build.md)雲端可以加速假設的測試。

> [!CAUTION]
> 資料的集中化是任何創新程式中常見的風險重點。 當資料集中化是[技術尖峰](./build.md#reduce-complexity-and-delay-technical-spikes)，而不是客戶價值的來源時，建議您延遲集中化，直到客戶假設已通過驗證。

如果需要集中化資料，第一個步驟是為集中式資料定義適當的資料存放區。 建議的方法是在雲端建立資料倉儲。 這個可調整的選項會提供所有資料的中央位置。 這種類型的解決方案適用于線上分析處理（OLAP）或海量資料選項。

[OLAP](https://docs.microsoft.com/azure/architecture/data-guide/relational-data/online-analytical-processing)和[Big Data](https://docs.microsoft.com/azure/architecture/data-guide/big-data)解決方案的參考架構可協助您選擇 Azure 中最相關的解決方案。 如果需要混合式解決方案，[擴充內部部署資料](https://docs.microsoft.com/azure/architecture/data-guide/scenarios/hybrid-on-premises-and-cloud)的參考架構也可能有助於加速解決方案開發。

> [!IMPORTANT]
> 根據客戶需求和一致的解決方案，較簡單的解決方案可能就已足夠。 雲端架構設計人員應該會挑戰小組考慮較低的成本解決方案，更快速地驗證客戶假設，特別是在早期開發期間。 收集下列資料一節將概述一些可能會提示您提供不同解決方案的案例。

### <a name="collect-data"></a>收集資料

當需要集中資料以提供客戶需求時，很可能也需要從各種來源收集資料，並將其移至集中式資料存放區。 資料收集有兩種主要形式：整合和內嵌。 每個都提供不同的方法來收集資料。

**整合：** 位於現有資料存放區中的現有資料，可以使用傳統資料移動技術整合到集中式資料存放區。 這在牽涉到多重雲端資料存放區的案例中特別常見。 這些技術包含從現有的資料存放區解壓縮資料。 然後將資料載入中央資料存放區。 在此程式中的某個時間點，資料通常會轉換成更多可用且與中央存放區相關。

以雲端為基礎的工具將這些技巧轉換成按使用次數付費的工具，以降低進入資料收集和集中化的屏障。 資料移轉服務和 Data Factory 之類的工具是 Azure 中的兩個常見範例。 [Data factory 與 OLAP 資料存放區](https://docs.microsoft.com/azure/architecture/data-guide/relational-data/etl)的參考架構會提供這類解決方案的範例。

內嵌 **：** 有些資料不在任何現有的資料存放區中。 當此暫時性資料是創新的主要來源時，應該考慮其他方法。 暫時性資料可以在各種現有的來源中找到，例如應用程式、Api、資料流程、IoT 裝置、區塊鏈、應用程式快取、媒體內容，甚至一般檔案。

這些各種形式的資料可以整合到 OLAP 或 Big Data 解決方案上的中央資料存放區。 不過，在組建-測量-學習週期的早期反復專案中，線上交易處理（OLTP）解決方案可能會比驗證客戶假設還多。 OLTP 解決方案不是任何報告案例的最高品質解決方案。 不過，[以客戶為](./build.md)依據，將焦點放在客戶的需求時，優先于技術工具決策。 一旦客戶假設是大規模驗證，則可能需要更適合的平臺。 [OLTP 資料存放區](https://docs.microsoft.com/azure/architecture/data-guide/relational-data/online-transaction-processing)的參考架構有助於判斷哪一個資料存放區最適合您的解決方案。

**虛擬化：** 資料的整合和內嵌有時可能會使創新變慢。 當資料虛擬化的解決方案已可供使用時，這可能是更相關的方法。 內嵌與整合可以重複儲存/開發需求、新增資料延遲、增加受攻擊面區域、插入品質問題，以及增加治理工作。 資料虛擬化是更現代化的替代方案，可將原始資料保留在單一位置，並建立來源資料的傳遞或快取查詢。

SQL Server 2017 和 Azure SQL 資料倉儲都支援[PolyBase](/sql/relational-databases/polybase/polybase-guide) ，這是 Azure 中最常使用的資料虛擬化方法。

## <a name="next-steps"></a>後續步驟

使用 democratizing 資料的策略時，下一步是評估[透過應用程式吸引客戶](./apps.md)的方法。

> [!div class="nextstepaction"]
> [透過應用程式吸引客戶](./apps.md)
