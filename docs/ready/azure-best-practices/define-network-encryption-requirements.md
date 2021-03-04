---
title: 定義網路加密需求
description: 檢查內部部署與 Azure 之間網路加密的重要設計考慮和建議。
author: JefferyMitchell
ms.author: brblanch
ms.date: 02/18/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: think-tank
ms.openlocfilehash: fbe328ba0c9c9922c9f4cb7ba0ae53f2b61f6d6f
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794968"
---
# <a name="define-network-encryption-requirements"></a>定義網路加密需求

本節將探討可在內部部署與 Azure 之間，以及在 Azure 區域之間達成網路加密的重要建議。

**設計考慮：**

- 成本和可用頻寬與端點之間的加密通道長度成反比。

- 當您使用 VPN 連線至 Azure 時，流量會透過網際網路透過 IPsec 通道進行加密。

- 當您使用具有私人對等互連的 ExpressRoute 時，流量目前不會加密。

- 透過 ExpressRoute 私用對等互連設定站對站 VPN 連線現已 [進入預覽階段](/azure/vpn-gateway/site-to-site-vpn-private-peering)。

- 您可以將 [媒體存取控制安全性 (MACsec) ](/azure/expressroute/expressroute-howto-MACsec) 加密套用至 ExpressRoute Direct，以達成網路加密。

- 當 Azure 流量在資料中心之間移動 (不是由 Microsoft 或代表 Microsoft) 控制的實體界限之外， [MACsec 資料連結層加密](/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit) 會用於基礎網路硬體。 這適用于 VNet 對等互連流量。

**設計建議：**

![說明加密流程的圖。](./media/enc-flows.png)

*圖1：加密流程。*

- 當您使用 VPN 閘道來建立從內部部署至 Azure 的 VPN 連線時，流量會透過 IPsec 通道以通訊協定層級進行加密。 上圖顯示此加密在 flow 中 `A` 。

- 當您使用 ExpressRoute Direct 時，請設定 [MACsec](/azure/expressroute/expressroute-howto-MACsec) ，以便在您組織的路由器與 MSEE 之間的兩層級加密流量。 此圖顯示此加密在 flow 中 `B` 。

- 針對不是 MACsec 選項 (的虛擬 WAN 案例，例如，請勿使用 ExpressRoute Direct) ，請使用虛擬 WAN VPN 閘道透過 [ExpressRoute 私用對等互連來建立 IPsec 通道](/azure/virtual-wan/vpn-over-expressroute)。 此圖顯示此加密在 flow 中 `C` 。

- 針對非虛擬 WAN 案例，而且 MACsec 不是選項 (例如，不使用 ExpressRoute Direct) ，唯一的選項如下：

  - 使用 partner Nva 透過 ExpressRoute 私用對等互連來建立 IPsec 通道。
  - 透過搭配 Microsoft 對等互連的 ExpressRoute 建立 VPN 通道。
  - 評估透過 ExpressRoute 私人對等互連設定站對站 VPN 連線 ([預覽版](/azure/vpn-gateway/site-to-site-vpn-private-peering)的功能。

- 如果必須加密 Azure 區域之間的流量，請使用全域 VNet 對等互連來跨區域連接虛擬網路。

- 如果原生 Azure 解決方案 (如流程所示 `B` ，而 `C` 在圖表中) 不符合您的需求，請使用 Azure 中的合作夥伴 nva 來加密透過 ExpressRoute 私人對等互連的流量。
