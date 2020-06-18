---
title: 設計原則
description: 設計原則
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: e5e9f2f6aec9285408c10927aa9e7aedc8ad178a
ms.sourcegitcommit: e5c4db8f660fa4c58d1441f0feb4cce415491dfd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/17/2020
ms.locfileid: "84942976"
---
# <a name="design-principles"></a>設計原則

本指南中規定的企業規模架構是以本節所述的設計原則為基礎。 這些原則可作為羅盤，以在重要的技術領域中進行後續的設計決策。 請熟悉這些原則，以進一步瞭解其影響，以及與 nonadherence 相關聯的取捨。

## <a name="subscription-democratization"></a>訂用帳戶 democratization

訂用帳戶應作為管理的單位，並隨著業務需求和優先順序而調整，以支援業務領域和公事包擁有者，以加速應用程式遷移和新的應用程式開發。 訂閱應提供給業務單位，以支援新工作負載和工作負載遷移的設計和開發/測試。

## <a name="policy-driven-governance"></a>原則導向的治理

Microsoft Azure 原則應該用來提供防護滑軌，並確保客戶平臺的持續相容性，以及部署到其上的應用程式，同時也提供應用程式擁有者足夠的自由度，以及安全的雲端不受妨礙路徑。

## <a name="single-control-and-management-plane"></a>單一控制和管理平面

<!-- cSpell:ignore AppOps -->

企業規模架構不應考慮任何抽象層（例如客戶開發的入口網站或工具），而且應該為 AppOps （集中管理的作業小組）和 DevOps （專屬的應用程式作業小組）提供一致的體驗。 Azure 在所有 Azure 資源和布建通道上提供統一且一致的控制平面，受限於角色型存取和原則導向的控制，並用來建立一組標準化的原則和控制項，以管理整個客戶資產。

## <a name="application-centric-and-archetype-neutral"></a>以應用程式為中心和原型-中性

企業級架構應著重于以應用程式為中心的遷移和開發，而不是單純的基礎結構隨即轉移（例如移動虛擬機器），而且不應該區分舊的應用程式或基礎結構即服務或平臺即服務應用程式。 最後，它應該為所有要部署到客戶 Azure 平臺的應用程式類型，提供安全且安全的基礎。

## <a name="aligning-azure-native-design-and-road-maps"></a>調整 Azure 原生設計和道路地圖

企業級架構方法提倡者在可能的情況下使用 Azure 原生平臺服務和功能，這應該與 Azure 平臺藍圖對應，以確保客戶環境內有新功能可用。 Azure 平臺藍圖應該有助於通知遷移策略和企業級的道路。

## <a name="recommendations"></a>建議

準備好進行功能的取捨，因為第一天不太可能需要所有專案。 使用預覽服務並接管服務藍圖的相依性，以移除技術障礙。
