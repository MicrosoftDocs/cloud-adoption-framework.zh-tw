---
title: 用於雲端管理的特製化平台
description: 使用「適用於 Azure 的雲端採用架構」來改善平台專屬的雲端管理作業，包括改善系統設計和自動化修復。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: fasttrack-edit, AQC
ms.localizationpriority: high
ms.openlocfilehash: 37826614e405ddb5cb15db62ffce7cfe43b2ec20
ms.sourcegitcommit: 60d8b863d431b5d7c005f2f14488620b6c4c49be
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83216634"
---
# <a name="platform-specialization-for-cloud-management"></a>用於雲端管理的平台特製化

和增強的管理基準很像，平台特製化也是超越標準管理基準的擴充功能。 請參閱下方影像和清單，其中說明擴展管理基準的方式。 本文將說明平台特製化選項。

![超越雲端管理基準](../../_images/manage/beyond-the-baseline.png)

- **工作負載作業：** 每個工作負載作業的最大投資和最高程度的復原能力。 我們建議針對大約 20% 可帶來商業價值的工作負載進行工作負載作業。 此特製化通常保留給高度重要性或任務關鍵性的工作負載。
- **平台作業：** 作業投資會分散到許多工作負載。 復原能力的改善會影響所有使用已定義平台的工作負載。 我們建議針對 20% 的最重要平台進行平台作業。 此特製化通常保留給中度到高度重要性的工作負載。
- **增強的管理基準：** 相對最低的作業投資。 此特製化可稍微改善使用其他雲端原生作業工具和流程的商務承諾。

工作負載作業和平台作業都必須變更設計和架構方面的原則。 這些變更需要不少時間，而且可能會導致營運費用增加。 為了減少需要這類投資的工作負載數目，增強的管理基準可提供足夠的商務承諾改善。

此資料表概述客戶增強的管理基準中常見的一些流程、工具和潛在影響：

| Process | 工具 | 目的  | 建議的管理層級 |
|---|---|---|---|
| 改善系統設計 | Azure Architecture Framework | 改善平台的架構設計以改善作業 | N/A |
| 自動化補救 | Azure 自動化 | 使用平台專屬的自動化來回應進階平台的資料 | 平台作業 |
| 服務類別目錄 | 受控應用程式中心 | 針對符合組織標準的已核准解決方案來提供自助式目錄 | 平台作業 |
| 容器效能 | 適用於容器的 Azure 監視器 | 容器的監視和診斷 | 平台作業 |
| 平台即服務 (PaaS) 資料效能 | Azure SQL 分析 | PaaS 資料庫的監視和診斷 | 平台作業 |
| 基礎結構即服務 (IaaS) 資料效能 | SQL Server 健康情況檢查 | IaaS 資料庫的監視和診斷 | 平台作業 |

## <a name="high-level-process"></a>高階流程

要進行平台特製化作業，就必須有紀律地反覆執行下列四個流程。 本文後面幾節會更詳細地說明每個流程。

- **改善系統設計：** 改善常見系統或平台的設計，以便有效地將中斷情形降到最低。
- **自動化補救：** 某些改善並不符合成本效益。 在這種情況下，將補救程序自動化並降低中斷所造成的影響，可能會是更好的做法。
- **擴展解決方案：** 隨著系統設計和自動化補救功能的改善，這些變更就能透過服務類別目錄擴展到整個環境。
- **持續改善：** 您可以使用不同的監視工具來探索增加的改善項目。 這些改善項目可以在下一階段的系統設計、自動化和調整中進行處理。

::: zone target="docs"

## <a name="improve-system-design"></a>改善系統設計

::: zone-end
::: zone target="chromeless"

## <a name="improve-system-design"></a>[改善系統設計](#tab/SystemsDesign)

::: zone-end

改善系統設計最能有效改善任何常見平台的作業。 透過改善系統設計，不僅穩定性會增加，業務中斷的情形也會減少。 個別系統的設計不在整個 Azure 雲端採用架構中所採用環境檢視的範圍內。

作為雲端採用架構的補充，Azure Architecture Framework 可提供最佳做法來讓您改善特定系統的復原能力和設計。 這些設計的改善可套用至平台或特定工作負載的系統設計。

Azure Architecture Framework 著重在改善系統設計的五大要素：

- **延展性：** 擴展常見平台資產以應付增加的負載。
- **可用性：** 藉由改善可能的執行時間來減少業務中斷。
- **復原能力：** 改善復原時間以縮短中斷持續時間。
- **安全性：** 讓應用程式和資料免於遭受外部威脅。
- **管理：** 這些常見平台資產特有的作業流程。

技術債務和架構瑕疵會是大部分業務中斷的原因。 針對現有部署，您可以將系統設計改善視為對現有技術債務的償還。 針對新的部署，您可以將這些改善項目視為避免發生技術債務的方式。

下一個 [自動補救]  索引標籤會示範如何解決無法或不應解決的技術債務。

深入了解 [Azure Architecture Framework](https://docs.microsoft.com/azure/architecture/guide/pillars) 以改善系統設計。

系統設計改善後，請回到本文來尋找新的改善機會，並將這些改善擴展到整個環境。

::: zone target="docs"

## <a name="automated-remediation"></a>自動補救

::: zone-end
::: zone target="chromeless"

## <a name="automated-remediation"></a>[自動補救](#tab/AutomatedRemediation)

::: zone-end

有些技術債務是無法解決的。 解決方案的成本可能太高，以至於無法修正，或可能已計畫，但專案持續時間太長。 業務中斷可能沒有重大的商務影響。 或者，商務優先權可能是快速復原，而不是投資復原能力。

當技術債務的解決方案不是企業想要的方式時，接下來他們通常會採用自動補救。 使用 Azure 自動化和 Azure 監視器來偵測趨勢並提供自動化補救功能，是最常見的自動化補救方法。

如需自動補救的指引，請參閱 [Azure 自動化和警示](https://docs.microsoft.com/azure/automation/automation-create-alert-triggered-runbook)。

::: zone target="docs"

## <a name="scale-the-solution-with-a-service-catalog"></a>使用服務類別目錄來擴展解決方案

::: zone-end
::: zone target="chromeless"

## <a name="scale-the-solution-with-a-service-catalog"></a>[使用服務類別目錄來擴展解決方案](#tab/ServiceCatalog)

::: zone-end

妥善管理的服務目錄是平台特製化和平台作業的基石。 使用目錄才能將系統設計的改善和補救方法擴展到整個環境。

雲端平台小組和雲端自動化小組會彼此配合，來為任何環境中最常見的平台建立可重複執行的解決方案。 但是，如果未能一致地使用這些解決方案，雲端管理所要付出的成本，就會比平常所付出的基準成本再多一些。

若要讓最佳化平台的採用最大化，並讓維護成本降到最低，就應該將平台新增至 Azure 服務目錄。 您可以透過服務目錄來部署目錄中的每個應用程式，以供內部取用，或作為市集供應項目來供外部取用者取用。

如需如何發佈至服務類別目錄的指示，請參閱關於[發佈至服務類別目錄](https://docs.microsoft.com/azure/managed-applications/publish-service-catalog-app)的文章系列。

### <a name="deploy-applications-from-the-service-catalog"></a>從服務類別目錄部署應用程式

1. 在 Azure 入口網站中，移至 [受控應用程式中心 (預覽)]  。
2. 在 [瀏覽]  窗格上，選取 [服務目錄應用程式]  。
3. 選取 [+ 新增]  ，從公司的服務類別目錄中選擇應用程式定義。

系統會顯示您目前正在維護的任何受控應用程式。

::: zone target="chromeless"

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/Microsoft_Azure_Appliance/ManagedAppsHubBlade/browseServiceCatalog]" submitText="Go to Virtual Machines" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

### <a name="manage-service-catalog-applications"></a>管理服務類別目錄應用程式

1. 在 Azure 入口網站中，移至 [受控應用程式中心 (預覽)]  。
1. 在 [服務]  窗格上，選取 [服務目錄應用程式]  。

系統會顯示您目前正在維護的任何受控應用程式。

::: zone target="chromeless"

::: form action="OpenBlade[#blade/Microsoft_Azure_Appliance/ManagedAppsHubBlade/serviceServiceCatalogApps]" submitText="Go to Virtual Machines" :::

::: zone-end

::: zone target="docs"

## <a name="continuous-improvement"></a>持續改善

::: zone-end
::: zone target="chromeless"

## <a name="continuous-improvement"></a>[持續改善](#tab/ContinuousImprovement)

::: zone-end

平台特製化和平台作業都仰賴採用、平台、自動化和管理小組之間的強大意見反應迴圈。 以資料作為這些意見反應迴圈的基礎，有助於讓每個小組做出明智的決策。 若要讓平台作業實現長期的商務承諾，請務必使用集中式平台專屬的深入解析。

容器和 SQL Server 是兩個最常見的集中管理平台。 下列文章可協助您開始在這些平台上收集用於持續改善的資料：

- [容器效能](https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-overview)
- [PaaS 資料庫效能](https://docs.microsoft.com/azure/azure-monitor/insights/azure-sql)
- [IaaS 資料庫效能](https://docs.microsoft.com/azure/azure-monitor/insights/sql-assessment)
