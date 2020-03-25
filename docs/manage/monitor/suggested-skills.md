---
title: 雲端監控的技能就緒
description: 雲端監控的技能就緒
author: BrianBlanchard
ms.author: magoedte
ms.date: 03/23/2020
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 7e078bbeea12c71997474d372b0b6b838ed5f4a3
ms.sourcegitcommit: a1209c9dab369171e1fe0cdc6a58e3f6ae6a8f22
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/25/2020
ms.locfileid: "80261533"
---
# <a name="skills-readiness-for-cloud-monitoring"></a>雲端監控的技能就緒

在您遷移旅程的計畫階段中，目標是要開發引導執行所需的計畫。 這些方案也必須包含如何在這些工作負載轉換或發行到生產環境之前進行操作，而不會在之後執行。 商務專案關係人預期有價值的服務，而他們預期不會中斷。 IT 人員的成員意識到他們必須學習新技能並進行調整，讓他們能夠安心地利用整合式 Azure 服務，有效監視 Azure 和混合式環境中的資源。 

您可以使用下列學習路徑來加速開發必要的技能：

- [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/management/overview)的簡介討論管理和部署 Azure 資源的基本概念。 管理整個企業中監視體驗的 IT 人員，應該使用來瞭解管理範圍，也就是角色型存取控制（RBAC）。 Azure Resource Manager 範本，以及使用 Azure CLI 和 Azure PowerShell 來管理資源。

- 瞭解如何使用原則、角色型存取控制和其他 Azure 服務來保護資源，方法是[在 Azure 中觀看執行資源管理安全性](https://docs.microsoft.com//learn/paths/implement-resource-mgmt-security/)。 

- [Azure 原則](https://docs.microsoft.com/azure/governance/policy/overview)簡介可協助您瞭解如何使用 Azure 原則來建立、指派和管理原則。 Azure 原則可以部署和設定 Azure 監視器代理程式、啟用適用於 VM 的 Azure 監視器和 Azure 資訊安全中心的監視、部署診斷設定、audit 來賓設定等。

- [Azure 命令列介面](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)（CLI）簡介，這是用來管理 Azure 資源的跨平臺命令列體驗。 另請參閱[Azure PowerShell](https://docs.microsoft.com/powershell/azure/?view=azps-3.6.1)簡介。 LinkedIn 供應專案是初學者層級課程[學習 Azure 管理工具](https://www.linkedin.com/learning/learning-azure-management-tools)的一部分，涵蓋 Azure CLI 和 PowerShell 程式設計語言的研討會：

   - [使用 Azure CLI](https://www.linkedin.com/learning/learning-azure-management-tools/use-the-azure-cli)。
   
   - [開始使用 Azure PowerShell](https://www.linkedin.com/learning/learning-azure-management-tools/understand-azure-powershell) 

- 瞭解如何[在 Azure 監視器中撰寫記錄查詢](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries)。  Kusto 查詢語言是用來撰寫 Azure 監視器記錄查詢的主要資源，用以探索和分析從 Azure 收集到的資料與混合式資源應用程式相依性（包括即時應用程式）之間的記錄資料。

- 瞭解[Azure 監視器](https://docs.microsoft.com/azure/azure-monitor/overview)如何協助您從一個位置同時查看應用程式和服務的可用性和效能。 Pluralsight 提供下列課程來協助您：

   - [Microsoft Azure Iaas 監視和管理](https://www.pluralsight.com/courses/azure-iaas-monitoring-management-getting-started)可協助您瞭解如何使用 Azure 監視器對 IaaS 上執行的工作負載進行基本監視。

   - [監視 Microsoft Azure 資源和工作負載](https://www.pluralsight.com/courses/microsoft-azure-resources-workloads-monitoring)可協助您瞭解如何使用 Microsoft Azure 監視工具來監視 Azure 網路（以及內部內部部署）資源。

   - [Microsoft Azure DevOps 工程師：優化意見反應機制](https://www.pluralsight.com/courses/microsoft-azure-optimize-feedback-mechanisms)可協助您準備使用 Azure 監視器，包括 Application Insights 和 Log Analytics 來監視和優化您的 web 應用程式。

   - [Microsoft Azure 資料庫監視](https://www.pluralsight.com/courses/microsoft-azure-database-playbook-monitoring)腳本可協助您瞭解如何執行和使用 Azure SQL Database、Azure SQL 資料倉儲和 Azure Cosmos DB 的監視。

- 使用[適用于伺服器的 Azure Arc](https://docs.microsoft.com/azure/azure-arc/servers/overview)時，您可以瞭解如何管理裝載于 azure 外部的 Windows 和 Linux 機器，如同管理原生 azure 虛擬機器的方式。

## <a name="deeper-skills-exploration"></a>更深入的技能探索

除了這些用於開發技能的初始選項之外，還有各種學習選項可供使用。

### <a name="typical-mappings-of-cloud-it-roles"></a>雲端 IT 角色的一般對應

Microsoft 與合作夥伴會為所有學員提供各種不同的課程選擇，協助他們培養 Azure 服務的各項技能：

- [MICROSOFT IT 專業人員職業中心](https://www.microsoft.com/itpro)：作為免費的線上資源，協助您對應雲端事業的途徑。 了解產業專家針對您雲端角色提供的建議，以及進行建議內容的技能。 依照您自己的步調遵循學習課程，以建立可讓您跟上趨勢的必要技能。

參加 [Microsoft Azure 認證訓練課程與測驗]( https://www.microsoft.com/learning/azure-certification.aspx)，讓您的 Azure 知識獲得官方認證。

## <a name="azure-devops-and-project-management"></a>Azure DevOps 和專案管理

混合式雲端環境會以未定義的角色、責任和活動來中斷它。 組織必須移至現代化的服務管理實務（包括 Agile 和 DevOps 方法），以簡化且有效率的方式，更符合現今企業的轉換和優化需求。 

作為遷移至雲端監視平臺的一部分，負責管理企業中監視的 IT 小組需要包含 agile 訓練和參與 DevOps 活動。 這也包括遵循 DevOps 的*開發*，方法是採取需求並轉換成組織的 agile 需求，以提供最少可行的監視解決方案，並以反復且符合業務需求的方式進行調整。 若要讓原始檔控制管理反復監視解決方案封裝和任何其他相關的宣傳，請將您的 Azure DevOps Server 專案與 GitHub Enterprise 伺服器存放庫連線。 這會在 GitHub 認可與工作專案的提取要求之間提供連結。 您可以使用 GitHub Enterprise 進行開發，以支援持續監視整合和部署，同時使用 Azure Boards 來規劃和追蹤您的工作。

若要深入瞭解，請參閱下列各項：

- [開始使用 Azure DevOps](https://docs.microsoft.com/learn/modules/get-started-with-devops/)。 

- [瞭解 DevOps dojo 的泛用皮帶基礎](https://docs.microsoft.com/learn/paths/devops-dojo-white-belt-foundation/)

- [發展您的 DevOps 實務](https://docs.microsoft.com/learn/paths/evolve-your-devops-practices/)

- [使用 Azure DevOps 自動化您的部署](https://docs.microsoft.com/learn/paths/automate-deployments-azure-devops/)

## <a name="other-considerations"></a>其他考量

客戶通常會難以管理、維護和提供預期的企業（和 IT 組織）結果，以符合傳遞的服務。 監視會被視為核心來管理基礎結構和企業，並著重于測量服務品質和客戶體驗。  若要達成這些目標，請使用 ITSM 搭配 DevOps 來奠定基礎，這可協助監視小組成熟其管理、交付及支援監視服務的方式。 採用 ITSM 架構可讓監視小組作為提供者，並藉由配合組織的策略性目標和需求，取得辨識作為信任的商務啟用者。

請參閱下列內容，以瞭解對最受歡迎的 ITSM framework [ITIL v4 和雲端運算技術白皮書](https://www.axelos.com/case-studies-and-white-papers/itil-4-and-the-cloud)的更新，其重點在於將現有的 ITIL 指導方針與 DevOps、Agile 和瘦的最佳做法聯結在一起。 也請考慮[IT4IT 參考架構](https://www.opengroup.org/it4it)，以提供如何使用不受處理的架構來轉換它的替代藍圖。

## <a name="learn-more"></a>進一步了解

若要探索其他學習路徑，請流覽[Microsoft Learn 目錄](https://docs.microsoft.com/learn/browse)。 使用 [角色] 篩選器可讓學習路徑與您的角色對齊。