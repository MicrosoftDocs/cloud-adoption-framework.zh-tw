---
title: 工具和範本
description: 尋找可在雲端採用架構中使用的工具和範本，以協助您加速採用雲端。
author: JanetCThomas
ms.author: janet
ms.date: 04/14/2020
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: reference
ms.custom: internal
ms.openlocfilehash: bf6d4651c4cf145bd21a94c7ac87bd669b7d83cb
ms.sourcegitcommit: b12b731b1cf22f9e80db108f79734bc4cf17895e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/26/2021
ms.locfileid: "98810800"
---
# <a name="tools-and-templates"></a>工具和範本

雲端採用架構包含可協助您快速執行技術變更的工具。 使用這些工具、範本和評量來加速雲端採用。 下列資源可協助您在採用的每個階段。 某些工具和範本可用於多個階段。

## <a name="strategy"></a>策略

| 資源 | 描述 |
|----------|-------------|
| [雲端旅程追蹤器](/assessments/?id=cloud-journey-tracker&mode=pre-assessment) | 根據您的業務需求，識別您的雲端採用途徑。 |
| [策略 &nbsp; 和 &nbsp; 方案 &nbsp; 範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx) | 當您執行您的雲端採用策略和規劃時，請記錄決策。 |

## <a name="plan"></a>計畫

| 資源 | 描述 |
|----------|-------------|
| [雲端旅程追蹤器](/assessments/?id=cloud-journey-tracker&mode=pre-assessment) | 根據您的業務需求，識別您的雲端採用途徑。 |
| [策略 &nbsp; 和 &nbsp; 方案 &nbsp; 範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/plan/cloud-adoption-framework-strategy-and-plan-template.docx) | 當您執行您的雲端採用策略和規劃時，請記錄決策。 |
| [雲端採用方案產生器](../plan/template.md) | 使用範本將待處理專案部署至 [Azure Boards](/azure/devops/boards/get-started/what-is-azure-boards) ，以將程式標準化。 |
| [使用策略-規劃就緒-治理 ADO 範本](https://azuredevopsdemogenerator.azurewebsites.net/?name=strategyplan) | 使用範本將待處理專案部署至 [Azure Boards](/azure/devops/boards/get-started/what-is-azure-boards) ，以將程式標準化。 |

## <a name="ready"></a>就緒

| 資源 | 描述 |
|----------|-------------|
| [就緒 &nbsp; 檢查清單](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/ready/readiness-checklist.docx) | 使用此檢查清單來準備您的環境以供採用，包括準備您的第一個遷移登陸區域、將藍圖個人化，以及將其擴充。 |
| [命名和標記慣例追蹤範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/ready/naming-and-tagging-conventions-tracking-template.xlsx) | 記錄有關命名和標記標準的決策，以確保一致性並減少上線時間。 |
| [CAF 基礎藍圖](https://github.com/Microsoft/CloudAdoptionFramework/tree/master/ready/migration-landing-zone-governance) | 使用初始治理基礎的輕量執行，在 Azure 中提供治理工具的實際操作體驗。 |
| [CAF 移轉登陸區域藍圖](https://github.com/Microsoft/CloudAdoptionFramework/tree/master/ready/migration-landing-zone) | 布建並準備將工作負載從內部部署環境遷移至 Azure。 如需此藍圖的詳細資訊，請參閱 [部署遷移登陸區域](../ready/landing-zone/migrate-landing-zone.md)。 |
| [Terraform 模組](../ready/landing-zone/terraform-landing-zone.md) | CAF 登陸區域之 Terraform 版本的開放原始碼程式碼基底。 |
| [Terraform 登錄](https://registry.terraform.io/search?q=aztfmod) | Terraform 登錄網站會經過篩選，以列出透過 Terraform 建立登陸區域所需的所有雲端採用架構模組。 |

## <a name="govern"></a>治理

| 資源 | 描述 |
|----------|-------------|
| [治理基準評估](https://cafbaseline.com) | 識別您目前的狀態與業務優先順序之間的差異，並取得適當的資源以利消除這些差異。 |
| [CAF 基礎藍圖](https://github.com/Microsoft/CloudAdoptionFramework/tree/master/ready/migration-landing-zone-governance) | 輕量的初始治理基礎，以提供有關 Azure 治理工具的實際操作經驗。 |
| [治理 &nbsp; 專業領域 &nbsp; 範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/govern/governance-discipline-template.docx) | 定義用來強制執行每個治理專業領域的一組基本治理流程。 |
| [成本管理專業領域範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/govern/cost-management-discipline-template.docx) | 定義原則聲明和設計指引，讓您能夠在組織內成熟雲端治理，並專注于成本管理。 |
| [部署加速專業領域範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/govern/deployment-acceleration-discipline-template.docx) | 定義原則聲明和設計指引，讓您能夠在部署加速時成熟組織內的雲端治理。 |
| [身分識別基準專業領域範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/govern/identity-baseline-discipline-template.docx) | 定義原則聲明和設計指引，讓您能夠在組織內成熟雲端治理，並專注于身分識別需求。 |
| [資源一致性專業領域範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/govern/resource-consistency-discipline-template.docx) | 定義原則聲明和設計指引，讓您能夠在組織內成熟雲端治理，並專注于資源一致性。 |
| [安全性基準專業領域範本](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/govern/security-baseline-discipline-template.docx) | 定義原則聲明和設計指引，讓您能夠在組織內成熟雲端治理，並專注于安全性基準。 |
| [Azure 安全性效能評定](https://docs.microsoft.com/azure/security/benchmarks/overview) | Azure 安全性基準測試 (ASB) 提供規定的最佳作法和建議，以協助改善 Azure 上的工作負載、資料和服務的安全性。 |
| [Azure 治理視覺化](https://github.com/JulianHayward/Azure-MG-Sub-Governance-Reporting) | Azure 治理的視覺化程式是 PowerShell 腳本，可逐一查看 Azure 租使用者的管理群組階層至訂用帳戶層級。 它會從最相關的 Azure 治理功能（例如 Azure 原則、Azure 角色型存取控制 (Azure RBAC) 和 Azure 藍圖）取得資料。 從收集到的資料，視覺化檢視會顯示您的階層地圖、建立租使用者摘要，以及建立有關您管理群組和訂用帳戶的細微範圍深入解析。 |

## <a name="migrate"></a>移轉

| 資源 | 描述 |
|----------|-------------|
| [資料中心遷移探索檢查清單](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/migrate/datacenter-migration-discovery-checklist.docx) | 請參閱此檢查清單，以取得有助於識別資料中心內工作負載、伺服器和其他資產的資訊。 您可以使用此資訊來協助規劃您的遷移。
| [遷移範本](https://aka.ms/adopt/plan/generator) | 在 Azure DevOps 產生器中，我們建立了一些範本，可讓您用來協助簡化您的專案。 已針對 [WVD](https://azuredevopsdemogenerator.azurewebsites.net/?name=wvdmigration)、 [伺服器遷移](https://azuredevopsdemogenerator.azurewebsites.net/?name=servermigration)、 [SQL 遷移](https://azuredevopsdemogenerator.azurewebsites.net/?name=sqlmigration) 和 [AKS 部署](https://azuredevopsdemogenerator.azurewebsites.net/?name=cafaks)建立範本。


## <a name="innovate"></a>創新

| 資源 | 描述 |
|----------|-------------|
| [部署範本](https://aka.ms/adopt/plan/generator) | [知識挖掘](https://azuredevopsdemogenerator.azurewebsites.net/?name=kmine)範本為您提供結構化的方法，以探索結構化和非結構化資料中包含的潛在見解。 .


## <a name="manage"></a>管理

| 資源 | 描述 |
|----------|-------------|
| [Microsoft Azure 架構完善的檢閱](/assessments/?id=azure-architecture-review) | 這項線上評量將有助於定義工作負載的特定架構和作業選項。 |
| [最佳 &nbsp; 做法 &nbsp; 來源程式 &nbsp; 碼](https://github.com/Microsoft/CloudAdoptionFramework/tree/master/manage/Automation-Best-Practices) | 此可部署的原始程式碼可補充並加速採用 Azure 伺服器管理服務的最佳做法。 使用此原始程式碼可快速啟用作業管理，並建立作業基準。 |
| [Operations management 活頁簿](https://raw.githubusercontent.com/Microsoft/CloudAdoptionFramework/master/manage/opsmanagementworkbook.xlsx) | 記錄有關雲端中作業管理的決策，並促進與企業的交談，以確保符合 Sla、投資復原能力以及與作業相關的預算配置。 |

## <a name="organize"></a>組織

| 資源 | 描述 |
|----------|-------------|
| [跨小組 RACI 圖](https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/organize/raci-template.xlsx) | 下載並修改 RACI 試算表範本，以追蹤一段時間的組織結構決策。 |
