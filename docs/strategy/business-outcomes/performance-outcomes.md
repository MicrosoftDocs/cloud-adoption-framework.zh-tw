---
title: 效能結果範例
description: 使用適用于 Azure 的雲端採用架構，瞭解雲端轉換內容中的效能結果。
author: mpvenables
ms.author: brblanch
ms.date: 03/02/2021
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: strategy
ms.custom: internal
ms.openlocfilehash: 05eef909e46b14f5f72212a7c8f5babf2d7933c3
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101784589"
---
# <a name="examples-of-performance-outcomes"></a>效能結果範例

如同在 [業務成果](./index.md)中所討論的，有數個潛在的業務成果可作為與企業之間任何轉型旅程交談的基礎。 本文著重于常見的商務量值：效能。

在現今的技術社會中，客戶假設應用程式會正常運作，而且一律可供使用。 當不符合此預期時，會造成信譽損毀，且成本可能很高。

[GE 航空的數位群組](https://customers.microsoft.com/story/846315-ge-aviation-manufacturing-azure) 部署了 Microsoft Azure 數位 Twins 和其他 Azure 資源，可從多個來源內嵌和建立資料的模型，並使用個別元件的內建數位可追蹤性來建立完整的數位飛機觀點，以協助客戶提高燃料效率、降低維護成本，並提高其機群的航班就緒程度。

## <a name="performance"></a>效能

最大的雲端運算服務是在安全資料中心的全球網路上執行，這些都是定期升級至最新一代的快速且有效率的運算硬體。 這可提供數種優於單一公司資料中心的優點，例如降低應用程式的網路延遲，以及更大的規模經濟。

使用符合全球超過100高度安全設施的節能基礎結構來轉換您的業務，並以地球上最大的網路之一連結，來降低成本。 Azure 擁有的全球區域多於其他任何雲端提供者。 這會轉譯成讓應用程式更接近全球使用者所需的規模、保留資料存放區，並為客戶提供完整的合規性與復原選項。

- **範例1：** 服務公司會使用裝載多個營運基礎結構資產的主機服務提供者。 這些系統會導致頻繁中斷和效能不佳。 公司將其資產遷移至 Azure，以利用雲端的 SLA 和效能控制。 任何停機都會花費大約每分鐘 $15000 美元的停機時間。 在每個月的四到八個小時中斷之間，很容易就能證明這個組織的轉型。

- **範例2：** 消費者投資公司在雲端應用程式創新工作的初期階段。 Agile 程式和 DevOps 的成熟不錯，但應用程式效能卻尖峰。 作為更成熟的轉換，公司根據使用需求啟動了監視和自動調整大小的程式。 公司使用 Azure 效能管理工具消除了調整大小問題，而導致交易中的5% 增加。

## <a name="reliability"></a>可靠性

雲端運算可讓資料備份、嚴重損壞修復和商務持續性更容易且成本更低，因為資料可以鏡像到雲端提供者網路上的多個重複網站。

其中一項重要的功能是確保公司資料永遠不會遺失，而且即使伺服器當機、電源中斷或自然災害，應用程式仍可供使用。 您可以將資料備份至 Azure，讓資料保持安全且可復原。

Azure 備份是一個簡單的解決方案，可降低您的基礎結構成本，並提供增強的安全性機制來保護您的資料免于遭受勒索軟體攻擊。 有一個解決方案，您可以保護在 Azure 中執行的工作負載，以及跨 Linux、Windows、VMware 和 Hyper-v 的內部部署。 您可以讓應用程式在 Azure 中執行，以確保商務持續性。

Azure Site Recovery 可讓您輕鬆地透過複寫 Azure 區域之間的應用程式來測試嚴重損壞修復。 您也可以將內部部署 VMware 和 Hyper-v 虛擬機器和實體伺服器複寫至 Azure，以在主要網站停止運作時保持可用。 而且，您可以在主要網站重新開機並執行時，將工作負載復原至該網站。

- **範例：** 石油和天然氣公司使用 Azure 技術來實行完整的 site recovery。 公司選擇不完全採用雲端進行日常作業，但雲端的商務持續性和嚴重損壞修復 (BCDR) 功能仍會保護其資料中心。 以颶風形成數百英里的地點，其實施合作夥伴開始將網站復原至 Azure。 在颶風觸及之前，所有任務關鍵性資產都是在 Azure 中執行，以防止任何停機時間。

## <a name="next-steps"></a>下一步

瞭解如何使用商務結果範本。

> [!div class="nextstepaction"]
> [使用商務結果範本](./business-outcome-template.md)
