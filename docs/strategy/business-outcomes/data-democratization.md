---
title: 資料民主化
description: 使用集中式資料存放庫，支援商務與資料創新。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/22/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: strategy
ms.custom: internal
ms.openlocfilehash: 6e011424301083c52e7b8ef9e935c2b3e57c327a
ms.sourcegitcommit: c167c45b66cc7324b60c88b8b7aac439f956b65d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2021
ms.locfileid: "102208484"
---
# <a name="data-democratization"></a>資料民主化

許多公司都會將資料倉儲保留在資料中心，以協助其商務的不同部分分析資料並做出決策。 銷售、行銷和財務部門高度依賴這些系統，以產生標準報告和儀表板。 公司也會採用商務分析師，在資料超市中執行特定查詢和資料分析。 這些資料超市使用自助商業智慧工具來執行多維度分析。

資料創新和新式資料資產所支援的企業，可讓廣大的投稿人（從 IT 專案關係人到資料專業人員，以及更高的範圍）。 他們可以對此集中式資料的存放庫採取動作，這通常稱為「單一事實來源」。

Azure Synapse Analytics 是一項服務，可進行順暢的共同作業和加速的時間見解。 若要更詳細地瞭解這項服務，請先考慮與一般資料資產相關的各種角色和技能：

**資料倉儲：** 資料庫管理員支援資料 lake 和資料倉儲的管理，同時以智慧方式優化工作負載並自動保護資料。

**資料整合：** 資料工程師會使用無程式碼的環境，輕鬆地連接多個來源和類型的資料。

**Big data 和 machine learning：** 資料科學家會快速建立概念證明，並在使用其選擇的語言時布建資源 (例如 T-sql、Python、Scala、.NET 或 Spark SQL) 。

**管理和安全性：** IT 專業人員可以更有效率地保護和管理資料、強制執行隱私權需求，以及安全地存取雲端和混合式設定。

**商業智慧：** 商務分析師會安全地存取資料集、建立儀表板，以及在組織內外共用資料。

## <a name="an-overview-of-classic-data-warehouse-architecture"></a>傳統資料倉儲架構的總覽

下圖顯示傳統資料倉儲架構的範例。

![傳統資料倉儲的圖表。](../../_images/analytics/the-classic-data-warehouse.png)

_圖1：傳統資料倉儲架構。_

已知的結構化資料會從核心交易處理系統中解壓縮，並複製到臨時區域中。 從該處，它會進行清除、轉換並整合到資料倉儲中的生產資料表。 在這裡可以建立幾年的歷程記錄交易資料。 這會提供瞭解銷售變化、客戶購買行為，以及一段時間內的客戶分割所需的資料。 它也提供年度財務報告和分析，以協助進行決策。

從該處，資料子集會解壓縮到資料超市，以分析與特定商務程式相關聯的活動。 這支援在企業特定部分進行決策。

若要讓企業有效率地執行，則需要所有類型的資料，以取得稍早所述的不同技能和角色。 您需要已為數據科學家清理的原始資料，才能建立機器學習服務模型。 您需要資料倉儲的乾淨資料和結構化資料，以提供可靠的效能給商務應用程式和儀表板。 最重要的是，您必須能夠在幾分鐘內（而不是幾天）從原始資料移至見解。

Azure Synapse Analytics 具有 Microsoft Power BI 的原生內建商務智慧工具。 在這裡，一個介面內的一項服務可讓您將原始資料快速轉換成顯示見解的儀表板。 

## <a name="next-steps"></a>下一步

<!-- TODO: More detail needed here. -->

[資料創新](./data-innovations.md)