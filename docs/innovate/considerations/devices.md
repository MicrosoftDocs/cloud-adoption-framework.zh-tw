---
title: 透過數位發明與裝置互動
description: 深入瞭解如何透過客戶與環境體驗和裝置的互動，而非應用程式，來減少創新工作。
author: BrianBlanchard
ms.author: brblanch
ms.date: 10/17/2019
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: innovate
ms.custom: internal
ms.openlocfilehash: e720f26b4189c30dd8f0a5fb0b37c7fac4535549
ms.sourcegitcommit: b8f8b7631aabaab28e9705934bf67dad15e3a179
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/03/2021
ms.locfileid: "101792715"
---
# <a name="ambient-experiences-interact-with-devices"></a>環境體驗：與裝置互動

在以 [客戶理解的組建](./build.md)中，我們討論了這三項真正創新的測試：解決客戶的需求、讓客戶持續回頭，以及調整客戶世代的基礎。 您的每個假設測試都需要投入時間和反復專案的採用方法。 本文提供一些可透過 *環境經驗* 減少這項工作的先進方法見解。 藉由與裝置互動，而不是應用程式，客戶可能更可能會先轉換成您的解決方案。

## <a name="ambient-experiences"></a>環境體驗

環境體驗是與直屬環境相關的數位體驗。 功能環境體驗的解決方案，會在需要時，盡力滿足客戶的需求。 可能的話，解決方案會滿足客戶需求，而不需要離開觸發它的活動流程。

數位經濟中的生活已滿分散注意力。 我們全都頤指氣使了社交、電子郵件、web、視覺效果和口頭訊息，每一個都有受到干擾的風險。 這項風險會隨著客戶的需求點與他們遇到解決方案的時間之間的每秒增加。 無數的客戶在短暫的時間間隔內會遺失。 為了促進重複採用的增加，您必須縮短解決方案的時間來減少分散注意力的數目。

## <a name="interact-with-devices"></a>與裝置互動

標準 web 體驗是最常見的應用程式開發技術，用來滿足客戶的需求。 這種方法假設客戶是在其電腦前面。 如果您的客戶在其膝上型電腦之前一直都符合其需求，請建立 web 應用程式。 該應用程式會在該案例中為該客戶提供環境體驗。 不過，我們知道此案例在目前的時代較不太可能。

![環境體驗](../../_images/innovate/ambient-experiences.png)

環境體驗通常需要超過一天的 web 應用程式。 透過[與客戶](./learn.md)的[測量](./measure.md)和學習，可以觀察、追蹤和使用觸發客戶需求的行為，以建立更多環境體驗。 下列清單摘要說明如何將環境解決方案整合到您的假設中的幾個方法，以及下列各段落各項的詳細資料。

- 行動 **[體驗](#mobile-experience)：** 就像膝上型電腦一樣，行動裝置應用程式在客戶環境中很普遍。 在某些情況下，這可能會提供足夠的互動層級，讓解決方案成為環境。
- **[混合現實](#mixed-reality)：** 有時，客戶的一般環境必須改變，以使互動環境成為互動環境。 這項因素會建立一個假的事實，讓客戶與解決方案互動並符合需求。 在此情況下，解決方案是在錯誤事實中的環境。
- **[整合現實](#integrated-reality)：** 相較于真實的環境，整合式現實解決方案著重于在客戶的現實中使用的裝置，以將解決方案整合至其自然行為。 虛擬助理是將現實整合到周圍環境的絕佳範例。 比較不知名的選項會考慮物聯網 (IoT) 技術，這些技術會整合客戶周圍已存在的裝置。
- **[調整的現實](#adjusted-reality)：** 當這些環境解決方案中有任何一個使用雲端中的預測性分析來定義，並透過自然的環境與客戶互動時，解決方案就已調整現實。

瞭解客戶的需求和測量客戶的影響，可協助您判斷是否需要裝置互動或環境體驗來驗證您的假設。 在每個資料點中，以下各節將協助您找出最佳的解決方案。

## <a name="mobile-experience"></a>行動體驗

在環境體驗的第一個階段中，使用者會移離電腦。 現今的取用者和商務專業人員會在行動裝置和電腦裝置之間移動流暢地。 您的客戶使用的每個平臺或裝置都會建立新的潛在體驗。 新增可延伸主要解決方案的行動體驗，是改善客戶直屬環境整合的最快速方式。 雖然行動裝置離環境更遠，但可能會接近客戶的需求。

當客戶頻繁地移動和變更位置時，這可能代表特定解決方案最相關的環境體驗形式。 過去十年來，在現有解決方案與行動體驗的整合中，經常會觸發創新。

Azure App Service 是此方法的絕佳範例。 在早期反覆運算期間， [Azure App Service 的 web 應用程式功能](/azure/app-service/overview) 可以用來測試假設。 隨著假設變得更複雜， [Azure App Service 的行動應用程式功能](/previous-versions/azure/app-service-mobile/) 可以擴充 web 應用程式，以在各種行動平臺上執行。

## <a name="mixed-reality"></a>混合實境

混合的現實解決方案代表環境體驗的下一個成熟度等級。 這種方法會擴大或複製客戶的環境。它會建立一項現實的延伸，讓客戶能夠在其中運作。

> [!IMPORTANT]
> 如果需要 VR 裝置，而且它還不是客戶直屬環境或自然行為的一部分，則增強型或虛擬實境是更多的替代體驗和較少的環境體驗。

混合的現實體驗在遠端工作力之間越來越普遍。 在需要共同作業或專長技術的產業中，其使用方式會變得更快，而不會在當地市場提供。 需要針對遠端人力強制執行複雜產品的集中式實作為支援的情況，特別是為了增強現實的豐富也基礎。 在這些案例中，中央支援小組和遠端員工可能會使用增強的事實來處理、疑難排解及安裝產品。

例如，請考慮空間錨點的大小寫。 空間錨點可讓您使用物件來建立混合現實體驗，以在一段時間內跨裝置保存各自的位置。 透過空間錨點，可以捕獲、記錄和保存特定的行為，藉此在使用者下次在該增強環境中運作時，提供環境體驗。 [Azure 空間錨點](/azure/spatial-anchors/overview) 是一項服務，可將此邏輯移至雲端，讓您可以跨裝置或甚至是跨解決方案共用體驗。

## <a name="integrated-reality"></a>整合現實

除了行動現實，或甚至混合現實之外，還整合了現實。 整合式現實旨在完全移除數位體驗。 所有我們都是使用計算和連線能力的裝置。 這些裝置可以用來從立即環境中收集資料，而不需要客戶觸及電話、膝上型電腦或虛擬實境 (VR) 裝置。

當某個裝置形式一致地在客戶需要發生的相同環境中時，此體驗就很理想。 常見案例包括工廠樓層、電梯，甚至是您的汽車。 這些大型裝置類型已包含計算能力。 您也可以使用裝置本身的資料來偵測客戶行為，並將這些行為傳送至雲端。 自動捕捉客戶行為資料，可大幅降低客戶輸入資料的需求。 此外，web、mobile 或 VR 體驗也可以做為意見反應迴圈，以分享從整合式現實解決方案中學到的內容。

<!-- docutune:casing "advanced computer vision" -->

Azure 中的整合式現實範例包括：

- [Azure 物聯網 (IoT) 解決方案](/azure/iot-fundamentals/)： azure 中的服務集合，每個服務都有助於管理裝置，以及從這些裝置到雲端的資料流程，以及向使用者傳送資料。
- [Azure 球體](/azure-sphere/)：硬體和軟體的組合，可提供本質上安全的方式，讓現有的裝置在裝置與 Azure IoT 解決方案之間安全地傳輸資料。
- [Azure KINECT 深色](/azure/kinect-dk/)的 AI 感應器，具有先進的電腦視覺和語音模型。 這些感應器可以從立即環境中收集視覺和音訊資料，並將這些輸入送入您的解決方案。

您可以使用這三種工具，從自然周圍和客戶需要的角度收集資料。 從該處，您的解決方案可以回應這些資料輸入以解決需求，有時候客戶也知道該需要的觸發程式已發生。

## <a name="adjusted-reality"></a>已調整的實境

最高形式的環境體驗已調整為現實，通常稱為 *環境智慧*。 調整的現實是一種方法，可使用解決方案中的資訊來變更客戶的實際情況，而不需要他們直接與應用程式互動。 在此方法中，您一開始所建立用來證明假設的應用程式可能不再相關。 相反地，環境中的裝置會協助 lambert 輸入和輸出，以符合客戶的需求。

虛擬助理和智慧型喇叭提供調整現實的絕佳範例。 簡單來說，智慧型喇叭就是簡單整合現實的範例。 但是，您可以將智慧型燈光和動作感應器新增至智慧型喇叭解決方案，而且可以輕鬆地建立基本解決方案，以在您輸入房間時開啟燈。

全球工廠樓層提供調整現實的其他範例。 在整合現實的初期階段，裝置上的感應器偵測到過高的狀況，然後透過應用程式向人發出警示。 在調整的現實情況下，客戶可能仍會參與，但意見反應迴圈更緊密。 在調整的現實工廠樓層中，一部裝置可能會在元件行的某處偵測到重要機器的熱。 其次，第二個裝置則會稍微降低生產環境的速度，讓電腦變得很酷，然後在解決此狀況時繼續進行完整的步調。 在此情況下，客戶是第二個參與者。 客戶會使用您的應用程式來設定規則，並瞭解這些規則對生產環境的影響，但不需要回饋迴圈。

Azure 物聯網中所述的 Azure 服務 [ (IoT) 解決方案](/azure/iot-fundamentals/)、 [Azure 球體](/azure-sphere/)和 [azure Kinect 深色](/azure/kinect-dk/) 都可以是已調整之現實解決方案的元件。 然後，您的原始應用程式和商務邏輯會作為環境輸入之間的媒介，以及應該在實體環境中進行的變更。

數位對應項是另一個調整事實的範例。 此詞彙是指實體裝置的數位標記法，以電腦、行動或混合式事實格式呈現。 不同于較不復雜的3D 模型，數位對應項會反映從實體環境中實際裝置收集而來的資料。 此解決方案可讓使用者以真實世界中無法完成的方式，與數位表示互動。 在此方法中，實體裝置會調整混合的現實環境。 不過，解決方案仍然會從整合式的現實解決方案收集資料，並使用該資料來塑造客戶目前周圍環境的實際情況。

在 Azure 中，會透過稱為 [Azure 數位 twins](/azure/digital-twins/overview)的服務來建立及存取數位 twins。

## <a name="next-steps"></a>下一步

既然您已深入瞭解裝置互動，以及適合您解決方案的環境體驗，您就可以開始探索創新、 [預測和影響](./predict.md)的最終專業領域。

> [!div class="nextstepaction"]
> [預測和影響](./predict.md)
