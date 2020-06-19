---
title: 常見問題集
description: 常見問題。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: befa3a167d38dc49120255b7ba7f5a992be8d888
ms.sourcegitcommit: 9b183014c7a6faffac0a1b48fdd321d9bbe640be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2020
ms.locfileid: "85076904"
---
# <a name="faq"></a>常見問題集

此頁面顯示企業規模設計和 Contoso 實行的常見問題。

## <a name="enterprise-scale-design"></a>企業規模設計

### <a name="where-a-landing-zone-maps-in-azure-in-the-context-of-enterprise-scale"></a>在企業規模的環境中，登陸區域會對應到 Azure

從企業規模的觀點來看，訂用帳戶是 Azure 中的登陸區域。

## <a name="contoso-implementation"></a>Contoso 的執行

### <a name="why-enterprise-scale-azure-resource-manager-templates-require-permissions-at-the-tenant-root--scope"></a>為何企業規模 Azure Resource Manager 範本需要租使用者根 '/' 範圍的許可權

建立管理群組是租使用者層級的 `put` 應用程式開發介面。 使用範例範本的必要條件是授與根範圍的許可權。

### <a name="why-sync-a-fork-with-the-upstream-repo"></a>為何要同步處理與上游存放庫的分叉

這可讓您控制要採取 bug/修補程式的頻率。 這是一個過渡解決方案，而我們會將管線程式碼基底封裝為 GitHub 動作，因此未來不需要此步驟。
