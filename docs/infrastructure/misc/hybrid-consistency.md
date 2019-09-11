---
title: 建立混合式雲端一致性
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: 定義建立混合式雲端一致性的方法。
author: BrianBlanchard
ms.author: brblanch
ms.date: 12/27/2018
ms.topic: guide
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 819d0c058acf48402296bbac8d4cb4837271259e
ms.sourcegitcommit: a26c27ed72ac89198231ec4b11917a20d03bd222
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/06/2019
ms.locfileid: "70821645"
---
# <a name="create-hybrid-cloud-consistency"></a>建立混合式雲端一致性

本文會引導您完成建立混合式雲端一致性的高階方法。

混合式部署模型在移轉期間可以降低風險，並提供順暢的基礎結構轉換。 在處理商務程序時，雲端平台可提供最高等級的彈性。 許多組織都是遲疑的, 可讓您移往雲端。 相反地, 他們會想要完全掌控其最敏感的資料。 遺憾的是，內部部署伺服器不允許與雲端相同的創新速率。 混合式雲端解決方案可提供雲端創新的速度, 以及內部部署管理的控制權。

## <a name="integrate-hybrid-cloud-consistency"></a>整合混合式雲端一致性

使用混合式雲端解決方案可讓組織調整運算資源。 它也消除為了處理短期需求高峰而支出大規模資本的需求。 對您的企業所做的變更, 可以促使釋放本機資源以取得更敏感性資料或應用程式的需求。 取消布建雲端資源變得更簡單、更快速且成本更低。 您只需要為組織暫時使用的那些資源付費，而不需要購買及維護額外資源。 這種方法可減少長時間內可能維持閒置的設備數量。 混合式雲端運算提供雲端計算彈性、擴充性和成本效益的所有優點, 而且可能會造成資料暴露的風險降到最低。

![建立跨身分識別、管理、安全性、資料、開發和 DevOps](../../_images/hybrid-consistency.png)
的混合式雲端一致性*圖 1-跨身分識別、管理、安全性、資料、開發和 DevOps 建立混合式雲端一致性。*

真正的混合式雲端解決方案必須提供四個元件, 其中每個都有顯著的優點:

- **內部部署和雲端應用程式的通用身分識別:** 此元件提供使用者單一登入 (SSO) 給其所有應用程式, 以提升使用者的生產力。 它也可確保應用程式和使用者跨網路或雲端界限的一致性。
- **跨混合式雲端的整合式管理和安全性:** 此元件可讓您以一致的方式來監視、管理及保護環境, 以提高可見度和控制能力。
- **適用于資料中心和雲端的一致資料平臺:** 此元件會建立資料可攜性, 並結合對內部部署和雲端資料服務的無縫存取, 以深入瞭解所有資料來源。
- **跨雲端和內部部署資料中心的統一開發和 DevOps:** 此元件可讓您視需要在兩個環境之間移動應用程式。 提升開發人員生產力, 因為這兩個位置現在具有相同的開發環境。

以下是從 Azure 觀點來看, 這些元件的一些範例:

- Azure Active Directory (Azure AD) 可與內部部署 Active Directory 合作, 為所有使用者提供通用身分識別。 跨內部部署和透過雲端的 SSO 可讓使用者輕鬆安全地存取需要的應用程式與資產。 系統管理員可以管理安全性和治理控制項, 同時也能彈性地調整許可權, 而不會影響使用者體驗。
- Azure 為雲端和內部部署基礎結構提供整合式管理和安全性服務。 這些服務包括一組整合的工具, 可用來監視、設定及保護混合式雲端。 這種端對端的管理方法特別解決了面對混合式雲端解決方案的組織所面臨的實際挑戰。
- Azure 混合式雲端提供通用工具，確保對所有資料的存取都是安全、順暢且有效率的。 Azure 資料服務結合 Microsoft SQL Server，以建立一致的資料平台。 一致的混合式雲端模型可讓使用者使用操作和分析資料。 在內部部署和雲端中提供相同的服務, 以進行資料倉儲、資料分析及資料視覺效果。
- Azure 雲端服務與內部部署 Azure Stack 結合, 可提供統一的開發和 DevOps。 跨雲端和內部部署的一致性意味著您的 DevOps 小組可以建立在任一環境中執行的應用程式, 並可輕鬆地部署到正確的位置。 您也可以跨混合式解決方案重複使用範本, 以進一步簡化 DevOps 流程。

## <a name="azure-stack-in-a-hybrid-cloud-environment"></a>混合式雲端環境中的 Azure Stack

Azure Stack 是混合式雲端解決方案, 可讓組織在其資料中心執行一致的 Azure 服務。 它提供與 Azure 公用雲端服務一致的簡化開發、管理和安全性體驗。 Azure Stack 是 Azure 的延伸模組。 您可以使用它從您的內部部署環境執行 Azure 服務, 然後在需要時移到 Azure 雲端。

透過 Azure Stack, 您可以使用相同的工具部署和操作 IaaS 和 PaaS, 並提供與 Azure 公用雲端相同的體驗。 不論是透過 Web UI 入口網站或 PowerShell，Azure Stack 的管理對於使用 Azure 的 IT 系統管理員和終端使用者都有一致的外觀與操作。

Azure 和 Azure Stack 為客戶面向和內部的企業營運應用程式開啟新的混合式使用案例:

- **邊緣和中斷連線的解決方案。** 若要解決延遲和連線需求, 客戶可以在 Azure Stack 本機處理資料, 然後在 Azure 中匯總以進一步分析。 他們可以在兩者之間使用通用應用程式邏輯。 許多客戶對此 edge 案例感興趣, 涵蓋不同的內容, 例如工廠樓層、巡航運送和上個軸。
- **符合各種法規的雲端應用程式。** 客戶可以在 Azure 中開發及部署應用程式, 並具備在 Azure Stack 上部署內部部署的完整彈性, 以符合法規或原則需求。 不需要變更程式碼。 應用程式範例包括全域稽核、財務報告、外匯交易、線上遊戲和費用報告。 客戶有時會根據商務和技術需求, 尋找將相同應用程式的不同實例部署至 Azure 或 Azure Stack。 Azure 可滿足大部分的需求，而 Azure Stack 可視需要補充部署方法。
- **內部部署的雲端應用程式模型。** 客戶可以使用 Azure Web 服務、容器、無伺服器和微服務架構來更新及延伸現有應用程式，或建置新的應用程式。 您可以跨雲端中的 Azure 和內部部署的 Azure Stack，使用一致的 DevOps 程序。 即使是核心任務關鍵性應用程式, 應用程式現代化也會越來越感興趣。

Azure Stack 透過兩種部署選項來提供現代化：

- **Azure Stack 整合系統:** Azure Stack 整合系統是透過 Microsoft 與硬體合作夥伴提供, 以建立解決方案, 讓雲端的創新與簡單的管理達成平衡。 因為 Azure Stack 會當做硬體和軟體的整合系統提供, 所以您會獲得彈性和控制權, 同時仍採用雲端的創新。 Azure Stack 整合系統的大小範圍是4到12個節點。 這些是由硬體合作夥伴和 Microsoft 共同支援。 您可以使用 Azure Stack 整合系統，來為生產環境工作負載啟用新案例。
- **Azure Stack 開發套件:** Microsoft Azure Stack 開發工具組是 Azure Stack 的單一節點部署。 您可以使用它來評估和瞭解 Azure Stack。 您也可以使用套件作為開發人員環境, 您可以在其中使用與 Azure 一致的 Api 和工具進行開發。 Azure Stack 開發套件不適合作為生產環境使用。

## <a name="azure-stack-one-cloud-ecosystem"></a>Azure Stack 一部雲端生態系統

您可以使用完整的 Azure 生態系統來加速 Azure Stack 的計畫:

- Azure 可確保 Azure 認證的大部分應用程式和服務都能在 Azure Stack 上執行。 有數個 Isv 將其解決方案延伸到 Azure Stack。 這些 Isv 包含 Bitnami、Docker、Kemp 技術、Pivotal Cloud Foundry、Red Hat Enterprise Linux 和 SUSE Linux。
- 您可以選擇讓 Azure Stack 提供這些解決方案，並以完全受控服務的方式運作。 有數個合作夥伴會在 Azure 中提供受控服務供應專案, 而且很快就會 Azure Stack。 這些合作夥伴包括 Tieto、Yourhosting、Revera、Pulsant 和 NTT。 這些合作夥伴透過雲端解決方案提供者 (CSP) 計畫, 為 Azure 提供受控服務。 他們正在擴充其供應專案, 以包含混合式解決方案。
- 作為完整且完全受控的混合式雲端解決方案範例, Avanade 提供了一套全功能的供應專案。 其中包含雲端轉換服務、軟體、基礎結構、安裝和設定, 以及進行中的受控服務。 如此一來, 客戶就可以像使用 Azure 一樣地取用 Azure Stack。
- 提供者可為客戶建立端對端的 Azure 解決方案, 以協助加速應用程式現代化計畫。 他們提供深度的 Azure 技能集、領域和產業知識, 以及流程專業知識, 例如 DevOps。 每個 Azure Stack 雲端都是提供者設計解決方案和潛在客戶和影響系統部署的機會。 他們也可以自訂包含的功能並提供操作活動。 提供者的範例包括 Avanade、DXC、Dell EMC 服務、InFront 諮詢小組、HPE Pointnext 和 PwC (先前稱為 PricewaterhouseCoopers)。
