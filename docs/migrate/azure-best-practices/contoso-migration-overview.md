---
title: Azure 的應用程式移轉範例概觀
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 概述「雲端採用架構移轉」一節中包含的應用程式移轉範例。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/11/2018
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 18b2bc641ba45c83a8ce6c5069857c398801adfd
ms.sourcegitcommit: bf9be7f2fe4851d83cdf3e083c7c25bd7e144c20
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/04/2019
ms.locfileid: "73566435"
---
# <a name="application-migration-patterns-and-examples"></a>應用程式移轉模式和範例

這個「雲端採用架構」小節提供數個常見移轉案例的範例，示範如何將內部部署基礎結構移轉至 [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure) 雲端。

## <a name="introduction"></a>簡介

Azure 提供全方位雲端服務組合的存取權。 讓開發人員與 IT 專業人員使用這組服務，透過全球資料中心網路在各種工具和架構上建置、部署及管理應用程式。 當貴公司面臨與數位轉換有關的挑戰時，Azure 雲端可協助您了解如何將資源和作業最佳化、與客戶和員工交流，以及轉換您的產品。

不過，Azure 了解，即使雲端可在速度和彈性、降至最低的成本、效能和可靠性等方面提供優勢，但許多組織還是需要在今後的一段時間裡，執行內部部署資料中心。 為了回應雲端採用障礙，Azure 提供混合式雲端策略來銜接內部部署資料中心與 Azure 公用雲端。 例如，使用 Azure 備份等 Azure 雲端資源來保護內部部署資源，或使用 Azure 分析來深入了解內部部署工作負載。

做為混合式雲端策略的一部分，Azure 會提供不斷增加的解決方案，協助您將內部部署應用程式和工作負載移轉至雲端。 透過簡單的步驟，您即可全面地評估內部部署資源，以了解要如何在 Azure 雲端中執行這些資源。 然後，在有了深入評估的情況下，您可以放心地將資源遷移至 Azure。 當資源在 Azure 中啟動並執行時，您可以將它們最佳化以維持和改善存取、彈性、安全性與可靠性。

## <a name="migration-patterns"></a>移轉模式

移轉至雲端的策略可分成以下四大模式：重新裝載、重構、重新架構或重建。 您採用的策略取決於商業誘因和移轉目標。 您可以採用多種模式。 例如，您可選擇對簡單的應用程式，或對業務重要性不大的應用程式採取重新裝載的方式；而較複雜或具業務關鍵功能的應用程式則採用重新架構的方式。 我們來看一下這些模式。

<!-- markdownlint-disable MD033 -->

**模式** | **定義** | **使用時機**
--- | --- | ---
**重新裝載** | 通常稱為隨即_轉移_。 這個選項不需要變更程式碼，可讓您將現有的應用程式迅速移轉至 Azure。 每個應用程式皆依原狀移轉，可直接享有雲端優勢，而無須承擔變更程式碼的相關風險和成本。 | 需要將應用程式快速移轉至雲端時。<br/><br/> 想要在不修改應用程式的情況下進行移轉時。<br/><br/> 當應用程式在移轉後進行建構以便運用 [Azure IaaS](https://azure.microsoft.com/overview/what-is-iaas) 延展性時。<br/><br/> 應用程式對您的業務很重要，但還不需要立即變更應用程式功能時。
**重構** | 重構通常稱為「重新封裝」，需要對應用程式進行最小程度的變更，以便您連結至 [Azure PaaS](https://azure.microsoft.com/overview/what-is-paas) 並使用雲端供應項目。<br/><br/> 例如，您可將現有應用程式移轉至 Azure App Service 或 Azure Kubernetes Service (AKS)。<br/><br/> 或者，您也可以將關聯式和非關聯式資料庫重構為 Azure SQL Database 受控執行個體、適用於 MySQL 的 Azure 資料庫、適用於 PostgreSQL 的 Azure 資料庫和 Azure Cosmos DB 之類的選項。 | 若您的應用程式可輕鬆地重新封裝以在 Azure 中運作。<br/><br/> 若想採用 Azure 提供的創新式 DevOps 做法或針對工作負載使用容器策略會考慮 DevOps 時。<br/><br/> 若要選擇重構，您必須考量現有程式碼基礎的可攜性及可用的開發技能。
**重新架構** | 若採用重新架構進行移轉，則會著重於修改和擴充應用程式功能及程式碼基礎，藉此將應用程式架構最佳化，以達成雲端延展性。<br/><br/> 例如，您可將單一應用程式劃分為數個微服務群組，如此即能輕鬆搭配運作和擴充。<br/><br/> 或者，您可將關聯式和非關聯式資料庫重新架構為完全受控的資料庫解決方案，例如 Azure SQL Database 受控執行個體、適用於 MySQL 的 Azure 資料庫、適用於 PostgreSQL 的 Azure 資料庫和 Azure Cosmos DB。 | 您的應用程式需要主要修訂，以納入新功能或在雲端平台上有效地運作時。<br/><br/> 您想要使用現有應用程式投資、達到延展性要求、採用創新的 Azure DevOps 做法，以及盡可能避免使用虛擬機器時。
**重建** | 重建的做法則更深入，您必須使用 Azure 雲端技術從頭重新打造應用程式。<br/><br/> 例如，使用 Azure Functions、Azure AI、Azure SQL Database 受控執行個體和 Azure Cosmos DB 等[雲端原生](https://azure.com/cloudnative)技術建置全新應用程式。 | 想要快速完成開發，且現有應用程式的功能及使用週期有限時。<br/><br/> 準備加速商務創新 (包括 Azure 提供的 DevOps 做法)、使用雲端原生技術建置全新應用程式，以及利用 AI、區塊鏈和 IoT 領域的先進技術時。

<!-- markdownlint-enable MD033 -->

## <a name="migration-example-articles"></a>移轉範例文章

本節中的文章提供數個常見移轉案例的範例。 每個範例都包含背景資訊和詳細的部署案例，說明如何設定移轉基礎結構，以及評估內部部署資源是否適合進行移轉。 本節後續將新增更多文章。

![一般移轉/現代化專案](./media/migration-patterns.png)

*一般移轉和現代化專案類別。*

以下摘要說明這一系列的文章。

- 由於每種移轉案例的商務目標都略有不同，因此移轉策略也會有所差異。
- 針對每個部署案例，我們提供的相關資訊包括：商業誘因和目標、提議的架構、執行移轉的步驟，以及清理建議和移轉完成後可採取的後續步驟。

### <a name="assessment"></a>評量

**文章** | **詳細資料**
--- | ---
[評估內部部署資源是否可移轉至 Azure](./contoso-migration-assessment.md) | 本文說明如何評估在 VMware 上執行的內部部署應用程式。 在此範例中，範例組織會使用 Azure Migrate 服務來評估應用程式 VM，並使用 Database Migration Assistant 來評估應用程式 SQL Server 資料庫。

### <a name="infrastructure"></a>基礎結構

**文章** | **詳細資料**
--- | ---
[部署 Azure 基礎結構](./contoso-migration-infrastructure.md) | 本文說明組織如何準備其內部部署基礎結構和其 Azure 基礎結構以進行移轉。 本文中建立的基礎結構範例，會供本節中提供的其他範例參考。

### <a name="windows-server-workloads"></a>Windows Server 工作負載

**文章** | **詳細資料**
--- | ---
[將應用程式重新裝載在 Azure VM 上](./contoso-migration-rehost-vm.md) | 本文提供使用 Azure Site Recovery 服務將內部部署應用程式 VM 移轉至 Azure 的範例。
[在 Azure 容器和 Azure SQL Database 中重新建構應用程式](./contoso-migration-rearchitect-container-sql.md) | 本文提供範例說明如何在重新建構應用程式 Web 層作為在 Azure Service Fabric 中執行的 Windows 容器，以及具有 Azure SQL Database 的資料庫時，移轉應用程式。

### <a name="linux-workloads"></a>Linux 工作負載

**文章** | **詳細資料**
--- | ---
[在 Azure VM 和適用於 MySQL 的 Azure 資料庫上重新裝載 Linux 應用程式](./contoso-migration-rehost-linux-vm-mysql.md) | 本文提供使用 Site Recovery 將 Linux 裝載的應用程式移轉至 Azure VM 的範例。 它會使用 MySQL Workbench 將應用程式資料庫遷移至適用於 MySQL 的 Azure 資料庫。
[將 Linux 應用程式重新裝載至 Azure VM](./contoso-migration-rehost-linux-vm.md) | 此範例示範如何使用 Site Recovery 服務，完成將 Linux 型應用程式隨即轉移至 Azure Vm 的操作。

### <a name="sql-server-workloads"></a>SQL Server 工作負載

**文章** | **詳細資料**
--- | ---
[在 Azure VM 和 SQL Database 受控執行個體上重新裝載應用程式](./contoso-migration-rehost-vm-sql-managed-instance.md) | 本文提供適用于內部部署應用程式的隨即轉移至 Azure 的範例。 此作業須使用 [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) 來移轉應用程式前端 VM，並使用 [Azure 資料庫移轉服務](https://docs.microsoft.com/azure/dms/dms-overview)將應用程式資料庫移轉至 Azure SQL Database 受控執行個體。
[在 Azure VM 上和 SQL Server Always On 可用性群組中重新裝載應用程式](./contoso-migration-rehost-vm-sql-ag.md) | 此範例說明如何使用 Azure 裝載的 SQL Server VM 移轉應用程式和資料。 範例中會使用 Site Recovery 來移轉應用程式 VM，並使用 Azure 資料庫移轉服務，將應用程式資料庫移轉至受到 SQL Server 可用性群組保護的 SQL Server 叢集。

### <a name="aspnet-php-and-java-apps"></a>ASP.NET、PHP 和 JAVA 應用程式

**文章** | **詳細資料**
--- | ---
[在 Azure Web 應用程式和 Azure SQL Database 中重構應用程式](./contoso-migration-refactor-web-app-sql.md) | 此範例說明如何將內部部署 Windows 型應用程式移轉至 Azure Web 應用程式，以及使用 Database Migration Assistant 將應用程式資料庫移轉至 Azure SQL Server 執行個體。
[使用 Azure App Service、Azure 流量管理員及適用於 MySQL 的 Azure 資料庫，將 Linux 應用程式重構至多個區域](./contoso-migration-refactor-linux-app-service-mysql.md) | 此範例說明如何使用 Azure 流量管理員，將內部部署 Linux 型應用程式移轉至多個 Azure 區域的 Azure Web 應用程式，與 GitHub 整合以進行持續傳遞。 應用程式資料庫會移轉至適用於 MySQL 的 Azure 資料庫執行個體。
[在 Azure 中重建應用程式](./contoso-migration-rebuild.md) | 本文提供的範例說明如何使用各種 Azure 功能和受控服務來重建內部部署應用程式，包括 Azure App Service、Azure Kubernetes Service (AKS)、Azure Functions、Azure 認知服務及 Azure Cosmos DB。
[在 Azure DevOps Services 上重構 Team Foundation Server](./contoso-migration-tfs-vsts.md) | 本文中的範例說明將內部部署 Team Foundation Server 部署移轉至 Azure 中的 Azure DevOps Services。

### <a name="migration-scaling"></a>移轉調整

**文章** | **詳細資料**
--- | ---
[對 Azure 進行大規模移轉](./contoso-migration-scale.md) | 本文說明如何讓範例組織準備好對 Azure 進行完整規模的移轉。

### <a name="demo-apps"></a>示範應用程式

本節所提供的範例文章使用兩個示範應用程式： SmartHotel360 和 osTicket。

- **SmartHotel360：** 此應用程式是由 Microsoft 開發做為測試應用程式，可讓您在使用 Azure 時使用。 SmartHotel360 以開放原始碼提供，您可從 [GitHub](https://github.com/Microsoft/SmartHotel360) 下載； 其為一種連線至 SQL Server 資料庫的 ASP.NET 應用程式。 在這些文章所討論的案例中，此應用程式的目前版本會部署到分別執行 Windows Server 2008 R2 和 SQL Server 2008 R2 的兩個 VMware VM。 這些應用程式 VM 裝載於內部部署，且由 vCenter Server 管理。
- **osTicket：** 在 Linux 上執行的開放原始碼服務台票證應用程式。 您可以從 [GitHub](https://github.com/osTicket/osTicket) 下載。 在這些文章所討論的案例中，此應用程式的目前版本會使用 Apache 2、PHP 7.0 和 MySQL 5.7 在內部部署到執行 Ubuntu 16.04 LTS 的兩個 VMware VM
