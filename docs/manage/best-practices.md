---
title: 作業管理簡介
description: 使用適用于 Azure 的雲端採用架構，以瞭解必須進行的各種轉換，才能在雲端中啟用操作管理。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 54c6e5d3dc292e3380fe0b51b265165b8fed3f28
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88574885"
---
# <a name="establish-operational-management-practices-in-the-cloud"></a>在雲端中建立營運管理實務

雲端採用是實現商務價值的催化管道。 不過，要實現實際的商業價值，則要透過持續且穩定地部署至雲端的技術資產作業。 雲端採用架構的這一節會引導您完成各種轉換到雲端中的作業管理。

## <a name="actionable-best-practices"></a>可行的最佳做法

新式作業管理解決方案會建立作業的多重雲端觀點。 透過下列最佳做法管理的資產，可能存在於雲端、現有資料中心，甚至是在競爭雲端提供者中。 目前，架構包含兩個最佳作法參考，以引導雲端中的作業管理成熟度：

- [Azure 伺服器管理](./azure-server-management/index.md)：加入管理作業所需的雲端原生工具和服務的上線指南。
- [混合式監視](./monitor/index.md)：許多客戶已在 System Center Operations Manager 中進行大量投資。 針對這些客戶，此混合式監視指南可以協助他們比較和對比雲端原生報表工具與 Operations Manager 工具。 這種比較可讓您更輕鬆地決定要用於作業管理的工具。

## <a name="cloud-operations"></a>雲端作業

這兩個最佳做法都是針對作業管理的未來狀態方法所建立，如下圖所示：

<!-- cSpell:ignore caf -->

![管理雲端採用架構的方法](../_images/manage/caf-manage.png)

**商務調整：** 在管理方法中，所有工作負載都會依重要性和商業價值進行分類。 然後可以透過影響分析來衡量該分類，該影響分析會計算因效能降低或營業中斷而損失的價值。 利用這種實際的營收影響，雲端營運團隊可以與業務部門合作，建立在成本和效能之間取得平衡的承諾。

**雲端作業專業領域：** 在商務調整之後，您可以更輕鬆地追蹤及報告每個工作負載的適當雲端作業專業領域。 然後，您可以將每個專業領域的決策轉換成可由企業輕鬆理解的承諾用量條款。 這種共同作業的方法可讓業務專案關係人成為尋找成本和效能之間適當平衡的合作夥伴。

- **清查和可見度：** 作業管理至少需要一種清查資產的方法，並為每個資產的執行狀態建立可見度。
- **營運合規性：** 定期管理資產的設定、調整大小、成本和效能，是維持效能預期的關鍵。
- **保護和復原：** 將作業中斷和加速復原降至最低有助於企業避免效能損失和負面的收入衝擊。 偵測和復原是這個專業領域的重要層面。
- **平臺作業：** 所有的 IT 環境都包含一組常用的平臺。 這些平臺可能包含資料存放區，例如 SQL Server 或 Azure HDInsight。 其他常見的平臺可能包含容器解決方案，例如 Azure Kubernetes Service (AKS) 。 無論平臺為何，平臺作業成熟度都著重于根據如何部署、設定及使用工作負載的一般平臺來自訂作業。
- **工作負載作業：** 在最高層級的作業成熟度，雲端營運團隊可以調整重要工作負載的作業。 針對這些工作負載，可用資料可以根據工作負載的使用量，協助自動化補救、調整大小或保護工作負載。

其他指導方針，例如 [設計審核架構 (程式碼名稱：雲端設計原則) ](/azure/architecture/framework/resiliency/overview)，可協助您在先前所述的專業領域中，針對每個工作負載進行詳細的架構決策。

雲端採用架構的這一節將會以上述每個主題為依據，以協助提升組織內成熟的雲端作業。
