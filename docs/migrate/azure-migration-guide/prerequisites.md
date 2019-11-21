---
title: 移轉至 Azure 的必要條件
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 移轉至 Azure 的必要條件
author: matticusau
ms.author: mlavery
ms.date: 04/04/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: fasttrack-new, AQC
ms.localizationpriority: high
ms.openlocfilehash: 93479cb16b25d40d2f49356b19c4722497bc19f4
ms.sourcegitcommit: 3655aa7f3e80249e0b2b562cd40dd750afc82043
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74251796"
---
::: zone target="chromeless"

# <a name="prerequisites"></a>必要條件

::: zone-end

::: zone target="docs"

# <a name="prerequisites-for-migrating-to-azure"></a>移轉至 Azure 的必要條件

::: zone-end

本節中的資源可協助您準備好目前的環境，以移轉至 Azure。

# <a name="overviewtaboverview"></a>[概觀](#tab/Overview)

移轉至 Azure 的原因，包括要移除與舊版硬體相關聯的風險、降低資本支出、釋出資料中心空間，以及快速實現投資報酬率 (ROI)。

- **排除舊版硬體。** 無論是在內部部署還是在主機服務提供者上，您都可能在生命週期或支援即將終止的基礎結構上裝載應用程式。 針對此挑戰，移轉至雲端提供了一個相當不錯的解決方案，讓小組能夠以「原狀」的方式進行移轉，進而能快速解決目前的基礎結構生命週期挑戰，並將其注意力投注於雲端中的應用程式生命週期和最佳化的長期規劃。
- **解決軟體支援終止的問題。** 您可能有某些應用程式相依於支援即將終止的其他軟體或作業系統。 移至 Azure 可為這些相依性提供延長支援選項，或可將重構需求降至最低以繼續支援應用程式的其他移轉選項。 例如, 請參閱 [Windows Server 2008 和 SQL Server 2008 的延長支援選項](https://azure.microsoft.com/blog/announcing-new-options-for-sql-server-2008-and-windows-server-2008-end-of-support)。
- **降低資本支出。** 要裝載您自己的伺服器基礎結構，需要投資大量的硬體、軟體、電力和人員。 移轉至雲端的解決方案可大幅降低資本支出。 為了達到最理想的資本支出縮減效果，可能需要重新設計解決方案。 不過，「依原狀」移轉是很適當的起始步驟。
- **釋放資料中心空間。** 您可以選擇 Azure 以擴充資料中心容量。 為此，使用雲端作為內部部署功能的延伸模組，是其中一種方法。
- **快速實現投資報酬率。** 使用雲端解決方案，實現投資報酬率 (ROI) 會容易得多，因為雲端支付模型提供絕佳的使用率深入解析，並提升了實現 ROI 的文化特性。

上述每個案例都可能是使用另一種方法 (重新裝載、重構、重新架構、重建或取代) 來擴充雲端使用量的進入點。

## <a name="migration-characteristics"></a>移轉特性

本指南假設在進行此移轉之前，您的數位資產大部分都是由內部部署裝載的基礎結構所組成，而且可能包含裝載的業務關鍵應用程式。 在成功移轉後，資料資產的行為可能會與內部部署環境非常相似，但其基礎結構裝載在雲端資源中。 或者，理想的資料資產是您目前資料資產的一種變體，因為它具有內部部署基礎結構的各個層面，且其元件已經過重構而可最佳化並充分運用雲端平台。

此移轉旅程的重點在於達成：

- 補救舊版硬體生命週期結束的問題。
- 降低資本支出。
- 投資報酬率。

> [!NOTE]
> 此移轉旅程的另一項優點，是 Windows 2008、Windows 2008 R2 和 SQL Server 2008 以及 SQL Server 2008 R2 的其他軟體支援模型。 如需詳細資訊，請參閱
>
> - [Windows Server 2008 和 Windows Server 2008 R2](https://www.microsoft.com/cloud-platform/windows-server-2008)。
> - [SQL Server 2008 和 SQL Server 2008 R2](https://www.microsoft.com/sql-server/sql-server-2008)。

# <a name="understand-migration-approachestabapproach"></a>[了解移轉方法](#tab/Approach)

用於將應用程式移轉至 Azure 的策略和工具，在很大程度上取決於您的商業動機、技術需求和時間表，以及對正在移轉的實際工作負載和資產 (基礎結構、應用程式和資料) 的深入理解。

在決定您的雲端移轉策略之前，請先分析候選應用程式，以確認它們是否與雲端裝載技術相容。 請利用雲端採用架構的[移轉工具決策指南](../../decision-guides/migrate-decision-guide/index.md)開始進行此程序。

以 IaaS 為基礎的移轉通常是將工作負載移至雲端最直接的方法；在這類移轉中，伺服器 (及其相關聯的應用程式和資料) 會使用虛擬機器 (VM) 在雲端中重新裝載。 但您必須考量到，相較於使用 Azure 中的 PaaS 服務，正確設定、保護和維護 VM 可能需要投入更多時間和 IT 專業知識。 如果您考慮採用 Azure 虛擬機器，請確定您已將修補、更新和管理 VM 環境所需的持續性維護工作都納入考量。

評估移轉的工作負載時，請識別不需要進行大量修改即可使用 PaaS 技術 (例如 Azure App Service) 或協調器 (如 Azure Kubernetes Service) 執行的應用程式。 這些應用程式應是現代化和雲端最佳化的首選。

## <a name="learn-more"></a>深入了解

- [雲端採用架構移轉工具決策指南](../../decision-guides/migrate-decision-guide/index.md)
- [合理化的五 R 策略](../../digital-estate/5-rs-of-rationalization.md)

# <a name="planning-checklisttabchecklist"></a>[規劃檢查清單](#tab/Checklist)

開始進行移轉之前，您必須符合某些必要條件。 這些活動確切的詳細資料會隨著要移轉的環境而有所不同。 一般情況下，可套用下列檢查清單：

> [!div class="checklist"]
>
> - **識別專案關係人：** 識別在移轉的結果中扮演某個角色或有利害關係的重要人員。
> - **識別重要里程碑：** 若要有效規劃移轉時程表，請識別須達成的重要里程碑。
> - **識別移轉策略：** 判斷您將使用的「合理化的 5R 策略」。
> - **評估技術適合度：** 驗證移轉的技術是否已準備就緒並適用，並確認可能需要何種程度的外部合作夥伴或 Azure 支援的協助。
> - **移轉規劃：** 執行準備資產 (基礎結構、應用程式和資料) 所需的詳細評估和規劃，以及用於移轉的 Azure 基礎結構。
> - **測試移轉：** 執行有限範圍的測試移轉，以驗證您的移轉計劃。
> - **移轉服務：** 實際進行移轉。
> - **移轉後：** 了解將環境移轉至 Azure 後必須執行的作業。

假設您選擇以重新裝載方法進行移轉，則將有下列相關活動：

> [!div class="checklist"]
>
> - **控管一致性：** 移轉基礎的控管一致性是否已達成相關共識？
> - **網路：** 應選取網路策略，且該策略必須符合 IT 安全性需求。
> - **身分識別：** 混合式身分識別策略應進行調整，以符合身分識別管理和雲端採用方案。

::: zone target="docs"

<!-- markdownlint-disable MD024 -->

## <a name="learn-more"></a>深入了解

- [合理化的 5R 策略](../../digital-estate/5-rs-of-rationalization.md)
- [移轉工具決策指南](../../decision-guides/migrate-decision-guide/index.md)
- [雲端採用架構規劃檢查清單](../migration-considerations/prerequisites/planning-checklist.md)

::: zone-end
