---
title: Azure 伺服器管理服務概觀
description: 了解雲端採用架構中的規範性方案，可提供在您的環境中部署伺服器管理服務。
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.custom: internal
ms.openlocfilehash: ecc1d930a05cfa9f1f521129ec7b87452f3bd455
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101792273"
---
# <a name="overview-of-azure-server-management-services"></a>Azure 伺服器管理服務的概觀

Azure 伺服器管理服務可為大規模管理伺服器提供一致的體驗。 這些服務同時涵蓋 Linux 和 Windows 作業系統， 並可用於生產、開發和測試環境。 伺服器管理服務可以支援 Azure IaaS 虛擬機器、實體伺服器，以及內部部署或其他裝載環境中裝載的虛擬機器。

Azure 伺服器管理服務套件包含下圖中的服務：![Azure 作業模型圖表](./media/operations-diagram.png)

Microsoft 雲端採用架構這一節的內容提供可行和規定方案，用於將伺服器管理服務部署在您的環境中。 此方案可協助將您快速導向這些服務，透過適用於所有環境大小的一組漸進式管理階段來引導您。

為了簡單起見，我們已將本指引分類為三個階段：

![將 Azure 伺服器管理套件上線的三個階段](./media/operations-stages.png)

## <a name="why-use-azure-server-management-services"></a>為何要使用 Azure 伺服器管理服務？

Azure 伺服器管理服務提供下列優勢：

- **原生至 Azure：** 伺服器管理服務是內建服務且原生與 Azure Resource Manager 整合。 這些服務會持續改進以提供新特性與功能。
- **Windows 和 Linux：** Windows 和 Linux 機器具有相同的一致管理體驗。
- **混合式：** 伺服器管理服務涵蓋了 Azure IaaS 虛擬機器，以及內部部署或其他裝載環境中裝載的實體及虛擬伺服器。
- **安全性：** Microsoft 將大量資源投入於所有形式的安全性。 這項投資不僅保護 Azure 基礎結構，還擴充產生的技術和專業知識，以保護客戶的資源，不論其所在。

## <a name="next-steps"></a>後續步驟

讓您熟悉採用 Azure 伺服器管理套件所涉及的[工具、服務和規劃](./prerequisites.md)。

> [!div class="nextstepaction"]
> [先決工具和規劃](./prerequisites.md)
