---
title: Azure 的應用程式移轉範例概觀
description: 提供在雲端採用架構中遷移方法所包含的應用程式遷移範例的總覽。
author: deltadan
ms.author: abuck
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 33be59a80186795bf33ee4a22e3dc98534624090
ms.sourcegitcommit: 26aee3c6f596bb8a9f1e16af93cdf94e41a61dee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2020
ms.locfileid: "87400474"
---
# <a name="overview-of-application-migration-examples-for-azure"></a>Azure 的應用程式移轉範例概觀

適用于 Azure 的雲端採用架構的這一節提供數個常見遷移案例的範例，並示範如何將內部部署基礎結構遷移至[Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure)。

## <a name="introduction"></a>簡介

Azure 提供全方位雲端服務組合的存取權。 身為開發人員和 IT 專業人員，您可以使用這些服務，透過資料中心的全球網路，在各種工具和架構上建立、部署和管理應用程式。 當您的企業面臨數位轉移的相關挑戰時，Azure plaftorm 可協助您瞭解如何：

- 優化資源和作業。
- 與您的客戶和員工互動。
- 轉換您的產品。

雲端提供速度和彈性的優點、最小化的成本、效能和可靠性。 但許多組織都需要繼續執行內部部署資料中心。 Azure 提供混合式雲端策略，在您的內部部署資料中心與 Azure 公用雲端之間建立橋樑，以回應雲端採用屏障。 例如，使用類似 Azure 備份的 Azure 雲端資源來保護內部部署資源或 Azure 分析，以深入瞭解內部部署工作負載。

Azure 是混合式雲端策略的一部分，提供持續成長的解決方案，可將內部部署應用程式和工作負載遷移至雲端。 透過簡單的步驟，您可以全面評估內部部署資源，以瞭解其在 Azure 平臺中的執行方式。 然後，在有了深入評估的情況下，您可以放心地將資源遷移至 Azure。 當資源在 Azure 中啟動並執行時，您可以將它們最佳化以維持和改善存取、彈性、安全性與可靠性。

## <a name="migration-patterns"></a>移轉模式

移轉至雲端的策略可分成以下四大模式：重新裝載、重構、重新架構或重建。 您採用的策略取決於商業誘因和移轉目標。 您可以採用多種模式。 例如，您可以選擇重新裝載非關鍵應用程式，同時重新架構更複雜且業務關鍵性的應用程式。 我們來看一下這些模式。

| 模式 | 定義 | 使用時機 |
| --- | --- | --- |
| **重新裝載** | 通常稱為「隨即轉移」的轉移，此選項不需要變更程式碼。 您可以使用它來快速將您現有的應用程式遷移至 Azure。 每個應用程式都是以相同的方式遷移，以獲得雲端的優勢，而不會有與程式碼變更相關聯的風險和成本。 | 當您需要快速將應用程式移至雲端時。 <br><br> 當您想要移動應用程式但不修改它時。 <br><br> 當您的應用程式設計成可在遷移後利用[Azure 基礎結構即服務（IaaS）](https://azure.microsoft.com/overview/what-is-iaas)擴充性。 <br><br> 當應用程式對您的企業很重要，但您不需要立即變更應用程式功能。 |
| **重構** | 重構通常稱為「重新封裝」，需要對應用程式進行最少的變更，使其能夠連接到[Azure 平臺即服務（PaaS）](https://azure.microsoft.com/overview/what-is-paas)並使用雲端供應專案。 <br><br> 例如，您可以將現有的應用程式遷移至 Azure App Service 或 Azure Kubernetes Service （AKS）。 <br><br> 或者，您可以將關聯式和非關聯式資料庫重構為 Azure SQL 受控執行個體、適用於 MySQL 的 Azure 資料庫、適用於 PostgreSQL 的 Azure 資料庫和 Azure Cosmos DB 等選項。 | 如果您的應用程式可以輕鬆地重新封裝以在 Azure 中工作。 <br><br> 如果您想要套用 Azure 所提供的創新 DevOps 實務，或如果您考慮使用工作負載的容器策略進行 DevOps。 <br><br> 若要進行重構，您必須考慮現有程式碼基底的可攜性和可用的開發技能。 |
| **重新架構** | 重新架構 for 遷移著重于修改和擴充應用程式功能，以及用來將應用程式架構優化以進行雲端擴充性的程式碼基底。 <br><br> 例如，您可將單一應用程式劃分為數個微服務群組，如此即能輕鬆搭配運作和擴充。 <br><br> 您也可以將關聯式和非關聯式資料庫重新架構到完全受控的資料庫解決方案，例如 SQL 受控執行個體、適用於 MySQL 的 Azure 資料庫、適用於 PostgreSQL 的 Azure 資料庫和 Azure Cosmos DB。 | 當您的應用程式需要主要修訂，以納入新功能或在雲端平臺上有效率地工作時。 <br><br> 當您想要使用現有的應用程式投資、滿足擴充性需求、套用創新的 DevOps 做法，並將虛擬機器的使用降到最低。 |
| **重建** | 重建會使用 Azure 雲端技術，從頭開始重建應用程式。 <br><br> 例如，您可以使用 Azure Functions、AI、SQL 受控執行個體和 Azure Cosmos DB 等[雲端原生](https://azure.microsoft.com/overview/cloudnative)技術來建立 greenfield 應用程式。 | 當您想要快速開發時，現有應用程式的功能和生命週期有限。 <br><br> 準備加速商務創新 (包括 Azure 提供的 DevOps 做法)、使用雲端原生技術建置全新應用程式，以及利用 AI、區塊鏈和 IoT 領域的先進技術時。 |

## <a name="migration-example-articles"></a>移轉範例文章

本節提供數個常見遷移案例的範例。 每個範例都包含背景資訊和詳細的部署案例，說明如何設定遷移基礎結構，並評估內部部署資源的適用性以進行遷移。 本節後續將新增更多文章。

![遷移和現代化專案分類的圖表。 ](./media/migration-patterns.png)
_圖1：一般遷移和現代化專案類別。_

此系列著重于每個遷移案例，並以稍微不同的商業目標來決定遷移策略。 針對每個部署案例，我們提供下列資訊：

- 商業驅動程式與目標。
- 提議的架構。
- 執行遷移的步驟。
- 在遷移完成後，清除和後續步驟的建議。

### <a name="assessment"></a>評量

| 發行項 | 詳細資料 |
| --- | --- |
| [評估內部部署資源以遷移至 Azure](../../plan/contoso-migration-assessment.md) | 此計畫方法中的最佳做法文章討論如何對 VMware 上執行的內部部署應用程式進行評估。 在本文中，範例組織會使用 Azure Migrate 和應用程式 SQL Server 資料庫，藉由使用 Data Migration Assistant 來評估應用程式 Vm。 |

### <a name="infrastructure"></a>基礎結構

| 發行項 | 詳細資料 |
| --- | --- |
| [部署 Azure 基礎結構](./contoso-migration-infrastructure.md) | 本文說明組織如何準備其內部部署基礎結構和其 Azure 基礎結構以進行移轉。 本文中建立的基礎結構範例，會供本節中提供的其他範例參考。 |

### <a name="windows-server-workloads"></a>Windows Server 工作負載

| 發行項 | 詳細資料 |
| --- | --- |
| [在 Azure Vm 上重新裝載應用程式](./contoso-migration-rehost-vm.md) | 本文提供使用 Azure Migrate 將內部部署應用程式 Vm 遷移至 Azure Vm 的範例。 |

### <a name="sql-server-workloads"></a>SQL Server 工作負載

| 發行項 | 詳細資料 |
| --- | --- |
| [移轉 SQL Server 資料庫到 Azure](./contoso-migration-sql-server-db-to-azure.md) | 本文示範虛構的公司 Contoso 如何評估、規劃、將其各種內部部署 SQL Server 資料庫移轉至 Azure。 |
| [在 Azure VM 和 SQL 受控執行個體上重新裝載應用程式](./contoso-migration-rehost-vm-sql-managed-instance.md) | 本文提供將內部部署應用程式隨即轉移至 Azure 的範例。 此程式牽涉到使用[Azure 資料庫移轉服務](https://docs.microsoft.com/azure/dms/dms-overview)，將 Azure Migrate 和應用程式資料庫移轉至 SQL 受控執行個體，以將應用程式前端 VM。 |
| [使用 SQL Server Always On 可用性群組在 Azure Vm 上重新裝載應用程式](./contoso-migration-rehost-vm-sql-ag.md) | 此範例示範如何使用 Azure 託管的 SQL Server Vm 來遷移應用程式和資料。 它會使用 Azure Migrate 來遷移應用程式 Vm 和資料庫移轉服務，以將應用程式資料庫移轉至 Always On 可用性群組所保護的 SQL Server 叢集。 |

### <a name="linux-and-open-source-databases"></a>Linux 和開放原始碼資料庫

| 發行項 | 詳細資料 |
| --- | --- |
| [將開放原始碼資料庫遷移到 Azure](./contoso-migration-oss-db-to-azure.md) | 本文示範虛構的公司 Contoso 如何評估、規劃並將其各種內部部署開放原始碼資料庫移轉至 Azure。 |
| [將 MySQL 遷移到 Azure](./contoso-migration-mysql-to-azure.md) | 本文示範虛構公司 Contoso 如何規劃並將其內部部署 MySQL 開放原始碼資料庫平臺遷移至 Azure。 |
| [將 PostgreSQL 遷移到 Azure](./contoso-migration-postgresql-to-azure.md) | 本文示範虛構公司 Contoso 如何規劃並將其內部部署于 postgresql 開放原始碼資料庫平臺遷移至 Azure。 |
| [將 MariaDB 遷移到 Azure](./contoso-migration-mariadb-to-azure.md) | 本文示範虛構公司 Contoso 如何規劃並將其內部部署適用于 mariadb 開放原始碼資料庫平臺遷移至 Azure。 |
| [在 Azure Vm 上重新裝載 Linux 應用程式和適用於 MySQL 的 Azure 資料庫](./contoso-migration-rehost-linux-vm-mysql.md) | 本文提供的範例會示範如何使用 Azure Migrate 將 Linux 裝載的應用程式遷移至 Azure Vm。 應用程式資料庫會使用[資料庫移轉服務](https://docs.microsoft.com/azure/dms/dms-overview)遷移至適用於 MySQL 的 Azure 資料庫。 |
| [在 Azure Vm 上重新裝載 Linux 應用程式](./contoso-migration-rehost-linux-vm.md) | 此範例示範如何使用 Azure Migrate，完成將 Linux 應用程式隨即轉移至 Azure Vm 的操作。 |

### <a name="devtest-workloads"></a>開發/測試工作負載

| 發行項 | 詳細資料 |
| --- | --- |
| [將開發/測試環境遷移至 Azure IaaS](./contoso-migration-devtest-to-iaas.md) | 本文示範 Contoso 如何藉由遷移至 Azure Vm，為 VMware Vm 上執行的兩個應用程式重新裝載其開發/測試環境。 |
| [遷移到 Azure DevTest Labs](./contoso-migration-devtest-to-labs.md) | 本文討論 Contoso 如何使用 DevTest Labs 將其開發/測試工作負載移至 Azure。 |

### <a name="aspnet-and-php-web-apps"></a>ASP.NET 和 PHP web 應用程式

| 發行項 | 詳細資料 |
| --- | --- |
| [使用 App Service 和 SQL Database 重構 Windows 應用程式](./contoso-migration-refactor-web-app-sql.md) | 此範例說明如何使用[資料庫移轉服務](https://docs.microsoft.com/azure/dms/dms-overview)，將內部部署 Windows 應用程式遷移至 azure web 應用程式，並將應用程式資料庫移轉至 azure SQL Server 實例。 |
| [使用 App Service 和 SQL 受控執行個體重構 Windows 應用程式](./contoso-migration-refactor-web-app-sql-managed-instance.md) | 此範例示範如何使用[資料庫移轉服務](https://docs.microsoft.com/azure/dms/dms-overview)，將內部部署 Windows 應用程式遷移至 Azure web 應用程式，並將應用程式資料庫移轉至 SQL 受控執行個體。 |
| [使用 App Service、Azure 流量管理員和適用於 MySQL 的 Azure 資料庫，將 Linux 應用程式重構至多個區域](./contoso-migration-refactor-linux-app-service-mysql.md) | 此範例示範如何使用流量管理員與 GitHub 整合以持續傳遞，將內部部署的 Linux 應用程式遷移至多個 Azure 區域上的 Azure web 應用程式。 應用程式資料庫會遷移至適用於 MySQL 的 Azure 資料庫實例。 |
| [在 Azure 中重建應用程式](./contoso-migration-rebuild.md) | 本文提供使用各種 Azure 功能和受控服務來重建內部部署應用程式的範例。 這些功能和服務包括 App Service、AKS、函數、Azure 認知服務和 Azure Cosmos DB。 |
| [重構 Team Foundation Server 至 Azure DevOps Services](./contoso-migration-tfs-vsts.md) | 本文中的範例說明將內部部署 Team Foundation Server 部署移轉至 Azure 中的 Azure DevOps Services。 |

### <a name="sap"></a>SAP

| 發行項 | 詳細資料 |
| --- | --- |
| [SAP 移轉指南](https://azure.microsoft.com/resources/sap-on-azure-implementation-guide) | 取得將您的內部部署 SAP 工作負載移至雲端的實用指引。 |
| [將 SAP 應用程式遷移至 Azure](https://azure.microsoft.com/resources/migrating-sap-applications-to-azure) | 雲端的 SAP 旅程白皮書和藍圖。 |
| [Azure 上的 SAP 移轉方法](https://azure.microsoft.com/resources/migration-methodologies-for-sap-on-azure) | 概述將 SAP 應用程式移至 Azure 的各種遷移選項。 |

### <a name="specialized-workloads"></a>特製化工作負載

| 發行項 | 詳細資料 |
| --- | --- |
| [將內部部署 VMware 基礎結構移至 Azure](./contoso-migration-vmware-to-azure.md) | 本文提供使用 Azure VMware 解決方案將內部部署 VMware Vm 移至 Azure 的範例。 |
| [Azure NetApp Files](https://azure.microsoft.com/services/netapp) | 由 NetApp 提供技術支援的企業檔案儲存體。 在 Azure 中執行 Linux 和 Windows 檔案工作負載。 |
| [Oracle on Azure](https://azure.microsoft.com/solutions/oracle) | 在 Azure 和 Oracle 雲端中執行您的 Oracle 資料庫和企業應用程式。 |
| [Azure 中的 Cray](https://azure.microsoft.com/solutions/high-performance-computing/cray) | 在 Azure 中使用 Cray 的高效能計算。 虛擬網路上的專用超級電腦。 |

### <a name="vdi"></a>VDI

| 發行項 | 詳細資料 |
| --- | --- |
| [將內部部署遠端桌面服務移至 Azure 中的 Windows 虛擬桌面](./contoso-migration-rds-to-wvd.md) | 本文說明如何將內部部署遠端桌面服務遷移至 Azure 中的 Windows 虛擬桌面。 |

### <a name="migration-scaling"></a>移轉調整

| 發行項 | 詳細資料 |
| --- | --- |
| [對 Azure 進行大規模移轉](./contoso-migration-scale.md) | 本文說明範例組織如何準備調整以完整遷移至 Azure。 |

### <a name="demo-applications"></a>示範應用程式

<!-- docsTest:ignore SmartHotel360 osTicket -->

本節所提供的範例文章使用兩個示範應用程式： SmartHotel360 和 osTicket。

**SmartHotel360**：此測試應用程式是由 Microsoft 在您使用 Azure 時所開發。 它是以開放原始碼的形式提供，您可以從[GitHub](https://github.com/Microsoft/SmartHotel360)下載。 它是連接到 SQL Server 資料庫的 ASP.NET 應用程式。 在這些文章中討論的案例中，此應用程式的目前版本會部署到兩個執行 Windows Server 2008 R2 和 SQL Server 2008 R2 的 VMware Vm。 這些應用程式 Vm 裝載于內部部署環境，並由 vCenter Server 管理。

**osTicket**：這個開放原始碼服務台票證應用程式會在 Linux 上執行。 您可以從 [GitHub](https://github.com/osTicket/osTicket) 下載。 在這些文章中討論的案例中，此應用程式的目前版本會部署到使用 Apache 2、PHP 7.0 和 MySQL 5.7 執行 Ubuntu 16.04 LTS 的兩個 VMware Vm。
