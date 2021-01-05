---
title: 雲端採用架構企業規模登陸區域架構
description: 瞭解適用于 Azure 的雲端採用架構中的企業規模登陸區域架構。
author: BrianBlanchard
ms.author: brblanch
ms.date: 06/15/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: ready
ms.custom: think-tank, csu
ms.openlocfilehash: 4d4ca4be94afeae4e019da935726ca0708097c64
ms.sourcegitcommit: a0ddde4afcc7d8c21559e79d406dc439ee4f38d2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/21/2020
ms.locfileid: "97713347"
---
# <a name="cloud-adoption-framework-enterprise-scale-landing-zone-architecture"></a>雲端採用架構企業規模登陸區域架構

企業規模是一種架構方法和參考實行，可讓您大規模地在 Azure 上有效率地結構化和運算化登陸區域。 這種方法與 azure 藍圖和 Azure 的雲端採用架構相符。

## <a name="architecture-overview"></a>架構概觀

雲端採用架構企業規模的登陸區域架構，代表組織 Azure 環境的策略性設計路徑和目標技術狀態。 其會隨著 Azure 平台持續發展，且由組織為了因應其 Azure 旅程所必須做出的各種設計決策來定義。

並非所有企業都採用相同的方式，因此雲端採用架構企業規模的登陸區域架構在客戶之間會有所不同。 本指南中的技術考慮和設計建議可能會根據您組織的案例而產生不同的取捨。 預計會有一些變化，但您如果遵循核心建議，則所產生的目標架構會將您組織放在可持續調整的路徑上。

## <a name="landing-zone-in-enterprise-scale"></a>企業規模的登陸區域

Azure 登陸區域是多訂用帳戶 Azure 環境的輸出，其可解釋規模、安全性、治理、網路功能和身分識別。 Azure 登陸區域可讓您在 Azure 中的企業規模進行應用程式移轉和綠地開發。 這些區域會考慮支援客戶的應用程式組合所需的所有平台資源，並不會區分基礎結構即服務或平台即服務。

例如，您可以在建立新的房屋之前，先存取水、天然氣和電力等城市公用程式。 在此內容中，網路、身分識別和存取管理、原則、管理和監視是共用的公用程式服務，必須立即可用以協助在應用程式開始之前簡化應用程式遷移程式。

![顯示登陸區域設計的圖表。](./media/lz-design.png)

_圖1：登陸區域設計。_

## <a name="high-level-architecture"></a>高階架構

企業規模的架構是由一組設計考慮和建議，在八個 [重要的設計區域](./design-guidelines.md)中定義，其中有兩個建議的網路拓撲：以 AZURE 虛擬 WAN 網路拓撲為基礎的企業級架構 (在 [圖 2] 上 depictured) ，或根據以中樞和輪輻架構為基礎的傳統 Azure 網路拓撲， (如圖 3) 所示。

[![此圖顯示以 Azure 虛擬 WAN 網路拓撲為基礎的雲端採用架構企業規模登陸區域架構。](./media/ns-arch-inline.png)](./media/ns-arch-expanded.png#lightbox)

_圖2：根據 Azure 虛擬 WAN 網路拓撲的雲端採用架構企業規模登陸區域架構。請注意，連線訂用帳戶會使用虛擬 WAN 中樞。_

[![顯示雲端採用架構企業規模登陸區域架構的圖表。](./media/ns-arch-cust-inline.png)](./media/ns-arch-cust-expanded.png#lightbox)

_圖3：雲端採用架構企業規模的登陸區域架構，以傳統的 Azure 網路拓撲為基礎。請注意，連線訂用帳戶會使用中樞 VNet。_

下載 PDF 或 Visio 檔案，其中包含根據 [虛擬 WAN (pdf) ](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/ready/enterprise-scale-architecture.pdf) 網路拓撲的企業級架構圖表，或是以 [中樞和輪輻 (pdf) ](https://github.com/microsoft/CloudAdoptionFramework/raw/master/ready/enterprise-scale-architecture-cust.pdf) 架構為基礎的傳統 Azure 網路拓撲。 您可以將包含虛擬 WAN 和中樞和輪輻架構圖表的 Visio 檔案下載為 [visio 圖表 (的) ](https://github.com/microsoft/CloudAdoptionFramework/raw/master/ready/enterprise-scale-architecture.vsdx)。

在 [圖 2] 和 [3] 中，有企業級關鍵設計區域的參考，這些都是以 a 到 I 的字母來表示：

![字母 A ](./media/a.png) [ENTERPRISE 合約 (EA) 註冊和 Azure Active Directory](./enterprise-enrollment-and-azure-ad-tenants.md)租使用者。 註冊 Enterprise 合約 (EA) 代表 Microsoft 與您的組織如何使用 Azure 之間的商業關係。 它提供您所有訂用帳戶的計費基礎，並影響您的數位資產管理。 您的 EA 註冊可透過 Azure EA 入口網站進行管理。 註冊通常代表組織的階層，其中包括部門、帳戶和訂用帳戶。 Azure AD 租用戶提供身分識別與存取權管理，這是安全性狀態的重要部分。 Azure AD 租用戶可確保已驗證和授權的使用者只能存取他們具有存取權限的資源。

![字母 B 身分 ](./media/b.png) [識別和存取管理](./identity-and-access-management.md)。 您必須建立 Azure Active Directory 的設計和整合，才能確保伺服器和使用者驗證。 Azure 角色型存取控制 (Azure RBAC) 必須進行模型化和部署，以強制將職責區隔和平臺作業和管理所需的權利分開。 金鑰管理必須經過設計和部署，以確保安全地存取資源和支援作業，例如輪替和復原。 最後，系統會將存取角色指派給控制項和資料平面的應用程式擁有者，以自主建立及管理資源。

![字母 C ](./media/c.png) [管理群組和訂](./management-group-and-subscription-organization.md)用帳戶組織。 Azure Active Directory (Azure AD) 租用戶內的管理群組結構支援組織對應，在組織規劃大規模採用 Azure 時必須詳細考慮管理群組。 訂用帳戶是 Azure 中管理、計費、縮放的單位。 當您設計大規模的 Azure 採用時，訂用帳戶扮演著重要的角色。 這個關鍵的設計領域可協助您根據重要因素來捕捉訂用帳戶需求和設計目標訂閱。 這些因素包括環境類型、擁有權和治理模型、組織結構、應用程式組合。

![字母 D 的 ](./media/d.png) [管理和監視](./management-and-monitoring.md)。 平台層級的整體 (水平) 資源監視與警示必須經過設計、部署及整合。 您也必須定義和簡化諸如修補和備份等作業工作。 安全性作業、監視和記錄必須與 Azure 上的資源和現有的內部部署系統進行設計及整合。 所有跨資源執行控制平面作業的訂用帳戶活動記錄，都應串流至 Log Analytics，以供查詢及分析，受限於 Azure RBAC 許可權。

![字母 E ](./media/e.png) [網路拓朴和連線能力](./network-topology-and-connectivity.md)。 您必須在 Azure 區域和內部部署環境中建立和部署端對端網路拓撲，以確保平臺部署之間的北歐和中東連線能力。 您必須在網路安全性設計中識別、部署和設定必要的服務和資源，例如防火牆和網路虛擬裝置，以確保完全符合安全性需求。

![字母 F ](./media/f.png) 、 ![ 字母 G ](./media/g.png) 、 ![ 字母 H ](./media/h.png) [商務持續性和](./business-continuity-and-disaster-recovery.md) 嚴重損壞修復和 [安全性、治理和合規性](./security-governance-and-compliance.md)。 您必須識別、描述、建立整體和登陸區域特定的原則，並將其部署到目標 Azure 平臺上，以確保公司、法規和企業營運控制項都已就緒。 最後，您應該使用原則來確保應用程式和基礎資源的相容性，而不需要任何抽象概念布建或管理功能。

![字母 I ](./media/i.png) [平臺自動化和 DevOps](platform-automation-and-devops.md)。 您必須設計、建立及部署具有穩固軟體發展生命週期實務的端對端 DevOps 體驗，以確保基礎結構即程式碼構件的安全、可重複且一致的傳遞。 您可以使用專用的整合、發行和部署管線，搭配強大的原始檔控制和追蹤功能，來開發、測試及部署這類構件。

## <a name="next-steps"></a>後續步驟

使用雲端採用架構企業規模的設計指導方針，自訂此架構的執行。

> [!div class="nextstepaction"]
> [設計指導方針](./design-guidelines.md)
