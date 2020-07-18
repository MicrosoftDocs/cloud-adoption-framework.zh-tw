---
title: Windows 虛擬桌面部署後和發行工作
description: 使用適用于 Azure 的雲端採用架構來瞭解 Windows 虛擬桌面的遷移最佳作法，以降低複雜度並將遷移程式標準化。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2010
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 4c5c4140bb5735706f8cefbb4f37cdc391601c75
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86451805"
---
# <a name="windows-virtual-desktop-post-deployment"></a>Windows 虛擬桌面部署後

遷移或部署 Windows 虛擬桌面實例的發行程式相當簡單。 此程式會鏡像[Windows 虛擬桌面概念證明](./proof-of-concept.md)期間所使用的進程：

- 測試應用程式群組和已部署桌上型電腦的效能和延遲，以進行使用者的取樣。
- 將使用者上線，告訴他們如何透過下列方式連線：
  - [Windows 桌面用戶端](https://docs.microsoft.com/azure/virtual-desktop/connect-windows-7-and-10)
  - [Web 用戶端](https://docs.microsoft.com/azure/virtual-desktop/connect-web)
  - [Android 用戶端](https://docs.microsoft.com/azure/virtual-desktop/connect-android)
  - [macOS 用戶端](https://docs.microsoft.com/azure/virtual-desktop/connect-macos)
  - [iOS 用戶端](https://docs.microsoft.com/azure/virtual-desktop/connect-ios)

## <a name="post-deployment"></a>部署後

發行完成後，通常會加入[記錄和診斷，以 Windows 虛擬桌面更好的操作](https://docs.microsoft.com/azure/virtual-desktop/diagnostics-log-analytics#push-diagnostics-data-to-your-workspace)。 Operations 小組也通常會將集區主機和桌面 Vm 上架到[Azure 伺服器管理最佳作法](../../manage/azure-server-management/index.md)，以管理報告、修補和 BCDR 設定。

在此遷移案例的範圍外，發行程式可能會在後續的遷移反復專案期間，公開將額外工作負載遷移至 Azure 的需求。 如果您尚未設定 Office 365 或 Azure AD，小組可以選擇在發行桌上型電腦案例時，將這些服務上架。 針對混合式作業模型，營運小組也可以選擇整合 Intune、System Center 或其他設定管理工具來改善作業、合規性和安全性。

## <a name="next-step-your-next-migration-iteration"></a>下一個步驟：您的下一個遷移反復專案

一旦 Windows 虛擬桌面遷移完成，雲端採用小組就可以開始進行下一次的案例特定遷移。 或者，如果還有其他要遷移的桌面，可以再次使用此文章系列來引導您進行下一 Windows 虛擬桌面遷移或部署。

- [規劃 Windows 虛擬桌面遷移或部署](./plan.md)
- [審查您的環境或 Azure 登陸區域](./ready.md)
- [完成 Windows 虛擬桌面概念證明](./proof-of-concept.md)
- [評估 Windows 虛擬桌面遷移或部署](./migrate-assess.md)
- [部署或遷移 Windows 虛擬桌面實例](./migrate-deploy.md)
- [將您的 Windows 虛擬桌面部署發行至生產環境](./migrate-release.md)
