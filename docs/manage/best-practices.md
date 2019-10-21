---
title: 作業管理簡介
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 了解雲端採用架構中的作業管理。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/07/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 23b9064b91744d5464f89ed4edf64c8d656514a5
ms.sourcegitcommit: 35c162d2d09ec1c4a57d3d57a5db1d56ee883806
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2019
ms.locfileid: "72558099"
---
# <a name="establishing-operational-management-practices-in-the-cloud"></a>在雲端中建立作業管理實務

雲端採用是實現商務價值的催化管道。 不過，要實現實際的商業價值，則要透過持續且穩定地部署至雲端的技術資產作業。 本節中的雲端採用架構，會指導讀者逐步完成各種轉換到雲端中的作業管理。

## <a name="actionable-best-practices"></a>可行的最佳做法

新式作業管理解決方案會建立作業的多重雲端視圖。 透過下列最佳做法管理的資產可能會存在於雲端、現有資料中心，甚至是競爭雲端提供者中。 目前，該架構包含兩個參考的最佳做法，以指導雲端中的作業管理成熟度：

- [Azure 伺服器管理](./azure-server-management/index.md)：包含管理作業所需之雲端原生工具和服務的《上線指南》。
- [混合式監視](./monitor/index.md)：許多客戶已對 System Center Operations Manager 投入大量投資。 對於這些客戶，此混合式監視指南可協助客戶比較和對比雲端原生報表工具與 Operations Manager 工具。 透過這樣的比較，可讓您更輕鬆地決定要用於作業管理的工具。

## <a name="cloud-operations"></a>雲端作業

這兩個最佳做法都是針對作業管理的未來狀態方法而建立。

![管理雲端採用架構的方法](../_images/manage/caf-manage.png)

**商務調整：** 在管理方法中，所有工作負載都是依重要性和商業價值進行分類。 然後可以透過影響分析來衡量該分類，該影響分析會計算因效能降低或營業中斷而損失的價值。 利用這種實際的營收影響，雲端營運團隊可以與業務部門合作，建立在成本和效能之間取得平衡的承諾。

**雲端作業專業領域：** 一旦商務對齊之後，您就可以更輕鬆地追蹤及報告每個工作負載的適當雲端作業專業領域。 隨著每個專業領域所做的決策，則可以轉換為企業容易理解的承諾條款。 這種共同作業的方法可讓業務專案關係人成為尋找成本和效能之間適當平衡的合作夥伴。

- **清查和可見度：** 作業管理至少需要一種方式來清查資產，並建立每個資產的執行狀態可見度。
- **營運合規性：** 定期管理資產的設定、調整大小、成本和效能，是維持效能預期的關鍵。
- **保護與復原：** 將作業中斷和加速復原降至最低，有助於避免效能損失和收益影響。 偵測和復原是這個專業領域的重要層面。
- **平臺作業：** 所有的 IT 環境都包含一組常用的平臺。 這些平台可能包含 SQL Server 或 HDInsight 之類的資料存放區。 其他常見的平台可能包含如 kubernetes 或 AKS 容器解決方案。 無論是哪種平台，平台作業的成熟度均著重於根據工作負載如何部署、設定以及使用這些常用平台的自訂作業。
- **工作負載作業：** 在最高層級的營運成熟度中，雲端營運小組可以針對企業成功的關鍵工作負載調整作業。 對於這些高重要性的工作負載，可用資料可以根據工作負載的使用量協助進行自動修復、調整大小或保護。

如[設計審查架構（代號：雲端設計原則）](https://docs.microsoft.com/azure/architecture/reliability)之類的其他指引，可以協助您針對上述專業領域中的每個工作負載進行詳細的架構決策。

本節中的雲端採用架構將建置在這些主題上，以在您的組織內完善雲端作業。
