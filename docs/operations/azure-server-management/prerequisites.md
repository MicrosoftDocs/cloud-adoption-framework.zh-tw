---
title: Azure 伺服器管理服務的先決條件規劃
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: Azure 伺服器管理服務的先決條件工具和規劃。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: a684880537fa4dc2d7a56e93b9a6e35a94425dae
ms.sourcegitcommit: a26c27ed72ac89198231ec4b11917a20d03bd222
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70819097"
---
# <a name="phase-1-prerequisite-planning-for-azure-server-management-services"></a>第 1 階段：Azure 伺服器管理服務的先決條件規劃

在此階段中, 您將會熟悉 Azure 伺服器管理服務套件, 並規劃如何部署執行這些管理解決方案所需的資源。

## <a name="understand-the-tools-and-services"></a>瞭解工具和服務

請參閱[azure 伺服器管理工具和服務](./tools-services.md), 以深入瞭解進行中 Azure 作業所涉及的管理區域, 以及協助您在這些區域中支援的 azure 服務和工具。 符合您的管理需求, 將會牽涉到使用其中幾個服務。 本指南中經常參考這些工具。

下列各節將討論使用這些工具和服務所需的規劃和準備。

## <a name="log-analytics-workspace-and-automation-account-planning"></a>Log Analytics 工作區和自動化帳戶規劃

您將用來將 Azure 管理服務上架的許多服務都需要 Log Analytics 工作區和連結的 Azure 自動化帳戶。

[Log Analytics 工作區](/azure/azure-monitor/learn/quick-create-workspace)是用來儲存 Azure 監視器記錄資料的唯一環境。 每個工作區都有自己的資料存放庫和設定。 資料來源和解決方案已設定為將其資料儲存在特定工作區中。 Azure 監視解決方案要求所有伺服器都必須連線到工作區, 才能儲存及存取其記錄資料。

某些管理服務需要[Azure 自動化](/azure/automation/automation-intro)帳戶。 藉由使用此帳戶和 Azure 自動化的功能, 您可以整合 Azure 服務和其他公用系統來部署、設定和管理您的伺服器管理進程。

下列 Azure 伺服器管理服務需要連結的 Log Analytics 工作區和自動化帳戶才能運作:

- [Azure 更新管理](/azure/automation/automation-update-management)
- [變更追蹤和清查](/azure/automation/change-tracking)
- [Hybrid Runbook Worker](/azure/automation/automation-hybrid-runbook-worker)
- [Desired State Configuration](/azure/virtual-machines/extensions/dsc-overview)

本指南的第二個階段著重于部署服務和自動化腳本。 其中說明如何建立 Log Analytics 工作區和自動化帳戶。 本指南也會示範如何使用 Azure 原則, 以確保新的 Vm 會連線到正確的工作區。

本指南中所涵蓋的範例假設尚未將伺服器部署到雲端的部署。 若要深入瞭解規劃工作區所涉及的原則和考慮, 請參閱[管理 Azure 監視器中的記錄資料和工作區](/azure/azure-monitor/platform/manage-access)。

## <a name="planning-considerations"></a>規劃考量

準備您為登入管理服務所建立的工作區和帳戶時, 請參閱下列問題討論:

- **Azure 地理位置和法規合規性**。 Azure 區域會組織成*地理*位置。 [Azure 地理位置](https://azure.microsoft.com/global-infrastructure/geographies/)可確保資料存放區、主權、合規性及復原需求會在地理界限內接受。 如果您的工作負載受限於資料主權或其他合規性需求, 則工作區和自動化帳戶必須部署到與它們所支援的工作負載資源位於相同 Azure 地理位置內的區域。
- **工作區數目**。 做為指導原則, 請建立每個 Azure 地理位置所需的最小工作區數目。 針對您的計算或儲存體資源所在的每個 Azure 地理位置, 我們建議至少有一個工作區。 這種初始對齊方式有助於避免未來將資料移轉到不同地理位置時的法規問題。
- **資料保留和上限**。 建立工作區或自動化帳戶時, 您可能也需要考慮資料保留原則或資料上限需求。 如需這些原則的詳細資訊, 以及規劃工作區時的其他考慮, 請參閱[管理 Azure 監視器中的記錄資料和工作區](/azure/azure-monitor/platform/manage-access)。
- **區域對應**。 只有在某些 Azure 區域之間才支援連結 Log Analytics 工作區和 Azure 自動化帳戶。 例如, 如果 Log Analytics 工作區裝載于*EastUS*區域中, 則必須在*EastUS2*區域中建立連結的自動化帳戶, 才能與管理服務搭配使用。 如果您有在另一個區域中建立的自動化帳戶, 它將無法連結至*EastUS*中的工作區。 選擇部署區域可能會大幅影響 Azure 地理位置需求。 請參閱[區域對應表](/azure/automation/how-to/region-mappings), 以決定哪個區域應裝載您的工作區和自動化帳戶。
- **工作區**多路連接。 Log Analytics 代理程式在某些情況下支援多路連接, 但是在此設定中執行時, 代理程式會面臨數個限制和問題。 除非 Microsoft 建議在您的案例中使用多路連接, 否則我們不建議您在 Log Analytics 代理程式上設定多宿主。

## <a name="resource-placement-examples"></a>資源位置範例

有數個不同的模型可供您選擇用來放置 Log Analytics 工作區和自動化帳戶的訂用帳戶。 簡言之, 您應該將工作區和自動化帳戶放在負責執行更新管理和變更追蹤和清查服務的小組所擁有的訂用帳戶中。

下列範例說明可部署工作區和自動化帳戶的一些方式。

### <a name="placement-by-geography"></a>依地理位置放置

針對具有單一訂用帳戶的小型和中型環境, 以及跨越多個 Azure 地理位置的數百個資源, 請在每個地理區域中建立一個 Log Analytics 工作區和一個 Azure 自動化帳戶。

您可以在每個資源群組中建立工作區和 Azure 自動化帳戶 (一對), 並將對應地理位置中的配對部署到虛擬機器。 或者, 如果您的資料合規性原則不會規定資源位於特定區域, 您可以建立一個配對來管理所有虛擬機器。 我們也建議您將工作區和自動化帳戶配對放在不同的資源群組中, 以提供更細微的角色型存取控制 (RBAC)。

下圖中的範例有一個訂用帳戶具有兩個資源群組, 分別位於不同的地理位置。

![適用于中小型環境的工作區模型](./media/workspace-model-small.png)

### <a name="placement-in-a-management-subscription"></a>在管理訂用帳戶中放置

對於跨越多個訂用帳戶且擁有具有監視和合規性之中央 IT 部門的大型環境, 請在 IT 管理訂用帳戶中建立配對的工作區和自動化帳戶。 在此模型中, 地理位置中的虛擬機器資源會將其資料儲存在 IT 管理訂用帳戶的對應地理工作區中。 需要執行自動化工作, 但不需要連結工作區和自動化帳戶的應用程式小組, 可以在自己的應用程式訂閱中建立不同的自動化帳戶。

![適用于大型環境的工作區模型](./media/workspace-model-large.png)

### <a name="decentralized-placement"></a>分散位置

在大型環境的替代模型中，修補和管理的責任可能與應用程式開發小組有關。 在此情況下, 您應該將工作區和自動化帳戶配對放在應用程式小組訂閱中, 以及其他資源。

  ![適用于分散環境的工作區帳戶模型](./media/workspace-model-decentralized.png)

## <a name="create-a-workspace-and-automation-account"></a>建立工作區和自動化帳戶

在決定如何最佳地放置和組織工作區和帳戶配對之後, 您必須先確定您已建立這些資源, 然後再開始上執行緒序。 本指南稍後包含的自動化範例會為您建立工作區和自動化帳戶配對。 不過, 如果您沒有現有的工作區和自動化帳戶配對, 如果您想要使用入口網站進行上架, 就必須建立一個。

若要透過 Azure 入口網站建立 Log Analytics 工作區, 請參閱[建立工作區](/azure/azure-monitor/learn/quick-create-workspace#create-a-workspace)。 接下來, 遵循[建立 Azure 自動化帳戶](/azure/automation/automation-quickstart-create-account)中的步驟, 為每個工作區建立相符的自動化帳戶。

> [!NOTE]
> 透過 Azure 入口網站建立自動化帳戶時, 預設行為會嘗試建立 Azure Resource Manager 和傳統部署模型資源的執行身分帳戶。 如果您的環境中沒有傳統 Vm, 而且您不是訂用帳戶的共同管理員, 則入口網站會建立 Resource Manager 的執行身分帳戶, 但在部署傳統執行身分帳戶時, 將會產生錯誤。 如果您不想要支援傳統資源, 可以忽略此錯誤。
>
> 您也可以使用[PowerShell](/azure/automation/manage-runas-account#create-run-as-account-using-powershell)建立執行身分帳戶。

## <a name="next-steps"></a>後續步驟

瞭解如何將[您的伺服器上架](./onboarding-overview.md)到 Azure 管理服務。

> [!div class="nextstepaction"]
> [上架至 Azure 伺服器管理服務](./onboarding-overview.md)
