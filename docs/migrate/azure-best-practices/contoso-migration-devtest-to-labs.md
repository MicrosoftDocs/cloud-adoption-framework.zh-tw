---
title: 將開發/測試環境遷移至 Azure DevTest Labs
description: 瞭解 Contoso 如何使用 Azure DevTest Labs 將其內部部署開發/測試環境移至 Azure。
author: deltadan
ms.author: abuck
ms.date: 07/1/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
services: azure-migrate
ms.openlocfilehash: 30fcad46c954865001b9667c5d0f04b61e9e256c
ms.sourcegitcommit: 949b87bad28d32df84df190160089f01826f3a31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2020
ms.locfileid: "88194158"
---
<!-- cSpell:ignore vcenter -->

# <a name="migrate-a-devtest-environment-to-azure-devtest-labs"></a>將開發/測試環境遷移至 Azure DevTest Labs

這篇文章示範 Contoso 虛構的公司如何將其開發/測試環境遷移至 Azure DevTest Labs。

## <a name="migration-options"></a>移轉選項

Contoso 將其開發/測試環境移至 Azure 時，有數個可用的選項。

| 移轉選項 | 結果 |
| --- | --- |
| [Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-services-overview) | [評估](https://docs.microsoft.com/azure/migrate/tutorial-assess-vmware) 和 [遷移](https://docs.microsoft.com/azure/migrate/tutorial-migrate-vmware) 內部部署 vm。 <br><br> 使用 Azure 基礎結構即服務 (IaaS) 來執行開發/測試伺服器。 <br><br> 使用 [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)管理 vm。 |
| [DevTest Labs](https://docs.microsoft.com/azure/devtest-labs/devtest-lab-overview) | 快速布建開發和測試環境。 <br><br> 使用配額和原則將浪費降至最低。 <br><br> 設定自動關機以將成本降至最低。 <br><br> 建立 Windows 和 Linux 環境。 |

> [!NOTE]
> 本文著重于使用 DevTest Labs 將內部部署開發/測試環境移至 Azure。 閱讀 Contoso 如何透過 Azure Migrate [將開發/測試移至 Azure IaaS](./contoso-migration-devtest-to-iaas.md) 。

## <a name="business-drivers"></a>商業動機

開發領導小組已概述它想要透過此遷移達成的目標：

- 讓開發人員能夠存取 DevOps 工具和自助環境。
- 提供 DevOps 工具的存取權，以進行持續整合/持續傳遞 (CI/CD) 管線，以及適用于開發/測試的雲端原生工具，例如 AI、機器學習服務和無伺服器。
- 確保開發/測試環境中的治理和合規性。
- 將所有開發/測試環境移出資料中心，而不再購買硬體以開發軟體，以節省成本。

> [!NOTE]
> Contoso 會使用適用于其環境的 [隨用隨付開發/測試訂](https://azure.microsoft.com/offers/ms-azr-0023p) 用帳戶供應專案。 小組中的每個 active Visual Studio 訂閱者都可以使用 Azure 虛擬機器上的訂用帳戶隨附的 Microsoft 軟體進行開發/測試，而不需要額外付費。 Contoso 只需支付所執行 Vm 的 Linux 費率。 其中包括具有 SQL Server、SharePoint Server 或其他軟體（通常以較高的費率計費）的 Vm。

<!-- -->

> [!NOTE]
> 具有 Enterprise 合約的 azure 客戶也可以受益于 [Azure 開發/測試訂](https://azure.microsoft.com/offers/ms-azr-0148p)用帳戶供應專案。 若要深入瞭解，請參閱使用 Enterprise 合約入口網站建立 Azure 開發/測試訂用帳戶的 [這段影片](https://channel9.msdn.com/blogs/ea.azure.com/enabling-and-creating-ea-devtest-subscriptions-through-the-ea-portal) 。

## <a name="migration-goals"></a>移轉目標

Contoso 開發小組已將此遷移的目標固定在一起。 這些目標是用來判斷最合適的移轉方法：

- 快速布建開發和測試環境。 建立開發人員撰寫或測試軟體所需的基礎結構需要幾分鐘的時間，而不是數個月。
- 在遷移之後，Contoso 在 Azure 中的開發/測試環境應該具備在目前內部部署系統上的增強功能。
- 作業模型會從已布建的 Vm 移到 DevOps，並提供自助式布建。
- Contoso 想要快速移出其內部部署開發/測試環境。
- 所有開發人員都會從遠端和安全地連接到開發/測試環境。

## <a name="solution-design"></a>解決方案設計

在將目標和需求固定之後，Contoso 會設計和審查部署解決方案。 解決方案包含將用於開發/測試的 Azure 服務。

### <a name="current-architecture"></a>目前架構

- Contoso 應用程式的開發/測試 Vm 正在內部部署資料中心的 VMware 上執行。
- 在程式碼升級至生產 Vm 之前，會使用這些 Vm 進行開發和測試。
- 開發人員會維護自己的工作站，但他們需要新的解決方案，以便從家庭辦公室遠端連線。

### <a name="proposed-architecture"></a>建議的架構

- Contoso 會使用 [Azure 開發/測試訂](https://azure.microsoft.com/offers/ms-azr-0023p) 用帳戶來降低 azure 資源的成本。 此訂用帳戶提供可觀的節約，包括不會產生 Microsoft 軟體授權費用的 Vm。
- Contoso 會使用 DevTest Labs 來管理環境。 系統會在 DevTest Labs 中建立新的 Vm，以支援移至新工具以在雲端中進行開發和測試。
- 在完成遷移之後，會解除委任 Contoso 資料中心內的內部部署開發/測試 Vm。
- 開發人員和測試人員可以存取其工作站的 Windows 虛擬桌面。

![案例架構的圖表。](./media/contoso-migration-devtest-to-labs/architecture.png)

_圖1：案例架構。_

### <a name="database-considerations"></a>資料庫考量

為了支援持續的開發，Contoso 已決定繼續使用在 Vm 上執行的資料庫。 但目前的虛擬機器將會取代為在 DevTest Labs 中執行的新 Vm。 在未來，Contoso 會採用平臺即服務 (PaaS) 服務，例如 [Azure SQL Database](https://docs.microsoft.com/azure/azure-sql/database/sql-database-paas-overview) 和 [適用於 MySQL 的 Azure 資料庫](https://docs.microsoft.com/azure/mysql/overview)。

目前的 VMware 資料庫 Vm 將會解除委任，並取代為 DevTest Labs 中的 Azure Vm。 將會使用簡單的備份和還原來遷移現有的資料庫。 使用 Azure 開發/測試訂用帳戶供應專案不會產生 Windows Server 和 SQL Server 實例的授權費用，因而將計算成本降至最低。

### <a name="solution-review"></a>解決方案檢閱

Contoso 會藉由將優缺點清單放在一起，來評估提議的設計。

| 考量 | 詳細資料 |
| --- | --- |
| **優點** | 所有目前的開發 Vm (應用程式和資料庫) 都會由 DevTest Labs 中執行的新 Vm 所取代。 這表示它們可以利用專為雲端開發環境提供的功能。 <br><br> Contoso 可以利用其在 Azure 開發/測試訂用帳戶中的投資，以節省授權費用。 <br><br> Contoso 會保留 Azure 中應用程式 Vm 的完整控制權。 <br><br> 開發人員將會提供訂用帳戶的許可權，讓他們能夠建立新的資源，而不需要等候它回應其要求。 |
| **缺點** | 遷移只會將開發工作移至雲端。 開發人員在開發時不會使用 PaaS 服務，因為它們仍在使用 Vm。 這表示 Contoso 必須開始支援其 Vm 的作業，包括安全性修補程式。 它會維護過去的 Vm，而 Contoso 必須尋找此新作業工作的解決方案。 <br><br> Contoso 必須建立新的應用程式和資料庫 Vm，以自動化該流程。 這表示它可以利用在雲端中建立 Vm 和 DevTest Labs 所提供的工具。 這是正面的結果，甚至是在清單上。 |

### <a name="migration-process"></a>移轉程序

Contoso 會使用 DevTest Labs，將其開發應用程式和資料庫 Vm 遷移至新的 Azure Vm。

- Contoso 已準備好 [Azure 基礎結構](./contoso-migration-infrastructure.md) ，包括開發虛擬網路。
- 完成所有準備工作之後，Contoso 會布建和設定 DevTest Labs。
- Contoso 會設定開發虛擬網路、指派資源群組，以及設定原則。
- Contoso 會建立 Windows 虛擬桌面，供開發人員在遠端位置使用。
- Contoso 會在 DevTest Labs 中建立用於開發和遷移資料庫的 Vm。

![說明遷移程式的圖表。](./media/contoso-migration-devtest-to-labs/migration-process-devtest-labs.png)

_圖2：遷移程式。_

## <a name="prerequisites"></a>必要條件

以下是 Contoso 要執行此案例所需的項目。

| 需求 | 詳細資料 |
| --- | --- |
| **Azure 開發/測試訂用帳戶** | Contoso 會建立 [Azure 開發/測試訂](https://azure.microsoft.com/offers/ms-azr-0023p) 用帳戶，以降低成本達80%。 <br><br> 如果您沒有 Azure 訂用帳戶，請建立[免費帳戶](https://azure.microsoft.com/free)。 <br><br> 如果您建立免費帳戶，您就是訂閱的系統管理員，而且可以執行所有動作。 <br><br> 如果您使用現有的訂用帳戶，而且您不是系統管理員，請與系統管理員合作，為您指派擁有者或參與者許可權。 <br><br> 如果您需要更細微的權限，請檢閱[此文章](https://docs.microsoft.com/azure/site-recovery/site-recovery-role-based-linked-access-control)。 |
| **Azure 基礎結構** | [了解](./contoso-migration-infrastructure.md) Contoso 如何設定 Azure 基礎結構。 |

## <a name="scenario-steps"></a>案例步驟

以下是 Contoso 管理員執行移轉的方式：

> [!div class="checklist"]
>
> - 步驟1：布建新的 Azure 開發/測試訂用帳戶，並建立 DevTest Labs 實例。
> - 步驟2：設定開發虛擬網路、指派資源群組，以及設定原則。
> - 步驟3：建立適用于開發人員的 Windows 10 企業版多會話虛擬桌面，以從遠端位置使用。
> - 步驟4：在 DevTest Labs 中建立用於開發和遷移資料庫的公式和 Vm。

## <a name="step-1-provision-a-new-azure-devtest-subscription-and-create-a-devtest-labs-instance"></a>步驟1：布建新的 Azure 開發/測試訂用帳戶，並建立 DevTest Labs 實例

Contoso 管理員必須先使用 Azure 開發/測試供應專案來布建新的訂用帳戶，然後建立 DevTest Labs 實例。

他們依照下列方式進行其設定：

系統管理員會遵循 [Azure 開發/測試訂](https://azure.microsoft.com/offers/ms-azr-0023p) 用帳戶供應專案的連結，並布建新的訂用帳戶，將其儲存到最多80% 的系統上。 此供應專案可讓他們在 Azure 上執行 Windows 10 映射以進行開發/測試。 他們將取得 [Windows 虛擬桌面](https://docs.microsoft.com/azure/virtual-desktop/overview) 的存取權，以簡化遠端開發人員的管理體驗。

![隨用隨付開發/測試供應專案的螢幕擷取畫面，包含 [啟動] 按鈕。](./media/contoso-migration-devtest-to-labs/devtest-subscription.png)

_圖3： Azure 開發/測試訂用帳戶供應專案。_

在布建新的訂用帳戶之後，Contoso 管理員會使用 Azure 入口網站建立新的 DevTest Labs 實例。 新的實驗室會建立在 `ContosoDevRG` 資源群組中。

![入口網站上 DevTest Labs 的 [建立] 按鈕的螢幕擷取畫面。](./media/contoso-migration-devtest-to-labs/new-lab.png)

_圖4：建立新的 DevTest Labs 實例。_

## <a name="step-2-configure-the-development-virtual-network-assign-a-resource-group-and-set-policies"></a>步驟2：設定開發虛擬網路、指派資源群組，以及設定原則

建立 DevTest Labs 實例之後，Contoso 會執行下列設定：

1. 設定虛擬網路：

   1. 在入口網站中，Contoso 會開啟 [DevTest Labs] 實例，並選取 [設定 **和原則**]。

      ![[設定和原則] ContosoDevTestLabs 的螢幕擷取畫面。](./media/contoso-migration-devtest-to-labs/configure-lab.png)

      _圖5： DevTest Labs 實例：設定和原則。_

   2. Contoso 會選取 [**虛擬網路**] [  >  **新增**]，選擇 [ **vnet-開發人員-eus2**]，然後選取 [**儲存**]。 這可讓開發虛擬網路用於 VM 部署。 在部署 DevTest Labs 實例期間，也會建立虛擬網路。

      ![新增虛擬網路之選取專案的螢幕擷取畫面。](./media/contoso-migration-devtest-to-labs/vnets.png)

      _圖6：虛擬網路。_

2. 指派資源群組：

    - 為了確保資源會部署到 `ContosoDevRG` 資源群組，Contoso 會在實驗室設定中進行這項設定。 它也會將 **參與者** 角色指派給其開發人員。

      ![指派資源群組之選取專案的螢幕擷取畫面。](./media/contoso-migration-devtest-to-labs/assign-resource-group.png)

      _圖7：指派資源群組。_

    > [!NOTE]
    > 「參與者」角色是具有擁有權限的系統管理員層級角色，但無法提供存取權給其他使用者。 深入瞭解 [Azure 角色型存取控制](https://docs.microsoft.com/azure/role-based-access-control/overview)。

3. 設定實驗室原則：

    1. Contoso 必須確保其開發人員在小組原則中使用 DevTest Labs。 Contoso 會使用這些原則來設定 DevTest Labs。

    1. Contoso 會在當地時間下午7:00:00 和正確的時區啟用自動關機。

       ![設定自動關機之選項的螢幕擷取畫面。](./media/contoso-migration-devtest-to-labs/autoshutdown.png)

       _圖8：自動關閉。_

    1. 當開發人員上線時，Contoso 可以讓自動啟動執行中的 Vm。 它們會設定為當地時區，以及開發人員工作的一周天數。

       ![設定自動啟動之選取專案的螢幕擷取畫面。](./media/contoso-migration-devtest-to-labs/autostart.png)

       _圖9：自動啟動。_

    1. Contoso 會設定允許的 VM 大小，以確保無法啟動大型且昂貴的 Vm。

       ![設定允許的 VM 大小之選項的螢幕擷取畫面。](./media/contoso-migration-devtest-to-labs/vm-sizes.png)

       _圖10：允許的 VM 大小。_

    1. Contoso 會設定支援訊息。

       ![設定支援訊息之選項的螢幕擷取畫面。](./media/contoso-migration-devtest-to-labs/support.png)

       _圖11：支援訊息。_

## <a name="step-3-create-windows-10-enterprise-multi-session-virtual-desktops-for-developers-to-use-from-remote-locations"></a>步驟3：建立適用于開發人員的 Windows 10 企業版多會話虛擬桌面以從遠端位置使用

Contoso 必須為遠端開發人員建立 Windows 虛擬桌面。

1. Contoso 會選取 [**所有虛擬機器**]  >  ，然後**新增**並選擇 VM 的 Windows 10 企業版多會話基礎。

   ![顯示選取 Windows 10 基礎的螢幕擷取畫面](./media/contoso-migration-devtest-to-labs/windows-10-vm-base.png)

   _圖12： Windows 10 企業版多會話基礎。_

1. Contoso 會設定 VM 的大小以及要安裝的構件。 在此情況下，開發人員可以存取常見的開發工具，例如 Visual Studio Code、Git 和 Chocolatey。

   ![顯示所選成品的螢幕擷取畫面。](./media/contoso-migration-devtest-to-labs/artifacts.png)

   _圖13：構件。_

1. Contoso 會檢查 VM 設定的正確性。

   ![顯示從基底建立虛擬機器之選項的螢幕擷取畫面。](./media/contoso-migration-devtest-to-labs/vm-from-base.png)

   _圖14：從基底建立虛擬機器。_

1. 建立 VM 之後，Contoso 的遠端開發人員就可以連接到此開發工作站，並將其用於其工作。 已安裝選取的成品，讓開發人員在設定其工作站時，節省時間。

   ![顯示 RemoteDevs 虛擬機器相關資訊的螢幕擷取畫面。](./media/contoso-migration-devtest-to-labs/remote-vm.png)

   _圖15：遠端開發人員 VM。_

## <a name="step-4-create-formulas-and-vms-within-devtest-labs-for-development-and-migrate-databases"></a>步驟4：在 DevTest Labs 中建立用於開發和遷移資料庫的公式和 Vm

當 DevTest Labs 已設定且遠端開發人員的工作站啟動並執行時，Contoso 著重于建立其 Vm 以供開發之用。 若要開始使用，Contoso 會完成下列步驟：

1. Contoso 會為應用程式和資料庫 Vm 建立可重複使用的基底) 公式 (，並使用公式來布建應用程式和資料庫 Vm。

   Contoso 會選取 [**公式**] [  >  **+ 新增**]，然後選取 [ **Windows Server 2012 R2 Datacenter** base]。

   ![顯示 Windows 2012 R2 基底選取範圍的螢幕擷取畫面。](./media/contoso-migration-devtest-to-labs/windows-2012-base.png)

   _圖16： Windows 2012 R2 的基礎。_

1. Contoso 會設定 VM 的大小以及要安裝的構件。 在此情況下，開發人員可以存取常見的開發工具，例如 Visual Studio Code、Git 和 Chocolatey。

   ![顯示 Windows 2012 R2 基本設定之所選 VM 大小和構件的螢幕擷取畫面。](./media/contoso-migration-devtest-to-labs/windows-2012-base-configuration.png)

   _圖17： Windows 2012 R2 基本設定。_

1. 為了建立資料庫 VM 公式，Contoso 會遵循相同的基本步驟。 這次，它會選取基底的 SQL Server 2012 映射。

   ![顯示 SQL Server 2012 R2 基底之選取專案的螢幕擷取畫面。](./media/contoso-migration-devtest-to-labs/sql-2012-base.png)
  
   _圖18： SQL Server 2012 映射。_

1. Contoso 會以大小和成品來設定公式。 成品包括 SQL Server Management Studio，這是此資料庫開發 VM 公式的必要專案。

   ![顯示 SQL 2012 R2 基本設定的螢幕擷取畫面。](./media/contoso-migration-devtest-to-labs/sql-2012-base-configuration.png)

   _圖19： SQL 2020 R2 基本設定。_

   深入瞭解如何搭配使用 [公式](https://docs.microsoft.com/azure/lab-services/devtest-lab-manage-formulas) 與 DevTest Labs。

1. Contoso 現在已建立 Windows 基底公式，供其開發人員用於應用程式和資料庫。

   ![顯示已設定之資料庫 VM 的螢幕擷取畫面。](./media/contoso-migration-devtest-to-labs/contoso-formulas.png)

   _圖20： Windows 基底公式。_

接下來的步驟會透過公式來布建應用程式和資料庫 Vm：

1. 建立公式之後，Contoso 接著會選取 [ **所有虛擬機器** ] 和 [ **Windows2012AppDevVmBase** ] 公式，以符合其目前應用程式開發 vm 的設定。

   ![顯示應用程式 VM 選擇作為基底的螢幕擷取畫面。](./media/contoso-migration-devtest-to-labs/app-vm.png)

   _圖21：應用程式開發 VM。_

1. Contoso 會使用此應用程式 VM 所需的大小和構件來設定 VM。

   ![顯示應用程式 VM 大小和成品選取專案的螢幕擷取畫面。](./media/contoso-migration-devtest-to-labs/app-vm-configuration.png)

   _圖22： VM 的大小和成品設定。_

1. Contoso 會使用 **SQLDbDevVmBase** 公式來布建資料庫 VM，以符合其目前資料庫開發 vm 的設定。

   ![顯示資料庫 VM 布建的螢幕擷取畫面。](./media/contoso-migration-devtest-to-labs/database-vm.png)

   _圖23：資料庫 VM。_

1. Contoso 會以所需的大小和成品來設定 VM。

   ![顯示資料庫 VM 之大小和成品選取專案的螢幕擷取畫面。](./media/contoso-migration-devtest-to-labs/database-vm-config.png)

   _圖24： VM 的資料庫設定。_

1. 隨著遠端開發人員的工作站一起建立的第一部 Vm，Contoso 的開發人員已準備好開始在 Azure 中撰寫程式碼。

   ![顯示 Contoso 虛擬機器的螢幕擷取畫面。](./media/contoso-migration-devtest-to-labs/contoso-vms.png)

   _圖25： Contoso Vm。_

1. Contoso 現在可以從備份還原其開發資料庫，或使用某種類型的程式碼產生程式來建立 Vm 上的架構。 透過成品已安裝 SQL Server Management Studio，這些是不需要安裝任何工具的簡單工作。

## <a name="clean-up-after-migration"></a>移轉之後進行清除

Contoso 會繼續使用這些步驟，使用 DevTest Labs 將其 Vm 遷移至 Azure。 每次遷移完成時，所有開發 Vm 現在都是在 DevTest Labs 中執行。

現在，Contoso 必須完成以下的清除步驟：

- 從 vCenter 清查中移除 Vm。
- 從本機備份作業中移除所有 Vm。
- 更新內部檔以顯示 Vm 的新位置和 IP 位址。
- 檢閱與 VM 互動的任何資源，並更新任何相關的設定或文件，以反映新的組態。

### <a name="security"></a>安全性

Contoso 安全性小組會檢查 Azure Vm，以判斷是否有任何安全性問題。 若要控制存取權，小組會檢閱 VM 的網路安全性群組 (NSG)。 Nsg 是用來確保只有應用程式允許的流量可以到達它。 小組也會考慮使用 Azure 磁碟加密和 Azure Key Vault 來保護磁片上的資料。 如需詳細資訊，請參閱 [Azure 中 IaaS 工作負載的安全性最佳作法](https://docs.microsoft.com/azure/security/fundamentals/iaas)。

### <a name="licensing-and-cost-optimization"></a>授權和成本最佳化

- Contoso 會確保所有的開發 Azure 資源都是透過此開發/測試訂用帳戶建立，以利用節省的80%。
- 系統會針對 Vm 的所有 DevTest Labs 實例和原則審查預算，以確保包含成本，且過度布建不會錯誤地發生。
- Contoso 會啟用 [Azure 成本管理和計費](https://docs.microsoft.com/azure/cost-management-billing/cost-management-billing-overview) ，以協助監視和管理 Azure 資源。

## <a name="conclusion"></a>結論

在本文中，Contoso 已將其開發環境移至 DevTest Labs。 它也會實作為遠端和合約開發人員的平臺 Windows 虛擬桌面。

**需要其他協助嗎？**

立即在您的訂用帳戶中[建立 DevTest labs 實例](https://docs.microsoft.com/azure/lab-services/devtest-lab-create-lab)，並瞭解如何使用[適用于開發人員的 DevTest labs](https://docs.microsoft.com/azure/lab-services/devtest-lab-developer-lab)。
