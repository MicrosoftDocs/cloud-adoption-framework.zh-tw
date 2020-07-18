---
title: 適用于 Windows 虛擬桌面實例的 Azure 登陸區域
description: 使用適用于 Azure 的雲端採用架構來學習虛擬桌面遷移的最佳作法，以降低複雜度並將遷移程式標準化。
author: BrianBlanchard
ms.author: brblanch
ms.date: 07/01/2010
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: migrate
ms.openlocfilehash: 21ae5207c7d4df5ef29c8a7f99000cbbf670f684
ms.sourcegitcommit: 9163a60a28ffce78ceb5dc8dc4fa1b83d7f56e6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2020
ms.locfileid: "86451808"
---
# <a name="windows-virtual-desktop-azure-landing-zone-review"></a>Windows 虛擬桌面 Azure 登陸區域審核

在遷移至 WVD 之前，小組需要能夠裝載桌面和任何支援工作負載的 Azure 登陸區域。 下列檢查清單可協助您評估登陸區域的相容性。 此架構的[準備方法](../../ready/index.md)中的指引可協助建立相容的 Azure 登陸區域（如果尚未提供的話）。

## <a name="evaluate-compatibility"></a>評估相容性

- **資源組織方案：** 登陸區域應包含要使用之訂用帳戶的參考、資源群組使用方式的指引，以及部署資源時要使用的標記和命名標準。
- **Azure AD：** 應提供 Azure Active Directory 或 Azure AD 的租使用者，以進行終端使用者驗證。
- **網路：** 在遷移之前，應該在登陸區域中建立任何必要的網路設定。
- **VPN 或 ExpressRoute：** 此外，任何支援虛擬桌面電腦的登陸區域都需要網路連線，使用者才能連線到登陸區域和託管的資產。 如果已針對虛擬桌面電腦設定了一組現有的端點，則仍然可以透過 VPN 或 ExpressRoute 連線，將終端使用者路由傳送到這些內部部署裝置。 如果尚未存在，您可能會想要參閱[準備方法](../../ready/index.md)中的設定網路連線選項的指引。
- **治理、使用者和身分識別：** 任何從虛擬桌面治理存取權的需求，以及管理使用者和其身分識別的任何需求，都應該設定為 Azure 原則並套用至登陸區域，以進行一致的強制執行。
- **安全性：** 安全性小組已審查登陸區域設定並核准每個登陸區域以供使用，包括外部連線的登陸區域，以及任何任務關鍵性應用程式或敏感性資料的登陸區域。
- **Windows 虛擬桌面（WVD）：** WVD PaaS 服務已啟用。 <!-- TODO: Add link to enable the service. -->

使用[準備好方法](../../ready/index.md)中的最佳作法所開發的任何登陸區域，皆可符合上述的特殊需求，以作為此遷移的候選登陸區域。

若要瞭解如何設計 Windows 虛擬桌面請參閱[此處的需求](https://docs.microsoft.com/azure/virtual-desktop/overview#requirements)。

## <a name="next-step-complete-a-windows-virtual-desktop-proof-of-concept"></a>下一步：完成 Windows 虛擬桌面概念證明

- [完成 Windows 虛擬桌面概念證明](./proof-of-concept.md)
- [評估 Windows 虛擬桌面遷移或部署](./migrate-assess.md)
- [部署或遷移 Windows 虛擬桌面實例](./migrate-deploy.md)
- [將您的 Windows 虛擬桌面部署發行至生產環境](./migrate-release.md)
