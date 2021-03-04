---
title: Azure 如何運作？
description: 瞭解 Azure 雲端平臺和雲端虛擬化的內部結構和運作的基本概念。
author: alexbuckgit
ms.author: abuck
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: overview
ms.custom: internal
ms.openlocfilehash: 541c55ce571e33c0e33d7bb1b52fd4584604ada6
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101791491"
---
# <a name="how-does-azure-work"></a>Azure 如何運作？

Azure 是 Microsoft 的公用雲端平台。 Azure 提供一系列大型的服務，包括平臺即服務 (PaaS) 、基礎結構即服務 (IaaS) 和受控資料庫服務功能。 但 Azure 到底是什麼？它如何運作？

<!-- markdownlint-disable MD034 -->

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2ixGo]

<!-- markdownlint-enable MD034 -->

Azure 就像其他雲端平台一樣，需仰賴名為 *虛擬化* 的技術。 大部分的電腦硬體都可用軟體來模擬，因為大部分的電腦硬體都只是一組永久或半永久編碼在矽晶片材料中的指令。 使用將軟體指令對應至硬體指令的模擬層，虛擬化的硬體即可用軟體執行，如同在實際的硬體中執行一般。

基本上，雲端是一或多個資料中心內部代替客戶執行虛擬化硬體的一組實體伺服器。 那麼，雲端要如何同時為數百萬個客戶建立、啟動、停止及刪除數百萬個虛擬化硬體執行個體呢？

要了解這一點，就必須看看資料中心裡的硬體架構。 在每個資料中心內都是位於伺服器機架中的伺服器集合。 每個伺服器機架都包含許多伺服器 *刀鋒*，以及提供網路連線的網路交換器和提供電力的配電裝置 (PDU)。 機架有時會一起分組到較大的單位中，名為 *叢集*。

在每個機架或叢集內，大部分的伺服器都會被指定用來代替使用者執行這些虛擬化硬體執行個體。 但是有些伺服器會執行稱為網狀架構控制器的雲端管理軟體。 *網狀架構控制器* 是一種分散式應用程式，負責執行多項工作。 它會配置服務、監視伺服器及其執行之服務的健全狀況，並在伺服器故障時加以修復。

每個網狀架構控制器執行個體都會連線至執行雲端協調流程軟體的另一組伺服器，一般稱之為 *前端*。 前端會主控 Web 服務、RESTful API，以及雲端執行的所有功能所使用的內部 Azure 資料庫。

例如，前端會裝載處理客戶要求的服務，以配置 Azure 資源（例如 [虛擬機器](/azure/virtual-machines/)）和服務（例如 [azure Cosmos DB](/azure/cosmos-db/introduction)）。 首先，前端會驗證使用者，並確認使用者是否有權配置要求的資源。 如果是，前端會檢查資料庫以找出具有足夠容量的伺服器機架，然後指示該機架上的網狀架構控制器配置資源。

基本上，Azure 是一組龐大的伺服器和網路硬體，執行一組複雜的分散式應用程式，以協調這些伺服器上虛擬化硬體和軟體的設定和操作。 這是讓 Azure 變得強大的協調流程，因為因為 Azure 會在幕後完成這些工作，所以使用者不再負責維護和升級硬體。

## <a name="next-steps"></a>下一步

透過適用于 Azure 的 Microsoft 雲端採用架構，深入瞭解雲端採用。

> [!div class="nextstepaction"]
> [瞭解適用于 Azure 的 Microsoft 雲端採用架構](../index.yml)
