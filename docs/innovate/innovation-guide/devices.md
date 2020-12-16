---
title: Azure 創新：透過裝置互動
description: 了解 Azure 如何透過已連線和感知的邊緣裝置，提供一個架構供您建置沉浸式且有效的商務解決方案。
author: umarmohamedusman
ms.author: brblanch
ms.date: 10/10/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.localizationpriority: high
ms.custom: think-tank, fasttrack-new, AQC
ms.openlocfilehash: bc0ebac1dd6dd0b0120dc4babd736ef691db7338
ms.sourcegitcommit: b6f2b4f8db6c3b1157299ece1f044cff56895919
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97017763"
---
<!-- cSpell:ignore umarmohamedusman umarm Moovit -->

# <a name="interact-through-devices"></a>透過裝置來互動

透過間歇性連線和感知的邊緣裝置進行創新。 協調數百萬個這類裝置、取得及處理無限資料，並利用日益增加的多重感應、多重裝置體驗。 對於網路邊緣的裝置，Azure 提供了一個架構供您建置沉浸式且有效的商務解決方案。 您可以透過 Azure 和 AI 技術相結合而實現的通用運算，建置所能想到的各種智慧型應用程式和系統。

Azure 客戶可以利用一組持續擴展的連線系統和裝置，來收集和分析資料 (接近其使用者、資料或兩者)。 使用者可以透過高回應性的內容感知應用程式取得即時深入解析和體驗。 將部分工作負載移至邊緣，這些裝置就能以較少的時間將訊息傳送至雲端，並更快速地回應空間事件。

> [!div class="checklist"]
>
> - 業界資產
> - [Microsoft HoloLens 2](/hololens)
> - [Azure Sphere](/azure-sphere/product-overview/what-is-azure-sphere)
> - [Azure Kinect DK](/azure/kinect-dk/about-azure-kinect-dk)
> - 無人機
> - [Azure SQL Edge](/azure/azure-sql-edge/overview)
> - [IoT 隨插即用](/azure/iot-pnp/overview-iot-plug-and-play)

## <a name="global-scale-iot-service"></a>[全球規模的 IoT 服務](#tab/IoTHub)

架構出解決方案來以數十億規模的 IoT 裝置執行雙向通訊。 使用現成可用的裝置到雲端遙測資料來了解裝置的狀態，並且只要透過設定就能定義傳給其他 Azure 服務的訊息路由。 您可利用雲端到裝置的訊息，可靠地將命令及通知傳送至連接的裝置，並透過通知回條來追蹤訊息傳遞。 而您將視需要自動重新傳送裝置訊息，以便配合間歇性連線。

以下是您會發現的一些功能：

- **增強安全性的通訊** 通道，用以從 IoT 裝置傳送和接收資料。
- **內建裝置管理** 和佈建，以大規模連線和管理 IoT 裝置。
- **與事件方格完全整合**，也與無伺服器運算完全整合，從而簡化 IoT 應用程式開發。
- **與 Azure IoT Edge 相容**，可建置混合式 IoT 應用程式。

::: zone target="docs"

### <a name="learn-more"></a>深入了解

- [Azure IoT 中心](/azure/iot-hub)
- [Azure IoT 中樞裝置佈建服務 (DPS)](/azure/iot-dps)

::: zone-end

::: zone target="chromeless"

### <a name="action"></a>動作

若要建立 IoT 中樞：

1. 移至 [IoT 中樞]。
2. 選取 [建立 IoT 中樞]。

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Devices%2FIotHubs]" submitText="Go to IoT Hub" :::

<!-- markdownlint-enable DOCSMD001 -->

Azure IoT 中樞裝置佈建服務是 Azure IoT 中樞適用的協助程式服務，可實現完全自動的 Just-In-Time 佈建。

<!-- markdownlint-disable MD024 -->

### <a name="action"></a>動作

若要建立 Azure IoT 中樞裝置佈建服務：

1. 移至 **裝置佈建服務**。
2. 選取 [新增]。

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Devices%2FProvisioningServices]" submitText="Go to Device Provisioning Services" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

## <a name="azure-digital-twins"></a>[Azure Digital Twins](#tab/DigitalTwins)

建置可重複使用、具有高度擴充性、可感知空間的體驗，讓實體和數位世界之間的串流資料彼此連結。 使用實體環境的全方位模型來增強客戶參與。 產生空間智慧圖形，用模型來呈現人員、空間和裝置之間的關聯性與互動情形。 從實體空間查詢資料，而不是從各不相同的感應器查詢。

**Azure Digital Twins 物件模型：** 此本體會描述智慧建築的區域 (region)、地點、樓層、辦公室、區域 (zone)、會議室和聚焦會議室，或是能源網的各種發電站、變電站、能源資源和客戶，並可使用 Digital Twins 物件模型和本體來建立模型。

**空間智慧圖形：** 定義於 Digital Twins 物件模型中的空間、裝置和人員的階層式圖形，可支援繼承、篩選、周遊、延展性和擴充性。 您可透過裝載於 Azure 的 REST API 集合來管理空間圖形並與其互動。

::: zone target="docs"

### <a name="learn-more"></a>深入了解

- [Azure Digital Twins](/azure/digital-twins/about-digital-twins)

::: zone-end

::: zone target="chromeless"

### <a name="action"></a>動作

若要建立 Azure Digital Twins：

1. 在左窗格中選取 [建立資源]。
2. 搜尋 **digital twins**，接著選取 [Digital Twins]。
3. 選取 [建立] 以啟動部署程序。
4. 若要檢閱現有的 Digital Twins，請選取此按鈕：

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResource/resourceType/Microsoft.IoTSpaces%2FGraph]" submitText="Go to Digital Twins" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

## <a name="location-intelligence"></a>[位置智慧](#tab/AzureMaps)

除了鄰近地點、交通和路線等傳統位置功能以外，Azure 地圖服務還可讓企業建立使用即時位置智慧的解決方案，此即時位置智慧由世界級的行動技術合作夥伴 TomTom 和 Moovit 所提供。 利用地理空間服務，輕鬆將進階定位及執行功能整合到您的應用程式中。

**Azure 地圖服務資料服務 (預覽)：** 上傳並儲存用於空間作業或影像編譯的地理空間資料，以在應用程式中縮短延遲、提升生產力，及實踐新的案例。

**空間作業：** 透過內含常見地理空間數學計算 (例如地理柵欄、最接近點、大圓距離及環域) 的程式庫增強您的定位智慧。

**地理位置：** 查閱 IP 位址所在的國家/地區。 依據使用者地點自訂同意與服務，並取得客戶地理分佈的見解。

::: zone target="docs"

### <a name="learn-more"></a>深入了解

- [Azure 地圖服務](/azure/azure-maps)

::: zone-end

::: zone target="chromeless"

### <a name="action"></a>動作

若要使用位置智慧：

1. 移至 [Azure 地圖服務帳戶]。
2. 選取 [建立 Azure 地圖服務帳戶]。

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Maps%2FAccounts]" submitText="Go to Azure Maps Account" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

## <a name="spatial-experiences"></a>[空間體驗](#tab/spatial)

Azure Spatial Anchors 可讓開發人員使用混合實境平台來察覺空間、指定精確的景點，以及從支援的裝置重新叫用這些景點。

**將內容新增至真實世界：** 藉由將您的數位內容置於並連接到實體興趣點，讓您的使用者更加了解自己的資料，以及何時何地需要使用資料。

**跨裝置共用全像投影：** 在您小組和客戶選擇的裝置上提供 3D 給他們，藉此加速決策和結果。 Spatial Anchors 可讓處於相同空間的人員輕鬆參與多使用者的混合實境應用程式。

**吸引人的體驗：** 藉由建立空間錨點之間的關聯性來連結兩者，並提供某種使用者體驗，在這種體驗中，可能會有兩個以上必須由使用者互動才能完成工作的景點。 您的應用程式可以讓使用者將虛擬成品放在現實世界中。 在產業設定中，使用者可藉由將支援的裝置相機指向某機器，以接收其相關內容資訊。

Azure Spatial Anchors 由支援的裝置平台適用的受控服務和用戶端 SDK 所組成。

::: zone target="docs"

### <a name="learn-more"></a>深入了解

- [Azure Spatial Anchors](/azure/spatial-anchors/overview)

::: zone-end

::: zone target="chromeless"

### <a name="action"></a>動作

若要使用 Azure Spatial Anchors：

1. 移至 [Spatial Anchors 帳戶]。
2. 選取 [建立 Spatial Anchors 帳戶]。

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResource/resourceType/Microsoft.MixedReality%2FSpatialAnchorsAccounts]" submitText="Go to Spatial Anchors Accounts" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end

## <a name="azure-remote-rendering"></a>[Azure 遠端轉譯](#tab/RemoteRender)

在雲端轉譯高品質的互動式 3D 內容，並將它即時串流到您的裝置。 媒體和娛樂業的特效 (VFX) 會大量使用轉譯工作負載。 其他如廣告、零售、石油和天然氣及製造等許多產業，也會使用轉譯。

轉譯流程需要大量計算。 可能會產生許多畫面或影像，而且每個影像可能需要數小時才能轉譯。 因此，轉譯是非常適合進行批次處理的工作負載，以便使用 Azure 和 Azure Batch 以平行方式執行許多轉譯。

::: zone target="docs"

### <a name="learn-more"></a>深入了解

- [Azure 遠端轉譯](/azure/remote-rendering/overview/about)
- [使用 Azure 進行轉譯](/azure/batch/batch-rendering-service)

::: zone-end

::: zone target="chromeless"

### <a name="action"></a>動作

若要使用遠端轉譯：

1. 移至 [Batch 帳戶]。
2. 選取 [建立 Batch 帳戶]。

<!-- markdownlint-disable DOCSMD001 -->

::: form action="OpenBlade[#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Batch%2FBatchAccounts]" submitText="Go to Azure Batch" :::

<!-- markdownlint-enable DOCSMD001 -->

::: zone-end
