---
title: 建立混合式雲端一致性
description: 使用適用于 Azure 的雲端採用架構，瞭解如何定義建立混合式雲端一致性的方法。
author: BrianBlanchard
ms.author: brblanch
ms.date: 12/27/2018
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: a5132e89cf9d20fb6cdbff1533737073ab190c3d
ms.sourcegitcommit: 07d56209d56ee199dd148dbac59671cbb57880c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88885978"
---
<!-- cSpell:ignore ISVs Bitnami Yourhosting Revera Avanade Pulsant PricewaterhouseCoopers Pointnext -->

# <a name="create-hybrid-cloud-consistency"></a>建立混合式雲端一致性

本文將引導您完成建立混合式雲端一致性的高階方法。

混合式部署模型在移轉期間可以降低風險，並提供順暢的基礎結構轉換。 在處理商務程序時，雲端平台可提供最高等級的彈性。 許多組織都遲疑移至雲端。 相反地，他們想要保留對其最敏感性資料的完整控制權。 可惜的是，內部部署伺服器不允許與雲端相同的創新率。 混合式雲端解決方案可提供雲端創新的速度，以及內部部署管理的控制權。

## <a name="integrate-hybrid-cloud-consistency"></a>整合混合式雲端一致性

使用混合式雲端解決方案可讓組織調整運算資源。 它也消除為了處理短期需求高峰而支出大規模資本的需求。 對您的企業所做的變更，可以促使針對更敏感的資料或應用程式釋出本機資源的需求。 取消布建雲端資源更容易、更快速且成本更低。 您只需要為組織暫時使用的那些資源付費，而不需要購買及維護額外資源。 這種方法可減少長時間可能維持閒置的設備數量。 混合式雲端運算提供雲端運算彈性、擴充性和成本效益的所有優點，並具備資料暴露風險的最低可能風險。

![建立跨身分識別、管理、安全性、資料、開發和 DevOps 的混合式雲端一致性 ](../../_images/hybrid-consistency.png)
 _：圖1：跨身分識別、管理、安全性、資料、開發和 DevOps 建立混合式雲端一致性。_

真正的混合式雲端解決方案必須提供四個元件，其中每個元件都有顯著的優點：

- **適用于內部部署和雲端應用程式的一般身分識別：** 此元件提供使用者單一登入 (SSO) 至所有應用程式，以提升使用者的生產力。 它也可確保應用程式和使用者跨越網路或雲端界限時的一致性。
- **跨混合式雲端的整合式管理和安全性：** 此元件提供您一致的方式來監視、管理及保護環境，進而提高可見度和控制能力。
- **適用于資料中心和雲端的一致資料平臺：** 此元件會建立資料可攜性，並結合對內部部署和雲端資料服務的無縫存取，以深入瞭解所有資料來源。
- **跨雲端和內部部署資料中心的統一開發和 DevOps：** 此元件可讓您視需要在兩個環境之間移動應用程式。 提升開發人員生產力的原因，是因為兩個位置現在都有相同的開發環境。

以下是 Azure 透視圖中這些元件的一些範例：

- Azure Active Directory (Azure AD) 可與內部部署 Active Directory 搭配使用，為所有使用者提供通用身分識別。 跨內部部署和透過雲端的 SSO 可讓使用者輕鬆安全地存取需要的應用程式與資產。 系統管理員可以管理安全性和治理控制項，也可以彈性地調整許可權，而不會影響使用者體驗。
- Azure 提供雲端和內部部署基礎結構的整合式管理和安全性服務。 這些服務包含一組整合的工具，可用來監視、設定和保護混合式雲端。 這種端對端的管理方法，專門解決組織考慮混合式雲端解決方案的真實世界挑戰。
- Azure 混合式雲端提供通用工具，確保對所有資料的存取都是安全、順暢且有效率的。 Azure 資料服務結合 Microsoft SQL Server，以建立一致的資料平台。 一致的混合式雲端模型可讓使用者同時使用操作和分析資料。 您可以在內部部署和雲端中提供相同的服務，以進行資料倉儲、資料分析和資料視覺效果。
- 結合 Azure Stack 內部部署的 azure 雲端服務，可提供統一的開發和 DevOps。 跨雲端和內部部署的一致性表示 DevOps 團隊可以建立在任一環境中執行的應用程式，而且可以輕鬆地部署到正確的位置。 您也可以在混合式解決方案中重複使用範本，這可進一步簡化 DevOps 程式。

## <a name="azure-stack-in-a-hybrid-cloud-environment"></a>混合式雲端環境中的 Azure Stack

Azure Stack 是混合式雲端解決方案，可讓組織在其資料中心執行一致的 Azure 服務。 它提供與 Azure 公用雲端服務一致的簡化開發、管理和安全性體驗。 Azure Stack 是 Azure 的延伸模組。 您可以使用它從您的內部部署環境執行 Azure 服務，然後在必要時移至 Azure 雲端。

透過 Azure Stack，您可以使用相同的工具，並提供與 Azure 公用雲端相同的體驗，來部署和操作 IaaS 和 PaaS。 無論是透過 web ui 入口網站或透過 PowerShell 來管理 Azure Stack，都能為 IT 系統管理員和使用 Azure 的使用者提供一致的外觀與風格。

Azure 和 Azure Stack 針對面向客戶與內部的企業營運應用程式，開啟新的混合式使用案例：

- **邊緣和中斷連線的解決方案。** 若要解決延遲和連線需求，客戶可以在 Azure Stack 本機處理資料，然後在 Azure 中匯總資料以進一步分析。 它們可以在兩者之間使用通用應用程式邏輯。 許多客戶對於此 edge 案例都有興趣，例如工廠樓層、巡航出貨和內建的環境。
- **符合各種法規的雲端應用程式。** 客戶可以在 Azure 中開發及部署應用程式，並具備完整的彈性，可在 Azure Stack 上部署內部部署，以符合法規或原則需求。 不需要變更程式碼。 應用程式範例包括全域稽核、財務報告、外匯交易、線上遊戲和費用報告。 客戶有時候會根據商務和技術需求，將相同應用程式的不同實例部署到 Azure 或 Azure Stack。 Azure 可滿足大部分的需求，而 Azure Stack 可視需要補充部署方法。
- **內部部署的雲端應用程式模型。** 客戶可以使用 Azure web 服務、容器、微服務和無伺服器架構來更新及擴充現有的應用程式，或建立新的應用程式。 您可以跨雲端中的 Azure 和內部部署的 Azure Stack，使用一致的 DevOps 程序。 即使是核心任務關鍵性應用程式，應用程式現代化也有越來越大的興趣。

Azure Stack 透過兩種部署選項來提供現代化：

- **Azure Stack 整合式系統：** Azure Stack 整合系統是透過 Microsoft 與硬體合作夥伴提供的解決方案，可讓您透過簡單的管理來提供雲端進度的創新。 因為 Azure Stack 是以硬體和軟體的整合系統形式提供，所以您可以獲得彈性和控制，同時又採用雲端的創新。 Azure Stack 整合系統的大小範圍為4到12個節點。 它們是由硬體合作夥伴與 Microsoft 共同支援。 您可以使用 Azure Stack 整合系統，來為生產環境工作負載啟用新案例。
- **Azure Stack 開發套件：** Microsoft Azure Stack 開發工具組是 Azure Stack 的單一節點部署。 您可以使用它來評估和瞭解 Azure Stack。 您也可以使用套件作為開發人員環境，您可以在其中使用與 Azure 一致的 Api 和工具進行開發。 Azure Stack 開發套件不適用於作為生產環境。

## <a name="azure-stack-one-cloud-ecosystem"></a>Azure Stack 單一雲端生態系統

您可以使用完整的 Azure 生態系統來加速 Azure Stack 方案：

<!-- docsTest:casing "EMC Services" "Infront Consulting Group" "HPE Pointnext" -->
<!-- cSpell:ignore ISVs Bitnami DXC EMC Infront Yourhosting Revera Avanade Pulsant PWC PricewaterhouseCoopers -->

- Azure 可確保大部分已通過 Azure 認證的應用程式和服務都可在 Azure Stack 上運作。 有數個 Isv 將其解決方案擴充至 Azure Stack。 這些 Isv 包括 Bitnami、Docker、kemp 技術、pivotal cloud foundry、Red Hat enterprise Linux 和 suse Linux。
- 您可以選擇讓 Azure Stack 提供這些解決方案，並以完全受控服務的方式運作。 數個合作夥伴很快就會在 Azure 和 Azure Stack 上提供受控服務供應專案。 這些合作夥伴包括 tieto、Yourhosting、Revera、Pulsant 和 ntt。 這些合作夥伴會透過雲端解決方案提供者 (CSP) 方案，為 Azure 提供受控服務。 他們正在擴充其供應專案，以納入混合式解決方案。
- 以完整且完全受控的混合式雲端解決方案作為範例，Avanade 提供了多項供應專案。 它包含雲端轉換服務、軟體、基礎結構、設定和設定，以及持續受管理的服務。 如此一來，客戶就可以使用 Azure Stack，就像現在的 Azure 一樣。
- 提供者可為客戶建立端對端的 Azure 解決方案，協助加速應用程式現代化方案。 每個提供者都帶來深層的 Azure 技能、領域和產業知識，以及 DevOps 等程式專長。 每個 Azure Stack 的執行都是一個機會，可讓提供者設計解決方案和潛在客戶，並影響系統部署。 他們也可以自訂包含的功能，並提供營運活動。 提供者的範例包括 Avanade、DXC、Dell EMC 服務、Infront 諮詢群組、HPE Pointnext 和 PWC (先前的 PricewaterhouseCoopers) 。
