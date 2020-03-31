---
title: 軟體定義網路：混合式網路
description: 使用適用于 Azure 的雲端採用架構，瞭解混合式網路如何將雲端虛擬網路連接到內部部署資源。
author: rotycenh
ms.author: abuck
ms.date: 02/11/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: decision-guide
ms.custom: governance
ms.openlocfilehash: 6ccc459f1d708964afdc7c25cefdf314c44a6e02
ms.sourcegitcommit: afe10f97fc0e0402a881fdfa55dadebd3aca75ab
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/31/2020
ms.locfileid: "80433131"
---
# <a name="software-defined-networking-hybrid-network"></a>軟體定義網路：混合式網路

混合式雲端網路架構可讓虛擬網路使用專用 WAN 連線 (例如 ExpressRoute 或其他直接連線網路的連線方法) 存取您的內部部署資源和服務，反之亦然。

![混合式網路](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/images/expressroute.png)

以雲端原生虛擬網路架構為基礎，一開始建立時，會隔離混合式虛擬網路。 將連線新增至內部部署環境會授與內部部署網路的存取權，雖然必須明確允許虛擬網路中的其他所有輸入流量目標資源。 您可以使用虛擬防火牆裝置和路由規則來保護連線以限制存取，或者您可以準確指定可使用雲端原生路由功能存取兩個網路之間的哪些服務，或部署網路虛擬設備 (NVA) 以管理流量。

雖然混合式網路架構支援 VPN 連線，但因為較高的效能和增強的安全性，因此偏好 ExpressRoute 這類專用的 WAN 連線。

## <a name="hybrid-assumptions"></a>混合式假設

部署混合式虛擬網路包含下列假設：

- 您的 IT 安全性小組具備一致的內部部署和雲端型網路安全性原則，以確保可以信任雲端型虛擬網路，直接與內部部署系統通訊。
- 您的雲端型工作負載需要存取儲存體、應用程式和您的內部部署或第三方網路上裝載的服務，或您的使用者或您內部部署中的應用程式需要存取雲端裝載資源。
- 您需要遷移現有的應用程式和服務 (相依於內部部署資源)，但不想要耗費資源重新開發來移除這些相依性。
- 公司原則、資料主權需求或其他法規合規性問題不會防止您透過 VPN 或專用 WAN 將內部部署網路連接到雲端資源。
- 您的工作負載不需要多個訂用帳戶就能略過訂用帳戶資源限制，或者您的工作負載牽涉到多個訂用帳戶，但不需要集中管理的連線能力，或散佈于的資源之間多個訂用帳戶。

當您想要執行混合式虛擬網路架構時，您的雲端採用小組應該考慮下列問題：

- 將內部部署網路與雲端網路連線，會使安全性需求變得更複雜。 這兩個網路都必須受到保護，以免受到混合式環境的外部弱點和未經授權的存取。
- 調整混合式雲端環境內工作負載的數量和大小，可以對路由和流量管理增加大幅複雜性。
- 您必須開發相容的管理和存取控制原則，以維護整個組織一致的治理。

## <a name="learn-more"></a>進一步了解

如需 Azure 中混合式網路功能的詳細資訊，請參閱：

- [混合式網路參考架構](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/expressroute)。 Azure 混合式虛擬網路使用 ExpressRoute 線路或 Azure VPN，將您的虛擬網路與您組織的現有 IT 資產（未裝載于 Azure 中）連線。 本文討論在 Azure 中建立混合式網路的選項。
