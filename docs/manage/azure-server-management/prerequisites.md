---
title: 規劃 Azure 伺服器管理服務
description: 瞭解管理 Azure 伺服器管理服務所需資源的工具和準備工作。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: 3a591c3f8419c11cb8077a897fb05784407a2d52
ms.sourcegitcommit: 826f2a3f0353bb711917e99d9a17f6198fb41ada
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2020
ms.locfileid: "93024584"
---
# <a name="phase-1-prerequisite-planning-for-azure-server-management-services"></a>第1階段： Azure 伺服器管理服務的先決條件規劃

在這個階段中，您將熟悉 Azure 伺服器管理服務套件，並規劃如何部署執行這些管理解決方案所需的資源。

## <a name="understand-the-tools-and-services"></a>瞭解工具和服務

如需詳細的總覽，請參閱 [Azure 伺服器管理工具和服務](./tools-services.md) ：

- 與進行中的 Azure 作業相關的管理區域。
- 可協助您在這些區域中支援的 Azure 服務和工具。

您將使用這些服務的數個，以符合您的管理需求。 這些工具通常會在本指南中參考。

下列各節將討論使用這些工具和服務所需的規劃和準備。

## <a name="log-analytics-workspace-and-automation-account-planning"></a>Log Analytics 工作區和自動化帳戶規劃

您將用來上架 Azure 管理服務的許多服務都需要 Log Analytics 工作區和連結的 Azure 自動化帳戶。

[Log Analytics 工作區](/azure/azure-monitor/learn/quick-create-workspace)是用來儲存 Azure 監視器記錄資料的唯一環境。 每個工作區都有自己的資料存放庫和設定。 資料來源和解決方案會設定為將其資料儲存在特定工作區中。 Azure 監視解決方案會要求所有伺服器都必須連線到工作區，以便能夠儲存和存取其記錄資料。

某些管理服務需要 [Azure 自動化](/azure/automation/automation-intro) 帳戶。 您可以使用此帳戶和 Azure 自動化的功能，來整合 Azure 服務和其他公用系統，以部署、設定及管理您的伺服器管理程式。

下列 Azure 伺服器管理服務需要連結的 Log Analytics 工作區和自動化帳戶：

- [Azure 更新管理](/azure/automation/update-management/overview)
- [變更追蹤和清查](/azure/automation/change-tracking)
- [Hybrid Runbook Worker](/azure/automation/automation-hybrid-runbook-worker)
- [Desired State Configuration](/azure/virtual-machines/extensions/dsc-overview)

本指南的第二個階段著重于部署服務和自動化腳本。 它會說明如何建立 Log Analytics 工作區和自動化帳戶。 本指南也會說明如何使用 Azure 原則來確保新的虛擬機器已連線到正確的工作區。

本指南中的範例假設部署尚未部署至雲端的伺服器。 若要深入瞭解規劃工作區所需的原則和考慮，請參閱 [Azure 監視器中的記錄管理資料和工作區](/azure/azure-monitor/platform/manage-access)。

## <a name="planning-considerations"></a>規劃考量

準備您需要的工作區和帳戶以登入管理服務時，請考慮下列問題：

- **Azure 地理位置和法規遵循：** Azure 區域會組織成 _地理_ 位置。 [Azure 地理位置](https://azure.microsoft.com/global-infrastructure/geographies)可確保符合地理界限內的資料落地、主權、合規性及復原需求。 如果您的工作負載受限於資料主權或其他合規性需求，則工作區和自動化帳戶必須部署到相同 Azure 地理位置內的區域，以作為其所支援的工作負載資源。
- **工作區數目：** 作為指導準則，請建立每個 Azure 地理位置所需的工作區數目下限。 針對您的計算或儲存體資源所在的每個 Azure 地理位置，建議至少要有一個工作區。 當您將資料移轉至不同的地理位置時，此初始對齊有助於避免未來的法規問題。
- **資料保留和上限：** 建立工作區或自動化帳戶時，您可能也需要將資料保留原則或資料上限需求納入考慮。 如需這些原則的詳細資訊，以及規劃工作區時的其他考慮，請參閱 [Azure 監視器中的記錄管理資料和工作區](/azure/azure-monitor/platform/manage-access)。
- **區域對應：** 只有在某些 Azure 區域之間，才支援連結 Log Analytics 工作區和 Azure 自動化帳戶。 例如，如果 Log Analytics 工作區裝載在 `East US` 區域中，則必須在 `East US 2` 要與管理服務搭配使用的區域中建立連結的自動化帳戶。 如果您的自動化帳戶是在另一個區域中建立的，則無法連結至中的工作區 `East US` 。 部署區域的選擇可能會大幅影響 Azure 地理位置的需求。 請參閱 [區域對應表](/azure/automation/how-to/region-mappings) ，以決定哪個區域應裝載您的工作區和自動化帳戶。
- **工作區多宿主：** Azure Log Analytics 代理程式在某些情況下支援多路連接，但在此設定中執行時，代理程式會面臨數個限制和挑戰。 除非 Microsoft 針對您的特定案例建議使用，否則請勿在 Log Analytics 代理程式上設定多路連接。

## <a name="resource-placement-examples"></a>資源放置範例

有幾個不同的模型可供選擇您放置 Log Analytics 工作區和自動化帳戶所在的訂用帳戶。 簡而言之，將工作區和自動化帳戶放在負責執行更新管理服務和變更追蹤和清查服務的小組所擁有的訂用帳戶中。

以下是一些部署工作區和自動化帳戶的方式範例。

### <a name="placement-by-geography"></a>依地理位置放置

小型和中型環境具有單一訂用帳戶，以及橫跨多個 Azure 地理位置的數百個資源。 針對這些環境，請在每個地理位置中建立一個 Log Analytics 工作區和一個 Azure 自動化帳戶。

您可以在每個資源群組中建立一個工作區和一個 Azure 自動化帳戶。 然後，將對應的地理位置中的配對部署到虛擬機器。

或者，如果您的資料合規性原則不規定資源位於特定區域，您可以建立一組來管理所有虛擬機器。 我們也建議您將工作區和自動化帳戶組放在不同的資源群組中，以提供更細微的角色型存取控制 (RBAC) 。

下圖中的範例有一個具有兩個資源群組的訂用帳戶，每個群組都位於不同的地理位置：

![適用于中小型環境的工作區模型](./media/workspace-model-small.png)

### <a name="placement-in-a-management-subscription"></a>管理訂用帳戶中的位置

較大型的環境會橫跨多個訂用帳戶，並擁有具有監視和合規性的中央 IT 小組。 針對這些環境，在 IT 管理訂用帳戶中建立工作區和自動化帳戶配對。 在此模型中，地理位置中的虛擬機器資源會將其資料儲存在 IT 管理訂用帳戶的對應地理工作區中。 如果應用程式小組需要執行自動化工作，但不需要連結的工作區和自動化帳戶，他們可以在自己的應用程式訂用帳戶中建立個別的自動化帳戶。

![大型環境的工作區模型](./media/workspace-model-large.png)

### <a name="decentralized-placement"></a>分散式放置

在大型環境的替代模型中，應用程式開發小組可以負責修補和管理。 在此情況下，請將工作區和自動化帳戶組放在應用程式小組訂用帳戶中，連同其他資源一起放置。

  ![非集中式環境的工作區帳戶模型](./media/workspace-model-decentralized.png)

## <a name="create-a-workspace-and-automation-account"></a>建立工作區和自動化帳戶

在您選擇最佳的工作區和帳戶組合的方式之後，請確定您已建立這些資源，然後再開始進行上架程式。 本指南稍後的自動化範例會為您建立工作區和自動化帳戶配對。 但是，如果您想要使用 Azure 入口網站上線，而且您沒有現有的工作區和自動化帳戶配對，您必須建立一個。

若要使用 Azure 入口網站建立 Log Analytics 工作區，請參閱 [建立工作區](/azure/azure-monitor/learn/quick-create-workspace#create-a-workspace)。 接下來，請遵循 [建立 Azure 自動化帳戶](/azure/automation/automation-quickstart-create-account)中的步驟，為每個工作區建立相符的自動化帳戶。

> [!NOTE]
> 當您使用 Azure 入口網站建立自動化帳戶時，入口網站預設會嘗試建立 Azure Resource Manager 和傳統部署模型資源的執行帳戶。 如果您的環境中沒有傳統虛擬機器，而且您不是訂用帳戶的共同管理員，則入口網站會為 Resource Manager 建立執行身分帳戶，但在部署傳統執行身分帳戶時，會產生錯誤。 如果您不打算支援傳統資源，則可以忽略此錯誤。
>
> 您也可以使用 [PowerShell](/azure/automation/manage-runas-account#creating-a-run-as-account-using-powershell)來建立執行身份帳戶。

## <a name="next-steps"></a>後續步驟

瞭解如何將 [您的伺服器上架](./onboarding-overview.md) 到 Azure 伺服器管理服務。

> [!div class="nextstepaction"]
> [上架到 Azure 伺服器管理服務](./onboarding-overview.md)
