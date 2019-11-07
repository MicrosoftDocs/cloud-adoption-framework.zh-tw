---
title: Azure 伺服器管理服務的概觀
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: Azure 伺服器管理服務的簡介
author: BrianBlanchard
ms.author: brblanch
ms.date: 05/10/2019
ms.topic: article
ms.service: cloud-adoption-framework
ms.subservice: operate
ms.openlocfilehash: c4939464c80668ca175a4d7ac53fe2198610afc1
ms.sourcegitcommit: bf9be7f2fe4851d83cdf3e083c7c25bd7e144c20
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/04/2019
ms.locfileid: "73565356"
---
# <a name="overview-of-azure-server-management-services"></a>Azure 伺服器管理服務的概觀

Azure 伺服器管理服務為客戶提供大規模管理其伺服器的一致體驗。 這些服務同時涵蓋了 Linux 和 Windows 作業系統，並可用於生產、部署和測試環境。 此外，它們可以支援 Azure IaaS 虛擬機器、實體伺服器，以及內部部署或其他裝載環境中裝載的虛擬機器。

Azure 伺服器管理服務套件包含下圖顯示的服務。

![Azure 作業模型圖](./media/operations-diagram.png)

Microsoft 雲端採用架構這一節中的指引提供可行和規定方案，用於將伺服器管理服務部署在您的環境中。 此方案旨在協助將您快速導向這些服務，透過適用於所有環境大小的一組漸進式管理階段來引導您。

為了簡單起見，我們已將本指引分類為三個階段：

![將 Azure 伺服器管理套件上線的三個階段](./media/operations-stages.png)

<!-- markdownlint-disable MD026 -->

## <a name="why-use-azure-management-services"></a>為何使用 Azure 管理服務？

Azure 管理服務提供下列優勢：

- **原生至 Azure：** 管理服務是內建服務且原生與 Azure Resource Manager 整合。 這些服務會持續改進以提供新特性與功能。
- **Windows 和 Linux：** Windows 和 Linux 機器具有相同的一致管理體驗。
- **混合式：** 管理服務涵蓋了 Azure IaaS 虛擬機器，以及內部部署或其他裝載環境中裝載的實體及虛擬伺服器。
- **安全性：** Microsoft 將大量資源投入於所有形式的安全性。 這項投資不僅保護 Azure 雲端基礎結構，還擴充產生的技術和專業知識，以保護客戶的資源，不論其所在。

## <a name="next-steps"></a>後續步驟

讓您熟悉採用 Azure 伺服器管理套件所涉及的[工具、服務和規劃](./prerequisites.md)。

> [!div class="nextstepaction"]
> [先決工具和規劃](./prerequisites.md)
