---
title: 規劃 IP 定址
description: 查看 Azure 中關於 IP 定址的重要設計考慮和建議。
author: JefferyMitchell
ms.author: brblanch
ms.date: 02/18/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: think-tank
ms.openlocfilehash: 8051775c1010bdb5ea4ae79883414bc802607375
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101793663"
---
<!-- docutune:ignore "Azure VPN Gateway" -->

# <a name="plan-for-ip-addressing"></a>規劃 IP 定址

您的組織務必規劃 Azure 中的 IP 位址，以確保在內部部署位置和 Azure 區域之間的 IP 位址空間不會重迭。

**設計考慮：**

- 跨內部部署和 Azure 區域的 IP 位址空間重迭會造成重大的爭用挑戰。

- 您可以在建立虛擬網路之後新增位址空間。 如果虛擬網路已透過虛擬網路對等互連連線到另一個虛擬網路，則此程式需要中斷，因為對等互連必須刪除再重新建立。

- Azure 會在每個子網中保留5個 IP 位址。 當您要調整虛擬網路和包含的子網大小時，這些位址中的因素。

- 某些 Azure 服務需要 [專用子網](/azure/virtual-network/virtual-network-for-azure-services#services-that-can-be-deployed-into-a-virtual-network)。 這些服務包括 Azure 防火牆和 Azure VPN 閘道。

- 您可以將子網委派給特定服務，以在子網內建立服務的實例。

**設計建議：**

- 事先規劃跨 Azure 區域與內部部署位置的非重迭 IP 位址空間。

- 使用私人網際網路位址配置中的 IP 位址 (RFC 1918) 。

- 針對可用性有限的私人 IP 位址 (RFC 1918) 的環境，請考慮使用 IPv6。

- 請勿建立非必要的大型虛擬網路 (例如， `/16`) 確保 IP 位址空間不會浪費。

- 請勿事先規劃所需的位址空間，而不建立虛擬網路。 新增位址空間將會在透過虛擬網路對等互連連線虛擬網路之後，導致中斷。

- 請勿將公用 IP 位址用於虛擬網路，特別是如果公用 IP 位址不屬於您的組織。
