---
title: 資料創新
description: 遷移和現代化您的資料倉儲，並擴充您的分析功能來推動新的商業價值。
author: v-hanki
ms.author: brblanch
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: strategy
ms.openlocfilehash: 3a12bf2ff26ff28ff7c561badba702bdc064e23d
ms.sourcegitcommit: 57b757759b676a22f13311640b8856557df36581
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2020
ms.locfileid: "94996939"
---
# <a name="data-innovations"></a>資料創新

許多公司都想要將其現有的資料倉儲遷移至雲端。 這些因素有許多因素，包括：

- 沒有硬體可購買或維護成本。
- 沒有可管理的基礎結構。
- 能夠切換到安全、可調整且低成本的雲端解決方案。

例如，來自 Azure 的雲端原生、隨用隨付服務稱為 Azure Synapse Analytics，可為組織提供分析資料庫管理系統。 Azure 技術可協助您在資料倉儲遷移後將其現代化，並擴充您的分析功能來推動新的商業價值。

資料倉儲遷移專案涉及許多元件。 這些包括架構、資料、解壓縮-轉換-載入 (ETL) 管線、授權許可權、使用者、BI 工具語義存取層，以及分析應用程式。

將資料倉儲遷移至 Azure Synapse Analytics 之後，您就可以利用 Microsoft 分析生態系統中的其他技術。 這樣做不僅可讓您將資料倉儲現代化，還能將在 Azure 上的其他分析資料存放區中所產生的見解結合在一起。

您可以放寬 ETL 處理，將任何類型的資料內嵌至 Azure Data Lake Storage。 您可以使用 Azure Data Factory，以大規模進行準備和整合。 這會產生可供您的資料倉儲取用，而且也可供資料科學家和其他應用程式存取的受信任、經常瞭解的資料資產。 您可以建立即時、批次導向的分析管線。 您也可以建立機器學習模型，以便在串流資料和視需要時即時部署到批次中執行。

此外，您還可以使用 PolyBase 超越您的資料倉儲。 這可簡化在 Azure 上的多個基礎分析平臺中產生之見解的存取權。 您可以在邏輯資料倉儲中建立全面的整合式視圖，以從 BI 工具和應用程式存取串流、big data 和傳統資料倉儲見解。

許多公司都有多年的資料倉儲在資料中心內執行，讓使用者能夠產生商務智慧。 資料倉儲會將資料從已知的交易系統解壓縮、暫存資料，然後進行清除、轉換及整合，以填入資料倉儲。

使用案例、商務案例和技術進展都可支援 Azure Synapse Analytics 如何協助您進行資料倉儲遷移。 下列各節列出許多範例。

## <a name="use-cases"></a>使用案例

- 聯網產品創新
- 未來的工廠
- 臨床分析
- 合規性分析
- 以成本為基礎的分析
- 全通道優化
- 個人化
- 智慧供應鏈
- 動態定價
- 採購分析
- 數位控制塔
- 風險管理
- 客戶分析
- 詐騙偵測
- 宣告分析

## <a name="business-cases"></a>商務案例

- 使用單一分析服務來建立端對端分析解決方案。
- 使用 Azure Synapse Analytics studio，其為數據準備、資料管理、資料倉儲、big data 和 AI 工作提供統一的工作區。
- 使用無程式碼的視覺化環境來建立和管理管線、將查詢優化自動化、組建概念證明，以及使用 Power BI，這些都是來自相同的分析服務。
- 將您的資料深入解析傳遞至資料倉儲和大型資料分析系統。
- 針對任務關鍵性工作負載，使用智慧型工作負載管理、工作負載隔離和無限制的平行存取，將所有查詢的效能優化。
- 直接從 Azure Synapse Analytics 編輯和建立 Power BI 儀表板。
- 縮短 BI 和機器學習專案的專案開發時間。
- 使用 Azure Synapse Analytics 內的 Azure Data Share 整合，只要按幾下滑鼠就能輕鬆共用資料。
- 使用資料行層級安全性和原生資料列層級安全性，來執行更精細的存取控制。
- 使用動態資料遮罩來即時自動保護機密資料。
- 具有內建安全性功能（例如自動化威脅偵測和 always on 資料加密）的領先業界安全性。

## <a name="technology-advances"></a>技術進展

- 沒有硬體可購買或維護成本，因此您只需支付所使用的部分。
- 沒有可管理的基礎結構，因此您可以專注于競爭見解。
- 大量平行 SQL 查詢處理（當您需要時），並在不需要時關閉或暫停的選項。
- 能夠從計算中獨立調整儲存體。
- 您可以避免在資料倉儲上的臨時區域所造成的不必要、昂貴的升級變得太大、佔用儲存體容量，以及強制進行升級。 例如，將預備區域移至 Azure Data Lake Storage。 然後以較低成本在 Azure 上執行的 ETL 工具（例如 Azure Data Factory 或您現有的 ETL 工具）進行處理。
- 使用 Azure Data Lake Storage 和 Azure Data Factory，藉由處理 Azure 中的 ETL 工作負載，以避免昂貴的硬體升級。 這通常比在現有的資料倉儲 DBMS 上執行的解決方案更好，因為執行工作的 SQL 查詢處理。 當暫存資料量增加時，ETL 會取用內部部署資料倉儲的更多儲存體和計算能力。 這接著會影響查詢、報告和分析工作負載的效能。
- 避免建立昂貴的資料超市，以在內部部署硬體上使用儲存體和資料庫軟體授權。 您可以改為在 Azure Synapse Analytics 中建立它們。 如果您的資料倉儲是資料保存庫設計，這通常會造成資料超市的需求增加，這會特別有説明。
- 避免在內部部署硬體上分析和儲存高速大量資料的成本。 例如，如果您需要分析即時、機器產生的資料（例如，按一下資料流程和串流處理資料倉儲中的 IoT 資料），您可以使用 Azure Synapse Analytics。
- 您可以在資料倉儲成長時，避免支付 premium 以將資料儲存在資料中心的昂貴倉儲硬體上。 Azure Synapse Analytics 可以將您的資料以較低的成本儲存在雲端儲存體中。

## <a name="next-steps"></a>下一步

<!-- TODO: More detail needed here. -->

[資料民主化](./data-democratization.md)
