---
title: 規劃登陸區域網路分割
description: 使用 Azure 登陸區域來檢查有關網路分割的主要設計考慮和建議。
author: JefferyMitchell
ms.author: brblanch
ms.date: 02/18/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: think-tank
ms.openlocfilehash: c0dbbfa85eed220540a0aabb42cab540267c270d
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794685"
---
# <a name="plan-for-landing-zone-network-segmentation"></a>規劃登陸區域網路分割

本節將探討在登陸區域內提供高度安全內部網路分割的主要建議，以推動網路的零信任實行。

**設計考慮：**

- 零信任模型會假設違反狀態，並驗證每個要求，就好像它源自于未受控制的網路一樣。

- 先進的零信任網路實施採用完全分散的輸入/輸出雲端微周邊和更深層的微型分割。

- 網路安全性群組可以使用 Azure 服務標籤來加速連線到 Azure PaaS 服務。

- 應用程式安全性群組不會跨越或提供跨虛擬網路的保護。

- 現在可透過 Azure Resource Manager 範本支援 NSG 流量記錄。

**設計建議：**

- 將子網路建立委派給登陸區域擁有者。 這可讓它們定義如何跨子網分割工作負載 (例如，單一大型子網、多層式應用程式或網路插入的應用程式) 。 平臺小組可以使用 Azure 原則，以確保具有特定規則的 NSG (例如拒絕來自網際網路的連入 SSH 或 RDP，或允許/封鎖登陸區域間的流量) 一律與具有僅限拒絕原則的子網相關聯。

- 使用 Nsg 協助保護跨子網的流量，以及跨平臺的東部/西部流量 (登陸區域間的流量) 。

- 應用程式小組應在子網層級 Nsg 使用應用程式安全性群組，以協助保護登陸區域內的多層式 Vm。

- 使用 Nsg 和應用程式安全性群組來微分割登陸區域內的流量，並避免使用中央 NVA 來篩選流量。

- 啟用 NSG 流量記錄，並將其饋送至流量分析，以深入瞭解內部和外部流量流程。

- 使用 Nsg 選擇性地允許登陸區域之間的連接。

- 針對虛擬 WAN 拓撲，如果您的組織需要在登陸區域間流動的流量進行篩選和記錄功能，請透過 Azure 防火牆將流量路由傳送到登陸區域。
