---
title: 作業管理簡介
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 了解雲端採用架構中的作業管理。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/19/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: manage
ms.openlocfilehash: 96f87583f50783fa0c6a8c947aa8b34ae440fc95
ms.sourcegitcommit: d19e026d119fbe221a78b10225230da8b9666fe1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2019
ms.locfileid: "71221432"
---
# <a name="establishing-operational-management-practices-in-the-cloud"></a>在雲端中建立作業管理實務

雲端採用是實現商務價值的催化管道。 不過，要實現實際的商業價值，則要透過持續且穩定地部署至雲端的技術資產作業。 本節中的雲端採用架構，會指導讀者逐步完成各種轉換到雲端中的作業管理。

## <a name="actionable-best-practices"></a>可行的最佳做法

現代化的作業管理解決方案提供了多重雲端檢視的作業。 透過下列建議做法來管理的資產，可能存在於雲端、現有資料中心，或甚至競爭的雲端服務提供者中。 目前，該架構包含兩個參考的建議做法，可使雲端中的作業管理機制發展成熟：

- [Azure 伺服器管理](./azure-server-management/index.md)：這是上架指南，可納入管理作業所需的雲端原生工具和服務。
- [混合式監視](./monitor/index.md)：許多客戶已經在 System Center Operations Manager 上進行大量投資。 對於這些客戶，此混合式監視指南可協助客戶比較和對比雲端原生報表工具與 Operations Manager 工具。 透過這樣的比較，可讓您更輕鬆地決定要用於作業管理的工具。

## <a name="cloud-operations"></a>雲端作業

這兩種最佳做法都建立在作業管理的未來狀態方法。

![CAF 管理方法](../_images/manage/caf-manage.png)

**業務配合：** 在管理方法中，所有的工作負載都依重要性和商業價值加以分類。 然後可以透過影響分析來衡量該分類，該影響分析會計算因效能降低或營業中斷而損失的價值。 利用這種實際的營收影響，雲端營運團隊可以與業務部門合作，建立在成本和效能之間取得平衡的承諾。

**雲端運算專業領域：** 在業務進行配合之後，就更容易針對每個工作負載追踪和報告雲端作業的正確專業領域。 順著每個專業領域做出決策便可推動能讓企業容易了解的承諾。 這種共同作業的方法可讓業務專案關係人成為尋找成本和效能之間適當平衡的合作夥伴。

- **清查和可見性：** 作業管理至少需要一種清查資產並建立每種資產運行狀態的可見性方法。
- **作業合規性：** 定期管理資產的設定、調整大小、成本和效能，是維持效能預期的關鍵。
- **保護和復原：** 盡可能地減少作業中斷並加速復原，都有助於避免效能損失和對營收的影響。 偵測和復原是這個專業領域的重要層面。
- **平台作業：** 所有的 IT 環境均包含一組常用的平台。 這些平台可能包含 SQL Server 或 HDInsight 之類的資料存放區。 其他常見的平台可能包含如 Kubernetes 或 AKS 容器解決方案。 無論是哪種平台，平台作業的成熟度均著重於根據工作負載如何部署、設定以及使用這些常用平台的自訂作業。
- **工作負載作業：** 在最高層級的作業成熟度，雲端營運團隊能夠對業務成功至關重要的工作負載進行調整作業。 對於這些重要的工作負載，可用資料可以根據工作負載的使用量協助進行自動修復、調整大小或保護。

其他指導方針如[設計檢閱架構 (代號：雲端設計原則)](https://docs.microsoft.com/azure/architecture/reliability) 可以協助在上述專業領域內進行有關每個工作負載的詳細架構決策。

本節中的雲端採用架構將建置在這些主題上，以在您的組織內完善雲端作業。
