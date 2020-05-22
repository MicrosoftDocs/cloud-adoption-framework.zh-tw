---
title: Azure 安全性指引
description: 使用 Microsoft 服務信任入口網站和合規性管理員，協助符合複雜的合規性義務並改善資料保護。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: governance
ms.openlocfilehash: 6307cb792b6c2cbbda472d1620a91ebdffc18845
ms.sourcegitcommit: 9a84c2dfa4c3859fd7d5b1e06bbb8549ff6967fa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2020
ms.locfileid: "83755911"
---
<!-- cSpell:ignore DPIAs -->

<!-- markdownlint-disable MD026 -->

# <a name="microsoft-security-guidance"></a>Microsoft 安全性指導方針

## <a name="tools"></a>工具

[Microsoft 服務信任入口網站](https://servicetrust.microsoft.com)和合規性管理員，以協助下列各項：

- 克服合規性管理的挑戰。
- 履行符合法規需求的責任。
- 執行企業雲端服務使用率的自助審核和風險評估。

這些工具的設計是為了在選擇和使用 Microsoft 雲端服務時，協助組織符合複雜的合規性義務並改善資料保護功能。

**Microsoft 服務信任入口網站**提供深入的資訊和工具，可協助您滿足使用 Microsoft 雲端服務的需求，包括 Azure、Office 365、Dynamics 365 和 Windows。 入口網站是與 Microsoft 雲端相關的安全性、法規、合規性和隱私權資訊的一站。 我們在這裡發佈執行雲端服務和工具自助風險評量所需的資訊和資源。 建立入口網站是為了協助追蹤 Azure 內的法規合規性活動，包括：

- **合規性管理員：** 合規性管理員是以工作流程為基礎的風險評估工具，在 Microsoft 服務信任入口網站中，可讓您追蹤、指派及驗證貴組織與 Microsoft 雲端服務相關的法規合規性活動，例如 Office 365、Dynamics 365 和 Azure。 您可以在下一節找到更多詳細資料。
- **信任檔：** 目前有三種類別的指南，可為您提供豐富的資源來評估 Microsoft 雲端;瞭解 Microsoft 在安全性、合規性和隱私權方面的作業;並協助您採取行動以改善資料保護功能。 這些包括：
- **審查報告：** 審查報告可讓您隨時掌握 Microsoft 雲端服務的最新隱私權、安全性及合規性相關資訊。 這包括 ISO、SOC、FedRAMP 和其他與 Microsoft 雲端服務（例如 Azure、Office 365、Dynamics 365 及其他）的獨立協力廠商審核相關的報告、橋接器字母和材質。
- **資料保護指南：** 資料保護指南提供 Microsoft 雲端服務如何保護您的資料，以及如何為您的組織管理雲端資料安全性與合規性的相關資訊。 這包括深入的白皮書，提供有關 Microsoft 如何設計和操作雲端服務、常見問題檔、年度安全性評量的報告、滲透測試結果，以及協助您進行風險評估並改善資料保護功能的指引。
- **Azure 安全性與合規性藍圖：** 藍圖提供的資源可協助您建立及啟動雲端提供的應用程式，以協助您遵守嚴格的法規和標準。 具有比其他任何雲端提供者更多的認證，您可以有信心地將關鍵工作負載部署至 Azure，使用包含下列項目的藍圖：
  - 業界特定的總覽和指引。
  - 客戶責任對照表。
  - 使用威脅模型的參考架構。
  - 控制執行矩陣。
  - 自動化以部署參考架構。
  - 隱私權資源。 提供資料保護影響評量、資料主體要求和資料缺口通知的檔，以納入您自己的責任計畫，以支援一般資料保護規定（GDPR）。
- **開始使用 GDPR：** Microsoft 產品和服務可協助組織在收集或處理個人資料時符合 GDPR 需求。 Microsoft 服務信任入口網站的設計，是為了提供您可以用來解決特定 GDPR 需求之 Microsoft 服務中的功能相關資訊。 文件可協助您的 GDPR 權責和了解技術和組織措施。 提供資料保護影響評量、資料主體要求和資料缺口通知的檔，以納入您自己的責任計畫，以支援 GDPR。
  - **資料主體要求：** GDPR 會授與個人（或資料主體）與處理其個人資料的特定許可權。 包括修正不正確資料、清除資料或限制其處理的權限，以及接收其資料和履行將資料傳送給另一個控制器之要求的權限。
  - **資料缺口：** GDPR 會在有違反個人資料的情況時，要求資料控制器和處理器的通知需求。 Microsoft 服務信任入口網站提供您有關 Microsoft 如何嘗試預防缺口的資訊、Microsoft 如何偵測缺口，以及 Microsoft 如何在發生缺口時做出回應，並將您通知為數據控制站。
  - **資料保護影響評量：** Microsoft 協助控制器完成 GDPR 資料保護影響評量（Dpia）。 GDPR 提供 Dpia 必須執行之案例的詳盡清單，例如分析和類似活動的用途自動化處理;處理大規模的個人資料，以及大規模的可公開存取區域的系統化監視。
  - **其他資源：** 除了上述各節所討論的工具指引之外，Microsoft 服務信任入口網站也提供其他資源，包括區域合規性、安全性與合規性中心的其他資源，以及有關 Microsoft 服務信任入口網站、合規性管理員、隱私權和 GDPR 的常見問題。
- **區域合規性：** Microsoft 服務信任入口網站提供許多符合性檔和指引，讓 Microsoft 線上服務符合不同區域（包括捷克文共和國、波蘭和羅馬尼亞）的合規性需求。

## <a name="unique-intelligent-insights"></a>唯一的 Intelligent Insights

隨著安全性訊號的數量和複雜度成長，判斷這些訊號是否為可信的威脅然後再做反應，會耗用太多時間。 Microsoft 提供在雲端傳遞，無與倫比廣度的安全性智慧，協助快速偵測和修復威脅。 如需詳細資訊，請參閱[Azure 資訊安全中心總覽](https://docs.microsoft.com/azure/security-center/security-center-intro)。

## <a name="azure-threat-intelligence"></a>Azure 威脅情報

使用資訊安全中心可用的威脅情報選項，IT 系統管理員可以識別對環境的安全性威脅。 例如，識別特定的電腦是否屬於殭屍網路。 如果攻擊者偷偷地安裝惡意程式碼，暗中將此電腦連接到命令和控制項，則電腦可能會成為殭屍網路中的節點。 威脅情報也可以識別來自地下通訊通道 (例如暗網) 的潛在威脅。

為了建置此威脅情報，資訊安全中心會使用來自 Microsoft 內多個來源的資料。 資訊安全中心將會使用這項資料來識別您環境的潛在威脅。 [威脅情報] 窗格是由三個主要選項所組成：

- 偵測到的威脅類型
- 威脅來源
- 威脅情報對應

## <a name="machine-learning-in-azure-security-center"></a>Azure 資訊安全中心的機器學習

Azure 資訊安全中心深入分析豐富的資料，這些資料來自 Microsoft 和各個合作夥伴解決方案，協助您達成更高的安全性。 為了利用這項資料，公司使用資料科學與機器學習服務來進行威脅預防、偵測及最終調查。

廣義來說，Azure Machine Learning 可協助達到兩個結果：

## <a name="next-generation-detection"></a>新一代偵測

攻擊者越來越自動化及複雜。 他們也使用資料科學。 他們會對保護進行反向工程，並建置系統以支援行為的突變。 它們會將其活動偽裝成雜訊，並從錯誤快速學習。 機器學習可協助我們回應這些發展。

## <a name="simplified-security-baseline"></a>簡化的安全性基準

進行有效率的安全性決策並不容易。 需要安全性經驗和專業知識。 有些大型組織在員工方面有這類專家，但許多公司都不這麼做。 Azure Machine Learning 可讓客戶在進行安全性決策時，受益於其他組織的智慧。

## <a name="behavioral-analytics"></a>行為分析

行為分析是一種可分析及比較資料與一組已知模式的技術。 這些模式不是簡單的簽章。 它們是透過會套用至大型資料集的複雜機器學習演算法來決定。 它們也是由專家分析師仔細分析惡意行為所決定。 Azure 資訊安全中心可以使用行為分析，根據虛擬機器記錄、虛擬網路裝置記錄、網狀架構記錄、毀損傾印和其他來源的分析，來識別遭到入侵的資源。
