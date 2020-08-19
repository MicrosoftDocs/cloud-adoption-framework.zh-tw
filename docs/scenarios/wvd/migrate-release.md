---
title: Windows 虛擬桌面部署後和發行工作
description: 使用「適用于 Azure 的雲端採用架構」來瞭解 Windows 虛擬桌面遷移最佳做法，以降低複雜度並將遷移程式標準化。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2010
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 301afe5e901685b5069ec73937c86674a1eb17d1
ms.sourcegitcommit: 011525720bd9e2d9bcf03a76f371c4fc68092c45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88574987"
---
# <a name="windows-virtual-desktop-post-deployment"></a>Windows 虛擬桌面部署後

Windows 虛擬桌面實例的遷移或部署發行程式相當簡單。 此程式會反映在 [Windows 虛擬桌面概念證明](./proof-of-concept.md)中所使用的進程：

- 測試應用程式群組和已部署桌上型電腦的效能和延遲，以進行使用者的取樣。
- 讓終端使用者讓他們教授如何透過下列方式連接：
  - [Windows 桌面用戶端](/azure/virtual-desktop/connect-windows-7-and-10)
  - [Web 用戶端](/azure/virtual-desktop/connect-web)
  - [Android 用戶端](/azure/virtual-desktop/connect-android)
  - [macOS 用戶端](/azure/virtual-desktop/connect-macos)
  - [iOS 用戶端](/azure/virtual-desktop/connect-ios)

## <a name="post-deployment"></a>部署後

發行完成之後，通常會新增 [記錄和診斷，以更妥善操作 Windows 虛擬桌面](/azure/virtual-desktop/diagnostics-log-analytics#push-diagnostics-data-to-your-workspace)。 作業小組將集區主機和桌面虛擬機器上線至 [Azure 伺服器管理最佳作法](../../manage/azure-server-management/index.md) ，以管理報告、修補以及商務持續性和嚴重損壞修復設定，也是很常見的情況。

雖然發行程式不在此遷移案例的範圍內，但此程式可能會在遷移的後續反復專案期間，公開將其他工作負載遷移至 Azure 的需求。 如果您尚未設定 Office 365 或 Azure Active Directory，則您的雲端採用小組可能會選擇在桌上型電腦案例的版本中上架這些服務。 對於混合式作業模型，操作小組也可以選擇整合 Intune、System Center 或其他設定管理工具，以改善作業、合規性和安全性。

## <a name="next-steps"></a>後續步驟

在 Windows 虛擬桌面遷移完成後，您的雲端採用小組就可以開始下一個案例特定的遷移。 或者，如果有其他桌面要遷移，您可以重複使用此文章系列來引導您下一次的 Windows 虛擬桌面遷移或部署。

- [規劃 Windows 虛擬桌面移轉或部署](./plan.md)
- [檢查您的環境或 Azure 登陸區域](./ready.md)
- [完成 Windows 虛擬桌面概念證明](./proof-of-concept.md)
- [評估 Windows 虛擬桌面移轉或部署](./migrate-assess.md)
- [部署或遷移 Windows 虛擬桌面執行個體](./migrate-deploy.md)
- [將 Windows 虛擬桌面部署發行至生產環境](./migrate-release.md)
