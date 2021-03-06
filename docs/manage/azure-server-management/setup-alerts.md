---
title: 設定基本警示
description: 瞭解如何使用 Azure 伺服器管理服務來設定警示和通知，以協助讓您的 IT 小組知道任何問題。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: internal
ms.openlocfilehash: 9508b43837944ebfeb2bb1c10b93ff8fb9fed36f
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102113668"
---
# <a name="set-up-basic-alerts"></a>設定基本警示

管理資源的重要部分是在發生問題時收到通知。 警示會根據計量、記錄或服務健康情況問題的觸發程式，主動通知您重大情況。 在上架 Azure 伺服器管理服務時，您可以設定警示和通知，讓您的 IT 小組知道任何問題。

## <a name="azure-monitor-alerts"></a>Azure 監視器警示

Azure 監視器提供 [警示](/azure/azure-monitor/alerts/alerts-overview) 功能，可在發生錯誤時透過電子郵件或訊息通知您。 這些功能是以常見的資料監視平臺為基礎，其中包含伺服器和其他資源所產生的記錄和計量。 您可以使用 Azure 監視器中的一組通用工具來分析合併自多個資源的資料，並使用它來觸發警示。 這些觸發程式可以包括：

- 度量值。
- 記錄搜尋查詢。
- 活動記錄事件。
- 基礎 Azure 平臺的健康情況。
- 測試網站的可用性。

如需此服務所收集的監視資料來源的詳細說明，請參閱 [Azure 監視器資料來源清單](/azure/azure-monitor/agents/data-sources) 。

如需使用 Azure 入口網站手動建立及管理警示的詳細資訊，請參閱 [Azure 監視器檔](/azure/azure-monitor/alerts/alerts-metric)。

## <a name="automated-deployment-of-recommended-alerts"></a>自動部署建議的警示

<!-- docutune:casing "Alert Toolkit" -->

在本指南中，我們建議您建立一組15個警示來監視基本的基礎結構。 在 [警示工具](https://github.com/Microsoft/manageability-toolkits) 組 GitHub 存放庫中尋找部署腳本。

此封裝會建立下列警示：

- 磁碟空間不足
- 可用記憶體不足
- 高 CPU 使用率
- 未預期的關機
- 檔案系統損毀
- 常見的硬體故障

封裝會使用 HPE 伺服器硬體作為範例。 變更相關聯設定檔中的設定，以反映您的 OEM 硬體。 您也可以在設定檔中新增更多效能計數器。 若要部署封裝，請執行檔案 `New-CoreAlerts.ps1` 。

## <a name="next-steps"></a>下一步

瞭解支援您進行中作業的作業和安全性機制。

> [!div class="nextstepaction"]
> [持續管理和安全性](./ongoing-management-overview.md)
