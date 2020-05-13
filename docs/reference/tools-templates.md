---
title: 工具和範本
description: 尋找可在雲端採用架構中使用的工具和範本，以協助您加速雲端採用。
author: JanetCThomas
ms.author: janet
ms.date: 04/14/2020
ms.service: cloud-adoption-framework
ms.subservice: reference
ms.topic: article
ms.openlocfilehash: 3373e5fd09aa9280de2c9b944d2b9648ffe8ae1a
ms.sourcegitcommit: 60d8b863d431b5d7c005f2f14488620b6c4c49be
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83219099"
---
<!-- docsTest:ignore Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template -->

<!-- cSpell:ignore Terraform's -->

# <a name="tools-and-templates"></a>工具和範本

雲端採用架構包含可協助您快速實行技術變更的工具。 使用這些工具、範本和評量來加速雲端採用。 下列資源可協助您進行採用的每個階段。 有些工具和範本可以用在多個階段。

## <a name="strategy"></a>策略

| 資源 | 描述 |
|----------|-------------|
| [雲端旅程追蹤](https://docs.microsoft.com/assessments/?mode=pre-assessment&id=cloud-journey-tracker) | 根據您的業務需求，識別您的雲端採用途徑。 |
| [策略 &nbsp; 與 &nbsp; 計畫 &nbsp; 範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx) | 當您執行雲端採用策略和計畫時的檔決策。 |

## <a name="plan"></a>計畫

| 資源 | 描述 |
|----------|-------------|
| [雲端旅程追蹤](https://docs.microsoft.com/assessments/?mode=pre-assessment&id=cloud-journey-tracker) | 根據您的業務需求，識別您的雲端採用途徑。 |
| [策略 &nbsp; 與 &nbsp; 計畫 &nbsp; 範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/Microsoft-Cloud-Adoption-Framework-Strategy-and-Plan-Template.docx) | 在您執行雲端採用策略和規劃時的檔決策。 |
| [雲端採用方案產生器](../plan/template.md) | 使用雲端採用方案範本將待處理專案部署到[Azure Boards](https://docs.microsoft.com/azure/devops/boards/get-started/what-is-azure-boards) ，以標準化程式。 |

## <a name="ready"></a>就緒

| 資源 | 描述 |
|----------|-------------|
| [準備就緒檢查清單](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/ready/readiness-checklist.docx) | 使用此檢查清單來準備您的環境以進行採用，包括準備您的第一個遷移登陸區域、將藍圖個人化，以及將它展開。 |
| [命名和標記追蹤範本](https://archcenter.blob.core.windows.net/cdn/fusion/readiness/CAF%20Readiness%20Naming%20and%20Tagging%20tracking%20template.xlsx) | 有關命名和標記標準的檔決策，以確保一致性並減少上線時間。 |
| [CAF &nbsp; foundation &nbsp; 藍圖](https://github.com/microsoft/CloudAdoptionFramework/tree/master/ready/migration-landing-zone-governance) | 使用初始治理基礎的輕量執行，在 Azure 中提供治理工具的實際操作體驗。 |
| [CAF 遷移登陸區域藍圖](https://github.com/microsoft/CloudAdoptionFramework/tree/master/ready/migration-landing-zone) | 布建和準備裝載從內部部署環境遷移至 Azure 的工作負載。 如需此藍圖的詳細資訊，請參閱[部署遷移登陸區域](../ready/landing-zone/migrate-landing-zone.md)。 |
| [Terraform 登陸區域藍圖](../ready/landing-zone/terraform-landing-zone.md) | CAF 登陸區域藍圖的 Terraform 版本的開放原始碼程式碼基底。 |
| [Terraform 登錄](https://registry.terraform.io/search?q=aztfmod) | Terraform 的登錄網站，經過篩選後，列出建立 Terraform 登陸區域所需的所有雲端採用架構模組。 |

## <a name="govern"></a>治理

| 資源 | 描述 |
|----------|-------------|
| [治理基準評估](https://cafbaseline.com) | 找出您目前狀態與業務優先順序之間的差距，並取得適當的資源來協助您解決這些差距。 |
| [CAF &nbsp; foundation &nbsp; 藍圖](https://github.com/microsoft/CloudAdoptionFramework/tree/master/ready/migration-landing-zone-governance) | 輕量執行初始治理基礎，以提供有關 Azure 中治理工具的實際操作體驗。 |
| [治理流程範本](https://archcenter.blob.core.windows.net/cdn/fusion/governance/Governance%20Discipline%20Template.docx) | 定義用來強制執行每個治理專業領域的一組基本治理流程。 |
| [成本管理流程範本](https://archcenter.blob.core.windows.net/cdn/fusion/governance/Cost%20Management%20Discipline%20Template.docx) | 定義原則聲明和設計指引，讓您在組織內成熟雲端治理，並著重于成本管理。 |
| [部署加速流程範本](https://archcenter.blob.core.windows.net/cdn/fusion/governance/Deployment%20Acceleration%20Discipline%20Template.docx) | 定義原則聲明和設計指引，讓您在組織內成熟雲端治理，並將焦點放在部署加速。 |
| [身分識別流程範本](https://archcenter.blob.core.windows.net/cdn/fusion/governance/identity%20baseline%20discipline%20template.docx) | 定義原則聲明和設計指引，讓您在組織內成熟雲端治理，並著重于身分識別需求。 |
| [資源一致性流程範本](https://archcenter.blob.core.windows.net/cdn/fusion/governance/Resource%20Consistency%20Discipline%20Template.docx) | 定義原則聲明和設計指引，讓您在組織內成熟雲端治理，並專注于資源一致性。 |
| [安全性基準流程範本](https://archcenter.blob.core.windows.net/cdn/fusion/governance/Security%20Baseline%20Discipline%20Template.docx) | 定義原則聲明和設計指引，讓您在組織內的雲端治理成熟，並著重于安全性基準。 |

## <a name="manage"></a>管理

| 資源 | 描述 |
|----------|-------------|
| [Azure 架構檢閱](https://docs.microsoft.com/assessments/?id=azure-architecture-review) | 這項線上評估會協助定義工作負載的特定架構和操作選項。 |
| [最佳 &nbsp; 做法 &nbsp; 原始程式碼 &nbsp;](https://github.com/microsoft/CloudAdoptionFramework/tree/master/manage/automation-best-practices) | 這個可部署的原始程式碼可補充並加速採用 Azure 伺服器管理服務的最佳作法。 使用此原始程式碼可快速啟用作業管理，並建立作業基準。 |
| [Operations management 活頁簿](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx) | 在雲端進行作業管理的相關檔決策，並加速與企業的交談，以確保 Sla、復原的投資，以及與作業相關的預算配置。 |

## <a name="organize"></a>組織

| 資源 | 描述 |
|----------|-------------|
| [跨小組 RACI 圖表](https://archcenter.blob.core.windows.net/cdn/fusion/management/raci-template.xlsx) | 下載並修改 RACI 試算表範本，以追蹤經過一段時間的組織結構決策。 |
