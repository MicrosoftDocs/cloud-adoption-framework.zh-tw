---
title: CAF 企業規模的商務持續性和嚴重損壞修復
description: 瞭解適用于 Azure 的 Microsoft 雲端採用架構中企業規模的商務持續性和嚴重損壞修復。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: dc998bcc4178815561a44b938dd79f56039d3a01
ms.sourcegitcommit: 917188fa930cadddb03f9e9bbcdd7b630e4ee33e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/17/2020
ms.locfileid: "88284617"
---
# <a name="caf-enterprise-scale-business-continuity-and-disaster-recovery"></a>CAF 企業規模的商務持續性和嚴重損壞修復

## <a name="planning-for-business-continuity-and-disaster-recovery"></a>規劃商務持續性和嚴重損壞修復

捕捉嚴重損壞修復 (DR) 需求，以設計適合應用程式工作負載使用的平台層級功能，以符合其特定的復原時間目標 (RTO) 和復原點目標 (RPO) 需求。

**設計考慮：**

- 應用程式和資料可用性需求，以及使用主動-主動和主動/被動可用性模式 (例如工作負載 RTO 和 RPO 需求) 。

- 平臺即服務的商務持續性和 DR (PaaS) 服務，以及原生 DR 和高可用性功能的可用性。

- 基於效能考慮，支援多區域部署以進行容錯移轉，並使元件更近。

- 在發生中斷時，功能較低或效能降低的應用程式作業。

- 可用性區域或可用性設定組的工作負載適用性。

  - 資料共用和區域之間的相依性。

  - 相較于可用性設定組，以及可同時進行維護的工作負載百分比，可用性區域對更新網域的影響。

  - 支援特定虛擬機器 (VM) 庫存單位可用性區域。

  - 如果使用 Microsoft Azure ultra 磁片儲存體，就需要使用可用性區域。

- 應用程式和資料的一致備份。

  - VM 快照集，以及使用 Azure 備份和復原服務保存庫。

  - 訂用帳戶限制，限制復原服務保存庫的數目和每個保存庫的大小。

  - PaaS 服務的異地複寫和 DR 功能。

- 發生容錯移轉時的網路連線能力。

  - Azure ExpressRoute 的頻寬容量規劃。

  - 配對的容錯移轉區域。

  - 發生區域、區域性或網路中斷時的流量路由。

- 規劃和未計畫的容錯移轉。

  - IP 位址一致性需求，以及在容錯移轉和容錯回復之後，可能需要維護 IP 位址。

  - 維護的工程 DevOps 功能。

- 應用程式金鑰、憑證和秘密的 Azure Key Vault DR。

**設計建議：**

- 將 Azure 的 Azure Site Recovery 運用於 Azure 虛擬機器的嚴重損壞修復案例，以跨區域複寫工作負載。

  Site Recovery 為 VM 工作負載提供內建的平臺功能，以符合低 RPO/RTO 需求（透過即時複寫和復原自動化）。 此外，此服務也可讓您執行復原演練，而不會影響生產環境中的工作負載。

- 使用原生 PaaS 服務的嚴重損壞修復功能。

  內建功能提供簡單的解決方案，讓您輕鬆地將複寫和容錯移轉到工作負載架構中，同時簡化設計和部署自動化。 已為其使用的服務定義標準的組織也可以透過 Azure 原則來審核和強制執行服務設定。

- 使用 Azure 原生備份功能。

  Azure 備份和 PaaS 原生備份功能不需要管理協力廠商備份軟體和基礎結構。 如同其他原生功能一樣，可以使用 Azure 原則來設定、審核備份設定，並加以強制執行，以確保服務符合組織的需求。

- 使用多個區域和對等互連位置進行 ExpressRoute 連線。

  重複的混合式網路架構可在發生影響 Azure 區域或對等提供者位置的中斷時，協助確保不中斷的跨單位連線。

- 為貴組織的嚴重損壞修復配置選取位置時，請參閱 [Azure 區域配對](/azure/best-practices-availability-paired-regions) 檔。

- 規劃商務持續性和 DR 時，請使用 Azure 配對的區域。

- 計畫的 Azure 系統更新會依序推出至配對的區域，但不能同時將停機時間、錯誤的影響，以及在不正常更新的罕見事件中的邏輯故障降至最低。

- 如果發生廣泛的中斷情況，復原一個區域的優先順序會在每個配對內。 跨配對區域部署的應用程式保證會有其中一個區域以優先權復原。 如果在未配對的區域中部署應用程式，最糟的情況是復原可能會延遲，而慣用的區域可能是最後一個要復原的區域。

- 避免針對生產和 DR 網站使用重迭的 IP 位址範圍。

  可能的話，請規劃可提供所有網站之並行連線的商務持續性和 DR 網路架構設計人員。 使用相同無類別網域間路由區塊的 DR 網路（例如生產網路）將需要網路容錯移轉程式，在發生中斷時，可能會使應用程式容錯移轉變得複雜且延遲。