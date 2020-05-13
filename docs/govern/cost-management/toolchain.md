---
title: Azure 中的成本管理工具
description: 瞭解 Azure 原生工具如何協助成熟的原則和流程，以支援成本管理的專業領域。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 6f2e08a85df87f7973f19ebd83b71de0f2003189
ms.sourcegitcommit: 60d8b863d431b5d7c005f2f14488620b6c4c49be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83220476"
---
# <a name="cost-management-tools-in-azure"></a>Azure 中的成本管理工具

[成本管理專業領域](./index.md)是[雲端治理的五個專業領域](../governance-disciplines.md)之一。 此專業領域著重於執行下列動作的方式：建立雲端支出方案、配置雲端預算、監視和強制執行雲端預算、偵測成本異常狀況，以及在實際支出不一致時調整雲端治理方案。

以下是 Azure 原生工具的清單，可協助您成熟支援此專業領域的原則和流程。

<!-- TODO: Content packs are deprecated. -->

| 工具 | [Azure 入口網站](https://azure.microsoft.com/features/azure-portal)  | [Azure 成本管理](https://docs.microsoft.com/azure/cost-management-billing/cost-management-billing-overview)  | [Azure EA 內容套件](https://docs.microsoft.com/power-bi/service-connect-to-azure-enterprise)  | [Azure 原則](https://docs.microsoft.com/azure/governance/policy/overview) |
|---------|---------|---------|---------|---------|
| Enterprise 合約需要嗎？     | 否         | 否         | 是         | 否         |
| 預算控制     | 否         | 是         | 否         | 是         |
| 監視對於單一資源的支出    | 是         | 是         | 是         | 否         |
| 監視對於多個資源的支出    | 否         | 是        | 是         | 否         |
| 控制對於單一資源的支出     | 是：手動調整大小         | 是         | 否         | 是         |
| 強制執行對於多個資源的支出    | 否         | 是         | 否         | 是         |
| 在資源上強制執行帳戶處理中繼資料    | 否         | 否         | 否         | 是         |
| 監視和偵測趨勢     | 是          | 是        | 是         | 否         |
| 偵測支出異常狀況     | 否         | 是        | 是         | 否        |
| 進行社交偏差     | 否        | 是        | 是        | 否        |
