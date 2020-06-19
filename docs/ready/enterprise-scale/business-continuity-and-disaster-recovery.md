---
title: 商務持續性和災害復原
description: 商務持續性和嚴重損壞修復。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: e20448cac2f08f895cc3fb1ff430156cb7a20f46
ms.sourcegitcommit: 9b183014c7a6faffac0a1b48fdd321d9bbe640be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2020
ms.locfileid: "85076994"
---
# <a name="business-continuity-and-disaster-recovery"></a>商務持續性和災害復原

## <a name="planning-for-business-continuity-and-disaster-recovery"></a>規劃商務持續性和嚴重損壞修復

本節將協助讀者捕捉客戶嚴重損壞修復（DR）需求，以設計適當的平台層級功能，讓應用程式工作負載能夠取用以滿足其特定的復原時間目標（RTO）和復原點目標（RPO）需求。

**設計考慮：**

- 應用程式和資料可用性需求，以及使用主動-主動和主動-被動可用性模式（例如工作負載 RTO 和 RPO 需求）。

- 平臺即服務（PaaS）服務的商務持續性和 DR，以及原生 DR 和高可用性功能的可用性。

- 基於效能考慮，支援多區域部署以進行容錯移轉，並以元件鄰近性進行。

- 具有降低功能或效能降低的應用程式作業存在中斷的情況。

- 可用性區域或可用性設定組的工作負載適用性。

  - 區域之間的資料共用和相依性。

  - 相較于可用性設定組，更新網域上可用性區域的影響，以及可以同時進行維護的工作負載百分比。

  - 支援特定虛擬機器（VM）的庫存單位與可用性區域。

  - 如果使用 Microsoft Azure ultra 磁片儲存體，則需要使用可用性區域。

- 應用程式和資料的一致備份。

  - VM 快照集，並使用 Microsoft Azure 備份和復原服務保存庫。

  - 訂用帳戶限制限制復原服務保存庫的數目和每個保存庫的大小。

  - PaaS 服務的異地複寫和 DR 功能。

- 發生容錯移轉時的網路連線能力。

  - Azure ExpressRoute 的頻寬容量規劃。

  - 配對的容錯移轉區域。

  - 發生區域、區域性或網路中斷時的流量路由。

- 已規劃和未計畫的容錯移轉。

  - IP 位址一致性需求，以及可能需要在容錯移轉和容錯回復後維護 IP 位址。

  - 維護的工程 DevOps 功能。

- Azure Key Vault 應用程式金鑰、憑證和秘密的 DR。

**設計建議：**

- 使用 Azure 的 Azure Site Recovery，虛擬機器嚴重損壞修復案例，跨區域複寫工作負載。

  Site Recovery 為 VM 工作負載提供內建的平臺功能，以透過即時複寫和復原自動化來符合低 RPO/RTO 需求。 此外，服務也提供執行復原演練的能力，而不會影響生產環境中的工作負載。

- 使用原生 PaaS 服務的嚴重損壞修復功能。

  內建功能可讓您輕鬆地解決建立複寫和容錯移轉至工作負載架構的複雜工作，同時簡化設計和部署自動化。 已針對其使用的服務定義標準的組織也可以透過 Azure 原則來審核及強制執行服務設定。

- 使用 Azure 原生備份功能。

  Azure 備份和 PaaS 原生備份功能不需要管理協力廠商備份軟體和基礎結構。 就像其他原生功能一樣，可以使用 Azure 原則來設定、審查和強制執行備份設定，以確保服務符合組織的需求。

- 使用多個區域和 ExpressRoute 連線的對等互連位置。

  重複的混合式網路架構可以在影響 Azure 區域或對等提供者位置的中斷事件時，協助確保不中斷的跨單位連線。

- 當您選取組織的嚴重損壞修復配置的位置時，請參閱[Azure 區域配對](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)檔。

- 規劃商務持續性和 DR 時，請使用 Azure 配對區域。

- 已規劃的 Azure 系統更新會順序轉出至配對的區域，但不會同時將停機時間降到最低、錯誤的影響，以及在錯誤更新的罕見事件中發生邏輯失敗的情況。

- 萬一發生廣泛的中斷，在每一組內復原一個區域的優先順序。 跨配對區域部署的應用程式保證會以優先順序復原其中一個區域。 如果在未配對的區域間部署應用程式，最糟的情況是復原可能會延遲，而慣用的區域可能會是最後一次進行復原。

- 避免針對生產和 DR 網站使用重迭的 IP 位址範圍。

  可能的話，請規劃商務持續性和 DR 網路架構設計人員，以提供與所有網站的並行連線。 使用相同無類別網域間路由區塊（例如生產網路）的 DR 網路將需要網路容錯移轉程式，萬一發生中斷時，可能會使應用程式容錯移轉複雜化並延遲。
