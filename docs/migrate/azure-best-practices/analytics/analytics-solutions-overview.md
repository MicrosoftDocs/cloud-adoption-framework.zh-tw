---
title: 分析解決方案
description: 使用適用于 Azure 的雲端採用架構，以瞭解使用 Teradata、Netezza 和 Exadata 的分析解決方案。
author: v-hanki
ms.author: brblanch
ms.date: 07/14/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 5d043cd95f8d32ffeaca6b510d04927dc51ce3e3
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86451732"
---
<!-- cSpell:ignore Netezza Teradata Exadata Giga GigaOM MPP -->

# <a name="analytics-solutions"></a>分析解決方案

目前的市場供應專案在符合組織日益成長的需求方面變得很短。 舊版內部部署環境（包括 Teradata、Netezza 和 Oracle Exadata）既昂貴、創新緩慢，又 inelastic。 內部部署系統的現有使用者現在會考慮利用較新環境（例如雲端、IaaS、PaaS）所提供的創新，並想要將工作的基礎結構維護和平臺開發作業委派給雲端提供者。 Microsoft Azure 是全球可用、高度安全、可擴充的雲端環境，其中包括支援工具和功能生態系統內的 Azure Synapse 分析。

![Teradata 遷移的設計和效能](../../../_images/analytics/analytics-solutions-overview.png)

Azure Synapse 分析藉由使用大量平行處理（MPP）和自動記憶體內部快取等技術，提供最佳的關係資料庫效能。 這種方法的結果可以在獨立的基準測試中看到，例如最近由[GigaOM](https://gigaom.com)執行，這會比較 Azure Synapse Analytics 與其他熱門的雲端資料倉儲供應專案。 已經遷移到此環境的組織已經看過許多好處，包括：

- 改善的效能和價格/效能。
- 增加靈活性和較短的價值。
- 更快速的伺服器部署和應用程式開發。
- 彈性的擴充性，確保您只需支付實際使用量的費用。
- 改良的安全性與合規性。
- 降低儲存空間和嚴重損壞修復成本。
- 降低整體 TCO 和更好的成本控制（營運費用）。

若要將這些優點發揮到極致，必須將現有的（或新的）資料和應用程式遷移到 Azure Synapse 分析平臺。 在許多組織中，此方法包括從舊版內部部署平臺（例如 Teradata、Netezza 或 Exadata）遷移現有的資料倉儲。 組織需要將其資料資產現代化，其具有具備性價效益、快速創新，且可調整且真正彈性的分析供應專案。 請在下列各節中深入瞭解 Teradata、Netezza 和 Exadata 的遷移最佳作法。

## <a name="next-steps"></a>後續步驟

- [適用于 Teradata 的分析解決方案](./analytics-solutions-teradata.md)
- [適用于 Netezza 的分析解決方案](./analytics-solutions-netezza.md)
- [適用于 Exadata 的分析解決方案](./analytics-solutions-exadata.md)
