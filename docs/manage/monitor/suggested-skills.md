---
title: 雲端監視的技能就緒
description: 雲端監視的技能就緒
author: BrianBlanchard
ms.author: brblanch
ms.date: 08/26/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: internal
ms.openlocfilehash: 64c70208dc0030a221dd657a5ab83309734c78c5
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102113957"
---
<!-- cSpell:ignore kusto ITIL -->

# <a name="skills-readiness-for-cloud-monitoring"></a>雲端監視的技能就緒

在規劃您的遷移旅程時，目標是開發引導執行所需的計畫。 這些方案也必須包含如何在將這些工作負載轉換或發行到生產環境之前，以及之後才進行操作。 商務專案關係人預期會有寶貴的服務，並預期他們不會中斷。 IT 人員成員瞭解他們需要學習新的技能並進行調整，讓他們準備好安心地使用整合的 Azure 服務，以有效地監視 Azure 和混合式環境中的資源。

您可以使用下列學習路徑來加速開發必要的技能。 它們是從學習基本概念開始，然後分成三個主要主體網域：基礎結構、應用程式和資料分析。

## <a name="fundamentals"></a>基礎

- [Azure Resource Manager](/azure/azure-resource-manager/management/overview)簡介討論管理和部署 azure 資源的基本概念。 管理整個企業的監視體驗的 IT 人員應該瞭解管理範圍、Azure 角色型存取控制 (Azure RBAC) 的使用。 Azure Resource Manager 範本，以及使用 Azure CLI 和 Azure PowerShell 來管理資源。

- [Azure 原則](/azure/governance/policy/overview)簡介可協助您瞭解如何使用 azure 原則來建立、指派和管理原則。 Azure 原則可以部署和設定 Azure 監視器代理程式、啟用適用于 Vm 的 Azure 監視器和 Azure 資訊安全中心的監視、部署診斷設定、audit guest configuration 設定等。

- [Azure 命令列介面 (CLI) ](/cli/azure/get-started-with-azure-cli)的簡介，這是我們用來管理 Azure 資源的跨平臺命令列體驗。 另請參閱 [Azure PowerShell](/powershell/azure/)簡介。 在初學者層級的課程中， [瞭解 azure 管理工具](https://www.linkedin.com/learning/learning-azure-management-tools)LinkedIn 提供涵蓋 azure CLI 和 PowerShell 程式設計語言的會話：

  - [使用 AZURE CLI](https://www.linkedin.com/learning/learning-azure-management-tools/use-the-azure-cli)。
  - [開始使用 Azure PowerShell](https://www.linkedin.com/learning/learning-azure-management-tools/understand-azure-powershell)

- 瞭解如何藉由 [在 Azure 中觀看實作為資源管理安全性](/learn/paths/implement-resource-mgmt-security/)，以使用原則、Azure 角色型存取控制和其他 Azure 服務來保護資源。

- [監視 Microsoft Azure 資源和工作負載](https://www.pluralsight.com/courses/microsoft-azure-resources-workloads-monitoring-update) 可協助您瞭解如何使用 azure 監視工具來監視 azure 網路資源，以及內部部署資源。

- 查看 [Azure 監視器的最佳作法和建議](https://www.youtube.com/watch?v=IWkqqahX_Ck&list=PLLasX02E8BPCDMuesOy2C0_TMFsoZWe_0&index=6)，以瞭解如何大規模規劃和設計您的監視部署，以及自動化動作。

## <a name="infrastructure-monitoring"></a>基礎結構監視

- [在 Microsoft azure 中設計基礎結構的監視策略](https://www.pluralsight.com/courses/microsoft-azure-monitoring-strategy-infrastructure-design-update) 可協助您瞭解在 Azure 中監視功能和解決方案的基本知識。

- [如何監視您的 Kubernetes](https://www.youtube.com/watch?time_continue=3&v=RjsNmapggPU&feature=emb_logo) 叢集會提供中繼層級深入探討，以協助您瞭解如何使用適用于容器的 Azure 監視器來監視您的 Kubernetes 叢集。

- 瞭解如何使用 Azure 監視器來監視 [Azure 儲存體和 HDInsight](https://www.pluralsight.com/courses/microsoft-azure-data-storage-monitoring)中的資料。

- [Microsoft Azure 資料庫監視](https://www.pluralsight.com/courses/microsoft-azure-database-playbook-monitoring) 腳本會探索可用來取得 Azure sql Database、Azure Sql 資料倉儲和 AZURE Cosmos DB 的深入解析和可採取動作步驟的重要監視功能。

- [監視 Microsoft Azure 混合式雲端網路](https://www.pluralsight.com/courses/microsoft-azure-hybrid-cloud-networks-monitoring) 是一種先進的課程，可協助您瞭解如何使用 Azure 監視工具來視覺化、維護和優化混合式雲端執行的虛擬網路和虛擬私人網路連線。

- 使用 [適用于伺服器的 Azure Arc](/azure/azure-arc/servers/overview)，瞭解如何管理裝載于 azure 外部的 Windows 和 Linux 機器，類似于如何管理在 azure 中執行的虛擬機器。

- [如何監視您的 vm](https://www.youtube.com/watch?v=O7scXPrsM_0&list=PLLasX02E8BPCDMuesOy2C0_TMFsoZWe_0&index=6&t=0s) 提供中繼層級深入探討，以協助您瞭解如何使用適用于 Vm 的 azure 監視器來監視混合式電腦或伺服器，以及 azure VM 或虛擬機器擴展集。

## <a name="application-monitoring"></a>應用程式監視

- 瞭解 [Azure 監視器](/azure/azure-monitor/overview) 如何協助您將應用程式和服務的可用性和效能集中在同一個位置。 Pluralsight 提供下列課程來協助您：

  - [Microsoft Azure DevOps 工程師：優化意見反應機制](https://www.pluralsight.com/courses/microsoft-azure-optimize-feedback-mechanisms) 可協助您使用 Azure 監視器（包括 application Insights）來監視和優化您的 web 應用程式。

  - [在您的 Azure web 應用程式中，捕獲並查看頁面載入時間](/learn/modules/capture-page-load-times-application-insights/)。 開始使用 Azure 監視器 Application Insights，以針對在 Azure 中執行的應用程式元件進行端對端監視。

  - [Microsoft Azure 資料庫監視](https://www.pluralsight.com/courses/microsoft-azure-database-playbook-monitoring) 腳本可協助您瞭解如何實行和使用 Azure sql Database、Azure Sql 資料倉儲和 AZURE Cosmos DB 的監視。

  - 使用[Azure 監視器 Application Insights 檢測應用](https://www.pluralsight.com/courses/microsoft-azure-application-insights-web-application-instrument)程式是深入探討如何使用 APPLICATION insights SDK 從應用程式收集遙測資料，並從應用程式中 Node.js 元件。

  - [應用程式的偵錯工具和分析](https://www.pluralsight.com/courses/devintersection-azureai-session-31) 是使用和解讀 Azure 監視器 Application Insights 快照集偵錯工具和分析工具所提供的資料，從 Microsoft 會議會話進行錄製。

<!-- docutune:ignore "from Scratch" -->

## <a name="data-analysis"></a>資料分析

- 瞭解如何 [在 Azure 監視器中撰寫記錄查詢](/learn/modules/analyze-infrastructure-with-azure-monitor-logs/)。 Kusto 查詢語言是用來撰寫 Azure 監視器記錄查詢的主要資源，可在從 Azure 收集的資料和混合式資源應用程式相依性（包括即時應用程式）之間探索和分析記錄資料。

- [Kusto Query Language (KQL) 從頭](https://www.pluralsight.com/courses/kusto-query-language-kql-from-scratch) 開始提供詳盡的課程，其中涵蓋各種使用案例和技術，以在 Azure 監視器記錄中進行記錄分析。

## <a name="deeper-skills-exploration"></a>更深入的技能探索

這些初始選項以外的各種學習選項都可用於開發技能。

### <a name="typical-mappings-of-cloud-it-roles"></a>雲端 IT 角色的一般對應

Microsoft 和合作夥伴會為所有的使用者提供各種選項，以使用 Azure 服務開發技能。

- [對應角色和技能](../../plan/suggested-skills.md)：用於對應雲端事業路徑的資源。 深入瞭解您的雲端角色和建議的技能。 依照您自己的步調遵循學習課程，建立您最需要的技能，以保持相關。

- 探索 [azure 認證訓練和測驗](/learn/certifications/) ，以取得您 azure 知識的官方認知。

## <a name="azure-devops-and-project-management"></a>Azure DevOps 和專案管理

混合式雲端環境會將未定義的角色、責任和活動中斷。 組織必須移至現代化實務來管理服務（包括 agile 和 DevOps 方法），以簡化且有效率的方式，更符合現今企業的轉換和優化需求。

在遷移至雲端監視平臺的過程中，負責管理企業監視的 IT 小組必須包含 agile 訓練和參與 DevOps 活動。 這也包括在 DevOps 的 *開發* 之後，藉由採取需求並轉換成組織敏捷式需求，來提供可反復調整並符合商務需求的最基本可行監視解決方案。 若要使用原始檔控制來管理反復監視解決方案套件和任何其他相關的輔助專案，請將您的 Azure DevOps Server 專案與 GitHub Enterprise 伺服器存放庫連接。 這會提供從 GitHub 中的認可和提取要求到工作專案的連結。 您可以使用 GitHub Enterprise 進行開發，以支援持續監視整合和部署，同時使用 Azure 面板來規劃和追蹤您的工作。

若要深入瞭解，請參閱下列各項：

- [開始使用 Azure DevOps](/learn/modules/get-started-with-devops/)。

- [深入瞭解 DevOps Dojo 的基礎](/learn/paths/devops-dojo-white-belt-foundation/)。

- [發展您的 DevOps 實務](/learn/paths/evolve-your-devops-practices/)。

- [使用 Azure DevOps 自動化您的部署](/learn/paths/automate-deployments-azure-devops/)。

## <a name="other-considerations"></a>其他考量

客戶通常會難以管理、維護及傳遞預期的商務 (，以及 IT 組織) 透過提供計費的服務結果。 監視會視為核心來管理基礎結構和業務，並著重于測量服務品質和客戶體驗。 為了達成這些目標，請使用 ITSM 搭配 DevOps 來奠定基礎，這可協助監視小組成熟管理、交付和支援監視服務的方式。 採用 ITSM 架構可讓監視小組以提供者的形式運作，並藉由與組織的策略性目標和需求，讓辨識成為受信任的商務啟用程式。

<!-- docutune:casing "ITIL 4 and the Cloud" -->

請參閱下列文章，以瞭解對最受歡迎的 ITSM framework [ITIL 4 和雲端白皮書](https://www.axelos.com/case-studies-and-white-papers/itil-4-and-the-cloud)所做的更新，其著重于將現有的 ITIL 指引與 DevOps、agile 和精簡方法的最佳作法聯結。 另外也請考慮 [IT4IT 的參考架構](https://www.opengroup.org/it4it) ，以提供如何使用程式中立的架構來轉換它的替代藍圖。

## <a name="learn-more"></a>深入了解

若要探索更多學習路徑，請流覽 [Microsoft learning 目錄](/learn/browse/)。 使用 [角色] 篩選，將學習路徑與您的角色對齊。
