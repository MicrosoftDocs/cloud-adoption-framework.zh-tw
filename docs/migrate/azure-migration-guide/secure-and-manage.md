---
title: 保護及管理
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 保護及管理
author: matticusau
ms.author: mlavery
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.custom: fasttrack-new, AQC
ms.localizationpriority: high
ms.openlocfilehash: 7d742242f2639708914927aedbf45d1c59020c7d
ms.sourcegitcommit: a26c27ed72ac89198231ec4b11917a20d03bd222
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70818722"
---
# <a name="secure-and-manage"></a>保護及管理

將您的環境遷移至 Azure 之後，請務必考慮用來管理環境的安全性和方法。 Azure 提供許多特性和功能來滿足您解決方案中的這些需求。

# <a name="azure-monitortabmonitor"></a>[Azure 監視器](#tab/monitor)

Azure 監視器可藉由提供全方位的解決方案，以便收集、分析及處理來自雲端和內部部署環境的遙測資料，進而將應用程式的可用性和效能最大化。 它可協助您了解您的應用程式表現如何，並主動識別影響它們的問題以及它們所依賴的資源。

## <a name="use-and-configure-azure-monitor"></a>使用和設定 Azure 監視器

1. 在 Azure 入口網站中，移至 [監視器]  。
2. 選取 [計量]  、[記錄]  或 [服務健康狀態]  以取得概觀。
3. 選取任何相關的見解。

::: zone target="chromeless"

::: form action="OpenBlade[#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/overview]" submitText="Go to Azure Monitor" :::

::: zone-end

::: zone target="docs"

## <a name="read-more"></a>閱讀更多資訊

- [Azure 監視器概觀](/azure/azure-monitor/overview)。

::: zone-end

# <a name="azure-service-healthtabservicehealth"></a>[Azure 服務健康狀態](#tab/servicehealth)

當 Azure 服務中發生的問題影響您時，Azure 服務健康狀態會提供個人化指導及支援。 它會通知您並協助您了解問題所帶來的影響，並在問題解決時通知您； 也會協助您針對可能影響資源可用性之預定進行的維修作業和變更做好準備。

Azure 服務健康狀態包含：

- **Azure 狀態：** Azure 服務健康狀態的全域檢視。
- **服務健康狀態：** Azure 服務健康狀態的個人化檢視。
- **資源健康狀態：** 針對由 Azure 服務佈建給您的個別資源健康狀態進行更深入的檢視。

這些體驗結合在一起，可讓您在與您相關的詳細資料層級，全面瞭解 Azure 健康情況。

## <a name="access-service-health"></a>存取服務健康狀態

1. 在 Azure 入口網站中，移至 [監視器]  。
2. 選取 [服務健康狀態]  。

::: zone target="chromeless"

::: form action="OpenBlade[#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/serviceIssues]" submitText="Go to Service Health" :::

::: zone-end

::: zone target="docs"

## <a name="read-more"></a>閱讀更多資訊

若要深入了解，請參閱 [Azure 服務健康狀態文件](/azure/service-health)。

::: zone-end

# <a name="azure-advisortabadvisor"></a>[Azure Advisor](#tab/advisor)

Azure 建議程式是個人化的雲端顧問，可協助您依最佳做法來最佳化您的 Azure 部署。 它會分析您的資源及用量遙測， 並建議解決方案，協助改善效能、安全性及資料的高可用性，同時尋找降低整體 Azure 費用的機會。

## <a name="access-azure-advisor"></a>存取 Azure Advisor

1. 移至 Azure 入口網站中的 [Advisor]  ，或搜尋資源。
2. 選取 [高可用性]  、[安全性]  、[效能]  、[成本]  。

::: zone target="chromeless"

::: form action="OpenBlade[#blade/Microsoft_Azure_Expert/AdvisorMenuBlade/overview]" submitText="Go to Azure Advisor" :::

::: zone-end

::: zone target="docs"

## <a name="read-more"></a>閱讀更多資訊

[概觀](/azure/advisor/advisor-overview)。

::: zone-end

# <a name="azure-security-centertabsecurity"></a>[Azure 資訊安全中心](#tab/security)

Azure 資訊安全中心是統一基礎結構安全性管理系統，可強化資料中心的安全性狀態，並在雲端 (無論在 Azure 中與否) 及內部部署的混合式工作負載全面提供進階威脅防護。

## <a name="access-azure-security-center"></a>存取 Azure 資訊安全中心

1. 移至 Azure 入口網站中的 [資訊安全中心]  ，或搜尋資源。
2. 選取 [建議]  。

::: zone target="chromeless"

::: form action="OpenBlade[#blade/Microsoft_Azure_Security/SecurityMenuBlade/0]" submitText="Go to Security Center" :::

::: zone-end

::: zone target="docs"

## <a name="read-more"></a>閱讀更多資訊

[概觀](/azure/security-center/security-center-intro)

::: zone-end

# <a name="azure-backuptabbackup"></a>[Azure 備份](#tab/backup)

Azure 備份是以 Azure 為基礎的服務，可用來備份 (或保護) 和還原 Microsoft Cloud 中的資料。 Azure 備份將以一個可靠、安全及具成本競爭力的雲端架構解決方案，取代您現有的內部部署或異地備份解決方案。

## <a name="enable-backup-for-an-azure-vm"></a>啟用 Azure VM 的備份

1. 在 Azure 入口網站中，選取 [虛擬機器]  ，然後選取您想要複寫的 VM。
1. 在 [作業]  中，選取 [備份]  。
1. 建立或選取現有的復原服務保存庫。
1. 選取 [建立 (或編輯) 新的原則]  。
1. 設定排程和保留期間。
1. 選取 [確定]  。
1. 選取 [啟用備份]  。

::: zone target="chromeless"

::: form action="OpenBlade[#blade/HubsExtension/Resources/resourceType/Microsoft.Compute%2FVirtualMachines]" submitText="Go to Virtual Machines" :::

::: zone-end

::: zone target="docs"

[概觀](/azure/backup/backup-introduction-to-azure-backup)

::: zone-end

# <a name="azure-site-recoverytabsiterecovery"></a>[Azure Site Recovery](#tab/siterecovery)

稍早在本指南中，我們討論了如何在執行移轉的過程中使用 Azure Site Recovery。 但是在移轉完成後，它也會形成您的災害復原策略中的重要元件。

Azure Site Recovery 服務可讓您將裝載於主要 Azure 區域的虛擬機器和工作負載複寫至裝載於次要區域的複本。 當主要區域發生中斷時，您可以容錯移轉至在次要區域中執行的複本，並從該處繼續存取您的應用程式和服務。 當虛擬機器主要複本的中斷再次執行之後，您就可以容錯回復到它。

## <a name="replicate-an-azure-vm-to-another-region-with-site-recovery-service"></a>使用 Site Recovery 服務將 Azure VM 複寫至另一個區域

下列步驟概述使用 Site Recovery 服務將 Azure VM 複寫至另一個區域 (Azure 至 Azure) 的程序：

>
> [!TIP]
> 視您的案例而定，確切的步驟可能稍有不同。
>

## <a name="enable-replication-for-the-azure-vm"></a>啟用 Azure VM 的複寫

1. 在 Azure 入口網站中，選取 [虛擬機器]  ，然後選取您想要複寫的 VM。
1. 在 [作業]  中，選取 [災害復原]  。
1. 在 [設定災害復原]   >  [目標區域]  中，選取您要複寫至的目標區域。
1. 在本快速入門中，接受其他預設設定。
1. 選取 [啟用複寫]  。 這會開始一項作業來啟用 VM 的複寫。

::: zone target="chromeless"

::: form action="OpenBlade[#blade/HubsExtension/Resources/resourceType/Microsoft.Compute%2FVirtualMachines]" submitText="Go to Virtual Machines" :::

::: zone-end

## <a name="verify-settings"></a>確認設定

在複寫作業完成之後，您可以檢查複寫狀態、確認複寫健康情況，以及測試部署。

1. 在 VM 功能表中，選取 [災害復原]  。
2. 確認複寫健康情況、已建立的復原點，以及地圖上的來源和目標區域。

::: zone target="chromeless"

::: form action="OpenBlade[#blade/HubsExtension/Resources/resourceType/Microsoft.Compute%2FVirtualMachines]" submitText="Go to Virtual Machines" :::

::: zone-end

::: zone target="docs"

## <a name="learn-more"></a>深入了解

- [Azure Site Recovery 概觀](/azure/site-recovery/site-recovery-overview)
- [將 Azure VM 複寫到另一個區域](/azure/site-recovery/azure-to-azure-quickstart)

::: zone-end
