---
title: 混合式、多重雲端和邊緣的整合作業
description: 跨混合式、多重雲端和邊緣部署，執行一致作業管理的有效控制。
author: brianblanchard
ms.author: brblanch
ms.date: 01/12/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: strategy
ms.custom: e2e-hybrid
ms.openlocfilehash: 1f1385095de368d52e4a4790c0267defbc96bb80
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101795013"
---
# <a name="introduction-to-unified-operations"></a>整合作業簡介

跨混合式、多重雲端和邊緣的一個雲端儀表板。

混合式、多重雲端和邊緣部署方法通常會導致營運成本增加。 非預期的成本增加是重複或不同作業的結果，每個雲端提供者都有一組操作作法。 **整合作業** 是一種刻意維護一組工具和程式的方法，可透過一組通用治理和操作管理實務來一致地管理每個雲端提供者。

## <a name="understand-and-minimize-costs-through-unified-operations"></a>透過整合作業瞭解並將成本降至最低

在混合式和多重雲端策略中，第一次增加的額外成本可能是重複的雲端平臺公用程式：網路、身分識別、治理、安全性和操作工具。 長期來說，可能會出現商務挑戰，例如人員管理核心功能或小組，以及管理不同環境所需的技能。

混合式和多重雲端策略促使許多決策產生者不正確地得出雲端比內部部署技術更昂貴。 最近由 Microsoft Forrester 的諮詢研究，發現混合式和多重雲端策略可以提供非常重要的 [三年投資報酬率，並大幅避免組織的內部部署基礎結構和員工成本](https://azure.microsoft.com/resources/forrester-tei-microsoft-azure-iaas/) 。 Accenture 和 wservice 的主要環境和能源研究可進一步提高大型部署的能源使用效率，讓 [組織能針對內部部署的商務應用程式減少能源使用和碳排放量超過 30%](https://download.microsoft.com/download/7/3/9/739BC4AD-A855-436E-961D-9C95EB51DAF9/Microsoft_Cloud_Carbon_Study_2018.pdf)，而針對小型部署，則會在共用雲端服務中達到90% 以上的降低成本。

組織可以使用簡單的方法來克服風險、額外負荷成本增加，或與人員配置核心功能相關的挑戰，以將整體作業現代化並優化。 「**整合作業**」是混合式、多重雲端和邊緣雲端策略的方法，可減少技術人員的短期重複和長期負擔。 本文說明在混合式、多重雲端和邊緣環境中，使用整合作業將單一企業控制平面延伸至分散式資產的方式，是提供者中立的方法。

接下來會有更多文章，概述整合作業的 Azure 方法：跨異類混合式、多重雲端和邊緣環境提供 [治理](./govern.md) 和 [作業管理](./manage.md) 。 Azure 特定的整合作業方法的整體目標是在任何基礎結構上的任何地方清查、組織和治理 IT 資產。 此集中式企業控制平面可在內部部署、多重雲端和邊緣環境之間提供一致的雲端營運管理體驗。

## <a name="primary-cloud-platform"></a>主要雲端平臺

成功的混合式、多重雲端和邊緣策略都是從主要雲端平臺開始。

![具有設備、服務及控制項的主要雲端平臺，可支援您的程式。](../../_images/hybrid/primary-cloud-provider.png)

無論位於公用或私用雲端，您的主要雲端平臺都是您的操作程式所裝載的位置，以及一組定義的雲端設備。 在 Azure 中，這些設施是 [azure 區域](https://azure.microsoft.com/global-infrastructure/)，而內部部署則是資料中心。 這些功能裝載了管理核心作業所需的雲端服務，並支援平臺上裝載的其他工作負載。 您的主要雲端平臺也會包含一系列的控制項，其設計目的是要支援該雲端中的作業。

> [!NOTE]
> 您的主要雲端平臺可能不會裝載您的工作負載的全部（甚至是多數），但會 **裝載完成營運管理、治理、合規性、安全性等核心程式所需的服務和控制項**。
>
> [!CAUTION]
> 您可能已經有主要雲端平臺。 可惜的是，許多雲端平臺都是在作業需要混合式、多重雲端或邊緣部署選項之前設計和建立。 這通常會導致客戶複寫程式，並使用不同的雲端控制項來管理每個雲端平臺上的雲端服務。 如果您的雲端策略呼叫混合式、多重雲端或邊緣部署選項， **而** 您的主要雲端平臺並不支援這些選項，請考慮可針對整合作業部署必要功能的平臺。
>

## <a name="defining-unified-operations"></a>定義整合作業

整合作業運作方式的概念很簡單：執行延伸模組或閘道，以在您的混合式、多重雲端或邊緣部署之間套用主要雲端提供者中的控制項。 以一致的方式跨內部部署、多重雲端和邊緣環境管理及管理您的作業。

在執行整合作業時，單一企業控制平面會延伸至您組織的分散式資產，以大規模的方式將一致的管理、應用程式開發和雲端服務帶入任何基礎結構。 為組織啟用一致的管理與治理，具有這類雲端控制項的閘道可在不同的內部部署、多重雲端和邊緣環境之間擴充一致的作業管理和資料服務。

識別您的主要雲端平臺時，請務必確保雲端具有必要的工具組，以管理您組合中的所有雲端。 許多雲端平臺都是在作業需要混合式、多重雲端或邊緣部署選項之前設計和建立。 目前操作工具中的功能不足，可能會要求作業小組複寫程式-使用不同的雲端控制項來管理每個雲端平臺上的雲端服務。 如果您的雲端策略呼叫混合式、多重雲端或邊緣部署選項， **而** 您的主要雲端平臺並不支援這些選項，您應該考慮可以針對整合作業部署必要功能的平臺。

## <a name="unified-operations"></a>整合作業

大規模分散分散資產組合的單一雲端管理和操作經驗 (將一致的治理、管理、應用程式開發和雲端服務整合到任何基礎結構上，) 提供整合的混合式和多重雲端策略，可增加組織未來的創新、靈活性和業務成長。 新增雲端控制項的閘道，可將管理和資料服務擴充至內部部署、多重雲端和邊緣，讓組織能夠進行一致的管理與管理;這是一種不可或缺的混合式和多重雲端策略，可在任何地方增加您組織未來的創新、靈活性和業務成長。 在您的混合式、多重雲端或邊緣部署中，執行延伸模組 (或閘道) ，以將主要雲端提供者中的控制項套用。

![整合作業可將雲端控制項延伸至混合式、多重雲端和邊緣部署](../../_images/hybrid/primary-cloud-provider-extended.png)

> [!WARNING]
> 整合作業的執行可能相當簡單。 但是，如果您的雲端平臺無法管理必要的主要整合作業程式，則需要額外的資本支出，並具備昂貴的開發，以建立延伸模組或其他雲端的閘道。 因為現有的主要雲端平臺具有這類限制，所以客戶建立重複或已斷開作業和程式的主要限制因素是因為現有的主要雲端平臺。
>
> 執行整合作業不一致的方法，可能會使組織的成本不佳，因為重複的雲端平臺公用程式或作業) 工具 (的營運成本提高，而不會有負面的業務影響 (不需要適當的雲端技能) 的人員。
>

如果您目前的主要雲端提供者未提供整合作業所需的功能，請考慮使用新式雲端提供者將您的作業和流程優化。

## <a name="unified-operations-decomposed"></a>已分解的整合作業

此影像會顯示整合作業所需的個別元件，並顯示它們彼此互動的方式。 下列各節提供每個整合作業元件的詳細大綱。

![資訊圖會顯示本文其餘部分所述的 (提供整合作業所需的元件) ](../../_images/hybrid/unified-operations.png)

## <a name="customer-processes"></a>客戶流程

整合作業的主要目標是在整個部署中建立盡可能多的進程一致性。 雲端服務提供者在所有混合式、多重雲端和邊緣部署中都無法達到100% 的功能同位。 但是，提供者應該能夠在所有部署中提供通用的基準功能集，讓您的 [治理](./govern.md) 和 [作業管理](./manage.md) 程式保持一致。

![整合作業可以支援的客戶流程](../../_images/hybrid/unified-operations-customer-processes.png)

最常見的情況是，客戶需要能夠在其定義的治理和作業管理程式中提供一致性。 為了符合長期需求，您的整合作業解決方案必須能夠調整規模，以符合以下指定的常見程式。

### <a name="common-governance-processes-tasks"></a> (工作的常見治理流程) 

- **成本管理：** 查看、管理或優化成本，並 **針對雲端相關的 IT 支出風險，找出並提供緩和指導** 方針。
- 安全性基準-從建議的安全性控制進行審核、套用或自動化需求，以及 **識別和提供安全性相關商務風險的風險降低指引**。
- **資源一致性：** 上架、組織及設定資源和服務，並 **識別並提供潛在業務風險的風險降低指引**。
- 身分 **識別基準：** 跨使用者身分識別和存取強制執行驗證和授權，並 **針對潛在的身分識別相關商務風險，識別並提供風險降低的指引**。
- **部署加速：** 使用範本、自動化和管線 (進行部署、設定對齊，以及可重複使用的資產) 、 **建立原則以確保符合規範、一致且可重複的資源部署和** 設定，以促進一致性。

### <a name="common-operations-management-processes-tasks"></a>常見的作業管理進程 (工作) 

- **清查和可見度：** 帳戶並確保所有資產的報告，並 **在企業級環境中收集和監視您的清查執行狀態**。
- **優化作業：** 追蹤、修補和優化支援的資源，並將 **不一致修補程式管理的設定漂移或弱點的安全中斷風險降至最低**。
- **保護和復原：** 備份、商務持續性和嚴重損壞修復的最佳作法，並 **減少無可避免發生中斷的持續時間和影響**。
- [平臺作業](../../manage/azure-management-guide/platform-specialization.md)：適用于一般技術平臺的特殊作業，例如 SQL、WVD 和 SAP (，適用于中型到高重要性的工作負載) 。
- [工作負載作業](../../manage/azure-management-guide/workload-specialization.md)：特殊作業 (適用于高優先順序/任務關鍵性的工作負載，) 具有更高的作業需求。

平臺和工作負載作業都會執行相當的 *反復* 程式來 **改善系統設計、自動補救、使用服務類別目錄調整變更，以及持續改善系統設計、自動化和調整規模**。

您的主要雲端平臺應該能夠提供所需的技術功能和工具，以將程式自動化，並達到上述目標以進行治理和操作管理。 您的整合作業解決方案應可讓您跨所有混合式、多重雲端和邊緣部署延伸這些程式。

## <a name="primary-cloud-controls"></a>主要雲端控制項

您的主要雲端平臺應包含一些重要功能，以協助或自動化雲端中通常需要的客戶程式：

![常見的雲端控制項，列在下列專案符號中](../../_images/hybrid/unified-operations-cloud-controls.png)

### <a name="basic-features"></a>基本功能

需要這些基本功能，才能大規模提供雲端採用方案：

- **搜尋、編制索引、群組及標記** 所有已部署的資產，以擴充基本的可見度和管理。
- 針對一致部署 **範本化、自動化及擴充工具**。
- **建立存取權和安全性界限** ，以保護已部署的資產。

### <a name="enhanced-features"></a>增強功能

您可能會需要下列所有增強功能，以大規模運作混合式和多重雲端環境：

- **效能和清查報告**
- **安全性和合規性審核和自動化**
- **追蹤和報告應用程式和相依性**

### <a name="automated-controls"></a>自動化控制項

使用工具將您的作業現代化，並將營運成本優化，讓您的環境自動化：

- **環境和來賓原則**
- **設定和更新**
- **保護和復原**

這些功能可能已包含在您目前用來操作主要雲端提供者的控制項集中。 這組控制項當中可能有許多額外的功能和自動化程式可供使用。 這些是您在整合作業解決方案中可跨混合式、多重雲端和邊緣使用的主要控制功能。

這是因為它們是實作為主要控制項，而這些都是我們經常會看到導致已完成或重複作業的功能。 如先前所述，執行整合作業的不一致方法，其影響可能來自于增加的營運成本 (例如，重複的雲端平臺公用程式、操作工具) ，可以將組織的成本效率降低，並在雲端採用旅程的初期階段產生重大的資本支出。

### <a name="hybrid-multicloud-gateway-and-enterprise-control-plane"></a>混合式、多重雲端閘道和企業控制平面

若要擴充您的主要雲端控制項，您必須設定擴充功能或閘道。 這種延伸模組可讓您的控制項查看已部署到雲端平臺以外的資源，並與其互動 (**事實上，在不同的異類環境) 之間建立一個控制平面和更高的可見度** 。

在 Microsoft 的雲端平臺中， [**Azure Arc**](/azure/azure-arc/overview) 是該擴充功能。 Azure Arc 會將用來管理 Azure 雲端的相同控制項和程式延伸至其他公用和私人雲端和邊緣。 這是這些雲端控制項，可在不同的內部部署、多重雲端和邊緣環境之間，提供一致的治理和作業管理程式。

整合作業可將 [**ARM**](/azure/azure-resource-manager/management/overview) 的範圍延伸 (Azure Resource Manager) ，也就是 azure 的「作業系統」。 ARM 可在 Azure 外部進行，以在 Azure 中投影這些分散的資源，並將它們表示為第一級公民。 藉由將 Azure 服務和管理功能帶入任何一種基礎結構，整合的作業方法可延伸 Azure 的觸及範圍，並提供新的混合式和多重雲端解決方案。

使用統一的作業方法，可讓您在任何地方組織、管理及保護任何環境，並集中可見度、營運和合規性。 使用標準化的應用程式服務，從部署到監視，大規模地在任何地方建立雲端應用程式。 使用一律最新的 Azure Arc 啟用服務，以更快速、一致且大規模的方式部署 Azure 服務。

跨傳統、雲端原生和分散式 edge 應用程式建立、操作及管理，並以一致的控制和流程管理治理和營運管理，將雲端創新擴充到散佈的資產。 新的混合式和多重雲端案例可以從簡化的管理、更快速的應用程式開發和一致的 Azure 服務（在任何基礎結構上，延伸至整個 IT 資產的所有資源環境）中解除鎖定。

著重于標準化、互通性和合規性的中央 Azure 控制平面可在混合式和多重雲端基礎結構之間提供一致的可見度和統一治理和作業管理，進而提高生產力、降低風險，以及加速組織的雲端採用和遷移實務和技術。

## <a name="next-steps"></a>下一步

若要開始使用您的混合式和多重雲端旅程，請先快速審視 [混合式和多重雲端文章的策略](./strategy.md)。
