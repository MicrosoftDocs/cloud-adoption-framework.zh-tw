---
title: 使用數位家發明將大眾化資料
description: 深入瞭解 democratization，這是將資料帶入適當手中以測試假設並推動創新的程式。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: 2fac366e56e279204a791d5d8813500fe57de8d6
ms.sourcegitcommit: afe10f97fc0e0402a881fdfa55dadebd3aca75ab
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/31/2020
ms.locfileid: "80433317"
---
# <a name="democratize-data"></a>將資料大眾化

在產業革命期間，煤力、石油和人力都是三個最具衍生性的資產。 這些資產內建的公司、移動的市場，最後改變了國家/地區。 在數位經濟中，有三個同樣重要的資產：資料、裝置和人為潛能。 每個資產都有絕佳的創新潛力。 針對現代化時代的任何創新工作，資料是新的石油。

現今每一家公司都有隨身的資料，可讓您更有效地尋找及滿足客戶的需求。 可惜的是，將該資料進行挖掘以推動創新的程式，既耗費昂貴又耗時。 許多最有價值的解決方案可滿足客戶需求，因為適當的人員無法存取所需的資料。

資料的 Democratization 是將這項資料帶入適當手中以推動創新的過程。 此程式可以採用數種形式，但它們通常包含內嵌或整合式原始資料、資料集中化、共用資料及保護資料的解決方案。 當這些方法成功時，公司周圍的專家就可以使用資料來測試假設。 在許多情況下，雲端採用小組可以只使用資料來建立，並快速解決現有[的](./build.md)客戶需求。

## <a name="process-of-democratizing-data"></a>Democratizing 資料的處理常式

下列階段將引導您採用促進大眾化資料的解決方案所需的決策和方法。 建立特定的解決方案不一定需要每個階段。 不過，當您為客戶假設建立解決方案時，您應該評估每個階段。 每個都提供獨特的方法來建立創新的解決方案。

![Democratizing 資料的處理常式](../../_images/innovate/democratize-data.png)

### <a name="share-data"></a>共用資料

當您[以客戶的理解方式建立](./build.md)時，所有程式都能提升客戶對技術解決方案的需求。 由於 democratizing 資料不例外，我們會從共用資料開始。 若要將大眾化資料，它必須包含與資料取用者共用資料的解決方案。 資料取用者可以是直接客戶或為客戶做出決策的 proxy。 核准的資料取用者可以分析、查閱和報告集中式資料，而不支援 IT 人員。

許多成功的創新已啟動為最基本的可行產品（MVP），可代表客戶提供手動、資料驅動的流程。 在此指引模型中，員工是資料取用者。 該員工會使用資料來協助客戶。 每次客戶參與手動支援時，就可以測試並驗證假設。 這種方法通常是一種符合成本效益的方法，可在您投入大量整合解決方案之前，先測試以客戶為焦點的假設。

直接與資料取用者共用資料的主要工具組括自助式報告或內嵌在其他體驗中的資料，使用[Power BI](https://docs.microsoft.com/power-bi)之類的工具。

> [!NOTE]
> 在您共用資料之前，請先確定您已閱讀下列各節。 共用資料可能需要治理來提供共用資料的保護。 此外，該資料可能會散佈到多個雲端，而且可能需要集中化。 大部分的資料甚至可能位於應用程式內，這需要先收集資料，然後才能共用。

### <a name="govern-data"></a>管理資料

共用資料可以快速產生您可以在客戶交談中使用的 MVP。 不過，若要將共用資料轉換成有用且可操作的知識，通常需要更多的。 透過資料共用來驗證假設之後，下一個階段的開發通常是資料管理。

資料控管是很廣泛的主題，可能需要它自己的專屬架構。 該程度的資料細微性超出[雲端採用架構](../../index.md)的範圍。 不過，有幾個層面的資料管理，您應該在客戶假設通過驗證後立即考慮。 例如，

- **共用資料是否區分大小寫？** [資料應](../../govern/policy-compliance/data-classification.md)在公開共用之前進行分類，以保護客戶和公司的興趣。
- **如果資料很敏感，是否受到保護？** 機密資料的保護應為任何大眾化資料的需求。 著重于[保護資料解決方案](https://docs.microsoft.com/azure/architecture/data-guide/scenarios/securing-data-solutions)的範例工作負載會提供幾個用於保護資料的參考。
- **資料是否已編目？** 若要深入瞭解所共用的資料，將有助於進行長期的資料管理。 記錄資料的工具（例如 Azure 資料目錄）可以讓此程式在雲端中更容易進行。 有關資料和[資料來源之](https://docs.microsoft.com/azure/data-catalog/data-catalog-how-to-documentation)[批註](https://docs.microsoft.com/azure/data-catalog/data-catalog-how-to-annotate)的指引可協助加速此流程。

當 democratization 資料對客戶為焦點的假設很重要時，請確定共用資料的治理是在發行計畫中的某處。 這將有助於保護客戶、資料取用者和公司。

### <a name="centralize-data"></a>集中資料

當資料在 IT 環境中中斷時，創新的機會會非常受限、昂貴且耗時。 雲端提供新的機會，將資料集中在資料定址接收器。 當需要集中多個資料來源時，必須以[客戶的理解來建立](./build.md)，雲端可以加速假設的測試。

> [!CAUTION]
> 資料的集中化代表任何創新程式中的風險重點。 當資料集中化是[技術尖峰](./build.md#reduce-complexity-and-delay-technical-spikes)（而不是客戶價值的來源）時，建議您延遲集中化，直到客戶假設通過驗證為止。

如果需要集中化資料，您應該先為集中式資料定義適當的資料存放區。 在雲端中建立資料倉儲是很好的作法。 這個可調整的選項會為您的所有資料提供一個集中的位置。 這種類型的解決方案適用于線上分析處理（OLAP）或海量資料選項。

[OLAP](https://docs.microsoft.com/azure/architecture/data-guide/relational-data/online-analytical-processing)和[Big Data](https://docs.microsoft.com/azure/architecture/data-guide/big-data)解決方案的參考架構可協助您選擇 Azure 中最相關的解決方案。 如果需要混合式解決方案，[擴充內部部署資料](https://docs.microsoft.com/azure/architecture/data-guide/scenarios/hybrid-on-premises-and-cloud)的參考架構也可以協助加速解決方案開發。

> [!IMPORTANT]
> 視客戶的需求和對齊的解決方案而定，較簡單的方法可能就已足夠。 雲端架構設計人員應該會挑戰小組考慮較低的成本解決方案，更快速地驗證客戶假設，特別是在早期開發期間。 下一節的收集資料涵蓋一些可能針對您的情況建議不同解決方案的案例。

### <a name="collect-data"></a>收集資料

當您需要集中資料以滿足客戶的需求時，您可能也必須從各種來源收集資料，並將其移至集中式資料存放區。 資料收集有兩種主要形式：*整合* *和內嵌*。

**整合：** 位於現有資料存放區中的資料可以使用傳統資料移動技術，整合到集中式資料存放區。 這在牽涉到多重雲端資料存放區的案例中特別常見。 這些技術牽涉到從現有的資料存放區解壓縮資料，然後將其載入中央資料存放區。 在此程式中的某個時間點，通常會將資料轉換成更容易使用且與中央存放區相關。

以雲端為基礎的工具已將這些技巧轉換成按使用量付費的工具，以降低進入資料收集和集中化的屏障。 Azure 資料庫移轉服務和 Azure Data Factory 之類的工具都是兩個範例。 [Data factory 與 OLAP 資料存放區](https://docs.microsoft.com/azure/architecture/data-guide/relational-data/etl)的參考架構是其中一個這類解決方案的範例。

內嵌 **：** 有些資料不在現有的資料存放區中。 當此暫時性資料是創新的主要來源時，您會想要考慮其他方法。 暫時性資料可以在各種現有的來源中找到，例如應用程式、Api、資料流程、IoT 裝置、區塊鏈、應用程式快取、媒體內容，甚至一般檔案。

您可以將這些各種形式的資料整合到 OLAP 或 Big Data 解決方案的中央資料存放區。 不過，針對組建–測量–學習週期的早期反復專案，線上交易處理（OLTP）解決方案可能會比驗證客戶假設還多。 OLTP 解決方案不是任何報告案例的最高品質解決方案。 不過，當您以[客戶的理解方式建立](./build.md)時，將焦點放在客戶的需求上會比技術工具決策更重要。 客戶假設是大規模驗證之後，可能需要更適合的平臺。 [OLTP 資料存放區](https://docs.microsoft.com/azure/architecture/data-guide/relational-data/online-transaction-processing)的參考架構可協助您判斷哪一個資料存放區最適合您的解決方案。

**虛擬化：** 資料的整合和內嵌有時可能會使創新變慢。 當資料虛擬化的解決方案已可供使用時，它可能代表更合理的方法。 內嵌與整合可以重複儲存和開發需求、新增資料延遲、增加受攻擊面區域、觸發品質問題，以及增加治理工作。 資料虛擬化是比較現代的替代方案，可將原始資料保留在單一位置，並建立來源資料的傳遞或快取查詢。

SQL Server 2017 和 Azure SQL 資料倉儲都支援[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) ，這是 Azure 中最常使用的資料虛擬化方法。

## <a name="next-steps"></a>後續步驟

有了 democratizing 資料的策略之後，接下來您會想要評估[透過應用程式吸引客戶](./apps.md)的方法。

> [!div class="nextstepaction"]
> [透過應用程式吸引客戶](./apps.md)
