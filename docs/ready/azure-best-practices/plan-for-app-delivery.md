---
title: 規劃應用程式傳遞
description: 以安全的方式，檢查有關提供應用程式的重要設計考慮和建議。
author: BrianBlanchard
ms.author: brblanch
ms.date: 02/18/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: internal
ms.openlocfilehash: b6813c18df70884af187c24d779af50dbd07c221
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101794885"
---
# <a name="plan-for-application-delivery"></a>規劃應用程式傳遞

本節將探索主要的建議，以安全、可高度擴充且高可用性的方式，提供面向內部和外部應用程式。

**設計考慮：**

- Azure 負載平衡器 (內部和公用) 為區域層級的應用程式傳遞提供高可用性。

- Azure 應用程式閘道可讓您在區域層級進行 HTTP/S 應用程式的安全傳遞。

- Azure Front 可在 Azure 區域之間提供高可用性 HTTP/S 應用程式的安全傳遞。

- Azure 流量管理員可讓您提供全球應用程式。

**設計建議：**

- 在登陸區域內執行應用程式傳遞，以進行面向內部和外部應用程式。

- 針對 HTTP/S 應用程式的安全傳遞，請使用應用程式閘道 v2，並確定已啟用 WAF 保護和原則。

- 如果您無法使用應用程式閘道 v2 來取得 HTTP/S 應用程式的安全性，請使用合作夥伴 NVA。

- 部署 Azure 應用程式閘道 v2 或合作夥伴 Nva，用於登陸區域虛擬網路內的輸入 HTTP/S 連線，以及其所保護的應用程式。

- 針對登陸區域中的所有公用 IP 位址，使用 DDoS 標準保護計劃。

- 使用 Azure Front WAF 原則來提供和協助保護橫跨 Azure 區域的全球 HTTP/S 應用程式。

- 當您使用前門和應用程式閘道來協助保護 HTTP/S 應用程式時，請使用 Front WAF 原則。 鎖定應用程式閘道，只接收來自前門的流量。

- 使用流量管理員來提供跨越 HTTP/S 以外之通訊協定的全球應用程式。
