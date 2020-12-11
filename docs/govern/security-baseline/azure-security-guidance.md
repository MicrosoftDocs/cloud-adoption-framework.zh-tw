---
title: Azure 安全性指引
description: 使用 Microsoft 服務信任入口網站和合規性管理員來協助符合複雜的合規性義務，並改善資料保護。
author: BrianBlanchard
ms.author: brblanch
ms.date: 09/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: govern
ms.custom: internal
ms.openlocfilehash: 787a3fb046e4bebdd25b6fbd40b4b8139e91bd32
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97019259"
---
<!-- cSpell:ignore DPIAs inexhaustive -->

# <a name="microsoft-security-guidance"></a>Microsoft 安全性指引

## <a name="tools"></a>工具

[Microsoft 服務信任入口網站](https://servicetrust.microsoft.com)和合規性管理員可協助滿足下列需求：

- 克服合規性管理挑戰。
- 履行符合法規要求的責任。
- 進行自助式審核和企業雲端服務使用率的風險評量。

這些工具的設計目的是協助組織符合複雜的合規性義務，並在選擇和使用 Microsoft 雲端服務時改善資料保護功能。

**Microsoft 服務信任入口網站** 提供深入的資訊和工具，以協助您滿足使用 Microsoft 雲端服務的需求，包括 Azure、Microsoft 365、Dynamics 365 和 Windows。 入口網站是與 Microsoft 雲端相關的安全性、法規、合規性和隱私權資訊的一次站。 我們在這裡發佈執行雲端服務和工具自助風險評量所需的資訊和資源。 入口網站的建立可協助追蹤 Azure 中的法規合規性活動，包括：

- **合規性管理員：** 合規性管理員是 Microsoft 服務信任入口網站中以工作流程為基礎的風險評估工具，可讓您追蹤、指派及確認組織與 Microsoft 雲端服務相關的法規合規性活動，例如 Microsoft 365、Dynamics 365 和 Azure。 您可以在下一節找到更多詳細資料。
- **信任檔：** 有三種類別的指南可提供豐富的資源來評估 Microsoft 雲端、瞭解 Microsoft 在安全性、合規性和隱私權方面的營運，以及協助您改善資料保護功能。 這些指南包括：
- **Audit reports：** Audit reports 可讓您隨時掌握最新的隱私權、安全性和 Microsoft 雲端服務的合規性相關資訊。 這項資訊包括 ISO、SOC、FedRAMP 和其他的審核報表、bridge 信件，以及與 Microsoft 雲端服務（例如 Azure、Microsoft 365、Dynamics 365 和其他）的獨立協力廠商審核相關的資料。
- **資料保護指南：** 資料保護指南提供有關 Microsoft 雲端服務如何保護您資料的資訊，以及您可以如何管理組織的雲端資料安全性和合規性。 這些指南包含有關 Microsoft 雲端服務的設計和操作的詳細白皮書、常見問題檔、年度安全性評定報告、滲透測試結果，以及協助您進行風險評估及改善資料保護功能的指引。
- **Azure 安全性與合規性藍圖：** 藍圖提供的資源可協助您建立及啟動雲端提供的應用程式，協助您遵守嚴格的法規和標準。 具有比其他任何雲端提供者更多的認證，您可以有信心地將關鍵工作負載部署至 Azure，使用包含下列項目的藍圖：
  - 業界專屬的總覽和指引。
  - 客戶責任矩陣。
  - 具有威脅模型的參考架構。
  - 控制項執行矩陣。
  - 自動化以部署參考架構。
  - 隱私權資源。 系統會提供資料保護影響評量、資料主體要求和資料缺口通知的檔，以納入您自己的責任計畫，以支援一般資料保護規定 (GDPR) 。
- **GDPR 入門：** Microsoft 產品和服務可協助組織在收集或處理個人資料時滿足 GDPR 需求。 Microsoft 服務信任入口網站的設計目的，是提供您 Microsoft 服務中可用來解決特定需求的相關資訊。 文件可協助您的 GDPR 權責和了解技術和組織措施。 系統會提供資料保護影響評量、資料主體要求和資料缺口通知的檔，以納入您自己的責任計畫以支援 GDPR。
  - **資料主體要求：** GDPR 會授與個人 (或資料主體) 與處理其個人資料相關的特定許可權。 這些權利包括修正不正確的資料、清除資料或限制其處理的許可權，以及取得資料的許可權，以及完成將資料傳輸到另一個控制器的要求。
  - **資料缺口：** GDPR 會在發生個人資料違規時，要求資料控制器和處理器的通知需求。 Microsoft 服務信任入口網站提供 Microsoft 如何運作的相關資訊，以防止缺口、Microsoft 如何偵測缺口，以及 Microsoft 將如何回應，並在發生缺口時通知您成為資料控制站。
  - **資料保護影響評量：** Microsoft 可協助控制器完成 GDPR 資料保護影響評量 (Dpia) 。 GDPR 會提供必須執行 Dpia 之案例的 inexhaustive 清單，例如程式碼剖析和類似活動的自動化處理;處理大量特殊的個人資料，以及大規模的可公開存取區域的系統監視。
  - **其他資源：** 除了上述各節所討論的工具指引以外，Microsoft 服務信任入口網站也提供其他資源，包括區域合規性、安全性與合規性中心的額外資源，以及有關 Microsoft 服務信任入口網站、合規性管理員、隱私權和 GDPR 的常見問題。
- **區域合規性：** Microsoft 服務信任入口網站提供許多合規性檔和指引，讓 Microsoft 線上服務符合不同區域的合規性需求，包括捷克共和國、波蘭及羅馬尼亞。

## <a name="unique-intelligent-insights"></a>唯一的 Intelligent Insights

隨著安全性訊號的數量和複雜度成長，判斷這些訊號是否為可信的威脅然後再做反應，會耗用太多時間。 Microsoft 提供在雲端傳遞，無與倫比廣度的安全性智慧，協助快速偵測和修復威脅。 如需詳細資訊，請參閱 [Azure 資訊安全中心總覽](/azure/security-center/security-center-intro)。

## <a name="azure-threat-intelligence"></a>Azure 威脅情報

使用資訊安全中心可用的威脅情報選項，IT 系統管理員可以識別對環境的安全性威脅。 例如，識別特定的電腦是否屬於殭屍網路。 如果攻擊者偷偷地安裝惡意程式碼，暗中將此電腦連接到命令和控制項，則電腦可能會成為殭屍網路中的節點。 威脅情報也可以識別來自地下通訊通道 (例如暗網) 的潛在威脅。

為了建立此威脅情報，安全性中心會使用來自 Microsoft 內多個來源的資料。 資訊安全中心將會使用這項資料來識別您環境的潛在威脅。 [威脅情報] 窗格是由三個主要選項所組成：

- 偵測到的威脅類型
- 威脅來源
- 威脅情報對應

## <a name="machine-learning-in-azure-security-center"></a>Azure 資訊安全中心的機器學習

Azure 資訊安全中心深入分析豐富的資料，這些資料來自 Microsoft 和各個合作夥伴解決方案，協助您達成更高的安全性。 為了利用這項資料，公司使用資料科學與機器學習服務來進行威脅預防、偵測及最終調查。

廣義來說，Azure Machine Learning 可協助達到兩個結果：

### <a name="next-generation-detection"></a>新一代偵測

攻擊者越來越自動化及複雜。 他們也使用資料科學。 他們會對保護進行反向工程，並建置系統以支援行為的突變。 它們會將其活動偽裝成雜訊，並從錯誤快速學習。 機器學習可協助我們回應這些發展。

### <a name="simplified-security-baseline"></a>簡化的安全性基準

進行有效率的安全性決策並不容易。 需要安全性經驗和專業知識。 雖然有些大型組織有這類專家，但許多公司都沒有。 Azure Machine Learning 可讓客戶在進行安全性決策時，受益於其他組織的智慧。

## <a name="behavioral-analytics"></a>行為分析

行為分析是一種可分析及比較資料與一組已知模式的技術。 這些模式不是簡單的特徵標記。 它們是透過適用于大量資料集的複雜機器學習演算法來決定。 它們也可以透過專業分析師仔細分析惡意行為來判定。 Azure 資訊安全中心可以使用行為分析，根據虛擬機器記錄、虛擬網路裝置記錄、網狀架構記錄、毀損傾印和其他來源的分析，來識別遭到入侵的資源。
