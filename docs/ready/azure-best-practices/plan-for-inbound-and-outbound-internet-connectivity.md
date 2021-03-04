---
title: 規劃輸入和輸出網際網路連線能力
description: 探索與公用網際網路之間的輸入和輸出連線所建議的連線性模型
author: JefferyMitchell
ms.author: brblanch
ms.date: 02/18/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: think-tank, csu
ms.openlocfilehash: 7a2d6126f5d529c466d70b6d65a1ced10e7bf34c
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101793666"
---
# <a name="plan-for-inbound-and-outbound-internet-connectivity"></a>規劃輸入和輸出網際網路連線能力

本節說明與公用網際網路之間的輸入和輸出連線所建議的連線性模型。

**設計考慮：**

- Azure 原生的網路安全性服務 (例如 Azure 防火牆、Azure 應用程式閘道上的 Azure Web 應用程式防火牆 (WAF)，以及 Azure Front Door) 為完全受控服務。 因此您不會產生與基礎結構部署相關聯的作業與管理成本，該成本在大規模的情況下可能會變得很複雜。

- 如果您的組織偏好使用 Nva 或原生服務無法滿足您組織的特定需求，則企業規模架構與合作夥伴 Nva 完全相容。

**設計建議：**

- 使用 Azure 防火牆來管理：

  - 流向網際網路的 Azure 輸出流量。

  - 非 HTTP/S 傳入連接。

  - 如果您的組織需要) 的東部/西部流量篩選 (。

- 使用具有虛擬 WAN 的防火牆管理員來部署和管理跨虛擬 WAN 中樞或中樞虛擬網路的 Azure 防火牆。 防火牆管理員現在已正式推出虛擬 WAN 和一般虛擬網路。

- 建立全域 Azure 防火牆原則，以控制全球網路環境的安全性狀態，並將其指派給所有的 Azure 防火牆實例。 藉由透過 Azure 角色型存取控制將累加式防火牆原則委派給本機安全性小組，以允許細微的原則符合特定區域的需求。

- 如果您的組織想要使用這類解決方案來協助保護輸出連線，請在防火牆管理員中設定支援的夥伴 SaaS 安全性提供者。

- 在登陸區域的虛擬網路內使用 WAF，以保護來自網際網路的輸入 HTTP/S 流量。

- 您可以使用 Azure Front 大門和 WAF 原則，在 Azure 區域之間提供全域保護，以用於登陸區域的輸入 HTTP/S 連接。

- 當您使用 Azure Front 大門和 Azure 應用程式閘道來協助保護 HTTP/S 應用程式時，請在 Azure Front 中使用 WAF 原則。 鎖定 Azure 應用程式閘道，只接收來自 Azure Front 的流量。

- 如果東部/西部或南/北流量保護和篩選都需要合作夥伴 Nva：

  - 針對虛擬 WAN 網路拓撲，請將 Nva 部署至不同的虛擬網路 (例如 NVA 虛擬網路) 。 然後將它連線到區域虛擬 WAN 中樞以及需要存取 Nva 的登陸區域。 本文將說明[此](/azure/virtual-wan/scenario-route-through-nva)流程。
  - 針對非虛擬 WAN 網路拓撲，請在中央中樞虛擬網路中部署合作夥伴 Nva。

- 如果輸入 HTTP/S 連線需要合作夥伴 Nva，請將它們部署在登陸區域的虛擬網路內，並與他們要保護並公開到網際網路的應用程式一起使用。

- 使用 [Azure DDoS 保護標準保護方案](/azure/ddos-protection/ddos-protection-overview) 來協助保護虛擬網路內裝載的所有公用端點。

- 請勿將內部部署周邊網路的概念與架構直接應用到 Azure。 Azure 中有類似的安全性功能，但實作和架構必須適應雲端。
