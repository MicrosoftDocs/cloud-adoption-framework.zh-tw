---
title: 設定訂用帳戶的 Azure 管理服務
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 設定訂用帳戶的 Azure 管理服務
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: a5b1d551f52ae8800e9a29d4c8a92c14965645cc
ms.sourcegitcommit: d19e026d119fbe221a78b10225230da8b9666fe1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2019
ms.locfileid: "71221506"
---
# <a name="configure-azure-management-services-at-scale"></a>大規模設定 Azure 管理服務

將 Azure 管理服務上架至您的伺服器牽涉到兩個工作: 將服務代理程式部署到您的伺服器, 並啟用管理解決方案。 本文涵蓋下列可讓您完成這些工作的程式:

- [使用 Azure 原則將必要的代理程式部署至 Azure Vm](#deploy-extensions-to-azure-vms-using-azure-policy)
- [將必要的代理程式部署到內部部署伺服器](#install-required-agents-on-on-premises-servers)
- [啟用和設定解決方案](#enable-and-configure-solutions)

> [!NOTE]
> 將虛擬機器上架至 Azure 管理服務之前, 請先建立必要的[Log Analytics 工作區和 Azure 自動化帳戶](./prerequisites.md#create-a-workspace-and-automation-account)。

## <a name="deploy-extensions-to-azure-vms-using-azure-policy"></a>使用 Azure 原則將擴充功能部署至 Azure Vm

[Azure 管理工具和服務](./tools-services.md)中所討論的所有管理解決方案, 都需要在 azure 虛擬機器 (vm) 和內部部署伺服器上安裝 Log Analytics 代理程式。 您可以使用 Azure 原則, 大規模上架您的 Azure Vm。 指派原則以確保代理程式已安裝在您所有的 Azure Vm 上, 並聯機至正確的 Log Analytics 工作區。

Azure 原則具有內建的[原則計畫](/azure/governance/policy/index#initiative-definition), 其中包含 Log Analytics 代理程式和[Microsoft Dependency agent](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#the-microsoft-dependency-agent), 這是適用於 VM 的 Azure 監視器所需的。

<!-- TODO: Add these when available.
- [Preview]: Enable Azure Monitor for virtual machine scale sets.
- [Preview]: Enable Azure Monitor for VMs.
 -->

> [!NOTE]
> 如需有關 Azure 監視之各種代理程式的詳細資訊, 請參閱[azure monitoring agent 的總覽](https://docs.microsoft.com/azure/azure-monitor/platform/agents-overview)。

### <a name="assign-policies"></a>指派原則

若要指派上一節中所列的原則:

1. 在 Azure 入口網站中, 移至 [ **Azure 原則** >   > 指派] [**指派方案**]。

    ![入口網站的原則介面螢幕擷取畫面](./media/onboarding-at-scale1.png)

2. 在 [**指派原則**] 頁面上, 按一下省略號 (...) 來選取**範圍**, 然後選取 [管理群組] 或 [訂用帳戶]。 選擇性地選取資源群組。 範圍會決定指派原則的資源或資源群組。 然後選擇 [**範圍**] 頁面底部的 [**選取**]。

3. 選取 [**原則定義**] 旁的省略號 (...), 以開啟可用定義的清單。 您可以在 [**搜尋**] 方塊中輸入**Azure 監視器**, 以篩選計畫定義:

    ![入口網站的原則介面螢幕擷取畫面](./media/onboarding-at-scale2.png)

4. [**指派名稱**] 會自動填入您選取的原則名稱, 但您可以變更它。 您也可以新增選擇性的描述, 以提供有關此原則指派的詳細資訊。 [**指派者**] 欄位會根據已登入的使用者自動填入。 這個欄位是選擇性的, 而且可以輸入自訂值。

5. 針對此原則, 請選取 log analytics**工作區**, 讓 log analytics 代理程式建立關聯。

    ![入口網站的原則介面螢幕擷取畫面](./media/onboarding-at-scale3.png)

6. 檢查**受控識別位置**。 如果此原則為類型[DeployIfNotExists](https://docs.microsoft.com/azure/governance/policy/concepts/effects#deployifnotexists), 則需要受控識別才能部署原則。 在入口網站中, 將會依照核取方塊選取專案的指示來建立帳戶。

7. 選取 [指派]。

完成此嚮導之後, 原則指派將會部署至環境。 最多可能需要30分鐘的時間, 原則才會生效。 您可以在30分鐘後建立新的 Vm 來測試它, 然後檢查預設是否已在 VM 上啟用 Microsoft Monitoring Agent (MMA)。

## <a name="install-required-agents-on-on-premises-servers"></a>在內部部署伺服器上安裝必要的代理程式

> [!NOTE]
> 將伺服器上架到 Azure 管理服務之前, 請先建立必要的[Log Analytics 工作區和 Azure 自動化帳戶](./prerequisites.md#create-a-workspace-and-automation-account)。

針對內部部署伺服器, 您必須手動下載並安裝[Log Analytics 代理程式和 Microsoft Dependency agent](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-hybrid-cloud) , 並將其設定為連接到正確的工作區。 若要這麼做, 請指定工作區識別碼和金鑰資訊, 您可以前往 Azure 入口網站中的 Log Analytics 工作區, 然後選取 [**設定** > ] [**高級設定**] 來尋找。

![Azure 入口網站中 Log Analytics 工作區的 [高級設定] 螢幕擷取畫面](./media/onboarding-on-premises.png)

## <a name="enable-and-configure-solutions"></a>啟用和設定解決方案

若要啟用解決方案, 您必須設定 Log Analytics 工作區。 上架 Azure Vm 和內部部署伺服器將會從所連線的 Log Analytics 工作區取得解決方案。

本節涵蓋下列解決方案:

- [更新管理](#update-management)
- [變更追蹤和清查](#change-tracking-and-inventory-solutions)
- [Azure 活動記錄](#azure-activity-log)
- [Azure Log Analytics 代理程式健全狀況](#azure-log-analytics-agent-health)
- [反惡意程式碼評估](#antimalware-assessment)
- [適用於 VM 的 Azure 監視器](#azure-monitor-for-vms)
- [Azure 資訊安全中心](#azure-security-center)

### <a name="update-management"></a>更新管理

更新管理、變更追蹤和清查解決方案都需要 Log Analytics 工作區和自動化帳戶。 為確保這些資源已正確設定, 建議您透過自動化帳戶進行上架。 如需詳細資訊, 請參閱上[架更新管理、變更追蹤和清查解決方案](https://docs.microsoft.com/azure/automation/automation-onboard-solutions-from-automation-account)。

建議為所有伺服器啟用更新管理解決方案。 更新管理免費提供給 Azure Vm 和內部部署伺服器。 如果您透過自動化帳戶啟用更新管理, 則會在工作區中建立[範圍](https://docs.microsoft.com/azure/automation/automation-onboard-solutions-from-automation-account#scope-configuration)設定。 您必須手動更新範圍, 以包含更新服務所涵蓋的機器。

若要涵蓋所有現有的伺服器, 以及未來的伺服器, 您必須移除範圍設定。 若要這麼做, 請在 Azure 入口網站中查看您的自動化帳戶, 然後選取 **更新管理** > **管理** > **所有可用和未來機器上啟用的**電腦。 啟用此設定可讓連線至工作區的所有 Azure Vm 使用更新管理。

![Azure 入口網站中更新管理的螢幕擷取畫面](./media/onboarding-configuration1.png)

### <a name="change-tracking-and-inventory-solutions"></a>變更追蹤和清查解決方案

若要將變更追蹤和清查解決方案上架, 請遵循與更新管理相同的步驟。 如需有關從您的自動化帳戶將這些解決方案上架的詳細資訊, 請參閱上[架更新管理、變更追蹤和清查解決方案](https://docs.microsoft.com/azure/automation/automation-onboard-solutions-from-automation-account)。

針對內部部署伺服器, 變更追蹤解決方案可免費用於 Azure Vm, 而每月每節點成本 $6 費用。 這種成本涵蓋變更追蹤、清查和 Desired State Configuration。 如果您只想要註冊特定的內部部署伺服器, 您可以加入宣告這些伺服器。 我們建議您將所有的實際執行伺服器上架。

#### <a name="opt-in-via-the-azure-portal"></a>透過 Azure 入口網站加入宣告

1. 移至已啟用變更追蹤和清查的自動化帳戶。
2. 選取 [**變更追蹤**]。
3. 選取右上方窗格中的 [**管理機器**]。
4. 選取 [**在選取的機器上啟用**]，然後按一下電腦名稱稱旁的 [**新增**]，以選取要啟用的電腦。
5. 選取 [**啟用**] 以啟用這些電腦的解決方案。

![Azure 入口網站中變更追蹤的螢幕擷取畫面](./media/onboarding-configuration2.png)

#### <a name="opt-in-by-using-saved-searches"></a>使用已儲存的搜尋來加入宣告

或者, 您可以將範圍設定設為加入宣告內部部署伺服器。 範圍設定會使用已儲存的搜尋。

若要建立或修改已儲存的搜尋, 請使用下列步驟:

1. 移至與您在先前步驟中設定的自動化帳戶連結的 Log Analytics 工作區。

2. 在 [一般] 下，選取 [已儲存的搜尋]。

3. 在 [**篩選**] 方塊中, 輸入**變更追蹤**來篩選已儲存搜尋的清單。 在結果中, 選取 [ **MicrosoftDefaultComputerGroup**]。

4. 輸入電腦名稱稱或 VMUUID, 以包含您想要加入變更追蹤的電腦。

    ```kusto
    Heartbeat
    | where AzureEnvironment=~"Azure" or Computer in~ ("list of the on-premises server names", "server1")
    | distinct Computer
    ```

    > [!NOTE]
    > 伺服器名稱必須完全符合運算式中包含的值, 而且不應該包含功能變數名稱尾碼。

5. 選取 [儲存]。

6. 根據預設, 範圍設定會連結至**MicrosoftDefaultComputerGroup**儲存的搜尋, 並會自動更新。

### <a name="azure-activity-log"></a>Azure 活動記錄檔

[Azure 活動記錄](https://docs.microsoft.com/azure/azure-monitor/platform/activity-logs-overview)也是 Azure 監視器的一部分。 它可讓您深入瞭解 Azure 中發生的訂用帳戶層級事件。

若要新增此解決方案:

1. 在 [Azure 入口網站] 中, 開啟 [**所有服務**], 然後選取 [**管理 + 治理** > **解決方案**]。
2. 在**解決方案**視圖中, 選取 [**新增**]。
3. 搜尋**活動記錄分析**並加以選取。
4. 選取 [建立]。

您將需要指定您在上一節中已啟用解決方案的工作區的**工作區名稱**。

### <a name="azure-log-analytics-agent-health"></a>Azure Log Analytics 代理程式健全狀況

Azure Log Analytics 代理程式健全狀況解決方案可讓您深入瞭解 Windows 和 Linux 伺服器的健全狀況、效能和可用性。

若要新增此解決方案:

1. 在 [Azure 入口網站] 中, 開啟 [**所有服務**], 然後選取 [**管理 + 治理** > **解決方案**]。
2. 在**解決方案**視圖中, 選取 [**新增**]。
3. 搜尋**Azure Log Analytics 代理程式健全狀況**並加以選取。
4. 選取 [建立]。

您將需要指定您在上一節中已啟用解決方案的工作區的**工作區名稱**。

建立完成後, 當您選取 [ **View**  > **解決方案**] 時, 工作區資源實例會顯示**AgentHealthAssessment** 。

### <a name="antimalware-assessment"></a>反惡意程式碼軟體評量

反惡意程式碼軟體評定解決方案可協助您識別受感染的伺服器, 或是受到惡意程式碼感染的風險。

若要新增此解決方案:

1. 在 [Azure 入口網站] 中, 開啟 [**所有服務**], 然後選取 [**管理 + 治理** > **解決方案**]。
2. 在**解決方案**視圖中, 選取 [**新增**]。
3. 搜尋**反惡意程式碼軟體評定**並加以選取。
4. 選取 [建立]。

您將需要指定您在上一節中已啟用解決方案的工作區的**工作區名稱**。

建立完成後, 當您選取 [ **View**  > **解決方案**] 時, 工作區資源實例會顯示**反惡意**代碼。

### <a name="azure-monitor-for-vms"></a>適用於 VM 的 Azure 監視器

您可以透過 VM 實例的 view 頁來啟用[適用於 VM 的 Azure 監視器](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview), 如前一篇文章: 在[單一 VM 上啟用管理服務以進行評估](./onboard-single-vm.md)中所述。 您不應該直接從 [**解決方案**] 頁面啟用解決方案, 就像您針對本文所述的其他解決方案所做的一樣。 針對大規模部署, 使用[自動化](./onboarding-automation.md)可能會更容易在工作區中啟用正確的解決方案。

### <a name="azure-security-center"></a>Azure 資訊安全中心

在本指南中, 我們建議您預設將所有伺服器上架到 Azure 資訊安全中心的*免費*層。 此選項為您的環境提供基本層級的安全性評量和可操作的安全性建議。 升級至資訊安全中心的*標準*層會提供額外的權益, 詳細說明請見[資訊安全中心定價頁面](https://docs.microsoft.com/azure/security-center/security-center-pricing)。

若要啟用 Azure 資訊安全中心免費層, 請使用下列步驟:

1. 移至**資訊安全中心**入口網站頁面。
2. 選取 [**原則 &AMP; 相容性**] 下的 [**安全性原則**]。
3. 在最右邊的窗格中, 尋找您所建立的 Log Analytics 工作區資源。
4. 選取該工作區的 [**編輯設定] >** 。
5. 選取 [定價層]。
6. 選擇 [**免費**] 選項。
7. 選取 [儲存]。

## <a name="next-steps"></a>後續步驟

瞭解如何使用自動化將伺服器上線並建立警示。

> [!div class="nextstepaction"]
> [自動上線和警示設定](./onboarding-automation.md)
