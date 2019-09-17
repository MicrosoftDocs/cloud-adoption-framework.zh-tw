---
title: Azure 虛擬資料中心
description: Microsoft Azure 虛擬資料中心的資源
author: tracsman
ms.author: jonor
ms.date: 06/12/2019
ms.topic: landing-page
ms.service: cloud-adoption-framework
ms.subservice: reference
keywords: Azure
layout: LandingPage
ms.openlocfilehash: 39cb6ac3c31873431206d8c6c525c9ce3467653f
ms.sourcegitcommit: 443c28f3afeedfbfe8b9980875a54afdbebd83a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2019
ms.locfileid: "71030947"
---
# <a name="azure-virtual-datacenter-and-the-enterprise-control-plane"></a>Azure 虛擬資料中心和 Enterprise 控制平面

Azure 虛擬資料中心能充分發揮 Azure 雲端平台的功能，同時還能遵循現有的安全性與網路原則。 IT 組織和營業單位在將企業工作負載部署到雲端時，必須在治理與開發人員敏捷度之間取得平衡。 Azure 虛擬資料中心提供模型來達成這項平衡且著重治理。

## <a name="resources"></a>資源

<!-- markdownlint-disable MD033 -->

<table>
<tr>
    <td style="width: 64px; vertical-align: middle;"><a href="https://aka.ms/VDC/Concepts"><img src="../_images/vdc/virtual-datacenter.svg" alt="Virtual Datacenter e-book" /></a></td>
    <td>
        <h3><a href="https://aka.ms/VDC/Concepts">Azure 虛擬資料中心：概念</a></h3>
        <p>這個電子書會說明如何將企業工作負載部署至 Azure 雲端平台，同時又能採用您現有的安全性和網路原則。</p>
    </td>
</tr>
<tr>
    <td style="width: 64px; vertical-align: middle;"><a href="./networking-vdc.md"><img src="../_images/vdc/vdc-network.png" alt="Network Perspective" /></a></td>
    <td>
        <h3><a href="./networking-vdc.md">Azure 虛擬資料中心：網路觀點</a></h3>
        <p>此線上文章概述可用來解決許多客戶在考慮移至雲端時所面臨之架構規模、效能和安全性問題的網路模式和設計。</p>
    </td>
</tr>
<tr>
    <td style="width: 64px; vertical-align: middle;"><a href="https://aka.ms/VDC/Lift"><img src="../_images/vdc/vdc-lift-and-shift.png" alt="Lift and Shift Guide" /></a></td>
    <td>
        <h3><a href="https://aka.ms/VDC/Lift">Azure 虛擬資料中心：隨即轉移指南</a></h3>
        <p>本白皮書討論企業 IT 人員和決策者可用來識別及規劃應用程式和伺服器移轉至 Azure 的程序，而在此程序中會使用「原形移轉」方法，以將任何額外開發成本降至最低，同時將雲端裝載選項最佳化。</p>
    </td>
</tr>
<tr>
    <td style="width: 64px; vertical-align: middle;"><a href="https://aka.ms/VDC/Deck"><img src="../_images/vdc/vdc-deck.png" alt="Presentation Deck" /></a></td>
    <td>
        <h3><a href="https://aka.ms/VDC/Deck">Azure 虛擬資料中心：簡報 Deck</a></h3>
        <p>此簡報投影片可探索 Azure 虛擬資料中心指引和工具。 其中涵蓋 VDC 目標、客戶驅動程式、Azure 區域，VDC 自動化的元素、工業化和信任的 Azure VDC，而結尾是以 CIO 指引為主的行動計畫。 此外，也提供支援和訓練資訊。</p>
    </td>
</tr>
</table>

<!-- markdownlint-enable MD033 -->

<!-- markdownlint-disable MD026 -->

## <a name="what-is-the-azure-virtual-datacenter"></a>什麼是 Azure 虛擬資料中心？

將工作負載部署到雲端，引起了發展及維護雲端信任的需求，信任程度就如同您信任現有的資料中心。 Azure 虛擬資料中心指引的第一個模型設計是透過虛擬基礎結構的鎖定方法來銜接該需求。 這種方法不適合每個人。 它專為引導企業 IT 團隊將其內部部署基礎結構擴充至 Azure 公用雲端而設計。 我們將這個方法稱為信任的資料中心擴充模型。 隨著時間推進，其他數種模型便會陸續推出，包括允許直接從虛擬資料中心進行安全網際網路存取的模型。

<!-- markdownlint-disable MD033 -->

<img src="../_images/vdc/vdc-components.svg" alt="Virtual Datacenter components" style="max-width:700px;"/>

<!-- markdownlint-enable MD033 -->

以下四個元件促成 Azure 虛擬資料中心：身分識別、加密、軟體定義的網路功能及合規性 (包括記錄和報告)。

在 Azure 虛擬資料中心模型中，您可以套用隔離原則、讓雲端更像您所知的實體資料中心，以及達到您所需的安全性和信任層級。 任何企業 IT 團隊認定可實現上述目標的四個元件：軟體定義的網路功能、加密、身分識別管理，以及 Azure 平台的基礎合規性標準和認證。 這四個元件是讓虛擬資料中心成為現有基礎結構投資之信任擴充的關鍵。

請繼續閱讀 [Azure 虛擬資料中心概念](https://azure.microsoft.com/resources/azure-virtual-datacenter) \(英文\) 電子書。
