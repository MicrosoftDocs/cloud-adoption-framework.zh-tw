---
title: 將 Team Foundation Server 部署重構到 Azure DevOps Services
description: 使用適用于 Azure 的雲端採用架構，以瞭解如何藉由將內部部署 Team Foundation Server 部署遷移至 Azure 中的 Azure DevOps Services 來進行重構。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: site-recovery
ms.openlocfilehash: 7a9845d302289567d96c807e110520dc6cac497a
ms.sourcegitcommit: 9662234674e663bc7d4bc134d303520cb146bd95
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/04/2020
ms.locfileid: "87560486"
---
<!-- cSpell:ignore contosodev contosodevmigration contosomigration onmicrosoft visualstudio sourceconnectionstring smarthotelcontainer identitymaplog CONTOSOTFS DACPAC SQLDB SQLSERVERNAME INSTANCENAME sqlpackage SSDT azuredevopsmigration validateonly ImportType -->

# <a name="refactor-a-team-foundation-server-deployment-to-azure-devops-services"></a>將 Team Foundation Server 部署重構到 Azure DevOps Services

本文說明虛構公司 Contoso 如何藉由將它遷移至 Azure 中的 Azure DevOps Services，來重構其內部部署 Visual Studio Team Foundation Server 部署。 Contoso 開發小組已使用 Team Foundation Server 來進行小組共同作業，以及在過去五年的原始檔控制。 現在，小組想要移至雲端式解決方案以進行開發和測試工作，以及進行原始檔控制。 Azure DevOps Services 將扮演角色，因為 Contoso 小組會移至 Azure DevOps 模型，並開發新的雲端原生應用程式。

## <a name="business-drivers"></a>商業動機

Contoso IT 領導小組與商務合作夥伴密切合作，以找出未來的目標。 合作夥伴並不太關心開發工具和技術，但是小組已捕捉到下列幾點：

- **軟體**：無論核心業務為何，現今所有的公司都可說是軟體公司，包括 Contoso 在內。 商務領導能力會對 IT 部門有興趣，讓公司能夠為使用者提供新的工作實務，並為客戶提供新的體驗。
- **效率**： Contoso 必須簡化其程式，並為開發人員和使用者移除不必要的程式。 這麼做可讓公司更有效率地交付客戶需求。 企業需要 IT 快速移動，而不浪費時間或金錢。
- 彈性 **：若**要在全球經濟中實現成功，Contoso IT 必須能夠更快地回應企業的需求。 It 必須能夠更快地回應 marketplace 中的變更。 它不能用來取得或成為商務封鎖程式。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已將下列目標固定在一起，以 Azure DevOps Services 進行遷移：

- 小組需要工具將其資料移轉至雲端。 有幾項程序必須手動執行。
- 他們必須移轉去年的工作項目資料和歷程記錄。
- 小組不想設定新的使用者名稱和密碼。 目前的所有系統指派都必須保留。
- 小組想要從 Team Foundation 版本控制 (TFVC) 移轉至 Git，以進行原始檔控制。
- 轉換至 Git 將會是只匯入最新版本原始程式碼的 tip 遷移。 當所有工作都將在程式碼基底轉移時停止時，就會在停機期間發生轉換。 小組了解，在移動之後，將只有目前的主要分支歷程記錄可供使用。
- 小組關心變更，想要在進行完整移動之前進行測試。 即使在移動到 Azure DevOps Services 之後，小組還是想要保留 Team Foundation Server 的存取權。
- 小組有多個集合，為了進一步瞭解程式，它想要從只有幾個專案的人開始。
- 小組瞭解 Team Foundation Server 集合與 Azure DevOps Services 組織之間是一對一關聯性，因此它會有多個 Url。 但這符合其目前的程式碼基底和專案分離模型。

## <a name="proposed-architecture"></a>建議的架構

- Contoso 會將其 Team Foundation Server 專案移至雲端，且不會再裝載其專案或內部部署的原始檔控制。
- Team Foundation Server 將會遷移至 Azure DevOps Services。
- Contoso 目前有一個名為的 Team Foundation Server 集合， `ContosoDev` 將會遷移至名為的 Azure DevOps Services 組織 `contosodevmigration.visualstudio.com` 。
- 去年的專案、工作專案、bug 和反覆運算將會遷移到 Azure DevOps Services。
- Contoso 會使用其 Azure Active Directory (Azure AD) 實例，當它在遷移規劃開始[部署其 Azure 基礎結構](./contoso-migration-infrastructure.md)時，就會設定它。

![提議架構的圖表。](./media/contoso-migration-tfs-vsts/architecture.png)

## <a name="migration-process"></a>移轉程序

Contoso 會按照下列方式完成移轉程序：

1. 需要大量的準備工作。 首先，Contoso 必須將其 Team Foundation Server 的執行升級為支援的層級。 Contoso 目前正在執行 Team Foundation Server 2017 Update 3，但若要使用資料庫移轉，則必須使用最新的更新來執行支援的2018版本。
1. 在 Contoso 升級之後，它會執行 Team Foundation Server 遷移工具，並驗證其集合。
1. Contoso 會建立一組準備檔案，然後執行遷移試執行以進行測試。
1. 然後，Contoso 會執行另一項移轉，這次將是包含工作項目、Bug、衝刺和程式碼的完整移轉。
1. 在遷移之後，Contoso 會將其程式碼從 TFVC 移至 Git。

![Contoso 遷移程式的圖表。](./media/contoso-migration-tfs-vsts/migration-process.png)

## <a name="prerequisites"></a>必要條件

若要執行此案例，Contoso 必須符合下列必要條件：

| 需求 | 詳細資料 |
| --- | --- |
| **Azure 訂用帳戶** | Contoso 已在本系列稍早的文章中建立訂用帳戶。 如果您沒有 Azure 訂用帳戶，請建立[免費帳戶](https://azure.microsoft.com/free)。 <br><br> 如果您建立免費帳戶，您就是訂用帳戶的管理員，並可執行所有動作。 <br><br> 如果您使用現有訂用帳戶，而且您不是系統管理員，則需要與系統管理員合作，讓其指派擁有者或參與者權限給您。 <br><br> 如果您需要更細微的許可權，請參閱[使用以角色為基礎的存取控制來管理 Site Recovery 存取 (RBAC) ](https://docs.microsoft.com/azure/site-recovery/site-recovery-role-based-linked-access-control)。 |
| **Azure 基礎結構** | Contoso 會如[用於遷移的 azure 基礎結構](./contoso-migration-infrastructure.md)中所述，設定其 azure 基礎結構。 |
| **內部部署 Team Foundation Server 實例** | 內部部署實例必須執行 Team Foundation Server 2018 升級2或升級為此程式的一部分。 |

## <a name="scenario-steps"></a>案例步驟

以下是 Contoso 完成移轉的方式：

> [!div class="checklist"]
>
> - **步驟1：建立 Azure 儲存體帳戶**。 在移轉程序期間將使用此儲存體帳戶。
> - **步驟2：升級 Team Foundation Server**。 Contoso 會將其部署升級至 Team Foundation Server 2018 upgrade 2。
> - **步驟3：驗證 Team Foundation Server 集合**。 Contoso 會驗證 Team Foundation Server 集合，以準備進行遷移。
> - **步驟4：建立遷移**檔案。 Contoso 會使用 Team Foundation Server 遷移工具來建立遷移檔案。

## <a name="step-1-create-an-azure-storage-account"></a>步驟1：建立 Azure 儲存體帳戶

1. 在 Azure 入口網站中，Contoso 管理員會建立儲存體帳戶 (`contosodevmigration`) 。
1. 他們會將帳戶放在次要區域中，以用於容錯移轉 (`Central US`) 。 他們使用具有本機備援儲存體的一般用途標準帳戶。

    ![[建立儲存體帳戶] 窗格的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/storage1.png)

**需要其他協助？**

- [Azure 儲存體簡介](https://docs.microsoft.com/azure/storage/common/storage-introduction)。
- [建立儲存體帳戶](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account)。

<!-- docsTest:ignore "Server Configuration Wizard" "Configure Features Wizard" "Detach Team Project Collection Wizard" -->

## <a name="step-2-upgrade-team-foundation-server"></a>步驟2：升級 Team Foundation Server

Contoso 管理員會將 Team Foundation Server 實例升級為 Team Foundation Server 2018 Update 2。 在開始之前，他們會：

- 下載[Team Foundation Server 2018 Update 2](https://visualstudio.microsoft.com/downloads)。
- 確認[硬體需求](https://docs.microsoft.com/azure/devops/server/requirements)。
- 閱讀[版本](https://docs.microsoft.com/visualstudio/releasenotes/tfs2018-relnotes)資訊和[升級陷阱](https://docs.microsoft.com/azure/devops/server/upgrade/get-started#before-you-upgrade-to-tfs-2018)。

他們會依照下列方式進行升級：

1. 若要開始，系統管理員會備份其在 VMware 虛擬機器上執行的 Team Foundation Server 實例（ (VM) ），並建立 VMware 快照集。

    ![[消費者入門] 窗格的螢幕擷取畫面，用以升級 Team Foundation Server。](./media/contoso-migration-tfs-vsts/upgrade1.png)

1. Team Foundation Server 安裝程式會啟動，並選擇安裝位置。 安裝程式需要存取網際網路。

    ![Visual Studio 中 Team Foundation Server 安裝網站的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/upgrade2.png)

1. 安裝完成後，會啟動 [伺服器組態精靈]。

    ![Team Foundation Server 2018 Update 2 configuration wizard 的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/upgrade3.png)

1. 驗證之後，伺服器設定向導會完成升級。

     ![Team Foundation Server configuration wizard 升級進度窗格的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/upgrade4.png)

1. 管理員會藉由檢查項目、工作專案和程式碼來確認 Team Foundation Server 安裝。

     ![[產品待處理專案] 窗格的螢幕擷取畫面，用來驗證 Team Foundation Server 安裝。](./media/contoso-migration-tfs-vsts/upgrade5.png)

> [!NOTE]
> 某些 Team Foundation Server 升級需要在升級完成之後執行 [設定功能]。 [深入了解](https://docs.microsoft.com/azure/devops/reference/configure-features-after-upgrade?utm_source=ms&utm_medium=guide&utm_campaign=vstsdataimportguide&view=vsts)。

**需要其他協助？**

瞭解如何[升級 Team Foundation Server](https://docs.microsoft.com/azure/devops/server/upgrade/get-started)。

## <a name="step-3-validate-the-team-foundation-server-collection"></a>步驟3：驗證 Team Foundation Server 集合

Contoso 管理員會對集合資料庫執行 Team Foundation Server 遷移工具 `contosodev` ，以在遷移前進行驗證。

1. 他們會下載並解壓縮[Team Foundation Server 遷移工具](https://www.microsoft.com/download/details.aspx?id=54274)。 請務必下載正在執行之 Team Foundation Server 更新的版本。 他們可在管理主控台中檢查版本。

    ![[Team Foundation Server] 窗格的螢幕擷取畫面，用來驗證產品版本。](./media/contoso-migration-tfs-vsts/collection1.png)

1. 他們會藉由指定專案集合的 URL 來執行此工具，如下列命令所示：

    `TfsMigrator validate /collection:http://contosotfs:8080/tfs/ContosoDev`

    此工具會顯示錯誤。

    ![Team Foundation Server 遷移工具中驗證錯誤的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/collection2.png)

1. 它們會在工具位置之前的資料夾中尋找記錄檔 `Logs` 。 每個主要驗證都會產生一個記錄檔。 `TfsMigration.log` 包含主要資訊。

    ![Team Foundation Server 中記錄檔位置的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/collection3.png)

1. 他們會找到與身分識別相關的此專案。

    ![顯示身分識別驗證期間發生之錯誤的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/collection4.png)

1. 它們會 `TfsMigrator validate /help` 在命令列執行，而且會看到命令 `/tenantDomainName` 似乎需要驗證身分識別。

     ![螢幕擷取畫面，其中顯示驗證身分識別所需的命令。](./media/contoso-migration-tfs-vsts/collection5.png)

1. 他們會再次執行驗證命令，並包含此值和其 Azure AD 名稱 `TfsMigrator validate /collection:http://contosotfs:8080/tfs/ContosoDev /tenantDomainName:contosomigration.onmicrosoft.com` 。

    ![顯示正確命令之命令提示字元的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/collection7.png)

1. 在開啟的 Azure AD 登入] 視窗中，他們會輸入全域管理員使用者的認證。

     ![使用系統管理員認證 Azure AD 登入視窗的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/collection8.png)

1. 驗證會通過，並由工具確認。

    ![螢幕擷取畫面，顯示已通過驗證。](./media/contoso-migration-tfs-vsts/collection9.png)

## <a name="step-4-build-the-migration-files"></a>步驟4：建立遷移檔案

完成驗證之後，Contoso 系統管理員就可以使用 Team Foundation Server 遷移工具來建立遷移檔案。

1. 他們會在工具中執行準備步驟。

    `TfsMigrator prepare /collection:http://contosotfs:8080/tfs/ContosoDev /tenantDomainName:contosomigration.onmicrosoft.com /accountRegion:cus`

     ![Team Foundation Server 遷移工具中 [準備] 命令的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/prep1.png)

    準備步驟會執行下列動作：
    - 掃描集合以尋找所有使用者的清單，然後將 () 填入識別對應記錄檔 `IdentityMapLog.csv` 。
    - 準備與 Azure AD 的連接，以尋找每個身分識別的相符項。
    - Contoso 已部署 Azure AD 並使用 Azure AD Connect 同步處理，因此「準備」命令應該能夠找到相符的身分識別，並將它們**標示為使用中。**

1. [Azure AD 登入] 畫面隨即出現，而且系統管理員會輸入全域系統管理員的認證。

    ![使用系統管理員認證 Azure AD 登入視窗的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/prep2.png)

1. 準備完成後，工具會報告已成功產生匯入檔案。

    ![遷移工具的螢幕擷取畫面，顯示集合驗證成功。](./media/contoso-migration-tfs-vsts/prep3.png)

1. 系統管理員現在可以看到*IdentityMapLog.csv*檔案和檔案*import.js*都已在新的資料夾中建立。

    ![準備](./media/contoso-migration-tfs-vsts/prep4.png)

1. 檔案會 `import.json` 提供匯入設定。 其中包含所需的組織名稱，以及儲存體帳戶詳細資料等資訊。 大部分的欄位會自動填入。 有些欄位需要使用者輸入。 系統管理員開啟檔案，並新增要建立的 Azure DevOps Services 組織名稱 `contosodevmigration` 。 使用此名稱時，Contoso Azure DevOps Services URL 會是 `contosodevmigration.visualstudio.com` 。

    ![顯示 Azure DevOps Services 組織名稱的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/prep5.png)

    > [!NOTE]
    > 必須在開始遷移之前建立組織。 您可以在完成遷移之後加以變更。

1. 系統管理員會檢查身分識別記錄對應檔，其中顯示匯入期間將會帶入 Azure DevOps Services 的帳戶。

    - 作用中的身分識別是指在匯入後會在 Azure DevOps Services 中成為使用者的身分識別。
    - 在 Azure DevOps Services 中，在遷移之後，這些身分識別將會授權並顯示為組織中的使用者。
    - 在檔案的 [預期的匯**入狀態**] 資料行中，識別會**標示為 [** 作用中]。

    ![身分識別記錄對應檔的螢幕擷取畫面，其中顯示要帶入 Azure DevOps Services 的帳戶。](./media/contoso-migration-tfs-vsts/prep6.png)

## <a name="step-5-migrate-to-azure-devops-services"></a>步驟 5：遷移至 Azure DevOps Services

完成準備之後，Contoso 管理員可以專注于遷移。 在執行遷移之後，他們會從使用 TFVC 切換至 Git 進行版本控制。

在開始之前，系統管理員會排程開發小組的停機時間，讓他們可以計畫讓集合離線以進行遷移。 

以下是他們將遵循的遷移程式：

1. 卸**離集合**。 當集合已連接並上線時，集合的身分識別資料會位於 Team Foundation Server 實例的設定資料庫中。 

    當集合與 Team Foundation Server 實例中斷連結時，就會建立該身分識別資料的複本，然後使用該集合來封裝傳輸。 如果沒有這項資料，則無法執行匯入的身分識別部分。 

    我們建議您在匯入完成之前，將集合保持中斷連結，因為無法匯入在匯入期間發生的變更。

1. **產生備份**。 下一個步驟是產生可匯入 Azure DevOps Services 的備份。  (DACPAC) 的資料層應用程式元件封裝是一種 SQL Server 功能，可將資料庫變更封裝成單一檔案，然後再部署到其他 SQL 實例。 

    您也可以將備份直接還原至 Azure DevOps Services，並使用它做為將集合資料取得至雲端的封裝方法。 Contoso 會使用此 `sqlpackage.exe` 工具來產生 DACPAC。 此工具隨附於 SQL Server Data Tools 中。

1. **上傳至儲存體**。 建立 DACPAC 之後，系統管理員會將它上傳至 Azure 儲存體。 完成上傳之後，他們會取得共用存取簽章 (SAS) ，以允許 Team Foundation Server 遷移工具存取存放裝置。
1. **填寫 [匯入**]。 接著，Contoso 可以在匯入檔案中完成遺漏的欄位，包括 DACPAC 設定。 為確保所有專案都能在完整遷移之前正常運作，系統管理員必須指定要執行_試_執行匯入。
1. **執行試**執行匯入。 「試執行匯入」可協助他們測試集合遷移。 試執行的生命週期有限，因此它們會在生產環境遷移執行之前被刪除。 試執行會在一段時間後自動刪除。 在匯入完成後傳送的成功電子郵件中包含通知 Contoso 在刪除試執行的情況下的注意事項。 小組會據此擬定記事和計畫。
1. **完成生產環境遷移**。 完成執行的遷移之後，Contoso 管理員會更新檔案，然後再次執行匯入，以進行最後的遷移 `import.json` 。

<!-- docsTest:ignore "Team Foundation Server Administration Console" -->

### <a name="detach-the-collection"></a>中斷集合的連結

在卸離集合之前，Contoso 管理員會採用本機 SQL Server 實例備份和 Team Foundation Server 實例的 VMware 快照集。

1. 在 [Team Foundation Server 管理主控台中，管理員會選取想要卸離的集合， **ContosoDev**。

    ![[Foundation Server 管理主控台] 的 [Team 專案集合] 區段的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/migrate1.png)

1. 他們會選取 [**一般**] 索引標籤，然後選取 [卸**離集合**]。

    ![「卸離集合」連結在 [Foundation Server 管理主控台] 中的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/migrate2.png)

1. 在 [卸**離 Team 專案集合**] 中，系統管理員會在 [**服務訊息**] 窗格中，為可能嘗試連接到集合中專案的使用者提供訊息。

    ![[卸離 Team 專案集合] 中 [服務訊息] 窗格的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/migrate3.png)

1. 在 [卸**離] 進度**窗格中，他們會監視進度。 當程式完成時，他們會選取 **[下一步]**。

    ![[中斷連結 Team 專案集合] 中的 [卸離進度] 窗格的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/migrate5.png)

1. 在 [**準備就緒檢查**] 窗格中，當檢查完成時，他們會選取 [卸**離**]。

    ![卸離 Team 專案集合 wizard 中 [準備就緒檢查] 窗格的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/migrate4.png)

1. 成功卸離集合後，他們會選取 [**關閉**] 以完成。

    ![[卸離 Team 專案集合] 嚮導中 [完成] 窗格的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/migrate6.png)

    Team Foundation Server 管理主控台中不再參考集合。

    ![Team Foundation Server 管理主控台的螢幕擷取畫面，其中顯示不會再列出集合。](./media/contoso-migration-tfs-vsts/migrate7.png)

### <a name="generate-a-dacpac"></a>產生 DACPAC

Contoso 管理員會建立要匯入 Azure DevOps Services 的備份或 DACPAC。

- 系統管理員會使用 `sqlpackage.exe` SQL Server Data Tools (SSDT) 中的公用程式來建立 DACPAC。 SQL Server Data Tools 安裝了多個版本 `sqlpackage.exe` ，而且它們位於具有、和等名稱的資料夾底下 `120` `130` `140` 。 務必要使用正確的版本來準備 DACPAC。

- Team Foundation Server 2018 匯入必須使用*140*資料夾或更新版本中的*sqlpackage.exe* 。 若為 `CONTOSOTFS` ，此檔案位於 `C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\Extensions\Microsoft\SQLDB\DAC\140` 。

Contoso 管理員會產生 DACPAC，如下所示：

1. 他們會開啟命令提示字元，並移至該 `sqlpackage.exe` 位置。 若要產生 DACPAC，他們會執行下列命令：

    `SqlPackage.exe /sourceconnectionstring:"Data Source=SQLSERVERNAME\INSTANCENAME;Initial Catalog=Tfs_ContosoDev;Integrated Security=True" /targetFile:C:\TFSMigrator\Tfs_ContosoDev.dacpac /action:extract /p:ExtractAllTableData=true /p:IgnoreUserLoginMappings=true /p:IgnorePermissions=true /p:Storage=Memory`

    ![命令提示字元的螢幕擷取畫面，其中顯示用來產生 DACPAC 的命令。](./media/contoso-migration-tfs-vsts/backup1.png)

    隨即會顯示下列訊息：

    ![命令提示字元的螢幕擷取畫面，顯示資料庫已成功解壓縮並儲存至 DACPAC 檔案的訊息。](./media/contoso-migration-tfs-vsts/backup2.png)

1. 他們會驗證 DACPAC 檔案的屬性。

    ![顯示 DACPAC 檔案屬性以進行驗證的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/backup3.png)

### <a name="upload-the-file-to-storage"></a>將檔案上傳至儲存體

在系統管理員建立 DACPAC 檔案之後，他們將其上傳至 Azure 儲存體帳戶。

1. 他們會下載並安裝 [Azure 儲存體總管](https://azure.microsoft.com/features/storage-explorer)。

    ![「下載儲存體總管免費」的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/backup5.png)

1. 在儲存體總管中，系統管理員會連線到其訂用帳戶，然後搜尋並選取其為 [遷移 (]) 所建立的儲存體帳戶 `contosodevmigration` 。 他們會建立新的 blob 容器 `azuredevopsmigration` 。

    ![儲存體總管中 [建立 Blob 容器] 連結的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/backup6.png)

1. 在 [**上傳**檔案] 窗格的 [ **Blob 類型**] 下拉式清單中，系統管理員針對 DACPAC 檔案上傳指定**區塊 Blob** 。

    ![儲存體總管中 [上傳檔案] 窗格的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/backup7.png)

1. 上傳檔案之後，他們會選取檔案名，然後選取 [**產生 SAS**]。 他們會展開儲存體帳戶底下的 [ **Blob 容器**] 清單，選取包含匯入檔案的容器，然後選取 [**取得共用存取**簽章]。

    ![儲存體總管中「取得共用存取簽章」連結的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/backup8.png)

1. 在 [**共用存取**簽章] 窗格中，他們接受預設設定，然後選取 [**建立**]。 這可以啟用 24 小時的存取。

    ![儲存體總管中 [共用存取簽章] 窗格的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/backup9.png)

1. 他們會複製共用存取簽章 URL，以供 Team Foundation Server 遷移工具使用。

    ![儲存體總管中的共用存取簽章 URL 的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/backup10.png)

> [!NOTE]
> 遷移必須在允許的時間範圍內發生，否則許可權將會過期。 請勿*從 Azure 入口網站產生 SAS*金鑰。 從入口網站產生的金鑰會以帳戶為範圍，且不會使用匯入。

### <a name="fill-in-the-import-settings"></a>填入匯入設定

先前，Contoso 管理員會部分填入匯入規格檔案， *import.js*。 現在，他們必須新增其餘設定。

他們會開啟檔案*上的import.js* ，並完成下欄欄位：

- **位置**：他們會輸入先前產生之 SAS 金鑰的位置。
- **Dacpac**：他們輸入先前上傳至儲存體帳戶的 dacpac 檔案名稱，並務必包含 *.dacpac*副檔名。
- **ImportType**：他們現在輸入**DryRun** 。

![已填入欄位之「import.js開啟」檔案的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/import1.png)

### <a name="perform-a-dry-run-migration"></a>執行試執行的遷移

Contoso 管理員會執行試進行的遷移，以確保一切都如預期般運作。

1. 他們會開啟命令提示字元，然後移至 `TfsMigrator` 位置 (`C:\TFSMigrator`) 。
1. 他們想要確定檔案的格式正確，且 SAS 金鑰運作正常。 他們會執行下列命令來驗證匯入檔案： 

    `TfsMigrator import /importFile:C:\TFSMigrator\import.json /validateonly`

    驗證會傳回錯誤，指出 SAS 金鑰在到期前需要較長的時間。

    ![顯示驗證錯誤之命令提示字元的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/test1.png)

1. 他們會使用 Azure 儲存體總管建立新的 SAS 金鑰，並在到期前設定為7天。

    ![顯示到期日儲存體總管 [共用存取簽章] 窗格的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/test2.png)

1. 他們會更新檔案 `import.json` ，並重新執行命令。 此時，驗證已順利完成。

    `TfsMigrator import /importFile:C:\TFSMigrator\import.json /validateonly`

    ![顯示「驗證已順利完成」訊息之命令提示字元的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/test3.png)

1. 他們會執行下列命令來開始試執行：

    `TfsMigrator import /importFile:C:\TFSMigrator\import.json`

    隨即顯示一則訊息，要求他們確認是否要繼續進行遷移。 請注意，在執行一段時間後，將會維護暫存資料的七天期間。

    ![訊息的螢幕擷取畫面，要求 Contoso 確認他們想要繼續進行遷移。](./media/contoso-migration-tfs-vsts/test4.png)

1. [Azure AD 登入] 視窗隨即開啟。 Contoso 管理員以系統管理員許可權登入 Azure AD。

    ![Visual Studio 中 Azure AD 登入] 視窗的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/test5.png)

    隨即顯示一則訊息，確認已成功啟動匯入。

    ![螢幕擷取畫面，顯示匯入已成功啟動。](./media/contoso-migration-tfs-vsts/test6.png)

1. 大約15分鐘之後，系統管理員會前往網站，並查看下列資訊：

     ![螢幕擷取畫面，顯示正在還原集合。](./media/contoso-migration-tfs-vsts/test7.png)

1. 在遷移完成後，Contoso 開發組長會登入 Azure DevOps Services，以確保試執行正常運作。 經過驗證之後，Azure DevOps Services 服務需要一些詳細資料來確認組織。

    ![Azure DevOps Services 視窗的螢幕擷取畫面，要求來自 Contoso 小組的其他資訊。](./media/contoso-migration-tfs-vsts/test8.png)

    開發人員主管可以看到專案已成功遷移。 接近頁面頂端的通知會警告您即將在15天內刪除試執行帳戶。

    ![Azure DevOps Services 介面的螢幕擷取畫面，並顯示一則訊息，指出即將在15天內刪除試執行帳戶。](./media/contoso-migration-tfs-vsts/test9.png)

1. 開發組長會開啟其中一個專案，然後選取 [指派給我的**工作專案**]  >  ** **。 此頁面會驗證工作專案資料是否已成功遷移，以及身分識別。

    ![Azure DevOps Services [工作專案] 窗格的螢幕擷取畫面，其中顯示所有已遷移的專案。](./media/contoso-migration-tfs-vsts/test10.png)

1. 為了確認原始程式碼和歷程記錄已遷移，開發組長會檢查其他專案和程式碼。

    ![Azure DevOps Services [歷程記錄] 窗格的螢幕擷取畫面，其中顯示已遷移所有程式碼和歷程記錄。](./media/contoso-migration-tfs-vsts/test11.png)

### <a name="run-the-production-migration"></a>執行生產環境移轉

試執行完成後，Contoso 管理員會繼續進行生產階段的遷移。 他們會刪除試執行、更新匯入設定，然後重新執行匯入。

1. 在 Azure DevOps Services 入口網站中，他們會刪除試執行組織。
1. 他們會更新檔案 `import.json` ，以將**ImportType**設定為**ProductionRun**。

    ![Azure DevOps Services 入口網站的螢幕擷取畫面，其中 [ImportType] 欄位設定為 [ProductionRun]。](./media/contoso-migration-tfs-vsts/full1.png)

1. 在試執行時，他們會執行下列命令來開始進行遷移：

    `TfsMigrator import /importFile:C:\TFSMigrator\import.json`.

    隨即顯示一則訊息，要求系統管理員確認該遷移。 它會警告資料可以保留在安全的位置做為臨時區域，最多七天。

    ![Azure DevOps Services 訊息的螢幕擷取畫面，警告表示已遷移的資料最多可保留七天。](./media/contoso-migration-tfs-vsts/full2.png)

1. 在 [Azure AD 登入] 視窗中，他們會指定 Contoso 管理員登入。

    ![Visual Studio 中 Azure AD 登入] 視窗的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/full3.png)

    隨即顯示一則訊息，表示匯入已成功啟動。

    ![已成功啟動匯入之 Azure DevOps Services 訊息的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/full4.png)

1. 大約15分鐘之後，系統管理員會前往網站，並查看下列資訊：

    ![螢幕擷取畫面，顯示正在將資料複製到雲端。](./media/contoso-migration-tfs-vsts/full5.png)

1. 在遷移完成後，開發組長會登入 Azure DevOps Services，以確保該遷移運作正常。 登入之後，開發組長可以看到專案已遷移。

    ![螢幕擷取畫面：顯示專案已遷移。](./media/contoso-migration-tfs-vsts/full6.png)

1. 開發組長會開啟其中一個專案，並選取 [指派給我的**工作專案**]  >  ** **。 這會顯示工作專案資料已遷移，以及身分識別。

    ![螢幕擷取畫面：顯示已遷移工作專案資料和身分識別。](./media/contoso-migration-tfs-vsts/full7.png)

1. 開發組長會進行檢查，確認是否已遷移其他工作專案資料。

    ![列出要確認之其他工作專案資料的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/full8.png)

1. 為了確認原始程式碼和歷程記錄已遷移，開發組長會檢查其他專案和程式碼。

    ![列出要確認之其他專案和原始程式碼遷移的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/full9.png)

### <a name="move-source-control-from-tfvc-to-git"></a>將原始檔控制從 TFVC 移至 Git

現在遷移完成後，Contoso 管理員會想要將原始程式碼管理從 TFVC 移至 Git。 系統管理員必須匯入目前在其 Azure DevOps Services 組織中的原始程式碼，做為相同組織中的 Git 存放庫。

1. 在 Azure DevOps Services 入口網站中，他們會開啟其中一個 TFVC 存放庫， `$/PolicyConnect` 並加以審查。

    ![Azure DevOps Services 入口網站中 "$/PolicyConnect" 存放庫的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/git1.png)

1. 在 [來源 **$/PolicyConnect** ] 下拉式清單中，選取 [匯**入存放庫**]。

    ![Azure DevOps Services 入口網站中 [匯入存放庫] 連結的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/git2.png)

1. 在 [**來源類型**] 下拉式清單中，選取 [ **TFVC**]。 在 [**路徑**] 方塊中，他們會指定存放庫的路徑，然後選取 [匯**入**]。 他們決定保留清除 [**遷移歷程記錄**] 核取方塊。

    ![[從 TFVC 匯入] 窗格的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/git3.png)

    > [!NOTE]
    > 因為 TFVC 和 Git 會以不同的方式儲存版本控制資訊，所以我們建議 Contoso*不要*遷移其存放庫歷程記錄。 這是 Microsoft 將 Windows 和其他產品從集中式版本控制遷移至 Git 時所採取的方法。

1. 匯入完成之後，系統管理員會檢查程式碼。

    ![確認匯入成功的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/git4.png)

1. 它們會為第二個存放庫重複此程式， `$/smarthotelcontainer` 。

    ![第二個存放庫的 [從 TFVC 匯入] 窗格的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/git5.png)

1. 在開發組長審查來源之後，他們同意 Azure DevOps Services 的轉移作業完成。 Azure DevOps Services 現在會成為涉及遷移之小組內所有開發的來源。

    ![螢幕擷取畫面，顯示 Azure DevOps Services 的遷移已完成。](./media/contoso-migration-tfs-vsts/git6.png)

**需要其他協助？**

如需詳細資訊，請參閱[將存放庫從 TFVC 匯入至 Git](https://docs.microsoft.com/azure/devops/repos/git/import-from-TFVC?view=vsts)。

## <a name="clean-up-after-migration"></a>移轉之後進行清除

現在遷移完成後，Contoso 小組必須執行下列動作：

- 檢閱[匯入之後](https://docs.microsoft.com/azure/devops/articles/migration-post-import?view=vsts)一文，以了解其他匯入活動的相關資訊。
- 刪除 TFVC 存放庫，或將其置於唯讀模式。 不能使用程式碼基底，但是可以參考其歷程記錄。

## <a name="post-migration-training"></a>移轉後訓練

Contoso 小組必須為相關小組成員提供 Azure DevOps Services 和 Git 訓練。
