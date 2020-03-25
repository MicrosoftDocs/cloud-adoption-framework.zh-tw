---
title: 軟體定義網路：雲端 DMZ
description: 瞭解雲端 DMZ 網路架構，這可讓您使用 VPN 來限制內部部署和雲端式網路之間的存取。
author: rotycenh
ms.author: abuck
ms.date: 02/11/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: decision-guide
ms.custom: governance
ms.openlocfilehash: cc9963118d8f69fd78f9cf5d84f5c140c200aec5
ms.sourcegitcommit: 25cd1b3f218d0644f911737a6d5fd259461b2458
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/24/2020
ms.locfileid: "80225848"
---
# <a name="software-defined-networking-cloud-dmz"></a>軟體定義網路：雲端 DMZ

透過虛擬私人網路 (VPN) 連線網路，雲端 DMZ 網路架構允許內部部署和雲端式網路之間的有限存取。 雖然當您想要保護外部網路存取時，通常會使用 DMZ 模型，但此處所討論的雲端 DMZ 架構是專門用來保護內部部署網路從雲端式資源的存取權，反之亦然。

![安全混合式網路架構](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/images/dmz-private.png)

此架構支援的案例為，您的組織想開始整合雲端式工作負載與內部部署工作負載，但雲端安全性原則可能尚未完全成熟，或尚未取得兩個環境之間安全的專用 WAN 連線。 因此，雲端網路應被視為周邊網路，以確保內部部署服務的安全。

DMZ 會部署網路虛擬裝置 (NVA) 以實作防火牆和封包檢查等安全性功能。 在內部部署與雲端式應用程式或服務之間傳遞的流量必須通過 DMZ 進行稽核。 VPN 連線以及判斷哪些流量可通過 DMZ 網路的規則由 IT 安全性小組嚴格控管。

## <a name="cloud-dmz-assumptions"></a>雲端 DMZ 假設

部署雲端 DMZ 包含下列假設：

- 您的安全性小組尚未完全符合內部部署和雲端式安全需求與原則。
- 您的雲端式工作負載需要存取裝載于內部部署或協力廠商網路上有限的服務子集，或內部部署環境中的使用者或應用程式需要有限的雲端託管資源存取權。
- 公司原則、法規需求或技術相容性問題無法阻止您在內部部署網路和雲端提供者之間實作 VPN。
- 您的工作負載不需透過多個訂用帳戶來避開資源限制，或者您的工作負載涉及多個訂用帳戶，但不需要集中管理分佈在多個訂用帳戶中的資源所使用的連線或共用服務。

當您想要執行雲端 DMZ 虛擬網路架構時，您的雲端採用小組應該考慮下列問題：

- 將內部部署網路與雲端網路連線，會使安全性需求變得更複雜。 雖然雲端網路與內部部署環境之間的連線會受到保護，但您仍然需要確保雲端資源受到保護。 任何為了存取雲端式工作負載所建立的公用 Ip，都必須使用[公開的 DMZ](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-dmz?toc=https://docs.microsoft.com/azure/cloud-adoption-framework/toc.json&bc=https://docs.microsoft.com/azure/cloud-adoption-framework/_bread/toc.json)或[Azure 防火牆](https://docs.microsoft.com/azure/firewall)來妥善保護。
- 雲端 DMZ 架構常作為跳板，當內部部署與雲端網路之間的連線進一步受到保護且安全性原則一致時，讓您能更廣泛採用完整的混合式網路功能架構。 不過，它也適用于隔離的部署，其具有雲端 DMZ 方法滿足的特定安全性、身分識別和連線需求。

## <a name="learn-more"></a>進一步了解

如需在 Azure 中執行雲端 DMZ 的詳細資訊，請參閱：

- [實作 Azure 與內部部署資料中心之間的 DMZ](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid)。 本文討論如何在 Azure 中實作安全的混合式網路架構。
