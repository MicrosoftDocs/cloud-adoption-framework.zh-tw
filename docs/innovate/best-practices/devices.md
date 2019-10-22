---
title: 雲端創新：與 Azure 中的裝置互動的工具
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 在 Azure 中與裝置互動的工具
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.openlocfilehash: a8dcdf770e31a1b0fb380ac0fe85bb2fe9c8ed0b
ms.sourcegitcommit: f3371811a36e12533ecbc3aa936e2a68e0cee25f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/21/2019
ms.locfileid: "72683387"
---
# <a name="tools-to-interact-with-devices-in-azure"></a>在 Azure 中與裝置互動的工具

如[與裝置互動](../considerations/devices.md)的理論文章中所述，用來與客戶互動的裝置，都取決於提供客戶需求和強化採用所需的環境體驗層級。 從觸發程式開始，以提示需求，而您的解決方案滿足客戶需求的能力是重複使用的決定因素。 環境體驗可協助加速該回應時間，並藉由將您的解決方案內嵌在客戶的直屬環境中，為您的客戶建立更好的體驗。

![與裝置互動的雲端採用架構方法](../../_images/innovate/ambient-experiences.png)

## <a name="alignment-to-the-methodology"></a>與方法的對齊

這種類型的數位家發明可以透過下列任一層級的環境體驗來傳遞，如上面所示，與方法一致。 左側的目錄中會列出加速數位家發明的技術指引。 這些文章已分為相同層級的環境體驗，以配合方法：

- 行動**體驗：** 行動應用程式通常是客戶周圍的一員。 在某些情況下，行動裝置可能會提供足夠的互動層級，讓解決方案成為環境的功能。
- **Mixed reality：** 有時，客戶的自然周圍必須透過混合現實來改變。 在該混合現實內吸引客戶，可提供一種環境體驗。
- **整合式現實：** 相較于真正的環境，整合式現實解決方案著重于在客戶的實體現實中使用的裝置，以將解決方案整合到自然行為中。
- 已**調整的事實：** 當上述任何環境解決方案使用預測性分析，在其自然的環境中提供與客戶的互動時，解決方案會建立最高形式的環境體驗。

## <a name="toolchain"></a>工具鏈

在 Azure 中，通常會利用下列工具來加速上述各種環境解決方案層級的數位家發明。 這些工具已根據所需的經驗層級進行分組，以降低將工具與所需體驗對齊的複雜性。

- 行動體驗： App Service、PowerApps、Microsoft Flow、Intune
- 混合現實： Unity、Azure 空間錨點、HoloLens
- 整合現實： Azure IoT 中樞、Azure Sphere、Kinect 深色
- 已調整現實： IoT 雲端到裝置、數位 Twins + HoloLens

## <a name="get-started"></a>開始使用

左邊的目錄概述許多文章，讓您開始使用此工具鏈中的每個工具。

> [!NOTE]
> 有些連結可能會離開雲端採用架構，以協助超越此架構的範圍。
