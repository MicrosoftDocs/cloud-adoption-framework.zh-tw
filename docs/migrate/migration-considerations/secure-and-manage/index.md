---
title: 保護及管理
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 安全性監視和管理工具
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 652637905f9de09972eed199f85245e99fb60e29
ms.sourcegitcommit: 443c28f3afeedfbfe8b9980875a54afdbebd83a8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2019
ms.locfileid: "71022578"
---
# <a name="secure-monitoring-and-management-tools"></a>安全性監視和管理工具

完成移轉之後，已遷移的資產應由受控制的 IT 作業來管理。 本文的目的不是建議有違最佳做法的做法。 反之，下列項目應視為保護及管理已遷移之資產 (不論是來自 IT 作業或作為 IT 作業獨立上線) 的最簡可行產品。

## <a name="monitoring"></a>監視

「監視」  係指收集和分析資料來判斷您業務工作負載及其相依資源之效能、健康情況和可用性的行為。 Azure 包含多項服務，能在監視空間內個別執行特定的角色或工作。 這些服務共同提供一套全面性解決方案，可從您的工作負載應用程式和支援它們之 Azure 資源收集遙測資料，並分析和採取行動。 使用 Azure 監視器、Log Analytics 和 Application Insights 等雲端監視工具，檢視 Azure 中之應用程式、基礎結構和資料的健康情況和效能。 使用這些雲端監視工具來採取動作並與您的服務管理解決方案整合：

- **核心監視。** 核心監視提供監視所有 Azure 資源所需的基本功能。 這些服務只需最基本的設定，並且會收集進階監視服務所使用的核心遙測資料。
- **深層應用程式和基礎結構監視功能。** Azure 服務提供收集和分析深層監視資料的豐富功能。 它們以核心監視功能為基礎，並且運用 Azure 中的通用功能。 它們透過所收集的資料提供強大的分析，為您提供應用程式和基礎結構的獨特深入解析。

深入了解可監視已遷移資產的 [Azure 監視器](https://docs.microsoft.com/azure/azure-monitor/overview)。

## <a name="security-monitoring"></a>安全性監控

Azure 資訊安全中心為您的混合式雲端工作負載，提供統一的安全性監視和進階威脅通知。 資訊安全中心可完整檢視並控制 Azure 中雲端應用程式的安全性。 快速偵測及採取動作來回應威脅，並啟用調適性威脅防護來減少攻擊面。 內建儀表板可供立即深入掌握需要注意的安全性警示和弱點。 Azure 資訊安全中心可協助許多功能，包括：

- **集中式監視原則。** 集中管理跨混合式雲端工作負載的安全性原則，確保符合公司或法規的安全性需求。
- **持續安全性評估。** 監視機器、網路、儲存體和資料服務，以及應用程式的安全性，以找出潛在安全性問題。
- **可操作的建議。** 在攻擊者利用安全性弱點之前修正它們。 包括已排定優先順序和可採取動作的安全性建議。
- **進階雲端防禦。** 透過 Just-In-Time 存取管理連接埠及安全清單設定來降低威脅，以控制在 VM 上執行的應用程式。
- **已排定優先順序的警示和事件。** 藉由已排定優先順序的安全性警示和事件，首先專注處理最嚴重的威脅。
- **整合式安全性解決方案。** 從各種來源 (包括已連線的合作夥伴解決方案) 收集、搜尋及分析安全性資料。

深入了解可保護已遷移資產的 [Azure 資訊安全中心](https://docs.microsoft.com/azure/security-center)。

## <a name="protect-assets-and-data"></a>保護資產和資料

Azure 備份提供保護 VM、檔案和資料的方法。 Azure 備份可協助許多功能，包括：

- 備份 VM。
- 備份檔案。
- 備份 SQL Server 資料庫。
- 復原受保護的資產。

深入了解可備份已遷移資產的 [Azure 備份](https://docs.microsoft.com/azure/backup)。
