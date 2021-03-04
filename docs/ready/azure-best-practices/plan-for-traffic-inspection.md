---
title: 規劃流量檢查
description: 檢查有關鏡像的主要設計考慮和建議，或在 Azure 虛擬網路內點擊流量。
author: JefferyMitchell
ms.author: brblanch
ms.date: 02/18/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: think-tank
ms.openlocfilehash: dba15dab8463e7708c7f9bf0e45080417886393e
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101795001"
---
# <a name="plan-for-traffic-inspection"></a>規劃流量檢查

在許多產業中，組織要求將 Azure 中的流量鏡像至網路封包收集器，以進行深度檢查和分析。 這項需求通常著重于輸入和輸出網際網路流量。 本節探討在 Azure 虛擬網路內鏡像或點擊流量的重要考慮和建議方法。

**設計考慮：**

<!-- docutune:ignore TAP -->

- [Azure 虛擬網路終端機存取點 (點) ](/azure/virtual-network/virtual-network-tap-overview) 目前為預覽狀態。 連絡人 `azurevnettap@microsoft.com` 以取得可用性詳細資料。

- Azure 網路監看員中的封包捕獲已正式運作，但會限制為五小時的最長期間。

**設計建議：**

您可以使用 Azure 虛擬網路的替代方法來評估下列選項：

- 您可以使用網路監看員封包來捕獲有限的捕獲視窗。

- 評估最新版本的 NSG 流量記錄是否提供您所需的詳細資料層級。

- 針對需要深入封包檢查的案例，請使用合作夥伴解決方案。

- 請勿開發自訂解決方案來鏡像流量。 雖然小規模案例可能可接受這種方法，但由於複雜性和可能發生的可支援性問題，我們不鼓勵大規模進行。
