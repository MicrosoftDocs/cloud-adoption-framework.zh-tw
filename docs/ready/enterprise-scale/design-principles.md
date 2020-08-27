---
title: 雲端採用架構企業規模的設計原則
description: 瞭解適用于 Azure 的 Microsoft 雲端採用架構中的企業規模設計原則。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.openlocfilehash: f2746c47b99204bff629efdf60e8a9e1436168de
ms.sourcegitcommit: 07d56209d56ee199dd148dbac59671cbb57880c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88885077"
---
# <a name="cloud-adoption-framework-enterprise-scale-design-principles"></a>雲端採用架構企業規模的設計原則

本指南中指定的企業規模架構是以此處所述的設計原則為基礎。 這些原則可作為羅盤，以在關鍵技術領域進行後續的設計決策。 熟悉這些原則，以進一步瞭解其影響以及與 nonadherence 相關聯的取捨。

## <a name="subscription-democratization"></a>訂用帳戶 democratization

訂用帳戶應該用來作為管理的單位和規模，以支援商務領域和組合的擁有者，以加速應用程式遷移和新的應用程式開發。 訂用帳戶應提供給業務單位，以支援新工作負載和工作負載遷移的設計、開發和測試。

## <a name="policy-driven-governance"></a>原則導向的治理

Azure 原則應用來提供護欄，並確保與您組織的平臺持續相容，以及部署在其上的應用程式。 Azure 原則也會提供具有足夠自由度的應用程式擁有者，以及雲端的安全不受妨礙路徑。

## <a name="single-control-and-management-plane"></a>單一控制和管理平面

企業規模架構不應考慮任何抽象層，例如客戶開發的入口網站或工具。 它應該為 AppOps (集中管理的作業小組提供一致的體驗，) 和 (DevOps 專屬的應用程式作業小組) 。 Azure 會在所有 Azure 資源之間提供統一且一致的控制平面，並提供以角色為基礎的存取和原則導向的控制項。 Azure 可以用來建立一組標準化的原則和控制項，以管理整個企業資產。

## <a name="application-centric-and-archetype-neutral"></a>以應用程式為中心和原型中立

企業規模的架構應該專注于以應用程式為中心的遷移和開發，而不是單純的基礎結構隨即轉移，例如移動虛擬機器。 它不應該區分舊的和新的應用程式、基礎結構即服務，或是平臺即服務應用程式。 最後，它應該提供安全且安全的基礎，將所有應用程式類型部署到您的 Azure 平臺。

## <a name="align-azure-native-design-and-roadmaps"></a>調整 Azure 原生設計和藍圖

企業規模的架構方法提倡者，盡可能使用 Azure 原生平臺服務和功能。 這種方法應該符合 Azure 平臺藍圖，以確保您的環境中有新功能可供使用。 Azure 平臺的藍圖應有助於通知遷移策略和企業規模的軌跡。

## <a name="recommendations"></a>建議

因為不太可能在第一天都需要所有專案，所以請做好準備以將功能取捨。 使用預覽服務並取得服務藍圖的相依性，以移除技術障礙。
