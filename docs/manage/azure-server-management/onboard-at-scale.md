---
title: 設定訂用帳戶的服務
description: 瞭解如何將服務代理程式部署到您的伺服器並啟用管理解決方案，以設定訂用帳戶的 Azure 伺服器管理服務。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 243bafcfc7033ff932fc8112255a8f6a4a1d2904
ms.sourcegitcommit: 070e6a60f05519705828fcc9c5770c3f9f986de5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83815322"
---
<!-- cSpell:ignore VMUUID kusto -->

# <a name="configure-azure-server-management-services-at-scale"></a>大規模設定 Azure 伺服器管理服務

您必須完成這兩項工作，才能將 Azure 伺服器管理服務上架到您的伺服器：

- 將服務代理程式部署到您的伺服器。
- 啟用管理解決方案。

本文涵蓋完成這些工作所需的三個程式：

1. 使用 Azure 原則，將必要的代理程式部署至 Azure Vm。
1. 將必要的代理程式部署到內部部署伺服器。
1. 啟用和設定解決方案。

> [!NOTE]
> 將虛擬機器上架至 Azure 伺服器管理服務之前，請先建立必要的[Log Analytics 工作區和 Azure 自動化帳戶](./prerequisites.md#create-a-workspace-and-automation-account)。

## <a name="use-azure-policy-to-deploy-extensions-to-azure-vms"></a>使用 Azure 原則將擴充功能部署至 Azure Vm

[Azure 管理工具和服務](./tools-services.md)中所討論的所有管理解決方案，都需要在 azure 虛擬機器和內部部署伺服器上安裝 Log Analytics 代理程式。 您可以使用 Azure 原則，大規模上架您的 Azure Vm。 指派原則以確保代理程式已安裝在您的 Azure Vm 上，並聯機至正確的 Log Analytics 工作區。

Azure 原則具有[內建的原則方案](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#initiatives)，其中包含 Log Analytics 代理程式和[Microsoft Dependency agent](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#the-microsoft-dependency-agent)，這是適用於 VM 的 Azure 監視器所需的。

<!-- TODOBACKLOG: Add these when available.
**Preview:** Enable Azure Monitor for virtual machine scale sets.
**Preview:** Enable Azure Monitor for VMs.
 -->

> [!NOTE]
> 如需有關 Azure 監視之各種代理程式的詳細資訊，請參閱[azure monitoring agent 的總覽](https://docs.microsoft.com/azure/azure-monitor/platform/agents-overview)。

### <a name="assign-policies"></a>指派原則

若要指派上一節中所述的原則：

1. 在 Azure 入口網站中，移至 [ **Azure 原則**指派] [  >  **Assignments**  >  **指派方案**]。

    ![入口網站的原則介面螢幕擷取畫面](./media/onboarding-at-scale1.png)

2. 在 [**指派原則**] 頁面上，選取省略號（...），然後選取 [管理群組] 或 [訂用帳戶]，以設定**範圍**。 選擇性地選取資源群組。 然後選擇 [**範圍**] 頁面底部的 [**選取**]。 範圍會決定指派原則的資源或資源群組。

3. 選取 [**原則定義**] 旁的省略號（**...**），以開啟可用定義的清單。 若要篩選計畫定義，請在 [**搜尋**] 方塊中輸入**Azure 監視器**：

    ![入口網站的原則介面螢幕擷取畫面](./media/onboarding-at-scale2.png)

4. [**指派名稱**] 會自動填入您選取的原則名稱，但您可以變更它。 您也可以新增選擇性的描述，以提供有關此原則指派的詳細資訊。 [**指派者**] 欄位會根據已登入的使用者自動填入。 這個欄位是選擇性的，而且它支援自訂值。

5. 針對此原則，請選取 log analytics**工作區**，讓 log analytics 代理程式建立關聯。

    ![入口網站的原則介面螢幕擷取畫面](./media/onboarding-at-scale3.png)

6. 選取 [**受控識別位置**] 核取方塊。 如果此原則的類型為[DeployIfNotExists](https://docs.microsoft.com/azure/governance/policy/concepts/effects#deployifnotexists)，則必須要有受控識別才能部署原則。 在入口網站中，會依照核取方塊選取專案的指示來建立帳戶。

7. 選取 [**指派**]。

完成嚮導之後，原則指派就會部署到環境。 最多可能需要30分鐘的時間，原則才會生效。 若要進行測試，請在30分鐘後建立新的 Vm，並檢查預設是否已在 VM 上啟用 Microsoft Monitoring Agent。

## <a name="install-agents-on-on-premises-servers"></a>在內部部署伺服器上安裝代理程式

> [!NOTE]
> 將 Azure 伺服器管理服務上架到伺服器之前，請先建立必要的[Log Analytics 工作區和 Azure 自動化帳戶](./prerequisites.md#create-a-workspace-and-automation-account)。

針對內部部署伺服器，您必須手動下載並安裝[Log Analytics 代理程式和 Microsoft Dependency agent](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-hybrid-cloud) ，並設定它們以連線至正確的工作區。 您必須指定工作區識別碼和金鑰資訊。 若要取得該資訊，請移至 Azure 入口網站中的 Log Analytics 工作區，然後選取 [**設定**] [  >  **高級設定**]。

![Azure 入口網站中 Log Analytics 工作區的 [高級設定] 螢幕擷取畫面](./media/onboarding-on-premises.png)

## <a name="enable-and-configure-solutions"></a>啟用和設定解決方案

若要啟用解決方案，您必須設定 Log Analytics 工作區。 上架 Azure Vm 和內部部署伺服器將會從他們所連線的 Log Analytics 工作區取得解決方案。

### <a name="update-management"></a>更新管理

更新管理、變更追蹤和清查解決方案都需要 Log Analytics 工作區和自動化帳戶。 為確保這些資源已正確設定，建議您透過自動化帳戶進行上架。 如需詳細資訊，請參閱上[架更新管理、變更追蹤和清查解決方案](https://docs.microsoft.com/azure/automation/automation-onboard-solutions-from-automation-account)。

我們建議您為所有伺服器啟用更新管理解決方案。 更新管理免費提供給 Azure Vm 和內部部署伺服器。 如果您透過自動化帳戶啟用更新管理，則會在工作區中建立[範圍](https://docs.microsoft.com/azure/automation/automation-onboard-solutions-from-automation-account#scope-configuration)設定。 手動更新範圍，以包含更新管理服務所涵蓋的機器。

若要涵蓋現有的伺服器以及未來的伺服器，您必須移除範圍設定。 若要這麼做，請在 Azure 入口網站中查看您的自動化帳戶。 選取 [**更新管理**  >  **Manage machine**  >  **在所有可用及未來的機器上啟用**管理電腦]。 此設定可讓連線至工作區的所有 Azure Vm 使用更新管理。

![Azure 入口網站中更新管理的螢幕擷取畫面](./media/onboarding-configuration1.png)

### <a name="change-tracking-and-inventory-solutions"></a>變更追蹤和清查解決方案

若要將變更追蹤和清查解決方案上架，請遵循與更新管理相同的步驟。 如需如何將這些解決方案從您的自動化帳戶上架的詳細資訊，請參閱上[架更新管理、變更追蹤和清查解決方案](https://docs.microsoft.com/azure/automation/automation-onboard-solutions-from-automation-account)。

針對內部部署伺服器，變更追蹤解決方案可免費用於 Azure Vm，而每月每節點成本 $6 費用。 這種成本涵蓋變更追蹤、清查和 Desired State Configuration。 如果您只想要註冊特定的內部部署伺服器，您可以加入宣告這些伺服器。 我們建議您將所有的實際執行伺服器上架。

#### <a name="opt-in-via-the-azure-portal"></a>透過 Azure 入口網站加入宣告

1. 移至已啟用變更追蹤和清查的自動化帳戶。
2. 選取 [**變更追蹤**]。
3. 選取右上方窗格中的 [**管理機器**]。
4. 選取 [**在選取的機器上啟用**]。 然後選取 [電腦名稱稱] 旁的 [**新增**]。
5. 選取 [**啟用**] 以啟用這些電腦的解決方案。

![Azure 入口網站中變更追蹤的螢幕擷取畫面](./media/onboarding-configuration2.png)

#### <a name="opt-in-by-using-saved-searches"></a>使用已儲存的搜尋來加入宣告

或者，您可以將範圍設定設為加入宣告內部部署伺服器。 範圍設定會使用已儲存的搜尋。

若要建立或修改已儲存的搜尋，請遵循下列步驟：

1. 移至與您在先前步驟中設定之自動化帳戶連結的 Log Analytics 工作區。

1. 在 [一般]**** 下，選取 [已儲存的搜尋]****。

1. 在 [**篩選**] 方塊中，輸入**變更追蹤**來篩選已儲存搜尋的清單。 在結果中，選取 [ **MicrosoftDefaultComputerGroup**]。

1. 輸入電腦名稱稱或 VMUUID，以包含您想要加入變更追蹤的電腦。

    ```kusto
    Heartbeat
    | where AzureEnvironment=~"Azure" or Computer in~ ("list of the on-premises server names", "server1")
    | distinct Computer
    ```

    > [!NOTE]
    > 伺服器名稱必須完全符合運算式中的值，而且不應該包含功能變數名稱尾碼。

1. 選取 [儲存]。 根據預設，範圍設定會連結至**MicrosoftDefaultComputerGroup**儲存的搜尋。 它會自動更新。

### <a name="azure-activity-log"></a>Azure 活動記錄檔

[Azure 活動記錄](https://docs.microsoft.com/azure/azure-monitor/platform/activity-logs-overview)也是 Azure 監視器的一部分。 它可讓您深入瞭解 Azure 中發生的訂用帳戶層級事件。

若要執行此解決方案：

1. 在 [Azure 入口網站] 中，開啟 [**所有服務**]，然後選取 [**管理 + 治理**  >  **解決方案**]。
2. 在**解決方案**視圖中，選取 [**新增**]。
3. 搜尋**活動記錄分析**並加以選取。
4. 選取 [建立]。

您必須指定您在上一節中已啟用解決方案之工作區的**工作區名稱**。

### <a name="azure-log-analytics-agent-health"></a>Azure Log Analytics 代理程式健全狀況

Azure Log Analytics 代理程式健全狀況解決方案會報告 Windows 和 Linux 伺服器的健全狀況、效能和可用性。

若要執行此解決方案：

1. 在 [Azure 入口網站] 中，開啟 [**所有服務**]，然後選取 [**管理 + 治理**  >  **解決方案**]。
2. 在**解決方案**視圖中，選取 [**新增**]。
3. 搜尋**Azure Log Analytics 代理程式健全狀況**並加以選取。
4. 選取 [建立]。

您必須指定您在上一節中已啟用解決方案之工作區的**工作區名稱**。

建立完成後，當您選取 [ **View**解決方案] 時，工作區資源實例會顯示**AgentHealthAssessment**  >  ** **。

### <a name="antimalware-assessment"></a>反惡意程式碼評估

反惡意程式碼軟體評定解決方案可協助您識別受感染的伺服器，或是受到惡意程式碼感染的風險。

若要執行此解決方案：

1. 在 [Azure 入口網站] 中，開啟 [**所有服務**]，選取 [選取**管理 + 治理**  >  **解決方案**]。
2. 在**解決方案**視圖中，選取 [**新增**]。
3. 搜尋，然後選取 [**反惡意程式碼軟體評定**]。
4. 選取 [建立]。

您必須指定您在上一節中已啟用解決方案之工作區的**工作區名稱**。

建立完成後，當您選取 [ **View**解決方案] 時，工作區資源實例會顯示**反惡意**代碼  >  ** **。

### <a name="azure-monitor-for-vms"></a>適用於 VM 的 Azure 監視器

您可以透過 VM 實例的 view 頁來啟用[適用於 VM 的 Azure 監視器](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)，如在[單一 VM 上啟用管理服務以進行評估](./onboard-single-vm.md)中所述。 您不應該直接從 [**解決方案**] 頁面啟用解決方案，就像您針對本文所述的其他解決方案所做的一樣。 針對大規模部署，使用[自動化](./onboarding-automation.md)可能會更容易在工作區中啟用正確的解決方案。

### <a name="azure-security-center"></a>Azure 資訊安全中心

我們建議您至少將所有伺服器上架到 Azure 資訊安全中心的免費層。 此選項為您的環境提供基本的安全性評量和可操作的安全性建議。 標準層提供額外的好處。 如需詳細資訊，請參閱[Azure 資訊安全中心定價](https://docs.microsoft.com/azure/security-center/security-center-pricing)。

若要啟用 Azure 資訊安全中心的免費層，請遵循下列步驟：

1. 移至**資訊安全中心**入口網站頁面。
2. 在 [**原則 & 相容性**] 底下，選取 [**安全性原則**]。
3. 尋找您在右側窗格中建立的 Log Analytics 工作區資源。
4. 選取該工作區的 [**編輯設定**]。
5. 選取 [定價層]  。
6. 選擇 [**免費**] 選項。
7. 選取 [儲存]  。

## <a name="next-steps"></a>後續步驟

瞭解如何使用自動化將伺服器上線並建立警示。

> [!div class="nextstepaction"]
> [自動上線和警示設定](./onboarding-automation.md)
