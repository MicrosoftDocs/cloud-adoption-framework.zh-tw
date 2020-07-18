---
title: 雲端基礎結構和端點安全性的功能
description: 瞭解雲端基礎結構和端點安全性的功能。
author: JanetCThomas
ms.author: janet
ms.service: cloud-adoption-framework
ms.subservice: organize
ms.topic: conceptual
ms.date: 05/15/2020
ms.openlocfilehash: bddab3d91ff2587de55e3a5821b8a369bf3dbbe7
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86450606"
---
# <a name="function-of-cloud-infrastructure-and-endpoint-security"></a>雲端基礎結構和端點安全性的功能

負責基礎結構和端點安全性的雲端安全性小組會針對企業應用程式和使用者所使用的基礎結構和網路元件，提供安全性保護、偵探和回應控制。

## <a name="modernization"></a>現代化

軟體定義的資料中心和其他雲端技術，協助解決基礎結構和端點安全性的長期以來非常挑戰，包括：

- 針對雲端裝載的資產 **，清查和設定錯誤探索**會更可靠，因為它們全都是立即可見的（與實體資料中心）。
- **弱點管理**演變成整體安全性狀況管理的重要部分。
- **新增容器技術**，由基礎結構和網路小組管理和保護，因為組織會廣泛採用這項技術。 如需範例，請參閱[資訊安全中心中的容器安全性](https://docs.microsoft.com/azure/security-center/container-security)。
- **安全性代理程式匯總**和工具簡化，可減少安全性代理程式和工具的維護和效能額外負荷。
- **允許-列出應用程式**和內部網路篩選變得更容易設定及部署雲端託管伺服器（使用機器學習產生的規則集）。 如需 Azure 範例，請參閱彈性[應用](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application)程式控制和彈性[網路強化](https://docs.microsoft.com/azure/security-center/security-center-adaptive-network-hardening)。
- 使用雲端中的軟體定義資料中心，可以更輕鬆地設定基礎結構和安全性的**自動化範本**。 Azure 範例為[Azure 藍圖](https://docs.microsoft.com/azure/governance/blueprints/overview)
- 及時 **（JIT）且只有足夠的存取權（JEA）** ，可以有效地將最低許可權原則應用到伺服器和端點的特殊許可權存取。
- **使用者體驗**變得非常重要，因為使用者逐漸可以選擇或購買其端點裝置。
- **整合端點管理**可讓您管理所有端點裝置（包括行動和傳統電腦）的安全性狀態，並提供零信任存取控制解決方案的重要裝置完整性信號。
- 透過轉移至雲端應用程式架構，**網路安全性架構**和控制項會部分降低，但它們仍然是基本的安全性措施。 如需詳細資訊，請參閱[網路安全性和](https://docs.microsoft.com/azure/architecture/framework/security/network-security-containment)內含專案。

## <a name="team-composition-and-key-relationships"></a>小組撰寫和按鍵關聯性

雲端基礎結構和端點安全性通常會與下列角色互動：

- IT 架構和作業
- 安全性架構
- 安全性作業中心 (SOC)
- 合規性小組
- 審核小組

## <a name="next-steps"></a>後續步驟

檢查[威脅情報](./cloud-security-threat-intelligence.md)的功能。
