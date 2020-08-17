---
title: 雲端監視的技能就緒
description: 雲端監視的技能就緒
author: BrianBlanchard
ms.author: magoedte
ms.date: 05/19/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: a3ea1f13db726fdc122e8a8acd07c8a15f8debca
ms.sourcegitcommit: 917188fa930cadddb03f9e9bbcdd7b630e4ee33e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/17/2020
ms.locfileid: "88283342"
---
<!-- cSpell:ignore kusto ITIL -->

# <a name="skills-readiness-for-cloud-monitoring"></a>雲端監視的技能就緒

在遷移旅程的計畫階段中，目標是要開發引導執行所需的計畫。 這些方案也必須包含如何在將這些工作負載轉換或發行到生產環境之前，以及之後才進行操作。 商務專案關係人預期會有寶貴的服務，並預期他們不會中斷。 IT 人員成員瞭解他們需要學習新的技能並進行調整，讓他們準備好安心地使用整合的 Azure 服務，以有效地監視 Azure 和混合式環境中的資源。

您可以使用下列學習路徑來加速開發必要的技能。 它們是從學習基本概念開始，然後劃分到三個主要的主旨網域-基礎結構、應用程式和資料分析。  

## <a name="fundamentals"></a>基礎

- [Azure Resource Manager](/azure/azure-resource-manager/management/overview)的簡介討論管理和部署 Azure 資源的基本概念。 管理整個企業的監視體驗的 IT 人員應該瞭解管理範圍、角色型存取控制 (RBAC) 的使用。 使用 Azure CLI 和 Azure PowerShell Azure Resource Manager 範本和資源管理。

- [Azure 原則](/azure/governance/policy/overview)簡介可協助您瞭解如何使用 Azure 原則來建立、指派和管理原則。 Azure 原則可以部署和設定 Azure 監視器代理程式、啟用適用於 VM 的 Azure 監視器和 Azure 資訊安全中心的監視、部署診斷設定、audit guest configuration 設定等。

- [Azure 命令列介面 (CLI) ](/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)的簡介，這是我們用來管理 Azure 資源的跨平臺命令列體驗。 另請參閱 [Azure PowerShell](/powershell/azure/?view=azps-3.6.1)的簡介。 LinkedIn 供應專案是其初學者層級課程 [學習 Azure 管理工具](https://www.linkedin.com/learning/learning-azure-management-tools)的一部分，其中涵蓋 Azure CLI 和 PowerShell 程式設計語言的會話：

  - [使用 Azure CLI](https://www.linkedin.com/learning/learning-azure-management-tools/use-the-azure-cli)。
  - [開始使用 Azure PowerShell](https://www.linkedin.com/learning/learning-azure-management-tools/understand-azure-powershell)

- 瞭解如何使用原則、角色型存取控制和其他 Azure 服務來保護資源安全，方法是 [在 Azure 中觀看執行資源管理安全性](/learn/paths/implement-resource-mgmt-security)。

- [監視 Microsoft Azure 資源和工作負載](https://app.pluralsight.com/library/courses/microsoft-azure-resources-workloads-monitoring-update/table-of-contents) 可協助您瞭解如何使用 azure 監視工具來監視 azure 網路資源，以及內部部署資源。

## <a name="infrastructure-monitoring"></a>基礎結構監視

- [設計 Microsoft Azure 基礎結構的監視策略](https://www.pluralsight.com/courses/microsoft-azure-monitoring-strategy-infrastructure-design-update) 可協助您瞭解在 Azure 中監視功能和解決方案的基本知識。

- [如何監視您的 Kubernetes](https://www.youtube.com/watch?time_continue=3&v=RjsNmapggPU&feature=emb_logo) 叢集會提供中繼層級深入探討，以協助您瞭解如何使用容器 Azure 監視器來監視您的 Kubernetes 叢集。

- 瞭解 Azure 監視器如何從 [Azure 儲存體和 HDInsight](https://www.pluralsight.com/courses/microsoft-azure-data-storage-monitoring)監視資料。

- [Microsoft Azure 資料庫監視](https://www.pluralsight.com/courses/microsoft-azure-database-playbook-monitoring) 腳本會探索可用來取得 Azure SQL Database、Azure SQL 資料倉儲和 Azure Cosmos DB 的深入解析和可採取動作步驟的重要監視功能。

- [監視 Microsoft Azure 混合式雲端網路](https://www.pluralsight.com/courses/microsoft-azure-hybrid-cloud-networks-monitoring) 是一種高階課程，可協助您瞭解如何使用 azure 監視工具，為您的混合式雲端執行視覺化、維護和優化 azure 虛擬網路和虛擬私人網路連線。

- 透過 [適用於伺服器的 Azure Arc](/azure/azure-arc/servers/overview)，瞭解如何管理裝載于 Azure 外部的 Windows 和 Linux 機器，類似于管理原生 azure 虛擬機器的方式。

## <a name="application-monitoring"></a>應用程式監視

- 瞭解 [Azure 監視器](/azure/azure-monitor/overview) 如何協助您將應用程式和服務的可用性和效能集中在一處。 Pluralsight 提供下列課程來協助您：

  - [Microsoft Azure DevOps 工程師：優化意見反應機制](https://www.pluralsight.com/courses/microsoft-azure-optimize-feedback-mechanisms) 可協助您使用 Azure 監視器（包括 Application Insights）來監視和優化您的 web 應用程式。

  - [Microsoft Azure 開發人員：監視效能](https://app.pluralsight.com/library/courses/microsoft-azure-performance-monitoring)。 開始使用 Azure 監視器 Application Insights，以在 Azure 中執行應用程式元件的端對端監視。
  
  - [Microsoft Azure 資料庫監視](https://www.pluralsight.com/courses/microsoft-azure-database-playbook-monitoring) 腳本可協助您瞭解如何執行 Azure SQL Database、Azure SQL 資料倉儲和 Azure Cosmos DB 的監視。

  - 使用[Azure 監視器 Application Insights 檢測應用程式](https://app.pluralsight.com/library/courses/microsoft-azure-application-insights-web-application-instrument)是使用 Application Insights SDK 從應用程式收集遙測和事件（具有角和 Node.js 元件）的深入教學課程。

  - [應用程式的偵錯工具和分析](https://www.pluralsight.com/courses/devintersection-azureai-session-31) 是 Microsoft 會議會話中使用和解讀 Azure 監視器 Application Insights 快照偵錯工具和 Profiler 所提供資料的記錄。

## <a name="data-analysis"></a>資料分析

- 瞭解如何 [在 Azure 監視器中撰寫記錄查詢](/azure/azure-monitor/log-query/get-started-queries)。 Kusto 查詢語言是撰寫 Azure 監視器記錄查詢的主要資源，用來探索和分析從 Azure 收集的資料和混合式資源應用程式相依性（包括即時應用程式）之間的記錄資料。

- [Kusto Query Language (KQL) 從頭](https://www.pluralsight.com/courses/kusto-query-language-kql-from-scratch) 開始是一個完整的課程，其中包含涵蓋各種使用案例的詳細範例，以及記錄分析在 Azure 監視器記錄檔中的各種技術。

## <a name="deeper-skills-exploration"></a>更深入的技能探索

除了這些用於開發技能的初始選項之外，還有各種學習選項可供使用。

### <a name="typical-mappings-of-cloud-it-roles"></a>雲端 IT 角色的一般對應

Microsoft 與合作夥伴會為所有學員提供各種不同的課程選擇，協助他們培養 Azure 服務的各項技能：

- [MICROSOFT IT 專業人員職業中心](https://www.microsoft.com/itpro)：作為免費的線上資源，可協助您對應雲端事業途徑。 了解產業專家針對您雲端角色提供的建議，以及進行建議內容的技能。 依照您自己的步調遵循學習課程，以建立可讓您跟上趨勢的必要技能。

透過 [azure 認證訓練和測驗，](https://www.microsoft.com/learning/certification-overview.aspx)讓您的 azure 知識變成官方辨識。

## <a name="azure-devops-and-project-management"></a>Azure DevOps 和專案管理

混合式雲端環境會將未定義的角色、責任和活動中斷。 組織必須移至現代化的服務管理實務（包括 Agile 和 DevOps 方法），以更有效率且有效率的方式滿足現今企業的轉換和優化需求。

在遷移至雲端監視平臺的過程中，負責管理企業中監視的 IT 小組必須包含 agile 訓練和參與 DevOps 活動。 這也包括在 DevOps 的 _開發_ 之後，藉由採取需求並轉換成組織敏捷式需求，來提供可反復調整並符合商務需求的最基本可行監視解決方案。 若要使用原始檔控制來管理反復監視解決方案套件和任何其他相關的輔助專案，請將您的 Azure DevOps Server 專案與 GitHub Enterprise 伺服器存放庫連接。 這會提供 GitHub 認可和提取要求至工作專案之間的連結。 您可以使用 GitHub Enterprise 進行開發，以支援持續監視整合和部署，同時使用 Azure Boards 來規劃和追蹤您的工作。

若要深入瞭解，請參閱下列各項：

- [Azure DevOps 入門](/learn/modules/get-started-with-devops)。

- [了解 DevOps Dojo 白帶基礎](/learn/paths/devops-dojo-white-belt-foundation)

- [發展您的 DevOps 實務](/learn/paths/evolve-your-devops-practices)

- [使用 Azure DevOps 自動化您的部署](/learn/paths/automate-deployments-azure-devops)

## <a name="other-considerations"></a>其他考量

客戶通常會難以管理、維護及傳遞預期的商務 (，以及 IT 組織) 透過提供計費的服務結果。 監視會視為核心來管理基礎結構和業務，並著重于測量服務品質和客戶體驗。 為了達成這些目標，請使用 ITSM 搭配 DevOps 來奠定基礎，這可協助監視小組成熟管理、交付和支援監視服務的方式。 採用 ITSM 架構可讓監視小組以提供者的形式運作，並藉由與組織的策略性目標和需求，讓辨識成為受信任的商務啟用程式。

請參閱下列各項，以瞭解對最受歡迎的 ITSM framework [ITIL v4 和雲端運算技術白皮書](https://www.axelos.com/case-studies-and-white-papers/itil-4-and-the-cloud)所進行的更新，其著重于將現有的 ITIL 指引與 DevOps、Agile 和精簡的最佳作法聯結。 另外也請考慮 [IT4IT 的參考架構](https://www.opengroup.org/it4it) ，以提供如何使用程式中立的架構來轉換它的替代藍圖。

## <a name="learn-more"></a>深入了解

如要探索其他學習路徑，請瀏覽 [Microsoft Learn 目錄](/learn/browse)。 使用角色篩選器，讓學習路徑與您的角色一致。