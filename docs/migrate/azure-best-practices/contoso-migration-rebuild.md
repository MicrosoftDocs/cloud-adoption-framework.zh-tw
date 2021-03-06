---
title: 在 Azure 中重建內部部署應用程式
description: 瞭解 Contoso 如何使用 Azure App Service、Azure Kubernetes Service (AKS) 、Azure Cosmos DB、Azure 函數和 Azure 認知服務，在 Azure 中重建應用程式。
author: BrianBlanchard
ms.author: brblanch
ms.date: 7/1/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: internal
ms.openlocfilehash: a2fe2068f5a7bc1f782ddd5496e59c2870d9367e
ms.sourcegitcommit: c167c45b66cc7324b60c88b8b7aac439f956b65d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2021
ms.locfileid: "102208607"
---
<!-- docutune:casing "Enable .NET" SmartHotel360-Backend Pet.Checker "contoso-datacenter" PetCheckerFunction -->

<!-- cSpell:ignore SQLVM WEBVM contoso contosohost vcenter contosodc smarthotel contososmarthotel smarthotelcontoso smarthotelpetchecker petchecker smarthotelakseus smarthotelacreus smarthotelpets kubectl contosodevops visualstudio azuredeploy cloudapp smarthotelsettingsurl appsettings -->

# <a name="rebuild-an-on-premises-application-in-azure"></a>在 Azure 中重建內部部署應用程式

本文示範虛構公司 Contoso 如何重建在 VMware 虛擬機器上執行的兩層式 Windows .NET 應用程式， (Vm) 作為遷移至 Azure 的一部分。 Contoso 會將前端 VM 遷移至 Azure App Service web 應用程式。 Contoso 會使用部署至 Azure Kubernetes Service 所管理之容器 (AKS) 的微服務來建立應用程式後端。 網站會與 Azure Functions 互動以提供寵物相片功能。

此範例中使用的 SmartHotel360 應用程式是以開放原始碼授權提供。 如果您想要將它用於您自己的測試用途，您可以從 [GitHub](https://github.com/Microsoft/SmartHotel360)下載。

## <a name="business-drivers"></a>商業動機

Contoso IT 領導小組與商務合作夥伴密切合作，以了解此次移轉所要實現的目標：

- **解決業務成長。** Contoso 正在成長，而想要為 Contoso 網站上的客戶提供差異化的體驗。
- **變得敏捷。** Contoso 必須能夠以更快的速度回應 marketplace 中的變更，以實現全球經濟的成功。
- **規模。** 隨著企業順利成長，Contoso IT 小組必須提供可依相同步調成長的系統。
- **降低成本。** Contoso 想要將授權費用降至最低。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已將此遷移的應用程式需求固定。 這些需求已用來判斷最合適的移轉方法：

- Azure 中的應用程式必須保持不變，因為它現在是在內部部署環境中。 其應該妥善執行並可輕鬆調整。
- 應用程式不應該使用基礎結構即服務 (IaaS) 元件。 所有專案都應該建立為使用平臺即服務 (PaaS) 或無伺服器服務。
- 應用程式組建應該在雲端服務中執行，而容器應該位於雲端中的私人全企業登錄中。
- 用於寵物相片的 API 服務在真實世界中應該是準確且可靠的，因為應用程式所做的決策必須在其旅館內接受。 任何獲准進入的寵物都可以留在旅館內。
- 為了滿足 DevOps 管線的需求，Contoso 會使用 Azure 存放庫中的 Git 存放庫來管理原始程式碼。 他們將使用自動化建置和發行來建置程式碼，並將其部署至 Azure App Service、Azure Functions 及 AKS。
- 個別的持續整合/持續開發 (CI/CD) 管線需要在後端上的微服務，以及前端上的網站。
- 後端服務和前端 web 應用程式有不同的發行週期。 為了符合此需求，Contoso 會部署兩個不同的管線。
- Contoso 需要管理部門核准才能進行所有前端網站部署，因此 CI/CD 管線必須提供此功能。

## <a name="solution-design"></a>解決方案設計

擬定好目標和需求後，Contoso 會設計和檢閱部署解決方案，並識別移轉程序，包括將用於移轉的 Azure 服務。

### <a name="current-application"></a>目前的應用程式

- SmartHotel360 內部部署應用程式會分層至兩個 Vm (`WEBVM` 和 `SQLVM`) 。
- VM 位於 VMware ESXi 主機 `contosohost1.contoso.com`(6.5 版)。
- VMware 環境是由 VM 上所執行的 VCenter Server 6.5 (`vcenter.contoso.com`) 進行管理。
- Contoso 有內部部署資料中心 (`contoso-datacenter`) 以及內部部署網域控制站 (`contosodc1`)。
- 移轉完成之後，將會解除委任 Contoso 資料中心的內部部署 VM。

### <a name="proposed-architecture"></a>建議的架構

- 應用程式的前端在主要 Azure 區域中部署為 Azure App Service web 應用程式。
- Azure 函式提供寵物相片上傳功能，而網站則與此功能互動。
- 寵物相片函式會使用 Azure 認知服務的電腦視覺 API 以及 Azure Cosmos DB。
- 網站的後端是使用微服務所建立。 這些微服務會部署到在 AKS 中管理的容器。
- 容器將使用 Azure DevOps 建立，然後推送至 Azure Container Registry。
- 目前，Contoso 會使用 Visual Studio 來手動部署 web 應用程式和函式程式碼。
- Contoso 會使用 PowerShell 腳本來部署微服務，以呼叫 Kubernetes 命令列工具。

    ![遷移至 Azure 的案例架構圖表。](./media/contoso-migration-rebuild/architecture.png)
    *圖1：案例架構。*

### <a name="solution-review"></a>解決方案檢閱

Contoso 會透過比較一份優缺點清單，來評估建議設計。

| 考量 | 詳細資料 |
| --- | --- |
| **優點** | 針對端對端部署使用 PaaS 和無伺服器解決方案，可大幅減少 Contoso 必須提供的管理時間。 <br><br> 移至以微服務為基礎的架構，可讓 Contoso 在一段時間內輕鬆擴充解決方案。 <br><br> 新功能可以上線，而不會中斷任何現有解決方案的程式碼基底。 <br><br> Web 應用程式將會使用多個實例來設定，而不會有單一失敗點。 <br><br> 將會啟用自動調整，讓應用程式可以處理不同的流量。 <br><br> 藉由移至 PaaS 服務，Contoso 可以淘汰在 Windows Server 2008 R2 作業系統上執行的過時解決方案。 <br><br> Azure Cosmos DB 有內建的容錯功能，不需要由 Contoso 進行設定。 這表示資料層不再是單一的容錯移轉點。 |
| **缺點** | 容器比其他移轉選項複雜得多。 其學習曲線可能會是 Contoso 的難題。 它們引進了新的複雜度層級，可提供值，而不是曲線。 <br><br> Contoso 的營運小組需要提升，以瞭解並支援應用程式的 Azure、容器和微服務。 <br><br> Contoso 尚未針對整個解決方案完全實作 DevOps。 Contoso 必須在將服務部署至 AKS、Azure Functions 及 Azure App Service 時，考慮到這一點。 |

### <a name="migration-process"></a>移轉程序

1. Contoso 會布建 Azure Container Registry、AKS 和 Azure Cosmos DB。
2. Contoso 會布建部署的基礎結構，包括 Azure App Service web 應用程式、儲存體帳戶、函式和 API。
3. 在基礎結構就緒之後，Contoso 會使用 Azure DevOps 來建立其微服務容器映射，以將映射推送至容器登錄。
4. Contoso 會使用 PowerShell 腳本將這些微服務部署至 AKS。
5. 最後，Contoso 會部署函數和 web 應用程式。

    ![從準備部署到雲端的遷移程式圖表。](./media/contoso-migration-rebuild/migration-process.png)
    *圖2：遷移程式。*

### <a name="azure-services"></a>Azure 服務

| 服務 | 描述 | 成本 |
|---|---|---|
| [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service/) | 簡化 Kubernetes 管理、部署和作業。 提供完全受控的 Kubernetes 容器協調流程服務。 | AKS 是免費服務。 只針對所耗用的 Vm 和相關聯的儲存體和網路資源支付費用。 [深入了解](https://azure.microsoft.com/pricing/details/kubernetes-service/)。 |
| [Azure Functions](https://azure.microsoft.com/services/functions/) | 以事件驅動的無伺服器計算體驗，加快開發速度。 依需求進行調整。 | 只需就取用的資源支付費用。 根據每秒的資源取用量和執行次數計算方案的費用。 [深入了解](https://azure.microsoft.com/pricing/details/functions/)。 |
| [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) | 儲存所有容器部署類型的映像。 | 成本是以功能、儲存體和使用持續時間為基礎。 [深入了解](https://azure.microsoft.com/pricing/details/container-registry/)。 |
| [Azure App Service](https://azure.microsoft.com/services/app-service/) | 快速建立、部署及調整企業級 web、行動和 API 應用程式，以在任何平臺上執行。 | App Service 方案會以每秒為單位計費。 [深入了解](https://azure.microsoft.com/pricing/details/app-service/windows/)。 |

## <a name="prerequisites"></a>必要條件

以下是 Contoso 針對此案例所需的項目：

| 需求 | 詳細資料 |
| --- | --- |
| Azure 訂用帳戶 | <li> Contoso 已在先前的文章中建立訂用帳戶。 如果您沒有 Azure 訂用帳戶，請建立[免費帳戶](https://azure.microsoft.com/free/)。 <li> 如果您建立免費帳戶，您就是訂用帳戶的系統管理員，而且可以執行所有動作。 <li> 如果您使用現有的訂用帳戶，但您不是系統管理員，則需要與系統管理員合作，以指派擁有者或參與者許可權給您。 |
| Azure 基礎結構 | <li> 瞭解 [Contoso 如何設定 Azure 基礎結構](./contoso-migration-infrastructure.md)。 |
| 開發人員必要條件 | 在開發人員工作站上，Contoso 需要下列工具： <li> [Visual Studio 社區2017版本15。5](https://visualstudio.microsoft.com/vs/community/) <li> .NET 工作負載，已啟用 <li> [Git](https://git-scm.com) <li> [Azure PowerShell](https://azure.microsoft.com/downloads/) <li> [Azure CLI](/cli/azure/install-azure-cli) <li> [Docker 社區版 (windows 10) 或 Docker Enterprise edition (Windows Server) ](https://docs.docker.com/docker-for-windows/install/)，設定為使用 windows 容器 |

## <a name="scenario-steps"></a>案例步驟

以下是 Contoso 執行移轉的方式：

> [!div class="checklist"]
>
> - **步驟1：布建 AKS 和 Azure Container Registry。** Contoso 會使用 PowerShell 布建受控 AKS 叢集和 container registry。
> - **步驟2：建立 Docker 容器。** Contoso 會使用 Azure DevOps 來設定 Docker 容器的持續整合 (CI) ，並將容器推送至容器登錄。
> - **步驟3：部署後端微服務。** Contoso 會部署將由後端微服務使用的其他基礎結構。
> - **步驟4：部署前端基礎結構。** Contoso 會部署前端基礎結構，包括寵物電話的 Blob 儲存體、Azure Cosmos DB，以及電腦視覺 API。
> - **步驟5：遷移後端。** Contoso 會部署微服務，並在 AKS 上執行它們來遷移後端。
> - **步驟6：發佈前端。** Contoso 會將 SmartHotel360 應用程式發佈到 Azure App Service，以及寵物服務所要呼叫的函數應用程式。

## <a name="provision-back-end-resources"></a>佈建後端資源

Contoso 管理員會執行部署腳本，使用 AKS 和 Azure Container Registry 建立受控 Kubernetes 叢集。 本節的指示會使用 [SmartHotel360 後端](https://github.com/Microsoft/SmartHotel360-Backend) GitHub 存放庫。 存放庫包含這部分部署的所有軟體。

### <a name="ensure-that-prerequisites-are-met"></a>確定符合必要條件

在開始之前，Contoso 管理員會確定所有先決條件軟體都已安裝在要用於部署的開發電腦上。 他們會使用 Git 將儲存機制複製到開發電腦本機：

`git clone https://github.com/Microsoft/SmartHotel360-Backend.git`

### <a name="provision-aks-and-azure-container-registry"></a>布建 AKS 和 Azure Container Registry

Contoso 管理員會布建 AKS 和 Azure Container Registry，如下所示：

1. 在 Visual Studio Code 中，他們會開啟資料夾，並移至 `/deploy/k8s` 包含腳本的目錄 `gen-aks-env.ps1` 。

2. 他們會執行腳本，以使用 AKS 和 Azure Container Registry 建立受控 Kubernetes 叢集。

    ![顯示 Visual Studio Code 中 AKS 腳本的螢幕擷取畫面。](./media/contoso-migration-rebuild/aks1.png)
    *圖3：建立受控 Kubernetes 叢集。*

3. 開啟檔案後，他們會將 $location 的參數更新為 `eastus2` ，然後儲存檔案。

    ![螢幕擷取畫面，顯示更新為 eastus2 的 AKS $location 參數。](./media/contoso-migration-rebuild/aks2.png)
    *圖4：儲存檔案。*

4. 他們會選取 [ **View**  >  **整合式終端** 機]，以開啟 Visual Studio Code 中的整合式終端機。

    ![顯示 [整合式終端機] 連結的螢幕擷取畫面。](./media/contoso-migration-rebuild/aks3.png)
    *圖5： Visual Studio Code 中的終端機。*

5. 在 PowerShell 整合式終端機中，他們會使用命令登入 Azure `Connect-AzureRmAccount` 。 如需詳細資訊，請參閱 [開始使用 PowerShell](/powershell/azure/get-started-azureps)。

    ![PowerShell 整合式終端機之登入視窗的螢幕擷取畫面。](./media/contoso-migration-rebuild/aks4.png)
    *圖6： PowerShell 整合式終端機。*

6. 他們會執行 `az login` 命令，並遵循指示以使用其網頁瀏覽器進行驗證，藉以驗證 AZURE CLI。 深入瞭解如何 [使用 AZURE CLI 登入](/cli/azure/authenticate-azure-cli)。

    ![顯示 Azure CLI 驗證視窗的螢幕擷取畫面。](./media/contoso-migration-rebuild/aks5.png)
    *圖7：驗證 Azure CLI。*

7. 它們會在傳遞的資源組名、AKS 叢集的 `ContosoRG` 名稱和新的登錄名稱時，執行下列命令 `smarthotel-aks-eus2` 。

    `.\gen-aks-env.ps1  -resourceGroupName ContosoRg -orchestratorName smarthotelakseus2 -registryName smarthotelacreus2`

    ![螢幕擷取畫面，顯示 [資源群組] 窗格上的 smarthotel 命令。](./media/contoso-migration-rebuild/aks6.png)
    *圖8：執行命令。*

8. Azure 會建立另一個資源群組，其中包含 AKS 叢集的資源。

    ![建立資源群組的螢幕擷取畫面。](./media/contoso-migration-rebuild/aks7.png)
    *圖9： Azure 建立資源群組。*

9. 部署完成後，他們會安裝 `kubectl` 命令列工具。 Azure Cloud Shell 上已安裝此工具。

    `az aks install-cli`

10. 他們會藉由執行命令來驗證叢集的連接 `kubectl get nodes` 。 節點的名稱與自動建立的資源群組中的 VM 相同。

    ![顯示叢集連接驗證的螢幕擷取畫面。 ](./media/contoso-migration-rebuild/aks8.png)
    *圖10：驗證叢集的連接。*

11. 他們會執行下列命令來啟動 Kubernetes 儀表板：

    `az aks browse --resource-group ContosoRG --name smarthotelakseus2`

12. [瀏覽器] 索引標籤會開啟至儀表板。 這是使用 Azure CLI 的通道連線。

    ![顯示通道連接的螢幕擷取畫面。 ](./media/contoso-migration-rebuild/aks9.png)
    *圖11：通道連接。*

## <a name="step-2-configure-the-back-end-pipeline"></a>步驟 2：設定後端管線

### <a name="create-an-azure-devops-project-and-build"></a>建立 Azure DevOps 專案和組建

Contoso 會建立 Azure DevOps 專案、設定 CI 組建來建立容器，然後將其推送至容器登錄。 本節中的指示會使用 [SmartHotel360 後端](https://github.com/Microsoft/SmartHotel360-Backend) 存放庫。

1. `visualstudio.com`他們會從建立新的組織 (`contosodevops360.visualstudio.com`) ，並將它設定為使用 Git。

2. 他們會建立新的專案 (`SmartHotelBackend`) ，選取 **Git** 進行版本控制，並為工作流程選取 **Agile** 。

    ![Azure DevOps [建立新專案] 窗格的螢幕擷取畫面。](./media/contoso-migration-rebuild/vsts1.png)
    *圖12：建立新專案。*

3. 他們會匯入 [GitHub](https://github.com/Microsoft/SmartHotel360-Backend)存放庫。

    ![Azure DevOps [匯入 Git 存放庫] 窗格的螢幕擷取畫面。](./media/contoso-migration-rebuild/vsts2.png)
    *圖13：匯入 GitHub 存放庫。*

4. 在 **管線** 中，他們會選取 [ **建立** ]，並使用 Azure 存放庫 Git 作為存放庫中的來源，以建立新的管線。

    ![用於建立新管線的 [DevOps] 窗格螢幕擷取畫面。](./media/contoso-migration-rebuild/vsts3.png)
    *圖14：建立新的管線。*

5. 他們會選取 **空白的工作**。

    ![Azure DevOps 中 [空白作業] 連結的螢幕擷取畫面。](./media/contoso-migration-rebuild/vsts4.png)
    *圖15：從空的作業開始。*

6. 他們會選取 [裝載的 Linux 預覽] 作為建置管線。

    ![在 Azure DevOps 中設定組建管線的螢幕擷取畫面。](./media/contoso-migration-rebuild/vsts5.png)
    *圖16：設定組建管線。*

7. 在 **步驟 1** 中，他們會新增 **Docker 撰寫** 工作。 這項工作會建立 Docker 撰寫。

    ![在 Azure DevOps 中建立 Docker 撰寫工作的螢幕擷取畫面。](./media/contoso-migration-rebuild/vsts6.png)
    *圖17：建立 Docker 撰寫。*

8. 他們會重複相同步驟，以新增另一個 [Docker Compose] 工作。 這會將容器推送至容器登錄。

     ![在 Azure DevOps 中新增另一個 Docker 撰寫工作的螢幕擷取畫面。](./media/contoso-migration-rebuild/vsts7.png)
    *圖18：新增另一個 Docker 撰寫工作。*

9. 他們會選取第一個工作，以使用 Azure 訂用帳戶、授權和 Container Registry 來建立和設定組建。

    ![在 Azure DevOps 中建立和設定組建的螢幕擷取畫面。](./media/contoso-migration-rebuild/vsts8.png)
    *圖19：建立和設定組建。*

10. 他們會指定存放庫資料夾中檔案的路徑 `docker-compose.yaml` `src` 。 他們選擇建立服務映射，並包含最新的標記。 當動作變更為 [建置服務映像] 時，Azure DevOps 工作的名稱會變更為 [自動建置服務]。

    ![Azure DevOps 中各種工作建立細節的螢幕擷取畫面。 ](./media/contoso-migration-rebuild/vsts9.png)
    *圖20：工作的詳細資訊。*

11. 現在，他們會設定第二個 Docker 工作 (用來推送)。 他們會選取訂用帳戶和 container registry (`smarthotelacreus2`) 。

    ![在 Azure DevOps 中設定第二個 Docker 工作的螢幕擷取畫面。 ](./media/contoso-migration-rebuild/vsts10.png)
    *圖21：設定第二個 Docker 工作。*

12. 他們會輸入 `docker-compose.yaml` 檔案名，然後選取 [ **推送服務映射**]，包括最新的標記。 當動作變更為 [推送服務映像] 時，Azure DevOps 工作的名稱會變更為 [自動推送服務]。

    ![變更 Azure DevOps 工作名稱的螢幕擷取畫面。 ](./media/contoso-migration-rebuild/vsts11.png)
    *圖22：變更 Azure DevOps 工作名稱。*

13. 設定好 Azure DevOps 工作之後，Contoso 會儲存組建管線並開始建立程式。

    ![在 Azure DevOps 中啟動組建流程的螢幕擷取畫面。 ](./media/contoso-migration-rebuild/vsts12.png)
    *圖23：啟動組建進程。*

14. 他們會選取建置作業以檢查進度。

    ![在 Azure DevOps 中檢查組建進度的螢幕擷取畫面。 ](./media/contoso-migration-rebuild/vsts13.png)
    *圖24：檢查進度。*

15. 組建完成後，容器登錄會顯示新的存放庫，其會填入微服務所使用的容器。

    ![在 Azure DevOps 中完成組建之後，查看新存放庫的螢幕擷取畫面。 ](./media/contoso-migration-rebuild/vsts14.png)
    *圖25：組建完成之後，請查看新的存放庫。*

### <a name="deploy-the-back-end-infrastructure"></a>部署後端基礎結構

建立 AKS 叢集並建置 Docker 映像之後，Contoso 管理員現在會部署將供後端微服務使用的基礎結構其餘部分。

- 本節中的指示會使用 [SmartHotel360 後端](https://github.com/Microsoft/SmartHotel360-Backend) 存放庫。
- 在此 `/deploy/k8s/arm` 資料夾中，有一個用來建立所有專案的腳本。

系統管理員會部署基礎結構，如下所示：

1. 他們會開啟開發人員命令提示字元，然後使用 Azure 訂用帳戶的命令 `az login` 。

2. 他們會在 `deploy.cmd` `ContosoRG` 資源群組和 `East US 2` 區域中輸入下列命令，以使用該檔案來部署 Azure 資源：

    `.\deploy.cmd azuredeploy ContosoRG -c eastus2`

    ![部署後端的螢幕擷取畫面。](./media/contoso-migration-rebuild/backend1.png)
    *圖26：部署後端。*

3. 在 Azure 入口網站中，它們會為每個資料庫捕獲連接字串，以供稍後使用。

    ![顯示每個資料庫之連接字串的螢幕擷取畫面。](./media/contoso-migration-rebuild/backend2.png)
    *圖27：捕獲每個資料庫的連接字串。*

### <a name="create-the-back-end-release-pipeline"></a>建立後端發行管線

現在，Contoso 管理員會執行下列操作：

- 部署 NGINX 輸入控制器，以允許對服務傳送的輸入流量。
- 將微服務部署至 AKS 叢集。
- 在第一個步驟中，系統管理員會使用 Azure DevOps 來更新微服務的連接字串。 然後，他們會設定新的 Azure DevOps 發行管線來部署微服務。
- 本節中的指示會使用 [SmartHotel360 後端](https://github.com/Microsoft/SmartHotel360-Backend) 存放庫。
- 某些設定設定 (例如，Active Directory B2C) 未涵蓋在本文中。 如需這些設定的詳細資訊，請參閱先前提及的 SmartHotel360-Backend 存放庫。

系統管理員會建立管線：

1. 在 Visual Studio 中，他們會 `/deploy/k8s/config_local.yml` 使用先前記下的資料庫連接資訊來更新檔案。

2. 他們會開啟 Azure DevOps，並在 SmartHotel360 專案中，選取 [**發行**] 窗格上的 [ **+ 新增管線**]。

    ![顯示 Azure DevOps 中 [新增管線] 按鈕的螢幕擷取畫面。](./media/contoso-migration-rebuild/back-pipe2.png)
    *圖29：建立新的管線。*

3. 他們會選取 [空白作業] 來開始管線，而不使用範本。
4. 他們會提供階段和管線名稱。

      ![顯示在 Azure DevOps 中建立階段名稱的螢幕擷取畫面。](./media/contoso-migration-rebuild/back-pipe4.png)
    *圖30：階段名稱。*

      ![顯示在 Azure DevOps 中建立管線名稱的螢幕擷取畫面。](./media/contoso-migration-rebuild/back-pipe5.png)
    *圖31：管線名稱。*

5. 他們會新增成品。

     ![顯示在 Azure DevOps 中新增成品的螢幕擷取畫面。](./media/contoso-migration-rebuild/back-pipe6.png)
    *圖32：加入成品。*

6. 他們會選取 **Git** 作為來源類型，並指定 SmartHotel360 應用程式的專案、來源和主要分支。

    ![[新增成品] 窗格的螢幕擷取畫面，其中以 Git 作為來源類型。](./media/contoso-migration-rebuild/back-pipe7.png)
    *圖33：成品設定窗格。*

7. 他們會選取工作連結。

    ![螢幕擷取畫面，顯示 Azure DevOps 中反白顯示的工作連結。](./media/contoso-migration-rebuild/back-pipe8.png)
    *圖34：工作連結。*

8. 他們會新增 Azure PowerShell 工作，以便在 Azure 環境中執行 PowerShell 指令碼。

    ![在 Azure 中新增 PowerShell 工作的螢幕擷取畫面。](./media/contoso-migration-rebuild/back-pipe9.png)
    *圖35：加入新的 PowerShell 工作。*

9. 他們會選取工作的 Azure 訂用帳戶，然後 `deploy.ps1` 從 Git 存放庫選取腳本。

    ![從 Git 存放庫選取要執行之腳本的螢幕擷取畫面。](./media/contoso-migration-rebuild/back-pipe10.png)
    *圖36：執行腳本。*

10. 他們會新增指令碼的引數。 此指令碼會刪除所有叢集內容 (**ingress** 和 **ingress controller** 除外)，然後部署微服務。

    ![螢幕擷取畫面，顯示要加入至腳本的引數。 ](./media/contoso-migration-rebuild/back-pipe11.png)
    *圖37：將引數加入至腳本。*

11. 他們會將慣用的 Azure PowerShell 版本設定為最新版本，並儲存管線。

12. 他們會回到 [ **建立新的發行** ] 窗格，並手動建立新的版本。

    ![[建立新發行] 窗格的螢幕擷取畫面。 ](./media/contoso-migration-rebuild/back-pipe12.png)
    *圖38：手動建立新的版本。*

13. 建立發行之後，他們會選取它，然後在 [ **動作**] 底下選取 [ **部署**]。

      ![反白顯示部署發行的 [部署] 按鈕的螢幕擷取畫面。 ](./media/contoso-migration-rebuild/back-pipe13.png)
    *圖39：部署發行。*

14. 當部署完成時，他們會執行下列命令，使用 Azure Cloud Shell 來檢查服務的狀態： `kubectl get services` 。

## <a name="step-3-provision-front-end-services"></a>步驟 3：佈建前端服務

Contoso 管理員需要部署將供前端應用程式使用的基礎結構。 他們會建立：

- 用來儲存寵物影像的 Blob 儲存體容器。
- Azure Cosmos DB 資料庫，用來儲存包含寵物資訊的檔。
- 網站的電腦視覺 API。

本節的指示會使用 [SmartHotel360 網站](https://github.com/microsoft/smartHotel360-website) 存放庫。

### <a name="create-blob-storage-containers"></a>建立 Blob 儲存體容器

1. 在 Azure 入口網站中，系統管理員開啟已建立的儲存體帳戶，然後選取 [ **blob**]。
2. 他們會建立名為的新容器 `Pets` ，並為容器設定公用存取層級。 使用者會將其寵物相片上傳至此容器。

    ![在 Azure 入口網站中建立新容器的螢幕擷取畫面。](./media/contoso-migration-rebuild/blob1.png)
    *圖40：建立新的容器。*

3. 他們會建立名為的第二個新容器 `settings` 。 具有所有前端應用程式設定的檔案將會放在此容器中。

    ![在 Azure 入口網站中建立第二個新容器的螢幕擷取畫面。](./media/contoso-migration-rebuild/blob2.png)
    *圖41：建立第二個容器。*

4. 他們會在文字檔中取得儲存體帳戶的存取詳細資料，以供日後參考。

    ![用來捕捉存取詳細資料的文字檔螢幕擷取畫面。](./media/contoso-migration-rebuild/blob3.png)
    *圖42：用來捕捉存取詳細資料的文字檔。*

### <a name="provision-an-azure-cosmos-db-database"></a>布建 Azure Cosmos DB 資料庫

Contoso 管理員會布建要用於寵物資訊的 Azure Cosmos DB 資料庫。

1. 他們會在 Azure Marketplace 中建立 **Azure COSMOS DB** 資料庫。

    ![顯示在 Azure Marketplace 中建立 Azure Cosmos DB 資料庫的螢幕擷取畫面。](./media/contoso-migration-rebuild/cosmos1.png)
    *圖43：建立 Azure Cosmos DB 資料庫。*

2. 他們會指定名稱 `contososmarthotel` 、選取 SQL API，並將它放在主要區域的生產資源群組中 `ContosoRG` `East US 2` 。

    ![Azure Cosmos DB 資料庫名稱和其他設定的螢幕擷取畫面。](./media/contoso-migration-rebuild/cosmos2.png)
    *圖44：命名 Azure Cosmos DB 資料庫。*

3. 他們會在資料庫內新增集合，並為其設定預設容量與輸送量。

    ![Azure Cosmos DB 的 [新增集合] 窗格螢幕擷取畫面。](./media/contoso-migration-rebuild/cosmos3.png)
    *圖45：將新的集合加入至資料庫。*

4. 他們會記下資料庫的連接資訊，以供日後參考。

    ![Azure Cosmos DB 資料庫連線資訊的螢幕擷取畫面。](./media/contoso-migration-rebuild/cosmos4.png)
    *圖46：資料庫的連接資訊。*

### <a name="provision-the-computer-vision-api"></a>布建電腦視覺 API

Contoso 管理員會佈建「電腦視覺 API」。 此函式會呼叫 API，以評估使用者所上傳的圖片。

1. 系統管理員會在 Azure Marketplace 中建立 **電腦視覺** 實例。

     ![在 Azure Marketplace 中建立新電腦視覺實例的螢幕擷取畫面。](./media/contoso-migration-rebuild/vision1.png)
    *圖47： Azure Marketplace 中的新實例。*

2. 他們會在 `smarthotelpets` `ContosoRG` 主要區域 () 的生產資源群組中布建 API () `East US 2` 。

    ![在生產資源群組中設定 API 的螢幕擷取畫面。](./media/contoso-migration-rebuild/vision2.png)
    *圖48：在生產資源群組中布建 API。*

3. 他們會將 API 的連線設定儲存至文字檔，以供日後參考。

     ![將 API 連接設定儲存至文字檔的螢幕擷取畫面。](./media/contoso-migration-rebuild/vision3.png)
    *圖49：儲存 API 連接設定。*

### <a name="provision-the-azure-web-app"></a>佈建 Azure Web 應用程式

Contoso 管理員會使用 Azure 入口網站布建 web 應用程式。

1. 在入口網站中選取 [Web 應用程式]。

    ![在 Azure 入口網站中選取 web 應用程式的螢幕擷取畫面。](./media/contoso-migration-rebuild/web-app1.png)
    *圖50：選取 web 應用程式。*

2. 他們提供 web 應用程式名稱 (`smarthotelcontoso`) 、在 Windows 上執行，然後將其放在生產資源群組中 `ContosoRG` 。 他們會建立新的 Application Insights 實例來監視應用程式。

    ![提供 web 應用程式名稱和其他詳細資料的螢幕擷取畫面。](./media/contoso-migration-rebuild/web-app2.png)
    *圖51： web 應用程式名稱。*

3. 完成之後，系統管理員就可以流覽至應用程式的位址，以檢查是否已成功建立該應用程式。

4. 在 Azure 入口網站中，他們會建立程式碼的預備位置。 管線將部署到這個位置。 這種方法可確保在系統管理員執行發行之前，程式碼不會進入生產環境。

    ![新增 web 應用程式預備位置的螢幕擷取畫面。](./media/contoso-migration-rebuild/web-app3.png)
    *圖52：新增 web 應用程式預備位置。*

### <a name="provision-the-function-app"></a>布建函數應用程式

在 Azure 入口網站中，Contoso 管理員會布建函數應用程式。

1. 他們會選取 [函數應用程式]。

    ![顯示建立函數應用程式的螢幕擷取畫面。](./media/contoso-migration-rebuild/function-app1.png)
    *圖53：選取函數應用程式。*

2. 它們會提供函數應用程式的名稱 (`smarthotelpetchecker`) 。 它們會將函數應用程式放在生產資源群組中 (`ContosoRG`) 。 他們將裝載位置設定為取用 **方案**，然後將函數應用程式放在 `East US 2` 區域中。 系統會建立新的儲存體帳戶，以及用於監視的 Application Insights 實例。

    ![顯示函數應用程式設定的螢幕擷取畫面。](./media/contoso-migration-rebuild/function-app2.png)
    *圖54：函數應用程式設定。*

3. 在部署函數應用程式之後，系統管理員會流覽至其位址，以確認是否已成功建立該應用程式。

## <a name="step-4-set-up-the-front-end-pipeline"></a>步驟 4：設定前端管線

Contoso 管理員會為前端網站建立兩個不同的專案。

1. 在 Azure DevOps 中，他們會建立一個專案 `SmartHotelFrontend` 。

    ![建立前端專案的螢幕擷取畫面。](./media/contoso-migration-rebuild/function-app1.png)
    *圖55：建立前端專案。*

2. 他們會將 [SmartHotel360 front-end](https://github.com/Microsoft/SmartHotel360-Website) Git 存放庫匯入至新專案。

3. 針對函數應用程式，他們會建立另一個 Azure DevOps 專案 (`SmartHotelPetChecker`) ，並將 [PetChecker](https://github.com/sonahander/SmartHotel360-PetCheckerFunction) Git 存放庫匯入此專案。

### <a name="configure-the-web-app"></a>設定 Web 應用程式

Contoso 管理員現在會設定 Web 應用程式以使用 Contoso 資源。

1. 系統管理員會連線到 Azure DevOps 專案，並在本機將存放庫複製到開發機器。
2. 在 Visual Studio 中，他們會開啟資料夾以顯示存放庫中的所有檔案。

    ![Visual Studio 的螢幕擷取畫面，其中顯示包含存放庫中所有檔案的資料夾。](./media/contoso-migration-rebuild/configure-webapp1.png)
    *圖56：查看存放庫中的所有檔案。*

3. 他們會視需要更新設定變更。

    - 當 web 應用程式啟動時，它會尋找 `SettingsUrl` 應用程式設定。
    - 此變數必須包含指向設定檔的 URL。
    - 根據預設，使用的設定是公用端點。

4. 他們會更新檔案 `/config-sample.json/sample.json` 。

    - 這是使用公用端點時的 web 設定檔。
    - 他們會 `urls` `pets_config` 使用 AKS API 端點、儲存體帳戶和 AZURE Cosmos DB 資料庫的值來編輯和區段。
    - URL 應符合 Contoso 所會建立的新 Web 應用程式 DNS 名稱。
    - 若為 Contoso，則為 `smarthotelcontoso.eastus2.cloudapp.azure.com` 。

    ![Visual Studio 中 json 設定的螢幕擷取畫面。](./media/contoso-migration-rebuild/configure-webapp2.png)
    *圖57： json 設定。*

5. 在更新檔案之後，系統管理員會將其重新命名， `smarthotelsettingsurl` 並將其上傳至先前建立的 Blob 儲存體。

    ![重新命名和上傳 json 檔案的螢幕擷取畫面。](./media/contoso-migration-rebuild/configure-webapp3.png)
    *圖58：重新命名及上傳檔案。*

6. 他們會選取檔案以取得 URL。 應用程式會在提取設定檔時使用此 URL。

    ![應用程式所使用之檔案 URL 的螢幕擷取畫面。](./media/contoso-migration-rebuild/configure-webapp4.png)
    *圖59：應用程式 URL。*

7. 在檔案中 `appsettings.Production.json` ，它們會將 `SettingsURL` 更新為新檔案的 URL。

    ![將 URL 更新為新檔案的螢幕擷取畫面。](./media/contoso-migration-rebuild/configure-webapp5.png)
    *圖60：更新新檔案的 URL。*

### <a name="deploy-the-website-to-azure-app-service"></a>將網站部署至 Azure App Service

Contoso 管理員現在已可發佈網站。

1. 他們會開啟 Azure DevOps，並在 `SmartHotelFrontend` **組建和版本** 的專案中選取 [ **+ 新增管線**]。
2. 他們會選取 [Azure DevOps Git] 作為來源。
3. 他們會選取 [ASP.NET Core] 範本。
4. 他們會檢查管線，並檢查以確定已選取 [ **發行 Web 專案** ] 和 [ **Zip 已發行] 專案** 。

    ![Web 專案之管線設定的螢幕擷取畫面。](./media/contoso-migration-rebuild/vsts-publish-front2.png)
    *圖61：管線設定。*

5. 在 **觸發** 程式中，他們會啟用持續整合並新增主要分支。 這可確保每次方案將新的程式碼認可至主要分支時，組建管線便會啟動。

    ![啟用持續整合和新增主要分支的螢幕擷取畫面。](./media/contoso-migration-rebuild/vsts-publish-front3.png)
    *圖62：啟用持續整合。*

6. 他們會選取 [儲存並排入佇列] 來啟動建置作業。
7. 組建完成後，他們會使用 **Azure App Service 部署** 來設定發行管線。
8. 它們提供階段名稱和 **預備** 環境。

    ![提供環境階段名稱的螢幕擷取畫面。](./media/contoso-migration-rebuild/vsts-publish-front4.png)
    *圖63：命名環境。*

9. 他們會新增成品，並選取它們所設定的組建。

    ![新增成品的螢幕擷取畫面，其中組建為來源類型。](./media/contoso-migration-rebuild/vsts-publish-front5.png)
    *圖64：加入成品。*

10. 他們會選取成品上的閃電圖示，然後將 [持續部署] 設定為 [ **啟用**]。

    ![啟用持續部署的螢幕擷取畫面。 ](./media/contoso-migration-rebuild/vsts-publish-front6.png)
    *圖65：啟用持續部署。*

11. 在 [環境] 中，他們會選取 [Staging] 底下的 [1 個作業, 1 個工作]。

12. 選取訂用帳戶和 web 應用程式名稱之後，系統管理員會開啟 [ **部署 Azure App Service** ] 工作。 此部署已設定成使用 [預備環境] 部署位置。 這會自動在此位置建置要檢閱和核准的程式碼。

     ![將 web 應用程式部署至某個位置的螢幕擷取畫面。 ](./media/contoso-migration-rebuild/vsts-publish-front7.png)
    *圖66：部署至位置。*

13. 在 [管線] 中，他們會新增新的階段。

    ![[管線] 索引標籤的螢幕擷取畫面，並新增新的階段。 ](./media/contoso-migration-rebuild/vsts-publish-front8.png)
    *圖67：加入新的階段。*

14. 他們會選取具有位置的 **Azure App Service 部署** ，然後將環境命名為 **生產** 環境。

15. 他們會選取 [ **1 個作業]、[2 個工作** ]，然後選取訂用帳戶、應用程式服務名稱和 **預備** 位置。

    ![顯示命名環境的螢幕擷取畫面。 ](./media/contoso-migration-rebuild/vsts-publish-front10.png)
    *圖68：命名環境。*

16. 他們會從管線中移除 [將 Azure App Service 部署至位置]。 這是先前的步驟所放置。

    ![顯示從管線中移除位置的螢幕擷取畫面。 ](./media/contoso-migration-rebuild/vsts-publish-front11.png)
    *圖69：從管線中移除插槽。*

17. 他們會儲存管線。 他們會在管線上選取 [部署後的條件]。

    ![[部署後的條件] 按鈕的螢幕擷取畫面。 ](./media/contoso-migration-rebuild/vsts-publish-front12.png)
    *圖70：部署後的條件。*

18. 他們會啟用 **部署後核准** ，然後將開發組長新增為核准者。

    ![啟用的部署後核准者清單螢幕擷取畫面。 ](./media/contoso-migration-rebuild/vsts-publish-front13.png)
    *圖71：加入核准者。*

19. 在組建管線中，系統管理員會手動啟動組建。 這會觸發新的發行管線，而將網站部署至預備位置。 就 Contoso 而言，此位置的 URL 是`https://smarthotelcontoso-staging.azurewebsites.net/`。

20. 完成組建並將發行部署到該位置之後，Azure DevOps 會將開發人員的電子郵件傳送給開發人員以進行核准。

21. 開發主管會選取 [ **View 核准** ]，並可在 Azure DevOps 入口網站中核准或拒絕要求。

    ![部署後核准 * * [核准] 或 [拒絕] 連結的螢幕擷取畫面。 ](./media/contoso-migration-rebuild/vsts-publish-front14.png)
    *圖72：暫止的發行核准要求。*

22. 開發主管會提出批註並核准。 這會開始交換 **預備** 和 **生產位置，並** 將組建移至生產環境。

    ![部署後核准和批註的螢幕擷取畫面。 ](./media/contoso-migration-rebuild/vsts-publish-front15.png)
    *圖73：將組建移至生產環境。*

23. 管線會完成交換。

    ![顯示組建部署狀態的螢幕擷取畫面。 ](./media/contoso-migration-rebuild/vsts-publish-front16.png)
    *圖74：正在完成交換。*

24. 小組會檢查 [生產環境] 位置，以確認該 Web 應用程式已於 `https://smarthotelcontoso.azurewebsites.net/` 投入生產環境。

### <a name="deploy-the-petchecker-function-app"></a>部署 PetChecker 函數應用程式

Contoso 管理員會藉由執行下列步驟來部署應用程式：

1. 他們會連線至 Azure DevOps 專案，以從本機將存放庫複製到開發機器。
2. 在 Visual Studio 中，他們會開啟資料夾以顯示存放庫中的所有檔案。
3. 他們會開啟檔案 `src/PetCheckerFunction/local.settings.json` ，並新增儲存體、Azure COSMOS DB 資料庫和電腦視覺 API 的應用程式設定。

    ![Visual Studio 中的 json 檔案中應用程式設定的螢幕擷取畫面。](./media/contoso-migration-rebuild/function5.png)
    *圖75：部署函數。*

4. 他們會認可程式碼，並將其同步處理回 Azure DevOps，並推送其變更。
5. 他們會新增新的組建管線，然後選取 **Azure DevOps Git** 作為來源。
6. 他們會選取 [ASP.NET Core (.NET Framework)] 範本。
7. 他們會接受範本的預設值。
8. 在 **觸發** 程式底下，選取 [ **啟用持續整合** ]，然後選取 [ **儲存 & 佇列** ] 以啟動組建。
9. 組建成功之後，他們會建立發行管線，並新增 **具有位置的 Azure App Service 部署**。

10. 他們將環境命名為 **生產** 環境，然後選取訂用帳戶。 他們會將 **應用程式類型** 設定為函式 **應用程式** ，並將應用程式服務名稱設定為 `smarthotelpetchecker` 。

    ![應用程式類型和應用程式服務名稱的螢幕擷取畫面。 ](./media/contoso-migration-rebuild/petchecker2.png)
    *圖76：函數應用程式。*

11. 他們會新增成品 **Build**。

    ![新增成品的螢幕擷取畫面，其中包含組建來源類型。 ](./media/contoso-migration-rebuild/petchecker3.png)
    *圖77：加入成品。*

12. 他們會啟用 **持續部署觸發** 程式，然後選取 [ **儲存**]。

13. 他們會選取 [將新組建排入佇列]，以執行完整的 CI/CD 管線。

14. 函式部署完成後，它會出現在 Azure 入口網站中， **且狀態為 [執行中]**。

    ![函式應用程式的螢幕擷取畫面，其中顯示 [正在執行] 狀態。 ](./media/contoso-migration-rebuild/function6.png)
    *圖78：更新函式的狀態。*

15. 他們會流覽至寵物檢查應用程式， `http://smarthotel360public.azurewebsites.net/pets` 以確認它是否正常運作。

16. 他們會選取虛擬人偶以上傳圖片。

    ![將圖片指派給頭像的窗格螢幕擷取畫面。 ](./media/contoso-migration-rebuild/function7.png)
    *圖79：將圖片指派給頭像。*

17. 他們想要檢查的第一張相片是一頭小型犬。

    ![顯示狗相片的螢幕擷取畫面。 ](./media/contoso-migration-rebuild/function8.png)
    *圖80：檢查相片。*

18. 應用程式會傳回接受訊息。

    ![接受訊息的螢幕擷取畫面。 ](./media/contoso-migration-rebuild/function9.png)
    *圖81：接受訊息。*

## <a name="review-the-deployment"></a>檢閱部署

在 Azure 中有了所移轉的資源之後，Contoso 現在必須讓新基礎結構完整運作且受到保護。

### <a name="security"></a>安全性

- Contoso 必須確保新資料庫安全無虞。 若要深入瞭解，請參閱 [AZURE Sql Database 和 SQL 受控實例安全性功能的總覽](/azure/azure-sql/database/security-overview)。
- 您必須將應用程式更新為搭配使用 SSL 與憑證。 容器執行個體應重新部署為會在 443 上接聽。
- Contoso 應考慮使用 Azure Key Vault 來協助保護其 Service Fabric 應用程式的秘密。 若要深入瞭解，請參閱 [管理 Service Fabric 應用程式中的加密密碼](/azure/service-fabric/service-fabric-application-secret-management)。

### <a name="backups-and-disaster-recovery"></a>備份和災害復原

- Contoso 必須檢查 [AZURE SQL Database 的備份需求](/azure/azure-sql/database/automated-backups-overview)。
- Contoso 應考慮執行 [SQL 容錯移轉群組，以提供資料庫的區域性容錯移轉](/azure/azure-sql/database/auto-failover-group-overview)。
- Contoso 可以使用 [Azure Container Registry PREMIUM SKU 的異地](/azure/container-registry/container-registry-geo-replication)複寫。
- Azure Cosmos DB 會自動備份。 若要深入瞭解，請參閱 [Azure COSMOS DB 中的線上備份和隨選資料還原](/azure/cosmos-db/online-backup-and-restore)。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- 部署好所有資源之後，Contoso 應根據[基礎結構規劃](./contoso-migration-infrastructure.md#set-up-tagging)來指派 Azure 標記。
- 所有授權費用都會併入 Contoso 使用的 PaaS 服務中。 這將從 Enterprise 合約中扣除。
- Contoso 會啟用 [Azure 成本管理 + 計費](/azure/cost-management-billing/cost-management-billing-overview) ，以協助監視和管理 azure 資源。

## <a name="conclusion"></a>結論

在本文中，Contoso 會在 Azure 中重建 SmartHotel360 應用程式。 針對 Azure App Service web apps 重建內部部署應用程式前端 VM。 應用程式後端是使用部署至 AKS 所管理之容器的微服務所建立。 Contoso 使用寵物相片應用程式增強的功能。

## <a name="suggested-skills"></a>建議的技能

Microsoft Learn 是新的學習方法。 針對雲端採用所帶來的新技術和責任做好準備並不容易。 Microsoft Learn 提供了更有價值的學習方法，可協助您更快達成目標。 透過 Microsoft 的學習，您可以贏得各層級，並達成更多目標。

以下是 Microsoft 量身打造的兩個範例，說明如何與 Azure 中的 Contoso SmartHotel360 應用程式保持一致。

<!-- docsTest:casing "computer vision functionality" -->

- **[使用 Azure App Service 將網站部署至 azure](/learn/paths/deploy-a-website-with-azure-app-service/)：** 藉由在 Azure 中建立 web 應用程式，您可以輕鬆地發佈及管理網站，而不需要使用基礎伺服器、儲存體或網路資產。 相反地，您可以專注于網站功能，並依賴健全的 Azure 平臺來協助提供網站的安全存取。

- **[使用 Azure 認知視覺服務來處理和分類影像](/learn/paths/process-classify-images-with-azure-cognitive-vision-services/)：** Azure 認知服務提供預建功能，可在您的應用程式中啟用電腦視覺功能。 瞭解如何使用 Azure 認知視覺服務來偵測臉部、標記和分類影像，以及識別物件。
