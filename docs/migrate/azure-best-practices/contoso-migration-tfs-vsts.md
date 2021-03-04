---
title: 將 Team Foundation Server 部署重構到 Azure DevOps Services
description: 使用適用于 Azure 的雲端採用架構，瞭解如何將內部部署的 Team Foundation Server 部署遷移至 Azure 中的 Azure DevOps Services，以進行重構。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: internal
ms.openlocfilehash: 4543821d183169ff4932f62118915c71db2fe6fc
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102113804"
---
<!-- cSpell:ignore contosodev contosodevmigration contosomigration onmicrosoft visualstudio sourceconnectionstring smarthotelcontainer identitymaplog CONTOSOTFS DACPAC SQLDB SQLSERVERNAME INSTANCENAME sqlpackage SSDT azuredevopsmigration validateonly ImportType -->

# <a name="refactor-a-team-foundation-server-deployment-to-azure-devops-services"></a>將 Team Foundation Server 部署重構到 Azure DevOps Services

本文說明虛構公司 Contoso 如何藉由將其遷移至 Azure 中的 Azure DevOps Services，來重構其內部部署 Visual Studio Team Foundation Server 部署。 Contoso 開發小組已使用 Team Foundation Server 進行小組共同作業，並將原始檔控制用於過去五年。 現在，小組想要移至雲端式解決方案以進行開發和測試工作，以及進行原始檔控制。 當 Contoso 團隊移至 Azure DevOps 模型，並開發新的雲端原生應用程式時，Azure DevOps Services 將扮演角色。

## <a name="business-drivers"></a>商業動機

Contoso IT 領導小組與商務合作夥伴密切合作，以找出未來的目標。 夥伴並不太在意開發工具和技術，但團隊已捕捉到下列幾點：

- **軟體：** 無論核心業務為何，所有公司現在都是軟體公司，包括 Contoso。 商務領導階層有興趣瞭解它如何協助引導公司提供新的工作實務給使用者，以及新的客戶體驗。
- **效率：** Contoso 需要簡化其流程，並移除開發人員和使用者不必要的程式。 這樣做可讓公司更有效率地提供客戶的需求。 企業需要 IT 快速移動，而不浪費時間或金錢。
- **靈活性：** 為了實現全球經濟的成功，Contoso IT 必須能夠更快回應企業的需求。 它必須能夠更快速地回應 marketplace 中的變更。 它不得以或成為企業封鎖程式。

## <a name="migration-goals"></a>移轉目標

Contoso 雲端小組已針對遷移至 Azure DevOps Services 的下列目標：

- 小組需要可將其資料移轉至雲端的工具。 有幾項程序必須手動執行。
- 他們必須移轉去年的工作項目資料和歷程記錄。
- 小組不想設定新的使用者名稱和密碼。 目前的所有系統指派都必須保留。
- 小組想要從 Team Foundation 版本控制 (TFVC) 移轉至 Git，以進行原始檔控制。
- 轉換至 Git 將會是僅匯入最新版本原始程式碼的 tip 遷移。 當程式碼基底轉移時，當所有工作都將停止時，就會在停機期間發生轉換。 小組瞭解在移動之後，只有目前的主要分支歷程記錄可供使用。
- 小組關心這項變更，而且想要在進行完整移動之前測試它。 即使在移至 Azure DevOps Services 之後，小組也想要保留對 Team Foundation Server 的存取權。
- 小組有多個集合，而若要進一步瞭解此程式，它想要從只有幾個專案的專案開始。
- 小組瞭解 Team Foundation Server 集合是與 Azure DevOps Services 組織之間的一對一關聯性，因此會有多個 Url。 但這符合其目前的程式碼基底和專案分離模型。

## <a name="proposed-architecture"></a>建議的架構

- Contoso 會將其 Team Foundation Server 專案移至雲端，而不會再裝載其專案或內部部署的原始檔控制。
- Team Foundation Server 將會遷移至 Azure DevOps Services。
- 目前，Contoso 有一個名為的 Team Foundation Server 集合， `ContosoDev` 其會遷移至名為的 Azure DevOps Services 組織 `contosodevmigration.visualstudio.com` 。
- 去年的專案、工作專案、bug 和反覆運算將會遷移至 Azure DevOps Services。
- Contoso 會使用其 Azure Active Directory (Azure AD) 實例，其會在遷移規劃開始時，于 [部署其 azure 基礎結構](./contoso-migration-infrastructure.md) 時進行設定。

![建議的架構圖表。](./media/contoso-migration-tfs-vsts/architecture.png)

## <a name="migration-process"></a>移轉程序

Contoso 會按照下列方式完成移轉程序：

1. 需要進行大量的準備工作。 首先，Contoso 必須將其 Team Foundation Server 的執行升級至支援的層級。 Contoso 目前執行的是 Team Foundation Server 2017 Update 3，但若要使用資料庫移轉，則必須使用最新的更新來執行支援的2018版本。
1. Contoso 在升級之後，會執行 Team Foundation Server 遷移工具並驗證其集合。
1. Contoso 會建立一組準備檔，然後執行遷移試執行以進行測試。
1. 然後，Contoso 會執行另一項移轉，這次將是包含工作項目、Bug、衝刺和程式碼的完整移轉。
1. 遷移之後，Contoso 會將其程式碼從 TFVC 移至 Git。

![Contoso 遷移程式的圖表。](./media/contoso-migration-tfs-vsts/migration-process.png)

## <a name="prerequisites"></a>必要條件

若要執行此案例，Contoso 必須符合下列必要條件：

| 需求 | 詳細資料 |
| --- | --- |
| **Azure 訂用帳戶** | Contoso 已在本系列稍早的文章中建立訂用帳戶。 如果您沒有 Azure 訂用帳戶，請建立[免費帳戶](https://azure.microsoft.com/free/)。 <br><br> 如果您建立免費帳戶，您就是訂用帳戶的管理員，並可執行所有動作。 <br><br> 如果您使用現有訂用帳戶，而且您不是系統管理員，則需要與系統管理員合作，讓其指派擁有者或參與者權限給您。 <br><br> 如果您需要更細微的許可權，請參閱 [使用 azure 角色型存取控制管理 Site Recovery 存取 (AZURE RBAC) ](/azure/site-recovery/site-recovery-role-based-linked-access-control)。 |
| **Azure 基礎結構** | Contoso 會如 Azure 基礎結構中所述，設定其 Azure 基礎結構 [以進行遷移](./contoso-migration-infrastructure.md)。 |
| **內部部署 Team Foundation Server 實例** | 內部部署實例需要執行 Team Foundation Server 2018 upgrade 2 或升級為此程式的一部分。 |

## <a name="scenario-steps"></a>案例步驟

以下是 Contoso 完成移轉的方式：

> [!div class="checklist"]
>
> - **步驟1：建立 Azure 儲存體帳戶**。 在移轉程序期間將使用此儲存體帳戶。
> - **步驟2：升級 Team Foundation Server**。 Contoso 會將其部署升級至 Team Foundation Server 2018 upgrade 2。
> - **步驟3：驗證 Team Foundation Server 集合**。 Contoso 將會驗證 Team Foundation Server 集合，以準備進行遷移。
> - **步驟4：建立遷移** 檔。 Contoso 會使用 Team Foundation Server 遷移工具來建立遷移檔。

## <a name="step-1-create-an-azure-storage-account"></a>步驟1：建立 Azure 儲存體帳戶

1. 在 Azure 入口網站中，Contoso 管理員會建立儲存體帳戶 (`contosodevmigration`) 。
1. 他們將帳戶放在次要區域中，以用於容錯移轉 (`Central US`) 。 他們使用具有本機備援儲存體的一般用途標準帳戶。

    ![[建立儲存體帳戶] 窗格的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/storage1.png)

**需要其他協助？**

- [Azure 儲存體簡介](/azure/storage/common/storage-introduction)。
- [建立儲存體帳戶](/azure/storage/common/storage-account-create)。

<!-- docutune:casing "Server Configuration Wizard" "Configure Features Wizard" "Detach Team Project Collection Wizard" -->

## <a name="step-2-upgrade-team-foundation-server"></a>步驟2：升級 Team Foundation Server

Contoso 管理員將 Team Foundation Server 實例升級為 Team Foundation Server 2018 Update 2。 開始之前，他們會：

- 下載 [Team Foundation Server 2018 Update 2](https://visualstudio.microsoft.com/downloads/)。
- 確認 [硬體需求](/azure/devops/server/requirements)。
- 閱讀 [版本](/visualstudio/releasenotes/tfs2018-relnotes) 資訊與 [升級](/azure/devops/server/upgrade/get-started)的問題。

他們會依照下列方式進行升級：

1. 若要開始，系統管理員會備份其 Team Foundation Server 實例，此實例是在 VMware 虛擬機器上執行 (VM) ，而且會採用 VMware 快照集。

    ![升級 Team Foundation Server 的 [開始使用] 窗格螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/upgrade1.png)

1. Team Foundation Server 安裝程式隨即啟動，並選擇安裝位置。 安裝程式需要存取網際網路。

    ![Visual Studio 中的 Team Foundation Server 安裝網站螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/upgrade2.png)

1. 安裝完成後，會啟動 [伺服器組態精靈]。

    ![Team Foundation Server 2018 Update 2 configuration wizard 的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/upgrade3.png)

1. 驗證之後，伺服器設定向導會完成升級。

     ![Team Foundation Server configuration wizard 升級進度窗格的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/upgrade4.png)

1. 系統管理員會藉由檢查項目、工作專案和程式碼來確認 Team Foundation Server 安裝。

     ![用於驗證 Team Foundation Server 安裝的 [產品待處理專案] 窗格螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/upgrade5.png)

> [!NOTE]
> 部分 Team Foundation Server 升級需要在升級完成後執行 [設定功能嚮導]。 [深入了解](/previous-versions/azure/devops/reference/upgrade/configure-features-after-upgrade?view=azure-devops&preserve-view=true&viewFallbackFrom=vsts)。

**需要其他協助？**

瞭解如何 [升級 Team Foundation Server](/azure/devops/server/upgrade/get-started)。

## <a name="step-3-validate-the-team-foundation-server-collection"></a>步驟3：驗證 Team Foundation Server 集合

Contoso 管理員會對集合資料庫執行 Team Foundation Server 遷移工具 `contosodev` ，以在遷移前進行驗證。

1. 他們會下載並解壓縮 [Team Foundation Server 遷移工具](https://www.microsoft.com/download/details.aspx?id=54274)。 請務必下載正在執行之 Team Foundation Server 更新的版本。 他們可在管理主控台中檢查版本。

    ![用於驗證產品版本的 [Team Foundation Server] 窗格螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/collection1.png)

1. 他們會指定專案集合的 URL，以執行此工具來執行驗證，如下列命令所示：

    `TfsMigrator validate /collection:http://contosotfs:8080/tfs/ContosoDev`

    此工具會顯示錯誤。

    ![Team Foundation Server 遷移工具中的驗證錯誤螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/collection2.png)

1. 它們會在工具位置之前的資料夾中尋找記錄檔 `Logs` 。 每個主要驗證都會產生一個記錄檔。 `TfsMigration.log` 包含主要資訊。

    ![Team Foundation Server 中記錄檔位置的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/collection3.png)

1. 他們會找到與身分識別相關的這個專案。

    ![顯示身分識別驗證期間發生之錯誤的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/collection4.png)

1. 它們會 `TfsMigrator validate /help` 在命令列中執行，並看到命令 `/tenantDomainName` 似乎是驗證身分識別所需的命令。

     ![螢幕擷取畫面，顯示驗證身分識別所需的命令。](./media/contoso-migration-tfs-vsts/collection5.png)

1. 他們會再次執行驗證命令，並包含此值和其 Azure AD 名稱 `TfsMigrator validate /collection:http://contosotfs:8080/tfs/ContosoDev /tenantDomainName:contosomigration.onmicrosoft.com` 。

    ![命令提示字元的螢幕擷取畫面，其中顯示正確的命令。](./media/contoso-migration-tfs-vsts/collection7.png)

1. 在開啟的 Azure AD 登入視窗中，他們會輸入全域管理員使用者的認證。

     ![[Azure AD 登入] 視窗的螢幕擷取畫面，其中包含系統管理員認證。](./media/contoso-migration-tfs-vsts/collection8.png)

1. 驗證通過，並由工具確認。

    ![螢幕擷取畫面，顯示已通過驗證。](./media/contoso-migration-tfs-vsts/collection9.png)

## <a name="step-4-build-the-migration-files"></a>步驟4：建立遷移檔

完成驗證之後，Contoso 管理員就可以使用 Team Foundation Server 遷移工具來建立遷移檔。

1. 他們會執行工具中的準備步驟。

    `TfsMigrator prepare /collection:http://contosotfs:8080/tfs/ContosoDev /tenantDomainName:contosomigration.onmicrosoft.com /accountRegion:cus`

     ![Team Foundation Server 遷移工具中 [準備] 命令的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/prep1.png)

    準備步驟會執行下列動作：
    - 掃描集合以找出所有使用者的清單，然後在 [識別對應記錄檔 (`IdentityMapLog.csv`) 中填入。
    - 準備與 Azure AD 的連線，以找出每個身分識別的相符項。
    - Contoso 已部署 Azure AD 並使用 Azure AD Connect 同步處理，因此「準備」命令應該可以找到相符的身分識別，並將其 **標示為作用中。**

1. Azure AD 登入畫面隨即出現，而系統管理員則會輸入全域管理員的認證。

    ![Azure AD 登入畫面的螢幕擷取畫面，其中包含在 [使用者] 文字方塊中輸入的系統管理員認證。](./media/contoso-migration-tfs-vsts/prep2.png)

1. 準備完成後，此工具會報告匯入檔案是否已成功產生。

    ![[遷移] 工具的螢幕擷取畫面，其中顯示集合驗證成功。](./media/contoso-migration-tfs-vsts/prep3.png)

1. 系統管理員現在可以看到檔案和檔案都是 `IdentityMapLog.csv` `import.json` 在新的資料夾中建立的。

    ![準備](./media/contoso-migration-tfs-vsts/prep4.png)

1. 檔案會 `import.json` 提供匯入設定。 它包含所需的組織名稱和儲存體帳戶詳細資料等資訊。 大部分的欄位會自動填入。 某些欄位需要使用者輸入。 系統管理員會開啟檔案，並新增要建立的 Azure DevOps Services 組織名稱 `contosodevmigration` 。 使用此名稱時，Contoso Azure DevOps Services URL 將會是 `contosodevmigration.visualstudio.com` 。

    ![顯示 Azure DevOps Services 組織名稱的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/prep5.png)

    > [!NOTE]
    > 在開始遷移之前，必須先建立組織。 您可以在完成遷移之後變更它。

1. 系統管理員會檢查身分識別記錄對應檔，其中顯示將在匯入期間帶入 Azure DevOps Services 的帳戶。

    - 作用中的身分識別是指在匯入後會在 Azure DevOps Services 中成為使用者的身分識別。
    - 在 Azure DevOps Services 中，這些身分識別會在遷移後以組織中的使用者身分進行授權和顯示。
    - 在檔案的 [預期的匯 **入狀態**] 資料行中，這些身分識別會標示為「**作用中」** 。

    ![身分識別記錄對應檔的螢幕擷取畫面，其中顯示要帶入 Azure DevOps Services 的帳戶。](./media/contoso-migration-tfs-vsts/prep6.png)

## <a name="step-5-migrate-to-azure-devops-services"></a>步驟 5：遷移至 Azure DevOps Services

準備完成後，Contoso 管理員可以專注于遷移。 執行遷移之後，他們會從使用 TFVC 切換至 Git 進行版本控制。

在開始之前，系統管理員會排定開發小組的停機時間，讓他們可以規劃讓集合離線進行遷移。

以下是他們將遵循的遷移程式：

1. **中斷集合的連結。** 當集合已附加且在線上時，集合的身分識別資料會位於 Team Foundation Server 實例的設定資料庫中。

    當集合從 Team Foundation Server 實例卸離時，就會建立該身分識別資料的複本，然後再與集合一起封裝以進行傳輸。 如果沒有這項資料，就無法執行匯入的身分識別部分。

    建議您在匯入完成之前保持卸離，因為無法匯入匯入期間發生的變更。

1. **產生備份。** 下一步是產生可匯入 Azure DevOps Services 的備份。 資料層應用程式元件封裝 (DACPAC) 是一種 SQL Server 功能，可讓資料庫變更封裝成單一檔案，然後部署到其他 SQL 實例。

    備份也可以直接還原至 Azure DevOps Services，並作為將集合資料放入雲端的封裝方法。 Contoso 會使用 `sqlpackage.exe` 工具來產生 DACPAC。 此工具隨附於 SQL Server Data Tools 中。

1. **上傳至儲存體。** 建立 DACPAC 之後，系統管理員會將它上傳至 Azure 儲存體。 在上傳後，他們會取得 (SAS) 的共用存取簽章，以允許 Team Foundation Server 遷移工具存取儲存體。
1. **填寫匯入。** 接著，Contoso 可以在匯入檔案中填寫遺漏的欄位，包括 DACPAC 設定。 為了確保所有專案在完整遷移之前都能正常運作，系統管理員會指定他們想要執行 *試* 執行匯入。
1. **執行試執行匯入。** 試執行匯入有助於測試集合遷移。 試執行的存留期有限，因此在執行生產環境遷移之前會先將其刪除。 試執行會在一段時間後自動刪除。 通知 Contoso 將刪除即將刪除的成功電子郵件中的通知，會包含在匯入完成之後傳送的成功電子郵件中。 小組會據以記下和計畫。
1. **完成生產環境移轉。** 完成執行的遷移作業之後，Contoso 管理員會更新檔案，然後再次執行匯入，以進行最後的遷移 `import.json` 。

<!-- docutune:casing "Team Foundation Server Administration Console" -->

### <a name="detach-the-collection"></a>中斷集合的連結

在卸離集合之前，Contoso 管理員會取得本機 SQL Server 實例備份和 Team Foundation Server 實例的 VMware 快照集。

1. 在 [Team Foundation Server 管理主控台] 中，系統管理員選取要卸離的集合， **ContosoDev**。

    ![基礎伺服器管理主控台之 [Team 專案集合] 區段的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/migrate1.png)

1. 他們會選取 [ **一般** ] 索引標籤，然後選取 [卸 **離集合**]。

    ![基礎伺服器管理主控台中 [卸離集合] 連結的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/migrate2.png)

1. 在 [卸 **離 Team 專案集合** 嚮導] 的 [ **服務訊息** ] 窗格中，系統管理員會為可能嘗試連接到集合中專案的使用者提供訊息。

    ![[卸離 Team 專案集合嚮導] 中 [服務訊息] 窗格的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/migrate3.png)

1. 在 [卸 **離進度** ] 窗格中，他們會監視進度。 當進程完成時，他們會選取 **[下一步]**。

    ![卸離 Team 專案集合 wizard 中 [卸離進度] 窗格的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/migrate5.png)

1. 在 [ **準備就緒檢查** ] 窗格中，當檢查完成時，他們會選取 [卸 **離**]。

    ![[卸離 Team 專案集合嚮導] 中 [準備檢查] 窗格的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/migrate4.png)

1. 成功卸離集合後，他們會選取 [ **關閉** ] 以完成。

    ![[卸離 Team 專案集合嚮導] 中 [完成] 窗格的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/migrate6.png)

    Team Foundation Server 管理主控台中不再參考該集合。

    ![[Team Foundation Server 管理主控台] 的螢幕擷取畫面，其中顯示已不再列出該集合。](./media/contoso-migration-tfs-vsts/migrate7.png)

### <a name="generate-a-dacpac"></a>產生 DACPAC

Contoso 管理員會建立備份或 DACPAC，以匯入至 Azure DevOps Services。

- 系統管理員會使用 `sqlpackage.exe` SQL Server Data Tools 中的公用程式 (SSDT) 來建立 DACPAC。 有多個版本 `sqlpackage.exe` 與 SQL Server Data Tools 一起安裝，且位於具有、和等名稱的資料夾 `120` 下 `130` `140` 。 務必要使用正確的版本來準備 DACPAC。

- Team Foundation Server 2018 imports 需要 `sqlpackage.exe` 從 *140* 資料夾或更高版本使用。 若為 `CONTOSOTFS` ，這個檔案位於 `C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\Extensions\Microsoft\SQLDB\DAC\140` 。

Contoso 管理員會產生 DACPAC，如下所示：

1. 他們會開啟命令提示字元，並移至該 `sqlpackage.exe` 位置。 若要產生 DACPAC，請執行下列命令：

    `SqlPackage.exe /sourceconnectionstring:"Data Source=SQLSERVERNAME\INSTANCENAME;Initial Catalog=Tfs_ContosoDev;Integrated Security=True" /targetFile:C:\TFSMigrator\Tfs_ContosoDev.dacpac /action:extract /p:ExtractAllTableData=true /p:IgnoreUserLoginMappings=true /p:IgnorePermissions=true /p:Storage=Memory`

    ![命令提示字元的螢幕擷取畫面，其中顯示用來產生 DACPAC 的命令。](./media/contoso-migration-tfs-vsts/backup1.png)

    隨即會顯示下列訊息：

    ![命令提示字元的螢幕擷取畫面，其中顯示資料庫已成功解壓縮並儲存至 DACPAC 檔案的訊息。](./media/contoso-migration-tfs-vsts/backup2.png)

1. 他們會驗證 DACPAC 檔案的屬性。

    ![顯示 DACPAC 檔案屬性以進行驗證的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/backup3.png)

### <a name="upload-the-file-to-storage"></a>將檔案上傳至儲存體

當系統管理員建立 DACPAC 檔案之後，他們會將它上傳到 Azure 儲存體帳戶。

1. 他們會下載並安裝 [Azure 儲存體總管](https://azure.microsoft.com/features/storage-explorer/)。

    ![[下載 Storage Explorer free] 的螢幕擷取畫面。 * *](./media/contoso-migration-tfs-vsts/backup5.png)

1. 在 [儲存體 Explorer] 中，系統管理員會連線至其訂用帳戶，然後搜尋並選取他們為遷移 () 所建立的儲存體帳戶 `contosodevmigration` 。 他們會建立新的 blob 容器 `azuredevopsmigration` 。

    ![儲存體 Explorer 中 [建立 Blob 容器] 連結的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/backup6.png)

1. 在 [ **上傳** 檔案] 窗格的 [ **Blob 類型** ] 下拉式清單中，系統管理員會針對 DACPAC 檔案上傳指定 **區塊 Blob** 。

    ![儲存體 Explorer 中 [上傳檔案] 窗格的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/backup7.png)

1. 上傳檔案之後，他們會選取檔案名，然後選取 [ **產生 SAS**]。 他們會展開儲存體帳戶下的 **Blob 容器** 清單、選取具有匯入檔案的容器，然後選取 [ **取得共用存取** 簽章]。

    ![儲存體 Explorer 中 [取得共用存取簽章] 連結的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/backup8.png)

1. 在 [ **共用存取** 簽章] 窗格上，他們接受預設設定，然後選取 [ **建立**]。 這可以啟用 24 小時的存取。

    ![儲存體 Explorer 中 [共用存取簽章] 窗格的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/backup9.png)

1. 他們會複製共用存取簽章 URL，讓 Team Foundation Server 遷移工具可以使用它。

    ![儲存體 Explorer 中共用存取簽章 URL 的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/backup10.png)

> [!NOTE]
> 遷移必須在允許的時間範圍內發生，否則許可權將會過期。 請勿從 Azure 入口 *網站產生 SAS* 金鑰。 從入口網站產生的金鑰會以帳戶為範圍，且不會使用匯入。

### <a name="fill-in-the-import-settings"></a>填入匯入設定

稍早，Contoso 管理員會部分填滿匯入規格檔案 `import.json` 。 現在，他們必須新增其餘設定。

他們會開啟檔案 `import.json` ，並完成下欄欄位：

- **位置：** 他們會輸入先前產生的 SAS 金鑰位置。
- **DACPAC：** 他們會輸入先前上傳至儲存體帳戶的 DACPAC 檔案名，並確定包含 *.dacpac* 副檔名。
- **ImportType：** 他們現在就輸入 **DryRun** 。

![填入欄位之 [import.js開啟] 檔案的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/import1.png)

### <a name="perform-a-dry-run-migration"></a>執行試執行遷移

Contoso 管理員會執行試執行的遷移，以確定一切都如預期般運作。

1. 他們會開啟命令提示字元，然後移至 `TfsMigrator` () 的位置 `C:\TFSMigrator` 。
1. 他們想要確定檔案的格式正確，而且 SAS 金鑰可以正常運作。 他們會執行下列命令來驗證匯入檔案：

    `TfsMigrator import /importFile:C:\TFSMigrator\import.json /validateonly`

    驗證會傳回錯誤，指出 SAS 金鑰需要較長的時間才會過期。

    ![顯示驗證錯誤之命令提示字元的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/test1.png)

1. 他們使用 Azure 儲存體 Explorer 來建立新的 SAS 金鑰，其期限設定為七天之前。

    ![顯示到期日的 [儲存體 Explorer * * 共用存取簽章] 窗格螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/test2.png)

1. 他們會更新檔案 `import.json` ，然後重新執行命令。 這次驗證已順利完成。

    `TfsMigrator import /importFile:C:\TFSMigrator\import.json /validateonly`

    ![命令提示字元的螢幕擷取畫面，顯示 [驗證已順利完成] 訊息。](./media/contoso-migration-tfs-vsts/test3.png)

1. 他們會執行下列命令來開始試執行：

    `TfsMigrator import /importFile:C:\TFSMigrator\import.json`

    系統會顯示一則訊息，要求他們確認是否要繼續進行遷移。 請注意在執行期間，將保留暫存資料的七天週期。

    ![訊息的螢幕擷取畫面，要求 Contoso 確認他們想要繼續進行遷移。](./media/contoso-migration-tfs-vsts/test4.png)

1. [Azure AD 登入] 視窗隨即開啟。 Contoso 管理員以系統管理員許可權登入 Azure AD。

    ![Visual Studio 中 Azure AD 登入視窗的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/test5.png)

    這時會顯示一則訊息，確認已成功啟動匯入。

    ![螢幕擷取畫面，顯示匯入已順利啟動。](./media/contoso-migration-tfs-vsts/test6.png)

1. 大約15分鐘之後，系統管理員會移至網站，並查看下列資訊：

     ![螢幕擷取畫面，顯示正在還原集合。](./media/contoso-migration-tfs-vsts/test7.png)

1. 在遷移完成後，Contoso 開發組長會登入 Azure DevOps Services，以確保試執行正常運作。 經過驗證之後，Azure DevOps Services 服務需要一些詳細資料來確認組織。

    ![Azure DevOps Services 視窗的螢幕擷取畫面，其中會要求來自 Contoso 團隊的其他資訊。](./media/contoso-migration-tfs-vsts/test8.png)

    開發主管可以看到專案已成功遷移。 頁面頂端附近的通知會警告您將在15天內刪除試執行帳戶。

    ![Azure DevOps Services 介面的螢幕擷取畫面，並顯示一則訊息，指出即將在15天內刪除試執行帳戶。](./media/contoso-migration-tfs-vsts/test9.png)

1. 開發組長會開啟其中一個專案，然後選取 [指派給我的 **工作專案**]  >  ****。 此頁面會確認已成功遷移工作專案資料和身分識別。

    ![[Azure DevOps Services] [工作專案] 窗格的螢幕擷取畫面，其中顯示所有已遷移的專案。](./media/contoso-migration-tfs-vsts/test10.png)

1. 為了確認已遷移原始程式碼和記錄，開發組長會檢查其他專案和程式碼。

    ![[Azure DevOps Services] [歷程記錄] 窗格的螢幕擷取畫面，顯示已遷移所有程式碼和歷程記錄。](./media/contoso-migration-tfs-vsts/test11.png)

### <a name="run-the-production-migration"></a>執行生產環境移轉

試執行完成後，Contoso 管理員會移至生產階段的遷移。 他們會刪除試執行、更新匯入設定，然後重新執行匯入。

1. 在 Azure DevOps Services 入口網站中，他們會刪除試執行的組織。
1. 他們會更新檔案 `import.json` ，將 **ImportType** 設定為 **ProductionRun**。

    ![Azure DevOps Services 入口網站的螢幕擷取畫面，其中 [ImportType] 欄位設定為 [ProductionRun]。](./media/contoso-migration-tfs-vsts/full1.png)

1. 在試執行時，它們會藉由執行下列命令來開始進行遷移：

    `TfsMigrator import /importFile:C:\TFSMigrator\import.json`.

    系統會顯示一則訊息，要求系統管理員確認遷移。 它會警告資料可保存在安全的位置做為臨時區域，最多七天。

    ![Azure DevOps Services 訊息的螢幕擷取畫面，警告指出遷移的資料最多可以保留七天。](./media/contoso-migration-tfs-vsts/full2.png)

1. 在 Azure AD 登入視窗中，他們會指定 Contoso 管理員登入。

    ![Visual Studio 中 Azure AD 登入畫面的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/full3.png)

    隨即顯示一則訊息，表示匯入已順利啟動。

    ![已成功啟動匯入之 Azure DevOps Services 訊息的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/full4.png)

1. 大約15分鐘之後，系統管理員會移至網站，並查看下列資訊：

    ![螢幕擷取畫面，顯示資料正在複製到雲端。](./media/contoso-migration-tfs-vsts/full5.png)

1. 在遷移完成後，開發組長會登入 Azure DevOps Services，以確保遷移正常運作。 登入之後，開發主管可以看到專案已遷移。

    ![螢幕擷取畫面，顯示已遷移專案。](./media/contoso-migration-tfs-vsts/full6.png)

1. 開發組長會開啟其中一個專案，並選取指派給我的 **工作專案**  >  ****。 這會顯示工作專案資料已遷移，以及身分識別。

    ![螢幕擷取畫面，顯示已遷移工作專案資料和身分識別。](./media/contoso-migration-tfs-vsts/full7.png)

1. 開發組長會檢查以確認其他工作專案資料已遷移。

    ![列出要確認的其他工作專案資料的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/full8.png)

1. 為了確認已遷移原始程式碼和記錄，開發組長會檢查其他專案和程式碼。

    ![列出要確認的其他專案和原始碼遷移的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/full9.png)

### <a name="move-source-control-from-tfvc-to-git"></a>將原始檔控制從 TFVC 移至 Git

現在完成遷移後，Contoso 管理員想要將原始程式碼管理從 TFVC 移至 Git。 系統管理員必須匯入目前在其 Azure DevOps Services 組織中的原始程式碼，作為相同組織中的 Git 存放庫。

1. 在 Azure DevOps Services 入口網站中，他們會開啟其中一個 TFVC 存放庫， `$/PolicyConnect` 然後進行審核。

    ![Azure DevOps Services 入口網站中的 * * $/PolicyConnect * * 存放庫的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/git1.png)

1. 在 [來源 **$/PolicyConnect** ] 下拉式清單中，選取 [匯 **入存放庫**]。

    ![Azure DevOps Services 入口網站中 [匯入存放庫] 連結的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/git2.png)

1. 在 [ **來源類型** ] 下拉式清單中，選取 [ **TFVC**]。 在 [ **路徑** ] 方塊中，他們會指定存放庫的路徑，然後選取 [匯 **入**]。 他們決定將 [ **遷移歷程記錄** ] 核取方塊保留為已清除。

    ![[從 TFVC 匯入] 窗格的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/git3.png)

    > [!NOTE]
    > 由於 TFVC 和 Git 儲存版本控制資訊的方式不同，因此我們建議 Contoso *不要* 遷移其存放庫歷程記錄。 當我們將 Windows 和其他產品從集中式版本控制遷移至 Git 時，這就是 Microsoft 所採取的方法。

1. 匯入完成後，系統管理員會檢查程式碼。

    ![確認匯入成功的螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/git4.png)

1. 他們會對第二個存放庫重複此程式 `$/smarthotelcontainer` 。

    ![第二個存放庫的 [從 TFVC 匯入] 窗格螢幕擷取畫面。](./media/contoso-migration-tfs-vsts/git5.png)

1. 在開發組長審核來源之後，他們即表示已完成遷移至 Azure DevOps Services。 Azure DevOps Services 現在會成為參與遷移之小組內所有開發的來源。

    ![螢幕擷取畫面，顯示已完成遷移至 Azure DevOps Services。](./media/contoso-migration-tfs-vsts/git6.png)

**需要其他協助？**

如需詳細資訊，請參閱 [將存放庫從 TFVC 匯入 Git](/azure/devops/repos/git/import-from-tfvc?view=azure-devops&preserve-view=true&viewFallbackFrom=vsts)。

## <a name="clean-up-after-migration"></a>移轉之後進行清除

現在完成遷移後，Contoso 團隊必須執行下列動作：

- 檢閱[匯入之後](/azure/devops/migrate/migration-post-import?view=azure-devops&preserve-view=true&viewFallbackFrom=vsts)一文，以了解其他匯入活動的相關資訊。
- 請刪除 TFVC 存放庫或將其放在唯讀模式中。 不能使用程式碼基底，但可以參考其歷程記錄。

## <a name="post-migration-training"></a>移轉後訓練

Contoso 團隊必須為相關小組成員提供 Azure DevOps Services 和 Git 訓練。
