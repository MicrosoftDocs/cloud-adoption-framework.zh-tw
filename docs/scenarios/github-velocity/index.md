---
title: GitHub 如何加速雲端採用
description: 公司可以利用 GitHub 的解決連線性給開放原始碼的社區，並從已成功採用 Azure 服務的組織中，找到數以千計的重述、增強且現成可用的雲端解決方案範例。
author: nkpatterson
ms.author: janet
ms.date: 1/11/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: strategy
ms.custom: think-tank
ms.openlocfilehash: 9ef511df74fe14c0a2e9906bdf51e9fc3d219251
ms.sourcegitcommit: 9cd2b48fbfee229edc778f8c5deaf2dc39dfe2d6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/01/2021
ms.locfileid: "99976228"
---
# <a name="how-github-accelerates-cloud-adoption"></a>GitHub 如何加速雲端採用

## <a name="overview"></a>總覽

創新是現今在競爭環境中的新貨幣。 進行共用、串流內容、自我駕駛汽車和其他服務，徹底改變了人員的每日 rhythms，同時讓市場變得更趨流暢，並顯示競爭的環境如何從實體資產移至數位體驗。

這些類型的優越數位體驗會導致崩解，也就是完善的企業面臨來自其他公司的激烈競爭，而這些公司可以更快速地創新並提供價值給客戶。 若要對抗且避免崩解，企業必須建立創新文化，並使用最佳且最適合的工具和雲端服務。

GitHub 提供各種功能，可協助公司：

- 利用 Azure 服務和功能。
- 將其做法現代化。
- 在此文化轉變期間變得更加靈活和創新。

公司可以利用 GitHub 的解決連線性給開放原始碼的社區，並從已成功採用 Azure 服務的組織中，找到數以千計的重述、增強且現成可用的雲端解決方案範例。 他們可以輕鬆地採用並逐一查看這些解決方案，以根據其業務需求進行量身打造。

GitHub 可讓組織輕鬆地在其小組內共用，進而更快速地現代化和部署下一個應用程式或工作負載。 公司可以查看 *innersource*（創新的關鍵原則），以借用共用和重複使用、共同作業和通訊等最佳作法，並將其套用至組織內。

從保護開放原始碼套件到每日撰寫的智慧財產，保護整個軟體供應鏈的安全，應該是每家公司的主要優先事項。 這專案標需要可在整個生命週期中整合及自動化的 advanced 安全性技術，而 GitHub advanced security 和 GitHub Actions 這類原生 GitHub 功能則提供這種類型的彈性。

## <a name="take-advantage-of-open-source-assets"></a>利用開放原始碼資產

高效率的組織會將開放原始碼軟體 (OSS) 識為現代軟體發展的基本與選用項目。 他們會與其所依賴的開發人員社群交流，並使用安全的平台策略性地投資 OSS。 因此，這些組織會快速地體驗創新、超過競爭對手，以及在將風險降至最低的同時降低成本。

雖然 OSS 可解讀為併入應用程式的套件、程式庫、指令碼和相依性，但有數以千計的開放原始碼資產形式為適用於妥善定義 Azure 架構的基礎結構即程式碼 (IaC)、文件、指引和藍圖。 這些藍圖已由 Microsoft、合作夥伴、廠商、客戶和個人貢獻給 OSS 社群，並可在 GitHub 中立即取得。 您可以輕鬆地修改、重複使用，並將其部署至特定的 Azure 環境。

### <a name="infrastructure-as-code"></a>基礎結構即程式碼

基礎結構即程式碼 (IaC) 是描述性模型中的基礎結構 (網路、虛擬機器、負載平衡器和連線拓撲) 管理，其使用 DevOps 小組用於原始程式碼的相同版本控制系統。 就像相同原始程式碼可產生相同二進位檔的準則一樣，IaC 模型會在每次套用時產生相同的環境。 IaC 是搭配[持續傳遞 (CD)](/azure/devops/learn/what-is-continuous-delivery) 使用的重要 DevOps 做法。

IaC 經過演進以解決發行管線中的環境漂移問題。 若未使用 IaC，小組必須維護個別部署環境的設定，而環境間的不一致會導致部署期間發生問題。 每個環境最後都會變成一片雪花，也就是無法自動重現的唯一組態。 有了了雪花，基礎結構管理和維護需要手動處理，以因應錯誤且難以追蹤。使用 IaC 的基礎結構部署是可重複的，可防止設定漂移或遺失相依性所造成的執行時間問題。

使用 IaC 時，小組可對環境描述進行變更並控制設定模型的版本，而設定模型通常為記載完善的程式碼格式 (例如 JSON)。如需詳細資訊，請參閱 [Azure Resource Manager 範本](/azure/azure-resource-manager/templates/overview)。 開發人員可以簡化其工作流程，方法是在與應用程式原始程式碼相同的 GitHub 存放庫中裝載 IaC 程式碼，並採用相同的持續整合 (CI) /CD 做法，以 [GitHub Actions](https://github.com/features/actions)支援的 IaC。

如需如何在各種 Azure 範圍部署自訂 Resource Manager 範本的範例，請參閱 [AzOps](https://github.com/Azure/azops) GitHub 動作。 如果您不熟悉 Resource Manager 範本或 IaC，您也可以流覽 GitHub 上的 [azure 快速入門範本](https://github.com/Azure/azure-quickstart-templates) 存放庫、尋找您想要部署的範本，然後選取 [ **部署至 azure** ] 按鈕來測試其運作方式。

![[部署至 Azure] 按鈕的螢幕擷取畫面。](./media/deploy-to-azure.png)

### <a name="cloud-pattern-components-and-best-practices"></a>雲端模式元件和最佳做法

下列架構圖醒目提示在 GitHub DevSecOps 環境的 GitHub 和 Azure 元件中執行的安全性檢查：

![架構圖會醒目提示在 GitHub DevSecOps 環境的 GitHub 和 Azure 元件中，執行的安全性檢查。](./media/github-security-checks.png)

- [GitHub](https://docs.github.com) 提供程式碼裝載平臺，可讓開發人員用來共同作業開啟和 innersource 專案。

- [Codespaces](https://docs.github.com/github/developing-online-with-codespaces/about-codespaces) 是一個線上開發環境。 由 GitHub 裝載並由 Microsoft Visual Studio Code 提供技術支援，這項工具可在雲端提供完整的開發解決方案。

- [GitHub 安全性](https://github.com/features/security) 的運作方式是以多種方式消除威脅。 代理程式和服務可識別存放庫和相依套件中的弱點。 其也會將相依性升級為目前的安全版本。

- [GitHub Actions](https://docs.github.com/actions/learn-github-actions) 是可直接在存放庫中提供 CI/CD 功能的自訂工作流程。 名為執行器的電腦會裝載這些 CI/CD 作業。

- [Azure Active Directory](/azure/active-directory/fundamentals/active-directory-whatis) 是以雲端為基礎的多租使用者雲端式身分識別服務，可控制對 Azure 和其他雲端應用程式（如 Microsoft 365 和 GitHub）的存取。

- [Azure App Service](https://azure.microsoft.com/services/app-service/) 提供了用於建置、部署及調整 Web 應用程式的架構。 此平台會提供內建的基礎結構維護、安全性修補和調整。

- [Azure 原則](/azure/governance/policy/overview)可協助小組進行管理，同時透過原則定義讓雲端資源強制執行規則，以避免發生 IT 問題。 例如，如果專案即將部署 SKU 無法辨識的虛擬機器，Azure 原則會傳送有關此問題的警示並停止部署。

- [Azure 資訊安全中心](/azure/security-center/security-center-introduction)為混合式雲端工作負載提供統一的安全性管理和進階威脅防護。

- [Azure 監視器](/azure/azure-monitor/overview) 會收集和分析效能計量、活動記錄和其他應用程式遙測。 此服務會在識別異常狀況時，對應用程式和人員發出警示。

## <a name="innersource"></a>Innersource

### <a name="innersource-overview"></a>Innersource 總覽

許多公司都使用「詞彙 *innersource* 來描述其工程團隊如何在程式碼上共同作業。 Innersource 是一種開發方法，在此方法中，工程師會使用大規模開放原始碼專案（例如 Kubernetes 或 Visual Studio Code）的最佳作法來建立專屬軟體。

大規模的開放原始碼專案需要數以千計的參與者進行協調和團隊合作。 最成功的專案是以視覺為其未來和每日使用者需求來推動：速度、可靠性和功能。 這些專案的操作規模可提供一些課程，並可協助公司使用 innersource 更快速地建立更好的軟體。

使用 GitHub 的提取要求和問題時，共同作業和程式碼審查都內建于開發過程中。 內部和外包小組可以在同一個位置分享工作、討論變更，以及取得意見反應。 這可協助組織在內部分享專業知識，避免重塑針對其他專案所開發且經過現場測試的解決方案。

### <a name="the-anatomy-of-an-innersource-project"></a>Innersource 專案的剖析

個人、小組和資源的適當組合可以確保專案的成功。 許多開放原始碼專案都遵循類似的組織結構，可協助組織設定跨功能的小組來管理 innersource 專案。 典型的開放原始碼專案具有下列人員類型：

- **維護人員：** 這些參與者負責推動願景，以及管理專案的組織層面。 他們可能不是程式碼的原始擁有者或作者。

- **參與者：** 這些人就是參與專案的每個人。

- **社群成員：** 這些是使用專案的人員。 這些活動可能會在交談中使用，或表達其對專案方向的意見。

較大型的專案也可讓小組委員會或工作團隊專注於不同的工作，例如工具準備、分級和社群仲裁。 Innersource 專案可能會遵循類似的結構。 許多工程組織會將開發人員區分成應用程式工程、平台工程和網頁開發等小組。 以這種方式來建構組織，可能會留下排除合格人員的盲點。 組織小組所支援的核心決策團隊，可協助重整更快速解決問題所需的專業知識。

在企業內，參與者是整個公司的開發人員，而維護人員是專案的領導人和關鍵決策者。

- **維護人員：** 公司內的開發人員、產品經理和其他重要決策者，負責推動專案的願景，以及管理每日的投稿。

- **參與者：** 公司內的開發人員、資料科學家、產品經理、行銷人員和其他角色可協助推動軟體的進展。 參與者可能不屬於直接專案小組，而可藉由貢獻程式碼、提交錯誤 (bug) 修正程式等，協助建置軟體。

若要深入瞭解，請參閱 [innersource 簡介](https://resources.github.com/whitepapers/introduction-to-innersource/) 白皮書。

## <a name="automation"></a>自動化

GitHub Actions 可支援使用者直接在其 GitHub 存放庫中建立自訂工作流程。 使用者可以探索、建立及共用動作來執行任何作業 (包括 CI/CD)，以及在完全自訂的工作流程中合併動作。 他們也可以建立 CI 工作流程，以建立和測試以不同程式設計語言撰寫的專案。 [GitHub 動作指南](https://docs.github.com/actions/guides)中有提供範例。

GitHub Actions 可用於結合 IaC 概念與 CI/CD 做法，以將整個端對端部署生命週期自動化，包括以可重複的方式佈建或更新目標環境，以及封裝和部署應用程式本身。

### <a name="example"></a>範例

[適用於 Azure 的 GitHub Actions](https://github.com/azure/actions) 是為了簡化將部署程序的目標自動設為 Azure 服務 (例如 Azure App Service、Azure Kubernetes Service、Azure Functions 等) 的方式而建置。 [Azure 入門動作工作流程](https://github.com/azure/actions-workflow-samples)包括任何語言存放庫和任何生態系統的 Web 應用程式，並將其部署至 Azure 的端對端工作流程。 造訪 [GitHub marketplace](https://github.com/marketplace?query=azure&type=actions) 以查看所有可用的動作。

## <a name="security"></a>安全性

### <a name="githubs-shift-left-security-features"></a>GitHub 的 shift + 左方安全性功能

從開發的前幾個步驟開始，DevSecOps 會遵循安全性最佳做法。 DevSecOps 會使用左移策略，將安全性焦點重新導向。 其不會在結束時指向稽核，而是從一開始就移向開發。 除了產生健全的程式碼之外，這種具速錯機制 (Fail-fast) 的方法有助於在問題易於修正時及早解決問題。

透過許多安全性功能，GitHub 提供的工具可支援 DevSecOps 工作流程的每個部分：

- 具有內建安全性擴充功能的瀏覽器型 IDE
- 持續監視資訊安全諮詢的代理程式，並取代易受攻擊和過時的相依性
- 可掃描原始程式碼是否有弱點的搜尋功能
- 以動作為基礎的工作流程，可將開發、測試和部署的每個步驟自動化
- 可提供方法來私下討論及解決安全性威脅，然後發佈資訊的空間
- 這些功能結合了 Azure 的監視和評估能力，可提供更優質的服務來建置安全的雲端解決方案

### <a name="example"></a>範例

GitHub DevSecOps 安裝涵蓋許多安全性案例。 可能的案例包括下列案例：

- 想要利用預先設定的環境，來提供安全性功能的開發人員
- 依賴隨時掌握最新優先順序的安全性報告，以及受影響程式碼和建議修正程式詳細資料的系統管理員
- 簡化的組織，若在程式碼中公開秘密，這類組織需要系統自動取得新的且未遭入侵的安全性裝置
- 有較新或更安全的外部套件版本可用時，可從自動升級獲益的開發小組

進階閱讀，請參閱：

- [GitHub 中的 DevSecOps： Azure 解決方案構想 |Microsoft 檔](/azure/architecture/solution-ideas/articles/devsecops-in-github)
- [在 Azure DevOps 管線內使用 GitHub advanced security 掃描 GitHub 存放庫的程式碼](https://github.blog/2020-10-27-code-scanning-a-github-repository-using-github-advanced-security-within-an-azure-devops-pipeline/)
- [將 DevSecOps 套用至您的軟體供應鏈](https://github.blog/2020-12-03-applying-devsecops-to-your-software-supply-chain/)

## <a name="next-steps"></a>下一步

- 選擇您的實作小組 (通常是開發人員管理員及一些定義為系統管理員的開發人員)，然後部署 GitHub。
- 了解一般和進階 Git 工作流程，以增強您使用 GitHub 的方式。

下列連結提供 GitHub 的詳細資訊。

- [Microsoft Learn 中的 GitHub 模組](/learn/browse/?products=github)
- [GitHub 學習實驗室](https://lab.github.com/)
- [GitHub Docs](https://docs.github.com/en)
- [開始使用 GitHub DevSecOps 的秘訣](https://resources.github.com/whitepapers/Architects-guide-to-DevOps/)
