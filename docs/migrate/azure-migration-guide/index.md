---
title: Azure 移轉指南簡介
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 透過逐步指南了解如何有效地將組織的服務移轉至 Azure。
author: matticusau
ms.author: mlavery
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: fasttrack-new, AQC
ms.localizationpriority: high
ms.openlocfilehash: 2fa30c01de9d09af2c2947f940264d9156941589
ms.sourcegitcommit: f3371811a36e12533ecbc3aa936e2a68e0cee25f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/21/2019
ms.locfileid: "72698527"
---
::: zone target="docs"

# <a name="azure-migration-guide-before-you-start"></a>Azure 移轉指南：開始之前

::: zone-end

::: zone target="chromeless"

# <a name="before-you-start"></a>開始之前

::: zone-end

在您將資源移轉至 Azure 之前，您需要選擇移轉方法，以及用來控管和保護環境安全的功能。 本指南會引導您完成此決策流程。

::: zone target="docs"

> [!TIP]
> 如需互動式體驗，請在 Azure 入口網站中檢視本指南。 在 Azure 入口網站移至 [Azure 快速入門中心](https://portal.azure.com/?feature.quickstart=true#blade/Microsoft_Azure_Resources/QuickstartCenterBlade)，然後選取 [將您的環境移轉至 Azure]  。

::: zone-end

# <a name="overviewtaboverview"></a>[概觀](#tab/Overview)

本指南會逐步說明將應用程式和資源從內部部署環境移轉至 Azure 的基本概念。 專為複雜性最小的移轉範圍所設計。 若要判斷您的移轉是否適合使用本指南，請參閱 [本指南使用時機]  索引標籤。

當您移轉至 Azure 時，您可能會使用 IaaS 型虛擬機器解決方案 (又稱為「重新裝載」或「隨即轉移」移轉) 依現狀移轉您的應用程式，或您可能彈性使用受控服務和其他雲端原生功能，將您的應用程式現代化。 如需這些選擇的詳細資訊，請參閱 [移轉選項]  索引標籤。 當您開發移轉策略時，可考量下列事項：

- 移轉的應用程式是否會在雲端中運作？
- 對我的應用程式而言，最佳策略為何 (就技術、工具和移轉層面而言)？ 如需詳細資訊，請參閱 Microsoft 雲端採用架構的[移轉工具決策指南](../../decision-guides/migrate-decision-guide/index.md)。
- 如何將移轉期間的停機時間降到最低？
- 如何控制成本？
- 如何追蹤資源成本並準確地計費？
- 如何確保依然符合規範並遵守法規？
- 如何符合某些國家/地區中資料主權的法律需求？

本指南可協助回答這些問題。 當您準備好要在 Azure 中部署資源時，建議您可考量的工作和功能，包括：

> [!div class="checklist"]
>
> - **設定必要條件。** 規劃並為移轉做準備。
> - **評估技術適合度。** 驗證移轉技術整備程度與適用性。
> - **管理成本和計費。** 查看資源的成本。
> - **移轉服務。** 實際進行移轉。
> - **組織資源。** 鎖定系統不可或缺的資源，並對資源加上標記來予以追蹤。
> - **最佳化和轉換。** 利用移轉後機會來檢閱資源。
> - **保護和管理。** 確保環境安全無虞且受到適當監視。
> - **取得協助。** 在移轉或移轉後活動期間取得說明和支援。

::: zone target="docs"

若要深入了解組織和建構訂用帳戶、管理已部署的資源，以及符合公司的原則需求，請參閱 [Azure 中的治理](https://docs.microsoft.com/azure/security/governance-in-azure)。

::: zone-end

# <a name="when-to-use-this-guidetabwhentousethisguide"></a>[本指南使用時機](#tab/WhenToUseThisGuide)

雖然本指南中討論的工具支援各種不同的移轉案例，但本指南將重點放在「複雜性最小」  的有限範圍。 若要判斷本移轉指南是否適用於您的專案，請考慮您是否符合下列條件：

- 您要移轉同質環境。
- 只要幾個營業單位配合，便可完成移轉。
- 將整個移轉自動化，並不在您的規畫之中。
- 您要移轉少量的伺服器。
- 要移轉的元件相依性對應很容易定義。
- 您的產業和此移轉相關的法規需求最少。

如果上述有任一條件不  適用於您的情況，就應該考慮改用[擴充範圍指南](../expanded-scope/index.md)。 我們也建議您向其中一個 Microsoft 小組或合作夥伴要求協助，執行需要擴充範圍指南的移轉。 有 Microsoft 或經認證的合作夥伴參與其中的客戶，較能成功移轉。 要求協助的詳細資訊可在本指南取得。

<!-- markdownlint-enable MD033 -->

::: zone target="docs"

如需詳細資訊，請參閱

- [擴充範圍指南](../expanded-scope/index.md)

::: zone-end

# <a name="migration-optionstabmigrationoptions"></a>[移轉選項](#tab/MigrationOptions)

您有數種方式可以執行雲端移轉。 不同的案例適合的方式也不盡相同。 如同您決定要如何移轉環境，您也可以在決定移轉策略時，考量下列選項：

- **重新裝載：** 重新裝載工作也稱為「隨即轉移」，可在盡可能不變更整體架構的情況下，將目前的狀態移至 Azure。
- **重構：** 平台即服務 (PaaS) 選項可減少許多應用程式的相關作業成本。 稍微重構應用程式以符合 PaaS 模型，是審慎的做法。 這也是指重構程式碼，讓應用程式能夠因應新商機的應用程式開發程序。
- **重新架構：** 某些過時應用程式與雲端提供者無法相容，因為其架構決策是在應用程式建置時所制定的。 在這種情況下，應用程式可能需要在移轉過程中重新架構。
- **重建：** 在某些情況下，移轉應用程式所需的變更過大，而無法進一步投資，因此必須重建解決方案。
- **取代：** 解決方案通常會使用時當前最好的技術和技巧來實作。 在某些情況下，現代的軟體即服務 (SaaS) 應用程式功能相當於裝載的應用程式提供的所有功能。 在這類情況下，可將工作負載排入未來替換的時程中，因此無須在移轉過程中考量。

::: zone target="chromeless"

這些並非彼此互斥的方法&mdash;例如，雖然初始移轉可能會使用**重新裝載**模型，您還是可在進行移轉後最佳化階段時，選擇**重構**或**重新架構**。 這會在本指南的**最佳化和轉換**一節中加以回顧。

::: zone-end

::: zone target="docs"

這些並非彼此互斥的方法&mdash;例如，雖然初始移轉可能會使用**重新裝載**模型，您還是可在進行移轉後最佳化階段時，選擇**重構**或**重新架構**。 這會在本指南的[最佳化和轉換](./optimize-and-transform.md)一節中加以回顧。

::: zone-end

![移轉選項的資訊圖](../../_images/migrate/migration-options.png)
