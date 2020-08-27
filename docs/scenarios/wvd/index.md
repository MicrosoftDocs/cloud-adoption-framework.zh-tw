---
title: 將 Windows 虛擬桌面執行個體遷移或部署至 Azure
description: 將 Windows 虛擬桌面執行個體遷移或部署至 Azure。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2010
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: strategy
ms.openlocfilehash: 9e07aa60e6201735905160f533115552f61c4d1a
ms.sourcegitcommit: 07d56209d56ee199dd148dbac59671cbb57880c0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88885417"
---
# <a name="migrate-or-deploy-windows-virtual-desktop-instances-to-azure"></a>將 Windows 虛擬桌面執行個體遷移或部署至 Azure

將組織的使用者桌面遷移至雲端是常見的雲端移轉案例。 這麼做有助於提升員工的生產力，並加速各種工作負載的遷移，以支援組織的使用者體驗。

## <a name="strategy-and-motivations"></a>策略與動機

虛擬桌面移轉是為了實現幾個常見的目標結果，如這裡所示：

![虛擬桌面移轉的動機清單。](../../_images/migrate/wvd/motivations.png)

- 組織想要將生產力延伸到不受 IT 小組直接控制的電腦、手機、平板電腦或瀏覽器。
- 員工需要從其裝置存取公司的資料和應用程式。
- 當工作負載遷移至雲端時，員工需要更多支援，以獲得低延遲和最佳化的體驗。
- 目前或建議的虛擬桌面體驗成本必須經過最佳化，以協助組織更有效率地擴充其遠端工作。
- IT 小組想要轉換工作場所，通常會從轉換員工體驗來開始。

雲端中的使用者桌面虛擬化可協助您的小組逐個了解這些結果。

## <a name="approach-windows-virtual-desktop-refactor-and-modernization"></a>方法：Windows 虛擬桌面重構和現代化

在本文章系列所述的方法中，現有的 Citrix、VMware 或遠端桌面服務伺服器陣列會現代化為使用稱為 Windows 虛擬桌面的平台即服務 (PaaS) 解決方案。

在此案例中，不是將桌面映像遷移至 Azure，就是會產生新的映像。 同樣地，使用者設定檔也會遷移至 Azure，或建立新的設定檔。 基本上，這項移轉工作能啟用用戶端解決方案，但大多不會有所變更。

![虛擬桌面移轉案例的圖表。](../../_images/migrate/wvd/scenario-solution.png)

當雲端的遷移完成時，虛擬桌面伺服器陣列的管理負荷和成本就會由雲端原生解決方案取代，以管理您小組的虛擬桌面體驗。 小組只需要擔心如何支援桌面映像、可用的應用程式、Azure Active Directory 和使用者設定檔。

## <a name="next-steps"></a>後續步驟

如需雲端採用旅程圖的特定元素指引，請參閱：

- [規劃 Windows 虛擬桌面移轉或部署](./plan.md)
- [檢閱環境或 Azure 登陸區域](./ready.md)
- [完成 Windows 虛擬桌面概念證明](./proof-of-concept.md)
- [評估 Windows 虛擬桌面移轉或部署](./migrate-assess.md)
- [部署或遷移 Windows 虛擬桌面執行個體](./migrate-deploy.md)
- [將 Windows 虛擬桌面部署發行至生產環境](./migrate-release.md)
