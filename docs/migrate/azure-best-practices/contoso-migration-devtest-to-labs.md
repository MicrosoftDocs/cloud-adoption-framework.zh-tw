---
title: 將開發/測試環境遷移至 Azure DevTest Labs
description: 瞭解 Contoso 如何使用 Azure DevTest Labs 將其內部部署 DevTest 至 Azure。
author: deltadan
ms.author: abuck
ms.date: 07/1/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: azure-migrate
ms.openlocfilehash: a65e509b2f48ce1dc9066bfc8e52501f555057b1
ms.sourcegitcommit: bcc73d194c6d00c16ae2e3c7fb2453ac7dbf2526
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/09/2020
ms.locfileid: "86197967"
---
<!-- cSpell:ignore vcenter -->

# <a name="migrate-a-devtest-environment-to-azure-devtest-labs"></a>將開發/測試環境遷移至 Azure DevTest Labs

本文示範 Contoso 虛構的公司如何將其開發/測試環境遷移至 Azure DevTest Labs。

## <a name="migration-options"></a>移轉選項

Contoso 在將開發/測試環境移至 Azure 時，有數個可用的選項。

| 移轉選項 | 成果 |
| --- | --- |
| [Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-services-overview) | [評估](https://docs.microsoft.com/azure/migrate/tutorial-assess-vmware)和[遷移](https://docs.microsoft.com/azure/migrate/tutorial-migrate-vmware)內部部署 vm <br><br> 使用 Azure IaaS 執行開發/測試伺服器 <br><br> 使用[Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)管理 vm |
| [DevTest Labs](https://docs.microsoft.com/azure/devtest-labs/devtest-lab-overview) | 快速布建開發和測試環境 <br><br> 使用配額和原則將浪費降至最低 <br><br> 設定自動關機以將成本降至最低 <br><br> 建立 Windows 和 Linux 環境 |

> [!NOTE]
> 本文著重于使用 DevTest Labs 將內部部署開發/測試環境移至 Azure。 閱讀 Contoso 如何透過 Azure Migrate[將開發/測試移至 Azure IaaS](./contoso-migration-devtest-to-iaas.md) 。

## <a name="business-drivers"></a>商業動機

開發領導小組已概述他們想要使用這種遷移達成的目標：

- 讓開發人員能夠存取 DevOps 工具和自助服務環境。
- 存取適用于 CI/CD 管線的 DevOps 工具，以及用於開發/測試的雲端原生工具，例如 AI、機器學習服務和無伺服器。
- 確保開發/測試環境中的治理和合規性。
- 將所有開發/測試環境移出其資料中心，而不再購買硬體以開發軟體，以節省成本。

> [!NOTE]
> Contoso 會使用適用于其環境的[隨用隨付開發/測試訂](https://azure.microsoft.com/offers/ms-azr-0023p)用帳戶供應專案。 其小組的每個 active Visual Studio 訂閱者都可以使用其訂用帳戶在 Azure 虛擬機器進行開發/測試的 Microsoft 軟體，而不需要額外付費。 Contoso 只需支付所執行 Vm 的 Linux 費率，甚至是具有 SQL Server、SharePoint Server 或其他軟體的 Vm，通常會以較高的費率計費。

<!-- -->

> [!NOTE]
> 具有 Enterprise 合約的 azure 客戶也可以受益于[Azure 開發/測試訂](https://azure.microsoft.com/offers/ms-azr-0148p)用帳戶供應專案。 若要深入瞭解，請參閱使用 Enterprise 合約入口網站建立 Azure 開發/測試訂用帳戶的這段[影片](https://channel9.msdn.com/blogs/ea.azure.com/enabling-and-creating-ea-devtest-subscriptions-through-the-ea-portal)。

## <a name="migration-goals"></a>移轉目標

Contoso 開發小組已將此遷移的目標固定在一起。 這些目標是用來判斷最合適的移轉方法：

- 快速布建開發和測試環境。 建立開發人員撰寫或測試軟體所需的基礎結構需要幾分鐘的時間。
- 在遷移之後，Contoso 在 Azure 中的開發/測試環境應該具備在目前內部部署系統上的增強功能。
- 作業模型會從已布建的 Vm 移至 DevOps，並提供自助式布建。
- Contoso 想要快速移出他們的內部部署開發/測試環境。
- 所有開發人員都會從遠端和安全地連接到開發/測試環境。

## <a name="solution-design"></a>解決方案設計

在將目標和需求固定之後，Contoso 會設計和審查部署解決方案，包括他們將用於開發/測試的 Azure 服務。

### <a name="current-architecture"></a>目前架構

- Contoso 應用程式的開發/測試 Vm 是在 VMware 的內部部署資料中心內執行。
- 這些 Vm 會在將程式碼升級至生產 Vm 之前，用於開發和測試。
- 開發人員會維護自己的工作站，但需要新的解決方案來從家庭辦公室進行遠端連線。

### <a name="proposed-architecture"></a>建議的架構

- Contoso 會使用[Azure 開發/測試訂](https://azure.microsoft.com/offers/ms-azr-0023p)用帳戶來降低 azure 資源的成本。 此訂用帳戶提供可觀的節約，包括不會產生 Microsoft 軟體授權費用的 Vm。
- DevTest Labs 將用於管理環境。 系統會在 DevTest Labs 中建立新的 Vm，以支援移至新工具以在雲端進行開發和測試。
- 在完成遷移之後，會解除委任 Contoso 資料中心內的內部部署開發/測試 Vm。
- 開發人員和測試人員可以存取其工作站的 Windows 虛擬桌面。

![案例架構 ](./media/contoso-migration-devtest-to-labs/architecture.png)
 _圖1：案例架構。_

### <a name="database-considerations"></a>資料庫考量

為了支援持續的開發，Contoso 已決定繼續使用在 Vm 上執行的資料庫，但目前的 Vm 將會取代為在 DevTest Labs 中執行的新虛擬機器。 在未來，Contoso 會使用 PaaS 服務，例如[Azure SQL Database](https://docs.microsoft.com/azure/azure-sql/database/sql-database-paas-overview)和[適用於 MySQL 的 Azure 資料庫](https://docs.microsoft.com/azure/mysql/overview)。

目前的 VMware 資料庫 Vm 將會解除委任，並取代為 DevTest Labs 中的 Azure Vm。 將會使用簡單的備份和還原來遷移現有的資料庫。 藉由使用 Azure 開發/測試訂用帳戶供應專案，Windows Server 和 SQL Server 實例不會產生授權費用，並可將計算成本降到最低。

### <a name="solution-review"></a>解決方案檢閱

Contoso 會透過比較一份優缺點清單，來評估建議設計。

| 考量 | 詳細資料 |
| --- | --- |
| **優點** | 所有目前的開發 Vm (應用程式和資料庫) 都會由 DevTest Labs 中執行的新 Vm 所取代。 這表示它們可以利用專為雲端開發環境提供的功能。 <br><br> Contoso 可以利用他們在 Azure 開發/測試訂用帳戶中的投資，以節省授權費用。 <br><br> Contoso 會保留 Azure 中應用程式 Vm 的完整控制權。 <br><br> 開發人員將會提供訂用帳戶的許可權，讓他們能夠建立新的資源，而不需要等候它回應其要求。 |
| **缺點** | 遷移只會將開發移至雲端，但它們在開發時不會使用 PaaS 服務，因為它們仍在使用 Vm。 這表示 Contoso 必須開始支援其 Vm 的作業，包括安全性修補程式。 這是由 IT 在過去進行維護，而且需要尋找此新作業工作的解決方案。 <br><br> Contoso 必須建立新的應用程式和資料庫 Vm，以自動化該流程。 這表示他們可以利用在雲端中建立 Vm 和 DevTest Labs 所提供的工具，因此這是正面的結果，即使是在其清單中。 |

### <a name="migration-process"></a>移轉程序

Contoso 會使用 DevTest Labs，將其開發應用程式和資料庫 Vm 遷移至新的 Azure Vm。

- 他們已經準備好[Azure 基礎結構](./contoso-migration-infrastructure.md)，包括其開發虛擬網路。
- 完成所有準備工作之後，Contoso 會布建並設定 DevTest Labs。
- 他們會設定開發虛擬網路、指派資源群組，以及設定原則。
- Contoso 會建立 Windows 虛擬桌面，讓開發人員使用遠端位置。
- 在 DevTest Labs 中建立用於開發和遷移資料庫的 Vm。

![遷移程式 ](./media/contoso-migration-devtest-to-labs/migration-process-devtest-labs.png)
 _圖2：遷移程式。_

## <a name="prerequisites"></a>必要條件

以下是 Contoso 要執行此案例所需的項目。

| 需求 | 詳細資料 |
| --- | --- |
| **Azure 開發/測試訂用帳戶** | Contoso 會建立[Azure 開發/測試訂](https://azure.microsoft.com/offers/ms-azr-0023p)用帳戶，以降低成本達80%。 <br><br> 若您沒有 Azure 訂用帳戶，請建立一個[免費帳戶](https://azure.microsoft.com/free)。 <br><br> 如果您建立免費帳戶，您就是訂閱的系統管理員，而且可以執行所有動作。 <br><br> 如果您使用現有的訂用帳戶，但您不是系統管理員，則必須與系統管理員合作，為您指派擁有者或參與者許可權。 <br><br> 如果您需要更細微的權限，請檢閱[此文章](https://docs.microsoft.com/azure/site-recovery/site-recovery-role-based-linked-access-control)。 |
| **Azure 基礎結構** | [了解](./contoso-migration-infrastructure.md) Contoso 如何設定 Azure 基礎結構。 |

## <a name="scenario-steps"></a>案例步驟

以下是 Contoso 管理員執行移轉的方式：

> [!div class="checklist"]
>
> - **步驟1：布建新的 Azure 開發/測試訂用帳戶，並建立 DevTest Labs 實例。**
> - **步驟2：設定開發虛擬網路、指派資源群組，以及設定原則。**
> - **步驟3：建立適用于開發人員的 Windows 10 企業版多會話虛擬桌面，以從遠端位置使用。**
> - **步驟4：在 DevTest Labs 中建立用於開發和遷移資料庫的公式和 Vm。**

## <a name="step-1-provision-a-new-azure-devtest-subscription-and-create-a-devtest-labs-instance"></a>步驟1：布建新的 Azure 開發/測試訂用帳戶，並建立 DevTest Labs 實例

Contoso 必須先使用 Azure 開發/測試供應專案來布建新的訂用帳戶，然後建立 DevTest Labs 實例。

他們依照下列方式進行其設定：

Contoso 會遵循[Azure 開發/測試訂](https://azure.microsoft.com/offers/ms-azr-0023p)用帳戶供應專案的連結，並布建新的訂用帳戶，將其儲存在其系統上最多80%。 此供應專案可讓 Contoso 在 Azure 上執行 Windows 10 映射以進行開發/測試。 他們將取得[Windows 虛擬桌面](https://docs.microsoft.com/azure/virtual-desktop/overview)的存取權，以簡化遠端開發人員的管理體驗。

![DevTest 供應專案 ](./media/contoso-migration-devtest-to-labs/devtest-subscription.png)
 _圖3： Azure 開發/測試訂_用帳戶供應專案。

在布建新的訂用帳戶之後，Contoso 會使用 Azure 入口網站建立新的 DevTest Labs 實例。 新的實驗室會建立在 `ContosoDevRG` 資源群組中。

![使用入口網站建立 DevTest Labs ](./media/contoso-migration-devtest-to-labs/new-lab.png)
 _圖4：建立新的 DevTest labs 實例。_

## <a name="step-2-configure-the-development-virtual-network-assign-a-resource-group-and-set-policies"></a>步驟2：設定開發虛擬網路、指派資源群組，以及設定原則

建立 DevTest Labs 之後，Contoso 會在下列步驟期間執行設定：

- 設定虛擬網路
- 指派資源群組
- 設定實驗室原則

他們依照下列方式進行其設定：

1. 設定虛擬網路：

    - 在入口網站中，Contoso 會開啟 [DevTest Labs] 實例，並選取 [設定**和原則]：**

      ![設定和原則 ](./media/contoso-migration-devtest-to-labs/configure-lab.png)
       _圖5： DevTest Labs 實例：設定和原則。_

    - Contoso 會選取 [**虛擬網路**  >  **+ 新增**]、[選擇] `vnet-dev-eus2` ，然後選取 [**儲存**]。 這可讓開發 VNet 用於 VM 部署。 此外，在部署 DevTest Labs 實例時，也會建立一個虛擬網路。

      ![虛擬網路 ](./media/contoso-migration-devtest-to-labs/vnets.png)
       _圖6：虛擬網路。_

2. 指派資源群組：

    - 為了確保資源會部署到 `ContosoDevRG` 資源群組，Contoso 會在實驗室設定中進行這項設定。 他們也會將角色指派給他們的開發人員 `Contributor` 。

      ![指派資源群組 ](./media/contoso-migration-devtest-to-labs/assign-resource-group.png)
       _圖7：指派資源群組。_

    > [!NOTE]
    > 「參與者」角色是具備擁有權限的系統管理員層級角色，除了能夠提供存取權給其他使用者以外。 閱讀更多[AZURE RBAC 控制項](https://docs.microsoft.com/azure/role-based-access-control/overview)的相關資訊。

3. 設定實驗室原則：

    - Contoso 必須確保他們的開發人員在其小組的原則中使用 DevTest Labs。 他們會使用這些原則來設定 DevTest Labs。

    - 自動關閉是以 7:00:00 pm 的本地時間和正確的時區啟用。

      ![自動關機 ](./media/contoso-migration-devtest-to-labs/autoshutdown.png)
       _圖8：自動關閉。_

    - 當開發人員上線運作時，自動啟動已啟用，讓其 Vm 啟動並執行。 它們會設定為當地時區，以及每週的工作天數。

      ![自動啟動 ](./media/contoso-migration-devtest-to-labs/autostart.png)
       _圖9：自動啟動。_

    - 已設定允許的 VM 大小，以確保不允許啟動大型且昂貴的 Vm。

      ![允許的 VM 大小 ](./media/contoso-migration-devtest-to-labs/vm-sizes.png)
       _圖10：允許的 vm 大小。_

    - 支援訊息已設定。

      ![支援訊息 ](./media/contoso-migration-devtest-to-labs/support.png)
       _圖11：支援訊息。_

## <a name="step-3-create-windows-10-enterprise-multi-session-virtual-desktops-for-developers-to-use-from-remote-locations"></a>步驟3：建立適用于開發人員的 Windows 10 企業版多會話虛擬桌面以從遠端位置使用

Contoso 必須為遠端開發人員建立 Windows 虛擬桌面。

Contoso 會建立一個來自基底的 Windows 10 企業版多重會話 VM：

- Contoso 會選取 [**所有虛擬機器**] [ >  **新增**] 並選擇 `Windows 10 enterprise multi-session` 基底。

  ![Windows 10 基礎 ](./media/contoso-migration-devtest-to-labs/windows-10-vm-base.png)
   _圖12： Windows 10 企業版多會話基礎。_

- 接下來，VM 的大小會與要安裝的成品一併設定。 在此情況下，開發人員可以存取常見的開發工具，例如 Visual Studio Code、Git 和 Chocolatey。

  ![構件 ](./media/contoso-migration-devtest-to-labs/artifacts.png)
   _圖13：構件。_

- 已檢查 VM 設定的正確性。

  ![從基本圖14建立虛擬機器 ](./media/contoso-migration-devtest-to-labs/vm-from-base.png)
   _：從基底建立虛擬機器。_

- 建立 VM 之後，Contoso 的遠端開發人員就可以連接並使用此開發工作站來執行其工作。 系統會安裝所選取的成品，讓開發人員在設定其工作站時儲存。

  ![RemoteDevs VM ](./media/contoso-migration-devtest-to-labs/remote-vm.png)
   _圖15：遠端開發人員 VM。_

## <a name="step-4-create-formulas-and-vms-within-devtest-labs-for-development-and-migrate-databases"></a>步驟4：在 DevTest Labs 中建立用於開發和遷移資料庫的公式和 Vm

設定 DevTest Labs 且遠端開發人員工作站啟動並執行後，Contoso 著重于建立其 Vm 以進行開發。 若要開始使用，Contoso 會完成下列步驟：

建立適用于應用程式和資料庫 Vm 的公式 (可重複使用的基底) ，並使用公式布建應用程式和資料庫 Vm。

建立應用程式和資料庫 Vm 的公式：

- Contoso 會選取 [**公式**] [  >  **+ 新增**]，然後選取 [ `Windows 2012 R2 Datacenter` 基底]。

  ![Windows 2012 R2 基礎 ](./media/contoso-migration-devtest-to-labs/windows-2012-base.png)
   _圖16： Windows 2012 r2 的基礎。_

- 接下來，VM 的大小會與要安裝的成品一併設定。 在此情況下，開發人員將可存取常見的開發工具，例如 Visual Studio Code、Git 和 Chocolatey。

  ![Windows 2012 R2 基本設定 ](./media/contoso-migration-devtest-to-labs/windows-2012-base-configuration.png)
   _圖17： Windows 2012 r2 基本設定。_

- 若要建立資料庫 VM 公式，Contoso 會遵循相同的基本步驟，這次請選取基底的 SQL Server 2012 映射。

  ![SQL 2012 R2 基礎 ](./media/contoso-migration-devtest-to-labs/sql-2012-base.png)
   _圖18： SQL Server 2012 映射。_

- 公式會以大小和成品進行設定，包括 SQL Server Management Studio，這是此資料庫開發 VM 公式所需的專案。

  ![SQL 2012 R2 基本設定 ](./media/contoso-migration-devtest-to-labs/sql-2012-base-configuration.png)
   _圖19： Sql 2020 r2 基本設定。_

深入瞭解如何搭配使用[公式](https://docs.microsoft.com/azure/lab-services/devtest-lab-manage-formulas)與 DevTest Labs。

- Contoso 現在已建立 Windows 基底公式，供其開發人員用於其應用程式和資料庫。

  ![設定的資料庫 VM ](./media/contoso-migration-devtest-to-labs/contoso-formulas.png)
   _圖20： Windows 基底公式。_

使用公式來布建應用程式和資料庫 Vm：

- 建立公式之後，Contoso 接著會選取 [**所有虛擬機器**]，然後選擇 `Windows2012AppDevVmBase` 符合其目前應用程式開發 vm 設定的公式。

  ![App VM ](./media/contoso-migration-devtest-to-labs/app-vm.png)
   _圖21：應用程式開發 vm。_

- VM 會以此應用程式 VM 所需的大小和成品進行設定。

  ![已設定的應用程式 VM ](./media/contoso-migration-devtest-to-labs/app-vm-configuration.png)
   _圖22： VM 的大小和_成品設定。

- 接下來，使用**SQLDbDevVmBase**公式來布建資料庫 VM，以符合其目前資料庫開發 vm 的設定。

  ![資料庫 VM ](./media/contoso-migration-devtest-to-labs/database-vm.png)
   _圖23：資料庫 vm。_

- VM 會以所需的大小和成品進行設定。

  ![資料庫 VM 設定 ](./media/contoso-migration-devtest-to-labs/database-vm-config.png)
   _圖24： VM 的資料庫設定。_

- 隨著他們的第一個 Vm 與其遠端開發人員的工作站一起建立，Contoso 的開發人員已準備好開始在 Azure 中撰寫程式碼。

  ![Contoso Vm ](./media/contoso-migration-devtest-to-labs/contoso-vms.png)
   _圖25： contoso vm。_

- Contoso 現在可以從備份還原其開發資料庫，或使用某種類型的程式碼產生程式來建立 Vm 上的架構。 透過已使用這些成品安裝的 SQL Server Management Studio，這些是不需要安裝任何工具的簡單工作。

## <a name="clean-up-after-migration"></a>移轉之後進行清除

Contoso 會繼續使用這些步驟，使用 DevTest Labs 將其 Vm 遷移至 Azure。 每次遷移完成時，所有開發 Vm 現在都是在 DevTest Labs 中執行。

現在，Contoso 必須完成以下的清除步驟：

- 從 vCenter 清查中移除 Vm。
- 從本機備份作業中移除所有 Vm。
- 更新內部檔以顯示 Vm 的新位置和 IP 位址。
- 檢閱與 VM 互動的任何資源，並更新任何相關的設定或文件，以反映新的組態。

### <a name="security"></a>安全性

Contoso 安全性小組會檢查 Azure Vm，以判斷是否有任何安全性問題。 若要控制存取權，小組會檢閱 VM 的網路安全性群組 (NSG)。 Nsg 是用來確保只有應用程式允許的流量可以到達它。 小組也會考慮使用 Azure 磁碟加密和 Azure Key Vault 來保護磁片上的資料。 如需詳細資訊，請參閱[Azure 中 IaaS 工作負載的安全性最佳作法](https://docs.microsoft.com/azure/security/fundamentals/iaas)。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- Contoso 會確保使用此開發/測試訂用帳戶建立所有開發 Azure 資源，以利用節省80% 的成本。
- 將會針對所有 DevTest 實驗室審查預算，並備妥 Vm 類型的原則，以確保包含成本，且過度布建不會錯誤地發生。
- Contoso 會啟用[Azure 成本管理和計費](https://docs.microsoft.com/azure/cost-management-billing/cost-management-billing-overview)，以協助監視和管理 Azure 資源。

## <a name="conclusion"></a>結論

在本文中，Contoso 將其開發環境移至 DevTest Labs。 它們也會實作為遠端和合約開發人員的平臺 Windows 虛擬桌面。

**需要其他協助？**

立即在您的訂用帳戶中[建立 DevTest labs 實例](https://docs.microsoft.com/azure/lab-services/devtest-lab-create-lab)，並瞭解如何使用[適用于開發人員的 DevTest labs](https://docs.microsoft.com/azure/lab-services/devtest-lab-developer-lab)。
