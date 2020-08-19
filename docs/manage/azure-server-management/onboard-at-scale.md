---
title: 設定訂用帳戶的服務
description: 瞭解如何將服務代理程式部署至您的伺服器並啟用管理解決方案，以設定訂用帳戶的 Azure 伺服器管理服務。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 5eeb8bb94ff0de3c1d684bdfd7523657678f9d4b
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88572063"
---
<!-- cSpell:ignore VMUUID kusto -->

# <a name="configure-azure-server-management-services-at-scale"></a>大規模設定 Azure 伺服器管理服務

您必須完成這兩項工作，才能將 Azure 伺服器管理服務上架到您的伺服器：

- 將服務代理程式部署到您的伺服器。
- 啟用管理解決方案。

本文涵蓋完成這些工作所需的三個程式：

1. 使用 Azure 原則將必要的代理程式部署至 Azure Vm。
1. 將必要的代理程式部署到內部部署伺服器。
1. 啟用及設定解決方案。

> [!NOTE]
> 在您將虛擬機器上架到 Azure 伺服器管理服務之前，請先建立必要的 [Log Analytics 工作區和 Azure 自動化帳戶](./prerequisites.md#create-a-workspace-and-automation-account) 。

## <a name="use-azure-policy-to-deploy-extensions-to-azure-vms"></a>使用 Azure 原則將擴充功能部署至 Azure Vm

[Azure 管理工具和服務](./tools-services.md)中討論的所有管理解決方案都需要在 azure 虛擬機器和內部部署伺服器上安裝 Log Analytics 代理程式。 您可以使用 Azure 原則來大規模上架 Azure Vm。 指派原則，以確保代理程式已安裝在您的 Azure Vm 上，並已連線到正確的 Log Analytics 工作區。

Azure 原則具有 [內建原則方案](/azure/governance/policy/concepts/definition-structure#initiatives) ，其中包含適用於 VM 的 Azure 監視器所需的 Log Analytics 代理程式和 [Microsoft Dependency Agent](/azure/azure-monitor/insights/vminsights-onboard#the-microsoft-dependency-agent)。

<!-- TODOBACKLOG: Add these when available.
**Preview:** Enable Azure Monitor for virtual machine scale sets.
**Preview:** Enable Azure Monitor for VMs.
 -->

> [!NOTE]
> 如需有關各種 Azure 監視代理程式的詳細資訊，請參閱 [azure 監視代理](/azure/azure-monitor/platform/agents-overview)程式的總覽。

### <a name="assign-policies"></a>指派原則

若要指派上一節中所述的原則：

1. 在 Azure 入口網站中，移至 [**原則**  >  **指派**  >  **指派方案**]。

    ![入口網站的原則介面螢幕擷取畫面](./media/onboarding-at-scale1.png)

2. 在 [ **指派原則** ] 頁面上，選取省略號 ( ... ) 然後選取管理群組或訂用帳戶，以設定 **範圍** 。 選擇性地選取資源群組。 然後選擇 [**範圍**] 頁面底部的 [**選取**]。 範圍會決定原則指派給哪些資源或資源群組。

3. 選取 [**原則定義**] 旁的省略號 (**...**) 以開啟可用定義的清單。 若要篩選計畫定義，請在 [**搜尋**] 方塊中輸入**Azure 監視器**：

    ![入口網站的原則介面螢幕擷取畫面](./media/onboarding-at-scale2.png)

4. **指派名稱**會自動填入您選取的原則名稱，但您可以加以變更。 您也可以新增選擇性的描述，以提供有關此原則指派的詳細資訊。 [ **指派者** ] 欄位會自動填入已登入的使用者。 此為選擇性欄位，而且支援自訂值。

5. 針對此原則，請選取要與 Log analytics 代理程式建立關聯的 **Log analytics 工作區** 。

    ![入口網站的原則介面螢幕擷取畫面](./media/onboarding-at-scale3.png)

6. 選取 [ **受控識別位置** ] 核取方塊。 如果此原則的類型為 [`DeployIfNotExists`](/azure/governance/policy/concepts/effects#deployifnotexists) ，則需要受控識別才能部署原則。 在入口網站中，將會依照核取方塊選取專案的指示建立帳戶。

7. 選取 [指派]。

完成嚮導之後，原則指派將會部署至環境。 原則可能需要30分鐘的時間才會生效。 若要進行測試，請在30分鐘之後建立新的 Vm，並根據預設，檢查 VM 上的 Microsoft Monitoring Agent 是否已啟用。

## <a name="install-agents-on-on-premises-servers"></a>在內部部署伺服器上安裝代理程式

> [!NOTE]
> 請先建立必要的 [Log Analytics 工作區和 Azure 自動化帳戶](./prerequisites.md#create-a-workspace-and-automation-account) ，再將 Azure 伺服器管理服務上架到伺服器。

針對內部部署伺服器，您必須手動下載並安裝 [Log Analytics 代理程式和 Microsoft Dependency Agent](/azure/azure-monitor/insights/vminsights-enable-hybrid-cloud) ，並將其設定為連線至正確的工作區。 您必須指定工作區識別碼和金鑰資訊。 若要取得該資訊，請移至 Azure 入口網站中的 Log Analytics 工作區，然後選取 [**設定**  >  **Advanced settings**]。

![Azure 入口網站中 Log Analytics 工作區 advanced 設定的螢幕擷取畫面](./media/onboarding-on-premises.png)

## <a name="enable-and-configure-solutions"></a>啟用及設定解決方案

若要啟用解決方案，您必須設定 Log Analytics 工作區。 上線 Azure Vm 和內部部署伺服器將會從其所連接的 Log Analytics 工作區取得解決方案。

### <a name="update-management"></a>更新管理

更新管理、變更追蹤和清查解決方案都需要 Log Analytics 工作區和自動化帳戶。 為確保這些資源已正確設定，建議您在自動化帳戶上架。 如需詳細資訊，請參閱 [更新管理、變更追蹤和清查解決方案上架](/azure/automation/automation-onboard-solutions-from-automation-account)。

建議您為所有伺服器啟用更新管理的解決方案。 適用于 Azure Vm 和內部部署伺服器的更新管理免費。 如果您透過自動化帳戶啟用更新管理，工作區中就會建立 [範圍](/azure/automation/automation-onboard-solutions-from-automation-account#scope-configuration) 設定。 手動更新範圍，以包含更新管理服務所涵蓋的電腦。

若要涵蓋您現有的伺服器以及未來的伺服器，您需要移除範圍設定。 若要這樣做，請在 Azure 入口網站中查看您的自動化帳戶。 選取**Update Management**[  >  **Manage machine**  >  **在所有可用及未來的機器上**更新管理管理機器]。 此設定可讓連線至工作區的所有 Azure Vm 使用更新管理。

![Azure 入口網站中更新管理的螢幕擷取畫面](./media/onboarding-configuration1.png)

### <a name="change-tracking-and-inventory-solutions"></a>變更追蹤和清查解決方案

若要將變更追蹤和清查解決方案上架，請依照更新管理的相同步驟執行。 如需如何從自動化帳戶將這些解決方案上線的詳細資訊，請參閱 [更新管理、變更追蹤和清查解決方案上架](/azure/automation/automation-onboard-solutions-from-automation-account)。

適用于 Azure Vm 的變更追蹤解決方案免費，對於內部部署伺服器，每月每個節點的成本為 $6。 此成本涵蓋變更追蹤、清查和 Desired State Configuration。 如果您只想要註冊特定的內部部署伺服器，您可以選擇這些伺服器。 建議您將所有的實際執行伺服器上架。

#### <a name="opt-in-via-the-azure-portal"></a>透過 Azure 入口網站加入宣告

1. 移至已啟用變更追蹤和清查的自動化帳戶。
2. 選取 [ **變更追蹤**]。
3. 選取右上方窗格中的 [ **管理機器** ]。
4. 選取 [ **在選取的電腦上啟用**]。 然後選取 [電腦名稱稱] 旁的 [ **新增** ]。
5. 選取 [ **啟用** ] 以啟用這些機器的解決方案。

![Azure 入口網站中變更追蹤的螢幕擷取畫面](./media/onboarding-configuration2.png)

#### <a name="opt-in-by-using-saved-searches"></a>使用已儲存的搜尋來加入宣告

或者，您可以設定範圍設定來選擇內部部署伺服器。 範圍設定會使用已儲存的搜尋。

若要建立或修改已儲存的搜尋，請遵循下列步驟：

1. 移至與您在先前步驟中設定的自動化帳戶連結的 Log Analytics 工作區。

1. 在 [一般]**** 下，選取 [已儲存的搜尋]****。

1. 在 [ **篩選** ] 方塊中，輸入 **變更追蹤** 以篩選已儲存的搜尋清單。 在結果中，選取 [ **>microsoftdefaultcomputergroup**]。

1. 輸入電腦名稱稱或 VMUUID，以包含您想要加入變更追蹤的電腦。

  ```kusto
  Heartbeat
  | where AzureEnvironment=~"Azure" or Computer in~ ("list of the on-premises server names", "server1")
  | distinct Computer
  ```

  > [!NOTE]
  > 伺服器名稱必須與運算式中的值完全相符，而且不應該包含功能變數名稱尾碼。

1. 選取 [儲存]。 根據預設，範圍設定會連結至 **>microsoftdefaultcomputergroup** 儲存的搜尋。 它會自動更新。

### <a name="azure-activity-log"></a>Azure 活動記錄檔

[Azure 活動記錄](/azure/azure-monitor/platform/activity-logs-overview) 也是 Azure 監視器的一部分。 它可讓您深入瞭解 Azure 中發生的訂用帳戶層級事件。

若要執行此解決方案：

1. 在 Azure 入口網站中，開啟 [**所有服務**]，然後選取 [**管理 + 治理**  >  **解決方案**]。
2. 在 [ **方案** ] 視圖中，選取 [ **新增**]。
3. 搜尋 **活動記錄分析** ，然後選取它。
4. 選取 [建立]。

您必須指定您在上一節中為啟用解決方案的工作區所建立的 **工作區名稱** 。

### <a name="azure-log-analytics-agent-health"></a>Azure Log Analytics 代理程式健全狀況

Azure Log Analytics 代理程式健全狀況解決方案會報告您 Windows 和 Linux 伺服器的健康狀態、效能和可用性。

若要執行此解決方案：

1. 在 Azure 入口網站中，開啟 [**所有服務**]，然後選取 [**管理 + 治理**  >  **解決方案**]。
2. 在 [ **方案** ] 視圖中，選取 [ **新增**]。
3. 搜尋 **Azure Log Analytics 代理程式健康** 情況，然後選取它。
4. 選取 [建立]。

您必須指定您在上一節中為啟用解決方案的工作區所建立的 **工作區名稱** 。

建立完成之後，當您選取 [ **View**解決方案] 時，工作區資源實例會顯示**AgentHealthAssessment**  >  ** **。

### <a name="antimalware-assessment"></a>反惡意程式碼評估

反惡意程式碼軟體評定的解決方案可協助您找出受到惡意程式碼感染或更高風險感染的伺服器。

若要執行此解決方案：

1. 在 Azure 入口網站中，開啟 [**所有服務**]，選取 [選取**管理 + 治理**  >  **解決方案**]。
2. 在 [ **方案** ] 視圖中，選取 [ **新增**]。
3. 搜尋，然後選取 [ **反惡意程式碼軟體評定**]。
4. 選取 [建立]。

您必須指定您在上一節中為啟用解決方案的工作區所建立的 **工作區名稱** 。

建立完成之後，當您選取 [ **View**解決方案] 時，工作區資源實例會顯示**反惡意**代碼  >  ** **。

### <a name="azure-monitor-for-vms"></a>適用於 VM 的 Azure 監視器

您可以透過 VM 實例的 [view] 頁面啟用 [適用於 VM 的 Azure 監視器](/azure/azure-monitor/insights/vminsights-overview) ，如在 [單一 VM 上啟用管理服務以進行評估](./onboard-single-vm.md)所述。 您不應該直接從 [ **方案** ] 頁面啟用解決方案，如同本文所述的其他解決方案一樣。 針對大規模部署，使用 [自動化](./onboarding-automation.md) 可在工作區中啟用正確的解決方案可能會比較容易。

### <a name="azure-security-center"></a>Azure 資訊安全中心

建議您至少將所有伺服器上架到 Azure 資訊安全中心的免費層。 此選項可為您的環境提供基本的安全性評定和可採取動作的安全性建議。 標準層提供額外的好處。 如需詳細資訊，請參閱 [Azure 資訊安全中心定價](/azure/security-center/security-center-pricing)。

若要啟用 Azure 資訊安全中心的免費層，請遵循下列步驟：

1. 移至 [ **Security Center** 入口網站] 頁面。
2. 在 [ **原則 & 合規性**] 底下，選取 [ **安全性原則**]。
3. 尋找您在右側窗格中建立的 Log Analytics 工作區資源。
4. 選取該工作區的 [ **編輯設定** ]。
5. 選取 [定價層]  。
6. 選擇 [ **免費** ] 選項。
7. 選取 [儲存]。

## <a name="next-steps"></a>後續步驟

瞭解如何使用自動化來將伺服器上架並建立警示。

> [!div class="nextstepaction"]
> [自動化上架和警示設定](./onboarding-automation.md)
