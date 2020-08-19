---
title: 軟體定義網路決策指南
description: 使用適用於 Azure 的雲端採用架構，了解軟體定義的網路如何透過軟體提供集中管理的虛擬化網路功能。
author: rotycenh
ms.author: abuck
ms.date: 02/11/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: decision-guide
ms.custom: governance
ms.openlocfilehash: 6ebc35f265af6a1808eb82870ea1e40f69d70888
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88574035"
---
# <a name="software-defined-networking-decision-guide"></a>軟體定義網路決策指南

軟體定義網路 (SDN) 是一種網路架構，其設計目的是允許已虛擬化的網路功能可透過軟體進行集中管理、設定及修改。 SDN 支援使用與內部部署網路中所用的實體路由器、防火牆和其他網路裝置對應的虛擬對等裝置，建立雲端式網路。 若要在像 Azure 這樣的公用雲端平台上建立安全虛擬網路，SDN 是不可或缺的。

## <a name="networking-decision-guide"></a>網路決策指南

![繪製符合下列快速連結的網路選項 (從最簡單到最複雜)](../../_images/decision-guides/decision-guide-software-defined-network.png)

跳至：[僅限 PaaS](./paas-only.md) | [雲端原生](./cloud-native.md) | [雲端 DMZ](./cloud-dmz.md) | [混合式](./hybrid.md) | [中樞與輪幅模型](./hub-spoke.md) | [深入了解](#learn-more)

SDN 提供數個選項，搭配各種不同程度的價格和複雜度。 上述探索指南提供可將這些選項快速個人化，以便最符合特定商務和技術策略的參考。

本指南中的轉折點取決於您的雲端策略小組在進行關於網路架構的決策之前所做的數個關鍵決策。 其中最重要的是涉及您[數位資產定義](../../digital-estate/index.md)與[訂用帳戶設計](../subscriptions/index.md)的決策，這也可能需要納入雲端計量和全球市場策略相關決策的內容。

少於 1,000 個 VM 的小型單一區域部署不太可能大幅受到此轉折點影響。 相反地，包含超過 1,000 個 VM、多個業務部門，或多個地理區域市場的大量採用工作可能會明顯受到您 SDN 決策和這個關鍵轉折點影響。

## <a name="choose-the-right-virtual-networking-architectures"></a>選擇正確的虛擬網路架構

本節將細述決策指南，以協助您選擇正確的虛擬網路架構。

有許多方式可實作 SDN 技術來建立雲端式虛擬網路。 您如何建立要在移轉中使用的虛擬網路結構，以及那些網路如何與您現有 IT 基礎結構互動的方式，將取決於工作負載需求和您治理需求的組合。

規劃要在規劃雲端移轉時考量哪個虛擬網路架構或架構組合時，請考慮下列問題，以協助判斷何者適合您的組織：

| 問題 | 僅限 PaaS | 雲端原生 | 雲端 DMZ | 混合式 | 中樞與輪幅 |
|-----|-----|-----|-----|-----|-----|
| 您的工作負載將只會使用 PaaS 服務，而且除了服務本身所提供的網路功能，不需要任何網路功能嗎？ | 是 | 否 | 否 | 否 | 否 |
| 您的工作負載需要與內部部署應用程式整合嗎？ | 否 | 否 | 是 | 是 | 是 |
| 您是否已在內部部署與雲端網路之間，建立了成熟的安全性原則和安全的連線？ | 否 | 否 | 否 | 是 | 是 |
| 您的工作負載是否需要不會透過雲端識別服務支援的驗證服務，或者您是否需要直接存取內部部署網域控制站？ | 否 | 否 | 否 | 是 | 是 |
| 您是否將需要部署和管理大量 VM 和工作負載？ | 否 | 否 | 否 | 否 | 是 |
| 將對資源的控制委派給個別工作負載小組時，您是否將需要提供集中式管理與內部部署連線能力？ | 否 | 否 | 否 | 否 | 是 |

## <a name="virtual-networking-architectures"></a>虛擬網路架構

深入了解主要的軟體定義網路架構：

- **[僅限 PaaS](./paas-only.md)：** 大部分的平台即服務 (PaaS) 產品支援有所限制的內建網路功能組合，而且可能不需要明確定義的軟體定義網路來支援工作負載需求。
- **[雲端原生](./cloud-native.md)：** 雲端原生架構使用建置於雲端平台預設軟體定義網路功能，且不需要依賴內部部署或其他外部資源的虛擬網路，來支援雲端式工作負載。
- **[雲端 DMZ](./cloud-dmz.md)：** 支援您的內部部署與雲端網路之間有限制的連線，並透過實作周邊網路以緊密控制兩個環境之間的流量來保護連線安全。
- **[混合式](./hybrid.md)：** 混合式雲端網路架構可允許信任的雲端環境中的虛擬網路存取您的內部部署資源，反之亦然。
- **[中樞與輪幅](./hub-spoke.md)：** 中樞與輪輻架構可讓您集中管理外部連線能力與共用服務、隔離個別工作負載，以及克服潛在的訂用帳戶限制。

## <a name="learn-more"></a>深入了解

如需 Azure 中軟體定義網路的詳細資訊，請參閱：

- [Azure 虛擬網路](/azure/virtual-network/virtual-networks-overview)。 在 Azure 上，核心 SDN 功能會由 Azure 虛擬網路所提供，以做為實體內部部署網路的雲端類比。 虛擬網路也可做為平台上資源之間的預設隔離界限。
- [適用於網路安全性的 Azure 最佳做法](/azure/security/fundamentals/network-best-practices)。 由 Azure 安全性小組所提供，對於如何設定您的虛擬網路以將安全性弱點最小化的建議。

## <a name="next-steps"></a>後續步驟

軟體定義網路只是雲端採用程序期間，其中一個需針對架構做出決策的核心基礎結構元件。 請瀏覽架構決策指南概觀，以了解在為其他類型的基礎結構制定設計決策時，使用的替代模式或模型。

> [!div class="nextstepaction"]
> [架構相關決策指南](../index.md)
