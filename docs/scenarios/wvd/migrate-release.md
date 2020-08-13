---
title: Windows 虛擬桌面部署後和發行工作
description: 使用適用于 Azure 的雲端採用架構來瞭解 Windows 虛擬桌面的遷移最佳作法，以降低複雜度並將遷移程式標準化。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2010
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: fea97dbec42c9f31fc8518c011696a818a41730b
ms.sourcegitcommit: 949b87bad28d32df84df190160089f01826f3a31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2020
ms.locfileid: "88196421"
---
# <a name="windows-virtual-desktop-post-deployment"></a>Windows 虛擬桌面部署後

遷移或部署 Windows 虛擬桌面實例的發行程式相當簡單。 此程式會鏡像 [Windows 虛擬桌面概念證明](./proof-of-concept.md)期間所使用的進程：

- 測試應用程式群組的效能和延遲以及已部署的桌面，以進行使用者的取樣。
- 將使用者上線，告訴他們如何透過下列方式連線：
  - [Windows 桌面用戶端](https://docs.microsoft.com/azure/virtual-desktop/connect-windows-7-and-10)
  - [Web 用戶端](https://docs.microsoft.com/azure/virtual-desktop/connect-web)
  - [Android 用戶端](https://docs.microsoft.com/azure/virtual-desktop/connect-android)
  - [macOS 用戶端](https://docs.microsoft.com/azure/virtual-desktop/connect-macos)
  - [iOS 用戶端](https://docs.microsoft.com/azure/virtual-desktop/connect-ios)

## <a name="post-deployment"></a>部署後

發行完成之後，通常會加入 [記錄和診斷，以 Windows 虛擬桌面更好的操作](https://docs.microsoft.com/azure/virtual-desktop/diagnostics-log-analytics#push-diagnostics-data-to-your-workspace)。 Operations 小組也通常會將集區主機和桌上型電腦虛擬機器上架至 [Azure 伺服器管理最佳作法](../../manage/azure-server-management/index.md) ，以管理報告、修補和商務持續性和嚴重損壞修復設定。

雖然發行程式超出此遷移案例的範圍，但在後續的遷移反復專案期間，此程式可能會造成將額外工作負載遷移至 Azure 的需求。 如果您尚未設定 Office 365 或 Azure Active Directory，您的雲端採用小組可能會在桌上型電腦案例發行時，選擇將這些服務上架。 針對混合式作業模型，營運小組也可以選擇整合 Intune、System Center 或其他設定管理工具，以改善作業、合規性和安全性。

## <a name="next-steps"></a>後續步驟

完成 Windows 虛擬桌面遷移之後，您的雲端採用小組就可以開始進行下一個案例特定的遷移。 或者，如果有其他要遷移的桌面，您可以重複使用這篇文章系列，引導您進行下一 Windows 虛擬桌面遷移或部署。

- [規劃 Windows 虛擬桌面移轉或部署](./plan.md)
- [審查您的環境或 Azure 登陸區域](./ready.md)
- [完成 Windows 虛擬桌面概念證明](./proof-of-concept.md)
- [評估 Windows 虛擬桌面移轉或部署](./migrate-assess.md)
- [部署或遷移 Windows 虛擬桌面執行個體](./migrate-deploy.md)
- [將 Windows 虛擬桌面部署發行至生產環境](./migrate-release.md)
