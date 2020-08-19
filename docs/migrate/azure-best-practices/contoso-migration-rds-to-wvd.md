---
title: 將內部部署遠端桌面服務移至 Azure Windows 虛擬桌面案例
description: 瞭解 Contoso 如何使用 Windows 虛擬桌面將其內部部署 RDS 遷移至 Azure。
author: benstegink
ms.author: abuck
ms.date: 07/01/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: azure-migrate
ms.openlocfilehash: be46b1d4d0848782ae2906498f38ba9bc45e180b
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88567320"
---
<!-- cSpell:ignore benstegink msiexec Logix Lakeside SysTrack Robocopy UPD UPDs -->

# <a name="move-on-premises-remote-desktop-services-to-azure-windows-virtual-desktop-scenario"></a>將內部部署遠端桌面服務移至 Azure Windows 虛擬桌面案例

Windows 虛擬桌面是在雲端中執行的全方位桌面與應用程式虛擬化服務。 它是唯一 (VDI) 的虛擬桌面基礎結構，可提供簡化的管理、Windows 10 企業版適用于企業的 Microsoft 365 應用程式的多重會話優化，以及遠端桌面服務 (RDS) 環境的支援。 幾分鐘內就能在 Azure 上部署及調整 Windows 桌面和應用程式，並取得內建的安全性和合規性功能。

| 移轉選項 | 結果 |
|--- | --- |
| [Azure Migrate](/azure/migrate/migrate-services-overview) | 評定及遷移內部部署 RDS 環境。 <br><br> 使用 Azure Windows 虛擬桌面執行工作負載。 <br><br> 使用 [Windows 虛擬桌面管理 UX](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/wvd-management-ux)管理 Windows 虛擬桌面。 |

> [!NOTE]
> 本文著重于在 Azure 中使用 Windows 虛擬桌面將內部部署 RDS 環境移至 Azure。

## <a name="business-drivers"></a>商業動機

與商務合作夥伴密切合作，Contoso IT 小組會定義將 VDI 遷移至 Azure 的商業驅動程式。 這些驅動程式可能包括：

- **目前的環境生命週期結束：** 當資料中心達到租用期結束或關閉時，就會超出容量。 遷移至雲端可提供幾乎無限制的容量。 目前的軟體也可能在生命週期結束後，必須將執行 Contoso 目前的 VDI 解決方案的軟體升級為必要。
- **多會話 WINDOWS 10 VDI：** 為 Contoso 使用者提供在雲端中虛擬化的唯一多會話 Windows 10 桌面，可高度擴充、最新且可在任何裝置上使用。
- 針對**enterprise 的 Microsoft 365 應用程式優化：** 為企業經驗提供最佳的 Microsoft 365 應用程式，提供多會話的虛擬桌面案例，為 Contoso 的使用者提供最有效率的虛擬化體驗。
- **幾分鐘內即可部署及調整：** 使用 Azure 入口網站中的整合管理，在短短幾分鐘內快速將現代化和舊版桌面應用程式虛擬化及部署至雲端。
- **Azure 和 Microsoft 365 的安全和生產力：** 部署完整的智慧型解決方案，為每個人增強創意和協同作業。 移至 Microsoft 365 並取得 Office 365、Windows 10 和 Enterprise Mobility + Security。

## <a name="rds-on-premises-to-windows-virtual-desktop-in-the-cloud-goals"></a>雲端目標中的 RDS 內部部署至 Windows 虛擬桌面

考慮到商務驅動程式之後，Contoso 已將此遷移的目標釘選：

- 將雲端的虛擬桌面環境現代化。
- 利用現有的 Microsoft 365 授權。
- 當使用者從遠端工作時，提升公司資料的安全性。
- 針對成本和成長優化新的環境。

這些目標支援使用 Windows 虛擬桌面的決策，並將其驗證為 Contoso 的最佳遷移方法。

## <a name="benefits-of-running-windows-virtual-desktop-in-azure"></a>在 Azure 中執行 Windows 虛擬桌面的優點

藉由在 Azure 中使用 Windows 虛擬桌面，Contoso 現在可以快速且輕鬆地順暢地執行、管理及調整其 VDI 解決方案。 公司也可以為其使用者提供優化的多會話 Windows 10 環境。

Contoso 會使用 Azure 的規模調整、效能、安全性和創新，以利用現有的 Microsoft 365 授權。

其他優點可能包括：

- 從任何地方存取 Windows 虛擬桌面。
- 針對企業環境優化 Microsoft 365 應用程式。
- 適用于開發/測試環境的 Windows 虛擬桌面。

## <a name="solutions-design"></a>解決方案設計

在達到目標和需求後，Contoso 會設計和審核部署解決方案，並識別遷移程式。

### <a name="current-architecture"></a>目前架構

RDS 會部署到內部部署資料中心。 Microsoft 365 已獲得組織授權和使用。

### <a name="proposed-architecture"></a>建議的架構

- 同步 Active Directory 或 Azure Active Directory Domain Services。
- 將 Windows 虛擬桌面部署至 Azure。
- 將內部部署 RDS 伺服器遷移至 Azure。
- 將使用者設定檔磁片 (Upd) 轉換為 FSLogix 設定檔容器。

  ![圖表顯示建議的架構。 ](./media/contoso-migration-rds-to-wvd/proposed-architecture.png)
  _圖1：建議的架構。_

## <a name="solution-review"></a>解決方案檢閱

Contoso 藉由結合一份優缺點來評估提議的設計。

| 考量 | 詳細資料 |
| --- | --- |
| **優點** | Windows 10 企業版多會話環境。 <br><br> 以雲端為基礎，允許從任何地方進行存取。 <br><br> 利用 Windows 虛擬桌面環境中的其他 Azure 服務（例如 Azure 檔案儲存體）。 <br><br> 已針對 Microsoft 新式桌面進行優化。 |
| **缺點** | 為了充分優化 Azure，Contoso 必須重建針對多使用者會話優化的 Windows 10 映射。 <br><br> Windows 虛擬桌面不支援使用者設定檔磁片，因此必須將 Upd 遷移至 FSLogix 設定檔容器。 |

## <a name="migration-process"></a>移轉程序

Contoso 會使用 Lakeside 評定工具和 Azure Migrate，將 Vm 移至 Azure 中的 Windows 虛擬桌面。 Contoso 將需要：

- 針對其內部部署 RDS 基礎結構執行評量工具，以在 Azure 中建立 Windows 虛擬桌面部署的規模。
- 透過 Windows 10 企業版多會話或持續性的虛擬機器，遷移至 Windows 虛擬桌面。
- 視需要相應增加和減少以管理成本，將 Windows 虛擬桌面多會話優化。
- 虛擬化應用程式，並視需要指派使用者，以繼續保護和管理 Windows 虛擬桌面環境。

  ![圖表顯示遷移程式。 ](./media/contoso-migration-rds-to-wvd/migration-process-01.png)
  _圖2：遷移程式。_

## <a name="scenario-steps"></a>案例步驟

1. 評估目前的 RDS 環境。
2. 在 Azure 中建立 VDI 和新映射，並將 Vm 遷移並保存到 Azure。
3. 將 Upd 轉換為 FSLogix 設定檔容器。
4. 將任何持續性的 Vm 複寫至 Azure。

## <a name="step-1-assess-the-current-on-premises-environment"></a>步驟1：評定目前的內部部署環境

Contoso 會在 Azure 區域中布建 Windows 虛擬桌面服務 `East US 2` 。 使用 Windows 虛擬桌面，Contoso 可以布建虛擬機器、主機集區，以及建立應用程式群組。 Windows 虛擬桌面也會設定 Windows 虛擬桌面解決方案中所有伺服器的可用性設定組。 Windows 虛擬桌面可讓 Contoso 建立高可用性的 VDI 環境，並視需要快速擴大和縮小。

> [!NOTE]
> Contoso 在評量期間已複習兩個案例：多會話 (RDS 和持續性 (的共用) 實例，或使用者專用的) 虛擬機器。

1. 確定網域服務（Active Directory 或 Azure Active Directory Domain Services）會與 Azure Active Directory (Azure AD) 進行同步處理。 確定可從 Azure 訂用帳戶和虛擬網路存取網域服務，以在其中部署 Windows 虛擬桌面。

    > [!NOTE]
    > 深入瞭解使用 Azure AD 同步處理 Active Directory 內部部署的 [Azure AD Connect](/azure/active-directory/hybrid/how-to-connect-install-express) 。

    <!-- -->

    > [!NOTE]
    > 瞭解如何布建 [Azure Active Directory Domain Services](/azure/active-directory-domain-services/tutorial-create-instance) 以及同步處理 Azure AD。

1. 建立新的 Azure Migrate 專案。

   ![顯示建立新 Azure Migrate 專案的螢幕擷取畫面。 ](./media/contoso-migration-rds-to-wvd/new-azure-migrate.png)
   _圖3：建立新的 Azure Migrate 專案。_

1. 選取用來評估和遷移伺服器的選項、選取 [ **VDI**]，然後新增工具。

   ![顯示 VDI Azure Migrate 目標的螢幕擷取畫面。](./media/contoso-migration-rds-to-wvd/azure-migrate-goals-vdi.png)

   _圖4：目標 Azure Migrate 目標。_

1. 設定遷移作業資料的訂用帳戶、資源群組、專案名稱和地理位置。

   ![顯示將作業資料新增至 Azure Migrate 專案的螢幕擷取畫面。 ](./media/contoso-migration-rds-to-wvd/add-a-tool.png)
   _圖5：將作業資料新增至遷移。_

    > [!IMPORTANT]
    > 此位置不是將部署新 Windows 虛擬桌面環境的位置。 只有與 Azure Migrate 專案相關的資料會儲存在這裡。

1. 選取 **Lakeside： SysTrack** 作為評量工具。

1. 選取 **Azure Migrate：** 作為遷移工具的伺服器遷移。

1. 將工具新增至遷移專案。

   ![顯示將工具新增至專案的螢幕擷取畫面。 ](./media/contoso-migration-rds-to-wvd/add-tools.png)
   _圖6：將工具新增至遷移。_

1. 在 Lakeside 工具中選取 [ **向 Azure Migrate** 登錄]，開始評估目前的環境。

   ![顯示 Lakeside 註冊 Azure Migrate 的螢幕擷取畫面。](./media/contoso-migration-rds-to-wvd/lakeside-register-with-azure-migrate.png)

   _圖7：評定目前的環境。_

1. Contoso 會連接 Azure Migrate 和 Lakeside，並接受任何要求的許可權。

   ![顯示登入以連接 Azure 和 Lakeside 的螢幕擷取畫面。 ](./media/contoso-migration-rds-to-wvd/lakeside-login.png)
   _圖8：將 Azure 連接到 lakeside。_

1. Contoso 會繼續使用 Lakeside 工具來建立新的租使用者，並開始評估目前的內部部署 RDS 環境。 從儀表板中，Contoso 可以存取部署指南、下載評量用戶端以部署至目前的環境，以及檢查從這些代理程式收集到的資料。

   ![顯示 Lakeside 儀表板的螢幕擷取畫面。 ](./media/contoso-migration-rds-to-wvd/lakeside-new-tenant-dashboard.png)
   _圖9： Lakeside 儀表板。_
1. 在捕獲到足夠的資料量之後，Contoso 會審核評量資料來判斷最佳的遷移路徑。 此評量資料包含來自桌面資料的原始評量資料，以及分成不同使用者角色的資料。 此資訊包括：

- 每個角色中的使用者數目。
- 使用者正在使用的應用程式。
- 依使用者的資源耗用量。
- 依使用者角色的資源使用率平均值。
- VDI 伺服器效能資料。
- 並行使用者報告。
- 使用中的熱門軟體套件。

    ![顯示 Lakeside 儀表板報表的螢幕擷取畫面。](./media/contoso-migration-rds-to-wvd/lakeside-dashboard-reports.png)
    _圖10： Lakeside 儀表板報表。_

Contoso 會分析資料，以判斷共用的 Windows 虛擬桌面資源和個人 Windows 虛擬桌面資源的最符合成本效益。

> [!NOTE]
> Contoso 也需要將應用程式伺服器遷移至 Azure，讓公司更接近 Windows 虛擬桌面環境，並減少其使用者的網路延遲。

## <a name="step-2-create-the-windows-virtual-desktop-environment-for-pooled-desktops"></a>步驟2：建立共用桌面的 Windows 虛擬桌面環境

使用 Azure 入口網站，Contoso 會建立用於共用資源的 Windows 虛擬桌面環境。 稍後會進行遷移步驟，以將個人桌面連接至相同的環境。

1. Contoso 會選取正確的訂用帳戶，並建立新的 Windows 虛擬桌面主機集區。

   ![顯示布建 Windows 虛擬桌面主機集區的螢幕擷取畫面。 ](./media/contoso-migration-rds-to-wvd/wvd-new-host-pool.png)
  _圖11：新的 Windows 虛擬桌面主機集區。_

1. 指定訂用帳戶、資源群組和區域。 然後選取主機集區的名稱、桌上型電腦類型，以及預設的桌面使用者。 桌面類型設定為集區 **，因為 Contoso** 會為其使用者的新共用環境啟動。 預設桌面使用者可以保留空白。 繼續設定虛擬機器。

   ![顯示設定虛擬機器之必要條件的螢幕擷取畫面。 ](./media/contoso-migration-rds-to-wvd/wvd-new-host-pool-basics-alt.png)
   _圖12：設定虛擬機器的必要條件。_

   - Contoso 會設定 VM，並選取 [ **變更大小** ] 或使用預設值來選擇自訂大小。
   - Windows 虛擬桌面會被選為這些共用桌面的 VM 名稱前置詞。
   - 因為 Contoso 正在建立共用伺服器，以使用新的 Windows 10 多會話功能來進行虛擬機器設定，請將映射來源設定為資源 **庫**。 此選項可讓 Contoso 選取 Vm 的 Windows 10 企業版多會話映射。
   - 根據 Lakeside 評量使用者的角色，Contoso 會將使用者總數設為 **150**。
   - 其他設定包括磁片類型、AD 網域加入 UPN 欄位、系統管理員密碼、新增機器的選用 OU 路徑、虛擬網路，以及用於新增伺服器的子網。

   ![顯示如何設定虛擬機器的螢幕擷取畫面。 ](./media/contoso-migration-rds-to-wvd/wvd-new-host-pool-configure-virtual-machines-alt.png)
   _圖13：設定虛擬機器。_

    > [!NOTE]
    > Contoso 無法在此步驟建立新的虛擬網路。 在達到此步驟之前，Contoso 應該已建立可存取 Active Directory 的虛擬網路。

   <!-- -->

    > [!NOTE]
    > Contoso 無法在此步驟中使用需要多重要素驗證的使用者帳戶。 如果 Contoso 打算針對其使用者使用多重要素驗證，則必須為此用途建立服務主體。

1. Contoso 會對 Windows 虛擬桌面設定執行一次驗證，並建立共用 Windows 虛擬桌面虛擬機器的新環境。

   ![顯示檢查和建立虛擬機器的螢幕擷取畫面。 ](./media/contoso-migration-rds-to-wvd/wvd-new-host-pool-review-create.png)
   _圖14：檢查和建立虛擬機器。_

## <a name="step-3-convert-the-upds-to-fslogix-profile-containers"></a>步驟3：將 Upd 轉換為 FSLogix 設定檔容器

因為 Windows 虛擬桌面不支援 (Upd) 的使用者設定檔磁片，所以 Contoso 需要透過 [FSLogixMigration PowerShell 模組](https://aka.ms/FSLogixMigrationPreviewModule)將所有 upd 轉換成 FSLogix。

<!-- docsTest:ignore FSLogixMigration -->

在 Contoso 匯入 FSLogixMigration 模組之後，它會執行下列 PowerShell Cmdlet，以從 Upd 遷移至 FSLogix。

> [!IMPORTANT]
> Hyper-v、Active Directory 和 Pester 的 PowerShell 模組是執行 Cmdlet 以將 Upd 轉換為 FSLogix 的必要條件。

UDP 轉換：

```powershell
Convert-RoamingProfile -ParentPath "C:\Users\" -Target "\\Server\FSLogixProfiles$" -MaxVHDSize 20 -VHDLogicalSectorSize 512
```

漫遊設定檔轉換：

```powershell
Convert-RoamingProfile -ProfilePath "C:\Users\User1" -Target "\\Server\FSLogixProfiles$" -MaxVHDSize 20 -VHDLogicalSectorSize 512 -VHD -IncludeRobocopyDetails -LogPath C:\temp\Log.txt
```

到目前為止，遷移已使用共用資源搭配 Windows 10 企業版多會話來啟用。 Contoso 可以開始將必要的應用程式部署到將使用 Windows 10 企業版多重會話的使用者。

但現在 Contoso 必須將持續性虛擬機器遷移至 Azure。

## <a name="step-4-replicate-and-persist-vms-to-windows-virtual-desktop"></a>步驟4：將 Vm 複寫並保存到 Windows 虛擬桌面

Contoso 遷移程式的下一個步驟是將其持續性虛擬機器遷移至 Windows 虛擬桌面。 若要這樣做，Contoso 會回到在程式開始時所建立的 Azure Migrate：伺服器遷移作業。

1. Contoso 一開始先選取 [Azure Migrate：伺服器遷移工具] 中的 [ **探索** ]。

   ![顯示 [Azure Migrate：伺服器遷移探索] 選項的螢幕擷取畫面。 ](./media/contoso-migration-rds-to-wvd/wvd-persistent-discover.png)
   _圖15：探索伺服器遷移。_

1. Contoso 會將管理機器複寫的環境中的設備轉換成 Windows 虛擬桌面。 確定目的地區域已設定為 `East US 2` Windows 虛擬桌面環境建立所在的。

   ![顯示建立設備以管理複寫的螢幕擷取畫面。 ](./media/contoso-migration-rds-to-wvd/wvd-persistent-appliance.png)
   _圖16：轉換設備。_

1. 複寫提供者會下載、安裝並註冊至 Azure Migrate 專案，以開始複寫至 Azure。

   ![顯示下載和設定複寫的螢幕擷取畫面。 ](./media/contoso-migration-rds-to-wvd/wvd-persistent-replication.png)
   _圖17：複寫到 Azure 的必要條件。_

1. 現在已啟動將主機複寫到 Azure Blob 儲存體的功能。 Contoso 可以繼續進行複寫，直到它準備好測試 Vm，然後將它們遷移到生產環境。
   - 當電腦在 Azure 中開始執行時，Contoso 會確定在每部電腦上安裝 [Windows 虛擬桌面 VM 代理程式](https://aka.ms/WVDVMAgent) 。
   - 在安裝過程中，請輸入 Windows 虛擬桌面環境的註冊權杖，以將伺服器與正確的環境產生關聯。

1. 您可以使用下列命令來取得註冊權杖：

    ```powershell
    Export-RDSRegistrationInfo -TenantName "Contoso" -HostPoolName "ContosoWVD" | Select-Object -ExpandProperty Token > .\registration-token.txt
    ```

    > [!NOTE]
    > Contoso 也可以使用 `msiexec` 命令並傳入註冊權杖作為變數，將此程式自動化。

1. 在最後一次遷移之前的最後一個步驟中，Contoso 會在 Azure Windows 虛擬桌面設定中選取 [ **使用者** ] 專案，以將伺服器對應至各自的使用者和群組。

   ![顯示將 Windows 虛擬桌面資源指派給使用者和群組的螢幕擷取畫面。 ](./media/contoso-migration-rds-to-wvd/wvd-persistent-user-mapping.png)
   _圖18：進行最後的遷移之前的最後一個步驟。_

將主機集區指派給使用者之後，Contoso 會完成這些機器的遷移作業，並繼續將其餘的內部部署 VDI 主機逐漸遷移至 Azure。

## <a name="review-the-deployment"></a>檢閱部署

現在，當虛擬桌面和應用程式伺服器在 Azure 中執行時，Contoso 現在必須完全讓並保護部署的安全。

### <a name="security"></a>安全性

Contoso 安全性小組會檢查 Azure Vm 以判斷任何安全性問題。 若要控制存取權，小組會檢閱 VM 的網路安全性群組 (NSG)。 Nsg 是用來確保只有應用程式允許的流量可以到達它。 小組也會考慮使用 Azure 磁碟加密和 Azure Key Vault 來保護磁片上的資料。

如需詳細資訊，請參閱 [Azure 中 IaaS 工作負載的安全性最佳作法](/azure/security/fundamentals/iaas)。

## <a name="business-continuity-and-disaster-recovery"></a>商務持續性和災害復原

針對商務持續性和嚴重損壞修復 (BCDR) ，Contoso 會使用 Azure 備份來備份 Vm 上的資料，以確保資料安全。 如需詳細資訊，請參閱 [AZURE VM 備份的總覽](/azure/backup/backup-azure-vms-introduction)。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- [Microsoft 365 授權](https://azure.microsoft.com/pricing/details/virtual-desktop/) 用於桌上型電腦部署。
- Contoso 會啟用 [Azure 成本管理和帳單](/azure/cost-management-billing/cost-management-billing-overview) ，以協助監視和管理 Azure 資源。
- Contoso 具有其 Vm 的現有授權，而且會利用應用程式伺服器的 Azure Hybrid Benefit。 Contoso 會轉換現有的 Azure VM，以便充分利用這個定價。

## <a name="conclusion"></a>結論

在本文中，Contoso 將其 RDS 部署移至裝載于 Azure 中的 Windows 虛擬桌面。
