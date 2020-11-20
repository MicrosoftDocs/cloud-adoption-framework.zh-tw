---
title: 瞭解雲端資料函數
description: 瞭解雲端資料功能，包括功能的來源、範圍和交付成果。
author: v-hanki
ms.author: janet
ms.service: cloud-adoption-framework
ms.subservice: organize
ms.topic: conceptual
ms.date: 07/14/2020
ms.openlocfilehash: a0231d05fdb05a89499f253fa4564faf286ca1ef
ms.sourcegitcommit: 57b757759b676a22f13311640b8856557df36581
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2020
ms.locfileid: "94995647"
---
# <a name="cloud-data-functions"></a>雲端資料功能

分析對話中牽涉到多個物件，包括一般賣方、資料庫架構設計人員和基礎結構團隊。 此外，分析解決方案包含企業架構、資料科學、商務分析師和主管領導角色的影響因素、recommender 和決策者。

Azure Synapse Analytics 讓整個企業（從 IT 專案關係人到商務分析師）共同作業分析解決方案，並瞭解雲端資料功能。 下列各節會更詳細地討論這些角色。

## <a name="database-administrators-and-architects"></a>資料庫管理員和架構設計人員

資料庫管理員和架構設計人員負責將資料來源整合和路由至集中式存放庫。 這些專家也會處理系統所需的系統管理與效能，以及對該資料進行查詢和分析模型的協助工具和效率。

使用 Azure Synapse Analytics，資料庫管理員可以比對資料倉儲和資料 lake 的擴充責任。 他們可以使用熟悉的語言和工具（例如 T-sql）來執行所需數量的工作負載。 他們可以指派資源，以根據智慧型工作負載重要性、工作負載隔離和增強的並行功能來提升重要的工作負載。

## <a name="infrastructure-teams"></a>基礎結構小組

這些團隊會處理大型分析系統所需之基礎計算資源的布建和架構。 在許多情況下，它們會管理資料中心和雲端系統之間的轉換，以及兩者之間的互通性目前需求。 嚴重損壞修復、商務持續性和高可用性都是常見的考慮。

透過 Azure Synapse Analytics，IT 專業人員可以更有效率地保護和管理其組織的資料。 它們可以透過隨選和布建的計算來啟用大型資料處理。 透過與 Azure Active Directory 的緊密整合，此服務可協助保護雲端和混合式設定的存取。 IT 專業人員可以使用資料遮罩，以及資料列層級和資料行層級安全性，來強制執行隱私權需求。

## <a name="enterprise-architects-and-data-engineers"></a>企業架構設計人員和資料工程師

這些小組負責將複雜的解決方案與橫跨各種資料工具和解決方案 swath 的元件整合在一起。 它們包括：

- 結構化與非結構化資料
- 轉換
- 儲存和擷取
- 分析模型
- 以訊息為基礎的中介軟體
- 資料超市
- 異地冗余和資料一致性
- 儀表板和報告

 企業架構設計人員和資料工程師通常會關心如何建立能以整合方式運作的有效架構。 這類架構會保留效能、可用性、輕鬆管理、彈性/擴充性及可動作性。

資料工程師可以使用 Azure Synapse Analytics，簡化從多個來源（包括串流、交易式和商務資料）整頓多個資料類型的步驟。 他們可以使用無程式碼的視覺化環境來連接至資料來源，並在 data lake 中內嵌、轉換和放置資料。

## <a name="data-scientists"></a>資料科學家

資料科學家瞭解如何針對大量重要，但通常會有不同的資料，建立先進的模型。 其工作牽涉到將企業的需求轉譯成正規化和轉換資料的技術需求。 他們會建立統計和其他分析模型，並確保企業營運團隊可以取得執行業務所需的分析。

使用 Azure Synapse Analytics，資料科學家可以在幾分鐘內建立概念證明，並建立或調整端對端解決方案。 他們可以視需要布建資源，或單純地依據大量資料查詢現有的資源。 他們可以使用各種不同的語言來執行工作，包括 T-sql、R、Python、Scala、.NET 和 Spark SQL。

## <a name="business-analysts"></a>商務分析師

這些小組會建立和使用儀表板、報表和其他形式的資料視覺效果，以取得作業所需的快速見解。 通常，每個企業營運部門都有專門的商務分析師，可收集並封裝來自特製化應用程式的資訊和分析。 這些特殊應用程式可用於信用卡、零售銀行、商業銀行、財政部、行銷和其他組織。

使用 Azure Synapse Analytics，商務分析師可以安全地存取資料集，並使用 Power BI 來建立儀表板。 他們也可以透過 Azure Data Share，安全地在組織內外共用資料。

## <a name="executives"></a>主管

主管負責制作圖表策略，並確保在 IT 和企業營運部門之間有效率地實行策略性計畫。 解決方案必須符合成本效益、避免業務中斷、可在需求變更和成長時輕鬆擴充，並將結果傳遞給企業。
