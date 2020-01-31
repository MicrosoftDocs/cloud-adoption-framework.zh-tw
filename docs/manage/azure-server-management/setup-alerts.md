---
title: 設定基本警示
description: 瞭解如何使用 Azure 伺服器管理服務來設定警示和通知，以協助您的 IT 小組留意任何問題。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: e24932b074f69f83aff583d560399d884c6c5b0e
ms.sourcegitcommit: 2362fb3154a91aa421224ffdb2cc632d982b129b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/28/2020
ms.locfileid: "76807949"
---
# <a name="set-up-basic-alerts"></a>設定基本警示

管理資源的重要部分是在發生問題時收到通知。 警示會根據計量、記錄或服務健康狀態問題的觸發程式，主動通知您重大狀況。 在將 Azure 伺服器管理服務上線時，您可以設定警示和通知，以協助您的 IT 小組留意任何問題。

## <a name="azure-monitor-alerts"></a>Azure 監視器警示

Azure 監視器提供[警示](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)功能，在發生問題時透過電子郵件或訊息通知您。 這些功能是以常見的資料監視平臺為基礎，其中包含您的伺服器和其他資源所產生的記錄和計量。 藉由在 Azure 監視器中使用一組常用的工具，您可以分析結合多個資源的資料，並使用它來觸發警示。 這些觸發程式可以包括：

- 度量值。
- 記錄搜尋查詢。
- 活動記錄事件。
- 基礎 Azure 平臺的健全狀況。
- 測試網站可用性。

如需此服務所收集監視資料來源的詳細描述，請參閱[Azure 監視器的資料來源清單](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources)。

如需使用 Azure 入口網站手動建立和管理警示的詳細資訊，請參閱[Azure 監視器檔](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric)。

## <a name="automated-deployment-of-recommended-alerts"></a>自動部署建議的警示

在本指南中，我們建議您建立一組15個警示以進行基本的基礎結構監視。 在[Azure 警示工具組 GitHub 存放庫](https://github.com/Microsoft/manageability-toolkits)中尋找部署腳本。

此套件會建立下列警示：

- 磁碟空間不足
- 可用記憶體不足
- 高 CPU 使用率
- 非預期的關機
- 損毀的檔案系統
- 常見的硬體故障

此套件使用 HP 伺服器硬體做為範例。 變更相關聯設定檔中的設定，以反映您的 OEM 硬體。 您也可以將更多效能計數器新增至設定檔。 若要部署封裝，請執行 New-CoreAlerts 檔案。

## <a name="next-steps"></a>後續步驟

瞭解支援您進行中作業的作業和安全性機制。

> [!div class="nextstepaction"]
> [持續的管理與安全性](./ongoing-management-overview.md)
