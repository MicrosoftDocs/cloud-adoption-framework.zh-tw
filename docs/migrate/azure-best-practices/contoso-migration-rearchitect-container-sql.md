---
title: 在 Azure 容器和 Azure SQL Database 中重新建構應用程式
description: 使用適用于 Azure 的雲端採用架構，以瞭解如何將內部部署應用程式重新架構至 Azure 容器和 Azure SQL database。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/11/2018
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: site-recovery
ms.openlocfilehash: 37bf2a4d96cc1f60b351f40f6a2c51c2ea1dcf95
ms.sourcegitcommit: 5411c3b64af966b5c56669a182d6425e226fd4f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/13/2020
ms.locfileid: "79311637"
---
<!-- cSpell:ignore reqs contosohost contosodc contosoacreus contososmarthotel smarthotel vcenter WEBVM SQLVM -->

# <a name="rearchitect-an-on-premises-app-to-an-azure-container-and-azure-sql-database"></a>將內部部署應用程式重新建構至 Azure 容器和 Azure SQL Database

本文示範虛構公司 Contoso 如何在移轉至 Azure 的過程中，重新建構在 VMware VM 上執行的兩層式 Windows .NET 應用程式。 Contoso 會將應用程式前端 VM 移轉至 Azure Windows 容器，並將應用程式資料庫移轉至 Azure SQL 資料庫。

此範例中使用的 SmartHotel360 應用程式以開放原始碼的形式提供。 如果想將它用於自己的測試目的，您可以從 [github](https://github.com/Microsoft/SmartHotel360) 進行下載。

## <a name="business-drivers"></a>商業動機

Contoso IT 領導小組與商務合作夥伴密切合作，以了解此次移轉所要實現的目標：

- **因應業務成長。** Contoso 的業務量日益增多，對其內部部署系統和基礎結構造成了壓力。
- **提高效率。** Contoso 必須移除不必要的程序，並且簡化開發人員和使用者的程序。 企業亟需快速且不浪費時間或金錢的 IT 服務，進而更快滿足客戶的需求。
- **提高靈活性。** Contoso IT 必須能夠更快因應企業的需求。 其因應速度必須能夠比市場變化更快，才能更在全球經濟中獲致成功。 它不得礙事，或成為企業的絆腳石。
- **調整。** 隨著企業順利成長，Contoso IT 必須提供可依相同步調成長的系統。
- **降低成本。** Contoso 想要將授權費用降至最低。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已針對此次移轉擬定好各項目標。 並用這些目標用來判斷最合適的移轉方法。

<!-- markdownlint-disable MD033 -->

**目標** | **詳細資料**
--- | ---
**應用程式需求** | Azure 中的應用程式應與現在一樣重要。<br/><br/> 其效能應與目前在 VMware 中的效能相同。<br/><br/> Contoso 想要停止支援目前裝載應用程式的 Windows Server 2008 R2，而 Contoso 願意投資應用程式。<br/><br/> Contoso 想要從 SQL Server 2008 R2 移到現代化的受控資料庫平臺，讓管理的需求降到最低。<br/><br/> Contoso 想要盡可能運用其在 SQL Server 授權及軟體保證中所做的投資。<br/><br/> Contoso 想要視需要調整應用程式 web 層。
**限制** | 應用程式由相同 VM 上執行的 ASP.NET 應用程式和 WCF 服務所組成。 Contoso 想要使用 Azure App Service 將其分成兩個 Web 應用程式。
**Azure 需求** | Contoso 想要將應用程式移至 Azure，並在容器中執行以延長應用程式的壽命。 其不想要完全從頭開始實作 Azure 中的應用程式。
**DevOps** | Contoso 想要移轉至 DevOps 模型，使用 Azure DevOps Services 來處理程式碼的建置和發行管線。

<!-- markdownlint-enable MD033 -->

## <a name="solution-design"></a>解決方案設計

擬定好目標和需求後，Contoso 會設計和檢閱部署解決方案，並識別移轉程序，包括 Contoso 會用於移轉的 Azure 服務。

### <a name="current-app"></a>目前的應用程式

- SmartHotel360 內部部署應用程式會分層至兩個 VM (WEBVM 和 SQLVM)。
- 這些 VM 位於 VMware ESXi 主機 **contosohost1.contoso.com** (6.5 版)
- VMware 環境是由 VM 上執行的 vCenter Server 6.5 (**vcenter.contoso.com**) 進行管理。
- Contoso 有內部部署資料中心 (contoso-datacenter) 以及內部部署網域控制站 (**contosodc1**)。
- 移轉完成之後，將會解除委任 Contoso 資料中心的內部部署 VM。

### <a name="proposed-architecture"></a>建議的架構

- 針對應用程式的資料庫層，Contoso 已透過[這篇文章](https://docs.microsoft.com/azure/sql-database/sql-database-features)來比較 Azure SQL Database 和 SQL Server。 因為下列幾個原因，其決定使用 Azure SQL Database：
  - Azure SQL Database 是由關聯式資料庫管理的服務。 其在多個服務層級上提供可預測的效能，而且幾乎免管理。 優點包括無須停機的動態延展性、內建智慧最佳化及全球延展性和可用性。
  - Contoso 會使用輕量型 Data Migration Assistant (DMA) 來評估內部部署資料庫，並將其移轉至 Azure SQL。
  - 透過軟體保證，Contoso 可以使用適用於 SQL Server 的 Azure Hybrid Benefit，以折扣優惠在 SQL Database 上交換其現有授權。 這可以提供最多 30% 的折扣。
  - SQL Database 提供數種安全性功能，包括永遠加密、動態資料遮罩和資料列層級的安全性/威脅偵測。
- 針對應用程式 Web 層，Contoso 已決定使用 Azure DevOps Services 將其轉換成 Windows 容器。
  - Contoso 會使用 Azure Service Fabric 部署應用程式，並從 Azure Container Registry (ACR) 提取 Windows 容器映像。
  - 用來擴充應用程式以包含情感分析的原型會實作為 Service Fabric 中的另一個服務，並連線至 Cosmos DB。 這將會讀取推文中的資訊，並顯示在應用程式上。
- 為了實作 DevOps 管線，Contoso 會使用 Azure DevOps 搭配 Git 存放庫來進行原始程式碼管理 (SCM)。 其會使用自動化建置和發行來建置程式碼，並將程式碼部署至 Azure Container Registry 和 Azure Service Fabric。

    ![案例架構](./media/contoso-migration-rearchitect-container-sql/architecture.png)

### <a name="solution-review"></a>解決方案檢閱

Contoso 會透過比較一份優缺點清單，來評估建議設計。

<!-- markdownlint-disable MD033 -->

**考量** | **詳細資料**
--- | ---
**優點** | 必須變更 SmartHotel360 應用程式的程式碼才能移轉至 Azure Service Fabric。 不過，這很簡單，使用 Service Fabric SDK Tools 來執行變更即可。<br/><br/> 移轉到 Service Fabric 後，Contoso 可以開始開發微服務，並在一段時間後快速地將微服務新增至應用程式，而這完全不會對原始程式碼基底造成不良影響。<br/><br/> Windows 容器會提供與一般容器相同的優點。 其提升了靈活度、可攜性和控制能力。<br/><br/> Contoso 可以使用 SQL Server 和 Windows Server 的 Azure Hybrid Benefit 來妥善運用軟體保證中的投資。<br/><br/> 在移轉之後，其不會再需要支援 Windows Server 2008 R2。 [詳細資訊](https://support.microsoft.com/lifecycle)。<br/><br/> Contoso 可以使用多個執行個體來設定應用程式的 Web 層，使它不再是單一失敗點。<br/><br/> 它將不再依賴過時的 SQL Server 2008 R2。<br/><br/> SQL Database 可支援 Contoso 的技術需求。 Contoso 管理員使用 Data Migration Assistant 來評估內部部署資料庫，並發現其可相容。<br/><br/> SQL Database 擁有內建容錯功能，無須 Contoso 進行設定。 這可確保資料層不再是單一的容錯移轉點。
**缺點** | 容器比其他移轉選項複雜得多。 容器上的學習曲線 (learning curve) 可能會是 Contoso 的難題。 儘管有學習曲線的問題，但容器帶來了新的複雜度等級，而可提供許多價值。<br/><br/> Contoso 的營運團隊必須提升能力，以了解和支援適用於應用程式的 Azure、容器和微服務。<br/><br/> 如果 Contoso 使用 Data Migration Assistant 而非 Azure 資料庫移轉服務來遷移資料庫，則不會有基礎結構可供大規模遷移資料庫。

<!-- markdownlint-enable MD033 -->

### <a name="migration-process"></a>移轉程序

1. Contoso 會佈建適用於 Windows 的 Azure 服務網狀架構叢集。

1. 它會佈建 Azure SQL 執行個體，並將 SmartHotel360 資料庫移轉至此。

1. Contoso 會使用 Service Fabric SDK Tools 將 Web 層 VM 轉換成 Docker 容器。

1. 其會使服務網狀架構叢集與 ACR 連線，並使用 Azure 服務網狀架構部署應用程式。

    ![移轉程序](./media/contoso-migration-rearchitect-container-sql/migration-process.png)

### <a name="azure-services"></a>Azure 服務

**服務** | **說明** | **成本**
--- | --- | ---
[Data Migration Assistant (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | 評估和偵測可能對 Azure 中的資料庫功能造成影響的相容性問題。 DMA 會評定 SQL 來源與目標之間的功能同位，並提出效能和可靠性改善建議。 | 此工具可免費下載。
[Azure SQL Database](https://azure.microsoft.com/services/sql-database) | 提供完全受控的智慧型關聯式雲端資料庫服務。 | 根據功能、輸送量和大小計算費用。 [詳細資訊](https://azure.microsoft.com/pricing/details/sql-database/managed)。
[Azure Container Registry](https://azure.microsoft.com/services/container-registry) | 儲存所有容器部署類型的映像。 | 根據功能、儲存體和使用期間計算費用。 [詳細資訊](https://azure.microsoft.com/pricing/details/container-registry)。
[Azure Service Fabric](https://azure.microsoft.com/services/service-fabric) | 建置及操作全年持續運作，並能隨時調整的分散式應用程式 | 根據大小、位置和計算節點的持續時間計算費用。 [詳細資訊](https://azure.microsoft.com/pricing/details/service-fabric)。
[Azure DevOps](https://docs.microsoft.com/azure/azure-portal/tutorial-azureportal-devops) | 為應用程式開發提供持續整合和持續部署 (CI/CD) 管線。 管線一開始會有一個用於管理應用程式程式碼的 Git 存放庫、一個用於產生套件及其他建置成品的建置系統，以及一個用來在開發、測試及生產環境中部署變更的「發行管理」系統。

## <a name="prerequisites"></a>Prerequisites

以下是 Contoso 要執行此案例所需的項目：

<!-- markdownlint-disable MD033 -->

**需求** | **詳細資料**
--- | ---
**Azure 訂用帳戶** | Contoso 稍早已在本文章系列中建立訂用帳戶。 如果您沒有 Azure 訂用帳戶，請建立[免費帳戶](https://azure.microsoft.com/pricing/free-trial)。<br/><br/> 如果您建立免費帳戶，您就是訂用帳戶的管理員，並可執行所有動作。<br/><br/> 如果您使用現有訂用帳戶，而且您不是系統管理員，則需要與系統管理員合作，讓其指派擁有者或參與者權限給您。
**Azure 基礎結構** | [了解](./contoso-migration-infrastructure.md) Contoso 先前如何設定 Azure 基礎結構。
**開發人員必要條件** | 在開發人員工作站上，Contoso 需要下列工具：<br/><br/> - [Visual Studio 2017 Community 版本：15.5 版](https://www.visualstudio.com)<br/><br/> - 已啟用.NET 工作負載。<br/><br/> - [Git](https://git-scm.com)<br/><br/> - [Service Fabric SDK v 3.0 或更新版本](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)<br/><br/> 已設定為使用 Windows 容器的 - [Docker CE (Windows 10) 或 Docker EE (Windows Server)](https://docs.docker.com/docker-for-windows/install)。

<!-- markdownlint-enable MD033 -->

## <a name="scenario-steps"></a>案例步驟

以下是 Contoso 執行移轉的方式：

> [!div class="checklist"]
>
> - **步驟1：在 Azure 中布建 SQL Database 實例。** Contoso 會在 Azure 中佈建 SQL 執行個體。 將前端 Web VM 移轉至 Azure 容器後，應用程式 Web 前端的容器執行個體會指向這個資料庫。
> - **步驟2：建立 Azure Container Registry （ACR）。** Contoso 會為 Docker 容器映像佈建企業容器登錄。
> - **步驟3：布建 Azure Service Fabric。** 佈建 Service Fabric 叢集。
> - **步驟4：管理 service fabric 憑證。** Contoso 會設定憑證以供 Azure DevOps Services 存取叢集。
> - **步驟5：使用 DMA 遷移資料庫。** 它會使用 Data Migration Assistant 來移轉應用程式資料庫。
> - **步驟6：設定 Azure DevOps Services。** Contoso 會在 Azure DevOps Services 中設定新的專案，並將程式碼匯入至 Git 存放庫。
> - **步驟7：轉換應用程式。** Contoso 會使用 Azure DevOps 和 SDK 工具將應用程式轉換成容器。
> - **步驟8：設定組建和發行。** Contoso 會設定建置和發行管線，以建立應用程式並發行至 ACR 和 Service Fabric 叢集。
> - **步驟9：擴充應用程式。** 公開應用程式之後，Contoso會將其擴充以善用 Azure 功能，並使用管線將其重新發行至 Azure。

## <a name="step-1-provision-an-azure-sql-database"></a>步驟 1：佈建 Azure SQL Database

Contoso 管理員會佈建 Azure SQL 資料庫。

1. 他們會選擇在 Azure 中建立 **SQL Database**。

    ![佈建 SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql1.png)

1. 他們會指定與內部部署 VM 上執行的資料庫相符的資料庫名稱 (**SmartHotel.Registration**)。 他們會將資料庫置於 ContosoRG 資源群組。 這是用於 Azure 中生產資源的資源群組。

    ![佈建 SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql2.png)

1. 在主要區域中設定新的 SQL Server 執行個體 (**sql-smarthotel-eus2**)。

    ![佈建 SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql3.png)

1. 他們會設定符合伺服器和資料庫需求的定價層。 並選擇使用 Azure Hybrid Benefit 來節省成本，因為他們已經有 SQL Server 授權。

1. 針對大小，他們使用以虛擬核心為基礎的購買方式，並為預期的需求設定限制。

    ![佈建 SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql4.png)

1. 然後，他們會建立資料庫執行個體。

    ![佈建 SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql5.png)

1. 建立執行個體之後，他們會開啟資料庫，並記下在使用 Data Migration Assistant 進行移轉時所需的詳細資料。

    ![佈建 SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql6.png)

**需要其他協助？**

- [說明](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal)如何佈建 SQL Database。
- [深入了解](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools)虛擬核心的資源限制。

## <a name="step-2-create-an-acr-and-provision-an-azure-container"></a>步驟 2：建立 ACR 和佈建 Azure 容器

Azure 容器會使用 Web VM 的匯出檔案來建立。 容器會裝載在 Azure Container Registry (ACR)。

1. Contoso 管理員會在 Azure 入口網站中建立 Container Registry。

     ![Container Registry](./media/contoso-migration-rearchitect-container-sql/container-registry1.png)

1. 他們會提供登錄的名稱 (**contosoacreus2**)，並將它放在主要區域中，也就是基礎結構資源所用的資源群組。 他們會啟用管理使用者的存取權，並將其設為進階 SKU，以便使用異地複寫。

    ![Container Registry](./media/contoso-migration-rearchitect-container-sql/container-registry2.png)

## <a name="step-3-provision-azure-service-fabric"></a>步驟 3：佈建 Azure Service Fabric

SmartHotel360 容器會在 Azure Service Fabric 叢集中執行。 Contoso 管理員會建立 Service Fabric 叢集，如下所示：

1. 從 Azure Marketplace 建立 Service Fabric 資源。

     ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric1.png)

1. 在 [基本] 中，他們會提供叢集的唯一 DS 名稱，以及用來存取內部部署 VM 的認證。 他們會將資源放在主要區域 (美國東部 2) 的生產資源群組 (**ContosoRG**) 中。

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric2.png)

1. 在 [節點類型組態] 中，他們會輸入節點類型名稱、持久性設定、VM 大小和應用程式端點。

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric3.png)

1. 在 [建立金鑰保存庫] 中，他們會在其基礎結構資源群組中建立新的金鑰保存庫，用以存放憑證。

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric4.png)

1. 在 [存取原則] 中，他們會啟用對虛擬機器的存取，以部署金鑰保存庫。

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric5.png)

1. 指定憑證的名稱。

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric6.png)

1. 在 [摘要] 頁面中，他們會複製用來下載憑證的連結。 這是連線至 Service Fabric 叢集的必要項目。

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric7.png)

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric8.png)

1. 通過驗證之後，他們就會佈建叢集。

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric9.png)

1. 在 [憑證匯入精靈] 中，他們會將下載的憑證匯入開發人員機器。 憑證會用來對叢集進行驗證。

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric10.png)

1. 佈建叢集之後，他們就可連線至 Service Fabric 叢集的 Explorer。

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric11.png)

1. 必須選取正確的憑證。

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric12.png)

1. Service Fabric Explorer 會隨即載入，而 Contoso 系統管理員可以管理該叢集。

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric13.png)

## <a name="step-4-manage-service-fabric-certificates"></a>步驟 4：管理 Service Fabric 憑證

Contoso 必須有叢集憑證，才能允許 Azure DevOps Services 存取叢集。 Contoso 管理員會就此進行設定。

1. 他們會開啟 Azure 入口網站，並瀏覽至 Key Vault。

1. 其會開啟憑證，並複製佈建程序進行期間所建立憑證的指紋。

    ![複製指紋](./media/contoso-migration-rearchitect-container-sql/cert1.png)

1. 其會將指紋複製到文字檔以供日後參考。

1. 現在，其會新增用戶端憑證，從而作為管理員在叢集上的用戶端憑證。 這可讓 Azure DevOps Services 連線到叢集，以便在發行管線中部署應用程式。 若要這麼做，他們會在入口網站中開啟 Key Vault，然後選取 [**憑證**] > [**產生/匯入**]。

    ![產生用戶端憑證](./media/contoso-migration-rearchitect-container-sql/cert2.png)

1. 其會輸入憑證的名稱，並在 [主旨] 中提供 X.509 辨別名稱。

     ![憑證名稱](./media/contoso-migration-rearchitect-container-sql/cert3.png)

1. 其會在憑證建立好之後，將憑證以 PFX 格式下載到本機。

     ![下載憑證](./media/contoso-migration-rearchitect-container-sql/cert4.png)

1. 現在，他們會回到 Key Vault 中的憑證清單，並對剛建立好的用戶端憑證複製其指紋。 其會將指紋儲存在文字檔中。

     ![用戶端憑證指紋](./media/contoso-migration-rearchitect-container-sql/cert5.png)

1. 為了部署 Azure DevOps Services，他們需要判斷憑證的 Base64 值。 其會使用 PowerShell 在本機開發人員工作站上進行此作業。 其會將輸出貼到文字檔以供日後使用。

    ```powershell
    [System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes("C:\path\to\certificate.pfx"))
    ```

     ![Base64 值](./media/contoso-migration-rearchitect-container-sql/cert6.png)

1. 最後，其會將新的憑證新增至 Service Fabric 叢集。 若要這麼做，請在入口網站中開啟叢集，然後選取 [**安全性**]。

     ![新增用戶端憑證](./media/contoso-migration-rearchitect-container-sql/cert7.png)

1. 他們會選取 [新增] > [管理員用戶端]，然後貼上新用戶端憑證的指紋。 然後，他們選取 [新增]。 這項作業可能需要 15 分鐘的時間。

     ![新增用戶端憑證](./media/contoso-migration-rearchitect-container-sql/cert8.png)

## <a name="step-5-migrate-the-database-with-dma"></a>步驟 5：使用 DMS 遷移資料庫

Contoso 管理員現在可以使用 DMA 來移轉 SmartHotel360 資料庫。

### <a name="install-dma"></a>安裝 DMA

1. 他們從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=53595)將工具下載至內部部署的 SQL Server VM (**SQLVM**)。

1. 他們在 VM 上執行安裝程式 (DownloadMigrationAssistant.msi)。

1. 在 [完成] 頁面上，他們會先選取 [啟動 Microsoft Data Migration Assistant]，再完成精靈。

### <a name="configure-the-firewall"></a>設定防火牆

為了連線至 Azure SQL Database，Contoso 管理員會設定防火牆規則以允許存取。

1. 在資料庫的 [防火牆與虛擬網路] 內容中，他們會允許存取 Azure 服務，並針對內部部署 SQL Server VM 的用戶端 IP 位址新增規則。

1. 伺服器層級的防火牆規則已建立。

    ![防火牆](./media/contoso-migration-rearchitect-container-sql/sql-firewall.png)

**需要其他協助？**

[深入了解](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure)建立及管理 Azure SQL Database 的防火牆規則。

### <a name="migrate"></a>遷移

Contoso 管理員現在會遷移資料庫。

1. 在 DMA 中，建立新的專案（**SmartHotelDB**），然後選取 [**遷移**]。

1. 他們將來源伺服器類型選取為 [SQL Server]，並以 **Azure SQL Database** 作為目標。

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-1.png)

1. 在移轉詳細資料中，他們新增 **SQLVM** 作為來源伺服器，並新增 **SmartHotel.Registration** 資料庫。

     ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-2.png)

1. 他們收到錯誤，似乎與驗證有關。 不過，在調查之後，問題是出自資料庫名稱中的句號 (.)。 他們決定使用 **SmartHotel-Registration** 名稱佈建新的 SQL 資料庫，以作為解決此問題的方法。 當他們再次執行 DMA 時，他們可以選取 [ **SmartHotel-註冊**]，然後繼續進行嚮導。

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-3.png)

1. 在 [選取物件] 中，他們會選取資料庫資料表，並產生 SQL 指令碼。

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-4.png)

1. 在 DMA 建立指令碼後，他們選取 [部署結構描述]。

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-5.png)

1. DMA 會確認部署成功。

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-6.png)

1. 現在，他們可以開始進行移轉。

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-7.png)

1. 移轉完成之後，Contoso 可以驗證資料庫是否正在 Azure SQL 執行個體上執行。

     ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-8.png)

1. 他們會在 Azure 入口網站中刪除額外的 SQL 資料庫 **SmartHotel.Registration**。

     ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-9.png)

## <a name="step-6-set-up-azure-devops-services"></a>步驟 6：設定 Azure DevOps Services

Contoso 需要為應用程式建置 DevOps 基礎結構和管線。 為此，Contoso 管理員會建立新的 Azure DevOps 專案、匯入其程式碼，然後建置並發行管線。

1. 在 Contoso Azure DevOps 帳戶中，他們會建立新的專案（**ContosoSmartHotelRearchitect**），然後選取 [ **Git** ] 來進行版本控制。
![新增專案](./media/contoso-migration-rearchitect-container-sql/vsts1.png)

1. 其會匯入目前保有其應用程式程式碼的 Git 存放庫。 此程式碼位於[公用存放庫](https://github.com/Microsoft/SmartHotel360-internal-booking-apps)，所以可供下載。

    ![下載應用程式程式碼](./media/contoso-migration-rearchitect-container-sql/vsts2.png)

1. 其會在匯入程式碼後，將 Visual Studio 連線至存放庫，並使用 Team Explorer 複製程式碼。

1. 將存放庫複製到開發人員機器後，他們開啟應用程式的解決方案檔案。 Web 應用程式和 wcf 服務各在檔案中擁有不同的專案。

    ![解決方案檔案](./media/contoso-migration-rearchitect-container-sql/vsts4.png)

## <a name="step-7-convert-the-app-to-a-container"></a>步驟 7：將應用程式轉換成容器

內部部署應用程式是傳統的三層式應用程式：

- 其中包含 WebForms 和連線至 SQL Server 的 WCF 服務。
- 它會使用 Entity Framework 來整合 SQL 資料庫中的資料，並透過 WCF 服務將其公開。
- WebForms 應用程式會與 WCF 服務互動。

Contoso 管理員會使用 Visual Studio 和 SDK Tools 將應用程式轉換成容器，如下所示：

1. 其會使用 Visual Studio 在本機存放庫的 **SmartHotel360-internal-booking-apps\src\Registration** 目錄中檢閱開啟的解決方案檔案 (SmartHotel.Registration.sln)。 其中會顯示兩個應用程式。 Web 前端 SmartHotel.Registration.Web 和 WCF 服務應用程式 SmartHotel.Registration.WCF。

    ![容器](./media/contoso-migration-rearchitect-container-sql/container2.png)

1. 使用滑鼠右鍵按一下 [Web 應用程式] > [新增] >  [容器協調器支援]。

1. 在 [新增容器協調器支援]中，選取 [Service Fabric]。

    ![容器](./media/contoso-migration-rearchitect-container-sql/container3.png)

1. 其會針對 SmartHotel.Registration.WCF 應用程式重複此程序。

1. 現在，其會檢查此解決方案有什麼改變。

    - 新的應用程式是 **SmartHotel.RegistrationApplication/**
    - 它包含兩個服務：**SmartHotel.Registration.WCF**和 **SmartHotel.Registration.Web**。

    ![容器](./media/contoso-migration-rearchitect-container-sql/container4.png)

1. Visual Studio 已建立 Docker 檔案，並將所需的映像從本機提取到開發人員機器。

    ![容器](./media/contoso-migration-rearchitect-container-sql/container5.png)

1. Visual Studio 會建立並開啟資訊清單檔 (**ServiceManifest.xml**)。 此檔案會告知 Service Fabric 如何在容器部署至 Azure 時設定該容器。

    ![容器](./media/contoso-migration-rearchitect-container-sql/container6.png)

1. 另一個資訊清單檔 (**ApplicationManifest.xml) 包含適用於容器的組態應用程式。

    ![容器](./media/contoso-migration-rearchitect-container-sql/container7.png)

1. 其會開啟 **ApplicationParameters/Cloud.xml** 檔案，並更新連接字串以將應用程式連線至 Azure SQL 資料庫。 連接字串可在 Azure 入口網站的資料庫中找到。

    ![連接字串](./media/contoso-migration-rearchitect-container-sql/container8.png)

1. 他們會認可更新過的程式碼並推送至 Azure DevOps Services。

    ![Commit](./media/contoso-migration-rearchitect-container-sql/container9.png)

## <a name="step-8-build-and-release-pipelines-in-azure-devops-services"></a>步驟 8：Azure DevOps Services 中的建置和發行管線

Contoso 管理員現在會設定 Azure DevOps Services 來執行建置和發行程序，以實施 DevOps 做法。

1. 在 Azure DevOps Services 中，他們會選取 [建置及發行] > [新增管線]。

    ![新增管線](./media/contoso-migration-rearchitect-container-sql/pipeline1.png)

1. 他們會選取 [Azure DevOps Services Git] 及相關的存放庫。

    ![Git 和存放庫](./media/contoso-migration-rearchitect-container-sql/pipeline2.png)

1. 在 [選取範本] 中，其會選取具有 Docker 支援的網狀架構。

     ![網狀架構和 Docker](./media/contoso-migration-rearchitect-container-sql/pipeline3.png)

1. 他們會將 [動作] 從 [標記映像] 變更為 [建置映像]，並將工作設定成使用已佈建的 ACR。

     ![登錄](./media/contoso-migration-rearchitect-container-sql/pipeline4.png)

1. 在 [推送映像] 工作中，他們會將映像設定成推送至 ACR，並選取要包含最新的標記。

1. 在 [觸發程序] 中，其會啟用持續整合，並新增主要分支。

    ![觸發程序](./media/contoso-migration-rearchitect-container-sql/pipeline5.png)

1. 他們會選取 [儲存並排入佇列] 以啟動建置作業。

1. 建置成功之後，其會繼續處理發行管線。 在 Azure DevOps Services 中，他們會選取 [發行] > [新增管線]。

    ![發行管線](./media/contoso-migration-rearchitect-container-sql/pipeline6.png)

1. 他們會選取 [Azure Service Fabric 部署] 範本，並為階段命名 (**SmartHotelSF**)。

    ![環境](./media/contoso-migration-rearchitect-container-sql/pipeline7.png)

1. 他們會提供管線名稱 (**ContosoSmartHotel360Rearchitect**)。 針對此階段，他們會選取 [1 個作業，1 個工作] 以設定 Service Fabric 部署。

    ![階段和工作](./media/contoso-migration-rearchitect-container-sql/pipeline8.png)

1. 現在，他們會選取 [新增] 以新增叢集連線。

    ![新增連線](./media/contoso-migration-rearchitect-container-sql/pipeline9.png)

1. 在 [新增 Service Fabric 服務連線] 中，他們會設定連線，以及將供 Azure DevOps Services 用來部署應用程式的驗證設定。 叢集端點可以在 Azure 入口網站中找到，而且其會新增 **tcp://** 以作為前置詞。

1. 其所收集的憑證資訊會輸入到 [伺服器憑證指紋] 和 [用戶端憑證]。

    ![憑證](./media/contoso-migration-rearchitect-container-sql/pipeline10.png)

1. 他們會選取 [管線] > [新增成品]。

     ![構件](./media/contoso-migration-rearchitect-container-sql/pipeline11.png)

1. 其會選取使用最新版本的專案和建置管線。

     ![Build](./media/contoso-migration-rearchitect-container-sql/pipeline12.png)

1. 請注意到，成品上的閃電標示為已核取狀態。

     ![構件狀態](./media/contoso-migration-rearchitect-container-sql/pipeline13.png)

1. 此外，也請注意，持續部署觸發程序已啟用。
   ![持續部署已啟用](./media/contoso-migration-rearchitect-container-sql/pipeline14.png)

1. 他們會選取 [儲存] > [建立發行]。

    ![版本](./media/contoso-migration-rearchitect-container-sql/pipeline15.png)

1. 部署完成後，SmartHotel360 現在會執行 Service Fabric。

    ![發佈](./media/contoso-migration-rearchitect-container-sql/publish4.png)

1. 為了連線至應用程式，他們會將流量導向到 Service Fabric 節點前 Azure 負載平衡器的公用 IP 位址。

    ![發佈](./media/contoso-migration-rearchitect-container-sql/publish5.png)

## <a name="step-9-extend-the-app-and-republish"></a>步驟 9：擴充應用程式並重新發行

當 SmartHotel360 應用程式和資料庫在 Azure 中執行之後，Contoso 想要擴充應用程式。

- Contoso 的開發人員會建立新的 .NET Core 應用程式原型，這會在 Service Fabric 叢集上執行。
- 此應用程式將用來從 Cosmos DB 提取情感資料。
- 此資料將會以推文的形式呈現，並由無伺服器的 Azure 函式和 Azure 認知服務的文字分析 API 進行處理。

### <a name="provision-azure-cosmos-db"></a>佈建 Azure Cosmos DB

第一個步驟，Contoso 管理員會佈建 Azure Cosmos 資料庫。

1. 他們會從 Azure Marketplace 建立 Azure Cosmos DB 資源。

    ![Extend](./media/contoso-migration-rearchitect-container-sql/extend1.png)

1. 提供資料庫名稱 (**contososmarthotel**)、選取 SQL API，並將資源放在主要區域 (美國東部 2) 的生產資源群組中。

    ![Extend](./media/contoso-migration-rearchitect-container-sql/extend2.png)

1. 在 [使用者入門] 中，他們選取 [資料總管] 並加入新的集合。

1. 在 [新增集合] 中，他們提供識別碼並設定儲存體容量與輸送量。

    ![Extend](./media/contoso-migration-rearchitect-container-sql/extend3.png)

1. 在入口網站中，他們會開啟新的資料庫 >**集合** > **檔**，然後選取 [**新增檔**]。

1. 他們會將下列 JSON 程式碼貼到文件視窗中。 這是單一推文形式的範例資料。

    ```json
    {
            "id": "2ed5e734-8034-bf3a-ac85-705b7713d911",
            "tweetId": 927750234331580911,
            "tweetUrl": "https://twitter.com/status/927750237331580911",
            "userName": "CoreySandersWA",
            "userAlias": "@CoreySandersWA",
            "userPictureUrl": "",
            "text": "This is a tweet about #SmartHotel360",
            "language": "en",
            "sentiment": 0.5,
            "retweet_count": 1,
            "followers": 500,
            "hashtags": [
                ""
            ]
    }
    ```

    ![Extend](./media/contoso-migration-rearchitect-container-sql/extend4.png)

1. 他們會找出 Cosmos DB 端點和驗證金鑰。 這些會用來將應用程式連線到集合。 在資料庫中，他們會選取 [金鑰]，並將 URI 和主要金鑰複製到記事本。

    ![Extend](./media/contoso-migration-rearchitect-container-sql/extend5.png)

### <a name="update-the-sentiment-app"></a>更新情感應用程式

佈建好 Cosmos DB 後，Contoso 管理員可以設定應用程式來與之連線。

1. 在 Visual Studio 中，從 [方案總管] 開啟 ApplicationModern\ApplicationParameters\cloud.xml 檔案。

    ![情感應用程式](./media/contoso-migration-rearchitect-container-sql/sentiment1.png)

1. 填入下列兩個參數：

   ```xml
   <Parameter Name="SentimentIntegration.CosmosDBEndpoint" Value="[URI]" />
   ```

   ```xml
   <Parameter Name="SentimentIntegration.CosmosDBAuthKey" Value="[Key]" />
   ```

    ![情感應用程式](./media/contoso-migration-rearchitect-container-sql/sentiment2.png)

### <a name="republish-the-app"></a>重新發佈應用程式

擴充應用程式之後，Contoso 管理員會使用管線將其重新發行到 Azure。

1. 他們會認可其程式碼並推送至 Azure DevOps Services。 這會啟動建置和發行管線。

1. 建置和部署作業完成之後，SmartHotel360 現在會執行 Service Fabric。 Service Fabric 管理主控台現在會顯示三個服務。

    ![重新發佈](./media/contoso-migration-rearchitect-container-sql/republish3.png)

1. 他們現在可以逐一點選服務，以查看 SentimentIntegration 應用程式是否已啟動並執行中。

    ![重新發佈](./media/contoso-migration-rearchitect-container-sql/republish4.png)

## <a name="clean-up-after-migration"></a>移轉之後進行清除

移轉之後，Contoso 必須完成以下的清除步驟：

- 從 vCenter 清查中移除內部部署 VM。
- 從本機備份作業中移除 VM。
- 更新內部文件以顯示 SmartHotel360 應用程式的新位置。 資料庫會顯示為在 Azure SQL 資料庫中執行，而前端會顯示為在 Service Fabric 中執行。
- 檢閱與已解除委任 VM 互動的任何資源，並更新任何相關的設定或文件，以反映新的組態。

## <a name="review-the-deployment"></a>檢閱部署

對於 Azure 中的移轉後資源，Contoso 必須能執行一切功能並保護其新的基礎結構。

### <a name="security"></a>安全性

- Contoso 管理員必須確保其新的 **SmartHotel-Registration** 資料庫很安全。 [詳細資訊](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview)。
- 特別是，他們應將容器更新為搭配使用 SSL 與憑證。
- 他們應考慮使用 Key Vault 來保護其 Service Fabric 應用程式的祕密。 [詳細資訊](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management)。

### <a name="backups"></a>備份

- Contoso 需要檢閱 Azure SQL Database 的備份需求。 [詳細資訊](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups)。
- Contoso 管理員應考慮實作容錯移轉群組，為該資料庫提供區域性容錯移轉。 [詳細資訊](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview)。
- 他們可以利用適用於 ACR 進階 SKU 的異地複寫。 [詳細資訊](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication)。
- 當用於容器的 Web App 可供使用時，Contoso 須考慮在主要的美國東部 2 和美國中部區域部署 Web 應用程式。 Contoso 管理員可以設定流量管理員，以確保在發生區域性的運行中斷時可進行容錯移轉。
- Cosmos DB 會自動備份。 Contoso 會[閱讀](https://docs.microsoft.com/azure/cosmos-db/online-backup-and-restore)此程序以便深入了解。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- 部署好所有資源之後，Contoso 應根據[基礎結構規劃](./contoso-migration-infrastructure.md#set-up-tagging)來指派 Azure 標記。
- 所有授權費用都會併入 Contoso 使用的 PaaS 服務中。 這將會從 EA 中扣除。
- Contoso 會啟用 Microsoft 子公司 Cloudyn 授權的 Azure 成本管理。 它是一種多雲端成本管理解決方案，可協助您使用和管理 Azure 和其他雲端資源。 [深入了解](https://docs.microsoft.com/azure/cost-management/overview) Azure 成本管理。

## <a name="conclusion"></a>結論

在本文中，Contoso 已藉由將應用程式前端 VM 移轉至 Service Fabric，來重構 SmartHotel360 應用程式。 應用程式資料庫已遷移至 Azure SQL 資料庫。
