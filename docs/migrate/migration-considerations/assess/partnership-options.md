---
title: 了解合作關係和支援選項
description: 使用適用于 Azure 的雲端採用架構，瞭解支援遷移成本的合作選項和方法。
author: BrianBlanchard
ms.author: brblanch
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 6ed3f2231350ecf93a561600946e13419f630735
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88575259"
---
<!-- cSpell:ignore CSPs MSPs -->

# <a name="understand-partnership-options"></a>了解合作關係選項

在移轉期間，雲端採用小組會實際地將工作負載移轉至雲端。 不同於在定義[數位資產](../../../digital-estate/index.md)或建置核心雲端基礎結構時的協作工作和問題解決工作，移轉更像是一系列重複執行的工作。 除了重複層面外，也可能有需要深入了解所選雲端提供者的測試和調整工作。 此程序的重複性質有時可由合作夥伴完美處理，從而降低對全職員工的負擔。 此外，當重複性程序遇到執行異常時，合作夥伴或許可以更好地配合深度的技術專業知識。

合作夥伴往往會和一家或少數幾家雲端廠商密切配合。 為了更清楚地說明合作關係選項，本文其餘部分假設所選中的雲端提供者是 Microsoft Azure。

在計劃、建置或遷移期間，公司一般會有四個執行合作關係選項：

- **引導式自助服務。** 現有的技術小組會執行移轉，並由 Microsoft 提供協助。
- **FastTrack for Azure。** 使用 Microsoft FastTrack for Azure 計劃來加速移轉。
- **解決方案合作夥伴。** 與 Azure 合作夥伴或雲端解決方案提供者 (Csp) ，以加速遷移。
- **支援的自助服務。** 由獲得 Microsoft 支援的現有技術人員來完成執行。

## <a name="guided-self-service"></a>引導式自助服務

如果組織自行規劃 Azure 移轉，Microsoft 隨時都能從旁協助其走完整個旅程。 為了協助組織快速移轉至 Azure，Microsoft 及其合作夥伴開發了一組廣泛的架構、指南、工具和服務，以降低風險並加快虛擬機器、應用程式和資料庫的移轉速度。 這些工具和服務支援廣泛的作業系統、程式設計語言、架構及資料庫等選擇。

- **評估和移轉工具。** Azure 提供各種工具供您在雲端轉換的不同階段使用，包括評估現有的基礎結構。 如需詳細資訊，請參閱下面「遷移」一章中的「評定」一節。
- **[Microsoft 雲端採用架構](../../index.md)。** 此架構為雲端採用和移轉提供了結構化的方法。 它是以各種 Microsoft 支援的客戶參與專案為基礎，並以一系列的步驟來組織，從架構和設計到執行。 在每個步驟中，支援指引可協助您設計應用程式架構。
- **[雲端設計模式](/azure/architecture/patterns)。** Azure 提供了一些實用的雲端設計模式供您在雲端中建置可靠、可擴充且安全的工作負載。 每個模式都會說明模式處理的問題、套用模式的考量，以及以 Azure 為基礎的範例。 大部分的模式會包括程式碼範例或程式碼片段，示範如何在 Azure 上實作模式。 不過，無論是裝載于 Azure 或其他雲端平臺上，它們都與任何分散式系統相關。
- **[雲端基本](/azure/architecture/guide)概念。** 基本概念有助於教導核心概念實作的基本方法。 此指南可協助技術人員思考超越單一 Azure 服務的解決方案。
- **[範例案例](/azure/architecture/example-scenario)。** 此指南提供得自實際客戶實作的參考，會概述以往的客戶為了達成特定業務目標所遵循的工具、方法和程序。
- **[參考架構](/azure/architecture/reference-architectures)。** 參考架構是依案例排列，將相關架構群組在一起。 每個架構都包含最佳做法，以及可調整性、可用性、管理性和安全性的考慮。 大部分還包含可部署的解決方案。

## <a name="fasttrack-for-azure"></a>FastTrack for Azure

[FastTrack for Azure](https://azure.microsoft.com/programs/azure-fasttrack) 提供來自 Azure 工程師的直接協助，與夥伴共同協助客戶快速、放心地建置 Azure 解決方案。 FastTrack 會帶來實際客戶體驗的最佳做法和工具，引領客戶一路完成 Azure 解決方案的安裝、設定、開發和生產，包括：

- 資料中心移轉
- Azure 上的 Windows Server
- Azure 上的 Linux
- SAP on Azure
- 業務持續性和災害復原 (BCDR)
- 高效能運算
- 雲端原生應用程式
- DevOps
- 應用程式現代化
- 雲端規模分析
- 智慧型應用程式
- 智慧型代理程式
- 移往 Azure 的資料現代化
- 安全性與管理
- 全域散發的資料
- Windows 虛擬桌面
- Azure Marketplace
- 基本概念和治理

在 FastTrack for Azure 的典型互動期間，Microsoft 會協助您確定企業願景，以成功規劃及開發 Azure 解決方案。 該小組會評估架構需求，並提供指導方針、設計準則、工具與資源，以協助您建置、部署及管理 Azure 解決方案。 該小組可依要求匹配經驗豐富而可提供部署服務的合作夥伴，並定期聯繫以確保部署進展順利，並協助排除阻礙。

FastTrack for Azure 的典型互動主要階段如下：

- **發現。** 找出重要的專案關係人、針對所要解決的問題了解其目標或願景，然後評估架構需求。
- **實現解決方案。** 了解建置應用程式的設計原則、檢閱應用程式與解決方案的架構，並取得指導和工具，以推動概念證明 (PoC) 到生產階段的落實。
- **持續的合作關係。** Azure 工程師和計劃經理會定期聯繫，以確保部署進展順利以及協助排除阻礙。

## <a name="microsoft-services-offerings-aligned-to-cloud-adoption-framework-approaches"></a>與雲端採用架構方法配合的 Microsoft 服務供應項目

![Microsoft 服務的雲端採用架構方法](../../../_images/migrate/mcs-program-approach.jpg)

**評定：** Microsoft 服務會使用 [統一的資料和工具導向方法](https://download.microsoft.com/download/C/7/C/C7CEA89D-7BDB-4E08-B998-737C13107361/Secure_Cloud_Insights_Datasheet_EN_US.pdf) ，其中包含架構研討會、Azure 即時資訊、安全性和身分識別威脅模型，以及各種工具，可讓您深入瞭解現有 Azure 環境的挑戰、風險、建議和問題，並提供重要的結果，例如 [高階現代化藍圖](https://download.microsoft.com/download/F/7/2/F72FAD7E-8BBD-4E04-8C7B-9AC4FE04A150/Cloud_Adoption_Discovery_and_Roadmap_Datasheet.pdf)。

**採用：** 使用 Microsoft 服務的 [Azure cloud foundation](https://download.microsoft.com/download/D/8/7/D872DFD0-1C46-4145-95E4-B5EAB2958B96/Hybrid_Cloud_Foundation_Datasheet_EN_US.pdf) ，藉由將您的需求對應至最適當的參考架構和規劃、設計及部署工作負載所需的基礎結構、管理、安全性和身分識別，建立您的核心 azure 設計、模式和治理架構。

**遷移/優化：** Microsoft 服務的 [雲端現代化解決方案](https://download.microsoft.com/download/3/7/3/373F90E3-8568-44F3-B096-CD9C1CD28AB7/Cloud_Modernization_Datasheet_EN_US.pdf) 提供了全方位的方法，可將應用程式和基礎結構移至 Azure，並在雲端部署之後優化和現代化，並以簡化的方式獲得支援。

**創新：** Microsoft 服務的 [卓越雲端中心 (CCoE) 解決方案](https://download.microsoft.com/download/F/8/B/F8BBE4BD-E5F8-4DFB-82F7-C0A4E17051BB/Cloud_Center_of_Excellence_Datasheet_EN_US.pdf) 提供了 DevOps 的輔導參與，並使用 DevOps 準則結合了規範的雲端原生服務管理和安全性控制，以協助推動業務創新、提高靈活性，並減少安全、可預測且彈性的服務傳遞和作業管理功能中的價值。

## <a name="azure-support"></a>Azure 支援

如果您有問題或需要協助，請[建立支援要求](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)。 如果您的支援要求需要深入的技術指引，請造訪 [Azure 支援計畫](https://azure.microsoft.com/support/plans) ，以符合您需求的最佳方案。

## <a name="azure-solutions-partner"></a>Azure 解決方案合作夥伴

Microsoft 認證解決方案提供者專精於根據全球各地的 Microsoft 技術，提供新型客戶解決方案。 透過具經驗之合作夥伴的協助，在雲端上最佳化您的業務。

從可以提供現成或自訂 Azure 解決方案的合作夥伴，以及可以部署和管理這些解決方案的合作夥伴取得協助：

- **[尋找雲端解決方案合作夥伴](https://www.microsoft.com/solution-providers/home)。** 認證 CSP 可協助您充分利用雲端，方法是評估雲端採用的業務目標，以及找出可符合業務需求並協助企業變得更敏捷且更有效率的適當雲端解決方案。
- **[尋找受控服務合作夥伴](https://www.microsoft.com/solution-providers/search?cacheId=16a3b49b-fef2-449d-bdf0-628008114cca)。** Azure 受控服務合作夥伴 (MSP) 可藉由引導雲端旅程的所有層面來協助企業轉移至 Azure。 從諮詢、移轉和營運管理，雲端 MSP 會向客戶展示採用雲端所能帶來的所有好處。 它們也會以一種方式來處理常見的支援、布建和帳單體驗，並具備彈性的隨用隨付商務模型。

## <a name="next-steps"></a>後續步驟

在選取合作夥伴和支援策略後，即可更新[發行和反覆執行待辦項目](./release-iteration-backlog.md)以反映所規劃的工作和任務。

> [!div class="nextstepaction"]
> [使用發行和反覆執行待辦項目來管理變更](./release-iteration-backlog.md)
