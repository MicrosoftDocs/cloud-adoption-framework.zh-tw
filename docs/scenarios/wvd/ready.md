---
title: 適用于 Windows 虛擬桌面實例的 Azure 登陸區域
description: 使用「適用于 Azure 的雲端採用架構」來準備您的環境，以使用可降低複雜性並將遷移程式標準化的最佳作法來進行虛擬桌面遷移。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/17/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: f7aeba65ddd357ea7a65c6e902d7403f0c403a20
ms.sourcegitcommit: c2249056464d748a6ce15c82cb35a9f164d8f661
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/23/2020
ms.locfileid: "91108304"
---
# <a name="windows-virtual-desktop-azure-landing-zone-review"></a>Windows 虛擬桌面 Azure 登陸區域審核

在 Contoso 雲端採用小組遷移至 Windows 虛擬桌面之前，它將需要一個能夠裝載桌上型電腦和任何支援工作負載的 Azure 登陸區域。 下列檢查清單可協助小組評估登陸區域的相容性。 此架構之 [就緒方法](../../ready/index.md) 的指導方針可協助小組建立相容的 Azure 登陸區域（如果尚未提供的話）。

## <a name="evaluate-compatibility"></a>評估相容性

- **資源組織方案：** 登陸區域應包含要使用的訂用帳戶或訂用帳戶的參考、資源群組使用方式的指引，以及小組部署資源時要使用的標記和命名標準。
- **Azure AD：** 應提供 Azure Active Directory (Azure AD) 實例或 Azure AD 租使用者，以進行使用者驗證。
- **網路：** 在遷移之前，您應該在登陸區域中建立任何必要的網路設定。
- **VPN 或 ExpressRoute：** 此外，任何支援虛擬桌面的登陸區域都需要網路連線，讓終端使用者可以連線到登陸區域和託管資產。 如果已針對虛擬桌面設定一組現有的端點，則終端使用者仍可透過 VPN 或 Azure ExpressRoute 連線，透過這些內部部署裝置來路由傳送。 如果連接還不存在，您可能會想要在 [準備好方法](../../ready/index.md)中，查看設定網路連線選項的指導方針。
- **治理、使用者和身分識別：** 為了一致地強制執行，任何從虛擬桌面管理存取以及管理使用者與其身分識別的需求，都應該設定為 Azure 原則並套用至登陸區域。
- **安全性：** 安全性小組已審核登陸區域設定，並核准每個登陸區域以供其預定使用，包括外部連線的登陸區域，以及任何任務關鍵性應用程式或機密資料的登陸區域。
- **Windows 虛擬桌面：** 已啟用 Windows 虛擬桌面平臺即服務。 <!-- TODO: Add link to enable the service. -->

小組使用 [就緒方法](../../ready/index.md) 中的最佳做法，並符合先前提及之特殊需求的任何登陸區域，都可以作為此遷移的登陸區域。

若要瞭解如何建立 Windows 虛擬桌面架構，請參閱 [Windows 虛擬桌面需求](/azure/virtual-desktop/overview#requirements)。

## <a name="next-steps"></a>後續步驟

如需雲端採用旅程圖的特定元素指引，請參閱：

- [完成 Windows 虛擬桌面概念證明](./proof-of-concept.md)
- [評估 Windows 虛擬桌面移轉或部署](./migrate-assess.md)
- [部署或遷移 Windows 虛擬桌面執行個體](./migrate-deploy.md)
- [將 Windows 虛擬桌面部署發行至生產環境](./migrate-release.md)
