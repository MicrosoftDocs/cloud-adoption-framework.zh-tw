---
title: Azure 中的成本管理工具
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: Azure 中的成本管理工具
author: BrianBlanchard
ms.author: brblanch
ms.date: 02/11/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 230e36d1ca59c208109eedbbdf7466f6373f4b00
ms.sourcegitcommit: 443c28f3afeedfbfe8b9980875a54afdbebd83a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2019
ms.locfileid: "71030256"
---
# <a name="cost-management-tools-in-azure"></a>Azure 中的成本管理工具

[成本管理](./index.md)是[五個雲端治理專業領域](../governance-disciplines.md)之一。 此專業領域著重於執行下列動作的方式：建立雲端支出方案、配置雲端預算、監視和強制執行雲端預算、偵測成本異常狀況，以及在實際支出不一致時調整雲端治理方案。

以下為 Azure 原生工具的清單，可協助使支援此治理專業領域的原則和流程臻至成熟。

| Tool | [Azure 入口網站](https://azure.microsoft.com/features/azure-portal)  | [Azure 成本管理](https://docs.microsoft.com/azure/cost-management/overview-cost-mgt)  | [Azure EA 內容套件](https://docs.microsoft.com/power-bi/service-connect-to-azure-enterprise)  | [Azure 原則](https://docs.microsoft.com/azure/governance/policy/overview) |
|---------|---------|---------|---------|---------|
|Enterprise 合約需要嗎？     | 否         | 否         | 是         | 否         |
|預算控制     | 否         | 是         | 否         | 是         |
|監視對於單一資源的支出    | 是         | 是         | 是         | 否         |
|監視對於多個資源的支出    | 否         | yes        | 是         | 否         |
|控制對於單一資源的支出     | 是：手動調整大小         | 是         | 否         | 是         |
|強制執行對於多個資源的支出    | 否         | 是         | 否         | 是         |
|在資源上強制執行帳戶處理中繼資料    | 否         | 否         | 否         | 是         |
|監視和偵測趨勢     | 是：有限制         | 是        | 是         | 否         |
|偵測支出異常狀況     | 否         | yes        | 是         | 否        |
|進行社交偏差     | 否        | yes        | 是        | 否        |
