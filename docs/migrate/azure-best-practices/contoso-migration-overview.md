---
title: Azure 的應用程式移轉範例概觀
description: 概述雲端採用架構遷移方法中包含的應用程式遷移範例。
author: deltadan
ms.author: abuck
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: think-tank
ms.openlocfilehash: fe416b568b2133d9ff1cd27c5694c98f2925cfce
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101789961"
---
# <a name="overview-of-application-migration-examples-for-azure"></a>Azure 的應用程式移轉範例概觀

適用于 Azure 的雲端採用架構的這一節會提供數個常見遷移案例的範例，並示範如何將內部部署基礎結構遷移至 [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/)。

## <a name="introduction"></a>簡介

Azure 提供全方位雲端服務組合的存取權。 作為開發人員和 IT 專業人員，您可以使用這些服務，透過全球資料中心網路在各種工具和架構上建立、部署和管理應用程式。 當您的業務面臨與數位轉班相關的挑戰時，Azure 平臺可協助您找出如何：

- 優化資源和作業。
- 與您的客戶和員工互動。
- 轉換您的產品。

雲端提供速度和彈性、最小化成本、效能和可靠性等優點。 但許多組織都必須繼續在內部部署資料中心執行。 為了回應雲端採用的障礙，Azure 提供了混合式雲端策略，可在您的內部部署資料中心與 Azure 公用雲端之間建立橋樑。 例如，使用 azure 備份等 Azure 雲端資源來保護內部部署資源或 Azure 分析，以深入瞭解內部部署工作負載。

Azure 是混合式雲端策略的一部分，可提供將內部部署應用程式和工作負載遷移至雲端的不斷成長解決方案。 您可以使用簡單的步驟，全面評估您的內部部署資源，以找出它們在 Azure 平臺中的執行方式。 然後，在有了深入評估的情況下，您可以放心地將資源遷移至 Azure。 當資源在 Azure 中啟動並執行時，您可以將它們最佳化以維持和改善存取、彈性、安全性與可靠性。

## <a name="migration-patterns"></a>移轉模式

移轉至雲端的策略可分成以下四大模式：重新裝載、重構、重新架構或重建。 您採用的策略取決於商業誘因和移轉目標。 您可以採用多種模式。 例如，您可以選擇重新裝載非關鍵性的應用程式，同時重新架構更複雜且更具商務關鍵性的應用程式。 我們來看一下這些模式。

| 模式 | 定義 | 使用時機 |
| --- | --- | --- |
| **重新裝載** | 通常稱為「隨即轉移」，此選項不需要變更程式碼。 您可以使用它來快速將現有的應用程式遷移至 Azure。 每個應用程式都會依原樣遷移，以充分利用雲端的優點，而不會有與程式碼變更相關聯的風險和成本。 | 當您需要快速將應用程式移至雲端時。 <br><br> 當您想要移動應用程式而不加以修改。 <br><br> 當您的應用程式已設計為可利用 [Azure 基礎結構即服務， (IaaS ](https://azure.microsoft.com/overview/what-is-iaas/) 在遷移後) 擴充性。 <br><br> 當應用程式對您的業務很重要時，但您不需要立即變更應用程式功能。 |
| **重構** | 重構通常稱為「重新封裝」，需要對應用程式進行極少量的變更，讓他們可以連線到 [Azure 平臺即服務 (PaaS) ](https://azure.microsoft.com/overview/what-is-paas/) 並使用雲端供應專案。 <br><br> 例如，您可以將現有的應用程式遷移至 Azure App Service 或 Azure Kubernetes Service (AKS) 。 <br><br> 或者，您可以將關聯式和非關聯式資料庫重構為 Azure SQL 受控實例、適用于 MySQL 的 Azure 資料庫、適用于于 postgresql 的 azure 資料庫和 Azure Cosmos DB 等選項。 | 如果您的應用程式可以輕鬆地重新封裝以在 Azure 中運作。 <br><br> 如果您想要套用 Azure 所提供的創新 DevOps 實務，或想要使用適用于工作負載的容器策略來考慮 DevOps。 <br><br> 針對重構，您需要考慮現有程式碼基底的可攜性和可用的開發技能。 |
| **重新架構** | 重新架構 for 遷移著重于修改和擴充應用程式功能和程式碼基底，以優化雲端擴充性的應用程式架構。 <br><br> 例如，您可將單一應用程式劃分為數個微服務群組，如此即能輕鬆搭配運作和擴充。 <br><br> 您也可以將關聯式和非關聯式資料庫重新架構到完全受控的資料庫解決方案，例如 SQL 受控實例、適用于 MySQL 的 Azure 資料庫、適用于于 postgresql 的 Azure 資料庫，以及 Azure Cosmos DB。 | 當您的應用程式需要主要修訂，以納入新功能或在雲端平臺上有效運作時。 <br><br> 當您想要使用現有的應用程式投資時，請滿足擴充性需求、套用創新的 DevOps 實務，以及將虛擬機器的使用降至最低。 |
| **重建** | 重建會使用 Azure 雲端技術從頭重建應用程式，以進行進一步的步驟。 <br><br> 例如，您可以使用 Azure 函式、AI、SQL 受控實例和 Azure Cosmos DB 等 [雲端原生](https://azure.microsoft.com/overview/cloudnative/) 技術來建立 greenfield 應用程式。 | 當您想要快速開發，且現有應用程式的功能和生命週期有限時。 <br><br> 準備加速商務創新 (包括 Azure 提供的 DevOps 做法)、使用雲端原生技術建置全新應用程式，以及利用 AI、區塊鏈和 IoT 領域的先進技術時。 |

## <a name="migration-example-articles"></a>移轉範例文章

本節提供數個常見遷移案例的範例。 每個範例都包含背景資訊和詳細的部署案例，說明如何設定遷移基礎結構，以及評估內部部署資源的適用性以進行遷移。 本節後續將新增更多文章。

![遷移和現代化專案分類的圖表。 ](./media/migration-patterns.png)
*圖1：常見的遷移和現代化專案類別。*

此系列著重于每個遷移案例，這是以稍微不同的商務目標來決定遷移策略所驅動。 針對每個部署案例，我們提供下列資訊：

- 商業驅動程式和目標。
- 建議的架構。
- 執行遷移的步驟。
- 在遷移後，清除和後續步驟的建議已完成。

### <a name="assessment"></a>評量

| 發行項 | 詳細資料 |
| --- | --- |
| [評估要遷移至 Azure 的內部部署資源](../../plan/contoso-migration-assessment.md) | 此計畫方法中的最佳做法文章會討論如何針對在 VMware 上執行的內部部署應用程式執行評量。 在本文中，範例組織會使用 Azure 遷移來評估應用程式 Vm，並使用資料移轉小幫手來評估應用程式 SQL Server 資料庫。 |

### <a name="infrastructure"></a>基礎結構

| 發行項 | 詳細資料 |
| --- | --- |
| [部署 Azure 基礎結構](./contoso-migration-infrastructure.md) | 本文說明組織如何準備其內部部署基礎結構和其 Azure 基礎結構以進行移轉。 本文中建立的基礎結構範例，會供本節中提供的其他範例參考。 |

### <a name="windows-server-workloads"></a>Windows Server 工作負載

| 發行項 | 詳細資料 |
| --- | --- |
| [在 Azure Vm 上重新裝載應用程式](./contoso-migration-rehost-vm.md) | 本文提供使用 Azure 遷移將內部部署應用程式 Vm 遷移至 Azure Vm 的範例。 |

### <a name="sql-server-workloads"></a>SQL Server 工作負載

| 發行項 | 詳細資料 |
| --- | --- |
| [移轉 SQL Server 資料庫到 Azure](./contoso-migration-sql-server-db-to-azure.md) | 本文示範虛構公司 Contoso 如何評估、規劃及遷移其各種內部部署 SQL Server 資料庫至 Azure。 |
| [在 Azure VM 和 SQL 受控實例上重新裝載應用程式](./contoso-migration-rehost-vm-sql-managed-instance.md) | 本文提供一個範例，說明如何將內部部署應用程式隨即轉移至 Azure。 此程式牽涉到使用 azure [資料庫移轉服務](/azure/dms/dms-overview)，將 azure 遷移和應用程式資料庫移轉至 SQL 受控實例，以將應用程式前端 VM 遷移至 SQL 受控實例。 |
| [使用 SQL Server Always On 可用性群組在 Azure Vm 上重新裝載應用程式](./contoso-migration-rehost-vm-sql-ag.md) | 此範例說明如何使用 Azure 裝載的 SQL Server Vm 來遷移應用程式和資料。 它會使用 Azure 遷移來遷移應用程式 Vm 和資料庫移轉服務，以將應用程式資料庫移轉至受 Always On 可用性群組保護的 SQL Server 叢集。 |

### <a name="linux-and-open-source-databases"></a>Linux 和開放原始碼資料庫

| 發行項 | 詳細資料 |
| --- | --- |
| [將開放原始碼資料庫遷移到 Azure](./contoso-migration-oss-db-to-azure.md) | 本文示範虛構公司 Contoso 如何評估、規劃及遷移其各種內部部署開放原始碼資料庫至 Azure。 |
| [將 MySQL 遷移到 Azure](./contoso-migration-mysql-to-azure.md) | 本文示範虛構公司 Contoso 如何規劃和遷移其內部部署 MySQL 開放原始碼資料庫平臺到 Azure。 |
| [將 PostgreSQL 遷移到 Azure](./contoso-migration-postgresql-to-azure.md) | 本文示範虛構公司 Contoso 如何規劃和遷移其內部部署于 postgresql 開放原始碼資料庫平臺到 Azure。 |
| [將 MariaDB 遷移到 Azure](./contoso-migration-mariadb-to-azure.md) | 本文示範虛構公司 Contoso 如何規劃和遷移其內部部署適用于 mariadb 開放原始碼資料庫平臺到 Azure。 |
| [在 Azure Vm 和適用于 MySQL 的 Azure 資料庫上重新裝載 Linux 應用程式](./contoso-migration-rehost-linux-vm-mysql.md) | 本文提供使用 Azure 遷移將 Linux 裝載的應用程式遷移至 Azure Vm 的範例。 使用 [資料庫移轉服務](/azure/dms/dms-overview)，將應用程式資料庫移轉至適用于 MySQL 的 Azure 資料庫。 |
| [在 Azure Vm 上重新裝載 Linux 應用程式](./contoso-migration-rehost-linux-vm.md) | 此範例說明如何使用 Azure 遷移來完成將 Linux 應用程式隨即轉移至 Azure Vm 的工作。 |

### <a name="devtest-workloads"></a>開發/測試工作負載

| 發行項 | 詳細資料 |
| --- | --- |
| [將開發/測試環境遷移至 Azure IaaS](./contoso-migration-devtest-to-iaas.md) | 本文示範 Contoso 如何藉由遷移至 Azure Vm，為 VMware Vm 上執行的兩個應用程式如何其開發/測試環境。 |
| [遷移到 Azure DevTest Labs](./contoso-migration-devtest-to-labs.md) | 本文討論 Contoso 如何使用 DevTest Labs 將其開發/測試工作負載移至 Azure。 |

### <a name="aspnet-and-php-web-apps"></a>ASP.NET 和 PHP web 應用程式

| 發行項 | 詳細資料 |
| --- | --- |
| [使用 App Service 和 SQL Database 重構 Windows 應用程式](./contoso-migration-refactor-web-app-sql.md) | 此範例說明如何使用 [資料庫移轉服務](/azure/dms/dms-overview)，將內部部署 Windows 應用程式遷移至 azure web 應用程式，並將應用程式資料庫移轉至 Azure SQL database 伺服器實例。 |
| [使用 App Service 和 SQL 受控實例重構 Windows 應用程式](./contoso-migration-refactor-web-app-sql-managed-instance.md) | 此範例說明如何使用 [資料庫移轉服務](/azure/dms/dms-overview)，將內部部署 Windows 應用程式遷移至 Azure web 應用程式，並將應用程式資料庫移轉至 SQL 受控實例。 |
| [使用 App Service、Azure 流量管理員和適用于 MySQL 的 Azure 資料庫，將 Linux 應用程式重構至多個區域](./contoso-migration-refactor-linux-app-service-mysql.md) | 此範例示範如何使用流量管理員與 GitHub 整合，以在多個 Azure 區域上將內部部署 Linux 應用程式遷移至 Azure web 應用程式，以進行持續傳遞。 應用程式資料庫會遷移至適用于 MySQL 的 Azure 資料庫實例。 |
| [在 Azure 中重建應用程式](./contoso-migration-rebuild.md) | 本文提供使用 Azure 功能和受控服務的範圍來重建內部部署應用程式的範例。 這些功能和服務包括 App Service、AKS、Azure 函數、Azure 認知服務和 Azure Cosmos DB。 |
| [將 Team Foundation Server 重構至 Azure DevOps Services](./contoso-migration-tfs-vsts.md) | 本文中的範例說明將內部部署 Team Foundation Server 部署移轉至 Azure 中的 Azure DevOps Services。 |

### <a name="sap"></a>SAP

| 發行項 | 詳細資料 |
| --- | --- |
| [SAP 移轉指南](https://azure.microsoft.com/resources/sap-on-azure-implementation-guide/) | 取得將內部部署 SAP 工作負載移至雲端的實用指導方針。 |
| [將 SAP 應用程式遷移至 Azure](https://azure.microsoft.com/resources/migrating-sap-applications-to-azure/) | 適用于 SAP 旅程至雲端的白皮書和藍圖。 |
| [Azure 上的 SAP 移轉方法](https://azure.microsoft.com/resources/migration-methodologies-for-sap-on-azure/) | 概述將 SAP 應用程式移至 Azure 的各種遷移選項。 |

### <a name="specialized-workloads"></a>特製化工作負載

| 發行項 | 詳細資料 |
| --- | --- |
| [將內部部署 VMware 基礎結構移至 Azure](./contoso-migration-vmware-to-azure.md) | 本文提供使用 Azure VMware 解決方案將內部部署 VMware Vm 移至 Azure 的範例。 |
| [Azure NetApp Files](https://azure.microsoft.com/services/netapp/) | 由 NetApp 提供技術支援的企業檔案儲存體。 在 Azure 中執行 Linux 和 Windows 檔案工作負載。 |
| [Oracle on Azure](https://azure.microsoft.com/solutions/oracle/) | 在 Azure 和 Oracle 雲端基礎結構中執行您的 Oracle 資料庫和企業應用程式。 |
| [Azure 中的 Cray](https://azure.microsoft.com/solutions/high-performance-computing/cray/) | 在 Azure 中使用 Cray 進行高效能運算。 您虛擬網路上的專用超級電腦。 |

### <a name="vdi"></a>VDI

| 發行項 | 詳細資料 |
| --- | --- |
| [將內部部署的遠端桌面服務移至 Azure 中的 Windows 虛擬桌面](./contoso-migration-rds-to-wvd.md) | 本文說明如何將內部部署遠端桌面服務遷移至 Azure 中的 Windows 虛擬桌面。 |

### <a name="migration-scaling"></a>移轉調整

| 發行項 | 詳細資料 |
| --- | --- |
| [對 Azure 進行大規模移轉](./contoso-migration-scale.md) | 本文說明範例組織如何準備調整以完整遷移至 Azure。 |

### <a name="demo-applications"></a>示範應用程式

本節提供的範例文章會使用兩個示範應用程式： SmartHotel360 和 osTicket。

**SmartHotel360**：此測試應用程式是由 Microsoft 開發，以在您使用 Azure 時使用。 它是以開放原始碼授權提供，您可以從 [GitHub](https://github.com/Microsoft/SmartHotel360)下載。 它是連接到 SQL Server 資料庫的 ASP.NET 應用程式。 在這些文章所討論的案例中，此應用程式的目前版本會部署至執行 Windows Server 2008 R2 和 SQL Server 2008 R2 的兩個 VMware Vm。 這些應用程式 Vm 裝載于內部部署環境，並由 vCenter Server 管理。

**osTicket**：此開放原始碼服務台票證應用程式會在 Linux 上執行。 您可以從 [GitHub](https://github.com/osTicket/osTicket) 下載。 在這些文章所討論的案例中，此應用程式的目前版本會使用 Apache 2、PHP 7.0 和 MySQL 5.7，在內部部署環境部署至執行 Ubuntu 16.04 LTS 的兩部 VMware Vm。
