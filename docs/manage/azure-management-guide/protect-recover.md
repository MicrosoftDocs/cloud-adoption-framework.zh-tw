---
title: Azure 中的保護和復原
description: 藉由減少復原時間來確保業務穩定性
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: fasttrack-edit, AQC
ms.localizationpriority: high
ms.openlocfilehash: 3e9eadbd246ba38f496d8c74b7bcd3e6ade03685
ms.sourcegitcommit: 2362fb3154a91aa421224ffdb2cc632d982b129b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/28/2020
ms.locfileid: "76808153"
---
# <a name="protect-and-recover-in-azure"></a>Azure 中的保護和復原

「保護和復原」  是任何雲端管理基準中的第三個和最後一個專業領域。

![雲端管理基準](../../_images/manage/management-baseline.png)

在 [Azure 中的作業合規性](./operational-compliance.md)裡，我們的目標是要降低發生業務中斷的可能性。 本文的目標則是要在發生無法防止的中斷時，降低其持續時間和影響。

針對任何企業級環境，下表會概述任何管理基準的最低建議：

|Process  |工具  |目的  |
|---------|---------|---------|
|保護資料|Azure 備份|備份雲端中的資料和虛擬機器。|
|保護環境|Azure 資訊安全中心|加強安全性，並在混合式工作負載中提供進階的威脅防護。|

::: zone target="docs"

## <a name="azure-backup"></a>Azure 備份

::: zone-end
::: zone target="chromeless"

## <a name="azure-backuptabupdbackupatemanagement"></a>[Azure 備份](#tab/UpdbackupateManagement)

::: zone-end

透過 Azure 備份，您可以在 Microsoft 雲端中備份、保護及還原資料。 Azure 備份會以雲端式解決方案取代您現有的內部部署或異地備份解決方案。 這是一個可靠、安全及具成本競爭力的新解決方案。 Azure 備份也可透過一個一致的解決方案，協助您保護和復原內部部署資產。

### <a name="enable-backup-for-an-azure-vm"></a>啟用 Azure VM 的備份

1. 在 Azure 入口網站中，選取 [虛擬機器]  ，然後選取您想要複寫的 VM。
1. 在 [作業]  窗格中，選取 [備份]  。
1. 建立或選取現有的 Azure 復原服務保存庫。
1. 選取 [建立 (或編輯) 新的原則]  。
1. 設定排程和保留期間。
1. 選取 [確定]  。
1. 選取 [啟用備份]  。

::: zone target="chromeless"

::: form action="OpenBlade[#blade/HubsExtension/Resources/resourceType/Microsoft.Compute%2FVirtualMachines]" submitText="Go to Virtual Machines" :::

::: zone-end

::: zone target="docs"

[概觀](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup)

## <a name="azure-site-recovery"></a>Azure Site Recovery

::: zone-end
::: zone target="chromeless"

## <a name="azure-site-recoverytabsiterecovery"></a>[Azure Site Recovery](#tab/siterecovery)

::: zone-end

Azure Site Recovery 是災害復原策略中的重要元件。

Site Recovery 會複寫裝載在主要 Azure 區域中的 VM 和工作負載。 然後將這些項目複寫到裝載於次要區域的複本。 當主要區域發生中斷時，您將會容錯移轉至在次要區域中執行的複本。 接著，您可以從該處繼續存取您的應用程式和服務。 這種主動式復原方法可以大幅減少復原時間。 當不再需要復原環境時，生產流量便可容錯回復至原始環境。

### <a name="replicate-an-azure-vm-to-another-region-with-site-recovery"></a>使用 Site Recovery 將 Azure VM 複寫至另一個區域

下列步驟概述使用 Site Recovery 進行 Azure 至 Azure 複寫 (將 Azure VM 複寫至另一個區域) 的程序。
>
> [!TIP]
> 視您的案例而定，確切的步驟可能稍有不同。
>

### <a name="enable-replication-for-the-azure-vm"></a>啟用 Azure VM 的複寫

1. 在 Azure 入口網站中，選取 [虛擬機器]  ，然後選取您想要複寫的 VM。
1. 在 [作業]  窗格中，選取 [災害復原]  。
1. 選取 [設定災害復原]   > [目標區域]  ，然後選擇要作為複寫目的地的目標區域。
1. 在本快速入門中，請接受所有其他選項的預設值。
1. 選取 [啟用複寫]  ，這會啟動作業來啟用 VM 的複寫。

::: zone target="chromeless"

::: form action="OpenBlade[#blade/HubsExtension/Resources/resourceType/Microsoft.Compute%2FVirtualMachines]" submitText="Go to Virtual Machines" :::

::: zone-end

### <a name="verify-settings"></a>確認設定

在複寫作業完成之後，您可以檢查複寫狀態、確認複寫健康情況，以及測試部署。

1. 在 VM 功能表中，選取 [災害復原]  。
1. 確認複寫健康情況、已建立的復原點，以及地圖上的來源和目標區域。

::: zone target="chromeless"

::: form action="OpenBlade[#blade/HubsExtension/Resources/resourceType/Microsoft.Compute%2FVirtualMachines]" submitText="Go to Virtual Machines" :::

::: zone-end

### <a name="learn-more"></a>深入了解

- [Azure Site Recovery 概觀](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)
- [將 Azure VM 複寫到另一個區域](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart)
