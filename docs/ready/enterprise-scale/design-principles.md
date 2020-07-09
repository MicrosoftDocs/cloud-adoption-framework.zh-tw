---
title: CAF 企業規模的設計原則
description: 瞭解適用于 Azure 的 Microsoft Cloud 採用架構中的企業規模設計原則。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: bb344743f7ea05dae13895e625825381def1840f
ms.sourcegitcommit: 4bbd5f6444d033ef1f38dc6f3bad7b914a82f68f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/08/2020
ms.locfileid: "86128433"
---
# <a name="caf-enterprise-scale-design-principles"></a>CAF 企業規模的設計原則

本指南中所規定的企業規模架構是根據這裡所述的設計原則。 這些原則可作為羅盤，以在重要的技術領域中進行後續的設計決策。 請熟悉這些原則，以進一步瞭解其影響，以及與 nonadherence 相關聯的取捨。

## <a name="subscription-democratization"></a>訂用帳戶 democratization

訂用帳戶應該做為管理單位，並隨著業務需求和優先順序調整，以支援業務領域和公事包擁有者，以加速應用程式的遷移和新的應用程式開發。 訂閱應提供給業務單位，以支援新工作負載的設計、開發和測試，以及工作負載的遷移。

## <a name="policy-driven-governance"></a>原則導向的治理

Microsoft Azure 原則應該用來提供防護 rails，並確保與您的組織平臺保持相容，再加上部署在其上的應用程式。 它也應該為應用程式擁有者提供足夠的自由度，以及安全的雲端不受妨礙路徑。

## <a name="single-control-and-management-plane"></a>單一控制和管理平面

<!-- cSpell:ignore AppOps -->

企業規模的架構不應考慮任何抽象層（例如客戶開發的入口網站或工具）。 它應該為 AppOps （集中管理的作業小組）和 DevOps （專屬的應用程式作業小組）提供一致的體驗。 Azure 會在所有 Azure 資源中提供統一且一致的控制平面，並根據角色型存取和原則導向控制來布建通道。 它可以用來建立一組標準化的原則和控制項，以管理整個企業資產。

## <a name="application-centric-and-archetype-neutral"></a>以應用程式為中心和原型-中性

企業規模架構應著重于以應用程式為中心的遷移和開發，而不是單純的基礎結構隨即轉移（例如移動虛擬機器）。 它不應該區別舊版和新的應用程式、基礎結構即服務或平臺即服務應用程式。 最後，它應該提供安全且安全的基礎，讓所有應用程式類型都部署到您的 Azure 平臺上。

## <a name="aligning-azure-native-design-and-roadmaps"></a>Azure 原生設計與藍圖的對齊

企業級架構方法提倡者會盡可能使用 Azure 原生平臺服務和功能。 這應該與 Azure 平臺藍圖一致，以確保您的環境中有新的功能可供使用。 Azure 平臺藍圖應該有助於通知遷移策略和企業級的軌跡。

## <a name="recommendations"></a>建議

準備好進行功能的取捨，因為第一天不太可能需要所有專案。 使用預覽服務並接管服務藍圖的相依性，以移除技術障礙。
