---
title: 操作管理簡介
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 瞭解雲端採用架構中的營運管理。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 8e58aea5c0d3b77cd194f8bd8919f43143ab18a4
ms.sourcegitcommit: 74c1eb00a3bfad1b24f43e75ae0340688e7aec48
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/28/2019
ms.locfileid: "72979946"
---
# <a name="establish-operational-management-practices-in-the-cloud"></a>在雲端中建立營運管理實務

雲端採用是實現商業價值的 catalyst。 不過，實際的商業價值是透過部署至雲端的技術資產持續穩定的作業來實現。 雲端採用架構的這一節會引導您完成各種轉換，進入雲端的營運管理。

## <a name="actionable-best-practices"></a>可操作的最佳作法

新式作業管理解決方案會建立作業的多重雲端視圖。 透過下列最佳做法管理的資產可能會存在於雲端、現有資料中心，甚至是競爭雲端提供者中。 目前，此架構包含兩個最佳做法參考，以引導雲端中的作業管理成熟度：

- [Azure 伺服器管理](./azure-server-management/index.md)：整合管理作業所需之雲端原生工具和服務的上線指南。
- [混合式監視](./monitor/index.md)：許多客戶已對 System Center Operations Manager 投入大量投資。 對於這些客戶，這份混合式監視指南可協助他們比較雲端原生報表工具與 Operations Manager 工具，並與其進行對比。 這種比較可讓您更輕鬆地決定要使用哪些工具來進行作業管理。

## <a name="cloud-operations"></a>雲端作業

這兩種最佳作法都是針對作業管理的未來狀態方法而建立的，如下圖所示：

![管理雲端採用架構的方法](../_images/manage/caf-manage.png)

**商務對齊**：在管理方法中，所有工作負載都會依重要性和商業價值進行分類。 然後可以透過影響分析來衡量該分類，該影響分析會計算因效能降低或營業中斷而損失的價值。 利用這種實際的營收影響，雲端營運團隊可以與業務部門合作，建立在成本和效能之間取得平衡的承諾。

**雲端作業專業領域**：在商務調整之後，針對每個工作負載，追蹤並報告雲端作業的適當專業領域會更加容易。 接著，每個專業領域的決策都可以轉換成可讓企業輕鬆瞭解的承諾用量條款。 這種共同作業的方法可讓業務專案關係人成為尋找成本和效能之間適當平衡的合作夥伴。

- **清查和可見度**：作業管理至少需要一種方式來清查資產，並建立每個資產的執行狀態可見度。
- **營運合規性**：定期管理資產的設定、調整大小、成本和效能，是維持效能預期的關鍵。
- **保護和復原**：將作業中斷降到最低並加速修復，可協助企業避免效能損失和負面的收益影響。 偵測和復原是這個專業領域的重要層面。
- **平臺作業**：所有 IT 環境都包含一組常用的平臺。 這些平臺可以包含資料存放區，例如 SQL Server 或 Azure HDInsight。 其他常見的平臺可以包含容器解決方案，例如 Azure Kubernetes Service （AKS）。 無論平臺為何，平臺作業成熟度都著重于根據一般平臺的部署、設定和使用方式，自訂作業。
- **工作負載作業**：在最高層級的營運成熟度中，雲端營運小組可以針對企業成功的關鍵工作負載調整作業。 針對這些高重要性的工作負載，可用的資料可協助自動化修復、調整大小，或根據其使用率來保護工作負載。

如[設計審查架構（程式碼名稱：雲端設計原則）](https://docs.microsoft.com/azure/architecture/reliability)等其他指引，可以協助您在先前所述的專業領域中，針對每個工作負載進行詳細的架構決策。

雲端採用架構的這一節將會在上述每個主題上建立，以協助提升您組織內成熟的雲端作業。
