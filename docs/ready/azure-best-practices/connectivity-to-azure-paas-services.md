---
title: Azure PaaS 服務的連線能力
description: 檢查與 Azure 平臺即服務技術連線相關的主要設計考慮和建議。
author: JefferyMitchell
ms.author: brblanch
ms.date: 02/18/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: think-tank
ms.openlocfilehash: f2bf97ead19f75a93d6350e8e8f1d4a1c3223094
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794760"
---
# <a name="connectivity-to-azure-paas-services"></a>Azure PaaS 服務的連線能力

本節將探討先前的連線章節，探討使用 Azure PaaS 服務的建議連線方法。

**設計考慮：**

- 通常會透過公用端點來存取 Azure PaaS 服務。 不過，Azure 平臺會提供保護這類端點的功能，甚至讓它們完全私密：

  - 虛擬網路插入可為支援的服務提供專用的私人部署。 管理平面流量仍會流經公用 IP 位址。

  - [Private Link](/azure/private-link/private-endpoint-overview#private-link-resource) 可讓您使用私人 IP 位址存取 azure PaaS 實例或 Azure 負載平衡器標準層後方的自訂服務，以提供專屬的存取權。

  - 虛擬網路服務端點可從選取的子網提供服務層級的存取權給選取的 PaaS 服務。

- 企業通常會對於必須適當地緩和的 PaaS 服務的公用端點有疑慮。

- 針對 [支援的服務](/azure/private-link/private-link-overview#availability)，Private Link 可解決與服務端點相關聯的資料遭到外泄問題。 或者，您可以透過 Nva 使用輸出篩選，以提供減少資料遭到外泄的步驟。

**設計建議：**

- 針對支援的 Azure 服務使用虛擬網路插入，使其可從您的虛擬網路中使用。

- 已插入虛擬網路的 Azure PaaS 服務仍會使用公用 IP 位址執行管理平面作業。 使用 Udr 和 Nsg，確定已在虛擬網路內鎖定此通訊。

- 使用私人連結（ [如果有的話](/azure/private-link/private-link-overview#availability)），適用于共用的 Azure PaaS 服務。 Private Link 已正式推出數項服務，並為許多服務提供公開預覽。

- 透過 ExpressRoute 私用對等互連從內部部署存取 Azure PaaS 服務。 針對專用的 Azure 服務或 Azure Private Link，請使用虛擬網路插入來取得可用的共用 Azure 服務。 若要在虛擬網路插入或私用連結無法使用時從內部部署存取 Azure PaaS 服務，請使用 ExpressRoute 搭配 Microsoft 對等互連。 此方法可避免透過公用網際網路傳輸。

- 使用虛擬網路服務端點來保護從虛擬網路記憶體取 Azure PaaS 服務的安全，但只有在無法使用私人連結且沒有資料遭到外泄考慮時。 若要解決與服務端點相關的資料遭到外泄問題，請使用 NVA 篩選，或使用 Azure 儲存體的虛擬網路服務端點原則。

- 預設不會在所有子網上啟用虛擬網路服務端點。

- 如果有資料遭到外泄考慮，除非您使用 NVA 篩選，否則請勿使用虛擬網路服務端點。

- 我們不建議您執行強制通道，以啟用從 Azure 到 Azure 資源的通訊。
