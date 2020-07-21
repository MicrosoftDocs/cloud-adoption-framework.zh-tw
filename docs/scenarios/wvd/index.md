---
title: 將 Windows 虛擬桌面執行個體遷移或部署至 Azure
description: 將 Windows 虛擬桌面執行個體遷移或部署至 Azure。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2010
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: strategy
ms.openlocfilehash: 9c28afd9aa3bed856f66fb96e13a07b196f7dfdd
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86451423"
---
# <a name="migrate-or-deploy-windows-virtual-desktop-instances-to-azure"></a>將 Windows 虛擬桌面執行個體遷移或部署至 Azure

將使用者桌面遷移至雲端是常見的雲端移轉案例。 此案例可提高產能，並加速各種工作負載的移轉，以支援使用者體驗。

## <a name="strategy-and-motivations"></a>策略與動機

虛擬桌面移轉是為了實現幾個常見的目標結果，如下圖所示：

![虛擬桌面移轉的動機](../../_images/migrate/wvd/motivations.png)

- 明確想要將生產力延伸到不受 IT 直接控制的電腦、手機、平板電腦或瀏覽器。
- 使用者需要從其裝置存取公司的資料和應用程式。
- 當工作負載遷移至雲端時，使用者需要更多支援，以獲得低延遲、高效能的體驗。
- 目前的或提議的虛擬桌面體驗成本需要最佳化，以便能夠以符合成本效益的方式調整遠端工作。
- IT 小組想要轉換工作場所，通常會從轉換使用者體驗來開始。

雲端中的使用者桌面虛擬化可協助您逐個了解這些動機。

## <a name="approach-wvd-refactor-and-modernization"></a>方法：WVD 重構和現代化

在本文所述的方法中，現有的 Citrix、VMware 或遠端桌面服務 (RDS) 伺服器陣列將會現代化為使用稱為 Windows 虛擬桌面 (WVD) 的平台即服務 (PaaS) 解決方案。

在此方法中，虛擬桌面的管理會現代化並更換為 Azure 中的平台即服務供應項目 (名稱為 Windows 虛擬桌面)。 不是將桌面映像遷移至 Azure，就是會產生新的映像。 使用者設定檔也一樣，不是會遷移至 Azure，就是會建立新的設定檔。 基本上，這項移轉工作能啟用用戶端解決方案，但大多不會有所變更。

![虛擬桌面移轉案例的解決方案示意圖](../../_images/migrate/wvd/scenario-solution.png)

完成時，虛擬桌面伺服器陣列的管理負荷和成本就會由雲端原生解決方案取代，以管理客戶的虛擬桌面體驗。 您的小組只需要擔心如何支援桌面映像、可用應用程式、Azure Active Directory 和使用者設定檔。

## <a name="next-step-integrate-this-strategy-into-your-cloud-adoption-journey"></a>下一步：將此策略整合到雲端採用旅程圖

下列文章清單會帶您前往在雲端採用旅程圖的特定時間點所找到的指引。

- [規劃 Windows 虛擬桌面移轉或部署](./plan.md)
- [檢閱環境或 Azure 登陸區域](./ready.md)
- [完成 Windows 虛擬桌面概念證明](./proof-of-concept.md)
- [評估 Windows 虛擬桌面移轉或部署](./migrate-assess.md)
- [部署或遷移 Windows 虛擬桌面執行個體](./migrate-deploy.md)
- [將 Windows 虛擬桌面部署發行至生產環境](./migrate-release.md)
