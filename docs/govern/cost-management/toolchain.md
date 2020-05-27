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
ms.openlocfilehash: a440cd0b73fc55e97fa6dc957ab08e39c9dbca1e
ms.sourcegitcommit: bd9872320b71245d4e9a359823be685e0f4047c5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/26/2020
ms.locfileid: "83862342"
---
# <a name="cost-management-tools-in-azure"></a>Azure 中的成本管理工具

[成本管理專業領域](./index.md)是[雲端治理的五個專業領域](../governance-disciplines.md)之一。 此專業領域著重於執行下列動作的方式：建立雲端支出方案、配置雲端預算、監視和強制執行雲端預算、偵測成本異常狀況，以及在實際支出不一致時調整雲端治理方案。

以下是 Azure 原生工具的清單，可協助您成熟支援此專業領域的原則和流程。

<!-- TODO: Content packs are deprecated. -->

| 工具 | [Azure 入口網站](https://azure.microsoft.com/features/azure-portal)  | [Azure 成本管理](https://docs.microsoft.com/azure/cost-management-billing/cost-management-billing-overview)  | [Azure EA 內容套件](https://docs.microsoft.com/power-bi/service-connect-to-azure-enterprise)  | [Azure 原則](https://docs.microsoft.com/azure/governance/policy/overview) |
|---------|---------|---------|---------|---------|
| Enterprise 合約需要嗎？     | No         | 否         | 是         | No         |
| 預算控制     | No         | 是         | No         | 是         |
| 監視對於單一資源的支出    | 是         | 是         | 是         | No         |
| 監視對於多個資源的支出    | No         | 是        | 是         | No         |
| 控制對於單一資源的支出     | 是：手動調整大小         | 是         | No         | 是         |
| 強制執行對於多個資源的支出    | No         | 是         | No         | 是         |
| 在資源上強制執行帳戶處理中繼資料    | No         | 否         | 否         | 是         |
| 監視和偵測趨勢     | 是          | 是        | 是         | No         |
| 偵測支出異常狀況     | No         | 是        | 是         | No        |
| 進行社交偏差     | No        | 是        | 是        | 否        |
