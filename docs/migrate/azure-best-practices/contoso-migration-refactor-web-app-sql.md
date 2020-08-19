---
title: 將應用程式遷移至 Azure App Service 並 SQL Database
description: 使用適用于 Azure 的雲端採用架構，瞭解如何藉由將應用程式遷移至 Azure App Service 並 Azure SQL Database 來重構應用程式。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: azure-migrate
ms.openlocfilehash: 3e550dafc582742a9cd0c4c83f0fbc416242bc8b
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88567388"
---
<!-- cSpell:ignore WEBVM SQLVM contosohost vcenter contosodc smarthotel SHWEB SHWCF -->

# <a name="migrate-an-application-to-azure-app-service-and-sql-database"></a>將應用程式遷移至 Azure App Service 並 SQL Database

本文示範虛構公司 Contoso 如何在遷移至 Azure 的過程中，重構在 VMware Vm 上執行的兩層式 Windows .NET 應用程式。 Contoso 小組會將應用程式前端虛擬機器 (VM) 遷移至 Azure App Service web 應用程式，並將應用程式資料庫移轉至 Azure SQL Database。

我們在此範例中使用的 SmartHotel360 應用程式是以開放原始碼的形式提供。 如果您想要將它用於您自己的測試用途，您可以從 [GitHub](https://github.com/Microsoft/SmartHotel360)下載。

## <a name="business-drivers"></a>商業動機

Contoso IT 領導小組與商務合作夥伴密切合作，以了解此次移轉所要實現的目標：

- **解決業務成長**。 Contoso 正在成長，而對內部部署系統和基礎結構造成了壓力。
- **提高效率**。 Contoso 必須移除不必要的程式，並簡化開發人員和使用者的流程。 企業亟需快速且不浪費時間或金錢的 IT 服務，進而更快滿足客戶的需求。
- **增加靈活性**。 為了實現全球經濟的成就，Contoso IT 必須能夠更快回應企業的需求。 它必須能夠更快速地回應 marketplace 中的變更。 它不得以或成為企業封鎖程式。
- **擴充**。 隨著企業順利成長，Contoso IT 必須提供能夠同步成長的系統。
- **降低成本**。 Contoso 想要將授權費用降至最低。

## <a name="migration-goals"></a>移轉目標

為了協助判斷最佳的遷移方法，Contoso 雲端小組已釘選下列目標：

| 需求 | 詳細資料 |
| --- | --- |
| **應用程式** | Azure 中的應用程式會保持不變，因為它目前在內部部署中。 <br><br> 其效能應與目前在 VMware 中的效能相同。 <br><br> 小組不想投資應用程式。 目前，系統管理員只需將應用程式安全地移至雲端。 <br><br> 小組想要停止支援目前執行應用程式的 Windows Server 2008 R2。 <br><br> 小組也想要將 SQL Server 2008 R2 移至現代化的平臺即服務 (PaaS) 資料庫，以將管理的需求降到最低。 <br><br> Contoso 想要盡可能運用其在 SQL Server 授權及軟體保證中所做的投資。 <br><br> 此外，Contoso 想要緩解 Web 層上單一失敗點的情形。 |
| **限制** | 此應用程式包含 ASP.NET 應用程式和 Windows Communication Foundation (在相同 VM 上執行的 WCF) 服務。 他們想要使用 Azure App Service 將這些元件分散到兩個 web 應用程式。 |
| **Azure** | Contoso 想要將應用程式移至 Azure，但不想在 Vm 上執行它。 Contoso 想要使用 Web 和資料層的 Azure PaaS 服務。 |
| **DevOps** | Contoso 想要移至 DevOps 模型，以使用其組建和發行管線的 Azure DevOps。 |

## <a name="solution-design"></a>解決方案設計

在釘選其目標和需求後，Contoso 會設計和審核部署解決方案。 它們也會識別遷移程式，包括他們將用於遷移的 Azure 服務。

### <a name="current-application"></a>目前的應用程式

- SmartHotel360 內部部署應用程式會分層至兩個 Vm `WEBVM` 和 `SQLVM` 。
- Vm 位於 VMware ESXi 主機 contosohost1.contoso.com 6.5 版。
- VMware 環境是由在 VM 上執行的 vCenter Server 6.5 (vcenter.contoso.com) 來管理。
- Contoso 有內部部署資料中心 (contoso-datacenter) 以及內部部署網域控制站 (contosodc1)。
- 移轉完成之後，將會解除委任 Contoso 資料中心的內部部署 VM。

### <a name="proposed-solution"></a>建議的解決方案

- 針對應用程式的資料庫層，Contoso 會藉由參考功能比較來比較 Azure SQL Database 與 SQL Server [： Azure SQL Database 和 AZURE SQL 受控執行個體](/azure/sql-database/sql-database-features)。 Contoso 決定使用 Azure SQL Database 有幾個原因：
  - Azure SQL Database 是受控關聯式資料庫服務。 其在多個服務層級上提供可預測的效能，而且幾乎免管理。 優點包括無須停機的動態延展性、內建智慧最佳化及全球延展性和可用性。
  - Contoso 可以使用輕量 Data Migration Assistant 來評估內部部署資料庫移轉至 Azure SQL。
  - Contoso 可以使用 Azure 資料庫移轉服務，將內部部署資料庫移轉至 Azure SQL。
  - 使用軟體保證，Contoso 可以使用 SQL Server 的 Azure Hybrid Benefit，在 SQL Database 的資料庫上交換現有的授權以取得折扣費率。 這種方法可以提供最高達30% 的成本節約。
  - SQL Database 提供了安全性功能，例如 Always Encrypted、動態資料遮罩、資料列層級安全性，以及 SQL 威脅偵測。
- 針對應用程式 web 層，Contoso 已決定使用 Azure App Service。 此 PaaS 服務可讓他們只需進行一些設定變更，就能部署應用程式。 Contoso 會使用 Visual Studio 來進行變更，而他們將會部署兩個 web 應用程式，一個用於網站，一個用於 WCF 服務。
- 為了滿足 DevOps 管線的需求，Contoso 會使用 Azure DevOps 來管理 Git 存放庫的原始程式碼。 他們將使用自動化組建和發行來建立程式碼，並將其部署至 Azure App Service。

### <a name="solution-review"></a>解決方案檢閱

Contoso 會藉由結合優缺點清單來評估其建議的設計，如下表所示：

| 考量 | 詳細資料 |
| --- | --- |
| **優點** | SmartHotel360 應用程式的程式碼不需要變更，就能遷移至 Azure。 <br><br> Contoso 可以使用 SQL Server 和 Windows Server 的 Azure Hybrid Benefit，來利用軟體保證的投資。 <br><br> 遷移之後，將不需要支援 Windows Server 2008 R2。 如需詳細資訊，請參閱 [Microsoft 生命週期原則](https://aka.ms/lifecycle)。 <br><br> Contoso 可以使用多個實例來設定應用程式的 web 層，讓 web 層不再是單一失敗點。 <br><br> 資料庫不會再依賴過時的 SQL Server 2008 R2。 <br><br> SQL Database 可支援其技術需求。 Contoso 會使用 Data Migration Assistant 來評估內部部署資料庫，併發現它是相容的。 <br><br> Azure SQL Database 具有 Contoso 不需要設定的內建容錯功能。 這可確保資料層不再是單一的容錯移轉點。 <br><br> 如果 Contoso 使用 Azure 資料庫移轉服務來遷移其資料庫，就會有可供大規模遷移資料庫的基礎結構。 |
| **缺點** | Azure App Service 只支援每個 web 應用程式部署一個應用程式。 這表示必須布建兩個 web 應用程式，一個用於網站，一個用於 WCF 服務。 |

## <a name="proposed-architecture"></a>建議的架構

![建議的架構圖表。](./media/contoso-migration-refactor-web-app-sql/architecture.png)

### <a name="migration-process"></a>移轉程序

1. Contoso 會布建 Azure SQL 受控實例，然後使用 Azure 資料庫移轉服務，將 SmartHotel360 資料庫移轉至該實例。
1. Contoso 會布建並設定 web 應用程式，並將 SmartHotel360 應用程式部署至這些應用程式。

    ![遷移程式的圖表。](./media/contoso-migration-refactor-web-app-sql/migration-process.png)

### <a name="azure-services"></a>Azure 服務

| 服務 | 描述 | 成本 |
| --- | --- | --- |
| [資料移轉小幫手](/sql/dma/dma-overview?view=ssdt-18vs2017) | Contoso 會使用 Data Migration Assistant 來評定和偵測可能影響 Azure 中資料庫功能的相容性問題。 Data Migration Assistant 會評估 SQL 來源與目標之間的功能同位，並建議效能和可靠性的改進。 | 這是可下載的免費工具。 |
| [Azure Database Migration Service](/azure/dms/dms-overview) | Azure 資料庫移轉服務可讓您從多個資料庫來源順暢地遷移到 Azure 資料平臺，並減少停機時間。 | 深入了解[支援的區域](/azure/dms/dms-overview#regional-availability)和[資料庫移轉服務定價](https://azure.microsoft.com/pricing/details/database-migration)。 |
| [Azure SQL Database](/azure/azure-sql/database/sql-database-paas-overview) | 完全受控的智慧型關聯式雲端資料庫服務。 | 成本是以功能、輸送量和大小為基礎。 [深入了解](https://azure.microsoft.com/pricing/details/sql-database/managed)。 |
| [Azure App Service](/azure/app-service/overview) | 協助建立強大的雲端應用程式，以使用完全受控平臺。 | 定價是根據大小、位置和使用持續時間。 [深入了解](https://azure.microsoft.com/pricing/details/app-service/windows)。 |
| [Azure DevOps](/azure/azure-portal/tutorial-azureportal-devops) | 提供持續整合和持續部署 (CI/CD) 管線以進行應用程式開發。 管線會從用於管理應用程式程式碼的 Git 存放庫開始，以及用來產生封裝和其他組建成品的組建系統，以及可在開發、測試和生產環境中部署變更的發行管理系統。 |

## <a name="prerequisites"></a>必要條件

若要執行此案例，Contoso 必須符合下列必要條件：

| 需求 | 詳細資料 |
| --- | --- |
| **Azure 訂用帳戶** | Contoso 稍早已在本文章系列中建立訂用帳戶。 如果您沒有 Azure 訂用帳戶，請建立[免費帳戶](https://azure.microsoft.com/free)。 <br><br> 如果您建立免費帳戶，您就是訂用帳戶的管理員，並可執行所有動作。 <br><br> 如果您使用現有訂用帳戶，而且您不是系統管理員，則需要與系統管理員合作，讓其指派擁有者或參與者權限給您。 |
| **Azure 基礎結構** | Contoso 會如[適用於移轉的 Azure 基礎結構](./contoso-migration-infrastructure.md)中所述，設定其 Azure 基礎結構。 |

## <a name="scenario-steps"></a>案例步驟

以下是 Contoso 執行移轉的方式：

> [!div class="checklist"]
>
> - **步驟1：在 Azure SQL Database 中**布建資料庫。 Contoso 會布建 Azure SQL Database 實例。 將應用程式網站遷移至 Azure 之後，WCF 服務 web 應用程式會指向此實例。
> - **步驟2：評估資料庫**。 Contoso 會使用 Data Migration Assistant 來評定要進行遷移的資料庫，然後透過 Azure 資料庫移轉服務進行遷移。
> - **步驟3：布建 web 應用程式**。 Contoso 會佈建兩個 Web 應用程式。
> - **步驟4：設定 Azure DevOps**。 Contoso 會建立新的 Azure DevOps 專案，並匯入 Git 存放庫。
> - **步驟5：設定連接字串**。 Contoso 會設定連接字串，以便 Web 層的 Web 應用程式、WCF 服務的 Web 應用程式和 SQL 執行個體進行通訊。
> - **步驟6：在 Azure DevOps 中設定組建和發行管線**。 在最後一個步驟中，Contoso 會在 Azure DevOps 中設定組建和發行管線來建立應用程式，然後將它們部署至兩個不同的 web 應用程式。

## <a name="step-1-provision-a-database-in-azure-sql-database"></a>步驟1：在 Azure SQL Database 中布建資料庫

1. Contoso 管理員決定建立 Azure SQL Database 實例。

    ![顯示 SQL Database 連結的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/provision-sql1.png)

1. 他們會指定資料庫名稱，以符合 `SmartHotel.Registration` 在內部部署 VM 上執行的資料庫。 他們會將資料庫置於 ContosoRG 資源群組。 這是用於 Azure 中生產資源的資源群組。

    ![顯示 SQL Database 實例詳細資料的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/provision-sql2.png)

1. 他們會在主要區域中設定新的 SQL Server 實例 **smarthotel-eus2**。

    ![新 SQL Server 實例的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/provision-sql3.png)

1. 他們會設定符合其伺服器和資料庫需求的定價層。 並選擇使用 Azure Hybrid Benefit 來節省成本，因為他們已經有 SQL Server 授權。
1. 針對調整大小，它們會使用 vCore 為基礎的購買，並設定其預期需求的限制。

    ![VCore 大小調整需求的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/provision-sql4.png)

1. 他們會建立資料庫實例。

    ![建立 SQL Database 實例的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/provision-sql5.png)

1. 他們會開啟資料庫，並記下它們在使用 Data Migration Assistant 進行遷移時需要的詳細資料。

    ![資料庫實例文字檔的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/provision-sql6.png)

**需要其他協助嗎？**

- [說明](/azure/sql-database/sql-database-get-started-portal)如何佈建 SQL Database。
- 瞭解 [vCore 資源限制](/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools)。

## <a name="step-2-assess-the-database"></a>步驟2：評估資料庫

Contoso 管理員會使用 Data Migration Assistant 來評定資料庫，然後藉由參考 [逐步執行遷移教學](/azure/dms/tutorial-sql-server-azure-sql-online)課程，使用 Azure 資料庫移轉服務來遷移資料庫。 他們可以執行線上、離線和混合式 (預覽版) 的遷移。

簡單來說，系統管理員會執行下列動作：

- 他們使用 Data Migration Assistant 來探索和解決任何資料庫移轉問題。
- 他們會使用連線到虛擬網路的 Premium SKU 來建立 Azure 資料庫移轉服務實例。
- 它們可確保實例可透過虛擬網路存取遠端 SQL Server。 這必須確保 Azure 中的所有連入埠都能在虛擬網路層級、網路 VPN 和裝載 SQL Server 的電腦上 SQL Server。
- 他們會設定實例：
  - 建立遷移專案。
  - 將來源 (內部部署資料庫) 。
  - 選取目標。
  - 選取要遷移的資料庫。
  - 設定 [advanced settings]。
  - 開始複寫。
  - 解決任何錯誤。
  - 執行最後的轉換。

## <a name="step-3-provision-web-apps"></a>步驟3：布建 web 應用程式

完成資料庫移轉之後，Contoso 管理員現在即可佈建這兩個 Web 應用程式。

1. 在 Azure 入口網站中，他們會選取 [ **Web 應用程式**]。

    ![Azure 入口網站中 [Web 應用程式] 連結的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/web-app1.png)

1. 他們提供 web 應用程式的名稱 **>shweb-eus2-EUS2**、在 Windows 上執行，然後將它放在 **ContosoRG** 生產資源群組中。 他們會建立新的 Web 應用程式與 Azure App Service 方案。

    ![顯示東部美國2位置的 [Web 應用程式] 窗格螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/web-app2.png)

1. 布建 web 應用程式之後，他們會重複此程式，以建立 WCF 服務的 web 應用程式 **>shwcf-eus2-EUS2**。

    ![顯示 WCF 服務的 [Web 應用程式] 窗格螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/web-app3.png)

1. 他們會前往應用程式的位址，以確保它們已成功建立。

## <a name="step-4-set-up-azure-devops"></a>步驟 4：設定 Azure DevOps

Contoso 需要為應用程式建置 DevOps 基礎結構和管線。 為此，Contoso 管理員會建立新的 DevOps 專案、匯入程式碼，然後設定建置和發行管線。

1. 在 Contoso Azure DevOps 帳戶中，他們會建立新的專案 **>contososmarthotelrefactor**，然後選取 **Git** 進行版本控制。

    ![在 Azure DevOps 中建立新專案的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/vsts1.png)

1. 他們會匯入目前保存其應用程式程式碼的 Git 存放庫。 他們會從 [公用 GitHub 存放庫](https://github.com/Microsoft/SmartHotel360-Registration)下載它。

    ![[匯入 Git 存放庫] 窗格的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/vsts2.png)

1. 他們會將 Visual Studio 連接到存放庫，然後使用 Team Explorer 將程式碼複製到開發人員電腦。

    ![[連接到專案] 窗格的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/devops1.png)

1. 他們會開啟應用程式的方案檔。 Web 應用程式和 WCF 服務在檔案中有不同的專案。

    ![方案總管的螢幕擷取畫面，其中列出 web 應用程式和 WCF 服務專案。](./media/contoso-migration-refactor-web-app-sql/vsts4.png)

## <a name="step-5-configure-connection-strings"></a>步驟 5：設定連接字串

Contoso 管理員可確保 web 應用程式和資料庫能夠彼此通訊。 若要這樣做，須在程式碼和 Web 應用程式中設定連接字串。

1. 在 WCF 服務的 web 應用程式中，在 [ `SHWCF-EUS2` **設定**  >  **應用程式設定**] 下，新增名為**DefaultConnection**的新連接字串。
1. 他們會從 SmartHotel 註冊資料庫提取連接字串，然後使用正確的認證加以更新。

    ![連接字串設定窗格的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/string1.png)

1. 在 Visual Studio 中，系統管理員會 `SmartHotel.Registration.wcf` 從方案檔中開啟專案。 在專案中，他們會 `connectionStrings` 使用連接字串來更新檔案的區段 `web.config` 。

     ![SmartHotel 中 web.config 檔案的 [connectionStrings] 區段螢幕擷取畫面。 wcf 專案。](./media/contoso-migration-refactor-web-app-sql/string2.png)

1. 他們會將的檔案區段變更為 `client` `web.config` `SmartHotel.Registration.Web` 指向 WCF 服務的新位置。 這是裝載服務端點的 WCF web 應用程式的 URL。

    ![SmartHotel 中 web.config 檔案之用戶端區段的螢幕擷取畫面。 wcf 專案。](./media/contoso-migration-refactor-web-app-sql/strings3.png)

1. 現在有了程式碼變更，系統管理員就可以使用 Visual Studio 中的 Team Explorer 來認可並同步處理這些變更。

## <a name="step-6-set-up-build-and-release-pipelines-in-azure-devops"></a>步驟 6：在 Azure DevOps 中設定建置和發行管線

Contoso 管理員現在會設定 Azure DevOps 來執行組建和發行程式。

1. 在 Azure DevOps 中，他們會選取 [**建立併發行**  >  **新的管線**]。

    ![Azure DevOps 中 [新增管線] 連結的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/pipeline1.png)

1. 他們會選取 **Azure Repos Git** ，並在 [存放庫] 下拉式清單中選取相關的存放 **庫** 。

    ![[Azure Repos Git] 按鈕和所選存放庫的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/pipeline2.png)

1. 在 [ **選取範本**] 下，他們會選取組建的 **ASP.NET** 範本。

     ![選取 [選取範本] 窗格以選取 ASP.NET 範本的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/pipeline3.png)

1. 它們會使用組建的名稱 **>contososmarthotelrefactor-ASP.NET-CI** ，然後選取 [ **儲存] & 佇列**，以啟動第一個組建。

     ![組建的 [儲存並排在佇列] 按鈕的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/pipeline4.png)

1. 他們選取組建編號以查看程序。 完成之後，系統管理員可以看到流程的意見反應， **並選取成品** 來檢查組建結果。

    ![用來檢查組建結果的 [組建] 頁面和 [成品] 連結的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/pipeline5.png)

    [成品 **瀏覽器** ] 窗格隨即開啟，而且 [ **放置** ] 資料夾會顯示組建結果。

    - 這兩個 .zip 檔案是包含應用程式的封裝。
    - 這些 .zip 檔案會在發行管線中用來部署至 Azure App Service。

     ![[構件 explorer] 窗格的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline6.png)

1. 他們會選取 [**發行**  >  **+ 新增管線**]。

    ![新增管線](./media/contoso-migration-refactor-web-app-sql/pipeline7.png)

1. 他們選取 Azure App Service 的部署範本。

    ![Azure App Service 部署範本的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/pipeline8.png)

1. 他們將發行管線命名為 **ContosoSmartHotel360Refactor** ，然後在 [ **階段名稱** ] 方塊中指定 **>SHWCF-EUS2-EUS2** 做為 WCF web 應用程式的名稱。

    ![WCF web 應用程式階段名稱的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/pipeline9.png)

1. 在階段底下，他們選取 [1 個作業，1 個工作]**** 以設定 WCF 服務的部署。

    ![[1 個作業，1個工作] 選項的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql-managed-instance/pipeline10.png)

1. 他們會確認已選取並授權訂用帳戶，然後選取 **應用程式服務名稱**。

     ![選取 app service 名稱的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/pipeline11.png)

1. 在管線 > **構件**上，他們會選取 [ **+ 新增**成品]，然後選取 [使用 **ContosoSmarthotel360Refactor** 管線建立]。

     ![[新增成品] 窗格上 [組建] 按鈕的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/pipeline12.png)

1. 若要啟用持續部署觸發程式，系統管理員可以選取構件上的閃電圖示。

     ![構件上閃電圖示的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/pipeline13.png)

1. 他們會將持續部署觸發程式設定為 **啟用**。

    ![顯示 [持續部署] 觸發程式設為 [已啟用] 的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/pipeline14.png)

1. 系統管理員回到第 **1 階段作業，1項工作，** 然後選取 [ **部署 Azure App Service**。

    ![選取 [部署 Azure App Service] 選項的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/pipeline15.png)

1. 在 [ **選取檔案或資料夾**] 中，展開 [ **放置** ] 資料夾，選取在組建期間建立的 *SmartHotel.Registration.Wcf.zip* 檔案，然後選取 [ **儲存**]。

    ![選取 WCF 檔案的 [選取檔案或資料夾] 窗格螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/pipeline16.png)

1. 他們會選取 [**管線**  >  **階段**]，然後選取 [ **+ 新增**] 以新增的環境 `SHWEB-EUS2` 。 他們會選取另一個 Azure App Service 部署。

    ![新增環境的 [1 個作業，1個工作] 連結的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/pipeline17.png)

1. 他們會重複此程式，將檔案發佈 `SmartHotel.Registration.Web.zip` 至正確的 web 應用程式，然後選取 [ **儲存**]。

    ![選取 WEB 檔案的 [選取檔案或資料夾] 窗格螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/pipeline18.png)

    發行管線隨即顯示，如下所示：

     ![發行管線摘要的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/pipeline19.png)

1. 他們回到 [ **組建**]，選取 [ **觸發**程式]，然後選取 [ **啟用持續整合** ] 核取方塊。 此動作會啟用管線，如此一來，當變更認可至程式碼時，就會進行完整的組建和發行。

    ![反白顯示 [啟用持續整合] 核取方塊的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/pipeline20.png)

1. 他們會選取 [ **儲存 & 佇列** ]，以執行完整管線。 觸發新的組建，然後再為 Azure App Service 建立應用程式的第一個版本。

    ![[儲存 & 佇列] 按鈕的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/pipeline21.png)

1. Contoso 管理員可以依循來自 Azure DevOps 的建置和發行管線程序。 組建完成後，就會開始發行。

    ![組建和發行應用程式進度的螢幕擷取畫面。](./media/contoso-migration-refactor-web-app-sql/pipeline22.png)

1. 在管線完成之後，這兩個網站都已部署，且應用程式已啟動並在線上執行。

    ![螢幕擷取畫面，顯示應用程式已啟動並正在執行。](./media/contoso-migration-refactor-web-app-sql/pipeline23.png)

    應用程式已成功遷移至 Azure。

## <a name="clean-up-after-migration"></a>移轉之後進行清除

遷移之後，Contoso 會完成這些清除步驟：

- 他們會從 vCenter 清查中移除內部部署 VM。
- 他們會從本機備份作業中移除 Vm。
- 他們會更新其內部檔，以顯示 SmartHotel360 應用程式的新位置。 本檔顯示在 Azure SQL Database 中執行的資料庫，以及在兩個 web 應用程式中執行的前端。
- 他們會檢查與已解除委任 Vm 互動的任何資源，並更新任何相關的設定或檔，以反映新的設定。

## <a name="review-the-deployment"></a>檢閱部署

現在將資源遷移至 Azure 之後，Contoso 需要完全讓並協助保護其新的基礎結構。

### <a name="security"></a>安全性

- Contoso 有助於確保其新的 `SmartHotel-Registration` 資料庫安全。 [深入了解](/azure/sql-database/sql-database-security-overview)。
- 尤其是，Contoso 會將 web 應用程式更新為搭配使用 SSL 與憑證。

### <a name="backups"></a>備份

- Contoso 團隊會審核 Azure SQL Database 的備份需求。 [深入了解](/azure/sql-database/sql-database-automated-backups)。
- 它們也會瞭解管理 SQL Database 備份和還原的相關資訊。 [深入了解](/azure/sql-database/sql-database-automated-backups)自動備份。
- 他們會考慮執行容錯移轉群組，以提供資料庫的區域性容錯移轉。 [深入了解](/azure/sql-database/sql-database-geo-replication-overview)。
- 針對恢復功能，他們會考慮將 web 應用程式部署在主要區域 (`East US 2`) 和次要區域 (`Central US`) 。 小組可以設定流量管理員，以確保在發生區域性中斷時進行容錯移轉。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- 部署所有資源之後，Contoso 會根據其 [基礎結構規劃](./contoso-migration-infrastructure.md#set-up-tagging)來指派 Azure 標記。
- 所有授權費用都會併入 Contoso 使用的 PaaS 服務中。 這項成本會從 Enterprise 合約中扣除。
- Contoso 將會使用 [Azure 成本管理和帳單](/azure/cost-management-billing/cost-management-billing-overview) ，以確保他們在 IT 領導的預算內保持不變。

## <a name="conclusion"></a>結論

在本文中，Contoso 會將應用程式前端 VM 遷移至兩個 Azure App Service web 應用程式，以重構 Azure 中的 SmartHotel360 應用程式。 應用程式資料庫已遷移至 Azure SQL Database。
