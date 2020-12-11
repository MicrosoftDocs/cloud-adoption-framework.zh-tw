---
title: 移轉工具決策指南
description: 使用此決策樹作為根據移轉決策選取最佳工具的高階指導。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: decision-guide
ms.custom: internal
ms.openlocfilehash: a0c706b457b26bc887a2995fc7f98d64c14b61dd
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97023475"
---
# <a name="migration-tools-decision-guide"></a>移轉工具決策指南

用於將應用程式移轉至 Azure 的策略和工具，在很大程度上取決於您的商業動機、技術策略和時間表，以及對正在移轉的實際工作負載和資產 (基礎結構、應用程式和資料) 的深入理解。 下列決策樹可作為根據移轉決策選取最佳工具的高階指導。 將此決策樹視為起點。

使用平台即服務 (PaaS) 或基礎結構服務 (IaaS) 技術進行移轉的選擇取決於成本、時間、現有的技術債務，和長期回報之間的平衡。 IaaS 通常是最快的雲端路徑，對工作負載的需求變化最小。 PaaS 可能需要修改資料結構或原始程式碼，但以降低的營運成本和更大的技術彈性的形式，產生大量的長期回報。 在下圖中，「現代化」一詞用於反映在移轉期間使資產現代化，並將現代化資產移轉到 PaaS 平台的決策。

![移轉工具決策樹範例。](../../_images/migrate/migration-tools-decision-tree.png)

## <a name="key-questions"></a>重要問題

回答下列問題可讓您根據上述樹狀結構制訂決策。

- **移轉期間應用程式平台的現代化是否是時間、精力和預算的明智投資？** 諸如 Azure App Service 或 Azure Functions 之類的 PaaS 技術，可以提高部署彈性並降低管理虛擬機器以裝載應用程式的複雜度。 應用程式可能需要重構才能利用這些雲端原生功能，這可能會為移轉工作增加大量時間和成本。 如果您的應用程式可以透過最少的修改移轉至 PaaS 技術，那麼它可能是現代化的良好候選項目。 如果需要進行大量重構，使用以 IaaS 為基礎的虛擬機器進行移轉可能是較好的選擇。
- **移轉期間資料平台的現代化是否是時間、精力和預算的明智投資？** 與應用程式移轉一樣，Azure PaaS 受控儲存體選項 (例如 Azure SQL Database、Azure Cosmos DB 和 Azure 儲存體) 提供了重要的管理和彈性優勢，但移轉到這些服務可能需要重構現有的資料和使用該資料的應用程式。 資料平台所需的重構通常比應用程式平台來得少。 因此，即使應用程式平台維持不變，對資料平台進行現代化也是很常見的。 如果您的資料可以透過最少的變更移轉至受控資料服務，那麼它就是現代化的理想選擇。 需要花費大量時間或成本來重構以利用這些 PaaS 服務的資料，可能更適合使用 IaaS 型虛擬機器進行移轉，以使其更符合現有的裝載功能。
- **您的應用程式目前是在專用的虛擬機器上執行，還是與其他應用程式共用裝載？** 與在共用的伺服器上執行的應用程式相比，在專用的虛擬機器上執行的應用程式可能會更輕鬆地移轉到 PaaS 裝載選項。
- **您的資料移轉是否會超過網路頻寬？** 內部部署資料來源與 Azure 之間的網路容量可能是資料移轉的瓶頸。 如果您需要傳送的資料面臨阻礙有效率或即時移轉的頻寬限制，您可能需要查看替代或離線的轉移機制。 Cloud Adoption Framework [有關移轉複寫的文章](../../migrate/migration-considerations/migrate/replicate.md#replication-risks---physics-of-replication)討論了複寫限制如何影響移轉工作。 作為移轉評估的一部分，請諮詢您的 IT 小組，以確認您的本機和 WAN 頻寬是否能夠處理您的移轉需求。 另請參閱[移轉期間處理超過網路容量的儲存體需求時的移轉案例](../../migrate/azure-best-practices/network-capacity-exceeded.md#suggested-prerequisites)。
- **您的應用程式是否使用現有的 DevOps 管線？** 在許多情況下，可以輕鬆地重構 Azure 管線，以將應用程式部署到雲端式裝載環境。
- **您的資料是否有複雜的資料儲存需求？** 生產應用程式通常需要高度可用的資料儲存體、提供 Always On 功能，和類似的服務執行時間和持續性功能。 以 Azure PaaS 為基礎的受控資料庫選項 (例如 Azure SQL Database、適用於 MySQL 的 Azure 資料庫，以及 Azure Cosmos DB) 均提供 99.99% 的執行時間服務等級協定。 相反，Azure VM 上以 IaaS 為基礎的 SQL Server 提供 99.95% 的單一執行個體服務等級協定。 如果您的資料無法現代化以使用 PaaS 儲存體選項，則確保較高的 IaaS 執行時間將涉及更複雜的資料儲存體案例，例如執行 SQL Server Always On 叢集，以及在執行個體之間不斷同步處理資料。 這可能涉及顯著的裝載及維護成本，因此在考慮資料移轉選項時，平衡正常執行時間需求、現代化工作和整體預算影響非常重要。

## <a name="innovation-and-migration"></a>創新和移轉

根據 Cloud Adoption Frameworks 對[累加移轉](../../migrate/index.md#migration-effort)工作的重視，對移轉策略和工具的初始決定並未排除未來的創新工作，以更新應用程式來利用 Azure 平台提供的機會。 雖然初始移轉作業可能主要著重在使用 IaaS 方法進行重新裝載，但您應該計劃定期重新瀏覽雲端裝載的應用程式組合，以調查最佳化機會。

## <a name="learn-more"></a>深入了解

- **[雲端基本概念：Azure 計算選項的概觀](/azure/architecture/guide/technology-choices/compute-decision-tree)：** 提供 Azure IaaS 和 PaaS 計算選項功能的相關資訊。
- **[雲端基本概念：選擇正確的資料存放區](/azure/architecture/guide/technology-choices/data-store-overview)：** 討論 Azure 平台上可用的 PaaS 儲存體選項。
- **[移轉的最佳做法：移轉工作期間資料需求超過網路容量](../../migrate/azure-best-practices/network-capacity-exceeded.md)：** 討論可用的網路頻寬阻礙資料移轉之案例的替代資料移轉機制。
- **[SQL Database：在 Azure 中選擇適當的 SQL Server 選項](/azure/sql-database/sql-database-paas-vs-sql-server-iaas#business-motivations-for-choosing-databases-managed-instances-or-sql-virtual-machines)：** 討論選擇在受控基礎結構 (IaaS) 或受控服務 (PaaS) 環境中，裝載 SQL Server 工作負載的選項和業務理由。
