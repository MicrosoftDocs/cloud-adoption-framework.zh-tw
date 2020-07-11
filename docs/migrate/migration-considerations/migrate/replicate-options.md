---
title: 複寫選項
description: 使用適用于 Azure 的雲端採用架構來瞭解複寫程式，以及為何需要複寫以進行雲端遷移。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 3be996a02a42505e6bd8168c8d05649387bb3da1
ms.sourcegitcommit: 84d7bfd11329eb4c151c4c32be5bab6c91f376ed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2020
ms.locfileid: "86234883"
---
# <a name="replication-options"></a>複寫選項

在任何移轉之前，您應確定主要系統安全無虞，並將繼續執行而不會發生問題。 任何停機都會干擾使用者或客戶，且耗費時間和金錢。 移轉並不像關閉內部部署虛擬機器並將其複製到 Azure 一樣簡單。 移轉工具必須將非同步或同步複寫納入考量，以確保可將即時系統複製到 Azure，而不需要停機。 在大部分的情況下，系統都必須與內部部署對應項目保持一致。 您可能想要在 Azure 中隔離的分割區中測試已遷移的資源，以確保工作負載如預期般運作。

雲端採用架構內的內容假設 Azure Migrate (或 Azure Site Recovery) 是最適合用於將資產複寫到雲端的工具。 不過，還有其他可用的選項。 本文討論可協助進行決策的選項。

## <a name="azure-site-recovery-also-known-as-azure-migrate"></a>Azure Site Recovery (也稱為 Azure Migrate)

[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) 可協調和管理 Azure VM、內部部署 VM 和實體伺服器的災害復原。 您也可以使用 Site Recovery 來管理將電腦內部部署和其他雲端提供者遷移至 Azure 的作業。 您會將內部部署機器複寫至 Azure，或將 Azure VM 複寫至次要地區。 然後，將 VM 從主要站台容錯移轉至次要站台，並完成移轉流程。 透過 Azure Site Recovery，您即可達成各種移轉案例：

- **從內部部署遷移至 Azure。** 將內部部署 VMware VM、Hyper-V VM 和實體伺服器遷移至 Azure。 若要這樣做，請完成與完整災害復原幾乎相同的步驟。 只是不要將機器從 Azure 容錯移轉回到內部部署站台。
- **在 Azure 區域之間進行遷移。** 將 Azure VM 從一個 Azure 區域遷移至另一個 Azure 區域。 完成移轉之後，在您遷移所在的次要區域中，設定 Azure VM 的災害復原。
- **從其他雲端遷移至 Azure。** 您可以將在其他雲端提供者上佈建的計算執行個體遷移至 Azure VM。 在進行移轉時，Site Recovery 會將這些執行個體視為實體伺服器。

![Azure Site Recovery ](../../../_images/migrate/asr-replication-image.png)
 _圖1： Azure Site Recovery 將資產移至 Azure 或其他雲端。_

在您評定內部部署和雲端基礎結構以進行移轉後，Azure Site Recovery 會藉由複寫內部部署機器來提供移轉策略。 透過下列簡單步驟，您可以設定將內部部署 VM、實體伺服器和雲端 VM 執行個體移轉至 Azure 的作業：

- 驗證必要條件。
- 準備 Azure 資源。
- 準備內部部署 VM 或雲端執行個體以進行移轉。
- 部署設定伺服器。
- 啟用 VM 複寫。
- 測試容錯移轉以確定一切都沒問題。
- 執行一次性容錯移轉至 Azure。

## <a name="azure-database-migration-service"></a>Azure 資料庫移轉服務

此服務捨棄多項工具，只使用單一全方位功能的服務，降低雲端移轉的複雜度。 [Azure 資料庫移轉服務](https://docs.microsoft.com/azure/dms/dms-overview)設計成可將內部部署 SQL Server 資料庫順暢地移至雲端的端對端解決方案。 這是一個完全受控的服務，能夠從多個資料庫來源無縫移轉至 Azure 資料平台，將停機時間降到最低。 它整合了現有工具和服務的某些功能，為客戶提供全方位、高可用性的解決方案。

服務會使用 Data Migration Assistant 來產生評量報告，以提供建議，引導您完成執行遷移之前所需的變更。 由您自行決定是否要執行任何必要補救。 當您準備好開始進行遷移程式時，Azure 資料庫移轉服務就會執行所有相關聯的步驟。 移轉程序會善用 Microsoft 決定的最佳做法，因此您可以放心地移轉專案。

## <a name="next-steps"></a>後續步驟

複寫完成後，即可開始[預備活動](./stage.md)。

> [!div class="nextstepaction"]
> [移轉期間的預備活動](./stage.md)
