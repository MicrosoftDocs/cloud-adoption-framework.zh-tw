---
title: 裝置互動的創新工具
description: 瞭解 Azure tools 如何透過裝置和環境體驗進行互動，以增強客戶的自然周圍和行為。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: ade0514d0f7d0aec344f7542e94face9ac437e3a
ms.sourcegitcommit: 07d56209d56ee199dd148dbac59671cbb57880c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88878668"
---
# <a name="tools-to-interact-with-devices-in-azure"></a>在 Azure 中與裝置互動的工具

如同 [與裝置互動](../considerations/devices.md)的概念性文章中所述，用來與客戶互動的裝置，取決於提供客戶需求和強化採用所需的環境體驗。 從觸發程式開始，以提示客戶的需求，而您的解決方案符合該需求的能力，會決定重複使用的因素。 環境體驗有助於加快回應時間，並藉由在客戶的直屬環境中內嵌您的解決方案，為您的客戶建立更好的體驗。

![與裝置互動的雲端採用架構方法](../../_images/innovate/ambient-experiences.png)

## <a name="alignment-to-the-methodology"></a>與方法配合

這種類型的數位發明可透過下列任何等級的環境體驗來傳遞。 這些層級會與上述影像中所示的方法一致。 此頁面左側的目錄列出加速數位發明的技術指引。 這些文章會依環境經驗等級分組，以配合方法。

- 行動**體驗：** 行動裝置應用程式通常是客戶周圍環境的一部分。 在某些情況下，行動裝置可能會提供足夠的互動功能來使解決方案環境更穩固。
- **混合現實：** 有時，客戶的自然周圍都必須透過混合現實來改變。 在該混合現實內吸引客戶，可提供一種環境體驗。
- **整合現實：** 相較于真正的環境，整合式現實解決方案著重于使用客戶的實體裝置，將解決方案整合到自然行為中。
- **調整的現實：** 當上述任何解決方案使用預測性分析來提供該客戶自然周圍環境內的客戶互動時，該解決方案會建立最高形式的環境體驗。

## <a name="toolchain"></a>工具鏈

在 Azure 中，您通常會使用下列工具，在每個上述的環境解決方案層級之間加速數位發明。 這些工具會根據所需的經驗量來分組，以降低將工具與這些體驗對齊的複雜度。

| 類別 | 工具 |
|---|---|
| 行動體驗 | <li> Azure App Service <li> Power Apps <li> Power Automate <li> Intune |
| 混合實境 | <li> Unity <li> Azure Spatial Anchors <li> HoloLens |
| 整合現實 | <li> Azure IoT 中樞 <li> Azure Sphere <li> Azure Kinect DK |
| 已調整的實境 | <li> IoT 雲端到裝置 <li> Azure 數位 Twins + HoloLens |

## <a name="get-started"></a>開始使用

此頁面左側的目錄概述許多文章。 這些文章可協助您開始使用此工具鏈中的每個工具。

> [!NOTE]
> 某些連結可能會離開雲端採用架構，以協助您超越此架構的範圍。
