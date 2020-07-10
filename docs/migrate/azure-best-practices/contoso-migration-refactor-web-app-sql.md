---
title: 將應用程式遷移至 Azure App Service 並 SQL Database
description: 使用適用于 Azure 的雲端採用架構，以瞭解如何藉由將應用程式遷移至 Azure App Service 並 Azure SQL Database 來重構它。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: azure-migrate
ms.openlocfilehash: a3b3acd34131a9974703619fdea7bb55c3d9be6e
ms.sourcegitcommit: bcc73d194c6d00c16ae2e3c7fb2453ac7dbf2526
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/09/2020
ms.locfileid: "86192329"
---
<!-- cSpell:ignore WEBVM SQLVM contosohost vcenter contosodc smarthotel SHWEB SHWCF -->

# <a name="migrate-an-application-to-azure-app-service-and-sql-database"></a>將應用程式遷移至 Azure App Service 並 SQL Database

本文示範虛構公司 Contoso 如何在遷移至 Azure 的過程中，重構在 VMware Vm 上執行的兩層式 Windows .NET 應用程式。 他們將應用程式前端 VM 遷移至 Azure App Service web 應用程式，並將應用程式資料庫移轉至 Azure SQL Database。

此範例中使用的 SmartHotel360 應用程式是以開放原始碼的形式提供。 如果想將它用於自己的測試目的，您可以從 [github](https://github.com/Microsoft/SmartHotel360) 進行下載。

## <a name="business-drivers"></a>商業動機

IT 領導小組與商務合作夥伴密切合作，以了解此次移轉所要實現的目標：

- **解決業務成長。** Contoso 的業務量日益增多，對內部部署系統和基礎結構造成了壓力。
- **提高效率。** Contoso 必須移除不必要的程序，並且簡化開發人員和使用者的程序。 企業亟需快速且不浪費時間或金錢的 IT 服務，進而更快滿足客戶的需求。
- **增加靈活性。**  Contoso IT 必須能夠更快因應企業的需求。 其因應速度必須能夠比市場變化更快，才能更在全球經濟中獲致成功。 它不得礙事，或成為企業的絆腳石。
- **尺度.** 隨著企業順利成長，Contoso IT 必須提供可依相同步調成長的系統。
- **降低成本。** Contoso 想要將授權費用降至最低。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已針對此次移轉擬定好各項目標。 並用這些目標用來判斷最合適的移轉方法。

| 需求 | 詳細資料 |
| --- | --- |
| **應用程式** | Azure 中的應用程式將維持不變，如同今天一樣。 <br><br> 其效能應與目前在 VMware 中的效能相同。 <br><br> 小組不想要投資應用程式。 目前，系統管理員會將應用程式安全地移至雲端。 <br><br> 小組想要停止支援目前執行應用程式的 Windows Server 2008 R2。 <br><br> 小組也想要從 SQL Server 2008 R2 移到現代化的 PaaS 資料庫平臺，這樣會將管理的需求降到最低。 <br><br> Contoso 想要盡可能運用其在 SQL Server 授權及軟體保證中所做的投資。 <br><br> 此外，Contoso 想要緩解 Web 層上單一失敗點的情形。 |
| **限制** | 應用程式是由 ASP.NET 應用程式和在相同 VM 上執行的 WCF 服務所組成。 他們想要使用 Azure App Service，將這些元件分散到兩個 web 應用程式。 |
| **Azure** | Contoso 想要將應用程式移至 Azure，但不想在 Vm 上執行。 Contoso 想要使用 Web 和資料層的 Azure PaaS 服務。 |
| **DevOps** | Contoso 想要移轉至 DevOps 模型，使用 Azure DevOps 來處理其建置和發行管線。 |

## <a name="solution-design"></a>解決方案設計

擬定好目標和需求後，Contoso 會設計和檢閱部署解決方案，並識別移轉程序，包括將用於移轉的 Azure 服務。

### <a name="current-application"></a>目前的應用程式

- SmartHotel360 內部部署應用程式會跨兩個 Vm 分層 (`WEBVM` 和 `SQLVM`) 。
- VM 位於 VMware ESXi 主機 `contosohost1.contoso.com`(6.5 版)。
- VMware 環境是由 VM 上所執行的 VCenter Server 6.5 (`vcenter.contoso.com`) 進行管理。
- Contoso 有內部部署資料中心 (`contoso-datacenter`) 以及內部部署網域控制站 (`contosodc1`)。
- 移轉完成之後，將會解除委任 Contoso 資料中心的內部部署 VM。

### <a name="proposed-solution"></a>建議的解決方案

- 針對應用程式的資料庫層，Contoso 會使用[這篇文章](https://docs.microsoft.com/azure/sql-database/sql-database-features)來比較 Azure SQL Database 與 SQL Server。 Contoso 決定使用 Azure SQL Database 的原因有幾個：
  - Azure SQL Database 是受控關聯式資料庫服務。 其在多個服務層級上提供可預測的效能，而且幾乎免管理。 優點包括無須停機的動態延展性、內建智慧最佳化及全球延展性和可用性。
  - Contoso 可以使用輕量 Data Migration Assistant (DMA) 來評估 Azure SQL 的內部部署資料庫。
  - Contoso 可以使用 Azure 資料庫移轉服務，將內部部署資料庫移轉至 Azure SQL。
  - 透過軟體保證，Contoso 可以使用適用於 SQL Server 的 Azure Hybrid Benefit，以折扣優惠在 SQL Database 上交換其現有授權。 這可以提供最多 30% 的折扣。
  - SQL Database 提供安全性功能，例如 Always Encrypted、動態資料遮罩、資料列層級安全性，以及 SQL 威脅偵測。
- 針對應用程式 web 層，Contoso 已決定使用 Azure App Service。 此 PaaS 服務可讓您部署應用程式，只需進行一些設定變更。 Contoso 將使用 Visual Studio 來進行變更，並部署兩個 Web 應用程式。 一個用於網站，另一個用於 WCF 服務。
- 為了符合 DevOps 管線的需求，Contoso 已選擇使用 Azure DevOps 進行原始程式碼管理 (SCM) 與 Git 存放庫。 他們將使用自動化組建和發行來建置程式碼，並將其部署至 Azure App Service。

### <a name="solution-review"></a>解決方案檢閱

Contoso 會透過比較一份優缺點清單，來評估其建議設計。

| 考量 | 詳細資料 |
| --- | --- |
| **優點** | SmartHotel360 應用程式程式碼不需要變更即可遷移至 Azure。 <br><br> Contoso 可以使用 SQL Server 和 Windows Server 的 Azure Hybrid Benefit 來妥善運用軟體保證中的投資。 <br><br> 在遷移之後，不需要支援 Windows Server 2008 R2。 如需詳細資訊，請參閱 [Microsoft 生命週期原則](https://aka.ms/lifecycle)。 <br><br> Contoso 可以使用多個實例來設定應用程式的 web 層，使它不再是單一失敗點。 <br><br> 資料庫不會再依賴過時的 SQL Server 2008 R2。 <br><br> SQL Database 可支援其技術需求。 Contoso 會使用 Data Migration Assistant 來評估內部部署資料庫，併發現它是相容的。 <br><br> Azure SQL Database 具有內建的容錯功能，Contoso 不需要進行設定。 這可確保資料層不再是單一的容錯移轉點。 <br><br> 如果 Contoso 使用 Azure 資料庫移轉服務來遷移其資料庫，則會有基礎結構準備好大規模遷移資料庫。 |
| **缺點** | Azure App Service 只支援每個 web 應用程式的一個應用程式部署。 這表示必須佈建兩個 Web 應用程式 (一個用於網站，一個用於 WCF 服務)。 |

## <a name="proposed-architecture"></a>建議的架構

![案例架構](./media/contoso-migration-refactor-web-app-sql/architecture.png)

### <a name="migration-process"></a>移轉程序

1. Contoso 會布建 Azure SQL 實例，並使用 Azure 資料庫移轉服務將 SmartHotel360 資料庫移轉至其中。
2. Contoso 會布建和設定 web apps，並將 SmartHotel360 應用程式部署至這些應用程式。

    ![移轉程序](./media/contoso-migration-refactor-web-app-sql/migration-process.png)

### <a name="azure-services"></a>Azure 服務

| 服務 | 描述 | 成本 |
| --- | --- | --- |
| [Data Migration Assistant (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Contoso 會使用 DMA，評估和偵測可能對其 Azure 中的資料庫功能造成影響的相容性問題。 DMA 會評定 SQL 來源與目標之間的功能同位，並提出效能和可靠性改善建議。 | 此工具可免費下載。 |
| [Azure 資料庫移轉服務](https://docs.microsoft.com/azure/dms/dms-overview) | Azure 資料庫移轉服務可在停機時間最短的情況之下，從多個資料庫來源順暢地遷移至 Azure 資料平臺。 | 深入了解[支援的區域](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability)和[資料庫移轉服務定價](https://azure.microsoft.com/pricing/details/database-migration)。 |
| [Azure SQL Database](https://docs.microsoft.com/azure/azure-sql/database/sql-database-paas-overview) | 完全受控的智慧型關聯式雲端資料庫服務。 | 根據功能、輸送量和大小計算費用。 [深入了解](https://azure.microsoft.com/pricing/details/sql-database/managed)。 |
| [Azure App Service](https://docs.microsoft.com/azure/app-service/overview) | 使用完全受控的平臺建立強大的雲端應用程式 | 根據大小、位置和使用期間計算費用。 [深入了解](https://azure.microsoft.com/pricing/details/app-service/windows)。 |
| [Azure DevOps](https://docs.microsoft.com/azure/azure-portal/tutorial-azureportal-devops) | 提供持續整合和持續部署， (CI/CD) 管線進行應用程式開發。 管線會從 Git 存放庫開始，以用於管理應用程式程式碼、用於產生封裝和其他組建成品的組建系統，以及在開發、測試和生產環境中部署變更的發行管理系統。 |

## <a name="prerequisites"></a>必要條件

以下是 Contoso 在此情況下所須執行的動作：

| 需求 | 詳細資料 |
| --- | --- |
| **Azure 訂用帳戶** | Contoso 已在前文說明的步驟中建立訂用帳戶。 若您沒有 Azure 訂用帳戶，請建立一個[免費帳戶](https://azure.microsoft.com/free)。 <br><br> 如果您建立免費帳戶，您就是訂用帳戶的管理員，並可執行所有動作。 <br><br> 如果您使用現有訂用帳戶，而且您不是系統管理員，則需要與系統管理員合作，讓其指派擁有者或參與者權限給您。 |
| **Azure 基礎結構** | [了解](./contoso-migration-infrastructure.md) Contoso 如何設定 Azure 基礎結構。 |

## <a name="scenario-steps"></a>案例步驟

以下是 Contoso 執行移轉的方式：

> [!div class="checklist"]
>
> - **步驟1：在 Azure 中布建 SQL Database 實例。** Contoso 會在 Azure 中佈建 SQL 執行個體。 將應用程式網站遷移至 Azure 之後，WCF 服務 web 應用程式會指向這個實例。
> - **步驟2：使用 Azure Data Migration Assistant (DMA) 評估資料庫，並透過 Azure 資料庫移轉服務進行遷移。** Contoso 會評估要進行遷移的資料庫，然後透過 Azure 資料庫移轉服務來遷移應用程式資料庫。
> - **步驟3：布建 web 應用程式。** Contoso 會佈建兩個 Web 應用程式。
> - **步驟4：設定 Azure DevOps。** Contoso 會建立新的 Azure DevOps 專案，並匯入 Git 存放庫。
> - **步驟5：設定連接字串。** Contoso 會設定連接字串，以便 Web 層的 Web 應用程式、WCF 服務的 Web 應用程式和 SQL 執行個體進行通訊。
> - **步驟6：設定組建和發行管線。** 在最後一個步驟中，Contoso 會設定組建和發行管線來建立應用程式，並將其部署至兩個不同的 web 應用程式。

## <a name="step-1-provision-azure-sql-database"></a>步驟1：布建 Azure SQL Database

1. Contoso 管理員會選擇建立 Azure SQL Database 實例。

    ![佈建 SQL](./media/contoso-migration-refactor-web-app-sql/provision-sql1.png)

2. 他們會指定資料庫名稱，以符合內部部署 VM 上執行的資料庫 (`SmartHotel.Registration`) 。 它們會將資料庫放在 `ContosoRG` 資源群組中。 這是用於 Azure 中生產資源的資源群組。

    ![布建 Azure SQL Database 實例](./media/contoso-migration-refactor-web-app-sql/provision-sql2.png)

3. 他們會 `sql-smarthotel-eus2` 在主要區域中 () 設定新的 SQL Server 實例。

    ![佈建 SQL](./media/contoso-migration-refactor-web-app-sql/provision-sql3.png)

4. 他們會設定符合其伺服器和資料庫需求的定價層。 並選擇使用 Azure Hybrid Benefit 來節省成本，因為他們已經有 SQL Server 授權。
5. 針對調整大小，他們會使用 vCore 為基礎的購買，並設定其預期需求的限制。

    ![佈建 SQL](./media/contoso-migration-refactor-web-app-sql/provision-sql4.png)

6. 然後，他們會建立資料庫執行個體。

    ![佈建 SQL](./media/contoso-migration-refactor-web-app-sql/provision-sql5.png)

7. 建立實例之後，他們會開啟資料庫，並記下它們在使用 Data Migration Assistant 進行遷移時所需的詳細資料。

    ![佈建 SQL](./media/contoso-migration-refactor-web-app-sql/provision-sql6.png)

**需要其他協助？**

- [說明](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal)如何佈建 SQL Database。
- 深入瞭解[vCore 資源限制](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools)。

## <a name="step-2-assess-the-database-using-data-migration-assistant-dma-and-migrate-it-using-azure-database-migration-service"></a>步驟2：使用 Data Migration Assistant (DMA) 評估資料庫，並使用 Azure 資料庫移轉服務進行遷移

Contoso 管理員會使用 Data Migration Assistant (DMA) 來評估資料庫，然後使用「[逐步遷移」教學](https://docs.microsoft.com/azure/dms/tutorial-sql-server-azure-sql-online)課程，透過 Azure 資料庫移轉服務來進行遷移。 他們可以執行線上、離線和混合式 (預覽) 遷移。

總而言之，您必須執行下列動作：

- 使用 Data Migration Assistant (DMA) 來探索及解決任何資料庫移轉問題。
- 使用連線到 VNet 的 SKU 來建立 Azure 資料庫移轉服務實例 `Premium` 。
- 確定實例可以透過虛擬網路存取遠端 SQL Server。 這會需要確保所有連入埠都可從 Azure 允許在虛擬網路層級、網路 VPN 和裝載 SQL Server 的電腦上 SQL Server。
- 設定實例：
  - 建立遷移專案。
  - 在內部部署資料庫) 中新增來源 (。
  - 選取目標。
  - 選取要遷移的資料庫。
  - 設定 [高級設定]。
  - 開始複寫。
  - 解決任何錯誤。
  - 執行最後的轉換。

## <a name="step-3-provision-web-apps"></a>步驟3：布建 web 應用程式

完成資料庫移轉之後，Contoso 管理員現在即可佈建這兩個 Web 應用程式。

1. 在入口網站中選取 [Web 應用程式]****。

    ![Web 應用程式](./media/contoso-migration-refactor-web-app-sql/web-app1.png)

2. 它們會提供 web 應用程式 () 的名稱 `SHWEB-EUS2` ，在 Windows 上執行，並將它放在 [生產] 資源群組中 `ContosoRG` 。 他們會建立新的 Web 應用程式與 Azure App Service 方案。

    ![Web 應用程式](./media/contoso-migration-refactor-web-app-sql/web-app2.png)

3. 布建 web 應用程式之後，他們會重複建立 WCF 服務 (之 web 應用程式的流程 `SHWCF-EUS2`) 

    ![Web 應用程式](./media/contoso-migration-refactor-web-app-sql/web-app3.png)

4. 完成之後，他們會流覽至應用程式的位址，以檢查它們是否已成功建立。

## <a name="step-4-set-up-azure-devops"></a>步驟 4：設定 Azure DevOps

Contoso 需要為應用程式建置 DevOps 基礎結構和管線。 為此，Contoso 管理員會建立新的 DevOps 專案、匯入程式碼，然後設定建置和發行管線。

1. 在 Contoso Azure DevOps 組織中，他們會 () 建立新的專案 `ContosoSmartHotelRefactor` ，然後選取 [ **Git** ] 來進行版本控制。

    ![新增專案](./media/contoso-migration-refactor-web-app-sql/vsts1.png)

2. 他們會匯入目前保存其應用程式程式碼的 Git 存放庫。 它位於[公用 GitHub 存放庫](https://github.com/Microsoft/SmartHotel360-Registration)中，您可以下載它。

    ![下載應用程式程式碼](./media/contoso-migration-refactor-web-app-sql/vsts2.png)

3. 其會在匯入程式碼後，將 Visual Studio 連線至存放庫，並使用 Team Explorer 複製程式碼。

    ![連線至專案](./media/contoso-migration-refactor-web-app-sql/devops1.png)

4. 將存放庫複製到開發人員機器之後，他們會開啟應用程式的方案檔。 Web 應用程式和 WCF 服務各自在檔案內有個別的專案。

    ![解決方案檔案](./media/contoso-migration-refactor-web-app-sql/vsts4.png)

## <a name="step-5-configure-connection-strings"></a>步驟 5：設定連接字串

Contoso 管理員必須確定 Web 應用程式和資料庫皆可進行通訊。 若要這樣做，須在程式碼和 Web 應用程式中設定連接字串。

1. 在 WCF 服務的 web 應用程式 (`SHWCF-EUS2`) >**設定**] [  >  **應用程式設定**] 中，他們會新增名為的新連接字串 `DefaultConnection` 。
2. 連接字串會從 `SmartHotel-Registration` 資料庫提取，而且應該使用正確的認證進行更新。

    ![連接字串](./media/contoso-migration-refactor-web-app-sql/string1.png)

3. 使用 Visual Studio，他們會 `SmartHotel.Registration.wcf` 從方案檔中開啟專案。 `connectionStrings` `web.config` `SmartHotel.Registration.wcf` 應使用連接字串來更新 WCF 服務之檔案的區段。

     ![連接字串](./media/contoso-migration-refactor-web-app-sql/string2.png)

4. 的檔案 `client` 區段 `web.config` `SmartHotel.registration.web` 應該變更為指向 WCF 服務的新位置。 這是裝載服務端點的 WCF Web 應用程式 URL。

    ![連接字串](./media/contoso-migration-refactor-web-app-sql/strings3.png)

5. 程式碼進行變更後，管理員必須認可這些變更。 他們在 Visual Studio 中使用 Team Explorer 進行認可和同步。

## <a name="step-6-set-up-build-and-release-pipelines-in-azure-devops"></a>步驟 6：在 Azure DevOps 中設定建置和發行管線

Contoso 管理員現在會設定 Azure DevOps 以執行建置和發行程序。

1. 在 Azure DevOps 中，他們會選取 [**建立和發行**  >  **新管線**]。

    ![新增管線](./media/contoso-migration-refactor-web-app-sql/pipeline1.png)

2. 他們會選取 [Azure Repos Git]**** 及相關的存放庫。

    ![Git 和存放庫](./media/contoso-migration-refactor-web-app-sql/pipeline2.png)

3. 在 [選取範本]**** 中，他們為其組建選取 ASP.NET 範本。

     ![ASP.NET 範本](./media/contoso-migration-refactor-web-app-sql/pipeline3.png)

4. 此名稱 `ContosoSmartHotelRefactor-ASP.NET-CI` 會用於組建。 他們選取 [儲存並排入佇列]****。

     ![儲存並排入佇列](./media/contoso-migration-refactor-web-app-sql/pipeline4.png)

5. 這會啟動第一個組建。 他們選取組建編號以查看程序。 完成之後，他們可以查看處理常式的意見，然後選取 [**構件**] 來審查組建結果。

    ![檢閱](./media/contoso-migration-refactor-web-app-sql/pipeline5.png)

6. [Drop]**** 資料夾會包含組建結果。

    - 這兩個 zip 檔案是包含應用程式的封裝。
    - 這些檔案會用於部署至 Azure App Service 的發行管線中。

     ![構件](./media/contoso-migration-refactor-web-app-sql/pipeline6.png)

7. 他們會選取 [**發行**  >  **+ 新增管線**]。

    ![新增管線](./media/contoso-migration-refactor-web-app-sql/pipeline7.png)

8. 他們選取 Azure App Service 的部署範本。

    ![Azure App Service 部署範本](./media/contoso-migration-refactor-web-app-sql/pipeline8.png)

9. 它們會為發行管線命名 `ContosoSmartHotel360Refactor` ，並指定 WCF web 應用程式的名稱 (`SHWCF-EUS2`) 作為**階段**名稱。

    ![環境](./media/contoso-migration-refactor-web-app-sql/pipeline9.png)

10. 在階段底下，他們選取 [1 個作業，1 個工作]**** 以設定 WCF 服務的部署。

    ![部署 WCF](./media/contoso-migration-refactor-web-app-sql/pipeline10.png)

11. 他們會確認已選取並授權訂用帳戶，然後選取 [ **app service 名稱**]。

     ![選取 App Service 名稱](./media/contoso-migration-refactor-web-app-sql/pipeline11.png)

12. 在 [**管線 > 成品**] 上，選取 [ **+ 新增**成品]，然後選取 [使用 `ContosoSmarthotel360Refactor` 管線建立]。

     ![建置](./media/contoso-migration-refactor-web-app-sql/pipeline12.png)

13. 他們會確認已核取成品上的閃電，以啟用持續部署觸發程式。

     ![閃電圖示](./media/contoso-migration-refactor-web-app-sql/pipeline13.png)

14. 持續部署觸發程序應設定為 [已啟用]****。

    ![持續部署已啟用](./media/contoso-migration-refactor-web-app-sql/pipeline14.png)

15. 現在，他們會移回**第1階段作業（1個工作**），然後選取 [**部署 Azure App Service**]。

    ![部署 Azure App Service](./media/contoso-migration-refactor-web-app-sql/pipeline15.png)

16. 在 [**選取檔案或資料夾**] 中，他們會找出 `SmartHotel.Registration.Wcf.zip` 組建期間所建立的檔案，然後選取 [**儲存**]。

    ![儲存 WCF](./media/contoso-migration-refactor-web-app-sql/pipeline16.png)

17. 他們會選取 [**管線**  >  **階段**]、[ **+ 新增**] 以新增的環境 `SHWEB-EUS2` 。 他們會選取另一個 Azure App Service 部署。

    ![新增環境](./media/contoso-migration-refactor-web-app-sql/pipeline17.png)

18. 他們會重複此程式，將 web 應用程式 () 檔案發佈 `SmartHotel.Registration.Web.zip` 至正確的 web 應用程式。

    ![發佈 Web 應用程式](./media/contoso-migration-refactor-web-app-sql/pipeline18.png)

19. 完成儲存後，發行管線會顯示如下。

     ![發行管線摘要](./media/contoso-migration-refactor-web-app-sql/pipeline19.png)

20. 他們會移回 [**組建**]，然後選取 [**觸發**程式] [  >  **啟用持續整合**]。 這會使管線在變更認可至程式碼以及完整組建和發行發生時進行整合。

    ![啟用持續整合](./media/contoso-migration-refactor-web-app-sql/pipeline20.png)

21. 他們選取 [儲存並排入佇列]****，以執行完整管線。 會觸發新的組建，然後再建立第一版的應用程式至 Azure App Service。

    ![儲存管線](./media/contoso-migration-refactor-web-app-sql/pipeline21.png)

22. Contoso 管理員可以依循來自 Azure DevOps 的建置和發行管線程序。 組件完成後，就會開始發行。

    ![組建和發行應用程式](./media/contoso-migration-refactor-web-app-sql/pipeline22.png)

23. 管線完成後，兩個網站都已部署，而且應用程式已啟動並在線上執行。

    ![完成發行](./media/contoso-migration-refactor-web-app-sql/pipeline23.png)

此時，應用程式已成功遷移至 Azure。

## <a name="clean-up-after-migration"></a>移轉之後進行清除

移轉之後，Contoso 必須完成以下的清除步驟：

- 從 vCenter 清查中移除內部部署 VM。
- 從本機備份作業中移除 VM。
- 更新內部檔以顯示 SmartHotel360 應用程式的新位置。 顯示在 Azure SQL Database 中執行的資料庫，以及在兩個 web 應用程式中執行的前端。
- 檢閱與已解除委任 VM 互動的任何資源，並更新任何相關的設定或文件，以反映新的組態。

## <a name="review-the-deployment"></a>檢閱部署

對於 Azure 中的移轉後資源，Contoso 必須能執行一切功能並保護其新的基礎結構。

### <a name="security"></a>安全性

- Contoso 必須確保其新的 `SmartHotel-Registration` 資料庫安全。 [深入了解](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview)。
- 特別是，Contoso 應將 Web 應用程式更新為搭配使用 SSL 與憑證。

### <a name="backups"></a>備份

- Contoso 需要檢閱 Azure SQL Database 的備份需求。 [深入了解](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups)。
- Contoso 也必須了解如何管理 SQL Database 備份和還原。 [深入了解](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups)自動備份。
- Contoso 應考慮實作容錯移轉群組，為該資料庫提供區域性容錯移轉。 [深入了解](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview)。
- Contoso 必須考慮在主要區域中部署 web 應用程式 (`East US 2`) ，並將次要地區 (`Central US`) 區域，以提供恢復功能。 Contoso 可以設定流量管理員以確保區域中斷期間的容錯移轉。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- 部署好所有資源之後，Contoso 應根據[基礎結構規劃](./contoso-migration-infrastructure.md#set-up-tagging)來指派 Azure 標記。
- 所有授權費用都會併入 Contoso 使用的 PaaS 服務中。 這將會從 EA 中扣除。
- Contoso 會使用[Azure 成本管理和帳單](https://docs.microsoft.com/azure/cost-management-billing/cost-management-billing-overview)，確保他們會在其 IT 領導地位所建立的預算內保持不變。

## <a name="conclusion"></a>結論

在本文中，Contoso 會藉由將應用程式前端 VM 遷移至兩個 Azure App Service web 應用程式，在 Azure 中重構 SmartHotel360 應用程式。 應用程式資料庫已遷移至 Azure SQL Database。
