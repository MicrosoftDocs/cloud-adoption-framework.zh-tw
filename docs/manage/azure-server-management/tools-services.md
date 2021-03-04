---
title: Azure 伺服器管理服務
description: 使用適用于 Azure 的雲端採用架構，瞭解 Azure 伺服器管理服務套件內的區域。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: internal
ms.openlocfilehash: 3aa4c56118c74c0bf40c72f17e2c314f723618f0
ms.sourcegitcommit: 9e4bc0e233a24642853f5e8acbeb9746b2444024
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "102114722"
---
# <a name="azure-server-management-tools-and-services"></a>Azure 伺服器管理工具和服務

如同本指南的 [概述](./index.md) 中所述，Azure 伺服器管理服務套件涵蓋下列各方面：

- 移轉
- 安全
- 保護
- 監視器
- 設定
- 治理

下列各節將簡短說明這些管理區域，並提供支援這些管理區域之主要 Azure 服務的詳細內容連結。

## <a name="migrate"></a>移轉

遷移服務可協助您將工作負載遷移至 Azure。 為了提供最佳的指導方針，Azure 遷移服務一開始會測量內部部署伺服器的效能，並評估遷移的適用性。 Azure 遷移完成評量之後，您可以使用 [Azure Site Recovery](/azure/site-recovery/site-recovery-overview) 和 [Azure 資料庫移轉服務](/azure/dms/dms-overview) ，將您的內部部署機器遷移至 azure。

## <a name="secure"></a>安全

[Azure 安全性中心](/azure/security-center/security-center-introduction) 是全方位的安全性管理應用程式。 藉由上線至安全中心，您可以快速評估環境的安全性和法規合規性狀態。 如需將伺服器上架到 Azure 資訊安全中心的指示，請參閱 [設定訂用帳戶的 Azure 管理服務](./onboard-at-scale.md#azure-security-center)。

## <a name="protect"></a>保護

若要保護您的資料，您需要規劃備份、高可用性、加密、授權和相關的操作問題。 這些主題廣泛地在線上討論，因此我們將著重于建立商務持續性和嚴重損壞修復 (BCDR) 方案。 我們將包含說明文件的參考，其中詳細說明如何執行和部署這種類型的計畫。

當您建立資料保護策略時，請考慮將您的工作負載應用程式細分成不同的層級。 這種方法會有説明，因為每一層通常都需要自己獨特的保護計劃。 若要深入瞭解如何設計具復原能力的應用程式，請參閱 [為 Azure 設計復原應用程式](/azure/architecture/framework/resiliency/overview)。

最基本的資料保護是備份。 若要在伺服器遺失時加速復原程式，請不要只備份資料，也備份伺服器設定。 備份是一種有效的機制，可處理意外的資料刪除和勒索軟體攻擊。 [Azure 備份](/azure/backup/) 可協助您在 azure 和執行 Windows 或 Linux 的內部部署伺服器上保護您的資料。 如需有關 Azure 備份可以做什麼和操作指南的詳細資訊，請參閱 [Azure 備份服務總覽](/azure/backup/backup-overview)。

如果工作負載需要即時商務持續性以進行硬體故障或資料中心中斷，請考慮使用資料複寫。 [Azure Site Recovery](/azure/site-recovery/site-recovery-overview) 提供連續複寫 vm 的解決方案，此解決方案可提供最少的資料遺失。 Site Recovery 也支援數種複寫案例，例如複寫：

- 在兩個 Azure 區域之間的 Azure Vm。
- 在內部部署伺服器之間。
- 在內部部署伺服器與 Azure 之間。

如需詳細資訊，請參閱 [完整的 Azure Site Recovery 複寫矩陣](/azure/site-recovery/site-recovery-overview#what-can-i-replicate)。

針對您的檔案伺服器資料，另一個要考慮的服務是 [Azure 檔案同步](/azure/storage/files/storage-sync-files-planning)。這項服務可協助您將組織的檔案共用集中在 Azure 檔案服務中，同時保有內部部署檔案伺服器的彈性、效能和相容性。 若要使用此服務，請遵循部署 Azure 檔案同步的指示。

## <a name="monitor"></a>監視器

[Azure 監視器](/azure/azure-monitor/overview) 可讓您查看各種資源，例如應用程式、容器和虛擬機器。 它也會從數個來源收集資料：

- [適用于 vm 的 Azure 監視器](/azure/azure-monitor/vm/vminsights-overview) 可提供 VM 健康情況、效能趨勢和相依性的深入觀點。 此服務會監視您的 Azure 虛擬機器、虛擬機器擴展集和內部部署環境中電腦的作業系統健康情況。
- [Log Analytics](/azure/azure-monitor/logs/log-query-overview) 是 Azure 監視器的一項功能。 其角色是整體 Azure 管理案例的核心。 它可作為記錄分析和許多其他 Azure 服務的資料存放區。 它提供豐富的查詢語言和分析引擎，可讓您深入瞭解應用程式和資源的運作。
- [Azure 活動記錄](/azure/azure-monitor/essentials/platform-logs-overview) 也是 azure 監視器的一項功能。 它可讓您深入瞭解 Azure 中發生的訂用帳戶層級事件。

## <a name="configure"></a>設定

有幾個服務符合這個類別。 它們可協助您：

- 自動化作業工作。
- 管理伺服器設定。
- 測量更新相容性。
- 排程更新。
- 偵測對您伺服器的變更。

這些服務是支援進行中作業的必要項：

- 「[更新管理](/azure/automation/update-management/overview)」會在您的環境中自動部署修補程式，包括部署至在 Azure 外部執行的作業系統實例。 它同時支援 Windows 和 Linux 作業系統，並追蹤因遺漏修補程式而造成的主要作業系統弱點和不符合項。
- [變更追蹤和清查](/azure/automation/change-tracking/overview) 可讓您深入瞭解在您的環境中執行的軟體，並反白顯示發生的任何變更。
- [Azure 自動化](/azure/automation/automation-intro) 可讓您執行 Python 和 PowerShell 腳本或 runbook，將整個環境中的工作自動化。 當您搭配混合式 Runbook 背景 [工作角色](/azure/automation/automation-hybrid-runbook-worker)使用 Azure 自動化時，您也可以將您的 runbook 延伸到內部部署資源。
- [Azure Automation State configuration](/azure/automation/automation-dsc-overview) 可讓您直接從 Azure 將 PowerShell 預期狀態設定推送 (DSC) 設定。 DSC 也可讓您監視及保留客體作業系統和工作負載的設定。

## <a name="govern"></a>治理

採用和移至雲端會帶來新的管理挑戰。 當您從操作管理負擔轉移至監視和治理時，它需要不同的思維。 雲端採用架構從 [治理](../../govern/index.md)開始著手。 此架構會說明如何遷移至雲端、旅程的外觀，以及應參與的人員。

標準組織的治理設計通常與複雜企業的治理設計不同。 若要深入瞭解標準組織的治理最佳作法，請參閱 [標準企業治理指南](../../govern/guides/standard/index.md)。 若要深入瞭解適用于複雜企業的治理最佳作法，請參閱 [適用于複雜企業的治理指南](../../govern/guides/complex/index.md)。

## <a name="billing-information"></a>帳單資訊

若要瞭解 Azure 管理服務的定價，請移至這些頁面：

- [Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/)

- [Azure 備份](https://azure.microsoft.com/pricing/details/backup/)

- [Azure 監視器](https://azure.microsoft.com/pricing/details/monitor/)

- [Azure 資訊安全中心](https://azure.microsoft.com/pricing/details/security-center/)

- [Azure 自動化](https://azure.microsoft.com/pricing/details/automation/)，包括：
  - Desired State Configuration
  - 更新管理解決方案
  - 「變更追蹤和清查」解決方案

- [Azure 原則](https://azure.microsoft.com/pricing/details/azure-policy/)

- [Azure 檔案同步服務](https://azure.microsoft.com/pricing/details/storage/blobs/)

> [!NOTE]
> Azure 更新管理解決方案是免費的，但有與資料內嵌相關的小型成本。 根據經驗法則，您可以免費使用每個月的前 5 gb (GB) 每月資料內嵌。 我們通常會觀察到每部機器每個月使用大約 25 MB。 因此，每個月大約200部機器免費涵蓋。 若為更多伺服器，請將額外伺服器的數目乘以每月 25 MB。 然後，將結果乘以您需要的額外儲存體的儲存體價格。 如需成本的詳細資訊，請參閱 [Azure 儲存體定價](https://azure.microsoft.com/pricing/details/storage/)。 每個額外的伺服器通常會對成本有名義的影響。
